---
title: Arbeiten mit mehreren Umgebungen in ASP.NET Core
author: rick-anderson
description: Erfahren Sie, wie ASP.NET Core umgebungsübergreifend Unterstützung für das Steuern des App-Verhaltens bereitstellt.
manager: wpickett
ms.author: riande
ms.date: 12/25/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/environments
ms.openlocfilehash: b9c3b8a15424ca637a2486450bfdde2762204935
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/03/2018
---
# <a name="work-with-multiple-environments-in-aspnet-core"></a>Arbeiten mit mehreren Umgebungen in ASP.NET Core

Von [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core bietet Unterstützung für das Festlegen des App-Verhaltens zur Laufzeit mit Umgebungsvariablen.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

## <a name="environments"></a>Umgebungen

ASP.NET Core liest die Umgebungsvariable `ASPNETCORE_ENVIRONMENT` beim Start der App und speichert diesen Wert unter [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName). `ASPNETCORE_ENVIRONMENT` kann auf einen beliebigen Wert festgelegt werden, das Framework unterstützt jedoch [drei Werte](/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0): [Entwicklung](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [Staging](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0) und [Produktion](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0). Wenn `ASPNETCORE_ENVIRONMENT` nicht festgelegt ist, wird `Production` als Standard verwendet.

[!code-csharp[](environments/sample/WebApp1/Startup.cs?name=snippet)]

Der vorangehende Code:

* Ruft [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) und [UseBrowserLink](/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) auf, wenn `ASPNETCORE_ENVIRONMENT` auf `Development` festgelegt ist.
* Ruft [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) auf, wenn der Wert von `ASPNETCORE_ENVIRONMENT` auf einen der folgenden Werte festgelegt ist:

    * `Staging`
    * `Production`
    * `Staging_2`

Das [Umgebungstaghilfsprogramm](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) verwendet den Wert von `IHostingEnvironment.EnvironmentName` zum Einschließen oder Ausschließen von Markup im Element:

[!code-html[](environments/sample/WebApp1/Pages/About.cshtml)]

Hinweis: Unter Windows und macOS wird bei Umgebungsvariablen und Werten die Groß-/Kleinschreibung nicht beachtet. Bei Linux-Umgebungsvariablen und -Werten **wird die Groß-/Kleinschreibung standardmäßig beachtet**.

### <a name="development"></a>Entwicklung

In der Entwicklungsumgebung können Features aktiviert werden, die in der Produktion nicht verfügbar gemacht werden sollten. Mit den ASP.NET Core-Vorlagen wird beispielsweise die [Seite mit Ausnahmen für Entwickler](xref:fundamentals/error-handling#the-developer-exception-page) in der Entwicklungsumgebung aktiviert.

Die Umgebung für die Entwicklung lokaler Computer kann in der Datei *Properties\launchSettings.json* des Projekts festgelegt werden. Mit in der Datei *launchSettings.json* festgelegten Umgebungsvariablen werden in der Systemumgebung festgelegte Werte überschrieben.

Die folgende JSON zeigt drei Profile aus der Datei *launchSettings.json* an:

[!code-json[](environments/sample/WebApp1/Properties/launchSettings.json?highlight=10,11,18,26)]

::: moniker range=">= aspnetcore-2.1"
> [!NOTE]
> Die Eigenschaft `applicationUrl` in *launchSettings.json* kann eine Liste von Server-URLs angeben. Verwenden Sie ein Semikolon zwischen den URLs in der Liste:
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

Wenn die Anwendung mit [dotnet run](/dotnet/core/tools/dotnet-run) gestartet wird, wird das erste Profil mit `"commandName": "Project"` verwendet. Der Wert von `commandName` gibt den zu startenden Webserver an. `commandName` kann Teil von Folgendem sein:

* IIS Express
* IIS
* Projekt (über das Kestrel gestartet wird)

Beim Start einer App mit [dotnet run](/dotnet/core/tools/dotnet-run) tritt Folgendes ein:

* Die Datei *launchSettings.json* wird, sofern verfügbar, gelesen. Durch `environmentVariables`-Einstellungen in der Datei *launchSettings.json* werden Umgebungsvariablen überschrieben.
* Die Hostingumgebung wird angezeigt.


Die folgende Ausgabe zeigt eine App, die mit [dotnet run](/dotnet/core/tools/dotnet-run) gestartet wurde:
```bash
PS C:\Webs\WebApp1> dotnet run
Using launch settings from C:\Webs\WebApp1\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Webs\WebApp1
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

Auf der Registerkarte **Debuggen** in Visual Studio wird eine grafische Benutzeroberfläche für die Bearbeitung der Datei *launchSettings.json* bereitgestellt:

![Projekteigenschaften zum Festlegen von Umgebungsvariablen](environments/_static/project-properties-debug.png)

An Projektprofilen vorgenommene Änderungen werden möglicherweise erst nach einem Neustart des Webservers wirksam. Kestrel muss neu gestartet werden, bevor es an der Umgebung vorgenommene Änderungen erkennen kann.

>[!WARNING]
> In der Datei *launchSettings.json* sollten keine geheimen Schlüssel gespeichert werden. Mit dem [Secret Manager-Tool](xref:security/app-secrets) können geheime Schlüssel für die lokale Umgebung gespeichert werden.

### <a name="production"></a>Produktion

Die Produktionsumgebung sollte so konfiguriert werden, dass Sicherheit, Leistung und Stabilität der App maximiert werden. Allgemeine Einstellungen, die sich von der Entwicklung unterscheiden, sind zum Beispiel:

* Zwischenspeicherung.
* Clientseitige Ressourcen werden gebündelt, verkleinert und potenziell von einem CDN bedient.
* Seiten zur Fehlerdiagnose sind deaktiviert.
* Angezeigte Fehlerseiten sind aktiviert.
* Die Produktionsprotokollierung und -überwachung ist aktiviert. Beispiel: [Application Insights](/azure/application-insights/app-insights-asp-net-core).

## <a name="setting-the-environment"></a>Festlegen der Umgebung

Es ist oft nützlich, zu Testzwecken eine bestimmte Umgebung festzulegen. Wenn die Umgebung nicht festgelegt ist, wird `Production` als Standardwert verwendet, wodurch die meisten Debugfeatures deaktiviert werden.

Welche Methode zum Festlegen der Umgebung verwendet wird, hängt vom Betriebssystem ab.

### <a name="azure"></a>Azure

Bei dem Azure App Service:

* Wählen Sie das Blatt **Anwendungseinstellungen** aus.
* Fügen Sie den Schlüssel und den Wert unter **Anwendungseinstellungen** hinzu.


### <a name="windows"></a>Windows
Zum Festlegen der `ASPNETCORE_ENVIRONMENT` für die aktuelle Sitzung werden folgende Befehle verwendet, wenn die App mit [dotnet run](/dotnet/core/tools/dotnet-run) gestartet wird:

**Befehlszeile**
```
set ASPNETCORE_ENVIRONMENT=Development
```
**PowerShell**
```
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

Diese Befehle sind nur für das aktuelle Fenster wirksam. Wenn das Fenster geschlossen wird, wird die Einstellung ASPNETCORE_ENVIRONMENT auf die Standardeinstellung oder den Computerwert zurückgesetzt. Wenn Sie den Wert global unter Windows festlegen möchten, müssen Sie die **Systemsteuerung** > **System** > **Erweiterte Systemeinstellungen** öffnen und den Wert `ASPNETCORE_ENVIRONMENT` hinzufügen oder bearbeiten.

![Erweiterte Systemeigenschaften](environments/_static/systemsetting_environment.png)

![ASP.NET Core-Umgebungsvariable](environments/_static/windows_aspnetcore_environment.png)


**web.config**

Weitere Informationen finden Sie im Abschnitt *Festlegen der Umgebungsvariablen* im Artikel [Verweis auf die Konfiguration des ASP.NET Core-Moduls](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).

**Pro IIS-Anwendungspool**

Wenn Sie Umgebungsvariablen für einzelne Apps festlegen möchten, die in isolierten Anwendungspools ausgeführt werden (unterstützt für IIS 10.0 und höher), finden Sie weitere Informationen im Abschnitt zum *Befehl „AppCmd.exe“* im Artikel [Umgebungsvariablen \< EnvironmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe).

### <a name="macos"></a>macOS
Die aktuelle Umgebung für macOS kann inline während der Ausführung der App erfolgen

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```
oder mit `export`, um die Umgebung vor Ausführung der App festzulegen.

```bash
export ASPNETCORE_ENVIRONMENT=Development
```
Umgebungsvariablen auf Computerebene werden in der Datei *.bashrc* oder *.bash_profile* festgelegt. Bearbeiten Sie die Datei mit einem beliebigen Text-Editor, und fügen Sie die folgende Anweisung hinzu.

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a>Linux
Verwenden Sie bei Linux-Distributionen für sitzungsbasierte Variableneinstellungen den Befehl `export` in der Befehlszeile und die Datei *bash_profile* für Umgebungseinstellungen auf Computerebene.

### <a name="configuration-by-environment"></a>Konfiguration nach Umgebung

Weitere Informationen finden Sie unter [Konfiguration nach Umgebung](xref:fundamentals/configuration/index#configuration-by-environment).

<a name="startup-conventions"></a>
## <a name="environment-based-startup-class-and-methods"></a>Umgebungsbasierte Startklasse und Methoden

Nach dem Start einer ASP.NET Core-App lädt die [Startklasse](xref:fundamentals/startup) die App. Wenn eine `Startup{EnvironmentName}`-Klasse vorhanden ist, wird diese Klasse für diesen `EnvironmentName` aufgerufen:

[!code-csharp[](environments/sample/WebApp1/StartupDev.cs?name=snippet&highlight=1)]

Hinweis: Durch das Aufrufen von [WebHostBuilder.UseStartup<TStartup>](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) werden Konfigurationsabschnitte überschrieben.

[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) und [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) unterstützen umgebungsspezifische Versionen der Form `Configure{EnvironmentName}` und `Configure{EnvironmentName}Services`:

[!code-csharp[](environments/sample/WebApp1/Startup.cs?name=snippet_all&highlight=15,37)]

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Starten von Anwendungen](xref:fundamentals/startup)
* [Konfiguration](xref:fundamentals/configuration/index)
* [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName)
