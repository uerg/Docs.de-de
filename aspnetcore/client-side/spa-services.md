---
title: Verwenden von JavaScriptServices zum Erstellen von einseitigen Anwendungen in ASP.NET Core
author: scottaddie
description: Informationen Sie zu den Vorteilen der Verwendung von JavaScriptServices zum Erstellen einer Single-Page Anwendung (SPA) von ASP.NET Core unterstützt wird.
ms.author: scaddie
ms.custom: H1Hack27Feb2017
ms.date: 08/02/2017
uid: client-side/spa-services
ms.openlocfilehash: 6ac922d82e5c93343cd0e9df312719c6df121dcb
ms.sourcegitcommit: 18339e3cb5a891a3ca36d8146fa83cf91c32e707
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37433999"
---
# <a name="use-javascriptservices-to-create-single-page-applications-in-aspnet-core"></a><span data-ttu-id="b068d-103">Verwenden von JavaScriptServices zum Erstellen von einseitigen Anwendungen in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b068d-103">Use JavaScriptServices to Create Single Page Applications in ASP.NET Core</span></span>

<span data-ttu-id="b068d-104">Durch [Scott Addie](https://github.com/scottaddie) und [Fiyaz Hasan](http://fiyazhasan.me/)</span><span class="sxs-lookup"><span data-stu-id="b068d-104">By [Scott Addie](https://github.com/scottaddie) and [Fiyaz Hasan](http://fiyazhasan.me/)</span></span>

<span data-ttu-id="b068d-105">Eine Single-Page-Anwendung (SPA) ist ein Webanwendungstyp, der aufgrund seiner hohen Servicequalität und Leistungsstärke sehr beliebt ist.</span><span class="sxs-lookup"><span data-stu-id="b068d-105">A Single Page Application (SPA) is a popular type of web application due to its inherent rich user experience.</span></span> <span data-ttu-id="b068d-106">Die Integration clientseitiger SPA-Frameworks oder -Bibliotheken wie [Angular](https://angular.io/) oder [React](https://facebook.github.io/react/) mit serverseitigen Frameworks wie ASP.NET Core schwierig sein kann.</span><span class="sxs-lookup"><span data-stu-id="b068d-106">Integrating client-side SPA frameworks or libraries, such as [Angular](https://angular.io/) or [React](https://facebook.github.io/react/), with server-side frameworks like ASP.NET Core can be difficult.</span></span> <span data-ttu-id="b068d-107">[JavaScriptServices](https://github.com/aspnet/JavaScriptServices) wurde entwickelt, um Inkonsistenzen im Integrationsprozess zu reduzieren.</span><span class="sxs-lookup"><span data-stu-id="b068d-107">[JavaScriptServices](https://github.com/aspnet/JavaScriptServices) was developed to reduce friction in the integration process.</span></span> <span data-ttu-id="b068d-108">Hierdurch wird ein reibungsloser Betrieb verschiedener Client- und Servertechnologiestapel möglich.</span><span class="sxs-lookup"><span data-stu-id="b068d-108">It enables seamless operation between the different client and server technology stacks.</span></span>

<span data-ttu-id="b068d-109">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/spa-services/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b068d-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/spa-services/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<a name="what-is-js-services"></a>

## <a name="what-is-javascriptservices"></a><span data-ttu-id="b068d-110">Was ist JavaScriptServices?</span><span class="sxs-lookup"><span data-stu-id="b068d-110">What is JavaScriptServices?</span></span>

<span data-ttu-id="b068d-111">JavaScriptServices ist eine Sammlung von clientseitigen Technologien für ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b068d-111">JavaScriptServices is a collection of client-side technologies for ASP.NET Core.</span></span> <span data-ttu-id="b068d-112">Das Ziel ist, die ASP.NET Core als Entwickler bevorzugte serverseitige Plattform zum Erstellen von SPAs zu positionieren.</span><span class="sxs-lookup"><span data-stu-id="b068d-112">Its goal is to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span>

<span data-ttu-id="b068d-113">JavaScriptServices besteht aus drei unterschiedlichen NuGet-Pakete:</span><span class="sxs-lookup"><span data-stu-id="b068d-113">JavaScriptServices consists of three distinct NuGet packages:</span></span>
* <span data-ttu-id="b068d-114">[Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)</span><span class="sxs-lookup"><span data-stu-id="b068d-114">[Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)</span></span>
* <span data-ttu-id="b068d-115">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)</span><span class="sxs-lookup"><span data-stu-id="b068d-115">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)</span></span>
* <span data-ttu-id="b068d-116">[Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)</span><span class="sxs-lookup"><span data-stu-id="b068d-116">[Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)</span></span>

<span data-ttu-id="b068d-117">Diese Pakete sind nützlich, wenn Sie:</span><span class="sxs-lookup"><span data-stu-id="b068d-117">These packages are useful if you:</span></span>
* <span data-ttu-id="b068d-118">JavaScript auf dem Server ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="b068d-118">Run JavaScript on the server</span></span>
* <span data-ttu-id="b068d-119">Verwenden Sie eine SPA-Framework oder der Bibliothek</span><span class="sxs-lookup"><span data-stu-id="b068d-119">Use a SPA framework or library</span></span>
* <span data-ttu-id="b068d-120">Erstellen Sie eine clientseitige-Assets mit Webpack</span><span class="sxs-lookup"><span data-stu-id="b068d-120">Build client-side assets with Webpack</span></span>

<span data-ttu-id="b068d-121">Den Schwerpunkt in diesem Artikel befindet sich auf die mithilfe des SpaServices-Pakets.</span><span class="sxs-lookup"><span data-stu-id="b068d-121">Much of the focus in this article is placed on using the SpaServices package.</span></span>

<a name="what-is-spa-services"></a>

## <a name="what-is-spaservices"></a><span data-ttu-id="b068d-122">Was ist SpaServices?</span><span class="sxs-lookup"><span data-stu-id="b068d-122">What is SpaServices?</span></span>

<span data-ttu-id="b068d-123">SpaServices wurde erstellt, um ASP.NET Core als Entwickler bevorzugte serverseitige Plattform zum Erstellen von SPAs zu positionieren.</span><span class="sxs-lookup"><span data-stu-id="b068d-123">SpaServices was created to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span> <span data-ttu-id="b068d-124">SpaServices ist nicht erforderlich, um der SPAs mit ASP.NET Core zu entwickeln, und es nicht in einem bestimmten Clientframework gesperrt.</span><span class="sxs-lookup"><span data-stu-id="b068d-124">SpaServices isn't required to develop SPAs with ASP.NET Core, and it doesn't lock you into a particular client framework.</span></span>

