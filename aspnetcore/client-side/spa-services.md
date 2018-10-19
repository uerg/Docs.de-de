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
# <a name="use-javascriptservices-to-create-single-page-applications-in-aspnet-core"></a><span data-ttu-id="55f21-103">Verwenden von JavaScriptServices zum Erstellen von einseitigen Anwendungen in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="55f21-103">Use JavaScriptServices to Create Single Page Applications in ASP.NET Core</span></span>

<span data-ttu-id="55f21-104">Durch [Scott Addie](https://github.com/scottaddie) und [Fiyaz Hasan](http://fiyazhasan.me/)</span><span class="sxs-lookup"><span data-stu-id="55f21-104">By [Scott Addie](https://github.com/scottaddie) and [Fiyaz Hasan](http://fiyazhasan.me/)</span></span>

<span data-ttu-id="55f21-105">Eine Single-Page-Anwendung (SPA) ist ein Webanwendungstyp, der aufgrund seiner hohen Servicequalität und Leistungsstärke sehr beliebt ist.</span><span class="sxs-lookup"><span data-stu-id="55f21-105">A Single Page Application (SPA) is a popular type of web application due to its inherent rich user experience.</span></span> <span data-ttu-id="55f21-106">Die Integration clientseitiger SPA-Frameworks oder -Bibliotheken wie [Angular](https://angular.io/) oder [React](https://facebook.github.io/react/) mit serverseitigen Frameworks wie ASP.NET Core kann schwierig sein.</span><span class="sxs-lookup"><span data-stu-id="55f21-106">Integrating client-side SPA frameworks or libraries, such as [Angular](https://angular.io/) or [React](https://facebook.github.io/react/), with server-side frameworks like ASP.NET Core can be difficult.</span></span> <span data-ttu-id="55f21-107">[JavaScriptServices](https://github.com/aspnet/JavaScriptServices) wurde entwickelt, um Inkonsistenzen im Integrationsprozess zu reduzieren.</span><span class="sxs-lookup"><span data-stu-id="55f21-107">[JavaScriptServices](https://github.com/aspnet/JavaScriptServices) was developed to reduce friction in the integration process.</span></span> <span data-ttu-id="55f21-108">Hierdurch wird ein reibungsloser Betrieb verschiedener Client- und Servertechnologiestapel möglich.</span><span class="sxs-lookup"><span data-stu-id="55f21-108">It enables seamless operation between the different client and server technology stacks.</span></span>

<a name="what-is-js-services"></a>

## <a name="what-is-javascriptservices"></a><span data-ttu-id="55f21-109">Was ist JavaScriptServices</span><span class="sxs-lookup"><span data-stu-id="55f21-109">What is JavaScriptServices</span></span>

<span data-ttu-id="55f21-110">JavaScriptServices ist eine Sammlung von clientseitigen Technologien für ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="55f21-110">JavaScriptServices is a collection of client-side technologies for ASP.NET Core.</span></span> <span data-ttu-id="55f21-111">Das Ziel ist, die ASP.NET Core als Entwickler bevorzugte serverseitige Plattform zum Erstellen von SPAs zu positionieren.</span><span class="sxs-lookup"><span data-stu-id="55f21-111">Its goal is to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span>

<span data-ttu-id="55f21-112">JavaScriptServices besteht aus drei unterschiedlichen NuGet-Pakete:</span><span class="sxs-lookup"><span data-stu-id="55f21-112">JavaScriptServices consists of three distinct NuGet packages:</span></span>

* <span data-ttu-id="55f21-113">[Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)</span><span class="sxs-lookup"><span data-stu-id="55f21-113">[Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)</span></span>
* <span data-ttu-id="55f21-114">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)</span><span class="sxs-lookup"><span data-stu-id="55f21-114">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)</span></span>
* <span data-ttu-id="55f21-115">[Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)</span><span class="sxs-lookup"><span data-stu-id="55f21-115">[Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)</span></span>

<span data-ttu-id="55f21-116">Diese Pakete sind nützlich, wenn Sie:</span><span class="sxs-lookup"><span data-stu-id="55f21-116">These packages are useful if you:</span></span>

* <span data-ttu-id="55f21-117">JavaScript auf dem Server ausführen.</span><span class="sxs-lookup"><span data-stu-id="55f21-117">Run JavaScript on the server</span></span>
* <span data-ttu-id="55f21-118">Ein SPA-Framework oder eine Bibliothek verwenden möchten</span><span class="sxs-lookup"><span data-stu-id="55f21-118">Use a SPA framework or library</span></span>
* <span data-ttu-id="55f21-119">Clientseitige Assets mit Webpack erstellen wollen</span><span class="sxs-lookup"><span data-stu-id="55f21-119">Build client-side assets with Webpack</span></span>

<span data-ttu-id="55f21-120">Der Schwerpunkt dieses Artikels liegt auf der Zuhilfenahme des SpaServices-Pakets.</span><span class="sxs-lookup"><span data-stu-id="55f21-120">Much of the focus in this article is placed on using the SpaServices package.</span></span>

<a name="what-is-spa-services"></a>

## <a name="what-is-spaservices"></a><span data-ttu-id="55f21-121">Was ist SpaServices</span><span class="sxs-lookup"><span data-stu-id="55f21-121">What is SpaServices</span></span>

<span data-ttu-id="55f21-122">SpaServices wurde erstellt, damit Entwickler ASP.NET Core bevorzugt als serverseitige Plattform zum Erstellen von SPAs zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="55f21-122">SpaServices was created to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span> <span data-ttu-id="55f21-123">SpaServices ist nicht erforderlich, um SPAs mit ASP.NET Core zu entwickeln, und ist nicht an ein bestimmtes Clientframework gebunden.</span><span class="sxs-lookup"><span data-stu-id="55f21-123">SpaServices isn't required to develop SPAs with ASP.NET Core, and it doesn't lock you into a particular client framework.</span></span>

<span data-ttu-id="55f21-124">SpaServices bietet nützliche Infrastruktur wie z.B.:</span><span class="sxs-lookup"><span data-stu-id="55f21-124">SpaServices provides useful infrastructure such as:</span></span>

