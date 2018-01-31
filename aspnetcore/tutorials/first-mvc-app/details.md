---
title: "Untersuchen der Methoden „Details“ und „Delete“"
author: rick-anderson
description: "Die Controllermethode „Details“ und Ansicht in einer einfachen ASP.NET Core MVC-App."
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/details
ms.openlocfilehash: 4a0004fc79f8e1d334e3acb96b28b2954d19f0a1
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
---
# <a name="examining-the-details-and-delete-methods"></a><span data-ttu-id="1c31c-103">Untersuchen der Methoden „Details“ und „Delete“</span><span class="sxs-lookup"><span data-stu-id="1c31c-103">Examining the Details and Delete methods</span></span>

<span data-ttu-id="1c31c-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1c31c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1c31c-105">Öffnen Sie den „Movie“-Controller, und untersuchen Sie die `Details`-Methode.</span><span class="sxs-lookup"><span data-stu-id="1c31c-105">Open the Movie controller and examine the `Details` method:</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_details)]

<span data-ttu-id="1c31c-106">Das MVC-Gerüstbaumodul, das diese Aktionsmethode erstellt hat, fügt einen Kommentar mit einer HTTP-Anforderung hinzu, die die Methode aufruft.</span><span class="sxs-lookup"><span data-stu-id="1c31c-106">The MVC scaffolding engine that created this action method adds a comment showing an HTTP request that invokes the method.</span></span> <span data-ttu-id="1c31c-107">In diesem Fall ist dies eine GET-Anforderung mit drei URL-Segmenten: dem Controller `Movies`, der Methode `Details` und dem Wert von `id`.</span><span class="sxs-lookup"><span data-stu-id="1c31c-107">In this case it's a GET request with three URL segments, the `Movies` controller, the `Details` method and an `id` value.</span></span> <span data-ttu-id="1c31c-108">Wie bereits erwähnt, werden diese Segmente in *Startup.cs* definiert.</span><span class="sxs-lookup"><span data-stu-id="1c31c-108">Recall these segments are defined in *Startup.cs*.</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

<span data-ttu-id="1c31c-109">EF erleichtert das Suchen nach Daten mithilfe der `SingleOrDefaultAsync`-Methode.</span><span class="sxs-lookup"><span data-stu-id="1c31c-109">EF makes it easy to search for data using the `SingleOrDefaultAsync` method.</span></span> <span data-ttu-id="1c31c-110">Ein wichtiges in die Methode integriertes Sicherheitsmerkmal ist, dass der Code überprüft, ob die Suchmethode einen Film gefunden hat, bevor sie versucht, etwas damit zu tun.</span><span class="sxs-lookup"><span data-stu-id="1c31c-110">An important security feature built into the method is that the code verifies that the search method has found a movie before it tries to do anything with it.</span></span> <span data-ttu-id="1c31c-111">Ein Hacker kann beispielsweise Fehler in die Website einschleusen, indem die von den Links erstellte URL von `http://localhost:xxxx/Movies/Details/1` in etwas wie `http://localhost:xxxx/Movies/Details/12345` geändert wird (oder einen anderen Wert, der keinen tatsächlichen Film darstellt).</span><span class="sxs-lookup"><span data-stu-id="1c31c-111">For example, a hacker could introduce errors into the site by changing the URL created by the links from `http://localhost:xxxx/Movies/Details/1` to something like  `http://localhost:xxxx/Movies/Details/12345` (or some other value that doesn't represent an actual movie).</span></span> <span data-ttu-id="1c31c-112">Wenn Sie nicht nach einem NULL-Movie gesucht haben, löst die App keine Ausnahme aus.</span><span class="sxs-lookup"><span data-stu-id="1c31c-112">If you didn't check for a null movie, the app would throw an exception.</span></span>

<span data-ttu-id="1c31c-113">Untersuchen Sie die Methoden `Delete` und `DeleteConfirmed`.</span><span class="sxs-lookup"><span data-stu-id="1c31c-113">Examine the `Delete` and `DeleteConfirmed` methods.</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete)]

