---
title: "Hinzufügen der Suche zu ASP.NET Core Razor-Seiten"
author: rick-anderson
description: "Informationen zum Hinzufügen der Suche zu ASP.NET Core Razor-Seiten"
keywords: ASP.NET Core, Suche, Razor-Seiten
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/razor-pages/search
ms.openlocfilehash: 2ffb6f13a7303527444085d137d1acac02d7e0ef
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/28/2017
---
# <a name="adding-search-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="93cc7-104">Hinzufügen der Suche zu einer ASP.NET Core MVC-App</span><span class="sxs-lookup"><span data-stu-id="93cc7-104">Adding search to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="93cc7-105">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="93cc7-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="93cc7-106">In diesem Dokument wird der Indexseite die Suchfunktion hinzugefügt, die das Suchen von Filmen nach *Genre* oder *Namen* ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="93cc7-106">In this document, search capability is added to the Index page that enables searching movies by *genre* or *name*.</span></span>

<span data-ttu-id="93cc7-107">Aktualisieren Sie die `OnGetAsync`-Methode der Indexseite mit folgendem Code:</span><span class="sxs-lookup"><span data-stu-id="93cc7-107">Update the Index page's `OnGetAsync` method with the following code:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_1stSearch)]

<span data-ttu-id="93cc7-108">Die erste Zeile der `OnGetAsync`-Methode erstellt eine [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/)-Abfrage zum Auswählen der Filme:</span><span class="sxs-lookup"><span data-stu-id="93cc7-108">The first line of the `OnGetAsync` method creates a [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/) query to select the movies:</span></span>

```csharp
 var movies = from m in _context.Movie
              select m;
```

<span data-ttu-id="93cc7-109">Die Abfrage wird an dieser Stelle *nur* definiert und **nicht** auf die Datenbank angewendet.</span><span class="sxs-lookup"><span data-stu-id="93cc7-109">The query is *only* defined at this point, it has **not** been run against the database.</span></span>

<span data-ttu-id="93cc7-110">Wenn der `searchString`-Parameter eine Zeichenfolge enthält, wird die Filmabfrage so geändert, dass die Suchzeichenfolge gefiltert wird:</span><span class="sxs-lookup"><span data-stu-id="93cc7-110">If the `searchString` parameter contains a string, the movies query is modified to filter on the search string:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchNull)]

