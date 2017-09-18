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
# <a name="set-up-a-hosting-environment-for-aspnet-core-on-linux-with-apache-and-deploy-to-it"></a><span data-ttu-id="6f1e1-104">Einrichten einer Hostumgebung für ASP.NET Core unter Linux mit Apache und anschließende Bereitstellung</span><span class="sxs-lookup"><span data-stu-id="6f1e1-104">Set up a hosting environment for ASP.NET Core on Linux with Apache, and deploy to it</span></span>

<span data-ttu-id="6f1e1-105">Von [Shayne Boyer](https://github.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="6f1e1-105">By [Shayne Boyer](https://github.com/spboyer)</span></span>

<span data-ttu-id="6f1e1-106">Apache ist ein sehr beliebter HTTP-Server, der als Proxy konfiguriert werden kann, um HTTP-Datenverkehr vergleichbar mit nginx umzuleiten.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-106">Apache is a very popular HTTP server and can be configured as a proxy to redirect HTTP traffic similar to nginx.</span></span> <span data-ttu-id="6f1e1-107">In dieser Anleitung erfahren Sie, wie Sie Apache unter CentOS 7 einrichten und als Reverseproxy nutzen, um eingehende Verbindungen anzunehmen und sie an die unter Kestrel ausgeführte ASP.NET Core-Anwendung umzuleiten.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-107">In this guide, we will learn how to set up Apache on CentOS 7 and use it as a reverse proxy to welcome incoming connections and redirect them to the ASP.NET Core application running on Kestrel.</span></span> <span data-ttu-id="6f1e1-108">Zu diesem Zweck verwenden wir die Erweiterung *mod_proxy* und andere zugehörige Apache-Module.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-108">For this purpose, we will use the *mod_proxy* extension and other related Apache modules.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6f1e1-109">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="6f1e1-109">Prerequisites</span></span>

1. <span data-ttu-id="6f1e1-110">Zugriff auf einen Server, auf dem CentOS 7 ausgeführt wird, mit einem Standardbenutzerkonto mit sudo-Berechtigung</span><span class="sxs-lookup"><span data-stu-id="6f1e1-110">A server running CentOS 7, with a standard user account with sudo privilege.</span></span>
2. <span data-ttu-id="6f1e1-111">Eine bestehende ASP.NET Core-Anwendung</span><span class="sxs-lookup"><span data-stu-id="6f1e1-111">An existing ASP.NET Core application.</span></span> 

## <a name="publish-your-application"></a><span data-ttu-id="6f1e1-112">Veröffentlichen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="6f1e1-112">Publish your application</span></span>

<span data-ttu-id="6f1e1-113">Führen Sie `dotnet publish -c Release` in Ihrer Entwicklungsumgebung, um Ihre Anwendung in einem eigenständigen Verzeichnis zu packen, das auf Ihrem Server ausgeführt werden kann.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-113">Run `dotnet publish -c Release` from your development environment to package your application into a self-contained directory that can run on your server.</span></span> <span data-ttu-id="6f1e1-114">Die veröffentlichte Anwendung muss dann über SCP, FTP oder eine andere Dateiübertragungsmethode an den Server kopiert werden.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-114">The published application must then be copied to the server using SCP, FTP or other file transfer method.</span></span> 

> [!NOTE]
> <span data-ttu-id="6f1e1-115">In einem Szenario für die Bereitstellung in der Produktion übernimmt ein Continuous Integration-Workflow das Veröffentlichen der Anwendungen und Kopieren der Objekte auf den Server.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-115">Under a production deployment scenario, a continuous integration workflow does the work of publishing the application and copying the assets to the server.</span></span> 

## <a name="configure-a-proxy-server"></a><span data-ttu-id="6f1e1-116">Konfigurieren eines Proxyservers</span><span class="sxs-lookup"><span data-stu-id="6f1e1-116">Configure a proxy server</span></span>

<span data-ttu-id="6f1e1-117">Ein Reverseproxy wird im Allgemeinen zur Unterstützung dynamischer Webanwendungen eingerichtet.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-117">A reverse proxy is a common setup for serving dynamic web applications.</span></span> <span data-ttu-id="6f1e1-118">Ein Reverseproxy beendet die HTTP-Anforderung und leitet diese an die ASP.NET-Anwendung weiter.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-118">The reverse proxy terminates the HTTP request and forwards it to the ASP.NET application.</span></span>

<span data-ttu-id="6f1e1-119">Ein Proxyserver dient zum Weiterleiten von Clientanforderungen an einen anderen Server, anstatt diese selbst zu erfüllen.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-119">A proxy server is one which forwards client requests to another server instead of fulfilling them itself.</span></span> <span data-ttu-id="6f1e1-120">Ein Reverseproxy leitet Daten an ein festes Ziel meist im Auftrag beliebiger Clients weiter.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-120">A reverse proxy forwards to a fixed destination, typically on behalf of arbitrary clients.</span></span> <span data-ttu-id="6f1e1-121">In diesem Handbuch wird Apache als Reverseproxy konfiguriert, der auf demselben Server ausgeführt wird, dem Kestrel die ASP.NET Core-Anwendung verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-121">In this guide, Apache is being configured as the reverse proxy running on the same server that Kestrel is serving the ASP.NET Core application.</span></span> 

<span data-ttu-id="6f1e1-122">Jeder Teil der Anwendung kann je nach architektonischen Anforderungen oder Einschränkungen auf unterschiedlichen physischen Computern, in Docker-Containern oder in einer Kombination von Konfigurationen vorhanden sein.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-122">Each piece of the application can exist on separate physical machines, Docker containers, or a combination of configurations depending on your architectural needs or restrictions.</span></span>

### <a name="install-apache"></a><span data-ttu-id="6f1e1-123">Installieren von Apache</span><span class="sxs-lookup"><span data-stu-id="6f1e1-123">Install Apache</span></span>

<span data-ttu-id="6f1e1-124">Das Installieren des Apache-Webservers unter CentOS erfolgt mit einem einzelnen Befehl, doch lassen Sie uns zuerst unsere Pakete aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-124">Installing the Apache web server on CentOS is a single command, but first let's update our packages.</span></span>

```bash
    sudo yum update -y
```

<span data-ttu-id="6f1e1-125">Dadurch wird sichergestellt, dass alle installierten Pakete auf die neueste Version aktualisiert werden.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-125">This ensures that all of the installed packages are updated to their latest version.</span></span> <span data-ttu-id="6f1e1-126">Installieren von Apache mithilfe von `yum`</span><span class="sxs-lookup"><span data-stu-id="6f1e1-126">Install Apache using `yum`</span></span>

```bash
    sudo yum -y install httpd mod_ssl
```

<span data-ttu-id="6f1e1-127">Die Ausgabe sollte ähnlich der folgenden sein.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-127">The output should reflect something similar to the following.</span></span>

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
> <span data-ttu-id="6f1e1-128">In diesem Beispiel enthält die Ausgabe „httpd.86_64“, da die CentOS 7-Version eine 64-Bit-Version ist.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-128">In this example the output reflects httpd.86_64 since the CentOS 7 version is 64 bit.</span></span> <span data-ttu-id="6f1e1-129">Die Ausgabe kann bei Ihrem Server unterschiedlich sein.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-129">The output may be different for your server.</span></span> <span data-ttu-id="6f1e1-130">Um zu prüfen, wo Apache installiert ist, führen Sie an einer Eingabeaufforderung `whereis httpd` aus.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-130">To verify where Apache is installed, run `whereis httpd` from a command prompt.</span></span> 

### <a name="configure-apache-for-reverse-proxy"></a><span data-ttu-id="6f1e1-131">Konfigurieren von Apache als Reverseproxy</span><span class="sxs-lookup"><span data-stu-id="6f1e1-131">Configure Apache for reverse proxy</span></span>

<span data-ttu-id="6f1e1-132">Konfigurationsdateien für Apache befinden sich im Verzeichnis `/etc/httpd/conf.d/`.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-132">Configuration files for Apache are located within the `/etc/httpd/conf.d/` directory.</span></span> <span data-ttu-id="6f1e1-133">Dateien mit der Erweiterung **.conf** werden in alphabetischer Reihenfolge zusätzlich zu den Modulkonfigurationsdateien im Verzeichnis `/etc/httpd/conf.modules.d/` verarbeitet, das Konfigurationsdateien enthält, die zum Laden von Modulen erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-133">Any file with the **.conf** extension will be processed in alphabetical order in addition to the module configuration files in `/etc/httpd/conf.modules.d/`, which contains any configuration files necessary to load modules.</span></span>

<span data-ttu-id="6f1e1-134">Erstellen Sie eine Konfigurationsdatei für Ihre App. Bei diesem Beispiel nennen wir sie `hellomvc.conf`.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-134">Create a configuration file for your app, for this example we'll call it `hellomvc.conf`</span></span>

```text
    <VirtualHost *:80>
        ProxyPreserveHost On
        ProxyPass / http://127.0.0.1:5000/
        ProxyPassReverse / http://127.0.0.1:5000/
        ErrorLog /var/log/httpd/hellomvc-error.log
        CustomLog /var/log/httpd/hellomvc-access.log common
    </VirtualHost>
```

<span data-ttu-id="6f1e1-135">Der Knoten *VirtualHost*, von dem es mehrere in einer Datei oder auf einem Server in zahlreichen Dateien geben kann, wird auf das Lauschen auf IP-Adressen an Port 80 festgelegt.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-135">The *VirtualHost* node, of which there can be multiple in a file or on a server in many files, is set to listen on any IP address using port 80.</span></span> <span data-ttu-id="6f1e1-136">Die nächsten beiden Zeilen sind auf das Durchlassen aller Anforderungen festgelegt, die an der Stammadresse des Computers 127.0.0.1 an Port 5000 und in umgekehrter Reihenfolge empfangen werden.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-136">The next two lines are set to pass all requests received at the root to the machine 127.0.0.1 port 5000 and in reverse.</span></span> <span data-ttu-id="6f1e1-137">Für bidirektionale Kommunikation sind die beiden Einstellungen *ProxyPass* und *ProxyPassReverse* erforderlich.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-137">For there to be bi-directional communication, both settings *ProxyPass* and *ProxyPassReverse* are required.</span></span>

<span data-ttu-id="6f1e1-138">Die Protokollierung kann für „VirtualHost“ mithilfe der Direktiven *ErrorLog* und *CustomLog* konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-138">Logging can be configured per VirtualHost using *ErrorLog* and *CustomLog* directives.</span></span> <span data-ttu-id="6f1e1-139">*ErrorLog* ist der Speicherort, an dem der Server Fehler protokolliert. *CustomLog* legt den Dateinamen und das Format der Protokolldatei fest.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-139">*ErrorLog* is the location where the server will log errors and *CustomLog* sets the filename and format of log file.</span></span> <span data-ttu-id="6f1e1-140">In unserem Fall werden Anforderungsinformationen hier protokolliert.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-140">In our case this is where request information will be logged.</span></span> <span data-ttu-id="6f1e1-141">Für jede Anforderung gibt es eine Zeile.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-141">There will be one line for each request.</span></span>

<span data-ttu-id="6f1e1-142">Speichern Sie die Datei, und testen Sie die Konfiguration.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-142">Save the file, and test the configuration.</span></span> <span data-ttu-id="6f1e1-143">Wenn alle Anforderungen durchgehen, muss die Antwort `Syntax [OK]` lauten.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-143">If everything passes, the response should be `Syntax [OK]`.</span></span>

```bash
    sudo service httpd configtest
```

<span data-ttu-id="6f1e1-144">Starten Sie Apache neu.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-144">Restart Apache.</span></span>

```text
    sudo systemctl restart httpd
    sudo systemctl enable httpd
```

## <a name="monitoring-our-application"></a><span data-ttu-id="6f1e1-145">Überwachen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="6f1e1-145">Monitoring our application</span></span>

<span data-ttu-id="6f1e1-146">Apache ist nun dafür eingerichtet, Anforderungen an `http://localhost:80` an die ASP.NET Core-Anwendung weiterzuleiten, die unter Kestrel unter `http://127.0.0.1:5000` ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-146">Apache is now setup to forward requests made to `http://localhost:80` on to the ASP.NET Core application running on Kestrel at `http://127.0.0.1:5000`.</span></span>  <span data-ttu-id="6f1e1-147">Apache ist jedoch nicht dafür eingerichtet, den Kestrel-Prozess zu verwalten.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-147">However, Apache is not set up to manage the Kestrel process.</span></span> <span data-ttu-id="6f1e1-148">Wir verwenden *systemd* und erstellen eine Dienstdatei, um die zugrunde liegende Web-App zu starten und zu überwachen.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-148">We will use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="6f1e1-149">*systemd* ist ein Initialisierungssystem, das viele leistungsstarke Features zum Starten, Beenden und Verwalten von Prozessen bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-149">*systemd* is an init system that provides many powerful features for starting, stopping and managing processes.</span></span> 


### <a name="create-the-service-file"></a><span data-ttu-id="6f1e1-150">Erstellen der Dienstdatei</span><span class="sxs-lookup"><span data-stu-id="6f1e1-150">Create the service file</span></span>

<span data-ttu-id="6f1e1-151">Erstellen der Dienstdefinitionsdatei</span><span class="sxs-lookup"><span data-stu-id="6f1e1-151">Create the service definition file</span></span> 

```bash
    sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="6f1e1-152">Eine Beispieldienstdatei für die Anwendung.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-152">An example service file for our application.</span></span>

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
> <span data-ttu-id="6f1e1-153">**Benutzer:** Wenn der Benutzer *apache* von Ihrer Konfiguration nicht verwendet wird, muss der hier definierte Benutzer zunächst erstellt werden. Ihm müssen auch die ordnungsgemäßen Besitzrechte für Dateien zugewiesen werden.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-153">**User** -- If the user *apache* is not used by your configuration, the user defined here must be created first and given proper ownership for files</span></span>

<span data-ttu-id="6f1e1-154">Speichern Sie die Datei, und aktivieren Sie den Dienst.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-154">Save the file and enable the service.</span></span>

```bash
    systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="6f1e1-155">Starten Sie den Dienst, und versichern Sie sich, dass er ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-155">Start the service and verify that it is running.</span></span>

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

<span data-ttu-id="6f1e1-156">Wenn der Reverseproxy konfiguriert ist und Kestrel durch „systemd“ verwaltet wird, ist die Webanwendung vollständig konfiguriert. Auf diese kann dann über einen Browser auf dem lokalen Computer unter `http://localhost` zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-156">With the reverse proxy configured and Kestrel managed through systemd, the web application is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="6f1e1-157">Beim Überprüfen der Antwortheader zeigt der **Server** weiter die ASP.NET Core-Anwendung an, die von Kestrel verarbeitet wird.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-157">Inspecting the response headers, the **Server** still shows the ASP.NET Core application being served by Kestrel.</span></span>

```text
    HTTP/1.1 200 OK
    Date: Tue, 11 Oct 2016 16:22:23 GMT
    Server: Kestrel
    Keep-Alive: timeout=5, max=98
    Connection: Keep-Alive
    Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="6f1e1-158">Anzeigen von Protokollen</span><span class="sxs-lookup"><span data-stu-id="6f1e1-158">Viewing logs</span></span>

<span data-ttu-id="6f1e1-159">Da die Webanwendung, die Kestrel verwendet, von „systemd“ verwaltet wird, werden alle Ereignisse und Prozesse in einem zentralen Journal protokolliert.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-159">Since the web application using Kestrel is managed using systemd, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="6f1e1-160">Dieses Journal enthält jedoch alle Einträge für alle Dienste und Prozesse, die von „systemd“ verwaltet werden.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-160">However, this journal includes all entries for all services and processes managed by systemd.</span></span> <span data-ttu-id="6f1e1-161">Verwenden Sie folgenden Befehl, um die für `kestrel-hellomvc.service` spezifischen Elemente anzuzeigen:</span><span class="sxs-lookup"><span data-stu-id="6f1e1-161">To view the `kestrel-hellomvc.service` specific items, use the following command.</span></span>

```bash
    sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="6f1e1-162">Für weiteres Filtern können Zeitoptionen wie `--since today`, `--until 1 hour ago` oder eine Kombination aus diesen die Menge der zurückgegebenen Einträge verringern.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-162">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
    sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-our-application"></a><span data-ttu-id="6f1e1-163">Schützen Ihrer Anwendung</span><span class="sxs-lookup"><span data-stu-id="6f1e1-163">Securing our application</span></span>

### <a name="configure-firewall"></a><span data-ttu-id="6f1e1-164">Konfigurieren der Firewall</span><span class="sxs-lookup"><span data-stu-id="6f1e1-164">Configure firewall</span></span>

<span data-ttu-id="6f1e1-165">*Firewalld* ist ein dynamischer Daemon zum Verwalten einer Firewall mit Unterstützung für Netzwerkzonen, wenngleich Sie weiterhin „iptables“ zum Verwalten von Ports und Paketfilterung verwenden können.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-165">*Firewalld* is a dynamic daemon to manage firewall with support for network zones, although you can still use iptables to manage ports and packet filtering.</span></span> <span data-ttu-id="6f1e1-166">„Firewalld“ sollten standardmäßig installiert sein. Mit `yum` kann das Paket installiert oder überprüft werden.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-166">Firewalld should be installed by default, `yum` can be used to install the package or verify.</span></span>

```bash
    sudo yum install firewalld -y
```

<span data-ttu-id="6f1e1-167">Mithilfe von `firewalld` können Sie nur die für die Anwendung erforderlichen Ports öffnen.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-167">Using `firewalld` you can open only the ports needed for the application.</span></span> <span data-ttu-id="6f1e1-168">In diesem Fall werden die Ports 80 und 443 verwendet.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-168">In this case, port 80 and 443 are used.</span></span> <span data-ttu-id="6f1e1-169">Über die folgenden Befehle werden diese dauerhaft geöffnet.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-169">The following commands permanently sets these to open.</span></span>

```bash
    sudo firewall-cmd --add-port=80/tcp --permanent
    sudo firewall-cmd --add-port=443/tcp --permanent
```

<span data-ttu-id="6f1e1-170">Laden Sie die Firewalleinstellungen neu, und überprüfen Sie die verfügbaren Dienste und Ports in der Standardzone.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-170">Reload the firewall settings, and check the available services and ports in the default zone.</span></span> <span data-ttu-id="6f1e1-171">Über `firewall-cmd -h` können Sie verfügbare Optionen bestimmen.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-171">Options are available by inspecting `firewall-cmd -h`</span></span>

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

### <a name="ssl-configuration"></a><span data-ttu-id="6f1e1-172">SSL-Konfiguration</span><span class="sxs-lookup"><span data-stu-id="6f1e1-172">SSL configuration</span></span>

<span data-ttu-id="6f1e1-173">Das Modul „mod_ssl“ wird verwendet, um Apache für SSL zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-173">To configure Apache for SSL, the mod_ssl module is used.</span></span>  <span data-ttu-id="6f1e1-174">Es wurde bei der Installation des Moduls `httpd` installiert.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-174">This was installed initially when we installed the `httpd` module.</span></span> <span data-ttu-id="6f1e1-175">Falls es fehlt bzw. nicht installiert wurde, fügen Sie es unserer Konfiguration mithilfe von yum hinzu.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-175">If it was missed or not installed, use yum to add it to your configuration.</span></span>

```bash
    sudo yum install mod_ssl
```
<span data-ttu-id="6f1e1-176">Um SSL zu erzwingen, installieren Sie `mod_rewrite`.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-176">To enforce SSL, install `mod_rewrite`</span></span>

```bash
    sudo yum install mod_rewrite
```

<span data-ttu-id="6f1e1-177">Die Datei `hellomvc.conf`, die für dieses Beispiel erstellt wurde, muss so geändert werden, dass das erneute Generieren aktiviert und der neue Abschnitt **VirtualHost** für HTTPS hinzugefügt wird.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-177">The `hellomvc.conf` file that was created for this example needs to be modified to enable the rewrite as well as adding the new **VirtualHost** section for HTTPS.</span></span>

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
> <span data-ttu-id="6f1e1-178">In diesem Beispiel wird ein lokal erstelltes Zertifikat verwendet.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-178">This example is using a locally generated certificate.</span></span> <span data-ttu-id="6f1e1-179">**SSLCertificateFile** muss die primäre Zertifikatdatei für Ihren Domänennamen sein.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-179">**SSLCertificateFile** should be your primary certificate file for your domain name.</span></span> <span data-ttu-id="6f1e1-180">**SSLCertificateKeyFile** muss die Schlüsseldatei sein, die beim Erstellen der Zertifikatsignaturanforderung generiert wurde.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-180">**SSLCertificateKeyFile** should be the key file generated when you created the CSR.</span></span> <span data-ttu-id="6f1e1-181">**SSLCertificateChainFile** muss die Zwischenzertifikatdatei (falls vorhanden) sein, die von Ihrer Zertifizierungsstelle bereitgestellt wurde.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-181">**SSLCertificateChainFile** should be the intermediate certificate file (if any) that was supplied by your certificate authority</span></span>

<span data-ttu-id="6f1e1-182">Speichern Sie die Datei, und testen Sie die Konfiguration.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-182">Save the file, and test the configuration.</span></span>

```bash
    sudo service httpd configtest
```

<span data-ttu-id="6f1e1-183">Starten Sie Apache neu.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-183">Restart Apache.</span></span>

```bash
    sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a><span data-ttu-id="6f1e1-184">Weitere Vorschläge für Apache</span><span class="sxs-lookup"><span data-stu-id="6f1e1-184">Additional Apache suggestions</span></span>

### <a name="additional-headers"></a><span data-ttu-id="6f1e1-185">Zusätzliche Header</span><span class="sxs-lookup"><span data-stu-id="6f1e1-185">Additional Headers</span></span> 
<span data-ttu-id="6f1e1-186">Als Schutz gegen böswillige Angriffe müssen einige Header geändert oder hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-186">In order to secure against malicious attacks there are a few headers that should either be modified or added.</span></span> <span data-ttu-id="6f1e1-187">Vergewissern Sie sich, dass das Modul `mod_headers` installiert ist.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-187">Ensure that the `mod_headers` module is installed.</span></span>

```bash
    sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking"></a><span data-ttu-id="6f1e1-188">Schützen von Apache vor Clickjacking</span><span class="sxs-lookup"><span data-stu-id="6f1e1-188">Secure Apache from clickjacking</span></span>
<span data-ttu-id="6f1e1-189">Clickjacking ist eine böswillige Technik zum Sammeln der Klicks eines infizierten Benutzers.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-189">Clickjacking is a malicious technique to collect an infected user's clicks.</span></span> <span data-ttu-id="6f1e1-190">Clickjacking bringt das Opfer (Besucher) dazu, auf eine infizierte Seite zu klicken.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-190">Clickjacking tricks the victim (visitor) into clicking on an infected site.</span></span> <span data-ttu-id="6f1e1-191">Verwenden Sie „X-FRAME-OPTIONS“ zum Schützen Ihrer Website.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-191">Use X-FRAME-OPTIONS to secure your site.</span></span>

<span data-ttu-id="6f1e1-192">Bearbeiten Sie die Datei „httpd.conf“.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-192">Edit the httpd.conf file.</span></span>

```bash
    sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="6f1e1-193">Fügen Sie die Zeile `Header append X-FRAME-OPTIONS "SAMEORIGIN"` hinzu, und speichern Sie die Datei. Starten Sie Apache dann neu.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-193">Add the line `Header append X-FRAME-OPTIONS "SAMEORIGIN"` and save the file, then restart Apache.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="6f1e1-194">MIME-Typermittlung</span><span class="sxs-lookup"><span data-stu-id="6f1e1-194">MIME-type sniffing</span></span>

<span data-ttu-id="6f1e1-195">Dieser Header hindert Internet Explorer an der MIME-Ermittlung einer Antwort vom deklarierten Inhaltstyp weg, da der Header den Browser anweist, den Inhaltstyp der Antwort nicht zu überschreiben.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-195">This header prevents Internet Explorer from MIME-sniffing a response away from the declared content-type as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="6f1e1-196">Durch die Option „nosniff“ wird der Inhalt vom Browser als „text/html“ gerendert, wenn der Server angibt, dass es sich bei diesem um „text/html“ handelt.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-196">With the nosniff option, if the server says the content is text/html, the browser will render it as text/html.</span></span>

<span data-ttu-id="6f1e1-197">Bearbeiten Sie die Datei „httpd.conf“.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-197">Edit the httpd.conf file.</span></span>

```bash
    sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="6f1e1-198">Fügen Sie die Zeile `Header set X-Content-Type-Options "nosniff"` hinzu, und speichern Sie die Datei. Starten Sie Apache dann neu.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-198">Add the line `Header set X-Content-Type-Options "nosniff"` and save the file, then restart Apache.</span></span>

### <a name="load-balancing"></a><span data-ttu-id="6f1e1-199">Lastenausgleich</span><span class="sxs-lookup"><span data-stu-id="6f1e1-199">Load Balancing</span></span> 

<span data-ttu-id="6f1e1-200">Dieses Beispiel veranschaulicht das Einrichten und Konfigurieren von Apache unter CentOS 7 und Kestrel auf demselben Instanzcomputer.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-200">This example shows how to setup and configure Apache on CentOS 7 and Kestrel on the same instance machine.</span></span>  <span data-ttu-id="6f1e1-201">Damit es jedoch zu keinem Single Point of Failure kommt, ermöglicht das Verwenden von *Mod_proxy_balancer* und Ändern von „VirtualHost“ das Verwalten mehrerer Instanzen der Webanwendungen hinter dem Apache-Proxyserver.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-201">However, in order to not have a single point of failure; using *mod_proxy_balancer* and modifying the VirtualHost would allow for managing mutliple instances of the web applications behind the Apache proxy server.</span></span>

```bash
    sudo yum install mod_proxy_balancer
```

<span data-ttu-id="6f1e1-202">In der Konfigurationsdatei wurde eine zusätzliche Instanz der `hellomvc`-App für die Ausführung an Port 5001 eingerichtet. Der Abschnitt *Proxy* wurde auf eine Lastenausgleichskonfiguration mit zwei Mitgliedern festgelegt, um die Last durch *byrequests* auszugleichen.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-202">In the configuration file, an additional instance of the `hellomvc` app has been setup to run on port 5001 and the *Proxy* section has been set with a balancer configuration with two members to load balance *byrequests*.</span></span>

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

### <a name="rate-limits"></a><span data-ttu-id="6f1e1-203">Begrenzung der Bandbreite</span><span class="sxs-lookup"><span data-stu-id="6f1e1-203">Rate Limits</span></span>
<span data-ttu-id="6f1e1-204">Mithilfe von `mod_ratelimit` im Modul `htttpd` können Sie die Bandbreite von Clients begrenzen.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-204">Using `mod_ratelimit`, which is included in the `htttpd` module you can limit the amount of bandwidth of clients.</span></span> 

```bash
    sudo nano /etc/httpd/conf.d/ratelimit.conf
```
<span data-ttu-id="6f1e1-205">In der Beispieldatei wird die Bandbreite am Stammspeicherort auf 600 KB/Sek. begrenzt.</span><span class="sxs-lookup"><span data-stu-id="6f1e1-205">The example file limits bandwidth as 600 KB/sec under the root location.</span></span>

```text
    <IfModule mod_ratelimit.c>
        <Location />
            SetOutputFilter RATE_LIMIT
            SetEnv rate-limit 600
        </Location>
    </IfModule>
```