* [<span data-ttu-id="55f21-125">Serverseitige prerendering</span><span class="sxs-lookup"><span data-stu-id="55f21-125">Server-side prerendering</span></span>](#server-prerendering)
* [<span data-ttu-id="55f21-126">Webpack-Dev-Middleware</span><span class="sxs-lookup"><span data-stu-id="55f21-126">Webpack Dev Middleware</span></span>](#webpack-dev-middleware)
* [<span data-ttu-id="55f21-127">Der Austausch eines "Hot"</span><span class="sxs-lookup"><span data-stu-id="55f21-127">Hot Module Replacement</span></span>](#hot-module-replacement)
* [<span data-ttu-id="55f21-128">Routing-Hilfsprogramme</span><span class="sxs-lookup"><span data-stu-id="55f21-128">Routing helpers</span></span>](#routing-helpers)

<span data-ttu-id="55f21-129">Diese Infrastrukturkomponenten beschleunigen sowohl den Entwicklungsworkflow als auch die Zusammenarbeit mit der Common Language Runtime-Umgebung.</span><span class="sxs-lookup"><span data-stu-id="55f21-129">Collectively, these infrastructure components enhance both the development workflow and the runtime experience.</span></span> <span data-ttu-id="55f21-130">Die Komponenten können einzeln verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="55f21-130">The components can be adopted individually.</span></span>

<a name="spa-services-prereqs"></a>

## <a name="prerequisites-for-using-spaservices"></a><span data-ttu-id="55f21-131">Voraussetzungen für die Verwendung von SpaServices</span><span class="sxs-lookup"><span data-stu-id="55f21-131">Prerequisites for using SpaServices</span></span>

<span data-ttu-id="55f21-132">Führen Sie die folgenden Schritte aus, um mit SpaServices zu arbeiten:</span><span class="sxs-lookup"><span data-stu-id="55f21-132">To work with SpaServices, install the following:</span></span>

* <span data-ttu-id="55f21-133">[Node.js](https://nodejs.org/) (Version 6 oder höher) mit Npm</span><span class="sxs-lookup"><span data-stu-id="55f21-133">[Node.js](https://nodejs.org/) (version 6 or later) with npm</span></span>
  * <span data-ttu-id="55f21-134">Führen Sie den folgenden Befehl über die Befehlszeile aus, um zu überprüfen, ob diese Komponenten installiert sind und gefunden werden können.</span><span class="sxs-lookup"><span data-stu-id="55f21-134">To verify these components are installed and can be found, run the following from the command line:</span></span>

    ```console
    node -v && npm -v
    ```

<span data-ttu-id="55f21-135">Hinweis: Wenn Sie eine Bereitstellung auf eine Azure-Website vornehmen möchten, müssen Sie hier nichts weiter tun, denn Node.js ist in den Serverumgebungen installiert und verfügbar.</span><span class="sxs-lookup"><span data-stu-id="55f21-135">Note: If you're deploying to an Azure web site, you don't need to do anything here &mdash; Node.js is installed and available in the server environments.</span></span>

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]

  * <span data-ttu-id="55f21-136">Wenn Sie Windows mit Visual Studio 2017 verwenden, wird das SDK durch Auswählen der Option **plattformübergreifende Entwicklung mit .NET Core** installiert.</span><span class="sxs-lookup"><span data-stu-id="55f21-136">If you're on Windows using Visual Studio 2017, the SDK is installed by selecting the **.NET Core cross-platform development** workload.</span></span>

* <span data-ttu-id="55f21-137">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet-Paket</span><span class="sxs-lookup"><span data-stu-id="55f21-137">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet package</span></span>

<a name="server-prerendering"></a>

## <a name="server-side-prerendering"></a><span data-ttu-id="55f21-138">Serverseitige prerendering</span><span class="sxs-lookup"><span data-stu-id="55f21-138">Server-side prerendering</span></span>

<span data-ttu-id="55f21-139">Eine universelle Anwendung (auch bekannt als isomorphe Anwendung) ist eine JavaScript-Anwendung, die sowohl auf dem Server als auch auf dem Client ausgeführt werden kann.</span><span class="sxs-lookup"><span data-stu-id="55f21-139">A universal (also known as isomorphic) application is a JavaScript application capable of running both on the server and the client.</span></span> <span data-ttu-id="55f21-140">Angular, React und andere beliebte Frameworks bieten eine universelle Plattform für diesen Anwendungsentwicklungsstil.</span><span class="sxs-lookup"><span data-stu-id="55f21-140">Angular, React, and other popular frameworks provide a universal platform for this application development style.</span></span> <span data-ttu-id="55f21-141">Die Idee dahinter ist, zunächst die Framework-Komponenten über Node.js auf dem Server zu rendern und dann weitere Ausführungen an den Client zu delegieren.</span><span class="sxs-lookup"><span data-stu-id="55f21-141">The idea is to first render the framework components on the server via Node.js, and then delegate further execution to the client.</span></span>

<span data-ttu-id="55f21-142">ASP.NET Core-[Taghilfsprogramme](xref:mvc/views/tag-helpers/intro), bereitgestellt durch SpaServices, vereinfachen die Implementierung von serverseitigem Prerendering durch Bereitstellen und Aufrufen der JavaScript-Funktionen auf dem Server.</span><span class="sxs-lookup"><span data-stu-id="55f21-142">ASP.NET Core [Tag Helpers](xref:mvc/views/tag-helpers/intro) provided by SpaServices simplify the implementation of server-side prerendering by invoking the JavaScript functions on the server.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="55f21-143">Vorraussetzungen</span><span class="sxs-lookup"><span data-stu-id="55f21-143">Prerequisites</span></span>

<span data-ttu-id="55f21-144">Installieren Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="55f21-144">Install the following:</span></span>

* <span data-ttu-id="55f21-145">[ASPNET-Prerendering](https://www.npmjs.com/package/aspnet-prerendering) Npm-Paket:</span><span class="sxs-lookup"><span data-stu-id="55f21-145">[aspnet-prerendering](https://www.npmjs.com/package/aspnet-prerendering) npm package:</span></span>

    ```console
    npm i -S aspnet-prerendering
    ```

### <a name="configuration"></a><span data-ttu-id="55f21-146">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="55f21-146">Configuration</span></span>

<span data-ttu-id="55f21-147">Die Taghilfsprogramme werden im Projekt über die Namespace-Registrierung in der *_ViewImports.cshtml*-Datei erkennbar gemacht:</span><span class="sxs-lookup"><span data-stu-id="55f21-147">The Tag Helpers are made discoverable via namespace registration in the project's *_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/_ViewImports.cshtml?highlight=3)]

<span data-ttu-id="55f21-148">Diese Taghilfsprogramme abstrahieren die Feinheiten der direkten Kommunikation mit Low-Level-APIs durch die Nutzung einer HTML-ähnlichen Syntax in der Razor-Ansicht:</span><span class="sxs-lookup"><span data-stu-id="55f21-148">These Tag Helpers abstract away the intricacies of communicating directly with low-level APIs by leveraging an HTML-like syntax inside the Razor view:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=5)]

### <a name="the-asp-prerender-module-tag-helper"></a><span data-ttu-id="55f21-149">Die `asp-prerender-module` Taghilfsprogramm</span><span class="sxs-lookup"><span data-stu-id="55f21-149">The `asp-prerender-module` Tag Helper</span></span>

<span data-ttu-id="55f21-150">Die `asp-prerender-module` Taghilfsprogramm, in dem vorherigen Beispiel, verwendet führt *ClientApp/dist/main-server.js* auf dem Server über Node.js.</span><span class="sxs-lookup"><span data-stu-id="55f21-150">The `asp-prerender-module` Tag Helper, used in the preceding code example, executes *ClientApp/dist/main-server.js* on the server via Node.js.</span></span> <span data-ttu-id="55f21-151">Für die der Verständlichkeit *Main-Datei "Server.js"* Datei ist ein Artefakt der Aufgabe in TypeScript und JavaScript-transpilation ziehen die [Webpack](http://webpack.github.io/) Buildprozess.</span><span class="sxs-lookup"><span data-stu-id="55f21-151">For clarity's sake, *main-server.js* file is an artifact of the TypeScript-to-JavaScript transpilation task in the [Webpack](http://webpack.github.io/) build process.</span></span> <span data-ttu-id="55f21-152">Webpack definiert einen Eintrag-Punkt-Alias für `main-server`; und das Abhängigkeitsdiagramm für diesen Alias Durchlauf beginnt bei der *ClientApp/Boot-server.ts* Datei:</span><span class="sxs-lookup"><span data-stu-id="55f21-152">Webpack defines an entry point alias of `main-server`; and, traversal of the dependency graph for this alias begins at the *ClientApp/boot-server.ts* file:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=53)]

<span data-ttu-id="55f21-153">Im folgenden Beispiel Angular die *ClientApp/Boot-server.ts* Datei nutzt die `createServerRenderer` Funktion und `RenderResult` Typ der `aspnet-prerendering` Npm-Paket so konfigurieren Sie Server-Rendering über Node.js.</span><span class="sxs-lookup"><span data-stu-id="55f21-153">In the following Angular example, the *ClientApp/boot-server.ts* file utilizes the `createServerRenderer` function and `RenderResult` type of the `aspnet-prerendering` npm package to configure server rendering via Node.js.</span></span> <span data-ttu-id="55f21-154">Das HTML-Markup, das bestimmt, für serverseitiges Rendering auf ein Funktionsaufruf auflösen übergeben wird, die von einer stark typisierten JavaScript umgeben sind `Promise` Objekt.</span><span class="sxs-lookup"><span data-stu-id="55f21-154">The HTML markup destined for server-side rendering is passed to a resolve function call, which is wrapped in a strongly-typed JavaScript `Promise` object.</span></span> <span data-ttu-id="55f21-155">Die `Promise` des Objekts "significance" ist, dass sie asynchron die HTML-Markup auf der Seite zur Injektion in das DOM Platzhalterelement bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="55f21-155">The `Promise` object's significance is that it asynchronously supplies the HTML markup to the page for injection in the DOM's placeholder element.</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-34,79-)]

### <a name="the-asp-prerender-data-tag-helper"></a><span data-ttu-id="55f21-156">Die `asp-prerender-data` Taghilfsprogramm</span><span class="sxs-lookup"><span data-stu-id="55f21-156">The `asp-prerender-data` Tag Helper</span></span>

<span data-ttu-id="55f21-157">In Kombination mit der `asp-prerender-module` Taghilfsprogramm, das `asp-prerender-data` Taghilfsprogramm kann verwendet werden, um die Kontextinformationen für das serverseitige JavaScript von der Razor-Ansicht zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="55f21-157">When coupled with the `asp-prerender-module` Tag Helper, the `asp-prerender-data` Tag Helper can be used to pass contextual information from the Razor view to the server-side JavaScript.</span></span> <span data-ttu-id="55f21-158">Das folgende Markup übergibt beispielsweise Benutzerdaten der `main-server` Modul:</span><span class="sxs-lookup"><span data-stu-id="55f21-158">For example, the following markup passes user data to the `main-server` module:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=9-12)]

