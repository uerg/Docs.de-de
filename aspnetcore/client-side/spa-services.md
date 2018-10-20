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

* JavaScript auf dem Server ausführen.
* Ein SPA-Framework oder eine Bibliothek verwenden möchten
* Clientseitige Assets mit Webpack erstellen wollen

Der Schwerpunkt dieses Artikels liegt auf der Zuhilfenahme des SpaServices-Pakets.

<a name="what-is-spa-services"></a>

## <a name="what-is-spaservices"></a>Was ist SpaServices

SpaServices wurde erstellt, damit Entwickler ASP.NET Core bevorzugt als serverseitige Plattform zum Erstellen von SPAs zu verwenden. SpaServices ist nicht erforderlich, um SPAs mit ASP.NET Core zu entwickeln, und ist nicht an ein bestimmtes Clientframework gebunden.

SpaServices bietet nützliche Infrastruktur wie z.B.:

* [Serverseitige prerendering](#server-prerendering)
* [Webpack-Dev-Middleware](#webpack-dev-middleware)
* [Der Austausch eines "Hot"](#hot-module-replacement)
* [Routing-Hilfsprogramme](#routing-helpers)

Diese Infrastrukturkomponenten beschleunigen sowohl den Entwicklungsworkflow als auch die Zusammenarbeit mit der Common Language Runtime-Umgebung. Die Komponenten können einzeln verwendet werden.

<a name="spa-services-prereqs"></a>

## <a name="prerequisites-for-using-spaservices"></a>Voraussetzungen für die Verwendung von SpaServices

Führen Sie die folgenden Schritte aus, um mit SpaServices zu arbeiten:

* [Node.js](https://nodejs.org/) (Version 6 oder höher) mit Npm
  * Führen Sie den folgenden Befehl über die Befehlszeile aus, um zu überprüfen, ob diese Komponenten installiert sind und gefunden werden können.

    ```console
    node -v && npm -v
    ```

Hinweis: Wenn Sie eine Bereitstellung auf eine Azure-Website vornehmen möchten, müssen Sie hier nichts weiter tun, denn Node.js ist in den Serverumgebungen installiert und verfügbar.

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]

  * Wenn Sie Windows mit Visual Studio 2017 verwenden, wird das SDK durch Auswählen der Option **plattformübergreifende Entwicklung mit .NET Core** installiert.

* [Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet-Paket

<a name="server-prerendering"></a>

## <a name="server-side-prerendering"></a>Serverseitige prerendering

Eine universelle Anwendung (auch bekannt als isomorphe Anwendung) ist eine JavaScript-Anwendung, die sowohl auf dem Server als auch auf dem Client ausgeführt werden kann. Angular, React und andere beliebte Frameworks bieten eine universelle Plattform für diesen Anwendungsentwicklungsstil. Die Idee dahinter ist, zunächst die Framework-Komponenten über Node.js auf dem Server zu rendern und dann weitere Ausführungen an den Client zu delegieren.

ASP.NET Core-[Taghilfsprogramme](xref:mvc/views/tag-helpers/intro), bereitgestellt durch SpaServices, vereinfachen die Implementierung von serverseitigem Prerendering durch Bereitstellen und Aufrufen der JavaScript-Funktionen auf dem Server.

### <a name="prerequisites"></a>Vorraussetzungen

Installieren Sie Folgendes:

* [ASPNET-Prerendering](https://www.npmjs.com/package/aspnet-prerendering) Npm-Paket:

    ```console
    npm i -S aspnet-prerendering
    ```

### <a name="configuration"></a>Konfiguration

Die Taghilfsprogramme werden im Projekt über die Namespace-Registrierung in der *_ViewImports.cshtml*-Datei erkennbar gemacht:

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/_ViewImports.cshtml?highlight=3)]

Diese Taghilfsprogramme abstrahieren die Feinheiten der direkten Kommunikation mit Low-Level-APIs durch die Nutzung einer HTML-ähnlichen Syntax in der Razor-Ansicht:

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=5)]

### <a name="the-asp-prerender-module-tag-helper"></a>Die `asp-prerender-module` Taghilfsprogramm

Die `asp-prerender-module` Taghilfsprogramm, in dem vorherigen Beispiel, verwendet führt *ClientApp/dist/main-server.js* auf dem Server über Node.js. Für die der Verständlichkeit *Main-Datei "Server.js"* Datei ist ein Artefakt der Aufgabe in TypeScript und JavaScript-transpilation ziehen die [Webpack](http://webpack.github.io/) Buildprozess. Webpack definiert einen Eintrag-Punkt-Alias für `main-server`; und das Abhängigkeitsdiagramm für diesen Alias Durchlauf beginnt bei der *ClientApp/Boot-server.ts* Datei:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=53)]

Im folgenden Beispiel Angular die *ClientApp/Boot-server.ts* Datei nutzt die `createServerRenderer` Funktion und `RenderResult` Typ der `aspnet-prerendering` Npm-Paket so konfigurieren Sie Server-Rendering über Node.js. Das HTML-Markup, das bestimmt, für serverseitiges Rendering auf ein Funktionsaufruf auflösen übergeben wird, die von einer stark typisierten JavaScript umgeben sind `Promise` Objekt. Die `Promise` des Objekts "significance" ist, dass sie asynchron die HTML-Markup auf der Seite zur Injektion in das DOM Platzhalterelement bereitstellt.

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-34,79-)]

### <a name="the-asp-prerender-data-tag-helper"></a>Die `asp-prerender-data` Taghilfsprogramm

In Kombination mit der `asp-prerender-module` Taghilfsprogramm, das `asp-prerender-data` Taghilfsprogramm kann verwendet werden, um die Kontextinformationen für das serverseitige JavaScript von der Razor-Ansicht zu übergeben. Das folgende Markup übergibt beispielsweise Benutzerdaten der `main-server` Modul:

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=9-12)]

