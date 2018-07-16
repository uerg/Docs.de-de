---
title: 'ASP.NET Core MVC mit EF Core: Sortieren, Filtern, Paging (3 von 10)'
author: rick-anderson
description: In diesem Tutorial fügen Sie mit ASP.NET Core und Entity Framework Core die Funktionen Sortieren, Filtern und Paging für das Paging hinzu.
ms.author: tdykstra
ms.date: 03/15/2017
uid: data/ef-mvc/sort-filter-page
ms.openlocfilehash: 1f80faf0e36332c28e8337ddc331cc8b4c4970d7
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38193948"
---
# <a name="aspnet-core-mvc-with-ef-core---sort-filter-paging---3-of-10"></a><span data-ttu-id="9cbee-103">ASP.NET Core MVC mit EF Core: Sortieren, Filtern, Paging (3 von 10)</span><span class="sxs-lookup"><span data-stu-id="9cbee-103">ASP.NET Core MVC with EF Core - Sort, Filter, Paging - 3 of 10</span></span>

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc-21.md)]

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="9cbee-104">Von [Tom Dykstra](https://github.com/tdykstra) und [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="9cbee-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="9cbee-105">Die Contoso University-Beispielwebanwendung veranschaulicht, wie ASP.NET Core MVC-Webanwendungen mit Entity Framework Core und Visual Studio erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="9cbee-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="9cbee-106">Informationen zu dieser Tutorialreihe finden Sie im [ersten Tutorial der Reihe](intro.md).</span><span class="sxs-lookup"><span data-stu-id="9cbee-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="9cbee-107">Im vorherigen Tutorial haben Sie eine Reihe von Webseiten für grundlegende CRUD-Vorgänge für Studentenentitäten implementiert.</span><span class="sxs-lookup"><span data-stu-id="9cbee-107">In the previous tutorial, you implemented a set of web pages for basic CRUD operations for Student entities.</span></span> <span data-ttu-id="9cbee-108">In diesem Tutorial fügen Sie die Funktionen zum Sortieren, Filtern und Paging zur Studentenindexseite hinzu.</span><span class="sxs-lookup"><span data-stu-id="9cbee-108">In this tutorial you'll add sorting, filtering, and paging functionality to the Students Index page.</span></span> <span data-ttu-id="9cbee-109">Sie werden auch eine Seite erstellen, auf der einfache Gruppierungsvorgänge ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="9cbee-109">You'll also create a page that does simple grouping.</span></span>

<span data-ttu-id="9cbee-110">Die folgende Abbildung zeigt, wie die Seite am Ende aussehen wird.</span><span class="sxs-lookup"><span data-stu-id="9cbee-110">The following illustration shows what the page will look like when you're done.</span></span> <span data-ttu-id="9cbee-111">Die Spaltenüberschriften sind Links, auf die der Benutzer klicken kann, um die Spalte zu sortieren.</span><span class="sxs-lookup"><span data-stu-id="9cbee-111">The column headings are links that the user can click to sort by that column.</span></span> <span data-ttu-id="9cbee-112">Wiederholtes Klicken auf eine Spaltenüberschrift schaltet zwischen aufsteigender und absteigender Sortierreihenfolge um.</span><span class="sxs-lookup"><span data-stu-id="9cbee-112">Clicking a column heading repeatedly toggles between ascending and descending sort order.</span></span>

![Indexseite „Studenten“](sort-filter-page/_static/paging.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a><span data-ttu-id="9cbee-114">Hinzufügen von Spaltensortierungslinks zur Studentenindexseite</span><span class="sxs-lookup"><span data-stu-id="9cbee-114">Add Column Sort Links to the Students Index Page</span></span>

<span data-ttu-id="9cbee-115">Ändern Sie die `Index`-Methode des Studentencontrollers, und fügen Sie Code zur Studentenindexansicht hinzu, um der Indexseite die Sortierfunktion hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="9cbee-115">To add sorting to the Student Index page, you'll change the `Index` method of the Students controller and add code to the Student Index view.</span></span>

### <a name="add-sorting-functionality-to-the-index-method"></a><span data-ttu-id="9cbee-116">Hinzufügen der Sortierfunktion zur Indexmethode</span><span class="sxs-lookup"><span data-stu-id="9cbee-116">Add sorting Functionality to the Index method</span></span>

<span data-ttu-id="9cbee-117">Ersetzen Sie in *StudentsController.cs* die `Index`-Methode durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="9cbee-117">In *StudentsController.cs*, replace the `Index` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly)]

<span data-ttu-id="9cbee-118">Dieser Code empfängt einen `sortOrder`-Parameter aus der Abfragezeichenfolge in der URL.</span><span class="sxs-lookup"><span data-stu-id="9cbee-118">This code receives a `sortOrder` parameter from the query string in the URL.</span></span> <span data-ttu-id="9cbee-119">Der Wert der Abfragezeichenfolge wird von ASP.NET Core MVC als Parameter an die Aktionsmethode übergeben.</span><span class="sxs-lookup"><span data-stu-id="9cbee-119">The query string value is provided by ASP.NET Core MVC as a parameter to the action method.</span></span> <span data-ttu-id="9cbee-120">Der Parameter ist eine Zeichenfolge, entweder „Name“ oder „Date“, optional gefolgt von einem Unterstrich und der Zeichenfolge „desc“, die die absteigende Reihenfolge angibt.</span><span class="sxs-lookup"><span data-stu-id="9cbee-120">The parameter will be a string that's either "Name" or "Date", optionally followed by an underscore and the string "desc" to specify descending order.</span></span> <span data-ttu-id="9cbee-121">Standardmäßig wird eine aufsteigende Sortierreihenfolge verwendet.</span><span class="sxs-lookup"><span data-stu-id="9cbee-121">The default sort order is ascending.</span></span>