<span data-ttu-id="55f21-159">Die empfangenen Daten `UserName` Argument wird mit der integrierten JSON-Serialisierungsprogramm serialisiert und befindet sich in der `params.data` Objekt.</span><span class="sxs-lookup"><span data-stu-id="55f21-159">The received `UserName` argument is serialized using the built-in JSON serializer and is stored in the `params.data` object.</span></span> <span data-ttu-id="55f21-160">Im folgenden Beispiel Angular-die Daten werden verwendet, um persönlich begrüßt im Erstellen einer `h1` Element:</span><span class="sxs-lookup"><span data-stu-id="55f21-160">In the following Angular example, the data is used to construct a personalized greeting within an `h1` element:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,38-52,79-)]

<span data-ttu-id="55f21-161">Hinweis: Taghilfsprogramme übergebene Eigenschaftennamen mit dargestellt **PascalCase** Notation.</span><span class="sxs-lookup"><span data-stu-id="55f21-161">Note: Property names passed in Tag Helpers are represented with **PascalCase** notation.</span></span> <span data-ttu-id="55f21-162">Im Gegensatz dazu stehen für JavaScript ist, in denen die gleichen Eigenschaftennamen mit dargestellt **CamelCase**.</span><span class="sxs-lookup"><span data-stu-id="55f21-162">Contrast that to JavaScript, where the same property names are represented with **camelCase**.</span></span> <span data-ttu-id="55f21-163">Die Standardkonfiguration für JSON-Serialisierung ist verantwortlich für diesen Unterschied.</span><span class="sxs-lookup"><span data-stu-id="55f21-163">The default JSON serialization configuration is responsible for this difference.</span></span>

