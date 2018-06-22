---
title: Verwenden Sie JavaScriptServices zu Single Page Applications in ASP.NET Kern
author: scottaddie
description: Weitere Informationen Sie zu den Vorteilen der Verwendung von JavaScriptServices zum Erstellen von einer einzelnen Seite Anwendung (SPA) durch ASP.NET Core gesichert wird.
ms.author: scaddie
ms.custom: H1Hack27Feb2017
ms.date: 08/02/2017
uid: client-side/spa-services
ms.openlocfilehash: c3f454ddd91fadf94e4ee4faa8930d8a89d13833
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279622"
---
# <a name="use-javascriptservices-to-create-single-page-applications-in-aspnet-core"></a><span data-ttu-id="bbe89-103">Verwenden Sie JavaScriptServices zu Single Page Applications in ASP.NET Kern</span><span class="sxs-lookup"><span data-stu-id="bbe89-103">Use JavaScriptServices to Create Single Page Applications in ASP.NET Core</span></span>

<span data-ttu-id="bbe89-104">Durch [Scott Addie](https://github.com/scottaddie) und [Fiyaz Hasan](http://fiyazhasan.me/)</span><span class="sxs-lookup"><span data-stu-id="bbe89-104">By [Scott Addie](https://github.com/scottaddie) and [Fiyaz Hasan](http://fiyazhasan.me/)</span></span>

<span data-ttu-id="bbe89-105">Eine Single-Page-Anwendung (SPA) ist ein Webanwendungstyp, der aufgrund seiner hohen Servicequalität und Leistungsstärke sehr beliebt ist.</span><span class="sxs-lookup"><span data-stu-id="bbe89-105">A Single Page Application (SPA) is a popular type of web application due to its inherent rich user experience.</span></span> <span data-ttu-id="bbe89-106">Die Integration clientseitiger SPA-Frameworks oder -Bibliotheken wie [Angular](https://angular.io/) oder [React](https://facebook.github.io/react/) mit serverseitigen Frameworks wie ASP.NET Core schwierig sein kann.</span><span class="sxs-lookup"><span data-stu-id="bbe89-106">Integrating client-side SPA frameworks or libraries, such as [Angular](https://angular.io/) or [React](https://facebook.github.io/react/), with server-side frameworks like ASP.NET Core can be difficult.</span></span> <span data-ttu-id="bbe89-107">[JavaScriptServices](https://github.com/aspnet/JavaScriptServices) wurde entwickelt, um Inkonsistenzen im Integrationsprozess zu reduzieren.</span><span class="sxs-lookup"><span data-stu-id="bbe89-107">[JavaScriptServices](https://github.com/aspnet/JavaScriptServices) was developed to reduce friction in the integration process.</span></span> <span data-ttu-id="bbe89-108">Hierdurch wird ein reibungsloser Betrieb verschiedener Client- und Servertechnologiestapel möglich.</span><span class="sxs-lookup"><span data-stu-id="bbe89-108">It enables seamless operation between the different client and server technology stacks.</span></span>

<span data-ttu-id="bbe89-109">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/spa-services/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="bbe89-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/spa-services/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<a name="what-is-js-services"></a>

## <a name="what-is-javascriptservices"></a><span data-ttu-id="bbe89-110">Was ist JavaScriptServices?</span><span class="sxs-lookup"><span data-stu-id="bbe89-110">What is JavaScriptServices?</span></span>

<span data-ttu-id="bbe89-111">JavaScriptServices ist eine Auflistung von clientseitigen Technologien zum ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bbe89-111">JavaScriptServices is a collection of client-side technologies for ASP.NET Core.</span></span> <span data-ttu-id="bbe89-112">Das Ziel besteht darin zu ASP.NET Core als Entwicklers bevorzugte serverseitige Plattform zum Erstellen von SPAs zu positionieren.</span><span class="sxs-lookup"><span data-stu-id="bbe89-112">Its goal is to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span>

<span data-ttu-id="bbe89-113">JavaScriptServices besteht aus drei unterschiedlichen NuGet-Pakete:</span><span class="sxs-lookup"><span data-stu-id="bbe89-113">JavaScriptServices consists of three distinct NuGet packages:</span></span>
* <span data-ttu-id="bbe89-114">[Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)</span><span class="sxs-lookup"><span data-stu-id="bbe89-114">[Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)</span></span>
* <span data-ttu-id="bbe89-115">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)</span><span class="sxs-lookup"><span data-stu-id="bbe89-115">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)</span></span>
* <span data-ttu-id="bbe89-116">[Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)</span><span class="sxs-lookup"><span data-stu-id="bbe89-116">[Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)</span></span>

<span data-ttu-id="bbe89-117">Diese Pakete sind nützlich, wenn Sie:</span><span class="sxs-lookup"><span data-stu-id="bbe89-117">These packages are useful if you:</span></span>
* <span data-ttu-id="bbe89-118">JavaScript wird auf dem Server ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="bbe89-118">Run JavaScript on the server</span></span>
* <span data-ttu-id="bbe89-119">Verwenden Sie einen SPA-Framework oder Bibliothek</span><span class="sxs-lookup"><span data-stu-id="bbe89-119">Use a SPA framework or library</span></span>
* <span data-ttu-id="bbe89-120">Erstellen Sie die clientseitige Medienobjekte mit Webpaketdatei</span><span class="sxs-lookup"><span data-stu-id="bbe89-120">Build client-side assets with Webpack</span></span>

<span data-ttu-id="bbe89-121">Ein Großteil der Fokus in diesem Artikel wird auf die mithilfe des Pakets SpaServices platziert.</span><span class="sxs-lookup"><span data-stu-id="bbe89-121">Much of the focus in this article is placed on using the SpaServices package.</span></span>

<a name="what-is-spa-services"></a>

## <a name="what-is-spaservices"></a><span data-ttu-id="bbe89-122">Was ist SpaServices?</span><span class="sxs-lookup"><span data-stu-id="bbe89-122">What is SpaServices?</span></span>

<span data-ttu-id="bbe89-123">SpaServices wurde erstellt, um ASP.NET Core als Entwicklers bevorzugte serverseitige Plattform zum Erstellen von SPAs zu positionieren.</span><span class="sxs-lookup"><span data-stu-id="bbe89-123">SpaServices was created to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span> <span data-ttu-id="bbe89-124">SpaServices ist nicht erforderlich, um SPAs mit ASP.NET Core entwickeln, und Sie in einem bestimmten Client-Framework ist nicht gesperrt.</span><span class="sxs-lookup"><span data-stu-id="bbe89-124">SpaServices isn't required to develop SPAs with ASP.NET Core, and it doesn't lock you into a particular client framework.</span></span>

