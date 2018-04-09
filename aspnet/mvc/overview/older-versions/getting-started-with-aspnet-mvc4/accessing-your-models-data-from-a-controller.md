---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
title: Zugriff auf das Modell Daten aus einem Controller | Microsoft Docs
author: Rick-Anderson
description: 'Hinweis: Eine aktualisierte Version dieses Lernprogramms ist hier verfügbar, die ASP.NET MVC 5 und Visual Studio 2013 verwendet. Es ist sicherer, viel einfacher zu verfolgen und demo...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 61e0206d-7f32-4018-992d-0a51b48b37dc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: cf896a6a9ce6cb8cd4adb13c3d87c4e7c3095fa6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="accessing-your-models-data-from-a-controller"></a><span data-ttu-id="d1d87-104">Zugriff auf das Modell Daten aus einem Controller</span><span class="sxs-lookup"><span data-stu-id="d1d87-104">Accessing Your Model's Data from a Controller</span></span>
====================
<span data-ttu-id="d1d87-105">durch [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="d1d87-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="d1d87-106">Eine aktualisierte Version dieses Lernprogramms steht [hier](../../getting-started/introduction/getting-started.md) , ASP.NET MVC 5 und Visual Studio 2013 verwendet.</span><span class="sxs-lookup"><span data-stu-id="d1d87-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="d1d87-107">Es ist sicherer, viel einfacher, führen und weitere Funktionen veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="d1d87-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="d1d87-108">In diesem Abschnitt erstellen Sie ein neues `MoviesController` Klasse, und Schreiben von Code, ruft die Filmdaten ab und zeigt ihn im Browser mit einer Vorlage anzeigen.</span><span class="sxs-lookup"><span data-stu-id="d1d87-108">In this section, you'll create a new `MoviesController` class and write code that retrieves the movie data and displays it in the browser using a view template.</span></span>

<span data-ttu-id="d1d87-109">**Erstellen Sie die Anwendung** bevor Sie mit dem nächsten Schritt fortfahren.</span><span class="sxs-lookup"><span data-stu-id="d1d87-109">**Build the application** before going on to the next step.</span></span>

<span data-ttu-id="d1d87-110">Mit der rechten Maustaste die *Controller* Ordner und erstellen Sie ein neues `MoviesController` Controller.</span><span class="sxs-lookup"><span data-stu-id="d1d87-110">Right-click the *Controllers* folder and create a new `MoviesController` controller.</span></span> <span data-ttu-id="d1d87-111">Die folgenden Optionen werden nicht angezeigt, bis Sie eine Anwendung erstellen.</span><span class="sxs-lookup"><span data-stu-id="d1d87-111">The options below will not appear until you build your application.</span></span> <span data-ttu-id="d1d87-112">Wählen Sie die folgenden Optionen:</span><span class="sxs-lookup"><span data-stu-id="d1d87-112">Select the following options:</span></span>

- <span data-ttu-id="d1d87-113">Controllername: **MoviesController**.</span><span class="sxs-lookup"><span data-stu-id="d1d87-113">Controller name: **MoviesController**.</span></span> <span data-ttu-id="d1d87-114">(Dies ist die Standardeinstellung.</span><span class="sxs-lookup"><span data-stu-id="d1d87-114">(This is the default.</span></span> <span data-ttu-id="d1d87-115">)</span><span class="sxs-lookup"><span data-stu-id="d1d87-115">)</span></span>
- <span data-ttu-id="d1d87-116">Vorlage: **MVC-Controller mit Lese-/schreibaktionen und Ansichten, Verwendung von Entity Framework**.</span><span class="sxs-lookup"><span data-stu-id="d1d87-116">Template: **MVC Controller with read/write actions and views, using Entity Framework**.</span></span>
- <span data-ttu-id="d1d87-117">Modellschemas: **Film (MvcMovie.Models)**.</span><span class="sxs-lookup"><span data-stu-id="d1d87-117">Model class: **Movie (MvcMovie.Models)**.</span></span>
- <span data-ttu-id="d1d87-118">Datenkontextklasse: **MovieDBContext (MvcMovie.Models)**.</span><span class="sxs-lookup"><span data-stu-id="d1d87-118">Data context class: **MovieDBContext (MvcMovie.Models)**.</span></span>
- <span data-ttu-id="d1d87-119">Ansichten: **Razor (CSHTML)**.</span><span class="sxs-lookup"><span data-stu-id="d1d87-119">Views: **Razor (CSHTML)**.</span></span> <span data-ttu-id="d1d87-120">(Die Standardeinstellung.)</span><span class="sxs-lookup"><span data-stu-id="d1d87-120">(The default.)</span></span>

![AddScaffoldedMovieController](accessing-your-models-data-from-a-controller/_static/image1.png)

<span data-ttu-id="d1d87-122">Klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="d1d87-122">Click **Add**.</span></span> <span data-ttu-id="d1d87-123">Visual Studio Express erstellt die folgenden Dateien und Ordner:</span><span class="sxs-lookup"><span data-stu-id="d1d87-123">Visual Studio Express creates the following files and folders:</span></span>

- <span data-ttu-id="d1d87-124">*Eine MoviesController.cs* -Datei in des Projekts *Controller* Ordner.</span><span class="sxs-lookup"><span data-stu-id="d1d87-124">*A MoviesController.cs* file in the project's *Controllers* folder.</span></span>
- <span data-ttu-id="d1d87-125">Ein *Filme* Ordner des Projekts *Ansichten* Ordner.</span><span class="sxs-lookup"><span data-stu-id="d1d87-125">A *Movies* folder in the project's *Views* folder.</span></span>
- <span data-ttu-id="d1d87-126">*Create.cshtml Delete.cshtml, Details.cshtml, Edit.cshtml*, und *Index.cshtml* in der neuen *Views\Movies* Ordner.</span><span class="sxs-lookup"><span data-stu-id="d1d87-126">*Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, and *Index.cshtml* in the new *Views\Movies* folder.</span></span>

<span data-ttu-id="d1d87-127">ASP.NET MVC 4 automatisch erstellt, die CRUD-Vorgänge (erstellen, lesen, aktualisieren und löschen) Aktionsmethoden und Ansichten für Sie (als Gerüstbau wird die automatische Erstellung von CRUD-Aktionsmethoden und Ansichten bezeichnet).</span><span class="sxs-lookup"><span data-stu-id="d1d87-127">ASP.NET MVC 4 automatically created the CRUD (create, read, update, and delete) action methods and views for you (the automatic creation of CRUD action methods and views is known as scaffolding).</span></span> <span data-ttu-id="d1d87-128">Sie verfügen jetzt über eine voll funktionsfähige Webanwendung, mit dem Sie erstellen, auflisten, bearbeiten und löschen Film-Einträge.</span><span class="sxs-lookup"><span data-stu-id="d1d87-128">You now have a fully functional web application that lets you create, list, edit, and delete movie entries.</span></span>

<span data-ttu-id="d1d87-129">Führen Sie die Anwendung, und navigieren Sie zu der `Movies` Controller durch Anfügen */Movies* an die URL in der Adressleiste des Browsers.</span><span class="sxs-lookup"><span data-stu-id="d1d87-129">Run the application and browse to the `Movies` controller by appending */Movies* to the URL in the address bar of your browser.</span></span> <span data-ttu-id="d1d87-130">Da die Anwendung auf das Standardrouting der vertrauenden Seite ist (definiert der *"Global.asax"* Datei), die Browseranforderung `http://localhost:xxxxx/Movies` an Standardeinstellung weitergeleitet `Index` Aktionsmethode von der `Movies` Controller.</span><span class="sxs-lookup"><span data-stu-id="d1d87-130">Because the application is relying on the default routing (defined in the *Global.asax* file), the browser request `http://localhost:xxxxx/Movies` is routed to the default `Index` action method of the `Movies` controller.</span></span> <span data-ttu-id="d1d87-131">Das heißt, die Browseranforderung `http://localhost:xxxxx/Movies` entspricht effektiv der Browseranforderung `http://localhost:xxxxx/Movies/Index`.</span><span class="sxs-lookup"><span data-stu-id="d1d87-131">In other words, the browser request `http://localhost:xxxxx/Movies` is effectively the same as the browser request `http://localhost:xxxxx/Movies/Index`.</span></span> <span data-ttu-id="d1d87-132">Das Ergebnis ist eine leere Liste von Filmen, da Sie noch keine hinzugefügt haben.</span><span class="sxs-lookup"><span data-stu-id="d1d87-132">The result is an empty list of movies, because you haven't added any yet.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image2.png)

### <a name="creating-a-movie"></a><span data-ttu-id="d1d87-133">Erstellen einen Film</span><span class="sxs-lookup"><span data-stu-id="d1d87-133">Creating a Movie</span></span>

<span data-ttu-id="d1d87-134">Klicken Sie auf den Link **Neu erstellen**.</span><span class="sxs-lookup"><span data-stu-id="d1d87-134">Select the **Create New** link.</span></span> <span data-ttu-id="d1d87-135">Geben Sie einige Details über einen Film, und klicken Sie dann auf die **erstellen** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="d1d87-135">Enter some details about a movie and then click the **Create** button.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image3.png)