<span data-ttu-id="55f21-164">Zum Erweitern auf das vorherige Codebeispiel Daten können werden vom Server an die Ansicht übergeben von hydrating der `globals` Eigenschaft bereitgestellt, um die `resolve` Funktion:</span><span class="sxs-lookup"><span data-stu-id="55f21-164">To expand upon the preceding code example, data can be passed from the server to the view by hydrating the `globals` property provided to the `resolve` function:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,57-77,79-)]

<span data-ttu-id="55f21-165">Die `postList` Array definiert, die innerhalb der `globals` Objekt angefügt ist im Browsers auf die globale `window` Objekt.</span><span class="sxs-lookup"><span data-stu-id="55f21-165">The `postList` array defined inside the `globals` object is attached to the browser's global `window` object.</span></span> <span data-ttu-id="55f21-166">Diese Variable anheben globalen Bereich entfällt die Duplizierung von Aufwand, vor allem mit Bezug auf die gleichen Daten einmal auf dem Server und erneut auf dem Client werden geladen.</span><span class="sxs-lookup"><span data-stu-id="55f21-166">This variable hoisting to global scope eliminates duplication of effort, particularly as it pertains to loading the same data once on the server and again on the client.</span></span>

![Globale postList Variable Window-Objekt angefügt](spa-services/_static/global_variable.png)

<a name="webpack-dev-middleware"></a>

## <a name="webpack-dev-middleware"></a><span data-ttu-id="55f21-168">Webpack-Dev-Middleware</span><span class="sxs-lookup"><span data-stu-id="55f21-168">Webpack Dev Middleware</span></span>

<span data-ttu-id="55f21-169">[Webpack-Dev-Middleware](https://webpack.github.io/docs/webpack-dev-middleware.html) bietet einen optimierte Entwicklung-Workflow, bei dem Webpack Ressourcen bei Bedarf erstellt.</span><span class="sxs-lookup"><span data-stu-id="55f21-169">[Webpack Dev Middleware](https://webpack.github.io/docs/webpack-dev-middleware.html) introduces a streamlined development workflow whereby Webpack builds resources on demand.</span></span> <span data-ttu-id="55f21-170">Die Middleware automatisch kompiliert und clientseitige Ressourcen dient, wenn eine Seite im Browser geladen wird.</span><span class="sxs-lookup"><span data-stu-id="55f21-170">The middleware automatically compiles and serves client-side resources when a page is reloaded in the browser.</span></span> <span data-ttu-id="55f21-171">Der alternative Ansatz ist Webpack über Npm-Buildskript des Projekts manuell aufrufen, wenn eine Drittanbieter Abhängigkeit oder den benutzerdefinierten Code geändert wird.</span><span class="sxs-lookup"><span data-stu-id="55f21-171">The alternate approach is to manually invoke Webpack via the project's npm build script when a third-party dependency or the custom code changes.</span></span> <span data-ttu-id="55f21-172">Ein Npm-Buildskript die *"Package.JSON"* Datei ist im folgenden Beispiel dargestellt:</span><span class="sxs-lookup"><span data-stu-id="55f21-172">An npm build script in the *package.json* file is shown in the following example:</span></span>

```json
"build": "npm run build:vendor && npm run build:custom",
```

### <a name="prerequisites"></a><span data-ttu-id="55f21-173">Vorraussetzungen</span><span class="sxs-lookup"><span data-stu-id="55f21-173">Prerequisites</span></span>

<span data-ttu-id="55f21-174">Installieren Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="55f21-174">Install the following:</span></span>

* <span data-ttu-id="55f21-175">[ASPNET-Webpack](https://www.npmjs.com/package/aspnet-webpack) Npm-Paket:</span><span class="sxs-lookup"><span data-stu-id="55f21-175">[aspnet-webpack](https://www.npmjs.com/package/aspnet-webpack) npm package:</span></span>

    ```console
    npm i -D aspnet-webpack
    ```

### <a name="configuration"></a><span data-ttu-id="55f21-176">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="55f21-176">Configuration</span></span>

<span data-ttu-id="55f21-177">Webpack-Dev-Middleware wird registriert, in die HTTP-Anforderungspipeline über den folgenden Code in die *"Startup.cs"* dateimodell `Configure` Methode:</span><span class="sxs-lookup"><span data-stu-id="55f21-177">Webpack Dev Middleware is registered into the HTTP request pipeline via the following code in the *Startup.cs* file's `Configure` method:</span></span>

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=webpack-middleware-registration&highlight=4)]

<span data-ttu-id="55f21-178">Die `UseWebpackDevMiddleware` Erweiterungsmethode aufgerufen werden muss, bevor [registrieren, statische Datei hosting](xref:fundamentals/static-files) über die `UseStaticFiles` -Erweiterungsmethode.</span><span class="sxs-lookup"><span data-stu-id="55f21-178">The `UseWebpackDevMiddleware` extension method must be called before [registering static file hosting](xref:fundamentals/static-files) via the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="55f21-179">Registrieren Sie die Middleware aus Sicherheitsgründen nur, wenn die app im Entwicklungsmodus ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="55f21-179">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="55f21-180">Die *webpack.config.js* dateimodell `output.publicPath` Eigenschaft gibt die Middleware, sehen Sie sich die `dist` Ordner auf Änderungen:</span><span class="sxs-lookup"><span data-stu-id="55f21-180">The *webpack.config.js* file's `output.publicPath` property tells the middleware to watch the `dist` folder for changes:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,13-16)]

<a name="hot-module-replacement"></a>

## <a name="hot-module-replacement"></a><span data-ttu-id="55f21-181">Der Austausch eines "Hot"</span><span class="sxs-lookup"><span data-stu-id="55f21-181">Hot Module Replacement</span></span>

