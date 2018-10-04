---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
title: Hinzufügen eines neuen Felds zum Modell "Movie" und zur Tabelle | Microsoft-Dokumentation
author: Rick-Anderson
description: 'Hinweis: Eine aktualisierte Version dieses Tutorials ist hier verfügbar, dass das ASP.NET MVC 5 und Visual Studio 2013 verwendet. Es ist eine sicherere, viel einfacher zu folgen und demo...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 9ef2c4f1-a305-4e0a-9fb8-bfbd9ef331d9
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
msc.type: authoredcontent
ms.openlocfilehash: 39b48c67b5264a9b3ad97389f6a5c2bf9d94d25f
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2018
ms.locfileid: "48576560"
---
<a name="adding-a-new-field-to-the-movie-model-and-table"></a><span data-ttu-id="748a7-104">Hinzufügen eines neuen Felds zum Modell "Movie" und zur Tabelle</span><span class="sxs-lookup"><span data-stu-id="748a7-104">Adding a New Field to the Movie Model and Table</span></span>
====================
<span data-ttu-id="748a7-105">durch [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="748a7-105">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

> > [!NOTE]
> > <span data-ttu-id="748a7-106">Eine aktualisierte Version dieses Tutorials finden Sie [hier](../../getting-started/introduction/getting-started.md) , ASP.NET MVC 5 und Visual Studio 2013 verwendet.</span><span class="sxs-lookup"><span data-stu-id="748a7-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="748a7-107">Dabei handelt es sich eine sicherere, einfacher, führen weitere Funktionen veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="748a7-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="748a7-108">In diesem Abschnitt verwenden Sie Entity Framework Code First-Migrationen, migrieren einige Änderungen in der ViewModel-Klassen, damit die Änderung auf die Datenbank angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="748a7-108">In this section you'll use Entity Framework Code First Migrations to migrate some changes to the model classes so the change is applied to the database.</span></span>

<span data-ttu-id="748a7-109">In der Standardeinstellung, wenn Sie Entity Framework Code First verwenden, um automatisch eine Datenbank erstellen, wie weiter oben in diesem Tutorial Code First eine Tabelle der Datenbank hinzugefügt, um nachzuverfolgen, ob das Schema der Datenbank mit den Modellklassen synchron ist, die sie aus generiert wurde.</span><span class="sxs-lookup"><span data-stu-id="748a7-109">By default, when you use Entity Framework Code First to automatically create a database, as you did earlier in this tutorial, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="748a7-110">Wenn sie nicht synchron sind, löst Entity Framework einen Fehler aus.</span><span class="sxs-lookup"><span data-stu-id="748a7-110">If they aren't in sync, the Entity Framework throws an error.</span></span> <span data-ttu-id="748a7-111">Dies erleichtert es, die zum Zeitpunkt der Entwicklung Identifizieren von Problemen, die andernfalls nur (durch ungewöhnliche Fehler) zur Laufzeit auftreten.</span><span class="sxs-lookup"><span data-stu-id="748a7-111">This makes it easier to track down issues at development time that you might otherwise only find (by obscure errors) at run time.</span></span>

## <a name="setting-up-code-first-migrations-for-model-changes"></a><span data-ttu-id="748a7-112">Zum Einrichten von Code First-Migrationen Änderungen des Datenmodells</span><span class="sxs-lookup"><span data-stu-id="748a7-112">Setting up Code First Migrations for Model Changes</span></span>

<span data-ttu-id="748a7-113">Wenn Sie Visual Studio 2012 verwenden, doppelklicken klicken Sie auf die *Movies.mdf* Datei im Projektmappen-Explorer, um das Tool zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="748a7-113">If you are using Visual Studio 2012, double click the *Movies.mdf* file from Solution Explorer to open the database tool.</span></span> <span data-ttu-id="748a7-114">Visual Studio Express für Web wird Datenbank-Explorer angezeigt, Visual Studio 2012 Server-Explorer angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="748a7-114">Visual Studio Express for Web will show Database Explorer, Visual Studio 2012 will show Server Explorer.</span></span> <span data-ttu-id="748a7-115">Wenn Sie Visual Studio 2010 verwenden, verwenden Sie die Objekt-Explorer von SQL Server.</span><span class="sxs-lookup"><span data-stu-id="748a7-115">If you are using Visual Studio 2010, use SQL Server Object Explorer.</span></span>

<span data-ttu-id="748a7-116">Im Datenbanktool (Datenbank-Explorer, Server-Explorer oder Objekt-Explorer von SQL Server), klicken Sie mit der rechten Maustaste auf `MovieDBContext` , und wählen Sie **löschen** Filme Datenbank zu löschen.</span><span class="sxs-lookup"><span data-stu-id="748a7-116">In the database tool (Database Explorer, Server Explorer or SQL Server Object Explorer), right click on `MovieDBContext` and select **Delete** to drop the movies database.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image1.png)