<span data-ttu-id="9cbee-122">Bei der ersten Anforderung der Indexseite gibt es keine Abfragezeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="9cbee-122">The first time the Index page is requested, there's no query string.</span></span> <span data-ttu-id="9cbee-123">Die Studenten werden nach Nachnamen in aufsteigender Reihenfolge angezeigt. Dies ist durch den Fall-Through-Fall in der `switch`-Anweisung standardmäßig festgelegt.</span><span class="sxs-lookup"><span data-stu-id="9cbee-123">The students are displayed in ascending order by last name, which is the default as established by the fall-through case in the `switch` statement.</span></span> <span data-ttu-id="9cbee-124">Wenn der Benutzer auf den Link einer Spaltenüberschrift klickt, wird der entsprechende `sortOrder`-Wert in der Abfragezeichenfolge bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="9cbee-124">When the user clicks a column heading hyperlink, the appropriate `sortOrder` value is provided in the query string.</span></span>

<span data-ttu-id="9cbee-125">Die beiden `ViewData`-Elemente (NameSortParm und DateSortParm) werden von der Ansicht verwendet, um die Links der Spaltenüberschriften mit den entsprechenden Abfragezeichenfolgenwerten zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="9cbee-125">The two `ViewData` elements (NameSortParm and DateSortParm) are used by the view to configure the column heading hyperlinks with the appropriate query string values.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly&highlight=3-4)]

<span data-ttu-id="9cbee-126">Hierbei handelt es sich um ternäre Anweisungen.</span><span class="sxs-lookup"><span data-stu-id="9cbee-126">These are ternary statements.</span></span> <span data-ttu-id="9cbee-127">Die erste gibt an, dass wenn der `sortOrder`-Parameter gleich 0 (null) oder leer ist, „NameSortParm“ auf „name_desc“ festgelegt werden soll. Andernfalls soll er auf eine leere Zeichenfolge festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="9cbee-127">The first one specifies that if the `sortOrder` parameter is null or empty, NameSortParm should be set to "name_desc"; otherwise, it should be set to an empty string.</span></span> <span data-ttu-id="9cbee-128">Diese beiden Anweisungen ermöglichen der Ansicht das Festlegen der Links für Spaltenüberschriften wie folgt:</span><span class="sxs-lookup"><span data-stu-id="9cbee-128">These two statements enable the view to set the column heading hyperlinks as follows:</span></span>

|  <span data-ttu-id="9cbee-129">Aktuelle Sortierreihenfolge</span><span class="sxs-lookup"><span data-stu-id="9cbee-129">Current sort order</span></span>  | <span data-ttu-id="9cbee-130">Hyperlink „Nachname“</span><span class="sxs-lookup"><span data-stu-id="9cbee-130">Last Name Hyperlink</span></span> | <span data-ttu-id="9cbee-131">Hyperlink „Datum“</span><span class="sxs-lookup"><span data-stu-id="9cbee-131">Date Hyperlink</span></span> |
|:--------------------:|:-------------------:|:--------------:|
| <span data-ttu-id="9cbee-132">Nachname (aufsteigend)</span><span class="sxs-lookup"><span data-stu-id="9cbee-132">Last Name ascending</span></span>  | <span data-ttu-id="9cbee-133">descending</span><span class="sxs-lookup"><span data-stu-id="9cbee-133">descending</span></span>          | <span data-ttu-id="9cbee-134">ascending</span><span class="sxs-lookup"><span data-stu-id="9cbee-134">ascending</span></span>      |
| <span data-ttu-id="9cbee-135">Nachname (absteigend)</span><span class="sxs-lookup"><span data-stu-id="9cbee-135">Last Name descending</span></span> | <span data-ttu-id="9cbee-136">ascending</span><span class="sxs-lookup"><span data-stu-id="9cbee-136">ascending</span></span>           | <span data-ttu-id="9cbee-137">ascending</span><span class="sxs-lookup"><span data-stu-id="9cbee-137">ascending</span></span>      |
| <span data-ttu-id="9cbee-138">Datum (aufsteigend)</span><span class="sxs-lookup"><span data-stu-id="9cbee-138">Date ascending</span></span>       | <span data-ttu-id="9cbee-139">ascending</span><span class="sxs-lookup"><span data-stu-id="9cbee-139">ascending</span></span>           | <span data-ttu-id="9cbee-140">descending</span><span class="sxs-lookup"><span data-stu-id="9cbee-140">descending</span></span>     |
| <span data-ttu-id="9cbee-141">Datum (absteigend)</span><span class="sxs-lookup"><span data-stu-id="9cbee-141">Date descending</span></span>      | <span data-ttu-id="9cbee-142">ascending</span><span class="sxs-lookup"><span data-stu-id="9cbee-142">ascending</span></span>           | <span data-ttu-id="9cbee-143">ascending</span><span class="sxs-lookup"><span data-stu-id="9cbee-143">ascending</span></span>      |

<span data-ttu-id="9cbee-144">Die Methode gibt über LINQ to Entities die Spalte an, nach der sortiert werden soll.</span><span class="sxs-lookup"><span data-stu-id="9cbee-144">The method uses LINQ to Entities to specify the column to sort by.</span></span> <span data-ttu-id="9cbee-145">Der Code erstellt vor der Switch-Anweisung eine `IQueryable`-Variable, ändert sie in der Switch-Anweisung und ruft die `ToListAsync`-Methode nach der `switch`-Anweisung auf.</span><span class="sxs-lookup"><span data-stu-id="9cbee-145">The code creates an `IQueryable` variable before the switch statement, modifies it in the switch statement, and calls the `ToListAsync` method after the `switch` statement.</span></span> <span data-ttu-id="9cbee-146">Es wir keine Abfrage an die Datenbank gesendet, wenn Sie die `IQueryable`-Variablen erstellen und ändern.</span><span class="sxs-lookup"><span data-stu-id="9cbee-146">When you create and modify `IQueryable` variables, no query is sent to the database.</span></span> <span data-ttu-id="9cbee-147">Die Abfrage wird nicht ausgeführt, bis Sie das `IQueryable`-Objekt in eine Sammlung konvertieren, indem Sie eine Methode aufrufen, z.B. die `ToListAsync`-Methode.</span><span class="sxs-lookup"><span data-stu-id="9cbee-147">The query isn't executed until you convert the `IQueryable` object into a collection by calling a method such as `ToListAsync`.</span></span> <span data-ttu-id="9cbee-148">Aus diesem Grund führt dieser Code zu einer einzelnen Abfrage, die bis zur `return View`-Anweisung nicht ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="9cbee-148">Therefore, this code results in a single query that's not executed until the `return View` statement.</span></span>