<span data-ttu-id="d1d87-136">Klicken auf die **erstellen** Schaltfläche bewirkt, dass das Formular an den Server zurückgesendet werden, in dem die Informationen in der Datenbank gespeichert.</span><span class="sxs-lookup"><span data-stu-id="d1d87-136">Clicking the **Create** button causes the form to be posted to the server, where the movie information is saved in the database.</span></span> <span data-ttu-id="d1d87-137">Sie sind dann umgeleitet, um die */Movies* URL, wo Sie in die Auflistung den neu erstellten Film finden können.</span><span class="sxs-lookup"><span data-stu-id="d1d87-137">You're then redirected to the */Movies* URL, where you can see the newly created movie in the listing.</span></span>

<span data-ttu-id="d1d87-138">![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image4.png "IndexWhenHarryMet")</span><span class="sxs-lookup"><span data-stu-id="d1d87-138">![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image4.png "IndexWhenHarryMet")</span></span>

<span data-ttu-id="d1d87-139">Erstellen Sie ein paar weitere Filmeinträge.</span><span class="sxs-lookup"><span data-stu-id="d1d87-139">Create a couple more movie entries.</span></span> <span data-ttu-id="d1d87-140">Testen Sie die Links **Edit** (Bearbeiten), **Details** und **Delete** (Löschen), die alle funktionsbereit sind.</span><span class="sxs-lookup"><span data-stu-id="d1d87-140">Try the **Edit**, **Details**, and **Delete** links, which are all functional.</span></span>

