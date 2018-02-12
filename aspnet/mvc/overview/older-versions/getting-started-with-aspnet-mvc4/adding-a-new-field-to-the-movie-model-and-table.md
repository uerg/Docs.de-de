---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
title: "Hinzufügen eines neuen Felds in die Film-Modell und die Tabelle | Microsoft Docs"
author: Rick-Anderson
description: "Hinweis: Eine aktualisierte Version dieses Lernprogramms ist hier verfügbar, die ASP.NET MVC 5 und Visual Studio 2013 verwendet. Es ist sicherer, viel einfacher zu verfolgen und demo..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 9ef2c4f1-a305-4e0a-9fb8-bfbd9ef331d9
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
msc.type: authoredcontent
ms.openlocfilehash: 9965c8a755857a8e8cb8ecbc6c467a6c856aa83d
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/11/2018
---
<a name="adding-a-new-field-to-the-movie-model-and-table"></a><span data-ttu-id="4c397-104">Das Movie-Modell und die Tabelle hinzugefügt ein neues Feld</span><span class="sxs-lookup"><span data-stu-id="4c397-104">Adding a New Field to the Movie Model and Table</span></span>
====================
<span data-ttu-id="4c397-105">Durch [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="4c397-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="4c397-106">Eine aktualisierte Version dieses Lernprogramms steht [hier](../../getting-started/introduction/getting-started.md) , ASP.NET MVC 5 und Visual Studio 2013 verwendet.</span><span class="sxs-lookup"><span data-stu-id="4c397-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="4c397-107">Es ist sicherer, viel einfacher, führen und weitere Funktionen veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="4c397-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="4c397-108">In diesem Abschnitt verwenden Sie Entity Framework Code First-Migrationen zu einige Änderungen an der Modellklassen migrieren, damit die Änderung auf die Datenbank angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="4c397-108">In this section you'll use Entity Framework Code First Migrations to migrate some changes to the model classes so the change is applied to the database.</span></span>

<span data-ttu-id="4c397-109">Standardmäßig Wenn Sie Entity Framework Code First verwenden, um automatisch eine Datenbank erstellen, wie Sie weiter oben in diesem Lernprogramm ausgeführt haben Code First eine Tabelle der Datenbank hinzugefügt, um nachzuverfolgen, ob das Schema der Datenbank mit der Modellklassen synchron ist, die sie aus generiert wurde.</span><span class="sxs-lookup"><span data-stu-id="4c397-109">By default, when you use Entity Framework Code First to automatically create a database, as you did earlier in this tutorial, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="4c397-110">Wenn sie nicht synchron sind, löst das Entity Framework einen Fehler auf.</span><span class="sxs-lookup"><span data-stu-id="4c397-110">If they aren't in sync, the Entity Framework throws an error.</span></span> <span data-ttu-id="4c397-111">Dies erleichtert das Identifizieren von Problemen zur Entwicklungszeit zu verfolgen, die andernfalls nur (durch kryptische Fehler) zur Laufzeit auftreten.</span><span class="sxs-lookup"><span data-stu-id="4c397-111">This makes it easier to track down issues at development time that you might otherwise only find (by obscure errors) at run time.</span></span>

## <a name="setting-up-code-first-migrations-for-model-changes"></a><span data-ttu-id="4c397-112">Einrichten von Code First-Migrationen für Modelländerungen</span><span class="sxs-lookup"><span data-stu-id="4c397-112">Setting up Code First Migrations for Model Changes</span></span>

<span data-ttu-id="4c397-113">Wenn Sie Visual Studio 2012 verwenden, doppelklicken klicken Sie auf die *Movies.mdf* Datei im Projektmappen-Explorer, um das Datenbanktool zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="4c397-113">If you are using Visual Studio 2012, double click the *Movies.mdf* file from Solution Explorer to open the database tool.</span></span> <span data-ttu-id="4c397-114">Visual Studio Express für Web wird Datenbank-Explorer anzeigen, Visual Studio 2012 Server-Explorer angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="4c397-114">Visual Studio Express for Web will show Database Explorer, Visual Studio 2012 will show Server Explorer.</span></span> <span data-ttu-id="4c397-115">Wenn Sie Visual Studio 2010 verwenden, verwenden Sie SQL Server-Objekt-Explorer.</span><span class="sxs-lookup"><span data-stu-id="4c397-115">If you are using Visual Studio 2010, use SQL Server Object Explorer.</span></span>

<span data-ttu-id="4c397-116">In der Datenbanktool (Datenbank-Explorer, Server-Explorer oder Objekt-Explorer von SQL Server), klicken Sie mit der mit der rechten Maustaste auf `MovieDBContext` , und wählen Sie **löschen** beim Löschen der Datenbank für Filme.</span><span class="sxs-lookup"><span data-stu-id="4c397-116">In the database tool (Database Explorer, Server Explorer or SQL Server Object Explorer), right click on `MovieDBContext` and select **Delete** to drop the movies database.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image1.png)

