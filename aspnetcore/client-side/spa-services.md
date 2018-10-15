---
title: Verwenden von JavaScriptServices zum Erstellen von einseitigen Anwendungen in ASP.NET Core
author: scottaddie
description: Informationen Sie zu den Vorteilen der Verwendung von JavaScriptServices zum Erstellen einer Single-Page Anwendung (SPA) von ASP.NET Core unterstützt wird.
ms.author: scaddie
ms.custom: H1Hack27Feb2017
ms.date: 08/02/2017
uid: client-side/spa-services
ms.openlocfilehash: 6d6a92427d5d4b853248e60a12625573c4375515
ms.sourcegitcommit: c12ebdab65853f27fbb418204646baf6ce69515e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/21/2018
ms.locfileid: "46523297"
---
# <a name="use-javascriptservices-to-create-single-page-applications-in-aspnet-core"></a>Verwenden von JavaScriptServices zum Erstellen von einseitigen Anwendungen in ASP.NET Core

Durch [Scott Addie](https://github.com/scottaddie) und [Fiyaz Hasan](http://fiyazhasan.me/)

Eine Single-Page-Anwendung (SPA) ist ein Webanwendungstyp, der aufgrund seiner hohen Servicequalität und Leistungsstärke sehr beliebt ist. Die Integration clientseitiger SPA-Frameworks oder -Bibliotheken wie [Angular](https://angular.io/) oder [React](https://facebook.github.io/react/) mit serverseitigen Frameworks wie ASP.NET Core kann schwierig sein. [JavaScriptServices](https://github.com/aspnet/JavaScriptServices) wurde entwickelt, um Inkonsistenzen im Integrationsprozess zu reduzieren. Hierdurch wird ein reibungsloser Betrieb verschiedener Client- und Servertechnologiestapel möglich.

<a name="what-is-js-services"></a>

## <a name="what-is-javascriptservices"></a>Was ist JavaScriptServices

JavaScriptServices ist eine Sammlung von clientseitigen Technologien für ASP.NET Core. Das Ziel ist, die ASP.NET Core als Entwickler bevorzugte serverseitige Plattform zum Erstellen von SPAs zu positionieren.

JavaScriptServices besteht aus drei unterschiedlichen NuGet-Pakete:

* [Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)
* [Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)
* [Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)

Diese Pakete sind nützlich, wenn Sie:

* JavaScript auf dem Server ausgeführen.
* Ein SPA-Framework oder eine Bibliothek verwenden möchten
* Clientseitige-Assets mit Webpack erstellen wollen

Der Schwerpunkt dieses Artikels liegt auf der Zuhilfenahme des SpaServices-Pakets.

<a name="what-is-spa-services"></a>

## <a name="what-is-spaservices"></a>Was ist SpaServices

SpaServices wurde erstellt, um Entwicklern ASP.NET Core als bevorzugte serverseitige Plattform zum Erstellen von SPAs zu positionieren. SpaServices ist nicht erforderlich, um der SPAs mit ASP.NET Core zu entwickeln, und es nicht an ein bestimmtes Clientframework gebunden.

SpaServices bietet nützliche Infrastruktur wie z.B.:

* [Serverseitige prerendering](#server-prerendering)
* [Webpack-Dev-Middleware](#webpack-dev-middleware)
* [Der Austausch eines "Hot"](#hot-module-replacement)
* [Routing-Hilfsprogramme](#routing-helpers)

Diese Infrastrukturkomponenten beschleunigen sowohl den Entwicklungsworkflow als auch die Zusammenarbeit mit der Common Language Runtime-Umgebung. Die Komponenten können einzeln verwendet werden.

<a name="spa-services-prereqs"></a>

## <a name="prerequisites-for-using-spaservices"></a>Voraussetzungen für die Verwendung von SpaServices

Zum Arbeiten mit SpaServices führen Sie die folgenden Schritte aus:

* [Node.js](https://nodejs.org/) (Version 6 oder höher) mit Npm
  * Installieren Sie die Node.js Umgebung und führen Sie anschließen folgenden Befehl auf der Eingabeaufforderung aus um die Installation zu überprüfen:

    ```console
    node -v && npm -v
    ```

Hinweis: Wenn Sie eine Azure-Website bereitstellen möchten, müssen Sie hier nicht prüfen ob Node.js installiert ist, da diese die Softwareumgebung dort automatisch installiert ist..

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]

  * Wenn Sie Windows mit Visual Studio 2017 verwenden, wird das SDK durch Auswählen der Option **rmübergreifende Entwicklung mit .NET Core** installiert.

* [Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet-Paket

<a name="server-prerendering"></a>

## <a name="server-side-prerendering"></a>Serverseitige prerendering

Eine universelle (auch bekannt als isomorph)-Anwendung ist eine JavaScript-Anwendung, die sowohl auf dem Server und dem Client ausgeführt werden kann. Dies bietet eine universelle Plattform für die Entwicklung dieses Anwendungsstils, mittels Angular, React und anderen beliebten Frameworks. Die Idee ist zunächst das erste Rendern die Framework-Komponenten auf dem Server über Node.js, und danach die weitere Ausführung an den Client zu delegieren.

ASP.NET Core [Taghilfsprogramme](xref:mvc/views/tag-helpers/intro), bereitgestellt durch SpaServices, vereinfachen die Implementierung von serverseitigen Prerendering durch Bereitstellen und Aufrufen der JavaScript-Funktionen auf dem Server.

### <a name="prerequisites"></a>Erforderliche Komponenten

Installieren Sie Folgendes:

* [ASPNET-Prerendering](https://www.npmjs.com/package/aspnet-prerendering) Npm-Paket:

    ```console
    npm i -S aspnet-prerendering
    ```

### <a name="configuration"></a>Konfiguration

Die Taghilfsprogramme werden im Projekt über die Namespace-Registrierung in der *_ViewImports.cshtml* Datei erkennbar gemacht:

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/_ViewImports.cshtml?highlight=3)]

Diese Taghilfsprogramme abstrahieren die Feinheiten der direkten Kommunikation mit Low-Level-APIs durch die Nutzung einer HTML-ähnlichen Syntax in der Razor-Ansicht:

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=5)]

### <a name="the-asp-prerender-module-tag-helper"></a>Die `asp-prerender-module` Taghilfsprogramm

Die `asp-prerender-module` Taghilfsprogramm, aus dem vorherigen Beispiel, führt *ClientApp/dist/main-server.js* auf dem Server über Node.js aus. Für die der Verständlichkeit: Die *main-server.js*-Datei ist ein Artefakt der TypeScript-zu-JavaScript-Transpilation aus dem [Webpack](http://webpack.github.io/) Buildprozess. Webpack definiert einen Einstiegspunkt-Alias für `main-server`, dessen  Abhängigkeitsdiagramm und Durchlauf beginnt bei der *ClientApp/boot-server.ts*-Datei:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=53)]

Im folgenden Angular-Beispiel nutzt die *ClientApp/boot-server.ts*-Datei die `createServerRenderer` Funktion und den `RenderResult`-Typ des `aspnet-prerendering` Npm-Pakets um das Server-Rendering über Node.js zu konfigurieren. Das, für das Server-Prerendering-bestimmte HTML-Markup wid an eine Auflösungsfunktion übergeben. die von einem stark typisierten JavaScript `Promise` Objekt umgeben ist. Die Signifikanz des `Promise` Objekts liegt darin, dass sie asynchron HTML-Markup zur Injektion auf der Seite in das DOM Platzhalterelement bereitstellt.

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-34,79-)]

### <a name="the-asp-prerender-data-tag-helper"></a>Die `asp-prerender-data` Taghilfsprogramm

In Kombination mit der `asp-prerender-module` Taghilfsprogramm, kann das `asp-prerender-data` Taghilfsprogramm dazu verwendet werden, um die Kontextinformationen an das serverseitige JavaScript von der Razor-Ansicht zu übergeben. Das folgende Markup übergibt beispielsweise Benutzerdaten an das `main-server` Modul:

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=9-12)]

Die empfangene `UserName`-Argument wird mit dem integrierten JSON-Serialisierungsprogramm serialisiert und befindet sich im `params.data` Objekt. Im folgenden Angular-Beispiel werden diese Daten verwendet, um eine persönliche Begrüßung mittels eines `h1` Elementes zu erstellen:

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,38-52,79-)]