<span data-ttu-id="9cbee-149">Dieser Code könnte mit einer großen Anzahl von Spalten ausführlich werden.</span><span class="sxs-lookup"><span data-stu-id="9cbee-149">This code could get verbose with a large number of columns.</span></span> <span data-ttu-id="9cbee-150">[Das letzte Tutorial dieser Reihe](advanced.md#dynamic-linq) zeigt, wie Sie Code schreiben, mit dem Sie den Namen der `OrderBy`-Spalte an eine Zeichenfolgenvariablen übergeben können.</span><span class="sxs-lookup"><span data-stu-id="9cbee-150">[The last tutorial in this series](advanced.md#dynamic-linq) shows how to write code that lets you pass the name of the `OrderBy` column in a string variable.</span></span>

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a><span data-ttu-id="9cbee-151">Hinzufügen von Spaltenüberschriftenlinks zur Studentenindexansicht</span><span class="sxs-lookup"><span data-stu-id="9cbee-151">Add column heading hyperlinks to the Student Index view</span></span>

<span data-ttu-id="9cbee-152">Ersetzen Sie den Code in *Views/Students/Index.cshtml* durch den folgenden Code, um Spaltenüberschriftenlinks hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="9cbee-152">Replace the code in *Views/Students/Index.cshtml*, with the following code to add column heading hyperlinks.</span></span> <span data-ttu-id="9cbee-153">Die geänderten Zeilen werden hervorgehoben.</span><span class="sxs-lookup"><span data-stu-id="9cbee-153">The changed lines are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index2.cshtml?highlight=16,22)]

<span data-ttu-id="9cbee-154">Dieser Code verwendet die Informationen in den `ViewData`-Eigenschaften zum Einrichten von Links mit den entsprechenden Abfragezeichenfolgenwerten.</span><span class="sxs-lookup"><span data-stu-id="9cbee-154">This code uses the information in `ViewData` properties to set up hyperlinks with the appropriate query string values.</span></span>

<span data-ttu-id="9cbee-155">Führen Sie die Anwendung aus, wählen Sie die Registerkarte **Students** (Studenten) aus, klicken Sie auf die Spaltenüberschriften **Last Name** (Nachname) und **Enrollment Date** (Anmeldedatum), um diese Sortierung zu überprüfen.</span><span class="sxs-lookup"><span data-stu-id="9cbee-155">Run the app, select the **Students** tab, and click the **Last Name** and **Enrollment Date** column headings to verify that sorting works.</span></span>

![Indexseite „Studenten“ in Reihenfolge der Namen](sort-filter-page/_static/name-order.png)

## <a name="add-a-search-box-to-the-students-index-page"></a><span data-ttu-id="9cbee-157">Hinzufügen eines Suchfelds zur Studentenindexseite</span><span class="sxs-lookup"><span data-stu-id="9cbee-157">Add a Search Box to the Students Index page</span></span>

<span data-ttu-id="9cbee-158">Wenn Sie eine Filterfunktion zur Studentenindexseite hinzufügen möchten, dann fügen Sie ein Textfeld und die Schaltfläche „Senden“ zur Ansicht hinzu, und führen Sie die entsprechenden Änderungen in der `Index`-Methode aus.</span><span class="sxs-lookup"><span data-stu-id="9cbee-158">To add filtering to the Students Index page, you'll add a text box and a submit button to the view and make corresponding changes in the `Index` method.</span></span> <span data-ttu-id="9cbee-159">Sie können eine Zeichenfolge in das Textfeld für Vor- und Nachnamen eingeben, um eine Suche zu starten.</span><span class="sxs-lookup"><span data-stu-id="9cbee-159">The text box will let you enter a string to search for in the first name and last name fields.</span></span>

### <a name="add-filtering-functionality-to-the-index-method"></a><span data-ttu-id="9cbee-160">Hinzufügen der Filterfunktion zur Indexmethode</span><span class="sxs-lookup"><span data-stu-id="9cbee-160">Add filtering functionality to the Index method</span></span>

<span data-ttu-id="9cbee-161">Ersetzen Sie in *StudentsController.cs* die `Index`-Methode durch den folgenden Code (die Änderungen sind hervorgehoben).</span><span class="sxs-lookup"><span data-stu-id="9cbee-161">In *StudentsController.cs*, replace the `Index` method with the following code (the changes are highlighted).</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilter&highlight=1,5,9-13)]

<span data-ttu-id="9cbee-162">Sie haben einen `searchString`-Parameter zur `Index`-Methode hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="9cbee-162">You've added a `searchString` parameter to the `Index` method.</span></span> <span data-ttu-id="9cbee-163">Der Zeichenfolgenwert für die Suche wird aus einem Textfeld empfangen, das Sie zur Indexansicht hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="9cbee-163">The search string value is received from a text box that you'll add to the Index view.</span></span> <span data-ttu-id="9cbee-164">Sie haben ebenfalls eine Where-Klausel zur LINQ-Anweisung hinzugefügt, die nur Studenten auswählt, deren Vor- oder Nachnamen die zu suchende Zeichenfolge enthält.</span><span class="sxs-lookup"><span data-stu-id="9cbee-164">You've also added to the LINQ statement a where clause that selects only students whose first name or last name contains the search string.</span></span> <span data-ttu-id="9cbee-165">Die Anweisung, die die Where-Klausel hinzufügt, wird nur ausgeführt, wenn nach einem Wert gesucht wird.</span><span class="sxs-lookup"><span data-stu-id="9cbee-165">The statement that adds the where clause is executed only if there's a value to search for.</span></span>