<span data-ttu-id="4c397-117">Navigieren Sie zurück zum Projektmappen-Explorer.</span><span class="sxs-lookup"><span data-stu-id="4c397-117">Navigate back to Solution Explorer.</span></span> <span data-ttu-id="4c397-118">Klicken Sie mit der rechten Maustaste auf die *Movies.mdf* Datei, und wählen Sie **löschen** zum Entfernen der Datenbank für Filme.</span><span class="sxs-lookup"><span data-stu-id="4c397-118">Right click on the *Movies.mdf* file and select **Delete** to remove the movies database.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image2.png)

<span data-ttu-id="4c397-119">Erstellen Sie die Anwendung aus, um sicherzustellen, dass keine Fehler vorliegen.</span><span class="sxs-lookup"><span data-stu-id="4c397-119">Build the application to make sure there are no errors.</span></span>

<span data-ttu-id="4c397-120">Aus der **Tools** Menü klicken Sie auf **Bibliothekspaket-Manager** und dann **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="4c397-120">From the **Tools** menu, click **Library Package Manager** and then **Package Manager Console**.</span></span>

![Pack Man hinzufügen](adding-a-new-field-to-the-movie-model-and-table/_static/image3.png)

<span data-ttu-id="4c397-122">In der **Package Manager Console** Fensters auf die `PM>` Eingabeaufforderung Geben Sie "Enable-Migrations - ContextTypeName MvcMovie.Models.MovieDBContext".</span><span class="sxs-lookup"><span data-stu-id="4c397-122">In the **Package Manager Console** window at the `PM>` prompt enter "Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext".</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image4.png)

<span data-ttu-id="4c397-123">Die **Enable-Migrations** (siehe oben) Befehl erstellt eine *"Configuration.cs"* Datei in einem neuen *Migrationen* Ordner.</span><span class="sxs-lookup"><span data-stu-id="4c397-123">The **Enable-Migrations** command (shown above) creates a *Configuration.cs* file in a new *Migrations* folder.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image5.png)

<span data-ttu-id="4c397-124">Visual Studio öffnet die *"Configuration.cs"* Datei.</span><span class="sxs-lookup"><span data-stu-id="4c397-124">Visual Studio opens the *Configuration.cs* file.</span></span> <span data-ttu-id="4c397-125">Ersetzen Sie die `Seed` Methode in der *"Configuration.cs"* Datei durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="4c397-125">Replace the `Seed` method in the *Configuration.cs* file with the following code:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample1.cs)]

<span data-ttu-id="4c397-126">Klicken Sie mit der rechten Maustaste auf die rote Wellenlinie unter `Movie` , und wählen Sie **beheben** dann **mit** **MvcMovie.Models;**</span><span class="sxs-lookup"><span data-stu-id="4c397-126">Right click on the red squiggly line under `Movie` and select **Resolve** then **using** **MvcMovie.Models;**</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image6.png)

<span data-ttu-id="4c397-127">Auf diese Weise fügt die folgende Anweisung:</span><span class="sxs-lookup"><span data-stu-id="4c397-127">Doing so adds the following using statement:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample2.cs)]