<span data-ttu-id="bbe89-125">Bietet eine SpaServices nützlich Infrastruktur wie z. B.:</span><span class="sxs-lookup"><span data-stu-id="bbe89-125">SpaServices provides useful infrastructure such as:</span></span>
* [<span data-ttu-id="bbe89-126">Serverseitige prerendering</span><span class="sxs-lookup"><span data-stu-id="bbe89-126">Server-side prerendering</span></span>](#server-prerendering)
* [<span data-ttu-id="bbe89-127">Webpaketdatei Dev Middleware</span><span class="sxs-lookup"><span data-stu-id="bbe89-127">Webpack Dev Middleware</span></span>](#webpack-dev-middleware)
* [<span data-ttu-id="bbe89-128">Ersetzen eines Moduls im laufenden Systembetrieb</span><span class="sxs-lookup"><span data-stu-id="bbe89-128">Hot Module Replacement</span></span>](#hot-module-replacement)
* [<span data-ttu-id="bbe89-129">Routing-Hilfsprogramme</span><span class="sxs-lookup"><span data-stu-id="bbe89-129">Routing helpers</span></span>](#routing-helpers)

<span data-ttu-id="bbe89-130">Erweitern Sie diese Infrastrukturkomponenten zusammen des entwicklungsworkflows und der Common Language Runtime-Umgebung.</span><span class="sxs-lookup"><span data-stu-id="bbe89-130">Collectively, these infrastructure components enhance both the development workflow and the runtime experience.</span></span> <span data-ttu-id="bbe89-131">Die Komponenten können einzeln übernommen werden.</span><span class="sxs-lookup"><span data-stu-id="bbe89-131">The components can be adopted individually.</span></span>

<a name="spa-services-prereqs"></a>

## <a name="prerequisites-for-using-spaservices"></a><span data-ttu-id="bbe89-132">Voraussetzungen für die Verwendung von SpaServices</span><span class="sxs-lookup"><span data-stu-id="bbe89-132">Prerequisites for using SpaServices</span></span>

<span data-ttu-id="bbe89-133">Zum Arbeiten mit SpaServices installieren Sie die folgenden Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="bbe89-133">To work with SpaServices, install the following:</span></span>
* <span data-ttu-id="bbe89-134">[Node.js](https://nodejs.org/) (Version 6 oder höher) mit Npm</span><span class="sxs-lookup"><span data-stu-id="bbe89-134">[Node.js](https://nodejs.org/) (version 6 or later) with npm</span></span>
  * <span data-ttu-id="bbe89-135">Um diese Komponenten installiert sind und verwendbaren zu überprüfen, führen Sie Folgendes über die Befehlszeile ein:</span><span class="sxs-lookup"><span data-stu-id="bbe89-135">To verify these components are installed and can be found, run the following from the command line:</span></span>

    ```console
    node -v && npm -v
    ```

<span data-ttu-id="bbe89-136">Hinweis: Wenn Sie auf ein Azure-Website bereitstellen, Sie brauchen dies hier tun &mdash; Node.js ist installiert und in den serverumgebungen verfügbar.</span><span class="sxs-lookup"><span data-stu-id="bbe89-136">Note: If you're deploying to an Azure web site, you don't need to do anything here &mdash; Node.js is installed and available in the server environments.</span></span>

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]

  * <span data-ttu-id="bbe89-137">Wenn Sie unter Windows mithilfe von Visual Studio 2017 sind, wird das SDK installiert, durch Auswählen der **.NET Core plattformübergreifende Entwicklung** arbeitsauslastung.</span><span class="sxs-lookup"><span data-stu-id="bbe89-137">If you're on Windows using Visual Studio 2017, the SDK is installed by selecting the **.NET Core cross-platform development** workload.</span></span>

* <span data-ttu-id="bbe89-138">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet-Paket</span><span class="sxs-lookup"><span data-stu-id="bbe89-138">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet package</span></span>

<a name="server-prerendering"></a>

## <a name="server-side-prerendering"></a><span data-ttu-id="bbe89-139">Serverseitige prerendering</span><span class="sxs-lookup"><span data-stu-id="bbe89-139">Server-side prerendering</span></span>

<span data-ttu-id="bbe89-140">Eine universelle (auch bekannt als funktionierenden) ist ein JavaScript-Anwendung kann sowohl auf den Server und dem Client ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="bbe89-140">A universal (also known as isomorphic) application is a JavaScript application capable of running both on the server and the client.</span></span> <span data-ttu-id="bbe89-141">Stellen eine universelle Plattform für diese Anwendung Entwicklungsstil, Angular reagieren und anderen beliebten Frameworks.</span><span class="sxs-lookup"><span data-stu-id="bbe89-141">Angular, React, and other popular frameworks provide a universal platform for this application development style.</span></span> <span data-ttu-id="bbe89-142">Die Idee ist zuerst Rendern die Framework-Komponenten auf dem Server über Node.js, und klicken Sie dann weitere Ausführung an den Client zu delegieren.</span><span class="sxs-lookup"><span data-stu-id="bbe89-142">The idea is to first render the framework components on the server via Node.js, and then delegate further execution to the client.</span></span>

<span data-ttu-id="bbe89-143">ASP.NET Core [Tag Hilfsprogramme](xref:mvc/views/tag-helpers/intro) gebotenen SpaServices die Implementierung eines serverseitigen Prerendering vereinfachen, indem Sie die JavaScript-Funktionen auf dem Server aufrufen.</span><span class="sxs-lookup"><span data-stu-id="bbe89-143">ASP.NET Core [Tag Helpers](xref:mvc/views/tag-helpers/intro) provided by SpaServices simplify the implementation of server-side prerendering by invoking the JavaScript functions on the server.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="bbe89-144">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="bbe89-144">Prerequisites</span></span>

<span data-ttu-id="bbe89-145">Installieren Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="bbe89-145">Install the following:</span></span>
* <span data-ttu-id="bbe89-146">[ASPNET-Prerendering](https://www.npmjs.com/package/aspnet-prerendering) Npm-Paket:</span><span class="sxs-lookup"><span data-stu-id="bbe89-146">[aspnet-prerendering](https://www.npmjs.com/package/aspnet-prerendering) npm package:</span></span>

    ```console
    npm i -S aspnet-prerendering
    ```

### <a name="configuration"></a><span data-ttu-id="bbe89-147">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="bbe89-147">Configuration</span></span>

<span data-ttu-id="bbe89-148">Die Tag-Hilfsprogramme werden in des Projekts über Namespace Registrierung erkennbar gemacht *_ViewImports.cshtml* Datei:</span><span class="sxs-lookup"><span data-stu-id="bbe89-148">The Tag Helpers are made discoverable via namespace registration in the project's *_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/_ViewImports.cshtml?highlight=3)]

<span data-ttu-id="bbe89-149">Diese Hilfsprogramme Tag Abstraktion der eigenheiten der direkten Kommunikation mit Low-Level-APIs durch die Nutzung einer HTML-ähnlichen Syntax in der Razor-Ansicht:</span><span class="sxs-lookup"><span data-stu-id="bbe89-149">These Tag Helpers abstract away the intricacies of communicating directly with low-level APIs by leveraging an HTML-like syntax inside the Razor view:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=5)]

### <a name="the-asp-prerender-module-tag-helper"></a><span data-ttu-id="bbe89-150">Die `asp-prerender-module` Helper kennzeichnen</span><span class="sxs-lookup"><span data-stu-id="bbe89-150">The `asp-prerender-module` Tag Helper</span></span>

<span data-ttu-id="bbe89-151">Die `asp-prerender-module` Tag-Hilfsprogramms, die im vorherigen Codebeispiel verwendet führt *ClientApp/dist/main-server.js* auf dem Server über Node.js.</span><span class="sxs-lookup"><span data-stu-id="bbe89-151">The `asp-prerender-module` Tag Helper, used in the preceding code example, executes *ClientApp/dist/main-server.js* on the server via Node.js.</span></span> <span data-ttu-id="bbe89-152">Aus Gründen der Klarheit *Main server.js* Datei ist ein Element der Aufgabe Transpilation TypeScript und JavaScript in der [Webpaketdatei](http://webpack.github.io/) Buildprozess.</span><span class="sxs-lookup"><span data-stu-id="bbe89-152">For clarity's sake, *main-server.js* file is an artifact of the TypeScript-to-JavaScript transpilation task in the [Webpack](http://webpack.github.io/) build process.</span></span> <span data-ttu-id="bbe89-153">Webpaketdatei definiert einen Eintrag Point-Alias des `main-server`; und das Abhängigkeitsdiagramm für diesen Alias Durchlauf beginnt bei der *ClientApp/Boot-server.ts* Datei:</span><span class="sxs-lookup"><span data-stu-id="bbe89-153">Webpack defines an entry point alias of `main-server`; and, traversal of the dependency graph for this alias begins at the *ClientApp/boot-server.ts* file:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=53)]

<span data-ttu-id="bbe89-154">Im folgenden Beispiel Angular der *ClientApp/Boot-server.ts* Datei nutzt die `createServerRenderer` Funktion und `RenderResult` Typ der `aspnet-prerendering` Npm-Paket zum Rendern der Server über Node.js zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="bbe89-154">In the following Angular example, the *ClientApp/boot-server.ts* file utilizes the `createServerRenderer` function and `RenderResult` type of the `aspnet-prerendering` npm package to configure server rendering via Node.js.</span></span> <span data-ttu-id="bbe89-155">Das HTML-Markup für serverseitiges Rendering an einen Funktionsaufruf Resolve übergeben wird, die in einer stark typisierten JavaScript umschlossen wird bestimmt `Promise` Objekt.</span><span class="sxs-lookup"><span data-stu-id="bbe89-155">The HTML markup destined for server-side rendering is passed to a resolve function call, which is wrapped in a strongly-typed JavaScript `Promise` object.</span></span> <span data-ttu-id="bbe89-156">Die `Promise` des Objekts "significance" ist, dass es das HTML-Markup der Seite für dateiinjektion in das DOM Platzhalterelement asynchron bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="bbe89-156">The `Promise` object's significance is that it asynchronously supplies the HTML markup to the page for injection in the DOM's placeholder element.</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-34,79-)]

### <a name="the-asp-prerender-data-tag-helper"></a><span data-ttu-id="bbe89-157">Die `asp-prerender-data` Helper kennzeichnen</span><span class="sxs-lookup"><span data-stu-id="bbe89-157">The `asp-prerender-data` Tag Helper</span></span>

<span data-ttu-id="bbe89-158">Verbindung mit der `asp-prerender-module` Tag-Hilfsobjekt, der `asp-prerender-data` Tag Helper können verwendet werden, um die Kontextinformationen für die serverseitige JavaScript aus der Razor-Ansicht zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="bbe89-158">When coupled with the `asp-prerender-module` Tag Helper, the `asp-prerender-data` Tag Helper can be used to pass contextual information from the Razor view to the server-side JavaScript.</span></span> <span data-ttu-id="bbe89-159">Z. B. das folgende Markup Benutzerdaten übergibt die `main-server` Modul:</span><span class="sxs-lookup"><span data-stu-id="bbe89-159">For example, the following markup passes user data to the `main-server` module:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=9-12)]

