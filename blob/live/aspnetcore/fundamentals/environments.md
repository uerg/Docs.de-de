---
title: Arbeiten mit mehreren Umgebungen in ASP.NET Core
author: rick-anderson
description: "Erfahren Sie, wie ASP.NET Core Unterstützung für das Steuern des Verhaltens der app über mehrere Umgebungen bereitstellt."
ms.author: riande
manager: wpickett
ms.date: 12/25/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/environments
ms.openlocfilehash: 83d1593d46761b1c00aa431cfdcde59cb3b28b65
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
---
# <a name="working-with-multiple-environments"></a><span data-ttu-id="c99e9-103">Arbeiten mit mehreren Umgebungen</span><span class="sxs-lookup"><span data-stu-id="c99e9-103">Working with multiple environments</span></span>

<span data-ttu-id="c99e9-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c99e9-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c99e9-105">ASP.NET Core bietet Unterstützung für das Verhalten der Anwendung zur Laufzeit mit Umgebungsvariablen festlegen.</span><span class="sxs-lookup"><span data-stu-id="c99e9-105">ASP.NET Core provides support for setting application behavior at runtime with environment variables.</span></span>

<span data-ttu-id="c99e9-106">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c99e9-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="c99e9-107">Umgebungen</span><span class="sxs-lookup"><span data-stu-id="c99e9-107">Environments</span></span>