Die empfangenen Daten `UserName` Argument wird mit der integrierten JSON-Serialisierungsprogramm serialisiert und befindet sich in der `params.data` Objekt. Im folgenden Beispiel Angular-die Daten werden verwendet, um persönlich begrüßt im Erstellen einer `h1` Element:

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,38-52,79-)]

Hinweis: Taghilfsprogramme übergebene Eigenschaftennamen mit dargestellt **PascalCase** Notation. Im Gegensatz dazu stehen für JavaScript ist, in denen die gleichen Eigenschaftennamen mit dargestellt **CamelCase**. Die Standardkonfiguration für JSON-Serialisierung ist verantwortlich für diesen Unterschied.

Zum Erweitern auf das vorherige Codebeispiel Daten können werden vom Server an die Ansicht übergeben von hydrating der `globals` Eigenschaft bereitgestellt, um die `resolve` Funktion:

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,57-77,79-)]

Die `postList` Array definiert, die innerhalb der `globals` Objekt angefügt ist im Browsers auf die globale `window` Objekt. Diese Variable anheben globalen Bereich entfällt die Duplizierung von Aufwand, vor allem mit Bezug auf die gleichen Daten einmal auf dem Server und erneut auf dem Client werden geladen.

![Globale postList Variable Window-Objekt angefügt](spa-services/_static/global_variable.png)

<a name="webpack-dev-middleware"></a>

## <a name="webpack-dev-middleware"></a>Webpack-Dev-Middleware