<span data-ttu-id="bbe89-160">Die empfangenen Daten `UserName` Argument wird mithilfe der integrierten JSON-Serialisierungsprogramms serialisiert und befindet sich in der `params.data` Objekt.</span><span class="sxs-lookup"><span data-stu-id="bbe89-160">The received `UserName` argument is serialized using the built-in JSON serializer and is stored in the `params.data` object.</span></span> <span data-ttu-id="bbe89-161">Im folgenden Beispiel Angular werden verwendet, die Daten so erstellen Sie eine personalisierte Begrüßung innerhalb einer `h1` Element:</span><span class="sxs-lookup"><span data-stu-id="bbe89-161">In the following Angular example, the data is used to construct a personalized greeting within an `h1` element:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,38-52,79-)]

<span data-ttu-id="bbe89-162">Hinweis: Tag Hilfsprogramme übergebene Eigenschaftennamen mit dargestellt **"PascalCase"** Notation.</span><span class="sxs-lookup"><span data-stu-id="bbe89-162">Note: Property names passed in Tag Helpers are represented with **PascalCase** notation.</span></span> <span data-ttu-id="bbe89-163">Vergleichen Sie für JavaScript ist, in denen die gleichen Eigenschaftennamen mit dargestellt werden, die **camelCase ändern möchten**.</span><span class="sxs-lookup"><span data-stu-id="bbe89-163">Contrast that to JavaScript, where the same property names are represented with **camelCase**.</span></span> <span data-ttu-id="bbe89-164">Die JSON-Serialisierung Standardkonfiguration ist verantwortlich für diesen Unterschied.</span><span class="sxs-lookup"><span data-stu-id="bbe89-164">The default JSON serialization configuration is responsible for this difference.</span></span>

