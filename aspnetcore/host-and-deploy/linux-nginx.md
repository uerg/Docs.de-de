---
title: Hosten von ASP.NET Core unter Linux mit Nginx
author: rick-anderson
description: Hier finden Sie Informationen zum Einrichten von Nginx als Reverseproxy unter Ubuntu 16.04, um den HTTP-Datenverkehr an eine ASP.NET Core-Web-App weiterzuleiten, die auf Kestrel ausgeführt wird.
ms.author: riande
ms.custom: mvc
ms.date: 05/22/2018
uid: host-and-deploy/linux-nginx
ms.openlocfilehash: d94640075f6fe5db06672f7dc641470c71076a16
ms.sourcegitcommit: 08bf41d4b3e696ab512b044970e8304816f8cc56
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2018
ms.locfileid: "44040012"
---
# <a name="host-aspnet-core-on-linux-with-nginx"></a><span data-ttu-id="d270e-103">Hosten von ASP.NET Core unter Linux mit Nginx</span><span class="sxs-lookup"><span data-stu-id="d270e-103">Host ASP.NET Core on Linux with Nginx</span></span>

<span data-ttu-id="d270e-104">Von [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="d270e-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="d270e-105">In diesem Leitfaden wird das Einrichten einer produktionsbereiten ASP.NET Core-Umgebung auf einem Ubuntu 16.04-Server erläutert.</span><span class="sxs-lookup"><span data-stu-id="d270e-105">This guide explains setting up a production-ready ASP.NET Core environment on an Ubuntu 16.04 server.</span></span> <span data-ttu-id="d270e-106">Diese Anweisungen sind wahrscheinlich auf neuere Versionen von Ubuntu anwendbar, sie wurden jedoch noch nicht mit neueren Versionen getestet.</span><span class="sxs-lookup"><span data-stu-id="d270e-106">These instructions likely work with newer versions of Ubuntu, but the instructions haven't been tested with newer versions.</span></span>

<span data-ttu-id="d270e-107">Weitere Informationen zu anderen Linux-Distributionen, die von ASP.NET Core unterstützt werden, finden Sie unter [Voraussetzungen für .NET Core unter Linux](/dotnet/core/linux-prerequisites).</span><span class="sxs-lookup"><span data-stu-id="d270e-107">For information on other Linux distributions supported by ASP.NET Core, see [Prerequisites for .NET Core on Linux](/dotnet/core/linux-prerequisites).</span></span>

> [!NOTE]
> <span data-ttu-id="d270e-108">Für Ubuntu 14.04 wird *supervisord* für die Überwachung des Kestrel-Prozesses empfohlen.</span><span class="sxs-lookup"><span data-stu-id="d270e-108">For Ubuntu 14.04, *supervisord* is recommended as a solution for monitoring the Kestrel process.</span></span> <span data-ttu-id="d270e-109">*systemd* ist unter Ubuntu 14.04 nicht verfügbar.</span><span class="sxs-lookup"><span data-stu-id="d270e-109">*systemd* isn't available on Ubuntu 14.04.</span></span> <span data-ttu-id="d270e-110">Anweisungen zu Ubuntu 14.04 finden Sie in der [vorherigen Version dieses Themas](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span><span class="sxs-lookup"><span data-stu-id="d270e-110">For Ubuntu 14.04 instructions, see the [previous version of this topic](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span></span>

<span data-ttu-id="d270e-111">In diesem Leitfaden:</span><span class="sxs-lookup"><span data-stu-id="d270e-111">This guide:</span></span>

* <span data-ttu-id="d270e-112">Wird eine bestehende ASP.NET Core-App hinter einem Reverseproxyserver eingefügt.</span><span class="sxs-lookup"><span data-stu-id="d270e-112">Places an existing ASP.NET Core app behind a reverse proxy server.</span></span>
* <span data-ttu-id="d270e-113">Wird der Reverseproxyserver eingerichtet, um Anforderungen an den Kestrel-Webserver weiterzuleiten.</span><span class="sxs-lookup"><span data-stu-id="d270e-113">Sets up the reverse proxy server to forward requests to the Kestrel web server.</span></span>
* <span data-ttu-id="d270e-114">Wird sichergestellt, dass die Web-App beim Start als Daemon ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="d270e-114">Ensures the web app runs on startup as a daemon.</span></span>
* <span data-ttu-id="d270e-115">Wird ein Prozessverwaltungstool konfiguriert, das die Web-App beim Neustarten unterstützt.</span><span class="sxs-lookup"><span data-stu-id="d270e-115">Configures a process management tool to help restart the web app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d270e-116">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="d270e-116">Prerequisites</span></span>

1. <span data-ttu-id="d270e-117">Greifen Sie auf einen Ubuntu 16.04-Server mit einem Standardbenutzerkonto mit sudo-Berechtigung zu.</span><span class="sxs-lookup"><span data-stu-id="d270e-117">Access to an Ubuntu 16.04 server with a standard user account with sudo privilege.</span></span>
1. <span data-ttu-id="d270e-118">Installieren Sie die .NET Core-Runtime auf dem Server.</span><span class="sxs-lookup"><span data-stu-id="d270e-118">Install the .NET Core runtime on the server.</span></span>
   1. <span data-ttu-id="d270e-119">Navigieren Sie zu der [.NET-Seite „All Downloads“ (Alle Downloads)](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="d270e-119">Visit the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>
   1. <span data-ttu-id="d270e-120">Wählen Sie unter **Runtime** die aktuelle Nicht-Vorschau-Runtime aus der Liste aus.</span><span class="sxs-lookup"><span data-stu-id="d270e-120">Select the latest non-preview runtime from the list under **Runtime**.</span></span>
   1. <span data-ttu-id="d270e-121">Wählen Sie die Anweisungen für Ubuntu aus, die der Ubuntu-Version des Servers entsprechen, und befolgen Sie diese.</span><span class="sxs-lookup"><span data-stu-id="d270e-121">Select and follow the instructions for Ubuntu that match the Ubuntu version of the server.</span></span>
1. <span data-ttu-id="d270e-122">Eine vorhandene ASP.NET Core-App.</span><span class="sxs-lookup"><span data-stu-id="d270e-122">An existing ASP.NET Core app.</span></span>

## <a name="publish-and-copy-over-the-app"></a><span data-ttu-id="d270e-123">Veröffentlichen und Kopieren der App</span><span class="sxs-lookup"><span data-stu-id="d270e-123">Publish and copy over the app</span></span>

<span data-ttu-id="d270e-124">Konfigurieren Sie die App für eine [Framework-abhängige Bereitstellung](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span><span class="sxs-lookup"><span data-stu-id="d270e-124">Configure the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span></span>

<span data-ttu-id="d270e-125">Führen Sie [dotnet publish](/dotnet/core/tools/dotnet-publish) in der Entwicklungsumgebung aus, um eine App in ein Verzeichnis zu packen (z.B. *bin/Release/&lt;target_framework_moniker&gt;/publish*), das auf dem Server ausgeführt werden kann:</span><span class="sxs-lookup"><span data-stu-id="d270e-125">Run [dotnet publish](/dotnet/core/tools/dotnet-publish) from the development environment to package an app into a directory (for example, *bin/Release/&lt;target_framework_moniker&gt;/publish*) that can run on the server:</span></span>

```console
dotnet publish --configuration Release
```

<span data-ttu-id="d270e-126">Die App kann auch als eine [eigenständige Bereitstellung](/dotnet/core/deploying/#self-contained-deployments-scd) veröffentlicht werden, wenn Sie die .NET Core-Runtime nicht auf dem Server verwalten möchten.</span><span class="sxs-lookup"><span data-stu-id="d270e-126">The app can also be published as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) if you prefer not to maintain the .NET Core runtime on the server.</span></span>

<span data-ttu-id="d270e-127">Kopieren Sie die ASP.NET Core-App auf den Server, indem Sie ein beliebiges Tool verwenden, das in den Workflow der Organisation integriert ist (z.B. SCP oder SFTP).</span><span class="sxs-lookup"><span data-stu-id="d270e-127">Copy the ASP.NET Core app to the server using a tool that integrates into the organization's workflow (for example, SCP, SFTP).</span></span> <span data-ttu-id="d270e-128">Web-Apps befinden sich üblicherweise im Verzeichnis *var* (z.B. *var/aspnetcore/hellomvc*).</span><span class="sxs-lookup"><span data-stu-id="d270e-128">It's common to locate web apps under the *var* directory (for example, *var/aspnetcore/hellomvc*).</span></span>

> [!NOTE]
> <span data-ttu-id="d270e-129">In einem Szenario für die Bereitstellung in der Produktion übernimmt ein Continuous Integration-Workflow die Veröffentlichung der App und das Kopieren der Objekte auf den Server.</span><span class="sxs-lookup"><span data-stu-id="d270e-129">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span>

<span data-ttu-id="d270e-130">Testen der App:</span><span class="sxs-lookup"><span data-stu-id="d270e-130">Test the app:</span></span>

1. <span data-ttu-id="d270e-131">Führen Sie die App über die Befehlszeile aus: `dotnet <app_assembly>.dll`.</span><span class="sxs-lookup"><span data-stu-id="d270e-131">From the command line, run the app: `dotnet <app_assembly>.dll`.</span></span>
1. <span data-ttu-id="d270e-132">Navigieren Sie in einem Browser zu `http://<serveraddress>:<port>`, und überprüfen Sie, ob die App lokal unter Linux funktioniert.</span><span class="sxs-lookup"><span data-stu-id="d270e-132">In a browser, navigate to `http://<serveraddress>:<port>` to verify the app works on Linux locally.</span></span>

## <a name="configure-a-reverse-proxy-server"></a><span data-ttu-id="d270e-133">Konfigurieren eines Reverseproxyservers</span><span class="sxs-lookup"><span data-stu-id="d270e-133">Configure a reverse proxy server</span></span>

<span data-ttu-id="d270e-134">Ein Reverseproxy wird im Allgemeinen zur Unterstützung dynamischer Web-Apps eingerichtet.</span><span class="sxs-lookup"><span data-stu-id="d270e-134">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="d270e-135">Ein Reverseproxy beendet die HTTP-Anforderung und leitet diese an die ASP.NET Core-App weiter.</span><span class="sxs-lookup"><span data-stu-id="d270e-135">A reverse proxy terminates the HTTP request and forwards it to the ASP.NET Core app.</span></span>

::: moniker range=">= aspnetcore-2.0"

> [!NOTE]
> <span data-ttu-id="d270e-136">Jede der beiden Konfigurationen &mdash;mit oder ohne einen Reverseproxyserver&mdash; ist eine gültige und unterstützte Hostingkonfiguration für ASP.NET Core 2.0 oder neuere Apps.</span><span class="sxs-lookup"><span data-stu-id="d270e-136">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span> <span data-ttu-id="d270e-137">Weitere Informationen finden Sie unter [When to use Kestrel with a reverse proxy (Verwenden von Kestrel mit einem Reverseproxy)](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="d270e-137">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

::: moniker-end

### <a name="use-a-reverse-proxy-server"></a><span data-ttu-id="d270e-138">Verwenden eines Reverseproxyservers</span><span class="sxs-lookup"><span data-stu-id="d270e-138">Use a reverse proxy server</span></span>

<span data-ttu-id="d270e-139">Kestrel eignet sich hervorragend für die Bereitstellung dynamischer Inhalte aus ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d270e-139">Kestrel is great for serving dynamic content from ASP.NET Core.</span></span> <span data-ttu-id="d270e-140">Die Webbereitstellungsfunktionen sind jedoch nicht so umfangreich wie bei Servern wie IIS, Apache oder Nginx.</span><span class="sxs-lookup"><span data-stu-id="d270e-140">However, the web serving capabilities aren't as feature rich as servers such as IIS, Apache, or Nginx.</span></span> <span data-ttu-id="d270e-141">Ein Reverseproxyserver kann Arbeiten wie das Verarbeiten von statischen Inhalten, das Zwischenspeichern und Komprimieren von Anforderungen und das Beenden von SSL vom HTTP-Server auslagern.</span><span class="sxs-lookup"><span data-stu-id="d270e-141">A reverse proxy server can offload work such as serving static content, caching requests, compressing requests, and SSL termination from the HTTP server.</span></span> <span data-ttu-id="d270e-142">Ein Reverseproxyserver kann sich auf einem dedizierten Computer befinden oder zusammen mit einem HTTP-Server bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="d270e-142">A reverse proxy server may reside on a dedicated machine or may be deployed alongside an HTTP server.</span></span>

<span data-ttu-id="d270e-143">Für diesen Leitfaden wird eine einzelne Instanz von Nginx verwendet.</span><span class="sxs-lookup"><span data-stu-id="d270e-143">For the purposes of this guide, a single instance of Nginx is used.</span></span> <span data-ttu-id="d270e-144">Diese wird auf demselben Server ausgeführt, zusammen mit dem HTTP-Server.</span><span class="sxs-lookup"><span data-stu-id="d270e-144">It runs on the same server, alongside the HTTP server.</span></span> <span data-ttu-id="d270e-145">Je nach Anforderungen kann ein anderes Setup ausgewählt werden.</span><span class="sxs-lookup"><span data-stu-id="d270e-145">Based on requirements, a different setup may be chosen.</span></span>

<span data-ttu-id="d270e-146">Da Anforderungen vom Reverseproxy weitergeleitet werden, sollten Sie die [Middleware für weitergeleitete Header](xref:host-and-deploy/proxy-load-balancer) aus dem Paket [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) verwenden.</span><span class="sxs-lookup"><span data-stu-id="d270e-146">Because requests are forwarded by reverse proxy, use the [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="d270e-147">Diese Middleware aktualisiert `Request.Scheme` mithilfe des `X-Forwarded-Proto`-Headers, sodass Umleitungs-URIs und andere Sicherheitsrichtlinien ordnungsgemäß funktionieren.</span><span class="sxs-lookup"><span data-stu-id="d270e-147">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="d270e-148">Jede vom Schema abhängige Komponente, wie etwa Authentifizierung, Linkgenerierung, Umleitungen und Geolocation, muss nach dem Aufrufen der Middleware für weitergeleitete Header eingefügt werden.</span><span class="sxs-lookup"><span data-stu-id="d270e-148">Any component that depends on the scheme, such as authentication, link generation, redirects, and geolocation, must be placed after invoking the Forwarded Headers Middleware.</span></span> <span data-ttu-id="d270e-149">Allgemein gilt, dass Middleware für weitergeleitete Header vor jeder anderen Middleware ausgeführt werden sollte, mit Ausnahme von Middleware für die Diagnose und Fehlerbehandlung.</span><span class="sxs-lookup"><span data-stu-id="d270e-149">As a general rule, Forwarded Headers Middleware should run before other middleware except diagnostics and error handling middleware.</span></span> <span data-ttu-id="d270e-150">Mit dieser Reihenfolge wird sichergestellt, dass die auf Informationen von weitergeleiteten Headern basierende Middleware die zu verarbeitenden Headerwerte nutzen kann.</span><span class="sxs-lookup"><span data-stu-id="d270e-150">This ordering ensures that the middleware relying on forwarded headers information can consume the header values for processing.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d270e-151">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d270e-151">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="d270e-152">Rufen Sie die Methode [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) in `Startup.Configure` auf, bevor Sie [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) oder eine ähnliche Middleware für Authentifizierungsschemas aufrufen.</span><span class="sxs-lookup"><span data-stu-id="d270e-152">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="d270e-153">Konfigurieren Sie die Middleware so, dass die Header `X-Forwarded-For` und `X-Forwarded-Proto` weitergeleitet werden:</span><span class="sxs-lookup"><span data-stu-id="d270e-153">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d270e-154">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d270e-154">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d270e-155">Rufen Sie die Methode [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) in `Startup.Configure` auf, bevor Sie [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) und [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) oder eine ähnliche Middleware für Authentifizierungsschemas aufrufen.</span><span class="sxs-lookup"><span data-stu-id="d270e-155">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="d270e-156">Konfigurieren Sie die Middleware so, dass die Header `X-Forwarded-For` und `X-Forwarded-Proto` weitergeleitet werden:</span><span class="sxs-lookup"><span data-stu-id="d270e-156">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

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

<span data-ttu-id="d270e-157">Wenn für die Middleware keine [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) angegeben sind, lauten die weiterzuleitenden Standardheader `None`.</span><span class="sxs-lookup"><span data-stu-id="d270e-157">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="d270e-158">Möglicherweise ist zusätzliche Konfiguration für Apps erforderlich, die hinter Proxyservern und Lastenausgleichsmodulen (Load Balancer) gehostet werden.</span><span class="sxs-lookup"><span data-stu-id="d270e-158">Additional configuration might be required for apps hosted behind proxy servers and load balancers.</span></span> <span data-ttu-id="d270e-159">Weitere Informationen hierzu feinden Sie unter [Konfigurieren von ASP.NET Core zur Verwendung mit Proxyservern und Lastenausgleich](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="d270e-159">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="install-nginx"></a><span data-ttu-id="d270e-160">Installieren von Nginx</span><span class="sxs-lookup"><span data-stu-id="d270e-160">Install Nginx</span></span>

<span data-ttu-id="d270e-161">Verwenden Sie `apt-get` zum Installieren von Nginx.</span><span class="sxs-lookup"><span data-stu-id="d270e-161">Use `apt-get` to install Nginx.</span></span> <span data-ttu-id="d270e-162">Das Installationsprogramm erstellt ein *systemd*-Initialisierungsskript, das Nginx beim Systemstart als Dämon ausführt.</span><span class="sxs-lookup"><span data-stu-id="d270e-162">The installer creates a *systemd* init script that runs Nginx as daemon on system startup.</span></span> 

```bash
sudo -s
nginx=stable # use nginx=development for latest development version
add-apt-repository ppa:nginx/$nginx
apt-get update
apt-get install nginx
```

<span data-ttu-id="d270e-163">Das Ubuntu Personal Package Archive (PPA) wird von Freiwilligen verwaltet und nicht durch [nginx.org](https://nginx.org/) verteilt. Weitere Informationen finden Sie unter [Nginx: Binary Releases: Official Debian/Ubuntu packages (Nginx: binäre Releases – offizielle Debian/Ubuntu-Pakete)](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages).</span><span class="sxs-lookup"><span data-stu-id="d270e-163">The Ubuntu Personal Package Archive (PPA) is maintained by volunteers and isn't distributed by [nginx.org](https://nginx.org/). For more information, see [Nginx: Binary Releases: Official Debian/Ubuntu packages](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages).</span></span>

> [!NOTE]
> <span data-ttu-id="d270e-164">Wenn optionale Nginx-Module benötigt werden, kann die Erstellung von Nginx aus der Quelle erforderlich sein.</span><span class="sxs-lookup"><span data-stu-id="d270e-164">If optional Nginx modules are required, building Nginx from source might be required.</span></span>

<span data-ttu-id="d270e-165">Da Nginx zum ersten Mal installiert wurde, starten Sie es explizit, indem Sie Folgendes ausführen:</span><span class="sxs-lookup"><span data-stu-id="d270e-165">Since Nginx was installed for the first time, explicitly start it by running:</span></span>

```bash
sudo service nginx start
```

<span data-ttu-id="d270e-166">Stellen Sie sicher, dass ein Browser die Standardangebotsseite für Nginx anzeigt.</span><span class="sxs-lookup"><span data-stu-id="d270e-166">Verify a browser displays the default landing page for Nginx.</span></span> <span data-ttu-id="d270e-167">Die Landing Page ist unter `http://<server_IP_address>/index.nginx-debian.html` erreichbar.</span><span class="sxs-lookup"><span data-stu-id="d270e-167">The landing page is reachable at `http://<server_IP_address>/index.nginx-debian.html`.</span></span>

### <a name="configure-nginx"></a><span data-ttu-id="d270e-168">Konfigurieren von Nginx</span><span class="sxs-lookup"><span data-stu-id="d270e-168">Configure Nginx</span></span>

<span data-ttu-id="d270e-169">Ändern Sie */etc/nginx/sites-available/default*, um Nginx als Reverseproxy für die Weiterleitung von Anforderungen zu Ihrer ASP.NET Core-App zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="d270e-169">To configure Nginx as a reverse proxy to forward requests to your ASP.NET Core app, modify */etc/nginx/sites-available/default*.</span></span> <span data-ttu-id="d270e-170">Öffnen Sie die Datei in einem Text-Editor und ersetzen Sie den Inhalt durch den Folgendes:</span><span class="sxs-lookup"><span data-stu-id="d270e-170">Open it in a text editor, and replace the contents with the following:</span></span>

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

<span data-ttu-id="d270e-171">Wenn keine Übereinstimmung mit `server_name` gefunden wird, verwendet Nginx den Standardserver.</span><span class="sxs-lookup"><span data-stu-id="d270e-171">When no `server_name` matches, Nginx uses the default server.</span></span> <span data-ttu-id="d270e-172">Wenn kein Server definiert ist, ist der erste Server in der Konfigurationsdatei der Standardserver.</span><span class="sxs-lookup"><span data-stu-id="d270e-172">If no default server is defined, the first server in the configuration file is the default server.</span></span> <span data-ttu-id="d270e-173">Als bewährte Methode gilt, einen bestimmten Standardserver hinzuzufügen, der den Statuscode 444 in Ihrer Konfigurationsdatei zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="d270e-173">As a best practice, add a specific default server which returns a status code of 444 in your configuration file.</span></span> <span data-ttu-id="d270e-174">Im Folgenden wird ein Beispiel für eine Standardserverkonfiguration aufgeführt:</span><span class="sxs-lookup"><span data-stu-id="d270e-174">A default server configuration example is:</span></span>

```nginx
server {
    listen   80 default_server;
    # listen [::]:80 default_server deferred;
    return   444;
}
```

<span data-ttu-id="d270e-175">Mit der vorhergehenden Konfigurationsdatei und dem vorhergehenden Standardserver akzeptiert Nginx den öffentlichen Datenverkehr über Port 80 mit dem Hostheader `example.com` oder `*.example.com`.</span><span class="sxs-lookup"><span data-stu-id="d270e-175">With the preceding configuration file and default server, Nginx accepts public traffic on port 80 with host header `example.com` or `*.example.com`.</span></span> <span data-ttu-id="d270e-176">Anforderungen, die mit diesen Hosts nicht übereinstimmen, werden nicht an Kestrel weitergeleitet.</span><span class="sxs-lookup"><span data-stu-id="d270e-176">Requests not matching these hosts won't get forwarded to Kestrel.</span></span> <span data-ttu-id="d270e-177">Nginx leitet die übereinstimmenden Anforderungen unter `http://localhost:5000` an Kestrel weiter.</span><span class="sxs-lookup"><span data-stu-id="d270e-177">Nginx forwards the matching requests to Kestrel at `http://localhost:5000`.</span></span> <span data-ttu-id="d270e-178">Weitere Informationen finden Sie unter [Verarbeitung einer Anforderung mit Nginx](https://nginx.org/docs/http/request_processing.html).</span><span class="sxs-lookup"><span data-stu-id="d270e-178">See [How nginx processes a request](https://nginx.org/docs/http/request_processing.html) for more information.</span></span> <span data-ttu-id="d270e-179">Informationen zum Ändern der IP-Adresse bzw. des Ports von Kestrel finden Sie unter [Kestrel: Endpoint configuration (Kestrel: Endpunktkonfiguration)](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="d270e-179">To change Kestrel's IP/port, see [Kestrel: Endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

> [!WARNING]
> <span data-ttu-id="d270e-180">Schlägt die Angabe einer ordnungsgemäßen [server_name-Anweisung](https://nginx.org/docs/http/server_names.html) fehlt, ist Ihre App Sicherheitsrisiken ausgesetzt.</span><span class="sxs-lookup"><span data-stu-id="d270e-180">Failure to specify a proper [server_name directive](https://nginx.org/docs/http/server_names.html) exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="d270e-181">Platzhalterbindungen in untergeordneten Domänen (z.B. `*.example.com`) verursachen kein Sicherheitsrisiko, wenn Sie die gesamte übergeordnete Domäne steuern (im Gegensatz zu `*.com`, das angreifbar ist).</span><span class="sxs-lookup"><span data-stu-id="d270e-181">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="d270e-182">Weitere Informationen finden Sie unter [rfc7230 im Abschnitt 5.4](https://tools.ietf.org/html/rfc7230#section-5.4).</span><span class="sxs-lookup"><span data-stu-id="d270e-182">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="d270e-183">Wenn die Nginx-Konfiguration eingerichtet ist, können Sie zur Überprüfung der Syntax der Konfigurationsdateien `sudo nginx -t` ausführen.</span><span class="sxs-lookup"><span data-stu-id="d270e-183">Once the Nginx configuration is established, run `sudo nginx -t` to verify the syntax of the configuration files.</span></span> <span data-ttu-id="d270e-184">Wenn der Test der Konfigurationsdatei erfolgreich ist, können Sie durch Ausführen von `sudo nginx -s reload` erzwingen, dass Nginx die Änderungen übernimmt.</span><span class="sxs-lookup"><span data-stu-id="d270e-184">If the configuration file test is successful, force Nginx to pick up the changes by running `sudo nginx -s reload`.</span></span>

<span data-ttu-id="d270e-185">Gehen Sie wie folgt vor, um die App auf dem Server direkt auszuführen:</span><span class="sxs-lookup"><span data-stu-id="d270e-185">To directly run the app on the server:</span></span>

1. <span data-ttu-id="d270e-186">Navigieren Sie zum Verzeichnis der App.</span><span class="sxs-lookup"><span data-stu-id="d270e-186">Navigate to the app's directory.</span></span>
1. <span data-ttu-id="d270e-187">Führen Sie die ausführbare Datei der App aus: `./<app_executable>`.</span><span class="sxs-lookup"><span data-stu-id="d270e-187">Run the app's executable: `./<app_executable>`.</span></span>

<span data-ttu-id="d270e-188">Sollte ein Berechtigungsfehler auftreten, ändern Sie die Berechtigungen:</span><span class="sxs-lookup"><span data-stu-id="d270e-188">If a permissions error occurs, change the permissions:</span></span>

```console
chmod u+x <app_executable>
```

<span data-ttu-id="d270e-189">Wenn die App auf dem Server ausgeführt wird, über das Internet jedoch nicht reagiert, sollten Sie die Firewall des Servers überprüfen und sicherstellen, dass Port 80 geöffnet ist.</span><span class="sxs-lookup"><span data-stu-id="d270e-189">If the app runs on the server but fails to respond over the Internet, check the server's firewall and confirm that port 80 is open.</span></span> <span data-ttu-id="d270e-190">Fügen Sie bei Verwendung einer Azure-Ubuntu-VM eine Regel der Netzwerksicherheitsgruppe (NSG) zur Aktivierung des eingehenden Datenverkehrs an Port 80 hinzu.</span><span class="sxs-lookup"><span data-stu-id="d270e-190">If using an Azure Ubuntu VM, add a Network Security Group (NSG) rule that enables inbound port 80 traffic.</span></span> <span data-ttu-id="d270e-191">Eine Regel für ausgehenden Datenverkehr an Port 80 muss nicht aktiviert werden, da der ausgehende Datenverkehr automatisch gewährt wird, wenn die Regel für den eingehenden Datenverkehr aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="d270e-191">There's no need to enable an outbound port 80 rule, as the outbound traffic is automatically granted when the inbound rule is enabled.</span></span>

<span data-ttu-id="d270e-192">Nach dem Testen der App können Sie die App mit `Ctrl+C` in der Eingabeaufforderung beenden.</span><span class="sxs-lookup"><span data-stu-id="d270e-192">When done testing the app, shut the app down with `Ctrl+C` at the command prompt.</span></span>

## <a name="monitoring-the-app"></a><span data-ttu-id="d270e-193">Überwachen der App</span><span class="sxs-lookup"><span data-stu-id="d270e-193">Monitoring the app</span></span>

<span data-ttu-id="d270e-194">Der Server ist dafür eingerichtet, Anforderungen an `http://<serveraddress>:80` an die ASP.NET Core-App weiterzuleiten, die unter `http://127.0.0.1:5000` unter Kestrel ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="d270e-194">The server is setup to forward requests made to `http://<serveraddress>:80` on to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="d270e-195">Nginx wurde jedoch nicht dafür eingerichtet, den Kestrel-Prozess zu verwalten.</span><span class="sxs-lookup"><span data-stu-id="d270e-195">However, Nginx isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="d270e-196">*systemd* kann für die Erstellung einer Dienstdatei verwendet werden, um die zugrunde liegende Web-App zu starten und zu überwachen.</span><span class="sxs-lookup"><span data-stu-id="d270e-196">*systemd* can be used to create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="d270e-197">*SystemD* ist ein Initialisierungssystem, das viele leistungsstarke Features zum Starten, Beenden und Verwalten von Prozessen bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="d270e-197">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="d270e-198">Erstellen der Dienstdatei</span><span class="sxs-lookup"><span data-stu-id="d270e-198">Create the service file</span></span>

<span data-ttu-id="d270e-199">Erstellen Sie die Dienstdefinitionsdatei:</span><span class="sxs-lookup"><span data-stu-id="d270e-199">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="d270e-200">Im Folgenden wird ein Beispiel für eine Dienstdatei der App aufgeführt:</span><span class="sxs-lookup"><span data-stu-id="d270e-200">The following is an example service file for the app:</span></span>

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

<span data-ttu-id="d270e-201">Wenn der Benutzer *www-data* durch Ihre Konfiguration nicht verwendet wird, muss der hier definierte Benutzer zunächst erstellt werden, und ihm muss der ordnungsgemäße Besitz für die Dateien erteilt werden.</span><span class="sxs-lookup"><span data-stu-id="d270e-201">If the user *www-data* isn't used by the configuration, the user defined here must be created first and given proper ownership for files.</span></span>

<span data-ttu-id="d270e-202">Linux verfügt über ein Dateisystem, bei dem die Groß-/Kleinschreibung beachtet wird.</span><span class="sxs-lookup"><span data-stu-id="d270e-202">Linux has a case-sensitive file system.</span></span> <span data-ttu-id="d270e-203">Das Festlegen von ASPNETCORE_ENVIRONMENT auf „Production“ führt dazu, dass nach der Konfigurationsdatei *appsettings.Production.json* und nicht nach *appsettings.production.json* gesucht wird.</span><span class="sxs-lookup"><span data-stu-id="d270e-203">Setting ASPNETCORE_ENVIRONMENT to "Production" results in searching for the configuration file *appsettings.Production.json*, not *appsettings.production.json*.</span></span>

> [!NOTE]
> <span data-ttu-id="d270e-204">Einige Werte (z.B. SQL-Verbindungszeichenfolgen) müssen mit Escapezeichen versehen werden, damit die Konfigurationsanbieter die Umgebungsvariablen lesen können.</span><span class="sxs-lookup"><span data-stu-id="d270e-204">Some values (for example, SQL connection strings) must be escaped for the configuration providers to read the environment variables.</span></span> <span data-ttu-id="d270e-205">Mit dem folgenden Befehl können Sie einen ordnungsgemäß mit Escapezeichen versehenen Wert für die Verwendung in der Konfigurationsdatei generieren:</span><span class="sxs-lookup"><span data-stu-id="d270e-205">Use the following command to generate a properly escaped value for use in the configuration file:</span></span>
>
> ```console
> systemd-escape "<value-to-escape>"
> ```

<span data-ttu-id="d270e-206">Speichern Sie die Datei, und aktivieren Sie den Dienst.</span><span class="sxs-lookup"><span data-stu-id="d270e-206">Save the file and enable the service.</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="d270e-207">Starten Sie den Dienst, und überprüfen Sie, ob er ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="d270e-207">Start the service and verify that it's running.</span></span>

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

<span data-ttu-id="d270e-208">Wenn der Reverseproxy konfiguriert ist und Kestrel durch „systemd“ verwaltet wird, ist die Webanwendung vollständig konfiguriert. Auf diese kann dann über einen Browser auf dem lokalen Computer unter `http://localhost` zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="d270e-208">With the reverse proxy configured and Kestrel managed through systemd, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="d270e-209">Der Zugriff ist auch von einem Remotecomputer aus möglich, sofern keine Firewall diesen blockiert.</span><span class="sxs-lookup"><span data-stu-id="d270e-209">It's also accessible from a remote machine, barring any firewall that might be blocking.</span></span> <span data-ttu-id="d270e-210">Beim Überprüfen der Antwortheader zeigt der Header `Server` die ASP.NET Core-App an, die von Kestrel verarbeitet wird.</span><span class="sxs-lookup"><span data-stu-id="d270e-210">Inspecting the response headers, the `Server` header shows the ASP.NET Core app being served by Kestrel.</span></span>

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="d270e-211">Anzeigen von Protokollen</span><span class="sxs-lookup"><span data-stu-id="d270e-211">Viewing logs</span></span>

<span data-ttu-id="d270e-212">Da die Web-App, die Kestrel verwendet, von `systemd` verwaltet wird, werden alle Ereignisse und Prozesse in einem zentralen Journal protokolliert.</span><span class="sxs-lookup"><span data-stu-id="d270e-212">Since the web app using Kestrel is managed using `systemd`, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="d270e-213">Dieses Journal enthält jedoch alle Einträge für alle Dienste und Prozesse, die von `systemd` verwaltet werden.</span><span class="sxs-lookup"><span data-stu-id="d270e-213">However, this journal includes all entries for all services and processes managed by `systemd`.</span></span> <span data-ttu-id="d270e-214">Verwenden Sie folgenden Befehl, um die `kestrel-hellomvc.service`-spezifischen Elemente anzuzeigen:</span><span class="sxs-lookup"><span data-stu-id="d270e-214">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="d270e-215">Für weiteres Filtern können Zeitoptionen wie `--since today`, `--until 1 hour ago` oder eine Kombination aus diesen die Menge der zurückgegebenen Einträge verringern.</span><span class="sxs-lookup"><span data-stu-id="d270e-215">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="data-protection"></a><span data-ttu-id="d270e-216">Schutz von Daten</span><span class="sxs-lookup"><span data-stu-id="d270e-216">Data protection</span></span>

<span data-ttu-id="d270e-217">Der [Stapel zum Schutz von Daten in ASP.NET Core](xref:security/data-protection/index) wird von mehreren [ASP.NET Core-Middlewares](xref:fundamentals/middleware/index) verwendet. Hierzu gehören die Authentifizierungsmiddleware (zum Beispiel die Cookiemiddleware) und Maßnahmen zum Schutz vor websiteübergreifenden Anforderungsfälschungen (CSRF).</span><span class="sxs-lookup"><span data-stu-id="d270e-217">The [ASP.NET Core Data Protection stack](xref:security/data-protection/index) is used by several ASP.NET Core [middlewares](xref:fundamentals/middleware/index), including authentication middleware (for example, cookie middleware) and cross-site request forgery (CSRF) protections.</span></span> <span data-ttu-id="d270e-218">Selbst wenn Datenschutz-APIs nicht im Benutzercode aufgerufen werden, sollte der Schutz von Daten konfiguriert werden, um einen persistenten kryptografischen [Schlüsselspeicher](xref:security/data-protection/implementation/key-management) zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="d270e-218">Even if Data Protection APIs aren't called by user code, data protection should be configured to create a persistent cryptographic [key store](xref:security/data-protection/implementation/key-management).</span></span> <span data-ttu-id="d270e-219">Wenn der Schutz von Daten nicht konfiguriert ist, werden die Schlüssel beim Neustarten der App im Arbeitsspeicher gespeichert und verworfen.</span><span class="sxs-lookup"><span data-stu-id="d270e-219">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="d270e-220">Falls der Schlüsselbund im Arbeitsspeicher gespeichert wird, wenn die App neu gestartet wird, gilt Folgendes:</span><span class="sxs-lookup"><span data-stu-id="d270e-220">If the key ring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="d270e-221">Alle cookiebasierten Authentifizierungstoken für ungültig erklärt.</span><span class="sxs-lookup"><span data-stu-id="d270e-221">All cookie-based authentication tokens are invalidated.</span></span>
* <span data-ttu-id="d270e-222">Benutzer müssen sich bei ihrer nächsten Anforderung erneut anmelden.</span><span class="sxs-lookup"><span data-stu-id="d270e-222">Users are required to sign in again on their next request.</span></span>
* <span data-ttu-id="d270e-223">Alle mit dem Schlüsselbund geschützte Daten können nicht mehr entschlüsselt werden.</span><span class="sxs-lookup"><span data-stu-id="d270e-223">Any data protected with the key ring can no longer be decrypted.</span></span> <span data-ttu-id="d270e-224">Dies kann [CSRF-Token](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) und [ASP.NET Core-MVC-TempData-Cookies](xref:fundamentals/app-state#tempdata) einschließen.</span><span class="sxs-lookup"><span data-stu-id="d270e-224">This may include [CSRF tokens](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) and [ASP.NET Core MVC TempData cookies](xref:fundamentals/app-state#tempdata).</span></span>

<span data-ttu-id="d270e-225">Wenn Sie den Schutz von Daten konfigurieren möchten, um den Schlüsselring persistent zu speichern und zu verschlüsseln, finden Sie in den folgenden Artikeln weitere Informationen:</span><span class="sxs-lookup"><span data-stu-id="d270e-225">To configure data protection to persist and encrypt the key ring, see:</span></span>

* <xref:security/data-protection/implementation/key-storage-providers>
* <xref:security/data-protection/implementation/key-encryption-at-rest>

## <a name="securing-the-app"></a><span data-ttu-id="d270e-226">Sichern der App</span><span class="sxs-lookup"><span data-stu-id="d270e-226">Securing the app</span></span>

### <a name="enable-apparmor"></a><span data-ttu-id="d270e-227">Aktivieren von AppArmor</span><span class="sxs-lookup"><span data-stu-id="d270e-227">Enable AppArmor</span></span>

<span data-ttu-id="d270e-228">Bei Linux Security Modules (LSM) handelt es sich um ein Framework, das seit Linux 2.6 Teil des Linux-Kernels ist.</span><span class="sxs-lookup"><span data-stu-id="d270e-228">Linux Security Modules (LSM) is a framework that's part of the Linux kernel since Linux 2.6.</span></span> <span data-ttu-id="d270e-229">LSM unterstützt verschiedene Implementierungen von Sicherheitsmodulen.</span><span class="sxs-lookup"><span data-stu-id="d270e-229">LSM supports different implementations of security modules.</span></span> <span data-ttu-id="d270e-230">[AppArmor](https://wiki.ubuntu.com/AppArmor) ist ein LSM, das ein obligatorisches Zugriffssteuerungssystem implementiert, das das Programm auf einen beschränkten Satz von Ressourcen begrenzt.</span><span class="sxs-lookup"><span data-stu-id="d270e-230">[AppArmor](https://wiki.ubuntu.com/AppArmor) is a LSM that implements a Mandatory Access Control system which allows confining the program to a limited set of resources.</span></span> <span data-ttu-id="d270e-231">Versichern Sie sich, dass AppArmor aktiviert und ordnungsgemäß konfiguriert ist.</span><span class="sxs-lookup"><span data-stu-id="d270e-231">Ensure AppArmor is enabled and properly configured.</span></span>

### <a name="configuring-the-firewall"></a><span data-ttu-id="d270e-232">Konfigurieren der Firewall</span><span class="sxs-lookup"><span data-stu-id="d270e-232">Configuring the firewall</span></span>

<span data-ttu-id="d270e-233">Schließen Sie alle externen Ports, die nicht verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="d270e-233">Close off all external ports that are not in use.</span></span> <span data-ttu-id="d270e-234">„Uncomplicated Firewall“ (UFW) stellt ein Front-End für `iptables` bereit, indem eine Befehlszeilenschnittstelle zum Konfigurieren der Firewall bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="d270e-234">Uncomplicated firewall (ufw) provides a front end for `iptables` by providing a command line interface for configuring the firewall.</span></span>

> [!WARNING]
> <span data-ttu-id="d270e-235">Eine Firewall verhindert den Zugriff auf das gesamte System, wenn dieses nicht ordnungsgemäß konfiguriert ist.</span><span class="sxs-lookup"><span data-stu-id="d270e-235">A firewall will prevent access to the whole system if not configured correctly.</span></span> <span data-ttu-id="d270e-236">Falls Sie SSH zum Verbindungsaufbau verwenden und den falschen SSH-Port angeben, können Sie nicht mehr auf das System zugreifen.</span><span class="sxs-lookup"><span data-stu-id="d270e-236">Failure to specify the correct SSH port will effectively lock you out of the system if you are using SSH to connect to it.</span></span> <span data-ttu-id="d270e-237">Der Standardport ist 22.</span><span class="sxs-lookup"><span data-stu-id="d270e-237">The default port is 22.</span></span> <span data-ttu-id="d270e-238">Weitere Informationen finden Sie unter [introduction to ufw (Einführung in ufw)](https://help.ubuntu.com/community/UFW) und in den [Manpages](http://manpages.ubuntu.com/manpages/bionic/man8/ufw.8.html).</span><span class="sxs-lookup"><span data-stu-id="d270e-238">For more information, see the [introduction to ufw](https://help.ubuntu.com/community/UFW) and the [manual](http://manpages.ubuntu.com/manpages/bionic/man8/ufw.8.html).</span></span>

<span data-ttu-id="d270e-239">Installieren Sie `ufw`, und konfigurieren Sie das Tool so, dass der Datenverkehr auf allen erforderlich Ports zugelassen wird.</span><span class="sxs-lookup"><span data-stu-id="d270e-239">Install `ufw` and configure it to allow traffic on any ports needed.</span></span>

```bash
sudo apt-get install ufw

sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

sudo ufw enable
```

### <a name="securing-nginx"></a><span data-ttu-id="d270e-240">Sichern von Nginx</span><span class="sxs-lookup"><span data-stu-id="d270e-240">Securing Nginx</span></span>

#### <a name="change-the-nginx-response-name"></a><span data-ttu-id="d270e-241">Ändern des Namens der Nginx-Antwort</span><span class="sxs-lookup"><span data-stu-id="d270e-241">Change the Nginx response name</span></span>

<span data-ttu-id="d270e-242">Bearbeiten Sie *src/http/ngx_http_header_filter_module.c*:</span><span class="sxs-lookup"><span data-stu-id="d270e-242">Edit *src/http/ngx_http_header_filter_module.c*:</span></span>

```c
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-options"></a><span data-ttu-id="d270e-243">Konfigurieren von Optionen</span><span class="sxs-lookup"><span data-stu-id="d270e-243">Configure options</span></span>

<span data-ttu-id="d270e-244">Konfigurieren Sie den Server mit zusätzlichen erforderlichen Modulen.</span><span class="sxs-lookup"><span data-stu-id="d270e-244">Configure the server with additional required modules.</span></span> <span data-ttu-id="d270e-245">Erwägen Sie zum Schutz der App die Verwendung einer Web-App-Firewall wie z.B. [ModSecurity](https://www.modsecurity.org/).</span><span class="sxs-lookup"><span data-stu-id="d270e-245">Consider using a web app firewall, such as [ModSecurity](https://www.modsecurity.org/), to harden the app.</span></span>

#### <a name="configure-ssl"></a><span data-ttu-id="d270e-246">Konfigurieren von SSL</span><span class="sxs-lookup"><span data-stu-id="d270e-246">Configure SSL</span></span>

* <span data-ttu-id="d270e-247">Konfigurieren Sie den Server, damit dieser für den HTTPS-Datenverkehr an Port `443` empfangsbereit ist, indem Sie ein gültiges Zertifikat angeben, das von einer vertrauenswürdigen Zertifizierungsstelle (Certificate Authority, CA) ausgestellt wurde.</span><span class="sxs-lookup"><span data-stu-id="d270e-247">Configure the server to listen to HTTPS traffic on port `443` by specifying a valid certificate issued by a trusted Certificate Authority (CA).</span></span>

* <span data-ttu-id="d270e-248">Stärken Sie Ihre Sicherheit, indem Sie einige der in der folgenden Datei (*/etc/nginx/nginx.conf*) dargestellten Methoden verwenden.</span><span class="sxs-lookup"><span data-stu-id="d270e-248">Harden the security by employing some of the practices depicted in the following */etc/nginx/nginx.conf* file.</span></span> <span data-ttu-id="d270e-249">Die Beispiele schließen das Auswählen einer stärkeren Verschlüsselung und das Weiterleiten allen Datenverkehrs über HTTP auf HTTPS ein.</span><span class="sxs-lookup"><span data-stu-id="d270e-249">Examples include choosing a stronger cipher and redirecting all traffic over HTTP to HTTPS.</span></span>

* <span data-ttu-id="d270e-250">Durch das Hinzufügen eines `HTTP Strict-Transport-Security`-Headers (HSTS) wird sichergestellt, dass alle nachfolgenden Anforderungen vom Client nur über HTTPS erfolgen.</span><span class="sxs-lookup"><span data-stu-id="d270e-250">Adding an `HTTP Strict-Transport-Security` (HSTS) header ensures all subsequent requests made by the client are over HTTPS only.</span></span>

* <span data-ttu-id="d270e-251">Fügen Sie keinen Strict-Transport-Security-Header hinzu, oder wählen Sie ein entsprechendes `max-age` aus, wenn Sie SSL in Zukunft deaktivieren möchten.</span><span class="sxs-lookup"><span data-stu-id="d270e-251">Don't add the Strict-Transport-Security header or chose an appropriate `max-age` if SSL will be disabled in the future.</span></span>

<span data-ttu-id="d270e-252">Fügen Sie die Konfigurationsdatei */etc/nginx/proxy.conf* hinzu:</span><span class="sxs-lookup"><span data-stu-id="d270e-252">Add the */etc/nginx/proxy.conf* configuration file:</span></span>

[!code-nginx[](linux-nginx/proxy.conf)]

<span data-ttu-id="d270e-253">Bearbeiten Sie die Konfigurationsdatei */etc/nginx/nginx.conf*.</span><span class="sxs-lookup"><span data-stu-id="d270e-253">Edit the */etc/nginx/nginx.conf* configuration file.</span></span> <span data-ttu-id="d270e-254">Das Beispiel enthält die Abschnitte `http` und `server` in einer Konfigurationsdatei.</span><span class="sxs-lookup"><span data-stu-id="d270e-254">The example contains both `http` and `server` sections in one configuration file.</span></span>

[!code-nginx[](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a><span data-ttu-id="d270e-255">Sichern von Nginx vor Clickjacking</span><span class="sxs-lookup"><span data-stu-id="d270e-255">Secure Nginx from clickjacking</span></span>
<span data-ttu-id="d270e-256">Clickjacking ist eine böswillige Technik zum Sammeln der Klicks eines infizierten Benutzers.</span><span class="sxs-lookup"><span data-stu-id="d270e-256">Clickjacking is a malicious technique to collect an infected user's clicks.</span></span> <span data-ttu-id="d270e-257">Clickjacking bringt das Opfer (Besucher) dazu, auf eine infizierte Seite zu klicken.</span><span class="sxs-lookup"><span data-stu-id="d270e-257">Clickjacking tricks the victim (visitor) into clicking on an infected site.</span></span> <span data-ttu-id="d270e-258">Verwenden Sie X-FRAME-OPTIONS zum Sichern der Website.</span><span class="sxs-lookup"><span data-stu-id="d270e-258">Use X-FRAME-OPTIONS to secure the site.</span></span>

<span data-ttu-id="d270e-259">Bearbeiten Sie die Datei *nginx.conf*:</span><span class="sxs-lookup"><span data-stu-id="d270e-259">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="d270e-260">Fügen Sie die Zeile `add_header X-Frame-Options "SAMEORIGIN";` hinzu, und speichern Sie die Datei. Starten Sie Nginx dann neu.</span><span class="sxs-lookup"><span data-stu-id="d270e-260">Add the line `add_header X-Frame-Options "SAMEORIGIN";` and save the file, then restart Nginx.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="d270e-261">MIME-Typermittlung</span><span class="sxs-lookup"><span data-stu-id="d270e-261">MIME-type sniffing</span></span>

<span data-ttu-id="d270e-262">Dieser Header hindert die meisten Browser an der MIME-Ermittlung einer Antwort vom deklarierten Inhaltstyp weg, da der Header den Browser anweist, den Inhaltstyp der Antwort nicht zu überschreiben.</span><span class="sxs-lookup"><span data-stu-id="d270e-262">This header prevents most browsers from MIME-sniffing a response away from the declared content type, as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="d270e-263">Durch die Option `nosniff` wird der Inhalt vom Browser als „text/html“ gerendert, wenn der Server sagt, dass es sich bei diesem um „text/html“ handelt.</span><span class="sxs-lookup"><span data-stu-id="d270e-263">With the `nosniff` option, if the server says the content is "text/html", the browser renders it as "text/html".</span></span>

<span data-ttu-id="d270e-264">Bearbeiten Sie die Datei *nginx.conf*:</span><span class="sxs-lookup"><span data-stu-id="d270e-264">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="d270e-265">Fügen Sie die Zeile `add_header X-Content-Type-Options "nosniff";` hinzu, und speichern Sie die Datei. Starten Sie Nginx dann neu.</span><span class="sxs-lookup"><span data-stu-id="d270e-265">Add the line `add_header X-Content-Type-Options "nosniff";` and save the file, then restart Nginx.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d270e-266">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="d270e-266">Additional resources</span></span>

* [<span data-ttu-id="d270e-267">Voraussetzungen für .NET Core unter Linux</span><span class="sxs-lookup"><span data-stu-id="d270e-267">Prerequisites for .NET Core on Linux</span></span>](/dotnet/core/linux-prerequisites)
* [<span data-ttu-id="d270e-268">Nginx: Binary Releases: Official Debian/Ubuntu packages (Nginx: Binäre Releases – offizielle Debian/Ubuntu-Pakete)</span><span class="sxs-lookup"><span data-stu-id="d270e-268">Nginx: Binary Releases: Official Debian/Ubuntu packages</span></span>](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages)
* [<span data-ttu-id="d270e-269">Konfigurieren von ASP.NET Core zur Verwendung mit Proxyservern und Lastenausgleich</span><span class="sxs-lookup"><span data-stu-id="d270e-269">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>](xref:host-and-deploy/proxy-load-balancer)
* [<span data-ttu-id="d270e-270">NGINX: Using the Forwarded header (NGINX: Verwenden des weitergeleiteten Headers)</span><span class="sxs-lookup"><span data-stu-id="d270e-270">NGINX: Using the Forwarded header</span></span>](https://www.nginx.com/resources/wiki/start/topics/examples/forwarded/)