<span data-ttu-id="55f21-182">Stellen Sie sich den Webpack ["Hot" Austausch eines Controllermoduls](https://webpack.js.org/concepts/hot-module-replacement/) (HMR)-Funktion als Weiterentwicklung von [Webpack-Dev-Middleware](#webpack-dev-middleware).</span><span class="sxs-lookup"><span data-stu-id="55f21-182">Think of Webpack's [Hot Module Replacement](https://webpack.js.org/concepts/hot-module-replacement/) (HMR) feature as an evolution of [Webpack Dev Middleware](#webpack-dev-middleware).</span></span> <span data-ttu-id="55f21-183">HMR führt die gleichen Vorteile, aber es weiter den Entwicklungsworkflow optimiert, indem die automatische Aktualisierung der Seiteninhalt nach dem Kompilieren der Änderungen.</span><span class="sxs-lookup"><span data-stu-id="55f21-183">HMR introduces all the same benefits, but it further streamlines the development workflow by automatically updating page content after compiling the changes.</span></span> <span data-ttu-id="55f21-184">Verwechseln Sie dies nicht mit einer Aktualisierung des Browsers, die den aktuellen in-Memory-Status und die Debugsitzung der SPA beeinträchtigen würde.</span><span class="sxs-lookup"><span data-stu-id="55f21-184">Don't confuse this with a refresh of the browser, which would interfere with the current in-memory state and debugging session of the SPA.</span></span> <span data-ttu-id="55f21-185">Es ist ein live-Link zwischen den Webpack-Dev-Middleware-Dienst und dem Browser, was bedeutet, dass Änderungen an den Browser mithilfe von Push übertragen werden.</span><span class="sxs-lookup"><span data-stu-id="55f21-185">There's a live link between the Webpack Dev Middleware service and the browser, which means changes are pushed to the browser.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="55f21-186">Vorraussetzungen</span><span class="sxs-lookup"><span data-stu-id="55f21-186">Prerequisites</span></span>

<span data-ttu-id="55f21-187">Installieren Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="55f21-187">Install the following:</span></span>

* <span data-ttu-id="55f21-188">[Webpack-hot-Middleware](https://www.npmjs.com/package/webpack-hot-middleware) Npm-Paket:</span><span class="sxs-lookup"><span data-stu-id="55f21-188">[webpack-hot-middleware](https://www.npmjs.com/package/webpack-hot-middleware) npm package:</span></span>

    ```console
    npm i -D webpack-hot-middleware
    ```

### <a name="configuration"></a><span data-ttu-id="55f21-189">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="55f21-189">Configuration</span></span>

<span data-ttu-id="55f21-190">Die Komponente HMR muss registriert werden, in MVC HTTP-Anforderungspipeline in die `Configure` Methode:</span><span class="sxs-lookup"><span data-stu-id="55f21-190">The HMR component must be registered into MVC's HTTP request pipeline in the `Configure` method:</span></span>

```csharp
app.UseWebpackDevMiddleware(new WebpackDevMiddlewareOptions {
    HotModuleReplacement = true
});
```

<span data-ttu-id="55f21-191">Wie wurde mit "true" [Webpack-Dev-Middleware](#webpack-dev-middleware), `UseWebpackDevMiddleware` Erweiterungsmethode aufgerufen werden muss, bevor die `UseStaticFiles` -Erweiterungsmethode.</span><span class="sxs-lookup"><span data-stu-id="55f21-191">As was true with [Webpack Dev Middleware](#webpack-dev-middleware), the `UseWebpackDevMiddleware` extension method must be called before the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="55f21-192">Registrieren Sie die Middleware aus Sicherheitsgründen nur, wenn die app im Entwicklungsmodus ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="55f21-192">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="55f21-193">Die *webpack.config.js* Datei definieren, muss ein `plugins` array, auch wenn es leer gelassen wird:</span><span class="sxs-lookup"><span data-stu-id="55f21-193">The *webpack.config.js* file must define a `plugins` array, even if it's left empty:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,25)]

<span data-ttu-id="55f21-194">Nach dem Laden der app im Browser, bietet die Registerkarte für die Entwicklertools-Konsole zur Bestätigung der HMR Aktivierung:</span><span class="sxs-lookup"><span data-stu-id="55f21-194">After loading the app in the browser, the developer tools' Console tab provides confirmation of HMR activation:</span></span>

!["Hot" verbundene Austausch eines Controllermoduls-Nachricht](spa-services/_static/hmr_connected.png)

<a name="routing-helpers"></a>

## <a name="routing-helpers"></a><span data-ttu-id="55f21-196">Routing-Hilfsprogramme</span><span class="sxs-lookup"><span data-stu-id="55f21-196">Routing helpers</span></span>

<span data-ttu-id="55f21-197">In den meisten ASP.NET Core-basierte SPAs werden sollten Sie die clientseitige routing neben dem routing von serverseitigen.</span><span class="sxs-lookup"><span data-stu-id="55f21-197">In most ASP.NET Core-based SPAs, you'll want client-side routing in addition to server-side routing.</span></span> <span data-ttu-id="55f21-198">Die SPA und MVC-routing-Systeme können ohne Störung unabhängig voneinander arbeiten.</span><span class="sxs-lookup"><span data-stu-id="55f21-198">The SPA and MVC routing systems can work independently without interference.</span></span> <span data-ttu-id="55f21-199">Vorhanden ist, jedoch einen Edge Groß-/Kleinschreibung machen Herausforderungen: Identifizieren von 404-HTTP-Antworten.</span><span class="sxs-lookup"><span data-stu-id="55f21-199">There's, however, one edge case posing challenges: identifying 404 HTTP responses.</span></span>

<span data-ttu-id="55f21-200">Betrachten Sie das Szenario, in dem eine Route ohne Erweiterung `/some/page` verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="55f21-200">Consider the scenario in which an extensionless route of `/some/page` is used.</span></span> <span data-ttu-id="55f21-201">Angenommen Sie, die Anforderung nicht-Musterübereinstimmung eine serverseitige Weg, Verwendungsmuster stimmt jedoch mit einer Client-Side-Route.</span><span class="sxs-lookup"><span data-stu-id="55f21-201">Assume the request doesn't pattern-match a server-side route, but its pattern does match a client-side route.</span></span> <span data-ttu-id="55f21-202">Betrachten Sie nun eine eingehende Anforderung für `/images/user-512.png`, die in der Regel davon ausgeht, eine Bilddatei auf dem Server.</span><span class="sxs-lookup"><span data-stu-id="55f21-202">Now consider an incoming request for `/images/user-512.png`, which generally expects to find an image file on the server.</span></span> <span data-ttu-id="55f21-203">Wenn dieser Pfad für die angeforderte Ressource keine serverseitigen Route oder eine statische Datei entspricht, ist es unwahrscheinlich, die die clientseitige Anwendung verarbeitet werden sollen, in der Regel eine HTTP-Statuscode 404 zurückgegeben werden soll.</span><span class="sxs-lookup"><span data-stu-id="55f21-203">If that requested resource path doesn't match any server-side route or static file, it's unlikely that the client-side application would handle it — you generally want to return a 404 HTTP status code.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="55f21-204">Vorraussetzungen</span><span class="sxs-lookup"><span data-stu-id="55f21-204">Prerequisites</span></span>

<span data-ttu-id="55f21-205">Installieren Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="55f21-205">Install the following:</span></span>

* <span data-ttu-id="55f21-206">Das routing Npm-Paket für die clientseitige.</span><span class="sxs-lookup"><span data-stu-id="55f21-206">The client-side routing npm package.</span></span> <span data-ttu-id="55f21-207">Verwenden von Angular als Beispiel:</span><span class="sxs-lookup"><span data-stu-id="55f21-207">Using Angular as an example:</span></span>

    ```console
    npm i -S @angular/router
    ```

### <a name="configuration"></a><span data-ttu-id="55f21-208">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="55f21-208">Configuration</span></span>

<span data-ttu-id="55f21-209">Eine Erweiterungsmethode namens `MapSpaFallbackRoute` werden in der `Configure` Methode:</span><span class="sxs-lookup"><span data-stu-id="55f21-209">An extension method named `MapSpaFallbackRoute` is used in the `Configure` method:</span></span>

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=mvc-routing-table&highlight=7-9)]

<span data-ttu-id="55f21-210">Tipp: Routen werden in der Reihenfolge ausgewertet, in denen sie konfiguriert sind.</span><span class="sxs-lookup"><span data-stu-id="55f21-210">Tip: Routes are evaluated in the order in which they're configured.</span></span> <span data-ttu-id="55f21-211">Daher die `default` Route im obigen Codebeispiel wird zuerst für den Musterabgleich verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="55f21-211">Consequently, the `default` route in the preceding code example is used first for pattern matching.</span></span>

<a name="new-project-creation"></a>

## <a name="creating-a-new-project"></a><span data-ttu-id="55f21-212">Erstellen eines neuen Projekts</span><span class="sxs-lookup"><span data-stu-id="55f21-212">Creating a new project</span></span>

<span data-ttu-id="55f21-213">JavaScriptServices bietet vorkonfigurierte Anwendungsvorlagen an.</span><span class="sxs-lookup"><span data-stu-id="55f21-213">JavaScriptServices provides pre-configured application templates.</span></span> <span data-ttu-id="55f21-214">SpaServices wird in dieser Vorlagen in Verbindung mit verschiedenen Frameworks und Bibliotheken wie Angular und React mit Redux verwendet.</span><span class="sxs-lookup"><span data-stu-id="55f21-214">SpaServices is used in these templates, in conjunction with different frameworks and libraries such as Angular, React, and Redux.</span></span>

<span data-ttu-id="55f21-215">Diese Vorlagen können über die .NET Core-CLI installiert werden, mithilfe des folgenden Befehls:</span><span class="sxs-lookup"><span data-stu-id="55f21-215">These templates can be installed via the .NET Core CLI by running the following command:</span></span>

```console
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

<span data-ttu-id="55f21-216">Eine Liste der verfügbaren SPA-Vorlagen wird angezeigt:</span><span class="sxs-lookup"><span data-stu-id="55f21-216">A list of available SPA templates is displayed:</span></span>

| <span data-ttu-id="55f21-217">Vorlagen</span><span class="sxs-lookup"><span data-stu-id="55f21-217">Templates</span></span>                                 | <span data-ttu-id="55f21-218">Kurzname</span><span class="sxs-lookup"><span data-stu-id="55f21-218">Short Name</span></span> | <span data-ttu-id="55f21-219">Sprache</span><span class="sxs-lookup"><span data-stu-id="55f21-219">Language</span></span> | <span data-ttu-id="55f21-220">Tags</span><span class="sxs-lookup"><span data-stu-id="55f21-220">Tags</span></span>        |
|:------------------------------------------|:-----------|:---------|:------------|
| <span data-ttu-id="55f21-221">MVC, ASP.NET Core mit Angular</span><span class="sxs-lookup"><span data-stu-id="55f21-221">MVC ASP.NET Core with Angular</span></span>             | <span data-ttu-id="55f21-222">angular</span><span class="sxs-lookup"><span data-stu-id="55f21-222">angular</span></span>    | <span data-ttu-id="55f21-223">[C#]</span><span class="sxs-lookup"><span data-stu-id="55f21-223">[C#]</span></span>     | <span data-ttu-id="55f21-224">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="55f21-224">Web/MVC/SPA</span></span> |
| <span data-ttu-id="55f21-225">MVC, ASP.NET Core mit React.js</span><span class="sxs-lookup"><span data-stu-id="55f21-225">MVC ASP.NET Core with React.js</span></span>            | <span data-ttu-id="55f21-226">react</span><span class="sxs-lookup"><span data-stu-id="55f21-226">react</span></span>      | <span data-ttu-id="55f21-227">[C#]</span><span class="sxs-lookup"><span data-stu-id="55f21-227">[C#]</span></span>     | <span data-ttu-id="55f21-228">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="55f21-228">Web/MVC/SPA</span></span> |
| <span data-ttu-id="55f21-229">MVC, ASP.NET Core mit React.js und Redux</span><span class="sxs-lookup"><span data-stu-id="55f21-229">MVC ASP.NET Core with React.js and Redux</span></span>  | <span data-ttu-id="55f21-230">reactredux</span><span class="sxs-lookup"><span data-stu-id="55f21-230">reactredux</span></span> | <span data-ttu-id="55f21-231">[C#]</span><span class="sxs-lookup"><span data-stu-id="55f21-231">[C#]</span></span>     | <span data-ttu-id="55f21-232">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="55f21-232">Web/MVC/SPA</span></span> |

<span data-ttu-id="55f21-233">Zum Erstellen eines neuen Projekts mithilfe einer der SPA-Vorlagen enthalten die **Kurznamen** der Vorlage in der [Dotnet neue](/dotnet/core/tools/dotnet-new) Befehl.</span><span class="sxs-lookup"><span data-stu-id="55f21-233">To create a new project using one of the SPA templates, include the **Short Name** of the template in the [dotnet new](/dotnet/core/tools/dotnet-new) command.</span></span> <span data-ttu-id="55f21-234">Der folgende Befehl erstellt eine Angular-Anwendung mit ASP.NET Core MVC, die für die Serverseite konfiguriert:</span><span class="sxs-lookup"><span data-stu-id="55f21-234">The following command creates an Angular application with ASP.NET Core MVC configured for the server side:</span></span>

```console
dotnet new angular
```

<a name="runtime-config-mode"></a>

### <a name="set-the-runtime-configuration-mode"></a><span data-ttu-id="55f21-235">Legen Sie den Modus der Common Language Runtime-Konfiguration</span><span class="sxs-lookup"><span data-stu-id="55f21-235">Set the runtime configuration mode</span></span>

<span data-ttu-id="55f21-236">Zwei Modi der primären Common Language Runtime-Konfiguration vorhanden sein:</span><span class="sxs-lookup"><span data-stu-id="55f21-236">Two primary runtime configuration modes exist:</span></span>

* <span data-ttu-id="55f21-237">**Entwicklung**:</span><span class="sxs-lookup"><span data-stu-id="55f21-237">**Development**:</span></span>
  * <span data-ttu-id="55f21-238">Enthält quellzuordnungen zur Erleichterung des Debuggings.</span><span class="sxs-lookup"><span data-stu-id="55f21-238">Includes source maps to ease debugging.</span></span>
  * <span data-ttu-id="55f21-239">Der clientseitige Code für die Leistung nicht optimiert werden.</span><span class="sxs-lookup"><span data-stu-id="55f21-239">Doesn't optimize the client-side code for performance.</span></span>
* <span data-ttu-id="55f21-240">**Produktion**:</span><span class="sxs-lookup"><span data-stu-id="55f21-240">**Production**:</span></span>
  * <span data-ttu-id="55f21-241">Schließt quellzuordnungen an.</span><span class="sxs-lookup"><span data-stu-id="55f21-241">Excludes source maps.</span></span>
  * <span data-ttu-id="55f21-242">Optimiert den clientseitigen Code über Bündelung und Minimierung.</span><span class="sxs-lookup"><span data-stu-id="55f21-242">Optimizes the client-side code via bundling & minification.</span></span>

<span data-ttu-id="55f21-243">ASP.NET Core verwendet eine Umgebungsvariable namens `ASPNETCORE_ENVIRONMENT` zum Speichern von des Konfigurationsmodus zu wechseln.</span><span class="sxs-lookup"><span data-stu-id="55f21-243">ASP.NET Core uses an environment variable named `ASPNETCORE_ENVIRONMENT` to store the configuration mode.</span></span> <span data-ttu-id="55f21-244">Finden Sie unter **[legen Sie die Umgebung](xref:fundamentals/environments#set-the-environment)** für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="55f21-244">See **[Set the environment](xref:fundamentals/environments#set-the-environment)** for more information.</span></span>

### <a name="running-with-net-core-cli"></a><span data-ttu-id="55f21-245">Mit .NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="55f21-245">Running with .NET Core CLI</span></span>

<span data-ttu-id="55f21-246">Stellen Sie die erforderlichen NuGet und Npm-Pakete mithilfe des folgenden Befehls auf das Stammverzeichnis des Projekts wieder her:</span><span class="sxs-lookup"><span data-stu-id="55f21-246">Restore the required NuGet and npm packages by running the following command at the project root:</span></span>

```console
dotnet restore && npm i
```

<span data-ttu-id="55f21-247">Erstellen und Ausführen der Anwendungs:</span><span class="sxs-lookup"><span data-stu-id="55f21-247">Build and run the application:</span></span>

```console
dotnet run
```

<span data-ttu-id="55f21-248">Starten der Anwendung auf "localhost", gemäß der [Common Language Runtime-Konfigurationsmodus](#runtime-config-mode).</span><span class="sxs-lookup"><span data-stu-id="55f21-248">The application starts on localhost according to the [runtime configuration mode](#runtime-config-mode).</span></span> <span data-ttu-id="55f21-249">Navigieren zu `http://localhost:5000` im Browser zeigt die Landing Page.</span><span class="sxs-lookup"><span data-stu-id="55f21-249">Navigating to `http://localhost:5000` in the browser displays the landing page.</span></span>

### <a name="running-with-visual-studio-2017"></a><span data-ttu-id="55f21-250">Mit Visual Studio 2017 ausgeführt wird</span><span class="sxs-lookup"><span data-stu-id="55f21-250">Running with Visual Studio 2017</span></span>

<span data-ttu-id="55f21-251">Öffnen der *csproj* von generierten Datei das [Dotnet neue](/dotnet/core/tools/dotnet-new) Befehl.</span><span class="sxs-lookup"><span data-stu-id="55f21-251">Open the *.csproj* file generated by the [dotnet new](/dotnet/core/tools/dotnet-new) command.</span></span> <span data-ttu-id="55f21-252">Die erforderlichen NuGet und Npm-Pakete werden automatisch auf dem geöffneten Projekt wiederhergestellt.</span><span class="sxs-lookup"><span data-stu-id="55f21-252">The required NuGet and npm packages are restored automatically upon project open.</span></span> <span data-ttu-id="55f21-253">Dieser Wiederherstellungsprozess kann einige Minuten dauern, und die Anwendung ist bereit, die ausgeführt werden, wenn der Vorgang abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="55f21-253">This restoration process may take up to a few minutes, and the application is ready to run when it completes.</span></span> <span data-ttu-id="55f21-254">Klicken Sie auf die grüne Schaltfläche "ausführen", oder drücken Sie `Ctrl + F5`, und der Browser zur Startseite der Anwendung geöffnet wird.</span><span class="sxs-lookup"><span data-stu-id="55f21-254">Click the green run button or press `Ctrl + F5`, and the browser opens to the application's landing page.</span></span> <span data-ttu-id="55f21-255">Die Anwendung ausgeführt wird, auf "localhost", gemäß der [Common Language Runtime-Konfigurationsmodus](#runtime-config-mode).</span><span class="sxs-lookup"><span data-stu-id="55f21-255">The application runs on localhost according to the [runtime configuration mode](#runtime-config-mode).</span></span>

<a name="app-testing"></a>

## <a name="testing-the-app"></a><span data-ttu-id="55f21-256">Testen der app</span><span class="sxs-lookup"><span data-stu-id="55f21-256">Testing the app</span></span>

<span data-ttu-id="55f21-257">SpaServices Vorlagen sind so vorkonfiguriert, dass die clientseitige Tests mithilfe von [Karma](https://karma-runner.github.io/1.0/index.html) und [Jasmine](https://jasmine.github.io/).</span><span class="sxs-lookup"><span data-stu-id="55f21-257">SpaServices templates are pre-configured to run client-side tests using [Karma](https://karma-runner.github.io/1.0/index.html) and [Jasmine](https://jasmine.github.io/).</span></span> <span data-ttu-id="55f21-258">Jasmine ist eine beliebte Komponententest-Framework für JavaScript, während Karma einen Test Runner für diese Tests ist.</span><span class="sxs-lookup"><span data-stu-id="55f21-258">Jasmine is a popular unit testing framework for JavaScript, whereas Karma is a test runner for those tests.</span></span> <span data-ttu-id="55f21-259">Karma ist so konfiguriert, dass die Arbeit mit der [Webpack-Dev-Middleware](#webpack-dev-middleware) so, dass der Entwickler ist nicht erforderlich sind, beenden, und führen Sie den Test, jedes Mal, wenn Änderungen vorgenommen werden.</span><span class="sxs-lookup"><span data-stu-id="55f21-259">Karma is configured to work with the [Webpack Dev Middleware](#webpack-dev-middleware) such that the developer isn't required to stop and run the test every time changes are made.</span></span> <span data-ttu-id="55f21-260">Ob sie den Code, der den Testfall oder den Testfall selbst ausgeführt wird, wird der Test automatisch ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="55f21-260">Whether it's the code running against the test case or the test case itself, the test runs automatically.</span></span>

<span data-ttu-id="55f21-261">Verwenden Sie als Beispiel die Angular-Anwendung ein, zwei Jasmine Testfälle bereits stehen für die `CounterComponent` in die *counter.component.spec.ts* Datei:</span><span class="sxs-lookup"><span data-stu-id="55f21-261">Using the Angular application as an example, two Jasmine test cases are already provided for the `CounterComponent` in the *counter.component.spec.ts* file:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/app/components/counter/counter.component.spec.ts?range=15-28)]