> [!NOTE] 
> 
> <span data-ttu-id="4c397-128">Code First-Migrationen Ruft die `Seed` Methode nach jeder Migration (d. h. Aufrufen **Update-Database '** in der Paket-Manager-Konsole), und diese Methode aktualisiert die Zeilen, die bereits eingefügt wurde, oder fügt sie an, wenn sie nicht noch vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="4c397-128">Code First Migrations calls the `Seed` method after every migration (that is, calling **update-database** in the Package Manager Console), and this method updates rows that have already been inserted, or inserts them if they don't exist yet.</span></span>


<span data-ttu-id="4c397-129">**Drücken Sie STRG-UMSCHALT + B, um das Projekt zu erstellen.** (Die folgenden Schritte aus, schlägt fehl, wenn die nicht zu diesem Zeitpunkt erstellen.)</span><span class="sxs-lookup"><span data-stu-id="4c397-129">**Press CTRL-SHIFT-B to build the project.**(The following steps will fail if your don't build at this point.)</span></span>

<span data-ttu-id="4c397-130">Der nächste Schritt ist die Erstellung einer `DbMigration` der anfänglichen Migration-Klasse.</span><span class="sxs-lookup"><span data-stu-id="4c397-130">The next step is to create a `DbMigration` class for the initial migration.</span></span> <span data-ttu-id="4c397-131">Diese Migration zur erstellt einer neuen Datenbank, die verdeutlichen, warum Sie gelöschte der *movie.mdf* Datei in einem vorherigen Schritt.</span><span class="sxs-lookup"><span data-stu-id="4c397-131">This migration to creates a new database, that's why you deleted the *movie.mdf* file in a previous step.</span></span>

<span data-ttu-id="4c397-132">In der **Package Manager Console** Fenster, geben Sie den Befehl "hinzufügen Migration anfängliche" zum Erstellen der anfänglichen Migrations.</span><span class="sxs-lookup"><span data-stu-id="4c397-132">In the **Package Manager Console** window, enter the command "add-migration Initial" to create the initial migration.</span></span> <span data-ttu-id="4c397-133">Der Name "Initial" ist ein beliebiger und wird verwendet, um die Namen der Migrationsdatei erstellt.</span><span class="sxs-lookup"><span data-stu-id="4c397-133">The name "Initial" is arbitrary and is used to name the migration file created.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image7.png)

<span data-ttu-id="4c397-134">Code First-Migrationen erstellt einen anderen Klassendatei in der *Migrationen* Ordner (mit dem Namen *{Datumsstempel}\_Initial.cs* ), und diese Klasse enthält Code, der das Schema der Datenbank erstellt.</span><span class="sxs-lookup"><span data-stu-id="4c397-134">Code First Migrations creates another class file in the *Migrations* folder (with the name *{DateStamp}\_Initial.cs* ), and this class contains code that creates the database schema.</span></span> <span data-ttu-id="4c397-135">Der Dateiname für die Migration ist bereits mit einem Zeitstempel fest, um mit der Reihenfolge zu.</span><span class="sxs-lookup"><span data-stu-id="4c397-135">The migration filename is pre-fixed with a timestamp to help with ordering.</span></span> <span data-ttu-id="4c397-136">Überprüfen Sie die *{Datumsstempel}\_Initial.cs* Datei, er enthält eine Anleitung zum Erstellen der Filme-Tabelle für die Film-DB.</span><span class="sxs-lookup"><span data-stu-id="4c397-136">Examine the *{DateStamp}\_Initial.cs* file, it contains the instructions to create the Movies table for the Movie DB.</span></span> <span data-ttu-id="4c397-137">Beim Aktualisieren der Datenbank in den Anweisungen unten, dies *{Datumsstempel}\_Initial.cs* Datei ausgeführt wird und der DB-Schema erstellen.</span><span class="sxs-lookup"><span data-stu-id="4c397-137">When you update the database in the instructions below, this *{DateStamp}\_Initial.cs* file will run and create the DB schema.</span></span> <span data-ttu-id="4c397-138">Die **Ausgangswert** Methode zum Auffüllen der Datenbank mit Testdaten ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="4c397-138">Then the **Seed** method will run to populate the DB with test data.</span></span>

<span data-ttu-id="4c397-139">In der **Package Manager Console**, geben Sie den Befehl "Update-Database" zum Erstellen der Datenbank, und führen Sie die **Ausgangswert** Methode.</span><span class="sxs-lookup"><span data-stu-id="4c397-139">In the **Package Manager Console**, enter the command "update-database" to create the database and run the **Seed** method.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image8.png)