Hinweis: An Taghilfsprogramme übergebene Eigenschaftennamen werden mit der **PascalCase**-Notation dargestellt. Im Gegensatz dazu stehen die gleichen Eigenschaftennamen, die im Javascript mit dargestellt **CamelCase** sind. Die Standardkonfiguration der JSON-Serialisierung ist für diesen Unterschied verantwortlich .

Aufbauend auf das vorherige Codebeispiel, können Daten vom Server an die Ansicht per Hydration der `globals` Eigenschaft bereitgestellt werden, um diese an die `resolve`-Funktion weiterzureichen:

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,57-77,79-)]

Das im `globals`-Objekt definierte `postList` Array, wird an das globale `window`-Javascript-Objekt gebunden. Das Hoisting dieser Variablen in den globalen Bereich führt zur Deduplizierung von Aufwand, vor allem mit Bezug des Ladens der Daten einmal auf dem Server und erneut auf dem Client.

![Globale postList Variable Window-Objekt angefügt](spa-services/_static/global_variable.png)

<a name="webpack-dev-middleware"></a>

## <a name="webpack-dev-middleware"></a>Webpack-Dev-Middleware

[Webpack-Dev-Middleware](https://webpack.github.io/docs/webpack-dev-middleware.html) bietet einen optimierten Entwicklungs-Workflow, bei dem Webpack die Ressourcen bei Bedarf erstellt. Die Middleware kompiliert und stellt clientseitig benötigte Ressourcen automatisch bereit, wenn eine Seite im Browser geladen wird. Der alternative Ansatz ist Webpack über das Npm-Buildskript des Projektes manuell aufrufen, wenn eine Drittanbieter-Abhängigkeit oder der benutzerdefinierte Code geändert wird. Ein Npm-Buildskript aus der *"Package.JSON"* Datei ist im folgenden Beispiel dargestellt:

```json
"build": "npm run build:vendor && npm run build:custom",
```

### <a name="prerequisites"></a>Erforderliche Komponenten

Installieren Sie Folgendes:

* [ASPNET-Webpack](https://www.npmjs.com/package/aspnet-webpack) Npm-Paket:

    ```console
    npm i -D aspnet-webpack
    ```

### <a name="configuration"></a>Konfiguration

Die Webpack-Dev-Middleware wird registriert und in die HTTP-Aufrufpipeline über den folgenden Code in der `Configure` Methode der *"Startup.cs"* Datei implementiert:

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=webpack-middleware-registration&highlight=4)]

