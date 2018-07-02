---
title: Verwenden von mehreren Umgebungen in ASP.NET Core
author: rick-anderson
description: Erfahren Sie, wie Sie in ASP.NET Core-Apps das App-Verhalten umgebungsübergreifend steuern können.
ms.author: riande
ms.date: 06/21/2018
uid: fundamentals/environments
ms.openlocfilehash: 505f19d8b4df6e476b46a1fe7c49872d3c4acc1a
ms.sourcegitcommit: e22097b84d26a812cd1380a6b2d12c93e522c125
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/22/2018
ms.locfileid: "36314103"
---
# <a name="use-multiple-environments-in-aspnet-core"></a>Verwenden von mehreren Umgebungen in ASP.NET Core

Von [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core konfiguriert das App-Verhalten basierend auf der Laufzeitumgebung mit einer Umgebungsvariablen.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

## <a name="environments"></a>Umgebungen

ASP.NET Core liest die Umgebungsvariable `ASPNETCORE_ENVIRONMENT` beim Start der App und speichert diesen Wert unter [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname). Sie können `ASPNETCORE_ENVIRONMENT` auf einen beliebigen Wert festlegen, das Framework unterstützt allerdings nur [drei Werte](/dotnet/api/microsoft.aspnetcore.hosting.environmentname): [Entwicklung](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development), [Staging](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging) und [Produktion](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production). Wenn `ASPNETCORE_ENVIRONMENT` nicht festgelegt ist, ist der Standardwert `Production`.

[!code-csharp[](environments/sample/WebApp1/Startup.cs?name=snippet)]

Der vorangehende Code:

* Ruft [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) und [UseBrowserLink](/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink) auf, wenn `ASPNETCORE_ENVIRONMENT` auf `Development` festgelegt ist.
* Ruft [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) auf, wenn der Wert von `ASPNETCORE_ENVIRONMENT` auf einen der folgenden Werte festgelegt ist:

    * `Staging`
    * `Production`
    * `Staging_2`

Das [Umgebungstaghilfsprogramm](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) verwendet den Wert von `IHostingEnvironment.EnvironmentName` zum Einschließen oder Ausschließen von Markup im Element:

[!code-cshtml[](environments/sample-snapshot/WebApp1/Pages/About.cshtml)]

Unter Windows und macOS wird bei Umgebungsvariablen und Werten die Groß-/Kleinschreibung nicht beachtet. Bei Linux-Umgebungsvariablen und -Werten **wird die Groß-/Kleinschreibung standardmäßig beachtet**.

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

Wenn die Anwendung mit [dotnet run](/dotnet/core/tools/dotnet-run) gestartet wird, wird das erste Profil mit `"commandName": "Project"` verwendet. Der Wert von `commandName` gibt den zu startenden Webserver an. `commandName` kann einer der folgenden sein:

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

> [!WARNING]
> In der Datei *launchSettings.json* sollten keine geheimen Schlüssel gespeichert werden. Mit dem [Secret Manager-Tool](xref:security/app-secrets) können geheime Schlüssel für die lokale Umgebung gespeichert werden.

Wenn Sie [Visual Studio Code](https://code.visualstudio.com/) verwenden, können Umgebungsvariablen in der Datei *.vscode/launch.json* festgelegt werden. Im folgenden Beispiel wird die Umgebung auf `Development` festgelegt:

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

Die Datei *.vscode/launch.json* im Projekt wird nicht gelesen, wenn die App mit `dotnet run` genauso wie *Properties/launchSettings.json* gestartet wird. Wenn Sie eine App in der Entwicklung starten, die keine *launchSettings.json*-Datei enthält, legen Sie die Umgebung mit einer Umgebungsvariablen oder einem Befehlszeilenargument auf den `dotnet run`-Befehl fest.

### <a name="production"></a>Produktion

Die Produktionsumgebung sollte so konfiguriert werden, dass Sicherheit, Leistung und Stabilität der App maximiert werden. Allgemeine Einstellungen, die sich von der Entwicklung unterscheiden, sind zum Beispiel:

* Zwischenspeicherung.
* Clientseitige Ressourcen werden gebündelt, verkleinert und potenziell von einem CDN bedient.
* Seiten zur Fehlerdiagnose sind deaktiviert.
* Angezeigte Fehlerseiten sind aktiviert.
* Die Produktionsprotokollierung und -überwachung ist aktiviert. Beispiel: [Application Insights](/azure/application-insights/app-insights-asp-net-core).

## <a name="setting-the-environment"></a>Festlegen der Umgebung

Es ist oft nützlich, zu Testzwecken eine bestimmte Umgebung festzulegen. Wenn die Umgebung nicht festgelegt ist, wird `Production` als Standardwert verwendet, wodurch die meisten Debugfeatures deaktiviert werden. Welche Methode zum Festlegen der Umgebung verwendet wird, hängt vom Betriebssystem ab.

### <a name="azure-app-service"></a>Azure App Service

Führen Sie die folgenden Schritte durch, um die Umgebung in [Azure App Service](https://azure.microsoft.com/services/app-service/) festzulegen:

1. Wählen Sie die App auf dem Blatt **App Services** aus.
1. Wählen Sie in der Gruppe **EINSTELLUNGEN** das Blatt **Anwendungseinstellung** aus.
1. Wählen Sie im Bereich **Anwendungseinstellung** **Neue Einstellung hinzufügen** aus.
1. Geben Sie `ASPNETCORE_ENVIRONMENT` als Namen ein. Geben Sie die Umgebung als Wert an (z.B. `Staging`).
1. Aktivieren Sie das Kontrollkästchen **Sloteinstellung**, wenn Sie möchten, dass die Umgebungseinstellung im aktuellen Slot bleibt, wenn Bereitstellungsslots getauscht werden. Weitere Informationen finden Sie in der [Azure-Dokumentation: Einrichten von Stagingumgebungen in Azure App Service](/azure/app-service/web-sites-staged-publishing).
1. Klicken Sie oben auf dem Blatt auf **Speichern**.

Azure App Service startet die App automatisch neu, nachdem eine App-Einstellung (Umgebungsvariable) im Azure-Portal hinzugefügt, geändert oder gelöscht wurde.

### <a name="windows"></a>Windows

Zum Festlegen der `ASPNETCORE_ENVIRONMENT` für die aktuelle Sitzung werden folgende Befehle verwendet, wenn die App mit [dotnet run](/dotnet/core/tools/dotnet-run) gestartet wird:

**Eingabeaufforderung**

```console
set ASPNETCORE_ENVIRONMENT=Development
```

**PowerShell**

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

Diese Befehle sind nur für das aktuelle Fenster wirksam. Wenn das Fenster geschlossen wird, wird die Einstellung `ASPNETCORE_ENVIRONMENT` auf die Standardeinstellung oder den Computerwert zurückgesetzt. Wenn Sie den Wert global unter Windows festlegen möchten, müssen Sie zu **Systemsteuerung** > **System** > **Erweiterte Systemeinstellungen** navigieren und den Wert `ASPNETCORE_ENVIRONMENT` hinzufügen oder bearbeiten:

![Erweiterte Systemeigenschaften](environments/_static/systemsetting_environment.png)

![ASP.NET Core-Umgebungsvariable](environments/_static/windows_aspnetcore_environment.png)

**web.config**

Weitere Informationen finden Sie im Abschnitt *Festlegen der Umgebungsvariablen* im Artikel [Verweis auf die Konfiguration des ASP.NET Core-Moduls](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).

**Pro IIS-Anwendungspool**

Wenn Sie Umgebungsvariablen für einzelne Apps festlegen möchten, die in isolierten Anwendungspools ausgeführt werden (unterstützt für IIS 10.0 und höher), finden Sie weitere Informationen im Abschnitt zum *Befehl „AppCmd.exe“* im Artikel [Umgebungsvariablen &lt; EnvironmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe).

### <a name="macos"></a>macOS

Das Festlegen der aktuellen Umgebung für macOS kann inline während der Ausführung der App erfolgen:

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

Legen Sie die Umgebung alternativ mit `export` vor der Ausführung der App fest:

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

Umgebungsvariablen auf Computerebene werden in der *BASHRC*- oder *BASH_PROFILE*-Datei festgelegt. Bearbeiten Sie die Datei mit einem beliebigen Text-Editor. Fügen Sie die folgende Anweisung hinzu:

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a>Linux

Verwenden Sie bei Linux-Distributionen für sitzungsbasierte Variableneinstellungen den Befehl `export` in der Befehlszeile und die *BASH-PROFILE*-Datei für Umgebungseinstellungen auf Computerebene.

### <a name="configuration-by-environment"></a>Konfiguration nach Umgebung

Weitere Informationen finden Sie unter [Konfiguration nach Umgebung](xref:fundamentals/configuration/index#configuration-by-environment).

## <a name="environment-based-startup-class-and-methods"></a>Umgebungsbasierte Startklasse und Methoden

Nach dem Start einer ASP.NET Core-App lädt die [Startklasse](xref:fundamentals/startup) die App. Wenn eine `Startup{EnvironmentName}`-Klasse vorhanden ist, wird diese Klasse für diesen `EnvironmentName` aufgerufen:

[!code-csharp[](environments/sample/WebApp1/StartupDev.cs?name=snippet&highlight=1)]

[WebHostBuilder.UseStartup<TStartup>](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) überschreibt Konfigurationsabschnitte.

[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) und [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) unterstützen umgebungsspezifische Versionen der Form `Configure{EnvironmentName}` und `Configure{EnvironmentName}Services`:

[!code-csharp[](environments/sample/WebApp1/Startup.cs?name=snippet_all&highlight=15,37)]

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Starten von Anwendungen](xref:fundamentals/startup)
* [Konfiguration](xref:fundamentals/configuration/index)
* [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname)