<span data-ttu-id="b068d-125">SpaServices bietet nützliche Infrastruktur wie z.B.:</span><span class="sxs-lookup"><span data-stu-id="b068d-125">SpaServices provides useful infrastructure such as:</span></span>
* [<span data-ttu-id="b068d-126">Serverseitige prerendering</span><span class="sxs-lookup"><span data-stu-id="b068d-126">Server-side prerendering</span></span>](#server-prerendering)
* [<span data-ttu-id="b068d-127">Webpack-Dev-Middleware</span><span class="sxs-lookup"><span data-stu-id="b068d-127">Webpack Dev Middleware</span></span>](#webpack-dev-middleware)
* [<span data-ttu-id="b068d-128">Der Austausch eines "Hot"</span><span class="sxs-lookup"><span data-stu-id="b068d-128">Hot Module Replacement</span></span>](#hot-module-replacement)
* [<span data-ttu-id="b068d-129">Routing-Hilfsprogramme</span><span class="sxs-lookup"><span data-stu-id="b068d-129">Routing helpers</span></span>](#routing-helpers)

<span data-ttu-id="b068d-130">Erweitern diese Infrastrukturkomponenten zusammen, sowohl für den Entwicklungsworkflow als auch für die Common Language Runtime-Umgebung.</span><span class="sxs-lookup"><span data-stu-id="b068d-130">Collectively, these infrastructure components enhance both the development workflow and the runtime experience.</span></span> <span data-ttu-id="b068d-131">Die Komponenten können einzeln übernommen werden.</span><span class="sxs-lookup"><span data-stu-id="b068d-131">The components can be adopted individually.</span></span>

<a name="spa-services-prereqs"></a>

## <a name="prerequisites-for-using-spaservices"></a><span data-ttu-id="b068d-132">Voraussetzungen für die Verwendung von SpaServices</span><span class="sxs-lookup"><span data-stu-id="b068d-132">Prerequisites for using SpaServices</span></span>

<span data-ttu-id="b068d-133">Zum Arbeiten mit SpaServices installieren Sie die folgenden Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="b068d-133">To work with SpaServices, install the following:</span></span>
* <span data-ttu-id="b068d-134">[Node.js](https://nodejs.org/) (Version 6 oder höher) mit Npm</span><span class="sxs-lookup"><span data-stu-id="b068d-134">[Node.js](https://nodejs.org/) (version 6 or later) with npm</span></span>
  * <span data-ttu-id="b068d-135">Um diese Komponenten werden installiert, und finden Sie zu überprüfen, führen Sie Folgendes über die Befehlszeile ein:</span><span class="sxs-lookup"><span data-stu-id="b068d-135">To verify these components are installed and can be found, run the following from the command line:</span></span>

    ```console
    node -v && npm -v
    ```

<span data-ttu-id="b068d-136">Hinweis: Wenn Sie in einer Azure-Website bereitstellen möchten, Sie müssen hier nichts &mdash; Node.js installiert ist und in der Server-Umgebung verfügbar.</span><span class="sxs-lookup"><span data-stu-id="b068d-136">Note: If you're deploying to an Azure web site, you don't need to do anything here &mdash; Node.js is installed and available in the server environments.</span></span>

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]

  * <span data-ttu-id="b068d-137">Wenn Sie Windows mit Visual Studio 2017 verwenden, wird das SDK installiert, durch Auswählen der **plattformübergreifende Entwicklung mit .NET Core** arbeitsauslastung.</span><span class="sxs-lookup"><span data-stu-id="b068d-137">If you're on Windows using Visual Studio 2017, the SDK is installed by selecting the **.NET Core cross-platform development** workload.</span></span>

* <span data-ttu-id="b068d-138">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet-Paket</span><span class="sxs-lookup"><span data-stu-id="b068d-138">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet package</span></span>

<a name="server-prerendering"></a>

## <a name="server-side-prerendering"></a><span data-ttu-id="b068d-139">Serverseitige prerendering</span><span class="sxs-lookup"><span data-stu-id="b068d-139">Server-side prerendering</span></span>

<span data-ttu-id="b068d-140">Eine universelle (auch bekannt als isomorph)-Anwendung ist eine JavaScript-Anwendung ausgeführt, die auf dem Server und der Client sowohl werden kann.</span><span class="sxs-lookup"><span data-stu-id="b068d-140">A universal (also known as isomorphic) application is a JavaScript application capable of running both on the server and the client.</span></span> <span data-ttu-id="b068d-141">Bieten eine universelle Plattform für diese Anwendung Entwicklungsstil, Angular, React und anderen beliebten Frameworks.</span><span class="sxs-lookup"><span data-stu-id="b068d-141">Angular, React, and other popular frameworks provide a universal platform for this application development style.</span></span> <span data-ttu-id="b068d-142">Die Idee ist zunächst Rendern die Framework-Komponenten auf dem Server über Node.js, und klicken Sie dann weitere Ausführung an den Client zu delegieren.</span><span class="sxs-lookup"><span data-stu-id="b068d-142">The idea is to first render the framework components on the server via Node.js, and then delegate further execution to the client.</span></span>

<span data-ttu-id="b068d-143">ASP.NET Core [Taghilfsprogramme](xref:mvc/views/tag-helpers/intro) gebotenen SpaServices vereinfachen die Implementierung von serverseitigen Prerendering durch Aufrufen der JavaScript-Funktionen auf dem Server.</span><span class="sxs-lookup"><span data-stu-id="b068d-143">ASP.NET Core [Tag Helpers](xref:mvc/views/tag-helpers/intro) provided by SpaServices simplify the implementation of server-side prerendering by invoking the JavaScript functions on the server.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="b068d-144">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="b068d-144">Prerequisites</span></span>

<span data-ttu-id="b068d-145">Installieren Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="b068d-145">Install the following:</span></span>
* <span data-ttu-id="b068d-146">[ASPNET-Prerendering](https://www.npmjs.com/package/aspnet-prerendering) Npm-Paket:</span><span class="sxs-lookup"><span data-stu-id="b068d-146">[aspnet-prerendering](https://www.npmjs.com/package/aspnet-prerendering) npm package:</span></span>

    ```console
    npm i -S aspnet-prerendering
    ```

### <a name="configuration"></a><span data-ttu-id="b068d-147">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="b068d-147">Configuration</span></span>

<span data-ttu-id="b068d-148">Die Taghilfsprogramme sind des Projekts über die Namespace-Registrierung erkennbar gemacht *_ViewImports.cshtml* Datei:</span><span class="sxs-lookup"><span data-stu-id="b068d-148">The Tag Helpers are made discoverable via namespace registration in the project's *_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/_ViewImports.cshtml?highlight=3)]

<span data-ttu-id="b068d-149">Diese Taghilfsprogramme abstrahieren die feinheiten der direkten Kommunikation mit Low-Level-APIs durch die Nutzung einer HTML-ähnliche Syntax in der Razor-Ansicht:</span><span class="sxs-lookup"><span data-stu-id="b068d-149">These Tag Helpers abstract away the intricacies of communicating directly with low-level APIs by leveraging an HTML-like syntax inside the Razor view:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=5)]

### <a name="the-asp-prerender-module-tag-helper"></a><span data-ttu-id="b068d-150">Die `asp-prerender-module` Taghilfsprogramm</span><span class="sxs-lookup"><span data-stu-id="b068d-150">The `asp-prerender-module` Tag Helper</span></span>

<span data-ttu-id="b068d-151">Die `asp-prerender-module` Taghilfsprogramm, in dem vorherigen Beispiel, verwendet führt *ClientApp/dist/main-server.js* auf dem Server über Node.js.</span><span class="sxs-lookup"><span data-stu-id="b068d-151">The `asp-prerender-module` Tag Helper, used in the preceding code example, executes *ClientApp/dist/main-server.js* on the server via Node.js.</span></span> <span data-ttu-id="b068d-152">Für die der Verständlichkeit *Main-Datei "Server.js"* Datei ist ein Artefakt der Aufgabe in TypeScript und JavaScript-transpilation ziehen die [Webpack](http://webpack.github.io/) Buildprozess.</span><span class="sxs-lookup"><span data-stu-id="b068d-152">For clarity's sake, *main-server.js* file is an artifact of the TypeScript-to-JavaScript transpilation task in the [Webpack](http://webpack.github.io/) build process.</span></span> <span data-ttu-id="b068d-153">Webpack definiert einen Eintrag-Punkt-Alias für `main-server`; und das Abhängigkeitsdiagramm für diesen Alias Durchlauf beginnt bei der *ClientApp/Boot-server.ts* Datei:</span><span class="sxs-lookup"><span data-stu-id="b068d-153">Webpack defines an entry point alias of `main-server`; and, traversal of the dependency graph for this alias begins at the *ClientApp/boot-server.ts* file:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=53)]

<span data-ttu-id="b068d-154">Im folgenden Beispiel Angular die *ClientApp/Boot-server.ts* Datei nutzt die `createServerRenderer` Funktion und `RenderResult` Typ der `aspnet-prerendering` Npm-Paket so konfigurieren Sie Server-Rendering über Node.js.</span><span class="sxs-lookup"><span data-stu-id="b068d-154">In the following Angular example, the *ClientApp/boot-server.ts* file utilizes the `createServerRenderer` function and `RenderResult` type of the `aspnet-prerendering` npm package to configure server rendering via Node.js.</span></span> <span data-ttu-id="b068d-155">Das HTML-Markup, das bestimmt, für serverseitiges Rendering auf ein Funktionsaufruf auflösen übergeben wird, die von einer stark typisierten JavaScript umgeben sind `Promise` Objekt.</span><span class="sxs-lookup"><span data-stu-id="b068d-155">The HTML markup destined for server-side rendering is passed to a resolve function call, which is wrapped in a strongly-typed JavaScript `Promise` object.</span></span> <span data-ttu-id="b068d-156">Die `Promise` des Objekts "significance" ist, dass sie asynchron die HTML-Markup auf der Seite zur Injektion in das DOM Platzhalterelement bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="b068d-156">The `Promise` object's significance is that it asynchronously supplies the HTML markup to the page for injection in the DOM's placeholder element.</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-34,79-)]

### <a name="the-asp-prerender-data-tag-helper"></a><span data-ttu-id="b068d-157">Die `asp-prerender-data` Taghilfsprogramm</span><span class="sxs-lookup"><span data-stu-id="b068d-157">The `asp-prerender-data` Tag Helper</span></span>

<span data-ttu-id="b068d-158">In Kombination mit der `asp-prerender-module` Taghilfsprogramm, das `asp-prerender-data` Taghilfsprogramm kann verwendet werden, um die Kontextinformationen für das serverseitige JavaScript von der Razor-Ansicht zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="b068d-158">When coupled with the `asp-prerender-module` Tag Helper, the `asp-prerender-data` Tag Helper can be used to pass contextual information from the Razor view to the server-side JavaScript.</span></span> <span data-ttu-id="b068d-159">Das folgende Markup übergibt beispielsweise Benutzerdaten der `main-server` Modul:</span><span class="sxs-lookup"><span data-stu-id="b068d-159">For example, the following markup passes user data to the `main-server` module:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=9-12)]

