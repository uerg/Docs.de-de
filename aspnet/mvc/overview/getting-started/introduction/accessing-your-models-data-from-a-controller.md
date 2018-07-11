---
uid: mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
title: Zugreifen auf Modelldaten anhand eines Controllers | Microsoft-Dokumentation
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 10/17/2013
ms.assetid: caa1ba4a-f9f0-4181-ba21-042e3997861d
msc.legacyurl: /mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: a3f3f4a030650ff65b070528c5efa1605be764a0
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38120152"
---
<a name="accessing-your-models-data-from-a-controller"></a><span data-ttu-id="81799-102">Zugreifen auf Modelldaten anhand eines Controllers</span><span class="sxs-lookup"><span data-stu-id="81799-102">Accessing Your Model's Data from a Controller</span></span>
====================
<span data-ttu-id="81799-103">durch [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="81799-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

<span data-ttu-id="81799-104">In diesem Abschnitt erstellen Sie ein neues `MoviesController` Klasse, und Schreiben von Code, das die Movie-Daten abgerufen und im Browser eine ansichtsvorlage anzeigt.</span><span class="sxs-lookup"><span data-stu-id="81799-104">In this section, you'll create a new `MoviesController` class and write code that retrieves the movie data and displays it in the browser using a view template.</span></span>

<span data-ttu-id="81799-105">**Erstellen Sie die Anwendung** bevor Sie mit dem nächsten Schritt fortfahren.</span><span class="sxs-lookup"><span data-stu-id="81799-105">**Build the application** before going on to the next step.</span></span> <span data-ttu-id="81799-106">Wenn Sie die Anwendung nicht erstellen, erhalten Sie einen Fehler mit dem Hinzufügen eines Controllers.</span><span class="sxs-lookup"><span data-stu-id="81799-106">If you don't build the application, you'll get an error adding a controller.</span></span>

<span data-ttu-id="81799-107">Klicken Sie im Projektmappen-Explorer mit der Maustaste der *Controller* Ordner, und klicken Sie dann auf **hinzufügen**, klicken Sie dann **Controller**.</span><span class="sxs-lookup"><span data-stu-id="81799-107">In Solution Explorer, right-click the *Controllers* folder and then click **Add**, then **Controller**.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image1.png)

<span data-ttu-id="81799-108">In der **Gerüst hinzufügen** Dialogfeld klicken Sie auf **MVC 5-Controller mit Ansichten unter Verwendung von Entity Framework**, und klicken Sie dann auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="81799-108">In the **Add Scaffold** dialog box, click **MVC 5 Controller with views, using Entity Framework**, and then click **Add**.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image2.png)

- <span data-ttu-id="81799-109">Wählen Sie **Movie (MvcMovie.Models)** für die Model-Klasse.</span><span class="sxs-lookup"><span data-stu-id="81799-109">Select **Movie (MvcMovie.Models)** for the Model class.</span></span>
- <span data-ttu-id="81799-110">Wählen Sie **MovieDBContext (MvcMovie.Models)** für das die Datenkontextklasse.</span><span class="sxs-lookup"><span data-stu-id="81799-110">Select **MovieDBContext (MvcMovie.Models)** for the Data context class.</span></span>
- <span data-ttu-id="81799-111">Geben Sie für den Controllernamen **MoviesController**.</span><span class="sxs-lookup"><span data-stu-id="81799-111">For the Controller name enter **MoviesController**.</span></span>

  <span data-ttu-id="81799-112">Die folgende Abbildung zeigt das Dialogfeld "Abgeschlossene".</span><span class="sxs-lookup"><span data-stu-id="81799-112">The image below shows the completed dialog.</span></span>  
  
![](accessing-your-models-data-from-a-controller/_static/image3.png)   

