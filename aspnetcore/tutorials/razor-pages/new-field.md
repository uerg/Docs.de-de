---
title: "Hinzufügen eines neuen Felds zu einer Razor-Seite"
author: rick-anderson
description: "Veranschaulicht das Hinzufügen ein neues Felds zu einer Razor-Seite mit Entity Framework Core"
keywords: ASP.NET Core, Entity Framework Core, Migrationen
ms.author: riande
manager: wpickett
ms.date: 8/7/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: bda00f290043251ad308192c5b1a873ae7cd0d85
ms.sourcegitcommit: e832a9b9f41a8b26a8c88edfd8fc35b8bfd97d5d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/18/2017
---
# <a name="adding-a-new-field-to-a-razor-page"></a><span data-ttu-id="a7137-104">Hinzufügen eines neuen Felds zu einer Razor-Seite</span><span class="sxs-lookup"><span data-stu-id="a7137-104">Adding a new field to a Razor Page</span></span>

<span data-ttu-id="a7137-105">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a7137-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a7137-106">In diesem Abschnitt verwenden Sie [Entity Framework](http://docs.efproject.net/en/latest/platforms/aspnetcore/new-db.html) Code First-Migrationen zum Hinzufügen eines neuen Felds zum Modell und zum Migrieren dieser Änderung in die Datenbank.</span><span class="sxs-lookup"><span data-stu-id="a7137-106">In this section you'll use [Entity Framework](http://docs.efproject.net/en/latest/platforms/aspnetcore/new-db.html) Code First Migrations to add a new field to the model and migrate that change to the database.</span></span>

<span data-ttu-id="a7137-107">Wenn Sie EF Code First verwenden, um eine Datenbank automatisch zu erstellen, fügt Code First der Datenbank eine Tabelle hinzu, um nachzuverfolgen, ob das Schema der Datenbank mit den Modellklassen synchron ist, aus denen sie generiert wurde.</span><span class="sxs-lookup"><span data-stu-id="a7137-107">When you use EF Code First to automatically create a database, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="a7137-108">Wenn sie nicht synchron sind, löst EF eine Ausnahme aus.</span><span class="sxs-lookup"><span data-stu-id="a7137-108">If they aren't in sync, EF throws an exception.</span></span> <span data-ttu-id="a7137-109">Dies erleichtert die Suche nach Problemen mit inkonsistenten Datenbanken bzw. inkonsistentem Code.</span><span class="sxs-lookup"><span data-stu-id="a7137-109">This makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="a7137-110">Hinzufügen einer Rating-Eigenschaft zum Movie-Modell</span><span class="sxs-lookup"><span data-stu-id="a7137-110">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="a7137-111">Öffnen Sie die Datei *Models/Movie.cs*, und fügen Sie eine `Rating`-Eigenschaft hinzu:</span><span class="sxs-lookup"><span data-stu-id="a7137-111">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

<span data-ttu-id="a7137-112">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]</span><span class="sxs-lookup"><span data-stu-id="a7137-112">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]</span></span>

<span data-ttu-id="a7137-113">Erstellen Sie die App (STRG+UMSCHALT+B).</span><span class="sxs-lookup"><span data-stu-id="a7137-113">Build the app (Ctrl+Shift+B).</span></span>

<span data-ttu-id="a7137-114">Bearbeiten Sie *Pages/Movies/Index.cshtml*, und fügen ein `Rating`-Feld hinzu:</span><span class="sxs-lookup"><span data-stu-id="a7137-114">Edit *Pages/Movies/Index.cshtml*, and add a `Rating` field:</span></span>

<span data-ttu-id="a7137-115">[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=40-42,61-63)]</span><span class="sxs-lookup"><span data-stu-id="a7137-115">[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=40-42,61-63)]</span></span>

<span data-ttu-id="a7137-116">Fügen Sie das `Rating`-Feld zu den Seiten „Delete“ und „Details“ hinzu.</span><span class="sxs-lookup"><span data-stu-id="a7137-116">Add the `Rating` field to the Delete and Details pages.</span></span>

<span data-ttu-id="a7137-117">Aktualisieren Sie die Datei *Create.cshtml* mit einem `Rating`-Feld.</span><span class="sxs-lookup"><span data-stu-id="a7137-117">Update *Create.cshtml* with a `Rating` field.</span></span> <span data-ttu-id="a7137-118">Sie können das vorherige `<div>`-Element kopieren und einfügen, und IntelliSense kann Ihnen beim Aktualisieren der Felder helfen.</span><span class="sxs-lookup"><span data-stu-id="a7137-118">You can copy/paste the previous `<div>` element and let intelliSense help you update the fields.</span></span> <span data-ttu-id="a7137-119">IntelliSense nutzt [Taghilfsprogramme](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="a7137-119">IntelliSense works with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

![Der Entwickler hat den Buchstaben R als Attributwert von „asp-for“ im zweiten „label“-Element der Ansicht eingegeben.](new-field/_static/cr.png)

<span data-ttu-id="a7137-123">Der folgende Code zeigt *Create.cshtml* mit einem `Rating`-Feld:</span><span class="sxs-lookup"><span data-stu-id="a7137-123">The following code shows *Create.cshtml* with a `Rating` field:</span></span>

<span data-ttu-id="a7137-124">[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?highlight=31-35)]</span><span class="sxs-lookup"><span data-stu-id="a7137-124">[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?highlight=31-35)]</span></span>