<span data-ttu-id="b068d-160">Die empfangenen Daten `UserName` Argument wird mit der integrierten JSON-Serialisierungsprogramm serialisiert und befindet sich in der `params.data` Objekt.</span><span class="sxs-lookup"><span data-stu-id="b068d-160">The received `UserName` argument is serialized using the built-in JSON serializer and is stored in the `params.data` object.</span></span> <span data-ttu-id="b068d-161">Im folgenden Beispiel Angular-die Daten werden verwendet, um persönlich begrüßt im Erstellen einer `h1` Element:</span><span class="sxs-lookup"><span data-stu-id="b068d-161">In the following Angular example, the data is used to construct a personalized greeting within an `h1` element:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,38-52,79-)]

<span data-ttu-id="b068d-162">Hinweis: Taghilfsprogramme übergebene Eigenschaftennamen mit dargestellt **PascalCase** Notation.</span><span class="sxs-lookup"><span data-stu-id="b068d-162">Note: Property names passed in Tag Helpers are represented with **PascalCase** notation.</span></span> <span data-ttu-id="b068d-163">Im Gegensatz dazu stehen für JavaScript ist, in denen die gleichen Eigenschaftennamen mit dargestellt **CamelCase**.</span><span class="sxs-lookup"><span data-stu-id="b068d-163">Contrast that to JavaScript, where the same property names are represented with **camelCase**.</span></span> <span data-ttu-id="b068d-164">Die Standardkonfiguration für JSON-Serialisierung ist verantwortlich für diesen Unterschied.</span><span class="sxs-lookup"><span data-stu-id="b068d-164">The default JSON serialization configuration is responsible for this difference.</span></span>