## <a name="examining-the-generated-code"></a><span data-ttu-id="d1d87-141">Überprüfen des generierten Codes</span><span class="sxs-lookup"><span data-stu-id="d1d87-141">Examining the Generated Code</span></span>

<span data-ttu-id="d1d87-142">Öffnen der *Controllers\MoviesController.cs* Datei, und überprüfen Sie die generierte `Index` Methode.</span><span class="sxs-lookup"><span data-stu-id="d1d87-142">Open the *Controllers\MoviesController.cs* file and examine the generated `Index` method.</span></span> <span data-ttu-id="d1d87-143">Ein Teil der Film-Controller mit dem `Index` Methode wird unten gezeigt.</span><span class="sxs-lookup"><span data-stu-id="d1d87-143">A portion of the movie controller with the `Index` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

<span data-ttu-id="d1d87-144">Die folgende Zeile aus der `MoviesController` Klasse instanziiert einen Film-Datenbankkontext aus, wie zuvor beschrieben.</span><span class="sxs-lookup"><span data-stu-id="d1d87-144">The following line from the `MoviesController` class instantiates a movie database context, as described previously.</span></span> <span data-ttu-id="d1d87-145">Den Datenbankkontext Film können Sie Abfragen, bearbeiten und Löschen von Filmen.</span><span class="sxs-lookup"><span data-stu-id="d1d87-145">You can use the movie database context to query, edit, and delete movies.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

<span data-ttu-id="d1d87-146">Eine Anforderung an die `Movies` Controller gibt alle Einträge in der `Movies` Tabelle der Datenbank Film und übergibt dann die Ergebnisse in der `Index` anzeigen.</span><span class="sxs-lookup"><span data-stu-id="d1d87-146">A request to the `Movies` controller returns all the entries in the `Movies` table of the movie database and then passes the results to the `Index` view.</span></span>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="d1d87-147">Stark typisierte Modelle und die @model Schlüsselwort</span><span class="sxs-lookup"><span data-stu-id="d1d87-147">Strongly Typed Models and the @model Keyword</span></span>