<span data-ttu-id="bbe89-165">Informationen zum Erweitern auf dem vorherigen Beispiel Daten können werden vom Server an die Ansicht übergeben von hydrating der `globals` Eigenschaft bereitgestellt, um die `resolve` Funktion:</span><span class="sxs-lookup"><span data-stu-id="bbe89-165">To expand upon the preceding code example, data can be passed from the server to the view by hydrating the `globals` property provided to the `resolve` function:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,57-77,79-)]

<span data-ttu-id="bbe89-166">Die `postList` Array definiert, die innerhalb der `globals` Objekt wird an des Browsers globale angefügt `window` Objekt.</span><span class="sxs-lookup"><span data-stu-id="bbe89-166">The `postList` array defined inside the `globals` object is attached to the browser's global `window` object.</span></span> <span data-ttu-id="bbe89-167">Diese Variable Korrelationsverlust globalen Bereich eliminiert Duplizierung des Aufwands, insbesondere, wie er bezieht sich auf das Laden der gleichen Daten einmal auf dem Server und erneut auf dem Client.</span><span class="sxs-lookup"><span data-stu-id="bbe89-167">This variable hoisting to global scope eliminates duplication of effort, particularly as it pertains to loading the same data once on the server and again on the client.</span></span>

![Globale postList Variable Fensterobjekt angefügt](spa-services/_static/global_variable.png)

<a name="webpack-dev-middleware"></a>

## <a name="webpack-dev-middleware"></a><span data-ttu-id="bbe89-169">Webpaketdatei Dev Middleware</span><span class="sxs-lookup"><span data-stu-id="bbe89-169">Webpack Dev Middleware</span></span>

<span data-ttu-id="bbe89-170">[Webpaketdatei Dev Middleware](https://webpack.github.io/docs/webpack-dev-middleware.html) bietet einen optimierte Entwicklung-Workflow, bei dem Webpaketdatei Ressourcen bei Bedarf erstellt.</span><span class="sxs-lookup"><span data-stu-id="bbe89-170">[Webpack Dev Middleware](https://webpack.github.io/docs/webpack-dev-middleware.html) introduces a streamlined development workflow whereby Webpack builds resources on demand.</span></span> <span data-ttu-id="bbe89-171">Die Middleware wird automatisch kompiliert und die clientseitige Ressourcen dient, wenn eine Seite im Browser geladen wird.</span><span class="sxs-lookup"><span data-stu-id="bbe89-171">The middleware automatically compiles and serves client-side resources when a page is reloaded in the browser.</span></span> <span data-ttu-id="bbe89-172">Der alternative Ansatz ist die Webpaketdatei manuell über das Projekt Npm Buildskript aufrufen, wenn eine Drittanbieter Abhängigkeit oder des benutzerdefinierten Codes ändert.</span><span class="sxs-lookup"><span data-stu-id="bbe89-172">The alternate approach is to manually invoke Webpack via the project's npm build script when a third-party dependency or the custom code changes.</span></span> <span data-ttu-id="bbe89-173">Ein Npm Skript erstellen, der *"Package.JSON"* Datei wird im folgenden Beispiel gezeigt:</span><span class="sxs-lookup"><span data-stu-id="bbe89-173">An npm build script in the *package.json* file is shown in the following example:</span></span>

[!code-json[](../client-side/spa-services/sample/SpaServicesSampleApp/package.json?range=5)]

### <a name="prerequisites"></a><span data-ttu-id="bbe89-174">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="bbe89-174">Prerequisites</span></span>

<span data-ttu-id="bbe89-175">Installieren Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="bbe89-175">Install the following:</span></span>
* <span data-ttu-id="bbe89-176">[ASPNET-Webpaketdatei](https://www.npmjs.com/package/aspnet-webpack) Npm-Paket:</span><span class="sxs-lookup"><span data-stu-id="bbe89-176">[aspnet-webpack](https://www.npmjs.com/package/aspnet-webpack) npm package:</span></span>

    ```console
    npm i -D aspnet-webpack
    ```

### <a name="configuration"></a><span data-ttu-id="bbe89-177">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="bbe89-177">Configuration</span></span>

<span data-ttu-id="bbe89-178">Webpaketdatei Dev Middleware registriert ist, in der HTTP-Anforderungspipeline über den folgenden Code in die *Startup.cs* Datei `Configure` Methode:</span><span class="sxs-lookup"><span data-stu-id="bbe89-178">Webpack Dev Middleware is registered into the HTTP request pipeline via the following code in the *Startup.cs* file's `Configure` method:</span></span>

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=webpack-middleware-registration&highlight=4)]

<span data-ttu-id="bbe89-179">Die `UseWebpackDevMiddleware` Erweiterungsmethode aufgerufen werden muss, bevor [registrieren, statische Datei hosting](xref:fundamentals/static-files) über die `UseStaticFiles` Erweiterungsmethode.</span><span class="sxs-lookup"><span data-stu-id="bbe89-179">The `UseWebpackDevMiddleware` extension method must be called before [registering static file hosting](xref:fundamentals/static-files) via the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="bbe89-180">Registrieren Sie die Middleware aus Gründen der Sicherheit nur, wenn die app in den Entwicklungsmodus ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="bbe89-180">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="bbe89-181">Die *webpack.config.js* Datei `output.publicPath` -Eigenschaft teilt die Middleware zum Überwachen der `dist` Ordner Änderungen:</span><span class="sxs-lookup"><span data-stu-id="bbe89-181">The *webpack.config.js* file's `output.publicPath` property tells the middleware to watch the `dist` folder for changes:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,13-16)]

<a name="hot-module-replacement"></a>

## <a name="hot-module-replacement"></a><span data-ttu-id="bbe89-182">Ersetzen eines Moduls im laufenden Systembetrieb</span><span class="sxs-lookup"><span data-stu-id="bbe89-182">Hot Module Replacement</span></span>