<span data-ttu-id="b068d-165">Zum Erweitern auf das vorherige Codebeispiel Daten können werden vom Server an die Ansicht übergeben von hydrating der `globals` Eigenschaft bereitgestellt, um die `resolve` Funktion:</span><span class="sxs-lookup"><span data-stu-id="b068d-165">To expand upon the preceding code example, data can be passed from the server to the view by hydrating the `globals` property provided to the `resolve` function:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,57-77,79-)]

<span data-ttu-id="b068d-166">Die `postList` Array definiert, die innerhalb der `globals` Objekt angefügt ist im Browsers auf die globale `window` Objekt.</span><span class="sxs-lookup"><span data-stu-id="b068d-166">The `postList` array defined inside the `globals` object is attached to the browser's global `window` object.</span></span> <span data-ttu-id="b068d-167">Diese Variable anheben globalen Bereich entfällt die Duplizierung von Aufwand, vor allem mit Bezug auf die gleichen Daten einmal auf dem Server und erneut auf dem Client werden geladen.</span><span class="sxs-lookup"><span data-stu-id="b068d-167">This variable hoisting to global scope eliminates duplication of effort, particularly as it pertains to loading the same data once on the server and again on the client.</span></span>

![Globale postList Variable Window-Objekt angefügt](spa-services/_static/global_variable.png)

<a name="webpack-dev-middleware"></a>

## <a name="webpack-dev-middleware"></a><span data-ttu-id="b068d-169">Webpack-Dev-Middleware</span><span class="sxs-lookup"><span data-stu-id="b068d-169">Webpack Dev Middleware</span></span>

<span data-ttu-id="b068d-170">[Webpack-Dev-Middleware](https://webpack.github.io/docs/webpack-dev-middleware.html) bietet einen optimierte Entwicklung-Workflow, bei dem Webpack Ressourcen bei Bedarf erstellt.</span><span class="sxs-lookup"><span data-stu-id="b068d-170">[Webpack Dev Middleware](https://webpack.github.io/docs/webpack-dev-middleware.html) introduces a streamlined development workflow whereby Webpack builds resources on demand.</span></span> <span data-ttu-id="b068d-171">Die Middleware automatisch kompiliert und clientseitige Ressourcen dient, wenn eine Seite im Browser geladen wird.</span><span class="sxs-lookup"><span data-stu-id="b068d-171">The middleware automatically compiles and serves client-side resources when a page is reloaded in the browser.</span></span> <span data-ttu-id="b068d-172">Der alternative Ansatz ist Webpack über Npm-Buildskript des Projekts manuell aufrufen, wenn eine Drittanbieter Abhängigkeit oder den benutzerdefinierten Code geändert wird.</span><span class="sxs-lookup"><span data-stu-id="b068d-172">The alternate approach is to manually invoke Webpack via the project's npm build script when a third-party dependency or the custom code changes.</span></span> <span data-ttu-id="b068d-173">Ein Npm-Buildskript die *"Package.JSON"* Datei ist im folgenden Beispiel dargestellt:</span><span class="sxs-lookup"><span data-stu-id="b068d-173">An npm build script in the *package.json* file is shown in the following example:</span></span>

[!code-json[](../client-side/spa-services/sample/SpaServicesSampleApp/package.json?range=5)]

### <a name="prerequisites"></a><span data-ttu-id="b068d-174">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="b068d-174">Prerequisites</span></span>

<span data-ttu-id="b068d-175">Installieren Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="b068d-175">Install the following:</span></span>
* <span data-ttu-id="b068d-176">[ASPNET-Webpack](https://www.npmjs.com/package/aspnet-webpack) Npm-Paket:</span><span class="sxs-lookup"><span data-stu-id="b068d-176">[aspnet-webpack](https://www.npmjs.com/package/aspnet-webpack) npm package:</span></span>

    ```console
    npm i -D aspnet-webpack
    ```

### <a name="configuration"></a><span data-ttu-id="b068d-177">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="b068d-177">Configuration</span></span>

<span data-ttu-id="b068d-178">Webpack-Dev-Middleware wird registriert, in die HTTP-Anforderungspipeline über den folgenden Code in die *"Startup.cs"* dateimodell `Configure` Methode:</span><span class="sxs-lookup"><span data-stu-id="b068d-178">Webpack Dev Middleware is registered into the HTTP request pipeline via the following code in the *Startup.cs* file's `Configure` method:</span></span>

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=webpack-middleware-registration&highlight=4)]

<span data-ttu-id="b068d-179">Die `UseWebpackDevMiddleware` Erweiterungsmethode aufgerufen werden muss, bevor [registrieren, statische Datei hosting](xref:fundamentals/static-files) über die `UseStaticFiles` -Erweiterungsmethode.</span><span class="sxs-lookup"><span data-stu-id="b068d-179">The `UseWebpackDevMiddleware` extension method must be called before [registering static file hosting](xref:fundamentals/static-files) via the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="b068d-180">Registrieren Sie die Middleware aus Sicherheitsgründen nur, wenn die app im Entwicklungsmodus ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="b068d-180">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="b068d-181">Die *webpack.config.js* dateimodell `output.publicPath` Eigenschaft gibt die Middleware, sehen Sie sich die `dist` Ordner auf Änderungen:</span><span class="sxs-lookup"><span data-stu-id="b068d-181">The *webpack.config.js* file's `output.publicPath` property tells the middleware to watch the `dist` folder for changes:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,13-16)]

<a name="hot-module-replacement"></a>

## <a name="hot-module-replacement"></a><span data-ttu-id="b068d-182">Der Austausch eines "Hot"</span><span class="sxs-lookup"><span data-stu-id="b068d-182">Hot Module Replacement</span></span>