> [!NOTE]
> <span data-ttu-id="9cbee-166">Wenn Sie die `Where`-Methode auf einem `IQueryable`-Objekt aufrufen, wird der Filter auf dem Server verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="9cbee-166">Here you are calling the `Where` method on an `IQueryable` object, and the filter will be processed on the server.</span></span> <span data-ttu-id="9cbee-167">In einigen Szenarios rufen Sie möglicherweise die `Where`-Methode als Erweiterungsmethode für eine speicherinterne Sammlung auf.</span><span class="sxs-lookup"><span data-stu-id="9cbee-167">In some scenarios you might be calling the `Where` method as an extension method on an in-memory collection.</span></span> <span data-ttu-id="9cbee-168">(Angenommen, Sie ändern den Verweis auf `_context.Students`. Dann wird nicht mehr auf ein `DbSet`-EF, sondern auf eine Repository-Methode verwiesen, die eine `IEnumerable`-Sammlung zurückgibt.) Das Ergebnis wäre normalerweise identisch, aber in einigen Fällen kann es unterschiedlich ausfallen.</span><span class="sxs-lookup"><span data-stu-id="9cbee-168">(For example, suppose you change the reference to `_context.Students` so that instead of an EF `DbSet` it references a repository method that returns an `IEnumerable` collection.) The result would normally be the same but in some cases may be different.</span></span>
>
><span data-ttu-id="9cbee-169">Die .NET Framework-Implementierung der `Contains`-Methode führt beispielsweise standardmäßig einen Vergleich unter Beachtung der Groß-/Kleinschreibung durch. Aber in SQL Server wird dies durch die Sortierungseinstellung der SQL Server-Instanz bestimmt.</span><span class="sxs-lookup"><span data-stu-id="9cbee-169">For example, the .NET Framework implementation of the `Contains` method performs a case-sensitive comparison by default, but in SQL Server this is determined by the collation setting of the SQL Server instance.</span></span> <span data-ttu-id="9cbee-170">Diese Einstellung berücksichtigt die Groß-/Kleinschreibung standardmäßig nicht.</span><span class="sxs-lookup"><span data-stu-id="9cbee-170">That setting defaults to case-insensitive.</span></span> <span data-ttu-id="9cbee-171">Sie können die `ToUpper`-Methode aufrufen, damit der Test die Groß-/Kleinschreibung explizit nicht berücksichtigt: *Where(s => s.LastName.ToUpper(). Contains(searchString.ToUpper())*.</span><span class="sxs-lookup"><span data-stu-id="9cbee-171">You could call the `ToUpper` method to make the test explicitly case-insensitive:  *Where(s => s.LastName.ToUpper().Contains(searchString.ToUpper())*.</span></span> <span data-ttu-id="9cbee-172">Das würde sicherstellen, dass die Ergebnisse gleich bleiben, wenn Sie den Code später ändern, um ein Repository zu verwenden, das eine `IEnumerable`-Sammlung anstelle eines `IQueryable`-Objekts zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="9cbee-172">That would ensure that results stay the same if you change the code later to use a repository which returns   an `IEnumerable` collection instead of an `IQueryable` object.</span></span> <span data-ttu-id="9cbee-173">(Beim Aufrufen der `Contains`-Methode einer `IEnumerable`-Sammlung erhalten Sie die .NET Framework-Implementierung. Wenn Sie sie auf einem `IQueryable`-Objekt aufrufen, erhalten Sie die Implementierung des Datenanbieters.) Es gibt jedoch eine Leistungseinbuße für diese Lösung.</span><span class="sxs-lookup"><span data-stu-id="9cbee-173">(When you call the `Contains` method on an `IEnumerable` collection, you get the .NET Framework implementation; when you call it on an `IQueryable` object, you get the database provider implementation.) However, there's a performance penalty for this solution.</span></span> <span data-ttu-id="9cbee-174">Der `ToUpper`-Code würde eine Funktion in die WHERE-Klausel der TSQL SELECT-Anweisung setzen.</span><span class="sxs-lookup"><span data-stu-id="9cbee-174">The `ToUpper` code would put a function in the WHERE clause of the TSQL SELECT statement.</span></span> <span data-ttu-id="9cbee-175">Die würde verhindern, dass der Optimierer einen Index verwendet.</span><span class="sxs-lookup"><span data-stu-id="9cbee-175">That would prevent the optimizer from using an index.</span></span> <span data-ttu-id="9cbee-176">Da SQL die Groß-/Kleinschreibung hauptsächlich nicht berücksichtigt, wird empfohlen, den `ToUpper`-Code zu vermeiden, bis die Migration zu einem Datenspeicher erfolgt ist, der die Groß-/Kleinschreibung beachtet.</span><span class="sxs-lookup"><span data-stu-id="9cbee-176">Given that SQL is mostly installed as case-insensitive, it's best to avoid the `ToUpper` code until you migrate to a case-sensitive data store.</span></span>

### <a name="add-a-search-box-to-the-student-index-view"></a><span data-ttu-id="9cbee-177">Hinzufügen eines Suchfelds zur Studentenindexansicht</span><span class="sxs-lookup"><span data-stu-id="9cbee-177">Add a Search Box to the Student Index View</span></span>

<span data-ttu-id="9cbee-178">Fügen Sie in *Views/Student/Index.cshtml* den hervorgehobenen Code unmittelbar vor dem Tag „Tabelle öffnen“ hinzu, um eine Beschriftung, ein Textfeld und eine **Suche**-Schaltfläche zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="9cbee-178">In *Views/Student/Index.cshtml*, add the highlighted code immediately before the opening table tag in order to create a caption, a text box, and a **Search** button.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index3.cshtml?range=9-23&highlight=5-13)]

