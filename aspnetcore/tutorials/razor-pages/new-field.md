---
title: Hinzufügen eines neuen Felds zu einer Razor-Seite in ASP.NET Core
author: rick-anderson
description: Veranschaulicht das Hinzufügen ein neues Felds zu einer Razor Page mit Entity Framework Core
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: d6d59ff336095e2f1b8b2e9a0338b7791605ad7a
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2018
ms.locfileid: "46010896"
---
# <a name="add-a-new-field-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="53126-103">Hinzufügen eines neuen Felds zu einer Razor-Seite in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="53126-103">Add a new field to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="53126-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="53126-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="53126-105">In diesem Abschnitt verwenden Sie [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First-Migrationen zum Hinzufügen eines neuen Felds zum Modell und zum Migrieren dieser Änderung zur Datenbank.</span><span class="sxs-lookup"><span data-stu-id="53126-105">In this section you use [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First Migrations to add a new field to the model and migrate that change to the database.</span></span>

<span data-ttu-id="53126-106">Bei der Verwendung von EF Code First für die automatische Erstellung einer Datenbank geht Code First wie folgt vor:</span><span class="sxs-lookup"><span data-stu-id="53126-106">When using EF Code First to automatically create a database, Code First:</span></span>

* <span data-ttu-id="53126-107">Es fügt eine Tabelle zur Datenbank hinzu, um nachzuverfolgen, ob das Schema der Datenbank mit den Modellklassen synchron ist, aus denen sie generiert wurde.</span><span class="sxs-lookup"><span data-stu-id="53126-107">Adds a table to the database to track whether the schema of the database is in sync with the model classes it was generated from.</span></span>
* <span data-ttu-id="53126-108">Wenn die Datenbank mit den Modellklassen nicht synchron ist, löst EF eine Ausnahme aus.</span><span class="sxs-lookup"><span data-stu-id="53126-108">If the model classes aren't in sync with the DB, EF throws an exception.</span></span> 

<span data-ttu-id="53126-109">Durch die automatische Überprüfung, ob das Schema und das Modell synchron sind, können Probleme inkonsistenter Datenbanken/Codes leichter ermittelt werden.</span><span class="sxs-lookup"><span data-stu-id="53126-109">Automatic verification of schema/model in sync makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="53126-110">Hinzufügen einer Rating-Eigenschaft zum Movie-Modell</span><span class="sxs-lookup"><span data-stu-id="53126-110">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="53126-111">Öffnen Sie die Datei *Models/Movie.cs*, und fügen Sie eine `Rating`-Eigenschaft hinzu:</span><span class="sxs-lookup"><span data-stu-id="53126-111">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Models/MovieDateRating.cs?highlight=13&name=snippet)]

::: moniker-end

<span data-ttu-id="53126-112">Erstellen Sie die App (STRG+UMSCHALT+B).</span><span class="sxs-lookup"><span data-stu-id="53126-112">Build the app (Ctrl+Shift+B).</span></span>

<span data-ttu-id="53126-113">Bearbeiten Sie *Pages/Movies/Index.cshtml*, und fügen ein `Rating`-Feld hinzu:</span><span class="sxs-lookup"><span data-stu-id="53126-113">Edit *Pages/Movies/Index.cshtml*, and add a `Rating` field:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=40-42,61-63)]

<span data-ttu-id="53126-114">Fügen Sie das `Rating`-Feld zu den Seiten „Delete“ und „Details“ hinzu.</span><span class="sxs-lookup"><span data-stu-id="53126-114">Add the `Rating` field to the Delete and Details pages.</span></span>

<span data-ttu-id="53126-115">Aktualisieren Sie die Datei *Create.cshtml* mit einem `Rating`-Feld.</span><span class="sxs-lookup"><span data-stu-id="53126-115">Update *Create.cshtml* with a `Rating` field.</span></span> <span data-ttu-id="53126-116">Sie können das vorherige `<div>`-Element kopieren und einfügen, und IntelliSense kann Ihnen beim Aktualisieren der Felder helfen.</span><span class="sxs-lookup"><span data-stu-id="53126-116">You can copy/paste the previous `<div>` element and let intelliSense help you update the fields.</span></span> <span data-ttu-id="53126-117">IntelliSense nutzt [Taghilfsprogramme](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="53126-117">IntelliSense works with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

![Der Entwickler hat den Buchstaben R als Attributwert von „asp-for“ im zweiten „label“-Element der Ansicht eingegeben.](new-field/_static/cr.png)

<span data-ttu-id="53126-121">Der folgende Code zeigt *Create.cshtml* mit einem `Rating`-Feld:</span><span class="sxs-lookup"><span data-stu-id="53126-121">The following code shows *Create.cshtml* with a `Rating` field:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?highlight=36-40)]

