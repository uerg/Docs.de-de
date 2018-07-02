---
title: 'Razor-Seiten mit EF Core in ASP.NET Core: Lesen verwandter Daten (6 von 8)'
author: rick-anderson
description: In diesem Tutorial werden verwandte Daten gelesen und angezeigt. Das gilt für Daten, die Entity Framework in Navigationseigenschaften lädt.
ms.author: riande
ms.date: 11/05/2017
uid: data/ef-rp/read-related-data
ms.openlocfilehash: 4e0aa7151cc54f666202458ba60500a7c04f5ebb
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276759"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---read-related-data---6-of-8"></a><span data-ttu-id="9e9e2-103">Razor-Seiten mit EF Core in ASP.NET Core: Lesen verwandter Daten (6 von 8)</span><span class="sxs-lookup"><span data-stu-id="9e9e2-103">Razor Pages with EF Core in ASP.NET Core - Read Related Data - 6 of 8</span></span>

<span data-ttu-id="9e9e2-104">Von [Tom Dykstra](https://github.com/tdykstra), [Jon P. Smith](https://twitter.com/thereformedprog) und [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="9e9e2-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="9e9e2-105">In diesem Tutorial werden verwandte Daten gelesen und angezeigt.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-105">In this tutorial, related data is read and displayed.</span></span> <span data-ttu-id="9e9e2-106">Verwandte Daten sind Daten, die von EF Core in die Navigationseigenschaften geladen werden.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-106">Related data is data that EF Core loads into navigation properties.</span></span>

<span data-ttu-id="9e9e2-107">Wenn nicht zu lösende Probleme auftreten, laden Sie die [abgeschlossene Anwendung für diese Phase](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related) herunter.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-107">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).</span></span>

<span data-ttu-id="9e9e2-108">Die folgenden Abbildungen zeigen die abgeschlossenen Seiten für dieses Tutorial:</span><span class="sxs-lookup"><span data-stu-id="9e9e2-108">The following illustrations show the completed pages for this tutorial:</span></span>

![Indexseite „Kurse“](read-related-data/_static/courses-index.png)

![Indexseite „Dozenten“](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="9e9e2-111">Explizites, Eager und Lazy Loading verwandter Daten</span><span class="sxs-lookup"><span data-stu-id="9e9e2-111">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="9e9e2-112">Es gibt mehrere Möglichkeiten, mit denen EF Core verwandte Daten in die Navigationseigenschaften einer Entität laden kann:</span><span class="sxs-lookup"><span data-stu-id="9e9e2-112">There are several ways that EF Core can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="9e9e2-113">[Eager Loading (vorzeitiges Laden)](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading).</span><span class="sxs-lookup"><span data-stu-id="9e9e2-113">[Eager loading](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading).</span></span> <span data-ttu-id="9e9e2-114">Beim Eager Loading lädt eine Abfrage für einen Entitätstyp auch verwandte Entitäten.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-114">Eager loading is when a query for one type of entity also loads related entities.</span></span> <span data-ttu-id="9e9e2-115">Wenn die Entität gelesen wird, werden ihre verwandten Daten abgerufen.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-115">When the entity is read, its related data is retrieved.</span></span> <span data-ttu-id="9e9e2-116">Dies führt normalerweise zu einer einzelnen Joinabfrage, die alle Daten abruft, die erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-116">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="9e9e2-117">EF Core wird mehrere Abfragen für einige Typen von Eager Loading ausgeben.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-117">EF Core will issue multiple queries for some types of eager loading.</span></span> <span data-ttu-id="9e9e2-118">Die Ausgabe mehrerer Abfragen kann effizienter sein, als dies bei einigen Abfragen in EF6 der Fall war. Dort war nur eine einzelne Abfrage vorhanden.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-118">Issuing multiple queries can be more efficient than was the case for some queries in EF6 where there was a single query.</span></span> <span data-ttu-id="9e9e2-119">Eager Loading wird mit den `Include`- und `ThenInclude`-Methoden angegeben.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-119">Eager loading is specified with the `Include` and `ThenInclude` methods.</span></span>

  ![Beispiel für Eager Loading](read-related-data/_static/eager-loading.png)
 
  <span data-ttu-id="9e9e2-121">Eager Loading sendet mehrere Abfragen, wenn eine Sammlungsnavigation enthalten ist:</span><span class="sxs-lookup"><span data-stu-id="9e9e2-121">Eager loading sends multiple queries when a collection navigation is included:</span></span>

  * <span data-ttu-id="9e9e2-122">Eine Abfrage für die Hauptabfrage</span><span class="sxs-lookup"><span data-stu-id="9e9e2-122">One query for the main query</span></span> 
  * <span data-ttu-id="9e9e2-123">Eine Abfrage für jeden „Sammlungsrand“ in der Ladestruktur.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-123">One query for each collection "edge" in the load tree.</span></span>

* <span data-ttu-id="9e9e2-124">Separate Abfragen mit `Load`: Die Daten können in separaten Abfragen abgerufen werden. EF Core korrigiert die Navigationseigenschaften.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-124">Separate queries with `Load`: The data can be retrieved in separate queries, and EF Core "fixes up" the navigation properties.</span></span> <span data-ttu-id="9e9e2-125">„Korrigieren“ bedeutet, dass EF Core die Navigationseigenschaften automatisch füllt.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-125">"fixes up" means that EF Core automatically populates the navigation properties.</span></span> <span data-ttu-id="9e9e2-126">Separate Abfragen mit `Load` ähneln mehr dem expliziten Laden als dem Eager Loading.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-126">Separate queries with `Load` is more like explict loading than eager loading.</span></span>

  ![Beispiel für separate Abfragen](read-related-data/_static/separate-queries.png)

  <span data-ttu-id="9e9e2-128">Hinweis: EF Core korrigiert automatisch Navigationseigenschaften für alle anderen Entitäten, die zuvor in die Kontextinstanz geladen wurden.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-128">Note: EF Core automatically fixes up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="9e9e2-129">Auch wenn die Daten für eine Navigationseigenschaft *nicht* explizit eingeschlossen sind, kann die Eigenschaft immer noch aufgefüllt werden, wenn einige oder alle verwandten Entitäten zuvor geladen wurden.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-129">Even if the data for a navigation property is *not* explicitly included, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

* <span data-ttu-id="9e9e2-130">[Explizites Laden](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading).</span><span class="sxs-lookup"><span data-stu-id="9e9e2-130">[Explicit loading](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading).</span></span> <span data-ttu-id="9e9e2-131">Wenn die Entität zuerst gelesen wird, werden verwandte Daten nicht abgerufen.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-131">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="9e9e2-132">Es muss Code geschrieben werden, um die verwandten Daten bei Bedarf abzurufen.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-132">Code must be written to retrieve the related data when it's needed.</span></span> <span data-ttu-id="9e9e2-133">Explizites Laden mit separaten Abfragen führt zu mehreren Abfragen, die an die Datenbank gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-133">Explicit loading with separate queries results in multiple queries sent to the DB.</span></span> <span data-ttu-id="9e9e2-134">Mit explizitem Laden gibt der Code die zu ladenden Navigationseigenschaften an.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-134">With explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="9e9e2-135">Verwenden Sie für explizites Laden die `Load`-Methode.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-135">Use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="9e9e2-136">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="9e9e2-136">For example:</span></span>

  ![Beispiel für explizites Laden](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="9e9e2-138">[Lazy Loading (verzögertes Laden)](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="9e9e2-138">[Lazy loading](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="9e9e2-139">[EF Core unterstützt das Lazy Loading derzeit nicht](https://github.com/aspnet/EntityFrameworkCore/issues/3797).</span><span class="sxs-lookup"><span data-stu-id="9e9e2-139">[EF Core doesn't currently support lazy loading](https://github.com/aspnet/EntityFrameworkCore/issues/3797).</span></span> <span data-ttu-id="9e9e2-140">Wenn die Entität zuerst gelesen wird, werden verwandte Daten nicht abgerufen.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-140">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="9e9e2-141">Wenn zum ersten Mal auf eine Navigationseigenschaft zugegriffen wird, werden die für diese Navigationseigenschaft erforderlichen Daten automatisch abgerufen.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-141">The first time a navigation property is accessed, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="9e9e2-142">Wenn zum ersten Mal auf eine Navigationseigenschaft zugegriffen wird, wird jedes Mal eine Abfrage an die Datenbank geschickt.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-142">A query is sent to the DB each time a navigation property is accessed for the first time.</span></span>

* <span data-ttu-id="9e9e2-143">Der `Select`-Operator lädt nur die erforderlichen verwandten Daten.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-143">The `Select` operator loads only the related data needed.</span></span>

## <a name="create-a-courses-page-that-displays-department-name"></a><span data-ttu-id="9e9e2-144">Erstellen einer Kursseite, die Abteilungsnamen anzeigt</span><span class="sxs-lookup"><span data-stu-id="9e9e2-144">Create a Courses page that displays department name</span></span>

<span data-ttu-id="9e9e2-145">Die Course-Entität enthält eine Navigationseigenschaft, welche die `Department`-Entität enthält.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-145">The Course entity includes a navigation property that contains the `Department` entity.</span></span> <span data-ttu-id="9e9e2-146">Die `Department`-Entität enthält die Abteilung, der der Kurs zugewiesen ist.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-146">The `Department` entity contains the department that the course is assigned to.</span></span>

<span data-ttu-id="9e9e2-147">So zeigen Sie den Namen der zugewiesenen Abteilung in einer Kursliste an:</span><span class="sxs-lookup"><span data-stu-id="9e9e2-147">To display the name of the assigned department in a list of courses:</span></span>

* <span data-ttu-id="9e9e2-148">Rufen Sie `Name`-Eigenschaft aus der `Department`-Entität ab.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-148">Get the `Name` property from the `Department` entity.</span></span>
* <span data-ttu-id="9e9e2-149">Die `Department`-Entität stammt aus der `Course.Department`-Navigationseigenschaft.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-149">The `Department` entity comes from the `Course.Department` navigation property.</span></span>

![ourse.Department](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>
### <a name="scaffold-the-course-model"></a><span data-ttu-id="9e9e2-151">Erstellen des Gerüsts für das Kursmodell</span><span class="sxs-lookup"><span data-stu-id="9e9e2-151">Scaffold the Course model</span></span>

* <span data-ttu-id="9e9e2-152">Beenden Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-152">Exit Visual Studio.</span></span>
* <span data-ttu-id="9e9e2-153">Öffnen Sie ein Befehlsfenster im Projektverzeichnis (das Verzeichnis mit den Dateien *Program.cs*, *Startup.cs*, und *CSPROJ*).</span><span class="sxs-lookup"><span data-stu-id="9e9e2-153">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="9e9e2-154">Führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="9e9e2-154">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
  ```

<span data-ttu-id="9e9e2-155">Der vorherige Befehl erstellt ein Gerüst für das `Course`-Modell.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-155">The preceding command scaffolds the `Course` model.</span></span> <span data-ttu-id="9e9e2-156">Öffnen Sie das Projekt in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-156">Open the project in Visual Studio.</span></span>

<span data-ttu-id="9e9e2-157">Erstellen Sie das Projekt.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-157">Build the project.</span></span> <span data-ttu-id="9e9e2-158">Der Build generiert z.B. die folgenden Fehler:</span><span class="sxs-lookup"><span data-stu-id="9e9e2-158">The build generates errors like the following:</span></span>

`1>Pages/Courses/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Course' and no extension method 'Course' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 <span data-ttu-id="9e9e2-159">Ändern Sie `_context.Course` allgemein in `_context.Courses` (d.h., fügen Sie ein „s“ zu `Course` hinzu).</span><span class="sxs-lookup"><span data-stu-id="9e9e2-159">Globally change `_context.Course` to `_context.Courses` (that is, add an "s" to `Course`).</span></span> <span data-ttu-id="9e9e2-160">Der Begriff wurde siebenmal gefunden und aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-160">7 occurrences are found and updated.</span></span>

<span data-ttu-id="9e9e2-161">Öffnen Sie *Pages/Courses/Index.cshtml.cs*. Untersuchen Sie die `OnGetAsync`-Methode.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-161">Open *Pages/Courses/Index.cshtml.cs* and examine the `OnGetAsync` method.</span></span> <span data-ttu-id="9e9e2-162">Die Gerüstbauengine gibt Eager Loading für die `Department`-Navigationseigenschaft an.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-162">The scaffolding engine specified eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="9e9e2-163">Die `Include`-Methode gibt Eager Loading an.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-163">The `Include` method specifies eager loading.</span></span>

<span data-ttu-id="9e9e2-164">Führen Sie die Anwendung aus, und klicken Sie auf den Link **Kurse**.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-164">Run the app and select the **Courses** link.</span></span> <span data-ttu-id="9e9e2-165">Die Abteilungsspalte zeigt die `DepartmentID` an, die nicht hilfreich ist.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-165">The department column displays the `DepartmentID`, which isn't useful.</span></span>

<span data-ttu-id="9e9e2-166">Aktualisieren Sie die `OnGetAsync`-Methode mit folgendem Code:</span><span class="sxs-lookup"><span data-stu-id="9e9e2-166">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="9e9e2-167">Der vorangehende Code fügt `AsNoTracking` hinzu.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-167">The preceding code adds `AsNoTracking`.</span></span> <span data-ttu-id="9e9e2-168">`AsNoTracking` verbessert die Leistung, da die zurückgegebenen Entitäten nicht nachverfolgt werden.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-168">`AsNoTracking` improves performance because the entities returned are not tracked.</span></span> <span data-ttu-id="9e9e2-169">Die Entitäten werden nicht nachverfolgt, da sie nicht im aktuellen Kontext aktualisiert werden.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-169">The entities are not tracked because they're not updated in the current context.</span></span>

<span data-ttu-id="9e9e2-170">Aktualisieren Sie *Pages/Courses/Index.cshtml* mit dem folgenden hervorgehobenen Markup:</span><span class="sxs-lookup"><span data-stu-id="9e9e2-170">Update *Pages/Courses/Index.cshtml* with the following highlighted markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="9e9e2-171">Die folgenden Änderungen wurden am Codegerüst vorgenommen:</span><span class="sxs-lookup"><span data-stu-id="9e9e2-171">The following changes have been made to the scaffolded code:</span></span>

* <span data-ttu-id="9e9e2-172">Die Überschrift wurde von „Index“ in „Kurse“ geändert.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-172">Changed the heading from Index to Courses.</span></span>
* <span data-ttu-id="9e9e2-173">Die Spalte **Anzahl** wurde hinzugefügt. Sie zeigt den `CourseID`-Eigenschaftswert an.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-173">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="9e9e2-174">Primärschlüssel werden nicht standardmäßig eingerüstet, da sie normalerweise ohne Bedeutung für Endbenutzer sind.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-174">By default, primary keys aren't scaffolded because normally they're meaningless to end users.</span></span> <span data-ttu-id="9e9e2-175">Allerdings hat der Primärschlüssel in diesem Fall jedoch Bedeutung.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-175">However, in this case the primary key is meaningful.</span></span>
* <span data-ttu-id="9e9e2-176">Die Spalte **Abteilung** wurde geändert, sodass sie jetzt den Namen der Abteilung anzeigt.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-176">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="9e9e2-177">Der Code zeigt die `Name`-Eigenschaft der `Department`-Entität an, die in die `Department`-Navigationseigenschaft geladen wird:</span><span class="sxs-lookup"><span data-stu-id="9e9e2-177">The code displays the `Name` property of the `Department` entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="9e9e2-178">Führen Sie die Anwendung aus. Wählen Sie die Registerkarte **Kurse** aus, um die Liste mit den Abteilungsnamen anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-178">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Indexseite „Kurse“](read-related-data/_static/courses-index.png)

<a name="select"></a>
### <a name="loading-related-data-with-select"></a><span data-ttu-id="9e9e2-180">Laden verwandter Daten mit „Select“</span><span class="sxs-lookup"><span data-stu-id="9e9e2-180">Loading related data with Select</span></span>

<span data-ttu-id="9e9e2-181">Die `OnGetAsync`-Methode lädt verwandte Daten mit der `Include`-Methode:</span><span class="sxs-lookup"><span data-stu-id="9e9e2-181">The `OnGetAsync` method loads related data with the `Include` method:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="9e9e2-182">Der `Select`-Operator lädt nur die erforderlichen verwandten Daten.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-182">The `Select` operator loads only the related data needed.</span></span> <span data-ttu-id="9e9e2-183">Für die einzelnen Elemente wie `Department.Name` wird ein INNER JOIN von SQL verwendet.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-183">For single items, like the `Department.Name` it uses a SQL INNER JOIN.</span></span> <span data-ttu-id="9e9e2-184">Für Sammlungen wird ein anderer Zugriff auf die Datenbank verwendet, aber Gleiches gilt für den `Include`-Operator für Sammlungen.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-184">For collections, it uses another database access, but so does the `Include` operator on collections.</span></span>

<span data-ttu-id="9e9e2-185">Der folgende Code lädt verwandte Daten mit der `Select`-Methode:</span><span class="sxs-lookup"><span data-stu-id="9e9e2-185">The following code loads related data with the `Select` method:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="9e9e2-186">Die `CourseViewModel`:</span><span class="sxs-lookup"><span data-stu-id="9e9e2-186">The `CourseViewModel`:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

<span data-ttu-id="9e9e2-187">Ein vollständiges Beispiel finden Sie unter [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) und [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs).</span><span class="sxs-lookup"><span data-stu-id="9e9e2-187">See [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) and [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) for a complete example.</span></span>

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="9e9e2-188">Erstellen einer Dozentenseite, die Kurse und Registrierungen anzeigt</span><span class="sxs-lookup"><span data-stu-id="9e9e2-188">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="9e9e2-189">In diesem Abschnitt wird die Dozentenseite erstellt.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-189">In this section, the Instructors page is created.</span></span>

<a name="IP"></a>
<span data-ttu-id="9e9e2-190">![Indexseite „Dozenten“](read-related-data/_static/instructors-index.png)</span><span class="sxs-lookup"><span data-stu-id="9e9e2-190">![Instructors Index page](read-related-data/_static/instructors-index.png)</span></span>

<span data-ttu-id="9e9e2-191">Auf dieser Seite werden verwandte Daten auf folgende Weise gelesen und angezeigt:</span><span class="sxs-lookup"><span data-stu-id="9e9e2-191">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="9e9e2-192">Die Liste der Dozenten zeigt verwandte Daten aus der `OfficeAssignment`-Entität (Office in der vorherigen Abbildung).</span><span class="sxs-lookup"><span data-stu-id="9e9e2-192">The list of instructors displays related data from the `OfficeAssignment` entity (Office in the preceding image).</span></span> <span data-ttu-id="9e9e2-193">Die `Instructor`- und `OfficeAssignment`-Entitäten stehen in einer 1:0..1-Beziehung zueinander.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-193">The `Instructor` and `OfficeAssignment` entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="9e9e2-194">Eager Loading wird für die `OfficeAssignment`-Entitäten verwendet.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-194">Eager loading is used for the `OfficeAssignment` entities.</span></span> <span data-ttu-id="9e9e2-195">Eager Loading ist in der Regel effizienter, wenn die verwandten Daten angezeigt werden müssen.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-195">Eager loading is typically more efficient when the related data needs to be displayed.</span></span> <span data-ttu-id="9e9e2-196">In diesem Fall werden die Office-Zuweisungen für die Dozenten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-196">In this case, office assignments for the instructors are displayed.</span></span>
* <span data-ttu-id="9e9e2-197">Wenn der Benutzer einen Dozenten auswählt (Harui in der vorherigen Abbildung), werden verwandte `Course`-Entitäten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-197">When the user selects an instructor (Harui in the preceding image), related `Course` entities are displayed.</span></span> <span data-ttu-id="9e9e2-198">Die `Instructor`- und `Course`-Entitäten stehen in einer m:n-Beziehung zueinander.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-198">The `Instructor` and `Course` entities are in a many-to-many relationship.</span></span> <span data-ttu-id="9e9e2-199">Für die `Course`-Entitäten und ihre verwandten `Department`-Entitäten wird das Eager Loading verwendet.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-199">Eager loading is used for the `Course` entities and their related `Department` entities.</span></span> <span data-ttu-id="9e9e2-200">In diesem Fall können separate Abfragen effizienter sein, da nur Kurse für den ausgewählten Dozenten benötigt werden.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-200">In this case, separate queries might be more efficient because only courses for the selected instructor are needed.</span></span> <span data-ttu-id="9e9e2-201">Dieses Beispiel zeigt, wie Eager Loading für Navigationseigenschaften in Entitäten in den Navigationseigenschaften verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-201">This example shows how to use eager loading for navigation properties in entities that are in navigation properties.</span></span>
* <span data-ttu-id="9e9e2-202">Wenn der Benutzer einen Kurs auswählt (Chemie in der vorherigen Abbildung), werden verwandte Daten aus der `Enrollments`-Entität angezeigt.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-202">When the user selects a course (Chemistry in the preceding image), related data from the `Enrollments` entity is displayed.</span></span> <span data-ttu-id="9e9e2-203">In der vorherigen Abbildung sind der Name des Studenten und die Note angezeigt.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-203">In the preceding image, student name and grade are displayed.</span></span> <span data-ttu-id="9e9e2-204">Die `Course`- und `Enrollment`-Entitäten stehen in einer 1:n-Beziehung zueinander.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-204">The `Course` and `Enrollment` entities are in a one-to-many relationship.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="9e9e2-205">Erstellen eines Ansichtsmodells für die Indexansicht „Dozenten“</span><span class="sxs-lookup"><span data-stu-id="9e9e2-205">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="9e9e2-206">Die Dozentenseite zeigt Daten aus drei verschiedenen Tabellen.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-206">The instructors page shows data from three different tables.</span></span> <span data-ttu-id="9e9e2-207">Es wird ein Ansichtsmodell erstellt, das die drei Entitäten enthält, die die drei Tabellen darstellen.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-207">A view model is created that includes the three entities representing the three tables.</span></span>

<span data-ttu-id="9e9e2-208">Erstellen Sie im Ordner *SchoolViewModels* die Datei *InstructorIndexData.cs* mit dem folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="9e9e2-208">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a><span data-ttu-id="9e9e2-209">Gerüstbau für das Dozentenmodell</span><span class="sxs-lookup"><span data-stu-id="9e9e2-209">Scaffold the Instructor model</span></span>

* <span data-ttu-id="9e9e2-210">Beenden Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-210">Exit Visual Studio.</span></span>
* <span data-ttu-id="9e9e2-211">Öffnen Sie ein Befehlsfenster im Projektverzeichnis (das Verzeichnis mit den Dateien *Program.cs*, *Startup.cs*, und *CSPROJ*).</span><span class="sxs-lookup"><span data-stu-id="9e9e2-211">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="9e9e2-212">Führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="9e9e2-212">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
  ```

<span data-ttu-id="9e9e2-213">Der vorherige Befehl erstellt ein Gerüst für das `Instructor`-Modell.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-213">The preceding command scaffolds the `Instructor` model.</span></span> <span data-ttu-id="9e9e2-214">Öffnen Sie das Projekt in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-214">Open the project in Visual Studio.</span></span>

<span data-ttu-id="9e9e2-215">Erstellen Sie das Projekt.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-215">Build the project.</span></span> <span data-ttu-id="9e9e2-216">Der Build generiert Fehler.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-216">The build generates errors.</span></span>

<span data-ttu-id="9e9e2-217">Ändern Sie `_context.Instructor` allgemein in `_context.Instructors` (d.h., fügen Sie ein „s“ zu `Instructor` hinzu).</span><span class="sxs-lookup"><span data-stu-id="9e9e2-217">Globally change `_context.Instructor` to `_context.Instructors` (that is, add an "s" to `Instructor`).</span></span> <span data-ttu-id="9e9e2-218">Der Begriff wurde siebenmal gefunden und aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-218">7 occurrences are found and updated.</span></span>

<span data-ttu-id="9e9e2-219">Führen Sie die Anwendung aus, und navigieren Sie zur Dozentenseite.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-219">Run the app and navigate to the instructors page.</span></span>

<span data-ttu-id="9e9e2-220">Ersetzen Sie *Pages/Instructors/Index.cshtml.cs* durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="9e9e2-220">Replace *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,20-99)]

<span data-ttu-id="9e9e2-221">Die `OnGetAsync`-Methode akzeptiert optional Routendaten für die ID des ausgewählten Dozenten.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-221">The `OnGetAsync` method accepts optional route data for the ID of the selected instructor.</span></span>

<span data-ttu-id="9e9e2-222">Untersuchen Sie die Abfrage in der Datei *Pages/Instructors/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="9e9e2-222">Examine the query in the *Pages/Instructors/Index.cshtml.cs* file:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="9e9e2-223">Die Abfrage enthält zwei Dinge:</span><span class="sxs-lookup"><span data-stu-id="9e9e2-223">The query has two includes:</span></span>

* <span data-ttu-id="9e9e2-224">`OfficeAssignment`: In der [Dozentenansicht](#IP) angezeigt.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-224">`OfficeAssignment`: Displayed in the [instructors view](#IP).</span></span>
* <span data-ttu-id="9e9e2-225">`CourseAssignments`: Welche Kurse gegeben werden.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-225">`CourseAssignments`: Which brings in the courses taught.</span></span>


### <a name="update-the-instructors-index-page"></a><span data-ttu-id="9e9e2-226">Aktualisieren der Indexseite „Dozenten“</span><span class="sxs-lookup"><span data-stu-id="9e9e2-226">Update the instructors Index page</span></span>

<span data-ttu-id="9e9e2-227">Aktualisieren Sie *Pages/Instructors/Index.cshtml* mit folgendem Markup:</span><span class="sxs-lookup"><span data-stu-id="9e9e2-227">Update *Pages/Instructors/Index.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

<span data-ttu-id="9e9e2-228">Das oben stehende Markup führt die folgenden Änderungen durch:</span><span class="sxs-lookup"><span data-stu-id="9e9e2-228">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="9e9e2-229">Aktualisiert die `page`-Anweisung von `@page` auf `@page "{id:int?}"`.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-229">Updates the `page` directive from `@page` to `@page "{id:int?}"`.</span></span> <span data-ttu-id="9e9e2-230">`"{id:int?}"` ist eine Routenvorlage.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-230">`"{id:int?}"` is a route template.</span></span> <span data-ttu-id="9e9e2-231">Die Routenvorlage ändert ganzzahlige Abfragezeichenfolgen in der URL in Routendaten.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-231">The route template changes integer query strings in the URL to route data.</span></span> <span data-ttu-id="9e9e2-232">Klicken Sie beispielsweise auf den Link **Auswählen** für einen Dozenten, wenn nur die `@page`-Anweisung eine URL wie die folgende erzeugt:</span><span class="sxs-lookup"><span data-stu-id="9e9e2-232">For example, clicking on the **Select** link for an instructor with only the `@page` directive produces a URL like the following:</span></span>

    `http://localhost:1234/Instructors?id=2`

    <span data-ttu-id="9e9e2-233">Wenn die Seitenanweisung `@page "{id:int?}"` ist, sieht die vorherige URL wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="9e9e2-233">When the page directive is `@page "{id:int?}"`, the previous URL is:</span></span>

    `http://localhost:1234/Instructors/2`

* <span data-ttu-id="9e9e2-234">Der Seitentitel lautet **Dozenten**.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-234">Page title is **Instructors**.</span></span>
* <span data-ttu-id="9e9e2-235">Es wurde eine **Office**-Spalte hinzugefügt, die `item.OfficeAssignment.Location` nur anzeigt, wenn `item.OfficeAssignment` nicht gleich 0 (null) ist.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-235">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` isn't null.</span></span> <span data-ttu-id="9e9e2-236">Da dies eine 1:0..1-Beziehung ist, gibt es möglicherweise keine verwandte OfficeAssignment-Entität.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-236">Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="9e9e2-237">Es wurde eine **Kurse**-Spalte hinzugefügt, die die Kurse eines jeden Dozenten anzeigt.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-237">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="9e9e2-238">Weitere Informationen über diese Razor-Syntax finden Sie unter [Explizite Zeilenübergänge mit `@:`](xref:mvc/views/razor#explicit-line-transition-with-).</span><span class="sxs-lookup"><span data-stu-id="9e9e2-238">See [Explicit Line Transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="9e9e2-239">Es wurde Code hinzugefügt, der `class="success"` dynamisch zum `tr`-Element des ausgewählten Dozenten hinzufügt.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-239">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="9e9e2-240">Hiermit wird eine Hintergrundfarbe mit einer Bootstrapklasse für die ausgewählte Zeile hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-240">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="9e9e2-241">Es wurde ein neuer Link mit der Bezeichnung **Auswählen** hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-241">Added a new hyperlink labeled **Select**.</span></span> <span data-ttu-id="9e9e2-242">Dieser Link sendet die ID des ausgewählten Dozenten an die `Index`-Methode und legt die Hintergrundfarbe fest.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-242">This link sends the selected instructor's ID to the `Index` method and sets a background color.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="9e9e2-243">Führen Sie die Anwendung aus. Klicken Sie auf die Registerkarte **Dozenten**. Die Seite zeigt den `Location` (Office) aus der verwandten `OfficeAssignment`-Entität an.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-243">Run the app and select the **Instructors** tab. The page displays the `Location` (office) from the related `OfficeAssignment` entity.</span></span> <span data-ttu-id="9e9e2-244">Wenn OfficeAssignment gleich ist 0 (null), wird eine leere Tabellenzelle angezeigt.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-244">If OfficeAssignment\` is null, an empty table cell is displayed.</span></span>

![Indexseite „Dozenten“, nichts ausgewählt](read-related-data/_static/instructors-index-no-selection.png)

<span data-ttu-id="9e9e2-246">Klicken Sie auf den Link **Auswählen**.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-246">Click on the **Select** link.</span></span> <span data-ttu-id="9e9e2-247">Der Zeilenstil verändert sich.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-247">The row style changes.</span></span>

### <a name="add-courses-taught-by-selected-instructor"></a><span data-ttu-id="9e9e2-248">Hinzufügen von Kursen eines ausgewählten Dozenten</span><span class="sxs-lookup"><span data-stu-id="9e9e2-248">Add courses taught by selected instructor</span></span>

<span data-ttu-id="9e9e2-249">Aktualisieren Sie die `OnGetAsync`-Methode in *Pages/Instructors/Index.cshtml.cs* mit dem folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="9e9e2-249">Update the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-999)]

<span data-ttu-id="9e9e2-250">Fügen Sie `public int CourseID { get; set; }` hinzu.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-250">Add `public int CourseID { get; set; }`</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_1&highlight=12)]

<span data-ttu-id="9e9e2-251">Überprüfen Sie die aktualisierte Abfrage:</span><span class="sxs-lookup"><span data-stu-id="9e9e2-251">Examine the updated query:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="9e9e2-252">Die vorhergehende Abfrage fügt die `Department`-Entitäten hinzu.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-252">The preceding query adds the `Department` entities.</span></span>

<span data-ttu-id="9e9e2-253">Der folgende Code wird ausgeführt, wenn ein Dozent ausgewählt wird (`id != null`).</span><span class="sxs-lookup"><span data-stu-id="9e9e2-253">The following code executes when an instructor is selected (`id != null`).</span></span> <span data-ttu-id="9e9e2-254">Der ausgewählte Dozent wird aus der Liste der Dozenten im Ansichtsmodell abgerufen.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-254">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="9e9e2-255">Die `Courses`-Eigenschaft des Ansichtsmodells wird mit den `Course`-Entitäten aus der `CourseAssignments`-Navigationseigenschaft dieses Dozenten geladen.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-255">The view model's `Courses` property is loaded with the `Course` entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

<span data-ttu-id="9e9e2-256">Die `Where`-Methode gibt eine Sammlung zurück.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-256">The `Where` method returns a collection.</span></span> <span data-ttu-id="9e9e2-257">In der vorherigen `Where`-Methode wird nur eine einzige `Instructor`-Entität zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-257">In the preceding `Where` method, only a single `Instructor` entity is returned.</span></span> <span data-ttu-id="9e9e2-258">Die `Single`-Methode konvertiert die Sammlung in eine einzelne `Instructor`-Entität.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-258">The `Single` method converts the collection into a single `Instructor` entity.</span></span> <span data-ttu-id="9e9e2-259">Die `Instructor`-Entität ermöglicht Zugriff auf die `CourseAssignments`-Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-259">The `Instructor` entity provides access to the `CourseAssignments` property.</span></span> <span data-ttu-id="9e9e2-260">`CourseAssignments` ermöglicht Zugriff auf die verwandten `Course`-Entitäten.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-260">`CourseAssignments` provides access to the related `Course` entities.</span></span>

![Dozent:Kurse m:N](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="9e9e2-262">Die `Single`-Methode wird für eine Sammlung verwendet, wenn die Sammlung nur ein Element aufweist.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-262">The `Single` method is used on a collection when the collection has only one item.</span></span> <span data-ttu-id="9e9e2-263">Die `Single`-Methode löst eine Ausnahme aus, wenn die Sammlung leer ist oder mehr als ein Element vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-263">The `Single` method throws an exception if the collection is empty or if there's more than one item.</span></span> <span data-ttu-id="9e9e2-264">Eine Alternative ist `SingleOrDefault`, womit ein Standardwert (in diesem Fall 0 (null)) zurückgegeben wird, wenn die Sammlung leer ist.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-264">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="9e9e2-265">Für eine leere Sammlung wird `SingleOrDefault` verwendet:</span><span class="sxs-lookup"><span data-stu-id="9e9e2-265">Using `SingleOrDefault` on an empty collection:</span></span>

* <span data-ttu-id="9e9e2-266">Löst eine Ausnahme aus (auf der Suche nach einer `Courses`-Eigenschaft eines Nullverweises).</span><span class="sxs-lookup"><span data-stu-id="9e9e2-266">Results in an exception (from trying to find a `Courses` property on a null reference).</span></span>
* <span data-ttu-id="9e9e2-267">Die Ausnahmemeldung würde die Ursache des Problems weniger deutlich angeben.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-267">The exception message would less clearly indicate the cause of the problem.</span></span>

<span data-ttu-id="9e9e2-268">Wenn ein Kurs ausgewählt ist, füllt der folgende Code die `Enrollments`-Eigenschaft des Ansichtsmodells:</span><span class="sxs-lookup"><span data-stu-id="9e9e2-268">The following code populates the view model's `Enrollments` property when a course is selected:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

<span data-ttu-id="9e9e2-269">Fügen Sie das folgende Markup zum Ende der Razor-Seite *Pages/Instructors/Index.cshtml* hinzu:</span><span class="sxs-lookup"><span data-stu-id="9e9e2-269">Add the following markup to the end of the *Pages/Instructors/Index.cshtml* Razor Page:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-999)]

<span data-ttu-id="9e9e2-270">Das vorhergehende Markup zeigt eine Liste der Kurse eines bestimmten Dozenten an, wenn einer ausgewählt wird.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-270">The preceding markup displays a list of courses related to an instructor when an instructor is selected.</span></span>

<span data-ttu-id="9e9e2-271">Testen Sie die App.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-271">Test the app.</span></span> <span data-ttu-id="9e9e2-272">Klicken Sie auf den Link **Auswählen** auf der Dozentenseite.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-272">Click on a **Select** link on the instructors page.</span></span>

![Indexseite „Dozenten“, Dozent ausgewählt](read-related-data/_static/instructors-index-instructor-selected.png)

### <a name="show-student-data"></a><span data-ttu-id="9e9e2-274">Anzeigen der Studentendaten</span><span class="sxs-lookup"><span data-stu-id="9e9e2-274">Show student data</span></span>

<span data-ttu-id="9e9e2-275">In diesem Abschnitt wird die Anwendung aktualisiert, um die Studentendaten für einen ausgewählten Kurs anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-275">In this section, the app is updated to show the student data for a selected course.</span></span>

<span data-ttu-id="9e9e2-276">Aktualisieren Sie die Abfrage in der `OnGetAsync`-Methode in *Pages/Instructors/Index.cshtml.cs* mit dem folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="9e9e2-276">Update the query in the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="9e9e2-277">Aktualisieren Sie *Pages/Instructors/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-277">Update *Pages/Instructors/Index.cshtml*.</span></span> <span data-ttu-id="9e9e2-278">Fügen Sie dem Dateiende das folgende Markup hinzu:</span><span class="sxs-lookup"><span data-stu-id="9e9e2-278">Add the following markup to the end of the file:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

<span data-ttu-id="9e9e2-279">Das vorhergehende Markup zeigt eine Liste der Studenten, die im ausgewählten Kurs registriert sind.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-279">The preceding markup displays a list of the students who are enrolled in the selected course.</span></span>

<span data-ttu-id="9e9e2-280">Aktualisieren Sie die Seite. Wählen Sie einen Dozenten aus.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-280">Refresh the page and select an instructor.</span></span> <span data-ttu-id="9e9e2-281">Wählen Sie einen Kurs aus, um die Liste der registrierten Studenten und deren Noten einzusehen.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-281">Select a course to see the list of enrolled students and their grades.</span></span>

![Indexseite „Dozenten“, Dozent und Kurs ausgewählt](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a><span data-ttu-id="9e9e2-283">Verwenden von „Single“</span><span class="sxs-lookup"><span data-stu-id="9e9e2-283">Using Single</span></span>

<span data-ttu-id="9e9e2-284">Die `Single`-Methode kann die `Where`-Bedingung übergehen, anstatt die `Where`-Methode separat aufzurufen:</span><span class="sxs-lookup"><span data-stu-id="9e9e2-284">The `Single` method can pass in the `Where` condition instead of calling the `Where` method separately:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21,28-29)]

<span data-ttu-id="9e9e2-285">Der vorangehende `Single`-Ansatz bietet keine Vorteile gegenüber `Where`.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-285">The preceding `Single` approach provides no benefits over using `Where`.</span></span> <span data-ttu-id="9e9e2-286">Einige Entwickler bevorzugen einfach den `Single`-Ansatz.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-286">Some developers prefer the `Single` approach style.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="9e9e2-287">Explizites Laden</span><span class="sxs-lookup"><span data-stu-id="9e9e2-287">Explicit loading</span></span>

<span data-ttu-id="9e9e2-288">Der aktuelle Code gibt Eager Loading für `Enrollments` und `Students` an:</span><span class="sxs-lookup"><span data-stu-id="9e9e2-288">The current code specifies eager loading for `Enrollments` and `Students`:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="9e9e2-289">Angenommen, Benutzer möchten Registrierungen für einen Kurs nur selten anzeigen lassen.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-289">Suppose users rarely want to see enrollments in a course.</span></span> <span data-ttu-id="9e9e2-290">In diesem Fall wäre es eine Optimierung, die Registrierungsdaten nur dann zu laden, wenn diese angefordert werden.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-290">In that case, an optimization would be to only load the enrollment data if it's requested.</span></span> <span data-ttu-id="9e9e2-291">In diesem Abschnitt wird die `OnGetAsync` aktualisiert, um das explizite Laden von `Enrollments` und `Students` zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-291">In this section, the `OnGetAsync` is updated to use explicit loading of `Enrollments` and `Students`.</span></span>

<span data-ttu-id="9e9e2-292">Aktualisieren Sie `OnGetAsync` mit dem folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="9e9e2-292">Update the `OnGetAsync` with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

<span data-ttu-id="9e9e2-293">Der vorangehende Code löscht die *ThenInclude*-Methodenaufrufe für Registrierung und Studentendaten.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-293">The preceding code drops the *ThenInclude* method calls for enrollment and student data.</span></span> <span data-ttu-id="9e9e2-294">Wenn ein Kurs ausgewählt ist, ruft der hervorgehobene Code Folgendes ab:</span><span class="sxs-lookup"><span data-stu-id="9e9e2-294">If a course is selected, the highlighted code retrieves:</span></span>

* <span data-ttu-id="9e9e2-295">Die `Enrollment`-Entitäten für den ausgewählten Kurs.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-295">The `Enrollment` entities for the selected course.</span></span>
* <span data-ttu-id="9e9e2-296">Die `Student`-Entitäten für jede `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-296">The `Student` entities for each `Enrollment`.</span></span>

<span data-ttu-id="9e9e2-297">Beachten Sie, dass der vorherige Code `.AsNoTracking()` auskommentiert.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-297">Notice the preceding code comments out `.AsNoTracking()`.</span></span> <span data-ttu-id="9e9e2-298">Navigationseigenschaften können nur für nachverfolgte Entitäten explizit geladen werden.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-298">Navigation properties can only be explicitly loaded for tracked entities.</span></span>

<span data-ttu-id="9e9e2-299">Testen Sie die App.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-299">Test the app.</span></span> <span data-ttu-id="9e9e2-300">Aus Benutzersicht verhält sich die Anwendung identisch mit der vorherigen Version.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-300">From a users perspective, the app behaves identically to the previous version.</span></span>

<span data-ttu-id="9e9e2-301">Das nächste Tutorial zeigt, wie verwandte Daten aktualisiert werden.</span><span class="sxs-lookup"><span data-stu-id="9e9e2-301">The next tutorial shows how to update related data.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="9e9e2-302">[Zurück](xref:data/ef-rp/complex-data-model)
>[Weiter](xref:data/ef-rp/update-related-data)</span><span class="sxs-lookup"><span data-stu-id="9e9e2-302">[Previous](xref:data/ef-rp/complex-data-model)
[Next](xref:data/ef-rp/update-related-data)</span></span>
