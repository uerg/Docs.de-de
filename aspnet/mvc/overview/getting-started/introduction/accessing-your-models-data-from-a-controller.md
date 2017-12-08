---
uid: mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
title: Zugriff auf das Modell Daten aus einem Controller | Microsoft Docs
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: caa1ba4a-f9f0-4181-ba21-042e3997861d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: b60913cef4b62745cf167e6074834bf7d0c228d1
ms.sourcegitcommit: d1d8071d4093bf2444b5ae19d6e45c3d187e338b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/19/2017
---
<a name="accessing-your-models-data-from-a-controller"></a><span data-ttu-id="45072-102">Zugriff auf das Modell Daten aus einem Controller</span><span class="sxs-lookup"><span data-stu-id="45072-102">Accessing Your Model's Data from a Controller</span></span>
====================
<span data-ttu-id="45072-103">Durch [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="45072-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE[Tutorial Note](sample/code-location.md)]

<span data-ttu-id="45072-104">In diesem Abschnitt erstellen Sie ein neues `MoviesController` Klasse, und Schreiben von Code, ruft die Filmdaten ab und zeigt ihn im Browser mit einer Vorlage anzeigen.</span><span class="sxs-lookup"><span data-stu-id="45072-104">In this section, you'll create a new `MoviesController` class and write code that retrieves the movie data and displays it in the browser using a view template.</span></span>

<span data-ttu-id="45072-105">**Erstellen Sie die Anwendung** bevor Sie mit dem nächsten Schritt fortfahren.</span><span class="sxs-lookup"><span data-stu-id="45072-105">**Build the application** before going on to the next step.</span></span> <span data-ttu-id="45072-106">Wenn Sie die Anwendung nicht erstellen, erhalten Sie Fehler beim Hinzufügen eines Controllers.</span><span class="sxs-lookup"><span data-stu-id="45072-106">If you don't build the application, you'll get an error adding a controller.</span></span>

<span data-ttu-id="45072-107">Klicken Sie im Projektmappen-Explorer mit der Maustaste die *Controller* Ordner, und klicken Sie dann auf **hinzufügen**, klicken Sie dann **Controller**.</span><span class="sxs-lookup"><span data-stu-id="45072-107">In Solution Explorer, right-click the *Controllers* folder and then click **Add**, then **Controller**.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image1.png)

<span data-ttu-id="45072-108">In der **Gerüst hinzufügen** (Dialogfeld), klicken Sie auf **MVC 5-Controller mit Ansichten unter Verwendung von Entity Framework**, und klicken Sie dann auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="45072-108">In the **Add Scaffold** dialog box, click **MVC 5 Controller with views, using Entity Framework**, and then click **Add**.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image2.png)

- <span data-ttu-id="45072-109">Wählen Sie **Film (MvcMovie.Models)** für die Modell-Klasse.</span><span class="sxs-lookup"><span data-stu-id="45072-109">Select **Movie (MvcMovie.Models)** for the Model class.</span></span>
- <span data-ttu-id="45072-110">Wählen Sie **MovieDBContext (MvcMovie.Models)** für die Daten Context-Klasse.</span><span class="sxs-lookup"><span data-stu-id="45072-110">Select **MovieDBContext (MvcMovie.Models)** for the Data context class.</span></span>
- <span data-ttu-id="45072-111">Geben Sie für den Controllernamen **MoviesController**.</span><span class="sxs-lookup"><span data-stu-id="45072-111">For the Controller name enter **MoviesController**.</span></span>

 <span data-ttu-id="45072-112">Die folgende Abbildung zeigt das Dialogfeld "Abgeschlossene".</span><span class="sxs-lookup"><span data-stu-id="45072-112">The image below shows the completed dialog.</span></span>  
  
![](accessing-your-models-data-from-a-controller/_static/image3.png)   

