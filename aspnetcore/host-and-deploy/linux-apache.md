---
title: Hosten von ASP.NET Core unter Linux mit Apache
description: Erfahren Sie, wie Apache als reverse-Proxy-Server auf CentOS einrichten, um HTTP-Datenverkehr an eine ASP.NET Core-Web-app unter Kestrel weiterzuleiten.
author: spboyer
manager: wpickett
ms.author: spboyer
ms.custom: mvc
ms.date: 03/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/linux-apache
ms.openlocfilehash: 473585f1be180645395c14a154c9c017ca50edab
ms.sourcegitcommit: 74be78285ea88772e7dad112f80146b6ed00e53e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
---
# <a name="host-aspnet-core-on-linux-with-apache"></a>Hosten von ASP.NET Core unter Linux mit Apache

Von [Shayne Boyer](https://github.com/spboyer)

Mithilfe dieser Anleitung erfahren Sie, wie einrichten [Apache](https://httpd.apache.org/) als reverse-Proxy-Server auf [CentOS 7](https://www.centos.org/) zum Umleiten von HTTP-Datenverkehr an eine ASP.NET Core-Web-app unter [Kestrel](xref:fundamentals/servers/kestrel). Die [Mod_proxy Erweiterung](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) und zugehörigen Module Reverseproxy des Servers zu erstellen.

## <a name="prerequisites"></a>Erforderliche Komponenten

1. Server mit CentOS 7 muss ein Standardbenutzerkonto mit Sudo-Berechtigungen
2. ASP.NET Core-app

## <a name="publish-the-app"></a>Veröffentlichen der App

Veröffentlichen Sie die app als eine [eigenständige Bereitstellung](/dotnet/core/deploying/#self-contained-deployments-scd) im Release-Konfiguration für die Laufzeit CentOS 7 (`centos.7-x64`). Kopieren Sie den Inhalt von der *bin/Release/netcoreapp2.0/centos.7-x64/publish* Ordner auf dem Server mit SCP, FTP oder andere Dateiübertragungsmethode verwenden.

> [!NOTE]
> Unter einem Produktionsszenario Bereitstellung funktioniert ein fortlaufende Integration Workflow die Veröffentlichung der app, und kopieren die Ressourcen auf den Server. 

## <a name="configure-a-proxy-server"></a>Konfigurieren eines Proxyservers

Reverse-Proxy ist ein gemeinsames Setup dynamic Web-apps zu verarbeiten. Reverse-Proxy die HTTP-Anforderung beendet und an die ASP.NET app weiterleitet.

Ein Proxyserver ist eine Clientanforderungen an einen anderen Server anstelle von Anforderungen erfüllen selbst weitergeleitet. Ein Reverseproxy leitet Daten an ein festes Ziel meist im Auftrag beliebiger Clients weiter. In diesem Handbuch wird Apache konfiguriert, als den reverse-Proxy auf dem gleichen Server ausgeführt wird, dass Kestrel ASP.NET Core app bedient.

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

### <a name="install-apache"></a>Installieren von Apache

CentOS-Pakete, die jeweils neuesten stabilen Version zu aktualisieren:

```bash
sudo yum update -y
```

Installieren Sie die Apache-Webserver auf CentOS, mit einem einzelnen `yum` Befehl:

```bash
sudo yum -y install httpd mod_ssl
```

Beispielausgabe nach dem Ausführen des Befehls:

```bash
Downloading packages:
httpd-2.4.6-40.el7.centos.4.x86_64.rpm               | 2.7 MB  00:00:01
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
Installing : httpd-2.4.6-40.el7.centos.4.x86_64      1/1 
Verifying  : httpd-2.4.6-40.el7.centos.4.x86_64      1/1 

Installed:
httpd.x86_64 0:2.4.6-40.el7.centos.4

Complete!
```

> [!NOTE]
> In diesem Beispiel gibt die Ausgabe httpd.86_64 wieder, da die Version CentOS 7 64-Bit ist. Um zu prüfen, wo Apache installiert ist, führen Sie an einer Eingabeaufforderung `whereis httpd` aus.

### <a name="configure-apache-for-reverse-proxy"></a>Konfigurieren von Apache als Reverseproxy

Konfigurationsdateien für Apache befinden sich im Verzeichnis `/etc/httpd/conf.d/`. Alle Dateien mit der *.conf* Erweiterung wird verarbeitet, in alphabetischer Reihenfolge zusätzlich zu den in der Modul-Konfigurationsdateien `/etc/httpd/conf.modules.d/`, die Konfiguration enthält Dateien erforderlich, um Module zu laden.

Erstellen Sie eine Konfigurationsdatei mit dem Namen *hellomvc.conf*, für die app:

```
<VirtualHost *:80>
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/
    ServerName www.example.com
    ServerAlias *.example.com
    ErrorLog ${APACHE_LOG_DIR}hellomvc-error.log
    CustomLog ${APACHE_LOG_DIR}hellomvc-access.log common
</VirtualHost>
```

Die `VirtualHost` Block kann mehrere Male in eine oder mehrere Dateien auf einem Server angezeigt werden. In der vorangegangenen Konfigurationsdatei akzeptiert Apache öffentlichen Datenverkehr über Port 80. Die Domäne `www.example.com` ist gesendet werden, und die `*.example.com` Alias in der gleichen Website aufgelöst wird. Finden Sie unter [Namen-basierte virtuelle Host Unterstützung](https://httpd.apache.org/docs/current/vhosts/name-based.html) für Weitere Informationen. Anforderungen werden auf der Stammebene an Port 5000 des Servers 127.0.0.1 Proxyanforderungen. Für die bidirektionale Kommunikation `ProxyPass` und `ProxyPassReverse` erforderlich sind.

> [!WARNING]
> Fehler an eine echte [ServerName Richtlinie](https://httpd.apache.org/docs/current/mod/core.html#servername) in der **VirtualHost** Block macht Ihre app zu Sicherheitsrisiken. Unterdomäne Platzhalter Bindung (z. B. `*.example.com`) nicht dieses Sicherheitsrisiko darstellen, wenn Sie steuern, dass die gesamte übergeordnete Domäne (im Gegensatz zu `*.com`, anfällig ist). Weitere Informationen finden Sie unter [rfc7230 im Abschnitt 5.4](https://tools.ietf.org/html/rfc7230#section-5.4).

Protokollierung kann konfiguriert werden, pro `VirtualHost` mit `ErrorLog` und `CustomLog` Direktiven. `ErrorLog` ist der Speicherort, in dem der Server Fehler protokolliert, und `CustomLog` legt den Dateinamen und das Format der Protokolldatei. In diesem Fall ist dies dem Anforderungsinformationen protokolliert wird. Es wird nur eine Zeile für jede Anforderung ein.

Speichern Sie die Datei, und Testen der Konfiguration. Wenn alle Anforderungen durchgehen, muss die Antwort `Syntax [OK]` lauten.

```bash
sudo service httpd configtest
```

Starten Sie die Apache neu:

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

## <a name="monitoring-the-app"></a>Überwachen der app

Apache ist jetzt Setup zum Weiterleiten von Anforderungen an `http://localhost:80` der ASP.NET Core-App unter Kestrel am `http://127.0.0.1:5000`.  Apache allerdings ist nicht eingerichtet Kestrel verwalten. Verwendung *Systemd* , und erstellen Sie eine Dienstdatei zum Starten und überwachen die zugrunde liegenden Web-app. *SystemD* ist ein Initialisierungssystem, das viele leistungsstarke Features zum Starten, Beenden und Verwalten von Prozessen bereitstellt. 


### <a name="create-the-service-file"></a>Erstellen der Dienstdatei

Erstellen Sie die Dienstdefinitionsdatei:

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

Eine Beispiel-Service-Datei für die app:

```
[Unit]
Description=Example .NET Web API App running on CentOS 7

[Service]
WorkingDirectory=/var/aspnetcore/hellomvc
ExecStart=/usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
Restart=always
# Restart service after 10 seconds if dotnet service crashes
RestartSec=10
SyslogIdentifier=dotnet-example
User=apache
Environment=ASPNETCORE_ENVIRONMENT=Production 

[Install]
WantedBy=multi-user.target
```

> [!NOTE]
> **Benutzer** &mdash; Wenn der Benutzer *Apache* verwendet, wird nicht durch die Konfiguration der Benutzer zunächst erstellt und ordnungsgemäße den Besitz für Dateien angegeben werden muss.

> [!NOTE]
> Einige Werte (z. B. SQL-Verbindungszeichenfolgen) müssen für den Konfigurationsanbieter, lesen die Umgebungsvariablen mit Escapezeichen versehen werden. Verwenden Sie den folgenden Befehl aus, um einen ordnungsgemäß mit Escapezeichen versehene Wert für die Verwendung in der Konfigurationsdatei zu generieren:
>
> ```console
> systemd-escape "<value-to-escape>"
> ```

Speichern Sie die Datei, und aktivieren Sie den Dienst:

```bash
systemctl enable kestrel-hellomvc.service
```

Starten Sie den Dienst, und stellen Sie sicher, dass er ausgeführt wird:

```bash
systemctl start kestrel-hellomvc.service
systemctl status kestrel-hellomvc.service

● kestrel-hellomvc.service - Example .NET Web API App running on CentOS 7
    Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-hellomvc.service
            └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

Mit der reverse-Proxy konfiguriert und verwaltet Kestrel *Systemd*, die Web-app ist vollständig konfiguriert und zugegriffen werden kann, in einem Browser auf dem lokalen Computer am `http://localhost`. Überprüfen die Antwortheader, die **Server** Header angibt, dass die app ASP.NET Core durch Kestrel verarbeitet wird:

```
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a>Anzeigen von Protokollen

Seit der Web-app mit Kestrel wird verwaltet mit *Systemd*, Ereignisse und Prozesse werden auf einer zentralen Journal protokolliert. Allerdings umfasst diese Erfassung Einträge für alle Dienste und Prozesse, die von verwalteten *Systemd*. Verwenden Sie folgenden Befehl, um die `kestrel-hellomvc.service`-spezifischen Elemente anzuzeigen:

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

Geben Sie Optionen für die uhrzeitfilterung, mit dem Befehl. Verwenden Sie z. B. `--since today` für den aktuellen Tag filtern oder `--until 1 hour ago` auf der vorherigen Stunde Einträge angezeigt werden. Weitere Informationen finden Sie unter der [Man-Seite für Journalctl](https://www.unix.com/man-page/centos/1/journalctl/).

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a>Sichern die app

### <a name="configure-firewall"></a>Konfigurieren der Firewall

*Firewalld* ist eine dynamische Daemon zum Verwalten der Firewall mit Unterstützung für Netzwerkzonen. Ports und paketfilterung können immer noch mit Iptables verwaltet werden. *Firewalld* sollte standardmäßig installiert werden. `yum` kann verwendet werden, um das Paket zu installieren, oder stellen Sie sicher, dass es installiert ist.

```bash
sudo yum install firewalld -y
```

Verwendung `firewalld` nur den erforderlichen Ports für die app zu öffnen. In diesem Fall werden die Ports 80 und 443 verwendet. Die folgenden Befehle festlegen dauerhaft Ports 80 und 443 geöffnet:

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
```

Laden Sie die Firewall-Einstellungen erneut. Überprüfen Sie die verfügbaren Dienste und Ports in der Standardzone. Optionen sind verfügbar, durch Überprüfen `firewall-cmd -h`.

```bash 
sudo firewall-cmd --reload
sudo firewall-cmd --list-all
```

```bash
public (default, active)
interfaces: eth0
sources: 
services: dhcpv6-client
ports: 443/tcp 80/tcp
masquerade: no
forward-ports: 
icmp-blocks: 
rich rules: 
```

### <a name="ssl-configuration"></a>SSL-Konfiguration

So konfigurieren Sie die Apache für SSL, die *Mod_ssl* Modul verwendet wird. Wenn die *Httpd* -Modul wurde installiert, die *Mod_ssl* auch-Modul installiert wurde. Wenn sie mit dem wurde nicht installiert sind, verwenden `yum` die Konfiguration hinzuzufügen.

```bash
sudo yum install mod_ssl
```
Um SSL zu erzwingen, installieren die `mod_rewrite` Modul, um URLs zu aktivieren:

```bash
sudo yum install mod_rewrite
```

Ändern der *hellomvc.conf* Datei aktivieren die URLs und sichere Kommunikation über Port 443:

```
<VirtualHost *:80>
    RewriteEngine On
    RewriteCond %{HTTPS} !=on
    RewriteRule ^/?(.*) https://%{SERVER_NAME}/ [R,L]
</VirtualHost>

<VirtualHost *:443>
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/
    ErrorLog /var/log/httpd/hellomvc-error.log
    CustomLog /var/log/httpd/hellomvc-access.log common
    SSLEngine on
    SSLProtocol all -SSLv2
    SSLCipherSuite ALL:!ADH:!EXPORT:!SSLv2:!RC4+RSA:+HIGH:+MEDIUM:!LOW:!RC4
    SSLCertificateFile /etc/pki/tls/certs/localhost.crt
    SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
</VirtualHost>
```

> [!NOTE]
> In diesem Beispiel wird eine lokal generiertes Zertifikat verwendet. **SSLCertificateFile** muss die primäre Zertifikatdatei für den Domänennamen. **SSLCertificateKeyFile** sollten die Schlüsseldatei generiert, wenn CSR erstellt wird. **SSLCertificateChainFile** muss die Zwischenzertifikat-Datei (falls vorhanden), die von der Zertifizierungsstelle angegeben wurde.

Speichern Sie die Datei, und Testen der Konfiguration:

```bash
sudo service httpd configtest
```

Starten Sie die Apache neu:

```bash
sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a>Weitere Vorschläge für Apache

### <a name="additional-headers"></a>Zusätzliche Header

Um Ihre vor böswilligen Angriffen zu schützen, gibt es einige Header, die entweder geändert oder hinzugefügt werden soll. Sicherstellen, dass die `mod_headers` -Modul installiert ist:

```bash
sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking-attacks"></a>Sichere Apache Angriffen clickjacking

[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), auch bekannt als ein *UI Schadenersatz Angriff*, wird ein bösartigen Angriffs, in denen ein Besucher auf einen Link oder eine Schaltfläche auf einer anderen Seite verleitet ist als derzeit besuchte. Verwendung `X-FRAME-OPTIONS` zum Sichern der Site.

Bearbeiten der *httpd.conf* Datei:

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

Fügen Sie die Zeile `Header append X-FRAME-OPTIONS "SAMEORIGIN"`. Speichern Sie die Datei. Starten Sie Apache neu.

#### <a name="mime-type-sniffing"></a>MIME-Typermittlung

Die `X-Content-Type-Options` Header wird verhindert, dass Internet Explorer aus *MIME-Ermittlung* (einer Datei ermitteln `Content-Type` vom Inhalt der Datei). Wenn der Server bestimmt die `Content-Type` Header `text/html` mit der `nosniff` -Option festgelegt, Internet Explorer rendert den Inhalt als `text/html` unabhängig vom Inhalt der Datei.

Bearbeiten der *httpd.conf* Datei:

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

Fügen Sie die Zeile `Header set X-Content-Type-Options "nosniff"`. Speichern Sie die Datei. Starten Sie Apache neu.

### <a name="load-balancing"></a>Lastenausgleich 

Dieses Beispiel veranschaulicht das Einrichten und Konfigurieren von Apache unter CentOS 7 und Kestrel auf demselben Instanzcomputer. Damit keine einzelne Fehlerquelle; mit *Mod_proxy_balancer* und Ändern der **VirtualHost** entschied sich für die Verwaltung von mehreren Instanzen von webapps hinter dem Proxyserver Apache.

```bash
sudo yum install mod_proxy_balancer
```

In der Konfigurationsdatei dargestellt, unten eine zusätzliche Instanz von der `hellomvc` app ist für Port 5001 ausführen. Die *Proxy* Abschnitt mit einem Lastenausgleichsmodul-Konfiguration mit zwei Membern festgelegt ist, für den Lastenausgleich *Byrequests*.

```
<VirtualHost *:80>
    RewriteEngine On
    RewriteCond %{HTTPS} !=on
    RewriteRule ^/?(.*) https://%{SERVER_NAME}/ [R,L]
</VirtualHost>

<VirtualHost *:443>
    ProxyPass / balancer://mycluster/ 

    ProxyPassReverse / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5001/

    <Proxy balancer://mycluster>
        BalancerMember http://127.0.0.1:5000
        BalancerMember http://127.0.0.1:5001 
        ProxySet lbmethod=byrequests
    </Proxy>

    <Location />
        SetHandler balancer
    </Location>
    ErrorLog /var/log/httpd/hellomvc-error.log
    CustomLog /var/log/httpd/hellomvc-access.log common
    SSLEngine on
    SSLProtocol all -SSLv2
    SSLCipherSuite ALL:!ADH:!EXPORT:!SSLv2:!RC4+RSA:+HIGH:+MEDIUM:!LOW:!RC4
    SSLCertificateFile /etc/pki/tls/certs/localhost.crt
    SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
</VirtualHost>
```

### <a name="rate-limits"></a>Begrenzung der Bandbreite
Mit *Mod_ratelimit*, im Lieferumfang der *Httpd* Modul, die Bandbreite von Clients beschränkt werden kann:

```bash
sudo nano /etc/httpd/conf.d/ratelimit.conf
```
Die Beispieldatei begrenzt die Bandbreite als 600 KB/Sek., unter dem Stammspeicherort:

```
<IfModule mod_ratelimit.c>
    <Location />
        SetOutputFilter RATE_LIMIT
        SetEnv rate-limit 600
    </Location>
</IfModule>
```