<span data-ttu-id="bbe89-183">Denken Sie an der Webpaketdatei [Hot Austausch eines Controllermoduls](https://webpack.js.org/concepts/hot-module-replacement/) (HMR)-Funktion als Weiterentwicklung der [Webpaketdatei Dev Middleware](#webpack-dev-middleware).</span><span class="sxs-lookup"><span data-stu-id="bbe89-183">Think of Webpack's [Hot Module Replacement](https://webpack.js.org/concepts/hot-module-replacement/) (HMR) feature as an evolution of [Webpack Dev Middleware](#webpack-dev-middleware).</span></span> <span data-ttu-id="bbe89-184">HMR führt dieselben Vorteile, aber es weiter optimiert des entwicklungsworkflows durch automatisches Aktualisieren der Seiteninhalt nach dem Kompilieren der Änderungen.</span><span class="sxs-lookup"><span data-stu-id="bbe89-184">HMR introduces all the same benefits, but it further streamlines the development workflow by automatically updating page content after compiling the changes.</span></span> <span data-ttu-id="bbe89-185">Verwechseln Sie dies mit einer Aktualisierung des Browsers, die mit dem aktuellen im Speicher enthaltenen Status und die Debugsitzung von der SPA beeinträchtigen würde.</span><span class="sxs-lookup"><span data-stu-id="bbe89-185">Don't confuse this with a refresh of the browser, which would interfere with the current in-memory state and debugging session of the SPA.</span></span> <span data-ttu-id="bbe89-186">Es ist ein Livelink zwischen der Webpaketdatei Dev Middleware-Dienst und den Browser, was bedeutet, dass Änderungen an den Browser per Push übertragen werden.</span><span class="sxs-lookup"><span data-stu-id="bbe89-186">There's a live link between the Webpack Dev Middleware service and the browser, which means changes are pushed to the browser.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="bbe89-187">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="bbe89-187">Prerequisites</span></span>

<span data-ttu-id="bbe89-188">Installieren Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="bbe89-188">Install the following:</span></span>
* <span data-ttu-id="bbe89-189">[Webpaketdatei während des Betriebs Middleware](https://www.npmjs.com/package/webpack-hot-middleware) Npm-Paket:</span><span class="sxs-lookup"><span data-stu-id="bbe89-189">[webpack-hot-middleware](https://www.npmjs.com/package/webpack-hot-middleware) npm package:</span></span>

    ```console
    npm i -D webpack-hot-middleware
    ```

### <a name="configuration"></a><span data-ttu-id="bbe89-190">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="bbe89-190">Configuration</span></span>

<span data-ttu-id="bbe89-191">Die Komponente HMR muss registriert werden, in MVCs-HTTP-Anforderungspipeline in die `Configure` Methode:</span><span class="sxs-lookup"><span data-stu-id="bbe89-191">The HMR component must be registered into MVC's HTTP request pipeline in the `Configure` method:</span></span>

```csharp
app.UseWebpackDevMiddleware(new WebpackDevMiddlewareOptions {
    HotModuleReplacement = true
});
```

<span data-ttu-id="bbe89-192">Wie wurde mit "true" [Webpaketdatei Dev Middleware](#webpack-dev-middleware), `UseWebpackDevMiddleware` Erweiterungsmethode aufgerufen werden muss, bevor die `UseStaticFiles` Erweiterungsmethode.</span><span class="sxs-lookup"><span data-stu-id="bbe89-192">As was true with [Webpack Dev Middleware](#webpack-dev-middleware), the `UseWebpackDevMiddleware` extension method must be called before the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="bbe89-193">Registrieren Sie die Middleware aus Gründen der Sicherheit nur, wenn die app in den Entwicklungsmodus ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="bbe89-193">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="bbe89-194">Die *webpack.config.js* Datei definieren, muss ein `plugins` array, auch wenn es leer gelassen wird:</span><span class="sxs-lookup"><span data-stu-id="bbe89-194">The *webpack.config.js* file must define a `plugins` array, even if it's left empty:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,25)]

<span data-ttu-id="bbe89-195">Nach dem Laden der app im Browser, bietet die Entwicklertools Konsole Registerkarte Bestätigung des HMR-Aktivierung:</span><span class="sxs-lookup"><span data-stu-id="bbe89-195">After loading the app in the browser, the developer tools' Console tab provides confirmation of HMR activation:</span></span>

![Im laufenden Systembetrieb Austausch eines Controllermoduls verbindungsmeldung](spa-services/_static/hmr_connected.png)

<a name="routing-helpers"></a>

## <a name="routing-helpers"></a><span data-ttu-id="bbe89-197">Routing-Hilfsprogramme</span><span class="sxs-lookup"><span data-stu-id="bbe89-197">Routing helpers</span></span>

<span data-ttu-id="bbe89-198">In den meisten ASP.NET Core-basierte SPAs sollten Sie clientseitige neben dem routing serverseitige routing.</span><span class="sxs-lookup"><span data-stu-id="bbe89-198">In most ASP.NET Core-based SPAs, you'll want client-side routing in addition to server-side routing.</span></span> <span data-ttu-id="bbe89-199">Die SPA und MVC-routing-Systeme können ohne Störung möglich sind unabhängig voneinander arbeiten.</span><span class="sxs-lookup"><span data-stu-id="bbe89-199">The SPA and MVC routing systems can work independently without interference.</span></span> <span data-ttu-id="bbe89-200">Vorhanden ist, jedoch eine Kante Groß-/Kleinschreibung machen Herausforderungen: 404 HTTP-Antworten zu identifizieren.</span><span class="sxs-lookup"><span data-stu-id="bbe89-200">There's, however, one edge case posing challenges: identifying 404 HTTP responses.</span></span>

<span data-ttu-id="bbe89-201">Betrachten Sie das Szenario, in dem ein Standarddokument Route des `/some/page` verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="bbe89-201">Consider the scenario in which an extensionless route of `/some/page` is used.</span></span> <span data-ttu-id="bbe89-202">Angenommen Sie, die Anforderung nicht-Musterübereinstimmung eine serverseitige Route, aber die entsprechende Muster entspricht einer Route für die clientseitige.</span><span class="sxs-lookup"><span data-stu-id="bbe89-202">Assume the request doesn't pattern-match a server-side route, but its pattern does match a client-side route.</span></span> <span data-ttu-id="bbe89-203">Betrachten Sie nun eine eingehende Anforderung für `/images/user-512.png`, die im Allgemeinen davon ausgeht, eine Bilddatei auf dem Server.</span><span class="sxs-lookup"><span data-stu-id="bbe89-203">Now consider an incoming request for `/images/user-512.png`, which generally expects to find an image file on the server.</span></span> <span data-ttu-id="bbe89-204">Wenn diesem Pfad für die angeforderte Ressource keine serverseitigen Route oder eine statische Datei übereinstimmt, ist es unwahrscheinlich, dass die clientseitige Anwendung behandelt werden würde, in der Regel einen 404 HTTP-Statuscode zurückgegeben werden soll.</span><span class="sxs-lookup"><span data-stu-id="bbe89-204">If that requested resource path doesn't match any server-side route or static file, it's unlikely that the client-side application would handle it — you generally want to return a 404 HTTP status code.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="bbe89-205">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="bbe89-205">Prerequisites</span></span>

<span data-ttu-id="bbe89-206">Installieren Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="bbe89-206">Install the following:</span></span>
* <span data-ttu-id="bbe89-207">Das routing Npm-Paket für die clientseitige.</span><span class="sxs-lookup"><span data-stu-id="bbe89-207">The client-side routing npm package.</span></span> <span data-ttu-id="bbe89-208">Verwenden Sie Angular als Beispiel:</span><span class="sxs-lookup"><span data-stu-id="bbe89-208">Using Angular as an example:</span></span>

    ```console
    npm i -S @angular/router
    ```

### <a name="configuration"></a><span data-ttu-id="bbe89-209">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="bbe89-209">Configuration</span></span>

<span data-ttu-id="bbe89-210">Eine Erweiterungsmethode mit dem Namen `MapSpaFallbackRoute` werden in der `Configure` Methode:</span><span class="sxs-lookup"><span data-stu-id="bbe89-210">An extension method named `MapSpaFallbackRoute` is used in the `Configure` method:</span></span>

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=mvc-routing-table&highlight=7-9)]

<span data-ttu-id="bbe89-211">Tipp: Routen werden in der Reihenfolge ausgewertet, in denen sie konfiguriert sind.</span><span class="sxs-lookup"><span data-stu-id="bbe89-211">Tip: Routes are evaluated in the order in which they're configured.</span></span> <span data-ttu-id="bbe89-212">Folglich die `default` Route im vorangehenden Codebeispiel wird zuerst für den Mustervergleich verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="bbe89-212">Consequently, the `default` route in the preceding code example is used first for pattern matching.</span></span>

<a name="new-project-creation"></a>

## <a name="creating-a-new-project"></a><span data-ttu-id="bbe89-213">Erstellen eines neuen Projekts</span><span class="sxs-lookup"><span data-stu-id="bbe89-213">Creating a new project</span></span>

<span data-ttu-id="bbe89-214">JavaScriptServices bietet vorkonfiguriert, dass Anwendungsvorlagen.</span><span class="sxs-lookup"><span data-stu-id="bbe89-214">JavaScriptServices provides pre-configured application templates.</span></span> <span data-ttu-id="bbe89-215">SpaServices wird in dieser Vorlagen in Verbindung mit anderen Frameworks und Bibliotheken, wie z. B. Angular reagieren und Redux verwendet.</span><span class="sxs-lookup"><span data-stu-id="bbe89-215">SpaServices is used in these templates, in conjunction with different frameworks and libraries such as Angular, React, and Redux.</span></span>

<span data-ttu-id="bbe89-216">Diese Vorlagen können über die .NET Core-CLI installiert werden, durch den folgenden Befehl ausführen:</span><span class="sxs-lookup"><span data-stu-id="bbe89-216">These templates can be installed via the .NET Core CLI by running the following command:</span></span>

```console
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

<span data-ttu-id="bbe89-217">Eine Liste der verfügbaren SPA-Vorlagen wird angezeigt:</span><span class="sxs-lookup"><span data-stu-id="bbe89-217">A list of available SPA templates is displayed:</span></span>

| <span data-ttu-id="bbe89-218">Vorlagen</span><span class="sxs-lookup"><span data-stu-id="bbe89-218">Templates</span></span>                                 | <span data-ttu-id="bbe89-219">Kurzname</span><span class="sxs-lookup"><span data-stu-id="bbe89-219">Short Name</span></span> | <span data-ttu-id="bbe89-220">Sprache</span><span class="sxs-lookup"><span data-stu-id="bbe89-220">Language</span></span> | <span data-ttu-id="bbe89-221">Tags</span><span class="sxs-lookup"><span data-stu-id="bbe89-221">Tags</span></span>        |
|:------------------------------------------|:-----------|:---------|:------------|
| <span data-ttu-id="bbe89-222">MVC ASP.NET Core mit Angular</span><span class="sxs-lookup"><span data-stu-id="bbe89-222">MVC ASP.NET Core with Angular</span></span>             | <span data-ttu-id="bbe89-223">angular</span><span class="sxs-lookup"><span data-stu-id="bbe89-223">angular</span></span>    | <span data-ttu-id="bbe89-224">[C#]</span><span class="sxs-lookup"><span data-stu-id="bbe89-224">[C#]</span></span>     | <span data-ttu-id="bbe89-225">MVC/Web/SPA</span><span class="sxs-lookup"><span data-stu-id="bbe89-225">Web/MVC/SPA</span></span> |
| <span data-ttu-id="bbe89-226">MVC ASP.NET Core mit React.js</span><span class="sxs-lookup"><span data-stu-id="bbe89-226">MVC ASP.NET Core with React.js</span></span>            | <span data-ttu-id="bbe89-227">react</span><span class="sxs-lookup"><span data-stu-id="bbe89-227">react</span></span>      | <span data-ttu-id="bbe89-228">[C#]</span><span class="sxs-lookup"><span data-stu-id="bbe89-228">[C#]</span></span>     | <span data-ttu-id="bbe89-229">MVC/Web/SPA</span><span class="sxs-lookup"><span data-stu-id="bbe89-229">Web/MVC/SPA</span></span> |
| <span data-ttu-id="bbe89-230">MVC ASP.NET Core mit React.js und Redux</span><span class="sxs-lookup"><span data-stu-id="bbe89-230">MVC ASP.NET Core with React.js and Redux</span></span>  | <span data-ttu-id="bbe89-231">reactredux</span><span class="sxs-lookup"><span data-stu-id="bbe89-231">reactredux</span></span> | <span data-ttu-id="bbe89-232">[C#]</span><span class="sxs-lookup"><span data-stu-id="bbe89-232">[C#]</span></span>     | <span data-ttu-id="bbe89-233">MVC/Web/SPA</span><span class="sxs-lookup"><span data-stu-id="bbe89-233">Web/MVC/SPA</span></span> |

<span data-ttu-id="bbe89-234">Zum Erstellen eines neuen Projekts mithilfe einer der SPA-Vorlagen enthalten die **Kurzname** der Vorlage in der [Dotnet neue](/dotnet/core/tools/dotnet-new) Befehl.</span><span class="sxs-lookup"><span data-stu-id="bbe89-234">To create a new project using one of the SPA templates, include the **Short Name** of the template in the [dotnet new](/dotnet/core/tools/dotnet-new) command.</span></span> <span data-ttu-id="bbe89-235">Der folgende Befehl erstellt eine Angular-Anwendung mit ASP.NET Core MVC für die Serverseite konfiguriert:</span><span class="sxs-lookup"><span data-stu-id="bbe89-235">The following command creates an Angular application with ASP.NET Core MVC configured for the server side:</span></span>

```console
dotnet new angular
```

<a name="runtime-config-mode"></a>

### <a name="set-the-runtime-configuration-mode"></a><span data-ttu-id="bbe89-236">Legen Sie den Modus der Common Language Runtime-Konfiguration</span><span class="sxs-lookup"><span data-stu-id="bbe89-236">Set the runtime configuration mode</span></span>

<span data-ttu-id="bbe89-237">Zwei primäre Runtime Konfigurationsmodi vorhanden sind:</span><span class="sxs-lookup"><span data-stu-id="bbe89-237">Two primary runtime configuration modes exist:</span></span>
* <span data-ttu-id="bbe89-238">**Entwicklung**:</span><span class="sxs-lookup"><span data-stu-id="bbe89-238">**Development**:</span></span>
    * <span data-ttu-id="bbe89-239">Enthält Zuordnungen von Quelle zur Erleichterung des Debuggings.</span><span class="sxs-lookup"><span data-stu-id="bbe89-239">Includes source maps to ease debugging.</span></span>
    * <span data-ttu-id="bbe89-240">Den clientseitige Code für die Leistung nicht optimiert werden.</span><span class="sxs-lookup"><span data-stu-id="bbe89-240">Doesn't optimize the client-side code for performance.</span></span>
* <span data-ttu-id="bbe89-241">**Produktion**:</span><span class="sxs-lookup"><span data-stu-id="bbe89-241">**Production**:</span></span>
    * <span data-ttu-id="bbe89-242">Schließt Quelle Karten an.</span><span class="sxs-lookup"><span data-stu-id="bbe89-242">Excludes source maps.</span></span>
    * <span data-ttu-id="bbe89-243">Optimiert die clientseitigen Code über Bündelung und Minimierung.</span><span class="sxs-lookup"><span data-stu-id="bbe89-243">Optimizes the client-side code via bundling & minification.</span></span>

<span data-ttu-id="bbe89-244">ASP.NET Core verwendet eine Umgebungsvariable namens `ASPNETCORE_ENVIRONMENT` zum Speichern des Konfigurationsmodus.</span><span class="sxs-lookup"><span data-stu-id="bbe89-244">ASP.NET Core uses an environment variable named `ASPNETCORE_ENVIRONMENT` to store the configuration mode.</span></span> <span data-ttu-id="bbe89-245">Finden Sie unter **[Einrichten der Umgebung](xref:fundamentals/environments#setting-the-environment)** für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="bbe89-245">See **[Setting the environment](xref:fundamentals/environments#setting-the-environment)** for more information.</span></span>

### <a name="running-with-net-core-cli"></a><span data-ttu-id="bbe89-246">Ausführen von mit .NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="bbe89-246">Running with .NET Core CLI</span></span>

<span data-ttu-id="bbe89-247">Stellen Sie die erforderlichen NuGet und Npm-Pakete durch den folgenden Befehl ausführen, auf der Stammebene des Projekts wieder her:</span><span class="sxs-lookup"><span data-stu-id="bbe89-247">Restore the required NuGet and npm packages by running the following command at the project root:</span></span>

```console
dotnet restore && npm i
```

<span data-ttu-id="bbe89-248">Erstellen und Ausführen der Anwendung:</span><span class="sxs-lookup"><span data-stu-id="bbe89-248">Build and run the application:</span></span>

```console
dotnet run
```

<span data-ttu-id="bbe89-249">Starten der Anwendung auf "localhost" gemäß der [Common Language Runtime-Konfigurationsmodus](#runtime-config-mode).</span><span class="sxs-lookup"><span data-stu-id="bbe89-249">The application starts on localhost according to the [runtime configuration mode](#runtime-config-mode).</span></span> <span data-ttu-id="bbe89-250">Navigieren zu `http://localhost:5000` die Startseite im Browser angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="bbe89-250">Navigating to `http://localhost:5000` in the browser displays the landing page.</span></span>

### <a name="running-with-visual-studio-2017"></a><span data-ttu-id="bbe89-251">Mit Visual Studio 2017 ausführen</span><span class="sxs-lookup"><span data-stu-id="bbe89-251">Running with Visual Studio 2017</span></span>

<span data-ttu-id="bbe89-252">Öffnen der *csproj* von generierten Datei die [Dotnet neue](/dotnet/core/tools/dotnet-new) Befehl.</span><span class="sxs-lookup"><span data-stu-id="bbe89-252">Open the *.csproj* file generated by the [dotnet new](/dotnet/core/tools/dotnet-new) command.</span></span> <span data-ttu-id="bbe89-253">Die erforderlichen NuGet und Npm-Pakete werden auf Projekt öffnen automatisch wiederhergestellt.</span><span class="sxs-lookup"><span data-stu-id="bbe89-253">The required NuGet and npm packages are restored automatically upon project open.</span></span> <span data-ttu-id="bbe89-254">Diese Wiederherstellung kann einige Minuten dauern, und die Anwendung ist bereit, die ausgeführt werden, wenn er abgeschlossen wurde.</span><span class="sxs-lookup"><span data-stu-id="bbe89-254">This restoration process may take up to a few minutes, and the application is ready to run when it completes.</span></span> <span data-ttu-id="bbe89-255">Klicken Sie auf die grüne Schaltfläche ausführen, oder drücken Sie `Ctrl + F5`, und der Browser geöffnet wird, zur Startseite der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="bbe89-255">Click the green run button or press `Ctrl + F5`, and the browser opens to the application's landing page.</span></span> <span data-ttu-id="bbe89-256">Die Anwendung ausgeführt wird, auf "localhost" gemäß der [Common Language Runtime-Konfigurationsmodus](#runtime-config-mode).</span><span class="sxs-lookup"><span data-stu-id="bbe89-256">The application runs on localhost according to the [runtime configuration mode](#runtime-config-mode).</span></span> 

<a name="app-testing"></a>

## <a name="testing-the-app"></a><span data-ttu-id="bbe89-257">Testen der app</span><span class="sxs-lookup"><span data-stu-id="bbe89-257">Testing the app</span></span>

<span data-ttu-id="bbe89-258">SpaServices Vorlagen sind für die clientseitige Tests mithilfe von vorkonfiguriert [Karma](https://karma-runner.github.io/1.0/index.html) und [Jasmine](https://jasmine.github.io/).</span><span class="sxs-lookup"><span data-stu-id="bbe89-258">SpaServices templates are pre-configured to run client-side tests using [Karma](https://karma-runner.github.io/1.0/index.html) and [Jasmine](https://jasmine.github.io/).</span></span> <span data-ttu-id="bbe89-259">Jasmine ist eine beliebte UnitTest-Framework für JavaScript, während der Karma ein Testprogramm für diese Tests ist.</span><span class="sxs-lookup"><span data-stu-id="bbe89-259">Jasmine is a popular unit testing framework for JavaScript, whereas Karma is a test runner for those tests.</span></span> <span data-ttu-id="bbe89-260">Karma ist so konfiguriert, dass die Bearbeitung der [Webpaketdatei Dev Middleware](#webpack-dev-middleware) , dass der Entwickler ist nicht erforderlich sind, beenden, und führen Sie den Test, jedes Mal, wenn Änderungen vorgenommen werden.</span><span class="sxs-lookup"><span data-stu-id="bbe89-260">Karma is configured to work with the [Webpack Dev Middleware](#webpack-dev-middleware) such that the developer isn't required to stop and run the test every time changes are made.</span></span> <span data-ttu-id="bbe89-261">Ob er den Code für den Testfall oder den Testfall selbst ausgeführt wird, wird der Test automatisch ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="bbe89-261">Whether it's the code running against the test case or the test case itself, the test runs automatically.</span></span>

<span data-ttu-id="bbe89-262">Verwendung der Angular-Anwendung als Beispiel zwei Jasmintee Testfälle sind bereits deckte die `CounterComponent` in der *counter.component.spec.ts* Datei:</span><span class="sxs-lookup"><span data-stu-id="bbe89-262">Using the Angular application as an example, two Jasmine test cases are already provided for the `CounterComponent` in the *counter.component.spec.ts* file:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/app/components/counter/counter.component.spec.ts?range=15-28)]