[Webpack-Dev-Middleware](https://webpack.github.io/docs/webpack-dev-middleware.html) bietet einen optimierte Entwicklung-Workflow, bei dem Webpack Ressourcen bei Bedarf erstellt. Die Middleware automatisch kompiliert und clientseitige Ressourcen dient, wenn eine Seite im Browser geladen wird. Der alternative Ansatz ist Webpack über Npm-Buildskript des Projekts manuell aufrufen, wenn eine Drittanbieter Abhängigkeit oder den benutzerdefinierten Code geändert wird. Ein Npm-Buildskript die *"Package.JSON"* Datei ist im folgenden Beispiel dargestellt:

```json
"build": "npm run build:vendor && npm run build:custom",
```

### <a name="prerequisites"></a>Vorraussetzungen

Installieren Sie Folgendes:

* [ASPNET-Webpack](https://www.npmjs.com/package/aspnet-webpack) Npm-Paket:

    ```console
    npm i -D aspnet-webpack
    ```

### <a name="configuration"></a>Konfiguration

Webpack-Dev-Middleware wird registriert, in die HTTP-Anforderungspipeline über den folgenden Code in die *"Startup.cs"* dateimodell `Configure` Methode:

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=webpack-middleware-registration&highlight=4)]

Die `UseWebpackDevMiddleware` Erweiterungsmethode aufgerufen werden muss, bevor [registrieren, statische Datei hosting](xref:fundamentals/static-files) über die `UseStaticFiles` -Erweiterungsmethode. Registrieren Sie die Middleware aus Sicherheitsgründen nur, wenn die app im Entwicklungsmodus ausgeführt wird.

Die *webpack.config.js* dateimodell `output.publicPath` Eigenschaft gibt die Middleware, sehen Sie sich die `dist` Ordner auf Änderungen:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,13-16)]

<a name="hot-module-replacement"></a>

## <a name="hot-module-replacement"></a>Der Austausch eines "Hot"

Stellen Sie sich den Webpack ["Hot" Austausch eines Controllermoduls](https://webpack.js.org/concepts/hot-module-replacement/) (HMR)-Funktion als Weiterentwicklung von [Webpack-Dev-Middleware](#webpack-dev-middleware). HMR führt die gleichen Vorteile, aber es weiter den Entwicklungsworkflow optimiert, indem die automatische Aktualisierung der Seiteninhalt nach dem Kompilieren der Änderungen. Verwechseln Sie dies nicht mit einer Aktualisierung des Browsers, die den aktuellen in-Memory-Status und die Debugsitzung der SPA beeinträchtigen würde. Es ist ein live-Link zwischen den Webpack-Dev-Middleware-Dienst und dem Browser, was bedeutet, dass Änderungen an den Browser mithilfe von Push übertragen werden.

### <a name="prerequisites"></a>Vorraussetzungen

Installieren Sie Folgendes:

* [Webpack-hot-Middleware](https://www.npmjs.com/package/webpack-hot-middleware) Npm-Paket:

    ```console
    npm i -D webpack-hot-middleware
    ```

### <a name="configuration"></a>Konfiguration

Die Komponente HMR muss registriert werden, in MVC HTTP-Anforderungspipeline in die `Configure` Methode:

```csharp
app.UseWebpackDevMiddleware(new WebpackDevMiddlewareOptions {
    HotModuleReplacement = true
});
```

Wie wurde mit "true" [Webpack-Dev-Middleware](#webpack-dev-middleware), `UseWebpackDevMiddleware` Erweiterungsmethode aufgerufen werden muss, bevor die `UseStaticFiles` -Erweiterungsmethode. Registrieren Sie die Middleware aus Sicherheitsgründen nur, wenn die app im Entwicklungsmodus ausgeführt wird.

Die *webpack.config.js* Datei definieren, muss ein `plugins` array, auch wenn es leer gelassen wird:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,25)]

Nach dem Laden der app im Browser, bietet die Registerkarte für die Entwicklertools-Konsole zur Bestätigung der HMR Aktivierung:

!["Hot" verbundene Austausch eines Controllermoduls-Nachricht](spa-services/_static/hmr_connected.png)

<a name="routing-helpers"></a>

## <a name="routing-helpers"></a>Routing-Hilfsprogramme

In den meisten ASP.NET Core-basierte SPAs werden sollten Sie die clientseitige routing neben dem routing von serverseitigen. Die SPA und MVC-routing-Systeme können ohne Störung unabhängig voneinander arbeiten. Vorhanden ist, jedoch einen Edge Groß-/Kleinschreibung machen Herausforderungen: Identifizieren von 404-HTTP-Antworten.

Betrachten Sie das Szenario, in dem eine Route ohne Erweiterung `/some/page` verwendet wird. Angenommen Sie, die Anforderung nicht-Musterübereinstimmung eine serverseitige Weg, Verwendungsmuster stimmt jedoch mit einer Client-Side-Route. Betrachten Sie nun eine eingehende Anforderung für `/images/user-512.png`, die in der Regel davon ausgeht, eine Bilddatei auf dem Server. Wenn dieser Pfad für die angeforderte Ressource keine serverseitigen Route oder eine statische Datei entspricht, ist es unwahrscheinlich, die die clientseitige Anwendung verarbeitet werden sollen, in der Regel eine HTTP-Statuscode 404 zurückgegeben werden soll.

### <a name="prerequisites"></a>Vorraussetzungen

Installieren Sie Folgendes:

* Das routing Npm-Paket für die clientseitige. Verwenden von Angular als Beispiel:

    ```console
    npm i -S @angular/router
    ```

### <a name="configuration"></a>Konfiguration

Eine Erweiterungsmethode namens `MapSpaFallbackRoute` werden in der `Configure` Methode:

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=mvc-routing-table&highlight=7-9)]