<span data-ttu-id="b068d-183">Stellen Sie sich den Webpack ["Hot" Austausch eines Controllermoduls](https://webpack.js.org/concepts/hot-module-replacement/) (HMR)-Funktion als Weiterentwicklung von [Webpack-Dev-Middleware](#webpack-dev-middleware).</span><span class="sxs-lookup"><span data-stu-id="b068d-183">Think of Webpack's [Hot Module Replacement](https://webpack.js.org/concepts/hot-module-replacement/) (HMR) feature as an evolution of [Webpack Dev Middleware](#webpack-dev-middleware).</span></span> <span data-ttu-id="b068d-184">HMR führt die gleichen Vorteile, aber es weiter den Entwicklungsworkflow optimiert, indem die automatische Aktualisierung der Seiteninhalt nach dem Kompilieren der Änderungen.</span><span class="sxs-lookup"><span data-stu-id="b068d-184">HMR introduces all the same benefits, but it further streamlines the development workflow by automatically updating page content after compiling the changes.</span></span> <span data-ttu-id="b068d-185">Verwechseln Sie dies nicht mit einer Aktualisierung des Browsers, die den aktuellen in-Memory-Status und die Debugsitzung der SPA beeinträchtigen würde.</span><span class="sxs-lookup"><span data-stu-id="b068d-185">Don't confuse this with a refresh of the browser, which would interfere with the current in-memory state and debugging session of the SPA.</span></span> <span data-ttu-id="b068d-186">Es ist ein live-Link zwischen den Webpack-Dev-Middleware-Dienst und dem Browser, was bedeutet, dass Änderungen an den Browser mithilfe von Push übertragen werden.</span><span class="sxs-lookup"><span data-stu-id="b068d-186">There's a live link between the Webpack Dev Middleware service and the browser, which means changes are pushed to the browser.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="b068d-187">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="b068d-187">Prerequisites</span></span>

<span data-ttu-id="b068d-188">Installieren Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="b068d-188">Install the following:</span></span>
* <span data-ttu-id="b068d-189">[Webpack-hot-Middleware](https://www.npmjs.com/package/webpack-hot-middleware) Npm-Paket:</span><span class="sxs-lookup"><span data-stu-id="b068d-189">[webpack-hot-middleware](https://www.npmjs.com/package/webpack-hot-middleware) npm package:</span></span>

    ```console
    npm i -D webpack-hot-middleware
    ```

### <a name="configuration"></a><span data-ttu-id="b068d-190">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="b068d-190">Configuration</span></span>

<span data-ttu-id="b068d-191">Die Komponente HMR muss registriert werden, in MVC HTTP-Anforderungspipeline in die `Configure` Methode:</span><span class="sxs-lookup"><span data-stu-id="b068d-191">The HMR component must be registered into MVC's HTTP request pipeline in the `Configure` method:</span></span>

```csharp
app.UseWebpackDevMiddleware(new WebpackDevMiddlewareOptions {
    HotModuleReplacement = true
});
```

<span data-ttu-id="b068d-192">Wie wurde mit "true" [Webpack-Dev-Middleware](#webpack-dev-middleware), `UseWebpackDevMiddleware` Erweiterungsmethode aufgerufen werden muss, bevor die `UseStaticFiles` -Erweiterungsmethode.</span><span class="sxs-lookup"><span data-stu-id="b068d-192">As was true with [Webpack Dev Middleware](#webpack-dev-middleware), the `UseWebpackDevMiddleware` extension method must be called before the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="b068d-193">Registrieren Sie die Middleware aus Sicherheitsgründen nur, wenn die app im Entwicklungsmodus ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="b068d-193">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="b068d-194">Die *webpack.config.js* Datei definieren, muss ein `plugins` array, auch wenn es leer gelassen wird:</span><span class="sxs-lookup"><span data-stu-id="b068d-194">The *webpack.config.js* file must define a `plugins` array, even if it's left empty:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,25)]

<span data-ttu-id="b068d-195">Nach dem Laden der app im Browser, bietet die Registerkarte für die Entwicklertools-Konsole zur Bestätigung der HMR Aktivierung:</span><span class="sxs-lookup"><span data-stu-id="b068d-195">After loading the app in the browser, the developer tools' Console tab provides confirmation of HMR activation:</span></span>

!["Hot" verbundene Austausch eines Controllermoduls-Nachricht](spa-services/_static/hmr_connected.png)

<a name="routing-helpers"></a>

## <a name="routing-helpers"></a><span data-ttu-id="b068d-197">Routing-Hilfsprogramme</span><span class="sxs-lookup"><span data-stu-id="b068d-197">Routing helpers</span></span>

<span data-ttu-id="b068d-198">In den meisten ASP.NET Core-basierte SPAs werden sollten Sie die clientseitige routing neben dem routing von serverseitigen.</span><span class="sxs-lookup"><span data-stu-id="b068d-198">In most ASP.NET Core-based SPAs, you'll want client-side routing in addition to server-side routing.</span></span> <span data-ttu-id="b068d-199">Die SPA und MVC-routing-Systeme können ohne Störung unabhängig voneinander arbeiten.</span><span class="sxs-lookup"><span data-stu-id="b068d-199">The SPA and MVC routing systems can work independently without interference.</span></span> <span data-ttu-id="b068d-200">Vorhanden ist, jedoch einen Edge Groß-/Kleinschreibung machen Herausforderungen: Identifizieren von 404-HTTP-Antworten.</span><span class="sxs-lookup"><span data-stu-id="b068d-200">There's, however, one edge case posing challenges: identifying 404 HTTP responses.</span></span>

<span data-ttu-id="b068d-201">Betrachten Sie das Szenario, in dem eine Route ohne Erweiterung `/some/page` verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="b068d-201">Consider the scenario in which an extensionless route of `/some/page` is used.</span></span> <span data-ttu-id="b068d-202">Angenommen Sie, die Anforderung nicht-Musterübereinstimmung eine serverseitige Weg, Verwendungsmuster stimmt jedoch mit einer Client-Side-Route.</span><span class="sxs-lookup"><span data-stu-id="b068d-202">Assume the request doesn't pattern-match a server-side route, but its pattern does match a client-side route.</span></span> <span data-ttu-id="b068d-203">Betrachten Sie nun eine eingehende Anforderung für `/images/user-512.png`, die in der Regel davon ausgeht, eine Bilddatei auf dem Server.</span><span class="sxs-lookup"><span data-stu-id="b068d-203">Now consider an incoming request for `/images/user-512.png`, which generally expects to find an image file on the server.</span></span> <span data-ttu-id="b068d-204">Wenn dieser Pfad für die angeforderte Ressource keine serverseitigen Route oder eine statische Datei entspricht, ist es unwahrscheinlich, die die clientseitige Anwendung verarbeitet werden sollen, in der Regel eine HTTP-Statuscode 404 zurückgegeben werden soll.</span><span class="sxs-lookup"><span data-stu-id="b068d-204">If that requested resource path doesn't match any server-side route or static file, it's unlikely that the client-side application would handle it — you generally want to return a 404 HTTP status code.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="b068d-205">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="b068d-205">Prerequisites</span></span>

<span data-ttu-id="b068d-206">Installieren Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="b068d-206">Install the following:</span></span>
* <span data-ttu-id="b068d-207">Das routing Npm-Paket für die clientseitige.</span><span class="sxs-lookup"><span data-stu-id="b068d-207">The client-side routing npm package.</span></span> <span data-ttu-id="b068d-208">Verwenden von Angular als Beispiel:</span><span class="sxs-lookup"><span data-stu-id="b068d-208">Using Angular as an example:</span></span>

    ```console
    npm i -S @angular/router
    ```

### <a name="configuration"></a><span data-ttu-id="b068d-209">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="b068d-209">Configuration</span></span>

<span data-ttu-id="b068d-210">Eine Erweiterungsmethode namens `MapSpaFallbackRoute` werden in der `Configure` Methode:</span><span class="sxs-lookup"><span data-stu-id="b068d-210">An extension method named `MapSpaFallbackRoute` is used in the `Configure` method:</span></span>

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=mvc-routing-table&highlight=7-9)]

<span data-ttu-id="b068d-211">Tipp: Routen werden in der Reihenfolge ausgewertet, in denen sie konfiguriert sind.</span><span class="sxs-lookup"><span data-stu-id="b068d-211">Tip: Routes are evaluated in the order in which they're configured.</span></span> <span data-ttu-id="b068d-212">Daher die `default` Route im obigen Codebeispiel wird zuerst für den Musterabgleich verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="b068d-212">Consequently, the `default` route in the preceding code example is used first for pattern matching.</span></span>

<a name="new-project-creation"></a>

## <a name="creating-a-new-project"></a><span data-ttu-id="b068d-213">Erstellen eines neuen Projekts</span><span class="sxs-lookup"><span data-stu-id="b068d-213">Creating a new project</span></span>

<span data-ttu-id="b068d-214">JavaScriptServices bietet vorkonfigurierte Anwendungsvorlagen an.</span><span class="sxs-lookup"><span data-stu-id="b068d-214">JavaScriptServices provides pre-configured application templates.</span></span> <span data-ttu-id="b068d-215">SpaServices wird in dieser Vorlagen in Verbindung mit verschiedenen Frameworks und Bibliotheken wie Angular und React mit Redux verwendet.</span><span class="sxs-lookup"><span data-stu-id="b068d-215">SpaServices is used in these templates, in conjunction with different frameworks and libraries such as Angular, React, and Redux.</span></span>

<span data-ttu-id="b068d-216">Diese Vorlagen können über die .NET Core-CLI installiert werden, mithilfe des folgenden Befehls:</span><span class="sxs-lookup"><span data-stu-id="b068d-216">These templates can be installed via the .NET Core CLI by running the following command:</span></span>

```console
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

<span data-ttu-id="b068d-217">Eine Liste der verfügbaren SPA-Vorlagen wird angezeigt:</span><span class="sxs-lookup"><span data-stu-id="b068d-217">A list of available SPA templates is displayed:</span></span>

| <span data-ttu-id="b068d-218">Vorlagen</span><span class="sxs-lookup"><span data-stu-id="b068d-218">Templates</span></span>                                 | <span data-ttu-id="b068d-219">Kurzname</span><span class="sxs-lookup"><span data-stu-id="b068d-219">Short Name</span></span> | <span data-ttu-id="b068d-220">Sprache</span><span class="sxs-lookup"><span data-stu-id="b068d-220">Language</span></span> | <span data-ttu-id="b068d-221">Tags</span><span class="sxs-lookup"><span data-stu-id="b068d-221">Tags</span></span>        |
|:------------------------------------------|:-----------|:---------|:------------|
| <span data-ttu-id="b068d-222">MVC, ASP.NET Core mit Angular</span><span class="sxs-lookup"><span data-stu-id="b068d-222">MVC ASP.NET Core with Angular</span></span>             | <span data-ttu-id="b068d-223">angular</span><span class="sxs-lookup"><span data-stu-id="b068d-223">angular</span></span>    | <span data-ttu-id="b068d-224">[C#]</span><span class="sxs-lookup"><span data-stu-id="b068d-224">[C#]</span></span>     | <span data-ttu-id="b068d-225">MVC/Web/SPA</span><span class="sxs-lookup"><span data-stu-id="b068d-225">Web/MVC/SPA</span></span> |
| <span data-ttu-id="b068d-226">MVC, ASP.NET Core mit React.js</span><span class="sxs-lookup"><span data-stu-id="b068d-226">MVC ASP.NET Core with React.js</span></span>            | <span data-ttu-id="b068d-227">react</span><span class="sxs-lookup"><span data-stu-id="b068d-227">react</span></span>      | <span data-ttu-id="b068d-228">[C#]</span><span class="sxs-lookup"><span data-stu-id="b068d-228">[C#]</span></span>     | <span data-ttu-id="b068d-229">MVC/Web/SPA</span><span class="sxs-lookup"><span data-stu-id="b068d-229">Web/MVC/SPA</span></span> |
| <span data-ttu-id="b068d-230">MVC, ASP.NET Core mit React.js und Redux</span><span class="sxs-lookup"><span data-stu-id="b068d-230">MVC ASP.NET Core with React.js and Redux</span></span>  | <span data-ttu-id="b068d-231">reactredux</span><span class="sxs-lookup"><span data-stu-id="b068d-231">reactredux</span></span> | <span data-ttu-id="b068d-232">[C#]</span><span class="sxs-lookup"><span data-stu-id="b068d-232">[C#]</span></span>     | <span data-ttu-id="b068d-233">MVC/Web/SPA</span><span class="sxs-lookup"><span data-stu-id="b068d-233">Web/MVC/SPA</span></span> |

<span data-ttu-id="b068d-234">Zum Erstellen eines neuen Projekts mithilfe einer der SPA-Vorlagen enthalten die **Kurznamen** der Vorlage in der [Dotnet neue](/dotnet/core/tools/dotnet-new) Befehl.</span><span class="sxs-lookup"><span data-stu-id="b068d-234">To create a new project using one of the SPA templates, include the **Short Name** of the template in the [dotnet new](/dotnet/core/tools/dotnet-new) command.</span></span> <span data-ttu-id="b068d-235">Der folgende Befehl erstellt eine Angular-Anwendung mit ASP.NET Core MVC, die für die Serverseite konfiguriert:</span><span class="sxs-lookup"><span data-stu-id="b068d-235">The following command creates an Angular application with ASP.NET Core MVC configured for the server side:</span></span>

```console
dotnet new angular
```

<a name="runtime-config-mode"></a>

### <a name="set-the-runtime-configuration-mode"></a><span data-ttu-id="b068d-236">Legen Sie den Modus der Common Language Runtime-Konfiguration</span><span class="sxs-lookup"><span data-stu-id="b068d-236">Set the runtime configuration mode</span></span>

<span data-ttu-id="b068d-237">Zwei Modi der primären Common Language Runtime-Konfiguration vorhanden sein:</span><span class="sxs-lookup"><span data-stu-id="b068d-237">Two primary runtime configuration modes exist:</span></span>
* <span data-ttu-id="b068d-238">**Entwicklung**:</span><span class="sxs-lookup"><span data-stu-id="b068d-238">**Development**:</span></span>
    * <span data-ttu-id="b068d-239">Enthält quellzuordnungen zur Erleichterung des Debuggings.</span><span class="sxs-lookup"><span data-stu-id="b068d-239">Includes source maps to ease debugging.</span></span>
    * <span data-ttu-id="b068d-240">Der clientseitige Code für die Leistung nicht optimiert werden.</span><span class="sxs-lookup"><span data-stu-id="b068d-240">Doesn't optimize the client-side code for performance.</span></span>
* <span data-ttu-id="b068d-241">**Produktion**:</span><span class="sxs-lookup"><span data-stu-id="b068d-241">**Production**:</span></span>
    * <span data-ttu-id="b068d-242">Schließt quellzuordnungen an.</span><span class="sxs-lookup"><span data-stu-id="b068d-242">Excludes source maps.</span></span>
    * <span data-ttu-id="b068d-243">Optimiert den clientseitigen Code über Bündelung und Minimierung.</span><span class="sxs-lookup"><span data-stu-id="b068d-243">Optimizes the client-side code via bundling & minification.</span></span>

<span data-ttu-id="b068d-244">ASP.NET Core verwendet eine Umgebungsvariable namens `ASPNETCORE_ENVIRONMENT` zum Speichern von des Konfigurationsmodus zu wechseln.</span><span class="sxs-lookup"><span data-stu-id="b068d-244">ASP.NET Core uses an environment variable named `ASPNETCORE_ENVIRONMENT` to store the configuration mode.</span></span> <span data-ttu-id="b068d-245">Finden Sie unter **[legen Sie die Umgebung](xref:fundamentals/environments#set-the-environment)** für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="b068d-245">See **[Set the environment](xref:fundamentals/environments#set-the-environment)** for more information.</span></span>

### <a name="running-with-net-core-cli"></a><span data-ttu-id="b068d-246">Mit .NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="b068d-246">Running with .NET Core CLI</span></span>

<span data-ttu-id="b068d-247">Stellen Sie die erforderlichen NuGet und Npm-Pakete mithilfe des folgenden Befehls auf das Stammverzeichnis des Projekts wieder her:</span><span class="sxs-lookup"><span data-stu-id="b068d-247">Restore the required NuGet and npm packages by running the following command at the project root:</span></span>

```console
dotnet restore && npm i
```

<span data-ttu-id="b068d-248">Erstellen und Ausführen der Anwendungs:</span><span class="sxs-lookup"><span data-stu-id="b068d-248">Build and run the application:</span></span>

```console
dotnet run
```

<span data-ttu-id="b068d-249">Starten der Anwendung auf "localhost", gemäß der [Common Language Runtime-Konfigurationsmodus](#runtime-config-mode).</span><span class="sxs-lookup"><span data-stu-id="b068d-249">The application starts on localhost according to the [runtime configuration mode](#runtime-config-mode).</span></span> <span data-ttu-id="b068d-250">Navigieren zu `http://localhost:5000` im Browser zeigt die Landing Page.</span><span class="sxs-lookup"><span data-stu-id="b068d-250">Navigating to `http://localhost:5000` in the browser displays the landing page.</span></span>

### <a name="running-with-visual-studio-2017"></a><span data-ttu-id="b068d-251">Mit Visual Studio 2017 ausgeführt wird</span><span class="sxs-lookup"><span data-stu-id="b068d-251">Running with Visual Studio 2017</span></span>

<span data-ttu-id="b068d-252">Öffnen der *csproj* von generierten Datei das [Dotnet neue](/dotnet/core/tools/dotnet-new) Befehl.</span><span class="sxs-lookup"><span data-stu-id="b068d-252">Open the *.csproj* file generated by the [dotnet new](/dotnet/core/tools/dotnet-new) command.</span></span> <span data-ttu-id="b068d-253">Die erforderlichen NuGet und Npm-Pakete werden automatisch auf dem geöffneten Projekt wiederhergestellt.</span><span class="sxs-lookup"><span data-stu-id="b068d-253">The required NuGet and npm packages are restored automatically upon project open.</span></span> <span data-ttu-id="b068d-254">Dieser Wiederherstellungsprozess kann einige Minuten dauern, und die Anwendung ist bereit, die ausgeführt werden, wenn der Vorgang abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="b068d-254">This restoration process may take up to a few minutes, and the application is ready to run when it completes.</span></span> <span data-ttu-id="b068d-255">Klicken Sie auf die grüne Schaltfläche "ausführen", oder drücken Sie `Ctrl + F5`, und der Browser zur Startseite der Anwendung geöffnet wird.</span><span class="sxs-lookup"><span data-stu-id="b068d-255">Click the green run button or press `Ctrl + F5`, and the browser opens to the application's landing page.</span></span> <span data-ttu-id="b068d-256">Die Anwendung ausgeführt wird, auf "localhost", gemäß der [Common Language Runtime-Konfigurationsmodus](#runtime-config-mode).</span><span class="sxs-lookup"><span data-stu-id="b068d-256">The application runs on localhost according to the [runtime configuration mode](#runtime-config-mode).</span></span> 

<a name="app-testing"></a>

## <a name="testing-the-app"></a><span data-ttu-id="b068d-257">Testen der app</span><span class="sxs-lookup"><span data-stu-id="b068d-257">Testing the app</span></span>

<span data-ttu-id="b068d-258">SpaServices Vorlagen sind so vorkonfiguriert, dass die clientseitige Tests mithilfe von [Karma](https://karma-runner.github.io/1.0/index.html) und [Jasmine](https://jasmine.github.io/).</span><span class="sxs-lookup"><span data-stu-id="b068d-258">SpaServices templates are pre-configured to run client-side tests using [Karma](https://karma-runner.github.io/1.0/index.html) and [Jasmine](https://jasmine.github.io/).</span></span> <span data-ttu-id="b068d-259">Jasmine ist eine beliebte Komponententest-Framework für JavaScript, während Karma einen Test Runner für diese Tests ist.</span><span class="sxs-lookup"><span data-stu-id="b068d-259">Jasmine is a popular unit testing framework for JavaScript, whereas Karma is a test runner for those tests.</span></span> <span data-ttu-id="b068d-260">Karma ist so konfiguriert, dass die Arbeit mit der [Webpack-Dev-Middleware](#webpack-dev-middleware) so, dass der Entwickler ist nicht erforderlich sind, beenden, und führen Sie den Test, jedes Mal, wenn Änderungen vorgenommen werden.</span><span class="sxs-lookup"><span data-stu-id="b068d-260">Karma is configured to work with the [Webpack Dev Middleware](#webpack-dev-middleware) such that the developer isn't required to stop and run the test every time changes are made.</span></span> <span data-ttu-id="b068d-261">Ob sie den Code, der den Testfall oder den Testfall selbst ausgeführt wird, wird der Test automatisch ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="b068d-261">Whether it's the code running against the test case or the test case itself, the test runs automatically.</span></span>

<span data-ttu-id="b068d-262">Verwenden Sie als Beispiel die Angular-Anwendung ein, zwei Jasmine Testfälle bereits stehen für die `CounterComponent` in die *counter.component.spec.ts* Datei:</span><span class="sxs-lookup"><span data-stu-id="b068d-262">Using the Angular application as an example, two Jasmine test cases are already provided for the `CounterComponent` in the *counter.component.spec.ts* file:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/app/components/counter/counter.component.spec.ts?range=15-28)]