Die `UseWebpackDevMiddleware`-Erweiterungsmethode muss vor der `UseStaticFiles`-Erweiterungsmethode zum [registrieren von statischem Datei-Hosting](xref:fundamentals/static-files) aufgerufen werden. Registrieren Sie die Middleware aus Sicherheitsgründen nur, wenn die Applikation im Entwicklungsmodus ausgeführt wird.

Die `output.publicPath`-Eigenschaft der *webpack.config.js*-Datei schreibt der Middleware vor, das `dist`-Verzeichnis auf Änderungen zu überwachen:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,13-16)]

<a name="hot-module-replacement"></a>

## <a name="hot-module-replacement"></a>Der Austausch eines "Hot-Module"

Stellen Sie sich den Webpack ["Hot" Austausch eines Controllermoduls](https://webpack.js.org/concepts/hot-module-replacement/) (HMR)-Funktion als Weiterentwicklung von [Webpack-Dev-Middleware](#webpack-dev-middleware) vor. HMR verfügt über die gleichen Vorteile, optimiert den Entwicklungsworkflow allerdings noch weiter, indem die automatische Aktualisierung der Seiteninhalte nach dem Kompilieren der Änderungen erfolgt. Verwechseln Sie dies nicht mit einer Aktualisierung des Browsers, die den aktuellen in-Memory-Status und die Debugsitzung der SPA beeinträchtigen würde. Es ist ein live-Link zwischen den Webpack-Dev-Middleware-Dienst und dem Browser, was bedeutet, dass Änderungen an den Browser mithilfe von Push übertragen werden.

### <a name="prerequisites"></a>Erforderliche Komponenten

Installieren Sie Folgendes:

* [Webpack-hot-Middleware](https://www.npmjs.com/package/webpack-hot-middleware) Npm-Paket:

    ```console
    npm i -D webpack-hot-middleware
    ```

### <a name="configuration"></a>Konfiguration

Die Komponente HMR muss in der MVC HTTP-Aufrufpipeline in der `Configure` Methode registriert werden:

```csharp
app.UseWebpackDevMiddleware(new WebpackDevMiddlewareOptions {
    HotModuleReplacement = true
});
```

Genau wie bei [Webpack-Dev-Middleware](#webpack-dev-middleware), muss die `UseWebpackDevMiddleware`-Erweiterungsmethode vor der `UseStaticFiles`-Erweiterungsmethode aufgerufen werden. Registrieren Sie die Middleware aus Sicherheitsgründen nur, wenn die Applikation im Entwicklungsmodus ausgeführt wird.

Die *webpack.config.js*-Datei muss ein `plugins`-Array definieren, auch wenn es leer gelassen wird:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,25)]

Nach dem Laden der Applikation im Browser, bietet die Registerkarte für die Entwicklertools-Konsole im Browser die Bestätigung der HMR Aktivierung:

!["Hot" verbundene Austausch eines Controllermoduls-Nachricht](spa-services/_static/hmr_connected.png)

<a name="routing-helpers"></a>

## <a name="routing-helpers"></a>Routing-Hilfsprogramme

In den meisten ASP.NET Core-basierte SPAs werden Sie clientseitiges Routing und serverseitiges Routing verwenden. Die SPA und MVC-Routing-Systeme können ohne Störung unabhängig voneinander arbeiten. Es gibt jedoch einen Grenzfall bei dem  Groß-/Kleinschreibung zur Herausforderung wird: Identifizieren von 404-HTTP-Antworten.

Betrachten Sie das Szenario, in dem eine Route ohne Erweiterung `/some/page` verwendet wird. Angenommen die Anforderung stimmt nicht mit dem Muster eines serverseitigen Pfades überein, jedoch stimmt die Route mit mit einer Client-Side-Route überein. Betrachten Sie nun eine eingehende Anforderung für `/images/user-512.png`, die in der Regel davon ausgeht, eine Bilddatei auf dem Server zu finden. Wenn dieser Pfad für die angeforderte Ressource keine serverseitigen Route oder einer statischen Datei entspricht, ist es unwahrscheinlich, das die Route durch die clientseitige Anwendung verarbeitet werden soll. In der Regel soll ein HTTP-Statuscode 404 zurückgegeben werden.

### <a name="prerequisites"></a>Erforderliche Komponenten

Installieren Sie Folgendes:

* Das Routing Npm-Paket für die clientseitige . Verwendung in einem Angular-Beispiel:

    ```console
    npm i -S @angular/router
    ```

### <a name="configuration"></a>Konfiguration

Eine Erweiterungsmethode namens `MapSpaFallbackRoute` wird in der `Configure` Methode verwendet:

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=mvc-routing-table&highlight=7-9)]