<span data-ttu-id="bbe89-263">Öffnen Sie die Eingabeaufforderung in das *ClientApp* Verzeichnis.</span><span class="sxs-lookup"><span data-stu-id="bbe89-263">Open the command prompt in the *ClientApp* directory.</span></span> <span data-ttu-id="bbe89-264">Führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="bbe89-264">Run the following command:</span></span>

```console
npm test
```

<span data-ttu-id="bbe89-265">Startet das Skript die Karma-Testprogramm, die in definierten Einstellungen liest die *karma.conf.js* Datei.</span><span class="sxs-lookup"><span data-stu-id="bbe89-265">The script launches the Karma test runner, which reads the settings defined in the *karma.conf.js* file.</span></span> <span data-ttu-id="bbe89-266">Neben anderen Einstellungen der *karma.conf.js* identifiziert die Testdateien auszuführenden über seine `files` Array:</span><span class="sxs-lookup"><span data-stu-id="bbe89-266">Among other settings, the *karma.conf.js* identifies the test files to be executed via its `files` array:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/test/karma.conf.js?range=4-5,8-11)]

<a name="app-publishing"></a>

## <a name="publishing-the-application"></a><span data-ttu-id="bbe89-267">Veröffentlichen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="bbe89-267">Publishing the application</span></span>