Tipp: Routen werden in der Reihenfolge ausgewertet, in denen sie konfiguriert sind. Daher die `default` Route im obigen Codebeispiel wird zuerst für den Musterabgleich verwendet wird.

<a name="new-project-creation"></a>

## <a name="creating-a-new-project"></a>Erstellen eines neuen Projekts

JavaScriptServices bietet vorkonfigurierte Anwendungsvorlagen an. SpaServices wird in dieser Vorlagen in Verbindung mit verschiedenen Frameworks und Bibliotheken wie Angular und React mit Redux verwendet.

Diese Vorlagen können über die .NET Core-CLI installiert werden, mithilfe des folgenden Befehls:

```console
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

Eine Liste der verfügbaren SPA-Vorlagen wird angezeigt:

| Vorlagen                                 | Kurzname | Sprache | Tags        |
|:------------------------------------------|:-----------|:---------|:------------|
| MVC, ASP.NET Core mit Angular             | angular    | [C#]     | Web/MVC/SPA |
| MVC, ASP.NET Core mit React.js            | react      | [C#]     | Web/MVC/SPA |
| MVC, ASP.NET Core mit React.js und Redux  | reactredux | [C#]     | Web/MVC/SPA |

Schließen Sie zum Erstellen eines neuen Projekts mithilfe einer der SPA-Vorlagen den **Kurznamen** der Vorlage in den Befehl [dotnet new](/dotnet/core/tools/dotnet-new) ein. Der folgende Befehl erstellt eine Angular-Anwendung mit ASP.NET Core MVC, die für die serverseitige Verwendung konfiguriert ist:

```console
dotnet new angular
```

<a name="runtime-config-mode"></a>

### <a name="set-the-runtime-configuration-mode"></a>Festlegen des Laufzeitkonfigurationsmodus

Es sind zwei Modi der primären Laufzeitkonfiguration vorhanden:

* **Entwicklung**:
  * Enthält Quellzuordnungen zur Erleichterung des Debuggings.
  * Der clientseitige Code wird nicht für Leistung optimiert.
* **Produktion**:
  * Schließt Quellzuordnungen aus.
  * Optimiert den clientseitigen Code über Bündelung und Minimierung.

ASP.NET Core verwendet eine Umgebungsvariable namens `ASPNETCORE_ENVIRONMENT` zum Speichern von des Konfigurationsmodus zu wechseln. Finden Sie unter **[legen Sie die Umgebung](xref:fundamentals/environments#set-the-environment)** für Weitere Informationen.

### <a name="running-with-net-core-cli"></a>Mit .NET Core-CLI

Stellen Sie die erforderlichen NuGet und Npm-Pakete mithilfe des folgenden Befehls auf das Stammverzeichnis des Projekts wieder her:

```console
dotnet restore && npm i
```

Erstellen und Ausführen der Anwendung:

```console
dotnet run
```

Die Anwendung wird auf dem Localhost gemäß des [Laufzeitkonfigurationsmodus](#runtime-config-mode) gestartet. Navigieren Sie zu `http://localhost:5000` im Browser, wo die Landing Page angezeigt wird.