<span data-ttu-id="1c31c-114">Beachten Sie, dass die `HTTP GET Delete`-Methode den angegebenen Film nicht löscht, sondern eine Ansicht des Films zurückgibt, in der Sie den Löschvorgang (HttpPost) auslösen können.</span><span class="sxs-lookup"><span data-stu-id="1c31c-114">Note that the `HTTP GET Delete` method doesn't delete the specified movie, it returns a view of the movie where you can submit (HttpPost) the deletion.</span></span> <span data-ttu-id="1c31c-115">Das Ausführen eines Löschvorgangs als Reaktion auf eine GET-Anforderung (oder eigentlich eines Bearbeitungs-, Erstellungs- oder sonstigen Vorgangs, der Daten ändern) stellt eine Sicherheitslücke dar.</span><span class="sxs-lookup"><span data-stu-id="1c31c-115">Performing a delete operation in response to a GET request (or for that matter, performing an edit operation, create operation, or any other operation that changes data) opens up a security hole.</span></span>

<span data-ttu-id="1c31c-116">Die `[HttpPost]`-Methode, die die Daten löscht, heißt `DeleteConfirmed`, um der HTTP-POST-Methode eine eindeutige Signatur bzw. einen eindeutigen Namen zu geben.</span><span class="sxs-lookup"><span data-stu-id="1c31c-116">The `[HttpPost]` method that deletes the data is named `DeleteConfirmed` to give the HTTP POST method a unique signature or name.</span></span> <span data-ttu-id="1c31c-117">Die beiden Methodensignaturen werden nachstehend gezeigt:</span><span class="sxs-lookup"><span data-stu-id="1c31c-117">The two method signatures are shown below:</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete2)]

[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete3)]


<span data-ttu-id="1c31c-118">Die Common Language Runtime (CLR) erfordert überladene Methoden, um eine eindeutige Parametersignatur zu erhalten (selber Methodenname, aber unterschiedliche Liste von Parametern).</span><span class="sxs-lookup"><span data-stu-id="1c31c-118">The common language runtime (CLR) requires overloaded methods to have a unique parameter signature (same method name but different list of parameters).</span></span> <span data-ttu-id="1c31c-119">Hier benötigen Sie jedoch zwei `Delete`-Methoden (eine für GET und eine für POST), die beide die gleiche Parametersignatur haben.</span><span class="sxs-lookup"><span data-stu-id="1c31c-119">However, here you need two `Delete` methods -- one for GET and one for POST -- that both have the same parameter signature.</span></span> <span data-ttu-id="1c31c-120">(Beide müssen eine einzelne ganze Zahl als Parameter akzeptieren.)</span><span class="sxs-lookup"><span data-stu-id="1c31c-120">(They both need to accept a single integer as a parameter.)</span></span>

<span data-ttu-id="1c31c-121">Es gibt zwei Ansätze zur Lösung dieses Problems. Eine besteht darin, die Methoden unterschiedlich zu benennen.</span><span class="sxs-lookup"><span data-stu-id="1c31c-121">There are two approaches to this problem, one is to give the methods different names.</span></span> <span data-ttu-id="1c31c-122">Dafür das der Gerüstbaumechanismus im vorherigen Beispiel gesorgt.</span><span class="sxs-lookup"><span data-stu-id="1c31c-122">That's what the scaffolding mechanism did in the preceding example.</span></span> <span data-ttu-id="1c31c-123">Dies bringt jedoch ein kleines Problem mit sich: ASP.NET ordnet Segmente einer URL anhand des Namens zu Aktionsmethoden zu. Wenn Sie die Methode umbenennen sollten, ist das Routing normalerweise nicht in der Lage, diese Methode zu finden.</span><span class="sxs-lookup"><span data-stu-id="1c31c-123">However, this introduces a small problem: ASP.NET maps segments of a URL to action methods by name, and if you rename a method, routing normally wouldn't be able to find that method.</span></span> <span data-ttu-id="1c31c-124">Die Lösung besteht (wie im Beispiel) im Hinzufügen des `ActionName("Delete")`-Attributs zur `DeleteConfirmed`-Methode.</span><span class="sxs-lookup"><span data-stu-id="1c31c-124">The solution is what you see in the example, which is to add the `ActionName("Delete")` attribute to the `DeleteConfirmed` method.</span></span> <span data-ttu-id="1c31c-125">Dieses Attribut führt die Zuordnung für das Routingsystem durch, sodass eine URL, die „/Delete/“ für eine POST-Anforderung enthält, die `DeleteConfirmed`-Methode findet.</span><span class="sxs-lookup"><span data-stu-id="1c31c-125">That attribute performs mapping for the routing system so that a URL that includes /Delete/ for a POST request will find the `DeleteConfirmed` method.</span></span>