<span data-ttu-id="748a7-117">Navigieren Sie zurück zum Projektmappen-Explorer.</span><span class="sxs-lookup"><span data-stu-id="748a7-117">Navigate back to Solution Explorer.</span></span> <span data-ttu-id="748a7-118">Klicken Sie mit der rechten Maustaste auf die *Movies.mdf* und wählen Sie **löschen** zum Entfernen der Datenbank Filme.</span><span class="sxs-lookup"><span data-stu-id="748a7-118">Right click on the *Movies.mdf* file and select **Delete** to remove the movies database.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image2.png)

<span data-ttu-id="748a7-119">Erstellen Sie die Anwendung aus, um sicherzustellen, dass keine Fehler vorliegen.</span><span class="sxs-lookup"><span data-stu-id="748a7-119">Build the application to make sure there are no errors.</span></span>

<span data-ttu-id="748a7-120">Von der **Tools** Menü klicken Sie auf **Bibliothekspaket-Manager** und dann **-Paket-Manager-Konsole**.</span><span class="sxs-lookup"><span data-stu-id="748a7-120">From the **Tools** menu, click **Library Package Manager** and then **Package Manager Console**.</span></span>

![Pack Man hinzufügen](adding-a-new-field-to-the-movie-model-and-table/_static/image3.png)

<span data-ttu-id="748a7-122">In der **-Paket-Manager-Konsole** Fenster auf die `PM>` Eingabeaufforderung Geben Sie "Enable-Migrations - ContextTypeName MvcMovie.Models.MovieDBContext".</span><span class="sxs-lookup"><span data-stu-id="748a7-122">In the **Package Manager Console** window at the `PM>` prompt enter "Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext".</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image4.png)

<span data-ttu-id="748a7-123">Die **Enable-Migrations** (siehe oben) Befehl erstellt eine *Configuration.cs* Datei in einem neuen *Migrationen* Ordner.</span><span class="sxs-lookup"><span data-stu-id="748a7-123">The **Enable-Migrations** command (shown above) creates a *Configuration.cs* file in a new *Migrations* folder.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image5.png)

<span data-ttu-id="748a7-124">Visual Studio öffnet die *Configuration.cs* Datei.</span><span class="sxs-lookup"><span data-stu-id="748a7-124">Visual Studio opens the *Configuration.cs* file.</span></span> <span data-ttu-id="748a7-125">Ersetzen Sie die `Seed` -Methode in der die *Configuration.cs* -Datei mit den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="748a7-125">Replace the `Seed` method in the *Configuration.cs* file with the following code:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample1.cs)]

<span data-ttu-id="748a7-126">Klicken Sie mit der rechten Maustaste auf die rote Wellenlinie unter `Movie` , und wählen Sie **beheben** dann **mit** **MvcMovie.Models;**</span><span class="sxs-lookup"><span data-stu-id="748a7-126">Right click on the red squiggly line under `Movie` and select **Resolve** then **using** **MvcMovie.Models;**</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image6.png)

<span data-ttu-id="748a7-127">Auf diese Weise fügt die folgenden using-Anweisung:</span><span class="sxs-lookup"><span data-stu-id="748a7-127">Doing so adds the following using statement:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample2.cs)]