<span data-ttu-id="4c397-140">Wenn Sie eine Fehlermeldung, der angibt erhalten, eine Tabelle ist bereits vorhanden und kann nicht erstellt werden, es ist wahrscheinlich, da die Anwendung ausgeführt wird, nachdem Sie die Datenbank gelöscht und bevor Sie ausgeführt `update-database`.</span><span class="sxs-lookup"><span data-stu-id="4c397-140">If you get an error that indicates a table already exists and can't be created, it is probably because you ran the application after you deleted the database and before you executed `update-database`.</span></span> <span data-ttu-id="4c397-141">In diesem Fall löschen Sie die *Movies.mdf* Datei erneut, und wiederholen Sie den `update-database` Befehl.</span><span class="sxs-lookup"><span data-stu-id="4c397-141">In that case, delete the *Movies.mdf* file again and retry the `update-database` command.</span></span> <span data-ttu-id="4c397-142">Wenn Sie weiterhin eine Fehlermeldung erhalten, löschen Sie den Ordner und den Inhalt dann mit den Anweisungen am oberen Rand dieser Seite beginnen (also Löschen der *Movies.mdf* Datei, und klicken Sie dann den Vorgang fortzusetzen, Enable-Migrations).</span><span class="sxs-lookup"><span data-stu-id="4c397-142">If you still get an error, delete the migrations folder and contents then start with the instructions at the top of this page (that is delete the *Movies.mdf* file then proceed to Enable-Migrations).</span></span>

<span data-ttu-id="4c397-143">Führen Sie die Anwendung, und navigieren Sie zu der */Movies* URL.</span><span class="sxs-lookup"><span data-stu-id="4c397-143">Run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="4c397-144">Die Seed-Daten werden angezeigt.</span><span class="sxs-lookup"><span data-stu-id="4c397-144">The seed data is displayed.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image9.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="4c397-145">Hinzufügen einer Rating-Eigenschaft zum Movie-Modell</span><span class="sxs-lookup"><span data-stu-id="4c397-145">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="4c397-146">Starten, indem er ein neues `Rating` Eigenschaft, um die vorhandene `Movie` Klasse.</span><span class="sxs-lookup"><span data-stu-id="4c397-146">Start by adding a new `Rating` property to the existing `Movie` class.</span></span> <span data-ttu-id="4c397-147">Öffnen der *Models\Movie.cs* und fügen die `Rating` wie die folgende Eigenschaft:</span><span class="sxs-lookup"><span data-stu-id="4c397-147">Open the *Models\Movie.cs* file and add the `Rating` property like this one:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample3.cs)]

<span data-ttu-id="4c397-148">Die vollständige `Movie` -Klasse sieht wie der folgende Code:</span><span class="sxs-lookup"><span data-stu-id="4c397-148">The complete `Movie` class now looks like the following code:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample4.cs?highlight=8)]

<span data-ttu-id="4c397-149">Erstellen der Anwendung über die **erstellen** &gt; **Film erstellen** Menü den Befehl oder durch Drücken von STRG-UMSCHALT-b.</span><span class="sxs-lookup"><span data-stu-id="4c397-149">Build the application using the **Build** &gt;**Build Movie** menu command or by pressing CTRL-SHIFT-B.</span></span>

