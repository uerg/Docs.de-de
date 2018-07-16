---
title: ASP.NET Core MVC mit EF Core – Lesen verwandter Daten (6 von 10)
author: rick-anderson
description: In diesem Tutorial lesen Sie verwandte Daten und zeigen sie an – d.h., die Daten, die Entity Framework in Navigationseigenschaften lädt.
ms.author: tdykstra
ms.date: 03/15/2017
uid: data/ef-mvc/read-related-data
ms.openlocfilehash: a310c9e4b9cec6e2ab2477461f395c9bbd3fa364
ms.sourcegitcommit: e12f45ddcbe99102a74d4077df27d6c0ebba49c1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/15/2018
ms.locfileid: "39063285"
---
# <a name="aspnet-core-mvc-with-ef-core---read-related-data---6-of-10"></a><span data-ttu-id="35825-103">ASP.NET Core MVC mit EF Core – Lesen verwandter Daten (6 von 10)</span><span class="sxs-lookup"><span data-stu-id="35825-103">ASP.NET Core MVC with EF Core - Read Related Data - 6 of 10</span></span>

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc-21.md)]

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="35825-104">Von [Tom Dykstra](https://github.com/tdykstra) und [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="35825-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="35825-105">Die Contoso University-Beispielwebanwendung veranschaulicht, wie ASP.NET Core MVC-Webanwendungen mit Entity Framework Core und Visual Studio erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="35825-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="35825-106">Informationen zu dieser Tutorialreihe finden Sie im [ersten Tutorial der Reihe](intro.md).</span><span class="sxs-lookup"><span data-stu-id="35825-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="35825-107">Im vorherigen Tutorial haben Sie das Schuldatenmodell abgeschlossen.</span><span class="sxs-lookup"><span data-stu-id="35825-107">In the previous tutorial, you completed the School data model.</span></span> <span data-ttu-id="35825-108">In diesem Tutorial lesen Sie verwandte Daten und zeigen sie an – d.h., die Daten, die Entity Framework in Navigationseigenschaften lädt.</span><span class="sxs-lookup"><span data-stu-id="35825-108">In this tutorial, you'll read and display related data -- that is, data that the Entity Framework loads into navigation properties.</span></span>

<span data-ttu-id="35825-109">Die folgenden Abbildungen zeigen die Seiten, mit den Sie arbeiten werden.</span><span class="sxs-lookup"><span data-stu-id="35825-109">The following illustrations show the pages that you'll work with.</span></span>

![Indexseite „Kurse“](read-related-data/_static/courses-index.png)

![Indexseite „Dozenten“](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="35825-112">Explizites, Eager und Lazy Loading verwandter Daten</span><span class="sxs-lookup"><span data-stu-id="35825-112">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="35825-113">Es gibt mehrere Möglichkeiten, mit denen Software für objektrelationales Mapping (ORM), z.B. Entity Framework, verwandte Daten in die Navigationseigenschaften einer Entität laden kann:</span><span class="sxs-lookup"><span data-stu-id="35825-113">There are several ways that Object-Relational Mapping (ORM) software such as Entity Framework can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="35825-114">Eager Loading (vorzeitiges Laden).</span><span class="sxs-lookup"><span data-stu-id="35825-114">Eager loading.</span></span> <span data-ttu-id="35825-115">Wenn die Entität gelesen wird, werden ihre verwandten Daten mit ihr abgerufen.</span><span class="sxs-lookup"><span data-stu-id="35825-115">When the entity is read, related data is retrieved along with it.</span></span> <span data-ttu-id="35825-116">Dies führt normalerweise zu einer einzelnen Joinabfrage, die alle Daten abruft, die erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="35825-116">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="35825-117">Sie geben Eager Loading in Entity Framework Core mit den `Include`- und `ThenInclude`-Methoden an.</span><span class="sxs-lookup"><span data-stu-id="35825-117">You specify eager loading in Entity Framework Core by using the `Include` and `ThenInclude` methods.</span></span>

  ![Beispiel für Eager Loading](read-related-data/_static/eager-loading.png)

  <span data-ttu-id="35825-119">Sie können einige der Daten in separaten Abfragen abrufen und EF „korrigiert“ die Navigationseigenschaften.</span><span class="sxs-lookup"><span data-stu-id="35825-119">You can retrieve some of the data in separate queries, and EF "fixes up" the navigation properties.</span></span>  <span data-ttu-id="35825-120">D.h., dass EF automatisch die separat abgerufenen Entitäten in die Navigationseigenschaften der zuvor abgerufenen Entitäten hinzufügt.</span><span class="sxs-lookup"><span data-stu-id="35825-120">That is, EF automatically adds the separately retrieved entities where they belong in navigation properties of previously retrieved entities.</span></span> <span data-ttu-id="35825-121">Für die Abfrage, die verwandte Daten abruft, können Sie die `Load`-Methode verwenden, anstelle einer Methode, die eine Liste oder ein Objekt wie `ToList` oder `Single` zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="35825-121">For the query that retrieves related data, you can use the `Load` method instead of a method that returns a list or object, such as `ToList` or `Single`.</span></span>

  ![Beispiel für separate Abfragen](read-related-data/_static/separate-queries.png)

* <span data-ttu-id="35825-123">Explizites Laden.</span><span class="sxs-lookup"><span data-stu-id="35825-123">Explicit loading.</span></span> <span data-ttu-id="35825-124">Wenn die Entität zuerst gelesen wird, werden verwandte Daten nicht abgerufen.</span><span class="sxs-lookup"><span data-stu-id="35825-124">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="35825-125">Sie schreiben Code, der die verwandten Daten abruft, wenn erforderlich.</span><span class="sxs-lookup"><span data-stu-id="35825-125">You write code that retrieves the related data if it's needed.</span></span> <span data-ttu-id="35825-126">So wie im Fall von Eager Loading mit separaten Abfragen führt explizites Laden zu mehreren Abfragen, die an die Datenbank gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="35825-126">As in the case of eager loading with separate queries, explicit loading results in multiple queries sent to the database.</span></span> <span data-ttu-id="35825-127">Der Unterschied liegt darin, dass beim expliziten Laden der Code die zu ladenden Navigationseigenschaften angibt.</span><span class="sxs-lookup"><span data-stu-id="35825-127">The difference is that with explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="35825-128">In Entity Framework Core 1.1 können Sie die `Load`-Methode verwenden, um das explizite Laden auszuführen.</span><span class="sxs-lookup"><span data-stu-id="35825-128">In Entity Framework Core 1.1 you can use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="35825-129">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="35825-129">For example:</span></span>

  ![Beispiel für explizites Laden](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="35825-131">Lazy Loading (verzögertes Laden).</span><span class="sxs-lookup"><span data-stu-id="35825-131">Lazy loading.</span></span> <span data-ttu-id="35825-132">Wenn die Entität zuerst gelesen wird, werden verwandte Daten nicht abgerufen.</span><span class="sxs-lookup"><span data-stu-id="35825-132">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="35825-133">Wenn Sie jedoch zum ersten Mal versuchen, auf eine Navigationseigenschaft zuzugreifen, werden die für diese Navigationseigenschaft erforderlichen Daten automatisch abgerufen.</span><span class="sxs-lookup"><span data-stu-id="35825-133">However, the first time you attempt to access a navigation property, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="35825-134">Jedes Mal, wenn Sie zum ersten mal versuchen, Daten von einer Navigationseigenschaft abzurufen, wird eine Anfrage an die Datenbank gesendet.</span><span class="sxs-lookup"><span data-stu-id="35825-134">A query is sent to the database each time you try to get data from a navigation property for the first time.</span></span> <span data-ttu-id="35825-135">Entity Framework Core 1.0 unterstützt das Lazy Loading nicht.</span><span class="sxs-lookup"><span data-stu-id="35825-135">Entity Framework Core 1.0 doesn't support lazy loading.</span></span>

### <a name="performance-considerations"></a><span data-ttu-id="35825-136">Überlegungen zur Leistung</span><span class="sxs-lookup"><span data-stu-id="35825-136">Performance considerations</span></span>

<span data-ttu-id="35825-137">Wenn Sie wissen, dass Sie für jede abgerufene Entität verwandte Daten benötigen, bietet Eager Loading häufig die beste Leistung, da eine einzelne Abfrage, die an die Datenbank gesendet wird, in der Regel effizienter ist als separate Abfragen für jede abgerufene Entität.</span><span class="sxs-lookup"><span data-stu-id="35825-137">If you know you need related data for every entity retrieved, eager loading often offers the best performance, because a single query sent to the database is typically more efficient than separate queries for each entity retrieved.</span></span> <span data-ttu-id="35825-138">Nehmen wir beispielsweise an, dass jede Abteilung zehn verwandte Kurse hat.</span><span class="sxs-lookup"><span data-stu-id="35825-138">For example, suppose that each department has ten related courses.</span></span> <span data-ttu-id="35825-139">Eager Loading aller verwandter Daten würde zu nur einer einzelnen (Join)-Abfrage und einem einzelnen Roundtrip zur Datenbank führen.</span><span class="sxs-lookup"><span data-stu-id="35825-139">Eager loading of all related data would result in just a single (join) query and a single round trip to the database.</span></span> <span data-ttu-id="35825-140">Eine separate Abfrage für Kurse jeder Abteilung würde zu elf Roundtrips zur Datenbank führen.</span><span class="sxs-lookup"><span data-stu-id="35825-140">A separate query for courses for each department would result in eleven round trips to the database.</span></span> <span data-ttu-id="35825-141">Die zusätzlichen Roundtrips zur Datenbank beeinträchtigen die Leistung besonders bei hoher Latenz.</span><span class="sxs-lookup"><span data-stu-id="35825-141">The extra round trips to the database are especially detrimental to performance when latency is high.</span></span>

<span data-ttu-id="35825-142">Andererseits sind separate Abfragen in einigen Szenarios jedoch effizienter.</span><span class="sxs-lookup"><span data-stu-id="35825-142">On the other hand, in some scenarios separate queries is more efficient.</span></span> <span data-ttu-id="35825-143">Eager Loading aller verwandter Daten in einer Abfrage könnte eine sehr komplexe Verknüpfung generieren, die der SQL Server nicht effizient verarbeiten kann.</span><span class="sxs-lookup"><span data-stu-id="35825-143">Eager loading of all related data in one query might cause a very complex join to be generated, which SQL Server can't process efficiently.</span></span> <span data-ttu-id="35825-144">Oder angenommen, Sie benötigen nur Zugriff auf Navigationseigenschaften einer Entität, um auf eine Teilmenge der Reihe von Entitäten, die Sie verarbeiten, zuzugreifen. In diesem Fall können separate Abfragen möglicherweise besser ausgeführt werden, da Eager Loading aller Elemente im Vorfeld mehr Daten als benötigt abrufen würde.</span><span class="sxs-lookup"><span data-stu-id="35825-144">Or if you need to access an entity's navigation properties only for a subset of a set of the entities you're processing, separate queries might perform better because eager loading of everything up front would retrieve more data than you need.</span></span> <span data-ttu-id="35825-145">Wenn die Leistung wichtig ist, empfiehlt es sich, die Leistung mit beiden Möglichkeiten zu testen, um die beste Wahl treffen zu können.</span><span class="sxs-lookup"><span data-stu-id="35825-145">If performance is critical, it's best to test performance both ways in order to make the best choice.</span></span>

## <a name="create-a-courses-page-that-displays-department-name"></a><span data-ttu-id="35825-146">Erstellen einer Kursseite, die Abteilungsnamen anzeigt</span><span class="sxs-lookup"><span data-stu-id="35825-146">Create a Courses page that displays Department name</span></span>

<span data-ttu-id="35825-147">Die Kursentität enthält eine Navigationseigenschaft, die die Abteilungsentität der Abteilung enthält, der der Kurs zugewiesen ist.</span><span class="sxs-lookup"><span data-stu-id="35825-147">The Course entity includes a navigation property that contains the Department entity of the department that the course is assigned to.</span></span> <span data-ttu-id="35825-148">Sie benötigen die Namenseigenschaft der Abteilungsentität in der `Course.Department`-Navigationseigenschaft, um den Namen der zugewiesenen Abteilung in einer Kursliste anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="35825-148">To display the name of the assigned department in a list of courses, you need to get the Name property from the Department entity that's in the `Course.Department` navigation property.</span></span>

<span data-ttu-id="35825-149">Erstellen Sie einen Controller mit dem Namen CoursesController für den Kursentitätstyp. Verwenden Sie dieselben Optionen für den  **MVC-Controller mit Ansichten unter Verwendung des Entity Framework**-Gerüstbauers, die Sie zuvor für den Studentencontroller verwendet haben, wie in der folgenden Abbildung gezeigt:</span><span class="sxs-lookup"><span data-stu-id="35825-149">Create a controller named CoursesController for the Course entity type, using the same options for the **MVC Controller with views, using Entity Framework** scaffolder that you did earlier for the Students controller, as shown in the following illustration:</span></span>

![Hinzufügen eines Kursecontrollers](read-related-data/_static/add-courses-controller.png)

<span data-ttu-id="35825-151">Öffnen Sie *CoursesController.cs*, und untersuchen Sie die `Index`-Methode.</span><span class="sxs-lookup"><span data-stu-id="35825-151">Open *CoursesController.cs* and examine the `Index` method.</span></span> <span data-ttu-id="35825-152">Der automatische Gerüstbau hat mithilfe der `Include`-Methode ein Eager Loading für die `Department`-Navigationseigenschaft angegeben.</span><span class="sxs-lookup"><span data-stu-id="35825-152">The automatic scaffolding has specified eager loading for the `Department` navigation property by using the `Include` method.</span></span>

<span data-ttu-id="35825-153">Ersetzen Sie die `Index`-Methode durch den folgenden Code, der einen geeigneteren Namen für `IQueryable` verwendet, der Kursentitäten (`courses` anstelle von `schoolContext`) zurückgibt:</span><span class="sxs-lookup"><span data-stu-id="35825-153">Replace the `Index` method with the following code that uses a more appropriate name for the `IQueryable` that returns Course entities (`courses` instead of `schoolContext`):</span></span>

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="35825-154">Öffnen Sie *Views/Courses/Index.cshtml*. Ersetzen Sie den Vorlagencode durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="35825-154">Open *Views/Courses/Index.cshtml* and replace the template code with the following code.</span></span> <span data-ttu-id="35825-155">Die Änderungen werden hervorgehoben:</span><span class="sxs-lookup"><span data-stu-id="35825-155">The changes are highlighted:</span></span>

[!code-html[](intro/samples/cu/Views/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="35825-156">Sie haben die folgenden Änderungen am eingerüsteten Code vorgenommen:</span><span class="sxs-lookup"><span data-stu-id="35825-156">You've made the following changes to the scaffolded code:</span></span>

* <span data-ttu-id="35825-157">Die Überschrift wurde von „Index“ in „Kurse“ geändert.</span><span class="sxs-lookup"><span data-stu-id="35825-157">Changed the heading from Index to Courses.</span></span>

* <span data-ttu-id="35825-158">Die Spalte **Anzahl** wurde hinzugefügt. Sie zeigt den `CourseID`-Eigenschaftswert an.</span><span class="sxs-lookup"><span data-stu-id="35825-158">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="35825-159">Primärschlüssel werden nicht standardmäßig eingerüstet, da sie normalerweise ohne Bedeutung für Endbenutzer sind.</span><span class="sxs-lookup"><span data-stu-id="35825-159">By default, primary keys aren't scaffolded because normally they're meaningless to end users.</span></span> <span data-ttu-id="35825-160">Allerdings ist in diesem Fall der Primärschlüssel sinnvoll, und Sie möchten ihn anzeigen.</span><span class="sxs-lookup"><span data-stu-id="35825-160">However, in this case the primary key is meaningful and you want to show it.</span></span>

* <span data-ttu-id="35825-161">Die Spalte **Abteilung** wurde geändert, sodass sie jetzt den Namen der Abteilung anzeigt.</span><span class="sxs-lookup"><span data-stu-id="35825-161">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="35825-162">Der Code zeigt die `Name`-Eigenschaft der Abteilungsentität an, die in die `Department`-Navigationseigenschaft geladen wird:</span><span class="sxs-lookup"><span data-stu-id="35825-162">The code displays the `Name` property of the Department entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="35825-163">Führen Sie die Anwendung aus, und wählen Sie die Registerkarte **Kurse** aus, um die Liste mit den Abteilungsnamen anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="35825-163">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Indexseite „Kurse“](read-related-data/_static/courses-index.png)

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="35825-165">Erstellen einer Dozentenseite, die Kurse und Registrierungen anzeigt</span><span class="sxs-lookup"><span data-stu-id="35825-165">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="35825-166">In diesem Abschnitt müssen Sie einen Controller und eine Ansicht für die Instructor-Entität erstellen, um die Dozentenseite anzuzeigen:</span><span class="sxs-lookup"><span data-stu-id="35825-166">In this section, you'll create a controller and view for the Instructor entity in order to display the Instructors page:</span></span>

![Indexseite „Dozenten“](read-related-data/_static/instructors-index.png)

<span data-ttu-id="35825-168">Auf dieser Seite werden verwandte Daten auf folgende Weise gelesen und angezeigt:</span><span class="sxs-lookup"><span data-stu-id="35825-168">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="35825-169">Die Liste der Dozenten zeigt verwandte Daten aus der OfficeAssignment-Entität.</span><span class="sxs-lookup"><span data-stu-id="35825-169">The list of instructors displays related data from the OfficeAssignment entity.</span></span> <span data-ttu-id="35825-170">Die Instructor- und OfficeAssignment-Entitäten sind in einer 1:0..1-Beziehung.</span><span class="sxs-lookup"><span data-stu-id="35825-170">The Instructor and OfficeAssignment entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="35825-171">Sie werden Eager Loading für die OfficeAssignment-Entitäten verwenden.</span><span class="sxs-lookup"><span data-stu-id="35825-171">You'll use eager loading for the OfficeAssignment entities.</span></span> <span data-ttu-id="35825-172">Wie zuvor erläutert, ist Eager Loading in der Regel effizienter, wenn Sie die verwandten Daten für alle abgerufenen Zeilen der primären Tabelle benötigen.</span><span class="sxs-lookup"><span data-stu-id="35825-172">As explained earlier, eager loading is typically more efficient when you need the related data for all retrieved rows of the primary table.</span></span> <span data-ttu-id="35825-173">In diesem Fall sollten Sie die Office-Anweisungen für alle angezeigten Dozenten anzeigen.</span><span class="sxs-lookup"><span data-stu-id="35825-173">In this case, you want to display office assignments for all displayed instructors.</span></span>

* <span data-ttu-id="35825-174">Wenn der Benutzer einen Dozenten auswählt, werden verwandte Course-Entitäten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="35825-174">When the user selects an instructor, related Course entities are displayed.</span></span> <span data-ttu-id="35825-175">Die Instructor- und Course-Entitäten stehen in einer m:n-Beziehung zueinander.</span><span class="sxs-lookup"><span data-stu-id="35825-175">The Instructor and Course entities are in a many-to-many relationship.</span></span> <span data-ttu-id="35825-176">Sie werden Eager Loading für die Kursentitäten und ihre zugehörigen Abteilungsentitäten verwenden.</span><span class="sxs-lookup"><span data-stu-id="35825-176">You'll use eager loading for the Course entities and their related Department entities.</span></span> <span data-ttu-id="35825-177">In diesem Fall können separate Abfrage effizienter sein, da nur Kurse für den ausgewählten Dozenten benötigt werden.</span><span class="sxs-lookup"><span data-stu-id="35825-177">In this case, separate queries might be more efficient because you need courses only for the selected instructor.</span></span> <span data-ttu-id="35825-178">Dieses Beispiel zeigt jedoch, wie Eager Loading für Navigationseigenschaften in Entitäten in den Navigationseigenschaften verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="35825-178">However, this example shows how to use eager loading for navigation properties within entities that are themselves in navigation properties.</span></span>

* <span data-ttu-id="35825-179">Wenn der Benutzer einen Kurs auswählt, werden verwandte Daten aus der Registrierungsentität angezeigt.</span><span class="sxs-lookup"><span data-stu-id="35825-179">When the user selects a course, related data from the Enrollments entity set is displayed.</span></span> <span data-ttu-id="35825-180">Die Kurs- und Registrierungsentitäten sind in einer 1:n-Beziehung.</span><span class="sxs-lookup"><span data-stu-id="35825-180">The Course and Enrollment entities are in a one-to-many relationship.</span></span> <span data-ttu-id="35825-181">Sie werden separate Abfragen für Registrierungsentitäten und ihre verwandten Studentenentitäten verwenden.</span><span class="sxs-lookup"><span data-stu-id="35825-181">You'll use separate queries for Enrollment entities and their related Student entities.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="35825-182">Erstellen eines Ansichtsmodells für die Indexansicht „Dozenten“</span><span class="sxs-lookup"><span data-stu-id="35825-182">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="35825-183">Die Dozentenseite zeigt Daten aus drei verschiedenen Tabellen.</span><span class="sxs-lookup"><span data-stu-id="35825-183">The Instructors page shows data from three different tables.</span></span> <span data-ttu-id="35825-184">Aus diesem Grund erstellen Sie ein Ansichtsmodell, das drei Eigenschaften enthält. Jede enthält Daten für eine der Tabellen.</span><span class="sxs-lookup"><span data-stu-id="35825-184">Therefore, you'll create a view model that includes three properties, each holding the data for one of the tables.</span></span>

<span data-ttu-id="35825-185">Erstellen Sie im Ordner *SchoolViewModels* *InstructorIndexData.cs*, und ersetzen Sie den bestehenden Code durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="35825-185">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="create-the-instructor-controller-and-views"></a><span data-ttu-id="35825-186">Erstellen der Dozentencontroller und -ansichten</span><span class="sxs-lookup"><span data-stu-id="35825-186">Create the Instructor controller and views</span></span>

<span data-ttu-id="35825-187">Erstellen Sie einen Dozentencontroller mit EF-Lese-/Schreibaktionen, wie in der folgenden Abbildung gezeigt:</span><span class="sxs-lookup"><span data-stu-id="35825-187">Create an Instructors controller with EF read/write actions as shown in the following illustration:</span></span>

![Hinzufügen von Dozentencontrollern](read-related-data/_static/add-instructors-controller.png)

<span data-ttu-id="35825-189">Öffnen Sie *InstructorsController.cs*. Fügen Sie eine Using-Anweisung für den Namespace ViewModels hinzu:</span><span class="sxs-lookup"><span data-stu-id="35825-189">Open *InstructorsController.cs* and add a using statement for the ViewModels namespace:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Using)]

<span data-ttu-id="35825-190">Ersetzen Sie die Indexmethode durch den folgenden Code, um Eager Loading verwandter Daten durchzuführen und ihn in das Ansichtsmodell einzufügen.</span><span class="sxs-lookup"><span data-stu-id="35825-190">Replace the Index method with the following code to do eager loading of related data and put it in the view model.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EagerLoading)]

