---
title: "Hinzufügen eines neuen Felds zu einer Razor-Seite"
author: rick-anderson
description: "Veranschaulicht das Hinzufügen ein neues Felds zu einer Razor-Seite mit Entity Framework Core"
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: 36412e9d1f3143f0d1999d0e754e6627f0984ad5
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
---
# <a name="adding-a-new-field-to-a-razor-page"></a><span data-ttu-id="79072-103">Hinzufügen eines neuen Felds zu einer Razor-Seite</span><span class="sxs-lookup"><span data-stu-id="79072-103">Adding a new field to a Razor Page</span></span>

<span data-ttu-id="79072-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="79072-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="79072-105">In diesem Abschnitt verwenden Sie [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First-Migrationen zum Hinzufügen eines neuen Felds zum Modell und zum Migrieren dieser Änderung in die Datenbank.</span><span class="sxs-lookup"><span data-stu-id="79072-105">In this section you'll use [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First Migrations to add a new field to the model and migrate that change to the database.</span></span>

<span data-ttu-id="79072-106">Wenn Sie EF Code First verwenden, um eine Datenbank automatisch zu erstellen, fügt Code First der Datenbank eine Tabelle hinzu, um nachzuverfolgen, ob das Schema der Datenbank mit den Modellklassen synchron ist, aus denen sie generiert wurde.</span><span class="sxs-lookup"><span data-stu-id="79072-106">When you use EF Code First to automatically create a database, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="79072-107">Wenn sie nicht synchron sind, löst EF eine Ausnahme aus.</span><span class="sxs-lookup"><span data-stu-id="79072-107">If they aren't in sync, EF throws an exception.</span></span> <span data-ttu-id="79072-108">Dies erleichtert die Suche nach Problemen mit inkonsistenten Datenbanken bzw. inkonsistentem Code.</span><span class="sxs-lookup"><span data-stu-id="79072-108">This makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="79072-109">Hinzufügen einer Rating-Eigenschaft zum Movie-Modell</span><span class="sxs-lookup"><span data-stu-id="79072-109">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="79072-110">Öffnen Sie die Datei *Models/Movie.cs*, und fügen Sie eine `Rating`-Eigenschaft hinzu:</span><span class="sxs-lookup"><span data-stu-id="79072-110">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

<span data-ttu-id="79072-111">Erstellen Sie die App (STRG+UMSCHALT+B).</span><span class="sxs-lookup"><span data-stu-id="79072-111">Build the app (Ctrl+Shift+B).</span></span>

<span data-ttu-id="79072-112">Bearbeiten Sie *Pages/Movies/Index.cshtml*, und fügen ein `Rating`-Feld hinzu:</span><span class="sxs-lookup"><span data-stu-id="79072-112">Edit *Pages/Movies/Index.cshtml*, and add a `Rating` field:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=40-42,61-63)]

<span data-ttu-id="79072-113">Fügen Sie das `Rating`-Feld zu den Seiten „Delete“ und „Details“ hinzu.</span><span class="sxs-lookup"><span data-stu-id="79072-113">Add the `Rating` field to the Delete and Details pages.</span></span>

<span data-ttu-id="79072-114">Aktualisieren Sie die Datei *Create.cshtml* mit einem `Rating`-Feld.</span><span class="sxs-lookup"><span data-stu-id="79072-114">Update *Create.cshtml* with a `Rating` field.</span></span> <span data-ttu-id="79072-115">Sie können das vorherige `<div>`-Element kopieren und einfügen, und IntelliSense kann Ihnen beim Aktualisieren der Felder helfen.</span><span class="sxs-lookup"><span data-stu-id="79072-115">You can copy/paste the previous `<div>` element and let intelliSense help you update the fields.</span></span> <span data-ttu-id="79072-116">IntelliSense nutzt [Taghilfsprogramme](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="79072-116">IntelliSense works with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

![Der Entwickler hat den Buchstaben R als Attributwert von „asp-for“ im zweiten „label“-Element der Ansicht eingegeben.](new-field/_static/cr.png)

<span data-ttu-id="79072-120">Der folgende Code zeigt *Create.cshtml* mit einem `Rating`-Feld:</span><span class="sxs-lookup"><span data-stu-id="79072-120">The following code shows *Create.cshtml* with a `Rating` field:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?highlight=36-40)]

<span data-ttu-id="79072-121">Fügen Sie das Feld `Rating` der Bearbeitungsseite hinzu.</span><span class="sxs-lookup"><span data-stu-id="79072-121">Add the `Rating` field to the Edit Page.</span></span>

