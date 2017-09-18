---
title: Hosten von ASP.NET Core unter Linux mit Apache
description: "Informationen zum Einrichten von Apache als Reverseproxyserver für CentOS, um HTTP-Datenverkehr an eine ASP.NET Core-Webanwendung weiterzuleiten, die unter Kestrel ausgeführt wird."
keywords: ASP.NET Core, Apache, CentOS, Reverseproxy, Linux, mod_proxy, HTTPD, Hosting
author: spboyer
ms.author: spboyer
manager: wpickett
ms.date: 10/19/2016
ms.topic: article
ms.assetid: fa9b0cb7-afb3-4361-9e7e-33afffeaca0c
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/apache-proxy
ms.openlocfilehash: 9dc22ea20a6ae2e2477f9e6db95ddabecc038dcb
ms.sourcegitcommit: f8f6b5934bd071a349f5bc1e389365c52b1c00fa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/14/2017
---
# <a name="set-up-a-hosting-environment-for-aspnet-core-on-linux-with-apache-and-deploy-to-it"></a>Einrichten einer Hostumgebung für ASP.NET Core unter Linux mit Apache und anschließende Bereitstellung

Von [Shayne Boyer](https://github.com/spboyer)

Apache ist ein sehr beliebter HTTP-Server, der als Proxy konfiguriert werden kann, um HTTP-Datenverkehr vergleichbar mit nginx umzuleiten. In dieser Anleitung erfahren Sie, wie Sie Apache unter CentOS 7 einrichten und als Reverseproxy nutzen, um eingehende Verbindungen anzunehmen und sie an die unter Kestrel ausgeführte ASP.NET Core-Anwendung umzuleiten. Zu diesem Zweck verwenden wir die Erweiterung *mod_proxy* und andere zugehörige Apache-Module.

## <a name="prerequisites"></a>Erforderliche Komponenten

1. Zugriff auf einen Server, auf dem CentOS 7 ausgeführt wird, mit einem Standardbenutzerkonto mit sudo-Berechtigung
2. Eine bestehende ASP.NET Core-Anwendung 

## <a name="publish-your-application"></a>Veröffentlichen der Anwendung

Führen Sie `dotnet publish -c Release` in Ihrer Entwicklungsumgebung, um Ihre Anwendung in einem eigenständigen Verzeichnis zu packen, das auf Ihrem Server ausgeführt werden kann. Die veröffentlichte Anwendung muss dann über SCP, FTP oder eine andere Dateiübertragungsmethode an den Server kopiert werden. 

> [!NOTE]
> In einem Szenario für die Bereitstellung in der Produktion übernimmt ein Continuous Integration-Workflow das Veröffentlichen der Anwendungen und Kopieren der Objekte auf den Server. 

## <a name="configure-a-proxy-server"></a>Konfigurieren eines Proxyservers

Ein Reverseproxy wird im Allgemeinen zur Unterstützung dynamischer Webanwendungen eingerichtet. Ein Reverseproxy beendet die HTTP-Anforderung und leitet diese an die ASP.NET-Anwendung weiter.

Ein Proxyserver dient zum Weiterleiten von Clientanforderungen an einen anderen Server, anstatt diese selbst zu erfüllen. Ein Reverseproxy leitet Daten an ein festes Ziel meist im Auftrag beliebiger Clients weiter. In diesem Handbuch wird Apache als Reverseproxy konfiguriert, der auf demselben Server ausgeführt wird, dem Kestrel die ASP.NET Core-Anwendung verarbeitet. 

Jeder Teil der Anwendung kann je nach architektonischen Anforderungen oder Einschränkungen auf unterschiedlichen physischen Computern, in Docker-Containern oder in einer Kombination von Konfigurationen vorhanden sein.

### <a name="install-apache"></a>Installieren von Apache

Das Installieren des Apache-Webservers unter CentOS erfolgt mit einem einzelnen Befehl, doch lassen Sie uns zuerst unsere Pakete aktualisieren.

```bash
    sudo yum update -y
```

Dadurch wird sichergestellt, dass alle installierten Pakete auf die neueste Version aktualisiert werden. Installieren von Apache mithilfe von `yum`

```bash
    sudo yum -y install httpd mod_ssl
```

Die Ausgabe sollte ähnlich der folgenden sein.

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
> In diesem Beispiel enthält die Ausgabe „httpd.86_64“, da die CentOS 7-Version eine 64-Bit-Version ist. Die Ausgabe kann bei Ihrem Server unterschiedlich sein. Um zu prüfen, wo Apache installiert ist, führen Sie an einer Eingabeaufforderung `whereis httpd` aus. 

### <a name="configure-apache-for-reverse-proxy"></a>Konfigurieren von Apache als Reverseproxy

Konfigurationsdateien für Apache befinden sich im Verzeichnis `/etc/httpd/conf.d/`. Dateien mit der Erweiterung **.conf** werden in alphabetischer Reihenfolge zusätzlich zu den Modulkonfigurationsdateien im Verzeichnis `/etc/httpd/conf.modules.d/` verarbeitet, das Konfigurationsdateien enthält, die zum Laden von Modulen erforderlich sind.

Erstellen Sie eine Konfigurationsdatei für Ihre App. Bei diesem Beispiel nennen wir sie `hellomvc.conf`.

```text
    <VirtualHost *:80>
        ProxyPreserveHost On
        ProxyPass / http://127.0.0.1:5000/
        ProxyPassReverse / http://127.0.0.1:5000/
        ErrorLog /var/log/httpd/hellomvc-error.log
        CustomLog /var/log/httpd/hellomvc-access.log common
    </VirtualHost>
```

Der Knoten *VirtualHost*, von dem es mehrere in einer Datei oder auf einem Server in zahlreichen Dateien geben kann, wird auf das Lauschen auf IP-Adressen an Port 80 festgelegt. Die nächsten beiden Zeilen sind auf das Durchlassen aller Anforderungen festgelegt, die an der Stammadresse des Computers 127.0.0.1 an Port 5000 und in umgekehrter Reihenfolge empfangen werden. Für bidirektionale Kommunikation sind die beiden Einstellungen *ProxyPass* und *ProxyPassReverse* erforderlich.

Die Protokollierung kann für „VirtualHost“ mithilfe der Direktiven *ErrorLog* und *CustomLog* konfiguriert werden. *ErrorLog* ist der Speicherort, an dem der Server Fehler protokolliert. *CustomLog* legt den Dateinamen und das Format der Protokolldatei fest. In unserem Fall werden Anforderungsinformationen hier protokolliert. Für jede Anforderung gibt es eine Zeile.

Speichern Sie die Datei, und testen Sie die Konfiguration. Wenn alle Anforderungen durchgehen, muss die Antwort `Syntax [OK]` lauten.

```bash
    sudo service httpd configtest
```

Starten Sie Apache neu.

```text
    sudo systemctl restart httpd
    sudo systemctl enable httpd
```

## <a name="monitoring-our-application"></a>Überwachen der Anwendung

Apache ist nun dafür eingerichtet, Anforderungen an `http://localhost:80` an die ASP.NET Core-Anwendung weiterzuleiten, die unter Kestrel unter `http://127.0.0.1:5000` ausgeführt wird.  Apache ist jedoch nicht dafür eingerichtet, den Kestrel-Prozess zu verwalten. Wir verwenden *systemd* und erstellen eine Dienstdatei, um die zugrunde liegende Web-App zu starten und zu überwachen. *systemd* ist ein Initialisierungssystem, das viele leistungsstarke Features zum Starten, Beenden und Verwalten von Prozessen bereitstellt. 


### <a name="create-the-service-file"></a>Erstellen der Dienstdatei

Erstellen der Dienstdefinitionsdatei 

```bash
    sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

Eine Beispieldienstdatei für die Anwendung.

```text
[Unit]
    Description=Example .NET Web API Application running on CentOS 7

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
> **Benutzer:** Wenn der Benutzer *apache* von Ihrer Konfiguration nicht verwendet wird, muss der hier definierte Benutzer zunächst erstellt werden. Ihm müssen auch die ordnungsgemäßen Besitzrechte für Dateien zugewiesen werden.

Speichern Sie die Datei, und aktivieren Sie den Dienst.

```bash
    systemctl enable kestrel-hellomvc.service
```

Starten Sie den Dienst, und versichern Sie sich, dass er ausgeführt wird.

```
    systemctl start kestrel-hellomvc.service
    systemctl status kestrel-hellomvc.service

    ● kestrel-hellomvc.service - Example .NET Web API Application running on CentOS 7
        Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
        Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
    Main PID: 9021 (dotnet)
        CGroup: /system.slice/kestrel-hellomvc.service
                └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

Wenn der Reverseproxy konfiguriert ist und Kestrel durch „systemd“ verwaltet wird, ist die Webanwendung vollständig konfiguriert. Auf diese kann dann über einen Browser auf dem lokalen Computer unter `http://localhost` zugegriffen werden. Beim Überprüfen der Antwortheader zeigt der **Server** weiter die ASP.NET Core-Anwendung an, die von Kestrel verarbeitet wird.

```text
    HTTP/1.1 200 OK
    Date: Tue, 11 Oct 2016 16:22:23 GMT
    Server: Kestrel
    Keep-Alive: timeout=5, max=98
    Connection: Keep-Alive
    Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a>Anzeigen von Protokollen

Da die Webanwendung, die Kestrel verwendet, von „systemd“ verwaltet wird, werden alle Ereignisse und Prozesse in einem zentralen Journal protokolliert. Dieses Journal enthält jedoch alle Einträge für alle Dienste und Prozesse, die von „systemd“ verwaltet werden. Verwenden Sie folgenden Befehl, um die für `kestrel-hellomvc.service` spezifischen Elemente anzuzeigen:

```bash
    sudo journalctl -fu kestrel-hellomvc.service
```

Für weiteres Filtern können Zeitoptionen wie `--since today`, `--until 1 hour ago` oder eine Kombination aus diesen die Menge der zurückgegebenen Einträge verringern.

```bash
    sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-our-application"></a>Schützen Ihrer Anwendung

### <a name="configure-firewall"></a>Konfigurieren der Firewall

*Firewalld* ist ein dynamischer Daemon zum Verwalten einer Firewall mit Unterstützung für Netzwerkzonen, wenngleich Sie weiterhin „iptables“ zum Verwalten von Ports und Paketfilterung verwenden können. „Firewalld“ sollten standardmäßig installiert sein. Mit `yum` kann das Paket installiert oder überprüft werden.

```bash
    sudo yum install firewalld -y
```

Mithilfe von `firewalld` können Sie nur die für die Anwendung erforderlichen Ports öffnen. In diesem Fall werden die Ports 80 und 443 verwendet. Über die folgenden Befehle werden diese dauerhaft geöffnet.

```bash
    sudo firewall-cmd --add-port=80/tcp --permanent
    sudo firewall-cmd --add-port=443/tcp --permanent
```

Laden Sie die Firewalleinstellungen neu, und überprüfen Sie die verfügbaren Dienste und Ports in der Standardzone. Über `firewall-cmd -h` können Sie verfügbare Optionen bestimmen.

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

Das Modul „mod_ssl“ wird verwendet, um Apache für SSL zu konfigurieren.  Es wurde bei der Installation des Moduls `httpd` installiert. Falls es fehlt bzw. nicht installiert wurde, fügen Sie es unserer Konfiguration mithilfe von yum hinzu.

```bash
    sudo yum install mod_ssl
```
Um SSL zu erzwingen, installieren Sie `mod_rewrite`.

```bash
    sudo yum install mod_rewrite
```

Die Datei `hellomvc.conf`, die für dieses Beispiel erstellt wurde, muss so geändert werden, dass das erneute Generieren aktiviert und der neue Abschnitt **VirtualHost** für HTTPS hinzugefügt wird.

```text
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
> In diesem Beispiel wird ein lokal erstelltes Zertifikat verwendet. **SSLCertificateFile** muss die primäre Zertifikatdatei für Ihren Domänennamen sein. **SSLCertificateKeyFile** muss die Schlüsseldatei sein, die beim Erstellen der Zertifikatsignaturanforderung generiert wurde. **SSLCertificateChainFile** muss die Zwischenzertifikatdatei (falls vorhanden) sein, die von Ihrer Zertifizierungsstelle bereitgestellt wurde.

Speichern Sie die Datei, und testen Sie die Konfiguration.

```bash
    sudo service httpd configtest
```

Starten Sie Apache neu.

```bash
    sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a>Weitere Vorschläge für Apache

### <a name="additional-headers"></a>Zusätzliche Header 
Als Schutz gegen böswillige Angriffe müssen einige Header geändert oder hinzugefügt werden. Vergewissern Sie sich, dass das Modul `mod_headers` installiert ist.

```bash
    sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking"></a>Schützen von Apache vor Clickjacking
Clickjacking ist eine böswillige Technik zum Sammeln der Klicks eines infizierten Benutzers. Clickjacking bringt das Opfer (Besucher) dazu, auf eine infizierte Seite zu klicken. Verwenden Sie „X-FRAME-OPTIONS“ zum Schützen Ihrer Website.

Bearbeiten Sie die Datei „httpd.conf“.

```bash
    sudo nano /etc/httpd/conf/httpd.conf
```

Fügen Sie die Zeile `Header append X-FRAME-OPTIONS "SAMEORIGIN"` hinzu, und speichern Sie die Datei. Starten Sie Apache dann neu.

#### <a name="mime-type-sniffing"></a>MIME-Typermittlung

Dieser Header hindert Internet Explorer an der MIME-Ermittlung einer Antwort vom deklarierten Inhaltstyp weg, da der Header den Browser anweist, den Inhaltstyp der Antwort nicht zu überschreiben. Durch die Option „nosniff“ wird der Inhalt vom Browser als „text/html“ gerendert, wenn der Server angibt, dass es sich bei diesem um „text/html“ handelt.

Bearbeiten Sie die Datei „httpd.conf“.

```bash
    sudo nano /etc/httpd/conf/httpd.conf
```

Fügen Sie die Zeile `Header set X-Content-Type-Options "nosniff"` hinzu, und speichern Sie die Datei. Starten Sie Apache dann neu.

### <a name="load-balancing"></a>Lastenausgleich 

Dieses Beispiel veranschaulicht das Einrichten und Konfigurieren von Apache unter CentOS 7 und Kestrel auf demselben Instanzcomputer.  Damit es jedoch zu keinem Single Point of Failure kommt, ermöglicht das Verwenden von *Mod_proxy_balancer* und Ändern von „VirtualHost“ das Verwalten mehrerer Instanzen der Webanwendungen hinter dem Apache-Proxyserver.

```bash
    sudo yum install mod_proxy_balancer
```

In der Konfigurationsdatei wurde eine zusätzliche Instanz der `hellomvc`-App für die Ausführung an Port 5001 eingerichtet. Der Abschnitt *Proxy* wurde auf eine Lastenausgleichskonfiguration mit zwei Mitgliedern festgelegt, um die Last durch *byrequests* auszugleichen.

```text
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
Mithilfe von `mod_ratelimit` im Modul `htttpd` können Sie die Bandbreite von Clients begrenzen. 

```bash
    sudo nano /etc/httpd/conf.d/ratelimit.conf
```
In der Beispieldatei wird die Bandbreite am Stammspeicherort auf 600 KB/Sek. begrenzt.

```text
    <IfModule mod_ratelimit.c>
        <Location />
            SetOutputFilter RATE_LIMIT
            SetEnv rate-limit 600
        </Location>
    </IfModule>
```
