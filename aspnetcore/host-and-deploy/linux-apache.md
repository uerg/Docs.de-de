---
title: Hosten von ASP.NET Core unter Linux mit Apache
description: Informationen zum Einrichten von Apache als Reverseproxyserver für CentOS zur Weiterleitung von HTTP-Datenverkehr an eine ASP.NET Core-Web-App, die unter Kestrel ausgeführt wird.
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
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
ms.locfileid: "33962816"
---
# <a name="host-aspnet-core-on-linux-with-apache"></a><span data-ttu-id="86634-103">Hosten von ASP.NET Core unter Linux mit Apache</span><span class="sxs-lookup"><span data-stu-id="86634-103">Host ASP.NET Core on Linux with Apache</span></span>

<span data-ttu-id="86634-104">Von [Shayne Boyer](https://github.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="86634-104">By [Shayne Boyer](https://github.com/spboyer)</span></span>

<span data-ttu-id="86634-105">In dieser Führungslinie finden Sie Informationen zur Einrichtung von [Apache](https://httpd.apache.org/) als Reverseproxyserver für [CentOS 7](https://www.centos.org/) zur Weiterleitung von HTTP-Datenverkehr an eine ASP.NET Core-Web-App, die unter [Kestrel](xref:fundamentals/servers/kestrel) ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="86634-105">Using this guide, learn how to set up [Apache](https://httpd.apache.org/) as a reverse proxy server on [CentOS 7](https://www.centos.org/) to redirect HTTP traffic to an ASP.NET Core web app running on [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="86634-106">Durch die Erweiterung [mod_proxy](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) und zugehörige Module wird der Reverseproxy des Servers erstellt.</span><span class="sxs-lookup"><span data-stu-id="86634-106">The [mod_proxy extension](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) and related modules create the server's reverse proxy.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="86634-107">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="86634-107">Prerequisites</span></span>

1. <span data-ttu-id="86634-108">Server, auf dem CentOS 7 ausgeführt wird, mit einem Standardbenutzerkonto mit sudo-Berechtigung</span><span class="sxs-lookup"><span data-stu-id="86634-108">Server running CentOS 7 with a standard user account with sudo privilege</span></span>
2. <span data-ttu-id="86634-109">ASP.NET Core-App</span><span class="sxs-lookup"><span data-stu-id="86634-109">ASP.NET Core app</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="86634-110">Veröffentlichen der App</span><span class="sxs-lookup"><span data-stu-id="86634-110">Publish the app</span></span>

<span data-ttu-id="86634-111">Veröffentlichen Sie die App in der Releasekonfiguration für die CentOS 7-Runtime (`centos.7-x64`) als [eigenständige Entwicklung](/dotnet/core/deploying/#self-contained-deployments-scd).</span><span class="sxs-lookup"><span data-stu-id="86634-111">Publish the app as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) in Release configuration for the CentOS 7 runtime (`centos.7-x64`).</span></span> <span data-ttu-id="86634-112">Kopieren Sie die Inhalte des Ordners *bin/Release/netcoreapp2.0/centos.7-x64/publish* mit SCP, FTP oder einer anderen Dateiübertragungsmethode auf den Server.</span><span class="sxs-lookup"><span data-stu-id="86634-112">Copy the contents of the *bin/Release/netcoreapp2.0/centos.7-x64/publish* folder to the server using SCP, FTP, or other file transfer method.</span></span>

> [!NOTE]
> <span data-ttu-id="86634-113">In einem Szenario für die Bereitstellung in der Produktion übernimmt ein Continuous Integration-Workflow die Veröffentlichung der App und das Kopieren der Objekte auf den Server.</span><span class="sxs-lookup"><span data-stu-id="86634-113">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span> 

## <a name="configure-a-proxy-server"></a><span data-ttu-id="86634-114">Konfigurieren eines Proxyservers</span><span class="sxs-lookup"><span data-stu-id="86634-114">Configure a proxy server</span></span>

<span data-ttu-id="86634-115">Ein Reverseproxy wird im Allgemeinen zur Unterstützung dynamischer Web-Apps eingerichtet.</span><span class="sxs-lookup"><span data-stu-id="86634-115">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="86634-116">Der Reverseproxy beendet die HTTP-Anforderung und leitet diese an die ASP.NET-App weiter.</span><span class="sxs-lookup"><span data-stu-id="86634-116">The reverse proxy terminates the HTTP request and forwards it to the ASP.NET app.</span></span>

<span data-ttu-id="86634-117">Ein Proxyserver dient zum Weiterleiten von Clientanforderungen an einen anderen Server, anstatt Anforderungen selbst zu erfüllen.</span><span class="sxs-lookup"><span data-stu-id="86634-117">A proxy server is one which forwards client requests to another server instead of fulfilling requests itself.</span></span> <span data-ttu-id="86634-118">Ein Reverseproxy leitet Daten an ein festes Ziel meist im Auftrag beliebiger Clients weiter.</span><span class="sxs-lookup"><span data-stu-id="86634-118">A reverse proxy forwards to a fixed destination, typically on behalf of arbitrary clients.</span></span> <span data-ttu-id="86634-119">In diesem Handbuch wird Apache als Reverseproxy konfiguriert, der auf demselben Server ausgeführt wird, auf dem Kestrel die ASP.NET Core-App verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="86634-119">In this guide, Apache is configured as the reverse proxy running on the same server that Kestrel is serving the ASP.NET Core app.</span></span>

<span data-ttu-id="86634-120">Da Anforderungen vom Reverseproxy weitergeleitet werden, sollten Sie die Middleware für weitergeleitete Header aus dem Paket [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) verwenden.</span><span class="sxs-lookup"><span data-stu-id="86634-120">Because requests are forwarded by reverse proxy, use the Forwarded Headers Middleware from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="86634-121">Diese Middleware aktualisiert `Request.Scheme` mithilfe des `X-Forwarded-Proto`-Headers, sodass Umleitungs-URIs und andere Sicherheitsrichtlinien ordnungsgemäß funktionieren.</span><span class="sxs-lookup"><span data-stu-id="86634-121">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="86634-122">Wenn Sie einen beliebigen Typ von Authentifizierungsmiddleware verwenden, muss die Middleware für weitergeleitete Header zuerst ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="86634-122">When using any type of authentication middleware, the Forwarded Headers Middleware must run first.</span></span> <span data-ttu-id="86634-123">Durch diese Reihenfolge wird sichergestellt, dass die Authentifizierungsmiddleware die Headerwerte nutzen und richtige Umleitungs-URIs generieren kann.</span><span class="sxs-lookup"><span data-stu-id="86634-123">This ordering ensures that the authentication middleware can consume the header values and generate correct redirect URIs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="86634-124">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="86634-124">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="86634-125">Rufen Sie die Methode [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) in `Startup.Configure` auf, bevor Sie [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) oder eine ähnliche Middleware für Authentifizierungsschemas aufrufen.</span><span class="sxs-lookup"><span data-stu-id="86634-125">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="86634-126">Konfigurieren Sie die Middleware so, dass die Header `X-Forwarded-For` und `X-Forwarded-Proto` weitergeleitet werden:</span><span class="sxs-lookup"><span data-stu-id="86634-126">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="86634-127">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="86634-127">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="86634-128">Rufen Sie die Methode [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) in `Startup.Configure` auf, bevor Sie [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) und [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) oder eine ähnliche Middleware für Authentifizierungsschemas aufrufen.</span><span class="sxs-lookup"><span data-stu-id="86634-128">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="86634-129">Konfigurieren Sie die Middleware so, dass die Header `X-Forwarded-For` und `X-Forwarded-Proto` weitergeleitet werden:</span><span class="sxs-lookup"><span data-stu-id="86634-129">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

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

<span data-ttu-id="86634-130">Wenn für die Middleware keine [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) angegeben sind, lauten die weiterzuleitenden Standardheader `None`.</span><span class="sxs-lookup"><span data-stu-id="86634-130">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="86634-131">Möglicherweise ist zusätzliche Konfiguration für Apps erforderlich, die hinter Proxyservern und Lastenausgleichsmodulen (Load Balancer) gehostet werden.</span><span class="sxs-lookup"><span data-stu-id="86634-131">Additional configuration might be required for apps hosted behind proxy servers and load balancers.</span></span> <span data-ttu-id="86634-132">Weitere Informationen hierzu feinden Sie unter [Konfigurieren von ASP.NET Core zur Verwendung mit Proxyservern und Lastenausgleich](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="86634-132">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="install-apache"></a><span data-ttu-id="86634-133">Installieren von Apache</span><span class="sxs-lookup"><span data-stu-id="86634-133">Install Apache</span></span>

<span data-ttu-id="86634-134">So aktualisieren Sie CentOS-Pakete auf ihre aktuellsten stabilen Versionen:</span><span class="sxs-lookup"><span data-stu-id="86634-134">Update CentOS packages to their latest stable versions:</span></span>

```bash
sudo yum update -y
```

<span data-ttu-id="86634-135">Installieren Sie die Apache-Webserver unter CentOS mit einem einzelnen `yum`-Befehl:</span><span class="sxs-lookup"><span data-stu-id="86634-135">Install the Apache web server on CentOS with a single `yum` command:</span></span>

```bash
sudo yum -y install httpd mod_ssl
```

<span data-ttu-id="86634-136">Beispielausgabe nach der Ausführung des Befehls:</span><span class="sxs-lookup"><span data-stu-id="86634-136">Sample output after running the command:</span></span>

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
> <span data-ttu-id="86634-137">In diesem Beispiel enthält die Ausgabe „httpd.86_64“, da die CentOS 7-Version eine 64-Bit-Version ist.</span><span class="sxs-lookup"><span data-stu-id="86634-137">In this example, the output reflects httpd.86_64 since the CentOS 7 version is 64 bit.</span></span> <span data-ttu-id="86634-138">Um zu prüfen, wo Apache installiert ist, führen Sie an einer Eingabeaufforderung `whereis httpd` aus.</span><span class="sxs-lookup"><span data-stu-id="86634-138">To verify where Apache is installed, run `whereis httpd` from a command prompt.</span></span>

### <a name="configure-apache-for-reverse-proxy"></a><span data-ttu-id="86634-139">Konfigurieren von Apache als Reverseproxy</span><span class="sxs-lookup"><span data-stu-id="86634-139">Configure Apache for reverse proxy</span></span>

<span data-ttu-id="86634-140">Konfigurationsdateien für Apache befinden sich im Verzeichnis `/etc/httpd/conf.d/`.</span><span class="sxs-lookup"><span data-stu-id="86634-140">Configuration files for Apache are located within the `/etc/httpd/conf.d/` directory.</span></span> <span data-ttu-id="86634-141">Dateien mit der Erweiterung *.conf* werden in alphabetischer Reihenfolge zusätzlich zu den Modulkonfigurationsdateien im Verzeichnis `/etc/httpd/conf.modules.d/` verarbeitet, das Konfigurationsdateien enthält, die zum Laden von Modulen erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="86634-141">Any file with the *.conf* extension is processed in alphabetical order in addition to the module configuration files in `/etc/httpd/conf.modules.d/`, which contains any configuration files necessary to load modules.</span></span>

<span data-ttu-id="86634-142">So erstellen Sie eine Konfigurationsdatei mit dem Namen *hellomvc.conf* für die App:</span><span class="sxs-lookup"><span data-stu-id="86634-142">Create a configuration file, named *hellomvc.conf*, for the app:</span></span>

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

<span data-ttu-id="86634-143">Der Block `VirtualHost` kann mehrmals erscheinen, in mindestens einer Datei auf einem Server.</span><span class="sxs-lookup"><span data-stu-id="86634-143">The `VirtualHost` block can appear multiple times, in one or more files on a server.</span></span> <span data-ttu-id="86634-144">In der vorangehenden Konfigurationsdatei akzeptiert Apache öffentlichen Datenverkehr über Port 80.</span><span class="sxs-lookup"><span data-stu-id="86634-144">In the preceding configuration file, Apache accepts public traffic on port 80.</span></span> <span data-ttu-id="86634-145">Die Domäne `www.example.com` wird bearbeitet, und der Alias `*.example.com` wird auf der gleichen Website aufgelöst.</span><span class="sxs-lookup"><span data-stu-id="86634-145">The domain `www.example.com` is being served, and the `*.example.com` alias resolves to the same website.</span></span> <span data-ttu-id="86634-146">Weitere Informationen finden Sie unter [Namensbasierte virtuelle Hostunterstützung](https://httpd.apache.org/docs/current/vhosts/name-based.html).</span><span class="sxs-lookup"><span data-stu-id="86634-146">See [Name-based virtual host support](https://httpd.apache.org/docs/current/vhosts/name-based.html) for more information.</span></span> <span data-ttu-id="86634-147">Anforderungen werden über einen Proxy auf Stammebene an Port 5000 des Servers unter 127.0.0.1 übergeben.</span><span class="sxs-lookup"><span data-stu-id="86634-147">Requests are proxied at the root to port 5000 of the server at 127.0.0.1.</span></span> <span data-ttu-id="86634-148">Für bidirektionale Kommunikation sind `ProxyPass` und `ProxyPassReverse` erforderlich.</span><span class="sxs-lookup"><span data-stu-id="86634-148">For bi-directional communication, `ProxyPass` and `ProxyPassReverse` are required.</span></span>

> [!WARNING]
> <span data-ttu-id="86634-149">Schlägt die Angabe einer ordnungsgemäßen [ServerName-Anweisung](https://httpd.apache.org/docs/current/mod/core.html#servername) im Block **VirtualHost** fehl, ist Ihre App Sicherheitsrisiken ausgesetzt.</span><span class="sxs-lookup"><span data-stu-id="86634-149">Failure to specify a proper [ServerName directive](https://httpd.apache.org/docs/current/mod/core.html#servername) in the **VirtualHost** block exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="86634-150">Platzhalterbindungen in untergeordneten Domänen (z.B. `*.example.com`) verursachen kein Sicherheitsrisiko, wenn Sie die gesamte übergeordnete Domäne steuern (im Gegensatz zu `*.com`, das angreifbar ist).</span><span class="sxs-lookup"><span data-stu-id="86634-150">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="86634-151">Weitere Informationen finden Sie unter [rfc7230 im Abschnitt 5.4](https://tools.ietf.org/html/rfc7230#section-5.4).</span><span class="sxs-lookup"><span data-stu-id="86634-151">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="86634-152">Die Protokollierung kann über `VirtualHost` mit den `ErrorLog`- und `CustomLog`-Anweisungen konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="86634-152">Logging can be configured per `VirtualHost` using `ErrorLog` and `CustomLog` directives.</span></span> <span data-ttu-id="86634-153">`ErrorLog` ist der Speicherort, an dem der Server Fehler protokolliert. `CustomLog` legt den Dateinamen und das Format der Protokolldatei fest.</span><span class="sxs-lookup"><span data-stu-id="86634-153">`ErrorLog` is the location where the server logs errors, and `CustomLog` sets the filename and format of log file.</span></span> <span data-ttu-id="86634-154">In diesem Fall werden hier Anforderungsinformationen protokolliert.</span><span class="sxs-lookup"><span data-stu-id="86634-154">In this case, this is where request information is logged.</span></span> <span data-ttu-id="86634-155">Für jede Anforderung gibt es eine Zeile.</span><span class="sxs-lookup"><span data-stu-id="86634-155">There's one line for each request.</span></span>

<span data-ttu-id="86634-156">Speichern Sie die Datei, und testen Sie die Konfiguration.</span><span class="sxs-lookup"><span data-stu-id="86634-156">Save the file and test the configuration.</span></span> <span data-ttu-id="86634-157">Wenn alle Anforderungen durchgehen, muss die Antwort `Syntax [OK]` lauten.</span><span class="sxs-lookup"><span data-stu-id="86634-157">If everything passes, the response should be `Syntax [OK]`.</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="86634-158">So starten Sie Apache neu:</span><span class="sxs-lookup"><span data-stu-id="86634-158">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

## <a name="monitoring-the-app"></a><span data-ttu-id="86634-159">Überwachen der App</span><span class="sxs-lookup"><span data-stu-id="86634-159">Monitoring the app</span></span>

<span data-ttu-id="86634-160">Apache ist jetzt dafür eingerichtet, Anforderungen an `http://localhost:80` an die ASP.NET Core-App weiterzuleiten, die unter Kestrel unter `http://127.0.0.1:5000` ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="86634-160">Apache is now setup to forward requests made to `http://localhost:80` to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span>  <span data-ttu-id="86634-161">Apache ist jedoch nicht dafür eingerichtet, den Kestrel-Prozess zu verwalten.</span><span class="sxs-lookup"><span data-stu-id="86634-161">However, Apache isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="86634-162">Verwenden Sie *systemd*, und erstellen eine Dienstdatei, um die zugrunde liegende Web-App zu starten und zu überwachen.</span><span class="sxs-lookup"><span data-stu-id="86634-162">Use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="86634-163">*SystemD* ist ein Initialisierungssystem, das viele leistungsstarke Features zum Starten, Beenden und Verwalten von Prozessen bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="86634-163">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 


### <a name="create-the-service-file"></a><span data-ttu-id="86634-164">Erstellen der Dienstdatei</span><span class="sxs-lookup"><span data-stu-id="86634-164">Create the service file</span></span>

<span data-ttu-id="86634-165">Erstellen Sie die Dienstdefinitionsdatei:</span><span class="sxs-lookup"><span data-stu-id="86634-165">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="86634-166">Eine Beispieldienstdatei für die App:</span><span class="sxs-lookup"><span data-stu-id="86634-166">An example service file for the app:</span></span>

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
> <span data-ttu-id="86634-167">**Benutzer:** &mdash;Wenn der Benutzer *apache* von Ihrer Konfiguration nicht verwendet wird, muss der Benutzer zunächst erstellt werden. Darüber hinaus müssen ihm die ordnungsgemäßen Besitzrechte für Dateien zugewiesen werden.</span><span class="sxs-lookup"><span data-stu-id="86634-167">**User** &mdash; If the user *apache* isn't used by the configuration, the user must be created first and given proper ownership for files.</span></span>

> [!NOTE]
> <span data-ttu-id="86634-168">Einige Werte (z.B. SQL-Verbindungszeichenfolgen) müssen mit Escapezeichen versehen werden, damit die Konfigurationsanbieter die Umgebungsvariablen lesen können.</span><span class="sxs-lookup"><span data-stu-id="86634-168">Some values (for example, SQL connection strings) must be escaped for the configuration providers to read the environment variables.</span></span> <span data-ttu-id="86634-169">Mit dem folgenden Befehl können Sie einen ordnungsgemäß mit Escapezeichen versehenen Wert für die Verwendung in der Konfigurationsdatei generieren:</span><span class="sxs-lookup"><span data-stu-id="86634-169">Use the following command to generate a properly escaped value for use in the configuration file:</span></span>
>
> ```console
> systemd-escape "<value-to-escape>"
> ```

<span data-ttu-id="86634-170">So speichern Sie die Datei und aktivieren den Dienst:</span><span class="sxs-lookup"><span data-stu-id="86634-170">Save the file and enable the service:</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="86634-171">So starten Sie den Dienst und überprüfen, ob er ausgeführt wird:</span><span class="sxs-lookup"><span data-stu-id="86634-171">Start the service and verify that it's running:</span></span>

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

<span data-ttu-id="86634-172">Wenn der Reverseproxy konfiguriert ist und Kestrel durch *systemd* verwaltet wird, ist die Web-App vollständig konfiguriert. Auf diese kann dann über einen Browser auf dem lokalen Computer unter `http://localhost` zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="86634-172">With the reverse proxy configured and Kestrel managed through *systemd*, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="86634-173">Beim Überprüfen der Antwortheader wird im Header **Server** angezeigt, dass die ASP.NET Core-App von Kestrel verarbeitet wird:</span><span class="sxs-lookup"><span data-stu-id="86634-173">Inspecting the response headers, the **Server** header indicates that the ASP.NET Core app is served by Kestrel:</span></span>

```
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="86634-174">Anzeigen von Protokollen</span><span class="sxs-lookup"><span data-stu-id="86634-174">Viewing logs</span></span>

<span data-ttu-id="86634-175">Da die von Kestrel verwendete Web-App von *systemd* verwaltet wird, werden Ereignisse und Protokolle in einem zentralen Journal protokolliert.</span><span class="sxs-lookup"><span data-stu-id="86634-175">Since the web app using Kestrel is managed using *systemd*, events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="86634-176">Dieses Journal enthält jedoch Einträge für alle Dienste und Prozesse, die von *systemd* verwaltet werden.</span><span class="sxs-lookup"><span data-stu-id="86634-176">However, this journal includes entries for all of the services and processes managed by *systemd*.</span></span> <span data-ttu-id="86634-177">Verwenden Sie folgenden Befehl, um die `kestrel-hellomvc.service`-spezifischen Elemente anzuzeigen:</span><span class="sxs-lookup"><span data-stu-id="86634-177">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="86634-178">Geben Sie für die Uhrzeitfilterung mit dem Befehl Uhrzeitoptionen an.</span><span class="sxs-lookup"><span data-stu-id="86634-178">For time filtering, specify time options with the command.</span></span> <span data-ttu-id="86634-179">Verwenden Sie beispielsweise `--since today` zum Filtern für den aktuellen Tag oder `--until 1 hour ago`, um die Einträge der letzten Stunde anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="86634-179">For example, use `--since today` to filter for the current day or `--until 1 hour ago` to see the previous hour's entries.</span></span> <span data-ttu-id="86634-180">Weitere Informationen finden Sie auf der [Manpage für „journalctl“](https://www.unix.com/man-page/centos/1/journalctl/).</span><span class="sxs-lookup"><span data-stu-id="86634-180">For more information, see the [man page for journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="86634-181">Sichern der App</span><span class="sxs-lookup"><span data-stu-id="86634-181">Securing the app</span></span>

### <a name="configure-firewall"></a><span data-ttu-id="86634-182">Konfigurieren der Firewall</span><span class="sxs-lookup"><span data-stu-id="86634-182">Configure firewall</span></span>

<span data-ttu-id="86634-183">*Firewalld* ist ein dynamischer Daemon zum Verwalten der Firewall mit Unterstützung für Netzwerkzonen.</span><span class="sxs-lookup"><span data-stu-id="86634-183">*Firewalld* is a dynamic daemon to manage the firewall with support for network zones.</span></span> <span data-ttu-id="86634-184">Ports und Paketfilterung können weiterhin mit „iptables“ verwaltet werden.</span><span class="sxs-lookup"><span data-stu-id="86634-184">Ports and packet filtering can still be managed by iptables.</span></span> <span data-ttu-id="86634-185">*Firewalld* sollte standardmäßig installiert werden.</span><span class="sxs-lookup"><span data-stu-id="86634-185">*Firewalld* should be installed by default.</span></span> <span data-ttu-id="86634-186">`yum` kann für die Installation oder zur Überprüfung einer erfolgreichen Installation verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="86634-186">`yum` can be used to install the package or verify it's installed.</span></span>

```bash
sudo yum install firewalld -y
```

<span data-ttu-id="86634-187">Mit `firewalld` können Sie nur die für die App erforderlichen Ports öffnen.</span><span class="sxs-lookup"><span data-stu-id="86634-187">Use `firewalld` to open only the ports needed for the app.</span></span> <span data-ttu-id="86634-188">In diesem Fall werden die Ports 80 und 443 verwendet.</span><span class="sxs-lookup"><span data-stu-id="86634-188">In this case, port 80 and 443 are used.</span></span> <span data-ttu-id="86634-189">Über die folgenden Befehle werden die Ports 80 und 443 dauerhaft geöffnet:</span><span class="sxs-lookup"><span data-stu-id="86634-189">The following commands permanently set ports 80 and 443 to open:</span></span>

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
```

<span data-ttu-id="86634-190">Laden Sie die Firewalleinstellungen erneut.</span><span class="sxs-lookup"><span data-stu-id="86634-190">Reload the firewall settings.</span></span> <span data-ttu-id="86634-191">Überprüfen Sie die verfügbaren Dienste und Ports in der Standardzone.</span><span class="sxs-lookup"><span data-stu-id="86634-191">Check the available services and ports in the default zone.</span></span> <span data-ttu-id="86634-192">Über `firewall-cmd -h` können Sie verfügbare Optionen ermitteln.</span><span class="sxs-lookup"><span data-stu-id="86634-192">Options are available by inspecting `firewall-cmd -h`.</span></span>

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

### <a name="ssl-configuration"></a><span data-ttu-id="86634-193">SSL-Konfiguration</span><span class="sxs-lookup"><span data-stu-id="86634-193">SSL configuration</span></span>

<span data-ttu-id="86634-194">Für die Konfiguration von Apache für SSL wird das Modul *mod_ssl* verwendet.</span><span class="sxs-lookup"><span data-stu-id="86634-194">To configure Apache for SSL, the *mod_ssl* module is used.</span></span> <span data-ttu-id="86634-195">Wenn das Modul *httpd* installiert wurde, wurde das Modul *mod_ssl* ebenfalls installiert.</span><span class="sxs-lookup"><span data-stu-id="86634-195">When the *httpd* module was installed, the *mod_ssl* module was also installed.</span></span> <span data-ttu-id="86634-196">Wurde es nicht installiert, fügen Sie es mit `yum` zur Konfiguration hinzu.</span><span class="sxs-lookup"><span data-stu-id="86634-196">If it wasn't installed, use `yum` to add it to the configuration.</span></span>

```bash
sudo yum install mod_ssl
```
<span data-ttu-id="86634-197">Installieren Sie zur Erzwingung von SSL das Modul `mod_rewrite`, um eine URL-Umschreibung zu aktivieren:</span><span class="sxs-lookup"><span data-stu-id="86634-197">To enforce SSL, install the `mod_rewrite` module to enable URL rewriting:</span></span>

```bash
sudo yum install mod_rewrite
```

<span data-ttu-id="86634-198">Ändern Sie die Datei *hellomvc.conf*, um eine URL-Umschreibung und sichere Kommunikation an Port 443 zu aktivieren:</span><span class="sxs-lookup"><span data-stu-id="86634-198">Modify the *hellomvc.conf* file to enable URL rewriting and secure communication on port 443:</span></span>

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
> <span data-ttu-id="86634-199">In diesem Beispiel wird ein lokal generiertes Zertifikat verwendet.</span><span class="sxs-lookup"><span data-stu-id="86634-199">This example is using a locally-generated certificate.</span></span> <span data-ttu-id="86634-200">**SSLCertificateFile** muss die primäre Zertifikatdatei für den Domänennamen sein.</span><span class="sxs-lookup"><span data-stu-id="86634-200">**SSLCertificateFile** should be the primary certificate file for the domain name.</span></span> <span data-ttu-id="86634-201">**SSLCertificateKeyFile** muss die Schlüsseldatei sein, die beim Erstellen der Zertifikatsignaturanforderung (Certificate Signing Request, CSR) generiert wurde.</span><span class="sxs-lookup"><span data-stu-id="86634-201">**SSLCertificateKeyFile** should be the key file generated when CSR is created.</span></span> <span data-ttu-id="86634-202">**SSLCertificateChainFile** muss die Zwischenzertifikatdatei (falls vorhanden) sein, die von der Zertifizierungsstelle bereitgestellt wurde.</span><span class="sxs-lookup"><span data-stu-id="86634-202">**SSLCertificateChainFile** should be the intermediate certificate file (if any) that was supplied by the certificate authority.</span></span>

<span data-ttu-id="86634-203">So speichern Sie die Datei und testen die Konfiguration:</span><span class="sxs-lookup"><span data-stu-id="86634-203">Save the file and test the configuration:</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="86634-204">So starten Sie Apache neu:</span><span class="sxs-lookup"><span data-stu-id="86634-204">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a><span data-ttu-id="86634-205">Weitere Vorschläge für Apache</span><span class="sxs-lookup"><span data-stu-id="86634-205">Additional Apache suggestions</span></span>

### <a name="additional-headers"></a><span data-ttu-id="86634-206">Zusätzliche Header</span><span class="sxs-lookup"><span data-stu-id="86634-206">Additional headers</span></span>

<span data-ttu-id="86634-207">Als Schutz gegen böswillige Angriffe müssen einige Header geändert oder hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="86634-207">In order to secure against malicious attacks, there are a few headers that should either be modified or added.</span></span> <span data-ttu-id="86634-208">So stellen Sie sicher, dass das Modul `mod_headers` installiert wurde:</span><span class="sxs-lookup"><span data-stu-id="86634-208">Ensure that the `mod_headers` module is installed:</span></span>

```bash
sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking-attacks"></a><span data-ttu-id="86634-209">Schützen von Apache vor Clickjacking-Angriffen</span><span class="sxs-lookup"><span data-stu-id="86634-209">Secure Apache from clickjacking attacks</span></span>

<span data-ttu-id="86634-210">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), auch bekannt als *UI Redress-Angriff*, ist ein böswilliger Angriff, bei dem ein Websitebesucher dazu verleitet wird, auf einen Link oder eine Schaltfläche zu klicken, der bzw. die sich auf einer anderen Seite als der aktuell besuchten Seite befindet.</span><span class="sxs-lookup"><span data-stu-id="86634-210">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), also known as a *UI redress attack*, is a malicious attack where a website visitor is tricked into clicking a link or button on a different page than they're currently visiting.</span></span> <span data-ttu-id="86634-211">Verwenden Sie `X-FRAME-OPTIONS` zum Sichern der Website.</span><span class="sxs-lookup"><span data-stu-id="86634-211">Use `X-FRAME-OPTIONS` to secure the site.</span></span>

<span data-ttu-id="86634-212">So bearbeiten Sie die Datei *httpd.conf*:</span><span class="sxs-lookup"><span data-stu-id="86634-212">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="86634-213">Fügen Sie die Zeile `Header append X-FRAME-OPTIONS "SAMEORIGIN"` hinzu.</span><span class="sxs-lookup"><span data-stu-id="86634-213">Add the line `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span></span> <span data-ttu-id="86634-214">Speichern Sie die Datei.</span><span class="sxs-lookup"><span data-stu-id="86634-214">Save the file.</span></span> <span data-ttu-id="86634-215">Starten Sie Apache neu.</span><span class="sxs-lookup"><span data-stu-id="86634-215">Restart Apache.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="86634-216">MIME-Typermittlung</span><span class="sxs-lookup"><span data-stu-id="86634-216">MIME-type sniffing</span></span>

<span data-ttu-id="86634-217">Der Header `X-Content-Type-Options` verhindert, dass eine *MIME-Ermittlung* über Internet Explorer (dabei wird der `Content-Type` einer Datei aus dem Dateiinhalt bestimmt) erfolgt.</span><span class="sxs-lookup"><span data-stu-id="86634-217">The `X-Content-Type-Options` header prevents Internet Explorer from *MIME-sniffing* (determining a file's `Content-Type` from the file's content).</span></span> <span data-ttu-id="86634-218">Wenn der Server den Header `Content-Type` auf `text/html` festlegt und die Option `nosniff` festgelegt ist, rendert Internet Explorer den Inhalt unabhängig vom Dateiinhalt als `text/html`.</span><span class="sxs-lookup"><span data-stu-id="86634-218">If the server sets the `Content-Type` header to `text/html` with the `nosniff` option set, Internet Explorer renders the content as `text/html` regardless of the file's content.</span></span>

<span data-ttu-id="86634-219">So bearbeiten Sie die Datei *httpd.conf*:</span><span class="sxs-lookup"><span data-stu-id="86634-219">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="86634-220">Fügen Sie die Zeile `Header set X-Content-Type-Options "nosniff"` hinzu.</span><span class="sxs-lookup"><span data-stu-id="86634-220">Add the line `Header set X-Content-Type-Options "nosniff"`.</span></span> <span data-ttu-id="86634-221">Speichern Sie die Datei.</span><span class="sxs-lookup"><span data-stu-id="86634-221">Save the file.</span></span> <span data-ttu-id="86634-222">Starten Sie Apache neu.</span><span class="sxs-lookup"><span data-stu-id="86634-222">Restart Apache.</span></span>

### <a name="load-balancing"></a><span data-ttu-id="86634-223">Lastenausgleich</span><span class="sxs-lookup"><span data-stu-id="86634-223">Load Balancing</span></span> 

<span data-ttu-id="86634-224">Dieses Beispiel veranschaulicht das Einrichten und Konfigurieren von Apache unter CentOS 7 und Kestrel auf demselben Instanzcomputer.</span><span class="sxs-lookup"><span data-stu-id="86634-224">This example shows how to setup and configure Apache on CentOS 7 and Kestrel on the same instance machine.</span></span> <span data-ttu-id="86634-225">Damit es zu keinem Single Point of Failure kommt, ermöglicht die Verwendung von *mod_proxy_balancer* und das Ändern von **VirtualHost** das Verwalten mehrerer Instanzen der Web-Apps hinter dem Apache-Proxyserver.</span><span class="sxs-lookup"><span data-stu-id="86634-225">In order to not have a single point of failure; using *mod_proxy_balancer* and modifying the **VirtualHost** would allow for managing multiple instances of the web apps behind the Apache proxy server.</span></span>

```bash
sudo yum install mod_proxy_balancer
```

<span data-ttu-id="86634-226">In der unten dargestellten Konfigurationsdatei wird eine zusätzliche Instanz der `hellomvc`-App zur Ausführung an Port 5001 eingerichtet.</span><span class="sxs-lookup"><span data-stu-id="86634-226">In the configuration file shown below, an additional instance of the `hellomvc` app is setup to run on port 5001.</span></span> <span data-ttu-id="86634-227">Der Abschnitt *Proxy* wird mit einer Lastenausgleichskonfiguration mit zwei Mitgliedern für den Lastenausgleich von *byrequests* festgelegt.</span><span class="sxs-lookup"><span data-stu-id="86634-227">The *Proxy* section is set with a balancer configuration with two members to load balance *byrequests*.</span></span>

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

### <a name="rate-limits"></a><span data-ttu-id="86634-228">Begrenzung der Bandbreite</span><span class="sxs-lookup"><span data-stu-id="86634-228">Rate Limits</span></span>
<span data-ttu-id="86634-229">Mit dem Modul *mod_ratelimit*, das im Modul *httpd* enthalten ist, kann die Bandbreite von Clients begrenzt werden:</span><span class="sxs-lookup"><span data-stu-id="86634-229">Using *mod_ratelimit*, which is included in the *httpd* module, the bandwidth of clients can be limited:</span></span>

```bash
sudo nano /etc/httpd/conf.d/ratelimit.conf
```
<span data-ttu-id="86634-230">In der Beispieldatei wird die Bandbreite am Stammspeicherort auf 600 KB/Sek. begrenzt:</span><span class="sxs-lookup"><span data-stu-id="86634-230">The example file limits bandwidth as 600 KB/sec under the root location:</span></span>

```
<IfModule mod_ratelimit.c>
    <Location />
        SetOutputFilter RATE_LIMIT
        SetEnv rate-limit 600
    </Location>
</IfModule>
```