<span data-ttu-id="b068d-263">Öffnen Sie die Eingabeaufforderung in das *ClientApp* Verzeichnis.</span><span class="sxs-lookup"><span data-stu-id="b068d-263">Open the command prompt in the *ClientApp* directory.</span></span> <span data-ttu-id="b068d-264">Führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="b068d-264">Run the following command:</span></span>

```console
npm test
```

<span data-ttu-id="b068d-265">Das Skript startet die Karma-Testprogramm, die in definierten Einstellungen liest die *karma.conf.js* Datei.</span><span class="sxs-lookup"><span data-stu-id="b068d-265">The script launches the Karma test runner, which reads the settings defined in the *karma.conf.js* file.</span></span> <span data-ttu-id="b068d-266">Neben anderen Einstellungen die *karma.conf.js* identifiziert die Testdateien über auszuführende seine `files` Array:</span><span class="sxs-lookup"><span data-stu-id="b068d-266">Among other settings, the *karma.conf.js* identifies the test files to be executed via its `files` array:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/test/karma.conf.js?range=4-5,8-11)]

<a name="app-publishing"></a>

## <a name="publishing-the-application"></a><span data-ttu-id="b068d-267">Veröffentlichen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="b068d-267">Publishing the application</span></span>

<span data-ttu-id="b068d-268">Kombinieren die generierte clientseitige Ressourcen und die veröffentlichte Elemente, die ASP.NET Core in einem Ready-to-deploy-Paket kann aufwendig sein.</span><span class="sxs-lookup"><span data-stu-id="b068d-268">Combining the generated client-side assets and the published ASP.NET Core artifacts into a ready-to-deploy package can be cumbersome.</span></span> <span data-ttu-id="b068d-269">Glücklicherweise orchestriert SpaServices dieses Prozesses für die gesamte Veröffentlichung mit einem benutzerdefinierten MSBuild-Ziel mit dem Namen `RunWebpack`:</span><span class="sxs-lookup"><span data-stu-id="b068d-269">Thankfully, SpaServices orchestrates that entire publication process with a custom MSBuild target named `RunWebpack`:</span></span>