Tipp: Routen werden in der Reihenfolge ausgewertet, in denen sie konfiguriert sind. Daher wird die `default` Route im obigen Codebeispiel zuerst für den Musterabgleich verwendet.

<a name="new-project-creation"></a>

## <a name="creating-a-new-project"></a>Erstellen eines neuen Projekts

JavaScriptServices bietet vorkonfigurierte Anwendungsvorlagen an. SpaServices wird in dieser Vorlagen in Verbindung mit verschiedenen Frameworks und Bibliotheken wie Angular und React mit Redux verwendet.

Diese Vorlagen können über die .NET Core-CLI, mithilfe des folgenden Befehls, installiert werden:

```console
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

Eine Liste der verfügbaren SPA-Vorlagen wird angezeigt:

| Vorlagen                                 | Kurzname | Sprache | Tags        |
|:------------------------------------------|:-----------|:---------|:------------|
| MVC, ASP.NET Core mit Angular             | angular    | [C#]     | MVC/Web/SPA |
| MVC, ASP.NET Core mit React.js            | react      | [C#]     | MVC/Web/SPA |
| MVC, ASP.NET Core mit React.js und Redux  | reactredux | [C#]     | MVC/Web/SPA |

Zum Erstellen eines neuen Projekts mithilfe einer der SPA-Vorlagen nutzen Sie die **Kurznamen** der Vorlage im [Dotnet neue](/dotnet/core/tools/dotnet-new) Befehl. Der folgende Befehl erstellt eine Angular-Anwendung mit ASP.NET Core MVC, die für die serverseitige Verwendung konfiguriert ist:

```console
dotnet new angular
```

<a name="runtime-config-mode"></a>

### <a name="set-the-runtime-configuration-mode"></a>Legen Sie den Modus der Common Language Runtime-Konfiguration fest

Es sind zwei Modi der primären Common Language Runtime-Konfiguration vorhanden:

* **Entwicklung**:
  * Enthält Source-Maps-Zuordnungen zur Erleichterung des Debuggings.
  * Der clientseitige Code wird nicht für Leistung optimiert.
* **Produktion**:
  * Schließt Source-Maps-Zuordnungen aus.
  * Optimiert den clientseitigen Code über Bündelung und Minimierung.

ASP.NET Core verwendet eine Umgebungsvariable namens `ASPNETCORE_ENVIRONMENT` zum Speichern des Konfigurationsmodus. Weitere Informationen finden Sie unter **[Festlegen der Umgebung](xref:fundamentals/environments#set-the-environment)**.

### <a name="running-with-net-core-cli"></a>Mit .NET Core-CLI

Stellen Sie die erforderlichen NuGet und Npm-Pakete mithilfe des folgenden Befehls auf das Stammverzeichnis des Projekts wieder her:

```console
dotnet restore && npm i
```

Erstellen und Ausführen der Anwendung:

```console
dotnet run
```

Starten der Anwendung auf "localhost", gemäß des [Common Language Runtime-Konfigurationsmodus](#runtime-config-mode). Navigieren Sie zu `http://localhost:5000` im Browser auf die Landing Page.