<span data-ttu-id="55f21-262">Öffnen Sie die Eingabeaufforderung in das *ClientApp* Verzeichnis.</span><span class="sxs-lookup"><span data-stu-id="55f21-262">Open the command prompt in the *ClientApp* directory.</span></span> <span data-ttu-id="55f21-263">Führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="55f21-263">Run the following command:</span></span>

```console
npm test
```

<span data-ttu-id="55f21-264">Das Skript startet die Karma-Testprogramm, die in definierten Einstellungen liest die *karma.conf.js* Datei.</span><span class="sxs-lookup"><span data-stu-id="55f21-264">The script launches the Karma test runner, which reads the settings defined in the *karma.conf.js* file.</span></span> <span data-ttu-id="55f21-265">Neben anderen Einstellungen die *karma.conf.js* identifiziert die Testdateien über auszuführende seine `files` Array:</span><span class="sxs-lookup"><span data-stu-id="55f21-265">Among other settings, the *karma.conf.js* identifies the test files to be executed via its `files` array:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/test/karma.conf.js?range=4-5,8-11)]

<a name="app-publishing"></a>

## <a name="publishing-the-application"></a><span data-ttu-id="55f21-266">Veröffentlichen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="55f21-266">Publishing the application</span></span>

<span data-ttu-id="55f21-267">Kombinieren die generierte clientseitige Ressourcen und die veröffentlichte Elemente, die ASP.NET Core in einem Ready-to-deploy-Paket kann aufwendig sein.</span><span class="sxs-lookup"><span data-stu-id="55f21-267">Combining the generated client-side assets and the published ASP.NET Core artifacts into a ready-to-deploy package can be cumbersome.</span></span> <span data-ttu-id="55f21-268">Glücklicherweise orchestriert SpaServices dieses Prozesses für die gesamte Veröffentlichung mit einem benutzerdefinierten MSBuild-Ziel mit dem Namen `RunWebpack`:</span><span class="sxs-lookup"><span data-stu-id="55f21-268">Thankfully, SpaServices orchestrates that entire publication process with a custom MSBuild target named `RunWebpack`:</span></span>