<span data-ttu-id="35825-191">Die Methode akzeptiert optionale Routendaten (`id`) und einen Abfragezeichenfolgenparameter (`courseID`), die die ID-Werte des ausgewählten Dozenten und Kurses bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="35825-191">The method accepts optional route data (`id`) and a query string parameter (`courseID`) that provide the ID values of the selected instructor and selected course.</span></span> <span data-ttu-id="35825-192">Die Parameter werden durch die **Auswählen**-Links auf der Seite bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="35825-192">The parameters are provided by the **Select** hyperlinks on the page.</span></span>

<span data-ttu-id="35825-193">Der Code erstellt zuerst eine Instanz des Ansichtsmodells und fügt die Dozentenliste ein.</span><span class="sxs-lookup"><span data-stu-id="35825-193">The code begins by creating an instance of the view model and putting in it the list of instructors.</span></span> <span data-ttu-id="35825-194">Der Code gibt Eager Loading für die `Instructor.OfficeAssignment`- und `Instructor.CourseAssignments`-Navigationseigenschaften an.</span><span class="sxs-lookup"><span data-stu-id="35825-194">The code specifies eager loading for the `Instructor.OfficeAssignment` and the `Instructor.CourseAssignments` navigation properties.</span></span> <span data-ttu-id="35825-195">Innerhalb der `CourseAssignments`-Eigenschaft wird die `Course`-Eigenschaft geladen, und innerhalb dieser werden die `Enrollments`- und `Department`-Eigenschaften geladen, und innerhalb jeder `Enrollment`-Entität wird die `Student`-Eigenschaft geladen.</span><span class="sxs-lookup"><span data-stu-id="35825-195">Within the `CourseAssignments` property, the `Course` property is loaded, and within that, the `Enrollments` and `Department` properties are loaded, and within each `Enrollment` entity the `Student` property is loaded.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude)]