> [!NOTE] 
> 
> <span data-ttu-id="748a7-128">Code First-Migrationen Ruft die `Seed` -Methode auf, nachdem jede Migration (d. h. Aufrufen **Update-Database** in der Paket-Manager-Konsole), und diese Methode aktualisiert die Zeilen, die bereits eingefügt wurde, oder fügt sie an, wenn sie nicht noch vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="748a7-128">Code First Migrations calls the `Seed` method after every migration (that is, calling **update-database** in the Package Manager Console), and this method updates rows that have already been inserted, or inserts them if they don't exist yet.</span></span>


<span data-ttu-id="748a7-129">**Drücken Sie STRG + UMSCHALT + B, um das Projekt erstellen.** (Die folgenden Schritte aus, schlägt fehl, wenn Ihre lassen sich nicht an diesem Punkt erstellen.)</span><span class="sxs-lookup"><span data-stu-id="748a7-129">**Press CTRL-SHIFT-B to build the project.**(The following steps will fail if your don't build at this point.)</span></span>

<span data-ttu-id="748a7-130">Der nächste Schritt ist die Erstellung einer `DbMigration` -Klasse für die anfängliche Migration.</span><span class="sxs-lookup"><span data-stu-id="748a7-130">The next step is to create a `DbMigration` class for the initial migration.</span></span> <span data-ttu-id="748a7-131">Diese Migration zur erstellt einer neuen Datenbank, die daher wird Sie gelöscht der *movie.mdf* -Datei in einem vorherigen Schritt.</span><span class="sxs-lookup"><span data-stu-id="748a7-131">This migration to creates a new database, that's why you deleted the *movie.mdf* file in a previous step.</span></span>

<span data-ttu-id="748a7-132">In der **-Paket-Manager-Konsole** Fenster, geben Sie den Befehl "add-Migration Initial" zum Erstellen der anfänglichen Migrations.</span><span class="sxs-lookup"><span data-stu-id="748a7-132">In the **Package Manager Console** window, enter the command "add-migration Initial" to create the initial migration.</span></span> <span data-ttu-id="748a7-133">Der Name "Initial" ist willkürlich und wird verwendet, um die Namen der Migrationsdatei erstellt.</span><span class="sxs-lookup"><span data-stu-id="748a7-133">The name "Initial" is arbitrary and is used to name the migration file created.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image7.png)

<span data-ttu-id="748a7-134">Code First-Migrationen wird eine weitere Klassendatei in der *Migrationen* Ordner (mit dem Namen *{Datumsstempel}\_Initial.cs* ), und diese Klasse enthält Code, der das Schema der Datenbank erstellt.</span><span class="sxs-lookup"><span data-stu-id="748a7-134">Code First Migrations creates another class file in the *Migrations* folder (with the name *{DateStamp}\_Initial.cs* ), and this class contains code that creates the database schema.</span></span> <span data-ttu-id="748a7-135">Der Dateiname für die Migration ist bereits mit einem Zeitstempel fest, um mit der Sortierung zu.</span><span class="sxs-lookup"><span data-stu-id="748a7-135">The migration filename is pre-fixed with a timestamp to help with ordering.</span></span> <span data-ttu-id="748a7-136">Überprüfen Sie die *{Datumsstempel}\_Initial.cs* -Datei, er enthält Anweisungen zum Erstellen der Filme-Tabelle für die Movie-Datenbank.</span><span class="sxs-lookup"><span data-stu-id="748a7-136">Examine the *{DateStamp}\_Initial.cs* file, it contains the instructions to create the Movies table for the Movie DB.</span></span> <span data-ttu-id="748a7-137">Beim Aktualisieren der Datenbank in den Anweisungen unten, diese *{Datumsstempel}\_Initial.cs* Datei ausgeführt wird und das Datenbankschema zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="748a7-137">When you update the database in the instructions below, this *{DateStamp}\_Initial.cs* file will run and create the DB schema.</span></span> <span data-ttu-id="748a7-138">Die **Ausgangswert** Methode wird ausgeführt, um die Datenbank mit Testdaten aufzufüllen.</span><span class="sxs-lookup"><span data-stu-id="748a7-138">Then the **Seed** method will run to populate the DB with test data.</span></span>