<span data-ttu-id="79072-122">Die App funktioniert erst, nachdem die Datenbank mit dem neuen Feld aktualisiert wurde.</span><span class="sxs-lookup"><span data-stu-id="79072-122">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="79072-123">Wenn sie jetzt ausgeführt wird, löst die App eine `SqlException` aus:</span><span class="sxs-lookup"><span data-stu-id="79072-123">If run now, the app throws a `SqlException`:</span></span>

```
SqlException: Invalid column name 'Rating'.
```

<span data-ttu-id="79072-124">Dieser Fehler wird dadurch verursacht, dass sich die aktualisierte Modellklasse „Movie“ vom Schema der Tabelle „Movie“ der Datenbank unterscheidet.</span><span class="sxs-lookup"><span data-stu-id="79072-124">This error is caused by the updated Movie model class being different than the schema of the Movie table of the database.</span></span> <span data-ttu-id="79072-125">(Die Datenbanktabelle enthält nicht die Spalte `Rating`.)</span><span class="sxs-lookup"><span data-stu-id="79072-125">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="79072-126">Es gibt mehrere Vorgehensweisen zum Beheben des Fehlers:</span><span class="sxs-lookup"><span data-stu-id="79072-126">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="79072-127">Lassen Sie die Datenbank von Entity Framework automatisch löschen und mit dem neuen Modellklassenschema neu erstellen.</span><span class="sxs-lookup"><span data-stu-id="79072-127">Have the Entity Framework automatically drop and re-create the database using  the new model class schema.</span></span> <span data-ttu-id="79072-128">Dieser Ansatz ist früh im Entwicklungszyklus sehr praktisch. Er ermöglicht Ihnen, das Modell und das Datenbankschema schnell gemeinsam weiterzuentwickeln.</span><span class="sxs-lookup"><span data-stu-id="79072-128">This approach is convenient early in the development cycle; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="79072-129">Der Nachteil ist, dass in der Datenbank vorhandene Daten verloren gehen.</span><span class="sxs-lookup"><span data-stu-id="79072-129">The downside is that you lose existing data in the database.</span></span> <span data-ttu-id="79072-130">Sie sollten diesen Ansatz nicht in einer Produktionsdatenbank verwenden!</span><span class="sxs-lookup"><span data-stu-id="79072-130">You don't want to use this approach on a production database!</span></span> <span data-ttu-id="79072-131">Das Löschen der Datenbank bei Schemaänderungen und das Verwenden eines Initialisierers zum automatischen Ausführen eines Seedings für eine Datenbank mit Testdaten ist häufig eine produktive Möglichkeit zum Entwickeln einer App.</span><span class="sxs-lookup"><span data-stu-id="79072-131">Dropping the DB on schema changes and using an initializer to automatically seed the database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="79072-132">Ändern Sie das Schema der vorhandenen Datenbank explizit so, dass es mit den Modellklassen übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="79072-132">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="79072-133">Der Vorteil dieses Ansatzes ist, dass Sie Ihre Daten behalten.</span><span class="sxs-lookup"><span data-stu-id="79072-133">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="79072-134">Sie können diese Änderung entweder manuell oder durch Erstellen eines Änderungsskripts für die Datenbank vornehmen.</span><span class="sxs-lookup"><span data-stu-id="79072-134">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="79072-135">Verwenden Sie Code First-Migrationen, um das Datenbankschema zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="79072-135">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="79072-136">Verwenden Sie für dieses Tutorial Code First-Migrationen.</span><span class="sxs-lookup"><span data-stu-id="79072-136">For this tutorial, use Code First Migrations.</span></span>

<span data-ttu-id="79072-137">Aktualisieren Sie die `SeedData`-Klasse so, dass sie einen Wert für die neue Spalte bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="79072-137">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="79072-138">Eine Beispieländerung ist nachstehend gezeigt, aber Sie sollten diese Änderung für jeden `new Movie`-Block vornehmen.</span><span class="sxs-lookup"><span data-stu-id="79072-138">A sample change is shown below, but you'll want to make this change for each `new Movie` block.</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

<span data-ttu-id="79072-139">Sehen Sie sich die [fertige SeedData.cs-Datei an](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).</span><span class="sxs-lookup"><span data-stu-id="79072-139">See the [completed SeedData.cs file](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).</span></span>

<span data-ttu-id="79072-140">Erstellen Sie die Projektmappe.</span><span class="sxs-lookup"><span data-stu-id="79072-140">Build the solution.</span></span>