<span data-ttu-id="35825-196">Da die Ansicht immer die OfficeAssignment-Entität erfordert, ist es effizienter, sie in derselben Abfrage abzurufen.</span><span class="sxs-lookup"><span data-stu-id="35825-196">Since the view always requires the OfficeAssignment entity, it's more efficient to fetch that in the same query.</span></span> <span data-ttu-id="35825-197">Kursentitäten sind erforderlich, wenn auf der Webseite ein Dozent ausgewählt ist. Somit ist eine einzelne Abfrage nur besser als mehrere Abfragen, wenn die Seite häufiger mit einem ausgewählten Kurs als ohne angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="35825-197">Course entities are required when an instructor is selected in the web page, so a single query is better than multiple queries only if the page is displayed more often with a course selected than without.</span></span>

<span data-ttu-id="35825-198">Der Code wiederholt `CourseAssignments` und `Course`, da Sie zwei Eigenschaften aus `Course` benötigen.</span><span class="sxs-lookup"><span data-stu-id="35825-198">The code repeats `CourseAssignments` and `Course` because you need two properties from `Course`.</span></span> <span data-ttu-id="35825-199">Die erste Zeichenfolge der `ThenInclude`-Abrufe erhält `CourseAssignment.Course`, `Course.Enrollments` und `Enrollment.Student`.</span><span class="sxs-lookup"><span data-stu-id="35825-199">The first string of `ThenInclude` calls gets `CourseAssignment.Course`, `Course.Enrollments`, and `Enrollment.Student`.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=3-6)]