<span data-ttu-id="9cbee-179">Dieser Code verwendet das [Taghilfsprogramm](xref:mvc/views/tag-helpers/intro) `<form>`, um das Suchtextfeld und die Schaltfläche hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="9cbee-179">This code uses the `<form>` [tag helper](xref:mvc/views/tag-helpers/intro) to add the search text box and button.</span></span> <span data-ttu-id="9cbee-180">Das Taghilfsprogramm `<form>` sendet standardmäßig Formulardaten mit einem POST, was bedeutet, dass Parameter als Abfragezeichenfolgen im Hauptteil der HTTP-Nachricht und nicht in der URL übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="9cbee-180">By default, the `<form>` tag helper submits form data with a POST, which means that parameters are passed in the HTTP message body and not in the URL as query strings.</span></span> <span data-ttu-id="9cbee-181">Bei der Angabe von HTTP GET werden die Formulardaten als Abfragezeichenfolgen an die URL übergeben. Dadurch können Benutzer ein Lesezeichen für die URL erstellen.</span><span class="sxs-lookup"><span data-stu-id="9cbee-181">When you specify HTTP GET, the form data is passed in the URL as query strings, which enables users to bookmark the URL.</span></span> <span data-ttu-id="9cbee-182">Die W3C-Richtlinien empfehlen die Verwendung eines GET-Vorgangs, wenn die Aktion nicht zu einem Update führt.</span><span class="sxs-lookup"><span data-stu-id="9cbee-182">The W3C guidelines recommend that you should use GET when the action doesn't result in an update.</span></span>

<span data-ttu-id="9cbee-183">Führen Sie die Anwendung aus, wählen Sie die Registerkarte **Studenten**, geben Sie eine Suchzeichenfolge ein, und klicken Sie auf „Suchen“, um die Funktionsweise des Filters zu überprüfen.</span><span class="sxs-lookup"><span data-stu-id="9cbee-183">Run the app, select the **Students** tab, enter a search string, and click Search to verify that filtering is working.</span></span>

![Indexseite Studenten mit Filtern](sort-filter-page/_static/filtering.png)

<span data-ttu-id="9cbee-185">Beachten Sie, dass die URL die Suchzeichenfolge enthält.</span><span class="sxs-lookup"><span data-stu-id="9cbee-185">Notice that the URL contains the search string.</span></span>

```html
http://localhost:5813/Students?SearchString=an
```

<span data-ttu-id="9cbee-186">Wenn Sie diese Seite kennzeichnen, erhalten Sie die gefilterte Liste bei der Verwendung von Lesezeichen.</span><span class="sxs-lookup"><span data-stu-id="9cbee-186">If you bookmark this page, you'll get the filtered list when you use the bookmark.</span></span> <span data-ttu-id="9cbee-187">Wird `method="get"` zum `form`-Tag hinzugefügt, wird die Abfragezeichenfolge generiert.</span><span class="sxs-lookup"><span data-stu-id="9cbee-187">Adding `method="get"` to the `form` tag is what caused the query string to be generated.</span></span>

<span data-ttu-id="9cbee-188">Wenn Sie in dieser Phase auf einen Sortierlink in einer Spaltenüberschrift klicken, verlieren Sie den Filterwert, den Sie im Feld neben **Suchen** eingegeben haben.</span><span class="sxs-lookup"><span data-stu-id="9cbee-188">At this stage, if you click a column heading sort link you'll lose the filter value that you entered in the **Search** box.</span></span> <span data-ttu-id="9cbee-189">Dies soll im nächsten Abschnitt behoben werden.</span><span class="sxs-lookup"><span data-stu-id="9cbee-189">You'll fix that in the next section.</span></span>

## <a name="add-paging-functionality-to-the-students-index-page"></a><span data-ttu-id="9cbee-190">Hinzufügen von Pagingfunktionen der Studentenindexseite</span><span class="sxs-lookup"><span data-stu-id="9cbee-190">Add paging functionality to the Students Index page</span></span>

<span data-ttu-id="9cbee-191">Um die Pagingfunktionen zur Studentenindexseite hinzuzufügen, erstellen Sie eine `PaginatedList`-Klasse, die `Skip`- und `Take`-Anweisungen zum Filtern von Daten auf dem Server verwendet, anstatt immer alle Zeilen der Tabelle abzurufen.</span><span class="sxs-lookup"><span data-stu-id="9cbee-191">To add paging to the Students Index page, you'll create a `PaginatedList` class that uses `Skip` and `Take` statements to filter data on the server instead of always retrieving all rows of the table.</span></span> <span data-ttu-id="9cbee-192">Dann nehmen Sie zusätzliche Änderungen der `Index`-Methode vor, und fügen Pagingschaltflächen zur `Index`-Ansicht hinzu.</span><span class="sxs-lookup"><span data-stu-id="9cbee-192">Then you'll make additional changes in the `Index` method and add paging buttons to the `Index` view.</span></span> <span data-ttu-id="9cbee-193">Die folgende Abbildung zeigt die Pagingschaltflächen.</span><span class="sxs-lookup"><span data-stu-id="9cbee-193">The following illustration shows the paging buttons.</span></span>

![Indexseite „Studenten“ mit Paginglinks](sort-filter-page/_static/paging.png)

<span data-ttu-id="9cbee-195">Erstellen Sie `PaginatedList.cs` im Projektordner. Ersetzen Sie den Vorlagencode dann durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="9cbee-195">In the project folder, create `PaginatedList.cs`, and then replace the template code with the following code.</span></span>

[!code-csharp[](intro/samples/cu/PaginatedList.cs)]

<span data-ttu-id="9cbee-196">Die `CreateAsync`-Methode in diesem Code akzeptiert die Seitengröße und die Seitenzahl und wendet die entsprechenden `Skip`- und `Take`-Anweisungen auf `IQueryable` an.</span><span class="sxs-lookup"><span data-stu-id="9cbee-196">The `CreateAsync` method in this code takes page size and page number and applies the appropriate `Skip` and `Take` statements to the `IQueryable`.</span></span> <span data-ttu-id="9cbee-197">Wenn `ToListAsync` auf `IQueryable` aufgerufen wird, wird eine Liste zurückgegeben, die nur die angeforderte Seite enthält.</span><span class="sxs-lookup"><span data-stu-id="9cbee-197">When `ToListAsync` is called on the `IQueryable`, it will return a List containing only the requested page.</span></span> <span data-ttu-id="9cbee-198">Die Eigenschaften `HasPreviousPage` und `HasNextPage` dienen zum Aktivieren oder Deaktivieren der Pagingschaltflächen **Zurück** und **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="9cbee-198">The properties `HasPreviousPage` and `HasNextPage` can be used to enable or disable **Previous** and **Next** paging buttons.</span></span>