<span data-ttu-id="748a7-139">In der **-Paket-Manager-Konsole**, geben Sie den Befehl "Update-Database" zum Erstellen der Datenbank und Ausführen der **Ausgangswert** Methode.</span><span class="sxs-lookup"><span data-stu-id="748a7-139">In the **Package Manager Console**, enter the command "update-database" to create the database and run the **Seed** method.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image8.png)

<span data-ttu-id="748a7-140">Wenn Sie eine Fehlermeldung, der angibt erhalten, eine Tabelle ist bereits vorhanden und kann nicht erstellt werden, es ist wahrscheinlich, da Sie die Anwendung ausgeführt, nachdem Sie die Datenbank gelöscht und bevor Sie ausgeführt `update-database`.</span><span class="sxs-lookup"><span data-stu-id="748a7-140">If you get an error that indicates a table already exists and can't be created, it is probably because you ran the application after you deleted the database and before you executed `update-database`.</span></span> <span data-ttu-id="748a7-141">In diesem Fall löschen Sie die *Movies.mdf* -Datei erneut, und wiederholen Sie den `update-database` Befehl.</span><span class="sxs-lookup"><span data-stu-id="748a7-141">In that case, delete the *Movies.mdf* file again and retry the `update-database` command.</span></span> <span data-ttu-id="748a7-142">Wenn Sie weiterhin eine Fehlermeldung erhalten, löschen Sie den Ordner "Migrations" und den Inhalt, und beginnen Sie mit den Anweisungen am oberen Rand dieser Seite (d.h. Löschen der *Movies.mdf* -Datei, und fahren Sie mit der Enable-Migrations).</span><span class="sxs-lookup"><span data-stu-id="748a7-142">If you still get an error, delete the migrations folder and contents then start with the instructions at the top of this page (that is delete the *Movies.mdf* file then proceed to Enable-Migrations).</span></span>

<span data-ttu-id="748a7-143">Führen Sie die Anwendung, und navigieren Sie zu der */Movies* URL.</span><span class="sxs-lookup"><span data-stu-id="748a7-143">Run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="748a7-144">Die Seed-Daten werden angezeigt.</span><span class="sxs-lookup"><span data-stu-id="748a7-144">The seed data is displayed.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image9.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="748a7-145">Hinzufügen einer Rating-Eigenschaft zum Movie-Modell</span><span class="sxs-lookup"><span data-stu-id="748a7-145">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="748a7-146">Starten Sie durch Hinzufügen eines neuen `Rating` Eigenschaft, um die vorhandene `Movie` Klasse.</span><span class="sxs-lookup"><span data-stu-id="748a7-146">Start by adding a new `Rating` property to the existing `Movie` class.</span></span> <span data-ttu-id="748a7-147">Öffnen der *Models\Movie.cs* -Datei und fügen die `Rating` Eigenschaft dieser Art:</span><span class="sxs-lookup"><span data-stu-id="748a7-147">Open the *Models\Movie.cs* file and add the `Rating` property like this one:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample3.cs)]

<span data-ttu-id="748a7-148">Die vollständige `Movie` -Klasse sieht wie im folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="748a7-148">The complete `Movie` class now looks like the following code:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample4.cs?highlight=8)]

<span data-ttu-id="748a7-149">Erstellen der Anwendung mithilfe der **erstellen** &gt; **erstellen Film** Menü Befehl oder durch Drücken der STRG-UMSCHALT-b.</span><span class="sxs-lookup"><span data-stu-id="748a7-149">Build the application using the **Build** &gt;**Build Movie** menu command or by pressing CTRL-SHIFT-B.</span></span>