<span data-ttu-id="93cc7-111">Der Code `s => s.Title.Contains()` ist ein [Lambdaausdruck](https://docs.microsoft.com/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span><span class="sxs-lookup"><span data-stu-id="93cc7-111">The `s => s.Title.Contains()` code is a [Lambda Expression](https://docs.microsoft.com/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="93cc7-112">Lambdaausdrücke werden in methodenbasierten [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/)-Abfragen als Argumente für standardmäßige Abfrageoperatormethoden wie die [Where](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq)-Methode oder `Contains` verwendet (siehe den vorangehenden Code).</span><span class="sxs-lookup"><span data-stu-id="93cc7-112">Lambdas are used in method-based [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/) queries as arguments to standard query operator methods such as the [Where](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) method or `Contains` (used in the preceding code).</span></span> <span data-ttu-id="93cc7-113">LINQ-Abfragen werden nicht ausgeführt, wenn sie definiert oder durch Aufrufen einer Methode geändert werden (z.B. `Where`, `Contains` oder `OrderBy`).</span><span class="sxs-lookup"><span data-stu-id="93cc7-113">LINQ queries are not executed when they are defined or when they are modified by calling a method (such as `Where`, `Contains`  or `OrderBy`).</span></span> <span data-ttu-id="93cc7-114">Stattdessen wird die Ausführung der Abfrage verzögert.</span><span class="sxs-lookup"><span data-stu-id="93cc7-114">Rather, query execution is deferred.</span></span> <span data-ttu-id="93cc7-115">Dies bedeutet, dass die Auswertung eines Ausdrucks so lange hinausgezögert wird, bis dessen realisierter Wert durchlaufen oder die `ToListAsync`-Methode aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="93cc7-115">That means the evaluation of an expression is delayed until its realized value is iterated over or the `ToListAsync` method is called.</span></span> <span data-ttu-id="93cc7-116">Weitere Informationen finden Sie unter [Abfrageausführung](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/language-reference/query-execution).</span><span class="sxs-lookup"><span data-stu-id="93cc7-116">See [Query Execution](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/language-reference/query-execution) for more information.</span></span>

<span data-ttu-id="93cc7-117">**Hinweis:** Die [Contains](https://docs.microsoft.com//dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains)-Methode wird in der Datenbank und nicht im C#-Code ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="93cc7-117">**Note:** The [Contains](https://docs.microsoft.com//dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) method is run on the database, not in the C# code.</span></span> <span data-ttu-id="93cc7-118">Die Groß-/Kleinschreibung in der Abfrage hängt von der Datenbank und Sortierung ab.</span><span class="sxs-lookup"><span data-stu-id="93cc7-118">The case sensitivity on the query depends on the database and the collation.</span></span> <span data-ttu-id="93cc7-119">In SQL Server wird `Contains` zu [SQL LIKE](https://docs.microsoft.com/sql/t-sql/language-elements/like-transact-sql) zugeordnet, das Groß-/Kleinschreibung nicht beachtet.</span><span class="sxs-lookup"><span data-stu-id="93cc7-119">On SQL Server, `Contains` maps to [SQL LIKE](https://docs.microsoft.com/sql/t-sql/language-elements/like-transact-sql), which is case insensitive.</span></span> <span data-ttu-id="93cc7-120">In SQLite wird bei der Standardsortierung Groß-/Kleinschreibung beachtet.</span><span class="sxs-lookup"><span data-stu-id="93cc7-120">In SQLite, with the default collation, it's case sensitive.</span></span>

<span data-ttu-id="93cc7-121">Navigieren Sie zur Seite „Movies“, und fügen Sie eine Abfragezeichenfolge wie z.B. `?searchString=Ghost` an die URL an (z.B. `http://localhost:5000/Movies?searchString=Ghost`).</span><span class="sxs-lookup"><span data-stu-id="93cc7-121">Navigate to the Movies page and append a query string such as `?searchString=Ghost` to the URL (for example, `http://localhost:5000/Movies?searchString=Ghost`).</span></span> <span data-ttu-id="93cc7-122">Die gefilterten Filme werden angezeigt.</span><span class="sxs-lookup"><span data-stu-id="93cc7-122">The filtered movies are displayed.</span></span>

![Indexansicht](search/_static/ghost.png)

<span data-ttu-id="93cc7-124">Wenn die Routenvorlage der Indexseite hinzugefügt wird, kann die Suchzeichenfolge als URL-Segment (z.B. `http://localhost:5000/Movies/ghost`) übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="93cc7-124">If the following route template is added to the Index page, the search string can be passed as a URL segment (for example, `http://localhost:5000/Movies/ghost`).</span></span>

```cshtml
@page "{searchString?}"
```

<span data-ttu-id="93cc7-125">Die vorangehende Routeneinschränkung ermöglicht das Suchen des Titels als Routendaten (URL-Segment) anstatt als Wert einer Abfragezeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="93cc7-125">The preceding route constraint allows searching the title as route data (a URL segment) instead of as a query string value.</span></span>  <span data-ttu-id="93cc7-126">Das `?` in `"{searchString?}"` bedeutet, dass dies ein optionaler Routenparameter ist.</span><span class="sxs-lookup"><span data-stu-id="93cc7-126">The `?` in `"{searchString?}"` means this is an optional route parameter.</span></span>

![Indexansicht mit dem der URL hinzugefügten Wort „ghost“ und einer zurückgegebenen Filmliste mit zwei Filmen: Ghostbusters und Ghostbusters 2](search/_static/g2.png)

<span data-ttu-id="93cc7-128">Sie können jedoch von den Benutzern nicht erwarten, dass sie die URL ändern, um nach einem Film zu suchen.</span><span class="sxs-lookup"><span data-stu-id="93cc7-128">However, you can't expect users to modify the URL to search for a movie.</span></span> <span data-ttu-id="93cc7-129">In diesem Schritt wird eine Benutzeroberflächenoption hinzugefügt, um Filme zu filtern.</span><span class="sxs-lookup"><span data-stu-id="93cc7-129">In this step, UI is added to filter movies.</span></span> <span data-ttu-id="93cc7-130">Wenn Sie die Routeneinschränkung `"{searchString?}"` hinzugefügt haben, entfernen Sie sie.</span><span class="sxs-lookup"><span data-stu-id="93cc7-130">If you added the route constraint `"{searchString?}"`, remove it.</span></span>

<span data-ttu-id="93cc7-131">Öffnen Sie die Datei *Pages/Movies/Index.cshtml*, und fügen Sie im folgenden Code das hervorgehobene `<form>`-Markup hinzu:</span><span class="sxs-lookup"><span data-stu-id="93cc7-131">Open the *Pages/Movies/Index.cshtml* file, and add the `<form>` markup highlighted in the following code:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index2.cshtml?highlight=14-19&range=1-22)]