<span data-ttu-id="45072-113">Klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="45072-113">Click **Add**.</span></span> <span data-ttu-id="45072-114">(Wenn Sie eine Fehlermeldung erhalten, haben nicht Sie wahrscheinlich die Anwendung vor dem Starten den Controller hinzufügen erstellen.) Visual Studio erstellt die folgenden Dateien und Ordner:</span><span class="sxs-lookup"><span data-stu-id="45072-114">(If you get an error, you probably didn't build the application before starting adding the controller.) Visual Studio creates the following files and folders:</span></span>

- <span data-ttu-id="45072-115">*Eine MoviesController.cs* in der Datei die *Controller* Ordner.</span><span class="sxs-lookup"><span data-stu-id="45072-115">*A MoviesController.cs* file in the *Controllers* folder.</span></span>
- <span data-ttu-id="45072-116">Ein *Views\Movies* Ordner.</span><span class="sxs-lookup"><span data-stu-id="45072-116">A *Views\Movies* folder.</span></span>
- <span data-ttu-id="45072-117">*Create.cshtml Delete.cshtml, Details.cshtml, Edit.cshtml*, und *Index.cshtml* in der neuen *Views\Movies* Ordner.</span><span class="sxs-lookup"><span data-stu-id="45072-117">*Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, and *Index.cshtml* in the new *Views\Movies* folder.</span></span>

<span data-ttu-id="45072-118">Visual Studio erstellt automatisch die [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (erstellen, lesen, aktualisieren und löschen) Aktionsmethoden und Ansichten für Sie (als Gerüstbau wird die automatische Erstellung von CRUD-Aktionsmethoden und Ansichten bezeichnet).</span><span class="sxs-lookup"><span data-stu-id="45072-118">Visual Studio automatically created the [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views for you (the automatic creation of CRUD action methods and views is known as scaffolding).</span></span> <span data-ttu-id="45072-119">Sie verfügen jetzt über eine voll funktionsfähige Webanwendung, mit dem Sie erstellen, auflisten, bearbeiten und löschen Film-Einträge.</span><span class="sxs-lookup"><span data-stu-id="45072-119">You now have a fully functional web application that lets you create, list, edit, and delete movie entries.</span></span>

<span data-ttu-id="45072-120">Führen Sie die Anwendung, und klicken Sie auf die **MVC Film** Link (oder navigieren Sie zu der `Movies` Controller durch Anfügen */Movies* an die URL in der Adressleiste des Browsers).</span><span class="sxs-lookup"><span data-stu-id="45072-120">Run the application and click on the **MVC Movie** link (or browse to the `Movies` controller by appending */Movies* to the URL in the address bar of your browser).</span></span> <span data-ttu-id="45072-121">Da die Anwendung auf das Standardrouting der vertrauenden Seite ist (definiert der *App\_Start\RouteConfig.cs* Datei), die Browseranforderung `http://localhost:xxxxx/Movies` an Standardeinstellung weitergeleitet `Index` Aktionsmethode, die von der `Movies` Controller.</span><span class="sxs-lookup"><span data-stu-id="45072-121">Because the application is relying on the default routing (defined in the *App\_Start\RouteConfig.cs* file), the browser request `http://localhost:xxxxx/Movies` is routed to the default `Index` action method of the `Movies` controller.</span></span> <span data-ttu-id="45072-122">Das heißt, die Browseranforderung `http://localhost:xxxxx/Movies` entspricht effektiv der Browseranforderung `http://localhost:xxxxx/Movies/Index`.</span><span class="sxs-lookup"><span data-stu-id="45072-122">In other words, the browser request `http://localhost:xxxxx/Movies` is effectively the same as the browser request `http://localhost:xxxxx/Movies/Index`.</span></span> <span data-ttu-id="45072-123">Das Ergebnis ist eine leere Liste von Filmen, da Sie noch keine hinzugefügt haben.</span><span class="sxs-lookup"><span data-stu-id="45072-123">The result is an empty list of movies, because you haven't added any yet.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image4.png)

### <a name="creating-a-movie"></a><span data-ttu-id="45072-124">Erstellen einen Film</span><span class="sxs-lookup"><span data-stu-id="45072-124">Creating a Movie</span></span>

<span data-ttu-id="45072-125">Klicken Sie auf den Link **Neu erstellen**.</span><span class="sxs-lookup"><span data-stu-id="45072-125">Select the **Create New** link.</span></span> <span data-ttu-id="45072-126">Geben Sie einige Details über einen Film, und klicken Sie dann auf die **erstellen** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="45072-126">Enter some details about a movie and then click the **Create** button.</span></span>


![](accessing-your-models-data-from-a-controller/_static/image5.png)

> [!NOTE]
> <span data-ttu-id="45072-127">Sie können im Feld "Preis" Dezimaltrennzeichen oder Kommas eingeben möglicherweise nicht.</span><span class="sxs-lookup"><span data-stu-id="45072-127">You may not be able to enter decimal points or commas in the Price field.</span></span> <span data-ttu-id="45072-128">Darin jQuery-Validierung bei nicht englischen Gebietsschemas zu unterstützen, verwenden ein Komma (&quot;,&quot;) für ein Dezimaltrennzeichen und einem nicht US-englischen Datums-und Uhrzeitformate, enthalten Sie *globalize.js* und Ihre spezifischen  *Cultures/globalize.Cultures.js* Datei (aus [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) und JavaScript verwenden `Globalize.parseFloat`.</span><span class="sxs-lookup"><span data-stu-id="45072-128">To support jQuery validation for non-English locales that use a comma (&quot;,&quot;) for a decimal point, and non US-English date formats, you must include *globalize.js* and your specific *cultures/globalize.cultures.js* file(from [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) and JavaScript to use `Globalize.parseFloat`.</span></span> <span data-ttu-id="45072-129">Ich zeige wie dies in den nächsten Lernprogrammen ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="45072-129">I'll show how to do this in the next tutorial.</span></span> <span data-ttu-id="45072-130">Geben Sie einstweilen ganze Zahlen wie 10 ein.</span><span class="sxs-lookup"><span data-stu-id="45072-130">For now, just enter whole numbers like 10.</span></span>


<span data-ttu-id="45072-131">Klicken auf die **erstellen** Schaltfläche bewirkt, dass das Formular an den Server zurückgesendet werden, in dem die Informationen in der Datenbank gespeichert.</span><span class="sxs-lookup"><span data-stu-id="45072-131">Clicking the **Create** button causes the form to be posted to the server, where the movie information is saved in the database.</span></span> <span data-ttu-id="45072-132">Sie sind dann umgeleitet, um die */Movies* URL, wo Sie in die Auflistung den neu erstellten Film finden können.</span><span class="sxs-lookup"><span data-stu-id="45072-132">You're then redirected to the */Movies* URL, where you can see the newly created movie in the listing.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image6.png)

<span data-ttu-id="45072-133">Erstellen Sie ein paar weitere Filmeinträge.</span><span class="sxs-lookup"><span data-stu-id="45072-133">Create a couple more movie entries.</span></span> <span data-ttu-id="45072-134">Testen Sie die Links **Edit** (Bearbeiten), **Details** und **Delete** (Löschen), die alle funktionsbereit sind.</span><span class="sxs-lookup"><span data-stu-id="45072-134">Try the **Edit**, **Details**, and **Delete** links, which are all functional.</span></span>

## <a name="examining-the-generated-code"></a><span data-ttu-id="45072-135">Überprüfen des generierten Codes</span><span class="sxs-lookup"><span data-stu-id="45072-135">Examining the Generated Code</span></span>

<span data-ttu-id="45072-136">Öffnen der *Controllers\MoviesController.cs* Datei, und überprüfen Sie die generierte `Index` Methode.</span><span class="sxs-lookup"><span data-stu-id="45072-136">Open the *Controllers\MoviesController.cs* file and examine the generated `Index` method.</span></span> <span data-ttu-id="45072-137">Ein Teil der Film-Controller mit dem `Index` Methode wird unten gezeigt.</span><span class="sxs-lookup"><span data-stu-id="45072-137">A portion of the movie controller with the `Index` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

<span data-ttu-id="45072-138">Eine Anforderung an die `Movies` Controller gibt alle Einträge in der `Movies` -Tabelle und übergibt dann die Ergebnisse in der `Index` anzeigen.</span><span class="sxs-lookup"><span data-stu-id="45072-138">A request to the `Movies` controller returns all the entries in the `Movies` table and then passes the results to the `Index` view.</span></span> <span data-ttu-id="45072-139">Die folgende Zeile aus der `MoviesController` Klasse instanziiert einen Film-Datenbankkontext aus, wie zuvor beschrieben.</span><span class="sxs-lookup"><span data-stu-id="45072-139">The following line from the `MoviesController` class instantiates a movie database context, as described previously.</span></span> <span data-ttu-id="45072-140">Den Datenbankkontext Film können Sie Abfragen, bearbeiten und Löschen von Filmen.</span><span class="sxs-lookup"><span data-stu-id="45072-140">You can use the movie database context to query, edit, and delete movies.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="45072-141">Stark typisierte Modelle und die @model Schlüsselwort</span><span class="sxs-lookup"><span data-stu-id="45072-141">Strongly Typed Models and the @model Keyword</span></span>

<span data-ttu-id="45072-142">Weiter oben in diesem Lernprogramm Sie gesehen haben wie ein Controller Daten oder Objekte in eine Ansicht Vorlage übergeben kann die `ViewBag` Objekt.</span><span class="sxs-lookup"><span data-stu-id="45072-142">Earlier in this tutorial, you saw how a controller can pass data or objects to a view template using the `ViewBag` object.</span></span> <span data-ttu-id="45072-143">Die `ViewBag` ist ein dynamisches Objekt, das eine spät gebundene auf bequeme Weise Informationen an eine Ansicht übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="45072-143">The `ViewBag` is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="45072-144">MVC bietet auch die Möglichkeit, übergeben *stark* typisierte Objekte einer Vorlage anzeigen.</span><span class="sxs-lookup"><span data-stu-id="45072-144">MVC also provides the ability to pass *strongly* typed objects to a view template.</span></span> <span data-ttu-id="45072-145">Diese stark typisierte Ansatz ermöglicht eine bessere Kompilierung des Codes überprüfen und umfangreichere [IntelliSense](https://msdn.microsoft.com/en-us/library/hcw1s69b(v=vs.120).aspx) in Visual Studio-Editor.</span><span class="sxs-lookup"><span data-stu-id="45072-145">This strongly typed approach enables better compile-time checking of your code and richer [IntelliSense](https://msdn.microsoft.com/en-us/library/hcw1s69b(v=vs.120).aspx) in the Visual Studio editor.</span></span> <span data-ttu-id="45072-146">Der Gerüstbau in Visual Studio verwendet diesen Ansatz (d. h. übergeben einer *stark* typisierten Modell) mit der `MoviesController` Klasse, und zeigen Vorlagen bei der Erstellung der Methoden und Ansichten.</span><span class="sxs-lookup"><span data-stu-id="45072-146">The scaffolding mechanism in Visual Studio used this approach (that is, passing a *strongly* typed model) with the `MoviesController` class and view templates when it created the methods and views.</span></span>

<span data-ttu-id="45072-147">In der *Controllers\MoviesController.cs* untersuchen Sie die generierte Datei `Details` Methode.</span><span class="sxs-lookup"><span data-stu-id="45072-147">In the *Controllers\MoviesController.cs* file examine the generated `Details` method.</span></span> <span data-ttu-id="45072-148">Die `Details` Methode wird unten gezeigt.</span><span class="sxs-lookup"><span data-stu-id="45072-148">The `Details` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs)]

<span data-ttu-id="45072-149">Die `id` Parameter ist im Allgemeinen übergebene als Weiterleitung von Daten, z. B. `http://localhost:1234/movies/details/1` wird den Controller mit dem Film-Controller, die Aktion, die festgelegt `details` und `id` auf 1.</span><span class="sxs-lookup"><span data-stu-id="45072-149">The `id` parameter is generally passed as route data, for example `http://localhost:1234/movies/details/1` will set the controller to the movie controller, the action to `details` and the `id` to 1.</span></span> <span data-ttu-id="45072-150">Sie könnten die Id mit einer Abfragezeichenfolge auch wie folgt übergeben:</span><span class="sxs-lookup"><span data-stu-id="45072-150">You could also pass in the id with a query string as follows:</span></span>

`http://localhost:1234/movies/details?id=1`

<span data-ttu-id="45072-151">Wenn eine `Movie` gefunden wird, wird eine Instanz von der `Movie` Modell wird zum Übergeben der `Details` anzeigen:</span><span class="sxs-lookup"><span data-stu-id="45072-151">If a `Movie` is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample4.cs)]

<span data-ttu-id="45072-152">Untersuchen des Inhalts der *Views\Movies\Details.cshtml* Datei:</span><span class="sxs-lookup"><span data-stu-id="45072-152">Examine the contents of the *Views\Movies\Details.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.cshtml?highlight=1-2)]