### <a name="running-with-visual-studio-2017"></a>Ausführen mit Visual Studio 2017

Öffnen Sie die *csproj*-Datei, die mittels des [dotnet new](/dotnet/core/tools/dotnet-new)-Befehls erzeugt wurde. Die erforderlichen NuGet- und npm-Pakete werden automatisch beim Projektstart wiederhergestellt. Dieser Wiederherstellungsprozess kann einige Minuten dauern, und die Anwendung kann ausgeführt werden, wenn der Vorgang abgeschlossen ist. Klicken Sie auf die grüne Schaltfläche "Ausführen", oder drücken Sie `Ctrl + F5`, und der Browser zur Landing Page der Anwendung wird geöffnet . Die Anwendung wird auf dem Localhost gemäß des [Laufzeitkonfigurationsmodus](#runtime-config-mode) ausgeführt.

<a name="app-testing"></a>

## <a name="testing-the-app"></a>Testen der App

SpaServices-Vorlagen sind so vorkonfiguriert, dass sie clientseitige Tests mithilfe von [Karma](https://karma-runner.github.io/1.0/index.html) und [Jasmine](https://jasmine.github.io/) entwickeln können. Jasmine ist ein beliebtes Komponententest-Framework für JavaScript, während Karma ein Test Runner für diese Tests ist. Karma ist so konfiguriert, dass mit der [Webpack Dev Middleware](#webpack-dev-middleware) so zusammen gearbeitet wird, dass der Entwickler Tests nicht jedes Mal anhalten und wieder ausführen muss, wenn Änderungen vorgenommen werden. Der Test wird automatisch ausgeführt, egal ob der Code für den Testfall ausgeführt wird oder der Testfall selbst.

Verwenden Sie als Beispiel die Angular-Anwendung, in der bereits zwei Jasmine-Testfälle für `CounterComponent` in der *counter.component.spec.ts*-Datei bereit stehen:

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/app/components/counter/counter.component.spec.ts?range=15-28)]

Öffnen Sie die Eingabeaufforderung im *ClientApp*-Verzeichnis. Führen Sie den folgenden Befehl aus:

```console
npm test
```

Das Skript startet den Karma-Test Runner, der die in der *karma.conf.js*-Datei definierten Einstellungen liest. Neben anderen Einstellungen identifiziert die *karma.conf.js*-Datei die Testdateien, die über das `files`-Array ausgeführt werden sollen:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/test/karma.conf.js?range=4-5,8-11)]

<a name="app-publishing"></a>

## <a name="publishing-the-application"></a>Veröffentlichen der Anwendung

Es kann aufwendig sein, die generierten clientseitigen Ressourcen und die veröffentlichten ASP.NET Core-Artefakte in einem Paket, das bereitgestellt werden kann, zu kombinieren. Glücklicherweise orchestriert SpaServices diesen gesamten Veröffentlichungsprozess mit einem benutzerdefinierten MSBuild-Ziel mit dem Namen `RunWebpack`:

[!code-xml[](../client-side/spa-services/sample/SpaServicesSampleApp/SpaServicesSampleApp.csproj?range=31-45)]

Das MSBuild-Ziel hat die folgenden Verantwortungen:

1. Die Npm-Pakete wiederherstellen
1. Erstellen eines Builds auf Produktionsniveau von clientseitigen Assets von Drittanbietern
1. Erstellen eines Builds auf Produktionsniveau der benutzerdefinierten clientseitigen Assets
1. Kopieren der generierten Webpack-Assets in den Ordner für die Veröffentlichung.

Das MSBuild-Ziel wird aufgerufen, wenn Folgendes ausgeführt wird:

```console
dotnet publish -c Release
```

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Angular-Docs](https://angular.io/docs)
