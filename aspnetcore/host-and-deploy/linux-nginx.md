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
# <a name="host-aspnet-core-on-linux-with-nginx"></a><span data-ttu-id="67ee7-103">Hosten von ASP.NET Core unter Linux mit Nginx</span><span class="sxs-lookup"><span data-stu-id="67ee7-103">Host ASP.NET Core on Linux with Nginx</span></span>

<span data-ttu-id="67ee7-104">Von [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="67ee7-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="67ee7-105">Dieser Leitfaden erläutert das Einrichten einer produktionsbereiten ASP.NET Core-Umgebungen auf einem Ubuntu 16.04-Server.</span><span class="sxs-lookup"><span data-stu-id="67ee7-105">This guide explains setting up a production-ready ASP.NET Core environment on an Ubuntu 16.04 Server.</span></span>

> [!NOTE]
> <span data-ttu-id="67ee7-106">Für Ubuntu 14.04 *Supervisord* wird als Lösung für die Überwachung der Kestrel empfohlen.</span><span class="sxs-lookup"><span data-stu-id="67ee7-106">For Ubuntu 14.04, *supervisord* is recommended as a solution for monitoring the Kestrel process.</span></span> <span data-ttu-id="67ee7-107">*Systemd* für Ubuntu 14.04 nicht verfügbar.</span><span class="sxs-lookup"><span data-stu-id="67ee7-107">*systemd* isn't available on Ubuntu 14.04.</span></span> <span data-ttu-id="67ee7-108">[Siehe die vorherige Version dieses Dokuments](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span><span class="sxs-lookup"><span data-stu-id="67ee7-108">[See previous version of this document](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span></span>

<span data-ttu-id="67ee7-109">In diesem Leitfaden:</span><span class="sxs-lookup"><span data-stu-id="67ee7-109">This guide:</span></span>

* <span data-ttu-id="67ee7-110">Fügt eine vorhandene ASP.NET Core app hinter einem reverse-Proxy-Server vor.</span><span class="sxs-lookup"><span data-stu-id="67ee7-110">Places an existing ASP.NET Core app behind a reverse proxy server.</span></span>
* <span data-ttu-id="67ee7-111">Der reverse-Proxy-Server zum Weiterleiten von Anforderungen an den Webserver Kestrel eingerichtet.</span><span class="sxs-lookup"><span data-stu-id="67ee7-111">Sets up the reverse proxy server to forward requests to the Kestrel web server.</span></span>
* <span data-ttu-id="67ee7-112">Stellt sicher, dass die Web-app beim Start als Daemon ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="67ee7-112">Ensures the web app runs on startup as a daemon.</span></span>
* <span data-ttu-id="67ee7-113">Konfiguriert eine Prozess-Verwaltungstool zum Neustart der Web-app helfen.</span><span class="sxs-lookup"><span data-stu-id="67ee7-113">Configures a process management tool to help restart the web app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="67ee7-114">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="67ee7-114">Prerequisites</span></span>

1. <span data-ttu-id="67ee7-115">Zugriff auf einen Ubuntu 16.04-Server mit einem Standardbenutzerkonto mit sudo-Berechtigung</span><span class="sxs-lookup"><span data-stu-id="67ee7-115">Access to an Ubuntu 16.04 Server with a standard user account with sudo privilege</span></span>
1. <span data-ttu-id="67ee7-116">Eine vorhandene ASP.NET Core-app</span><span class="sxs-lookup"><span data-stu-id="67ee7-116">An existing ASP.NET Core app</span></span>

## <a name="copy-over-the-app"></a><span data-ttu-id="67ee7-117">Kopieren Sie die app</span><span class="sxs-lookup"><span data-stu-id="67ee7-117">Copy over the app</span></span>

<span data-ttu-id="67ee7-118">Führen Sie [Dotnet veröffentlichen](/dotnet/core/tools/dotnet-publish) aus der Umgebung Dev auf eine app in eine geschlossene Verzeichnis zu verpacken, die auf dem Server ausgeführt werden kann.</span><span class="sxs-lookup"><span data-stu-id="67ee7-118">Run [dotnet publish](/dotnet/core/tools/dotnet-publish) from the dev environment to package an app into a self-contained directory that can run on the server.</span></span>

<span data-ttu-id="67ee7-119">Kopieren Sie die ASP.NET Core-app auf den Server, die mit beliebigen Tool in der Organisation Workflow (z. B. SCP, FTP) integriert wird.</span><span class="sxs-lookup"><span data-stu-id="67ee7-119">Copy the ASP.NET Core app to the server using whatever tool integrates into the organization's workflow (for example, SCP, FTP).</span></span> <span data-ttu-id="67ee7-120">Testen Sie die App wie im folgenden Beispiel:</span><span class="sxs-lookup"><span data-stu-id="67ee7-120">Test the app, for example:</span></span>

* <span data-ttu-id="67ee7-121">Führen Sie über die Befehlszeile `dotnet <app_assembly>.dll`.</span><span class="sxs-lookup"><span data-stu-id="67ee7-121">From the command line, run `dotnet <app_assembly>.dll`.</span></span>
* <span data-ttu-id="67ee7-122">Navigieren Sie in einem Browser zu `http://<serveraddress>:<port>` und überprüfen Sie, dass die App unter Linux funktioniert.</span><span class="sxs-lookup"><span data-stu-id="67ee7-122">In a browser, navigate to `http://<serveraddress>:<port>` to verify the app works on Linux.</span></span> 
 
## <a name="configure-a-reverse-proxy-server"></a><span data-ttu-id="67ee7-123">Konfigurieren eines Reverseproxyservers</span><span class="sxs-lookup"><span data-stu-id="67ee7-123">Configure a reverse proxy server</span></span>

<span data-ttu-id="67ee7-124">Reverse-Proxy ist ein gemeinsames Setup dynamic Web-apps zu verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="67ee7-124">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="67ee7-125">Reverseproxy beendet die HTTP-Anforderung und ASP.NET Core-App weiterleitet.</span><span class="sxs-lookup"><span data-stu-id="67ee7-125">A reverse proxy terminates the HTTP request and forwards it to the ASP.NET Core app.</span></span>

### <a name="why-use-a-reverse-proxy-server"></a><span data-ttu-id="67ee7-126">Gründe für das Verwenden eines Reverseproxyservers:</span><span class="sxs-lookup"><span data-stu-id="67ee7-126">Why use a reverse proxy server?</span></span>

<span data-ttu-id="67ee7-127">Kestrel eignet sich hervorragend für dynamische Inhalte von ASP.NET Core bedient.</span><span class="sxs-lookup"><span data-stu-id="67ee7-127">Kestrel is great for serving dynamic content from ASP.NET Core.</span></span> <span data-ttu-id="67ee7-128">Die Web-Funktionen bedient ist jedoch als umfangreichen Features wie z. B. IIS, Apache oder Nginx-Server nicht möglich.</span><span class="sxs-lookup"><span data-stu-id="67ee7-128">However, the web serving capabilities aren't as feature rich as servers such as IIS, Apache, or Nginx.</span></span> <span data-ttu-id="67ee7-129">Ein reverse-Proxy-Server kann die Arbeit wie statische Inhalte Zwischenspeichern von Anforderungen, Komprimieren von Anforderungen und aus dem HTTP-Server SSL-Tunnelabschluss auslagern.</span><span class="sxs-lookup"><span data-stu-id="67ee7-129">A reverse proxy server can offload work such as serving static content, caching requests, compressing requests, and SSL termination from the HTTP server.</span></span> <span data-ttu-id="67ee7-130">Ein Reverseproxyserver kann sich auf einem dedizierten Computer befinden oder zusammen mit einem HTTP-Server bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="67ee7-130">A reverse proxy server may reside on a dedicated machine or may be deployed alongside an HTTP server.</span></span>

<span data-ttu-id="67ee7-131">Für diesen Leitfaden wird eine einzelne Instanz von Nginx verwendet.</span><span class="sxs-lookup"><span data-stu-id="67ee7-131">For the purposes of this guide, a single instance of Nginx is used.</span></span> <span data-ttu-id="67ee7-132">Diese wird auf demselben Server ausgeführt, zusammen mit dem HTTP-Server.</span><span class="sxs-lookup"><span data-stu-id="67ee7-132">It runs on the same server, alongside the HTTP server.</span></span> <span data-ttu-id="67ee7-133">Eine andere Installation kann basierend auf Anforderungen ausgewählt werden.</span><span class="sxs-lookup"><span data-stu-id="67ee7-133">Based on requirements, a different setup may be chosen.</span></span>

<span data-ttu-id="67ee7-134">Da Anforderungen von Reverseproxy weitergeleitet werden, verwenden Sie die Middleware weitergeleitet Header aus der [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) Paket.</span><span class="sxs-lookup"><span data-stu-id="67ee7-134">Because requests are forwarded by reverse proxy, use the Forwarded Headers Middleware from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="67ee7-135">Die Middleware Updates der `Request.Scheme`unter Verwendung der `X-Forwarded-Proto` -Header, damit diese umleitungs-URIs und andere Sicherheitsrichtlinien ordnungsgemäß funktioniert.</span><span class="sxs-lookup"><span data-stu-id="67ee7-135">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="67ee7-136">Wenn Sie einen beliebigen Typ von Authentifizierung-Middleware zu verwenden, muss der Header weitergeleitet Middleware zuerst ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="67ee7-136">When using any type of authentication middleware, the Forwarded Headers Middleware must run first.</span></span> <span data-ttu-id="67ee7-137">Dieser Anordnung wird sichergestellt, dass die authentifizierungsmiddleware die Headerwerte beansprucht. außerdem Generieren der richtigen umleitungs-URIs.</span><span class="sxs-lookup"><span data-stu-id="67ee7-137">This ordering ensures that the authentication middleware can consume the header values and generate correct redirect URIs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="67ee7-138">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="67ee7-138">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="67ee7-139">Aufrufen der [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) Methode im `Startup.Configure` vor dem Aufruf [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) oder ähnliche authentifizierungsmiddleware Schema.</span><span class="sxs-lookup"><span data-stu-id="67ee7-139">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="67ee7-140">Konfigurieren Sie die Middleware zum Weiterleiten der `X-Forwarded-For` und `X-Forwarded-Proto` Header:</span><span class="sxs-lookup"><span data-stu-id="67ee7-140">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="67ee7-141">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="67ee7-141">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="67ee7-142">Aufrufen der [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) Methode im `Startup.Configure` vor dem Aufruf [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) und [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) oder ähnliche Authentifizierungsschema Middleware.</span><span class="sxs-lookup"><span data-stu-id="67ee7-142">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="67ee7-143">Konfigurieren Sie die Middleware zum Weiterleiten der `X-Forwarded-For` und `X-Forwarded-Proto` Header:</span><span class="sxs-lookup"><span data-stu-id="67ee7-143">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

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

<span data-ttu-id="67ee7-144">Wenn kein [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) werden angegeben, um die Middleware, die Standardheader zum Weiterleiten sind `None`.</span><span class="sxs-lookup"><span data-stu-id="67ee7-144">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="67ee7-145">Möglicherweise ist zusätzliche Konfiguration für Apps erforderlich, die hinter Proxyservern und Lastenausgleichsmodulen (Load Balancer) gehostet werden.</span><span class="sxs-lookup"><span data-stu-id="67ee7-145">Additional configuration might be required for apps hosted behind proxy servers and load balancers.</span></span> <span data-ttu-id="67ee7-146">Weitere Informationen hierzu feinden Sie unter [Konfigurieren von ASP.NET Core zur Verwendung mit Proxyservern und Lastenausgleich](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="67ee7-146">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="install-nginx"></a><span data-ttu-id="67ee7-147">Installieren von Nginx</span><span class="sxs-lookup"><span data-stu-id="67ee7-147">Install Nginx</span></span>

```bash
sudo apt-get install nginx
```

> [!NOTE]
> <span data-ttu-id="67ee7-148">Wenn optionale Nginx-Module installiert werden, kann das Erstellen von Nginx aus Quelle erforderlich sein.</span><span class="sxs-lookup"><span data-stu-id="67ee7-148">If optional Nginx modules will be installed, building Nginx from source might be required.</span></span>

<span data-ttu-id="67ee7-149">Verwenden Sie `apt-get` zum Installieren von Nginx.</span><span class="sxs-lookup"><span data-stu-id="67ee7-149">Use `apt-get` to install Nginx.</span></span> <span data-ttu-id="67ee7-150">Das Installationsprogramm erstellt ein System V-Initialisierungsskript, das Nginx beim Systemstart als Daemon ausführt.</span><span class="sxs-lookup"><span data-stu-id="67ee7-150">The installer creates a System V init script that runs Nginx as daemon on system startup.</span></span> <span data-ttu-id="67ee7-151">Da Nginx zum ersten Mal installiert wurde, starten Sie es explizit, indem Sie Folgendes ausführen:</span><span class="sxs-lookup"><span data-stu-id="67ee7-151">Since Nginx was installed for the first time, explicitly start it by running:</span></span>

```bash
sudo service nginx start
```

<span data-ttu-id="67ee7-152">Stellen Sie sicher, dass ein Browser die Standardangebotsseite für Nginx anzeigt.</span><span class="sxs-lookup"><span data-stu-id="67ee7-152">Verify a browser displays the default landing page for Nginx.</span></span>

### <a name="configure-nginx"></a><span data-ttu-id="67ee7-153">Konfigurieren von Nginx</span><span class="sxs-lookup"><span data-stu-id="67ee7-153">Configure Nginx</span></span>

<span data-ttu-id="67ee7-154">Ändern Sie zum Konfigurieren von Nginx als Reverseproxy für forward-Anforderungen an Ihre app ASP.NET Core */etc/nginx/sites-available/default*.</span><span class="sxs-lookup"><span data-stu-id="67ee7-154">To configure Nginx as a reverse proxy to forward requests to your ASP.NET Core app, modify */etc/nginx/sites-available/default*.</span></span> <span data-ttu-id="67ee7-155">Öffnen Sie die Datei in einem Text-Editor und ersetzen Sie den Inhalt durch den Folgendes:</span><span class="sxs-lookup"><span data-stu-id="67ee7-155">Open it in a text editor, and replace the contents with the following:</span></span>

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

<span data-ttu-id="67ee7-156">Wenn kein `server_name` Übereinstimmungen Nginx verwendet den Standardserver an.</span><span class="sxs-lookup"><span data-stu-id="67ee7-156">When no `server_name` matches, Nginx uses the default server.</span></span> <span data-ttu-id="67ee7-157">Wenn kein Server definiert ist, ist der erste Server in der Konfigurationsdatei den Standardserver an.</span><span class="sxs-lookup"><span data-stu-id="67ee7-157">If no default server is defined, the first server in the configuration file is the default server.</span></span> <span data-ttu-id="67ee7-158">Fügen Sie als bewährte Methode einen bestimmte Standardserver für der in der Konfigurationsdatei der 444 Statuscode zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="67ee7-158">As a best practice, add a specific default server which returns a status code of 444 in your configuration file.</span></span> <span data-ttu-id="67ee7-159">Ein Standard Server Configuration-Beispiel ist:</span><span class="sxs-lookup"><span data-stu-id="67ee7-159">A default server configuration example is:</span></span>

```nginx
server {
    listen   80 default_server;
    # listen [::]:80 default_server deferred;
    return   444;
}
```

<span data-ttu-id="67ee7-160">Mit dem vorherigen und Konfigurationsserver akzeptiert Nginx öffentlichen Datenverkehr über Port 80 mit Hostheader `example.com` oder `*.example.com`.</span><span class="sxs-lookup"><span data-stu-id="67ee7-160">With the preceding configuration file and default server, Nginx accepts public traffic on port 80 with host header `example.com` or `*.example.com`.</span></span> <span data-ttu-id="67ee7-161">Anforderungen, die nicht mit dem diese Hosts wird nicht an die Kestrel weitergeleitet abrufen.</span><span class="sxs-lookup"><span data-stu-id="67ee7-161">Requests not matching these hosts won't get forwarded to Kestrel.</span></span> <span data-ttu-id="67ee7-162">Nginx leitet die entsprechenden Anforderungen an Kestrel am `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="67ee7-162">Nginx forwards the matching requests to Kestrel at `http://localhost:5000`.</span></span> <span data-ttu-id="67ee7-163">Finden Sie unter [wie Nginx eine Anforderung verarbeitet](https://nginx.org/docs/http/request_processing.html) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="67ee7-163">See [How nginx processes a request](https://nginx.org/docs/http/request_processing.html) for more information.</span></span>

> [!WARNING]
> <span data-ttu-id="67ee7-164">Fehler beim Geben Sie einer echte [Server_name Richtlinie](https://nginx.org/docs/http/server_names.html) macht Sie Ihre app zu Sicherheitslücken.</span><span class="sxs-lookup"><span data-stu-id="67ee7-164">Failure to specify a proper [server_name directive](https://nginx.org/docs/http/server_names.html) exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="67ee7-165">Unterdomäne Platzhalter Bindung (z. B. `*.example.com`) nicht dieses Sicherheitsrisiko darstellen, wenn Sie steuern, dass die gesamte übergeordnete Domäne (im Gegensatz zu `*.com`, anfällig ist).</span><span class="sxs-lookup"><span data-stu-id="67ee7-165">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="67ee7-166">Weitere Informationen finden Sie unter [rfc7230 im Abschnitt 5.4](https://tools.ietf.org/html/rfc7230#section-5.4).</span><span class="sxs-lookup"><span data-stu-id="67ee7-166">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="67ee7-167">Nachdem die Nginx-Konfiguration eingerichtet ist, führen Sie `sudo nginx -t` überprüfen Sie die Syntax der Konfigurationsdateien.</span><span class="sxs-lookup"><span data-stu-id="67ee7-167">Once the Nginx configuration is established, run `sudo nginx -t` to verify the syntax of the configuration files.</span></span> <span data-ttu-id="67ee7-168">Wenn die Datei Konfigurationstest erfolgreich ist, erzwingen Sie Nginx, durch Ausführen der Änderungen zu übernehmen `sudo nginx -s reload`.</span><span class="sxs-lookup"><span data-stu-id="67ee7-168">If the configuration file test is successful, force Nginx to pick up the changes by running `sudo nginx -s reload`.</span></span>

## <a name="monitoring-the-app"></a><span data-ttu-id="67ee7-169">Überwachen der app</span><span class="sxs-lookup"><span data-stu-id="67ee7-169">Monitoring the app</span></span>

<span data-ttu-id="67ee7-170">Der Server ist zum Weiterleiten von Anforderungen an Setup `http://<serveraddress>:80` sich bei der ASP.NET Core Ausführen einer app im Kestrel am `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="67ee7-170">The server is setup to forward requests made to `http://<serveraddress>:80` on to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="67ee7-171">Nginx allerdings ist nicht eingerichtet Kestrel verwalten.</span><span class="sxs-lookup"><span data-stu-id="67ee7-171">However, Nginx isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="67ee7-172">*Systemd* können verwendet werden, um eine Dienstdatei zum Starten und überwachen die zugrunde liegenden Web-app erstellen.</span><span class="sxs-lookup"><span data-stu-id="67ee7-172">*systemd* can be used to create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="67ee7-173">*SystemD* ist ein Initialisierungssystem, das viele leistungsstarke Features zum Starten, Beenden und Verwalten von Prozessen bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="67ee7-173">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="67ee7-174">Erstellen der Dienstdatei</span><span class="sxs-lookup"><span data-stu-id="67ee7-174">Create the service file</span></span>

<span data-ttu-id="67ee7-175">Erstellen Sie die Dienstdefinitionsdatei:</span><span class="sxs-lookup"><span data-stu-id="67ee7-175">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="67ee7-176">Im folgenden finden eine Dienst-Beispieldatei für die app:</span><span class="sxs-lookup"><span data-stu-id="67ee7-176">The following is an example service file for the app:</span></span>

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

<span data-ttu-id="67ee7-177">**Hinweis:** Wenn der Benutzer *Www-Daten* verwendet, wird nicht durch die Konfiguration der benutzerdefinierten zuerst erstellt und ordnungsgemäße den Besitz für Dateien angegeben werden muss.</span><span class="sxs-lookup"><span data-stu-id="67ee7-177">**Note:** If the user *www-data* isn't used by the configuration, the user defined here must be created first and given proper ownership for files.</span></span>
<span data-ttu-id="67ee7-178">**Hinweis:** Linux verfügt über ein Dateisystem für die Groß-/Kleinschreibung beachtet.</span><span class="sxs-lookup"><span data-stu-id="67ee7-178">**Note:** Linux has a case-sensitive file system.</span></span> <span data-ttu-id="67ee7-179">Festlegen ASPNETCORE_ENVIRONMENT "Produktion" in der Konfigurationsdatei gesucht *"appSettings". Production.JSON*, nicht *appsettings.production.json*.</span><span class="sxs-lookup"><span data-stu-id="67ee7-179">Setting ASPNETCORE_ENVIRONMENT to "Production" results in searching for the configuration file *appsettings.Production.json*, not *appsettings.production.json*.</span></span>

> [!NOTE]
> <span data-ttu-id="67ee7-180">Einige Werte (z. B. SQL-Verbindungszeichenfolgen) müssen für den Konfigurationsanbieter, lesen die Umgebungsvariablen mit Escapezeichen versehen werden.</span><span class="sxs-lookup"><span data-stu-id="67ee7-180">Some values (for example, SQL connection strings) must be escaped for the configuration providers to read the environment variables.</span></span> <span data-ttu-id="67ee7-181">Verwenden Sie den folgenden Befehl aus, um einen ordnungsgemäß mit Escapezeichen versehene Wert für die Verwendung in der Konfigurationsdatei zu generieren:</span><span class="sxs-lookup"><span data-stu-id="67ee7-181">Use the following command to generate a properly escaped value for use in the configuration file:</span></span>
>
> ```console
> systemd-escape "<value-to-escape>"
> ```

<span data-ttu-id="67ee7-182">Speichern Sie die Datei, und aktivieren Sie den Dienst.</span><span class="sxs-lookup"><span data-stu-id="67ee7-182">Save the file and enable the service.</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="67ee7-183">Starten Sie den Dienst, und stellen Sie sicher, dass er ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="67ee7-183">Start the service and verify that it's running.</span></span>

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

<span data-ttu-id="67ee7-184">Der reverse-Proxy konfiguriert und Kestrel über Systemd verwaltet, die Web-app ist vollständig konfiguriert und zugegriffen werden kann, in einem Browser auf dem lokalen Computer am `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="67ee7-184">With the reverse proxy configured and Kestrel managed through systemd, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="67ee7-185">Es ist auch von einem Remotecomputer aus, wobei jeder Firewall, die blockiert werden möglicherweise zugegriffen werden kann.</span><span class="sxs-lookup"><span data-stu-id="67ee7-185">It's also accessible from a remote machine, barring any firewall that might be blocking.</span></span> <span data-ttu-id="67ee7-186">Überprüfen die Antwortheader, die `Server` -Header zeigt die ASP.NET Core-app von Kestrel gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="67ee7-186">Inspecting the response headers, the `Server` header shows the ASP.NET Core app being served by Kestrel.</span></span>

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="67ee7-187">Anzeigen von Protokollen</span><span class="sxs-lookup"><span data-stu-id="67ee7-187">Viewing logs</span></span>

<span data-ttu-id="67ee7-188">Seit der Web-app mit Kestrel wird verwaltet mit `systemd`, alle Ereignisse und Prozesse werden auf einer zentralen Journal protokolliert.</span><span class="sxs-lookup"><span data-stu-id="67ee7-188">Since the web app using Kestrel is managed using `systemd`, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="67ee7-189">Dieses Journal enthält jedoch alle Einträge für alle Dienste und Prozesse, die von `systemd` verwaltet werden.</span><span class="sxs-lookup"><span data-stu-id="67ee7-189">However, this journal includes all entries for all services and processes managed by `systemd`.</span></span> <span data-ttu-id="67ee7-190">Verwenden Sie folgenden Befehl, um die `kestrel-hellomvc.service`-spezifischen Elemente anzuzeigen:</span><span class="sxs-lookup"><span data-stu-id="67ee7-190">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="67ee7-191">Für weiteres Filtern können Zeitoptionen wie `--since today`, `--until 1 hour ago` oder eine Kombination aus diesen die Menge der zurückgegebenen Einträge verringern.</span><span class="sxs-lookup"><span data-stu-id="67ee7-191">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="67ee7-192">Sichern die app</span><span class="sxs-lookup"><span data-stu-id="67ee7-192">Securing the app</span></span>

### <a name="enable-apparmor"></a><span data-ttu-id="67ee7-193">Aktivieren von AppArmor</span><span class="sxs-lookup"><span data-stu-id="67ee7-193">Enable AppArmor</span></span>

<span data-ttu-id="67ee7-194">Linux Security Modules (LSM) ist ein Framework, das Teil der Linux-Kernel seit Linux 2.6 ist.</span><span class="sxs-lookup"><span data-stu-id="67ee7-194">Linux Security Modules (LSM) is a framework that's part of the Linux kernel since Linux 2.6.</span></span> <span data-ttu-id="67ee7-195">LSM unterstützt verschiedene Implementierungen von Sicherheitsmodulen.</span><span class="sxs-lookup"><span data-stu-id="67ee7-195">LSM supports different implementations of security modules.</span></span> <span data-ttu-id="67ee7-196">[AppArmor](https://wiki.ubuntu.com/AppArmor) ist ein LSM, das ein obligatorisches Zugriffssteuerungssystem implementiert, das das Programm auf einen beschränkten Satz von Ressourcen begrenzt.</span><span class="sxs-lookup"><span data-stu-id="67ee7-196">[AppArmor](https://wiki.ubuntu.com/AppArmor) is a LSM that implements a Mandatory Access Control system which allows confining the program to a limited set of resources.</span></span> <span data-ttu-id="67ee7-197">Versichern Sie sich, dass AppArmor aktiviert und ordnungsgemäß konfiguriert ist.</span><span class="sxs-lookup"><span data-stu-id="67ee7-197">Ensure AppArmor is enabled and properly configured.</span></span>

### <a name="configuring-the-firewall"></a><span data-ttu-id="67ee7-198">Konfigurieren der firewall</span><span class="sxs-lookup"><span data-stu-id="67ee7-198">Configuring the firewall</span></span>

<span data-ttu-id="67ee7-199">Schließen Sie alle externen Ports, die nicht verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="67ee7-199">Close off all external ports that are not in use.</span></span> <span data-ttu-id="67ee7-200">„Uncomplicated Firewall“ (UFW) stellt ein Front-End für `iptables` bereit, indem eine Befehlszeilenschnittstelle zum Konfigurieren der Firewall bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="67ee7-200">Uncomplicated firewall (ufw) provides a front end for `iptables` by providing a command line interface for configuring the firewall.</span></span> <span data-ttu-id="67ee7-201">Überprüfen Sie, ob `ufw` zum Zulassen von Datenverkehr auf alle benötigten Ports konfiguriert ist.</span><span class="sxs-lookup"><span data-stu-id="67ee7-201">Verify that `ufw` is configured to allow traffic on any ports needed.</span></span>

```bash
sudo apt-get install ufw
sudo ufw enable

sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### <a name="securing-nginx"></a><span data-ttu-id="67ee7-202">Sichern von Nginx</span><span class="sxs-lookup"><span data-stu-id="67ee7-202">Securing Nginx</span></span>

<span data-ttu-id="67ee7-203">Die Standardverteilung von Nginx aktiviert SSL nicht.</span><span class="sxs-lookup"><span data-stu-id="67ee7-203">The default distribution of Nginx doesn't enable SSL.</span></span> <span data-ttu-id="67ee7-204">Erstellen Sie aus der Quelle, um zusätzliche Sicherheitsfeatures zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="67ee7-204">To enable additional security features, build from source.</span></span>

#### <a name="download-the-source-and-install-the-build-dependencies"></a><span data-ttu-id="67ee7-205">Herunterladen der Quelle und Installieren der Buildabhängigkeiten</span><span class="sxs-lookup"><span data-stu-id="67ee7-205">Download the source and install the build dependencies</span></span>

```bash
# Install the build dependencies
sudo apt-get update
sudo apt-get install build-essential zlib1g-dev libpcre3-dev libssl-dev libxslt1-dev libxml2-dev libgd2-xpm-dev libgeoip-dev libgoogle-perftools-dev libperl-dev

# Download Nginx 1.10.0 or latest
wget http://www.nginx.org/download/nginx-1.10.0.tar.gz
tar zxf nginx-1.10.0.tar.gz
```

#### <a name="change-the-nginx-response-name"></a><span data-ttu-id="67ee7-206">Ändern des Namens der Nginx-Antwort</span><span class="sxs-lookup"><span data-stu-id="67ee7-206">Change the Nginx response name</span></span>

<span data-ttu-id="67ee7-207">Bearbeiten Sie *src/http/ngx_http_header_filter_module.c*:</span><span class="sxs-lookup"><span data-stu-id="67ee7-207">Edit *src/http/ngx_http_header_filter_module.c*:</span></span>

```c
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-the-options-and-build"></a><span data-ttu-id="67ee7-208">Konfigurieren der Optionen und des Builds</span><span class="sxs-lookup"><span data-stu-id="67ee7-208">Configure the options and build</span></span>

<span data-ttu-id="67ee7-209">Die PCRE-Bibliothek wird für reguläre Ausdrücke benötigt.</span><span class="sxs-lookup"><span data-stu-id="67ee7-209">The PCRE library is required for regular expressions.</span></span> <span data-ttu-id="67ee7-210">Reguläre Ausdrücke werden im Verzeichnis des Speicherorts von „gx_http_rewrite_module“ verwendet.</span><span class="sxs-lookup"><span data-stu-id="67ee7-210">Regular expressions are used in the location directive for the ngx_http_rewrite_module.</span></span> <span data-ttu-id="67ee7-211">Durch „http_ssl_module“ wird die Unterstützung für HTTPS-Protokolle hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="67ee7-211">The http_ssl_module adds HTTPS protocol support.</span></span>

<span data-ttu-id="67ee7-212">Erwägen Sie eine Web-app-Firewall wie *ModSecurity* um die app besser zu schützen.</span><span class="sxs-lookup"><span data-stu-id="67ee7-212">Consider using a web app firewall like *ModSecurity* to harden the app.</span></span>

```bash
./configure
--with-pcre=../pcre-8.38
--with-zlib=../zlib-1.2.8
--with-http_ssl_module
--with-stream
--with-mail=dynamic
```

#### <a name="configure-ssl"></a><span data-ttu-id="67ee7-213">Konfigurieren von SSL</span><span class="sxs-lookup"><span data-stu-id="67ee7-213">Configure SSL</span></span>

* <span data-ttu-id="67ee7-214">Konfigurieren Sie den Server, die HTTPS-Datenverkehr über Port Lauschen `443` durch Angeben eines gültigen Zertifikats von einer vertrauenswürdigen Zertifizierungsstelle (CA) ausgegeben.</span><span class="sxs-lookup"><span data-stu-id="67ee7-214">Configure the server to listen to HTTPS traffic on port `443` by specifying a valid certificate issued by a trusted Certificate Authority (CA).</span></span>

* <span data-ttu-id="67ee7-215">Härten die Sicherheit durch die Verwendung einige Methoden, die in den in der folgenden */etc/nginx/nginx.conf* Datei.</span><span class="sxs-lookup"><span data-stu-id="67ee7-215">Harden the security by employing some of the practices depicted in the following */etc/nginx/nginx.conf* file.</span></span> <span data-ttu-id="67ee7-216">Die Beispiele schließen das Auswählen einer stärkeren Verschlüsselung und das Weiterleiten allen Datenverkehrs über HTTP auf HTTPS ein.</span><span class="sxs-lookup"><span data-stu-id="67ee7-216">Examples include choosing a stronger cipher and redirecting all traffic over HTTP to HTTPS.</span></span>

* <span data-ttu-id="67ee7-217">Durch das Hinzufügen eines `HTTP Strict-Transport-Security`-Headers (HSTS) wird sichergestellt, dass alle nachfolgenden Anforderungen vom Client nur über HTTPS erfolgen.</span><span class="sxs-lookup"><span data-stu-id="67ee7-217">Adding an `HTTP Strict-Transport-Security` (HSTS) header ensures all subsequent requests made by the client are over HTTPS only.</span></span>

* <span data-ttu-id="67ee7-218">Fügen Sie nicht den Strict-Transport-Security-Header hinzu, oder wählen Sie eine entsprechende `max-age` Wenn SSL wird in der Zukunft deaktiviert werden.</span><span class="sxs-lookup"><span data-stu-id="67ee7-218">Don't add the Strict-Transport-Security header or chose an appropriate `max-age` if SSL will be disabled in the future.</span></span>

<span data-ttu-id="67ee7-219">Fügen Sie die Konfigurationsdatei */etc/nginx/proxy.conf* hinzu:</span><span class="sxs-lookup"><span data-stu-id="67ee7-219">Add the */etc/nginx/proxy.conf* configuration file:</span></span>

[!code-nginx[](linux-nginx/proxy.conf)]

<span data-ttu-id="67ee7-220">Bearbeiten Sie die Konfigurationsdatei */etc/nginx/nginx.conf*.</span><span class="sxs-lookup"><span data-stu-id="67ee7-220">Edit the */etc/nginx/nginx.conf* configuration file.</span></span> <span data-ttu-id="67ee7-221">Das Beispiel enthält die Abschnitte `http` und `server` in einer Konfigurationsdatei.</span><span class="sxs-lookup"><span data-stu-id="67ee7-221">The example contains both `http` and `server` sections in one configuration file.</span></span>

[!code-nginx[](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a><span data-ttu-id="67ee7-222">Sichern von Nginx vor Clickjacking</span><span class="sxs-lookup"><span data-stu-id="67ee7-222">Secure Nginx from clickjacking</span></span>
<span data-ttu-id="67ee7-223">Clickjacking ist eine böswillige Technik zum Sammeln der Klicks eines infizierten Benutzers.</span><span class="sxs-lookup"><span data-stu-id="67ee7-223">Clickjacking is a malicious technique to collect an infected user's clicks.</span></span> <span data-ttu-id="67ee7-224">Clickjacking bringt das Opfer (Besucher) dazu, auf eine infizierte Seite zu klicken.</span><span class="sxs-lookup"><span data-stu-id="67ee7-224">Clickjacking tricks the victim (visitor) into clicking on an infected site.</span></span> <span data-ttu-id="67ee7-225">Verwendet X-FRAME-OPTIONS zum Sichern der Site.</span><span class="sxs-lookup"><span data-stu-id="67ee7-225">Use X-FRAME-OPTIONS to secure the site.</span></span>

<span data-ttu-id="67ee7-226">Bearbeiten Sie die Datei *nginx.conf*:</span><span class="sxs-lookup"><span data-stu-id="67ee7-226">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="67ee7-227">Fügen Sie die Zeile `add_header X-Frame-Options "SAMEORIGIN";` hinzu, und speichern Sie die Datei. Starten Sie Nginx dann neu.</span><span class="sxs-lookup"><span data-stu-id="67ee7-227">Add the line `add_header X-Frame-Options "SAMEORIGIN";` and save the file, then restart Nginx.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="67ee7-228">MIME-Typermittlung</span><span class="sxs-lookup"><span data-stu-id="67ee7-228">MIME-type sniffing</span></span>

<span data-ttu-id="67ee7-229">Dieser Header hindert die meisten Browser an der MIME-Ermittlung einer Antwort vom deklarierten Inhaltstyp weg, da der Header den Browser anweist, den Inhaltstyp der Antwort nicht zu überschreiben.</span><span class="sxs-lookup"><span data-stu-id="67ee7-229">This header prevents most browsers from MIME-sniffing a response away from the declared content type, as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="67ee7-230">Durch die Option `nosniff` wird der Inhalt vom Browser als „text/html“ gerendert, wenn der Server sagt, dass es sich bei diesem um „text/html“ handelt.</span><span class="sxs-lookup"><span data-stu-id="67ee7-230">With the `nosniff` option, if the server says the content is "text/html", the browser renders it as "text/html".</span></span>

<span data-ttu-id="67ee7-231">Bearbeiten Sie die Datei *nginx.conf*:</span><span class="sxs-lookup"><span data-stu-id="67ee7-231">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="67ee7-232">Fügen Sie die Zeile `add_header X-Content-Type-Options "nosniff";` hinzu, und speichern Sie die Datei. Starten Sie Nginx dann neu.</span><span class="sxs-lookup"><span data-stu-id="67ee7-232">Add the line `add_header X-Content-Type-Options "nosniff";` and save the file, then restart Nginx.</span></span>