<span data-ttu-id="a7137-125">Fügen Sie das Feld `Rating` der Bearbeitungsseite hinzu.</span><span class="sxs-lookup"><span data-stu-id="a7137-125">Add the `Rating` field to the Edit Page.</span></span>

<span data-ttu-id="a7137-126">Die App funktioniert erst, nachdem die Datenbank mit dem neuen Feld aktualisiert wurde.</span><span class="sxs-lookup"><span data-stu-id="a7137-126">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="a7137-127">Wenn sie jetzt ausgeführt wird, löst die App eine `SqlException` aus:</span><span class="sxs-lookup"><span data-stu-id="a7137-127">If run now, the app throws a `SqlException`:</span></span>

`SqlException: Invalid column name 'Rating'.`

<span data-ttu-id="a7137-128">Dieser Fehler wird dadurch verursacht, dass sich die aktualisierte Modellklasse „Movie“ vom Schema der Tabelle „Movie“ der Datenbank unterscheidet.</span><span class="sxs-lookup"><span data-stu-id="a7137-128">This error is caused by the updated Movie model class being different than the schema of the Movie table of the database.</span></span> <span data-ttu-id="a7137-129">(Die Datenbanktabelle enthält nicht die Spalte `Rating`.)</span><span class="sxs-lookup"><span data-stu-id="a7137-129">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="a7137-130">Es gibt mehrere Vorgehensweisen zum Beheben des Fehlers:</span><span class="sxs-lookup"><span data-stu-id="a7137-130">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="a7137-131">Lassen Sie die Datenbank von Entity Framework automatisch löschen und mit dem neuen Modellklassenschema neu erstellen.</span><span class="sxs-lookup"><span data-stu-id="a7137-131">Have the Entity Framework automatically drop and re-create the database using  the new model class schema.</span></span> <span data-ttu-id="a7137-132">Dieser Ansatz ist früh im Entwicklungszyklus sehr praktisch. Er ermöglicht Ihnen, das Modell und das Datenbankschema schnell gemeinsam weiterzuentwickeln.</span><span class="sxs-lookup"><span data-stu-id="a7137-132">This approach is convenient early in the development cycle; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="a7137-133">Der Nachteil ist, dass in der Datenbank vorhandene Daten verloren gehen.</span><span class="sxs-lookup"><span data-stu-id="a7137-133">The downside is that you lose existing data in the database.</span></span> <span data-ttu-id="a7137-134">Sie sollten diesen Ansatz nicht in einer Produktionsdatenbank verwenden!</span><span class="sxs-lookup"><span data-stu-id="a7137-134">You don't want to use this approach on a production database!</span></span> <span data-ttu-id="a7137-135">Das Löschen der Datenbank bei Schemaänderungen und das Verwenden eines Initialisierers zum automatischen Ausführen eines Seedings für eine Datenbank mit Testdaten ist häufig eine produktive Möglichkeit zum Entwickeln einer App.</span><span class="sxs-lookup"><span data-stu-id="a7137-135">Dropping the DB on schema changes and using an initializer to automatically seed the database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="a7137-136">Ändern Sie das Schema der vorhandenen Datenbank explizit so, dass es mit den Modellklassen übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="a7137-136">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="a7137-137">Der Vorteil dieses Ansatzes ist, dass Sie Ihre Daten beibehalten.</span><span class="sxs-lookup"><span data-stu-id="a7137-137">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="a7137-138">Sie können diese Änderung entweder manuell oder durch Erstellen eines Änderungsskripts für die Datenbank vornehmen.</span><span class="sxs-lookup"><span data-stu-id="a7137-138">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="a7137-139">Verwenden Sie Code First-Migrationen, um das Datenbankschema zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="a7137-139">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="a7137-140">Verwenden Sie für dieses Tutorial Code First-Migrationen.</span><span class="sxs-lookup"><span data-stu-id="a7137-140">For this tutorial, use Code First Migrations.</span></span>

<span data-ttu-id="a7137-141">Aktualisieren Sie die `SeedData`-Klasse so, dass sie einen Wert für die neue Spalte bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="a7137-141">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="a7137-142">Eine Beispieländerung ist nachstehend gezeigt, aber Sie sollten diese Änderung für jeden `new Movie`-Block vornehmen.</span><span class="sxs-lookup"><span data-stu-id="a7137-142">A sample change is shown below, but you'll want to make this change for each `new Movie` block.</span></span>

<span data-ttu-id="a7137-143">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]</span><span class="sxs-lookup"><span data-stu-id="a7137-143">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]</span></span>

<span data-ttu-id="a7137-144">Sehen Sie sich die [fertige SeedData.cs-Datei an](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).</span><span class="sxs-lookup"><span data-stu-id="a7137-144">See the [completed SeedData.cs file](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).</span></span>