<span data-ttu-id="9cbee-199">Ein `CreateAsync`-Methode wird anstelle eines Konstruktors verwendet, um das `PaginatedList<T>`-Objekt zu erstellen, da die Konstruktoren keinen asynchronen Code ausführen können.</span><span class="sxs-lookup"><span data-stu-id="9cbee-199">A `CreateAsync` method is used instead of a constructor to create the `PaginatedList<T>` object because constructors can't run asynchronous code.</span></span>

## <a name="add-paging-functionality-to-the-index-method"></a><span data-ttu-id="9cbee-200">Fügen Sie Pagingfunktionen zur Indexmethode hinzu</span><span class="sxs-lookup"><span data-stu-id="9cbee-200">Add paging functionality to the Index method</span></span>

<span data-ttu-id="9cbee-201">Ersetzen Sie in *StudentsController.cs* die `Index`-Methode mit dem folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="9cbee-201">In *StudentsController.cs*, replace the `Index` method with the following code.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilterPage&highlight=1-5,7,11-18,45-46)]

<span data-ttu-id="9cbee-202">Dieser Code fügt ein Parameter für die Seitenanzahl, die aktuelle Sortierreihenfolge und den aktuellen Filter zur Methodensignatur hinzu.</span><span class="sxs-lookup"><span data-stu-id="9cbee-202">This code adds a page number parameter, a current sort order parameter, and a current filter parameter to the method signature.</span></span>

```csharp
public async Task<IActionResult> Index(
    string sortOrder,
    string currentFilter,
    string searchString,
    int? page)
```

<span data-ttu-id="9cbee-203">Wenn die Seite zum ersten Mal angezeigt wird oder wenn der Benutzer nicht auf einen Paging- oder Sortierlink geklickt hat, werden alle Parameter gleich 0 (null) sein.</span><span class="sxs-lookup"><span data-stu-id="9cbee-203">The first time the page is displayed, or if the user hasn't clicked a paging or sorting link, all the parameters will be null.</span></span>  <span data-ttu-id="9cbee-204">Wenn auf ein Paginglink geklickt wird, enthält die Seitenvariable die anzuzeigende Seitenzahl.</span><span class="sxs-lookup"><span data-stu-id="9cbee-204">If a paging link is clicked, the page variable will contain the page number to display.</span></span>

<span data-ttu-id="9cbee-205">Das `ViewData`-Element „CurrentSort“ stellt die aktuelle Sortierreihenfolge für die Ansicht bereit. Diese Sortierreihenfolge muss in den Paginglinks enthalten sein, damit sie beim Pagingvorgang identisch bleibt.</span><span class="sxs-lookup"><span data-stu-id="9cbee-205">The `ViewData` element named CurrentSort provides the view with the current sort order, because this must be included in the paging links in order to keep the sort order the same while paging.</span></span>

<span data-ttu-id="9cbee-206">Das `ViewData`-Element „CurrentFilter“ stellt der Ansicht die aktuelle Filterzeichenfolge zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="9cbee-206">The `ViewData` element named CurrentFilter provides the view with the current filter string.</span></span> <span data-ttu-id="9cbee-207">Dieser Wert muss in den Paginglinks enthalten sein, damit die Filtereinstellungen während des Pagingvorgangs beibehalten werden, und er muss im Textfeld wiederhergestellt werden, wenn die Seite erneut angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="9cbee-207">This value must be included in the paging links in order to maintain the filter settings during paging, and it must be restored to the text box when the page is redisplayed.</span></span>

<span data-ttu-id="9cbee-208">Wenn die Suchzeichenfolge während des Pagingvorgangs geändert wird, muss die Seite auf 1 zurückgesetzt werden, da der neue Filter andere Daten anzeigen kann.</span><span class="sxs-lookup"><span data-stu-id="9cbee-208">If the search string is changed during paging, the page has to be reset to 1, because the new filter can result in different data to display.</span></span> <span data-ttu-id="9cbee-209">Die Suchzeichenfolge wird geändert, wenn ein Wert in das Textfeld eingegeben und auf die Schaltfläche „Senden“ geklickt wird.</span><span class="sxs-lookup"><span data-stu-id="9cbee-209">The search string is changed when a value is entered in the text box and the Submit button is pressed.</span></span> <span data-ttu-id="9cbee-210">In diesem Fall ist der `searchString`-Parameter nicht gleich 0 (null).</span><span class="sxs-lookup"><span data-stu-id="9cbee-210">In that case, the `searchString` parameter isn't null.</span></span>

```csharp
if (searchString != null)
{
    page = 1;
}
else
{
    searchString = currentFilter;
}
```

<span data-ttu-id="9cbee-211">Am Ende der `Index`-Methode konvertiert die `PaginatedList.CreateAsync`-Methode die Abfrage der Studentendaten in eine einzelne Seite in einem Sammlungstyp, der Pagingvorgänge unterstützt.</span><span class="sxs-lookup"><span data-stu-id="9cbee-211">At the end of the `Index` method, the `PaginatedList.CreateAsync` method converts the student query to a single page of students in a collection type that supports paging.</span></span> <span data-ttu-id="9cbee-212">Diese einzelnen Seite mit Studentendaten wird dann an die Ansicht übergeben.</span><span class="sxs-lookup"><span data-stu-id="9cbee-212">That single page of students is then passed to the view.</span></span>

```csharp
return View(await PaginatedList<Student>.CreateAsync(students.AsNoTracking(), page ?? 1, pageSize));
```

