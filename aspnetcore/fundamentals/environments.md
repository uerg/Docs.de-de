---
title: Verwenden von mehreren Umgebungen in ASP.NET Core
author: rick-anderson
description: Erfahren Sie, wie Sie in ASP.NET Core-Apps das App-Verhalten umgebungsübergreifend steuern können.
ms.author: riande
ms.date: 07/03/2018
uid: fundamentals/environments
ms.openlocfilehash: 8983a0ce81beb16d68c799d30bfbfce6e7b693b1
ms.sourcegitcommit: 18339e3cb5a891a3ca36d8146fa83cf91c32e707
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37433947"
---
# <a name="use-multiple-environments-in-aspnet-core"></a><span data-ttu-id="fa01d-103">Verwenden von mehreren Umgebungen in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fa01d-103">Use multiple environments in ASP.NET Core</span></span>

<span data-ttu-id="fa01d-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fa01d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="fa01d-105">ASP.NET Core konfiguriert das App-Verhalten basierend auf der Laufzeitumgebung mit einer Umgebungsvariablen.</span><span class="sxs-lookup"><span data-stu-id="fa01d-105">ASP.NET Core configures app behavior based on the runtime environment using an environment variable.</span></span>

<span data-ttu-id="fa01d-106">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="fa01d-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="fa01d-107">Umgebungen</span><span class="sxs-lookup"><span data-stu-id="fa01d-107">Environments</span></span>

<span data-ttu-id="fa01d-108">ASP.NET Core liest die Umgebungsvariable `ASPNETCORE_ENVIRONMENT` beim Start der App und speichert diesen Wert unter [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname).</span><span class="sxs-lookup"><span data-stu-id="fa01d-108">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at app startup and stores the value in [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname).</span></span> <span data-ttu-id="fa01d-109">Sie können `ASPNETCORE_ENVIRONMENT` auf einen beliebigen Wert festlegen, das Framework unterstützt allerdings nur [drei Werte](/dotnet/api/microsoft.aspnetcore.hosting.environmentname): [Entwicklung](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development), [Staging](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging) und [Produktion](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production).</span><span class="sxs-lookup"><span data-stu-id="fa01d-109">You can set `ASPNETCORE_ENVIRONMENT` to any value, but [three values](/dotnet/api/microsoft.aspnetcore.hosting.environmentname) are supported by the framework: [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development), [Staging](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging), and [Production](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production).</span></span> <span data-ttu-id="fa01d-110">Wenn `ASPNETCORE_ENVIRONMENT` nicht festgelegt ist, ist der Standardwert `Production`.</span><span class="sxs-lookup"><span data-stu-id="fa01d-110">If `ASPNETCORE_ENVIRONMENT` isn't set, it defaults to `Production`.</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet)]

<span data-ttu-id="fa01d-111">Der vorangehende Code:</span><span class="sxs-lookup"><span data-stu-id="fa01d-111">The preceding code:</span></span>

