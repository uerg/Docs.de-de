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
ms.openlocfilehash: 033adddc586b60c9f7453df5434617aa838737f8
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/15/2018
---
# <a name="host-aspnet-core-on-linux-with-apache"></a><span data-ttu-id="2e783-103">Hosten von ASP.NET Core unter Linux mit Apache</span><span class="sxs-lookup"><span data-stu-id="2e783-103">Host ASP.NET Core on Linux with Apache</span></span>

<span data-ttu-id="2e783-104">Von [Shayne Boyer](https://github.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="2e783-104">By [Shayne Boyer](https://github.com/spboyer)</span></span>

<span data-ttu-id="2e783-105">Mithilfe dieser Anleitung erfahren Sie, wie einrichten [Apache](https://httpd.apache.org/) als reverse-Proxy-Server auf [CentOS 7](https://www.centos.org/) zum Umleiten von HTTP-Datenverkehr an eine ASP.NET Core-Web-app unter [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="2e783-105">Using this guide, learn how to set up [Apache](https://httpd.apache.org/) as a reverse proxy server on [CentOS 7](https://www.centos.org/) to redirect HTTP traffic to an ASP.NET Core web app running on [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="2e783-106">Die [Mod_proxy Erweiterung](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) und zugehörigen Module Reverseproxy des Servers zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="2e783-106">The [mod_proxy extension](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) and related modules create the server's reverse proxy.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2e783-107">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="2e783-107">Prerequisites</span></span>

1. <span data-ttu-id="2e783-108">Server mit CentOS 7 muss ein Standardbenutzerkonto mit Sudo-Berechtigungen</span><span class="sxs-lookup"><span data-stu-id="2e783-108">Server running CentOS 7 with a standard user account with sudo privilege</span></span>
2. <span data-ttu-id="2e783-109">ASP.NET Core-app</span><span class="sxs-lookup"><span data-stu-id="2e783-109">ASP.NET Core app</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="2e783-110">Veröffentlichen der App</span><span class="sxs-lookup"><span data-stu-id="2e783-110">Publish the app</span></span>

<span data-ttu-id="2e783-111">Veröffentlichen Sie die app als eine [eigenständige Bereitstellung](/dotnet/core/deploying/#self-contained-deployments-scd) im Release-Konfiguration für die Laufzeit CentOS 7 (`centos.7-x64`).</span><span class="sxs-lookup"><span data-stu-id="2e783-111">Publish the app as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) in Release configuration for the CentOS 7 runtime (`centos.7-x64`).</span></span> <span data-ttu-id="2e783-112">Kopieren Sie den Inhalt von der *bin/Release/netcoreapp2.0/centos.7-x64/publish* Ordner auf dem Server mit SCP, FTP oder andere Dateiübertragungsmethode verwenden.</span><span class="sxs-lookup"><span data-stu-id="2e783-112">Copy the contents of the *bin/Release/netcoreapp2.0/centos.7-x64/publish* folder to the server using SCP, FTP, or other file transfer method.</span></span>

> [!NOTE]
> <span data-ttu-id="2e783-113">Unter einem Produktionsszenario Bereitstellung funktioniert ein fortlaufende Integration Workflow die Veröffentlichung der app, und kopieren die Ressourcen auf den Server.</span><span class="sxs-lookup"><span data-stu-id="2e783-113">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span> 

## <a name="configure-a-proxy-server"></a><span data-ttu-id="2e783-114">Konfigurieren eines Proxyservers</span><span class="sxs-lookup"><span data-stu-id="2e783-114">Configure a proxy server</span></span>

<span data-ttu-id="2e783-115">Reverse-Proxy ist ein gemeinsames Setup dynamic Web-apps zu verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="2e783-115">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="2e783-116">Reverse-Proxy die HTTP-Anforderung beendet und an die ASP.NET app weiterleitet.</span><span class="sxs-lookup"><span data-stu-id="2e783-116">The reverse proxy terminates the HTTP request and forwards it to the ASP.NET app.</span></span>

<span data-ttu-id="2e783-117">Ein Proxyserver ist eine Clientanforderungen an einen anderen Server anstelle von Anforderungen erfüllen selbst weitergeleitet.</span><span class="sxs-lookup"><span data-stu-id="2e783-117">A proxy server is one which forwards client requests to another server instead of fulfilling requests itself.</span></span> <span data-ttu-id="2e783-118">Ein Reverseproxy leitet Daten an ein festes Ziel meist im Auftrag beliebiger Clients weiter.</span><span class="sxs-lookup"><span data-stu-id="2e783-118">A reverse proxy forwards to a fixed destination, typically on behalf of arbitrary clients.</span></span> <span data-ttu-id="2e783-119">In diesem Handbuch wird Apache konfiguriert, als den reverse-Proxy auf dem gleichen Server ausgeführt wird, dass Kestrel ASP.NET Core app bedient.</span><span class="sxs-lookup"><span data-stu-id="2e783-119">In this guide, Apache is configured as the reverse proxy running on the same server that Kestrel is serving the ASP.NET Core app.</span></span>

<span data-ttu-id="2e783-120">Da Anforderungen von Reverseproxy weitergeleitet werden, verwenden Sie die Middleware weitergeleitet Header aus der [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) Paket.</span><span class="sxs-lookup"><span data-stu-id="2e783-120">Because requests are forwarded by reverse proxy, use the Forwarded Headers Middleware from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="2e783-121">Die Middleware Updates der `Request.Scheme`unter Verwendung der `X-Forwarded-Proto` -Header, damit diese umleitungs-URIs und andere Sicherheitsrichtlinien ordnungsgemäß funktioniert.</span><span class="sxs-lookup"><span data-stu-id="2e783-121">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="2e783-122">Wenn Sie einen beliebigen Typ von Authentifizierung-Middleware zu verwenden, muss der Header weitergeleitet Middleware zuerst ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="2e783-122">When using any type of authentication middleware, the Forwarded Headers Middleware must run first.</span></span> <span data-ttu-id="2e783-123">Dieser Anordnung wird sichergestellt, dass die authentifizierungsmiddleware die Headerwerte beansprucht. außerdem Generieren der richtigen umleitungs-URIs.</span><span class="sxs-lookup"><span data-stu-id="2e783-123">This ordering ensures that the authentication middleware can consume the header values and generate correct redirect URIs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2e783-124">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2e783-124">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="2e783-125">Aufrufen der [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) Methode im `Startup.Configure` vor dem Aufruf [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) oder ähnliche authentifizierungsmiddleware Schema:</span><span class="sxs-lookup"><span data-stu-id="2e783-125">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2e783-126">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2e783-126">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="2e783-127">Aufrufen der [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) Methode im `Startup.Configure` vor dem Aufruf [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) und [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) oder ähnliche Authentifizierungsschema Middleware:</span><span class="sxs-lookup"><span data-stu-id="2e783-127">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware:</span></span>

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

<span data-ttu-id="2e783-128">Wenn kein [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) werden angegeben, um die Middleware, die Standardheader zum Weiterleiten sind `None`.</span><span class="sxs-lookup"><span data-stu-id="2e783-128">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

### <a name="install-apache"></a><span data-ttu-id="2e783-129">Installieren von Apache</span><span class="sxs-lookup"><span data-stu-id="2e783-129">Install Apache</span></span>

<span data-ttu-id="2e783-130">CentOS-Pakete, die jeweils neuesten stabilen Version zu aktualisieren:</span><span class="sxs-lookup"><span data-stu-id="2e783-130">Update CentOS packages to their latest stable versions:</span></span>

```bash
sudo yum update -y
```

<span data-ttu-id="2e783-131">Installieren Sie die Apache-Webserver auf CentOS, mit einem einzelnen `yum` Befehl:</span><span class="sxs-lookup"><span data-stu-id="2e783-131">Install the Apache web server on CentOS with a single `yum` command:</span></span>

```bash
sudo yum -y install httpd mod_ssl
```

<span data-ttu-id="2e783-132">Beispielausgabe nach dem Ausführen des Befehls:</span><span class="sxs-lookup"><span data-stu-id="2e783-132">Sample output after running the command:</span></span>

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
> <span data-ttu-id="2e783-133">In diesem Beispiel gibt die Ausgabe httpd.86_64 wieder, da die Version CentOS 7 64-Bit ist.</span><span class="sxs-lookup"><span data-stu-id="2e783-133">In this example, the output reflects httpd.86_64 since the CentOS 7 version is 64 bit.</span></span> <span data-ttu-id="2e783-134">Um zu prüfen, wo Apache installiert ist, führen Sie an einer Eingabeaufforderung `whereis httpd` aus.</span><span class="sxs-lookup"><span data-stu-id="2e783-134">To verify where Apache is installed, run `whereis httpd` from a command prompt.</span></span>

### <a name="configure-apache-for-reverse-proxy"></a><span data-ttu-id="2e783-135">Konfigurieren von Apache als Reverseproxy</span><span class="sxs-lookup"><span data-stu-id="2e783-135">Configure Apache for reverse proxy</span></span>

<span data-ttu-id="2e783-136">Konfigurationsdateien für Apache befinden sich im Verzeichnis `/etc/httpd/conf.d/`.</span><span class="sxs-lookup"><span data-stu-id="2e783-136">Configuration files for Apache are located within the `/etc/httpd/conf.d/` directory.</span></span> <span data-ttu-id="2e783-137">Alle Dateien mit der *.conf* Erweiterung wird verarbeitet, in alphabetischer Reihenfolge zusätzlich zu den in der Modul-Konfigurationsdateien `/etc/httpd/conf.modules.d/`, die Konfiguration enthält Dateien erforderlich, um Module zu laden.</span><span class="sxs-lookup"><span data-stu-id="2e783-137">Any file with the *.conf* extension is processed in alphabetical order in addition to the module configuration files in `/etc/httpd/conf.modules.d/`, which contains any configuration files necessary to load modules.</span></span>

<span data-ttu-id="2e783-138">Erstellen Sie eine Konfigurationsdatei mit dem Namen *hellomvc.conf*, für die app:</span><span class="sxs-lookup"><span data-stu-id="2e783-138">Create a configuration file, named *hellomvc.conf*, for the app:</span></span>

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

<span data-ttu-id="2e783-139">Die `VirtualHost` Block kann mehrere Male in eine oder mehrere Dateien auf einem Server angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="2e783-139">The `VirtualHost` block can appear multiple times, in one or more files on a server.</span></span> <span data-ttu-id="2e783-140">In der vorangegangenen Konfigurationsdatei akzeptiert Apache öffentlichen Datenverkehr über Port 80.</span><span class="sxs-lookup"><span data-stu-id="2e783-140">In the preceding configuration file, Apache accepts public traffic on port 80.</span></span> <span data-ttu-id="2e783-141">Die Domäne `www.example.com` ist gesendet werden, und die `*.example.com` Alias in der gleichen Website aufgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="2e783-141">The domain `www.example.com` is being served, and the `*.example.com` alias resolves to the same website.</span></span> <span data-ttu-id="2e783-142">Finden Sie unter [Namen-basierte virtuelle Host Unterstützung](https://httpd.apache.org/docs/current/vhosts/name-based.html) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="2e783-142">See [Name-based virtual host support](https://httpd.apache.org/docs/current/vhosts/name-based.html) for more information.</span></span> <span data-ttu-id="2e783-143">Anforderungen werden auf der Stammebene an Port 5000 des Servers 127.0.0.1 Proxyanforderungen.</span><span class="sxs-lookup"><span data-stu-id="2e783-143">Requests are proxied at the root to port 5000 of the server at 127.0.0.1.</span></span> <span data-ttu-id="2e783-144">Für die bidirektionale Kommunikation `ProxyPass` und `ProxyPassReverse` erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="2e783-144">For bi-directional communication, `ProxyPass` and `ProxyPassReverse` are required.</span></span>

> [!WARNING]
> <span data-ttu-id="2e783-145">Fehler an eine echte [ServerName Richtlinie](https://httpd.apache.org/docs/current/mod/core.html#servername) in der **VirtualHost** Block macht Ihre app zu Sicherheitsrisiken.</span><span class="sxs-lookup"><span data-stu-id="2e783-145">Failure to specify a proper [ServerName directive](https://httpd.apache.org/docs/current/mod/core.html#servername) in the **VirtualHost** block exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="2e783-146">Unterdomäne Platzhalter Bindung (z. B. `*.example.com`) nicht dieses Sicherheitsrisiko darstellen, wenn Sie steuern, dass die gesamte übergeordnete Domäne (im Gegensatz zu `*.com`, anfällig ist).</span><span class="sxs-lookup"><span data-stu-id="2e783-146">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="2e783-147">Finden Sie unter [rfc7230 Abschnitt-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="2e783-147">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="2e783-148">Protokollierung kann konfiguriert werden, pro `VirtualHost` mit `ErrorLog` und `CustomLog` Direktiven.</span><span class="sxs-lookup"><span data-stu-id="2e783-148">Logging can be configured per `VirtualHost` using `ErrorLog` and `CustomLog` directives.</span></span> <span data-ttu-id="2e783-149">`ErrorLog` ist der Speicherort, in dem der Server Fehler protokolliert, und `CustomLog` legt den Dateinamen und das Format der Protokolldatei.</span><span class="sxs-lookup"><span data-stu-id="2e783-149">`ErrorLog` is the location where the server logs errors, and `CustomLog` sets the filename and format of log file.</span></span> <span data-ttu-id="2e783-150">In diesem Fall ist dies dem Anforderungsinformationen protokolliert wird.</span><span class="sxs-lookup"><span data-stu-id="2e783-150">In this case, this is where request information is logged.</span></span> <span data-ttu-id="2e783-151">Es wird nur eine Zeile für jede Anforderung ein.</span><span class="sxs-lookup"><span data-stu-id="2e783-151">There's one line for each request.</span></span>

<span data-ttu-id="2e783-152">Speichern Sie die Datei, und Testen der Konfiguration.</span><span class="sxs-lookup"><span data-stu-id="2e783-152">Save the file and test the configuration.</span></span> <span data-ttu-id="2e783-153">Wenn alle Anforderungen durchgehen, muss die Antwort `Syntax [OK]` lauten.</span><span class="sxs-lookup"><span data-stu-id="2e783-153">If everything passes, the response should be `Syntax [OK]`.</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="2e783-154">Starten Sie die Apache neu:</span><span class="sxs-lookup"><span data-stu-id="2e783-154">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

## <a name="monitoring-the-app"></a><span data-ttu-id="2e783-155">Überwachen der app</span><span class="sxs-lookup"><span data-stu-id="2e783-155">Monitoring the app</span></span>

<span data-ttu-id="2e783-156">Apache ist jetzt Setup zum Weiterleiten von Anforderungen an `http://localhost:80` der ASP.NET Core-App unter Kestrel am `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="2e783-156">Apache is now setup to forward requests made to `http://localhost:80` to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span>  <span data-ttu-id="2e783-157">Apache allerdings ist nicht eingerichtet Kestrel verwalten.</span><span class="sxs-lookup"><span data-stu-id="2e783-157">However, Apache isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="2e783-158">Verwendung *Systemd* , und erstellen Sie eine Dienstdatei zum Starten und überwachen die zugrunde liegenden Web-app.</span><span class="sxs-lookup"><span data-stu-id="2e783-158">Use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="2e783-159">*SystemD* ist ein Initialisierungssystem, das viele leistungsstarke Features zum Starten, Beenden und Verwalten von Prozessen bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="2e783-159">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 


### <a name="create-the-service-file"></a><span data-ttu-id="2e783-160">Erstellen der Dienstdatei</span><span class="sxs-lookup"><span data-stu-id="2e783-160">Create the service file</span></span>

<span data-ttu-id="2e783-161">Erstellen Sie die Dienstdefinitionsdatei:</span><span class="sxs-lookup"><span data-stu-id="2e783-161">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="2e783-162">Eine Beispiel-Service-Datei für die app:</span><span class="sxs-lookup"><span data-stu-id="2e783-162">An example service file for the app:</span></span>

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
> <span data-ttu-id="2e783-163">**Benutzer** &mdash; Wenn der Benutzer *Apache* verwendet, wird nicht durch die Konfiguration der Benutzer zunächst erstellt und ordnungsgemäße den Besitz für Dateien angegeben werden muss.</span><span class="sxs-lookup"><span data-stu-id="2e783-163">**User** &mdash; If the user *apache* isn't used by the configuration, the user must be created first and given proper ownership for files.</span></span>

<span data-ttu-id="2e783-164">Speichern Sie die Datei, und aktivieren Sie den Dienst:</span><span class="sxs-lookup"><span data-stu-id="2e783-164">Save the file and enable the service:</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="2e783-165">Starten Sie den Dienst, und stellen Sie sicher, dass er ausgeführt wird:</span><span class="sxs-lookup"><span data-stu-id="2e783-165">Start the service and verify that it's running:</span></span>

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

<span data-ttu-id="2e783-166">Mit der reverse-Proxy konfiguriert und verwaltet Kestrel *Systemd*, die Web-app ist vollständig konfiguriert und zugegriffen werden kann, in einem Browser auf dem lokalen Computer am `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="2e783-166">With the reverse proxy configured and Kestrel managed through *systemd*, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="2e783-167">Überprüfen die Antwortheader, die **Server** Header angibt, dass die app ASP.NET Core durch Kestrel verarbeitet wird:</span><span class="sxs-lookup"><span data-stu-id="2e783-167">Inspecting the response headers, the **Server** header indicates that the ASP.NET Core app is served by Kestrel:</span></span>

```
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="2e783-168">Anzeigen von Protokollen</span><span class="sxs-lookup"><span data-stu-id="2e783-168">Viewing logs</span></span>

<span data-ttu-id="2e783-169">Seit der Web-app mit Kestrel wird verwaltet mit *Systemd*, Ereignisse und Prozesse werden auf einer zentralen Journal protokolliert.</span><span class="sxs-lookup"><span data-stu-id="2e783-169">Since the web app using Kestrel is managed using *systemd*, events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="2e783-170">Allerdings umfasst diese Erfassung Einträge für alle Dienste und Prozesse, die von verwalteten *Systemd*.</span><span class="sxs-lookup"><span data-stu-id="2e783-170">However, this journal includes entries for all of the services and processes managed by *systemd*.</span></span> <span data-ttu-id="2e783-171">Verwenden Sie folgenden Befehl, um die `kestrel-hellomvc.service`-spezifischen Elemente anzuzeigen:</span><span class="sxs-lookup"><span data-stu-id="2e783-171">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="2e783-172">Geben Sie Optionen für die uhrzeitfilterung, mit dem Befehl.</span><span class="sxs-lookup"><span data-stu-id="2e783-172">For time filtering, specify time options with the command.</span></span> <span data-ttu-id="2e783-173">Verwenden Sie z. B. `--since today` für den aktuellen Tag filtern oder `--until 1 hour ago` auf der vorherigen Stunde Einträge angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="2e783-173">For example, use `--since today` to filter for the current day or `--until 1 hour ago` to see the previous hour's entries.</span></span> <span data-ttu-id="2e783-174">Weitere Informationen finden Sie unter der [Man-Seite für Journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span><span class="sxs-lookup"><span data-stu-id="2e783-174">For more information, see the [man page for journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="2e783-175">Sichern die app</span><span class="sxs-lookup"><span data-stu-id="2e783-175">Securing the app</span></span>

### <a name="configure-firewall"></a><span data-ttu-id="2e783-176">Konfigurieren der Firewall</span><span class="sxs-lookup"><span data-stu-id="2e783-176">Configure firewall</span></span>

<span data-ttu-id="2e783-177">*Firewalld* ist eine dynamische Daemon zum Verwalten der Firewall mit Unterstützung für Netzwerkzonen.</span><span class="sxs-lookup"><span data-stu-id="2e783-177">*Firewalld* is a dynamic daemon to manage the firewall with support for network zones.</span></span> <span data-ttu-id="2e783-178">Ports und paketfilterung können immer noch mit Iptables verwaltet werden.</span><span class="sxs-lookup"><span data-stu-id="2e783-178">Ports and packet filtering can still be managed by iptables.</span></span> <span data-ttu-id="2e783-179">*Firewalld* sollte standardmäßig installiert werden.</span><span class="sxs-lookup"><span data-stu-id="2e783-179">*Firewalld* should be installed by default.</span></span> <span data-ttu-id="2e783-180">`yum` kann verwendet werden, um das Paket zu installieren, oder stellen Sie sicher, dass es installiert ist.</span><span class="sxs-lookup"><span data-stu-id="2e783-180">`yum` can be used to install the package or verify it's installed.</span></span>

```bash
sudo yum install firewalld -y
```

<span data-ttu-id="2e783-181">Verwendung `firewalld` nur den erforderlichen Ports für die app zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="2e783-181">Use `firewalld` to open only the ports needed for the app.</span></span> <span data-ttu-id="2e783-182">In diesem Fall werden die Ports 80 und 443 verwendet.</span><span class="sxs-lookup"><span data-stu-id="2e783-182">In this case, port 80 and 443 are used.</span></span> <span data-ttu-id="2e783-183">Die folgenden Befehle festlegen dauerhaft Ports 80 und 443 geöffnet:</span><span class="sxs-lookup"><span data-stu-id="2e783-183">The following commands permanently set ports 80 and 443 to open:</span></span>

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
```

<span data-ttu-id="2e783-184">Laden Sie die Firewall-Einstellungen erneut.</span><span class="sxs-lookup"><span data-stu-id="2e783-184">Reload the firewall settings.</span></span> <span data-ttu-id="2e783-185">Überprüfen Sie die verfügbaren Dienste und Ports in der Standardzone.</span><span class="sxs-lookup"><span data-stu-id="2e783-185">Check the available services and ports in the default zone.</span></span> <span data-ttu-id="2e783-186">Optionen sind verfügbar, durch Überprüfen `firewall-cmd -h`.</span><span class="sxs-lookup"><span data-stu-id="2e783-186">Options are available by inspecting `firewall-cmd -h`.</span></span>

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

### <a name="ssl-configuration"></a><span data-ttu-id="2e783-187">SSL-Konfiguration</span><span class="sxs-lookup"><span data-stu-id="2e783-187">SSL configuration</span></span>

<span data-ttu-id="2e783-188">So konfigurieren Sie die Apache für SSL, die *Mod_ssl* Modul verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="2e783-188">To configure Apache for SSL, the *mod_ssl* module is used.</span></span> <span data-ttu-id="2e783-189">Wenn die *Httpd* -Modul wurde installiert, die *Mod_ssl* auch-Modul installiert wurde.</span><span class="sxs-lookup"><span data-stu-id="2e783-189">When the *httpd* module was installed, the *mod_ssl* module was also installed.</span></span> <span data-ttu-id="2e783-190">Wenn sie mit dem wurde nicht installiert sind, verwenden `yum` die Konfiguration hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="2e783-190">If it wasn't installed, use `yum` to add it to the configuration.</span></span>

```bash
sudo yum install mod_ssl
```
<span data-ttu-id="2e783-191">Um SSL zu erzwingen, installieren die `mod_rewrite` Modul, um URLs zu aktivieren:</span><span class="sxs-lookup"><span data-stu-id="2e783-191">To enforce SSL, install the `mod_rewrite` module to enable URL rewriting:</span></span>

```bash
sudo yum install mod_rewrite
```

<span data-ttu-id="2e783-192">Ändern der *hellomvc.conf* Datei aktivieren die URLs und sichere Kommunikation über Port 443:</span><span class="sxs-lookup"><span data-stu-id="2e783-192">Modify the *hellomvc.conf* file to enable URL rewriting and secure communication on port 443:</span></span>

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
> <span data-ttu-id="2e783-193">In diesem Beispiel wird eine lokal generiertes Zertifikat verwendet.</span><span class="sxs-lookup"><span data-stu-id="2e783-193">This example is using a locally-generated certificate.</span></span> <span data-ttu-id="2e783-194">**SSLCertificateFile** muss die primäre Zertifikatdatei für den Domänennamen.</span><span class="sxs-lookup"><span data-stu-id="2e783-194">**SSLCertificateFile** should be the primary certificate file for the domain name.</span></span> <span data-ttu-id="2e783-195">**SSLCertificateKeyFile** sollten die Schlüsseldatei generiert, wenn CSR erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="2e783-195">**SSLCertificateKeyFile** should be the key file generated when CSR is created.</span></span> <span data-ttu-id="2e783-196">**SSLCertificateChainFile** muss die Zwischenzertifikat-Datei (falls vorhanden), die von der Zertifizierungsstelle angegeben wurde.</span><span class="sxs-lookup"><span data-stu-id="2e783-196">**SSLCertificateChainFile** should be the intermediate certificate file (if any) that was supplied by the certificate authority.</span></span>

<span data-ttu-id="2e783-197">Speichern Sie die Datei, und Testen der Konfiguration:</span><span class="sxs-lookup"><span data-stu-id="2e783-197">Save the file and test the configuration:</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="2e783-198">Starten Sie die Apache neu:</span><span class="sxs-lookup"><span data-stu-id="2e783-198">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a><span data-ttu-id="2e783-199">Weitere Vorschläge für Apache</span><span class="sxs-lookup"><span data-stu-id="2e783-199">Additional Apache suggestions</span></span>

### <a name="additional-headers"></a><span data-ttu-id="2e783-200">Zusätzliche Header</span><span class="sxs-lookup"><span data-stu-id="2e783-200">Additional headers</span></span>

<span data-ttu-id="2e783-201">Um Ihre vor böswilligen Angriffen zu schützen, gibt es einige Header, die entweder geändert oder hinzugefügt werden soll.</span><span class="sxs-lookup"><span data-stu-id="2e783-201">In order to secure against malicious attacks, there are a few headers that should either be modified or added.</span></span> <span data-ttu-id="2e783-202">Sicherstellen, dass die `mod_headers` -Modul installiert ist:</span><span class="sxs-lookup"><span data-stu-id="2e783-202">Ensure that the `mod_headers` module is installed:</span></span>

```bash
sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking-attacks"></a><span data-ttu-id="2e783-203">Sichere Apache Angriffen clickjacking</span><span class="sxs-lookup"><span data-stu-id="2e783-203">Secure Apache from clickjacking attacks</span></span>

<span data-ttu-id="2e783-204">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), auch bekannt als ein *UI Schadenersatz Angriff*, wird ein bösartigen Angriffs, in denen ein Besucher auf einen Link oder eine Schaltfläche auf einer anderen Seite verleitet ist als derzeit besuchte.</span><span class="sxs-lookup"><span data-stu-id="2e783-204">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), also known as a *UI redress attack*, is a malicious attack where a website visitor is tricked into clicking a link or button on a different page than they're currently visiting.</span></span> <span data-ttu-id="2e783-205">Verwendung `X-FRAME-OPTIONS` zum Sichern der Site.</span><span class="sxs-lookup"><span data-stu-id="2e783-205">Use `X-FRAME-OPTIONS` to secure the site.</span></span>

<span data-ttu-id="2e783-206">Bearbeiten der *httpd.conf* Datei:</span><span class="sxs-lookup"><span data-stu-id="2e783-206">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="2e783-207">Fügen Sie die Zeile `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span><span class="sxs-lookup"><span data-stu-id="2e783-207">Add the line `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span></span> <span data-ttu-id="2e783-208">Speichern Sie die Datei.</span><span class="sxs-lookup"><span data-stu-id="2e783-208">Save the file.</span></span> <span data-ttu-id="2e783-209">Starten Sie Apache neu.</span><span class="sxs-lookup"><span data-stu-id="2e783-209">Restart Apache.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="2e783-210">MIME-Typermittlung</span><span class="sxs-lookup"><span data-stu-id="2e783-210">MIME-type sniffing</span></span>

<span data-ttu-id="2e783-211">Die `X-Content-Type-Options` Header wird verhindert, dass Internet Explorer aus *MIME-Ermittlung* (einer Datei ermitteln `Content-Type` vom Inhalt der Datei).</span><span class="sxs-lookup"><span data-stu-id="2e783-211">The `X-Content-Type-Options` header prevents Internet Explorer from *MIME-sniffing* (determining a file's `Content-Type` from the file's content).</span></span> <span data-ttu-id="2e783-212">Wenn der Server bestimmt die `Content-Type` Header `text/html` mit der `nosniff` -Option festgelegt, Internet Explorer rendert den Inhalt als `text/html` unabhängig vom Inhalt der Datei.</span><span class="sxs-lookup"><span data-stu-id="2e783-212">If the server sets the `Content-Type` header to `text/html` with the `nosniff` option set, Internet Explorer renders the content as `text/html` regardless of the file's content.</span></span>

<span data-ttu-id="2e783-213">Bearbeiten der *httpd.conf* Datei:</span><span class="sxs-lookup"><span data-stu-id="2e783-213">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="2e783-214">Fügen Sie die Zeile `Header set X-Content-Type-Options "nosniff"`.</span><span class="sxs-lookup"><span data-stu-id="2e783-214">Add the line `Header set X-Content-Type-Options "nosniff"`.</span></span> <span data-ttu-id="2e783-215">Speichern Sie die Datei.</span><span class="sxs-lookup"><span data-stu-id="2e783-215">Save the file.</span></span> <span data-ttu-id="2e783-216">Starten Sie Apache neu.</span><span class="sxs-lookup"><span data-stu-id="2e783-216">Restart Apache.</span></span>

### <a name="load-balancing"></a><span data-ttu-id="2e783-217">Lastenausgleich</span><span class="sxs-lookup"><span data-stu-id="2e783-217">Load Balancing</span></span> 

<span data-ttu-id="2e783-218">Dieses Beispiel veranschaulicht das Einrichten und Konfigurieren von Apache unter CentOS 7 und Kestrel auf demselben Instanzcomputer.</span><span class="sxs-lookup"><span data-stu-id="2e783-218">This example shows how to setup and configure Apache on CentOS 7 and Kestrel on the same instance machine.</span></span> <span data-ttu-id="2e783-219">Damit keine einzelne Fehlerquelle; mit *Mod_proxy_balancer* und Ändern der **VirtualHost** entschied sich für die Verwaltung von mehreren Instanzen von webapps hinter dem Proxyserver Apache.</span><span class="sxs-lookup"><span data-stu-id="2e783-219">In order to not have a single point of failure; using *mod_proxy_balancer* and modifying the **VirtualHost** would allow for managing multiple instances of the web apps behind the Apache proxy server.</span></span>

```bash
sudo yum install mod_proxy_balancer
```

<span data-ttu-id="2e783-220">In der Konfigurationsdatei dargestellt, unten eine zusätzliche Instanz von der `hellomvc` app ist für Port 5001 ausführen.</span><span class="sxs-lookup"><span data-stu-id="2e783-220">In the configuration file shown below, an additional instance of the `hellomvc` app is setup to run on port 5001.</span></span> <span data-ttu-id="2e783-221">Die *Proxy* Abschnitt mit einem Lastenausgleichsmodul-Konfiguration mit zwei Membern festgelegt ist, für den Lastenausgleich *Byrequests*.</span><span class="sxs-lookup"><span data-stu-id="2e783-221">The *Proxy* section is set with a balancer configuration with two members to load balance *byrequests*.</span></span>

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

### <a name="rate-limits"></a><span data-ttu-id="2e783-222">Begrenzung der Bandbreite</span><span class="sxs-lookup"><span data-stu-id="2e783-222">Rate Limits</span></span>
<span data-ttu-id="2e783-223">Mit *Mod_ratelimit*, im Lieferumfang der *Httpd* Modul, die Bandbreite von Clients beschränkt werden kann:</span><span class="sxs-lookup"><span data-stu-id="2e783-223">Using *mod_ratelimit*, which is included in the *httpd* module, the bandwidth of clients can be limited:</span></span>

```bash
sudo nano /etc/httpd/conf.d/ratelimit.conf
```
<span data-ttu-id="2e783-224">Die Beispieldatei begrenzt die Bandbreite als 600 KB/Sek., unter dem Stammspeicherort:</span><span class="sxs-lookup"><span data-stu-id="2e783-224">The example file limits bandwidth as 600 KB/sec under the root location:</span></span>

```
<IfModule mod_ratelimit.c>
    <Location />
        SetOutputFilter RATE_LIMIT
        SetEnv rate-limit 600
    </Location>
</IfModule>
```