<span data-ttu-id="53126-122">Fügen Sie das Feld `Rating` der Bearbeitungsseite hinzu.</span><span class="sxs-lookup"><span data-stu-id="53126-122">Add the `Rating` field to the Edit Page.</span></span>

<span data-ttu-id="53126-123">Die App funktioniert erst, nachdem die Datenbank mit dem neuen Feld aktualisiert wurde.</span><span class="sxs-lookup"><span data-stu-id="53126-123">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="53126-124">Wenn sie jetzt ausgeführt wird, löst die App eine `SqlException` aus:</span><span class="sxs-lookup"><span data-stu-id="53126-124">If run now, the app throws a `SqlException`:</span></span>

```
SqlException: Invalid column name 'Rating'.
```

<span data-ttu-id="53126-125">Dieser Fehler wird dadurch verursacht, dass sich die aktualisierte Modellklasse „Movie“ vom Schema der Tabelle „Movie“ der Datenbank unterscheidet.</span><span class="sxs-lookup"><span data-stu-id="53126-125">This error is caused by the updated Movie model class being different than the schema of the Movie table of the database.</span></span> <span data-ttu-id="53126-126">(Die Datenbanktabelle enthält nicht die Spalte `Rating`.)</span><span class="sxs-lookup"><span data-stu-id="53126-126">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="53126-127">Es gibt mehrere Vorgehensweisen zum Beheben des Fehlers:</span><span class="sxs-lookup"><span data-stu-id="53126-127">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="53126-128">Lassen Sie die Datenbank von Entity Framework automatisch löschen und mit dem neuen Modellklassenschema neu erstellen.</span><span class="sxs-lookup"><span data-stu-id="53126-128">Have the Entity Framework automatically drop and re-create the database using  the new model class schema.</span></span> <span data-ttu-id="53126-129">Dieser Ansatz ist früh im Entwicklungszyklus sehr praktisch. Er ermöglicht Ihnen, das Modell und das Datenbankschema schnell gemeinsam weiterzuentwickeln.</span><span class="sxs-lookup"><span data-stu-id="53126-129">This approach is convenient early in the development cycle; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="53126-130">Der Nachteil ist, dass in der Datenbank vorhandene Daten verloren gehen.</span><span class="sxs-lookup"><span data-stu-id="53126-130">The downside is that you lose existing data in the database.</span></span> <span data-ttu-id="53126-131">Sie sollten diesen Ansatz nicht in einer Produktionsdatenbank verwenden!</span><span class="sxs-lookup"><span data-stu-id="53126-131">You don't want to use this approach on a production database!</span></span> <span data-ttu-id="53126-132">Das Löschen der Datenbank bei Schemaänderungen und das Verwenden eines Initialisierers zum automatischen Ausführen eines Seedings für eine Datenbank mit Testdaten ist häufig eine produktive Möglichkeit zum Entwickeln einer App.</span><span class="sxs-lookup"><span data-stu-id="53126-132">Dropping the DB on schema changes and using an initializer to automatically seed the database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="53126-133">Ändern Sie das Schema der vorhandenen Datenbank explizit so, dass es mit den Modellklassen übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="53126-133">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="53126-134">Der Vorteil dieses Ansatzes ist, dass Sie Ihre Daten behalten.</span><span class="sxs-lookup"><span data-stu-id="53126-134">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="53126-135">Sie können diese Änderung entweder manuell oder durch Erstellen eines Änderungsskripts für die Datenbank vornehmen.</span><span class="sxs-lookup"><span data-stu-id="53126-135">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="53126-136">Verwenden Sie Code First-Migrationen, um das Datenbankschema zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="53126-136">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="53126-137">Verwenden Sie für dieses Tutorial Code First-Migrationen.</span><span class="sxs-lookup"><span data-stu-id="53126-137">For this tutorial, use Code First Migrations.</span></span>

<span data-ttu-id="53126-138">Aktualisieren Sie die `SeedData`-Klasse so, dass sie einen Wert für die neue Spalte bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="53126-138">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="53126-139">Eine Beispieländerung ist nachstehend gezeigt, aber Sie sollten diese Änderung für jeden `new Movie`-Block vornehmen.</span><span class="sxs-lookup"><span data-stu-id="53126-139">A sample change is shown below, but you'll want to make this change for each `new Movie` block.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="53126-140">Sehen Sie sich die [fertige SeedData.cs-Datei an](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).</span><span class="sxs-lookup"><span data-stu-id="53126-140">See the [completed SeedData.cs file](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="53126-141">Sehen Sie sich die [fertige SeedData.cs-Datei an](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/SeedDataRating.cs).</span><span class="sxs-lookup"><span data-stu-id="53126-141">See the [completed SeedData.cs file](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/SeedDataRating.cs).</span></span>

