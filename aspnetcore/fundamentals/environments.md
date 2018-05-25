---
title: Verwenden von mehreren Umgebungen in ASP.NET Core
author: rick-anderson
description: Erfahren Sie, wie ASP.NET Core umgebungsübergreifend Unterstützung für das Steuern des App-Verhaltens bereitstellt.
manager: wpickett
ms.author: riande
ms.date: 12/25/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/environments
ms.openlocfilehash: 2c8441db527203aeea516073dae3bc335c335565
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="use-multiple-environments-in-aspnet-core"></a><span data-ttu-id="e2e4f-103">Verwenden von mehreren Umgebungen in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e2e4f-103">Use multiple environments in ASP.NET Core</span></span>

<span data-ttu-id="e2e4f-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e2e4f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e2e4f-105">ASP.NET Core bietet Unterstützung für das Festlegen des App-Verhaltens zur Laufzeit mit Umgebungsvariablen.</span><span class="sxs-lookup"><span data-stu-id="e2e4f-105">ASP.NET Core provides support for setting application behavior at runtime with environment variables.</span></span>

<span data-ttu-id="e2e4f-106">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e2e4f-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="e2e4f-107">Umgebungen</span><span class="sxs-lookup"><span data-stu-id="e2e4f-107">Environments</span></span>