* <span data-ttu-id="fa01d-112">Ruft [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) und [UseBrowserLink](/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink) auf, wenn `ASPNETCORE_ENVIRONMENT` auf `Development` festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="fa01d-112">Calls [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) and [UseBrowserLink](/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="fa01d-113">Ruft [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) auf, wenn der Wert von `ASPNETCORE_ENVIRONMENT` auf einen der folgenden Werte festgelegt ist:</span><span class="sxs-lookup"><span data-stu-id="fa01d-113">Calls [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

    * `Staging`
    * `Production`
    * `Staging_2`

<span data-ttu-id="fa01d-114">Das [Umgebungstaghilfsprogramm](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) verwendet den Wert von `IHostingEnvironment.EnvironmentName` zum Einschließen oder Ausschließen von Markup im Element:</span><span class="sxs-lookup"><span data-stu-id="fa01d-114">The [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

<span data-ttu-id="fa01d-115">Unter Windows und macOS wird bei Umgebungsvariablen und Werten die Groß-/Kleinschreibung nicht beachtet.</span><span class="sxs-lookup"><span data-stu-id="fa01d-115">On Windows and macOS, environment variables and values aren't case sensitive.</span></span> <span data-ttu-id="fa01d-116">Bei Linux-Umgebungsvariablen und -Werten **wird die Groß-/Kleinschreibung standardmäßig beachtet**.</span><span class="sxs-lookup"><span data-stu-id="fa01d-116">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="fa01d-117">Entwicklung</span><span class="sxs-lookup"><span data-stu-id="fa01d-117">Development</span></span>

<span data-ttu-id="fa01d-118">In der Entwicklungsumgebung können Features aktiviert werden, die in der Produktion nicht verfügbar gemacht werden sollten.</span><span class="sxs-lookup"><span data-stu-id="fa01d-118">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="fa01d-119">Mit den ASP.NET Core-Vorlagen wird beispielsweise die [Seite mit Ausnahmen für Entwickler](xref:fundamentals/error-handling#the-developer-exception-page) in der Entwicklungsumgebung aktiviert.</span><span class="sxs-lookup"><span data-stu-id="fa01d-119">For example, the ASP.NET Core templates enable the [developer exception page](xref:fundamentals/error-handling#the-developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="fa01d-120">Die Umgebung für die Entwicklung lokaler Computer kann in der Datei *Properties\launchSettings.json* des Projekts festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="fa01d-120">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="fa01d-121">Mit in der Datei *launchSettings.json* festgelegten Umgebungsvariablen werden in der Systemumgebung festgelegte Werte überschrieben.</span><span class="sxs-lookup"><span data-stu-id="fa01d-121">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="fa01d-122">Die folgende JSON zeigt drei Profile aus der Datei *launchSettings.json* an:</span><span class="sxs-lookup"><span data-stu-id="fa01d-122">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

```json
{
  "iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iisExpress": {
      "applicationUrl": "http://localhost:54339/",
      "sslPort": 0
    }
  },
  "profiles": {
    "IIS Express": {
      "commandName": "IISExpress",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_My_Environment": "1",
        "ASPNETCORE_DETAILEDERRORS": "1",
        "ASPNETCORE_ENVIRONMENT": "Staging"
      }
    },
    "EnvironmentsSample": {
      "commandName": "Project",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Staging"
      },
      "applicationUrl": "http://localhost:54340/"
    },
    "Kestrel Staging": {
      "commandName": "Project",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_My_Environment": "1",
        "ASPNETCORE_DETAILEDERRORS": "1",
        "ASPNETCORE_ENVIRONMENT": "Staging"
      },
      "applicationUrl": "http://localhost:51997/"
    }
  }
}
```

::: moniker range=">= aspnetcore-2.1"

> [!NOTE]
> <span data-ttu-id="fa01d-123">Die Eigenschaft `applicationUrl` in *launchSettings.json* kann eine Liste von Server-URLs angeben.</span><span class="sxs-lookup"><span data-stu-id="fa01d-123">The `applicationUrl` property in *launchSettings.json* can specify a list of server URLs.</span></span> <span data-ttu-id="fa01d-124">Verwenden Sie ein Semikolon zwischen den URLs in der Liste:</span><span class="sxs-lookup"><span data-stu-id="fa01d-124">Use a semicolon between the URLs in the list:</span></span>
>
> ```json
> "EnvironmentsSample": {
>    "commandName": "Project",
>    "launchBrowser": true,
>    "applicationUrl": "https://localhost:5001;http://localhost:5000",
>    "environmentVariables": {
>      "ASPNETCORE_ENVIRONMENT": "Development"
>    }
> }
> ```

::: moniker-end

<span data-ttu-id="fa01d-125">Wenn die Anwendung mit [dotnet run](/dotnet/core/tools/dotnet-run) gestartet wird, wird das erste Profil mit `"commandName": "Project"` verwendet.</span><span class="sxs-lookup"><span data-stu-id="fa01d-125">When the app is launched with [dotnet run](/dotnet/core/tools/dotnet-run), the first profile with `"commandName": "Project"` is used.</span></span> <span data-ttu-id="fa01d-126">Der Wert von `commandName` gibt den zu startenden Webserver an.</span><span class="sxs-lookup"><span data-stu-id="fa01d-126">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="fa01d-127">`commandName` kann einer der folgenden sein:</span><span class="sxs-lookup"><span data-stu-id="fa01d-127">`commandName` can be any one of the following:</span></span>

* <span data-ttu-id="fa01d-128">IIS Express</span><span class="sxs-lookup"><span data-stu-id="fa01d-128">IIS Express</span></span>
* <span data-ttu-id="fa01d-129">IIS</span><span class="sxs-lookup"><span data-stu-id="fa01d-129">IIS</span></span>
* <span data-ttu-id="fa01d-130">Projekt (über das Kestrel gestartet wird)</span><span class="sxs-lookup"><span data-stu-id="fa01d-130">Project (which launches Kestrel)</span></span>

<span data-ttu-id="fa01d-131">Beim Start einer App mit [dotnet run](/dotnet/core/tools/dotnet-run) tritt Folgendes ein:</span><span class="sxs-lookup"><span data-stu-id="fa01d-131">When an app is launched with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

* <span data-ttu-id="fa01d-132">Die Datei *launchSettings.json* wird, sofern verfügbar, gelesen.</span><span class="sxs-lookup"><span data-stu-id="fa01d-132">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="fa01d-133">Durch `environmentVariables`-Einstellungen in der Datei *launchSettings.json* werden Umgebungsvariablen überschrieben.</span><span class="sxs-lookup"><span data-stu-id="fa01d-133">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="fa01d-134">Die Hostingumgebung wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="fa01d-134">The hosting environment is displayed.</span></span>

<span data-ttu-id="fa01d-135">Die folgende Ausgabe zeigt eine App, die mit [dotnet run](/dotnet/core/tools/dotnet-run) gestartet wurde:</span><span class="sxs-lookup"><span data-stu-id="fa01d-135">The following output shows an app started with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

```bash
PS C:\Websites\EnvironmentsSample> dotnet run
Using launch settings from C:\Websites\EnvironmentsSample\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Websites\EnvironmentsSample
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="fa01d-136">Auf der Registerkarte **Debuggen** der Visual Studio-Projekteigenschaften wird eine grafische Benutzeroberfläche für die Bearbeitung der Datei *launchSettings.json* bereitgestellt:</span><span class="sxs-lookup"><span data-stu-id="fa01d-136">The Visual Studio project properties **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![Projekteigenschaften zum Festlegen von Umgebungsvariablen](environments/_static/project-properties-debug.png)

<span data-ttu-id="fa01d-138">An Projektprofilen vorgenommene Änderungen werden möglicherweise erst nach einem Neustart des Webservers wirksam.</span><span class="sxs-lookup"><span data-stu-id="fa01d-138">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="fa01d-139">Kestrel muss neu gestartet werden, bevor es an der Umgebung vorgenommene Änderungen erkennen kann.</span><span class="sxs-lookup"><span data-stu-id="fa01d-139">Kestrel must be restarted before it can detect changes made to its environment.</span></span>

> [!WARNING]
> <span data-ttu-id="fa01d-140">In der Datei *launchSettings.json* sollten keine geheimen Schlüssel gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="fa01d-140">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="fa01d-141">Mit dem [Secret Manager-Tool](xref:security/app-secrets) können geheime Schlüssel für die lokale Umgebung gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="fa01d-141">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

<span data-ttu-id="fa01d-142">Wenn Sie [Visual Studio Code](https://code.visualstudio.com/) verwenden, können Umgebungsvariablen in der Datei *.vscode/launch.json* festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="fa01d-142">When using [Visual Studio Code](https://code.visualstudio.com/), environment variables can be set in the *.vscode/launch.json* file.</span></span> <span data-ttu-id="fa01d-143">Im folgenden Beispiel wird die Umgebung auf `Development` festgelegt:</span><span class="sxs-lookup"><span data-stu-id="fa01d-143">The following example sets the environment to `Development`:</span></span>

```json
{
   "version": "0.2.0",
   "configurations": [
        {
            "name": ".NET Core Launch (web)",

            ... additional VS Code configuration settings ...

            "env": {
                "ASPNETCORE_ENVIRONMENT": "Development"
            }
        }
    ]
}
```

<span data-ttu-id="fa01d-144">Die Datei *.vscode/launch.json* im Projekt wird nicht gelesen, wenn die App mit `dotnet run` genauso wie *Properties/launchSettings.json* gestartet wird.</span><span class="sxs-lookup"><span data-stu-id="fa01d-144">A *.vscode/launch.json* file in the project isn't read when starting the app with `dotnet run` in the same way as *Properties/launchSettings.json*.</span></span> <span data-ttu-id="fa01d-145">Wenn Sie eine App in der Entwicklung starten, die keine *launchSettings.json*-Datei enthält, legen Sie die Umgebung mit einer Umgebungsvariablen oder einem Befehlszeilenargument auf den `dotnet run`-Befehl fest.</span><span class="sxs-lookup"><span data-stu-id="fa01d-145">When launching an app in development that doesn't have a *launchSettings.json* file, either set the environment with an environment variable or a command-line argument to the `dotnet run` command.</span></span>

### <a name="production"></a><span data-ttu-id="fa01d-146">Produktion</span><span class="sxs-lookup"><span data-stu-id="fa01d-146">Production</span></span>

<span data-ttu-id="fa01d-147">Die Produktionsumgebung sollte so konfiguriert werden, dass Sicherheit, Leistung und Stabilität der App maximiert werden.</span><span class="sxs-lookup"><span data-stu-id="fa01d-147">The production environment should be configured to maximize security, performance, and app robustness.</span></span> <span data-ttu-id="fa01d-148">Allgemeine Einstellungen, die sich von der Entwicklung unterscheiden, sind zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="fa01d-148">Some common settings that differ from development include:</span></span>

* <span data-ttu-id="fa01d-149">Zwischenspeicherung.</span><span class="sxs-lookup"><span data-stu-id="fa01d-149">Caching.</span></span>
* <span data-ttu-id="fa01d-150">Clientseitige Ressourcen werden gebündelt, verkleinert und potenziell von einem CDN bedient.</span><span class="sxs-lookup"><span data-stu-id="fa01d-150">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="fa01d-151">Seiten zur Fehlerdiagnose sind deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="fa01d-151">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="fa01d-152">Angezeigte Fehlerseiten sind aktiviert.</span><span class="sxs-lookup"><span data-stu-id="fa01d-152">Friendly error pages enabled.</span></span>
* <span data-ttu-id="fa01d-153">Die Produktionsprotokollierung und -überwachung ist aktiviert.</span><span class="sxs-lookup"><span data-stu-id="fa01d-153">Production logging and monitoring enabled.</span></span> <span data-ttu-id="fa01d-154">Beispiel: [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="fa01d-154">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="fa01d-155">Festlegen der Umgebung</span><span class="sxs-lookup"><span data-stu-id="fa01d-155">Set the environment</span></span>

<span data-ttu-id="fa01d-156">Es ist oft nützlich, zu Testzwecken eine bestimmte Umgebung festzulegen.</span><span class="sxs-lookup"><span data-stu-id="fa01d-156">It's often useful to set a specific environment for testing.</span></span> <span data-ttu-id="fa01d-157">Wenn die Umgebung nicht festgelegt ist, wird `Production` als Standardwert verwendet, wodurch die meisten Debugfeatures deaktiviert werden.</span><span class="sxs-lookup"><span data-stu-id="fa01d-157">If the environment isn't set, it defaults to `Production`, which disables most debugging features.</span></span> <span data-ttu-id="fa01d-158">Welche Methode zum Festlegen der Umgebung verwendet wird, hängt vom Betriebssystem ab.</span><span class="sxs-lookup"><span data-stu-id="fa01d-158">The method for setting the environment depends on the operating system.</span></span>

### <a name="azure-app-service"></a><span data-ttu-id="fa01d-159">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="fa01d-159">Azure App Service</span></span>

<span data-ttu-id="fa01d-160">Führen Sie die folgenden Schritte durch, um die Umgebung in [Azure App Service](https://azure.microsoft.com/services/app-service/) festzulegen:</span><span class="sxs-lookup"><span data-stu-id="fa01d-160">To set the environment in [Azure App Service](https://azure.microsoft.com/services/app-service/), perform the following steps:</span></span>

1. <span data-ttu-id="fa01d-161">Wählen Sie die App auf dem Blatt **App Services** aus.</span><span class="sxs-lookup"><span data-stu-id="fa01d-161">Select the app from the **App Services** blade.</span></span>
1. <span data-ttu-id="fa01d-162">Wählen Sie in der Gruppe **EINSTELLUNGEN** das Blatt **Anwendungseinstellung** aus.</span><span class="sxs-lookup"><span data-stu-id="fa01d-162">In the **SETTINGS** group, select the **Application settings** blade.</span></span>
1. <span data-ttu-id="fa01d-163">Wählen Sie im Bereich **Anwendungseinstellung** **Neue Einstellung hinzufügen** aus.</span><span class="sxs-lookup"><span data-stu-id="fa01d-163">In the **Application settings** area, select **Add new setting**.</span></span>
1. <span data-ttu-id="fa01d-164">Geben Sie `ASPNETCORE_ENVIRONMENT` als Namen ein.</span><span class="sxs-lookup"><span data-stu-id="fa01d-164">For **Enter a name**, provide `ASPNETCORE_ENVIRONMENT`.</span></span> <span data-ttu-id="fa01d-165">Geben Sie die Umgebung als Wert an (z.B. `Staging`).</span><span class="sxs-lookup"><span data-stu-id="fa01d-165">For **Enter a value**, provide the environment (for example, `Staging`).</span></span>
1. <span data-ttu-id="fa01d-166">Aktivieren Sie das Kontrollkästchen **Sloteinstellung**, wenn Sie möchten, dass die Umgebungseinstellung im aktuellen Slot bleibt, wenn Bereitstellungsslots getauscht werden.</span><span class="sxs-lookup"><span data-stu-id="fa01d-166">Select the **Slot Setting** check box if you wish the environment setting to remain with the current slot when deployment slots are swapped.</span></span> <span data-ttu-id="fa01d-167">Weitere Informationen finden Sie in der [Azure-Dokumentation: Einrichten von Stagingumgebungen in Azure App Service](/azure/app-service/web-sites-staged-publishing).</span><span class="sxs-lookup"><span data-stu-id="fa01d-167">For more information, see [Azure Documentation: Which settings are swapped?](/azure/app-service/web-sites-staged-publishing).</span></span>
1. <span data-ttu-id="fa01d-168">Klicken Sie oben auf dem Blatt auf **Speichern**.</span><span class="sxs-lookup"><span data-stu-id="fa01d-168">Select **Save** at the top of the blade.</span></span>

<span data-ttu-id="fa01d-169">Azure App Service startet die App automatisch neu, nachdem eine App-Einstellung (Umgebungsvariable) im Azure-Portal hinzugefügt, geändert oder gelöscht wurde.</span><span class="sxs-lookup"><span data-stu-id="fa01d-169">Azure App Service automatically restarts the app after an app setting (environment variable) is added, changed, or deleted in the Azure portal.</span></span>

### <a name="windows"></a><span data-ttu-id="fa01d-170">Windows</span><span class="sxs-lookup"><span data-stu-id="fa01d-170">Windows</span></span>

<span data-ttu-id="fa01d-171">Zum Festlegen der `ASPNETCORE_ENVIRONMENT` für die aktuelle Sitzung werden folgende Befehle verwendet, wenn die App mit [dotnet run](/dotnet/core/tools/dotnet-run) gestartet wird:</span><span class="sxs-lookup"><span data-stu-id="fa01d-171">To set the `ASPNETCORE_ENVIRONMENT` for the current session when the app is started using [dotnet run](/dotnet/core/tools/dotnet-run), the following commands are used:</span></span>

<span data-ttu-id="fa01d-172">**Eingabeaufforderung**</span><span class="sxs-lookup"><span data-stu-id="fa01d-172">**Command prompt**</span></span>

```console
set ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="fa01d-173">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="fa01d-173">**PowerShell**</span></span>

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="fa01d-174">Diese Befehle sind nur für das aktuelle Fenster wirksam.</span><span class="sxs-lookup"><span data-stu-id="fa01d-174">These commands only take effect for the current window.</span></span> <span data-ttu-id="fa01d-175">Wenn das Fenster geschlossen wird, wird die Einstellung `ASPNETCORE_ENVIRONMENT` auf die Standardeinstellung oder den Computerwert zurückgesetzt.</span><span class="sxs-lookup"><span data-stu-id="fa01d-175">When the window is closed, the `ASPNETCORE_ENVIRONMENT` setting reverts to the default setting or machine value.</span></span> <span data-ttu-id="fa01d-176">Wenn Sie den Wert global unter Windows festlegen möchten, müssen Sie zu **Systemsteuerung** > **System** > **Erweiterte Systemeinstellungen** navigieren und den Wert `ASPNETCORE_ENVIRONMENT` hinzufügen oder bearbeiten:</span><span class="sxs-lookup"><span data-stu-id="fa01d-176">To set the value globally in Windows, open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value:</span></span>

![Erweiterte Systemeigenschaften](environments/_static/systemsetting_environment.png)

![ASP.NET Core-Umgebungsvariable](environments/_static/windows_aspnetcore_environment.png)

<span data-ttu-id="fa01d-179">**web.config**</span><span class="sxs-lookup"><span data-stu-id="fa01d-179">**web.config**</span></span>

<span data-ttu-id="fa01d-180">Weitere Informationen finden Sie im Abschnitt *Festlegen der Umgebungsvariablen* im Artikel [Verweis auf die Konfiguration des ASP.NET Core-Moduls](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span><span class="sxs-lookup"><span data-stu-id="fa01d-180">See the *Setting environment variables* section of the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) topic.</span></span>

<span data-ttu-id="fa01d-181">**Pro IIS-Anwendungspool**</span><span class="sxs-lookup"><span data-stu-id="fa01d-181">**Per IIS Application Pool**</span></span>

<span data-ttu-id="fa01d-182">Wenn Sie Umgebungsvariablen für einzelne Apps festlegen möchten, die in isolierten Anwendungspools ausgeführt werden (unterstützt für IIS 10.0 und höher), finden Sie weitere Informationen im Abschnitt zum *Befehl „AppCmd.exe“* im Artikel [Umgebungsvariablen &lt; EnvironmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe).</span><span class="sxs-lookup"><span data-stu-id="fa01d-182">To set environment variables for individual apps running in isolated Application Pools (supported on IIS 10.0+), see the *AppCmd.exe command* section of the [Environment Variables &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span>

### <a name="macos"></a><span data-ttu-id="fa01d-183">macOS</span><span class="sxs-lookup"><span data-stu-id="fa01d-183">macOS</span></span>

<span data-ttu-id="fa01d-184">Das Festlegen der aktuellen Umgebung für macOS kann inline während der Ausführung der App erfolgen:</span><span class="sxs-lookup"><span data-stu-id="fa01d-184">Setting the current environment for macOS can be performed in-line when running the app:</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

<span data-ttu-id="fa01d-185">Legen Sie die Umgebung alternativ mit `export` vor der Ausführung der App fest:</span><span class="sxs-lookup"><span data-stu-id="fa01d-185">Alternatively, set the environment with `export` prior to running the app:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="fa01d-186">Umgebungsvariablen auf Computerebene werden in der *BASHRC*- oder *BASH_PROFILE*-Datei festgelegt.</span><span class="sxs-lookup"><span data-stu-id="fa01d-186">Machine-level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="fa01d-187">Bearbeiten Sie die Datei mit einem beliebigen Text-Editor.</span><span class="sxs-lookup"><span data-stu-id="fa01d-187">Edit the file using any text editor.</span></span> <span data-ttu-id="fa01d-188">Fügen Sie die folgende Anweisung hinzu:</span><span class="sxs-lookup"><span data-stu-id="fa01d-188">Add the following statement:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a><span data-ttu-id="fa01d-189">Linux</span><span class="sxs-lookup"><span data-stu-id="fa01d-189">Linux</span></span>

<span data-ttu-id="fa01d-190">Verwenden Sie bei Linux-Distributionen für sitzungsbasierte Variableneinstellungen den Befehl `export` in der Befehlszeile und die *BASH-PROFILE*-Datei für Umgebungseinstellungen auf Computerebene.</span><span class="sxs-lookup"><span data-stu-id="fa01d-190">For Linux distros, use the `export` command at a command prompt for session-based variable settings and *bash_profile* file for machine-level environment settings.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="fa01d-191">Konfiguration nach Umgebung</span><span class="sxs-lookup"><span data-stu-id="fa01d-191">Configuration by environment</span></span>

<span data-ttu-id="fa01d-192">Weitere Informationen finden Sie unter [Konfiguration nach Umgebung](xref:fundamentals/configuration/index#configuration-by-environment).</span><span class="sxs-lookup"><span data-stu-id="fa01d-192">See [Configuration by environment](xref:fundamentals/configuration/index#configuration-by-environment) for more information.</span></span>

## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="fa01d-193">Umgebungsbasierte Startklasse und Methoden</span><span class="sxs-lookup"><span data-stu-id="fa01d-193">Environment-based Startup class and methods</span></span>

<span data-ttu-id="fa01d-194">Nach dem Start einer ASP.NET Core-App lädt die [Startklasse](xref:fundamentals/startup) die App.</span><span class="sxs-lookup"><span data-stu-id="fa01d-194">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="fa01d-195">Wenn eine `Startup{EnvironmentName}`-Klasse vorhanden ist, wird diese Klasse für diesen `EnvironmentName` aufgerufen:</span><span class="sxs-lookup"><span data-stu-id="fa01d-195">If a `Startup{EnvironmentName}` class exists, the class is called for that `EnvironmentName`:</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/StartupDev.cs?name=snippet&highlight=1)]

<span data-ttu-id="fa01d-196">[WebHostBuilder.UseStartup&lt;TStartup&gt;](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) überschreibt Konfigurationsabschnitte.</span><span class="sxs-lookup"><span data-stu-id="fa01d-196">[WebHostBuilder.UseStartup&lt;TStartup&gt;](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) overrides configuration sections.</span></span>

<span data-ttu-id="fa01d-197">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) und [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) unterstützen umgebungsspezifische Versionen der Form `Configure{EnvironmentName}` und `Configure{EnvironmentName}Services`:</span><span class="sxs-lookup"><span data-stu-id="fa01d-197">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) support environment-specific versions of the form `Configure{EnvironmentName}` and `Configure{EnvironmentName}Services`:</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet_all&highlight=15,51)]

## <a name="additional-resources"></a><span data-ttu-id="fa01d-198">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="fa01d-198">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>
* [<span data-ttu-id="fa01d-199">IHostingEnvironment.EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="fa01d-199">IHostingEnvironment.EnvironmentName</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname)