<span data-ttu-id="35825-200">An diesem Punkt im Code wäre eine andere `ThenInclude` für Navigationseigenschaften von `Student`, die Sie nicht benötigen.</span><span class="sxs-lookup"><span data-stu-id="35825-200">At that point in the code, another `ThenInclude` would be for navigation properties of `Student`, which you don't need.</span></span> <span data-ttu-id="35825-201">Aber ein Aufruf von `Include` beginnt mit `Instructor`-Eigenschaften neu. Daher müssen Sie den Vorgang erneut durchlaufen und `Course.Department` anstelle von `Course.Enrollments` angeben.</span><span class="sxs-lookup"><span data-stu-id="35825-201">But calling `Include` starts over with `Instructor` properties, so you have to go through the chain again, this time specifying `Course.Department` instead of `Course.Enrollments`.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=7-9)]

<span data-ttu-id="35825-202">Der folgende Code wird ausgeführt, wenn ein Dozent ausgewählt wurde.</span><span class="sxs-lookup"><span data-stu-id="35825-202">The following code executes when an instructor was selected.</span></span> <span data-ttu-id="35825-203">Der ausgewählte Dozent wird aus der Liste der Dozenten im Ansichtsmodell abgerufen.</span><span class="sxs-lookup"><span data-stu-id="35825-203">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="35825-204">Die `Courses`-Eigenschaft des Ansichtsmodells wird dann mit den Kursentitäten aus der `CourseAssignments`-Navigationseigenschaft dieses Dozenten geladen.</span><span class="sxs-lookup"><span data-stu-id="35825-204">The view model's `Courses` property is then loaded with the Course entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?range=56-62)]