<span data-ttu-id="45072-153">Durch Einschließen einer `@model` -Anweisung am Anfang der Vorlagendatei anzeigen, können Sie den Typ des Objekts, das die Sicht erwartet angeben.</span><span class="sxs-lookup"><span data-stu-id="45072-153">By including a `@model` statement at the top of the view template file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="45072-154">Beim Erstellen des „Movies“-Controllers hat Visual Studio automatisch die folgende `@model`-Anweisung am Anfang der Datei *Details.cshtml* hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="45072-154">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Details.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

<span data-ttu-id="45072-155">Diese `@model`-Direktive ermöglicht Ihnen den Zugriff auf den Film, den der Controller an die Ansicht übergeben hat, indem ein stark typisiertes `Model`-Objekt verwendet wir.</span><span class="sxs-lookup"><span data-stu-id="45072-155">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="45072-156">Beispielsweise ist in der *Details.cshtml* Vorlage, die Code übergibt die jeweiligen Film-Feld können Sie die `DisplayNameFor` und [DisplayFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) HTML-Hilfsmethoden mit stark typisierten `Model` Objekt.</span><span class="sxs-lookup"><span data-stu-id="45072-156">For example, in the *Details.cshtml* template, the code passes each movie field to the `DisplayNameFor` and [DisplayFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="45072-157">Die `Create` und `Edit` Methoden und Ansichtsvorlagen auch Film Model-Objekts übergeben.</span><span class="sxs-lookup"><span data-stu-id="45072-157">The `Create` and `Edit` methods and view templates also pass a movie model object.</span></span>

<span data-ttu-id="45072-158">Überprüfen Sie die *Index.cshtml* Vorlage anzeigen und die `Index` Methode in der *MoviesController.cs* Datei.</span><span class="sxs-lookup"><span data-stu-id="45072-158">Examine the *Index.cshtml* view template and the `Index` method in the *MoviesController.cs* file.</span></span> <span data-ttu-id="45072-159">Beachten Sie, wie der Code erstellt ein [ `List` ](https://msdn.microsoft.com/en-us/library/6sh2ey19.aspx) Objekt beim Aufrufen der `View` Hilfsmethode in der `Index` Aktionsmethode.</span><span class="sxs-lookup"><span data-stu-id="45072-159">Notice how the code creates a [`List`](https://msdn.microsoft.com/en-us/library/6sh2ey19.aspx) object when it calls the `View` helper method in the `Index` action method.</span></span> <span data-ttu-id="45072-160">Der Code übergibt dann diese `Movies` aus Liste der `Index` Aktionsmethode zur Ansicht:</span><span class="sxs-lookup"><span data-stu-id="45072-160">The code then passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample7.cs?highlight=3)]

<span data-ttu-id="45072-161">Beim Erstellen des Controllers Film enthalten Visual Studio automatisch die folgenden `@model` Anweisung am Anfang der *Index.cshtml* Datei:</span><span class="sxs-lookup"><span data-stu-id="45072-161">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample8.cshtml)]