[!code-xml[](../client-side/spa-services/sample/SpaServicesSampleApp/SpaServicesSampleApp.csproj?range=31-45)]

<span data-ttu-id="b068d-270">Das MSBuild-Ziel hat die folgenden Verantwortungen:</span><span class="sxs-lookup"><span data-stu-id="b068d-270">The MSBuild target has the following responsibilities:</span></span>
1. <span data-ttu-id="b068d-271">Die Npm-Pakete wiederherstellen</span><span class="sxs-lookup"><span data-stu-id="b068d-271">Restore the npm packages</span></span>
1. <span data-ttu-id="b068d-272">Erstellen Sie einen Build Produktionsniveau von Drittanbietern, clientseitige Ressourcen</span><span class="sxs-lookup"><span data-stu-id="b068d-272">Create a production-grade build of the third-party, client-side assets</span></span>
1. <span data-ttu-id="b068d-273">Erstellen Sie einen Build Produktionsniveau benutzerdefinierte clientseitige Ressourcen</span><span class="sxs-lookup"><span data-stu-id="b068d-273">Create a production-grade build of the custom client-side assets</span></span>
1. <span data-ttu-id="b068d-274">Kopieren Sie die generierte Webpack-Assets in Ordner "Publish"</span><span class="sxs-lookup"><span data-stu-id="b068d-274">Copy the Webpack-generated assets to the publish folder</span></span>

<span data-ttu-id="b068d-275">Das MSBuild-Ziel wird aufgerufen, bei der Ausführung:</span><span class="sxs-lookup"><span data-stu-id="b068d-275">The MSBuild target is invoked when running:</span></span>

```console
dotnet publish -c Release
```

## <a name="additional-resources"></a><span data-ttu-id="b068d-276">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="b068d-276">Additional resources</span></span>

* [<span data-ttu-id="b068d-277">Angular-Docs</span><span class="sxs-lookup"><span data-stu-id="b068d-277">Angular Docs</span></span>](https://angular.io/docs)