<span data-ttu-id="35825-205">Die `Where`-Methode gibt eine Sammlung zurück. Aber in diesem Fall resultieren die an diese Methode übergebenen Kriterien nur in einer einzigen zurückgegebenen Instructor-Entität.</span><span class="sxs-lookup"><span data-stu-id="35825-205">The `Where` method returns a collection, but in this case the criteria passed to that method result in only a single Instructor entity being returned.</span></span> <span data-ttu-id="35825-206">Die `Single`-Methode konvertiert die Sammlung in eine einzelne Instructor-Entität, die Ihnen Zugriff auf die `CourseAssignments`-Eigenschaft dieser Entität gibt.</span><span class="sxs-lookup"><span data-stu-id="35825-206">The `Single` method converts the collection into a single Instructor entity, which gives you access to that entity's `CourseAssignments` property.</span></span> <span data-ttu-id="35825-207">Die `CourseAssignments`-Eigenschaft enthält `CourseAssignment`-Entitäten, aus der Sie nur die verwandten `Course`-Entitäten benötigen.</span><span class="sxs-lookup"><span data-stu-id="35825-207">The `CourseAssignments` property contains `CourseAssignment` entities, from which you want only the related `Course` entities.</span></span>

<span data-ttu-id="35825-208">Verwenden Sie die `Single`-Methode für eine Sammlung, wenn Sie wissen, dass die Sammlung nur über ein Element verfügt.</span><span class="sxs-lookup"><span data-stu-id="35825-208">You use the `Single` method on a collection when you know the collection will have only one item.</span></span> <span data-ttu-id="35825-209">Die Single-Methode löst eine Ausnahme aus, wenn die Sammlung, an die übergeben wurde, leer ist, oder wenn mehr als ein Element vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="35825-209">The Single method throws an exception if the collection passed to it's empty or if there's more than one item.</span></span> <span data-ttu-id="35825-210">Eine Alternative ist `SingleOrDefault`, womit ein Standardwert (in diesem Fall null) zurückgegeben wird, wenn die Sammlung leer ist.</span><span class="sxs-lookup"><span data-stu-id="35825-210">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="35825-211">In diesem Fall würde jedoch trotzdem eine Ausnahme ausgelöst werden (da versucht wird, eine `Courses`-Eigenschaft auf einem Null-Verweis zu finden). Die Ausnahmemeldung würde die Ursache des Problems weniger deutlich angeben.</span><span class="sxs-lookup"><span data-stu-id="35825-211">However, in this case that would still result in an exception (from trying to find a `Courses` property on a null reference), and the exception message would less clearly indicate the cause of the problem.</span></span> <span data-ttu-id="35825-212">Wenn Sie die `Single`-Methode aufrufen, können Sie auch in die Where-Bedingung übergehen, anstatt die `Where`-Methode separat aufzurufen:</span><span class="sxs-lookup"><span data-stu-id="35825-212">When you call the `Single` method, you can also pass in the Where condition instead of calling the `Where` method separately:</span></span>