<a name="pmc"></a> <span data-ttu-id="79072-141">Wählen Sie im Menü **Tools** **NuGet-Paket-Manager > Paket-Manager-Konsole** aus.</span><span class="sxs-lookup"><span data-stu-id="79072-141">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>
<span data-ttu-id="79072-142">Geben Sie in der PMC die folgenden Befehle ein:</span><span class="sxs-lookup"><span data-stu-id="79072-142">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="79072-143">Der `Add-Migration`-Befehl weist das Framework an, Folgendes auszuführen:</span><span class="sxs-lookup"><span data-stu-id="79072-143">The `Add-Migration` command tells the framework to:</span></span>

* <span data-ttu-id="79072-144">Das `Movie`-Modell mit dem Schema der `Movie`-Datenbank vergleichen.</span><span class="sxs-lookup"><span data-stu-id="79072-144">Compare the `Movie` model with the `Movie` DB schema.</span></span>
* <span data-ttu-id="79072-145">Code erstellen, um das Schema der Datenbank in das neue Modell zu migrieren.</span><span class="sxs-lookup"><span data-stu-id="79072-145">Create code to migrate the DB schema to the new model.</span></span>

<span data-ttu-id="79072-146">Der Name „Rating“ ist beliebig und wird verwendet, um die Migrationsdatei zu benennen.</span><span class="sxs-lookup"><span data-stu-id="79072-146">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="79072-147">Es ist hilfreich, einen aussagekräftigen Namen für die Migrationsdatei zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="79072-147">It's helpful to use a meaningful name for the migration file.</span></span>

<a name="ssox"></a> <span data-ttu-id="79072-148">Wenn Sie alle Datensätze aus der Datenbank löschen, führt der Initialisierer ein Seeding für die Datenbank aus und fügt das Feld `Rating` hinzu.</span><span class="sxs-lookup"><span data-stu-id="79072-148">If you delete all the records in the DB, the initializer will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="79072-149">Dies ist über die Links „Löschen“ im Browser oder [SQL Server-Objekt-Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX) möglich.</span><span class="sxs-lookup"><span data-stu-id="79072-149">You can do this with the delete links in the browser or from [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span></span> <span data-ttu-id="79072-150">So löschen Sie die Datenbank aus SSOX</span><span class="sxs-lookup"><span data-stu-id="79072-150">To delete the database from SSOX:</span></span>

* <span data-ttu-id="79072-151">Wählen Sie die Datenbank in SSOX aus.</span><span class="sxs-lookup"><span data-stu-id="79072-151">Select the database in SSOX.</span></span>
* <span data-ttu-id="79072-152">Klicken Sie mit der rechten Maustaste auf die Datenbank, und wählen Sie *Löschen* aus.</span><span class="sxs-lookup"><span data-stu-id="79072-152">Right click on the database, and select *Delete*.</span></span>
* <span data-ttu-id="79072-153">Aktivieren Sie **Vorhandene Verbindungen schließen**.</span><span class="sxs-lookup"><span data-stu-id="79072-153">Check **Close existing connections**.</span></span>
* <span data-ttu-id="79072-154">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="79072-154">Select **OK**.</span></span>
* <span data-ttu-id="79072-155">Aktualisieren Sie die Datenbank in [PMC](xref:tutorials/razor-pages/new-field#pmc):</span><span class="sxs-lookup"><span data-stu-id="79072-155">In the [PMC](xref:tutorials/razor-pages/new-field#pmc), update the database:</span></span>

  ```powershell
  Update-Database
  ```

<span data-ttu-id="79072-156">Führen Sie die App aus, und überprüfen Sie, ob Sie Filme mit dem Feld `Rating` erstellen/bearbeiten/anzeigen können.</span><span class="sxs-lookup"><span data-stu-id="79072-156">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="79072-157">Wenn für die Datenbank kein Seed ausgeführt wird, beenden Sie IIS Express, und führen Sie dann die App aus.</span><span class="sxs-lookup"><span data-stu-id="79072-157">If the database isn't seeded, stop IIS Express, and then run the app.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="79072-158">[Zurück: Hinzufügen der Suche](xref:tutorials/razor-pages/search)
[Weiter: Hinzufügen der Validierung](xref:tutorials/razor-pages/validation)</span><span class="sxs-lookup"><span data-stu-id="79072-158">[Previous: Adding Search](xref:tutorials/razor-pages/search)
[Next: Adding Validation](xref:tutorials/razor-pages/validation)</span></span>
