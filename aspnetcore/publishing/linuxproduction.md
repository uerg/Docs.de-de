---
title: Hosten von ASP.NET Core unter Linux mit Nginx
description: "Beschreibt das Einrichten von Nginx als Reverseproxy für Ubuntu 16.04, um den HTTP-Datenverkehr auf eine ASP.NET Core-Webanwendung weiterzuleiten, die auf Kestrel ausgeführt wird."
keywords: ASP.NET Core, Linux, Nginx, Ubuntu, Reverseproxy
author: rick-anderson
ms.author: riande
manager: wpickett
ms.date: 08/21/2017
ms.topic: article
ms.assetid: 1c33e576-33de-481a-8ad3-896b94fde0e3
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/linuxproduction
ms.openlocfilehash: 7c7b949fc922c605aa4554c158200a4123c4eb1c
ms.sourcegitcommit: fc98e93464ccf37d9904e89a71cdddbd4bbdb86a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/05/2018
---
# <a name="set-up-a-hosting-environment-for-aspnet-core-on-linux-with-nginx-and-deploy-to-it"></a>Einrichten einer Hostumgebung für ASP.NET Core unter Linux mit Nginx und Bereitstellen auf diese

Von [Sourabh Shirhatti](https://twitter.com/sshirhatti)

Dieser Leitfaden erläutert das Einrichten einer produktionsbereiten ASP.NET Core-Umgebungen auf einem Ubuntu 16.04-Server.

**Hinweis:** Für Ubuntu 14.04 wird „Supervisored“ (überwacht) für die Überwachung des Kestrel-Prozesses empfohlen. „SystemD“ ist auf Ubuntu 14.04 nicht verfügbar. [Hier finden Sie die vorherige Version dieses Dokuments.](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md)

In diesem Leitfaden:

* Wird eine bestehende ASP.NET Core-Anwendung hinter einem Reverseproxyserver eingefügt
* Wird der Reverseproxyserver eingerichtet, um Anforderungen an den Kestrel-Webserver weiterzuleiten
* Wird sichergestellt, dass die Webanwendung beim Start als Daemon ausgeführt wird
* Wird ein Prozessverwaltungstool konfiguriert, das die Webanwendung beim Neustarten unterstützt

## <a name="prerequisites"></a>Erforderliche Komponenten

1. Zugriff auf einen Ubuntu 16.04-Server mit einem Standardbenutzerkonto mit sudo-Berechtigung
2. Eine bestehende ASP.NET Core-Anwendung

## <a name="copy-over-your-app"></a>Kopieren Ihrer App

Führen Sie `dotnet publish` in der Bereitstellungsumgebung aus, um eine App in ein eigenständiges Verzeichnis zu verpacken, das auf dem Server ausgeführt werden kann.

Kopieren Sie die ASP.NET Core-App auf den Server, indem Sie ein beliebiges Tool (SCP, FTP usw.) verwenden, das in Ihren Workflow integriert ist. Testen Sie die App wie im folgenden Beispiel:
 - Führen Sie `dotnet yourapp.dll` über die Befehlszeile aus.
 - Navigieren Sie in einem Browser zu `http://<serveraddress>:<port>` und überprüfen Sie, dass die App unter Linux funktioniert. 
 
## <a name="configure-a-reverse-proxy-server"></a>Konfigurieren eines Reverseproxyservers

Ein Reverseproxy ist ein allgemeines Setup zum Verarbeiten von dynamischen Webanwendungen. Ein Reverseproxy beendet die HTTP-Anforderung und leitet diese an die ASP.NET Core-Anwendung weiter.

### <a name="why-use-a-reverse-proxy-server"></a>Gründe für das Verwenden eines Reverseproxyservers:

Kestrel eignet sich hervorragend zum Verarbeiten von dynamischen Inhalten von ASP.NET Core, die Features für das Webserving sind jedoch weniger umfangreich als bei Servern wie IIS, Apache oder Nginx. Ein Reverseproxyserver kann Arbeiten wie das Verarbeiten von statischen Inhalten, das Zwischenspeichern und Komprimieren von Anforderungen und das Beenden von SSL vom HTTP-Server auslagern. Ein Reverseproxyserver kann sich auf einem dedizierten Computer befinden oder zusammen mit einem HTTP-Server bereitgestellt werden.

Für diesen Leitfaden wird eine einzelne Instanz von Nginx verwendet. Diese wird auf demselben Server ausgeführt, zusammen mit dem HTTP-Server. Je nach Ihren Anforderungen können Sie auch ein anderes Setup auswählen.

Da Anforderungen vom Reverseproxy weitergeleitet werden, verwenden Sie die `ForwardedHeaders`-Middleware aus dem `Microsoft.AspNetCore.HttpOverrides`-Paket. Diese Middleware aktualisiert `Request.Scheme` mithilfe des `X-Forwarded-Proto`-Headers, sodass Umleitungs-URIs und andere Sicherheitsrichtlinien ordnungsgemäß funktionieren.

Beim Einrichten eines Reverseproxyservers wird `UseForwardedHeaders` für das erste Ausführen der Authentifizierungsmiddleware benötigt. Durch diese Reihenfolge wird sichergestellt, dass die Authentifizierungsmiddleware die betroffenen Werte nutzen und richtige Umleitungs-URIs generieren kann.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Rufen Sie die `UseForwardedHeaders`-Methode auf (in der `Configure`-Methode von *Startup.cs*), bevor Sie `UseAuthentication` oder eine ähnliche Middleware für das Authentifizierungsschema aufrufen:

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Rufen Sie die `UseForwardedHeaders`-Methode auf (in der `Configure`-Methode von *Startup.cs*), bevor Sie `UseIdentity` und `UseFacebookAuthentication` oder eine ähnliche Middleware für das Authentifizierungsschema aufrufen:

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

### <a name="install-nginx"></a>Installieren von Nginx

```bash
sudo apt-get install nginx
```

> [!NOTE]
> Wenn Sie optionale Nginx-Module installieren möchten, müssen Sie Nginx aus der Quelle erstellen.

Verwenden Sie `apt-get` zum Installieren von Nginx. Das Installationsprogramm erstellt ein System V-Initialisierungsskript, das Nginx beim Systemstart als Daemon ausführt. Da Nginx zum ersten Mal installiert wurde, starten Sie es explizit, indem Sie Folgendes ausführen:

```bash
sudo service nginx start
```

Stellen Sie sicher, dass ein Browser die Standardangebotsseite für Nginx anzeigt.

### <a name="configure-nginx"></a>Konfigurieren von Nginx

Ändern Sie `/etc/nginx/sites-available/default`, um Nginx als Reverseproxy für das Weiterleiten von Anforderungen zu Ihrer ASP.NET Core-Anwendung zu konfigurieren. Öffnen Sie die Datei in einem Text-Editor und ersetzen Sie den Inhalt durch den Folgendes:

```nginx
server {
    listen 80;
    location / {
        proxy_pass http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection keep-alive;
        proxy_set_header Host $http_host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

Diese Nginx-Konfigurationsdatei leitet eingehenden öffentlichen Datenverkehr von Port `80` an Port `5000` weiter.

Sobald Sie die Änderungen an Ihrer Nginx-Konfiguration abgeschlossen haben, können Sie `sudo nginx -t` ausführen, um die Syntax Ihrer Konfigurationsdateien zu überprüfen. Wenn der Test der Konfigurationsdatei erfolgreich ist, können Sie Nginx die Änderungen durch Ausführen von `sudo nginx -s reload` übernehmen lassen.

## <a name="monitoring-our-application"></a>Überwachen Ihrer Anwendung

Nginx ist nun dafür eingerichtet, Anforderungen an `http://yourhost:80` an die ASP.NET Core-Anwendung weiterzuleiten, die unter Kestrel auf `http://127.0.0.1:5000` ausgeführt wird. Nginx wurde jedoch nicht dafür eingerichtet, den Kestrel-Prozess zu verwalten. Sie können *SystemD* verwenden und eine Dienstdatei erstellen, um die zugrunde liegende Web-App zu starten und zu überwachen. *SystemD* ist ein Initialisierungssystem, das viele leistungsstarke Features zum Starten, Beenden und Verwalten von Prozessen bereitstellt. 

### <a name="create-the-service-file"></a>Erstellen der Dienstdatei

Erstellen Sie die Dienstdefinitionsdatei:

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

Beim Folgenden handelt es sich um eine Beispieldienstdatei für die Anwendung:

```ini
[Unit]
Description=Example .NET Web API Application running on Ubuntu

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

**Hinweis:** Wenn der Benutzer *www-data* durch Ihre Konfiguration nicht verwendet wird, muss der hier definierte Benutzer zunächst erstellt, und ihm muss die richtige Inhaberschaft für die Dateien erteilt werden.

Speichern Sie die Datei, und aktivieren Sie den Dienst.

```bash
systemctl enable kestrel-hellomvc.service
```

Starten Sie den Dienst, und versichern Sie sich, dass er ausgeführt wird.

```
systemctl start kestrel-hellomvc.service
systemctl status kestrel-hellomvc.service

● kestrel-hellomvc.service - Example .NET Web API Application running on Ubuntu
    Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-hellomvc.service
            └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

Wenn der Reverseproxy konfiguriert ist und Kestrel durch „SystemD“ verwaltet wird, ist die Webanwendung vollständig konfiguriert. Auf diese kann dann über einen Browser auf dem lokalen Computer unter `http://localhost` zugegriffen werden. Der Zugriff ist auch von einem Remotecomputer aus möglich, sofern keine Firewall diesen blockiert. Beim Überprüfen der Antwortheader zeigt der Header `Server` die ASP.NET Core-Anwendung an, die von Kestrel verarbeitet wird.

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a>Anzeigen von Protokollen

Da die Webanwendung, die Kestrel verwendet, von `systemd` verwaltet wird, werden alle Ereignisse und Prozesse in einem zentralen Journal protokolliert. Dieses Journal enthält jedoch alle Einträge für alle Dienste und Prozesse, die von `systemd` verwaltet werden. Verwenden Sie folgenden Befehl, um die `kestrel-hellomvc.service`-spezifischen Elemente anzuzeigen:

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

Für weiteres Filtern können Zeitoptionen wie `--since today`, `--until 1 hour ago` oder eine Kombination aus diesen die Menge der zurückgegebenen Einträge verringern.

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-our-application"></a>Sichern Ihrer Anwendung

### <a name="enable-apparmor"></a>Aktivieren von AppArmor

Bei Linux Security Modules (LSM) handelt es sich um ein Framework, das seit Linux 2.6 Teil des Linux-Kernels ist. LSM unterstützt verschiedene Implementierungen von Sicherheitsmodulen. [AppArmor](https://wiki.ubuntu.com/AppArmor) ist ein LSM, das ein obligatorisches Zugriffssteuerungssystem implementiert, das das Programm auf einen beschränkten Satz von Ressourcen begrenzt. Versichern Sie sich, dass AppArmor aktiviert und ordnungsgemäß konfiguriert ist.

### <a name="configuring-our-firewall"></a>Konfigurieren Ihrer Firewall

Schließen Sie alle externen Ports, die nicht verwendet werden. „Uncomplicated Firewall“ (UFW) stellt ein Front-End für `iptables` bereit, indem eine Befehlszeilenschnittstelle zum Konfigurieren der Firewall bereitgestellt wird. Überprüfen Sie, ob `ufw` konfiguriert ist, um Datenverkehr für alle Ports zuzulassen, die Sie benötigen.

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

# Download nginx 1.10.0 or latest
wget http://www.nginx.org/download/nginx-1.10.0.tar.gz
tar zxf nginx-1.10.0.tar.gz
```

#### <a name="change-the-nginx-response-name"></a>Ändern des Namens der Nginx-Antwort

Bearbeiten Sie *src/http/ngx_http_header_filter_module.c*:

```c
static char ngx_http_server_string[] = "Server: Your Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Your Web Server" CRLF;
```

#### <a name="configure-the-options-and-build"></a>Konfigurieren der Optionen und des Builds

Die PCRE-Bibliothek wird für reguläre Ausdrücke benötigt. Reguläre Ausdrücke werden im Verzeichnis des Speicherorts von „gx_http_rewrite_module“ verwendet. Durch „http_ssl_module“ wird die Unterstützung für HTTPS-Protokolle hinzugefügt.

Erwägen Sie das Verwenden einer Webanwendungsfirewall wie *ModSecurity*, um Ihre Anwendung zu härten.

```bash
./configure
--with-pcre=../pcre-8.38
--with-zlib=../zlib-1.2.8
--with-http_ssl_module
--with-stream
--with-mail=dynamic
```

#### <a name="configure-ssl"></a>Konfigurieren von SSL

* Konfigurieren Sie Ihren Server, damit dieser dem HTTPS-Datenverkehr auf Port `443` lauscht, indem Sie ein gültiges Zertifikat angeben, das von einer vertrauenswürdigen Zertifizierungsstelle (CA) ausgestellt wurde.

* Härten Sie Ihre Sicherheit, indem Sie einige der in der folgenden Datei (*/etc/nginx/nginx.conf*) dargestellten Methoden verwenden. Die Beispiele schließen das Auswählen einer stärkeren Verschlüsselung und das Weiterleiten allen Datenverkehrs über HTTP auf HTTPS ein.

* Durch das Hinzufügen eines `HTTP Strict-Transport-Security`-Headers (HSTS) wird sichergestellt, dass alle nachfolgenden Anforderungen vom Client nur über HTTPS erfolgen.

* Fügen Sie keinen Strict-Transport-Security-Header hinzu, oder wählen Sie ein zuverlässiges `max-age`, wenn Sie SSL in Zukunft deaktivieren möchten.

Fügen Sie die Konfigurationsdatei */etc/nginx/proxy.conf* hinzu:

[!code-nginx[Main](linuxproduction/proxy.conf)]

Bearbeiten Sie die Konfigurationsdatei */etc/nginx/nginx.conf*. Das Beispiel enthält die Abschnitte `http` und `server` in einer Konfigurationsdatei.

[!code-nginx[Main](../publishing/linuxproduction/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a>Sichern von Nginx vor Clickjacking
Clickjacking ist eine böswillige Technik zum Sammeln der Klicks eines infizierten Benutzers. Clickjacking bringt das Opfer (Besucher) dazu, auf eine infizierte Seite zu klicken. Verwenden Sie „X-FRAME-OPTIONS“ zum Sichern Ihrer Website.

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