<span data-ttu-id="4c397-150">Nun, dass Sie aktualisiert haben die `Model` -Klasse, Sie müssen auch zum Aktualisieren der *\Views\Movies\Index.cshtml* und *\Views\Movies\Create.cshtml* Vorlagen anzeigen, um die neue anzuzeigen`Rating`Eigenschaft in der Browseransicht.</span><span class="sxs-lookup"><span data-stu-id="4c397-150">Now that you've updated the `Model` class, you also need to update the *\Views\Movies\Index.cshtml* and *\Views\Movies\Create.cshtml* view templates in order to display the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="4c397-151">Öffnen der*\Views\Movies\Index.cshtml* und fügen eine `<th>Rating</th>` Spaltenüberschrift direkt nach der **Preis** Spalte.</span><span class="sxs-lookup"><span data-stu-id="4c397-151">Open the*\Views\Movies\Index.cshtml* file and add a `<th>Rating</th>` column heading just after the **Price** column.</span></span> <span data-ttu-id="4c397-152">Fügen Sie dann eine `<td>` Spalte am Ende der Vorlage zum Rendern der `@item.Rating` Wert.</span><span class="sxs-lookup"><span data-stu-id="4c397-152">Then add a `<td>` column near the end of the template to render the `@item.Rating` value.</span></span> <span data-ttu-id="4c397-153">Im folgenden finden Sie welche die aktualisierte *Index.cshtml* Vorlage anzeigen sieht wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="4c397-153">Below is what the updated *Index.cshtml* view template looks like:</span></span>

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample5.cshtml?highlight=26-28,46-48)]

<span data-ttu-id="4c397-154">Öffnen Sie als Nächstes die *\Views\Movies\Create.cshtml* Datei, und fügen Sie das folgende Markup gegen Ende des Formulars.</span><span class="sxs-lookup"><span data-stu-id="4c397-154">Next, open the *\Views\Movies\Create.cshtml* file and add the following markup near the end of the form.</span></span> <span data-ttu-id="4c397-155">Dies rendert ein Textfeld, sodass Sie eine Bewertung angeben können, wenn ein neue Film erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="4c397-155">This renders a text box so that you can specify a rating when a new movie is created.</span></span>

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample6.cshtml)]

<span data-ttu-id="4c397-156">Sie haben jetzt den Code der Anwendung, um die Unterstützung der neuen `Rating` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="4c397-156">You've now updated the application code to support the new `Rating` property.</span></span>

<span data-ttu-id="4c397-157">Die Anwendung jetzt ausführen, und navigieren Sie zu der */Movies* URL.</span><span class="sxs-lookup"><span data-stu-id="4c397-157">Now run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="4c397-158">Wenn Sie dies tun, wird jedoch eine der folgenden Fehlermeldungen angezeigt:</span><span class="sxs-lookup"><span data-stu-id="4c397-158">When you do this, though, you'll see one of the following errors:</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image10.png)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image11.png)

<span data-ttu-id="4c397-159">Dieser Fehler gemeldet wird, da die aktualisierte `Movie` Modellklasse in der Anwendung unterscheidet sich von dem Schema der der `Movie` Tabelle der vorhandenen Datenbank.</span><span class="sxs-lookup"><span data-stu-id="4c397-159">You're seeing this error because the updated `Movie` model class in the application is now different than the schema of the `Movie` table of the existing database.</span></span> <span data-ttu-id="4c397-160">(Die Datenbanktabelle enthält nicht die Spalte `Rating`.)</span><span class="sxs-lookup"><span data-stu-id="4c397-160">(There's no `Rating` column in the database table.)</span></span>