<span data-ttu-id="d1d87-148">Weiter oben in diesem Lernprogramm Sie gesehen haben wie ein Controller Daten oder Objekte in eine Ansicht Vorlage übergeben kann die `ViewBag` Objekt.</span><span class="sxs-lookup"><span data-stu-id="d1d87-148">Earlier in this tutorial, you saw how a controller can pass data or objects to a view template using the `ViewBag` object.</span></span> <span data-ttu-id="d1d87-149">Die `ViewBag` ist ein dynamisches Objekt, das eine spät gebundene auf bequeme Weise Informationen an eine Ansicht übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="d1d87-149">The `ViewBag` is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="d1d87-150">ASP.NET MVC bietet außerdem die Möglichkeit, stark übergeben Daten oder Objekte zu einer Sicht Vorlage eingegeben.</span><span class="sxs-lookup"><span data-stu-id="d1d87-150">ASP.NET MVC also provides the ability to pass strongly typed data or objects to a view template.</span></span> <span data-ttu-id="d1d87-151">Stark typisierte dieser Ansatz ermöglicht eine bessere Kompilierung von Code und umfangreichere IntelliSense in Visual Studio-Editor.</span><span class="sxs-lookup"><span data-stu-id="d1d87-151">This strongly typed approach enables better compile-time checking of your code and richer IntelliSense in the Visual Studio editor.</span></span> <span data-ttu-id="d1d87-152">Der Gerüstbau in Visual Studio verwendet diesen Ansatz, mit der `MoviesController` Klasse, und zeigen Vorlagen bei der Erstellung der Methoden und Ansichten.</span><span class="sxs-lookup"><span data-stu-id="d1d87-152">The scaffolding mechanism in Visual Studio used this approach with the `MoviesController` class and view templates when it created the methods and views.</span></span>

<span data-ttu-id="d1d87-153">In der *Controllers\MoviesController.cs* untersuchen Sie die generierte Datei `Details` Methode.</span><span class="sxs-lookup"><span data-stu-id="d1d87-153">In the *Controllers\MoviesController.cs* file examine the generated `Details` method.</span></span> <span data-ttu-id="d1d87-154">Ein Teil der Film-Controller mit dem `Details` Methode wird unten gezeigt.</span><span class="sxs-lookup"><span data-stu-id="d1d87-154">A portion of the movie controller with the `Details` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs?highlight=3,8)]

<span data-ttu-id="d1d87-155">Wenn eine `Movie` gefunden wird, wird eine Instanz von der `Movie` Modell an die Ansicht übergeben wird.</span><span class="sxs-lookup"><span data-stu-id="d1d87-155">If a `Movie` is found, an instance of the `Movie` model is passed to the Details view.</span></span> <span data-ttu-id="d1d87-156">Untersuchen des Inhalts der *Views\Movies\Details.cshtml* Datei.</span><span class="sxs-lookup"><span data-stu-id="d1d87-156">Examine the contents of the *Views\Movies\Details.cshtml* file.</span></span>

<span data-ttu-id="d1d87-157">Durch Einschließen einer `@model` -Anweisung am Anfang der Vorlagendatei anzeigen, können Sie den Typ des Objekts, das die Sicht erwartet angeben.</span><span class="sxs-lookup"><span data-stu-id="d1d87-157">By including a `@model` statement at the top of the view template file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="d1d87-158">Beim Erstellen des „Movies“-Controllers hat Visual Studio automatisch die folgende `@model`-Anweisung am Anfang der Datei *Details.cshtml* hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="d1d87-158">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Details.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.cshtml)]