<span data-ttu-id="9cbee-213">Die `PaginatedList.CreateAsync`-Methode nimmt eine Seitenanzahl.</span><span class="sxs-lookup"><span data-stu-id="9cbee-213">The `PaginatedList.CreateAsync` method takes a page number.</span></span> <span data-ttu-id="9cbee-214">Die zwei Fragezeichen stellen den Nullzusammensetzungsoperator dar.</span><span class="sxs-lookup"><span data-stu-id="9cbee-214">The two question marks represent the null-coalescing operator.</span></span> <span data-ttu-id="9cbee-215">Der Nullzusammensetzungsoperator definiert einen Standardwert für einen Nullable-Typ. Der `(page ?? 1)`-Ausdruck bedeutet, dass `page` zurückgegeben wird, wenn dies über einen Wert verfügt, oder 1, wenn `page` gleich 0 (null) ist.</span><span class="sxs-lookup"><span data-stu-id="9cbee-215">The null-coalescing operator defines a default value for a nullable type; the expression `(page ?? 1)` means return the value of `page` if it has a value, or return 1 if `page` is null.</span></span>

## <a name="add-paging-links-to-the-student-index-view"></a><span data-ttu-id="9cbee-216">Hinzufügen eines Paginglinks zur Studentenindexansicht</span><span class="sxs-lookup"><span data-stu-id="9cbee-216">Add paging links to the Student Index view</span></span>

<span data-ttu-id="9cbee-217">Ersetzen Sie in *Views/Students/Index.cshtml* den vorhandenen Code durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="9cbee-217">In *Views/Students/Index.cshtml*, replace the existing code with the following code.</span></span> <span data-ttu-id="9cbee-218">Die Änderungen werden hervorgehoben.</span><span class="sxs-lookup"><span data-stu-id="9cbee-218">The changes are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index.cshtml?highlight=1,27,30,33,61-79)]

<span data-ttu-id="9cbee-219">Die `@model`-Anweisung am oberen Rand der Seite gibt an, dass die Ansicht nun ein `PaginatedList<T>`-Objekt anstelle eines `List<T>`-Objekts aufruft.</span><span class="sxs-lookup"><span data-stu-id="9cbee-219">The `@model` statement at the top of the page specifies that the view now gets a `PaginatedList<T>` object instead of a `List<T>` object.</span></span>

<span data-ttu-id="9cbee-220">Die Spaltenüberschriftenlinks verwenden die Abfragezeichenfolge, um die aktuelle Suchzeichenfolge an den Controller zu übergeben, damit Benutzer Filterergebnisse sortieren können:</span><span class="sxs-lookup"><span data-stu-id="9cbee-220">The column header links use the query string to pass the current search string to the controller so that the user can sort within filter results:</span></span>

```html
<a asp-action="Index" asp-route-sortOrder="@ViewData["DateSortParm"]" asp-route-currentFilter ="@ViewData["CurrentFilter"]">Enrollment Date</a>
```

<span data-ttu-id="9cbee-221">Die Pagingschaltflächen werden durch Taghilfsprogramme angezeigt:</span><span class="sxs-lookup"><span data-stu-id="9cbee-221">The paging buttons are displayed by tag helpers:</span></span>

```html
<a asp-action="Index"
   asp-route-sortOrder="@ViewData["CurrentSort"]"
   asp-route-page="@(Model.PageIndex - 1)"
   asp-route-currentFilter="@ViewData["CurrentFilter"]"
   class="btn btn-default @prevDisabled">
   Previous
</a>
```

<span data-ttu-id="9cbee-222">Führen Sie die Anwendung aus. Wechseln Sie zur Studentenseite.</span><span class="sxs-lookup"><span data-stu-id="9cbee-222">Run the app and go to the Students page.</span></span>

![Indexseite „Studenten“ mit Paginglinks](sort-filter-page/_static/paging.png)

<span data-ttu-id="9cbee-224">Klicken Sie auf die Paginglinks in verschiedenen Sortierreihenfolgen, um sicherzustellen, dass die Paging funktioniert.</span><span class="sxs-lookup"><span data-stu-id="9cbee-224">Click the paging links in different sort orders to make sure paging works.</span></span> <span data-ttu-id="9cbee-225">Geben Sie dann eine Suchzeichenfolge ein. Probieren Sie Paging erneut aus, um sicherzustellen, dass sie auch mit Sortier- und Filtervorgängen ordnungsgemäß funktioniert.</span><span class="sxs-lookup"><span data-stu-id="9cbee-225">Then enter a search string and try paging again to verify that paging also works correctly with sorting and filtering.</span></span>

## <a name="create-an-about-page-that-shows-student-statistics"></a><span data-ttu-id="9cbee-226">Erstellen einer Informationsseite, die Statistiken der Studentendaten anzeigt</span><span class="sxs-lookup"><span data-stu-id="9cbee-226">Create an About page that shows Student statistics</span></span>

<span data-ttu-id="9cbee-227">Auf der **Infoseite** der Contoso University wird angezeigt, wie viele Studenten sich an welchem Datum angemeldet haben.</span><span class="sxs-lookup"><span data-stu-id="9cbee-227">For the Contoso University website's **About** page, you'll display how many students have enrolled for each enrollment date.</span></span> <span data-ttu-id="9cbee-228">Das erfordert Gruppieren und einfache Berechnungen dieser Gruppen.</span><span class="sxs-lookup"><span data-stu-id="9cbee-228">This requires grouping and simple calculations on the groups.</span></span> <span data-ttu-id="9cbee-229">Um dies zu erreichen, ist Folgendes erforderlich:</span><span class="sxs-lookup"><span data-stu-id="9cbee-229">To accomplish this, you'll do the following:</span></span>

* <span data-ttu-id="9cbee-230">Erstellen Sie eine Ansichtsmodellklasse für die Daten, die Sie an die Ansicht übergeben müssen.</span><span class="sxs-lookup"><span data-stu-id="9cbee-230">Create a view model class for the data that you need to pass to the view.</span></span>

* <span data-ttu-id="9cbee-231">Ändern Sie die Infomethode im Home-Controller.</span><span class="sxs-lookup"><span data-stu-id="9cbee-231">Modify the About method in the Home controller.</span></span>

* <span data-ttu-id="9cbee-232">Ändern Sie die Infoansicht.</span><span class="sxs-lookup"><span data-stu-id="9cbee-232">Modify the About view.</span></span>