<span data-ttu-id="748a7-150">Nun, dass Sie aktualisiert haben die `Model` -Klasse, Sie müssen auch zum Aktualisieren der *\Views\Movies\Index.cshtml* und *\Views\Movies\Create.cshtml* Vorlagen anzeigen, um die neue anzuzeigen`Rating`Eigenschaft in der Browseransicht.</span><span class="sxs-lookup"><span data-stu-id="748a7-150">Now that you've updated the `Model` class, you also need to update the *\Views\Movies\Index.cshtml* and *\Views\Movies\Create.cshtml* view templates in order to display the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="748a7-151">Öffnen der<em>\Views\Movies\Index.cshtml</em> -Datei und fügen eine `<th>Rating</th>` Spaltenüberschrift direkt nach der <strong>Preis</strong> Spalte.</span><span class="sxs-lookup"><span data-stu-id="748a7-151">Open the<em>\Views\Movies\Index.cshtml</em> file and add a `<th>Rating</th>` column heading just after the <strong>Price</strong> column.</span></span> <span data-ttu-id="748a7-152">Fügen Sie dann eine `<td>` Spalte am Ende der Vorlage zum Rendern der `@item.Rating` Wert.</span><span class="sxs-lookup"><span data-stu-id="748a7-152">Then add a `<td>` column near the end of the template to render the `@item.Rating` value.</span></span> <span data-ttu-id="748a7-153">Im folgenden finden Sie welche die aktualisierte <em>"Index.cshtml"</em> ansichtsvorlage sieht wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="748a7-153">Below is what the updated <em>Index.cshtml</em> view template looks like:</span></span>

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample5.cshtml?highlight=26-28,46-48)]

<span data-ttu-id="748a7-154">Öffnen Sie als Nächstes die *\Views\Movies\Create.cshtml* Datei, und fügen Sie das folgende Markup in der Nähe des Formulars.</span><span class="sxs-lookup"><span data-stu-id="748a7-154">Next, open the *\Views\Movies\Create.cshtml* file and add the following markup near the end of the form.</span></span> <span data-ttu-id="748a7-155">Dies rendert ein Textfeld, sodass Sie eine Bewertung angeben können, wenn Sie ein neuer Film erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="748a7-155">This renders a text box so that you can specify a rating when a new movie is created.</span></span>

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample6.cshtml)]

<span data-ttu-id="748a7-156">Sie haben jetzt den Anwendungscode so, dass die Unterstützung der neuen `Rating` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="748a7-156">You've now updated the application code to support the new `Rating` property.</span></span>

<span data-ttu-id="748a7-157">Die Anwendung jetzt ausführen, und navigieren Sie zu der */Movies* URL.</span><span class="sxs-lookup"><span data-stu-id="748a7-157">Now run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="748a7-158">Wenn Sie dies tun, wird jedoch eine der folgenden Fehlermeldungen angezeigt:</span><span class="sxs-lookup"><span data-stu-id="748a7-158">When you do this, though, you'll see one of the following errors:</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image10.png)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image11.png)

<span data-ttu-id="748a7-159">Sie sehen diesen Fehler, da die aktualisierte `Movie` Modellklasse, die in der Anwendung unterscheidet sich von dem Schema der der `Movie` Tabelle der vorhandenen Datenbank.</span><span class="sxs-lookup"><span data-stu-id="748a7-159">You're seeing this error because the updated `Movie` model class in the application is now different than the schema of the `Movie` table of the existing database.</span></span> <span data-ttu-id="748a7-160">(Die Datenbanktabelle enthält nicht die Spalte `Rating`.)</span><span class="sxs-lookup"><span data-stu-id="748a7-160">(There's no `Rating` column in the database table.)</span></span>


