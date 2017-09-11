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
ms.openlocfilehash: 1f2b5fc6d769c63110f832a31cd0d0aa8c3298e9
ms.sourcegitcommit: bd05f7ea8f87ad076ef6e8b704698ebcba5ca80c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/25/2017
---
# <a name="set-up-a-hosting-environment-for-aspnet-core-on-linux-with-nginx-and-deploy-to-it"></a><span data-ttu-id="980b6-104">Einrichten einer Hostumgebung für ASP.NET Core unter Linux mit Nginx und Bereitstellen auf diese</span><span class="sxs-lookup"><span data-stu-id="980b6-104">Set up a hosting environment for ASP.NET Core on Linux with Nginx, and deploy to it</span></span>

<span data-ttu-id="980b6-105">Von [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="980b6-105">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="980b6-106">Dieser Leitfaden erläutert das Einrichten einer produktionsbereiten ASP.NET Core-Umgebungen auf einem Ubuntu 16.04-Server.</span><span class="sxs-lookup"><span data-stu-id="980b6-106">This guide explains setting up a production-ready ASP.NET Core environment on an Ubuntu 16.04 Server.</span></span>

<span data-ttu-id="980b6-107">**Hinweis:** Für Ubuntu 14.04 wird „Supervisored“ (überwacht) für die Überwachung des Kestrel-Prozesses empfohlen.</span><span class="sxs-lookup"><span data-stu-id="980b6-107">**Note:** For Ubuntu 14.04, supervisord is recommended as a solution for monitoring the Kestrel process.</span></span> <span data-ttu-id="980b6-108">„SystemD“ ist auf Ubuntu 14.04 nicht verfügbar.</span><span class="sxs-lookup"><span data-stu-id="980b6-108">systemd is not available on Ubuntu 14.04.</span></span> [<span data-ttu-id="980b6-109">Hier finden Sie die vorherige Version dieses Dokuments.</span><span class="sxs-lookup"><span data-stu-id="980b6-109">See previous version of this document</span></span>](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md)

<span data-ttu-id="980b6-110">In diesem Leitfaden:</span><span class="sxs-lookup"><span data-stu-id="980b6-110">This guide:</span></span>

* <span data-ttu-id="980b6-111">Wird eine bestehende ASP.NET Core-Anwendung hinter einem Reverseproxyserver eingefügt</span><span class="sxs-lookup"><span data-stu-id="980b6-111">Places an existing ASP.NET Core application behind a reverse proxy server</span></span>
* <span data-ttu-id="980b6-112">Wird der Reverseproxyserver eingerichtet, um Anforderungen an den Kestrel-Webserver weiterzuleiten</span><span class="sxs-lookup"><span data-stu-id="980b6-112">Sets up the reverse proxy server to forward requests to the Kestrel web server</span></span>
* <span data-ttu-id="980b6-113">Wird sichergestellt, dass die Webanwendung beim Start als Daemon ausgeführt wird</span><span class="sxs-lookup"><span data-stu-id="980b6-113">Ensures the web application runs on startup as a daemon</span></span>
* <span data-ttu-id="980b6-114">Wird ein Prozessverwaltungstool konfiguriert, das die Webanwendung beim Neustarten unterstützt</span><span class="sxs-lookup"><span data-stu-id="980b6-114">Configures a process management tool to help restart the web application</span></span>

## <a name="prerequisites"></a><span data-ttu-id="980b6-115">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="980b6-115">Prerequisites</span></span>

1. <span data-ttu-id="980b6-116">Zugriff auf einen Ubuntu 16.04-Server mit einem Standardbenutzerkonto mit sudo-Berechtigung</span><span class="sxs-lookup"><span data-stu-id="980b6-116">Access to an Ubuntu 16.04 Server with a standard user account with sudo privilege</span></span>
2. <span data-ttu-id="980b6-117">Eine bestehende ASP.NET Core-Anwendung</span><span class="sxs-lookup"><span data-stu-id="980b6-117">An existing ASP.NET Core application</span></span>

## <a name="copy-over-your-app"></a><span data-ttu-id="980b6-118">Kopieren Ihrer App</span><span class="sxs-lookup"><span data-stu-id="980b6-118">Copy over your app</span></span>

<span data-ttu-id="980b6-119">Führen Sie `dotnet publish` in der Bereitstellungsumgebung aus, um eine App in ein eigenständiges Verzeichnis zu verpacken, das auf dem Server ausgeführt werden kann.</span><span class="sxs-lookup"><span data-stu-id="980b6-119">Run `dotnet publish` from the dev environment to package an app into a self-contained directory that can run on the server.</span></span>

<span data-ttu-id="980b6-120">Kopieren Sie die ASP.NET Core-App auf den Server, indem Sie ein beliebiges Tool (SCP, FTP usw.) verwenden, das in Ihren Workflow integriert ist.</span><span class="sxs-lookup"><span data-stu-id="980b6-120">Copy the ASP.NET Core app to the server using whatever tool (SCP, FTP, etc.) integrates into your workflow.</span></span> <span data-ttu-id="980b6-121">Testen Sie die App wie im folgenden Beispiel:</span><span class="sxs-lookup"><span data-stu-id="980b6-121">Test the app, for example:</span></span>
 - <span data-ttu-id="980b6-122">Führen Sie `dotnet yourapp.dll` über die Befehlszeile aus.</span><span class="sxs-lookup"><span data-stu-id="980b6-122">From the command line, run `dotnet yourapp.dll`</span></span>
 - <span data-ttu-id="980b6-123">Navigieren Sie in einem Browser zu `http://<serveraddress>:<port>` und überprüfen Sie, dass die App unter Linux funktioniert.</span><span class="sxs-lookup"><span data-stu-id="980b6-123">In a browser, navigate to `http://<serveraddress>:<port>` to verify the app works on Linux.</span></span> 
 
<span data-ttu-id="980b6-124">**Hinweis:** Verwenden Sie [Yeoman](xref:client-side/yeoman) zum Erstellen einer neuen ASP.NET Core-App für ein neues Projekt.</span><span class="sxs-lookup"><span data-stu-id="980b6-124">**Note:** Use [Yeoman](xref:client-side/yeoman) to create a new ASP.NET Core app for a new project.</span></span>

## <a name="configure-a-reverse-proxy-server"></a><span data-ttu-id="980b6-125">Konfigurieren eines Reverseproxyservers</span><span class="sxs-lookup"><span data-stu-id="980b6-125">Configure a reverse proxy server</span></span>

<span data-ttu-id="980b6-126">Ein Reverseproxy ist ein allgemeines Setup zum Verarbeiten von dynamischen Webanwendungen.</span><span class="sxs-lookup"><span data-stu-id="980b6-126">A reverse proxy is a common setup for serving dynamic web applications.</span></span> <span data-ttu-id="980b6-127">Ein Reverseproxy beendet die HTTP-Anforderung und leitet diese an die ASP.NET Core-Anwendung weiter.</span><span class="sxs-lookup"><span data-stu-id="980b6-127">A reverse proxy terminates the HTTP request and forwards it to the ASP.NET Core application.</span></span>

### <a name="why-use-a-reverse-proxy-server"></a><span data-ttu-id="980b6-128">Gründe für das Verwenden eines Reverseproxyservers:</span><span class="sxs-lookup"><span data-stu-id="980b6-128">Why use a reverse proxy server?</span></span>

<span data-ttu-id="980b6-129">Kestrel eignet sich hervorragend zum Verarbeiten von dynamischen Inhalten von ASP.NET Core, die Features für das Webserving sind jedoch weniger umfangreich als bei Servern wie IIS, Apache oder Nginx.</span><span class="sxs-lookup"><span data-stu-id="980b6-129">Kestrel is great for serving dynamic content from ASP.NET Core; however, the web serving parts aren’t as feature rich as servers like IIS, Apache, or Nginx.</span></span> <span data-ttu-id="980b6-130">Ein Reverseproxyserver kann Arbeiten wie das Verarbeiten von statischen Inhalten, das Zwischenspeichern und Komprimieren von Anforderungen und das Beenden von SSL vom HTTP-Server auslagern.</span><span class="sxs-lookup"><span data-stu-id="980b6-130">A reverse proxy server can offload work like serving static content, caching requests, compressing requests, and SSL termination from the HTTP server.</span></span> <span data-ttu-id="980b6-131">Ein Reverseproxyserver kann sich auf einem dedizierten Computer befinden oder zusammen mit einem HTTP-Server bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="980b6-131">A reverse proxy server may reside on a dedicated machine or may be deployed alongside an HTTP server.</span></span>

<span data-ttu-id="980b6-132">Für diesen Leitfaden wird eine einzelne Instanz von Nginx verwendet.</span><span class="sxs-lookup"><span data-stu-id="980b6-132">For the purposes of this guide, a single instance of Nginx is used.</span></span> <span data-ttu-id="980b6-133">Diese wird auf demselben Server ausgeführt, zusammen mit dem HTTP-Server.</span><span class="sxs-lookup"><span data-stu-id="980b6-133">It runs on the same server, alongside the HTTP server.</span></span> <span data-ttu-id="980b6-134">Je nach Ihren Anforderungen können Sie auch ein anderes Setup auswählen.</span><span class="sxs-lookup"><span data-stu-id="980b6-134">Based on your requirements, you may choose a different setup.</span></span>

<span data-ttu-id="980b6-135">Da Anforderungen vom Reverseproxy weitergeleitet werden, verwenden Sie die `ForwardedHeaders`-Middleware aus dem `Microsoft.AspNetCore.HttpOverrides`-Paket.</span><span class="sxs-lookup"><span data-stu-id="980b6-135">Because requests are forwarded by reverse proxy, use the `ForwardedHeaders` middleware from the `Microsoft.AspNetCore.HttpOverrides` package.</span></span> <span data-ttu-id="980b6-136">Diese Middleware aktualisiert `Request.Scheme` mithilfe des `X-Forwarded-Proto`-Headers, sodass Umleitungs-URIs und andere Sicherheitsrichtlinien ordnungsgemäß funktionieren.</span><span class="sxs-lookup"><span data-stu-id="980b6-136">This middleware updates `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="980b6-137">Beim Einrichten eines Reverseproxyservers wird `UseForwardedHeaders` für das erste Ausführen der Authentifizierungsmiddleware benötigt.</span><span class="sxs-lookup"><span data-stu-id="980b6-137">When setting up a reverse proxy server, the authentication middleware needs `UseForwardedHeaders` to run first.</span></span> <span data-ttu-id="980b6-138">Durch diese Reihenfolge wird sichergestellt, dass die Authentifizierungsmiddleware die betroffenen Werte nutzen und richtige Umleitungs-URIs generieren kann.</span><span class="sxs-lookup"><span data-stu-id="980b6-138">This ordering ensures that the authentication middleware can consume the affected values and generate correct redirect URIs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="980b6-139">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="980b6-139">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="980b6-140">Rufen Sie die `UseForwardedHeaders`-Methode auf (in der `Configure`-Methode von *Startup.cs*), bevor Sie `UseAuthentication` oder eine ähnliche Middleware für das Authentifizierungsschema aufrufen:</span><span class="sxs-lookup"><span data-stu-id="980b6-140">Invoke the `UseForwardedHeaders` method (in the `Configure` method of *Startup.cs*) before calling `UseAuthentication` or similar authentication scheme middleware:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="980b6-141">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="980b6-141">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="980b6-142">Rufen Sie die `UseForwardedHeaders`-Methode auf (in der `Configure`-Methode von *Startup.cs*), bevor Sie `UseIdentity` und `UseFacebookAuthentication` oder eine ähnliche Middleware für das Authentifizierungsschema aufrufen:</span><span class="sxs-lookup"><span data-stu-id="980b6-142">Invoke the `UseForwardedHeaders` method (in the `Configure` method of *Startup.cs*) before calling `UseIdentity` and `UseFacebookAuthentication` or similar authentication scheme middleware:</span></span>

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

### <a name="install-nginx"></a><span data-ttu-id="980b6-143">Installieren von Nginx</span><span class="sxs-lookup"><span data-stu-id="980b6-143">Install Nginx</span></span>

```bash
sudo apt-get install nginx
```

> [!NOTE]
> <span data-ttu-id="980b6-144">Wenn Sie optionale Nginx-Module installieren möchten, müssen Sie Nginx aus der Quelle erstellen.</span><span class="sxs-lookup"><span data-stu-id="980b6-144">If you plan to install optional Nginx modules, you may be required to build Nginx from source.</span></span>

<span data-ttu-id="980b6-145">Verwenden Sie `apt-get` zum Installieren von Nginx.</span><span class="sxs-lookup"><span data-stu-id="980b6-145">Use `apt-get` to install Nginx.</span></span> <span data-ttu-id="980b6-146">Das Installationsprogramm erstellt ein System V-Initialisierungsskript, das Nginx beim Systemstart als Daemon ausführt.</span><span class="sxs-lookup"><span data-stu-id="980b6-146">The installer creates a System V init script that runs Nginx as daemon on system startup.</span></span> <span data-ttu-id="980b6-147">Da Nginx zum ersten Mal installiert wurde, starten Sie es explizit, indem Sie Folgendes ausführen:</span><span class="sxs-lookup"><span data-stu-id="980b6-147">Since Nginx was installed for the first time, explicitly start it by running:</span></span>

```bash
sudo service nginx start
```

<span data-ttu-id="980b6-148">Stellen Sie sicher, dass ein Browser die Standardangebotsseite für Nginx anzeigt.</span><span class="sxs-lookup"><span data-stu-id="980b6-148">Verify a browser displays the default landing page for Nginx.</span></span>

### <a name="configure-nginx"></a><span data-ttu-id="980b6-149">Konfigurieren von Nginx</span><span class="sxs-lookup"><span data-stu-id="980b6-149">Configure Nginx</span></span>

<span data-ttu-id="980b6-150">Ändern Sie `/etc/nginx/sites-available/default`, um Nginx als Reverseproxy für das Weiterleiten von Anforderungen zu Ihrer ASP.NET Core-Anwendung zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="980b6-150">To configure Nginx as a reverse proxy to forward requests to our ASP.NET Core application, modify `/etc/nginx/sites-available/default`.</span></span> <span data-ttu-id="980b6-151">Öffnen Sie die Datei in einem Text-Editor und ersetzen Sie den Inhalt durch den Folgendes:</span><span class="sxs-lookup"><span data-stu-id="980b6-151">Open it in a text editor, and replace the contents with the following:</span></span>

```nginx
server {
    listen 80;
    location / {
        proxy_pass http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection keep-alive;
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

<span data-ttu-id="980b6-152">Diese Nginx-Konfigurationsdatei leitet eingehenden öffentlichen Datenverkehr von Port `80` an Port `5000` weiter.</span><span class="sxs-lookup"><span data-stu-id="980b6-152">This Nginx configuration file forwards incoming public traffic from port `80` to port `5000`.</span></span>

<span data-ttu-id="980b6-153">Sobald Sie die Änderungen an Ihrer Nginx-Konfiguration abgeschlossen haben, können Sie `sudo nginx -t` ausführen, um die Syntax Ihrer Konfigurationsdateien zu überprüfen.</span><span class="sxs-lookup"><span data-stu-id="980b6-153">Once you have completed making changes to your Nginx configuration, you can run `sudo nginx -t` to verify the syntax of your configuration files.</span></span> <span data-ttu-id="980b6-154">Wenn der Test der Konfigurationsdatei erfolgreich ist, können Sie Nginx die Änderungen durch Ausführen von `sudo nginx -s reload` übernehmen lassen.</span><span class="sxs-lookup"><span data-stu-id="980b6-154">If the configuration file test is successful, you can ask Nginx to pick up the changes by running `sudo nginx -s reload`.</span></span>

## <a name="monitoring-our-application"></a><span data-ttu-id="980b6-155">Überwachen Ihrer Anwendung</span><span class="sxs-lookup"><span data-stu-id="980b6-155">Monitoring our application</span></span>

<span data-ttu-id="980b6-156">Nginx ist nun dafür eingerichtet, Anforderungen an `http://yourhost:80` an die ASP.NET Core-Anwendung weiterzuleiten, die unter Kestrel auf `http://127.0.0.1:5000` ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="980b6-156">Nginx is now setup to forward requests made to `http://yourhost:80` on to the ASP.NET Core application running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="980b6-157">Nginx wurde jedoch nicht dafür eingerichtet, den Kestrel-Prozess zu verwalten.</span><span class="sxs-lookup"><span data-stu-id="980b6-157">However, Nginx is not set up to manage the Kestrel process.</span></span> <span data-ttu-id="980b6-158">Sie können *SystemD* verwenden und eine Dienstdatei erstellen, um die zugrunde liegende Web-App zu starten und zu überwachen.</span><span class="sxs-lookup"><span data-stu-id="980b6-158">You can use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="980b6-159">*SystemD* ist ein Initialisierungssystem, das viele leistungsstarke Features zum Starten, Beenden und Verwalten von Prozessen bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="980b6-159">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="980b6-160">Erstellen der Dienstdatei</span><span class="sxs-lookup"><span data-stu-id="980b6-160">Create the service file</span></span>

<span data-ttu-id="980b6-161">Erstellen Sie die Dienstdefinitionsdatei:</span><span class="sxs-lookup"><span data-stu-id="980b6-161">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="980b6-162">Beim Folgenden handelt es sich um eine Beispieldienstdatei für die Anwendung:</span><span class="sxs-lookup"><span data-stu-id="980b6-162">The following is an example service file for our application:</span></span>

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

[Install]
WantedBy=multi-user.target
```

<span data-ttu-id="980b6-163">**Hinweis:** Wenn der Benutzer *www-data* durch Ihre Konfiguration nicht verwendet wird, muss der hier definierte Benutzer zunächst erstellt, und ihm muss die richtige Inhaberschaft für die Dateien erteilt werden.</span><span class="sxs-lookup"><span data-stu-id="980b6-163">**Note:** If the user *www-data* is not used by your configuration, the user defined here must be created first and given proper ownership for files.</span></span>

<span data-ttu-id="980b6-164">Speichern Sie die Datei, und aktivieren Sie den Dienst.</span><span class="sxs-lookup"><span data-stu-id="980b6-164">Save the file, and enable the service.</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="980b6-165">Starten Sie den Dienst, und versichern Sie sich, dass er ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="980b6-165">Start the service and verify that it is running.</span></span>

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

<span data-ttu-id="980b6-166">Wenn der Reverseproxy konfiguriert ist und Kestrel durch „SystemD“ verwaltet wird, ist die Webanwendung vollständig konfiguriert. Auf diese kann dann über einen Browser auf dem lokalen Computer unter `http://localhost` zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="980b6-166">With the reverse proxy configured and Kestrel managed through systemd, the web application is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="980b6-167">Der Zugriff ist auch von einem Remotecomputer aus möglich, sofern keine Firewall diesen blockiert.</span><span class="sxs-lookup"><span data-stu-id="980b6-167">It is also accessible from a remote machine, barring any firewall that might be blocking.</span></span> <span data-ttu-id="980b6-168">Beim Überprüfen der Antwortheader zeigt der Header `Server` die ASP.NET Core-Anwendung an, die von Kestrel verarbeitet wird.</span><span class="sxs-lookup"><span data-stu-id="980b6-168">Inspecting the response headers, the `Server` header shows the ASP.NET Core application being served by Kestrel.</span></span>

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="980b6-169">Anzeigen von Protokollen</span><span class="sxs-lookup"><span data-stu-id="980b6-169">Viewing logs</span></span>

<span data-ttu-id="980b6-170">Da die Webanwendung, die Kestrel verwendet, von `systemd` verwaltet wird, werden alle Ereignisse und Prozesse in einem zentralen Journal protokolliert.</span><span class="sxs-lookup"><span data-stu-id="980b6-170">Since the web application using Kestrel is managed using `systemd`, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="980b6-171">Dieses Journal enthält jedoch alle Einträge für alle Dienste und Prozesse, die von `systemd` verwaltet werden.</span><span class="sxs-lookup"><span data-stu-id="980b6-171">However, this journal includes all entries for all services and processes managed by `systemd`.</span></span> <span data-ttu-id="980b6-172">Verwenden Sie folgenden Befehl, um die `kestrel-hellomvc.service`-spezifischen Elemente anzuzeigen:</span><span class="sxs-lookup"><span data-stu-id="980b6-172">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="980b6-173">Für weiteres Filtern können Zeitoptionen wie `--since today`, `--until 1 hour ago` oder eine Kombination aus diesen die Menge der zurückgegebenen Einträge verringern.</span><span class="sxs-lookup"><span data-stu-id="980b6-173">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-our-application"></a><span data-ttu-id="980b6-174">Sichern Ihrer Anwendung</span><span class="sxs-lookup"><span data-stu-id="980b6-174">Securing our application</span></span>

### <a name="enable-apparmor"></a><span data-ttu-id="980b6-175">Aktivieren von AppArmor</span><span class="sxs-lookup"><span data-stu-id="980b6-175">Enable AppArmor</span></span>

<span data-ttu-id="980b6-176">Bei Linux Security Modules (LSM) handelt es sich um ein Framework, das seit Linux 2.6 Teil des Linux-Kernels ist.</span><span class="sxs-lookup"><span data-stu-id="980b6-176">Linux Security Modules (LSM) is a framework that is part of the Linux kernel since Linux 2.6.</span></span> <span data-ttu-id="980b6-177">LSM unterstützt verschiedene Implementierungen von Sicherheitsmodulen.</span><span class="sxs-lookup"><span data-stu-id="980b6-177">LSM supports different implementations of security modules.</span></span> <span data-ttu-id="980b6-178">[AppArmor](https://wiki.ubuntu.com/AppArmor) ist ein LSM, das ein obligatorisches Zugriffssteuerungssystem implementiert, das das Programm auf einen beschränkten Satz von Ressourcen begrenzt.</span><span class="sxs-lookup"><span data-stu-id="980b6-178">[AppArmor](https://wiki.ubuntu.com/AppArmor) is a LSM that implements a Mandatory Access Control system which allows confining the program to a limited set of resources.</span></span> <span data-ttu-id="980b6-179">Versichern Sie sich, dass AppArmor aktiviert und ordnungsgemäß konfiguriert ist.</span><span class="sxs-lookup"><span data-stu-id="980b6-179">Ensure AppArmor is enabled and properly configured.</span></span>

### <a name="configuring-our-firewall"></a><span data-ttu-id="980b6-180">Konfigurieren Ihrer Firewall</span><span class="sxs-lookup"><span data-stu-id="980b6-180">Configuring our firewall</span></span>

<span data-ttu-id="980b6-181">Schließen Sie alle externen Ports, die nicht verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="980b6-181">Close off all external ports that are not in use.</span></span> <span data-ttu-id="980b6-182">„Uncomplicated Firewall“ (UFW) stellt ein Front-End für `iptables` bereit, indem eine Befehlszeilenschnittstelle zum Konfigurieren der Firewall bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="980b6-182">Uncomplicated firewall (ufw) provides a front end for `iptables` by providing a command line interface for configuring the firewall.</span></span> <span data-ttu-id="980b6-183">Überprüfen Sie, ob `ufw` konfiguriert ist, um Datenverkehr für alle Ports zuzulassen, die Sie benötigen.</span><span class="sxs-lookup"><span data-stu-id="980b6-183">Verify that `ufw` is configured to allow traffic on any ports you need.</span></span>

```bash
sudo apt-get install ufw
sudo ufw enable

sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### <a name="securing-nginx"></a><span data-ttu-id="980b6-184">Sichern von Nginx</span><span class="sxs-lookup"><span data-stu-id="980b6-184">Securing Nginx</span></span>

<span data-ttu-id="980b6-185">Die Standardverteilung von Nginx aktiviert SSL nicht.</span><span class="sxs-lookup"><span data-stu-id="980b6-185">The default distribution of Nginx doesn't enable SSL.</span></span> <span data-ttu-id="980b6-186">Erstellen Sie aus der Quelle, um zusätzliche Sicherheitsfeatures zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="980b6-186">To enable additional security features, build from source.</span></span>

#### <a name="download-the-source-and-install-the-build-dependencies"></a><span data-ttu-id="980b6-187">Herunterladen der Quelle und Installieren der Buildabhängigkeiten</span><span class="sxs-lookup"><span data-stu-id="980b6-187">Download the source and install the build dependencies</span></span>

```bash
# Install the build dependencies
sudo apt-get update
sudo apt-get install build-essential zlib1g-dev libpcre3-dev libssl-dev libxslt1-dev libxml2-dev libgd2-xpm-dev libgeoip-dev libgoogle-perftools-dev libperl-dev

# Download nginx 1.10.0 or latest
wget http://www.nginx.org/download/nginx-1.10.0.tar.gz
tar zxf nginx-1.10.0.tar.gz
```

#### <a name="change-the-nginx-response-name"></a><span data-ttu-id="980b6-188">Ändern des Namens der Nginx-Antwort</span><span class="sxs-lookup"><span data-stu-id="980b6-188">Change the Nginx response name</span></span>

<span data-ttu-id="980b6-189">Bearbeiten Sie *src/http/ngx_http_header_filter_module.c*:</span><span class="sxs-lookup"><span data-stu-id="980b6-189">Edit *src/http/ngx_http_header_filter_module.c*:</span></span>

```c
static char ngx_http_server_string[] = "Server: Your Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Your Web Server" CRLF;
```

#### <a name="configure-the-options-and-build"></a><span data-ttu-id="980b6-190">Konfigurieren der Optionen und des Builds</span><span class="sxs-lookup"><span data-stu-id="980b6-190">Configure the options and build</span></span>

<span data-ttu-id="980b6-191">Die PCRE-Bibliothek wird für reguläre Ausdrücke benötigt.</span><span class="sxs-lookup"><span data-stu-id="980b6-191">The PCRE library is required for regular expressions.</span></span> <span data-ttu-id="980b6-192">Reguläre Ausdrücke werden im Verzeichnis des Speicherorts von „gx_http_rewrite_module“ verwendet.</span><span class="sxs-lookup"><span data-stu-id="980b6-192">Regular expressions are used in the location directive for the ngx_http_rewrite_module.</span></span> <span data-ttu-id="980b6-193">Durch „http_ssl_module“ wird die Unterstützung für HTTPS-Protokolle hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="980b6-193">The http_ssl_module adds HTTPS protocol support.</span></span>

<span data-ttu-id="980b6-194">Erwägen Sie das Verwenden einer Webanwendungsfirewall wie *ModSecurity*, um Ihre Anwendung zu härten.</span><span class="sxs-lookup"><span data-stu-id="980b6-194">Consider using a web application firewall like *ModSecurity* to harden your application.</span></span>

```bash
./configure
--with-pcre=../pcre-8.38
--with-zlib=../zlib-1.2.8
--with-http_ssl_module
--with-stream
--with-mail=dynamic
```

#### <a name="configure-ssl"></a><span data-ttu-id="980b6-195">Konfigurieren von SSL</span><span class="sxs-lookup"><span data-stu-id="980b6-195">Configure SSL</span></span>

* <span data-ttu-id="980b6-196">Konfigurieren Sie Ihren Server, damit dieser dem HTTPS-Datenverkehr auf Port `443` lauscht, indem Sie ein gültiges Zertifikat angeben, das von einer vertrauenswürdigen Zertifizierungsstelle (CA) ausgestellt wurde.</span><span class="sxs-lookup"><span data-stu-id="980b6-196">Configure your server to listen to HTTPS traffic on port `443` by specifying a valid certificate issued by a trusted Certificate Authority (CA).</span></span>

* <span data-ttu-id="980b6-197">Härten Sie Ihre Sicherheit, indem Sie einige der in der folgenden Datei (*/etc/nginx/nginx.conf*) dargestellten Methoden verwenden.</span><span class="sxs-lookup"><span data-stu-id="980b6-197">Harden your security by employing some of the practices depicted in the following */etc/nginx/nginx.conf* file.</span></span> <span data-ttu-id="980b6-198">Die Beispiele schließen das Auswählen einer stärkeren Verschlüsselung und das Weiterleiten allen Datenverkehrs über HTTP auf HTTPS ein.</span><span class="sxs-lookup"><span data-stu-id="980b6-198">Examples include choosing a stronger cipher and redirecting all traffic over HTTP to HTTPS.</span></span>

* <span data-ttu-id="980b6-199">Durch das Hinzufügen eines `HTTP Strict-Transport-Security`-Headers (HSTS) wird sichergestellt, dass alle nachfolgenden Anforderungen vom Client nur über HTTPS erfolgen.</span><span class="sxs-lookup"><span data-stu-id="980b6-199">Adding an `HTTP Strict-Transport-Security` (HSTS) header ensures all subsequent requests made by the client are over HTTPS only.</span></span>

* <span data-ttu-id="980b6-200">Fügen Sie keinen Strict-Transport-Security-Header hinzu, oder wählen Sie ein zuverlässiges `max-age`, wenn Sie SSL in Zukunft deaktivieren möchten.</span><span class="sxs-lookup"><span data-stu-id="980b6-200">Do not add the Strict-Transport-Security header or chose an appropriate `max-age` if you plan to disable SSL in the future.</span></span>

<span data-ttu-id="980b6-201">Fügen Sie die Konfigurationsdatei */etc/nginx/proxy.conf* hinzu:</span><span class="sxs-lookup"><span data-stu-id="980b6-201">Add the */etc/nginx/proxy.conf* configuration file:</span></span>

<span data-ttu-id="980b6-202">[!code-nginx[Main](linuxproduction/proxy.conf)]</span><span class="sxs-lookup"><span data-stu-id="980b6-202">[!code-nginx[Main](linuxproduction/proxy.conf)]</span></span>

<span data-ttu-id="980b6-203">Bearbeiten Sie die Konfigurationsdatei */etc/nginx/nginx.conf*.</span><span class="sxs-lookup"><span data-stu-id="980b6-203">Edit the */etc/nginx/nginx.conf* configuration file.</span></span> <span data-ttu-id="980b6-204">Das Beispiel enthält die Abschnitte `http` und `server` in einer Konfigurationsdatei.</span><span class="sxs-lookup"><span data-stu-id="980b6-204">The example contains both `http` and `server` sections in one configuration file.</span></span>

<span data-ttu-id="980b6-205">[!code-nginx[Main](../publishing/linuxproduction/nginx.conf?highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="980b6-205">[!code-nginx[Main](../publishing/linuxproduction/nginx.conf?highlight=2)]</span></span>

#### <a name="secure-nginx-from-clickjacking"></a><span data-ttu-id="980b6-206">Sichern von Nginx vor Clickjacking</span><span class="sxs-lookup"><span data-stu-id="980b6-206">Secure Nginx from clickjacking</span></span>
<span data-ttu-id="980b6-207">Clickjacking ist eine böswillige Technik zum Sammeln der Klicks eines infizierten Benutzers.</span><span class="sxs-lookup"><span data-stu-id="980b6-207">Clickjacking is a malicious technique to collect an infected user's clicks.</span></span> <span data-ttu-id="980b6-208">Clickjacking bringt das Opfer (Besucher) dazu, auf eine infizierte Seite zu klicken.</span><span class="sxs-lookup"><span data-stu-id="980b6-208">Clickjacking tricks the victim (visitor) into clicking on an infected site.</span></span> <span data-ttu-id="980b6-209">Verwenden Sie „X-FRAME-OPTIONS“ zum Sichern Ihrer Website.</span><span class="sxs-lookup"><span data-stu-id="980b6-209">Use X-FRAME-OPTIONS to secure your site.</span></span>

<span data-ttu-id="980b6-210">Bearbeiten Sie die Datei *nginx.conf*:</span><span class="sxs-lookup"><span data-stu-id="980b6-210">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="980b6-211">Fügen Sie die Zeile `add_header X-Frame-Options "SAMEORIGIN";` hinzu, und speichern Sie die Datei. Starten Sie Nginx dann neu.</span><span class="sxs-lookup"><span data-stu-id="980b6-211">Add the line `add_header X-Frame-Options "SAMEORIGIN";` and save the file, then restart Nginx.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="980b6-212">MIME-Typermittlung</span><span class="sxs-lookup"><span data-stu-id="980b6-212">MIME-type sniffing</span></span>

<span data-ttu-id="980b6-213">Dieser Header hindert die meisten Browser an der MIME-Ermittlung einer Antwort vom deklarierten Inhaltstyp weg, da der Header den Browser anweist, den Inhaltstyp der Antwort nicht zu überschreiben.</span><span class="sxs-lookup"><span data-stu-id="980b6-213">This header prevents most browsers from MIME-sniffing a response away from the declared content type, as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="980b6-214">Durch die Option `nosniff` wird der Inhalt vom Browser als „text/html“ gerendert, wenn der Server sagt, dass es sich bei diesem um „text/html“ handelt.</span><span class="sxs-lookup"><span data-stu-id="980b6-214">With the `nosniff` option, if the server says the content is "text/html", the browser renders it as "text/html".</span></span>

<span data-ttu-id="980b6-215">Bearbeiten Sie die Datei *nginx.conf*:</span><span class="sxs-lookup"><span data-stu-id="980b6-215">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="980b6-216">Fügen Sie die Zeile `add_header X-Content-Type-Options "nosniff";` hinzu, und speichern Sie die Datei. Starten Sie Nginx dann neu.</span><span class="sxs-lookup"><span data-stu-id="980b6-216">Add the line `add_header X-Content-Type-Options "nosniff";` and save the file, then restart Nginx.</span></span>
