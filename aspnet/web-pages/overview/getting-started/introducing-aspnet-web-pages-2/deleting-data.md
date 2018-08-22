---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
title: 'Einführung in ASP.NET Web Pages: Löschen von DatenbankenDaten | Microsoft-Dokumentation'
author: tfitzmac
description: In diesem Tutorial erfahren Sie, wie Sie einen Eintrag für die einzelnen Datenbank zu löschen. Es wird davon ausgegangen, dass Sie der Reihe über das Aktualisieren von Datenbank-Daten in ASP.NET Web-PA abgeschlossen haben...
ms.author: riande
ms.date: 01/02/2018
ms.assetid: 75b5c1cf-84bd-434f-8a86-85c568eb5b09
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
msc.type: authoredcontent
ms.openlocfilehash: 3b759a5c88b066640005c823ce0cc3cc3ac89bc2
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826932"
---
<a name="introducing-aspnet-web-pages---deleting-database-data"></a><span data-ttu-id="929a1-104">Einführung in ASP.NET Web Pages: Löschen von DatenbankenDaten</span><span class="sxs-lookup"><span data-stu-id="929a1-104">Introducing ASP.NET Web Pages - Deleting Database Data</span></span>
====================
<span data-ttu-id="929a1-105">durch [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="929a1-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="929a1-106">In diesem Tutorial erfahren Sie, wie Sie einen Eintrag für die einzelnen Datenbank zu löschen.</span><span class="sxs-lookup"><span data-stu-id="929a1-106">This tutorial shows you how to delete an individual database entry.</span></span> <span data-ttu-id="929a1-107">Es wird vorausgesetzt, Sie haben die Reihe über [Aktualisieren von Daten in ASP.NET Web Pages](updating-data.md).</span><span class="sxs-lookup"><span data-stu-id="929a1-107">It assumes you have completed the series through [Updating Database Data in ASP.NET Web Pages](updating-data.md).</span></span>
> 
> <span data-ttu-id="929a1-108">Sie lernen Folgendes:</span><span class="sxs-lookup"><span data-stu-id="929a1-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="929a1-109">So wählen Sie einen einzelnen Datensatz in einer Auflistung der Datensätze.</span><span class="sxs-lookup"><span data-stu-id="929a1-109">How to select an individual record from a listing of records.</span></span>
> - <span data-ttu-id="929a1-110">Vorgehensweise: Löschen ein einzelnes Datensatzes aus einer Datenbank.</span><span class="sxs-lookup"><span data-stu-id="929a1-110">How to delete a single record from a database.</span></span>
> - <span data-ttu-id="929a1-111">So überprüfen Sie, dass eine bestimmte Schaltfläche in einem Formular geklickt wurde.</span><span class="sxs-lookup"><span data-stu-id="929a1-111">How to check that a specific button was clicked in a form.</span></span>
>   
> 
> <span data-ttu-id="929a1-112">Features/Technologien erläutert:</span><span class="sxs-lookup"><span data-stu-id="929a1-112">Features/technologies discussed:</span></span>
> 
> - <span data-ttu-id="929a1-113">Die `WebGrid` Helper.</span><span class="sxs-lookup"><span data-stu-id="929a1-113">The `WebGrid` helper.</span></span>
> - <span data-ttu-id="929a1-114">Die SQL-Anweisung `Delete` Befehl.</span><span class="sxs-lookup"><span data-stu-id="929a1-114">The SQL `Delete` command.</span></span>
> - <span data-ttu-id="929a1-115">Die `Database.Execute` Methode für die Ausführung einer SQL `Delete` Befehl.</span><span class="sxs-lookup"><span data-stu-id="929a1-115">The `Database.Execute` method to run a SQL `Delete` command.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="929a1-116">Sie lernen Folgendes</span><span class="sxs-lookup"><span data-stu-id="929a1-116">What You'll Build</span></span>

<span data-ttu-id="929a1-117">Im vorherigen Tutorial haben Sie gelernt, wie einen vorhandener Datenbankdatensatz zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="929a1-117">In the previous tutorial, you learned how to update an existing database record.</span></span> <span data-ttu-id="929a1-118">Dieses Lernprogramm ist ähnlich, außer dass statt den Datensatz aktualisieren Sie sie löschen müssen.</span><span class="sxs-lookup"><span data-stu-id="929a1-118">This tutorial is similar, except that instead of updating the record, you'll delete it.</span></span> <span data-ttu-id="929a1-119">Die Prozesse sind nahezu identisch, außer dass löschen einfacher ist, damit in diesem Tutorial kurz werden.</span><span class="sxs-lookup"><span data-stu-id="929a1-119">The processes are much the same, except that deleting is simpler, so this tutorial will be short.</span></span>

<span data-ttu-id="929a1-120">In der *Filme* Seite, die Sie aktualisieren müssen die `WebGrid` Helper, sodass, dass die It anzeigt eine **löschen** neben jeder Film zur Ergänzung der **bearbeiten** Verknüpfung, die Sie zuvor hinzugefügt haben.</span><span class="sxs-lookup"><span data-stu-id="929a1-120">In the *Movies* page, you'll update the `WebGrid` helper so that it displays a **Delete** link next to each movie to accompany the **Edit** link you added earlier.</span></span>

![Seite "Movies" mit einem Link "löschen" für jeden Film](deleting-data/_static/image1.png)

<span data-ttu-id="929a1-122">Wie bei der Bearbeitung beim Klicken auf die **löschen** Link gelangen Sie zu einer anderen Seite befindet, in dem die Filminformationen bereits in einem Formular:</span><span class="sxs-lookup"><span data-stu-id="929a1-122">As with editing, when you click the **Delete** link, it takes you to a different page, where the movie information is already in a form:</span></span>

![Löschen Sie Seite "Movie" mit einem Film angezeigt](deleting-data/_static/image2.png)

<span data-ttu-id="929a1-124">Sie können dann die Schaltfläche, um den Datensatz dauerhaft zu löschen klicken.</span><span class="sxs-lookup"><span data-stu-id="929a1-124">You can then click the button to delete the record permanently.</span></span>

## <a name="adding-a-delete-link-to-the-movie-listing"></a><span data-ttu-id="929a1-125">Die Movie-Auflistung hinzugefügt einen Link "löschen"</span><span class="sxs-lookup"><span data-stu-id="929a1-125">Adding a Delete Link to the Movie Listing</span></span>

<span data-ttu-id="929a1-126">Beginnen Sie durch das Hinzufügen einer **löschen** Verknüpfen mit der `WebGrid` Helper.</span><span class="sxs-lookup"><span data-stu-id="929a1-126">You'll start by adding a **Delete** link to the `WebGrid` helper.</span></span> <span data-ttu-id="929a1-127">Dieser Link ist ähnlich wie die **bearbeiten** Verknüpfung, die Sie in einem vorherigen Tutorial hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="929a1-127">This link is similar to the **Edit** link you added in a previous tutorial.</span></span>

<span data-ttu-id="929a1-128">Öffnen der *Movies.cshtml* Datei.</span><span class="sxs-lookup"><span data-stu-id="929a1-128">Open the *Movies.cshtml* file.</span></span>

<span data-ttu-id="929a1-129">Ändern der `WebGrid` Markup im Hauptteil der Seite durch Hinzufügen einer Spalte.</span><span class="sxs-lookup"><span data-stu-id="929a1-129">Change the `WebGrid` markup in the body of the page by adding a column.</span></span> <span data-ttu-id="929a1-130">Dies ist die geänderte Markup:</span><span class="sxs-lookup"><span data-stu-id="929a1-130">Here's the modified markup:</span></span>

[!code-html[Main](deleting-data/samples/sample1.html?highlight=9-10)]

<span data-ttu-id="929a1-131">Die neue Spalte, ist diese:</span><span class="sxs-lookup"><span data-stu-id="929a1-131">The new column is this one:</span></span>

[!code-html[Main](deleting-data/samples/sample2.html)]

<span data-ttu-id="929a1-132">Die Möglichkeit, die das Raster konfiguriert ist, die **bearbeiten** Spalte ganz links im Raster ist und die **löschen** Spalte ganz rechts ist.</span><span class="sxs-lookup"><span data-stu-id="929a1-132">The way the grid is configured, the **Edit** column is leftmost in the grid and the **Delete** column is rightmost.</span></span> <span data-ttu-id="929a1-133">(Es ist ein Komma nach dem die `Year` Spalte, für den Fall, dass Sie nicht feststellen.) Es ist nichts Besonderes, diese Linkspalten wo, und Sie könnten einfach platzieren Sie sie nebeneinander.</span><span class="sxs-lookup"><span data-stu-id="929a1-133">(There's a comma after the `Year` column now, in case you didn't notice that.) There's nothing special about where these link columns go, and you could as easily put them next to each other.</span></span> <span data-ttu-id="929a1-134">In diesem Fall sind separate, um sie vermischt wird schwieriger zu machen.</span><span class="sxs-lookup"><span data-stu-id="929a1-134">In this case, they're separate to make them harder to get mixed up.</span></span>

![Seite "Movies" mit Links "Bearbeiten" und "Details markiert, um anzugeben, dass sie sich nicht nebeneinander befinden](deleting-data/_static/image3.png)

<span data-ttu-id="929a1-136">Die neue Spalte zeigt eine Verknüpfung (`<a>` Element), dessen Text lautet "Löschen".</span><span class="sxs-lookup"><span data-stu-id="929a1-136">The new column shows a link (`<a>` element) whose text says "Delete".</span></span> <span data-ttu-id="929a1-137">Das Ziel des Links (die `href` Attribut) wird der Code, der letztendlich löst z. B. diese URL, mit der `id` Wert für jeden Film:</span><span class="sxs-lookup"><span data-stu-id="929a1-137">The target of the link (its `href` attribute) is code that ultimately resolves to something like this URL, with the `id` value different for each movie:</span></span>

[!code-css[Main](deleting-data/samples/sample3.css)]

<span data-ttu-id="929a1-138">Dieser Link wird aufgerufen, eine Seite namens *DeleteMovie* , und übergeben sie die ID des Films, die Sie ausgewählt haben.</span><span class="sxs-lookup"><span data-stu-id="929a1-138">This link will invoke a page named *DeleteMovie* and pass it the ID of the movie you've selected.</span></span>

<span data-ttu-id="929a1-139">In diesem Tutorial Gehe nicht ausführlich, wie dieser Link erstellt wird, da sie fast identisch mit ist der **bearbeiten** Link aus dem vorherigen Lernprogramm ([Aktualisieren von Daten in ASP.NET Web Pages](updating-data.md)).</span><span class="sxs-lookup"><span data-stu-id="929a1-139">This tutorial won't go into detail about how this link is constructed, because it's almost identical to the **Edit** link from the previous tutorial ([Updating Database Data in ASP.NET Web Pages](updating-data.md)).</span></span>

## <a name="creating-the-delete-page"></a><span data-ttu-id="929a1-140">Erstellen die Seite "löschen"</span><span class="sxs-lookup"><span data-stu-id="929a1-140">Creating the Delete Page</span></span>

<span data-ttu-id="929a1-141">Nun können Sie die Seite erstellen, die das Ziel für die **löschen** -Link in das Raster.</span><span class="sxs-lookup"><span data-stu-id="929a1-141">Now you can create the page that will be the target for the **Delete** link in the grid.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="929a1-142">**Wichtige** die Technik zunächst auswählen einen Datensatz zu löschen, und klicken Sie dann mit einer separaten Seite und einer Schaltfläche Überprüfen, ob den Vorgang ist äußerst wichtig, für die Sicherheit.</span><span class="sxs-lookup"><span data-stu-id="929a1-142">**Important** The technique of first selecting a record to delete and then using a separate page and button to confirm the process is extremely important for security.</span></span> <span data-ttu-id="929a1-143">Wie Sie in vorherigen Tutorials gelesen haben, sodass *alle* Art von Änderung an Ihrer Website sollte *immer* erfolgen mit einem Formular &mdash; , also mithilfe eines HTTP POST-Vorgangs.</span><span class="sxs-lookup"><span data-stu-id="929a1-143">As you've read in previous tutorials, making *any* sort of change to your website should *always* be done using a form &mdash; that is, using an HTTP POST operation.</span></span> <span data-ttu-id="929a1-144">Wenn Sie es wurde ermöglicht, ändern Sie die Website nur über einen Link (die mithilfe einen GET-Vorgang), könnten Personen einfache Anforderungen an Ihre Website vornehmen und löschen Sie Ihre Daten.</span><span class="sxs-lookup"><span data-stu-id="929a1-144">If you made it possible to change the site just by clicking a link (that is, using a GET operation), people could make simple requests to your site and delete your data.</span></span> <span data-ttu-id="929a1-145">Selbst ein Suchmaschinen-Crawler, der Ihre Website Indizierung ist konnte Daten nur über Links versehentlich gelöscht werden.</span><span class="sxs-lookup"><span data-stu-id="929a1-145">Even a search-engine crawler that's indexing your site could inadvertently delete data just by following links.</span></span>
> 
> <span data-ttu-id="929a1-146">Wenn Ihre app auf Personen, die einen Datensatz zu ändern kann, müssen Sie den Datensatz für dem Benutzer vorhanden, für die Bearbeitung trotzdem.</span><span class="sxs-lookup"><span data-stu-id="929a1-146">When your app lets people change a record, you have to present the record to the user for editing anyway.</span></span> <span data-ttu-id="929a1-147">Aber Sie könnten versucht sein, überspringen Sie diesen Schritt für das Löschen eines Datensatzes.</span><span class="sxs-lookup"><span data-stu-id="929a1-147">But you might be tempted to skip this step for deleting a record.</span></span> <span data-ttu-id="929a1-148">Überspringen Sie diesen Schritt, jedoch nicht.</span><span class="sxs-lookup"><span data-stu-id="929a1-148">Don't skip that step, though.</span></span> <span data-ttu-id="929a1-149">(Es ist auch hilfreich für Benutzer finden Sie unter dem Datensatz, und bestätigen Sie, dass sie den Datensatz löschen, den sie vorgesehen.)</span><span class="sxs-lookup"><span data-stu-id="929a1-149">(It's also helpful for users to see the record and confirm that they're deleting the record that they intended.)</span></span>
> 
> <span data-ttu-id="929a1-150">In einer nachfolgenden Tutorial Menge sehen Sie, wie Anmeldename-Funktionalität hinzufügen, damit ein Benutzer sich anmelden, bevor das Löschen eines Datensatzes müssten.</span><span class="sxs-lookup"><span data-stu-id="929a1-150">In a subsequent tutorial set, you'll see how to add login functionality so a user would have to log in before deleting a record.</span></span>


<span data-ttu-id="929a1-151">Erstellen Sie eine Seite mit dem Namen *DeleteMovie.cshtml* , und Ersetzen Sie die neuerungen in der Datei mit folgendem Markup:</span><span class="sxs-lookup"><span data-stu-id="929a1-151">Create a page named *DeleteMovie.cshtml* and replace what's in the file with the following markup:</span></span>

[!code-cshtml[Main](deleting-data/samples/sample4.cshtml)]

<span data-ttu-id="929a1-152">Dieses Markup ist, wie die *EditMovie* -Seiten, außer dass anstelle von Textfeldern (`<input type="text">`), das Markup enthält `<span>` Elemente.</span><span class="sxs-lookup"><span data-stu-id="929a1-152">This markup is like the *EditMovie* pages, except that instead of using text boxes (`<input type="text">`), the markup includes `<span>` elements.</span></span> <span data-ttu-id="929a1-153">Es gibt hier nichts zu bearbeiten.</span><span class="sxs-lookup"><span data-stu-id="929a1-153">There's nothing here to edit.</span></span> <span data-ttu-id="929a1-154">Müssen Sie lediglich die Movie-Details anzuzeigen, damit die Benutzer können Sie sicher, dass sie den richtigen Film löschen.</span><span class="sxs-lookup"><span data-stu-id="929a1-154">All you have to do is display the movie details so that users can make sure that they're deleting the right movie.</span></span>

<span data-ttu-id="929a1-155">Das Markup enthält bereits einen Link, der der Benutzer auf die Movie-Angebotsseite zurückzukehren.</span><span class="sxs-lookup"><span data-stu-id="929a1-155">The markup already contains a link that lets the user return to the movie listing page.</span></span>

<span data-ttu-id="929a1-156">Wie in der *EditMovie* Seite, die den ausgewählten Film-ID wird in einem ausgeblendeten Feld gespeichert.</span><span class="sxs-lookup"><span data-stu-id="929a1-156">As in the *EditMovie* page, the ID of the selected movie is stored in a hidden field.</span></span> <span data-ttu-id="929a1-157">(Es wird auf der Seite im vornherein als Wert einer Abfragezeichenfolge übergeben.) Es gibt eine `Html.ValidationSummary` Aufruf, der Validierungsfehler angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="929a1-157">(It's passed into the page in the first place as a query string value.) There's an `Html.ValidationSummary` call that will display validation errors.</span></span> <span data-ttu-id="929a1-158">In diesem Fall möglicherweise der Fehler, dass keine Film-ID an die Seite übergeben wurde oder die Film-ID ungültig ist.</span><span class="sxs-lookup"><span data-stu-id="929a1-158">In this case, the error might be that no movie ID was passed to the page or that the movie ID is invalid.</span></span> <span data-ttu-id="929a1-159">Diese Situation kann eintreten, wenn ein Benutzer auf dieser Seite ausgeführt wurde, ohne zuvor einen Film in den *Filme* Seite.</span><span class="sxs-lookup"><span data-stu-id="929a1-159">This situation could occur if someone ran this page without first selecting a movie in the *Movies* page.</span></span>

<span data-ttu-id="929a1-160">Die Beschriftung der Schaltfläche ist **löschen Film**, und dessen Namensattribut nastaven NA hodnotu `buttonDelete`.</span><span class="sxs-lookup"><span data-stu-id="929a1-160">The button caption is **Delete Movie**, and its name attribute is set to `buttonDelete`.</span></span> <span data-ttu-id="929a1-161">Die `name` Attribut wird im Code verwendet werden, auf um die Schaltfläche zu identifizieren, die das Formular übermittelt.</span><span class="sxs-lookup"><span data-stu-id="929a1-161">The `name` attribute will be used in the code to identify the button that submitted the form.</span></span>

<span data-ttu-id="929a1-162">Sie müssen Code schreiben, um die (1) die Filmdetails lesen, wenn die Seite zum ersten Mal angezeigt wird, und löschen (2) tatsächlich den Film aus, klickt der Benutzer die Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="929a1-162">You'll have to write code to 1) read the movie details when the page is first displayed and 2) actually delete the movie when the user clicks the button.</span></span>

## <a name="adding-code-to-read-a-single-movie"></a><span data-ttu-id="929a1-163">Hinzufügen von Code zum Lesen eines einzelnen Films</span><span class="sxs-lookup"><span data-stu-id="929a1-163">Adding Code to Read a Single Movie</span></span>

<span data-ttu-id="929a1-164">Am oberen Rand der *DeleteMovie.cshtml* Seite, fügen Sie den folgenden Codeblock:</span><span class="sxs-lookup"><span data-stu-id="929a1-164">At the top of the *DeleteMovie.cshtml* page, add the following code block:</span></span>

[!code-cshtml[Main](deleting-data/samples/sample5.cshtml)]

<span data-ttu-id="929a1-165">Dieses Markup immer das gleiche wie der entsprechende Code in die *EditMovie* Seite.</span><span class="sxs-lookup"><span data-stu-id="929a1-165">This markup is the same as the corresponding code in the *EditMovie* page.</span></span> <span data-ttu-id="929a1-166">Es ruft die Film-ID aus der Abfragezeichenfolge ab und verwendet die ID, um einen Datensatz aus der Datenbank zu lesen.</span><span class="sxs-lookup"><span data-stu-id="929a1-166">It gets the movie ID out of the query string and uses the ID to read a record from the database.</span></span> <span data-ttu-id="929a1-167">Der Code enthält den Test zur Überprüfung (`IsInt()` und `row != null`) sicherstellen, dass die Film-ID übergeben wird, auf der Seite gültig ist.</span><span class="sxs-lookup"><span data-stu-id="929a1-167">The code includes the validation test (`IsInt()` and `row != null`) to make sure that the movie ID being passed to the page is valid.</span></span>

<span data-ttu-id="929a1-168">Denken Sie daran, dass dieser Code sollte nur zum ersten Mal ausführen, die die Seite ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="929a1-168">Remember that this code should only run the first time the page runs.</span></span> <span data-ttu-id="929a1-169">Sie möchten die Movie-Datensatz aus der Datenbank erneut zu lesen, klickt der Benutzer die **löschen Film** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="929a1-169">You don't want to re-read the movie record from the database when the user clicks the **Delete Movie** button.</span></span> <span data-ttu-id="929a1-170">Aus diesem Grund code zum Lesen des Films befindet sich innerhalb eines Tests, die besagt, `if(!IsPost)` &mdash; , also *ist die Anforderung keinen Post-Vorgang (Formularübermittlung)*.</span><span class="sxs-lookup"><span data-stu-id="929a1-170">Therefore, code to read the movie is inside a test that says `if(!IsPost)` &mdash; that is, *if the request is not a post operation (form submission)*.</span></span>

## <a name="adding-code-to-delete-the-selected-movie"></a><span data-ttu-id="929a1-171">Hinzufügen von Code, um den ausgewählten Film zu löschen.</span><span class="sxs-lookup"><span data-stu-id="929a1-171">Adding Code to Delete the Selected Movie</span></span>

<span data-ttu-id="929a1-172">Um den Film löschen, wenn der Benutzer auf die Schaltfläche klickt, fügen Sie den folgenden Code innerhalb der schließenden Klammer der der `@` blockieren:</span><span class="sxs-lookup"><span data-stu-id="929a1-172">To delete the movie when the user clicks the button, add the following code just inside the closing brace of the `@` block:</span></span>

[!code-csharp[Main](deleting-data/samples/sample6.cs)]

<span data-ttu-id="929a1-173">Dieser Code ist mit dem Code zum Aktualisieren eines vorhandenen Datensatzes ähnlich, aber einfacher.</span><span class="sxs-lookup"><span data-stu-id="929a1-173">This code is similar to the code for updating an existing record, but simpler.</span></span> <span data-ttu-id="929a1-174">Der Code führt im Grunde eine SQL `Delete` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="929a1-174">The code basically runs a SQL `Delete` statement.</span></span>

 <span data-ttu-id="929a1-175">Wie in der *EditMovie* Seite der Code befindet sich in einem `if(IsPost)` Block.</span><span class="sxs-lookup"><span data-stu-id="929a1-175">As in the *EditMovie* page, the code is in an `if(IsPost)` block.</span></span> <span data-ttu-id="929a1-176">Dieses Mal die `if()` Bedingung ist ein wenig komplizierter:</span><span class="sxs-lookup"><span data-stu-id="929a1-176">This time, the `if()` condition is a little more complicated:</span></span> 

[!code-csharp[Main](deleting-data/samples/sample7.cs)]

<span data-ttu-id="929a1-177">Es gibt hier zwei Bedingungen.</span><span class="sxs-lookup"><span data-stu-id="929a1-177">There are two conditions here.</span></span> <span data-ttu-id="929a1-178">Die erste ist, dass die Seite übermittelt wird, ist, wie Sie vor dem gesehen haben &mdash; `if(IsPost)`.</span><span class="sxs-lookup"><span data-stu-id="929a1-178">The first is that the page is being submitted, as you've seen before &mdash; `if(IsPost)`.</span></span>

<span data-ttu-id="929a1-179">Die zweite Bedingung ist `!Request["buttonDelete"].IsEmpty()`, was bedeutet, dass die Anforderung ein Objekt namens hat `buttonDelete`.</span><span class="sxs-lookup"><span data-stu-id="929a1-179">The second condition is `!Request["buttonDelete"].IsEmpty()`, meaning that the request has an object named `buttonDelete`.</span></span> <span data-ttu-id="929a1-180">Zugegeben, ist es ein indirektes Verfahren dar, testen, auf welche Schaltfläche auf das Formular übermittelt.</span><span class="sxs-lookup"><span data-stu-id="929a1-180">Admittedly, it's an indirect way of testing which button submitted the form.</span></span> <span data-ttu-id="929a1-181">Wenn ein Formular mehrere Submit-Schaltflächen enthält, wird nur der Name der Schaltfläche, auf die geklickt wurde, in der Anforderung angezeigt.</span><span class="sxs-lookup"><span data-stu-id="929a1-181">If a form contains multiple submit buttons, only the name of the button that was clicked appears in the request.</span></span> <span data-ttu-id="929a1-182">Aus diesem Grund, logisch, wenn der Name einer bestimmten Schaltfläche angezeigt, in der Anforderung wird &mdash; oder wie im Code angegeben werden, wenn diese Schaltfläche nicht leer ist &mdash; , das die Schaltfläche, die das Formular übermittelt wird.</span><span class="sxs-lookup"><span data-stu-id="929a1-182">Therefore, logically, if the name of a particular button appears in the request &mdash; or as stated in the code, if that button isn't empty &mdash; that's the button that submitted the form.</span></span>

<span data-ttu-id="929a1-183">Die `&&` Operator bedeutet, dass "und" (logisches AND).</span><span class="sxs-lookup"><span data-stu-id="929a1-183">The `&&` operator means "and" (logical AND).</span></span> <span data-ttu-id="929a1-184">Aus diesem Grund für die gesamte `if` Bedingung...</span><span class="sxs-lookup"><span data-stu-id="929a1-184">Therefore the entire `if` condition is ...</span></span>

<span data-ttu-id="929a1-185">*Diese Anforderung ist ein Beitrag (keine Anforderung zum ersten Mal)*</span><span class="sxs-lookup"><span data-stu-id="929a1-185">*This request is a post (not a first-time request)*</span></span>  
  
 <span data-ttu-id="929a1-186">UND</span><span class="sxs-lookup"><span data-stu-id="929a1-186">AND</span></span>  
  
<span data-ttu-id="929a1-187">*Die* `buttonDelete` *die Schaltfläche, die das Formular übermittelt wurde.*</span><span class="sxs-lookup"><span data-stu-id="929a1-187">*The* `buttonDelete`*button was the button that submitted the form.*</span></span>

<span data-ttu-id="929a1-188">Dieses Formular (genauer gesagt auf dieser Seite) enthält nur ein Optionsfeld, also die zusätzliche Tests für `buttonDelete` ist technisch nicht erforderlich.</span><span class="sxs-lookup"><span data-stu-id="929a1-188">This form (in fact, this page) contains only one button, so the additional test for `buttonDelete` is technically not required.</span></span> <span data-ttu-id="929a1-189">Dennoch können Sie einen Vorgang ausführen, der Daten dauerhaft entfernt.</span><span class="sxs-lookup"><span data-stu-id="929a1-189">Still, you're about to perform an operation that will permanently remove data.</span></span> <span data-ttu-id="929a1-190">Sie möchten also so sicher wie möglich sein, dass Sie den Vorgang durchführen, nur, wenn der Benutzer ausdrücklich angefordert hat.</span><span class="sxs-lookup"><span data-stu-id="929a1-190">So you want to be as sure as possible that you're performing the operation only when the user has explicitly requested it.</span></span> <span data-ttu-id="929a1-191">Nehmen wir beispielsweise an, dass Sie diese Seite später erweitert, und weitere Schaltflächen hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="929a1-191">For example, suppose that you expanded this page later and added other buttons to it.</span></span> <span data-ttu-id="929a1-192">Der Code, der den Film löscht wird auch dann ausgeführt, wenn nur die `buttonDelete` Schaltfläche geklickt wurde.</span><span class="sxs-lookup"><span data-stu-id="929a1-192">Even then, the code that deletes the movie will run only if the `buttonDelete` button was clicked.</span></span>

<span data-ttu-id="929a1-193">Wie in der *EditMovie* Seite, Sie rufen Sie die ID aus dem ausgeblendeten Feld und führen Sie den SQL-Befehl.</span><span class="sxs-lookup"><span data-stu-id="929a1-193">As in the *EditMovie* page, you get the ID from the hidden field and then run the SQL command.</span></span> <span data-ttu-id="929a1-194">Die Syntax für die `Delete` -Anweisung ist:</span><span class="sxs-lookup"><span data-stu-id="929a1-194">The syntax for the `Delete` statement is:</span></span>

`DELETE FROM table WHERE ID = value`

<span data-ttu-id="929a1-195">Es ist wichtig, enthalten die `WHERE` -Klausel und die-ID.</span><span class="sxs-lookup"><span data-stu-id="929a1-195">It's vital to include the `WHERE` clause and the ID.</span></span> <span data-ttu-id="929a1-196">Wenn Sie der WHERE-Klausel auslassen *alle Datensätze in der Tabelle gelöscht werden*.</span><span class="sxs-lookup"><span data-stu-id="929a1-196">If you leave out the WHERE clause, *all the records in the table will be deleted*.</span></span> <span data-ttu-id="929a1-197">Wie Sie gesehen haben, übergeben Sie den ID-Wert an den SQL-Befehl mit einem Platzhalter.</span><span class="sxs-lookup"><span data-stu-id="929a1-197">As you have seen, you pass the ID value to the SQL command by using a placeholder.</span></span>

## <a name="testing-the-movie-delete-process"></a><span data-ttu-id="929a1-198">Testen des Prozesses der Film löschen</span><span class="sxs-lookup"><span data-stu-id="929a1-198">Testing the Movie Delete Process</span></span>

<span data-ttu-id="929a1-199">Jetzt können Sie testen.</span><span class="sxs-lookup"><span data-stu-id="929a1-199">Now you can test.</span></span> <span data-ttu-id="929a1-200">Führen Sie die *Filme* Seite, und klicken Sie auf **löschen** neben einen Film.</span><span class="sxs-lookup"><span data-stu-id="929a1-200">Run the *Movies* page, and click **Delete** next to a movie.</span></span> <span data-ttu-id="929a1-201">Wenn die *DeleteMovie* klicken Sie mit der Seite angezeigt wird, auf **löschen Film**.</span><span class="sxs-lookup"><span data-stu-id="929a1-201">When the *DeleteMovie* page appears, click **Delete Movie**.</span></span>

![Löschen Sie Seite "Movie" mit hervorgehobener Schaltfläche "Löschen Film"](deleting-data/_static/image4.png)

<span data-ttu-id="929a1-203">Wenn Sie die Schaltfläche klicken, wird der Code löscht die Filme und gibt Sie zurück auf die Movie-Auflistung.</span><span class="sxs-lookup"><span data-stu-id="929a1-203">When you click the button, the code deletes the movies and returns to the movie listing.</span></span> <span data-ttu-id="929a1-204">Sie können es für den gelöschten Film suchen und vergewissern Sie sich, dass es gelöscht wird.</span><span class="sxs-lookup"><span data-stu-id="929a1-204">There you can search for the deleted movie and confirm that it's been deleted.</span></span>

## <a name="coming-up-next"></a><span data-ttu-id="929a1-205">Als Nächstes kommen</span><span class="sxs-lookup"><span data-stu-id="929a1-205">Coming Up Next</span></span>

<span data-ttu-id="929a1-206">Im nächste Tutorial erfahren Sie, wie Sie alle Seiten auf Ihrer Website zu geben, eine allgemeine Aussehen und Layout.</span><span class="sxs-lookup"><span data-stu-id="929a1-206">The next tutorial shows you how to give all the pages on your site a common look and layout.</span></span>

## <a name="complete-listing-for-movie-page-updated-with-delete-links"></a><span data-ttu-id="929a1-207">Vollständige Liste für Movie-Seite (Aktualisierung mit Links "löschen")</span><span class="sxs-lookup"><span data-stu-id="929a1-207">Complete Listing for Movie Page (Updated with Delete Links)</span></span>

[!code-cshtml[Main](deleting-data/samples/sample8.cshtml)]

## <a name="complete-listing-for-deletemovie-page"></a><span data-ttu-id="929a1-208">Vollständige Liste für DeleteMovie-Seite</span><span class="sxs-lookup"><span data-stu-id="929a1-208">Complete Listing for DeleteMovie Page</span></span>

[!code-cshtml[Main](deleting-data/samples/sample9.cshtml)]

## <a name="additional-resources"></a><span data-ttu-id="929a1-209">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="929a1-209">Additional Resources</span></span>

- [<span data-ttu-id="929a1-210">Einführung in ASP.NET-Webprogrammierung mithilfe der Razor-Syntax</span><span class="sxs-lookup"><span data-stu-id="929a1-210">Introduction to ASP.NET Web Programming by Using the Razor Syntax</span></span>](../introducing-razor-syntax-c.md)
- <span data-ttu-id="929a1-211">[SQL-DELETE-Anweisung](http://www.w3schools.com/sql/sql_delete.asp) auf der Website W3Schools</span><span class="sxs-lookup"><span data-stu-id="929a1-211">[SQL DELETE Statement](http://www.w3schools.com/sql/sql_delete.asp) on the W3Schools site</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="929a1-212">[Zurück](updating-data.md)
> [Weiter](layouts.md)</span><span class="sxs-lookup"><span data-stu-id="929a1-212">[Previous](updating-data.md)
[Next](layouts.md)</span></span>
