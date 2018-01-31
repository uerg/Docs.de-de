---
title: Hosten von ASP.NET Core unter Windows mit IIS
author: guardrex
description: Erfahren Sie, wie ASP.NET Core-Apps in Windows Server Internet Information Services (IIS) gehostet werden.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/index
ms.openlocfilehash: 1df438af2394f41b686413cd1ce5ad73a9416ec5
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
---
# <a name="host-aspnet-core-on-windows-with-iis"></a><span data-ttu-id="21dce-103">Hosten von ASP.NET Core unter Windows mit IIS</span><span class="sxs-lookup"><span data-stu-id="21dce-103">Host ASP.NET Core on Windows with IIS</span></span>

<span data-ttu-id="21dce-104">Von [Luke Latham](https://github.com/guardrex) und [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="21dce-104">By [Luke Latham](https://github.com/guardrex) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="supported-operating-systems"></a><span data-ttu-id="21dce-105">Unterstützte Betriebssysteme</span><span class="sxs-lookup"><span data-stu-id="21dce-105">Supported operating systems</span></span>

<span data-ttu-id="21dce-106">Die folgenden Betriebssysteme werden unterstützt:</span><span class="sxs-lookup"><span data-stu-id="21dce-106">The following operating systems are supported:</span></span>

* <span data-ttu-id="21dce-107">Windows 7 und höher</span><span class="sxs-lookup"><span data-stu-id="21dce-107">Windows 7 and newer</span></span>
* <span data-ttu-id="21dce-108">Windows Server 2008 R2 und höher&#8224;</span><span class="sxs-lookup"><span data-stu-id="21dce-108">Windows Server 2008 R2 and newer&#8224;</span></span>

<span data-ttu-id="21dce-109">&#8224;Vom Konzept her gilt die in diesem Dokument beschriebene IIS-Konfiguration auch für das Hosten von ASP.NET Core-Apps mit IIS unter Nano Server, die genauen Anweisungen finden Sie jedoch unter [ASP.NET Core mit IIS unter Nano Server](xref:tutorials/nano-server).</span><span class="sxs-lookup"><span data-stu-id="21dce-109">&#8224;Conceptually, the IIS configuration described in this document also applies to hosting ASP.NET Core apps on Nano Server IIS, but refer to [ASP.NET Core with IIS on Nano Server](xref:tutorials/nano-server) for specific instructions.</span></span>

<span data-ttu-id="21dce-110">Der [HTTP.SYS-Server](xref:fundamentals/servers/httpsys) (zuvor [WebListener](xref:fundamentals/servers/weblistener) genannt) funktioniert nicht in einer Reverseproxykonfiguration mit IIS.</span><span class="sxs-lookup"><span data-stu-id="21dce-110">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)) won't work in a reverse proxy configuration with IIS.</span></span> <span data-ttu-id="21dce-111">Verwenden Sie den [Kestrel-Server](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="21dce-111">Use the [Kestrel server](xref:fundamentals/servers/kestrel).</span></span>

## <a name="iis-configuration"></a><span data-ttu-id="21dce-112">IIS-Konfiguration</span><span class="sxs-lookup"><span data-stu-id="21dce-112">IIS configuration</span></span>

<span data-ttu-id="21dce-113">Aktivieren Sie die Rolle **Webserver (IIS)**, und richten Sie Rollendienste ein.</span><span class="sxs-lookup"><span data-stu-id="21dce-113">Enable the **Web Server (IIS)** role and establish role services.</span></span>

### <a name="windows-desktop-operating-systems"></a><span data-ttu-id="21dce-114">Windows-Desktopbetriebssysteme</span><span class="sxs-lookup"><span data-stu-id="21dce-114">Windows desktop operating systems</span></span>

<span data-ttu-id="21dce-115">Navigieren Sie zu **Systemsteuerung** > **Programme** > **Programme und Features** > **Windows-Features aktivieren oder deaktivieren** (links auf dem Bildschirm).</span><span class="sxs-lookup"><span data-stu-id="21dce-115">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="21dce-116">Öffnen Sie die Gruppe für **Internetinformationsdienste** und **Webverwaltungstools**.</span><span class="sxs-lookup"><span data-stu-id="21dce-116">Open the group for **Internet Information Services** and **Web Management Tools**.</span></span> <span data-ttu-id="21dce-117">Aktivieren Sie das Kontrollkästchen für **IIS-Verwaltungskonsole**.</span><span class="sxs-lookup"><span data-stu-id="21dce-117">Check the box for **IIS Management Console**.</span></span> <span data-ttu-id="21dce-118">Aktivieren Sie das Kontrollkästchen für **WWW-Dienste**.</span><span class="sxs-lookup"><span data-stu-id="21dce-118">Check the box for **World Wide Web Services**.</span></span> <span data-ttu-id="21dce-119">Akzeptieren Sie die Standardfeatures für **WWW-Dienste**, oder passen Sie die IIS-Features an.</span><span class="sxs-lookup"><span data-stu-id="21dce-119">Accept the default features for **World Wide Web Services** or customize the IIS features.</span></span>

![Die IIS-Verwaltungskonsole und WWW-Dienste werden in Windows-Features ausgewählt.](index/_static/windows-features-win10.png)

### <a name="windows-server-operating-systems"></a><span data-ttu-id="21dce-121">Windows Server-Betriebssysteme</span><span class="sxs-lookup"><span data-stu-id="21dce-121">Windows Server operating systems</span></span>

<span data-ttu-id="21dce-122">Verwenden Sie für Serverbetriebssysteme den Assistenten **Rollen und Features hinzufügen** im Menü **Verwalten** oder den Link in **Server-Manager**.</span><span class="sxs-lookup"><span data-stu-id="21dce-122">For server operating systems, use the **Add Roles and Features** wizard via the **Manage** menu or the link in **Server Manager**.</span></span> <span data-ttu-id="21dce-123">Aktivieren Sie im Schritt **Serverrollen** das Kontrollkästchen für **Webserver (IIS)**.</span><span class="sxs-lookup"><span data-stu-id="21dce-123">On the **Server Roles** step, check the box for **Web Server (IIS)**.</span></span>

![Die Rolle „Webserver (IIS)“ wird im Schritt „Serverrollen auswählen“ ausgewählt.](index/_static/server-roles-ws2016.png)

<span data-ttu-id="21dce-125">Wählen Sie im Schritt **Rollendienste** die gewünschten IIS-Rollendienste aus, oder übernehmen Sie die bereitgestellten Standardrollendienste.</span><span class="sxs-lookup"><span data-stu-id="21dce-125">On the **Role services** step, select the IIS role services desired or accept the default role services provided.</span></span>

![Die Standardrollendienste werden im Schritt „Rollendienste auswählen“ ausgewählt.](index/_static/role-services-ws2016.png)

<span data-ttu-id="21dce-127">Fahren Sie mit dem Schritt **Bestätigung** fort, um die Webserverrolle und die Dienste zu installieren.</span><span class="sxs-lookup"><span data-stu-id="21dce-127">Proceed through the **Confirmation** step to install the web server role and services.</span></span> <span data-ttu-id="21dce-128">Ein Server-/IIS-Neustart ist nach der Installation der Rolle „Webserver (IIS)“ nicht erforderlich.</span><span class="sxs-lookup"><span data-stu-id="21dce-128">A server/IIS restart isn't required after installing the Web Server (IIS) role.</span></span>

## <a name="install-the-net-core-windows-server-hosting-bundle"></a><span data-ttu-id="21dce-129">Installieren des Pakets „.NET Core Windows Server Hosting“</span><span class="sxs-lookup"><span data-stu-id="21dce-129">Install the .NET Core Windows Server Hosting bundle</span></span>