<span data-ttu-id="bbe89-268">Kombinieren die generierte clientseitige Bestand und der veröffentlichten ASP.NET Core-Artefakte in ein Paket kann jetzt bereitstellen kann sehr aufwändig sein.</span><span class="sxs-lookup"><span data-stu-id="bbe89-268">Combining the generated client-side assets and the published ASP.NET Core artifacts into a ready-to-deploy package can be cumbersome.</span></span> <span data-ttu-id="bbe89-269">Glücklicherweise orchestriert SpaServices dieses Prozesses für die gesamte Veröffentlichung mit einem benutzerdefinierten MSBuild-Ziel mit dem Namen `RunWebpack`:</span><span class="sxs-lookup"><span data-stu-id="bbe89-269">Thankfully, SpaServices orchestrates that entire publication process with a custom MSBuild target named `RunWebpack`:</span></span>

[!code-xml[](../client-side/spa-services/sample/SpaServicesSampleApp/SpaServicesSampleApp.csproj?range=31-45)]

<span data-ttu-id="bbe89-270">Das MSBuild-Ziel hat die folgenden Verantwortungen:</span><span class="sxs-lookup"><span data-stu-id="bbe89-270">The MSBuild target has the following responsibilities:</span></span>
1. <span data-ttu-id="bbe89-271">Wiederherstellen der Npm-Pakete</span><span class="sxs-lookup"><span data-stu-id="bbe89-271">Restore the npm packages</span></span>
1. <span data-ttu-id="bbe89-272">Erstellen eines Builds Produktions-Grade der Ressourcen von Drittanbietern, die clientseitige</span><span class="sxs-lookup"><span data-stu-id="bbe89-272">Create a production-grade build of the third-party, client-side assets</span></span>
1. <span data-ttu-id="bbe89-273">Erstellen eines Builds Produktions-Grade benutzerdefinierte clientseitige Objekte</span><span class="sxs-lookup"><span data-stu-id="bbe89-273">Create a production-grade build of the custom client-side assets</span></span>
1. <span data-ttu-id="bbe89-274">Kopieren Sie die Webpaketdatei generierte Objekte in dem Veröffentlichungsordner</span><span class="sxs-lookup"><span data-stu-id="bbe89-274">Copy the Webpack-generated assets to the publish folder</span></span>

<span data-ttu-id="bbe89-275">Das MSBuild-Ziel wird aufgerufen, wenn ausgeführt:</span><span class="sxs-lookup"><span data-stu-id="bbe89-275">The MSBuild target is invoked when running:</span></span>

```console
dotnet publish -c Release
```

## <a name="additional-resources"></a><span data-ttu-id="bbe89-276">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="bbe89-276">Additional resources</span></span>

* [<span data-ttu-id="bbe89-277">Angular Docs</span><span class="sxs-lookup"><span data-stu-id="bbe89-277">Angular Docs</span></span>](https://angular.io/docs)