<span data-ttu-id="4c397-161">Es gibt mehrere Vorgehensweisen zum Beheben des Fehlers:</span><span class="sxs-lookup"><span data-stu-id="4c397-161">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="4c397-162">Lassen Sie die Datenbank von Entity Framework automatisch löschen und basierend auf dem neuen Modellklassenschema neu erstellen.</span><span class="sxs-lookup"><span data-stu-id="4c397-162">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="4c397-163">Dieser Ansatz ist sehr praktisch, bei der aktiven Entwicklung auf eine Testdatenbank; Sie können das Modell und Datenbank-Schema schnell zusammen weiterentwickelt.</span><span class="sxs-lookup"><span data-stu-id="4c397-163">This approach is very convenient when doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="4c397-164">Der Nachteil ist jedoch, dass Sie in der Datenbank vorhandene Daten verloren gehen – damit Sie *nicht* dieser Ansatz in einer Produktionsdatenbank verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="4c397-164">The downside, though, is that you lose existing data in the database — so you *don't* want to use this approach on a production database!</span></span> <span data-ttu-id="4c397-165">Automatisch Ausgangswert für eine Datenbank mit Testdaten mit einem Initialisierer ist häufig eine produktive Weg zur Entwicklung einer Anwendung.</span><span class="sxs-lookup"><span data-stu-id="4c397-165">Using an initializer to automatically seed a database with test data is often a productive way to develope an application.</span></span> <span data-ttu-id="4c397-166">Weitere Informationen zu Entity Framework Database Initialisierer, finden Sie unter der Tom Dykstra [ASP.NET MVC/Entity Framework-Lernprogramm](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="4c397-166">For more information on Entity Framework database initializers, see Tom Dykstra's [ASP.NET MVC/Entity Framework tutorial](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>
2. <span data-ttu-id="4c397-167">Ändern Sie das Schema der vorhandenen Datenbank explizit so, dass es mit den Modellklassen übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="4c397-167">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="4c397-168">Der Vorteil dieses Ansatzes ist, dass Sie Ihre Daten behalten.</span><span class="sxs-lookup"><span data-stu-id="4c397-168">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="4c397-169">Sie können diese Änderung entweder manuell oder durch Erstellen eines Änderungsskripts für die Datenbank vornehmen.</span><span class="sxs-lookup"><span data-stu-id="4c397-169">You can make this change either manually or by creating a database change script.</span></span>
3. <span data-ttu-id="4c397-170">Verwenden Sie Code First-Migrationen, um das Datenbankschema zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="4c397-170">Use Code First Migrations to update the database schema.</span></span>


<span data-ttu-id="4c397-171">Für dieses Tutorial verwenden wir Code First-Migrationen.</span><span class="sxs-lookup"><span data-stu-id="4c397-171">For this tutorial, we'll use Code First Migrations.</span></span>

<span data-ttu-id="4c397-172">Aktualisieren Sie Seed-Methode, sodass sie einen Wert für die neue Spalte bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="4c397-172">Update the Seed method so that it provides a value for the new column.</span></span> <span data-ttu-id="4c397-173">Öffnen Sie Migrations\Configuration.cs-Datei und fügen Sie ein Bewertungsfeld auf jedes Movie-Objekt.</span><span class="sxs-lookup"><span data-stu-id="4c397-173">Open Migrations\Configuration.cs file and add a Rating field to each Movie object.</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample7.cs?highlight=6)]

<span data-ttu-id="4c397-174">Erstellen Sie die Projektmappe, und öffnen Sie die **Package Manager Console** Fenster, und geben Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="4c397-174">Build the solution, and then open the **Package Manager Console** window and enter the following command:</span></span>

`add-migration AddRatingMig`

<span data-ttu-id="4c397-175">Die `add-migration` Befehl weist das Framework Migration, überprüfen das aktuelle Film-Modell mit dem aktuellen Film DB Schema und den erforderlichen Code zum Migrieren der Datenbank in das neue Modell zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="4c397-175">The `add-migration` command tells the migration framework to examine the current movie model with the current movie DB schema and create the necessary code to migrate the DB to the new model.</span></span> <span data-ttu-id="4c397-176">Die AddRatingMig ist willkürlich und wird verwendet, um die Migrationsdatei angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="4c397-176">The AddRatingMig is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="4c397-177">Es ist hilfreich, einen aussagekräftigen Namen für den Schritt der Migration zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="4c397-177">It's helpful to use a meaningful name for the migration step.</span></span>

<span data-ttu-id="4c397-178">Wenn dieser Befehl abgeschlossen ist, handelt es sich bei Visual Studio öffnet die Klassendatei, die die neue definiert `DbMIgration` abgeleitete Klasse, und klicken Sie in der `Up` Methode sehen Sie den Code, der die neue Spalte erstellt.</span><span class="sxs-lookup"><span data-stu-id="4c397-178">When this command finishes, Visual Studio opens the class file that defines the new `DbMIgration` derived class, and in the `Up` method you can see the code that creates the new column.</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample8.cs)]

<span data-ttu-id="4c397-179">Erstellen Sie die Projektmappe, und geben Sie dann den Befehl "Update-Database" in der **Package Manager Console** Fenster.</span><span class="sxs-lookup"><span data-stu-id="4c397-179">Build the solution, and then enter the "update-database" command in the **Package Manager Console** window.</span></span>