<span data-ttu-id="748a7-161">Es gibt mehrere Vorgehensweisen zum Beheben des Fehlers:</span><span class="sxs-lookup"><span data-stu-id="748a7-161">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="748a7-162">Lassen Sie die Datenbank von Entity Framework automatisch löschen und basierend auf dem neuen Modellklassenschema neu erstellen.</span><span class="sxs-lookup"><span data-stu-id="748a7-162">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="748a7-163">Dieser Ansatz ist sehr praktisch, bei der aktiven Entwicklung in einer Testdatenbank; Sie können das Modell und das Datenbankschema schnell gemeinsam weiterzuentwickeln.</span><span class="sxs-lookup"><span data-stu-id="748a7-163">This approach is very convenient when doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="748a7-164">Der Nachteil ist jedoch, dass Sie in der Datenbank vorhandene Daten verloren gehen, damit Sie *nicht* dieser Ansatz in einer Produktionsdatenbank verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="748a7-164">The downside, though, is that you lose existing data in the database — so you *don't* want to use this approach on a production database!</span></span> <span data-ttu-id="748a7-165">Verwenden eines Initialisierers ist zum automatisch Seeding für eine Datenbank mit Testdaten häufig eine produktive Möglichkeit zum Entwickeln einer Anwendung.</span><span class="sxs-lookup"><span data-stu-id="748a7-165">Using an initializer to automatically seed a database with test data is often a productive way to develope an application.</span></span> <span data-ttu-id="748a7-166">Weitere Informationen zu Entity Framework-Datenbankinitialisierer, finden Sie unter der Tom Dykstra [ASP.NET MVC/Entity Framework-Tutorial](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="748a7-166">For more information on Entity Framework database initializers, see Tom Dykstra's [ASP.NET MVC/Entity Framework tutorial](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>
2. <span data-ttu-id="748a7-167">Ändern Sie das Schema der vorhandenen Datenbank explizit so, dass es mit den Modellklassen übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="748a7-167">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="748a7-168">Der Vorteil dieses Ansatzes ist, dass Sie Ihre Daten behalten.</span><span class="sxs-lookup"><span data-stu-id="748a7-168">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="748a7-169">Sie können diese Änderung entweder manuell oder durch Erstellen eines Änderungsskripts für die Datenbank vornehmen.</span><span class="sxs-lookup"><span data-stu-id="748a7-169">You can make this change either manually or by creating a database change script.</span></span>
3. <span data-ttu-id="748a7-170">Verwenden Sie Code First-Migrationen, um das Datenbankschema zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="748a7-170">Use Code First Migrations to update the database schema.</span></span>


<span data-ttu-id="748a7-171">Für dieses Tutorial verwenden wir Code First-Migrationen.</span><span class="sxs-lookup"><span data-stu-id="748a7-171">For this tutorial, we'll use Code First Migrations.</span></span>

<span data-ttu-id="748a7-172">Aktualisieren Sie die Seed-Methode, sodass sie einen Wert für die neue Spalte bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="748a7-172">Update the Seed method so that it provides a value for the new column.</span></span> <span data-ttu-id="748a7-173">Öffnen Sie migrations\configuration. cs-Datei, und fügen ein Bewertungsfeld jedes Movie-Objekt hinzu.</span><span class="sxs-lookup"><span data-stu-id="748a7-173">Open Migrations\Configuration.cs file and add a Rating field to each Movie object.</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample7.cs?highlight=6)]

<span data-ttu-id="748a7-174">Erstellen Sie die Projektmappe, und öffnen Sie die **-Paket-Manager-Konsole** Fenster, und geben Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="748a7-174">Build the solution, and then open the **Package Manager Console** window and enter the following command:</span></span>

`add-migration AddRatingMig`

<span data-ttu-id="748a7-175">Die `add-migration` Befehl weist das migrationsframework an das aktuelle Movie-Modell mit dem aktuellen Film DB Schema überprüfen und den erforderlichen Code zum Migrieren der Datenbank in das neue Modell zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="748a7-175">The `add-migration` command tells the migration framework to examine the current movie model with the current movie DB schema and create the necessary code to migrate the DB to the new model.</span></span> <span data-ttu-id="748a7-176">Die AddRatingMig ist willkürlich und wird verwendet, um die Migrationsdatei zu benennen.</span><span class="sxs-lookup"><span data-stu-id="748a7-176">The AddRatingMig is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="748a7-177">Es ist hilfreich, einen aussagekräftigen Namen für den Migrationsschritt zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="748a7-177">It's helpful to use a meaningful name for the migration step.</span></span>

<span data-ttu-id="748a7-178">Wenn dieser Befehl abgeschlossen ist, handelt es sich bei Visual Studio öffnet die Klassendatei, die die neue definiert `DbMIgration` abgeleitete Klasse sein, und klicken Sie in der `Up` Methode sehen Sie den Code, der die neue Spalte erstellt.</span><span class="sxs-lookup"><span data-stu-id="748a7-178">When this command finishes, Visual Studio opens the class file that defines the new `DbMIgration` derived class, and in the `Up` method you can see the code that creates the new column.</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample8.cs)]

<span data-ttu-id="748a7-179">Erstellen Sie die Projektmappe, und geben Sie dann den Befehl "Update-Database" in der **-Paket-Manager-Konsole** Fenster.</span><span class="sxs-lookup"><span data-stu-id="748a7-179">Build the solution, and then enter the "update-database" command in the **Package Manager Console** window.</span></span>