<span data-ttu-id="81799-113">Klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="81799-113">Click **Add**.</span></span> <span data-ttu-id="81799-114">(Wenn Sie eine Fehlermeldung erhalten, nicht haben Sie wahrscheinlich die Anwendung vor dem Starten den Controller hinzufügen erstellen.) Visual Studio erstellt die folgenden Dateien und Ordner:</span><span class="sxs-lookup"><span data-stu-id="81799-114">(If you get an error, you probably didn't build the application before starting adding the controller.) Visual Studio creates the following files and folders:</span></span>

- <span data-ttu-id="81799-115">*Eine "moviescontroller.cs"* Datei die *Controller* Ordner.</span><span class="sxs-lookup"><span data-stu-id="81799-115">*A MoviesController.cs* file in the *Controllers* folder.</span></span>
- <span data-ttu-id="81799-116">Ein *ansichten\filme* Ordner.</span><span class="sxs-lookup"><span data-stu-id="81799-116">A *Views\Movies* folder.</span></span>
- <span data-ttu-id="81799-117">*Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, und *"Index.cshtml"* in der neuen *ansichten\filme* Ordner.</span><span class="sxs-lookup"><span data-stu-id="81799-117">*Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, and *Index.cshtml* in the new *Views\Movies* folder.</span></span>

<span data-ttu-id="81799-118">Visual Studio automatisch erstellt, die [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (erstellen, lesen, aktualisieren und löschen) Aktionsmethoden und Ansichten für Sie (die automatische Erstellung von CRUD-Aktionsmethoden und Ansichten wird als Gerüstbau bezeichnet).</span><span class="sxs-lookup"><span data-stu-id="81799-118">Visual Studio automatically created the [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views for you (the automatic creation of CRUD action methods and views is known as scaffolding).</span></span> <span data-ttu-id="81799-119">Sie verfügen nun über eine voll funktionsfähige Webanwendung, mit dem Sie das Erstellen, auflisten, bearbeiten und löschen filmeinträge.</span><span class="sxs-lookup"><span data-stu-id="81799-119">You now have a fully functional web application that lets you create, list, edit, and delete movie entries.</span></span>

<span data-ttu-id="81799-120">Führen Sie die Anwendung, und klicken Sie auf die **MVC Movie** Link (oder navigieren Sie zu der `Movies` Controller durch Anfügen */Movies* an die URL in die Adressleiste des Browsers).</span><span class="sxs-lookup"><span data-stu-id="81799-120">Run the application and click on the **MVC Movie** link (or browse to the `Movies` controller by appending */Movies* to the URL in the address bar of your browser).</span></span> <span data-ttu-id="81799-121">Da die Anwendung auf das Standardrouting der vertrauenden Seite ist (in definiert die *App\_start\routeconfig* Datei), der Browseranforderung `http://localhost:xxxxx/Movies` weitergeleitet wird, auf den Standardwert `Index` Action-Methode der der `Movies` Controller.</span><span class="sxs-lookup"><span data-stu-id="81799-121">Because the application is relying on the default routing (defined in the *App\_Start\RouteConfig.cs* file), the browser request `http://localhost:xxxxx/Movies` is routed to the default `Index` action method of the `Movies` controller.</span></span> <span data-ttu-id="81799-122">Das heißt, der die Browseranforderung `http://localhost:xxxxx/Movies` ist identisch mit die Browseranforderung `http://localhost:xxxxx/Movies/Index`.</span><span class="sxs-lookup"><span data-stu-id="81799-122">In other words, the browser request `http://localhost:xxxxx/Movies` is effectively the same as the browser request `http://localhost:xxxxx/Movies/Index`.</span></span> <span data-ttu-id="81799-123">Das Ergebnis ist eine leere Liste von Filmen, da Sie noch keine hinzugefügt haben.</span><span class="sxs-lookup"><span data-stu-id="81799-123">The result is an empty list of movies, because you haven't added any yet.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image4.png)

### <a name="creating-a-movie"></a><span data-ttu-id="81799-124">Erstellen einen Film</span><span class="sxs-lookup"><span data-stu-id="81799-124">Creating a Movie</span></span>

<span data-ttu-id="81799-125">Klicken Sie auf den Link **Neu erstellen**.</span><span class="sxs-lookup"><span data-stu-id="81799-125">Select the **Create New** link.</span></span> <span data-ttu-id="81799-126">Geben Sie einige Details zu einem Film, und klicken Sie dann auf die **erstellen** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="81799-126">Enter some details about a movie and then click the **Create** button.</span></span>


![](accessing-your-models-data-from-a-controller/_static/image5.png)

> [!NOTE]
> <span data-ttu-id="81799-127">Sie können im Feld für den Preis Dezimaltrennzeichen oder Kommas eingeben möglicherweise nicht.</span><span class="sxs-lookup"><span data-stu-id="81799-127">You may not be able to enter decimal points or commas in the Price field.</span></span> <span data-ttu-id="81799-128">zur Unterstützung von jQuery-Validierung für nicht englische Gebietsschemas, in denen ein Komma (&quot;,&quot;) für ein Dezimaltrennzeichen und einem nicht-US-englischen Datums-und Uhrzeitformate, müssen Sie enthalten *globalize.js* und Ihren speziellen  *Cultures/globalize.Cultures.js* Datei (aus [ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) und JavaScript verwenden `Globalize.parseFloat`.</span><span class="sxs-lookup"><span data-stu-id="81799-128">To support jQuery validation for non-English locales that use a comma (&quot;,&quot;) for a decimal point, and non US-English date formats, you must include *globalize.js* and your specific *cultures/globalize.cultures.js* file(from [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) and JavaScript to use `Globalize.parseFloat`.</span></span> <span data-ttu-id="81799-129">Ich zeige, wie Sie dazu im nächsten Tutorial.</span><span class="sxs-lookup"><span data-stu-id="81799-129">I'll show how to do this in the next tutorial.</span></span> <span data-ttu-id="81799-130">Geben Sie einstweilen ganze Zahlen wie 10 ein.</span><span class="sxs-lookup"><span data-stu-id="81799-130">For now, just enter whole numbers like 10.</span></span>


<span data-ttu-id="81799-131">Klicken auf die **erstellen** -Schaltfläche bewirkt, dass das Formular an den Server gesendet werden, in dem die Filminformationen in der Datenbank gespeichert.</span><span class="sxs-lookup"><span data-stu-id="81799-131">Clicking the **Create** button causes the form to be posted to the server, where the movie information is saved in the database.</span></span> <span data-ttu-id="81799-132">Sie werden dann auf umgeleitet, die */Movies* URL hier Sie den neu erstellten Film in der Auflistung sehen.</span><span class="sxs-lookup"><span data-stu-id="81799-132">You're then redirected to the */Movies* URL, where you can see the newly created movie in the listing.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image6.png)

<span data-ttu-id="81799-133">Erstellen Sie ein paar weitere Filmeinträge.</span><span class="sxs-lookup"><span data-stu-id="81799-133">Create a couple more movie entries.</span></span> <span data-ttu-id="81799-134">Testen Sie die Links **Edit** (Bearbeiten), **Details** und **Delete** (Löschen), die alle funktionsbereit sind.</span><span class="sxs-lookup"><span data-stu-id="81799-134">Try the **Edit**, **Details**, and **Delete** links, which are all functional.</span></span>

## <a name="examining-the-generated-code"></a><span data-ttu-id="81799-135">Überprüfen des generierten Codes</span><span class="sxs-lookup"><span data-stu-id="81799-135">Examining the Generated Code</span></span>

<span data-ttu-id="81799-136">Öffnen der *Controllers\MoviesController.cs* Datei, und untersuchen Sie die generierte `Index` Methode.</span><span class="sxs-lookup"><span data-stu-id="81799-136">Open the *Controllers\MoviesController.cs* file and examine the generated `Index` method.</span></span> <span data-ttu-id="81799-137">Ein Teil der Movie-Controller mit dem `Index` Methode wird unten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="81799-137">A portion of the movie controller with the `Index` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

<span data-ttu-id="81799-138">Eine Anforderung an die `Movies` Controller gibt alle Einträge in der `Movies` -Tabelle und übergibt dann die Ergebnisse an die `Index` anzeigen.</span><span class="sxs-lookup"><span data-stu-id="81799-138">A request to the `Movies` controller returns all the entries in the `Movies` table and then passes the results to the `Index` view.</span></span> <span data-ttu-id="81799-139">Die folgende Zeile aus der `MoviesController` Klasse eine filmdatenbankkontext instanziiert, wie zuvor beschrieben.</span><span class="sxs-lookup"><span data-stu-id="81799-139">The following line from the `MoviesController` class instantiates a movie database context, as described previously.</span></span> <span data-ttu-id="81799-140">Die filmdatenbankkontext können Sie Abfragen, bearbeiten und Löschen von Filmen.</span><span class="sxs-lookup"><span data-stu-id="81799-140">You can use the movie database context to query, edit, and delete movies.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="81799-141">Stark typisierte Modelle und die @model Schlüsselwort</span><span class="sxs-lookup"><span data-stu-id="81799-141">Strongly Typed Models and the @model Keyword</span></span>

<span data-ttu-id="81799-142">Weiter oben in diesem Tutorial haben Sie gesehen haben wie ein Controller Daten oder Objekte in eine Ansicht Vorlage übergeben kann, die `ViewBag` Objekt.</span><span class="sxs-lookup"><span data-stu-id="81799-142">Earlier in this tutorial, you saw how a controller can pass data or objects to a view template using the `ViewBag` object.</span></span> <span data-ttu-id="81799-143">Die `ViewBag` ist ein dynamisches Objekt, das spät gebundene bequem übergeben Informationen an eine Ansicht bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="81799-143">The `ViewBag` is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="81799-144">MVC bietet außerdem die Möglichkeit, übergeben Sie *stark* typisierte Objekte eine ansichtsvorlage.</span><span class="sxs-lookup"><span data-stu-id="81799-144">MVC also provides the ability to pass *strongly* typed objects to a view template.</span></span> <span data-ttu-id="81799-145">Dieser stark typisierte Ansatz ermöglicht eine bessere während der Kompilierung des Codes überprüfen und umfassendere [IntelliSense](https://msdn.microsoft.com/library/hcw1s69b(v=vs.120).aspx) in Visual Studio-Editor.</span><span class="sxs-lookup"><span data-stu-id="81799-145">This strongly typed approach enables better compile-time checking of your code and richer [IntelliSense](https://msdn.microsoft.com/library/hcw1s69b(v=vs.120).aspx) in the Visual Studio editor.</span></span> <span data-ttu-id="81799-146">Der Gerüstbau in Visual Studio verwendet diesen Ansatz (übergeben, also eine *stark* typisierten Modell) mit der `MoviesController` Klasse, und zeigen-Vorlagen, wenn sie die Methoden und Ansichten erstellt.</span><span class="sxs-lookup"><span data-stu-id="81799-146">The scaffolding mechanism in Visual Studio used this approach (that is, passing a *strongly* typed model) with the `MoviesController` class and view templates when it created the methods and views.</span></span>

<span data-ttu-id="81799-147">In der *Controllers\MoviesController.cs* untersuchen Sie die generierte Datei `Details` Methode.</span><span class="sxs-lookup"><span data-stu-id="81799-147">In the *Controllers\MoviesController.cs* file examine the generated `Details` method.</span></span> <span data-ttu-id="81799-148">Die `Details` Methode wird unten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="81799-148">The `Details` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs)]

<span data-ttu-id="81799-149">Die `id` Parameter wird in der Regel als übergeben, Weiterleiten von Daten, z. B. `http://localhost:1234/movies/details/1` des Controllers wird festgelegt werden, mit der Movie-Controller, die Aktion, die `details` und `id` auf 1.</span><span class="sxs-lookup"><span data-stu-id="81799-149">The `id` parameter is generally passed as route data, for example `http://localhost:1234/movies/details/1` will set the controller to the movie controller, the action to `details` and the `id` to 1.</span></span> <span data-ttu-id="81799-150">Sie könnten auch wie folgt in die Id mit einer Abfragezeichenfolge übergeben:</span><span class="sxs-lookup"><span data-stu-id="81799-150">You could also pass in the id with a query string as follows:</span></span>

`http://localhost:1234/movies/details?id=1`

<span data-ttu-id="81799-151">Wenn eine `Movie` gefunden wird, wird eine Instanz von der `Movie` Modell wird übergeben, um die `Details` anzeigen:</span><span class="sxs-lookup"><span data-stu-id="81799-151">If a `Movie` is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample4.cs)]

<span data-ttu-id="81799-152">Untersuchen des Inhalts der *Views\Movies\Details.cshtml* Datei:</span><span class="sxs-lookup"><span data-stu-id="81799-152">Examine the contents of the *Views\Movies\Details.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.cshtml?highlight=1-2)]

