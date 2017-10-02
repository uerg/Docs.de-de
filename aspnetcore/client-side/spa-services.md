---
title: "Verwenden JavaScriptServices für einseitige Anwendungen erstellen"
author: scottaddie
description: Weitere Informationen Sie zu den Vorteilen der Verwendung von JavaScriptServices zum Erstellen von einer einzelnen Seite Anwendung (SPA) durch ASP.NET Core gesichert wird.
keywords: ASP.NET Core Angular, SPA, JavaScriptServices, SpaServices
ms.author: scaddie
manager: wpickett
ms.date: 08/02/2017
ms.topic: article
ms.assetid: 4b30576b-2718-4c39-9253-a59966747893
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/spa-services
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a93dae3edec73f1b5254aa60662834ca83de62fd
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2017
---
# <a name="using-javascriptservices-for-creating-single-page-applications-with-aspnet-core"></a><span data-ttu-id="2a656-104">Verwenden JavaScriptServices für einseitige Anwendungen mit ASP.NET Core erstellen</span><span class="sxs-lookup"><span data-stu-id="2a656-104">Using JavaScriptServices for Creating Single Page Applications with ASP.NET Core</span></span>

<span data-ttu-id="2a656-105">Durch [Scott Addie](https://github.com/scottaddie) und [Fiyaz Hasan](http://fiyazhasan.me/)</span><span class="sxs-lookup"><span data-stu-id="2a656-105">By [Scott Addie](https://github.com/scottaddie) and [Fiyaz Hasan](http://fiyazhasan.me/)</span></span>

<span data-ttu-id="2a656-106">Eine einzelne Seite Anwendung (SPA) ist eine beliebte Web Anwendungstyp aufgrund der inhärenten leistungsstarke, optimierte Benutzeroberfläche.</span><span class="sxs-lookup"><span data-stu-id="2a656-106">A Single Page Application (SPA) is a popular type of web application due to its inherent rich user experience.</span></span> <span data-ttu-id="2a656-107">Integrieren von clientseitigen SPA-Frameworks oder-Bibliotheken, z. B. [Angular](https://angular.io/) oder [reagieren](https://facebook.github.io/react/), mit serverseitigen Frameworks wie ASP.NET Core schwierig sein kann.</span><span class="sxs-lookup"><span data-stu-id="2a656-107">Integrating client-side SPA frameworks or libraries, such as [Angular](https://angular.io/) or [React](https://facebook.github.io/react/), with server-side frameworks like ASP.NET Core can be difficult.</span></span> <span data-ttu-id="2a656-108">[JavaScriptServices](https://github.com/aspnet/JavaScriptServices) wurde entwickelt, um die Unstimmigkeiten in den Integrationsprozess zu reduzieren.</span><span class="sxs-lookup"><span data-stu-id="2a656-108">[JavaScriptServices](https://github.com/aspnet/JavaScriptServices) was developed to reduce friction in the integration process.</span></span> <span data-ttu-id="2a656-109">Dadurch werden nahtlos zwischen den Client- und Server-Technologie Stapel.</span><span class="sxs-lookup"><span data-stu-id="2a656-109">It enables seamless operation between the different client and server technology stacks.</span></span>

<span data-ttu-id="2a656-110">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/spa-services/sample) ([zum Herunterladen von](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2a656-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/spa-services/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<a name="what-is-js-services"></a>

## <a name="what-is-javascriptservices"></a><span data-ttu-id="2a656-111">Was ist JavaScriptServices?</span><span class="sxs-lookup"><span data-stu-id="2a656-111">What is JavaScriptServices?</span></span>

<span data-ttu-id="2a656-112">JavaScriptServices ist eine Auflistung von clientseitigen Technologien zum ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2a656-112">JavaScriptServices is a collection of client-side technologies for ASP.NET Core.</span></span> <span data-ttu-id="2a656-113">Das Ziel besteht darin zu ASP.NET Core als Entwicklers bevorzugte serverseitige Plattform zum Erstellen von SPAs zu positionieren.</span><span class="sxs-lookup"><span data-stu-id="2a656-113">Its goal is to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span>

<span data-ttu-id="2a656-114">JavaScriptServices besteht aus drei unterschiedlichen NuGet-Pakete:</span><span class="sxs-lookup"><span data-stu-id="2a656-114">JavaScriptServices consists of three distinct NuGet packages:</span></span>
* <span data-ttu-id="2a656-115">[Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)</span><span class="sxs-lookup"><span data-stu-id="2a656-115">[Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)</span></span>
* <span data-ttu-id="2a656-116">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)</span><span class="sxs-lookup"><span data-stu-id="2a656-116">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)</span></span>
* <span data-ttu-id="2a656-117">[Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)</span><span class="sxs-lookup"><span data-stu-id="2a656-117">[Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)</span></span>

<span data-ttu-id="2a656-118">Diese Pakete sind nützlich, wenn Sie:</span><span class="sxs-lookup"><span data-stu-id="2a656-118">These packages are useful if you:</span></span>
* <span data-ttu-id="2a656-119">JavaScript wird auf dem Server ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="2a656-119">Run JavaScript on the server</span></span>
* <span data-ttu-id="2a656-120">Verwenden Sie einen SPA-Framework oder Bibliothek</span><span class="sxs-lookup"><span data-stu-id="2a656-120">Use a SPA framework or library</span></span>
* <span data-ttu-id="2a656-121">Erstellen Sie die clientseitige Medienobjekte mit Webpaketdatei</span><span class="sxs-lookup"><span data-stu-id="2a656-121">Build client-side assets with Webpack</span></span>

<span data-ttu-id="2a656-122">Ein Großteil der Fokus in diesem Artikel wird auf die mithilfe des Pakets SpaServices platziert.</span><span class="sxs-lookup"><span data-stu-id="2a656-122">Much of the focus in this article is placed on using the SpaServices package.</span></span>

<a name="what-is-spa-services"></a>

## <a name="what-is-spaservices"></a><span data-ttu-id="2a656-123">Was ist SpaServices?</span><span class="sxs-lookup"><span data-stu-id="2a656-123">What is SpaServices?</span></span>

<span data-ttu-id="2a656-124">SpaServices wurde erstellt, um ASP.NET Core als Entwicklers bevorzugte serverseitige Plattform zum Erstellen von SPAs zu positionieren.</span><span class="sxs-lookup"><span data-stu-id="2a656-124">SpaServices was created to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span> <span data-ttu-id="2a656-125">SpaServices ist nicht erforderlich, um SPAs mit ASP.NET Core entwickeln, und Sie in einem bestimmten Client-Framework ist nicht gesperrt.</span><span class="sxs-lookup"><span data-stu-id="2a656-125">SpaServices is not required to develop SPAs with ASP.NET Core, and it doesn't lock you into a particular client framework.</span></span>

<span data-ttu-id="2a656-126">Bietet eine SpaServices nützlich Infrastruktur wie z. B.:</span><span class="sxs-lookup"><span data-stu-id="2a656-126">SpaServices provides useful infrastructure such as:</span></span>
* [<span data-ttu-id="2a656-127">Serverseitige prerendering</span><span class="sxs-lookup"><span data-stu-id="2a656-127">Server-side prerendering</span></span>](#server-prerendering)
* [<span data-ttu-id="2a656-128">Webpaketdatei Dev Middleware</span><span class="sxs-lookup"><span data-stu-id="2a656-128">Webpack Dev Middleware</span></span>](#webpack-dev-middleware)
* [<span data-ttu-id="2a656-129">Ersetzen eines Moduls im laufenden Systembetrieb</span><span class="sxs-lookup"><span data-stu-id="2a656-129">Hot Module Replacement</span></span>](#hot-module-replacement)
* [<span data-ttu-id="2a656-130">Routing-Hilfsprogramme</span><span class="sxs-lookup"><span data-stu-id="2a656-130">Routing helpers</span></span>](#routing-helpers)

<span data-ttu-id="2a656-131">Erweitern Sie diese Infrastrukturkomponenten zusammen des entwicklungsworkflows und der Common Language Runtime-Umgebung.</span><span class="sxs-lookup"><span data-stu-id="2a656-131">Collectively, these infrastructure components enhance both the development workflow and the runtime experience.</span></span> <span data-ttu-id="2a656-132">Die Komponenten können einzeln übernommen werden.</span><span class="sxs-lookup"><span data-stu-id="2a656-132">The components can be adopted individually.</span></span>

<a name="spa-services-prereqs"></a>

## <a name="prerequisites-for-using-spaservices"></a><span data-ttu-id="2a656-133">Voraussetzungen für die Verwendung von SpaServices</span><span class="sxs-lookup"><span data-stu-id="2a656-133">Prerequisites for using SpaServices</span></span>

<span data-ttu-id="2a656-134">Zum Arbeiten mit SpaServices installieren Sie die folgenden Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="2a656-134">To work with SpaServices, install the following:</span></span>
* <span data-ttu-id="2a656-135">[Node.js](https://nodejs.org/) (Version 6 oder höher) mit Npm</span><span class="sxs-lookup"><span data-stu-id="2a656-135">[Node.js](https://nodejs.org/) (version 6 or later) with npm</span></span>
    * <span data-ttu-id="2a656-136">Um diese Komponenten installiert sind und verwendbaren zu überprüfen, führen Sie Folgendes über die Befehlszeile ein:</span><span class="sxs-lookup"><span data-stu-id="2a656-136">To verify these components are installed and can be found, run the following from the command line:</span></span>

    ```console
    node -v && npm -v
    ```

<span data-ttu-id="2a656-137">Hinweis: Wenn Sie auf ein Azure-Website bereitstellen, Sie brauchen dies hier tun &mdash; Node.js ist installiert und in den serverumgebungen verfügbar.</span><span class="sxs-lookup"><span data-stu-id="2a656-137">Note: If you're deploying to an Azure web site, you don't need to do anything here &mdash; Node.js is installed and available in the server environments.</span></span>

* <span data-ttu-id="2a656-138">[.NET Core SDK](https://www.microsoft.com/net/download/core) 1.0 (oder höher)</span><span class="sxs-lookup"><span data-stu-id="2a656-138">[.NET Core SDK](https://www.microsoft.com/net/download/core) 1.0 (or later)</span></span>
    * <span data-ttu-id="2a656-139">Wenn Sie auf Windows nutzen, kann dies durch Auswählen von Visual Studio 2017 installiert **.NET Core plattformübergreifende Entwicklung** arbeitsauslastung.</span><span class="sxs-lookup"><span data-stu-id="2a656-139">If you're on Windows, this can be installed by selecting Visual Studio 2017's **.NET Core cross-platform development** workload.</span></span>

* <span data-ttu-id="2a656-140">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet-Paket</span><span class="sxs-lookup"><span data-stu-id="2a656-140">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet package</span></span>

<a name="server-prerendering"></a>

## <a name="server-side-prerendering"></a><span data-ttu-id="2a656-141">Serverseitige prerendering</span><span class="sxs-lookup"><span data-stu-id="2a656-141">Server-side prerendering</span></span>

<span data-ttu-id="2a656-142">Eine universelle (auch bekannt als funktionierenden) ist ein JavaScript-Anwendung kann sowohl auf den Server und dem Client ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="2a656-142">A universal (also known as isomorphic) application is a JavaScript application capable of running both on the server and the client.</span></span> <span data-ttu-id="2a656-143">Stellen eine universelle Plattform für diese Anwendung Entwicklungsstil, Angular reagieren und anderen beliebten Frameworks.</span><span class="sxs-lookup"><span data-stu-id="2a656-143">Angular, React, and other popular frameworks provide a universal platform for this application development style.</span></span> <span data-ttu-id="2a656-144">Die Idee ist zuerst Rendern die Framework-Komponenten auf dem Server über Node.js, und klicken Sie dann weitere Ausführung an den Client zu delegieren.</span><span class="sxs-lookup"><span data-stu-id="2a656-144">The idea is to first render the framework components on the server via Node.js, and then delegate further execution to the client.</span></span>

<span data-ttu-id="2a656-145">ASP.NET Core [Tag Hilfsprogramme](xref:mvc/views/tag-helpers/intro) gebotenen SpaServices die Implementierung eines serverseitigen Prerendering vereinfachen, indem Sie die JavaScript-Funktionen auf dem Server aufrufen.</span><span class="sxs-lookup"><span data-stu-id="2a656-145">ASP.NET Core [Tag Helpers](xref:mvc/views/tag-helpers/intro) provided by SpaServices simplify the implementation of server-side prerendering by invoking the JavaScript functions on the server.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="2a656-146">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="2a656-146">Prerequisites</span></span>

<span data-ttu-id="2a656-147">Installieren Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="2a656-147">Install the following:</span></span>
* <span data-ttu-id="2a656-148">[ASPNET-Prerendering](https://www.npmjs.com/package/aspnet-prerendering) Npm-Paket:</span><span class="sxs-lookup"><span data-stu-id="2a656-148">[aspnet-prerendering](https://www.npmjs.com/package/aspnet-prerendering) npm package:</span></span>

    ```console
    npm i -S aspnet-prerendering
    ```

### <a name="configuration"></a><span data-ttu-id="2a656-149">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="2a656-149">Configuration</span></span>

<span data-ttu-id="2a656-150">Die Tag-Hilfsprogramme werden in des Projekts über Namespace Registrierung erkennbar gemacht *_ViewImports.cshtml* Datei:</span><span class="sxs-lookup"><span data-stu-id="2a656-150">The Tag Helpers are made discoverable via namespace registration in the project's *_ViewImports.cshtml* file:</span></span>

[!code-csharp[Main](../client-side/spa-services/sample/SpaServicesSampleApp/Views/_ViewImports.cshtml?highlight=3)]

<span data-ttu-id="2a656-151">Diese Hilfsprogramme Tag Abstraktion der eigenheiten der direkten Kommunikation mit Low-Level-APIs durch die Nutzung einer HTML-ähnlichen Syntax in der Razor-Ansicht:</span><span class="sxs-lookup"><span data-stu-id="2a656-151">These Tag Helpers abstract away the intricacies of communicating directly with low-level APIs by leveraging an HTML-like syntax inside the Razor view:</span></span>

[!code-html[Main](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=5)]

### <a name="the-asp-prerender-module-tag-helper"></a><span data-ttu-id="2a656-152">Die `asp-prerender-module` Helper kennzeichnen</span><span class="sxs-lookup"><span data-stu-id="2a656-152">The `asp-prerender-module` Tag Helper</span></span>

<span data-ttu-id="2a656-153">Die `asp-prerender-module` Tag-Hilfsprogramms, die im vorherigen Codebeispiel verwendet führt *ClientApp/dist/main-server.js* auf dem Server über Node.js.</span><span class="sxs-lookup"><span data-stu-id="2a656-153">The `asp-prerender-module` Tag Helper, used in the preceding code example, executes *ClientApp/dist/main-server.js* on the server via Node.js.</span></span> <span data-ttu-id="2a656-154">Aus Gründen der Klarheit *Main server.js* Datei ist ein Element der Aufgabe Transpilation TypeScript und JavaScript in der [Webpaketdatei](http://webpack.github.io/) Buildprozess.</span><span class="sxs-lookup"><span data-stu-id="2a656-154">For clarity's sake, *main-server.js* file is an artifact of the TypeScript-to-JavaScript transpilation task in the [Webpack](http://webpack.github.io/) build process.</span></span> <span data-ttu-id="2a656-155">Webpaketdatei definiert einen Eintrag Point-Alias des `main-server`; und das Abhängigkeitsdiagramm für diesen Alias Durchlauf beginnt bei der *ClientApp/Boot-server.ts* Datei:</span><span class="sxs-lookup"><span data-stu-id="2a656-155">Webpack defines an entry point alias of `main-server`; and, traversal of the dependency graph for this alias begins at the *ClientApp/boot-server.ts* file:</span></span>

[!code-javascript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=53)]

<span data-ttu-id="2a656-156">Im folgenden Beispiel Angular der *ClientApp/Boot-server.ts* Datei nutzt die `createServerRenderer` Funktion und `RenderResult` Typ der `aspnet-prerendering` Npm-Paket zum Rendern der Server über Node.js zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="2a656-156">In the following Angular example, the *ClientApp/boot-server.ts* file utilizes the `createServerRenderer` function and `RenderResult` type of the `aspnet-prerendering` npm package to configure server rendering via Node.js.</span></span> <span data-ttu-id="2a656-157">Das HTML-Markup für serverseitiges Rendering an einen Funktionsaufruf Resolve übergeben wird, die in einer stark typisierten JavaScript umschlossen wird bestimmt `Promise` Objekt.</span><span class="sxs-lookup"><span data-stu-id="2a656-157">The HTML markup destined for server-side rendering is passed to a resolve function call, which is wrapped in a strongly-typed JavaScript `Promise` object.</span></span> <span data-ttu-id="2a656-158">Die `Promise` des Objekts "significance" ist, dass es das HTML-Markup der Seite für dateiinjektion in das DOM Platzhalterelement asynchron bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="2a656-158">The `Promise` object's significance is that it asynchronously supplies the HTML markup to the page for injection in the DOM's placeholder element.</span></span>

[!code-typescript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-34,79-)]

### <a name="the-asp-prerender-data-tag-helper"></a><span data-ttu-id="2a656-159">Die `asp-prerender-data` Helper kennzeichnen</span><span class="sxs-lookup"><span data-stu-id="2a656-159">The `asp-prerender-data` Tag Helper</span></span>

<span data-ttu-id="2a656-160">Verbindung mit der `asp-prerender-module` Tag-Hilfsobjekt, der `asp-prerender-data` Tag Helper können verwendet werden, um die Kontextinformationen für die serverseitige JavaScript aus der Razor-Ansicht zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="2a656-160">When coupled with the `asp-prerender-module` Tag Helper, the `asp-prerender-data` Tag Helper can be used to pass contextual information from the Razor view to the server-side JavaScript.</span></span> <span data-ttu-id="2a656-161">Z. B. das folgende Markup Benutzerdaten übergibt die `main-server` Modul:</span><span class="sxs-lookup"><span data-stu-id="2a656-161">For example, the following markup passes user data to the `main-server` module:</span></span>

[!code-html[Main](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=9-12)]

<span data-ttu-id="2a656-162">Die empfangenen Daten `UserName` Argument wird mithilfe der integrierten JSON-Serialisierungsprogramms serialisiert und befindet sich in der `params.data` Objekt.</span><span class="sxs-lookup"><span data-stu-id="2a656-162">The received `UserName` argument is serialized using the built-in JSON serializer and is stored in the `params.data` object.</span></span> <span data-ttu-id="2a656-163">Im folgenden Beispiel Angular werden verwendet, die Daten so erstellen Sie eine personalisierte Begrüßung innerhalb einer `h1` Element:</span><span class="sxs-lookup"><span data-stu-id="2a656-163">In the following Angular example, the data is used to construct a personalized greeting within an `h1` element:</span></span>

[!code-typescript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,38-52,79-)]

<span data-ttu-id="2a656-164">Hinweis: Tag Hilfsprogramme übergebene Eigenschaftennamen mit dargestellt **"PascalCase"** Notation.</span><span class="sxs-lookup"><span data-stu-id="2a656-164">Note: Property names passed in Tag Helpers are represented with **PascalCase** notation.</span></span> <span data-ttu-id="2a656-165">Vergleichen Sie für JavaScript ist, in denen die gleichen Eigenschaftennamen mit dargestellt werden, die **camelCase ändern möchten**.</span><span class="sxs-lookup"><span data-stu-id="2a656-165">Contrast that to JavaScript, where the same property names are represented with **camelCase**.</span></span> <span data-ttu-id="2a656-166">Die JSON-Serialisierung Standardkonfiguration ist verantwortlich für diesen Unterschied.</span><span class="sxs-lookup"><span data-stu-id="2a656-166">The default JSON serialization configuration is responsible for this difference.</span></span>

<span data-ttu-id="2a656-167">Informationen zum Erweitern auf dem vorherigen Beispiel Daten können werden vom Server an die Ansicht übergeben von hydrating der `globals` Eigenschaft bereitgestellt, um die `resolve` Funktion:</span><span class="sxs-lookup"><span data-stu-id="2a656-167">To expand upon the preceding code example, data can be passed from the server to the view by hydrating the `globals` property provided to the `resolve` function:</span></span>

[!code-typescript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,57-77,79-)]

<span data-ttu-id="2a656-168">Die `postList` Array definiert, die innerhalb der `globals` Objekt wird an des Browsers globale angefügt `window` Objekt.</span><span class="sxs-lookup"><span data-stu-id="2a656-168">The `postList` array defined inside the `globals` object is attached to the browser's global `window` object.</span></span> <span data-ttu-id="2a656-169">Diese Variable Korrelationsverlust globalen Bereich eliminiert Duplizierung des Aufwands, insbesondere, wie er bezieht sich auf das Laden der gleichen Daten einmal auf dem Server und erneut auf dem Client.</span><span class="sxs-lookup"><span data-stu-id="2a656-169">This variable hoisting to global scope eliminates duplication of effort, particularly as it pertains to loading the same data once on the server and again on the client.</span></span>

![Globale postList Variable Fensterobjekt angefügt](spa-services/_static/global_variable.png)

<a name="webpack-dev-middleware"></a>

## <a name="webpack-dev-middleware"></a><span data-ttu-id="2a656-171">Webpaketdatei Dev Middleware</span><span class="sxs-lookup"><span data-stu-id="2a656-171">Webpack Dev Middleware</span></span>

<span data-ttu-id="2a656-172">[Webpaketdatei Dev Middleware](https://webpack.github.io/docs/webpack-dev-middleware.html) bietet einen optimierte Entwicklung-Workflow, bei dem Webpaketdatei Ressourcen bei Bedarf erstellt.</span><span class="sxs-lookup"><span data-stu-id="2a656-172">[Webpack Dev Middleware](https://webpack.github.io/docs/webpack-dev-middleware.html) introduces a streamlined development workflow whereby Webpack builds resources on demand.</span></span> <span data-ttu-id="2a656-173">Die Middleware wird automatisch kompiliert und die clientseitige Ressourcen dient, wenn eine Seite im Browser geladen wird.</span><span class="sxs-lookup"><span data-stu-id="2a656-173">The middleware automatically compiles and serves client-side resources when a page is reloaded in the browser.</span></span> <span data-ttu-id="2a656-174">Der alternative Ansatz ist die Webpaketdatei manuell über das Projekt Npm Buildskript aufrufen, wenn eine Drittanbieter Abhängigkeit oder des benutzerdefinierten Codes ändert.</span><span class="sxs-lookup"><span data-stu-id="2a656-174">The alternate approach is to manually invoke Webpack via the project's npm build script when a third-party dependency or the custom code changes.</span></span> <span data-ttu-id="2a656-175">Ein Npm Skript erstellen, der *"Package.JSON"* Datei wird im folgenden Beispiel gezeigt:</span><span class="sxs-lookup"><span data-stu-id="2a656-175">An npm build script in the *package.json* file is shown in the following example:</span></span>

[!code-json[Main](../client-side/spa-services/sample/SpaServicesSampleApp/package.json?range=5)]

### <a name="prerequisites"></a><span data-ttu-id="2a656-176">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="2a656-176">Prerequisites</span></span>

<span data-ttu-id="2a656-177">Installieren Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="2a656-177">Install the following:</span></span>
* <span data-ttu-id="2a656-178">[ASPNET-Webpaketdatei](https://www.npmjs.com/package/aspnet-webpack) Npm-Paket:</span><span class="sxs-lookup"><span data-stu-id="2a656-178">[aspnet-webpack](https://www.npmjs.com/package/aspnet-webpack) npm package:</span></span>

    ```console
    npm i -D aspnet-webpack
    ```

### <a name="configuration"></a><span data-ttu-id="2a656-179">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="2a656-179">Configuration</span></span>

<span data-ttu-id="2a656-180">Webpaketdatei Dev Middleware registriert ist, in der HTTP-Anforderungspipeline über den folgenden Code in die *Startup.cs* Datei `Configure` Methode:</span><span class="sxs-lookup"><span data-stu-id="2a656-180">Webpack Dev Middleware is registered into the HTTP request pipeline via the following code in the *Startup.cs* file's `Configure` method:</span></span>

[!code-csharp[Main](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=webpack-middleware-registration&highlight=4)]

<span data-ttu-id="2a656-181">Die `UseWebpackDevMiddleware` Erweiterungsmethode aufgerufen werden muss, bevor [registrieren, statische Datei hosting](xref:fundamentals/static-files) über die `UseStaticFiles` Erweiterungsmethode.</span><span class="sxs-lookup"><span data-stu-id="2a656-181">The `UseWebpackDevMiddleware` extension method must be called before [registering static file hosting](xref:fundamentals/static-files) via the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="2a656-182">Registrieren Sie die Middleware aus Gründen der Sicherheit nur, wenn die app in den Entwicklungsmodus ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="2a656-182">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="2a656-183">Die *webpack.config.js* Datei `output.publicPath` -Eigenschaft teilt die Middleware zum Überwachen der `dist` Ordner Änderungen:</span><span class="sxs-lookup"><span data-stu-id="2a656-183">The *webpack.config.js* file's `output.publicPath` property tells the middleware to watch the `dist` folder for changes:</span></span>

[!code-javascript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,13-16)]

<a name="hot-module-replacement"></a>

## <a name="hot-module-replacement"></a><span data-ttu-id="2a656-184">Ersetzen eines Moduls im laufenden Systembetrieb</span><span class="sxs-lookup"><span data-stu-id="2a656-184">Hot Module Replacement</span></span>

<span data-ttu-id="2a656-185">Denken Sie an der Webpaketdatei [Hot Austausch eines Controllermoduls](https://webpack.github.io/docs/hot-module-replacement-with-webpack.html) (HMR)-Funktion als Weiterentwicklung der [Webpaketdatei Dev Middleware](#webpack-dev-middleware).</span><span class="sxs-lookup"><span data-stu-id="2a656-185">Think of Webpack's [Hot Module Replacement](https://webpack.github.io/docs/hot-module-replacement-with-webpack.html) (HMR) feature as an evolution of [Webpack Dev Middleware](#webpack-dev-middleware).</span></span> <span data-ttu-id="2a656-186">HMR führt dieselben Vorteile, aber es weiter optimiert des entwicklungsworkflows durch automatisches Aktualisieren der Seiteninhalt nach dem Kompilieren der Änderungen.</span><span class="sxs-lookup"><span data-stu-id="2a656-186">HMR introduces all the same benefits, but it further streamlines the development workflow by automatically updating page content after compiling the changes.</span></span> <span data-ttu-id="2a656-187">Verwechseln Sie dies mit einer Aktualisierung des Browsers, die mit dem aktuellen im Speicher enthaltenen Status und die Debugsitzung von der SPA beeinträchtigen würde.</span><span class="sxs-lookup"><span data-stu-id="2a656-187">Don't confuse this with a refresh of the browser, which would interfere with the current in-memory state and debugging session of the SPA.</span></span> <span data-ttu-id="2a656-188">Ist es einem Livelink zwischen der Webpaketdatei Dev Middleware-Dienst und den Browser, d. h. geändert werden ~ einfach ein anderes gesperrten Wort ~ Push an den Browser.</span><span class="sxs-lookup"><span data-stu-id="2a656-188">There is a live link between the Webpack Dev Middleware service and the browser, which means changes are ~simply another banned word~ pushed to the browser.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="2a656-189">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="2a656-189">Prerequisites</span></span>

<span data-ttu-id="2a656-190">Installieren Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="2a656-190">Install the following:</span></span>
* <span data-ttu-id="2a656-191">[Webpaketdatei während des Betriebs Middleware](https://www.npmjs.com/package/webpack-hot-middleware) Npm-Paket:</span><span class="sxs-lookup"><span data-stu-id="2a656-191">[webpack-hot-middleware](https://www.npmjs.com/package/webpack-hot-middleware) npm package:</span></span>

    ```console
    npm i -D webpack-hot-middleware
    ```

### <a name="configuration"></a><span data-ttu-id="2a656-192">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="2a656-192">Configuration</span></span>

<span data-ttu-id="2a656-193">Die Komponente HMR muss registriert werden, in MVCs-HTTP-Anforderungspipeline in die `Configure` Methode:</span><span class="sxs-lookup"><span data-stu-id="2a656-193">The HMR component must be registered into MVC's HTTP request pipeline in the `Configure` method:</span></span>

```csharp
app.UseWebpackDevMiddleware(new WebpackDevMiddlewareOptions {
    HotModuleReplacement = true
});
```

<span data-ttu-id="2a656-194">Wie wurde mit "true" [Webpaketdatei Dev Middleware](#webpack-dev-middleware), `UseWebpackDevMiddleware` Erweiterungsmethode aufgerufen werden muss, bevor die `UseStaticFiles` Erweiterungsmethode.</span><span class="sxs-lookup"><span data-stu-id="2a656-194">As was true with [Webpack Dev Middleware](#webpack-dev-middleware), the `UseWebpackDevMiddleware` extension method must be called before the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="2a656-195">Registrieren Sie die Middleware aus Gründen der Sicherheit nur, wenn die app in den Entwicklungsmodus ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="2a656-195">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="2a656-196">Die *webpack.config.js* Datei definieren, muss ein `plugins` array, auch wenn es leer gelassen wird:</span><span class="sxs-lookup"><span data-stu-id="2a656-196">The *webpack.config.js* file must define a `plugins` array, even if it's left empty:</span></span>

[!code-javascript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,25)]

<span data-ttu-id="2a656-197">Nach dem Laden der app im Browser, bietet die Entwicklertools Konsole Registerkarte Bestätigung des HMR-Aktivierung:</span><span class="sxs-lookup"><span data-stu-id="2a656-197">After loading the app in the browser, the developer tools' Console tab provides confirmation of HMR activation:</span></span>

![Im laufenden Systembetrieb Austausch eines Controllermoduls verbindungsmeldung](spa-services/_static/hmr_connected.png)

<a name="routing-helpers"></a>

## <a name="routing-helpers"></a><span data-ttu-id="2a656-199">Routing-Hilfsprogramme</span><span class="sxs-lookup"><span data-stu-id="2a656-199">Routing helpers</span></span>

<span data-ttu-id="2a656-200">In den meisten ASP.NET Core-basierte SPAs sollten Sie clientseitige neben dem routing serverseitige routing.</span><span class="sxs-lookup"><span data-stu-id="2a656-200">In most ASP.NET Core-based SPAs, you'll want client-side routing in addition to server-side routing.</span></span> <span data-ttu-id="2a656-201">Die SPA und MVC-routing-Systeme können ohne Störung möglich sind unabhängig voneinander arbeiten.</span><span class="sxs-lookup"><span data-stu-id="2a656-201">The SPA and MVC routing systems can work independently without interference.</span></span> <span data-ttu-id="2a656-202">Es ist jedoch eine Herausforderung machen Grenzfall: 404 HTTP-Antworten zu identifizieren.</span><span class="sxs-lookup"><span data-stu-id="2a656-202">There is, however, one edge case posing challenges: identifying 404 HTTP responses.</span></span>

<span data-ttu-id="2a656-203">Betrachten Sie das Szenario, in dem ein Standarddokument Route des `/some/page` verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="2a656-203">Consider the scenario in which an extensionless route of `/some/page` is used.</span></span> <span data-ttu-id="2a656-204">Angenommen Sie, die Anforderung nicht-Musterübereinstimmung eine serverseitige Route, aber die entsprechende Muster entspricht einer Route für die clientseitige.</span><span class="sxs-lookup"><span data-stu-id="2a656-204">Assume the request doesn't pattern-match a server-side route, but its pattern does match a client-side route.</span></span> <span data-ttu-id="2a656-205">Betrachten Sie nun eine eingehende Anforderung für `/images/user-512.png`, die im Allgemeinen davon ausgeht, eine Bilddatei auf dem Server.</span><span class="sxs-lookup"><span data-stu-id="2a656-205">Now consider an incoming request for `/images/user-512.png`, which generally expects to find an image file on the server.</span></span> <span data-ttu-id="2a656-206">Wenn diesem Pfad für die angeforderte Ressource keine serverseitigen Route oder eine statische Datei übereinstimmt, ist es unwahrscheinlich, dass die clientseitige Anwendung behandelt werden würde, in der Regel einen 404 HTTP-Statuscode zurückgegeben werden soll.</span><span class="sxs-lookup"><span data-stu-id="2a656-206">If that requested resource path doesn't match any server-side route or static file, it's unlikely that the client-side application would handle it — you generally want to return a 404 HTTP status code.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="2a656-207">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="2a656-207">Prerequisites</span></span>

<span data-ttu-id="2a656-208">Installieren Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="2a656-208">Install the following:</span></span>
* <span data-ttu-id="2a656-209">Das routing Npm-Paket für die clientseitige.</span><span class="sxs-lookup"><span data-stu-id="2a656-209">The client-side routing npm package.</span></span> <span data-ttu-id="2a656-210">Verwenden Sie Angular als Beispiel:</span><span class="sxs-lookup"><span data-stu-id="2a656-210">Using Angular as an example:</span></span>

    ```console
    npm i -S @angular/router
    ```

### <a name="configuration"></a><span data-ttu-id="2a656-211">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="2a656-211">Configuration</span></span>

<span data-ttu-id="2a656-212">Eine Erweiterungsmethode mit dem Namen `MapSpaFallbackRoute` werden in der `Configure` Methode:</span><span class="sxs-lookup"><span data-stu-id="2a656-212">An extension method named `MapSpaFallbackRoute` is used in the `Configure` method:</span></span>

[!code-csharp[Main](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=mvc-routing-table&highlight=7-9)]

<span data-ttu-id="2a656-213">Tipp: Routen werden in der Reihenfolge ausgewertet, in denen sie konfiguriert sind.</span><span class="sxs-lookup"><span data-stu-id="2a656-213">Tip: Routes are evaluated in the order in which they're configured.</span></span> <span data-ttu-id="2a656-214">Folglich die `default` Route im vorangehenden Codebeispiel wird zuerst für den Mustervergleich verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="2a656-214">Consequently, the `default` route in the preceding code example is used first for pattern matching.</span></span>

<a name="new-project-creation"></a>

## <a name="creating-a-new-project"></a><span data-ttu-id="2a656-215">Erstellen eines neuen Projekts</span><span class="sxs-lookup"><span data-stu-id="2a656-215">Creating a new project</span></span>

<span data-ttu-id="2a656-216">JavaScriptServices bietet vorkonfiguriert, dass Anwendungsvorlagen.</span><span class="sxs-lookup"><span data-stu-id="2a656-216">JavaScriptServices provides pre-configured application templates.</span></span> <span data-ttu-id="2a656-217">SpaServices wird in dieser Vorlagen in Verbindung mit verschiedenen Frameworks und Bibliotheken wie Angular, Aurelia Knockout, reagieren und Vue verwendet.</span><span class="sxs-lookup"><span data-stu-id="2a656-217">SpaServices is used in these templates, in conjunction with different frameworks and libraries such as Angular, Aurelia, Knockout, React, and Vue.</span></span>

<span data-ttu-id="2a656-218">Diese Vorlagen können über die .NET Core-CLI installiert werden, durch den folgenden Befehl ausführen:</span><span class="sxs-lookup"><span data-stu-id="2a656-218">These templates can be installed via the .NET Core CLI by running the following command:</span></span>

```console
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

<span data-ttu-id="2a656-219">Eine Liste der verfügbaren SPA-Vorlagen wird angezeigt:</span><span class="sxs-lookup"><span data-stu-id="2a656-219">A list of available SPA templates is displayed:</span></span>

| <span data-ttu-id="2a656-220">Vorlagen</span><span class="sxs-lookup"><span data-stu-id="2a656-220">Templates</span></span>                                 | <span data-ttu-id="2a656-221">Kurzname</span><span class="sxs-lookup"><span data-stu-id="2a656-221">Short Name</span></span> | <span data-ttu-id="2a656-222">Sprache</span><span class="sxs-lookup"><span data-stu-id="2a656-222">Language</span></span> | <span data-ttu-id="2a656-223">Tags</span><span class="sxs-lookup"><span data-stu-id="2a656-223">Tags</span></span>        |
|:------------------------------------------|:-----------|:---------|:------------|
| <span data-ttu-id="2a656-224">MVC ASP.NET Core mit Angular</span><span class="sxs-lookup"><span data-stu-id="2a656-224">MVC ASP.NET Core with Angular</span></span>             | <span data-ttu-id="2a656-225">angular</span><span class="sxs-lookup"><span data-stu-id="2a656-225">angular</span></span>    | <span data-ttu-id="2a656-226">[C#]</span><span class="sxs-lookup"><span data-stu-id="2a656-226">[C#]</span></span>     | <span data-ttu-id="2a656-227">MVC/Web/SPA</span><span class="sxs-lookup"><span data-stu-id="2a656-227">Web/MVC/SPA</span></span> |
| <span data-ttu-id="2a656-228">MVC ASP.NET Core mit Aurelia</span><span class="sxs-lookup"><span data-stu-id="2a656-228">MVC ASP.NET Core with Aurelia</span></span>             | <span data-ttu-id="2a656-229">aurelia</span><span class="sxs-lookup"><span data-stu-id="2a656-229">aurelia</span></span>    | <span data-ttu-id="2a656-230">[C#]</span><span class="sxs-lookup"><span data-stu-id="2a656-230">[C#]</span></span>     | <span data-ttu-id="2a656-231">MVC/Web/SPA</span><span class="sxs-lookup"><span data-stu-id="2a656-231">Web/MVC/SPA</span></span> |
| <span data-ttu-id="2a656-232">MVC ASP.NET Core mit Knockout.js</span><span class="sxs-lookup"><span data-stu-id="2a656-232">MVC ASP.NET Core with Knockout.js</span></span>         | <span data-ttu-id="2a656-233">Knockout</span><span class="sxs-lookup"><span data-stu-id="2a656-233">knockout</span></span>   | <span data-ttu-id="2a656-234">[C#]</span><span class="sxs-lookup"><span data-stu-id="2a656-234">[C#]</span></span>     | <span data-ttu-id="2a656-235">MVC/Web/SPA</span><span class="sxs-lookup"><span data-stu-id="2a656-235">Web/MVC/SPA</span></span> |
| <span data-ttu-id="2a656-236">MVC ASP.NET Core mit React.js</span><span class="sxs-lookup"><span data-stu-id="2a656-236">MVC ASP.NET Core with React.js</span></span>            | <span data-ttu-id="2a656-237">react</span><span class="sxs-lookup"><span data-stu-id="2a656-237">react</span></span>      | <span data-ttu-id="2a656-238">[C#]</span><span class="sxs-lookup"><span data-stu-id="2a656-238">[C#]</span></span>     | <span data-ttu-id="2a656-239">MVC/Web/SPA</span><span class="sxs-lookup"><span data-stu-id="2a656-239">Web/MVC/SPA</span></span> |
| <span data-ttu-id="2a656-240">MVC ASP.NET Core mit React.js und Redux</span><span class="sxs-lookup"><span data-stu-id="2a656-240">MVC ASP.NET Core with React.js and Redux</span></span>  | <span data-ttu-id="2a656-241">reactredux</span><span class="sxs-lookup"><span data-stu-id="2a656-241">reactredux</span></span> | <span data-ttu-id="2a656-242">[C#]</span><span class="sxs-lookup"><span data-stu-id="2a656-242">[C#]</span></span>     | <span data-ttu-id="2a656-243">MVC/Web/SPA</span><span class="sxs-lookup"><span data-stu-id="2a656-243">Web/MVC/SPA</span></span> |
| <span data-ttu-id="2a656-244">MVC ASP.NET Core mit Vue.js</span><span class="sxs-lookup"><span data-stu-id="2a656-244">MVC ASP.NET Core with Vue.js</span></span>              | <span data-ttu-id="2a656-245">VUE</span><span class="sxs-lookup"><span data-stu-id="2a656-245">vue</span></span>        | <span data-ttu-id="2a656-246">[C#]</span><span class="sxs-lookup"><span data-stu-id="2a656-246">[C#]</span></span>     | <span data-ttu-id="2a656-247">MVC/Web/SPA</span><span class="sxs-lookup"><span data-stu-id="2a656-247">Web/MVC/SPA</span></span> | 

<span data-ttu-id="2a656-248">Zum Erstellen eines neuen Projekts mithilfe einer der SPA-Vorlagen enthalten die **Kurzname** der Vorlage in der `dotnet new` Befehl.</span><span class="sxs-lookup"><span data-stu-id="2a656-248">To create a new project using one of the SPA templates, include the **Short Name** of the template in the `dotnet new` command.</span></span> <span data-ttu-id="2a656-249">Der folgende Befehl erstellt eine Angular-Anwendung mit ASP.NET Core MVC für die Serverseite konfiguriert:</span><span class="sxs-lookup"><span data-stu-id="2a656-249">The following command creates an Angular application with ASP.NET Core MVC configured for the server side:</span></span>

```console
dotnet new angular
```

<a name="runtime-config-mode"></a>

### <a name="set-the-runtime-configuration-mode"></a><span data-ttu-id="2a656-250">Legen Sie den Modus der Common Language Runtime-Konfiguration</span><span class="sxs-lookup"><span data-stu-id="2a656-250">Set the runtime configuration mode</span></span>

<span data-ttu-id="2a656-251">Zwei primäre Runtime Konfigurationsmodi vorhanden sind:</span><span class="sxs-lookup"><span data-stu-id="2a656-251">Two primary runtime configuration modes exist:</span></span>
* <span data-ttu-id="2a656-252">**Entwicklung**:</span><span class="sxs-lookup"><span data-stu-id="2a656-252">**Development**:</span></span>
    * <span data-ttu-id="2a656-253">Enthält Zuordnungen von Quelle zur Erleichterung des Debuggings.</span><span class="sxs-lookup"><span data-stu-id="2a656-253">Includes source maps to ease debugging.</span></span>
    * <span data-ttu-id="2a656-254">Den clientseitige Code für die Leistung nicht optimiert werden.</span><span class="sxs-lookup"><span data-stu-id="2a656-254">Doesn't optimize the client-side code for performance.</span></span>
* <span data-ttu-id="2a656-255">**Produktion**:</span><span class="sxs-lookup"><span data-stu-id="2a656-255">**Production**:</span></span>
    * <span data-ttu-id="2a656-256">Schließt Quelle Karten an.</span><span class="sxs-lookup"><span data-stu-id="2a656-256">Excludes source maps.</span></span>
    * <span data-ttu-id="2a656-257">Optimiert die clientseitigen Code über Bündelung und Minimierung.</span><span class="sxs-lookup"><span data-stu-id="2a656-257">Optimizes the client-side code via bundling & minification.</span></span>

<span data-ttu-id="2a656-258">ASP.NET Core verwendet eine Umgebungsvariable namens `ASPNETCORE_ENVIRONMENT` zum Speichern des Konfigurationsmodus.</span><span class="sxs-lookup"><span data-stu-id="2a656-258">ASP.NET Core uses an environment variable named `ASPNETCORE_ENVIRONMENT` to store the configuration mode.</span></span> <span data-ttu-id="2a656-259">Finden Sie unter  **[Einrichten der Umgebung](xref:fundamentals/environments#setting-the-environment)**  für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="2a656-259">See **[Setting the environment](xref:fundamentals/environments#setting-the-environment)** for more information.</span></span>

### <a name="running-with-net-core-cli"></a><span data-ttu-id="2a656-260">Ausführen von mit .NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="2a656-260">Running with .NET Core CLI</span></span>

<span data-ttu-id="2a656-261">Stellen Sie die erforderlichen NuGet und Npm-Pakete durch den folgenden Befehl ausführen, auf der Stammebene des Projekts wieder her:</span><span class="sxs-lookup"><span data-stu-id="2a656-261">Restore the required NuGet and npm packages by running the following command at the project root:</span></span>

```console
dotnet restore && npm i
```

<span data-ttu-id="2a656-262">Erstellen und Ausführen der Anwendung:</span><span class="sxs-lookup"><span data-stu-id="2a656-262">Build and run the application:</span></span>

```console
dotnet run
```

<span data-ttu-id="2a656-263">Starten der Anwendung auf "localhost" gemäß der [Common Language Runtime-Konfigurationsmodus](#runtime-config-mode).</span><span class="sxs-lookup"><span data-stu-id="2a656-263">The application starts on localhost according to the [runtime configuration mode](#runtime-config-mode).</span></span> <span data-ttu-id="2a656-264">Navigieren zu `http://localhost:5000` die Startseite im Browser angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="2a656-264">Navigating to `http://localhost:5000` in the browser displays the landing page.</span></span>

### <a name="running-with-visual-studio-2017"></a><span data-ttu-id="2a656-265">Mit Visual Studio 2017 ausführen</span><span class="sxs-lookup"><span data-stu-id="2a656-265">Running with Visual Studio 2017</span></span>

<span data-ttu-id="2a656-266">Öffnen der *csproj* von generierten Datei die `dotnet new` Befehl.</span><span class="sxs-lookup"><span data-stu-id="2a656-266">Open the *.csproj* file generated by the `dotnet new` command.</span></span> <span data-ttu-id="2a656-267">Die erforderlichen NuGet und Npm-Pakete werden auf Projekt öffnen automatisch wiederhergestellt.</span><span class="sxs-lookup"><span data-stu-id="2a656-267">The required NuGet and npm packages are restored automatically upon project open.</span></span> <span data-ttu-id="2a656-268">Diese Wiederherstellung kann einige Minuten dauern, und die Anwendung ist bereit, die ausgeführt werden, wenn er abgeschlossen wurde.</span><span class="sxs-lookup"><span data-stu-id="2a656-268">This restoration process may take up to a few minutes, and the application is ready to run when it completes.</span></span> <span data-ttu-id="2a656-269">Klicken Sie auf die grüne Schaltfläche ausführen, oder drücken Sie `Ctrl + F5`, und der Browser geöffnet wird, zur Startseite der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="2a656-269">Click the green run button or press `Ctrl + F5`, and the browser opens to the application's landing page.</span></span> <span data-ttu-id="2a656-270">Die Anwendung ausgeführt wird, auf "localhost" gemäß der [Common Language Runtime-Konfigurationsmodus](#runtime-config-mode).</span><span class="sxs-lookup"><span data-stu-id="2a656-270">The application runs on localhost according to the [runtime configuration mode](#runtime-config-mode).</span></span> 

<a name="app-testing"></a>

## <a name="testing-the-app"></a><span data-ttu-id="2a656-271">Testen der app</span><span class="sxs-lookup"><span data-stu-id="2a656-271">Testing the app</span></span>

<span data-ttu-id="2a656-272">SpaServices Vorlagen sind für die clientseitige Tests mithilfe von vorkonfiguriert [Karma](https://karma-runner.github.io/1.0/index.html) und [Jasmine](https://jasmine.github.io/).</span><span class="sxs-lookup"><span data-stu-id="2a656-272">SpaServices templates are pre-configured to run client-side tests using [Karma](https://karma-runner.github.io/1.0/index.html) and [Jasmine](https://jasmine.github.io/).</span></span> <span data-ttu-id="2a656-273">Jasmine ist eine beliebte UnitTest-Framework für JavaScript, während der Karma ein Testprogramm für diese Tests ist.</span><span class="sxs-lookup"><span data-stu-id="2a656-273">Jasmine is a popular unit testing framework for JavaScript, whereas Karma is a test runner for those tests.</span></span> <span data-ttu-id="2a656-274">Karma ist so konfiguriert, dass die Bearbeitung der [Webpaketdatei Dev Middleware](#webpack-dev-middleware) , dass Sie nicht beenden, und führen den Test, jedes Mal, wenn Änderungen vorgenommen werden müssen.</span><span class="sxs-lookup"><span data-stu-id="2a656-274">Karma is configured to work with the [Webpack Dev Middleware](#webpack-dev-middleware) such that you don’t have to stop and run the test every time changes are made.</span></span> <span data-ttu-id="2a656-275">Ob er den Code für den Testfall oder den Testfall selbst ausgeführt wird, wird der Test automatisch ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="2a656-275">Whether it's the code running against the test case or the test case itself, the test runs automatically.</span></span>

<span data-ttu-id="2a656-276">Verwendung der Angular-Anwendung als Beispiel zwei Jasmintee Testfälle sind bereits deckte die `CounterComponent` in der *counter.component.spec.ts* Datei:</span><span class="sxs-lookup"><span data-stu-id="2a656-276">Using the Angular application as an example, two Jasmine test cases are already provided for the `CounterComponent` in the *counter.component.spec.ts* file:</span></span>

[!code-typescript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/app/components/counter/counter.component.spec.ts?range=15-28)]

<span data-ttu-id="2a656-277">Öffnen Sie die Eingabeaufforderung im Stammverzeichnis des Projekts, und führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="2a656-277">Open the command prompt at the project root, and run the following command:</span></span>

```console
npm test
```

<span data-ttu-id="2a656-278">Startet das Skript die Karma-Testprogramm, die in definierten Einstellungen liest die *karma.conf.js* Datei.</span><span class="sxs-lookup"><span data-stu-id="2a656-278">The script launches the Karma test runner, which reads the settings defined in the *karma.conf.js* file.</span></span> <span data-ttu-id="2a656-279">Neben anderen Einstellungen der *karma.conf.js* identifiziert die Testdateien auszuführenden über seine `files` Array:</span><span class="sxs-lookup"><span data-stu-id="2a656-279">Among other settings, the *karma.conf.js* identifies the test files to be executed via its `files` array:</span></span>

[!code-javascript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/test/karma.conf.js?range=4-5,8-11)]

<a name="app-publishing"></a>

## <a name="publishing-the-application"></a><span data-ttu-id="2a656-280">Veröffentlichen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="2a656-280">Publishing the application</span></span>

<span data-ttu-id="2a656-281">Kombinieren die generierte clientseitige Bestand und der veröffentlichten ASP.NET Core-Artefakte in ein Paket kann jetzt bereitstellen kann sehr aufwändig sein.</span><span class="sxs-lookup"><span data-stu-id="2a656-281">Combining the generated client-side assets and the published ASP.NET Core artifacts into a ready-to-deploy package can be cumbersome.</span></span> <span data-ttu-id="2a656-282">Glücklicherweise orchestriert SpaServices dieses Prozesses für die gesamte Veröffentlichung mit einem benutzerdefinierten MSBuild-Ziel mit dem Namen `RunWebpack`:</span><span class="sxs-lookup"><span data-stu-id="2a656-282">Thankfully, SpaServices orchestrates that entire publication process with a custom MSBuild target named `RunWebpack`:</span></span>

[!code-xml[Main](../client-side/spa-services/sample/SpaServicesSampleApp/SpaServicesSampleApp.csproj?range=31-45)]

<span data-ttu-id="2a656-283">Das MSBuild-Ziel hat die folgenden Verantwortungen:</span><span class="sxs-lookup"><span data-stu-id="2a656-283">The MSBuild target has the following responsibilities:</span></span>
1. <span data-ttu-id="2a656-284">Wiederherstellen der Npm-Pakete</span><span class="sxs-lookup"><span data-stu-id="2a656-284">Restore the npm packages</span></span>
1. <span data-ttu-id="2a656-285">Erstellen eines Builds Produktions-Grade der Ressourcen von Drittanbietern, die clientseitige</span><span class="sxs-lookup"><span data-stu-id="2a656-285">Create a production-grade build of the third-party, client-side assets</span></span>
1. <span data-ttu-id="2a656-286">Erstellen eines Builds Produktions-Grade benutzerdefinierte clientseitige Objekte</span><span class="sxs-lookup"><span data-stu-id="2a656-286">Create a production-grade build of the custom client-side assets</span></span>
1. <span data-ttu-id="2a656-287">Kopieren Sie die Webpaketdatei generierte Objekte in dem Veröffentlichungsordner</span><span class="sxs-lookup"><span data-stu-id="2a656-287">Copy the Webpack-generated assets to the publish folder</span></span>

<span data-ttu-id="2a656-288">Das MSBuild-Ziel wird aufgerufen, wenn ausgeführt:</span><span class="sxs-lookup"><span data-stu-id="2a656-288">The MSBuild target is invoked when running:</span></span>

```console
dotnet publish -c Release
```

## <a name="additional-resources"></a><span data-ttu-id="2a656-289">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="2a656-289">Additional resources</span></span>

* [<span data-ttu-id="2a656-290">Angular Docs</span><span class="sxs-lookup"><span data-stu-id="2a656-290">Angular Docs</span></span>](https://angular.io/docs)