<span data-ttu-id="c99e9-108">ASP.NET Core liest die Umgebungsvariable `ASPNETCORE_ENVIRONMENT` am Start der Anwendung und Speichervorgänge aus, die im Wert [IHostingEnvironment.EnvironmentName](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName).</span><span class="sxs-lookup"><span data-stu-id="c99e9-108">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at application startup and stores that value in [IHostingEnvironment.EnvironmentName](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName).</span></span> <span data-ttu-id="c99e9-109">`ASPNETCORE_ENVIRONMENT`kann auf einen beliebigen Wert festgelegt werden, aber [drei Werte](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0) werden durch das Framework unterstützt: [Entwicklung](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [Staging](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0), und [Produktion](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="c99e9-109">`ASPNETCORE_ENVIRONMENT` can be set to any value, but [three values](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0) are supported by the framework: [Development](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [Staging](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0), and [Production](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0).</span></span> <span data-ttu-id="c99e9-110">Wenn `ASPNETCORE_ENVIRONMENT` ist nicht festgelegt ist, wird standardmäßig `Production`.</span><span class="sxs-lookup"><span data-stu-id="c99e9-110">If `ASPNETCORE_ENVIRONMENT` is not set, it will default to `Production`.</span></span>

[!code-csharp[Main](environments/sample/WebApp1/Startup.cs?name=snippet)]

<span data-ttu-id="c99e9-111">Der vorangehende Code:</span><span class="sxs-lookup"><span data-stu-id="c99e9-111">The preceding code:</span></span>

* <span data-ttu-id="c99e9-112">Aufrufe [UseDeveloperExceptionPage](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) und [UseBrowserLink](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) Wenn `ASPNETCORE_ENVIRONMENT` festgelegt ist, um `Development`.</span><span class="sxs-lookup"><span data-stu-id="c99e9-112">Calls [UseDeveloperExceptionPage](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseBrowserLink](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="c99e9-113">Aufrufe [UseExceptionHandler](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) Wenn der Wert der `ASPNETCORE_ENVIRONMENT` festgelegt ist, eine der folgenden:</span><span class="sxs-lookup"><span data-stu-id="c99e9-113">Calls [UseExceptionHandler](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

    * `Staging`
    * `Production`
    * `Staging_2`

<span data-ttu-id="c99e9-114">Die [Umgebung Tag Helper ](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) verwendet den Wert der `IHostingEnvironment.EnvironmentName` einschließen oder Ausschließen von Markup im Element:</span><span class="sxs-lookup"><span data-stu-id="c99e9-114">The [Environment Tag Helper ](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-html[Main](environments/sample/WebApp1/Pages/About.cshtml)]

<span data-ttu-id="c99e9-115">Hinweis: Auf Windows- und Mac OS sind Umgebungsvariablen und die Werte nicht Groß-/Kleinschreibung beachtet.</span><span class="sxs-lookup"><span data-stu-id="c99e9-115">Note: On Windows and macOS, environment variables and values are not case sensitive.</span></span> <span data-ttu-id="c99e9-116">Linux-Umgebungsvariablen und Werte sind **Groß-/Kleinschreibung beachtet** standardmäßig.</span><span class="sxs-lookup"><span data-stu-id="c99e9-116">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="c99e9-117">Entwicklung</span><span class="sxs-lookup"><span data-stu-id="c99e9-117">Development</span></span>

<span data-ttu-id="c99e9-118">Die Entwicklungsumgebung kann Funktionen aktivieren, die in der Produktion nicht verfügbar gemacht werden soll.</span><span class="sxs-lookup"><span data-stu-id="c99e9-118">The development environment can enable features that should not be exposed in production.</span></span> <span data-ttu-id="c99e9-119">Z. B. die Vorlagen für ASP.NET Core aktivieren die [Developer Ausnahmeseite](xref:fundamentals/error-handling#the-developer-exception-page) in der Entwicklungsumgebung.</span><span class="sxs-lookup"><span data-stu-id="c99e9-119">For example, the ASP.NET Core templates enable the [developer exception page](xref:fundamentals/error-handling#the-developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="c99e9-120">Die Umgebung für die Entwicklung des lokalen Computers kann festgelegt werden, der *Properties\launchSettings.json* -Datei des Projekts.</span><span class="sxs-lookup"><span data-stu-id="c99e9-120">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="c99e9-121">Umgebungswerte festgelegt *launchSettings.json* in die Umgebung für die festgelegte Werte zu überschreiben.</span><span class="sxs-lookup"><span data-stu-id="c99e9-121">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="c99e9-122">Das folgende XML zeigt drei Profile aus einem *launchSettings.json* Datei:</span><span class="sxs-lookup"><span data-stu-id="c99e9-122">The following XML shows three profiles from a *launchSettings.json* file:</span></span>

[!code-xml[Main](environments/sample/WebApp1/Properties/launchSettings.json?highlight=10,11,18,26)]

<span data-ttu-id="c99e9-123">Beim Start der Anwendung mit `dotnet run`, das erste Profil mit `"commandName": "Project"` verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="c99e9-123">When the application is launched with `dotnet run`, the first profile with `"commandName": "Project"` will be used.</span></span> <span data-ttu-id="c99e9-124">Der Wert der `commandName` gibt den Webserver zu starten.</span><span class="sxs-lookup"><span data-stu-id="c99e9-124">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="c99e9-125">`commandName`möglich:</span><span class="sxs-lookup"><span data-stu-id="c99e9-125">`commandName` can be one of :</span></span>

* <span data-ttu-id="c99e9-126">IIS Express</span><span class="sxs-lookup"><span data-stu-id="c99e9-126">IIS Express</span></span>
* <span data-ttu-id="c99e9-127">IIS</span><span class="sxs-lookup"><span data-stu-id="c99e9-127">IIS</span></span>
* <span data-ttu-id="c99e9-128">Projekt (die Kestrel gestartet)</span><span class="sxs-lookup"><span data-stu-id="c99e9-128">Project (which launches Kestrel)</span></span>

<span data-ttu-id="c99e9-129">Beim Start einer Anwendung mit `dotnet run`:</span><span class="sxs-lookup"><span data-stu-id="c99e9-129">When an app is launched with `dotnet run`:</span></span>

* <span data-ttu-id="c99e9-130">*launchSettings.json* wird gelesen, falls verfügbar.</span><span class="sxs-lookup"><span data-stu-id="c99e9-130">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="c99e9-131">`environmentVariables`Einstellungen im *launchSettings.json* Überschreiben von Umgebungsvariablen.</span><span class="sxs-lookup"><span data-stu-id="c99e9-131">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="c99e9-132">Die Hostingumgebung wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="c99e9-132">The hosting environment is displayed.</span></span>


<span data-ttu-id="c99e9-133">Die folgende Ausgabe zeigt eine app Einstieg `dotnet run`:</span><span class="sxs-lookup"><span data-stu-id="c99e9-133">The following output shows an app started with `dotnet run`:</span></span>
```bash
PS C:\Webs\WebApp1> dotnet run
Using launch settings from C:\Webs\WebApp1\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Webs\WebApp1
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="c99e9-134">Visual Studio **Debuggen** Registerkarte finden Sie eine GUI so bearbeiten Sie die *launchSettings.json* Datei:</span><span class="sxs-lookup"><span data-stu-id="c99e9-134">The Visual Studio **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![Projekt Umgebungsvariablen Eigenschaften festlegen](environments/_static/project-properties-debug.png)

<span data-ttu-id="c99e9-136">Änderungen an Projekt Profile können erst nach dem Neustart des Servers wirksam.</span><span class="sxs-lookup"><span data-stu-id="c99e9-136">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="c99e9-137">Kestrel muss neu gestartet werden, bevor es an ihre Umgebung vorgenommenen Änderungen erkannt werden.</span><span class="sxs-lookup"><span data-stu-id="c99e9-137">Kestrel must be restarted before it will detect changes made to its environment.</span></span>

>[!WARNING]
> <span data-ttu-id="c99e9-138">*launchSettings.json* geheimen Schlüssel sollten nicht speichern.</span><span class="sxs-lookup"><span data-stu-id="c99e9-138">*launchSettings.json* should not store secrets.</span></span> <span data-ttu-id="c99e9-139">Die [Secret-Manager-Tool](xref:security/app-secrets) Speichern von geheimen Schlüsseln für die lokale Entwicklung verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="c99e9-139">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

### <a name="production"></a><span data-ttu-id="c99e9-140">Produktion</span><span class="sxs-lookup"><span data-stu-id="c99e9-140">Production</span></span>

<span data-ttu-id="c99e9-141">Die produktionsumgebung sollte konfiguriert werden, um Sicherheit, Leistung und Stabilität der Anwendung zu maximieren.</span><span class="sxs-lookup"><span data-stu-id="c99e9-141">The production environment should be configured to maximize security, performance, and application robustness.</span></span> <span data-ttu-id="c99e9-142">Einige allgemeinen Einstellungen, die möglicherweise von eine produktiven Umgebung, die von der Entwicklung unterscheiden würde gehören:</span><span class="sxs-lookup"><span data-stu-id="c99e9-142">Some common settings that a production environment might have that would differ from development include:</span></span>

* <span data-ttu-id="c99e9-143">Das Zwischenspeichern.</span><span class="sxs-lookup"><span data-stu-id="c99e9-143">Caching.</span></span>
* <span data-ttu-id="c99e9-144">Die clientseitige Ressourcen sind gebündelt, verkleinert und potenziell von einem CDN bedient.</span><span class="sxs-lookup"><span data-stu-id="c99e9-144">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="c99e9-145">Diagnose Fehlerseiten deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="c99e9-145">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="c99e9-146">Angezeigter Fehlerseiten aktiviert.</span><span class="sxs-lookup"><span data-stu-id="c99e9-146">Friendly error pages enabled.</span></span>
* <span data-ttu-id="c99e9-147">Produktions-Protokollierung und Überwachung aktiviert.</span><span class="sxs-lookup"><span data-stu-id="c99e9-147">Production logging and monitoring enabled.</span></span> <span data-ttu-id="c99e9-148">Beispielsweise [Application Insights](https://azure.microsoft.com/documentation/articles/app-insights-asp-net-five/).</span><span class="sxs-lookup"><span data-stu-id="c99e9-148">For example, [Application Insights](https://azure.microsoft.com/documentation/articles/app-insights-asp-net-five/).</span></span>

## <a name="setting-the-environment"></a><span data-ttu-id="c99e9-149">Einrichten der Umgebung</span><span class="sxs-lookup"><span data-stu-id="c99e9-149">Setting the environment</span></span>

<span data-ttu-id="c99e9-150">Es ist häufig nützlich, um eine bestimmte Umgebung zu Testzwecken festgelegt.</span><span class="sxs-lookup"><span data-stu-id="c99e9-150">It's often useful to set a specific environment for testing.</span></span> <span data-ttu-id="c99e9-151">Wenn die Umgebung nicht festgelegt ist, wird standardmäßig `Production` deaktiviert die meisten Debugfunktionen.</span><span class="sxs-lookup"><span data-stu-id="c99e9-151">If the environment is not set, it will default to `Production` which disables most debugging features.</span></span>

<span data-ttu-id="c99e9-152">Die Methode zum Festlegen der umgebungs hängt vom Betriebssystem ab.</span><span class="sxs-lookup"><span data-stu-id="c99e9-152">The method for setting the environment depends on the operating system.</span></span>

### <a name="azure"></a><span data-ttu-id="c99e9-153">Azure</span><span class="sxs-lookup"><span data-stu-id="c99e9-153">Azure</span></span>

<span data-ttu-id="c99e9-154">Für Azure app Service:</span><span class="sxs-lookup"><span data-stu-id="c99e9-154">For Azure app service:</span></span>

* <span data-ttu-id="c99e9-155">Wählen Sie die **Anwendungseinstellungen** Blatt.</span><span class="sxs-lookup"><span data-stu-id="c99e9-155">Select the **Application settings** blade.</span></span>
* <span data-ttu-id="c99e9-156">Fügen Sie die Schlüssel und Wert im **Anwendungseinstellungen**.</span><span class="sxs-lookup"><span data-stu-id="c99e9-156">Add the key and value in **App settings**.</span></span>


### <a name="windows"></a><span data-ttu-id="c99e9-157">Windows</span><span class="sxs-lookup"><span data-stu-id="c99e9-157">Windows</span></span>
<span data-ttu-id="c99e9-158">Festlegen der `ASPNETCORE_ENVIRONMENT` für die aktuelle Sitzung, wenn die app gestartet wird mit `dotnet run`, werden die folgenden Befehle verwendet.</span><span class="sxs-lookup"><span data-stu-id="c99e9-158">To set the `ASPNETCORE_ENVIRONMENT` for the current session, if the app is started using `dotnet run`, the following commands are used</span></span>

<span data-ttu-id="c99e9-159">**Befehlszeile**</span><span class="sxs-lookup"><span data-stu-id="c99e9-159">**Command line**</span></span>
```
set ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="c99e9-160">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="c99e9-160">**PowerShell**</span></span>
```
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="c99e9-161">Diese Befehle wirksam, nur für das aktuelle Fenster.</span><span class="sxs-lookup"><span data-stu-id="c99e9-161">These commands take effect only for the current window.</span></span> <span data-ttu-id="c99e9-162">Wenn das Fenster geschlossen wird, wird die ASPNETCORE_ENVIRONMENT-Einstellung in der Standardeinstellung oder Computer Wert wiederhergestellt.</span><span class="sxs-lookup"><span data-stu-id="c99e9-162">When the window is closed, the ASPNETCORE_ENVIRONMENT setting reverts to the default setting or machine value.</span></span> <span data-ttu-id="c99e9-163">Um den Wert global beim Öffnen von Windows Festlegen der **Systemsteuerung** > **System** > **Erweiterte Systemeinstellungen** und hinzufügen oder Bearbeiten der `ASPNETCORE_ENVIRONMENT` Wert.</span><span class="sxs-lookup"><span data-stu-id="c99e9-163">In order to set the value globally on Windows open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value.</span></span>

![Erweiterte Eigenschaften System](environments/_static/systemsetting_environment.png)

![ASPNET-Core-Umgebungsvariable](environments/_static/windows_aspnetcore_environment.png)


<span data-ttu-id="c99e9-166">**web.config**</span><span class="sxs-lookup"><span data-stu-id="c99e9-166">**web.config**</span></span>

<span data-ttu-id="c99e9-167">Finden Sie unter der *Einstellungsumgebungsvariablen* Teil der [Konfigurationsverweis ASP.NET Core-Modul](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) Thema.</span><span class="sxs-lookup"><span data-stu-id="c99e9-167">See the *Setting environment variables* section of the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) topic.</span></span>

<span data-ttu-id="c99e9-168">**Pro IIS-Anwendungspool**</span><span class="sxs-lookup"><span data-stu-id="c99e9-168">**Per IIS Application Pool**</span></span>

<span data-ttu-id="c99e9-169">Zum Festlegen von Umgebungsvariablen für die einzelnen apps, die in isolierten Anwendungspools (für IIS 10.0 und höher unterstützt) ausgeführt wird, finden Sie unter der *AppCmd.exe Befehl* Teil der [Umgebungsvariablen \< EnvironmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) Thema.</span><span class="sxs-lookup"><span data-stu-id="c99e9-169">To set environment variables for individual apps running in isolated Application Pools (supported on IIS 10.0+), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span>

### <a name="macos"></a><span data-ttu-id="c99e9-170">macOS</span><span class="sxs-lookup"><span data-stu-id="c99e9-170">macOS</span></span>
<span data-ttu-id="c99e9-171">Festlegen der aktuellen Umgebung für MacOS kann Inline erfolgen, wenn die Anwendung ausgeführt;</span><span class="sxs-lookup"><span data-stu-id="c99e9-171">Setting the current environment for macOS can be done in-line when running the application;</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```
<span data-ttu-id="c99e9-172">oder `export` vor dem Ausführen der app festlegen.</span><span class="sxs-lookup"><span data-stu-id="c99e9-172">or using `export` to set it prior to running the app.</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="c99e9-173">Level-Umgebungsvariablen festgelegt sind, der *.bashrc* oder *.bash_profile* Datei.</span><span class="sxs-lookup"><span data-stu-id="c99e9-173">Machine level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="c99e9-174">Bearbeiten Sie die Datei mit einem beliebigen Texteditor, und fügen Sie die folgende Anweisung hinzu.</span><span class="sxs-lookup"><span data-stu-id="c99e9-174">Edit the file using any text editor and add the following statment.</span></span>

```
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a><span data-ttu-id="c99e9-175">Linux</span><span class="sxs-lookup"><span data-stu-id="c99e9-175">Linux</span></span>
<span data-ttu-id="c99e9-176">Verwenden Sie für Linux-Distributionen der `export` -Befehl an der Befehlszeile für variableneinstellungen sitzungsbasierte und *Bash_profile* -Datei für Computer auf umgebungseinstellungen.</span><span class="sxs-lookup"><span data-stu-id="c99e9-176">For Linux distros, use the `export` command at the command line for session based variable settings and *bash_profile* file for machine level environment settings.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="c99e9-177">Konfiguration nach Umgebung</span><span class="sxs-lookup"><span data-stu-id="c99e9-177">Configuration by environment</span></span>

<span data-ttu-id="c99e9-178">Finden Sie unter [Konfiguration von Umgebung](xref:fundamentals/configuration/index#configuration-by-environment) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="c99e9-178">See [Configuration by environment](xref:fundamentals/configuration/index#configuration-by-environment) for more information.</span></span>

<a name="startup-conventions"></a>
## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="c99e9-179">Basierten Umgebung Startklasse und Methoden</span><span class="sxs-lookup"><span data-stu-id="c99e9-179">Environment based Startup class and methods</span></span>

<span data-ttu-id="c99e9-180">Nach dem Start einer app ASP.NET Core der [Startklasse](xref:fundamentals/startup) startet die app.</span><span class="sxs-lookup"><span data-stu-id="c99e9-180">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="c99e9-181">Wenn eine Klasse `Startup{EnvironmentName}` vorhanden ist, dass für diese Klasse aufgerufen werden `EnvironmentName`:</span><span class="sxs-lookup"><span data-stu-id="c99e9-181">If a class `Startup{EnvironmentName}` exists, that class will be called for that `EnvironmentName`:</span></span>

[!code-csharp[Main](environments/sample/WebApp1/StartupDev.cs?name=snippet&highlight=1)]

<span data-ttu-id="c99e9-182">Hinweis: Aufrufen von [WebHostBuilder.UseStartup<TStartup> ](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) Konfigurationsabschnitte überschreibt.</span><span class="sxs-lookup"><span data-stu-id="c99e9-182">Note: Calling [WebHostBuilder.UseStartup<TStartup>](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) overrides configuration sections.</span></span>

<span data-ttu-id="c99e9-183">[Konfigurieren Sie](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) und [ConfigureServices](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) Umgebung bestimmte Versionen des Formulars unterstützen `Configure{EnvironmentName}` und `Configure{EnvironmentName}Services`:</span><span class="sxs-lookup"><span data-stu-id="c99e9-183">[Configure](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [ConfigureServices](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) support environment specific versions of the form `Configure{EnvironmentName}` and `Configure{EnvironmentName}Services`:</span></span>

[!code-csharp[Main](environments/sample/WebApp1/Startup.cs?name=snippet_all&highlight=15,37)]

## <a name="additional-resources"></a><span data-ttu-id="c99e9-184">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="c99e9-184">Additional Resources</span></span>

* [<span data-ttu-id="c99e9-185">Starten von Anwendungen</span><span class="sxs-lookup"><span data-stu-id="c99e9-185">Application startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="c99e9-186">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="c99e9-186">Configuration</span></span>](xref:fundamentals/configuration/index)
* [<span data-ttu-id="c99e9-187">IHostingEnvironment.EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="c99e9-187">IHostingEnvironment.EnvironmentName</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName)