<span data-ttu-id="81799-153">Durch Einschließen einer `@model` -Anweisung am Anfang der Ansichtsdatei für die Vorlage können Sie den Typ des Objekts, das die Ansicht erwartet angeben.</span><span class="sxs-lookup"><span data-stu-id="81799-153">By including a `@model` statement at the top of the view template file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="81799-154">Beim Erstellen des „Movies“-Controllers hat Visual Studio automatisch die folgende `@model`-Anweisung am Anfang der Datei *Details.cshtml* hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="81799-154">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Details.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

<span data-ttu-id="81799-155">Diese `@model`-Direktive ermöglicht Ihnen den Zugriff auf den Film, den der Controller an die Ansicht übergeben hat, indem ein stark typisiertes `Model`-Objekt verwendet wir.</span><span class="sxs-lookup"><span data-stu-id="81799-155">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="81799-156">Z. B. in der *Details.cshtml* Vorlage übergibt der Code jedes filmfeld an die `DisplayNameFor` und ["DisplayFor"](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) HTML-Hilfsprogrammen, mit dem stark typisierten `Model` Objekt.</span><span class="sxs-lookup"><span data-stu-id="81799-156">For example, in the *Details.cshtml* template, the code passes each movie field to the `DisplayNameFor` and [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="81799-157">Die `Create` und `Edit` Methoden und Anzeigen von Vorlagen auch ein Movie-Modell-Objekt übergeben.</span><span class="sxs-lookup"><span data-stu-id="81799-157">The `Create` and `Edit` methods and view templates also pass a movie model object.</span></span>

<span data-ttu-id="81799-158">Überprüfen Sie die *"Index.cshtml"* Vorlage anzeigen und die `Index` -Methode in der die *"moviescontroller.cs"* Datei.</span><span class="sxs-lookup"><span data-stu-id="81799-158">Examine the *Index.cshtml* view template and the `Index` method in the *MoviesController.cs* file.</span></span> <span data-ttu-id="81799-159">Beachten Sie, wie der Code erstellt eine [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) Objekt beim Aufrufen der `View` -Hilfsmethode in den `Index` Aktionsmethode.</span><span class="sxs-lookup"><span data-stu-id="81799-159">Notice how the code creates a [`List`](https://msdn.microsoft.com/library/6sh2ey19.aspx) object when it calls the `View` helper method in the `Index` action method.</span></span> <span data-ttu-id="81799-160">Der Code übergibt diese dann `Movies` Liste aus der `Index` Aktionsmethode zur Ansicht:</span><span class="sxs-lookup"><span data-stu-id="81799-160">The code then passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample7.cs?highlight=3)]

<span data-ttu-id="81799-161">Bei der Erstellung des Movie-Controllers enthalten Visual Studio automatisch die folgenden `@model` Anweisung am Anfang der *"Index.cshtml"* Datei:</span><span class="sxs-lookup"><span data-stu-id="81799-161">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample8.cshtml)]