<span data-ttu-id="a7137-145">Erstellen Sie die Projektmappe.</span><span class="sxs-lookup"><span data-stu-id="a7137-145">Build the solution.</span></span>

<a name="pmc"></a>

<span data-ttu-id="a7137-146">Öffnen Sie das Menü **Extras**. Wählen Sie dort **NuGet-Paket-Manager > Paket-Manager-Konsole** aus.</span><span class="sxs-lookup"><span data-stu-id="a7137-146">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>
<span data-ttu-id="a7137-147">Geben Sie in der PMC die folgenden Befehle ein:</span><span class="sxs-lookup"><span data-stu-id="a7137-147">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Rating
Update-Database
```

<span data-ttu-id="a7137-148">Der `Add-Migration`-Befehl weist das Framework an, Folgendes auszuführen:</span><span class="sxs-lookup"><span data-stu-id="a7137-148">The `Add-Migration` command tells the framework to:</span></span>

* <span data-ttu-id="a7137-149">Das `Movie`-Modell mit dem Schema der `Movie`-Datenbank vergleichen.</span><span class="sxs-lookup"><span data-stu-id="a7137-149">Compare the `Movie` model with the `Movie` DB schema.</span></span>
* <span data-ttu-id="a7137-150">Code erstellen, um das Schema der Datenbank in das neue Modell zu migrieren.</span><span class="sxs-lookup"><span data-stu-id="a7137-150">Create code to migrate the DB schema to the new model.</span></span>

<span data-ttu-id="a7137-151">Der Name „Rating“ ist beliebig und wird verwendet, um die Migrationsdatei zu benennen.</span><span class="sxs-lookup"><span data-stu-id="a7137-151">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="a7137-152">Es ist hilfreich, einen aussagekräftigen Namen für die Migrationsdatei zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="a7137-152">It's helpful to use a meaningful name for the migration file.</span></span>

<span data-ttu-id="a7137-153"><a name="ssox"></a> Wenn Sie alle Datensätze aus der Datenbank löschen, führt der Initialisierer ein Seeding für die Datenbank aus und fügt das Feld `Rating` hinzu.</span><span class="sxs-lookup"><span data-stu-id="a7137-153"><a name="ssox"></a> If you delete all the records in the DB, the initializer will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="a7137-154">Dies ist über die Links „Löschen“ im Browser oder [SQL Server-Objekt-Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX) möglich.</span><span class="sxs-lookup"><span data-stu-id="a7137-154">You can do this with the delete links in the browser or from [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span></span> <span data-ttu-id="a7137-155">So löschen Sie die Datenbank aus SSOX</span><span class="sxs-lookup"><span data-stu-id="a7137-155">To delete the database from SSOX:</span></span>

* <span data-ttu-id="a7137-156">Wählen Sie die Datenbank in SSOX aus.</span><span class="sxs-lookup"><span data-stu-id="a7137-156">Select the database in SSOX.</span></span>
* <span data-ttu-id="a7137-157">Klicken Sie mit der rechten Maustaste auf die Datenbank, und wählen Sie *Löschen* aus.</span><span class="sxs-lookup"><span data-stu-id="a7137-157">Right click on the database, and select *Delete*.</span></span>
* <span data-ttu-id="a7137-158">Aktivieren Sie **Vorhandene Verbindungen schließen*.</span><span class="sxs-lookup"><span data-stu-id="a7137-158">Check **Close existing connections*</span></span>
* <span data-ttu-id="a7137-159">Wählen Sie **OK** aus.</span><span class="sxs-lookup"><span data-stu-id="a7137-159">Select **OK**</span></span>
* <span data-ttu-id="a7137-160">Aktualisieren Sie die Datenbank in [PMC](xref:tutorials/razor-pages/new-field#pmc).</span><span class="sxs-lookup"><span data-stu-id="a7137-160">In the [PMC](xref:tutorials/razor-pages/new-field#pmc), update the database</span></span> 

    ```PMC
    Update-Database
    ```

<span data-ttu-id="a7137-161">Führen Sie die App aus, und überprüfen Sie, ob Sie Filme mit dem Feld `Rating` erstellen/bearbeiten/anzeigen können.</span><span class="sxs-lookup"><span data-stu-id="a7137-161">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="a7137-162">Wenn für die Datenbank kein Seeding ausgeführt wird, beenden Sie IIS Express, und führen Sie dann die App aus.</span><span class="sxs-lookup"><span data-stu-id="a7137-162">If the database is not seeded, stop IIS Express, and then run the app.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="a7137-163">[Vorheriges Tutorial: Hinzufügen der Suche](xref:tutorials/razor-pages/search)
[Nächstes Tutorial: Hinzufügen eines neuen Felds](xref:tutorials/razor-pages/new-field)</span><span class="sxs-lookup"><span data-stu-id="a7137-163">[Previous: Adding Search](xref:tutorials/razor-pages/search)
[Next: Adding a new field](xref:tutorials/razor-pages/new-field)</span></span>