### <a name="running-with-visual-studio-2017"></a>Mit Visual Studio 2017 ausgeführen

Öffnen Sie die *csproj*-Datei, die mittels des [Dotnet New](/dotnet/core/tools/dotnet-new) Befehls erzeugt wurde. Die erforderlichen NuGet und Npm-Pakete werden automatisch auf dem geöffneten Projekt wiederhergestellt. Dieser Wiederherstellungsprozess kann einige Minuten dauern, und die Anwendung ist bereit, ausgeführt zu werden, wenn der Vorgang abgeschlossen ist. Klicken Sie auf die grüne Schaltfläche "ausführen", oder drücken Sie `Ctrl + F5`, und der Browser zur Startseite der Anwendung wird geöffnet . Die Anwendung wird auf "localhost", gemäß der [Common Language Runtime-Konfigurationsmodus](#runtime-config-mode) ausgeführt.

<a name="app-testing"></a>

## <a name="testing-the-app"></a>Testen der Applikation

SpaServices Vorlagen sind so vorkonfiguriert, dass Sie clientseitige Tests mithilfe von [Karma](https://karma-runner.github.io/1.0/index.html) und [Jasmine](https://jasmine.github.io/) entwickeln können. Jasmine ist ein beliebtes Komponententest-Framework für JavaScript, während Karma einen Test Runner für diese Tests ist. Karma ist so konfiguriert, dass mit der [Webpack-Dev-Middleware](#webpack-dev-middleware) so zusammen gearbeitet wird. Das bedeutet für den Entwickler, dass die Tests jedes mal durchgeführt werden, wenn es Änderungen im Code oder im Testfall selbst geben.

Verwenden Sie als Beispiel die Angular-Anwendung, in der bereits zwei Jasmine Testfälle für die `CounterComponent` in der *counter.component.spec.ts*-Datei bereit stehen:

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/app/components/counter/counter.component.spec.ts?range=15-28)]

Öffnen Sie die Eingabeaufforderung im *ClientApp* Verzeichnis. Führen Sie den folgenden Befehl aus:

```console
npm test
```

Das Skript startet die Karma-Testprogramm, welches die in der *karma.conf.js*-Datei definierten Einstellungen liest. Neben anderen Einstellungen identifiziert die die *karma.conf.js*-Datei die auszuführenden Testdateien über das `files`-Array:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/test/karma.conf.js?range=4-5,8-11)]

<a name="app-publishing"></a>

## <a name="publishing-the-application"></a>Veröffentlichen der Anwendung

Das Kombinieren der generierten clientseitigen Ressourcen und der veröffentlichten Elemente in ein ASP.NET Core Ready-to-Deploy-Paket kann aufwendig sein. Glücklicherweise orchestriert SpaServices diesen Prozess für die gesamte Veröffentlichung mit einem benutzerdefinierten MSBuild-Ziel mit dem Namen `RunWebpack`:

[!code-xml[](../client-side/spa-services/sample/SpaServicesSampleApp/SpaServicesSampleApp.csproj?range=31-45)]

Das MSBuild-Ziel hat die folgenden Verantwortungen:

1. Die Npm-Pakete wiederherstellen
1. Erstellen eines Builds auf Produktionsniveau von Drittanbietermodulen und clientseitige Ressourcen
1. Erstellen eines Builds Produktionsniveau von benutzerdefinierten clientseitigen Ressourcen
1. Kopieren der generierte Webpack-Assets in das Verzeichnis "Publish"

Das MSBuild-Ziel wird aufgerufen, bei der Ausführung von:

```console
dotnet publish -c Release
```

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Angular-Docs](https://angular.io/docs)
