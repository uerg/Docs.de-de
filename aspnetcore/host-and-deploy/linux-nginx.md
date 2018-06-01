---
title: Hosten von ASP.NET Core unter Linux mit Nginx
author: rick-anderson
description: Beschreibt das Einrichten von Nginx als Reverseproxy für Ubuntu 16.04, um den HTTP-Datenverkehr auf eine ASP.NET Core-Web-App weiterzuleiten, die auf Kestrel ausgeführt wird.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/linux-nginx
ms.openlocfilehash: d37aa25c712d715aa4134587a84e5923f9cb5b79
ms.sourcegitcommit: 50d40c83fa641d283c097f986dde5341ebe1b44c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/22/2018
ms.locfileid: "34452554"
---
# <a name="host-aspnet-core-on-linux-with-nginx"></a><span data-ttu-id="a6850-103">Hosten von ASP.NET Core unter Linux mit Nginx</span><span class="sxs-lookup"><span data-stu-id="a6850-103">Host ASP.NET Core on Linux with Nginx</span></span>

<span data-ttu-id="a6850-104">Von [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="a6850-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="a6850-105">Dieser Leitfaden erläutert das Einrichten einer produktionsbereiten ASP.NET Core-Umgebungen auf einem Ubuntu 16.04-Server.</span><span class="sxs-lookup"><span data-stu-id="a6850-105">This guide explains setting up a production-ready ASP.NET Core environment on an Ubuntu 16.04 Server.</span></span>

> [!NOTE]
> <span data-ttu-id="a6850-106">Für Ubuntu 14.04 wird *supervisord* für die Überwachung des Kestrel-Prozesses empfohlen.</span><span class="sxs-lookup"><span data-stu-id="a6850-106">For Ubuntu 14.04, *supervisord* is recommended as a solution for monitoring the Kestrel process.</span></span> <span data-ttu-id="a6850-107">*systemd* ist unter Ubuntu 14.04 nicht verfügbar.</span><span class="sxs-lookup"><span data-stu-id="a6850-107">*systemd* isn't available on Ubuntu 14.04.</span></span> <span data-ttu-id="a6850-108">[Hier finden Sie die vorherige Version dieses Dokuments.](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md)</span><span class="sxs-lookup"><span data-stu-id="a6850-108">[See previous version of this document](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span></span>

<span data-ttu-id="a6850-109">In diesem Leitfaden:</span><span class="sxs-lookup"><span data-stu-id="a6850-109">This guide:</span></span>

* <span data-ttu-id="a6850-110">Wird eine bestehende ASP.NET Core-App hinter einem Reverseproxyserver eingefügt.</span><span class="sxs-lookup"><span data-stu-id="a6850-110">Places an existing ASP.NET Core app behind a reverse proxy server.</span></span>
* <span data-ttu-id="a6850-111">Wird der Reverseproxyserver eingerichtet, um Anforderungen an den Kestrel-Webserver weiterzuleiten.</span><span class="sxs-lookup"><span data-stu-id="a6850-111">Sets up the reverse proxy server to forward requests to the Kestrel web server.</span></span>
* <span data-ttu-id="a6850-112">Wird sichergestellt, dass die Web-App beim Start als Daemon ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="a6850-112">Ensures the web app runs on startup as a daemon.</span></span>
* <span data-ttu-id="a6850-113">Wird ein Prozessverwaltungstool konfiguriert, das die Web-App beim Neustarten unterstützt.</span><span class="sxs-lookup"><span data-stu-id="a6850-113">Configures a process management tool to help restart the web app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a6850-114">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="a6850-114">Prerequisites</span></span>

1. <span data-ttu-id="a6850-115">Zugriff auf einen Ubuntu 16.04-Server mit einem Standardbenutzerkonto mit sudo-Berechtigung</span><span class="sxs-lookup"><span data-stu-id="a6850-115">Access to an Ubuntu 16.04 Server with a standard user account with sudo privilege</span></span>
1. <span data-ttu-id="a6850-116">Eine bestehende ASP.NET Core-App</span><span class="sxs-lookup"><span data-stu-id="a6850-116">An existing ASP.NET Core app</span></span>

## <a name="copy-over-the-app"></a><span data-ttu-id="a6850-117">Kopieren der App</span><span class="sxs-lookup"><span data-stu-id="a6850-117">Copy over the app</span></span>

<span data-ttu-id="a6850-118">Führen Sie [dotnet publish](/dotnet/core/tools/dotnet-publish) in der Bereitstellungsumgebung aus, um eine App in ein eigenständiges Verzeichnis zu packen, das auf dem Server ausgeführt werden kann.</span><span class="sxs-lookup"><span data-stu-id="a6850-118">Run [dotnet publish](/dotnet/core/tools/dotnet-publish) from the dev environment to package an app into a self-contained directory that can run on the server.</span></span>

<span data-ttu-id="a6850-119">Kopieren Sie die ASP.NET Core-App auf den Server, indem Sie ein beliebiges Tool verwenden, das in den Workflow der Organisation integriert ist (z.B. SCP oder FTP).</span><span class="sxs-lookup"><span data-stu-id="a6850-119">Copy the ASP.NET Core app to the server using whatever tool integrates into the organization's workflow (for example, SCP, FTP).</span></span> <span data-ttu-id="a6850-120">Testen Sie die App wie im folgenden Beispiel:</span><span class="sxs-lookup"><span data-stu-id="a6850-120">Test the app, for example:</span></span>

* <span data-ttu-id="a6850-121">Führen Sie `dotnet <app_assembly>.dll` über die Befehlszeile aus.</span><span class="sxs-lookup"><span data-stu-id="a6850-121">From the command line, run `dotnet <app_assembly>.dll`.</span></span>
* <span data-ttu-id="a6850-122">Navigieren Sie in einem Browser zu `http://<serveraddress>:<port>` und überprüfen Sie, dass die App unter Linux funktioniert.</span><span class="sxs-lookup"><span data-stu-id="a6850-122">In a browser, navigate to `http://<serveraddress>:<port>` to verify the app works on Linux.</span></span> 
 
## <a name="configure-a-reverse-proxy-server"></a><span data-ttu-id="a6850-123">Konfigurieren eines Reverseproxyservers</span><span class="sxs-lookup"><span data-stu-id="a6850-123">Configure a reverse proxy server</span></span>

<span data-ttu-id="a6850-124">Ein Reverseproxy wird im Allgemeinen zur Unterstützung dynamischer Web-Apps eingerichtet.</span><span class="sxs-lookup"><span data-stu-id="a6850-124">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="a6850-125">Ein Reverseproxy beendet die HTTP-Anforderung und leitet diese an die ASP.NET Core-App weiter.</span><span class="sxs-lookup"><span data-stu-id="a6850-125">A reverse proxy terminates the HTTP request and forwards it to the ASP.NET Core app.</span></span>

### <a name="why-use-a-reverse-proxy-server"></a><span data-ttu-id="a6850-126">Gründe für das Verwenden eines Reverseproxyservers:</span><span class="sxs-lookup"><span data-stu-id="a6850-126">Why use a reverse proxy server?</span></span>

<span data-ttu-id="a6850-127">Kestrel eignet sich hervorragend für die Bereitstellung dynamischer Inhalte aus ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a6850-127">Kestrel is great for serving dynamic content from ASP.NET Core.</span></span> <span data-ttu-id="a6850-128">Die Webbereitstellungsfunktionen sind jedoch nicht so umfangreich wie bei Servern wie IIS, Apache oder Nginx.</span><span class="sxs-lookup"><span data-stu-id="a6850-128">However, the web serving capabilities aren't as feature rich as servers such as IIS, Apache, or Nginx.</span></span> <span data-ttu-id="a6850-129">Ein Reverseproxyserver kann Arbeiten wie das Verarbeiten von statischen Inhalten, das Zwischenspeichern und Komprimieren von Anforderungen und das Beenden von SSL vom HTTP-Server auslagern.</span><span class="sxs-lookup"><span data-stu-id="a6850-129">A reverse proxy server can offload work such as serving static content, caching requests, compressing requests, and SSL termination from the HTTP server.</span></span> <span data-ttu-id="a6850-130">Ein Reverseproxyserver kann sich auf einem dedizierten Computer befinden oder zusammen mit einem HTTP-Server bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="a6850-130">A reverse proxy server may reside on a dedicated machine or may be deployed alongside an HTTP server.</span></span>

<span data-ttu-id="a6850-131">Für diesen Leitfaden wird eine einzelne Instanz von Nginx verwendet.</span><span class="sxs-lookup"><span data-stu-id="a6850-131">For the purposes of this guide, a single instance of Nginx is used.</span></span> <span data-ttu-id="a6850-132">Diese wird auf demselben Server ausgeführt, zusammen mit dem HTTP-Server.</span><span class="sxs-lookup"><span data-stu-id="a6850-132">It runs on the same server, alongside the HTTP server.</span></span> <span data-ttu-id="a6850-133">Je nach Anforderungen kann ein anderes Setup ausgewählt werden.</span><span class="sxs-lookup"><span data-stu-id="a6850-133">Based on requirements, a different setup may be chosen.</span></span>

<span data-ttu-id="a6850-134">Da Anforderungen vom Reverseproxy weitergeleitet werden, sollten Sie die Middleware für weitergeleitete Header aus dem Paket [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) verwenden.</span><span class="sxs-lookup"><span data-stu-id="a6850-134">Because requests are forwarded by reverse proxy, use the Forwarded Headers Middleware from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="a6850-135">Diese Middleware aktualisiert `Request.Scheme` mithilfe des `X-Forwarded-Proto`-Headers, sodass Umleitungs-URIs und andere Sicherheitsrichtlinien ordnungsgemäß funktionieren.</span><span class="sxs-lookup"><span data-stu-id="a6850-135">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="a6850-136">Wenn Sie einen beliebigen Typ von Authentifizierungsmiddleware verwenden, muss die Middleware für weitergeleitete Header zuerst ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="a6850-136">When using any type of authentication middleware, the Forwarded Headers Middleware must run first.</span></span> <span data-ttu-id="a6850-137">Durch diese Reihenfolge wird sichergestellt, dass die Authentifizierungsmiddleware die Headerwerte nutzen und richtige Umleitungs-URIs generieren kann.</span><span class="sxs-lookup"><span data-stu-id="a6850-137">This ordering ensures that the authentication middleware can consume the header values and generate correct redirect URIs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a6850-138">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a6850-138">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="a6850-139">Rufen Sie die Methode [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) in `Startup.Configure` auf, bevor Sie [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) oder eine ähnliche Middleware für Authentifizierungsschemas aufrufen.</span><span class="sxs-lookup"><span data-stu-id="a6850-139">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="a6850-140">Konfigurieren Sie die Middleware so, dass die Header `X-Forwarded-For` und `X-Forwarded-Proto` weitergeleitet werden:</span><span class="sxs-lookup"><span data-stu-id="a6850-140">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a6850-141">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a6850-141">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="a6850-142">Rufen Sie die Methode [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) in `Startup.Configure` auf, bevor Sie [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) und [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) oder eine ähnliche Middleware für Authentifizierungsschemas aufrufen.</span><span class="sxs-lookup"><span data-stu-id="a6850-142">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="a6850-143">Konfigurieren Sie die Middleware so, dass die Header `X-Forwarded-For` und `X-Forwarded-Proto` weitergeleitet werden:</span><span class="sxs-lookup"><span data-stu-id="a6850-143">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

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

<span data-ttu-id="a6850-144">Wenn für die Middleware keine [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) angegeben sind, lauten die weiterzuleitenden Standardheader `None`.</span><span class="sxs-lookup"><span data-stu-id="a6850-144">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="a6850-145">Möglicherweise ist zusätzliche Konfiguration für Apps erforderlich, die hinter Proxyservern und Lastenausgleichsmodulen (Load Balancer) gehostet werden.</span><span class="sxs-lookup"><span data-stu-id="a6850-145">Additional configuration might be required for apps hosted behind proxy servers and load balancers.</span></span> <span data-ttu-id="a6850-146">Weitere Informationen hierzu feinden Sie unter [Konfigurieren von ASP.NET Core zur Verwendung mit Proxyservern und Lastenausgleich](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="a6850-146">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="install-nginx"></a><span data-ttu-id="a6850-147">Installieren von Nginx</span><span class="sxs-lookup"><span data-stu-id="a6850-147">Install Nginx</span></span>

```bash
sudo apt-get install nginx
```

> [!NOTE]
> <span data-ttu-id="a6850-148">Wenn optionale Nginx-Module installiert werden, kann die Erstellung von Nginx aus der Quelle erforderlich sein.</span><span class="sxs-lookup"><span data-stu-id="a6850-148">If optional Nginx modules will be installed, building Nginx from source might be required.</span></span>

<span data-ttu-id="a6850-149">Verwenden Sie `apt-get` zum Installieren von Nginx.</span><span class="sxs-lookup"><span data-stu-id="a6850-149">Use `apt-get` to install Nginx.</span></span> <span data-ttu-id="a6850-150">Das Installationsprogramm erstellt ein System V-Initialisierungsskript, das Nginx beim Systemstart als Daemon ausführt.</span><span class="sxs-lookup"><span data-stu-id="a6850-150">The installer creates a System V init script that runs Nginx as daemon on system startup.</span></span> <span data-ttu-id="a6850-151">Da Nginx zum ersten Mal installiert wurde, starten Sie es explizit, indem Sie Folgendes ausführen:</span><span class="sxs-lookup"><span data-stu-id="a6850-151">Since Nginx was installed for the first time, explicitly start it by running:</span></span>

```bash
sudo service nginx start
```

<span data-ttu-id="a6850-152">Stellen Sie sicher, dass ein Browser die Standardangebotsseite für Nginx anzeigt.</span><span class="sxs-lookup"><span data-stu-id="a6850-152">Verify a browser displays the default landing page for Nginx.</span></span>

### <a name="configure-nginx"></a><span data-ttu-id="a6850-153">Konfigurieren von Nginx</span><span class="sxs-lookup"><span data-stu-id="a6850-153">Configure Nginx</span></span>

<span data-ttu-id="a6850-154">Ändern Sie */etc/nginx/sites-available/default*, um Nginx als Reverseproxy für die Weiterleitung von Anforderungen zu Ihrer ASP.NET Core-App zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="a6850-154">To configure Nginx as a reverse proxy to forward requests to your ASP.NET Core app, modify */etc/nginx/sites-available/default*.</span></span> <span data-ttu-id="a6850-155">Öffnen Sie die Datei in einem Text-Editor und ersetzen Sie den Inhalt durch den Folgendes:</span><span class="sxs-lookup"><span data-stu-id="a6850-155">Open it in a text editor, and replace the contents with the following:</span></span>

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

<span data-ttu-id="a6850-156">Wenn keine Übereinstimmung mit `server_name` gefunden wird, verwendet Nginx den Standardserver.</span><span class="sxs-lookup"><span data-stu-id="a6850-156">When no `server_name` matches, Nginx uses the default server.</span></span> <span data-ttu-id="a6850-157">Wenn kein Server definiert ist, ist der erste Server in der Konfigurationsdatei der Standardserver.</span><span class="sxs-lookup"><span data-stu-id="a6850-157">If no default server is defined, the first server in the configuration file is the default server.</span></span> <span data-ttu-id="a6850-158">Als bewährte Methode gilt, einen bestimmten Standardserver hinzuzufügen, der den Statuscode 444 in Ihrer Konfigurationsdatei zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="a6850-158">As a best practice, add a specific default server which returns a status code of 444 in your configuration file.</span></span> <span data-ttu-id="a6850-159">Im Folgenden wird ein Beispiel für eine Standardserverkonfiguration aufgeführt:</span><span class="sxs-lookup"><span data-stu-id="a6850-159">A default server configuration example is:</span></span>

```nginx
server {
    listen   80 default_server;
    # listen [::]:80 default_server deferred;
    return   444;
}
```

<span data-ttu-id="a6850-160">Mit der vorhergehenden Konfigurationsdatei und dem vorhergehenden Standardserver akzeptiert Nginx den öffentlichen Datenverkehr über Port 80 mit dem Hostheader `example.com` oder `*.example.com`.</span><span class="sxs-lookup"><span data-stu-id="a6850-160">With the preceding configuration file and default server, Nginx accepts public traffic on port 80 with host header `example.com` or `*.example.com`.</span></span> <span data-ttu-id="a6850-161">Anforderungen, die mit diesen Hosts nicht übereinstimmen, werden nicht an Kestrel weitergeleitet.</span><span class="sxs-lookup"><span data-stu-id="a6850-161">Requests not matching these hosts won't get forwarded to Kestrel.</span></span> <span data-ttu-id="a6850-162">Nginx leitet die übereinstimmenden Anforderungen unter `http://localhost:5000` an Kestrel weiter.</span><span class="sxs-lookup"><span data-stu-id="a6850-162">Nginx forwards the matching requests to Kestrel at `http://localhost:5000`.</span></span> <span data-ttu-id="a6850-163">Weitere Informationen finden Sie unter [Verarbeitung einer Anforderung mit Nginx](https://nginx.org/docs/http/request_processing.html).</span><span class="sxs-lookup"><span data-stu-id="a6850-163">See [How nginx processes a request](https://nginx.org/docs/http/request_processing.html) for more information.</span></span>

> [!WARNING]
> <span data-ttu-id="a6850-164">Schlägt die Angabe einer ordnungsgemäßen [server_name-Anweisung](https://nginx.org/docs/http/server_names.html) fehlt, ist Ihre App Sicherheitsrisiken ausgesetzt.</span><span class="sxs-lookup"><span data-stu-id="a6850-164">Failure to specify a proper [server_name directive](https://nginx.org/docs/http/server_names.html) exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="a6850-165">Platzhalterbindungen in untergeordneten Domänen (z.B. `*.example.com`) verursachen kein Sicherheitsrisiko, wenn Sie die gesamte übergeordnete Domäne steuern (im Gegensatz zu `*.com`, das angreifbar ist).</span><span class="sxs-lookup"><span data-stu-id="a6850-165">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="a6850-166">Weitere Informationen finden Sie unter [rfc7230 im Abschnitt 5.4](https://tools.ietf.org/html/rfc7230#section-5.4).</span><span class="sxs-lookup"><span data-stu-id="a6850-166">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="a6850-167">Wenn die Nginx-Konfiguration eingerichtet ist, können Sie zur Überprüfung der Syntax der Konfigurationsdateien `sudo nginx -t` ausführen.</span><span class="sxs-lookup"><span data-stu-id="a6850-167">Once the Nginx configuration is established, run `sudo nginx -t` to verify the syntax of the configuration files.</span></span> <span data-ttu-id="a6850-168">Wenn der Test der Konfigurationsdatei erfolgreich ist, können Sie durch Ausführen von `sudo nginx -s reload` erzwingen, dass Nginx die Änderungen übernimmt.</span><span class="sxs-lookup"><span data-stu-id="a6850-168">If the configuration file test is successful, force Nginx to pick up the changes by running `sudo nginx -s reload`.</span></span>

## <a name="monitoring-the-app"></a><span data-ttu-id="a6850-169">Überwachen der App</span><span class="sxs-lookup"><span data-stu-id="a6850-169">Monitoring the app</span></span>

<span data-ttu-id="a6850-170">Der Server ist dafür eingerichtet, Anforderungen an `http://<serveraddress>:80` an die ASP.NET Core-App weiterzuleiten, die unter `http://127.0.0.1:5000` unter Kestrel ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="a6850-170">The server is setup to forward requests made to `http://<serveraddress>:80` on to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="a6850-171">Nginx wurde jedoch nicht dafür eingerichtet, den Kestrel-Prozess zu verwalten.</span><span class="sxs-lookup"><span data-stu-id="a6850-171">However, Nginx isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="a6850-172">*systemd* kann für die Erstellung einer Dienstdatei verwendet werden, um die zugrunde liegende Web-App zu starten und zu überwachen.</span><span class="sxs-lookup"><span data-stu-id="a6850-172">*systemd* can be used to create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="a6850-173">*SystemD* ist ein Initialisierungssystem, das viele leistungsstarke Features zum Starten, Beenden und Verwalten von Prozessen bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="a6850-173">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="a6850-174">Erstellen der Dienstdatei</span><span class="sxs-lookup"><span data-stu-id="a6850-174">Create the service file</span></span>

<span data-ttu-id="a6850-175">Erstellen Sie die Dienstdefinitionsdatei:</span><span class="sxs-lookup"><span data-stu-id="a6850-175">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="a6850-176">Im Folgenden wird ein Beispiel für eine Dienstdatei der App aufgeführt:</span><span class="sxs-lookup"><span data-stu-id="a6850-176">The following is an example service file for the app:</span></span>

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

<span data-ttu-id="a6850-177">Wenn der Benutzer *www-data* durch Ihre Konfiguration nicht verwendet wird, muss der hier definierte Benutzer zunächst erstellt werden, und ihm muss der ordnungsgemäße Besitz für die Dateien erteilt werden.</span><span class="sxs-lookup"><span data-stu-id="a6850-177">If the user *www-data* isn't used by the configuration, the user defined here must be created first and given proper ownership for files.</span></span>

<span data-ttu-id="a6850-178">Linux verfügt über ein Dateisystem, bei dem die Groß-/Kleinschreibung beachtet wird.</span><span class="sxs-lookup"><span data-stu-id="a6850-178">Linux has a case-sensitive file system.</span></span> <span data-ttu-id="a6850-179">Das Festlegen von ASPNETCORE_ENVIRONMENT auf „Production“ führt dazu, dass nach der Konfigurationsdatei *appsettings.Production.json* und nicht nach *appsettings.production.json* gesucht wird.</span><span class="sxs-lookup"><span data-stu-id="a6850-179">Setting ASPNETCORE_ENVIRONMENT to "Production" results in searching for the configuration file *appsettings.Production.json*, not *appsettings.production.json*.</span></span>

> [!NOTE]
> <span data-ttu-id="a6850-180">Einige Werte (z.B. SQL-Verbindungszeichenfolgen) müssen mit Escapezeichen versehen werden, damit die Konfigurationsanbieter die Umgebungsvariablen lesen können.</span><span class="sxs-lookup"><span data-stu-id="a6850-180">Some values (for example, SQL connection strings) must be escaped for the configuration providers to read the environment variables.</span></span> <span data-ttu-id="a6850-181">Mit dem folgenden Befehl können Sie einen ordnungsgemäß mit Escapezeichen versehenen Wert für die Verwendung in der Konfigurationsdatei generieren:</span><span class="sxs-lookup"><span data-stu-id="a6850-181">Use the following command to generate a properly escaped value for use in the configuration file:</span></span>
>
> ```console
> systemd-escape "<value-to-escape>"
> ```

<span data-ttu-id="a6850-182">Speichern Sie die Datei, und aktivieren Sie den Dienst.</span><span class="sxs-lookup"><span data-stu-id="a6850-182">Save the file and enable the service.</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="a6850-183">Starten Sie den Dienst, und überprüfen Sie, ob er ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="a6850-183">Start the service and verify that it's running.</span></span>

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

<span data-ttu-id="a6850-184">Wenn der Reverseproxy konfiguriert ist und Kestrel durch „systemd“ verwaltet wird, ist die Webanwendung vollständig konfiguriert. Auf diese kann dann über einen Browser auf dem lokalen Computer unter `http://localhost` zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="a6850-184">With the reverse proxy configured and Kestrel managed through systemd, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="a6850-185">Der Zugriff ist auch von einem Remotecomputer aus möglich, sofern keine Firewall diesen blockiert.</span><span class="sxs-lookup"><span data-stu-id="a6850-185">It's also accessible from a remote machine, barring any firewall that might be blocking.</span></span> <span data-ttu-id="a6850-186">Beim Überprüfen der Antwortheader zeigt der Header `Server` die ASP.NET Core-App an, die von Kestrel verarbeitet wird.</span><span class="sxs-lookup"><span data-stu-id="a6850-186">Inspecting the response headers, the `Server` header shows the ASP.NET Core app being served by Kestrel.</span></span>

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="a6850-187">Anzeigen von Protokollen</span><span class="sxs-lookup"><span data-stu-id="a6850-187">Viewing logs</span></span>

<span data-ttu-id="a6850-188">Da die Web-App, die Kestrel verwendet, von `systemd` verwaltet wird, werden alle Ereignisse und Prozesse in einem zentralen Journal protokolliert.</span><span class="sxs-lookup"><span data-stu-id="a6850-188">Since the web app using Kestrel is managed using `systemd`, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="a6850-189">Dieses Journal enthält jedoch alle Einträge für alle Dienste und Prozesse, die von `systemd` verwaltet werden.</span><span class="sxs-lookup"><span data-stu-id="a6850-189">However, this journal includes all entries for all services and processes managed by `systemd`.</span></span> <span data-ttu-id="a6850-190">Verwenden Sie folgenden Befehl, um die `kestrel-hellomvc.service`-spezifischen Elemente anzuzeigen:</span><span class="sxs-lookup"><span data-stu-id="a6850-190">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="a6850-191">Für weiteres Filtern können Zeitoptionen wie `--since today`, `--until 1 hour ago` oder eine Kombination aus diesen die Menge der zurückgegebenen Einträge verringern.</span><span class="sxs-lookup"><span data-stu-id="a6850-191">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="a6850-192">Sichern der App</span><span class="sxs-lookup"><span data-stu-id="a6850-192">Securing the app</span></span>

### <a name="enable-apparmor"></a><span data-ttu-id="a6850-193">Aktivieren von AppArmor</span><span class="sxs-lookup"><span data-stu-id="a6850-193">Enable AppArmor</span></span>

<span data-ttu-id="a6850-194">Bei Linux Security Modules (LSM) handelt es sich um ein Framework, das seit Linux 2.6 Teil des Linux-Kernels ist.</span><span class="sxs-lookup"><span data-stu-id="a6850-194">Linux Security Modules (LSM) is a framework that's part of the Linux kernel since Linux 2.6.</span></span> <span data-ttu-id="a6850-195">LSM unterstützt verschiedene Implementierungen von Sicherheitsmodulen.</span><span class="sxs-lookup"><span data-stu-id="a6850-195">LSM supports different implementations of security modules.</span></span> <span data-ttu-id="a6850-196">[AppArmor](https://wiki.ubuntu.com/AppArmor) ist ein LSM, das ein obligatorisches Zugriffssteuerungssystem implementiert, das das Programm auf einen beschränkten Satz von Ressourcen begrenzt.</span><span class="sxs-lookup"><span data-stu-id="a6850-196">[AppArmor](https://wiki.ubuntu.com/AppArmor) is a LSM that implements a Mandatory Access Control system which allows confining the program to a limited set of resources.</span></span> <span data-ttu-id="a6850-197">Versichern Sie sich, dass AppArmor aktiviert und ordnungsgemäß konfiguriert ist.</span><span class="sxs-lookup"><span data-stu-id="a6850-197">Ensure AppArmor is enabled and properly configured.</span></span>

### <a name="configuring-the-firewall"></a><span data-ttu-id="a6850-198">Konfigurieren der Firewall</span><span class="sxs-lookup"><span data-stu-id="a6850-198">Configuring the firewall</span></span>

<span data-ttu-id="a6850-199">Schließen Sie alle externen Ports, die nicht verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="a6850-199">Close off all external ports that are not in use.</span></span> <span data-ttu-id="a6850-200">„Uncomplicated Firewall“ (UFW) stellt ein Front-End für `iptables` bereit, indem eine Befehlszeilenschnittstelle zum Konfigurieren der Firewall bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="a6850-200">Uncomplicated firewall (ufw) provides a front end for `iptables` by providing a command line interface for configuring the firewall.</span></span> <span data-ttu-id="a6850-201">Überprüfen Sie, ob `ufw` konfiguriert ist, um Datenverkehr für alle erforderlichen Ports zuzulassen.</span><span class="sxs-lookup"><span data-stu-id="a6850-201">Verify that `ufw` is configured to allow traffic on any ports needed.</span></span>

```bash
sudo apt-get install ufw
sudo ufw enable

sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### <a name="securing-nginx"></a><span data-ttu-id="a6850-202">Sichern von Nginx</span><span class="sxs-lookup"><span data-stu-id="a6850-202">Securing Nginx</span></span>

<span data-ttu-id="a6850-203">Die Standardverteilung von Nginx aktiviert SSL nicht.</span><span class="sxs-lookup"><span data-stu-id="a6850-203">The default distribution of Nginx doesn't enable SSL.</span></span> <span data-ttu-id="a6850-204">Erstellen Sie aus der Quelle, um zusätzliche Sicherheitsfeatures zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="a6850-204">To enable additional security features, build from source.</span></span>

#### <a name="download-the-source-and-install-the-build-dependencies"></a><span data-ttu-id="a6850-205">Herunterladen der Quelle und Installieren der Buildabhängigkeiten</span><span class="sxs-lookup"><span data-stu-id="a6850-205">Download the source and install the build dependencies</span></span>

```bash
# Install the build dependencies
sudo apt-get update
sudo apt-get install build-essential zlib1g-dev libpcre3-dev libssl-dev libxslt1-dev libxml2-dev libgd2-xpm-dev libgeoip-dev libgoogle-perftools-dev libperl-dev

# Download Nginx 1.10.0 or latest
wget http://www.nginx.org/download/nginx-1.10.0.tar.gz
tar zxf nginx-1.10.0.tar.gz
```

#### <a name="change-the-nginx-response-name"></a><span data-ttu-id="a6850-206">Ändern des Namens der Nginx-Antwort</span><span class="sxs-lookup"><span data-stu-id="a6850-206">Change the Nginx response name</span></span>

<span data-ttu-id="a6850-207">Bearbeiten Sie *src/http/ngx_http_header_filter_module.c*:</span><span class="sxs-lookup"><span data-stu-id="a6850-207">Edit *src/http/ngx_http_header_filter_module.c*:</span></span>

```c
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-the-options-and-build"></a><span data-ttu-id="a6850-208">Konfigurieren der Optionen und des Builds</span><span class="sxs-lookup"><span data-stu-id="a6850-208">Configure the options and build</span></span>

<span data-ttu-id="a6850-209">Die PCRE-Bibliothek wird für reguläre Ausdrücke benötigt.</span><span class="sxs-lookup"><span data-stu-id="a6850-209">The PCRE library is required for regular expressions.</span></span> <span data-ttu-id="a6850-210">Reguläre Ausdrücke werden im Verzeichnis des Speicherorts von „gx_http_rewrite_module“ verwendet.</span><span class="sxs-lookup"><span data-stu-id="a6850-210">Regular expressions are used in the location directive for the ngx_http_rewrite_module.</span></span> <span data-ttu-id="a6850-211">Durch „http_ssl_module“ wird die Unterstützung für HTTPS-Protokolle hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="a6850-211">The http_ssl_module adds HTTPS protocol support.</span></span>

<span data-ttu-id="a6850-212">Erwägen Sie zum Schutz der App die Verwendung einer Web-App-Firewall wie *ModSecurity*.</span><span class="sxs-lookup"><span data-stu-id="a6850-212">Consider using a web app firewall like *ModSecurity* to harden the app.</span></span>

```bash
./configure
--with-pcre=../pcre-8.38
--with-zlib=../zlib-1.2.8
--with-http_ssl_module
--with-stream
--with-mail=dynamic
```

#### <a name="configure-ssl"></a><span data-ttu-id="a6850-213">Konfigurieren von SSL</span><span class="sxs-lookup"><span data-stu-id="a6850-213">Configure SSL</span></span>

* <span data-ttu-id="a6850-214">Konfigurieren Sie den Server, damit dieser für den HTTPS-Datenverkehr an Port `443` empfangsbereit ist, indem Sie ein gültiges Zertifikat angeben, das von einer vertrauenswürdigen Zertifizierungsstelle (Certificate Authority, CA) ausgestellt wurde.</span><span class="sxs-lookup"><span data-stu-id="a6850-214">Configure the server to listen to HTTPS traffic on port `443` by specifying a valid certificate issued by a trusted Certificate Authority (CA).</span></span>

* <span data-ttu-id="a6850-215">Stärken Sie Ihre Sicherheit, indem Sie einige der in der folgenden Datei (*/etc/nginx/nginx.conf*) dargestellten Methoden verwenden.</span><span class="sxs-lookup"><span data-stu-id="a6850-215">Harden the security by employing some of the practices depicted in the following */etc/nginx/nginx.conf* file.</span></span> <span data-ttu-id="a6850-216">Die Beispiele schließen das Auswählen einer stärkeren Verschlüsselung und das Weiterleiten allen Datenverkehrs über HTTP auf HTTPS ein.</span><span class="sxs-lookup"><span data-stu-id="a6850-216">Examples include choosing a stronger cipher and redirecting all traffic over HTTP to HTTPS.</span></span>

* <span data-ttu-id="a6850-217">Durch das Hinzufügen eines `HTTP Strict-Transport-Security`-Headers (HSTS) wird sichergestellt, dass alle nachfolgenden Anforderungen vom Client nur über HTTPS erfolgen.</span><span class="sxs-lookup"><span data-stu-id="a6850-217">Adding an `HTTP Strict-Transport-Security` (HSTS) header ensures all subsequent requests made by the client are over HTTPS only.</span></span>

* <span data-ttu-id="a6850-218">Fügen Sie keinen Strict-Transport-Security-Header hinzu, oder wählen Sie ein entsprechendes `max-age` aus, wenn Sie SSL in Zukunft deaktivieren möchten.</span><span class="sxs-lookup"><span data-stu-id="a6850-218">Don't add the Strict-Transport-Security header or chose an appropriate `max-age` if SSL will be disabled in the future.</span></span>

<span data-ttu-id="a6850-219">Fügen Sie die Konfigurationsdatei */etc/nginx/proxy.conf* hinzu:</span><span class="sxs-lookup"><span data-stu-id="a6850-219">Add the */etc/nginx/proxy.conf* configuration file:</span></span>

[!code-nginx[](linux-nginx/proxy.conf)]

<span data-ttu-id="a6850-220">Bearbeiten Sie die Konfigurationsdatei */etc/nginx/nginx.conf*.</span><span class="sxs-lookup"><span data-stu-id="a6850-220">Edit the */etc/nginx/nginx.conf* configuration file.</span></span> <span data-ttu-id="a6850-221">Das Beispiel enthält die Abschnitte `http` und `server` in einer Konfigurationsdatei.</span><span class="sxs-lookup"><span data-stu-id="a6850-221">The example contains both `http` and `server` sections in one configuration file.</span></span>

[!code-nginx[](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a><span data-ttu-id="a6850-222">Sichern von Nginx vor Clickjacking</span><span class="sxs-lookup"><span data-stu-id="a6850-222">Secure Nginx from clickjacking</span></span>
<span data-ttu-id="a6850-223">Clickjacking ist eine böswillige Technik zum Sammeln der Klicks eines infizierten Benutzers.</span><span class="sxs-lookup"><span data-stu-id="a6850-223">Clickjacking is a malicious technique to collect an infected user's clicks.</span></span> <span data-ttu-id="a6850-224">Clickjacking bringt das Opfer (Besucher) dazu, auf eine infizierte Seite zu klicken.</span><span class="sxs-lookup"><span data-stu-id="a6850-224">Clickjacking tricks the victim (visitor) into clicking on an infected site.</span></span> <span data-ttu-id="a6850-225">Verwenden Sie X-FRAME-OPTIONS zum Sichern der Website.</span><span class="sxs-lookup"><span data-stu-id="a6850-225">Use X-FRAME-OPTIONS to secure the site.</span></span>

<span data-ttu-id="a6850-226">Bearbeiten Sie die Datei *nginx.conf*:</span><span class="sxs-lookup"><span data-stu-id="a6850-226">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="a6850-227">Fügen Sie die Zeile `add_header X-Frame-Options "SAMEORIGIN";` hinzu, und speichern Sie die Datei. Starten Sie Nginx dann neu.</span><span class="sxs-lookup"><span data-stu-id="a6850-227">Add the line `add_header X-Frame-Options "SAMEORIGIN";` and save the file, then restart Nginx.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="a6850-228">MIME-Typermittlung</span><span class="sxs-lookup"><span data-stu-id="a6850-228">MIME-type sniffing</span></span>

<span data-ttu-id="a6850-229">Dieser Header hindert die meisten Browser an der MIME-Ermittlung einer Antwort vom deklarierten Inhaltstyp weg, da der Header den Browser anweist, den Inhaltstyp der Antwort nicht zu überschreiben.</span><span class="sxs-lookup"><span data-stu-id="a6850-229">This header prevents most browsers from MIME-sniffing a response away from the declared content type, as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="a6850-230">Durch die Option `nosniff` wird der Inhalt vom Browser als „text/html“ gerendert, wenn der Server sagt, dass es sich bei diesem um „text/html“ handelt.</span><span class="sxs-lookup"><span data-stu-id="a6850-230">With the `nosniff` option, if the server says the content is "text/html", the browser renders it as "text/html".</span></span>

<span data-ttu-id="a6850-231">Bearbeiten Sie die Datei *nginx.conf*:</span><span class="sxs-lookup"><span data-stu-id="a6850-231">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="a6850-232">Fügen Sie die Zeile `add_header X-Content-Type-Options "nosniff";` hinzu, und speichern Sie die Datei. Starten Sie Nginx dann neu.</span><span class="sxs-lookup"><span data-stu-id="a6850-232">Add the line `add_header X-Content-Type-Options "nosniff";` and save the file, then restart Nginx.</span></span>