<span data-ttu-id="e2e4f-108">ASP.NET Core liest die Umgebungsvariable `ASPNETCORE_ENVIRONMENT` beim Start der App und speichert diesen Wert unter [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName).</span><span class="sxs-lookup"><span data-stu-id="e2e4f-108">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at application startup and stores that value in [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName).</span></span> <span data-ttu-id="e2e4f-109">`ASPNETCORE_ENVIRONMENT` kann auf einen beliebigen Wert festgelegt werden, das Framework unterstützt jedoch [drei Werte](/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0): [Entwicklung](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [Staging](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0) und [Produktion](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="e2e4f-109">`ASPNETCORE_ENVIRONMENT` can be set to any value, but [three values](/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0) are supported by the framework: [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [Staging](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0), and [Production](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0).</span></span> <span data-ttu-id="e2e4f-110">Wenn `ASPNETCORE_ENVIRONMENT` nicht festgelegt ist, wird `Production` als Standard verwendet.</span><span class="sxs-lookup"><span data-stu-id="e2e4f-110">If `ASPNETCORE_ENVIRONMENT` isn't set, it will default to `Production`.</span></span>

[!code-csharp[](environments/sample/WebApp1/Startup.cs?name=snippet)]

<span data-ttu-id="e2e4f-111">Der vorangehende Code:</span><span class="sxs-lookup"><span data-stu-id="e2e4f-111">The preceding code:</span></span>

* <span data-ttu-id="e2e4f-112">Ruft [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) und [UseBrowserLink](/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) auf, wenn `ASPNETCORE_ENVIRONMENT` auf `Development` festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="e2e4f-112">Calls [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseBrowserLink](/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="e2e4f-113">Ruft [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) auf, wenn der Wert von `ASPNETCORE_ENVIRONMENT` auf einen der folgenden Werte festgelegt ist:</span><span class="sxs-lookup"><span data-stu-id="e2e4f-113">Calls [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

    * `Staging`
    * `Production`
    * `Staging_2`

<span data-ttu-id="e2e4f-114">Das [Umgebungstaghilfsprogramm](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) verwendet den Wert von `IHostingEnvironment.EnvironmentName` zum Einschließen oder Ausschließen von Markup im Element:</span><span class="sxs-lookup"><span data-stu-id="e2e4f-114">The [Environment Tag Helper ](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-html[](environments/sample/WebApp1/Pages/About.cshtml)]

<span data-ttu-id="e2e4f-115">Hinweis: Unter Windows und macOS wird bei Umgebungsvariablen und Werten die Groß-/Kleinschreibung nicht beachtet.</span><span class="sxs-lookup"><span data-stu-id="e2e4f-115">Note: On Windows and macOS, environment variables and values are not case sensitive.</span></span> <span data-ttu-id="e2e4f-116">Bei Linux-Umgebungsvariablen und -Werten **wird die Groß-/Kleinschreibung standardmäßig beachtet**.</span><span class="sxs-lookup"><span data-stu-id="e2e4f-116">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="e2e4f-117">Entwicklung</span><span class="sxs-lookup"><span data-stu-id="e2e4f-117">Development</span></span>

<span data-ttu-id="e2e4f-118">In der Entwicklungsumgebung können Features aktiviert werden, die in der Produktion nicht verfügbar gemacht werden sollten.</span><span class="sxs-lookup"><span data-stu-id="e2e4f-118">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="e2e4f-119">Mit den ASP.NET Core-Vorlagen wird beispielsweise die [Seite mit Ausnahmen für Entwickler](xref:fundamentals/error-handling#the-developer-exception-page) in der Entwicklungsumgebung aktiviert.</span><span class="sxs-lookup"><span data-stu-id="e2e4f-119">For example, the ASP.NET Core templates enable the [developer exception page](xref:fundamentals/error-handling#the-developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="e2e4f-120">Die Umgebung für die Entwicklung lokaler Computer kann in der Datei *Properties\launchSettings.json* des Projekts festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="e2e4f-120">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="e2e4f-121">Mit in der Datei *launchSettings.json* festgelegten Umgebungsvariablen werden in der Systemumgebung festgelegte Werte überschrieben.</span><span class="sxs-lookup"><span data-stu-id="e2e4f-121">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="e2e4f-122">Die folgende JSON zeigt drei Profile aus der Datei *launchSettings.json* an:</span><span class="sxs-lookup"><span data-stu-id="e2e4f-122">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

[!code-json[](environments/sample/WebApp1/Properties/launchSettings.json?highlight=10,11,18,26)]

::: moniker range=">= aspnetcore-2.1"
> [!NOTE]
> <span data-ttu-id="e2e4f-123">Die Eigenschaft `applicationUrl` in *launchSettings.json* kann eine Liste von Server-URLs angeben.</span><span class="sxs-lookup"><span data-stu-id="e2e4f-123">The `applicationUrl` property in *launchSettings.json* can specify a list of server URLs.</span></span> <span data-ttu-id="e2e4f-124">Verwenden Sie ein Semikolon zwischen den URLs in der Liste:</span><span class="sxs-lookup"><span data-stu-id="e2e4f-124">Use a semicolon between the URLs in the list:</span></span>
>
> ```json
> "WebApplication1": {
>    "commandName": "Project",
>    "launchBrowser": true,
>    "applicationUrl": "https://localhost:5001;http://localhost:5000",
>    "environmentVariables": {
>      "ASPNETCORE_ENVIRONMENT": "Development"
>    }
> }
> ```
::: moniker-end

<span data-ttu-id="e2e4f-125">Wenn die Anwendung mit [dotnet run](/dotnet/core/tools/dotnet-run) gestartet wird, wird das erste Profil mit `"commandName": "Project"` verwendet.</span><span class="sxs-lookup"><span data-stu-id="e2e4f-125">When the application is launched with [dotnet run](/dotnet/core/tools/dotnet-run), the first profile with `"commandName": "Project"` will be used.</span></span> <span data-ttu-id="e2e4f-126">Der Wert von `commandName` gibt den zu startenden Webserver an.</span><span class="sxs-lookup"><span data-stu-id="e2e4f-126">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="e2e4f-127">`commandName` kann Teil von Folgendem sein:</span><span class="sxs-lookup"><span data-stu-id="e2e4f-127">`commandName` can be one of :</span></span>

* <span data-ttu-id="e2e4f-128">IIS Express</span><span class="sxs-lookup"><span data-stu-id="e2e4f-128">IIS Express</span></span>
* <span data-ttu-id="e2e4f-129">IIS</span><span class="sxs-lookup"><span data-stu-id="e2e4f-129">IIS</span></span>
* <span data-ttu-id="e2e4f-130">Projekt (über das Kestrel gestartet wird)</span><span class="sxs-lookup"><span data-stu-id="e2e4f-130">Project (which launches Kestrel)</span></span>

<span data-ttu-id="e2e4f-131">Beim Start einer App mit [dotnet run](/dotnet/core/tools/dotnet-run) tritt Folgendes ein:</span><span class="sxs-lookup"><span data-stu-id="e2e4f-131">When an app is launched with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

* <span data-ttu-id="e2e4f-132">Die Datei *launchSettings.json* wird, sofern verfügbar, gelesen.</span><span class="sxs-lookup"><span data-stu-id="e2e4f-132">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="e2e4f-133">Durch `environmentVariables`-Einstellungen in der Datei *launchSettings.json* werden Umgebungsvariablen überschrieben.</span><span class="sxs-lookup"><span data-stu-id="e2e4f-133">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="e2e4f-134">Die Hostingumgebung wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="e2e4f-134">The hosting environment is displayed.</span></span>


<span data-ttu-id="e2e4f-135">Die folgende Ausgabe zeigt eine App, die mit [dotnet run](/dotnet/core/tools/dotnet-run) gestartet wurde:</span><span class="sxs-lookup"><span data-stu-id="e2e4f-135">The following output shows an app started with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>
```bash
PS C:\Webs\WebApp1> dotnet run
Using launch settings from C:\Webs\WebApp1\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Webs\WebApp1
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="e2e4f-136">Auf der Registerkarte **Debuggen** in Visual Studio wird eine grafische Benutzeroberfläche für die Bearbeitung der Datei *launchSettings.json* bereitgestellt:</span><span class="sxs-lookup"><span data-stu-id="e2e4f-136">The Visual Studio **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![Projekteigenschaften zum Festlegen von Umgebungsvariablen](environments/_static/project-properties-debug.png)

<span data-ttu-id="e2e4f-138">An Projektprofilen vorgenommene Änderungen werden möglicherweise erst nach einem Neustart des Webservers wirksam.</span><span class="sxs-lookup"><span data-stu-id="e2e4f-138">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="e2e4f-139">Kestrel muss neu gestartet werden, bevor es an der Umgebung vorgenommene Änderungen erkennen kann.</span><span class="sxs-lookup"><span data-stu-id="e2e4f-139">Kestrel must be restarted before it will detect changes made to its environment.</span></span>

>[!WARNING]
> <span data-ttu-id="e2e4f-140">In der Datei *launchSettings.json* sollten keine geheimen Schlüssel gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="e2e4f-140">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="e2e4f-141">Mit dem [Secret Manager-Tool](xref:security/app-secrets) können geheime Schlüssel für die lokale Umgebung gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="e2e4f-141">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

### <a name="production"></a><span data-ttu-id="e2e4f-142">Produktion</span><span class="sxs-lookup"><span data-stu-id="e2e4f-142">Production</span></span>

<span data-ttu-id="e2e4f-143">Die Produktionsumgebung sollte so konfiguriert werden, dass Sicherheit, Leistung und Stabilität der App maximiert werden.</span><span class="sxs-lookup"><span data-stu-id="e2e4f-143">The production environment should be configured to maximize security, performance, and application robustness.</span></span> <span data-ttu-id="e2e4f-144">Allgemeine Einstellungen, die sich von der Entwicklung unterscheiden, sind zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="e2e4f-144">Some common settings that differ from development include:</span></span>

* <span data-ttu-id="e2e4f-145">Zwischenspeicherung.</span><span class="sxs-lookup"><span data-stu-id="e2e4f-145">Caching.</span></span>
* <span data-ttu-id="e2e4f-146">Clientseitige Ressourcen werden gebündelt, verkleinert und potenziell von einem CDN bedient.</span><span class="sxs-lookup"><span data-stu-id="e2e4f-146">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="e2e4f-147">Seiten zur Fehlerdiagnose sind deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="e2e4f-147">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="e2e4f-148">Angezeigte Fehlerseiten sind aktiviert.</span><span class="sxs-lookup"><span data-stu-id="e2e4f-148">Friendly error pages enabled.</span></span>
* <span data-ttu-id="e2e4f-149">Die Produktionsprotokollierung und -überwachung ist aktiviert.</span><span class="sxs-lookup"><span data-stu-id="e2e4f-149">Production logging and monitoring enabled.</span></span> <span data-ttu-id="e2e4f-150">Beispiel: [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="e2e4f-150">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="setting-the-environment"></a><span data-ttu-id="e2e4f-151">Festlegen der Umgebung</span><span class="sxs-lookup"><span data-stu-id="e2e4f-151">Setting the environment</span></span>

<span data-ttu-id="e2e4f-152">Es ist oft nützlich, zu Testzwecken eine bestimmte Umgebung festzulegen.</span><span class="sxs-lookup"><span data-stu-id="e2e4f-152">It's often useful to set a specific environment for testing.</span></span> <span data-ttu-id="e2e4f-153">Wenn die Umgebung nicht festgelegt ist, wird `Production` als Standardwert verwendet, wodurch die meisten Debugfeatures deaktiviert werden.</span><span class="sxs-lookup"><span data-stu-id="e2e4f-153">If the environment isn't set, it will default to `Production` which disables most debugging features.</span></span>

<span data-ttu-id="e2e4f-154">Welche Methode zum Festlegen der Umgebung verwendet wird, hängt vom Betriebssystem ab.</span><span class="sxs-lookup"><span data-stu-id="e2e4f-154">The method for setting the environment depends on the operating system.</span></span>

### <a name="azure"></a><span data-ttu-id="e2e4f-155">Azure</span><span class="sxs-lookup"><span data-stu-id="e2e4f-155">Azure</span></span>

<span data-ttu-id="e2e4f-156">Bei dem Azure App Service:</span><span class="sxs-lookup"><span data-stu-id="e2e4f-156">For Azure app service:</span></span>

* <span data-ttu-id="e2e4f-157">Wählen Sie das Blatt **Anwendungseinstellungen** aus.</span><span class="sxs-lookup"><span data-stu-id="e2e4f-157">Select the **Application settings** blade.</span></span>
* <span data-ttu-id="e2e4f-158">Fügen Sie den Schlüssel und den Wert unter **Anwendungseinstellungen** hinzu.</span><span class="sxs-lookup"><span data-stu-id="e2e4f-158">Add the key and value in **App settings**.</span></span>


### <a name="windows"></a><span data-ttu-id="e2e4f-159">Windows</span><span class="sxs-lookup"><span data-stu-id="e2e4f-159">Windows</span></span>
<span data-ttu-id="e2e4f-160">Zum Festlegen der `ASPNETCORE_ENVIRONMENT` für die aktuelle Sitzung werden folgende Befehle verwendet, wenn die App mit [dotnet run](/dotnet/core/tools/dotnet-run) gestartet wird:</span><span class="sxs-lookup"><span data-stu-id="e2e4f-160">To set the `ASPNETCORE_ENVIRONMENT` for the current session, if the app is started using [dotnet run](/dotnet/core/tools/dotnet-run), the following commands are used</span></span>

<span data-ttu-id="e2e4f-161">**Befehlszeile**</span><span class="sxs-lookup"><span data-stu-id="e2e4f-161">**Command line**</span></span>
```
set ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="e2e4f-162">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="e2e4f-162">**PowerShell**</span></span>
```
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="e2e4f-163">Diese Befehle sind nur für das aktuelle Fenster wirksam.</span><span class="sxs-lookup"><span data-stu-id="e2e4f-163">These commands take effect only for the current window.</span></span> <span data-ttu-id="e2e4f-164">Wenn das Fenster geschlossen wird, wird die Einstellung ASPNETCORE_ENVIRONMENT auf die Standardeinstellung oder den Computerwert zurückgesetzt.</span><span class="sxs-lookup"><span data-stu-id="e2e4f-164">When the window is closed, the ASPNETCORE_ENVIRONMENT setting reverts to the default setting or machine value.</span></span> <span data-ttu-id="e2e4f-165">Wenn Sie den Wert global unter Windows festlegen möchten, müssen Sie die **Systemsteuerung** > **System** > **Erweiterte Systemeinstellungen** öffnen und den Wert `ASPNETCORE_ENVIRONMENT` hinzufügen oder bearbeiten.</span><span class="sxs-lookup"><span data-stu-id="e2e4f-165">In order to set the value globally on Windows open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value.</span></span>

![Erweiterte Systemeigenschaften](environments/_static/systemsetting_environment.png)

![ASP.NET Core-Umgebungsvariable](environments/_static/windows_aspnetcore_environment.png)


<span data-ttu-id="e2e4f-168">**web.config**</span><span class="sxs-lookup"><span data-stu-id="e2e4f-168">**web.config**</span></span>

<span data-ttu-id="e2e4f-169">Weitere Informationen finden Sie im Abschnitt *Festlegen der Umgebungsvariablen* im Artikel [Verweis auf die Konfiguration des ASP.NET Core-Moduls](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span><span class="sxs-lookup"><span data-stu-id="e2e4f-169">See the *Setting environment variables* section of the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) topic.</span></span>

<span data-ttu-id="e2e4f-170">**Pro IIS-Anwendungspool**</span><span class="sxs-lookup"><span data-stu-id="e2e4f-170">**Per IIS Application Pool**</span></span>

<span data-ttu-id="e2e4f-171">Wenn Sie Umgebungsvariablen für einzelne Apps festlegen möchten, die in isolierten Anwendungspools ausgeführt werden (unterstützt für IIS 10.0 und höher), finden Sie weitere Informationen im Abschnitt zum *Befehl „AppCmd.exe“* im Artikel [Umgebungsvariablen \< EnvironmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe).</span><span class="sxs-lookup"><span data-stu-id="e2e4f-171">To set environment variables for individual apps running in isolated Application Pools (supported on IIS 10.0+), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span>

### <a name="macos"></a><span data-ttu-id="e2e4f-172">macOS</span><span class="sxs-lookup"><span data-stu-id="e2e4f-172">macOS</span></span>
<span data-ttu-id="e2e4f-173">Die aktuelle Umgebung für macOS kann inline während der Ausführung der App erfolgen</span><span class="sxs-lookup"><span data-stu-id="e2e4f-173">Setting the current environment for macOS can be done in-line when running the application;</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```
<span data-ttu-id="e2e4f-174">oder mit `export`, um die Umgebung vor Ausführung der App festzulegen.</span><span class="sxs-lookup"><span data-stu-id="e2e4f-174">or using `export` to set it prior to running the app.</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="e2e4f-175">Umgebungsvariablen auf Computerebene werden in der Datei *.bashrc* oder *.bash_profile* festgelegt.</span><span class="sxs-lookup"><span data-stu-id="e2e4f-175">Machine level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="e2e4f-176">Bearbeiten Sie die Datei mit einem beliebigen Text-Editor, und fügen Sie die folgende Anweisung hinzu.</span><span class="sxs-lookup"><span data-stu-id="e2e4f-176">Edit the file using any text editor and add the following statment.</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a><span data-ttu-id="e2e4f-177">Linux</span><span class="sxs-lookup"><span data-stu-id="e2e4f-177">Linux</span></span>
<span data-ttu-id="e2e4f-178">Verwenden Sie bei Linux-Distributionen für sitzungsbasierte Variableneinstellungen den Befehl `export` in der Befehlszeile und die Datei *bash_profile* für Umgebungseinstellungen auf Computerebene.</span><span class="sxs-lookup"><span data-stu-id="e2e4f-178">For Linux distros, use the `export` command at the command line for session based variable settings and *bash_profile* file for machine level environment settings.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="e2e4f-179">Konfiguration nach Umgebung</span><span class="sxs-lookup"><span data-stu-id="e2e4f-179">Configuration by environment</span></span>

<span data-ttu-id="e2e4f-180">Weitere Informationen finden Sie unter [Konfiguration nach Umgebung](xref:fundamentals/configuration/index#configuration-by-environment).</span><span class="sxs-lookup"><span data-stu-id="e2e4f-180">See [Configuration by environment](xref:fundamentals/configuration/index#configuration-by-environment) for more information.</span></span>

<a name="startup-conventions"></a>
## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="e2e4f-181">Umgebungsbasierte Startklasse und Methoden</span><span class="sxs-lookup"><span data-stu-id="e2e4f-181">Environment based Startup class and methods</span></span>

<span data-ttu-id="e2e4f-182">Nach dem Start einer ASP.NET Core-App lädt die [Startklasse](xref:fundamentals/startup) die App.</span><span class="sxs-lookup"><span data-stu-id="e2e4f-182">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="e2e4f-183">Wenn eine `Startup{EnvironmentName}`-Klasse vorhanden ist, wird diese Klasse für diesen `EnvironmentName` aufgerufen:</span><span class="sxs-lookup"><span data-stu-id="e2e4f-183">If a class `Startup{EnvironmentName}` exists, that class will be called for that `EnvironmentName`:</span></span>

[!code-csharp[](environments/sample/WebApp1/StartupDev.cs?name=snippet&highlight=1)]

<span data-ttu-id="e2e4f-184">Hinweis: Durch das Aufrufen von [WebHostBuilder.UseStartup<TStartup>](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) werden Konfigurationsabschnitte überschrieben.</span><span class="sxs-lookup"><span data-stu-id="e2e4f-184">Note: Calling [WebHostBuilder.UseStartup<TStartup>](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) overrides configuration sections.</span></span>

<span data-ttu-id="e2e4f-185">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) und [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) unterstützen umgebungsspezifische Versionen der Form `Configure{EnvironmentName}` und `Configure{EnvironmentName}Services`:</span><span class="sxs-lookup"><span data-stu-id="e2e4f-185">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) support environment specific versions of the form `Configure{EnvironmentName}` and `Configure{EnvironmentName}Services`:</span></span>

[!code-csharp[](environments/sample/WebApp1/Startup.cs?name=snippet_all&highlight=15,37)]

## <a name="additional-resources"></a><span data-ttu-id="e2e4f-186">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="e2e4f-186">Additional resources</span></span>

* [<span data-ttu-id="e2e4f-187">Starten von Anwendungen</span><span class="sxs-lookup"><span data-stu-id="e2e4f-187">Application startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="e2e4f-188">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="e2e4f-188">Configuration</span></span>](xref:fundamentals/configuration/index)
* [<span data-ttu-id="e2e4f-189">IHostingEnvironment.EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="e2e4f-189">IHostingEnvironment.EnvironmentName</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName)