::: moniker-end

<span data-ttu-id="53126-142">Erstellen Sie die Projektmappe.</span><span class="sxs-lookup"><span data-stu-id="53126-142">Build the solution.</span></span>

<a name="pmc"></a> <span data-ttu-id="53126-143">Wählen Sie im Menü **Tools** **NuGet-Paket-Manager > Paket-Manager-Konsole** aus.</span><span class="sxs-lookup"><span data-stu-id="53126-143">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>
<span data-ttu-id="53126-144">Geben Sie in der PMC die folgenden Befehle ein:</span><span class="sxs-lookup"><span data-stu-id="53126-144">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="53126-145">Der `Add-Migration`-Befehl weist das Framework an, Folgendes auszuführen:</span><span class="sxs-lookup"><span data-stu-id="53126-145">The `Add-Migration` command tells the framework to:</span></span>

* <span data-ttu-id="53126-146">Das `Movie`-Modell mit dem Schema der `Movie`-Datenbank vergleichen.</span><span class="sxs-lookup"><span data-stu-id="53126-146">Compare the `Movie` model with the `Movie` DB schema.</span></span>
* <span data-ttu-id="53126-147">Code erstellen, um das Schema der Datenbank in das neue Modell zu migrieren.</span><span class="sxs-lookup"><span data-stu-id="53126-147">Create code to migrate the DB schema to the new model.</span></span>

<span data-ttu-id="53126-148">Der Name „Rating“ ist beliebig und wird verwendet, um die Migrationsdatei zu benennen.</span><span class="sxs-lookup"><span data-stu-id="53126-148">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="53126-149">Es ist hilfreich, einen aussagekräftigen Namen für die Migrationsdatei zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="53126-149">It's helpful to use a meaningful name for the migration file.</span></span>

<a name="ssox"></a> <span data-ttu-id="53126-150">Wenn Sie alle Datensätze aus der Datenbank löschen, führt der Initialisierer ein Seeding für die Datenbank aus und fügt das Feld `Rating` hinzu.</span><span class="sxs-lookup"><span data-stu-id="53126-150">If you delete all the records in the DB, the initializer will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="53126-151">Dies ist über die Links „Löschen“ im Browser oder [SQL Server-Objekt-Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX) möglich.</span><span class="sxs-lookup"><span data-stu-id="53126-151">You can do this with the delete links in the browser or from [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span></span> <span data-ttu-id="53126-152">So löschen Sie die Datenbank aus SSOX</span><span class="sxs-lookup"><span data-stu-id="53126-152">To delete the database from SSOX:</span></span>

* <span data-ttu-id="53126-153">Wählen Sie die Datenbank in SSOX aus.</span><span class="sxs-lookup"><span data-stu-id="53126-153">Select the database in SSOX.</span></span>
* <span data-ttu-id="53126-154">Klicken Sie mit der rechten Maustaste auf die Datenbank, und wählen Sie *Löschen* aus.</span><span class="sxs-lookup"><span data-stu-id="53126-154">Right click on the database, and select *Delete*.</span></span>
* <span data-ttu-id="53126-155">Aktivieren Sie **Vorhandene Verbindungen schließen**.</span><span class="sxs-lookup"><span data-stu-id="53126-155">Check **Close existing connections**.</span></span>
* <span data-ttu-id="53126-156">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="53126-156">Select **OK**.</span></span>
* <span data-ttu-id="53126-157">Aktualisieren Sie die Datenbank in [PMC](xref:tutorials/razor-pages/new-field#pmc):</span><span class="sxs-lookup"><span data-stu-id="53126-157">In the [PMC](xref:tutorials/razor-pages/new-field#pmc), update the database:</span></span>

  ```powershell
  Update-Database
  ```

<span data-ttu-id="53126-158">Führen Sie die App aus, und überprüfen Sie, ob Sie Filme mit dem Feld `Rating` erstellen/bearbeiten/anzeigen können.</span><span class="sxs-lookup"><span data-stu-id="53126-158">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="53126-159">Wenn für die Datenbank kein Seed ausgeführt wird, beenden Sie IIS Express, und führen Sie dann die App aus.</span><span class="sxs-lookup"><span data-stu-id="53126-159">If the database isn't seeded, stop IIS Express, and then run the app.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="53126-160">[Zurück: Hinzufügen der Suche](xref:tutorials/razor-pages/search)
> [Weiter: Hinzufügen der Validierung](xref:tutorials/razor-pages/validation)</span><span class="sxs-lookup"><span data-stu-id="53126-160">[Previous: Adding Search](xref:tutorials/razor-pages/search)
[Next: Adding Validation](xref:tutorials/razor-pages/validation)</span></span>