```csharp
.Single(i => i.ID == id.Value)
```

<span data-ttu-id="35825-213">anstelle von:</span><span class="sxs-lookup"><span data-stu-id="35825-213">Instead of:</span></span>

```csharp
.Where(i => i.ID == id.Value).Single()
```

<span data-ttu-id="35825-214">Wenn ein Kurs ausgewählt wurde, wird der ausgewählte Kurs aus der Kursliste im Ansichtsmodell abgerufen.</span><span class="sxs-lookup"><span data-stu-id="35825-214">Next, if a course was selected, the selected course is retrieved from the list of courses in the view model.</span></span> <span data-ttu-id="35825-215">Die `Enrollments`-Eigenschaft des Ansichtsmodells wird dann mit den Registrierungsentitäten aus der `Enrollments`-Navigationseigenschaft dieses Kurses geladen.</span><span class="sxs-lookup"><span data-stu-id="35825-215">Then the view model's `Enrollments` property is loaded with the Enrollment entities from that course's `Enrollments` navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?range=64-69)]

### <a name="modify-the-instructor-index-view"></a><span data-ttu-id="35825-216">Ändern der Dozentenindexansicht</span><span class="sxs-lookup"><span data-stu-id="35825-216">Modify the Instructor Index view</span></span>

<span data-ttu-id="35825-217">Ersetzen Sie in *Views/Instructors/Index.cshtml* den Vorlagencode durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="35825-217">In *Views/Instructors/Index.cshtml*, replace the template code with the following code.</span></span> <span data-ttu-id="35825-218">Die Änderungen werden hervorgehoben.</span><span class="sxs-lookup"><span data-stu-id="35825-218">The changes are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=1-64&highlight=1,3-7,15-19,24,26-31,41-54,56)]

