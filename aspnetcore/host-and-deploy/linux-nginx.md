---
title: Hosten von ASP.NET Core unter Linux mit Nginx
author: rick-anderson
description: Hier finden Sie Informationen zum Einrichten von Nginx als Reverseproxy unter Ubuntu 16.04, um den HTTP-Datenverkehr an eine ASP.NET Core-Web-App weiterzuleiten, die auf Kestrel ausgeführt wird.
ms.author: riande
ms.custom: mvc
ms.date: 05/22/2018
uid: host-and-deploy/linux-nginx
ms.openlocfilehash: 840a9f98b3409f74b9a41ee24ff7bcb33a875470
ms.sourcegitcommit: 18339e3cb5a891a3ca36d8146fa83cf91c32e707
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37433934"
---
# <a name="host-aspnet-core-on-linux-with-nginx"></a>Hosten von ASP.NET Core unter Linux mit Nginx

Von [Sourabh Shirhatti](https://twitter.com/sshirhatti)

In diesem Leitfaden wird das Einrichten einer produktionsbereiten ASP.NET Core-Umgebung auf einem Ubuntu 16.04-Server erläutert. Diese Anweisungen sind wahrscheinlich auf neuere Versionen von Ubuntu anwendbar, sie wurden jedoch noch nicht mit neueren Versionen getestet.

> [!NOTE]
> Für Ubuntu 14.04 wird *supervisord* für die Überwachung des Kestrel-Prozesses empfohlen. *systemd* ist unter Ubuntu 14.04 nicht verfügbar. Anweisungen zu Ubuntu 14.04 finden Sie in der [vorherigen Version dieses Themas](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).

In diesem Leitfaden:

* Wird eine bestehende ASP.NET Core-App hinter einem Reverseproxyserver eingefügt.
* Wird der Reverseproxyserver eingerichtet, um Anforderungen an den Kestrel-Webserver weiterzuleiten.
* Wird sichergestellt, dass die Web-App beim Start als Daemon ausgeführt wird.
* Wird ein Prozessverwaltungstool konfiguriert, das die Web-App beim Neustarten unterstützt.

## <a name="prerequisites"></a>Erforderliche Komponenten

1. Greifen Sie auf einen Ubuntu 16.04-Server mit einem Standardbenutzerkonto mit sudo-Berechtigung zu.
1. Installieren Sie die .NET Core-Runtime auf dem Server.
   1. Navigieren Sie zu der [.NET-Seite „All Downloads“ (Alle Downloads)](https://www.microsoft.com/net/download/all).
   1. Wählen Sie unter **Runtime** die aktuelle Nicht-Vorschau-Runtime aus der Liste aus.
   1. Wählen Sie die Anweisungen für Ubuntu aus, die der Ubuntu-Version des Servers entsprechen, und befolgen Sie diese.
1. Eine vorhandene ASP.NET Core-App.

## <a name="publish-and-copy-over-the-app"></a>Veröffentlichen und Kopieren der App

Konfigurieren Sie die App für eine [Framework-abhängige Bereitstellung](/dotnet/core/deploying/#framework-dependent-deployments-fdd).

Führen Sie [dotnet publish](/dotnet/core/tools/dotnet-publish) in der Entwicklungsumgebung aus, um eine App in ein Verzeichnis zu packen (z.B. *bin/Release/&lt;target_framework_moniker&gt;/publish*), das auf dem Server ausgeführt werden kann:

```console
dotnet publish --configuration Release
```

Die App kann auch als eine [eigenständige Bereitstellung](/dotnet/core/deploying/#self-contained-deployments-scd) veröffentlicht werden, wenn Sie die .NET Core-Runtime nicht auf dem Server verwalten möchten.

Kopieren Sie die ASP.NET Core-App auf den Server, indem Sie ein beliebiges Tool verwenden, das in den Workflow der Organisation integriert ist (z.B. SCP oder SFTP). Web-Apps befinden sich üblicherweise im Verzeichnis *var* (z.B. *var/aspnetcore/hellomvc*).

> [!NOTE]
> In einem Szenario für die Bereitstellung in der Produktion übernimmt ein Continuous Integration-Workflow die Veröffentlichung der App und das Kopieren der Objekte auf den Server.

Testen der App:

1. Führen Sie die App über die Befehlszeile aus: `dotnet <app_assembly>.dll`.
1. Navigieren Sie in einem Browser zu `http://<serveraddress>:<port>`, und überprüfen Sie, ob die App lokal unter Linux funktioniert.

## <a name="configure-a-reverse-proxy-server"></a>Konfigurieren eines Reverseproxyservers

Ein Reverseproxy wird im Allgemeinen zur Unterstützung dynamischer Web-Apps eingerichtet. Ein Reverseproxy beendet die HTTP-Anforderung und leitet diese an die ASP.NET Core-App weiter.

::: moniker range=">= aspnetcore-2.0"

> [!NOTE]
> Jede der beiden Konfigurationen &mdash;mit oder ohne einen Reverseproxyserver&mdash; ist eine gültige und unterstützte Hostingkonfiguration für ASP.NET Core 2.0 oder neuere Apps. Weitere Informationen finden Sie unter [When to use Kestrel with a reverse proxy (Verwenden von Kestrel mit einem Reverseproxy)](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).

::: moniker-end

### <a name="use-a-reverse-proxy-server"></a>Verwenden eines Reverseproxyservers

Kestrel eignet sich hervorragend für die Bereitstellung dynamischer Inhalte aus ASP.NET Core. Die Webbereitstellungsfunktionen sind jedoch nicht so umfangreich wie bei Servern wie IIS, Apache oder Nginx. Ein Reverseproxyserver kann Arbeiten wie das Verarbeiten von statischen Inhalten, das Zwischenspeichern und Komprimieren von Anforderungen und das Beenden von SSL vom HTTP-Server auslagern. Ein Reverseproxyserver kann sich auf einem dedizierten Computer befinden oder zusammen mit einem HTTP-Server bereitgestellt werden.

Für diesen Leitfaden wird eine einzelne Instanz von Nginx verwendet. Diese wird auf demselben Server ausgeführt, zusammen mit dem HTTP-Server. Je nach Anforderungen kann ein anderes Setup ausgewählt werden.

Da Anforderungen vom Reverseproxy weitergeleitet werden, sollten Sie die [Middleware für weitergeleitete Header](xref:host-and-deploy/proxy-load-balancer) aus dem Paket [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) verwenden. Diese Middleware aktualisiert `Request.Scheme` mithilfe des `X-Forwarded-Proto`-Headers, sodass Umleitungs-URIs und andere Sicherheitsrichtlinien ordnungsgemäß funktionieren.

Jede vom Schema abhängige Komponente, wie etwa Authentifizierung, Linkgenerierung, Umleitungen und Geolocation, muss nach dem Aufrufen der Middleware für weitergeleitete Header eingefügt werden. Allgemein gilt, dass Middleware für weitergeleitete Header vor jeder anderen Middleware ausgeführt werden sollte, mit Ausnahme von Middleware für die Diagnose und Fehlerbehandlung. Mit dieser Reihenfolge wird sichergestellt, dass die auf Informationen von weitergeleiteten Headern basierende Middleware die zu verarbeitenden Headerwerte nutzen kann.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Rufen Sie die Methode [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) in `Startup.Configure` auf, bevor Sie [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) oder eine ähnliche Middleware für Authentifizierungsschemas aufrufen. Konfigurieren Sie die Middleware so, dass die Header `X-Forwarded-For` und `X-Forwarded-Proto` weitergeleitet werden:

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Rufen Sie die Methode [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) in `Startup.Configure` auf, bevor Sie [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) und [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) oder eine ähnliche Middleware für Authentifizierungsschemas aufrufen. Konfigurieren Sie die Middleware so, dass die Header `X-Forwarded-For` und `X-Forwarded-Proto` weitergeleitet werden:

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseIdentity();
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

---

Wenn für die Middleware keine [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) angegeben sind, lauten die weiterzuleitenden Standardheader `None`.

Möglicherweise ist zusätzliche Konfiguration für Apps erforderlich, die hinter Proxyservern und Lastenausgleichsmodulen (Load Balancer) gehostet werden. Weitere Informationen hierzu feinden Sie unter [Konfigurieren von ASP.NET Core zur Verwendung mit Proxyservern und Lastenausgleich](xref:host-and-deploy/proxy-load-balancer).

### <a name="install-nginx"></a>Installieren von Nginx

Verwenden Sie `apt-get` zum Installieren von Nginx. Das Installationsprogramm erstellt ein *systemd*-Initialisierungsskript, das Nginx beim Systemstart als Dämon ausführt. 

```bash
sudo -s
nginx=stable # use nginx=development for latest development version
add-apt-repository ppa:nginx/$nginx
apt-get update
apt-get install nginx
```

Das Ubuntu Personal Package Archive (PPA) wird von Freiwilligen verwaltet und nicht durch [nginx.org](https://nginx.org/) verteilt. Weitere Informationen finden Sie unter [Nginx: Binary Releases: Official Debian/Ubuntu packages (Nginx: binäre Releases – offizielle Debian/Ubuntu-Pakete)](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages).

> [!NOTE]
> Wenn optionale Nginx-Module benötigt werden, kann die Erstellung von Nginx aus der Quelle erforderlich sein.

Da Nginx zum ersten Mal installiert wurde, starten Sie es explizit, indem Sie Folgendes ausführen:

```bash
sudo service nginx start
```

Stellen Sie sicher, dass ein Browser die Standardangebotsseite für Nginx anzeigt. Die Landing Page ist unter `http://<server_IP_address>/index.nginx-debian.html` erreichbar.

### <a name="configure-nginx"></a>Konfigurieren von Nginx

Ändern Sie */etc/nginx/sites-available/default*, um Nginx als Reverseproxy für die Weiterleitung von Anforderungen zu Ihrer ASP.NET Core-App zu konfigurieren. Öffnen Sie die Datei in einem Text-Editor und ersetzen Sie den Inhalt durch den Folgendes:

```nginx
server {
    listen        80;
    server_name   example.com *.example.com;
    location / {
        proxy_pass         http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header   Upgrade $http_upgrade;
        proxy_set_header   Connection keep-alive;
        proxy_set_header   Host $host;
        proxy_cache_bypass $http_upgrade;
        proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header   X-Forwarded-Proto $scheme;
    }
}
```

Wenn keine Übereinstimmung mit `server_name` gefunden wird, verwendet Nginx den Standardserver. Wenn kein Server definiert ist, ist der erste Server in der Konfigurationsdatei der Standardserver. Als bewährte Methode gilt, einen bestimmten Standardserver hinzuzufügen, der den Statuscode 444 in Ihrer Konfigurationsdatei zurückgibt. Im Folgenden wird ein Beispiel für eine Standardserverkonfiguration aufgeführt:

```nginx
server {
    listen   80 default_server;
    # listen [::]:80 default_server deferred;
    return   444;
}
```

Mit der vorhergehenden Konfigurationsdatei und dem vorhergehenden Standardserver akzeptiert Nginx den öffentlichen Datenverkehr über Port 80 mit dem Hostheader `example.com` oder `*.example.com`. Anforderungen, die mit diesen Hosts nicht übereinstimmen, werden nicht an Kestrel weitergeleitet. Nginx leitet die übereinstimmenden Anforderungen unter `http://localhost:5000` an Kestrel weiter. Weitere Informationen finden Sie unter [Verarbeitung einer Anforderung mit Nginx](https://nginx.org/docs/http/request_processing.html). Informationen zum Ändern der IP-Adresse bzw. des Ports von Kestrel finden Sie unter [Kestrel: Endpoint configuration (Kestrel: Endpunktkonfiguration)](xref:fundamentals/servers/kestrel#endpoint-configuration).

> [!WARNING]
> Schlägt die Angabe einer ordnungsgemäßen [server_name-Anweisung](https://nginx.org/docs/http/server_names.html) fehlt, ist Ihre App Sicherheitsrisiken ausgesetzt. Platzhalterbindungen in untergeordneten Domänen (z.B. `*.example.com`) verursachen kein Sicherheitsrisiko, wenn Sie die gesamte übergeordnete Domäne steuern (im Gegensatz zu `*.com`, das angreifbar ist). Weitere Informationen finden Sie unter [rfc7230 im Abschnitt 5.4](https://tools.ietf.org/html/rfc7230#section-5.4).

Wenn die Nginx-Konfiguration eingerichtet ist, können Sie zur Überprüfung der Syntax der Konfigurationsdateien `sudo nginx -t` ausführen. Wenn der Test der Konfigurationsdatei erfolgreich ist, können Sie durch Ausführen von `sudo nginx -s reload` erzwingen, dass Nginx die Änderungen übernimmt.

Gehen Sie wie folgt vor, um die App auf dem Server direkt auszuführen:

1. Navigieren Sie zum Verzeichnis der App.
1. Führen Sie die ausführbare Datei der App aus: `./<app_executable>`.

Sollte ein Berechtigungsfehler auftreten, ändern Sie die Berechtigungen:

```console
chmod u+x <app_executable>
```

Wenn die App auf dem Server ausgeführt wird, über das Internet jedoch nicht reagiert, sollten Sie die Firewall des Servers überprüfen und sicherstellen, dass Port 80 geöffnet ist. Fügen Sie bei Verwendung einer Azure-Ubuntu-VM eine Regel der Netzwerksicherheitsgruppe (NSG) zur Aktivierung des eingehenden Datenverkehrs an Port 80 hinzu. Eine Regel für ausgehenden Datenverkehr an Port 80 muss nicht aktiviert werden, da der ausgehende Datenverkehr automatisch gewährt wird, wenn die Regel für den eingehenden Datenverkehr aktiviert ist.

Nach dem Testen der App können Sie die App mit `Ctrl+C` in der Eingabeaufforderung beenden.

## <a name="monitoring-the-app"></a>Überwachen der App

Der Server ist dafür eingerichtet, Anforderungen an `http://<serveraddress>:80` an die ASP.NET Core-App weiterzuleiten, die unter `http://127.0.0.1:5000` unter Kestrel ausgeführt wird. Nginx wurde jedoch nicht dafür eingerichtet, den Kestrel-Prozess zu verwalten. *systemd* kann für die Erstellung einer Dienstdatei verwendet werden, um die zugrunde liegende Web-App zu starten und zu überwachen. *SystemD* ist ein Initialisierungssystem, das viele leistungsstarke Features zum Starten, Beenden und Verwalten von Prozessen bereitstellt. 

### <a name="create-the-service-file"></a>Erstellen der Dienstdatei

Erstellen Sie die Dienstdefinitionsdatei:

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

Im Folgenden wird ein Beispiel für eine Dienstdatei der App aufgeführt:

```ini
[Unit]
Description=Example .NET Web API App running on Ubuntu

[Service]
WorkingDirectory=/var/aspnetcore/hellomvc
ExecStart=/usr/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
Restart=always
# Restart service after 10 seconds if the dotnet service crashes:
RestartSec=10
SyslogIdentifier=dotnet-example
User=www-data
Environment=ASPNETCORE_ENVIRONMENT=Production
Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false

[Install]
WantedBy=multi-user.target
```

Wenn der Benutzer *www-data* durch Ihre Konfiguration nicht verwendet wird, muss der hier definierte Benutzer zunächst erstellt werden, und ihm muss der ordnungsgemäße Besitz für die Dateien erteilt werden.

Linux verfügt über ein Dateisystem, bei dem die Groß-/Kleinschreibung beachtet wird. Das Festlegen von ASPNETCORE_ENVIRONMENT auf „Production“ führt dazu, dass nach der Konfigurationsdatei *appsettings.Production.json* und nicht nach *appsettings.production.json* gesucht wird.

> [!NOTE]
> Einige Werte (z.B. SQL-Verbindungszeichenfolgen) müssen mit Escapezeichen versehen werden, damit die Konfigurationsanbieter die Umgebungsvariablen lesen können. Mit dem folgenden Befehl können Sie einen ordnungsgemäß mit Escapezeichen versehenen Wert für die Verwendung in der Konfigurationsdatei generieren:
>
> ```console
> systemd-escape "<value-to-escape>"
> ```

Speichern Sie die Datei, und aktivieren Sie den Dienst.

```bash
systemctl enable kestrel-hellomvc.service
```

Starten Sie den Dienst, und überprüfen Sie, ob er ausgeführt wird.

```
systemctl start kestrel-hellomvc.service
systemctl status kestrel-hellomvc.service

● kestrel-hellomvc.service - Example .NET Web API App running on Ubuntu
    Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-hellomvc.service
            └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

Wenn der Reverseproxy konfiguriert ist und Kestrel durch „systemd“ verwaltet wird, ist die Webanwendung vollständig konfiguriert. Auf diese kann dann über einen Browser auf dem lokalen Computer unter `http://localhost` zugegriffen werden. Der Zugriff ist auch von einem Remotecomputer aus möglich, sofern keine Firewall diesen blockiert. Beim Überprüfen der Antwortheader zeigt der Header `Server` die ASP.NET Core-App an, die von Kestrel verarbeitet wird.

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a>Anzeigen von Protokollen

Da die Web-App, die Kestrel verwendet, von `systemd` verwaltet wird, werden alle Ereignisse und Prozesse in einem zentralen Journal protokolliert. Dieses Journal enthält jedoch alle Einträge für alle Dienste und Prozesse, die von `systemd` verwaltet werden. Verwenden Sie folgenden Befehl, um die `kestrel-hellomvc.service`-spezifischen Elemente anzuzeigen:

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

Für weiteres Filtern können Zeitoptionen wie `--since today`, `--until 1 hour ago` oder eine Kombination aus diesen die Menge der zurückgegebenen Einträge verringern.

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a>Sichern der App

### <a name="enable-apparmor"></a>Aktivieren von AppArmor

Bei Linux Security Modules (LSM) handelt es sich um ein Framework, das seit Linux 2.6 Teil des Linux-Kernels ist. LSM unterstützt verschiedene Implementierungen von Sicherheitsmodulen. [AppArmor](https://wiki.ubuntu.com/AppArmor) ist ein LSM, das ein obligatorisches Zugriffssteuerungssystem implementiert, das das Programm auf einen beschränkten Satz von Ressourcen begrenzt. Versichern Sie sich, dass AppArmor aktiviert und ordnungsgemäß konfiguriert ist.

### <a name="configuring-the-firewall"></a>Konfigurieren der Firewall

Schließen Sie alle externen Ports, die nicht verwendet werden. „Uncomplicated Firewall“ (UFW) stellt ein Front-End für `iptables` bereit, indem eine Befehlszeilenschnittstelle zum Konfigurieren der Firewall bereitgestellt wird. Überprüfen Sie, ob `ufw` konfiguriert ist, um Datenverkehr für alle erforderlichen Ports zuzulassen.

```bash
sudo apt-get install ufw
sudo ufw enable

sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### <a name="securing-nginx"></a>Sichern von Nginx

#### <a name="change-the-nginx-response-name"></a>Ändern des Namens der Nginx-Antwort

Bearbeiten Sie *src/http/ngx_http_header_filter_module.c*:

```c
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-options"></a>Konfigurieren von Optionen

Konfigurieren Sie den Server mit zusätzlichen erforderlichen Modulen. Erwägen Sie zum Schutz der App die Verwendung einer Web-App-Firewall wie z.B. [ModSecurity](https://www.modsecurity.org/).

#### <a name="configure-ssl"></a>Konfigurieren von SSL

* Konfigurieren Sie den Server, damit dieser für den HTTPS-Datenverkehr an Port `443` empfangsbereit ist, indem Sie ein gültiges Zertifikat angeben, das von einer vertrauenswürdigen Zertifizierungsstelle (Certificate Authority, CA) ausgestellt wurde.

* Stärken Sie Ihre Sicherheit, indem Sie einige der in der folgenden Datei (*/etc/nginx/nginx.conf*) dargestellten Methoden verwenden. Die Beispiele schließen das Auswählen einer stärkeren Verschlüsselung und das Weiterleiten allen Datenverkehrs über HTTP auf HTTPS ein.

* Durch das Hinzufügen eines `HTTP Strict-Transport-Security`-Headers (HSTS) wird sichergestellt, dass alle nachfolgenden Anforderungen vom Client nur über HTTPS erfolgen.

* Fügen Sie keinen Strict-Transport-Security-Header hinzu, oder wählen Sie ein entsprechendes `max-age` aus, wenn Sie SSL in Zukunft deaktivieren möchten.

Fügen Sie die Konfigurationsdatei */etc/nginx/proxy.conf* hinzu:

[!code-nginx[](linux-nginx/proxy.conf)]

Bearbeiten Sie die Konfigurationsdatei */etc/nginx/nginx.conf*. Das Beispiel enthält die Abschnitte `http` und `server` in einer Konfigurationsdatei.

[!code-nginx[](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a>Sichern von Nginx vor Clickjacking
Clickjacking ist eine böswillige Technik zum Sammeln der Klicks eines infizierten Benutzers. Clickjacking bringt das Opfer (Besucher) dazu, auf eine infizierte Seite zu klicken. Verwenden Sie X-FRAME-OPTIONS zum Sichern der Website.

Bearbeiten Sie die Datei *nginx.conf*:

```bash
sudo nano /etc/nginx/nginx.conf
```

Fügen Sie die Zeile `add_header X-Frame-Options "SAMEORIGIN";` hinzu, und speichern Sie die Datei. Starten Sie Nginx dann neu.

#### <a name="mime-type-sniffing"></a>MIME-Typermittlung

Dieser Header hindert die meisten Browser an der MIME-Ermittlung einer Antwort vom deklarierten Inhaltstyp weg, da der Header den Browser anweist, den Inhaltstyp der Antwort nicht zu überschreiben. Durch die Option `nosniff` wird der Inhalt vom Browser als „text/html“ gerendert, wenn der Server sagt, dass es sich bei diesem um „text/html“ handelt.

Bearbeiten Sie die Datei *nginx.conf*:

```bash
sudo nano /etc/nginx/nginx.conf
```

Fügen Sie die Zeile `add_header X-Content-Type-Options "nosniff";` hinzu, und speichern Sie die Datei. Starten Sie Nginx dann neu.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Nginx: Binary Releases: Official Debian/Ubuntu packages (Nginx: Binäre Releases – offizielle Debian/Ubuntu-Pakete)](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages)
* [Konfigurieren von ASP.NET Core zur Verwendung mit Proxyservern und Lastenausgleich](xref:host-and-deploy/proxy-load-balancer)
* [NGINX: Using the Forwarded header (NGINX: Verwenden des weitergeleiteten Headers)](https://www.nginx.com/resources/wiki/start/topics/examples/forwarded/)