1. <span data-ttu-id="21dce-130">Installieren Sie das [Paket „.NET Core Windows Server Hosting“](https://aka.ms/dotnetcore-2-windowshosting) im Hostsystem.</span><span class="sxs-lookup"><span data-stu-id="21dce-130">Install the [.NET Core Windows Server Hosting bundle](https://aka.ms/dotnetcore-2-windowshosting) on the hosting system.</span></span> <span data-ttu-id="21dce-131">Das Paket installiert die .NET Core-Runtime, die .NET Core-Bibliothek und das [ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="21dce-131">The bundle installs the .NET Core Runtime, .NET Core Library, and the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="21dce-132">Das Modul erstellt den Reverseproxy zwischen IIS und dem Kestrel-Server.</span><span class="sxs-lookup"><span data-stu-id="21dce-132">The module creates the reverse proxy between IIS and the Kestrel server.</span></span> <span data-ttu-id="21dce-133">Wenn das System nicht über eine Internetverbindung verfügt, beziehen und installieren Sie [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840), bevor Sie das Paket „.NET Core Windows Server Hosting“ installieren.</span><span class="sxs-lookup"><span data-stu-id="21dce-133">If the system doesn't have an Internet connection, obtain and install the [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) before installing the .NET Core Windows Server Hosting bundle.</span></span>

2. <span data-ttu-id="21dce-134">Starten Sie das System neu, oder führen Sie **net stop was /y** gefolgt von **net start w3svc** über eine Eingabeaufforderung aus, um eine Änderung am Systempfad zu übernehmen.</span><span class="sxs-lookup"><span data-stu-id="21dce-134">Restart the system or execute **net stop was /y** followed by **net start w3svc** from a command prompt to pick up a change to the system PATH.</span></span>

> [!NOTE]
> <span data-ttu-id="21dce-135">Informationen zur Verwendung einer IIS-Freigabekonfiguration finden Sie unter [ASP.NET Core-Modul mit IIS-Freigabekonfiguration](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span><span class="sxs-lookup"><span data-stu-id="21dce-135">For information on IIS Shared Configuration, see [ASP.NET Core Module with IIS Shared Configuration](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span></span>

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a><span data-ttu-id="21dce-136">Installieren von Web Deploy beim Veröffentlichen mit Visual Studio</span><span class="sxs-lookup"><span data-stu-id="21dce-136">Install Web Deploy when publishing with Visual Studio</span></span>

<span data-ttu-id="21dce-137">Wenn Sie Apps auf Servern mit [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) bereitstellen, installieren Sie die neueste Version von Web Deploy auf dem Server.</span><span class="sxs-lookup"><span data-stu-id="21dce-137">When deploying apps to servers with [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy), install the latest version of Web Deploy on the server.</span></span> <span data-ttu-id="21dce-138">Um Web Deploy zu installieren, verwenden Sie den [Webplattform-Installer (Web PI)](https://www.microsoft.com/web/downloads/platform.aspx) oder rufen einen Installer direkt aus dem [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717) ab.</span><span class="sxs-lookup"><span data-stu-id="21dce-138">To install Web Deploy, use the [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) or obtain an installer directly from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717).</span></span> <span data-ttu-id="21dce-139">Die bevorzugte Methode ist die Verwendung von WebPI.</span><span class="sxs-lookup"><span data-stu-id="21dce-139">The preferred method is to use WebPI.</span></span> <span data-ttu-id="21dce-140">WebPI bietet ein eigenständiges Setup und eine Konfiguration für Hostinganbieter.</span><span class="sxs-lookup"><span data-stu-id="21dce-140">WebPI offers a standalone setup and a configuration for hosting providers.</span></span>

## <a name="application-configuration"></a><span data-ttu-id="21dce-141">Anwendungskonfiguration</span><span class="sxs-lookup"><span data-stu-id="21dce-141">Application configuration</span></span>

### <a name="enabling-the-iisintegration-components"></a><span data-ttu-id="21dce-142">Aktivieren der IISIntegration-Komponenten</span><span class="sxs-lookup"><span data-stu-id="21dce-142">Enabling the IISIntegration components</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="21dce-143">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="21dce-143">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="21dce-144">Eine typische *Program.cs*-Datei ruft [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) auf, um die Einrichtung eines Hosts zu starten.</span><span class="sxs-lookup"><span data-stu-id="21dce-144">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to begin setting up a host.</span></span> <span data-ttu-id="21dce-145">`CreateDefaultBuilder` konfiguriert [Kestrel](xref:fundamentals/servers/kestrel) als Webserver und aktiviert die IIS-Integration durch Konfigurierung des Basispfads und Ports für das [ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module):</span><span class="sxs-lookup"><span data-stu-id="21dce-145">`CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and enables IIS integration by configuring the base path and port for the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module):</span></span>

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="21dce-146">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="21dce-146">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="21dce-147">Nehmen Sie eine Abhängigkeit vom Paket [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) in die App-Abhängigkeiten auf.</span><span class="sxs-lookup"><span data-stu-id="21dce-147">Include a dependency on the [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) package in the app's dependencies.</span></span> <span data-ttu-id="21dce-148">Binden Sie Middleware für die Integration von IIS in die App ein, indem Sie die Erweiterungsmethode *UseIISIntegration* zu *WebHostBuilder* hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="21dce-148">Incorporate IIS Integration middleware into the app by adding the *UseIISIntegration* extension method to *WebHostBuilder*:</span></span>

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .UseIISIntegration()
    ...
```

<span data-ttu-id="21dce-149">Es sind jeweils `UseKestrel` und `UseIISIntegration` erforderlich.</span><span class="sxs-lookup"><span data-stu-id="21dce-149">Both `UseKestrel` and `UseIISIntegration` are required.</span></span> <span data-ttu-id="21dce-150">Ein Code, der `UseIISIntegration` aufruft, hat keinen Einfluss auf die Codeportabilität.</span><span class="sxs-lookup"><span data-stu-id="21dce-150">Code calling `UseIISIntegration` doesn't affect code portability.</span></span> <span data-ttu-id="21dce-151">Wenn Die App nicht hinter IIS ausgeführt wird (die App wird z.B. direkt auf Kestrel ausgeführt), hat `UseIISIntegration` keine Funktion.</span><span class="sxs-lookup"><span data-stu-id="21dce-151">If the app isn't run behind IIS (for example, the app is run directly on Kestrel), `UseIISIntegration` no-ops.</span></span>

---

<span data-ttu-id="21dce-152">Weitere Informationen zum Hosten finden Sie unter [Hosting in ASP.NET Core (Hosten in ASP.NET Core)](xref:fundamentals/hosting).</span><span class="sxs-lookup"><span data-stu-id="21dce-152">For more information on hosting, see [Hosting in ASP.NET Core](xref:fundamentals/hosting).</span></span>

### <a name="iis-options"></a><span data-ttu-id="21dce-153">IIS-Optionen</span><span class="sxs-lookup"><span data-stu-id="21dce-153">IIS options</span></span>

<span data-ttu-id="21dce-154">Schließen Sie zur Konfiguration von IIS-Optionen eine Dienstkonfiguration für `IISOptions` in `ConfigureServices` ein:</span><span class="sxs-lookup"><span data-stu-id="21dce-154">To configure IIS options, include a service configuration for `IISOptions` in `ConfigureServices`:</span></span>

```csharp
services.Configure<IISOptions>(options => 
{
    ...
});
```

| <span data-ttu-id="21dce-155">Option</span><span class="sxs-lookup"><span data-stu-id="21dce-155">Option</span></span>                         | <span data-ttu-id="21dce-156">Standard</span><span class="sxs-lookup"><span data-stu-id="21dce-156">Default</span></span> | <span data-ttu-id="21dce-157">Einstellung</span><span class="sxs-lookup"><span data-stu-id="21dce-157">Setting</span></span> |
| ------------------------------ | ------- | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="21dce-158">Wenn `true`, dann legt die Authentifizierungsmiddleware `HttpContext.User` fest und reagiert auf generische Herausforderungen.</span><span class="sxs-lookup"><span data-stu-id="21dce-158">If `true`, the authentication middleware sets the `HttpContext.User` and responds to generic challenges.</span></span> <span data-ttu-id="21dce-159">Wenn `false`, dann stellt die Authentifizierungsmiddleware nur eine Identität bereit (`HttpContext.User`) und reagiert nur dann auf Herausforderungen, wenn sie dazu explizit von `AuthenticationScheme` angewiesen wird.</span><span class="sxs-lookup"><span data-stu-id="21dce-159">If `false`, the authentication middleware only provides an identity (`HttpContext.User`) and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="21dce-160">Die Windows-Authentifizierung muss in IIS aktiviert sein, damit `AutomaticAuthentication` funktioniert.</span><span class="sxs-lookup"><span data-stu-id="21dce-160">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="21dce-161">Legt den Anzeigename fest, der Benutzern auf Anmeldungsseiten angezeigt wird</span><span class="sxs-lookup"><span data-stu-id="21dce-161">Sets the display name shown to users on login pages.</span></span> |
| `ForwardClientCertificate`     | `true`  | <span data-ttu-id="21dce-162">Wenn diese Option `true` ist und der Anforderungsheader `MS-ASPNETCORE-CLIENTCERT` vorhanden ist, wird das `HttpContext.Connection.ClientCertificate` aufgefüllt.</span><span class="sxs-lookup"><span data-stu-id="21dce-162">If `true` and the `MS-ASPNETCORE-CLIENTCERT` request header is present, the `HttpContext.Connection.ClientCertificate` is populated.</span></span> |

### <a name="webconfig"></a><span data-ttu-id="21dce-163">web.config</span><span class="sxs-lookup"><span data-stu-id="21dce-163">web.config</span></span>

<span data-ttu-id="21dce-164">Die Datei *Web.config* dient in erster Linie der Konfiguration des [ASP.NET Core-Moduls](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="21dce-164">The *web.config* file's primary purpose is to configure the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="21dce-165">Sie kann optional zusätzliche IIS-Konfigurationseinstellungen bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="21dce-165">It may optionally provide additional IIS configuration settings.</span></span> <span data-ttu-id="21dce-166">Das Erstellen, das Transformieren und die Veröffentlichung von *web.config* erfolgt durch das .NET Core Web SDK (`Microsoft.NET.Sdk.Web`).</span><span class="sxs-lookup"><span data-stu-id="21dce-166">Creating, transforming, and publishing *web.config* is handled by the .NET Core Web SDK (`Microsoft.NET.Sdk.Web`).</span></span> <span data-ttu-id="21dce-167">Das SDK wird am Anfang der Projektdatei (`<Project Sdk="Microsoft.NET.Sdk.Web">`) festgelegt.</span><span class="sxs-lookup"><span data-stu-id="21dce-167">The SDK is set  at the top of the project file, `<Project Sdk="Microsoft.NET.Sdk.Web">`.</span></span> <span data-ttu-id="21dce-168">Um zu verhindern, dass das SDK die Datei *web.config* transformiert, fügen Sie der Projektdatei die Eigenschaft **\<IsTransformWebConfigDisabled** mit der Einstellung `true` hinzu:</span><span class="sxs-lookup"><span data-stu-id="21dce-168">To prevent the SDK from transforming the *web.config* file, add the **\<IsTransformWebConfigDisabled>** property to the project file with a setting of `true`:</span></span>

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="21dce-169">Wenn eine Datei namens *Web.config* im Projekt vorhanden ist, wird sie zur Konfiguration des [ASP.NET Core-Moduls](xref:fundamentals/servers/aspnet-core-module) mit dem richtigen *processPath*- und *arguments*-Attribut transformiert und in die [veröffentlichte Ausgabe](xref:host-and-deploy/directory-structure) verschoben.</span><span class="sxs-lookup"><span data-stu-id="21dce-169">If a *web.config* file is in the project, it's transformed with the correct *processPath* and *arguments* to configure the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and moved to [published output](xref:host-and-deploy/directory-structure).</span></span> <span data-ttu-id="21dce-170">Die Transformation ändert die IIS-Konfigurationseinstellungen in der Datei nicht.</span><span class="sxs-lookup"><span data-stu-id="21dce-170">The transformation doesn't modify IIS configuration settings in the file.</span></span>

### <a name="webconfig-location"></a><span data-ttu-id="21dce-171">Speicherort der Datei „Web.config“</span><span class="sxs-lookup"><span data-stu-id="21dce-171">web.config location</span></span>

<span data-ttu-id="21dce-172">.NET Core-Apps werden über einen Reverseproxy zwischen IIS und dem Kestrel-Server gehostet.</span><span class="sxs-lookup"><span data-stu-id="21dce-172">.NET Core apps are hosted via a reverse proxy between IIS and the Kestrel server.</span></span> <span data-ttu-id="21dce-173">Zum Erstellen des Reverseproxys muss die Datei *Web.config* im Stammpfad des Inhalts (normalerweise dem App-Basispfad) der bereitgestellten App vorhanden sein. Dies ist der physische Pfad der Website, der in IIS bereitgestellt wurde.</span><span class="sxs-lookup"><span data-stu-id="21dce-173">In order to create the reverse proxy, the *web.config* file must be present at the content root path (typically the app base path) of the deployed app, which is the website physical path provided to IIS.</span></span> <span data-ttu-id="21dce-174">Die Datei *Web.config* wird im Stammverzeichnis der App benötigt, um die Veröffentlichung mehrerer Apps mit Web Deploy zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="21dce-174">The *web.config* file is required at the root of the app to enable the publishing of multiple apps using Web Deploy.</span></span>

<span data-ttu-id="21dce-175">Im physischen Pfad und den Unterordnern der App sind vertrauliche Dateien vorhanden, wie z.B. *\<assembly_name>.runtimeconfig.json*, *\<assembly_name>.xml* (XML-Dokumentationskommentare) und *\<assembly_name>.deps.json*.</span><span class="sxs-lookup"><span data-stu-id="21dce-175">Sensitive files exist on the app's physical path, including subfolders, such as *\<assembly_name>.runtimeconfig.json*, *\<assembly_name>.xml* (XML Documentation comments), and *\<assembly_name>.deps.json*.</span></span> <span data-ttu-id="21dce-176">Wenn die Datei *Web.config* vorhanden ist und die Website konfiguriert, verhindert IIS, dass diese vertraulichen Dateien verarbeitet werden.</span><span class="sxs-lookup"><span data-stu-id="21dce-176">When the *web.config* file is present and configures the site, IIS prevents these sensitive files from being served.</span></span> <span data-ttu-id="21dce-177">**Daher ist es wichtig, dass die Datei *web.config* nicht versehentlich umbenannt oder aus der Bereitstellung entfernt wird.**</span><span class="sxs-lookup"><span data-stu-id="21dce-177">**Therefore, it's important that the *web.config* file isn't accidently renamed or removed from the deployment.**</span></span>

## <a name="create-the-iis-website"></a><span data-ttu-id="21dce-178">Erstellen der IIS-Website</span><span class="sxs-lookup"><span data-stu-id="21dce-178">Create the IIS Website</span></span>

1. <span data-ttu-id="21dce-179">Erstellen Sie im IIS-Zielsystem einen Ordner für die veröffentlichten Ordner und Dateien der App, die in [Verzeichnisstruktur](xref:host-and-deploy/directory-structure) beschrieben sind.</span><span class="sxs-lookup"><span data-stu-id="21dce-179">On the target IIS system, create a folder to contain the app's published folders and files, which are described in [Directory Structure](xref:host-and-deploy/directory-structure).</span></span>

2. <span data-ttu-id="21dce-180">Erstellen Sie im Ordner einen Ordner *Protokolle*, um darin StdOut-Protokolle zu speichern, wenn die StdOut-Protokollierung aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="21dce-180">Within the folder, create a *logs* folder to hold stdout logs when stdout logging is enabled.</span></span> <span data-ttu-id="21dce-181">Wenn die App mit einem Ordner *Protokolle* in der Payload bereitgestellt wird, können Sie diesen Schritt überspringen.</span><span class="sxs-lookup"><span data-stu-id="21dce-181">If the app is deployed with a *logs* folder in the payload, skip this step.</span></span> <span data-ttu-id="21dce-182">Im Artikel zur *Verzeichnisstruktur* wird erläutert, wie Sie MSBuild dazu veranlassen, den Ordner [Protokolle](xref:host-and-deploy/directory-structure) zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="21dce-182">For instructions on how to make MSBuild create the *logs* folder, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>

3. <span data-ttu-id="21dce-183">Erstellen Sie im **IIS-Manager** eine neue Website.</span><span class="sxs-lookup"><span data-stu-id="21dce-183">In **IIS Manager**, create a new website.</span></span> <span data-ttu-id="21dce-184">Geben Sie einen **Websitenamen** an, und legen Sie den **physischen Pfad** auf den Bereitstellungsordner der App fest.</span><span class="sxs-lookup"><span data-stu-id="21dce-184">Provide a **Site name** and set the **Physical path** to the app's deployment folder.</span></span> <span data-ttu-id="21dce-185">Geben Sie die Konfiguration unter **Bindung** an, und erstellen Sie die Website.</span><span class="sxs-lookup"><span data-stu-id="21dce-185">Provide the **Binding** configuration and create the website.</span></span>

4. <span data-ttu-id="21dce-186">Legen Sie den Anwendungspool auf **Kein verwalteter Code** fest.</span><span class="sxs-lookup"><span data-stu-id="21dce-186">Set the application pool to **No Managed Code**.</span></span> <span data-ttu-id="21dce-187">ASP.NET Core wird in einem separaten Prozess ausgeführt und verwaltet die Runtime.</span><span class="sxs-lookup"><span data-stu-id="21dce-187">ASP.NET Core runs in a separate process and manages the runtime.</span></span>

5. <span data-ttu-id="21dce-188">Öffnen Sie das Fenster **Website hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="21dce-188">Open the **Add Website** window.</span></span>

   ![Klicken Sie im Kontextmenü „Websites“ auf „Website hinzufügen“.](index/_static/add-website-context-menu-ws2016.png)

6. <span data-ttu-id="21dce-190">Konfigurieren Sie die Website.</span><span class="sxs-lookup"><span data-stu-id="21dce-190">Configure the website.</span></span>

   ![Geben Sie den Websitenamen, den physischen Pfad und den Hostnamen im Schritt „Website hinzufügen“ an.](index/_static/add-website-ws2016.png)

7. <span data-ttu-id="21dce-192">Öffnen Sie im Bereich **Anwendungspools** das Fenster **Anwendungspool bearbeiten**, indem Sie mit der rechten Maustaste auf den Anwendungspool der Website klicken und im Popupmenü **Grundeinstellungen** auswählen.</span><span class="sxs-lookup"><span data-stu-id="21dce-192">In the **Application Pools** panel, open the **Edit Application Pool** window by right-clicking on the website's app pool and selecting **Basic Settings...** from the popup menu.</span></span>

   ![Wählen Sie „Grundeinstellungen“ aus dem Kontextmenü des Anwendungspools aus.](index/_static/apppools-basic-settings-ws2016.png)

8. <span data-ttu-id="21dce-194">Legen Sie die **.NET CLR-Version** auf **Kein verwalteter Code** fest.</span><span class="sxs-lookup"><span data-stu-id="21dce-194">Set the **.NET CLR version** to **No Managed Code**.</span></span>

   ![Legen Sie „Kein verwalteter Code“ für die .NET CLR-Version fest.](index/_static/edit-apppool-ws2016.png)
     
    <span data-ttu-id="21dce-196">Hinweis: Das Festlegen der **.NET CLR-Version** auf **Kein verwalteter Code** ist optional.</span><span class="sxs-lookup"><span data-stu-id="21dce-196">Note: Setting the **.NET CLR version** to **No Managed Code** is optional.</span></span> <span data-ttu-id="21dce-197">Für ASP.NET Core ist das Laden der Desktop-CLR nicht erforderlich.</span><span class="sxs-lookup"><span data-stu-id="21dce-197">ASP.NET Core doesn't rely on loading the desktop CLR.</span></span>

9. <span data-ttu-id="21dce-198">Vergewissern Sie sich, dass die Prozessmodellidentität über die richtigen Berechtigungen verfügt.</span><span class="sxs-lookup"><span data-stu-id="21dce-198">Confirm the process model identity has the proper permissions.</span></span>

   <span data-ttu-id="21dce-199">Wenn die Standardidentität des App-Pools (**Prozessmodell** > **Identität**) von **ApplicationPoolIdentity** in eine andere Identität geändert wird, stellen Sie sicher, dass die neue Identität über die erforderlichen Berechtigungen zum Zugriff auf den Ordner, die Datenbank und andere erforderliche Ressourcen der App verfügt.</span><span class="sxs-lookup"><span data-stu-id="21dce-199">If the default identity of the app pool (**Process Model** > **Identity**) is changed from **ApplicationPoolIdentity** to another identity, verify that the new identity has the required permissions to access the app's folder, database, and other required resources.</span></span>
   
## <a name="deploy-the-app"></a><span data-ttu-id="21dce-200">Bereitstellen der App</span><span class="sxs-lookup"><span data-stu-id="21dce-200">Deploy the app</span></span>

<span data-ttu-id="21dce-201">Stellen Sie die App in dem Ordner bereit, den Sie im IIS-Zielsystem erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="21dce-201">Deploy the app to the folder created on the target IIS system.</span></span> <span data-ttu-id="21dce-202">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) ist die empfohlene Methode zur Bereitstellung.</span><span class="sxs-lookup"><span data-stu-id="21dce-202">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) is the recommended mechanism for deployment.</span></span>

<span data-ttu-id="21dce-203">Stellen Sie sicher, dass die zur Bereitstellung veröffentlichte App nicht ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="21dce-203">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="21dce-204">Die Dateien im Ordner *publish* sind gesperrt, wenn die App ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="21dce-204">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="21dce-205">Gesperrte Dateien können nicht überschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="21dce-205">Locked files can't be overwritten.</span></span> <span data-ttu-id="21dce-206">Um gesperrte Dateien in einer Bereitstellung freizugeben, beenden Sie den App-Pool mit einer der folgenden Methoden:</span><span class="sxs-lookup"><span data-stu-id="21dce-206">To release locked files in a deployment, stop the app pool:</span></span>

* <span data-ttu-id="21dce-207">Durch manuelles Beenden im IIS-Manager auf dem Server.</span><span class="sxs-lookup"><span data-stu-id="21dce-207">Manually in the IIS Manager on the server.</span></span>
* <span data-ttu-id="21dce-208">Durch Verwenden von Web Deploy und Verweisen auf `Microsoft.NET.Sdk.Web` in der Projektdatei.</span><span class="sxs-lookup"><span data-stu-id="21dce-208">Using Web Deploy and referencing `Microsoft.NET.Sdk.Web` in the project file.</span></span> <span data-ttu-id="21dce-209">Eine Datei namens *app_offline.htm* befindet sich im Stammverzeichnis des Web-App-Verzeichnisses.</span><span class="sxs-lookup"><span data-stu-id="21dce-209">An *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="21dce-210">Wenn die Datei vorhanden ist, fährt das ASP.NET Core Module die App ordnungsgemäß herunter und verarbeitet die Datei *app_offline.htm* während der Bereitstellung.</span><span class="sxs-lookup"><span data-stu-id="21dce-210">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="21dce-211">Weitere Informationen finden Sie unter [Konfigurationsreferenz für das ASP.NET Core-Modul](xref:host-and-deploy/aspnet-core-module#appofflinehtm).</span><span class="sxs-lookup"><span data-stu-id="21dce-211">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#appofflinehtm).</span></span>
* <span data-ttu-id="21dce-212">Verwenden Sie PowerShell, um den App-Pool zu beenden und neu zu starten (erfordert PowerShell 5 oder höher):</span><span class="sxs-lookup"><span data-stu-id="21dce-212">Use PowerShell to stop and restart the app pool (requires PowerShell 5 or later):</span></span>
  ```PowerShell
  $webAppPoolName = 'APP_POOL_NAME'

  # Stop the AppPool
  if((Get-WebAppPoolState $webAppPoolName).Value -ne 'Stopped') {
      Stop-WebAppPool -Name $webAppPoolName
      while((Get-WebAppPoolState $webAppPoolName).Value -ne 'Stopped') {
          Start-Sleep -s 1
      }
      Write-Host `-AppPool Stopped
  }

  # Provide script commands here to deploy the app

  # Restart the AppPool
  if((Get-WebAppPoolState $webAppPoolName).Value -ne 'Started') {
      Start-WebAppPool -Name $webAppPoolName
      while((Get-WebAppPoolState $webAppPoolName).Value -ne 'Started') {
          Start-Sleep -s 1
      }
      Write-Host `-AppPool Started
  }
  ```

### <a name="web-deploy-with-visual-studio"></a><span data-ttu-id="21dce-213">Web Deploy mit Visual Studio</span><span class="sxs-lookup"><span data-stu-id="21dce-213">Web Deploy with Visual Studio</span></span>

<span data-ttu-id="21dce-214">Informationen zum Erstellen eines Veröffentlichungsprofils zur Verwendung mit Web Deploy finden Sie im Thema [Visual Studio publish profiles for ASP.NET Core app deployment (Visual Studio-Veröffentlichungsprofile zum Bereitstellen von ASP.NET Core-Apps)](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles).</span><span class="sxs-lookup"><span data-stu-id="21dce-214">See the [Visual Studio publish profiles for ASP.NET Core app deployment](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) topic to learn how to create a publish profile for use with Web Deploy.</span></span> <span data-ttu-id="21dce-215">Wenn der Hostinganbieter ein Veröffentlichungsprofil oder Unterstützung für das Erstellen eines solchen Profils bereitstellt, laden Sie dessen Profil herunter, und importieren Sie es mithilfe des Visual Studio-Dialogfelds **Veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="21dce-215">If the hosting provider supplies a Publish Profile or support for creating one, download their profile and import it using the Visual Studio **Publish** dialog.</span></span>

![Dialogfeld „Veröffentlichen“](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a><span data-ttu-id="21dce-217">Web Deploy außerhalb von Visual Studio</span><span class="sxs-lookup"><span data-stu-id="21dce-217">Web Deploy outside of Visual Studio</span></span>

<span data-ttu-id="21dce-218">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) kann über die Befehlszeile auch außerhalb von Visual Studio verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="21dce-218">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) can also be used outside of Visual Studio from the command line.</span></span> <span data-ttu-id="21dce-219">Weitere Informationen finden Sie unter [Web Deployment Tool](/iis/publish/using-web-deploy/use-the-web-deployment-tool) (Webbereitstellungstool).</span><span class="sxs-lookup"><span data-stu-id="21dce-219">For more information, see [Web Deployment Tool](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span></span>

### <a name="alternatives-to-web-deploy"></a><span data-ttu-id="21dce-220">Alternativen zu Web Deploy</span><span class="sxs-lookup"><span data-stu-id="21dce-220">Alternatives to Web Deploy</span></span>

<span data-ttu-id="21dce-221">Verwenden Sie eine der folgenden Methoden, um die App zum Hostingsystem zu migrieren, z.B. XCopy, Robocopy oder PowerShell.</span><span class="sxs-lookup"><span data-stu-id="21dce-221">Use any of several methods to move the app to the hosting system, such as Xcopy, Robocopy, or PowerShell.</span></span> <span data-ttu-id="21dce-222">Visual Studio-Benutzer können die [Veröffentlichungsbeispiele](https://github.com/aspnet/vsweb-publish/blob/master/samples/samples.md) verwenden.</span><span class="sxs-lookup"><span data-stu-id="21dce-222">Visual Studio users may use the [Publish Samples](https://github.com/aspnet/vsweb-publish/blob/master/samples/samples.md).</span></span>

## <a name="browse-the-website"></a><span data-ttu-id="21dce-223">Navigieren auf der Website</span><span class="sxs-lookup"><span data-stu-id="21dce-223">Browse the website</span></span>

![Der Microsoft Edge-Browser hat die IIS-Startseite geladen.](index/_static/browsewebsite.png)

## <a name="data-protection"></a><span data-ttu-id="21dce-225">Schutz von Daten</span><span class="sxs-lookup"><span data-stu-id="21dce-225">Data protection</span></span>

<span data-ttu-id="21dce-226">Der Schutz von Daten wird von mehreren ASP.NET-Middlewarefunktionen genutzt, einschließlich der Middleware, die bei der Authentifizierung verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="21dce-226">Data Protection is used by several ASP.NET middlewares, including those used in authentication.</span></span> <span data-ttu-id="21dce-227">Selbst wenn die Datenschutz-APIs nicht aus dem Code des Benutzers aufgerufen werden, sollte der Schutz von Daten für die Erstellung eines beständigen Schlüsselspeichers mit einem Bereitstellungsskript oder im Benutzercode konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="21dce-227">Even if Data Protection APIs aren't called from user's code, Data Protection should be configured with a deployment script or in user code to create a persistent key store.</span></span> <span data-ttu-id="21dce-228">Wenn der Schutz von Daten nicht konfiguriert ist, werden die Schlüssel beim Neustarten der App im Arbeitsspeicher gespeichert und verworfen.</span><span class="sxs-lookup"><span data-stu-id="21dce-228">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="21dce-229">Falls der Schlüsselbund im Arbeitsspeicher gespeichert wird, wenn die App neu gestartet wird:</span><span class="sxs-lookup"><span data-stu-id="21dce-229">If the keyring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="21dce-230">Werden alle Formularauthentifizierungstoken als ungültig ausgewiesen.</span><span class="sxs-lookup"><span data-stu-id="21dce-230">All forms authentication tokens are invalidated.</span></span> 
* <span data-ttu-id="21dce-231">Benutzer müssen sich bei ihrer nächsten Anforderung erneut anmelden.</span><span class="sxs-lookup"><span data-stu-id="21dce-231">Users are required to sign in again on their next request.</span></span> 
* <span data-ttu-id="21dce-232">Alle mit dem Schlüsselbund geschützte Daten können nicht mehr entschlüsselt werden.</span><span class="sxs-lookup"><span data-stu-id="21dce-232">Any data protected with the keyring can no longer be decrypted.</span></span>

<span data-ttu-id="21dce-233">Zum Konfigurieren des Schutzes von Daten unter IIS verwenden Sie **einen** der folgenden Ansätze:</span><span class="sxs-lookup"><span data-stu-id="21dce-233">To configure Data Protection under IIS, use **one** of the following approaches:</span></span>

### <a name="create-a-data-protection-registry-hive"></a><span data-ttu-id="21dce-234">Erstellen einer Registrierungsstruktur für den Schutz von Daten</span><span class="sxs-lookup"><span data-stu-id="21dce-234">Create a Data Protection Registry Hive</span></span>

<span data-ttu-id="21dce-235">Schlüssel für den Schutz von Daten, die von ASP.NET-Apps verwendet werden, werden in Registrierungsstrukturen außerhalb der Apps gespeichert.</span><span class="sxs-lookup"><span data-stu-id="21dce-235">Data Protection keys used by ASP.NET apps are stored in registry hives external to the apps.</span></span> <span data-ttu-id="21dce-236">Um die Schlüssel für eine bestimmte App beizubehalten, müssen Sie eine Registrierungsstruktur für den App-Pool erstellen.</span><span class="sxs-lookup"><span data-stu-id="21dce-236">To persist the keys for a given app, create a registry hive for the app pool.</span></span>

<span data-ttu-id="21dce-237">Bei eigenständigen IIS-Installationen, die ohne Webfarm vorgesehen sind, kann das [PowerShell-Skript „Provision-AutoGenKeys.ps1“ für den Schutz von Daten](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) für jeden App-Pool genutzt werden, das mit einer ASP.NET Core-App verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="21dce-237">For standalone, non-webfarm IIS installations, the [Data Protection Provision-AutoGenKeys.ps1 PowerShell script](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) can be used for each app pool used with an ASP.NET Core app.</span></span> <span data-ttu-id="21dce-238">Dieses Skript erstellt einen besonderen Registrierungsschlüssel in der HKLM-Registrierung, der nur in der ACL des Workerprozesskontos bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="21dce-238">This script creates a special registry key in the HKLM registry that's ACLed only to the worker process account.</span></span> <span data-ttu-id="21dce-239">Schlüssel werden in ruhendem Zustand mit DPAPI mit einem computerweiten Schlüssel verschlüsselt.</span><span class="sxs-lookup"><span data-stu-id="21dce-239">Keys are encrypted at rest using DPAPI with a machine-wide key.</span></span>

<span data-ttu-id="21dce-240">In Webfarmszenarios kann eine App so konfiguriert werden, dass sie einen UNC-Pfad verwendet, um den Schlüsselbund für den Schutz von Daten zu speichern.</span><span class="sxs-lookup"><span data-stu-id="21dce-240">In web farm scenarios, an app can be configured to use a UNC path to store its data protection keyring.</span></span> <span data-ttu-id="21dce-241">Standardmäßig werden die Schlüssel für den Schutz von Daten nicht verschlüsselt.</span><span class="sxs-lookup"><span data-stu-id="21dce-241">By default, the data protection keys are not encrypted.</span></span> <span data-ttu-id="21dce-242">Stellen Sie sicher, dass die Dateiberechtigungen für eine solche Freigabe auf das Windows-Konto beschränkt sind, mit dem die App ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="21dce-242">Ensure that the file permissions for such a share are limited to the Windows account the app runs as.</span></span> <span data-ttu-id="21dce-243">Darüber hinaus kann ein X.509-Zertifikat zum Schützen von Schlüsseln im ruhenden Zustand verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="21dce-243">In addition, an X509 certificate can be used to protect keys at rest.</span></span> <span data-ttu-id="21dce-244">Ziehen Sie einen Mechanismus in Erwägung, um es Benutzern zu ermöglichen, Zertifikate hochzuladen: Platzieren Sie Zertifikate im Zertifikatspeicher des Benutzers für vertrauenswürdige Anbieter, und stellen Sie sicher, dass sie auf allen Computern verfügbar sind, auf denen die App des Benutzers ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="21dce-244">Consider a mechanism to allow users to upload certificates: Place certificates into the user's trusted certificate store and ensure they're available on all machines where the user's app runs.</span></span> <span data-ttu-id="21dce-245">Details finden Sie unter [Konfigurieren des Schutzes von Daten](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="21dce-245">See [Configuring Data Protection](xref:security/data-protection/configuration/overview) for details.</span></span>

### <a name="configure-the-iis-application-pool-to-load-the-user-profile"></a><span data-ttu-id="21dce-246">Konfigurieren des IIS-Anwendungspools zum Laden des Benutzerprofils</span><span class="sxs-lookup"><span data-stu-id="21dce-246">Configure the IIS Application Pool to load the user profile</span></span>

<span data-ttu-id="21dce-247">Diese Einstellung befindet sich im Abschnitt **Prozessmodell** unter **Erweiterte Einstellungen** für den App-Pool.</span><span class="sxs-lookup"><span data-stu-id="21dce-247">This setting is in the **Process Model** section under the **Advanced Settings** for the app pool.</span></span> <span data-ttu-id="21dce-248">Legen Sie „Benutzerprofil laden“ auf `True` fest.</span><span class="sxs-lookup"><span data-stu-id="21dce-248">Set Load User Profile to `True`.</span></span> <span data-ttu-id="21dce-249">Dadurch werden Schlüssel im Benutzerprofilverzeichnis gespeichert und mit DPAPI mit einem Schlüssel geschützt, der nur für das Benutzerkonto gilt, das für den App-Pool verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="21dce-249">This stores keys under the user profile directory and protects them using DPAPI with a key specific to the user account used for the app pool.</span></span>

### <a name="use-the-file-system-as-a-key-ring-store"></a><span data-ttu-id="21dce-250">Verwenden des Dateisystems als Schlüsselbundspeicher</span><span class="sxs-lookup"><span data-stu-id="21dce-250">Use the file system as a key ring store</span></span>

<span data-ttu-id="21dce-251">Passen Sie den App-Code so an, dass er [das Dateisystem als Schlüsselbundspeicher verwendet](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="21dce-251">Adjust the app code to [use the file system as a key ring store](xref:security/data-protection/configuration/overview).</span></span> <span data-ttu-id="21dce-252">Verwenden Sie ein X.509-Zertifikat, um den Schlüsselbund zu schützen, und stellen Sie sicher, dass es sich bei dem Zertifikat um ein vertrauenswürdiges Zertifikat handelt.</span><span class="sxs-lookup"><span data-stu-id="21dce-252">Use an X509 certificate to protect the keyring and ensure the certificate is a trusted certificate.</span></span> <span data-ttu-id="21dce-253">Ein selbstsigniertes Zertifikat müssen Sie im vertrauenswürdigen Stammspeicher platzieren.</span><span class="sxs-lookup"><span data-stu-id="21dce-253">If it's a self signed certificate, place the certificate in the Trusted Root store.</span></span>

<span data-ttu-id="21dce-254">Wenn IIS in einer Webfarm verwendet wird:</span><span class="sxs-lookup"><span data-stu-id="21dce-254">When using IIS in a web farm:</span></span>

* <span data-ttu-id="21dce-255">Verwenden Sie eine Dateifreigabe, auf die alle Computer zugreifen können.</span><span class="sxs-lookup"><span data-stu-id="21dce-255">Use a file share that all machines can access.</span></span>
* <span data-ttu-id="21dce-256">Stellen Sie ein X509-Zertifikat auf jedem Computer bereit.</span><span class="sxs-lookup"><span data-stu-id="21dce-256">Deploy an X509 certificate to each machine.</span></span> <span data-ttu-id="21dce-257">Konfigurieren Sie den [Schutz von Daten im Code](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="21dce-257">Configure [data protection in code](xref:security/data-protection/configuration/overview).</span></span>

### <a name="set-a-machine-wide-policy-for-data-protection"></a><span data-ttu-id="21dce-258">Festlegen einer computerweiten Richtlinie für den Schutz von Daten</span><span class="sxs-lookup"><span data-stu-id="21dce-258">Set a machine-wide policy for data protection</span></span>

<span data-ttu-id="21dce-259">Das System zum Schutz von Daten verfügt über eine eingeschränkte Unterstützung zum Festlegen einer [computerweiten Standardrichtlinie](xref:security/data-protection/configuration/machine-wide-policy) für alle Apps, die die Datenschutz-APIs nutzen.</span><span class="sxs-lookup"><span data-stu-id="21dce-259">The data protection system has limited support for setting a default [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy) for all apps that consume the Data Protection APIs.</span></span> <span data-ttu-id="21dce-260">Weitere Details finden Sie in der Dokumentation zum [Schutz von Daten](xref:security/data-protection/index).</span><span class="sxs-lookup"><span data-stu-id="21dce-260">See the [data protection](xref:security/data-protection/index) documentation for more details.</span></span>

## <a name="configuration-of-sub-applications"></a><span data-ttu-id="21dce-261">Konfiguration von untergeordneten Anwendungen</span><span class="sxs-lookup"><span data-stu-id="21dce-261">Configuration of sub-applications</span></span>

<span data-ttu-id="21dce-262">Untergeordnete Apps, die unter der Stamm-App hinzugefügt werden, sollten kein ASP.NET Core-Modul als Handler enthalten.</span><span class="sxs-lookup"><span data-stu-id="21dce-262">Sub-apps added under the root app shouldn't include the ASP.NET Core Module as a handler.</span></span> <span data-ttu-id="21dce-263">Wenn das Modul als Handler in einer Datei namens *Web.config* einer untergeordneten App hinzugefügt wird, erhalten Sie bei dem Versuch zum Navigieren in der untergeordneten App den Code 500.19 (interner Serverfehler), der auf die fehlerhafte Konfigurationsdatei verweist.</span><span class="sxs-lookup"><span data-stu-id="21dce-263">If the module is added as a handler in a sub-app's *web.config* file, a 500.19 (Internal Server Error) referencing the faulty config file is received when attempting to browse the sub-app.</span></span> <span data-ttu-id="21dce-264">Das folgende Beispiel zeigt den Inhalt einer veröffentlichten *web.config*-Datei für eine untergeordnete ASP.NET Core-App:</span><span class="sxs-lookup"><span data-stu-id="21dce-264">The following example shows the contents of a published *web.config* file for an ASP.NET Core sub-app:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <aspNetCore processPath="dotnet" 
      arguments=".\<assembly_name>.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="21dce-265">Wenn Sie eine untergeordnete App ohne ASP.NET Core unterhalb einer ASP.NET Core-App hosten, entfernen Sie den geerbten Handler ausdrücklich aus der Datei *Web.config* der untergeordneten App:</span><span class="sxs-lookup"><span data-stu-id="21dce-265">When hosting a non-ASP.NET Core sub-app underneath an ASP.NET Core app, explicitly remove the inherited handler in the sub-app *web.config* file:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <remove name="aspNetCore"/>
    </handlers>
    <aspNetCore processPath="dotnet" 
      arguments=".\<assembly_name>.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="21dce-266">Weitere Informationen zum Konfigurieren des ASP.NET Core-Moduls finden Sie im Thema [Einführung in das ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module) und in der [Konfigurationsreferenz für das ASP.NET Core-Modul](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="21dce-266">For more information on configuring the ASP.NET Core Module, see the [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) topic and the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

## <a name="configuration-of-iis-with-webconfig"></a><span data-ttu-id="21dce-267">Konfiguration von IIS mit der Datei „web.config“</span><span class="sxs-lookup"><span data-stu-id="21dce-267">Configuration of IIS with web.config</span></span>

<span data-ttu-id="21dce-268">Die IIS-Konfiguration wird im Hinblick auf IIS-Features, die für eine Reverseproxykonfiguration gelten, vom Abschnitt **\<system.webServer>** der Datei *Web.config* beeinflusst.</span><span class="sxs-lookup"><span data-stu-id="21dce-268">IIS configuration is influenced by the **\<system.webServer>** section of *web.config* for those IIS features that apply to a reverse proxy configuration.</span></span> <span data-ttu-id="21dce-269">Wenn IIS auf Systemebene für die Verwendung der dynamischen Komprimierung konfiguriert ist, kann diese Einstellung für eine App mit dem Element **\<urlCompression>** in der Datei *Web.config* der App deaktiviert werden.</span><span class="sxs-lookup"><span data-stu-id="21dce-269">If IIS is configured at the system level to use dynamic compression, that setting can be disabled for an app with the **\<urlCompression>** element in the app's *web.config* file.</span></span> <span data-ttu-id="21dce-270">Weitere Informationen finden Sie in der [Konfigurationsreferenz für \<system.webServer>](https://docs.microsoft.com/iis/configuration/system.webServer/), der [Konfigurationsreferenz für das ASP.NET Core-Modul](xref:host-and-deploy/aspnet-core-module) und unter [Verwenden von IIS-Modulen mit ASP.NET Core](xref:host-and-deploy/iis/modules).</span><span class="sxs-lookup"><span data-stu-id="21dce-270">For more information, see the [configuration reference for \<system.webServer>](https://docs.microsoft.com/iis/configuration/system.webServer/), [ASP.NET Core Module Configuration Reference](xref:host-and-deploy/aspnet-core-module) and [Using IIS Modules with ASP.NET Core](xref:host-and-deploy/iis/modules).</span></span> <span data-ttu-id="21dce-271">Wenn Umgebungsvariablen für einzelne Apps festgelegt werden müssen, die in isolierten App-Pools ausgeführt werden (unterstützt für IIS 10.0 und höher), lesen Sie den Abschnitt zum *Befehl „AppCmd.exe“* im Thema [Umgebungsvariablen \<EnvironmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) in der IIS-Referenzdokumentation.</span><span class="sxs-lookup"><span data-stu-id="21dce-271">If there's a need to set environment variables for individual apps running in isolated app pools (supported on IIS 10.0+), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic in the IIS reference documentation.</span></span>

## <a name="configuration-sections-of-webconfig"></a><span data-ttu-id="21dce-272">Konfigurationsabschnitte von „web.config“</span><span class="sxs-lookup"><span data-stu-id="21dce-272">Configuration sections of web.config</span></span>

<span data-ttu-id="21dce-273">Konfigurationsabschnitte von ASP.NET Framework-Apps in der Datei *Web.config* werden nicht von ASP.NET Core-Apps für die Konfiguration verwendet:</span><span class="sxs-lookup"><span data-stu-id="21dce-273">Configruation sections of ASP.NET Framework apps in *web.config* aren't used by ASP.NET Core apps for configuration:</span></span>

* <span data-ttu-id="21dce-274">**\<system.web>**</span><span class="sxs-lookup"><span data-stu-id="21dce-274">**\<system.web>**</span></span>
* <span data-ttu-id="21dce-275">**\<appSettings>**</span><span class="sxs-lookup"><span data-stu-id="21dce-275">**\<appSettings>**</span></span>
* <span data-ttu-id="21dce-276">**\<connectionStrings>**</span><span class="sxs-lookup"><span data-stu-id="21dce-276">**\<connectionStrings>**</span></span>
* <span data-ttu-id="21dce-277">**\<location>**</span><span class="sxs-lookup"><span data-stu-id="21dce-277">**\<location>**</span></span>

<span data-ttu-id="21dce-278">ASP.NET Core-Apps werden mit anderen Konfigurationsanbietern konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="21dce-278">ASP.NET Core apps are configured using other configuration providers.</span></span> <span data-ttu-id="21dce-279">Weitere Informationen finden Sie unter [Konfiguration](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="21dce-279">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="application-pools"></a><span data-ttu-id="21dce-280">Anwendungspools</span><span class="sxs-lookup"><span data-stu-id="21dce-280">Application Pools</span></span>

<span data-ttu-id="21dce-281">Wenn Sie mehrere Websites in einem einzigen System hosten, isolieren Sie die Apps voneinander, indem Sie jede App in ihrem eigenen App-Pool ausführen.</span><span class="sxs-lookup"><span data-stu-id="21dce-281">When hosting multiple websites on a single system, isolate the apps from each other by running each app in its own app pool.</span></span> <span data-ttu-id="21dce-282">Im IIS-Dialogfeld **Website hinzufügen** wird standardmäßig diese Konfiguration eingesetzt.</span><span class="sxs-lookup"><span data-stu-id="21dce-282">The IIS **Add Website** dialog defaults to this configuration.</span></span> <span data-ttu-id="21dce-283">Wenn ein **Websitename** angegeben ist, wird der Text automatisch in das Textfeld **Anwendungspool** übertragen.</span><span class="sxs-lookup"><span data-stu-id="21dce-283">When **Site name** is provided, the text is automatically transferred to the **Application pool** textbox.</span></span> <span data-ttu-id="21dce-284">Ein neuer App-Pool wird mit dem Namen der Website erstellt, wenn die Website hinzugefügt wird.</span><span class="sxs-lookup"><span data-stu-id="21dce-284">A new app pool is created using the site name when the website is added.</span></span>

## <a name="application-pool-identity"></a><span data-ttu-id="21dce-285">Identität des Anwendungspools</span><span class="sxs-lookup"><span data-stu-id="21dce-285">Application Pool Identity</span></span>

<span data-ttu-id="21dce-286">Mit einem Konto für die Identität des App-Pools können Sie eine App unter einem eindeutigen Konto ausführen, ohne Domänen oder lokale Konten erstellen und verwalten zu müssen.</span><span class="sxs-lookup"><span data-stu-id="21dce-286">An app pool identity account allows an app to run under a unique account without having to create and manage domains or local accounts.</span></span> <span data-ttu-id="21dce-287">Mit IIS 8.0 und höher erstellt der IIS-Administratorworkerprozess (WAS) ein virtuelles Konto mit dem Namen des neuen App-Pools und führt die Workerprozesse des App-Pools standardmäßig unter diesem Konto aus.</span><span class="sxs-lookup"><span data-stu-id="21dce-287">On IIS 8.0+, the IIS Admin Worker Process (WAS) creates a virtual account with the name of the new app pool and runs the app pool's worker processes under this account by default.</span></span> <span data-ttu-id="21dce-288">Stellen Sie sicher, dass in der IIS-Verwaltungskonsole unter **Erweiterte Einstellungen** für den App-Pool die **Identität** auf **ApplicationPoolIdentity** festgelegt ist:</span><span class="sxs-lookup"><span data-stu-id="21dce-288">In the IIS Management Console under **Advanced Settings** for the app pool, ensure that the **Identity** is set to use **ApplicationPoolIdentity**:</span></span>

![Dialogfeld „Erweiterte Einstellungen“ für den Anwendungspool](index/_static/apppool-identity.png)

<span data-ttu-id="21dce-290">Der IIS-Verwaltungsprozess erstellt im Windows-Sicherheitssystem einen sicheren Bezeichner mit dem Namen des App-Pools.</span><span class="sxs-lookup"><span data-stu-id="21dce-290">The IIS management process creates a secure identifier with the name of the app pool in the Windows Security System.</span></span> <span data-ttu-id="21dce-291">Ressourcen können unter Verwendung dieser Identität gesichert werden. Allerdings ist diese Identität kein echtes Benutzerkonto und wird in der Windows-Benutzerverwaltungskonsole nicht angezeigt.</span><span class="sxs-lookup"><span data-stu-id="21dce-291">Resources can be secured by using this identity; however, this identity isn't a real user account and won't show up in the Windows User Management Console.</span></span>

<span data-ttu-id="21dce-292">Wenn der IIS-Workerprozess erhöhte Rechte für den Zugriff auf Ihre Anwendung erfordert, ändern Sie die Zugriffssteuerungsliste (ACL) für das Verzeichnis mit der App:</span><span class="sxs-lookup"><span data-stu-id="21dce-292">If the IIS worker process requires elevated access to the app, modify the Access Control List (ACL) for the directory containing the app:</span></span>

1. <span data-ttu-id="21dce-293">Öffnen Sie Windows Explorer, und navigieren Sie zum Verzeichnis.</span><span class="sxs-lookup"><span data-stu-id="21dce-293">Open Windows Explorer and navigate to the directory.</span></span>

2. <span data-ttu-id="21dce-294">Klicken Sie mit der rechten Maustaste auf das Verzeichnis, und klicken Sie auf **Eigenschaften**.</span><span class="sxs-lookup"><span data-stu-id="21dce-294">Right-click on the directory and select **Properties**.</span></span>

3. <span data-ttu-id="21dce-295">Klicken Sie auf der Registerkarte **Sicherheit** auf die Schaltfläche **Bearbeiten** und dann auf die Schaltfläche **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="21dce-295">Under the **Security** tab, select the **Edit** button and then the **Add** button.</span></span>

4. <span data-ttu-id="21dce-296">Wählen Sie die Schaltfläche **Speicherorte** aus, und stellen Sie sicher, dass das System ausgewählt ist.</span><span class="sxs-lookup"><span data-stu-id="21dce-296">Select the **Locations** button and make sure the system is selected.</span></span>

5. <span data-ttu-id="21dce-297">Geben Sie in das Textfeld **Geben Sie die zu verwendenden Objektnamen ein** den Text **IIS AppPool\DefaultAppPool** ein.</span><span class="sxs-lookup"><span data-stu-id="21dce-297">Enter **IIS AppPool\DefaultAppPool** in **Enter the object names to select** textbox.</span></span>

   ![Dialogfeld „Benutzer oder Gruppen auswählen“ für den App-Ordner](index/_static/select-users-or-groups-1.png)

6. <span data-ttu-id="21dce-299">Klicken Sie auf die Schaltfläche **Namen überprüfen**.</span><span class="sxs-lookup"><span data-stu-id="21dce-299">Select the **Check Names** button.</span></span> <span data-ttu-id="21dce-300">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="21dce-300">Select **OK**.</span></span>

   ![Dialogfeld „Benutzer oder Gruppen auswählen“ für den App-Ordner](index/_static/select-users-or-groups-2.png)

<span data-ttu-id="21dce-302">Dies kann auch über eine Eingabeaufforderung mit dem Tool **ICACLS** erreicht werden:</span><span class="sxs-lookup"><span data-stu-id="21dce-302">This can also be accomplished via a command prompt using the **ICACLS** tool:</span></span>

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

## <a name="additional-resources"></a><span data-ttu-id="21dce-303">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="21dce-303">Additional resources</span></span>

* [<span data-ttu-id="21dce-304">Problembehandlung bei ASP.NET Core in IIS</span><span class="sxs-lookup"><span data-stu-id="21dce-304">Troubleshoot ASP.NET Core on IIS</span></span>](xref:host-and-deploy/iis/troubleshoot)
* [<span data-ttu-id="21dce-305">Referenz zu häufigen Fehlern bei Azure App Service und IIS mit ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="21dce-305">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)
* [<span data-ttu-id="21dce-306">Einführung in das ASP.NET Core-Modul</span><span class="sxs-lookup"><span data-stu-id="21dce-306">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="21dce-307">Konfigurationsreferenz für das ASP.NET Core-Modul</span><span class="sxs-lookup"><span data-stu-id="21dce-307">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="21dce-308">Verwenden von IIS-Modulen mit ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="21dce-308">Using IIS Modules with ASP.NET Core</span></span>](xref:host-and-deploy/iis/modules)
* [<span data-ttu-id="21dce-309">Einführung in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="21dce-309">Introduction to ASP.NET Core</span></span>](../index.md)
* [<span data-ttu-id="21dce-310">Die offizielle Microsoft IIS-Website</span><span class="sxs-lookup"><span data-stu-id="21dce-310">The Official Microsoft IIS Site</span></span>](https://www.iis.net/)
* [<span data-ttu-id="21dce-311">Microsoft TechNet-Bibliothek: Windows Server</span><span class="sxs-lookup"><span data-stu-id="21dce-311">Microsoft TechNet Library: Windows Server</span></span>](https://docs.microsoft.com/windows-server/windows-server-versions)