<span data-ttu-id="35825-219">Sie haben die folgenden Änderungen am bestehenden Code vorgenommen:</span><span class="sxs-lookup"><span data-stu-id="35825-219">You've made the following changes to the existing code:</span></span>

* <span data-ttu-id="35825-220">Die Modellklasse wurde zu `InstructorIndexData` geändert.</span><span class="sxs-lookup"><span data-stu-id="35825-220">Changed the model class to `InstructorIndexData`.</span></span>

* <span data-ttu-id="35825-221">Der Seitenname wurde von **Index** in **Dozenten** geändert.</span><span class="sxs-lookup"><span data-stu-id="35825-221">Changed the page title from **Index** to **Instructors**.</span></span>

* <span data-ttu-id="35825-222">Es wurde eine **Office**-Spalte hinzugefügt, die `item.OfficeAssignment.Location` nur anzeigt, wenn `item.OfficeAssignment` nicht gleich null ist.</span><span class="sxs-lookup"><span data-stu-id="35825-222">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` isn't null.</span></span> <span data-ttu-id="35825-223">(Da dies eine 1:0..1-Beziehung ist, gibt es möglicherweise keine verwandte OfficeAssignment-Entität.)</span><span class="sxs-lookup"><span data-stu-id="35825-223">(Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.)</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="35825-224">Es wurde eine **Kurse**-Spalte hinzugefügt, die die Kurse eines jeden Dozenten anzeigt.</span><span class="sxs-lookup"><span data-stu-id="35825-224">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="35825-225">Weitere Informationen über diese Razor-Syntax finden Sie unter [Explizite Zeilenübergänge mit `@:`](xref:mvc/views/razor#explicit-line-transition-with-).</span><span class="sxs-lookup"><span data-stu-id="35825-225">See [Explicit Line Transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="35825-226">Es wurde Code hinzugefügt, der `class="success"` dynamisch zum `tr`-Element des ausgewählten Dozenten hinzufügt.</span><span class="sxs-lookup"><span data-stu-id="35825-226">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="35825-227">Hiermit wird mit einer Bootstrapklasse eine Hintergrundfarbe für die ausgewählte Zeile hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="35825-227">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.ID == (int?)ViewData["InstructorID"])
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="35825-228">Es wurde ein neuer Link mit der Bezeichnung **Auswählen** unmittelbar vor den anderen Links in jeder Zeile hinzugefügt. Dies führt dazu, dass die ID des ausgewählten Dozenten an die `Index`-Methode gesendet wird.</span><span class="sxs-lookup"><span data-stu-id="35825-228">Added a new hyperlink labeled **Select** immediately before the other links in each row, which causes the selected instructor's ID to be sent to the `Index` method.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="35825-229">Führen Sie die Anwendung aus. Klicken Sie auf die Registerkarte **Dozenten**. Die Seite zeigt die Eigenschaft für den Standort verwandter OfficeAssignment-Entitäten und eine leere Zelle an, wenn keine verwandte OfficeAssignment-Entität vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="35825-229">Run the app and select the **Instructors** tab. The page displays the Location property of related OfficeAssignment entities and an empty table cell when there's no related OfficeAssignment entity.</span></span>

![Indexseite „Dozenten“, nichts ausgewählt](read-related-data/_static/instructors-index-no-selection.png)

<span data-ttu-id="35825-231">Fügen Sie in der Datei *Views/Instructors/Index.cshtml* nach dem Schließen des Tabellenelements (am Ende der Datei) den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="35825-231">In the *Views/Instructors/Index.cshtml* file, after the closing table element (at the end of the file), add the following code.</span></span> <span data-ttu-id="35825-232">Dieser Code zeigt eine Liste der Kurse an, die im Zusammenhang mit einem Dozenten stehen, wenn ein Dozent ausgewählt wird.</span><span class="sxs-lookup"><span data-stu-id="35825-232">This code displays a list of courses related to an instructor when an instructor is selected.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=66-101)]

<span data-ttu-id="35825-233">Dieser Code liest die `Courses`-Eigenschaft des Ansichtsmodells, um eine Kursliste anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="35825-233">This code reads the `Courses` property of the view model to display a list of courses.</span></span> <span data-ttu-id="35825-234">Er bietet außerdem einen **Auswählen**-Link, der die ID des ausgewählten Kurses an die `Index`-Aktionsmethode sendet.</span><span class="sxs-lookup"><span data-stu-id="35825-234">It also provides a **Select** hyperlink that sends the ID of the selected course to the `Index` action method.</span></span>

<span data-ttu-id="35825-235">Aktualisieren Sie die Seite. Wählen Sie einen Dozenten aus.</span><span class="sxs-lookup"><span data-stu-id="35825-235">Refresh the page and select an instructor.</span></span> <span data-ttu-id="35825-236">Jetzt sehen Sie ein Raster, das die dem Dozenten zugewiesenen Kurse anzeigt. Sie sehen auch den Namen der zugewiesenen Abteilung für jeden Kurs.</span><span class="sxs-lookup"><span data-stu-id="35825-236">Now you see a grid that displays courses assigned to the selected instructor, and for each course you see the name of the assigned department.</span></span>

![Indexseite „Dozenten“, Dozent ausgewählt](read-related-data/_static/instructors-index-instructor-selected.png)

<span data-ttu-id="35825-238">Fügen Sie den folgenden Code hinzu, nachdem Sie den Codeblock hinzugefügt haben.</span><span class="sxs-lookup"><span data-stu-id="35825-238">After the code block you just added, add the following code.</span></span> <span data-ttu-id="35825-239">Dies zeigt eine Liste der Studenten an, die im Kurs registriert sind, wenn dieser Kurs ausgewählt ist.</span><span class="sxs-lookup"><span data-stu-id="35825-239">This displays a list of the students who are enrolled in a course when that course is selected.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=103-125)]

<span data-ttu-id="35825-240">Dieser Code liest die Registrierungseigenschaft des Ansichtsmodells, um eine Liste der in diesem Kurs registrierten Studenten anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="35825-240">This code reads the Enrollments property of the view model in order to display a list of students enrolled in the course.</span></span>

<span data-ttu-id="35825-241">Aktualisieren Sie die Seite erneut. Wählen Sie einen Dozenten aus.</span><span class="sxs-lookup"><span data-stu-id="35825-241">Refresh the page again and select an instructor.</span></span> <span data-ttu-id="35825-242">Wählen Sie dann einen Kurs aus, um die Liste der registrierten Studenten und deren Noten einzusehen.</span><span class="sxs-lookup"><span data-stu-id="35825-242">Then select a course to see the list of enrolled students and their grades.</span></span>

![Indexseite „Dozenten“, Dozent und Kurs ausgewählt](read-related-data/_static/instructors-index.png)

## <a name="explicit-loading"></a><span data-ttu-id="35825-244">Explizites Laden</span><span class="sxs-lookup"><span data-stu-id="35825-244">Explicit loading</span></span>

<span data-ttu-id="35825-245">Wenn Sie die Liste der Dozenten aus *InstructorsController.cs* abrufen, haben Sie Eager Loading für die `CourseAssignments`-Navigationseigenschaft angegeben.</span><span class="sxs-lookup"><span data-stu-id="35825-245">When you retrieved the list of instructors in *InstructorsController.cs*, you specified eager loading for the `CourseAssignments` navigation property.</span></span>

<span data-ttu-id="35825-246">Angenommen, Sie haben erwartet, dass Benutzer nur selten Registrierungen für einen ausgewählten Dozenten und Kurs angezeigt haben möchten.</span><span class="sxs-lookup"><span data-stu-id="35825-246">Suppose you expected users to only rarely want to see enrollments in a selected instructor and course.</span></span> <span data-ttu-id="35825-247">In diesem Fall möchten Sie die Registrierungsdaten möglicherweise nur laden, wenn diese angefordert werden.</span><span class="sxs-lookup"><span data-stu-id="35825-247">In that case, you might want to load the enrollment data only if it's requested.</span></span> <span data-ttu-id="35825-248">Ersetzen Sie die `Index`-Methode durch folgenden Code, um ein Beispiel für Eager Loading zu sehen. Dieser Code entfernt das Eager Loading für Registrierungen und lädt diese Eigenschaft explizit.</span><span class="sxs-lookup"><span data-stu-id="35825-248">To see an example of how to do explicit loading, replace the `Index` method with the following code, which removes eager loading for Enrollments and loads that property explicitly.</span></span> <span data-ttu-id="35825-249">Die Codeänderungen werden hervorgehoben.</span><span class="sxs-lookup"><span data-stu-id="35825-249">The code changes are highlighted.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ExplicitLoading&highlight=23-29)]

<span data-ttu-id="35825-250">Der neue Code löscht die *ThenInclude*-Methodenaufrufe für Registrierungsdaten aus dem Code, der Instructor-Entitäten abruft.</span><span class="sxs-lookup"><span data-stu-id="35825-250">The new code drops the *ThenInclude* method calls for enrollment data from the code that retrieves instructor entities.</span></span> <span data-ttu-id="35825-251">Wenn ein Dozent und Kurs ausgewählt werden, ruft der hervorgehobene Code Registrierungsentitäten für den ausgewählten Kurs und Studentenentitäten für jede Registrierung ab.</span><span class="sxs-lookup"><span data-stu-id="35825-251">If an instructor and course are selected, the highlighted code retrieves Enrollment entities for the selected course, and Student entities for each Enrollment.</span></span>

<span data-ttu-id="35825-252">Führen Sie die Anwendung aus, navigieren Sie zur Dozentenindexseite, und Sie werden keinen Unterschied in der Anzeige auf der Seite bemerken, obwohl Sie die Abrufart für Daten geändert haben.</span><span class="sxs-lookup"><span data-stu-id="35825-252">Run the app, go to the Instructors Index page now and you'll see no difference in what's displayed on the page, although you've changed how the data is retrieved.</span></span>

## <a name="summary"></a><span data-ttu-id="35825-253">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="35825-253">Summary</span></span>

<span data-ttu-id="35825-254">Sie haben Eager Loading jetzt mit einer und mehreren Abfragen verwendet, um verwandte Daten in die Navigationseigenschaften zu lesen.</span><span class="sxs-lookup"><span data-stu-id="35825-254">You've now used eager loading with one query and with multiple queries to read related data into navigation properties.</span></span> <span data-ttu-id="35825-255">Das nächste Tutorial zeigt die Aktualisierung verwandter Daten.</span><span class="sxs-lookup"><span data-stu-id="35825-255">In the next tutorial you'll learn how to update related data.</span></span>

::: moniker-end

>[!div class="step-by-step"]
><span data-ttu-id="35825-256">[Zurück](complex-data-model.md)
>[Weiter](update-related-data.md)</span><span class="sxs-lookup"><span data-stu-id="35825-256">[Previous](complex-data-model.md)
[Next](update-related-data.md)</span></span>