<span data-ttu-id="1c31c-126">Eine weitere gängige Behelfslösung für Methoden mit identischen Namen und Signaturen ist das künstliche Ändern der POST-Methode durch Hinzufügen eines zusätzlichen (nicht verwendeten) Parameters.</span><span class="sxs-lookup"><span data-stu-id="1c31c-126">Another common work around for methods that have identical names and signatures is to artificially change the signature of the POST method to include an extra (unused) parameter.</span></span> <span data-ttu-id="1c31c-127">Siehe dazu die vorherige POST-Anforderung, der wir den `notUsed`-Parameter hinzugefügt haben.</span><span class="sxs-lookup"><span data-stu-id="1c31c-127">That's what we did in a previous post when we added the `notUsed` parameter.</span></span> <span data-ttu-id="1c31c-128">Sie können dasselbe hier für die `[HttpPost] Delete`-Methode ausführen:</span><span class="sxs-lookup"><span data-stu-id="1c31c-128">You could do the same thing here for the `[HttpPost] Delete` method:</span></span>

```csharp
// POST: Movies/Delete/6
[ValidateAntiForgeryToken]
public async Task<IActionResult> Delete(int id, bool notUsed)
```

### <a name="publish-to-azure"></a><span data-ttu-id="1c31c-129">Veröffentlichen in Azure</span><span class="sxs-lookup"><span data-stu-id="1c31c-129">Publish to Azure</span></span>

<span data-ttu-id="1c31c-130">Anweisungen zum Veröffentlichen dieser App in Azure mit Visual Studio finden Sie unter [Veröffentlichen einer ASP.NET Core-Web-App in Azure App Service mit Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs).</span><span class="sxs-lookup"><span data-stu-id="1c31c-130">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on how to publish this app to Azure using Visual Studio.</span></span>  <span data-ttu-id="1c31c-131">Die App kann auch über die [Befehlszeile](xref:tutorials/publish-to-azure-webapp-using-cli) veröffentlicht werden.</span><span class="sxs-lookup"><span data-stu-id="1c31c-131">The app can also be published from the [command line](xref:tutorials/publish-to-azure-webapp-using-cli).</span></span>

<span data-ttu-id="1c31c-132">Vielen Dank für Ihr Interesse an dieser Einführung in ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="1c31c-132">Thanks for completing this introduction to ASP.NET Core MVC.</span></span> <span data-ttu-id="1c31c-133">Wir freuen uns über Kommentare, die Sie hinterlassen.</span><span class="sxs-lookup"><span data-stu-id="1c31c-133">We appreciate any comments you leave.</span></span> <span data-ttu-id="1c31c-134">[Erste Schritte mit MVC und EF Core](xref:data/ef-mvc/intro) ist ein ausgezeichneter Anschlussartikel an dieses Tutorial.</span><span class="sxs-lookup"><span data-stu-id="1c31c-134">[Getting started with MVC and EF Core](xref:data/ef-mvc/intro) is an excellent follow up to this tutorial.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="1c31c-135">Vorherige</span><span class="sxs-lookup"><span data-stu-id="1c31c-135">Previous</span></span>](validation.md)