<span data-ttu-id="748a7-180">Die folgende Abbildung zeigt die Ausgabe in die **-Paket-Manager-Konsole** Fenster (der Datumsstempel voranstellen AddRatingMig unterscheiden.)</span><span class="sxs-lookup"><span data-stu-id="748a7-180">The following image shows the output in the **Package Manager Console** window (The date stamp prepending AddRatingMig will be different.)</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image12.png)

<span data-ttu-id="748a7-181">Führen Sie die Anwendung erneut aus, und navigieren Sie zu der URL /Movies.</span><span class="sxs-lookup"><span data-stu-id="748a7-181">Re-run the application and navigate to the /Movies URL.</span></span> <span data-ttu-id="748a7-182">Sie sehen, dass das neue Feld für die Bewertung.</span><span class="sxs-lookup"><span data-stu-id="748a7-182">You can see the new Rating field.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image13.png)

<span data-ttu-id="748a7-183">Klicken Sie auf die **neu erstellen** Link, um einen neuen Film hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="748a7-183">Click the **Create New** link to add a new movie.</span></span> <span data-ttu-id="748a7-184">Beachten Sie, dass Sie eine Bewertung hinzufügen können.</span><span class="sxs-lookup"><span data-stu-id="748a7-184">Note that you can add a rating.</span></span>

![7_CreateRioII](adding-a-new-field-to-the-movie-model-and-table/_static/image14.png)

<span data-ttu-id="748a7-186">Klicken Sie auf **Erstellen**.</span><span class="sxs-lookup"><span data-stu-id="748a7-186">Click **Create**.</span></span> <span data-ttu-id="748a7-187">Der neue Film, einschließlich "Rating", wird jetzt in den Filmen auflisten:</span><span class="sxs-lookup"><span data-stu-id="748a7-187">The new movie, including the rating, now shows up in the movies listing:</span></span>

![7_ourNewMovie_SM](adding-a-new-field-to-the-movie-model-and-table/_static/image15.png)

<span data-ttu-id="748a7-189">Sie sollten auch hinzufügen, die `Rating` Datumsfeld in den Bearbeitungsmodus, Details und SearchIndex Vorlagen anzeigen.</span><span class="sxs-lookup"><span data-stu-id="748a7-189">You should also add the `Rating` field to the Edit, Details and SearchIndex view templates.</span></span>

<span data-ttu-id="748a7-190">Könnten Sie eingeben, den Befehl "Update-Database" in der **-Paket-Manager-Konsole** Fenster erneut und keine Änderungen würden vorgenommen werden, da das Modell mit dem Schema übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="748a7-190">You could enter the "update-database" command in the **Package Manager Console** window again and no changes would be made, because the schema matches the model.</span></span>

<span data-ttu-id="748a7-191">In diesem Abschnitt wurde erläutert, wie Sie Modellobjekte ändern und die Datenbank mit den Änderungen synchron zu halten.</span><span class="sxs-lookup"><span data-stu-id="748a7-191">In this section you saw how you can modify model objects and keep the database in sync with the changes.</span></span> <span data-ttu-id="748a7-192">Sie haben zudem eine Möglichkeit, eine neu erstellte Datenbank mit Beispieldaten aufzufüllen, damit Sie die Szenarien ausprobieren können.</span><span class="sxs-lookup"><span data-stu-id="748a7-192">You also learned a way to populate a newly created database with sample data so you can try out scenarios.</span></span> <span data-ttu-id="748a7-193">Als Nächstes sehen wir uns, wie Sie die Modellklassen umfangreichere Validierungslogik hinzugefügt und einige Geschäftsregeln erzwungen werden zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="748a7-193">Next, let's look at how you can add richer validation logic to the model classes and enable some business rules to be enforced.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="748a7-194">[Zurück](examining-the-edit-methods-and-edit-view.md)
> [Weiter](adding-validation-to-the-model.md)</span><span class="sxs-lookup"><span data-stu-id="748a7-194">[Previous](examining-the-edit-methods-and-edit-view.md)
[Next](adding-validation-to-the-model.md)</span></span>
