---
title: Hosten von ASP.NET Core unter Linux mit Nginx
author: rick-anderson
description: Beschreibt, wie beim Einrichten des Nginx als Reverseproxy für Ubuntu 16.04 zum Weiterleiten von HTTP-Datenverkehr an eine ASP.NET Core-Web-app auf Kestrel ausgeführt wird.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/linux-nginx
ms.openlocfilehash: fe772203e5e3fceb7489e0a5866f60ea914b7329
ms.sourcegitcommit: 74be78285ea88772e7dad112f80146b6ed00e53e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
---
# <a name="host-aspnet-core-on-linux-with-nginx"></a>Hosten von ASP.NET Core unter Linux mit Nginx

Von [Sourabh Shirhatti](https://twitter.com/sshirhatti)

Dieser Leitfaden erläutert das Einrichten einer produktionsbereiten ASP.NET Core-Umgebungen auf einem Ubuntu 16.04-Server.

> [!NOTE]
> Für Ubuntu 14.04 *Supervisord* wird als Lösung für die Überwachung der Kestrel empfohlen. *Systemd* für Ubuntu 14.04 nicht verfügbar. [Siehe die vorherige Version dieses Dokuments](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).

In diesem Leitfaden:

* Fügt eine vorhandene ASP.NET Core app hinter einem reverse-Proxy-Server vor.
* Der reverse-Proxy-Server zum Weiterleiten von Anforderungen an den Webserver Kestrel eingerichtet.
* Stellt sicher, dass die Web-app beim Start als Daemon ausgeführt wird.
* Konfiguriert eine Prozess-Verwaltungstool zum Neustart der Web-app helfen.

## <a name="prerequisites"></a>Erforderliche Komponenten

1. Zugriff auf einen Ubuntu 16.04-Server mit einem Standardbenutzerkonto mit sudo-Berechtigung
1. Eine vorhandene ASP.NET Core-app

## <a name="copy-over-the-app"></a>Kopieren Sie die app

Führen Sie [Dotnet veröffentlichen](/dotnet/core/tools/dotnet-publish) aus der Umgebung Dev auf eine app in eine geschlossene Verzeichnis zu verpacken, die auf dem Server ausgeführt werden kann.

Kopieren Sie die ASP.NET Core-app auf den Server, die mit beliebigen Tool in der Organisation Workflow (z. B. SCP, FTP) integriert wird. Testen Sie die App wie im folgenden Beispiel:

* Führen Sie über die Befehlszeile `dotnet <app_assembly>.dll`.
* Navigieren Sie in einem Browser zu `http://<serveraddress>:<port>` und überprüfen Sie, dass die App unter Linux funktioniert. 
 
## <a name="configure-a-reverse-proxy-server"></a>Konfigurieren eines Reverseproxyservers

Reverse-Proxy ist ein gemeinsames Setup dynamic Web-apps zu verarbeiten. Reverseproxy beendet die HTTP-Anforderung und ASP.NET Core-App weiterleitet.

### <a name="why-use-a-reverse-proxy-server"></a>Gründe für das Verwenden eines Reverseproxyservers:

Kestrel eignet sich hervorragend für dynamische Inhalte von ASP.NET Core bedient. Die Web-Funktionen bedient ist jedoch als umfangreichen Features wie z. B. IIS, Apache oder Nginx-Server nicht möglich. Ein reverse-Proxy-Server kann die Arbeit wie statische Inhalte Zwischenspeichern von Anforderungen, Komprimieren von Anforderungen und aus dem HTTP-Server SSL-Tunnelabschluss auslagern. Ein Reverseproxyserver kann sich auf einem dedizierten Computer befinden oder zusammen mit einem HTTP-Server bereitgestellt werden.

Für diesen Leitfaden wird eine einzelne Instanz von Nginx verwendet. Diese wird auf demselben Server ausgeführt, zusammen mit dem HTTP-Server. Eine andere Installation kann basierend auf Anforderungen ausgewählt werden.

Da Anforderungen von Reverseproxy weitergeleitet werden, verwenden Sie die Middleware weitergeleitet Header aus der [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) Paket. Die Middleware Updates der `Request.Scheme`unter Verwendung der `X-Forwarded-Proto` -Header, damit diese umleitungs-URIs und andere Sicherheitsrichtlinien ordnungsgemäß funktioniert.

Wenn Sie einen beliebigen Typ von Authentifizierung-Middleware zu verwenden, muss der Header weitergeleitet Middleware zuerst ausgeführt. Dieser Anordnung wird sichergestellt, dass die authentifizierungsmiddleware die Headerwerte beansprucht. außerdem Generieren der richtigen umleitungs-URIs.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Aufrufen der [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) Methode im `Startup.Configure` vor dem Aufruf [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) oder ähnliche authentifizierungsmiddleware Schema. Konfigurieren Sie die Middleware zum Weiterleiten der `X-Forwarded-For` und `X-Forwarded-Proto` Header:

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Aufrufen der [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) Methode im `Startup.Configure` vor dem Aufruf [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) und [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) oder ähnliche Authentifizierungsschema Middleware. Konfigurieren Sie die Middleware zum Weiterleiten der `X-Forwarded-For` und `X-Forwarded-Proto` Header:

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

Wenn kein [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) werden angegeben, um die Middleware, die Standardheader zum Weiterleiten sind `None`.

Möglicherweise ist zusätzliche Konfiguration für Apps erforderlich, die hinter Proxyservern und Lastenausgleichsmodulen (Load Balancer) gehostet werden. Weitere Informationen hierzu feinden Sie unter [Konfigurieren von ASP.NET Core zur Verwendung mit Proxyservern und Lastenausgleich](xref:host-and-deploy/proxy-load-balancer).

### <a name="install-nginx"></a>Installieren von Nginx

```bash
sudo apt-get install nginx
```

> [!NOTE]
> Wenn optionale Nginx-Module installiert werden, kann das Erstellen von Nginx aus Quelle erforderlich sein.

Verwenden Sie `apt-get` zum Installieren von Nginx. Das Installationsprogramm erstellt ein System V-Initialisierungsskript, das Nginx beim Systemstart als Daemon ausführt. Da Nginx zum ersten Mal installiert wurde, starten Sie es explizit, indem Sie Folgendes ausführen:

```bash
sudo service nginx start
```

Stellen Sie sicher, dass ein Browser die Standardangebotsseite für Nginx anzeigt.

### <a name="configure-nginx"></a>Konfigurieren von Nginx

Ändern Sie zum Konfigurieren von Nginx als Reverseproxy für forward-Anforderungen an Ihre app ASP.NET Core */etc/nginx/sites-available/default*. Öffnen Sie die Datei in einem Text-Editor und ersetzen Sie den Inhalt durch den Folgendes:

```nginx
server {
    listen        80;
    server_name   example.com *.example.com;
    location / {
        proxy_pass         http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header   Upgrade $http_upgrade;
        proxy_set_header   Connection keep-alive;
        proxy_set_header   Host $http_host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

Wenn kein `server_name` Übereinstimmungen Nginx verwendet den Standardserver an. Wenn kein Server definiert ist, ist der erste Server in der Konfigurationsdatei den Standardserver an. Fügen Sie als bewährte Methode einen bestimmte Standardserver für der in der Konfigurationsdatei der 444 Statuscode zurückgibt. Ein Standard Server Configuration-Beispiel ist:

```nginx
server {
    listen   80 default_server;
    # listen [::]:80 default_server deferred;
    return   444;
}
```

Mit dem vorherigen und Konfigurationsserver akzeptiert Nginx öffentlichen Datenverkehr über Port 80 mit Hostheader `example.com` oder `*.example.com`. Anforderungen, die nicht mit dem diese Hosts wird nicht an die Kestrel weitergeleitet abrufen. Nginx leitet die entsprechenden Anforderungen an Kestrel am `http://localhost:5000`. Finden Sie unter [wie Nginx eine Anforderung verarbeitet](https://nginx.org/docs/http/request_processing.html) für Weitere Informationen.

> [!WARNING]
> Fehler beim Geben Sie einer echte [Server_name Richtlinie](https://nginx.org/docs/http/server_names.html) macht Sie Ihre app zu Sicherheitslücken. Unterdomäne Platzhalter Bindung (z. B. `*.example.com`) nicht dieses Sicherheitsrisiko darstellen, wenn Sie steuern, dass die gesamte übergeordnete Domäne (im Gegensatz zu `*.com`, anfällig ist). Weitere Informationen finden Sie unter [rfc7230 im Abschnitt 5.4](https://tools.ietf.org/html/rfc7230#section-5.4).

Nachdem die Nginx-Konfiguration eingerichtet ist, führen Sie `sudo nginx -t` überprüfen Sie die Syntax der Konfigurationsdateien. Wenn die Datei Konfigurationstest erfolgreich ist, erzwingen Sie Nginx, durch Ausführen der Änderungen zu übernehmen `sudo nginx -s reload`.

## <a name="monitoring-the-app"></a>Überwachen der app

Der Server ist zum Weiterleiten von Anforderungen an Setup `http://<serveraddress>:80` sich bei der ASP.NET Core Ausführen einer app im Kestrel am `http://127.0.0.1:5000`. Nginx allerdings ist nicht eingerichtet Kestrel verwalten. *Systemd* können verwendet werden, um eine Dienstdatei zum Starten und überwachen die zugrunde liegenden Web-app erstellen. *SystemD* ist ein Initialisierungssystem, das viele leistungsstarke Features zum Starten, Beenden und Verwalten von Prozessen bereitstellt. 

### <a name="create-the-service-file"></a>Erstellen der Dienstdatei

Erstellen Sie die Dienstdefinitionsdatei:

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

Im folgenden finden eine Dienst-Beispieldatei für die app:

```ini
[Unit]
Description=Example .NET Web API App running on Ubuntu

[Service]
WorkingDirectory=/var/aspnetcore/hellomvc
ExecStart=/usr/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
Restart=always
RestartSec=10  # Restart service after 10 seconds if dotnet service crashes
SyslogIdentifier=dotnet-example
User=www-data
Environment=ASPNETCORE_ENVIRONMENT=Production
Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false

[Install]
WantedBy=multi-user.target
```

**Hinweis:** Wenn der Benutzer *Www-Daten* verwendet, wird nicht durch die Konfiguration der benutzerdefinierten zuerst erstellt und ordnungsgemäße den Besitz für Dateien angegeben werden muss.
**Hinweis:** Linux verfügt über ein Dateisystem für die Groß-/Kleinschreibung beachtet. Festlegen ASPNETCORE_ENVIRONMENT "Produktion" in der Konfigurationsdatei gesucht *"appSettings". Production.JSON*, nicht *appsettings.production.json*.

> [!NOTE]
> Einige Werte (z. B. SQL-Verbindungszeichenfolgen) müssen für den Konfigurationsanbieter, lesen die Umgebungsvariablen mit Escapezeichen versehen werden. Verwenden Sie den folgenden Befehl aus, um einen ordnungsgemäß mit Escapezeichen versehene Wert für die Verwendung in der Konfigurationsdatei zu generieren:
>
> ```console
> systemd-escape "<value-to-escape>"
> ```

Speichern Sie die Datei, und aktivieren Sie den Dienst.

```bash
systemctl enable kestrel-hellomvc.service
```

Starten Sie den Dienst, und stellen Sie sicher, dass er ausgeführt wird.

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

Der reverse-Proxy konfiguriert und Kestrel über Systemd verwaltet, die Web-app ist vollständig konfiguriert und zugegriffen werden kann, in einem Browser auf dem lokalen Computer am `http://localhost`. Es ist auch von einem Remotecomputer aus, wobei jeder Firewall, die blockiert werden möglicherweise zugegriffen werden kann. Überprüfen die Antwortheader, die `Server` -Header zeigt die ASP.NET Core-app von Kestrel gesendet werden.

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a>Anzeigen von Protokollen

Seit der Web-app mit Kestrel wird verwaltet mit `systemd`, alle Ereignisse und Prozesse werden auf einer zentralen Journal protokolliert. Dieses Journal enthält jedoch alle Einträge für alle Dienste und Prozesse, die von `systemd` verwaltet werden. Verwenden Sie folgenden Befehl, um die `kestrel-hellomvc.service`-spezifischen Elemente anzuzeigen:

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

Für weiteres Filtern können Zeitoptionen wie `--since today`, `--until 1 hour ago` oder eine Kombination aus diesen die Menge der zurückgegebenen Einträge verringern.

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a>Sichern die app

### <a name="enable-apparmor"></a>Aktivieren von AppArmor

Linux Security Modules (LSM) ist ein Framework, das Teil der Linux-Kernel seit Linux 2.6 ist. LSM unterstützt verschiedene Implementierungen von Sicherheitsmodulen. [AppArmor](https://wiki.ubuntu.com/AppArmor) ist ein LSM, das ein obligatorisches Zugriffssteuerungssystem implementiert, das das Programm auf einen beschränkten Satz von Ressourcen begrenzt. Versichern Sie sich, dass AppArmor aktiviert und ordnungsgemäß konfiguriert ist.

### <a name="configuring-the-firewall"></a>Konfigurieren der firewall

Schließen Sie alle externen Ports, die nicht verwendet werden. „Uncomplicated Firewall“ (UFW) stellt ein Front-End für `iptables` bereit, indem eine Befehlszeilenschnittstelle zum Konfigurieren der Firewall bereitgestellt wird. Überprüfen Sie, ob `ufw` zum Zulassen von Datenverkehr auf alle benötigten Ports konfiguriert ist.

```bash
sudo apt-get install ufw
sudo ufw enable

sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### <a name="securing-nginx"></a>Sichern von Nginx

Die Standardverteilung von Nginx aktiviert SSL nicht. Erstellen Sie aus der Quelle, um zusätzliche Sicherheitsfeatures zu aktivieren.

#### <a name="download-the-source-and-install-the-build-dependencies"></a>Herunterladen der Quelle und Installieren der Buildabhängigkeiten

```bash
# Install the build dependencies
sudo apt-get update
sudo apt-get install build-essential zlib1g-dev libpcre3-dev libssl-dev libxslt1-dev libxml2-dev libgd2-xpm-dev libgeoip-dev libgoogle-perftools-dev libperl-dev

# Download Nginx 1.10.0 or latest
wget http://www.nginx.org/download/nginx-1.10.0.tar.gz
tar zxf nginx-1.10.0.tar.gz
```

#### <a name="change-the-nginx-response-name"></a>Ändern des Namens der Nginx-Antwort

Bearbeiten Sie *src/http/ngx_http_header_filter_module.c*:

```c
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-the-options-and-build"></a>Konfigurieren der Optionen und des Builds

Die PCRE-Bibliothek wird für reguläre Ausdrücke benötigt. Reguläre Ausdrücke werden im Verzeichnis des Speicherorts von „gx_http_rewrite_module“ verwendet. Durch „http_ssl_module“ wird die Unterstützung für HTTPS-Protokolle hinzugefügt.

Erwägen Sie eine Web-app-Firewall wie *ModSecurity* um die app besser zu schützen.

```bash
./configure
--with-pcre=../pcre-8.38
--with-zlib=../zlib-1.2.8
--with-http_ssl_module
--with-stream
--with-mail=dynamic
```

#### <a name="configure-ssl"></a>Konfigurieren von SSL

* Konfigurieren Sie den Server, die HTTPS-Datenverkehr über Port Lauschen `443` durch Angeben eines gültigen Zertifikats von einer vertrauenswürdigen Zertifizierungsstelle (CA) ausgegeben.

* Härten die Sicherheit durch die Verwendung einige Methoden, die in den in der folgenden */etc/nginx/nginx.conf* Datei. Die Beispiele schließen das Auswählen einer stärkeren Verschlüsselung und das Weiterleiten allen Datenverkehrs über HTTP auf HTTPS ein.

* Durch das Hinzufügen eines `HTTP Strict-Transport-Security`-Headers (HSTS) wird sichergestellt, dass alle nachfolgenden Anforderungen vom Client nur über HTTPS erfolgen.

* Fügen Sie nicht den Strict-Transport-Security-Header hinzu, oder wählen Sie eine entsprechende `max-age` Wenn SSL wird in der Zukunft deaktiviert werden.

Fügen Sie die Konfigurationsdatei */etc/nginx/proxy.conf* hinzu:

[!code-nginx[](linux-nginx/proxy.conf)]

Bearbeiten Sie die Konfigurationsdatei */etc/nginx/nginx.conf*. Das Beispiel enthält die Abschnitte `http` und `server` in einer Konfigurationsdatei.

[!code-nginx[](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a>Sichern von Nginx vor Clickjacking
Clickjacking ist eine böswillige Technik zum Sammeln der Klicks eines infizierten Benutzers. Clickjacking bringt das Opfer (Besucher) dazu, auf eine infizierte Seite zu klicken. Verwendet X-FRAME-OPTIONS zum Sichern der Site.

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
