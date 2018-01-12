---
uid: mvc/overview/getting-started/introduction/adding-search
title: Suche | Microsoft Docs
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2015
ms.topic: article
ms.assetid: df001954-18bf-4550-b03d-43911a0ea186
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-search
msc.type: authoredcontent
ms.openlocfilehash: 10457d154f5fda875f7d1054d48daeeba3a50b7c
ms.sourcegitcommit: 2b263e87217658caa42eedc4f9d2d21ef0ab5d59
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/12/2018
---
<a name="search"></a><span data-ttu-id="a4825-102">Suchen</span><span class="sxs-lookup"><span data-stu-id="a4825-102">Search</span></span>
====================
<span data-ttu-id="a4825-103">Durch [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="a4825-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE[Tutorial Note](sample/code-location.md)]

## <a name="adding-a-search-method-and-search-view"></a><span data-ttu-id="a4825-104">Hinzufügen eines Search-Methode und des Anzeigebereichs</span><span class="sxs-lookup"><span data-stu-id="a4825-104">Adding a Search Method and Search View</span></span>

<span data-ttu-id="a4825-105">In diesem Abschnitt fügen Sie die Suchfunktion auf der `Index` Aktionsmethode, mit dem Sie suchen, Filme nach Genre oder Namen.</span><span class="sxs-lookup"><span data-stu-id="a4825-105">In this section you'll add search capability to the `Index` action method that lets you search movies by genre or name.</span></span>

## <a name="updating-the-index-form"></a><span data-ttu-id="a4825-106">Die Index-Formular aktualisieren</span><span class="sxs-lookup"><span data-stu-id="a4825-106">Updating the Index Form</span></span>

<span data-ttu-id="a4825-107">Starten mit dem Aktualisieren der `Index` Aktionsmethode zur vorhandenen `MoviesController` Klasse.</span><span class="sxs-lookup"><span data-stu-id="a4825-107">Start by updating the `Index` action method to the existing `MoviesController` class.</span></span> <span data-ttu-id="a4825-108">Hier ist der Code ein:</span><span class="sxs-lookup"><span data-stu-id="a4825-108">Here's the code:</span></span>

[!code-csharp[Main](adding-search/samples/sample1.cs?highlight=1,6-9)]

<span data-ttu-id="a4825-109">Die erste Zeile der `Index` -Methode erstellt die folgenden [LINQ](https://msdn.microsoft.com/en-us/library/bb397926.aspx) Abfrage Filme auswählen:</span><span class="sxs-lookup"><span data-stu-id="a4825-109">The first line of the `Index` method creates the following [LINQ](https://msdn.microsoft.com/en-us/library/bb397926.aspx) query to select the movies:</span></span>

[!code-csharp[Main](adding-search/samples/sample2.cs)]

<span data-ttu-id="a4825-110">Die Abfrage wird an diesem Punkt definiert, jedoch noch nicht für die Datenbank noch ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="a4825-110">The query is defined at this point, but hasn't yet been run against the database.</span></span>

<span data-ttu-id="a4825-111">Wenn die `searchString` -Parameters enthält eine Zeichenfolge, die Filme Abfrage wurde geändert, um auf den Wert der Suchzeichenfolge, mithilfe des folgenden Codes zu filtern:</span><span class="sxs-lookup"><span data-stu-id="a4825-111">If the `searchString` parameter contains a string, the movies query is modified to filter on the value of the search string, using the following code:</span></span>

[!code-csharp[Main](adding-search/samples/sample3.cs)]

<span data-ttu-id="a4825-112">Der Code `s => s.Title` oben ist ein [Lambdaausdruck](https://msdn.microsoft.com/en-us/library/bb397687.aspx).</span><span class="sxs-lookup"><span data-stu-id="a4825-112">The `s => s.Title` code above is a [Lambda Expression](https://msdn.microsoft.com/en-us/library/bb397687.aspx).</span></span> <span data-ttu-id="a4825-113">Lambdas werden in methodenbasierten verwendet [LINQ](https://msdn.microsoft.com/en-us/library/bb397926.aspx) -Abfragen als Argumente für Standardabfrageoperator-Methoden wie z. B. die [, in denen](https://msdn.microsoft.com/en-us/library/system.linq.enumerable.where.aspx) Methode, die im obigen Code verwendet.</span><span class="sxs-lookup"><span data-stu-id="a4825-113">Lambdas are used in method-based [LINQ](https://msdn.microsoft.com/en-us/library/bb397926.aspx) queries as arguments to standard query operator methods such as the [Where](https://msdn.microsoft.com/en-us/library/system.linq.enumerable.where.aspx) method used in the above code.</span></span> <span data-ttu-id="a4825-114">LINQ-Abfragen werden nicht ausgeführt, wenn sie definiert sind oder wenn sie geändert werden, durch Aufrufen einer Methode wie z. B. `Where` oder `OrderBy`.</span><span class="sxs-lookup"><span data-stu-id="a4825-114">LINQ queries are not executed when they are defined or when they are modified by calling a method such as `Where` or `OrderBy`.</span></span> <span data-ttu-id="a4825-115">Stattdessen Ausführung der Abfrage wird verzögert, was bedeutet, dass die Auswertung eines Ausdrucks verzögert wird, bis der realisierte Wert tatsächlich in einer Schleife durchlaufen wird oder die [ `ToList` ](https://msdn.microsoft.com/en-us/library/bb342261.aspx) -Methode aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="a4825-115">Instead, query execution is deferred, which means that the evaluation of an expression is delayed until its realized value is actually iterated over or the [`ToList`](https://msdn.microsoft.com/en-us/library/bb342261.aspx) method is called.</span></span> <span data-ttu-id="a4825-116">In der `Search` Beispiel wird die Abfrage wird ausgeführt, der *Index.cshtml* anzeigen.</span><span class="sxs-lookup"><span data-stu-id="a4825-116">In the `Search` sample, the query is executed in the *Index.cshtml* view.</span></span> <span data-ttu-id="a4825-117">Weitere Informationen zur verzögerten Abfrageausführung finden Sie unter [Abfrageausführung](https://msdn.microsoft.com/en-us/library/bb738633.aspx).</span><span class="sxs-lookup"><span data-stu-id="a4825-117">For more information about deferred query execution, see [Query Execution](https://msdn.microsoft.com/en-us/library/bb738633.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="a4825-118">Die [Contains](https://msdn.microsoft.com/en-us/library/bb155125.aspx) -Methode für die Datenbank, nicht der c#-Code oben ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="a4825-118">The [Contains](https://msdn.microsoft.com/en-us/library/bb155125.aspx) method is run on the database, not the c# code above.</span></span> <span data-ttu-id="a4825-119">Für die Datenbank [Contains](https://msdn.microsoft.com/en-us/library/bb155125.aspx) ordnet [SQL LIKE](https://msdn.microsoft.com/en-us/library/ms179859.aspx), die Groß-/Kleinschreibung beachtet wird.</span><span class="sxs-lookup"><span data-stu-id="a4825-119">On the database, [Contains](https://msdn.microsoft.com/en-us/library/bb155125.aspx) maps to [SQL LIKE](https://msdn.microsoft.com/en-us/library/ms179859.aspx), which is case insensitive.</span></span>

<span data-ttu-id="a4825-120">Jetzt können Sie aktualisieren die `Index` anzeigen, die der Benutzer das Formular angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="a4825-120">Now you can update the `Index` view that will display the form to the user.</span></span>

<span data-ttu-id="a4825-121">Führen Sie die Anwendung, und navigieren Sie zu */Filme/Index*.</span><span class="sxs-lookup"><span data-stu-id="a4825-121">Run the application and navigate to */Movies/Index*.</span></span> <span data-ttu-id="a4825-122">Fügen Sie eine Abfragezeichenfolge wie `?searchString=ghost` an die URL an.</span><span class="sxs-lookup"><span data-stu-id="a4825-122">Append a query string such as `?searchString=ghost` to the URL.</span></span> <span data-ttu-id="a4825-123">Die gefilterten Filme werden angezeigt.</span><span class="sxs-lookup"><span data-stu-id="a4825-123">The filtered movies are displayed.</span></span>

![SearchQryStr](adding-search/_static/image1.png)

<span data-ttu-id="a4825-125">Wenn Sie die Signatur der Ändern der `Index` Methode zu einem Parameter mit dem Namen `id`, `id` Parameter entspricht der `{id}` Platzhalter für den standardmäßigen leitet Menge in der *App\_Start RouteConfig.cs* Datei.</span><span class="sxs-lookup"><span data-stu-id="a4825-125">If you change the signature of the `Index` method to have a parameter named `id`, the `id` parameter will match the `{id}` placeholder for the default routes set in the *App\_Start\RouteConfig.cs* file.</span></span>

[!code-json[Main](adding-search/samples/sample4.json)]

<span data-ttu-id="a4825-126">Die ursprüngliche `Index` -Methode sieht folgendermaßen aus:</span><span class="sxs-lookup"><span data-stu-id="a4825-126">The original `Index` method looks like this::</span></span>

[!code-csharp[Main](adding-search/samples/sample5.cs)]

<span data-ttu-id="a4825-127">Das geänderte `Index` Methode würde wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="a4825-127">The modified `Index` method would look as follows:</span></span>

[!code-csharp[Main](adding-search/samples/sample6.cs?highlight=1,3)]

<span data-ttu-id="a4825-128">Sie können nun den Suchtitel als Routendaten (ein URL-Segment) anstatt als Wert einer Abfragezeichenfolge übergeben.</span><span class="sxs-lookup"><span data-stu-id="a4825-128">You can now pass the search title as route data (a URL segment) instead of as a query string value.</span></span>

![](adding-search/_static/image2.png)

<span data-ttu-id="a4825-129">Sie können jedoch von den Benutzern nicht erwarten, dass sie jedes Mal die URL ändern, wenn sie nach einem Film suchen möchten.</span><span class="sxs-lookup"><span data-stu-id="a4825-129">However, you can't expect users to modify the URL every time they want to search for a movie.</span></span> <span data-ttu-id="a4825-130">Ja, nachdem Sie Hilfe der Benutzeroberfläche hinzufügen, sie filtern, Filme.</span><span class="sxs-lookup"><span data-stu-id="a4825-130">So now you you'll add UI to help them filter movies.</span></span> <span data-ttu-id="a4825-131">Wenn Sie die Signatur der geändert und die `Index` IMM Methode zum Testen wie die Route datengebundenen-ID-Parameter übergeben, damit Ihre `Index` Methode nimmt einen Zeichenfolgenparameter, der mit dem Namen `searchString`:</span><span class="sxs-lookup"><span data-stu-id="a4825-131">If you changed the signature of the `Index` method to test how to pass the route-bound ID parameter, change it back so that your `Index` method takes a string parameter named `searchString`:</span></span>

[!code-csharp[Main](adding-search/samples/sample7.cs)]

<span data-ttu-id="a4825-132">Öffnen der *Views\Movies\Index.cshtml* , und stellen Sie direkt nach `@Html.ActionLink("Create New", "Create")`, fügen Sie die hervorgehobenen Formularmarkup hinzu:</span><span class="sxs-lookup"><span data-stu-id="a4825-132">Open the *Views\Movies\Index.cshtml* file, and just after `@Html.ActionLink("Create New", "Create")`, add the form markup highlighted below:</span></span>

[!code-cshtml[Main](adding-search/samples/sample8.cshtml?highlight=12-15)]

<span data-ttu-id="a4825-133">Die `Html.BeginForm` Hilfsprogramm erstellt ein öffnendes `<form>` Tag.</span><span class="sxs-lookup"><span data-stu-id="a4825-133">The `Html.BeginForm` helper creates an opening `<form>` tag.</span></span> <span data-ttu-id="a4825-134">Die `Html.BeginForm` Helper bewirkt, dass das Formular an sich selbst zu senden, wenn der Benutzer durch Klicken auf das Formular sendet die **Filter** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="a4825-134">The `Html.BeginForm` helper causes the form to post to itself when the user submits the form by clicking the **Filter** button.</span></span>

<span data-ttu-id="a4825-135">Visual Studio 2013 ist eine gute Verbesserung bei der Anzeige und Bearbeitung von Dateien anzeigen.</span><span class="sxs-lookup"><span data-stu-id="a4825-135">Visual Studio 2013 has a nice improvement when displaying and editing View files.</span></span> <span data-ttu-id="a4825-136">Wenn Sie die Anwendung mit einer Ansichtsdatei Öffnen ausführen, ruft Visual Studio 2013 die richtige Controlleraktionsmethode um die Ansicht anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="a4825-136">When you run the application with a view file open, Visual Studio 2013 invokes the correct controller action method to display the view.</span></span>

![](adding-search/_static/image3.png)

<span data-ttu-id="a4825-137">Klicken Sie mit die Indexansicht in Visual Studio öffnen Sie, (wie in der Abbildung oben gezeigt), und tippen Sie auf Ctr F5 oder F5, um die Anwendung auszuführen, und wiederholen dann einen Film gesucht.</span><span class="sxs-lookup"><span data-stu-id="a4825-137">With the Index view open in Visual Studio (as shown in the image above), tap Ctr F5 or F5 to run the application and then try searching for a movie.</span></span>

![](adding-search/_static/image4.png)

<span data-ttu-id="a4825-138">Es ist keine `HttpPost` Überladung von der `Index` Methode.</span><span class="sxs-lookup"><span data-stu-id="a4825-138">There's no `HttpPost` overload of the `Index` method.</span></span> <span data-ttu-id="a4825-139">Nicht erforderlich, da die Methode ändern den Status der Anwendung ist nicht nur das Filtern von Daten.</span><span class="sxs-lookup"><span data-stu-id="a4825-139">You don't need it, because the method isn't changing the state of the application, just filtering data.</span></span>

<span data-ttu-id="a4825-140">Sie können die folgende `HttpPost Index`-Methode hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="a4825-140">You could add the following `HttpPost Index` method.</span></span> <span data-ttu-id="a4825-141">In diesem Fall würde die Instanz zum Aufrufen der Aktion übereinstimmen der `HttpPost Index` -Methode, und die `HttpPost Index` Methode würde ausgeführt, wie in der folgenden Abbildung dargestellt.</span><span class="sxs-lookup"><span data-stu-id="a4825-141">In that case, the action invoker would match the `HttpPost Index` method, and the `HttpPost Index` method would run as shown in the image below.</span></span>

[!code-csharp[Main](adding-search/samples/sample9.cs)]

![SearchPostGhost](adding-search/_static/image5.png)

<span data-ttu-id="a4825-143">Doch selbst wenn Sie diese `HttpPost`-Version der `Index`-Methode hinzufügen, gibt es eine Einschränkung für die gesamte Implementierung.</span><span class="sxs-lookup"><span data-stu-id="a4825-143">However, even if you add this `HttpPost` version of the `Index` method, there's a limitation in how this has all been implemented.</span></span> <span data-ttu-id="a4825-144">Stellen Sie sich vor, Sie möchten eine bestimmte Suche als Favorit speichern oder einen Link an Freunde senden, auf den diese klicken können, um dieselbe gefilterte Liste von Filmen anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="a4825-144">Imagine that you want to bookmark a particular search or you want to send a link to friends that they can click in order to see the same filtered list of movies.</span></span> <span data-ttu-id="a4825-145">Beachten Sie, dass die URL für die HTTP POST-Anforderung identisch mit der URL für die GET-Anforderung (Localhost:xxxxx/Filme/Index ist) – es keine Suchlaufinformationen Sie in der URL selbst sind.</span><span class="sxs-lookup"><span data-stu-id="a4825-145">Notice that the URL for the HTTP POST request is the same as the URL for the GET request (localhost:xxxxx/Movies/Index) -- there's no search information in the URL itself.</span></span> <span data-ttu-id="a4825-146">Rechts wird nun Informationen für die Suche an den Server als Wert eines Formulars gesendet.</span><span class="sxs-lookup"><span data-stu-id="a4825-146">Right now, the search string information is sent to the server as a form field value.</span></span> <span data-ttu-id="a4825-147">Dies bedeutet, dass Sie erfasst werden können, Suchinformationen Lesezeichen oder zum Hinzufügen von Freunden in einer URL senden.</span><span class="sxs-lookup"><span data-stu-id="a4825-147">This means you can't capture that search information to bookmark or send to friends in a URL.</span></span>

<span data-ttu-id="a4825-148">Die Lösung besteht darin, eine Überladung der `BeginForm` , der angibt, dass die POST-Anforderung die Suchinformationen URL hinzugefügt werden soll, und dass sie weitergeleitet werden soll die `HttpGet` Version der `Index` Methode.</span><span class="sxs-lookup"><span data-stu-id="a4825-148">The solution is to use an overload of `BeginForm` that specifies that the POST request should add the search information to the URL and that it should be routed to the `HttpGet` version of the `Index` method.</span></span> <span data-ttu-id="a4825-149">Ersetzen Sie die vorhandene parameterlosen `BeginForm` -Methode durch Folgendes Markup:</span><span class="sxs-lookup"><span data-stu-id="a4825-149">Replace the existing parameterless `BeginForm` method with the following markup:</span></span>

[!code-cshtml[Main](adding-search/samples/sample10.cshtml)]

![BeginFormPost_SM](adding-search/_static/image6.png)

<span data-ttu-id="a4825-151">Wenn Sie eine Suche übermitteln, enthält die URL nun eine Abfrage zu suchende Zeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="a4825-151">Now when you submit a search, the URL contains a search query string.</span></span> <span data-ttu-id="a4825-152">Das Suchen wird auch an Aktionsmethode `HttpGet Index` übertragen, auch wenn Sie eine `HttpPost Index`-Methode haben.</span><span class="sxs-lookup"><span data-stu-id="a4825-152">Searching will also go to the `HttpGet Index` action method, even if you have a `HttpPost Index` method.</span></span>

![IndexWithGetURL](adding-search/_static/image7.png)

## <a name="adding-search-by-genre"></a><span data-ttu-id="a4825-154">Suche nach "Genre" hinzufügen</span><span class="sxs-lookup"><span data-stu-id="a4825-154">Adding Search by Genre</span></span>

<span data-ttu-id="a4825-155">Wenn Sie hinzugefügt haben die `HttpPost` Version der `Index` -Methode, löschen Sie es jetzt.</span><span class="sxs-lookup"><span data-stu-id="a4825-155">If you added the `HttpPost` version of the `Index` method, delete it now.</span></span>

<span data-ttu-id="a4825-156">Als Nächstes fügen Sie eine Funktion zum Benutzer, die für Filme nach Genre suchen können.</span><span class="sxs-lookup"><span data-stu-id="a4825-156">Next, you'll add a feature to let users search for movies by genre.</span></span> <span data-ttu-id="a4825-157">Ersetzen Sie die `Index`-Methode durch folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="a4825-157">Replace the `Index` method with the following code:</span></span>

[!code-csharp[Main](adding-search/samples/sample11.cs)]

<span data-ttu-id="a4825-158">Diese Version von den `Index` Methode nimmt einen zusätzlichen Parameter, nämlich `movieGenre`.</span><span class="sxs-lookup"><span data-stu-id="a4825-158">This version of the `Index` method takes an additional parameter, namely `movieGenre`.</span></span> <span data-ttu-id="a4825-159">Erstellen Sie die ersten Zeilen des Codes eine `List` Objekt um Filmgenres aus der Datenbank zu speichern.</span><span class="sxs-lookup"><span data-stu-id="a4825-159">The first few lines of code create a `List` object to hold movie genres from the database.</span></span>

<span data-ttu-id="a4825-160">Der folgende Code ist eine LINQ-Abfrage, die alle Genres aus der Datenbank abruft.</span><span class="sxs-lookup"><span data-stu-id="a4825-160">The following code is a LINQ query that retrieves all the genres from the database.</span></span>

[!code-csharp[Main](adding-search/samples/sample12.cs)]

<span data-ttu-id="a4825-161">Der Code verwendet die `AddRange` der generischen Methode `List` Auflistung, die unterschiedliche Genres zur Liste hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="a4825-161">The code uses the `AddRange` method of the generic `List` collection to add all the distinct genres to the list.</span></span> <span data-ttu-id="a4825-162">(Ohne die `Distinct` Modifizierer, doppelte Genres würde hinzugefügt – zweimal in unserem Beispiel würde z. B. Comedy hinzugefügt werden).</span><span class="sxs-lookup"><span data-stu-id="a4825-162">(Without the `Distinct` modifier, duplicate genres would be added — for example, comedy would be added twice in our sample).</span></span> <span data-ttu-id="a4825-163">Der Code speichert dann die Liste von Genres in die `ViewBag.MovieGenre` Objekt.</span><span class="sxs-lookup"><span data-stu-id="a4825-163">The code then stores the list of genres in the `ViewBag.MovieGenre` object.</span></span> <span data-ttu-id="a4825-164">Speichern von Daten (solche einen Film "Genre" des) als Kategorie ein [SelectList](https://msdn.microsoft.com/en-us/library/system.web.mvc.selectlist(v=vs.108).aspx) -Objekt in ein `ViewBag`, und klicken Sie dann den Zugriff auf die Daten der Kategorie in einem Dropdown-Listenfeld ein typischer Ansatz für MVC-Anwendungen ist.</span><span class="sxs-lookup"><span data-stu-id="a4825-164">Storing category data (such a movie genre's) as a [SelectList](https://msdn.microsoft.com/en-us/library/system.web.mvc.selectlist(v=vs.108).aspx) object in a `ViewBag`, then accessing the category data in a dropdown list box is a typical approach for MVC applications.</span></span>

<span data-ttu-id="a4825-165">Der folgende Code zeigt die Vorgehensweise beim Überprüfen der `movieGenre` Parameter.</span><span class="sxs-lookup"><span data-stu-id="a4825-165">The following code shows how to check the `movieGenre` parameter.</span></span> <span data-ttu-id="a4825-166">Wenn er nicht leer ist, schränkt der Code weiter Filme Abfrage ausgewählten Filme an der angegebenen "Genre" beschränken.</span><span class="sxs-lookup"><span data-stu-id="a4825-166">If it's not empty, the code further constrains the movies query to limit the selected movies to the specified genre.</span></span>

[!code-csharp[Main](adding-search/samples/sample13.cs)]

<span data-ttu-id="a4825-167">Wie bereits erwähnt, die Abfrage wird nicht ausgeführt auf der Basis der Daten, bis die Filmliste in einer Schleife durchlaufen wird (das geschieht in der Ansicht nach der `Index` Aktionsmethode zurückgegeben).</span><span class="sxs-lookup"><span data-stu-id="a4825-167">As stated previously, the query is not run on the data base until the movie list is iterated over (which happens in the View, after the `Index` action method returns).</span></span>

## <a name="adding-markup-to-the-index-view-to-support-search-by-genre"></a><span data-ttu-id="a4825-168">Die Indexansicht zur Unterstützung der Suche nach "Genre" Markup hinzufügen</span><span class="sxs-lookup"><span data-stu-id="a4825-168">Adding Markup to the Index View to Support Search by Genre</span></span>

<span data-ttu-id="a4825-169">Hinzufügen einer `Html.DropDownList` Hilfsmethode zum der *Views\Movies\Index.cshtml* Datei, direkt vor der `TextBox` Helper.</span><span class="sxs-lookup"><span data-stu-id="a4825-169">Add an `Html.DropDownList` helper to the *Views\Movies\Index.cshtml* file, just before the `TextBox` helper.</span></span> <span data-ttu-id="a4825-170">Das vollständige Markup wird unten gezeigt:</span><span class="sxs-lookup"><span data-stu-id="a4825-170">The completed markup is shown below:</span></span>

[!code-cshtml[Main](adding-search/samples/sample14.cshtml?highlight=11)]

<span data-ttu-id="a4825-171">Im folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="a4825-171">In the following code:</span></span>

[!code-cshtml[Main](adding-search/samples/sample15.cshtml)]

<span data-ttu-id="a4825-172">Der Parameter "MovieGenre" stellt den Schlüssel für die `DropDownList` Hilfsmethode zum Suchen einer `IEnumerable<SelectListItem>` in der `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="a4825-172">The parameter "MovieGenre" provides the key for the `DropDownList` helper to find a `IEnumerable<SelectListItem>` in the `ViewBag`.</span></span> <span data-ttu-id="a4825-173">Die `ViewBag` wurde in der Aktionsmethode aufgefüllt:</span><span class="sxs-lookup"><span data-stu-id="a4825-173">The `ViewBag` was populated in the action method:</span></span>

[!code-csharp[Main](adding-search/samples/sample16.cs?highlight=10)]

<span data-ttu-id="a4825-174">Der Parameter "All" stellt eine optionsbezeichnung bereit.</span><span class="sxs-lookup"><span data-stu-id="a4825-174">The parameter "All" provides an option label.</span></span> <span data-ttu-id="a4825-175">Wenn Sie diese Auswahl in Ihrem Browser überprüfen, sehen Sie sich, dass ihr Attribut "Value" leer ist.</span><span class="sxs-lookup"><span data-stu-id="a4825-175">If you inspect that choice in your browser, you'll see that its "value" attribute is empty.</span></span> <span data-ttu-id="a4825-176">Da unser Controller nur filtert `if` die Zeichenfolge ist kein `null` oder leer ist, senden einen leeren Wert `movieGenre` zeigt alle Genres.</span><span class="sxs-lookup"><span data-stu-id="a4825-176">Since our controller only filters `if` the string is not `null` or empty, submitting an empty value for `movieGenre` shows all genres.</span></span>

<span data-ttu-id="a4825-177">Sie können auch eine Option in der Standardeinstellung ausgewählt werden festlegen.</span><span class="sxs-lookup"><span data-stu-id="a4825-177">You can also set an option to be selected by default.</span></span> <span data-ttu-id="a4825-178">Wenn Sie "Comedy" als Standardwert verwenden möchten, ändern Sie den Code im Controller wie folgt:</span><span class="sxs-lookup"><span data-stu-id="a4825-178">If you wanted "Comedy" as your default option, you would change the code in the Controller like so:</span></span>

[!code-cshtml[Main](adding-search/samples/sample17.cshtml)]

<span data-ttu-id="a4825-179">Führen Sie die Anwendung, und navigieren Sie zu */Filme/Index*.</span><span class="sxs-lookup"><span data-stu-id="a4825-179">Run the application and browse to */Movies/Index*.</span></span> <span data-ttu-id="a4825-180">Versuchen Sie eine Suche durch "Genre" Movie namentlich und von beiden Kriterien.</span><span class="sxs-lookup"><span data-stu-id="a4825-180">Try a search by genre, by movie name, and by both criteria.</span></span>

![](adding-search/_static/image8.png)

<span data-ttu-id="a4825-181">In diesem Abschnitt erstellt Sie eine Aktionsmethode suchen und anzeigen, mit denen Benutzer anhand der Filmtitel und die "Genre" suchen kann.</span><span class="sxs-lookup"><span data-stu-id="a4825-181">In this section you created a search action method and view that let users search by movie title and genre.</span></span> <span data-ttu-id="a4825-182">Im nächsten Abschnitt, betrachten Sie zum Hinzufügen einer Eigenschaft, um die `Movie` Modell und wie Sie einen Initialisierer hinzufügen, die automatisch eine Testdatenbank erstellen.</span><span class="sxs-lookup"><span data-stu-id="a4825-182">In the next section, you'll look at how to add a property to the `Movie` model and how to add an initializer that will automatically create a test database.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="a4825-183">[Zurück](examining-the-edit-methods-and-edit-view.md)
[Weiter](adding-a-new-field.md)</span><span class="sxs-lookup"><span data-stu-id="a4825-183">[Previous](examining-the-edit-methods-and-edit-view.md)
[Next](adding-a-new-field.md)</span></span>