<span data-ttu-id="4c397-180">Die folgende Abbildung zeigt die Ausgabe in die **Package Manager Console** Fenster (der Datumsstempel vorangestellt wird AddRatingMig unterscheiden.)</span><span class="sxs-lookup"><span data-stu-id="4c397-180">The following image shows the output in the **Package Manager Console** window (The date stamp prepending AddRatingMig will be different.)</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image12.png)

<span data-ttu-id="4c397-181">Führen Sie die Anwendung erneut aus, und navigieren Sie zu der URL /Movies.</span><span class="sxs-lookup"><span data-stu-id="4c397-181">Re-run the application and navigate to the /Movies URL.</span></span> <span data-ttu-id="4c397-182">Sehen Sie die neue Rating-Felds an.</span><span class="sxs-lookup"><span data-stu-id="4c397-182">You can see the new Rating field.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image13.png)

<span data-ttu-id="4c397-183">Klicken Sie auf die **neu erstellen** Link, um einen neuen Film hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="4c397-183">Click the **Create New** link to add a new movie.</span></span> <span data-ttu-id="4c397-184">Beachten Sie, dass Sie eine Bewertung hinzufügen können.</span><span class="sxs-lookup"><span data-stu-id="4c397-184">Note that you can add a rating.</span></span>

![7_CreateRioII](adding-a-new-field-to-the-movie-model-and-table/_static/image14.png)

<span data-ttu-id="4c397-186">Klicken Sie auf **Erstellen**.</span><span class="sxs-lookup"><span data-stu-id="4c397-186">Click **Create**.</span></span> <span data-ttu-id="4c397-187">Der neue Film, einschließlich der Bewertung wird jetzt im Filme auflisten:</span><span class="sxs-lookup"><span data-stu-id="4c397-187">The new movie, including the rating, now shows up in the movies listing:</span></span>

![7_ourNewMovie_SM](adding-a-new-field-to-the-movie-model-and-table/_static/image15.png)

<span data-ttu-id="4c397-189">Sie sollten auch hinzufügen, die `Rating` Feld bearbeiten, Details und SearchIndex Vorlagen anzeigen.</span><span class="sxs-lookup"><span data-stu-id="4c397-189">You should also add the `Rating` field to the Edit, Details and SearchIndex view templates.</span></span>

<span data-ttu-id="4c397-190">Konnte, geben Sie den Befehl "Update-Database" in der **Package Manager Console** Fenster erneut und keine Änderungen würde vorgenommen werden, da das Modell mit dem Schema übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="4c397-190">You could enter the "update-database" command in the **Package Manager Console** window again and no changes would be made, because the schema matches the model.</span></span>

<span data-ttu-id="4c397-191">In diesem Abschnitt haben gesehen, wie Sie Modellobjekte ändern und die Datenbank mit den Änderungen synchron bleiben.</span><span class="sxs-lookup"><span data-stu-id="4c397-191">In this section you saw how you can modify model objects and keep the database in sync with the changes.</span></span> <span data-ttu-id="4c397-192">Außerdem haben Sie gelernt, eine Möglichkeit, eine neu erstellte Datenbank mit Beispieldaten aufzufüllen, damit Sie die Szenarien ausprobieren können.</span><span class="sxs-lookup"><span data-stu-id="4c397-192">You also learned a way to populate a newly created database with sample data so you can try out scenarios.</span></span> <span data-ttu-id="4c397-193">Als Nächstes sehen wir uns wie können Sie der Modellklassen umfangreichere Validierungslogik hinzu, und aktivieren einige Geschäftsregeln erzwungen werden.</span><span class="sxs-lookup"><span data-stu-id="4c397-193">Next, let's look at how you can add richer validation logic to the model classes and enable some business rules to be enforced.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="4c397-194">[Zurück](examining-the-edit-methods-and-edit-view.md)
[Weiter](adding-validation-to-the-model.md)</span><span class="sxs-lookup"><span data-stu-id="4c397-194">[Previous](examining-the-edit-methods-and-edit-view.md)
[Next](adding-validation-to-the-model.md)</span></span>