<span data-ttu-id="93cc7-132">Das HTML-Tag `<form>` verwendet das [Hilfsprogramm für Formulartags](xref:mvc/views/working-with-forms#the-form-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="93cc7-132">The HTML `<form>` tag uses the [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="93cc7-133">Bei Übermittlung des Formulars wird die Filterzeichenfolge an die Seite *Pages/Movies/Index* gesendet.</span><span class="sxs-lookup"><span data-stu-id="93cc7-133">When the form is submitted, the filter string is sent to the *Pages/Movies/Index* page.</span></span> <span data-ttu-id="93cc7-134">Speichern Sie die Änderungen, und testen Sie den Filter.</span><span class="sxs-lookup"><span data-stu-id="93cc7-134">Save the changes and test the filter.</span></span>

![Indexansicht mit dem in das Filtertextfeld eingegebenen Wort „ghost“](search/_static/filter.png)

## <a name="search-by-genre"></a><span data-ttu-id="93cc7-136">Suche nach Genre</span><span class="sxs-lookup"><span data-stu-id="93cc7-136">Search by genre</span></span>

<span data-ttu-id="93cc7-137">Fügen Sie *Pages/Movies/Index.cshtml.cs* die folgenden hervorgehobenen Eigenschaften hinzu:</span><span class="sxs-lookup"><span data-stu-id="93cc7-137">Add the the following highlighted properties to *Pages/Movies/Index.cshtml.cs*:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-)]

<span data-ttu-id="93cc7-138">`SelectList Genres` enthält die Liste der Genres.</span><span class="sxs-lookup"><span data-stu-id="93cc7-138">The `SelectList Genres` contains the list of genres.</span></span> <span data-ttu-id="93cc7-139">Dies ermöglicht dem Benutzer, ein Genre in der Liste auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="93cc7-139">This allows the user to select a genre from the list.</span></span>

<span data-ttu-id="93cc7-140">Die `MovieGenre`-Eigenschaft enthält das spezifische Genre, das der Benutzer wählt (z.B. Western).</span><span class="sxs-lookup"><span data-stu-id="93cc7-140">The `MovieGenre` property contains the specific genre the user selects (for example, "Western").</span></span>

<span data-ttu-id="93cc7-141">Aktualisieren Sie die `OnGetAsync`-Methode mit folgendem Code:</span><span class="sxs-lookup"><span data-stu-id="93cc7-141">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchGenre)]

<span data-ttu-id="93cc7-142">Der folgende Code ist eine LINQ-Abfrage, die alle Genres aus der Datenbank abruft.</span><span class="sxs-lookup"><span data-stu-id="93cc7-142">The following code is a LINQ query that retrieves all the genres from the database.</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_LINQ)]

<span data-ttu-id="93cc7-143">Die `SelectList` von Genres wird durch Projizieren der unterschiedlichen Genres erstellt.</span><span class="sxs-lookup"><span data-stu-id="93cc7-143">The `SelectList` of genres is created by projecting the distinct genres.</span></span>

<!-- BUG in OPS
Tag snippet_selectlist's start line '75' should be less than end line '29' when resolving "[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]"

There is no start line.

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]
-->

```csharp
Genres = new SelectList(await genreQuery.Distinct().ToListAsync());
```

### <a name="adding-search-by-genre"></a><span data-ttu-id="93cc7-144">Hinzufügen einer Suche nach Genre</span><span class="sxs-lookup"><span data-stu-id="93cc7-144">Adding search by genre</span></span>

<span data-ttu-id="93cc7-145">Aktualisieren Sie *Index.cshtml* wie folgt:</span><span class="sxs-lookup"><span data-stu-id="93cc7-145">Update *Index.cshtml* as follows:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/IndexFormGenreNoRating.cshtml?highlight=16-18&range=1-26)]

<span data-ttu-id="93cc7-146">Testen Sie die App mit einer Suche nach Genre, Filmtitel und beidem.</span><span class="sxs-lookup"><span data-stu-id="93cc7-146">Test the app by searching by genre, by movie title, and by both.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="93cc7-147">[Zurück: Aktualisieren der Seiten](xref:tutorials/razor-pages/da1)
[Weiter: Hinzufügen eines neuen Felds](xref:tutorials/razor-pages/new-field)</span><span class="sxs-lookup"><span data-stu-id="93cc7-147">[Previous: Updating the pages](xref:tutorials/razor-pages/da1)
[Next: Adding a new field](xref:tutorials/razor-pages/new-field)</span></span>