### <a name="create-the-view-model"></a><span data-ttu-id="9cbee-233">Erstellen des Ansichtsmodells</span><span class="sxs-lookup"><span data-stu-id="9cbee-233">Create the view model</span></span>

<span data-ttu-id="9cbee-234">Erstellen Sie im Ordner *Models* (Modelle) den Ordner *SchoolViewModels*.</span><span class="sxs-lookup"><span data-stu-id="9cbee-234">Create a *SchoolViewModels* folder in the *Models* folder.</span></span>

<span data-ttu-id="9cbee-235">Fügen Sie im neuen Ordner die Klassendatei *EnrollmentDateGroup.cs* hinzu. Ersetzen Sie den Vorlagencode durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="9cbee-235">In the new folder, add a class file *EnrollmentDateGroup.cs* and replace the template code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/EnrollmentDateGroup.cs)]

### <a name="modify-the-home-controller"></a><span data-ttu-id="9cbee-236">Ändern des Home-Controllers</span><span class="sxs-lookup"><span data-stu-id="9cbee-236">Modify the Home Controller</span></span>

<span data-ttu-id="9cbee-237">Fügen Sie in *HomeController.cs* am Anfang der Datei die folgenden Anweisungen hinzu:</span><span class="sxs-lookup"><span data-stu-id="9cbee-237">In *HomeController.cs*, add the following using statements at the top of the file:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings1)]

<span data-ttu-id="9cbee-238">Fügen Sie eine Klassenvariable für den Datenbankkontext hinzu, unmittelbar nachdem Sie die geschweifte Klammer für die Klasse geöffnet haben. Rufen Sie eine Instanz des Kontexts von ASP.NET Core DI auf:</span><span class="sxs-lookup"><span data-stu-id="9cbee-238">Add a class variable for the database context immediately after the opening curly brace for the class, and get an instance of the context from ASP.NET Core DI:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_AddContext&highlight=3,5,7)]

<span data-ttu-id="9cbee-239">Ersetzen Sie die `About`-Methode durch folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="9cbee-239">Replace the `About` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseDbSet)]

<span data-ttu-id="9cbee-240">Die LINQ-Anweisung gruppiert die Studentenentitäten nach Anmeldedatum, berechnet die Anzahl der Entitäten in jeder Gruppe und speichert die Ergebnisse in einer Sammlung von `EnrollmentDateGroup`-Ansichtsmodellobjekten.</span><span class="sxs-lookup"><span data-stu-id="9cbee-240">The LINQ statement groups the student entities by enrollment date, calculates the number of entities in each group, and stores the results in a collection of `EnrollmentDateGroup` view model objects.</span></span>
> [!NOTE]
> <span data-ttu-id="9cbee-241">In der Version 1.0 von Entity Framework Core wird das gesamte Ergebnis an den Client zurückgegeben, und die Gruppierung erfolgt auf dem Client.</span><span class="sxs-lookup"><span data-stu-id="9cbee-241">In the 1.0 version of Entity Framework Core, the entire result set is returned to the client, and grouping is done on the client.</span></span> <span data-ttu-id="9cbee-242">In einigen Szenarios könnte das Leistungsprobleme hervorrufen.</span><span class="sxs-lookup"><span data-stu-id="9cbee-242">In some scenarios this could create performance problems.</span></span> <span data-ttu-id="9cbee-243">Achten Sie darauf, dass Sie die Leistung mit Produktionsdatenmengen überprüfen. Verwenden Sie bei Bedarf unformatiertes SQL, um die Gruppierung auf dem Server auszuführen.</span><span class="sxs-lookup"><span data-stu-id="9cbee-243">Be sure to test performance with production volumes of data, and if necessary use raw SQL to do the grouping on the server.</span></span> <span data-ttu-id="9cbee-244">Weitere Informationen zur Verwendung von unformatiertem SQL finden Sie im [letzten Tutorial dieser Reihe](advanced.md).</span><span class="sxs-lookup"><span data-stu-id="9cbee-244">For information about how to use raw SQL, see [the last tutorial in this series](advanced.md).</span></span>

### <a name="modify-the-about-view"></a><span data-ttu-id="9cbee-245">Ändern der Infoansicht</span><span class="sxs-lookup"><span data-stu-id="9cbee-245">Modify the About View</span></span>

<span data-ttu-id="9cbee-246">Ersetzen Sie den Code in der *Views/Home/About.cshtml*-Datei durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="9cbee-246">Replace the code in the *Views/Home/About.cshtml* file with the following code:</span></span>

[!code-html[](intro/samples/cu/Views/Home/About.cshtml)]

<span data-ttu-id="9cbee-247">Führen Sie die Anwendung aus, und wechseln Sie zur Infoseite.</span><span class="sxs-lookup"><span data-stu-id="9cbee-247">Run the app and go to the About page.</span></span> <span data-ttu-id="9cbee-248">Die Anzahl der Studenten für jedes Anmeldedatum wird in einer Tabelle angezeigt.</span><span class="sxs-lookup"><span data-stu-id="9cbee-248">The count of students for each enrollment date is displayed in a table.</span></span>

![Seite „Info“](sort-filter-page/_static/about.png)

## <a name="summary"></a><span data-ttu-id="9cbee-250">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="9cbee-250">Summary</span></span>

<span data-ttu-id="9cbee-251">In diesem Tutorial haben Sie das Sortieren, Filtern, Paging und Gruppieren gelernt.</span><span class="sxs-lookup"><span data-stu-id="9cbee-251">In this tutorial, you've seen how to perform sorting, filtering, paging, and grouping.</span></span> <span data-ttu-id="9cbee-252">Im nächsten Tutorial lernen Sie, wie Sie mithilfe von Migrationen Datenmodelländerungen verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="9cbee-252">In the next tutorial, you'll learn how to handle data model changes by using migrations.</span></span>

::: moniker-end

> [!div class="step-by-step"]
> <span data-ttu-id="9cbee-253">[Zurück](crud.md)
> [Weiter](migrations.md)</span><span class="sxs-lookup"><span data-stu-id="9cbee-253">[Previous](crud.md)
[Next](migrations.md)</span></span>