<span data-ttu-id="45072-162">Dies `@model` Richtlinie können Sie die Liste von Filmen zuzugreifen, die mithilfe der Controller auf die Ansicht durch Übergeben einer `Model` -Objekt, das stark typisiert ist.</span><span class="sxs-lookup"><span data-stu-id="45072-162">This `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="45072-163">Beispielsweise ist in der *Index.cshtml* Vorlage, der Code durchläuft die Filme durch praktische eine `foreach` Anweisung über die stark typisierte `Model` Objekt:</span><span class="sxs-lookup"><span data-stu-id="45072-163">For example, in the *Index.cshtml* template, the code loops through the movies by doing a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample9.cshtml?highlight=1,4,7,10,13,16,19-21)]

<span data-ttu-id="45072-164">Da die `Model` Objekt stark typisiert ist (als ein `IEnumerable<Movie>` Objekt), die jeweils `item` als Objekt in der Schleife typisiert ist `Movie`.</span><span class="sxs-lookup"><span data-stu-id="45072-164">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each `item` object in the loop is typed as `Movie`.</span></span> <span data-ttu-id="45072-165">U. a. bedeutet dies, dass Sie erhalten die kompilierzeitüberprüfung des Codes und vollständige IntelliSense-Unterstützung im Code-Editor:</span><span class="sxs-lookup"><span data-stu-id="45072-165">Among other benefits, this means that you get compile-time checking of the code and full IntelliSense support in the code editor:</span></span>

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image8.png)

## <a name="working-with-sql-server-localdb"></a><span data-ttu-id="45072-167">Arbeiten mit SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="45072-167">Working with SQL Server LocalDB</span></span>

<span data-ttu-id="45072-168">Entity Framework Code First wurde festgestellt, dass die Datenbank-Verbindungszeichenfolge, die bereitgestellt wurden, zeigt eine `Movies` Datenbank, die noch nicht vorhanden ist, sodass Code First die Datenbank automatisch erstellt.</span><span class="sxs-lookup"><span data-stu-id="45072-168">Entity Framework Code First detected that the database connection string that was provided pointed to a `Movies` database that didn't exist yet, so Code First created the database automatically.</span></span> <span data-ttu-id="45072-169">Sie können überprüfen, dass es durch die Überprüfung erstellt wird die *App\_Daten* Ordner.</span><span class="sxs-lookup"><span data-stu-id="45072-169">You can verify that it's been created by looking in the *App\_Data* folder.</span></span> <span data-ttu-id="45072-170">Wenn Sie sehen die *Movies.mdf* Datei, klicken Sie auf die **alle Dateien anzeigen** Schaltfläche der **Projektmappen-Explorer** -Symbolleiste klicken Sie auf die **aktualisieren** Schaltfläche, und erweitern Sie dann die *App\_Daten* Ordner.</span><span class="sxs-lookup"><span data-stu-id="45072-170">If you don't see the *Movies.mdf* file, click the **Show All Files** button in the **Solution Explorer** toolbar, click the **Refresh** button, and then expand the *App\_Data* folder.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image9.png)

<span data-ttu-id="45072-171">Doppelklicken Sie auf *Movies.mdf* öffnen **SERVER-EXPLORER**, erweitern Sie dann die **Tabellen** Ordner Filme anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="45072-171">Double-click *Movies.mdf* to open **SERVER EXPLORER**, then expand the **Tables** folder to see the Movies table.</span></span> <span data-ttu-id="45072-172">Beachten Sie das Schlüsselsymbol neben ID.</span><span class="sxs-lookup"><span data-stu-id="45072-172">Note the key icon next to ID.</span></span> <span data-ttu-id="45072-173">Standardmäßig stellen EF eine Eigenschaft namens-ID der primären Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="45072-173">By default, EF will make a property named ID the primary key.</span></span> <span data-ttu-id="45072-174">Weitere Informationen zu EF und MVC, finden Sie unter Tom Dykstras ausgezeichnete Lernprogramm auf [MVC und EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="45072-174">For more information on EF and MVC, see Tom Dykstra's excellent tutorial on [MVC and EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

<span data-ttu-id="45072-175">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")</span><span class="sxs-lookup"><span data-stu-id="45072-175">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")</span></span>

<span data-ttu-id="45072-176">Mit der rechten Maustaste die `Movies` Tabelle, und wählen Sie **Tabellendaten anzeigen** die Daten sehen Sie erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="45072-176">Right-click the `Movies` table and select **Show Table Data** to see the data you created.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image11.png) 

![](accessing-your-models-data-from-a-controller/_static/image12.png)

<span data-ttu-id="45072-177">Mit der rechten Maustaste die `Movies` Tabelle, und wählen Sie **Tabellendefinition öffnen** , finden in der Tabelle, die Struktur dieser Entity Framework Code First für Sie erstellt.</span><span class="sxs-lookup"><span data-stu-id="45072-177">Right-click the `Movies` table and select **Open Table Definition** to see the table structure that Entity Framework Code First created for you.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image13.png)

![](accessing-your-models-data-from-a-controller/_static/image14.png)

<span data-ttu-id="45072-178">Beachten Sie, dass wie das Schema der `Movies` Tabelle zugeordnet, die `Movie` Klasse, die Sie zuvor erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="45072-178">Notice how the schema of the `Movies` table maps to the `Movie` class you created earlier.</span></span> <span data-ttu-id="45072-179">Entity Framework Code First automatisch erstellt, dieses Schema für Sie auf der Grundlage Ihrer `Movie` Klasse.</span><span class="sxs-lookup"><span data-stu-id="45072-179">Entity Framework Code First automatically created this schema for you based on your `Movie` class.</span></span>

<span data-ttu-id="45072-180">Wenn Sie fertig sind, schließen Sie die Verbindung, indem Sie mit der rechten Maustaste auf *MovieDBContext* auswählen und **Verbindung schließen**.</span><span class="sxs-lookup"><span data-stu-id="45072-180">When you're finished, close the connection by right clicking *MovieDBContext* and selecting **Close Connection**.</span></span> <span data-ttu-id="45072-181">(Wenn Sie die Verbindung nicht schließen, erhalten Sie möglicherweise einen Fehler das nächste Mal des Projekts ausführen).</span><span class="sxs-lookup"><span data-stu-id="45072-181">(If you don't close the connection, you might get an error the next time you run the project).</span></span>

<span data-ttu-id="45072-182">![](accessing-your-models-data-from-a-controller/_static/image15.png "CloseConnection")</span><span class="sxs-lookup"><span data-stu-id="45072-182">![](accessing-your-models-data-from-a-controller/_static/image15.png "CloseConnection")</span></span>

<span data-ttu-id="45072-183">Sie verfügen jetzt über eine Datenbank und Seiten zum Anzeigen, Bearbeiten, Aktualisieren und Löschen von Daten.</span><span class="sxs-lookup"><span data-stu-id="45072-183">You now have a database and pages to display, edit, update and delete data.</span></span> <span data-ttu-id="45072-184">In den nächsten Lernprogrammen wir untersuchen Sie den Rest des Codes scaffolded und Hinzufügen einer `SearchIndex` Methode und eine `SearchIndex` anzeigen, die für Filme in dieser Datenbank durchsuchen kann.</span><span class="sxs-lookup"><span data-stu-id="45072-184">In the next tutorial, we'll examine the rest of the scaffolded code and add a `SearchIndex` method and a `SearchIndex` view that lets you search for movies in this database.</span></span> <span data-ttu-id="45072-185">Weitere Informationen zur Verwendung von Entity Framework mit MVC finden Sie unter [Erstellen eines Entity Framework-Datenmodells für eine ASP.NET MVC-Anwendung](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="45072-185">For more information on using Entity Framework with MVC, see [Creating an Entity Framework Data Model for an ASP.NET MVC Application](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="45072-186">[Zurück](creating-a-connection-string.md)
[Weiter](examining-the-edit-methods-and-edit-view.md)</span><span class="sxs-lookup"><span data-stu-id="45072-186">[Previous](creating-a-connection-string.md)
[Next](examining-the-edit-methods-and-edit-view.md)</span></span>