[!code-xml[](../client-side/spa-services/sample/SpaServicesSampleApp/SpaServicesSampleApp.csproj?range=31-45)]

<span data-ttu-id="55f21-269">Das MSBuild-Ziel hat die folgenden Verantwortungen:</span><span class="sxs-lookup"><span data-stu-id="55f21-269">The MSBuild target has the following responsibilities:</span></span>

1. <span data-ttu-id="55f21-270">Die Npm-Pakete wiederherstellen</span><span class="sxs-lookup"><span data-stu-id="55f21-270">Restore the npm packages</span></span>
1. <span data-ttu-id="55f21-271">Erstellen Sie einen Build Produktionsniveau von Drittanbietern, clientseitige Ressourcen</span><span class="sxs-lookup"><span data-stu-id="55f21-271">Create a production-grade build of the third-party, client-side assets</span></span>
1. <span data-ttu-id="55f21-272">Erstellen Sie einen Build Produktionsniveau benutzerdefinierte clientseitige Ressourcen</span><span class="sxs-lookup"><span data-stu-id="55f21-272">Create a production-grade build of the custom client-side assets</span></span>
1. <span data-ttu-id="55f21-273">Kopieren Sie die generierte Webpack-Assets in Ordner "Publish"</span><span class="sxs-lookup"><span data-stu-id="55f21-273">Copy the Webpack-generated assets to the publish folder</span></span>

<span data-ttu-id="55f21-274">Das MSBuild-Ziel wird aufgerufen, bei der Ausführung:</span><span class="sxs-lookup"><span data-stu-id="55f21-274">The MSBuild target is invoked when running:</span></span>

```console
dotnet publish -c Release
```

## <a name="additional-resources"></a><span data-ttu-id="55f21-275">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="55f21-275">Additional resources</span></span>

* [<span data-ttu-id="55f21-276">Angular-Docs</span><span class="sxs-lookup"><span data-stu-id="55f21-276">Angular Docs</span></span>](https://angular.io/docs)
