---
title: Arbeiten mit mehreren Umgebungen in ASP.NET Core
author: ardalis
description: "Erfahren Sie, wie ASP.NET Core Unterstützung für das Steuern des Verhaltens der app über mehrere Umgebungen bereitstellt."
keywords: ASP.NET Core, umgebungseinstellungen, ASPNETCORE_ENVIRONMENT
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: b5bba985-be12-4464-9a01-df3599b2a6f1
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/environments
ms.openlocfilehash: 9127c3d7180422c0e3dbd813340dd485bf360c81
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2018
---
# <a name="working-with-multiple-environments"></a>Arbeiten mit mehreren Umgebungen

Durch [Steve Smith](https://ardalis.com/)

ASP.NET Core bietet Unterstützung für das Steuern des Verhaltens der app über mehrere Umgebungen, wie z. B. Entwicklungs-, Staging- und produktionsumgebungen. Umgebungsvariablen werden verwendet, um anzugeben, die Common Language Runtime-Umgebung ermöglicht es der app für die Umgebung konfiguriert werden.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

## <a name="development-staging-production"></a>Entwicklung, Bereitstellung, Produktion

ASP.NET Core verweist auf eine bestimmte Umgebungsvariable `ASPNETCORE_ENVIRONMENT` die Umgebung beschreiben Sie die Anwendung im derzeit ausgeführt wird. Diese Variable kann festgelegt werden, auf einen beliebigen Wert gewünscht jedoch drei Werte werden gemäß der Konvention verwendet: `Development`, `Staging`, und `Production`. Finden Sie diese in den Beispielen verwendeten Werte und die Vorlagen mit ASP.NET Core bereitgestellt.

Die aktuelle umgebungseinstellung kann programmgesteuert aus innerhalb der Anwendung erkannt werden. Darüber hinaus können Sie die Umgebung [tag Helper](../mvc/views/tag-helpers/index.md) auf bestimmte Abschnitte enthalten die [Ansicht](../mvc/views/index.md) basierend auf der aktuellen Umgebung der Anwendung.

Hinweis: Auf Windows- und Mac OS wird der angegebenen Umgebungsnamen Groß-/Kleinschreibung beachtet. Ob die Variable festgelegt wird, um `Development` oder `development` oder `DEVELOPMENT` werden die Ergebnissen identisch sein. Linux ist jedoch ein **Groß-/Kleinschreibung beachtet** OS standardmäßig. Umgebungsvariablen, Dateinamen und Einstellungen erfordern Groß-/Kleinschreibung berücksichtigt.

### <a name="development"></a>Entwicklung

Dies sollte der Umgebung, die beim Entwickeln einer Anwendung verwendet werden. Dient normalerweise zum Aktivieren von Funktionen, die wäre nicht verfügbar, wenn die app wie z. B. in der Produktion wird ausgeführt werden sollen die [Developer Ausnahmeseite](xref:fundamentals/error-handling#the-developer-exception-page).

Wenn Sie Visual Studio verwenden, kann die Umgebung in Ihrem Projekt debugprofile konfiguriert werden. Debuggen Sie Profile angeben der [Server](xref:fundamentals/servers/index) mit, dass beim Starten der Anwendung und alle Umgebungsvariablen festgelegt werden. Ihr Projekt kann mehrere debugprofile verfügen, die Umgebungsvariablen anders festgelegt. Sie verwalten diese Profile mit den **Debuggen** auf der Registerkarte des Webanwendungsprojekts **Eigenschaften** Menü. Die Werte, die Sie in den Projekteigenschaften festlegen, werden beibehalten, der *launchSettings.json* -Datei, und Sie können auch Profile durch Konfigurieren dieser Datei direkt bearbeiten.

Das Profil für IIS Express wird hier gezeigt:

![Projekt Umgebungsvariablen Eigenschaften festlegen](environments/_static/project-properties-debug.png)

Hier ist ein `launchSettings.json` -Datei, Profile für umfasst `Development` und `Staging`:

[!code-json[Main](../fundamentals/environments/sample/src/Environments/Properties/launchSettings.json?highlight=15,22)]

Änderungen an Projekt Profile möglicherweise nicht wirksam erst nach dem Neustart des Servers verwendet wird (insbesondere Kestrel muss neu gestartet werden, bevor es an ihre Umgebung vorgenommenen Änderungen erkannt werden).

>[!WARNING]
> In Umgebungsvariablen gespeichert *launchSettings.json* nicht in keiner Weise gesichert werden und werden Teil der Quellcoderepository für Ihr Projekt aus, wenn Sie einen. **Nie Anmeldeinformationen oder anderen vertraulichen Daten in dieser Datei gespeichert werden.** Wenn Sie solche Daten speichern möchten, verwenden die *geheimen Manager* Tool beschrieben, [sichere Speicherung von app-Kennwörter während der Entwicklung](xref:security/app-secrets).

### <a name="staging"></a>Staging

Gemäß der Konvention eine `Staging` Umgebung ist ein Pre-Production-Umgebung für die letzten Test-, vor der Bereitstellung bis hin zur Produktion verwendet. Im Idealfall sollte die physischen Merkmale der Produktion spiegeln, damit alle Probleme, die auftreten können, in der Produktion zuerst in der Stagingumgebung auftreten, in denen sie ohne Auswirkungen auf Benutzer behandelt werden können.

### <a name="production"></a>Produktion

Die `Production` Umgebung ist die Umgebung, in dem die Anwendung ausgeführt wird, wenn es aktiv ist, und von Endbenutzern verwendet wird. Diese Umgebung sollte konfiguriert werden, um Sicherheit, Leistung und Stabilität der Anwendung zu maximieren. Einige allgemeinen Einstellungen, die möglicherweise von eine produktiven Umgebung, die von der Entwicklung unterscheiden würde gehören:

* Aktivieren Sie das Zwischenspeichern

* Sicherstellen, dass alle Ressourcen für die clientseitige gebündelt, verkleinert und potenziell von einem CDN bedient.

* Diagnose ErrorPages deaktivieren

* Angezeigter Fehlerseiten aktivieren

* Produktion Protokollierung und Überwachung zu aktivieren (z. B. [Application Insights](https://azure.microsoft.com/documentation/articles/app-insights-asp-net-five/))

Dies ist keine soll eine vollständige Liste sein. Es wird empfohlen, um zu vermeiden, Streuung Umgebung Überprüfungen in vielen Teilen der Anwendung. Stattdessen wird empfohlen, diese Prüfungen innerhalb der Anwendungsverzeichnis durchzuführen `Startup` Klasse(n) möglichst

## <a name="setting-the-environment"></a>Einrichten der Umgebung

Die Methode zum Festlegen der umgebungs hängt vom Betriebssystem ab.

### <a name="windows"></a>Windows
Festlegen der `ASPNETCORE_ENVIRONMENT` für die aktuelle Sitzung, wenn die app gestartet wird mit `dotnet run`, werden die folgenden Befehle verwendet.

**Befehlszeile**
```
set ASPNETCORE_ENVIRONMENT=Development
```
**PowerShell**
```
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

Diese Befehle wirksam, nur für das aktuelle Fenster. Wenn das Fenster geschlossen wird, wird die ASPNETCORE_ENVIRONMENT-Einstellung in der Standardeinstellung oder Computer Wert wiederhergestellt. Um den Wert global beim Öffnen von Windows Festlegen der **Systemsteuerung** > **System** > **Erweiterte Systemeinstellungen** und hinzufügen oder Bearbeiten der `ASPNETCORE_ENVIRONMENT` Wert.

![Erweiterte Eigenschaften System](environments/_static/systemsetting_environment.png)

![ASPNET-Core-Umgebungsvariable](environments/_static/windows_aspnetcore_environment.png) 

**"Web.config"**

Finden Sie unter der *Einstellungsumgebungsvariablen* Teil der [Konfigurationsverweis ASP.NET Core-Modul](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) Thema.

**Pro IIS-Anwendungspool**

Wenn Sie Umgebungsvariablen für einzelne Apps festlegen müssen, die in isolierten Anwendungspools ausgeführt werden (unterstützt für IIS 10.0 und höher), finden Sie weitere Informationen im Abschnitt zum *Befehl „AppCmd.exe“* im Thema [Umgebungsvariablen \< EnvironmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) in der IIS-Referenzdokumentation.

### <a name="macos"></a>macOS
Festlegen der aktuellen Umgebung für MacOS kann Inline erfolgen, wenn die Anwendung ausgeführt;

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```
oder `export` vor dem Ausführen der app festlegen.

```bash
export ASPNETCORE_ENVIRONMENT=Development
``` 
Level-Umgebungsvariablen festgelegt sind, der *.bashrc* oder *.bash_profile* Datei. Bearbeiten Sie die Datei mit einem beliebigen Texteditor, und fügen Sie die folgende Anweisung hinzu.

```
export ASPNETCORE_ENVIRONMENT=Development
```  

### <a name="linux"></a>Linux
Verwenden Sie für Linux-Distributionen der `export` -Befehl an der Befehlszeile für variableneinstellungen sitzungsbasierte und *Bash_profile* -Datei für Computer auf umgebungseinstellungen.

## <a name="determining-the-environment-at-runtime"></a>Bestimmen die Umgebung zur Laufzeit

Die `IHostingEnvironment` Service bietet die Core-Abstraktion für die Arbeit mit Umgebungen. Dieser Dienst ist von ASP.NET gehostet Ebene, und können eingegeben werden, in die Startlogik über [Abhängigkeitsinjektion](dependency-injection.md). Die ASP.NET Core-Website-Vorlage in Visual Studio verwendet diesen Ansatz, umgebungsspezifische Konfigurationsdateien (falls vorhanden) geladen und fehlerbehandlungseinstellungen für die app anpassen. In beiden Fällen erfolgt durch einen Verweis auf die aktuell angegebenen Umgebung durch Aufrufen dieses Verhalten `EnvironmentName` oder `IsEnvironment` für die Instanz von `IHostingEnvironment` an die entsprechende Methode übergeben.

> [!NOTE]
> Wenn Sie überprüfen, ob die Anwendung, in einer bestimmten Umgebung verwenden ausgeführt wird müssen `env.IsEnvironment("environmentname")` , da er ordnungsgemäß Groß-und Kleinschreibung wird (statt überprüft, ob `env.EnvironmentName == "Development"` z. B.).

Z. B. können den folgenden Code in der Methode konfigurieren Sie beim Einrichten der Umgebung bestimmte Fehlerbehandlung:

[!code-csharp[Main](environments/sample/src/Environments/Startup.cs?range=19-30)]

Wenn die app, in ausgeführt wird einer `Development` Umgebung, und es ermöglicht die Common Language Runtime-Unterstützung erforderlich ist, verwenden Sie die Funktion "BrowserLink" in Visual Studio, die Entwicklung-spezifische Fehlerseiten (die in der Regel nicht in der produktionsumgebung ausgeführt werden soll) und die spezielle Datenbank mit dem Fehler Seiten, die (der bieten eine Möglichkeit zum Anwenden von Migrationen und sollte daher nur in der Entwicklung verwendet werden). Andernfalls, wenn die app nicht in einer Entwicklungsumgebung ausgeführt wird, ist eine Standard-Fehlerbehandlung Seite konfiguriert als Antwort auf nicht behandelten Ausnahmen angezeigt werden.

Sie müssen, um zu bestimmen, welche Inhalte an den Client zur Laufzeit, abhängig von der aktuellen Umgebung zu senden. Beispielsweise dienen in einer Entwicklungsumgebung Sie im Allgemeinen nicht minimiert, Skripts und Stylesheets verwendet, wodurch das debugging erleichtern. Produktions- und Testcomputer-Umgebungen sollten die verkleinerte Versionen dienen und in der Regel aus einem CDN. Hierzu können Sie mithilfe der Umgebung [tag Helper](../mvc/views/tag-helpers/intro.md). Die Umgebung Tags Hilfsprogramm nur seinen Inhalt gerendert, wenn die aktuelle Umgebung mit einer von Umgebungen mit angegebenen entspricht der `names` Attribut.

[!code-html[Main](environments/sample/src/Environments/Views/Shared/_Layout.cshtml?range=13-22)]

Um die ersten Schritte mit der Tag-Hilfsprogramme in Ihrer Anwendung finden Sie unter [Einführung in die Tag-Hilfsprogrammen](../mvc/views/tag-helpers/intro.md).

## <a name="startup-conventions"></a>Start-Konventionen

ASP.NET Core unterstützt einen Konvention basierenden Ansatz zum Konfigurieren einer Anwendung starten, die basierend auf der aktuellen Umgebung. Sie können auch programmgesteuert steuern, wie Ihre Anwendung verhält sich entsprechend, welcher Umgebung, wird Ihnen das Erstellen und verwalten Ihren eigenen Konventionen ermöglicht.

Wenn eine ASP.NET Core-Anwendung gestartet wird, die `Startup` Klasse wird verwendet, um das Bootstrapping der Anwendung an, und Laden ihre Konfigurationseinstellungen usw. ([erfahren Sie mehr über ASP.NET Start](startup.md)). Jedoch ggf. eine Klasse mit dem Namen `Startup{EnvironmentName}` (z. B. `StartupDevelopment`), und die `ASPNETCORE_ENVIRONMENT` Umgebungsvariablen übereinstimmt, diesen Namen, und klicken Sie dann, `Startup` Klasse dient. Sie könnten daher festlegen `Startup` für die Entwicklung, bieten jedoch eine Separate `StartupProduction` , die beim Ausführen der app in der Produktion verwendet werden würde. Oder umgekehrt.

> [!NOTE]
> Aufrufen von `WebHostBuilder.UseStartup<TStartup>()` Konfigurationsabschnitte überschreibt.

Zusätzlich zur Verwendung einer vollständig separaten `Startup` -Klasse auf Grundlage der aktuellen Umgebung möglich auch Anpassungen an der Konfiguration der Anwendung innerhalb einer `Startup` Klasse. Die `Configure()` und `ConfigureServices()` Methoden unterstützen umgebungsspezifische Versionen ähnelt der `Startup` selbst dar, der das Formular `Configure{EnvironmentName}()` und `Configure{EnvironmentName}Services()`. Wenn Sie eine Methode definieren `ConfigureDevelopment()` wird anstelle von heißen `Configure()` Wenn die Umgebung auf Development festgelegt ist. Ebenso `ConfigureDevelopmentServices()` würde aufgerufen werden, anstelle von `ConfigureServices()` in derselben Umgebung.

## <a name="summary"></a>Zusammenfassung

ASP.NET Core bietet eine Reihe von Features und Konventionen, mit denen Entwickler auf einfache Weise zu steuern, wie ihre Anwendungen in verschiedenen Umgebungen Verhalten. Beim Veröffentlichen einer Anwendung von der Entwicklung in die Stagingumgebung bis hin zur Produktion Umgebungsvariablen festgelegt entsprechend für die Umgebung Optimierung ermöglicht der Anwendung für die Verwendung von Debuggen, testen oder zur Produktion, nach Bedarf.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Konfiguration](xref:fundamentals/configuration/index)

* [Einführung in Taghilfsprogramme](../mvc/views/tag-helpers/intro.md)