<span data-ttu-id="d1d87-159">Diese `@model`-Direktive ermöglicht Ihnen den Zugriff auf den Film, den der Controller an die Ansicht übergeben hat, indem ein stark typisiertes `Model`-Objekt verwendet wir.</span><span class="sxs-lookup"><span data-stu-id="d1d87-159">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="d1d87-160">Beispielsweise ist in der *Details.cshtml* Vorlage, die Code übergibt die jeweiligen Film-Feld können Sie die `DisplayNameFor` und [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) HTML-Hilfsmethoden mit stark typisierten `Model` Objekt.</span><span class="sxs-lookup"><span data-stu-id="d1d87-160">For example, in the *Details.cshtml* template, the code passes each movie field to the `DisplayNameFor` and [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="d1d87-161">Erstellen und Bearbeiten von Methoden und Vorlagen anzeigen, auch ein Modellobjekt Film übergeben.</span><span class="sxs-lookup"><span data-stu-id="d1d87-161">The Create and Edit methods and view templates also pass a movie model object.</span></span>

<span data-ttu-id="d1d87-162">Überprüfen Sie die *Index.cshtml* Vorlage anzeigen und die `Index` Methode in der *MoviesController.cs* Datei.</span><span class="sxs-lookup"><span data-stu-id="d1d87-162">Examine the *Index.cshtml* view template and the `Index` method in the *MoviesController.cs* file.</span></span> <span data-ttu-id="d1d87-163">Beachten Sie, wie der Code erstellt ein [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) Objekt beim Aufrufen der `View` Hilfsmethode in der `Index` Aktionsmethode.</span><span class="sxs-lookup"><span data-stu-id="d1d87-163">Notice how the code creates a [`List`](https://msdn.microsoft.com/library/6sh2ey19.aspx) object when it calls the `View` helper method in the `Index` action method.</span></span> <span data-ttu-id="d1d87-164">Der Code übergibt dann diese `Movies` Liste auf dem Controller aus, um die Ansicht:</span><span class="sxs-lookup"><span data-stu-id="d1d87-164">The code then passes this `Movies` list from the controller to the view:</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample5.cs?highlight=3)]

<span data-ttu-id="d1d87-165">Beim Erstellen des Controllers Film Visual Studio Express automatisch zählen folgende `@model` Anweisung am Anfang der *Index.cshtml* Datei:</span><span class="sxs-lookup"><span data-stu-id="d1d87-165">When you created the movie controller, Visual Studio Express automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

<span data-ttu-id="d1d87-166">Dies `@model` Richtlinie können Sie die Liste von Filmen zuzugreifen, die mithilfe der Controller auf die Ansicht durch Übergeben einer `Model` -Objekt, das stark typisiert ist.</span><span class="sxs-lookup"><span data-stu-id="d1d87-166">This `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="d1d87-167">Beispielsweise ist in der *Index.cshtml* Vorlage, der Code durchläuft die Filme durch praktische eine `foreach` Anweisung über die stark typisierte `Model` Objekt:</span><span class="sxs-lookup"><span data-stu-id="d1d87-167">For example, in the *Index.cshtml* template, the code loops through the movies by doing a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample7.cshtml?highlight=1,4,7,10,13,16,19-21)]

<span data-ttu-id="d1d87-168">Da die `Model` Objekt stark typisiert ist (als ein `IEnumerable<Movie>` Objekt), die jeweils `item` als Objekt in der Schleife typisiert ist `Movie`.</span><span class="sxs-lookup"><span data-stu-id="d1d87-168">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each `item` object in the loop is typed as `Movie`.</span></span> <span data-ttu-id="d1d87-169">U. a. bedeutet dies, dass Sie erhalten die kompilierzeitüberprüfung des Codes und vollständige IntelliSense-Unterstützung im Code-Editor:</span><span class="sxs-lookup"><span data-stu-id="d1d87-169">Among other benefits, this means that you get compile-time checking of the code and full IntelliSense support in the code editor:</span></span>

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image5.png)

## <a name="working-with-sql-server-localdb"></a><span data-ttu-id="d1d87-171">Arbeiten mit SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="d1d87-171">Working with SQL Server LocalDB</span></span>

<span data-ttu-id="d1d87-172">Entity Framework Code First wurde festgestellt, dass die Datenbank-Verbindungszeichenfolge, die bereitgestellt wurden, zeigt eine `Movies` Datenbank, die noch nicht vorhanden ist, sodass Code First die Datenbank automatisch erstellt.</span><span class="sxs-lookup"><span data-stu-id="d1d87-172">Entity Framework Code First detected that the database connection string that was provided pointed to a `Movies` database that didn't exist yet, so Code First created the database automatically.</span></span> <span data-ttu-id="d1d87-173">Sie können überprüfen, dass es durch die Überprüfung erstellt wird die *App\_Daten* Ordner.</span><span class="sxs-lookup"><span data-stu-id="d1d87-173">You can verify that it's been created by looking in the *App\_Data* folder.</span></span> <span data-ttu-id="d1d87-174">Wenn Sie sehen die *Movies.mdf* Datei, klicken Sie auf die **alle Dateien anzeigen** Schaltfläche der **Projektmappen-Explorer** -Symbolleiste klicken Sie auf die **aktualisieren** Schaltfläche, und erweitern Sie dann die *App\_Daten* Ordner.</span><span class="sxs-lookup"><span data-stu-id="d1d87-174">If you don't see the *Movies.mdf* file, click the **Show All Files** button in the **Solution Explorer** toolbar, click the **Refresh** button, and then expand the *App\_Data* folder.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image6.png)

<span data-ttu-id="d1d87-175">Doppelklicken Sie auf *Movies.mdf* öffnen **Datenbank-EXPLORER**, erweitern Sie dann die **Tabellen** Ordner Filme anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="d1d87-175">Double-click *Movies.mdf* to open **DATABASE EXPLORER**, then expand the **Tables** folder to see the Movies table.</span></span>

<span data-ttu-id="d1d87-176">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image7.png "DB_explorer")</span><span class="sxs-lookup"><span data-stu-id="d1d87-176">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image7.png "DB_explorer")</span></span>

> [!NOTE]
> <span data-ttu-id="d1d87-177">Wenn die Datenbank-Explorer nicht angezeigt wird, aus der **TOOLS** klicken Sie im Menü **mit Datenbank verbinden**, klicken Sie dann "Abbrechen" die **Datenquelle auswählen** Dialogfeld.</span><span class="sxs-lookup"><span data-stu-id="d1d87-177">If the database explorer doesn't appear, from the **TOOLS** menu, select **Connect to Database**, then cancel the **Choose Data Source** dialog.</span></span> <span data-ttu-id="d1d87-178">Hierdurch wird Öffnen der Datenbank-Explorer erzwungen.</span><span class="sxs-lookup"><span data-stu-id="d1d87-178">This will force open the database explorer.</span></span>


> [!NOTE]
> <span data-ttu-id="d1d87-179">Wenn Sie die VWD oder Visual Studio 2010 verwenden und eine Fehlermeldung ähnlich der folgenden folgenden erhalten:</span><span class="sxs-lookup"><span data-stu-id="d1d87-179">If you are using VWD or Visual Studio 2010 and get an error similar to any of the following following:</span></span>
> 
> - <span data-ttu-id="d1d87-180">Die Datenbank "C:\Webs\MVC4\MVCMOVIE\MVCMOVIE\APP\_DATA\MOVIES. MDF-Datei ' kann nicht geöffnet werden, da es sich um Version 706 handelt.</span><span class="sxs-lookup"><span data-stu-id="d1d87-180">The database 'C:\Webs\MVC4\MVCMOVIE\MVCMOVIE\APP\_DATA\MOVIES.MDF' cannot be opened because it is version 706.</span></span> <span data-ttu-id="d1d87-181">Dieser Server unterstützt Version 655 und früher.</span><span class="sxs-lookup"><span data-stu-id="d1d87-181">This server supports version 655 and earlier.</span></span> <span data-ttu-id="d1d87-182">Ein Herabstufungspfad wird nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="d1d87-182">A downgrade path is not supported.</span></span>
> - <span data-ttu-id="d1d87-183">&quot;InvalidOperation-Ausnahme nicht behandelt wurde durch Benutzercode&quot; die bereitgestellte SqlConnection gibt keinen Anfangskatalog.</span><span class="sxs-lookup"><span data-stu-id="d1d87-183">&quot;InvalidOperation Exception was unhandled by user code&quot; The supplied SqlConnection does not specify an initial catalog.</span></span>
> 
> <span data-ttu-id="d1d87-184">Sie müssen zum Installieren der [SQL Server Data Tools](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx) und [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0).</span><span class="sxs-lookup"><span data-stu-id="d1d87-184">You need to install the [SQL Server Data Tools](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx) and [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0).</span></span> <span data-ttu-id="d1d87-185">Überprüfen Sie die `MovieDBContext` Verbindungszeichenfolge, die auf der vorherigen Seite angegeben.</span><span class="sxs-lookup"><span data-stu-id="d1d87-185">Verify the `MovieDBContext` connection string specified on the previous page.</span></span>


<span data-ttu-id="d1d87-186">Mit der rechten Maustaste die `Movies` Tabelle, und wählen Sie **Tabellendaten anzeigen** die Daten sehen Sie erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="d1d87-186">Right-click the `Movies` table and select **Show Table Data** to see the data you created.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image8.png)

<span data-ttu-id="d1d87-187">Mit der rechten Maustaste die `Movies` Tabelle, und wählen Sie **Tabellendefinition öffnen** , finden in der Tabelle, die Struktur dieser Entity Framework Code First für Sie erstellt.</span><span class="sxs-lookup"><span data-stu-id="d1d87-187">Right-click the `Movies` table and select **Open Table Definition** to see the table structure that Entity Framework Code First created for you.</span></span>

<span data-ttu-id="d1d87-188">![](accessing-your-models-data-from-a-controller/_static/image9.png "MoviesTable")</span><span class="sxs-lookup"><span data-stu-id="d1d87-188">![](accessing-your-models-data-from-a-controller/_static/image9.png "MoviesTable")</span></span>

![](accessing-your-models-data-from-a-controller/_static/image10.png)

<span data-ttu-id="d1d87-189">Beachten Sie, dass wie das Schema der `Movies` Tabelle zugeordnet, die `Movie` Klasse, die Sie zuvor erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="d1d87-189">Notice how the schema of the `Movies` table maps to the `Movie` class you created earlier.</span></span> <span data-ttu-id="d1d87-190">Entity Framework Code First automatisch erstellt, dieses Schema für Sie auf der Grundlage Ihrer `Movie` Klasse.</span><span class="sxs-lookup"><span data-stu-id="d1d87-190">Entity Framework Code First automatically created this schema for you based on your `Movie` class.</span></span>

<span data-ttu-id="d1d87-191">Wenn Sie fertig sind, schließen Sie die Verbindung, indem Sie mit der rechten Maustaste auf *MovieDBContext* auswählen und **Verbindung schließen**.</span><span class="sxs-lookup"><span data-stu-id="d1d87-191">When you're finished, close the connection by right clicking *MovieDBContext* and selecting **Close Connection**.</span></span> <span data-ttu-id="d1d87-192">(Wenn Sie die Verbindung nicht schließen, erhalten Sie möglicherweise einen Fehler das nächste Mal des Projekts ausführen).</span><span class="sxs-lookup"><span data-stu-id="d1d87-192">(If you don't close the connection, you might get an error the next time you run the project).</span></span>

<span data-ttu-id="d1d87-193">![](accessing-your-models-data-from-a-controller/_static/image11.png "CloseConnection")</span><span class="sxs-lookup"><span data-stu-id="d1d87-193">![](accessing-your-models-data-from-a-controller/_static/image11.png "CloseConnection")</span></span>

<span data-ttu-id="d1d87-194">Sie haben jetzt die Datenbank und eine einfache Liste Seite daraus angezeigt werden sollen.</span><span class="sxs-lookup"><span data-stu-id="d1d87-194">You now have the database and a simple listing page to display content from it.</span></span> <span data-ttu-id="d1d87-195">In den nächsten Lernprogrammen wir untersuchen Sie den Rest des Codes scaffolded und Hinzufügen einer `SearchIndex` Methode und eine `SearchIndex` anzeigen, die für Filme in dieser Datenbank durchsuchen kann.</span><span class="sxs-lookup"><span data-stu-id="d1d87-195">In the next tutorial, we'll examine the rest of the scaffolded code and add a `SearchIndex` method and a `SearchIndex` view that lets you search for movies in this database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d1d87-196">[Zurück](adding-a-model.md)
> [Weiter](examining-the-edit-methods-and-edit-view.md)</span><span class="sxs-lookup"><span data-stu-id="d1d87-196">[Previous](adding-a-model.md)
[Next](examining-the-edit-methods-and-edit-view.md)</span></span>