<span data-ttu-id="81799-162">Dies `@model` -Direktive ermöglicht Ihnen Zugriff auf die Liste von Filmen, die mithilfe der Controller an die Ansicht übergeben eine `Model` -Objekt, das stark typisiert ist.</span><span class="sxs-lookup"><span data-stu-id="81799-162">This `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="81799-163">Z. B. in der *"Index.cshtml"* Vorlage, der Code durchläuft die Filme durch praktische Übungen einen `foreach` Anweisung für das stark typisierte `Model` Objekt:</span><span class="sxs-lookup"><span data-stu-id="81799-163">For example, in the *Index.cshtml* template, the code loops through the movies by doing a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample9.cshtml?highlight=1,4,7,10,13,16,19-21)]

<span data-ttu-id="81799-164">Da die `Model` -Objekt stark typisiert ist (als ein `IEnumerable<Movie>` Objekt), die jeweils `item` Objekt in der Schleife wird als eingegeben `Movie`.</span><span class="sxs-lookup"><span data-stu-id="81799-164">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each `item` object in the loop is typed as `Movie`.</span></span> <span data-ttu-id="81799-165">Neben anderen Vorteilen bedeutet dies, dass Sie während der Kompilierung des Codes überprüfen und vollständige IntelliSense-Unterstützung im Code-Editor:</span><span class="sxs-lookup"><span data-stu-id="81799-165">Among other benefits, this means that you get compile-time checking of the code and full IntelliSense support in the code editor:</span></span>

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image8.png)

## <a name="working-with-sql-server-localdb"></a><span data-ttu-id="81799-167">Arbeiten mit SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="81799-167">Working with SQL Server LocalDB</span></span>

<span data-ttu-id="81799-168">Entity Framework Code First hat festgestellt, dass es sich bei die Datenbank-Verbindungszeichenfolge, die bereitgestellt wurden, zeigt eine `Movies` -Datenbank, die noch nicht vorhanden ist, sodass Code First die Datenbank automatisch erstellt.</span><span class="sxs-lookup"><span data-stu-id="81799-168">Entity Framework Code First detected that the database connection string that was provided pointed to a `Movies` database that didn't exist yet, so Code First created the database automatically.</span></span> <span data-ttu-id="81799-169">Sie können überprüfen, ob dieser erstellt wurde anhand der *App\_Daten* Ordner.</span><span class="sxs-lookup"><span data-stu-id="81799-169">You can verify that it's been created by looking in the *App\_Data* folder.</span></span> <span data-ttu-id="81799-170">Wenn Sie nicht angezeigt wird der *Movies.mdf* , klicken Sie auf die **alle Dateien anzeigen** Schaltfläche der **Projektmappen-Explorer** -Symbolleiste klicken Sie auf die **aktualisieren** Schaltfläche, und erweitern Sie dann die *App\_Daten* Ordner.</span><span class="sxs-lookup"><span data-stu-id="81799-170">If you don't see the *Movies.mdf* file, click the **Show All Files** button in the **Solution Explorer** toolbar, click the **Refresh** button, and then expand the *App\_Data* folder.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image9.png)

<span data-ttu-id="81799-171">Doppelklicken Sie auf *Movies.mdf* öffnen **SERVER-EXPLORER**, erweitern Sie dann die **Tabellen** Ordner finden in der Tabelle "Movies".</span><span class="sxs-lookup"><span data-stu-id="81799-171">Double-click *Movies.mdf* to open **SERVER EXPLORER**, then expand the **Tables** folder to see the Movies table.</span></span> <span data-ttu-id="81799-172">Beachten Sie das Schlüsselsymbol neben ID</span><span class="sxs-lookup"><span data-stu-id="81799-172">Note the key icon next to ID.</span></span> <span data-ttu-id="81799-173">Standardmäßig wird EF eine Eigenschaft namens-ID der primäre Schlüssel stellen.</span><span class="sxs-lookup"><span data-stu-id="81799-173">By default, EF will make a property named ID the primary key.</span></span> <span data-ttu-id="81799-174">Weitere Informationen zu EF und MVC, finden Sie unter Tom Dykstras ausgezeichnetes Tutorial auf [MVC und EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="81799-174">For more information on EF and MVC, see Tom Dykstra's excellent tutorial on [MVC and EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

<span data-ttu-id="81799-175">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")</span><span class="sxs-lookup"><span data-stu-id="81799-175">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")</span></span>

<span data-ttu-id="81799-176">Mit der rechten Maustaste die `Movies` Tabelle, und wählen Sie **Tabellendaten anzeigen** zu ermitteln, die Daten erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="81799-176">Right-click the `Movies` table and select **Show Table Data** to see the data you created.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image11.png) 

![](accessing-your-models-data-from-a-controller/_static/image12.png)

<span data-ttu-id="81799-177">Mit der rechten Maustaste die `Movies` Tabelle, und wählen Sie **Tabellendefinition öffnen** um die Tabelle, Entity Framework Code First für Sie erstellten Struktur anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="81799-177">Right-click the `Movies` table and select **Open Table Definition** to see the table structure that Entity Framework Code First created for you.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image13.png)

![](accessing-your-models-data-from-a-controller/_static/image14.png)

<span data-ttu-id="81799-178">Beachten Sie, dass wie das Schema der `Movies` Tabelle zugeordnet, die `Movie` Klasse, die Sie zuvor erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="81799-178">Notice how the schema of the `Movies` table maps to the `Movie` class you created earlier.</span></span> <span data-ttu-id="81799-179">Entity Framework Code First automatisch erstellt, diesem Schema basierend auf Ihrer `Movie` Klasse.</span><span class="sxs-lookup"><span data-stu-id="81799-179">Entity Framework Code First automatically created this schema for you based on your `Movie` class.</span></span>

<span data-ttu-id="81799-180">Wenn Sie fertig sind, schließen Sie die Verbindung mit der rechten Maustaste *MovieDBContext* und **Verbindung schließen**.</span><span class="sxs-lookup"><span data-stu-id="81799-180">When you're finished, close the connection by right clicking *MovieDBContext* and selecting **Close Connection**.</span></span> <span data-ttu-id="81799-181">(Wenn Sie die Verbindung nicht schließen, Sie erhalten möglicherweise einen Fehler beim nächsten des Projekts ausführen).</span><span class="sxs-lookup"><span data-stu-id="81799-181">(If you don't close the connection, you might get an error the next time you run the project).</span></span>

<span data-ttu-id="81799-182">![](accessing-your-models-data-from-a-controller/_static/image15.png "CloseConnection")</span><span class="sxs-lookup"><span data-stu-id="81799-182">![](accessing-your-models-data-from-a-controller/_static/image15.png "CloseConnection")</span></span>

<span data-ttu-id="81799-183">Sie verfügen jetzt über eine Datenbank und Seiten zum Anzeigen, Bearbeiten, Aktualisieren und Löschen von Daten.</span><span class="sxs-lookup"><span data-stu-id="81799-183">You now have a database and pages to display, edit, update and delete data.</span></span> <span data-ttu-id="81799-184">Im nächsten Tutorial, wir die restlichen aus der eingerüstete Code überprüfen und Hinzufügen einer `SearchIndex` Methode und eine `SearchIndex` an, die für Filme in dieser Datenbank durchsuchen kann.</span><span class="sxs-lookup"><span data-stu-id="81799-184">In the next tutorial, we'll examine the rest of the scaffolded code and add a `SearchIndex` method and a `SearchIndex` view that lets you search for movies in this database.</span></span> <span data-ttu-id="81799-185">Weitere Informationen zur Verwendung von Entity Framework mit MVC finden Sie unter [erstellen ein Entity Framework-Datenmodells für eine ASP.NET MVC-Anwendung](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="81799-185">For more information on using Entity Framework with MVC, see [Creating an Entity Framework Data Model for an ASP.NET MVC Application](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="81799-186">[Zurück](creating-a-connection-string.md)
> [Weiter](examining-the-edit-methods-and-edit-view.md)</span><span class="sxs-lookup"><span data-stu-id="81799-186">[Previous](creating-a-connection-string.md)
[Next](examining-the-edit-methods-and-edit-view.md)</span></span>
