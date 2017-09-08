---
title: Migrieren von ASP.NET Web-API
author: ardalis
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 4f0564b4-ed4e-4e1e-9755-c1144d21a0ef
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/webapi
ms.openlocfilehash: 55125e711a8b04f5a363ba965ab2223da02aab78
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2017
---
# <a name="migrating-from-aspnet-web-api"></a><span data-ttu-id="ff752-103">Migrieren von ASP.NET Web-API</span><span class="sxs-lookup"><span data-stu-id="ff752-103">Migrating from ASP.NET Web API</span></span>

<span data-ttu-id="ff752-104">Durch [Steve Smith](http://ardalis.com) und [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="ff752-104">By [Steve Smith](http://ardalis.com) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="ff752-105">Web-APIs sind HTTP-Diensten, die eine Breite Palette von Clients, einschließlich Browsern und mobilen Geräten erreichen.</span><span class="sxs-lookup"><span data-stu-id="ff752-105">Web APIs are HTTP services that reach a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="ff752-106">ASP.NET Core MVC umfasst Unterstützung für das Erstellen von Web-APIs, die einen einzelnen, konsistenten lässt sich das Erstellen von Webanwendungen.</span><span class="sxs-lookup"><span data-stu-id="ff752-106">ASP.NET Core MVC includes support for building Web APIs providing a single, consistent way of building web applications.</span></span> <span data-ttu-id="ff752-107">In diesem Artikel veranschaulichen wir die erforderlichen Schritte zum Migrieren von einer Web-API-Implementierung von ASP.NET Web-API zu ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="ff752-107">In this article, we demonstrate the steps required to migrate a Web API implementation from ASP.NET Web API to ASP.NET Core MVC.</span></span>

[<span data-ttu-id="ff752-108">Anzeigen oder Herunterladen von Beispielcode</span><span class="sxs-lookup"><span data-stu-id="ff752-108">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample)

## <a name="review-aspnet-web-api-project"></a><span data-ttu-id="ff752-109">Überprüfen Sie ASP.NET Web-API-Projekt</span><span class="sxs-lookup"><span data-stu-id="ff752-109">Review ASP.NET Web API Project</span></span>

<span data-ttu-id="ff752-110">In diesem Artikel verwendet das Beispielprojekt *ProductsApp*, in dem Artikel erstellte [erste Schritte mit ASP.NET Web API](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) als Ausgangspunkt erforderlich.</span><span class="sxs-lookup"><span data-stu-id="ff752-110">This article uses the sample project, *ProductsApp*, created in the article [Getting Started with ASP.NET Web API](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) as its starting point.</span></span> <span data-ttu-id="ff752-111">In diesem Projekt wird ein einfache ASP.NET Web-API-Projekt wie folgt konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="ff752-111">In that project, a simple ASP.NET Web API  project is configured as follows.</span></span>

<span data-ttu-id="ff752-112">In *Global.asax.cs*, erfolgt ein Aufruf zum `WebApiConfig.Register`:</span><span class="sxs-lookup"><span data-stu-id="ff752-112">In *Global.asax.cs*, a call is made to `WebApiConfig.Register`:</span></span>

<span data-ttu-id="ff752-113">[!code-csharp[Main](../migration/webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]</span><span class="sxs-lookup"><span data-stu-id="ff752-113">[!code-csharp[Main](../migration/webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]</span></span>

<span data-ttu-id="ff752-114">`WebApiConfig`wird definiert, *App_Start*, und weist nur eine statische `Register` Methode:</span><span class="sxs-lookup"><span data-stu-id="ff752-114">`WebApiConfig` is defined in *App_Start*, and has just one static `Register` method:</span></span>

<span data-ttu-id="ff752-115">[!code-csharp[Main](../migration/webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15,16,17,18,19,20)]</span><span class="sxs-lookup"><span data-stu-id="ff752-115">[!code-csharp[Main](../migration/webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15,16,17,18,19,20)]</span></span>


<span data-ttu-id="ff752-116">Diese Klasse konfiguriert [routing-Attribut](http://www.asp.net/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), obwohl sie nicht tatsächlich im Projekt verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="ff752-116">This class configures [attribute routing](http://www.asp.net/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), although it's not actually being used in the project.</span></span> <span data-ttu-id="ff752-117">Außerdem konfiguriert die Routingtabelle der von ASP.NET Web-API verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="ff752-117">It also configures the routing table which is used by ASP.NET Web API.</span></span> <span data-ttu-id="ff752-118">In diesem Fall wird ASP.NET Web API URLs entsprechend das Format erwarten */api/ {Controller} / {Id}*, mit *{Id}* wird optional.</span><span class="sxs-lookup"><span data-stu-id="ff752-118">In this case, ASP.NET Web API will expect URLs to match the format */api/{controller}/{id}*, with *{id}* being optional.</span></span>

<span data-ttu-id="ff752-119">Die *ProductsApp* -Projekt enthält nur eine einfache Controller, der vom erbt `ApiController` und macht zwei Methoden verfügbar:</span><span class="sxs-lookup"><span data-stu-id="ff752-119">The *ProductsApp* project includes just one simple controller, which inherits from `ApiController` and exposes two methods:</span></span>

<span data-ttu-id="ff752-120">[!code-csharp[Main](../migration/webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]</span><span class="sxs-lookup"><span data-stu-id="ff752-120">[!code-csharp[Main](../migration/webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]</span></span>

<span data-ttu-id="ff752-121">Zum Schluss das Modell *Produkt*, verwendet der *ProductsApp*, ist eine einfache Klasse:</span><span class="sxs-lookup"><span data-stu-id="ff752-121">Finally, the model, *Product*, used by the *ProductsApp*, is a simple class:</span></span>

<span data-ttu-id="ff752-122">[!code-csharp[Main](webapi/sample/ProductsApp/Models/Product.cs)]</span><span class="sxs-lookup"><span data-stu-id="ff752-122">[!code-csharp[Main](webapi/sample/ProductsApp/Models/Product.cs)]</span></span>

<span data-ttu-id="ff752-123">Nun, da wir ein einfaches Projekt aus dem gestartet haben, können wir diese Web-API-Projekts zu ASP.NET Core MVC migrieren veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="ff752-123">Now that we have a simple project from which to start, we can demonstrate how to migrate this Web API project to ASP.NET Core MVC.</span></span>

## <a name="create-the-destination-project"></a><span data-ttu-id="ff752-124">Erstellen Sie das Zielprojekt</span><span class="sxs-lookup"><span data-stu-id="ff752-124">Create the Destination Project</span></span>

<span data-ttu-id="ff752-125">Erstellen Sie eine neue, leere Projektmappe mithilfe von Visual Studio, und nennen Sie sie *WebAPIMigration*.</span><span class="sxs-lookup"><span data-stu-id="ff752-125">Using Visual Studio, create a new, empty solution, and name it *WebAPIMigration*.</span></span> <span data-ttu-id="ff752-126">Fügen Sie der vorhandenen *ProductsApp* Projekt, und dann eine neue ASP.NET Core Webanwendungsprojekt zur Projektmappe hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="ff752-126">Add the existing *ProductsApp* project to it, then, add a new ASP.NET Core Web Application Project to the solution.</span></span> <span data-ttu-id="ff752-127">Nennen Sie das neue Projekt *ProductsCore*.</span><span class="sxs-lookup"><span data-stu-id="ff752-127">Name the new project *ProductsCore*.</span></span>

![Dialogfeld "Neues Projekt" öffnen, um Webvorlagen](webapi/_static/add-web-project.png)

<span data-ttu-id="ff752-129">Wählen Sie anschließend die Web-API-Projektvorlage aus.</span><span class="sxs-lookup"><span data-stu-id="ff752-129">Next, choose the Web API project template.</span></span> <span data-ttu-id="ff752-130">Wir migrieren der *ProductsApp* Inhalt in dieses neue Projekt.</span><span class="sxs-lookup"><span data-stu-id="ff752-130">We will migrate the *ProductsApp* contents to this new project.</span></span>

![Dialogfeld "neues Web Application" mit Web-API-Projektvorlage, die in der Liste der Vorlagen ASP.NET Core ausgewählt](webapi/_static/aspnet-5-webapi.png)

<span data-ttu-id="ff752-132">Löschen der `Project_Readme.html` Datei aus dem neuen Projekt.</span><span class="sxs-lookup"><span data-stu-id="ff752-132">Delete the `Project_Readme.html` file from the new project.</span></span> <span data-ttu-id="ff752-133">Die Projektmappe sollte jetzt wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="ff752-133">Your solution should now look like this:</span></span>

![Öffnen Sie im Projektmappen-Explorer mit Dateien und Ordner der Projekte ProductsApp und ProductsCore webanwendungslösung](webapi/_static/webapimigration-solution.png)

## <a name="migrate-configuration"></a><span data-ttu-id="ff752-135">Migrieren der Konfiguration</span><span class="sxs-lookup"><span data-stu-id="ff752-135">Migrate Configuration</span></span>

<span data-ttu-id="ff752-136">Nicht mehr mithilfe von ASP.NET Core *"Global.asax"*, *"Web.config"*, oder *App_Start* Ordner.</span><span class="sxs-lookup"><span data-stu-id="ff752-136">ASP.NET Core no longer uses *Global.asax*, *web.config*, or *App_Start* folders.</span></span> <span data-ttu-id="ff752-137">Alle starttasks erfolgen dagegen *Startup.cs* im Stammverzeichnis des Projekts (finden Sie unter [Anwendungsstart](../fundamentals/startup.md)).</span><span class="sxs-lookup"><span data-stu-id="ff752-137">Instead, all startup tasks are done in *Startup.cs* in the root of the project (see [Application Startup](../fundamentals/startup.md)).</span></span> <span data-ttu-id="ff752-138">In ASP.NET-MVC Core attributbasierte routing ist jetzt standardmäßig enthalten, wenn `UseMvc()` aufgerufen wird; dies ist die empfohlene Vorgehensweise zum Konfigurieren von Routen für Web-API (und ist wie das Startprojekt für die Web-API behandelt routing).</span><span class="sxs-lookup"><span data-stu-id="ff752-138">In ASP.NET Core MVC, attribute-based routing is now included by default when `UseMvc()` is called; and, this is the recommended approach for configuring Web API routes (and is how the Web API starter project handles routing).</span></span>

<span data-ttu-id="ff752-139">[!code-none[Main](../migration/webapi/sample/ProductsCore/Startup.cs?highlight=40)]</span><span class="sxs-lookup"><span data-stu-id="ff752-139">[!code-none[Main](../migration/webapi/sample/ProductsCore/Startup.cs?highlight=40)]</span></span>

<span data-ttu-id="ff752-140">Angenommen, Sie in Ihrem Projekt vorwärts routing-Attribut verwenden möchten, ist keine zusätzliche Konfiguration erforderlich.</span><span class="sxs-lookup"><span data-stu-id="ff752-140">Assuming you want to use attribute routing in your project going forward, no additional configuration is needed.</span></span> <span data-ttu-id="ff752-141">Wenden Sie die Attribute nach Bedarf auf Ihre Controller und Aktionen, wie im Beispiel erfolgt einfach `ValuesController` -Klasse, die in das Startprojekt für die Web-API enthalten ist:</span><span class="sxs-lookup"><span data-stu-id="ff752-141">Simply apply the attributes as needed to your controllers and actions, as is done in the sample `ValuesController` class that is included in the Web API starter project:</span></span>

<span data-ttu-id="ff752-142">[!code-csharp[Main](../migration/webapi/sample/ProductsCore/Controllers/ValuesController.cs?highlight=9,13,20,27,33,39)]</span><span class="sxs-lookup"><span data-stu-id="ff752-142">[!code-csharp[Main](../migration/webapi/sample/ProductsCore/Controllers/ValuesController.cs?highlight=9,13,20,27,33,39)]</span></span>

<span data-ttu-id="ff752-143">Beachten Sie das Vorhandensein von *[Controller]* in Zeile 8.</span><span class="sxs-lookup"><span data-stu-id="ff752-143">Note the presence of *[controller]* on line 8.</span></span> <span data-ttu-id="ff752-144">Attribut-basierter jetzt routing unterstützt bestimmte-Token, wie z. B. *[Controller]* und *[Aktion]*.</span><span class="sxs-lookup"><span data-stu-id="ff752-144">Attribute-based routing now supports certain tokens, such as *[controller]* and *[action]*.</span></span> <span data-ttu-id="ff752-145">Diese Token werden zur Laufzeit mit dem Namen des Controllers oder Aktion, bzw. ersetzt, der das Attribut angewendet wurde.</span><span class="sxs-lookup"><span data-stu-id="ff752-145">These tokens are replaced at runtime with the name of the controller or action, respectively, to which the attribute has been applied.</span></span> <span data-ttu-id="ff752-146">Dies dient zum Reduzieren der Anzahl der Magic-Zeichenfolgen im Projekt, und es wird sichergestellt, dass die Routen verbleiben synchronisiert mit ihren entsprechenden Controller und Aktionen wenn automatische Umbenennung Refactorings angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="ff752-146">This serves to reduce the number of magic strings in the project, and it ensures the routes will be kept synchronized with their corresponding controllers and actions when automatic rename refactorings are applied.</span></span>

<span data-ttu-id="ff752-147">Um die Produkte-API-Controller zu migrieren, müssen wir zuerst kopieren *ProductsController* in das neue Projekt.</span><span class="sxs-lookup"><span data-stu-id="ff752-147">To migrate the Products API controller, we must first copy *ProductsController* to the new project.</span></span> <span data-ttu-id="ff752-148">Klicken Sie dann einfach enthalten Sie das routenattribut auf dem Controller:</span><span class="sxs-lookup"><span data-stu-id="ff752-148">Then simply include the route attribute on the controller:</span></span>

```csharp
[Route("api/[controller]")]
```

<span data-ttu-id="ff752-149">Sie müssen auch hinzufügen der `[HttpGet]` -Attribut auf die beiden Methoden, da beide über HTTP Get aufgerufen werden soll.</span><span class="sxs-lookup"><span data-stu-id="ff752-149">You also need to add the `[HttpGet]` attribute to the two methods, since they both should be called via HTTP Get.</span></span> <span data-ttu-id="ff752-150">Der Erwartung, dass ein Parameter "Id" im Attribut enthalten `GetProduct()`:</span><span class="sxs-lookup"><span data-stu-id="ff752-150">Include the expectation of an "id" parameter in the attribute for `GetProduct()`:</span></span>

```csharp
// /api/products
[HttpGet]
...

// /api/products/1
[HttpGet("{id}")]
```

<span data-ttu-id="ff752-151">An diesem Punkt ist das routing richtig konfiguriert. jedoch können wir noch es nicht testen.</span><span class="sxs-lookup"><span data-stu-id="ff752-151">At this point, routing is configured correctly; however, we can't yet test it.</span></span> <span data-ttu-id="ff752-152">Müssen weitere Änderungen vorgenommen werden, bevor *ProductsController* kompiliert.</span><span class="sxs-lookup"><span data-stu-id="ff752-152">Additional changes must be made before *ProductsController* will compile.</span></span>

## <a name="migrate-models-and-controllers"></a><span data-ttu-id="ff752-153">Migrieren von Modellen und Controllern</span><span class="sxs-lookup"><span data-stu-id="ff752-153">Migrate Models and Controllers</span></span>

<span data-ttu-id="ff752-154">Der letzte Schritt des Migrationsvorgangs für dieses einfache Web-API-Projekt ist, kopieren Sie die Controller und alle Modelle, die sie verwenden.</span><span class="sxs-lookup"><span data-stu-id="ff752-154">The last step in the migration process for this simple Web API project is to copy over the Controllers and any Models they use.</span></span> <span data-ttu-id="ff752-155">In diesem Fall einfach kopieren *Controllers/ProductsController.cs* aus dem ursprünglichen Projekt in das neue Projekt.</span><span class="sxs-lookup"><span data-stu-id="ff752-155">In this case, simply copy *Controllers/ProductsController.cs* from the original project to the new one.</span></span> <span data-ttu-id="ff752-156">Kopieren Sie Sie dann den gesamten Ordner Models aus dem ursprünglichen Projekt in das neue Projekt.</span><span class="sxs-lookup"><span data-stu-id="ff752-156">Then, copy the entire Models folder from the original project to the new one.</span></span> <span data-ttu-id="ff752-157">Passen Sie die Namespaces, um den neuen Projektnamen angepasst (*ProductsCore*).</span><span class="sxs-lookup"><span data-stu-id="ff752-157">Adjust the namespaces to match the new project name (*ProductsCore*).</span></span>  <span data-ttu-id="ff752-158">An diesem Punkt können Sie die Anwendung erstellen, und finden Sie eine Anzahl von Kompilierungsfehlern.</span><span class="sxs-lookup"><span data-stu-id="ff752-158">At this point, you can build the application, and you will find a number of compilation errors.</span></span> <span data-ttu-id="ff752-159">Diese sollte im Allgemeinen in den folgenden Kategorien fallen:</span><span class="sxs-lookup"><span data-stu-id="ff752-159">These should generally fall into the following categories:</span></span>

* <span data-ttu-id="ff752-160">*ApiController* ist nicht vorhanden</span><span class="sxs-lookup"><span data-stu-id="ff752-160">*ApiController* does not exist</span></span>

* <span data-ttu-id="ff752-161">*System.Web.Http* Namespace ist nicht vorhanden.</span><span class="sxs-lookup"><span data-stu-id="ff752-161">*System.Web.Http* namespace does not exist</span></span>

* <span data-ttu-id="ff752-162">*IHttpActionResult* ist nicht vorhanden</span><span class="sxs-lookup"><span data-stu-id="ff752-162">*IHttpActionResult* does not exist</span></span>

<span data-ttu-id="ff752-163">Glücklicherweise sind dies so beheben alle sehr einfach:</span><span class="sxs-lookup"><span data-stu-id="ff752-163">Fortunately, these are all very easy to correct:</span></span>

* <span data-ttu-id="ff752-164">Änderung *ApiController* auf *Controller* (müssen Sie möglicherweise hinzufügen *mit Microsoft.AspNetCore.Mvc*)</span><span class="sxs-lookup"><span data-stu-id="ff752-164">Change *ApiController* to *Controller* (you may need to add *using Microsoft.AspNetCore.Mvc*)</span></span>

* <span data-ttu-id="ff752-165">Löschen Sie alle mit Verweisen auf Anweisung *System.Web.Http*</span><span class="sxs-lookup"><span data-stu-id="ff752-165">Delete any using statement referring to *System.Web.Http*</span></span>

* <span data-ttu-id="ff752-166">Ändern Sie eine beliebige Methode zurückgeben *IHttpActionResult* zurückzugebenden eine *IActionResult*</span><span class="sxs-lookup"><span data-stu-id="ff752-166">Change any method returning *IHttpActionResult* to return a *IActionResult*</span></span>

<span data-ttu-id="ff752-167">Nachdem diese Änderungen wurden vorgenommen und nicht verwendete using-Anweisungen entfernt, wird das migrierte *ProductsController* Klasse sieht wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="ff752-167">Once these changes have been made and unused using statements removed, the migrated *ProductsController* class looks like this:</span></span>

<span data-ttu-id="ff752-168">[!code-csharp[Main](../migration/webapi/sample/ProductsCore/Controllers/ProductsController.cs?highlight=1,2,6,8,9,27)]</span><span class="sxs-lookup"><span data-stu-id="ff752-168">[!code-csharp[Main](../migration/webapi/sample/ProductsCore/Controllers/ProductsController.cs?highlight=1,2,6,8,9,27)]</span></span>

<span data-ttu-id="ff752-169">Sie sollten jetzt möglich, führen Sie die migrierte Projekt, und navigieren Sie zu */api/Products*; und die vollständige Liste der 3 Produkte angezeigt.</span><span class="sxs-lookup"><span data-stu-id="ff752-169">You should now be able to run the migrated project and browse to */api/products*; and, you should see the full list of 3 products.</span></span> <span data-ttu-id="ff752-170">Navigieren Sie zu */api/products/1* und das erste Produkt sollte angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="ff752-170">Browse to */api/products/1* and you should see the first product.</span></span>

## <a name="summary"></a><span data-ttu-id="ff752-171">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="ff752-171">Summary</span></span>

<span data-ttu-id="ff752-172">Migrieren eines einfachen ASP.NET Web-API-Projekts zu ASP.NET Core MVC ist recht einfach, Dank an die integrierte Unterstützung für Web-APIs in ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="ff752-172">Migrating a simple ASP.NET Web API project to ASP.NET Core MVC is fairly straightforward, thanks to the built-in support for Web APIs in ASP.NET Core MVC.</span></span> <span data-ttu-id="ff752-173">Die Hauptteilen jedes ASP.NET Web-API-Projekt migrieren müssen werden Routen, Controllern und Modelle, zusammen mit Updates für die Typen von Controllern und Aktionen verwendet.</span><span class="sxs-lookup"><span data-stu-id="ff752-173">The main pieces every ASP.NET Web API project will need to migrate are routes, controllers, and models, along with updates to the types used by  controllers and actions.</span></span>
