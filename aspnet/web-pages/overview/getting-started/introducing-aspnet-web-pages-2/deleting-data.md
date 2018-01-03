---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
title: "Einführung in ASP.NET Web Pages - Datenbankdaten löschen | Microsoft Docs"
author: tfitzmac
description: "In diesem Lernprogramm wird gezeigt, wie einen einzelne Datenbank-Eintrag zu löschen. Es wird davon ausgegangen, dass Sie der Reihe durch Aktualisieren der Datenbankdaten in ASP.NET Web-PA abgeschlossen haben..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/02/2018
ms.topic: article
ms.assetid: 75b5c1cf-84bd-434f-8a86-85c568eb5b09
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
msc.type: authoredcontent
ms.openlocfilehash: 5bc92b5d40e7a55dcd730d552c71031d913b277e
ms.sourcegitcommit: 281f0c614543a6c3db565ea4655b70fe49b61d84
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/03/2018
---
<a name="introducing-aspnet-web-pages---deleting-database-data"></a><span data-ttu-id="f8807-104">Einführung in ASP.NET Web Pages - Datenbankdaten löschen</span><span class="sxs-lookup"><span data-stu-id="f8807-104">Introducing ASP.NET Web Pages - Deleting Database Data</span></span>
====================
<span data-ttu-id="f8807-105">durch [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="f8807-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="f8807-106">In diesem Lernprogramm wird gezeigt, wie einen einzelne Datenbank-Eintrag zu löschen.</span><span class="sxs-lookup"><span data-stu-id="f8807-106">This tutorial shows you how to delete an individual database entry.</span></span> <span data-ttu-id="f8807-107">Es wird vorausgesetzt, Sie haben die Reihe über abgeschlossen [Aktualisieren der Datenbankdaten in ASP.NET Web Pages](updating-data.md).</span><span class="sxs-lookup"><span data-stu-id="f8807-107">It assumes you have completed the series through [Updating Database Data in ASP.NET Web Pages](updating-data.md).</span></span>
> 
> <span data-ttu-id="f8807-108">Lernen Sie:</span><span class="sxs-lookup"><span data-stu-id="f8807-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="f8807-109">Auswählen ein einzelnes Datensatzes aus einer Liste von Datensätzen</span><span class="sxs-lookup"><span data-stu-id="f8807-109">How to select an individual record from a listing of records.</span></span>
> - <span data-ttu-id="f8807-110">Vorgehensweise: Löschen Sie einen einzelnen Datensatz aus einer Datenbank.</span><span class="sxs-lookup"><span data-stu-id="f8807-110">How to delete a single record from a database.</span></span>
> - <span data-ttu-id="f8807-111">Wie Sie überprüfen, dass eine bestimmte Schaltfläche in einem Formular geklickt wurde.</span><span class="sxs-lookup"><span data-stu-id="f8807-111">How to check that a specific button was clicked in a form.</span></span>
>   
> 
> <span data-ttu-id="f8807-112">Features/Technologien erläutert:</span><span class="sxs-lookup"><span data-stu-id="f8807-112">Features/technologies discussed:</span></span>
> 
> - <span data-ttu-id="f8807-113">Die `WebGrid` Helper.</span><span class="sxs-lookup"><span data-stu-id="f8807-113">The `WebGrid` helper.</span></span>
> - <span data-ttu-id="f8807-114">Die SQL `Delete` Befehl.</span><span class="sxs-lookup"><span data-stu-id="f8807-114">The SQL `Delete` command.</span></span>
> - <span data-ttu-id="f8807-115">Die `Database.Execute` zum Ausführen einer SQL-Methode `Delete` Befehl.</span><span class="sxs-lookup"><span data-stu-id="f8807-115">The `Database.Execute` method to run a SQL `Delete` command.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="f8807-116">Was müssen Sie erstellen</span><span class="sxs-lookup"><span data-stu-id="f8807-116">What You'll Build</span></span>

<span data-ttu-id="f8807-117">Im vorherigen Lernprogramm haben Sie gelernt, wie einen vorhandene Datenbank-Datensatz aktualisiert wird.</span><span class="sxs-lookup"><span data-stu-id="f8807-117">In the previous tutorial, you learned how to update an existing database record.</span></span> <span data-ttu-id="f8807-118">Dieses Lernprogramm ist ähnlich, außer dass anstelle der Datensatz aktualisiert, Sie sie löschen müssen.</span><span class="sxs-lookup"><span data-stu-id="f8807-118">This tutorial is similar, except that instead of updating the record, you'll delete it.</span></span> <span data-ttu-id="f8807-119">Die Prozesse sind nahezu identisch, außer dass löschen einfacher, damit dieses Lernprogramm kurz sein wird.</span><span class="sxs-lookup"><span data-stu-id="f8807-119">The processes are much the same, except that deleting is simpler, so this tutorial will be short.</span></span>

<span data-ttu-id="f8807-120">In der *Filme* Seite, aktualisieren Sie die `WebGrid` Helper so, dass die It zeigt eine **löschen** neben jeder Film, begleitet die **bearbeiten** verknüpfen, die Sie zuvor hinzugefügt haben.</span><span class="sxs-lookup"><span data-stu-id="f8807-120">In the *Movies* page, you'll update the `WebGrid` helper so that it displays a **Delete** link next to each movie to accompany the **Edit** link you added earlier.</span></span>

![Filme-Seite, die mit einer Delete-Link, um jedes Films](deleting-data/_static/image1.png)

<span data-ttu-id="f8807-122">Wie bei der Bearbeitung beim Klicken auf die **löschen** Link, es führt Sie zu einer anderen Seite ist, in dem die Informationen bereits in einem Formular:</span><span class="sxs-lookup"><span data-stu-id="f8807-122">As with editing, when you click the **Delete** link, it takes you to a different page, where the movie information is already in a form:</span></span>

![Löschen Sie die filmdetailseite mit einen Film angezeigt](deleting-data/_static/image2.png)

<span data-ttu-id="f8807-124">Sie können dann die Schaltfläche, um den Datensatz endgültig löschen klicken.</span><span class="sxs-lookup"><span data-stu-id="f8807-124">You can then click the button to delete the record permanently.</span></span>

## <a name="adding-a-delete-link-to-the-movie-listing"></a><span data-ttu-id="f8807-125">Beim Hinzufügen einer Delete-Verknüpfung auf die Film-Auflistung</span><span class="sxs-lookup"><span data-stu-id="f8807-125">Adding a Delete Link to the Movie Listing</span></span>

<span data-ttu-id="f8807-126">Beginnen Sie durch Hinzufügen einer **löschen** Verknüpfen mit der `WebGrid` Helper.</span><span class="sxs-lookup"><span data-stu-id="f8807-126">You'll start by adding a **Delete** link to the `WebGrid` helper.</span></span> <span data-ttu-id="f8807-127">Dieser Link ist ähnlich wie die **bearbeiten** verknüpfen, die Sie in einem vorherigen Lernprogramm hinzugefügt haben.</span><span class="sxs-lookup"><span data-stu-id="f8807-127">This link is similar to the **Edit** link you added in a previous tutorial.</span></span>

<span data-ttu-id="f8807-128">Öffnen der *Movies.cshtml* Datei.</span><span class="sxs-lookup"><span data-stu-id="f8807-128">Open the *Movies.cshtml* file.</span></span>

<span data-ttu-id="f8807-129">Ändern der `WebGrid` Markup im Text der Seite durch Hinzufügen einer Spalte.</span><span class="sxs-lookup"><span data-stu-id="f8807-129">Change the `WebGrid` markup in the body of the page by adding a column.</span></span> <span data-ttu-id="f8807-130">So sieht die geänderte Markup aus:</span><span class="sxs-lookup"><span data-stu-id="f8807-130">Here's the modified markup:</span></span>

[!code-html[Main](deleting-data/samples/sample1.html?highlight=9-10)]

<span data-ttu-id="f8807-131">Die neue Spalte wird diese:</span><span class="sxs-lookup"><span data-stu-id="f8807-131">The new column is this one:</span></span>

[!code-html[Main](deleting-data/samples/sample2.html)]

<span data-ttu-id="f8807-132">Das Raster konfiguriert ist, wie die **bearbeiten** Spalte ganz links im Raster ist und die **löschen** Spalte ganz rechts ist.</span><span class="sxs-lookup"><span data-stu-id="f8807-132">The way the grid is configured, the **Edit** column is leftmost in the grid and the **Delete** column is rightmost.</span></span> <span data-ttu-id="f8807-133">(Es ist ein Komma nach der `Year` Spalte jetzt für den Fall, dass Sie nicht feststellen.) Sind keine besonderen über diesen Linkspalten an mich, und Sie konnte sie genauso einfach nebeneinander platzieren.</span><span class="sxs-lookup"><span data-stu-id="f8807-133">(There's a comma after the `Year` column now, in case you didn't notice that.) There's nothing special about where these link columns go, and you could as easily put them next to each other.</span></span> <span data-ttu-id="f8807-134">In diesem Fall können sie separate zu machen schwieriger zu verwechselt abrufen.</span><span class="sxs-lookup"><span data-stu-id="f8807-134">In this case, they're separate to make them harder to get mixed up.</span></span>

![Filme-Seite mit Links bearbeiten "und" Details markiert, um anzugeben, dass sie nicht nebeneinander sind](deleting-data/_static/image3.png)

<span data-ttu-id="f8807-136">Die neue Spalte zeigt eine Verknüpfung (`<a>` Element), dessen Text lautet "Delete".</span><span class="sxs-lookup"><span data-stu-id="f8807-136">The new column shows a link (`<a>` element) whose text says "Delete".</span></span> <span data-ttu-id="f8807-137">Das Ziel der Verknüpfung (dessen `href` Attribut) ist Code, der letztendlich in etwa diese URL mit aufgelöst wird der `id` Wert für jede Film unterschiedlich:</span><span class="sxs-lookup"><span data-stu-id="f8807-137">The target of the link (its `href` attribute) is code that ultimately resolves to something like this URL, with the `id` value different for each movie:</span></span>

[!code-css[Main](deleting-data/samples/sample3.css)]

<span data-ttu-id="f8807-138">Dieser Link wird eine Seite mit dem Namen aufrufen *DeleteMovie* , und übergeben sie die ID des Films, die Sie ausgewählt haben.</span><span class="sxs-lookup"><span data-stu-id="f8807-138">This link will invoke a page named *DeleteMovie* and pass it the ID of the movie you've selected.</span></span>

<span data-ttu-id="f8807-139">In diesem Lernprogramm wird nicht näher wie diesen Link erstellt wird, da sie fast identisch mit ist der **bearbeiten** Link aus dem vorherigen Lernprogramm ([Aktualisieren der Datenbankdaten in ASP.NET Web Pages](updating-data.md)).</span><span class="sxs-lookup"><span data-stu-id="f8807-139">This tutorial won't go into detail about how this link is constructed, because it's almost identical to the **Edit** link from the previous tutorial ([Updating Database Data in ASP.NET Web Pages](updating-data.md)).</span></span>

## <a name="creating-the-delete-page"></a><span data-ttu-id="f8807-140">Erstellen die Seite "löschen"</span><span class="sxs-lookup"><span data-stu-id="f8807-140">Creating the Delete Page</span></span>

<span data-ttu-id="f8807-141">Nun können Sie die Seite erstellen, die das Ziel für die **löschen** Link im Raster.</span><span class="sxs-lookup"><span data-stu-id="f8807-141">Now you can create the page that will be the target for the **Delete** link in the grid.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="f8807-142">**Wichtige** die Technik der zuerst wählen Sie einen Datensatz löschen und dann mithilfe einer separaten Seite und einer Schaltfläche Überprüfen, ob den Prozess ist äußerst wichtig für die Sicherheit.</span><span class="sxs-lookup"><span data-stu-id="f8807-142">**Important** The technique of first selecting a record to delete and then using a separate page and button to confirm the process is extremely important for security.</span></span> <span data-ttu-id="f8807-143">Wie Sie in vorherigen Lernprogrammen gelesen haben, sodass *alle* Art von Änderung an Ihrer Website sollte *immer* erfolgen über ein Formular &mdash; , also mithilfe eines HTTP POST-Vorgangs.</span><span class="sxs-lookup"><span data-stu-id="f8807-143">As you've read in previous tutorials, making *any* sort of change to your website should *always* be done using a form &mdash; that is, using an HTTP POST operation.</span></span> <span data-ttu-id="f8807-144">Wenn es vorgenommenen möglich, den Standort zu ändern, indem Sie einfach einen Link (die mithilfe einen GET-Vorgang), konnte Personen stellen einfache Anforderungen an Ihre Website und Ihre Daten zu löschen.</span><span class="sxs-lookup"><span data-stu-id="f8807-144">If you made it possible to change the site just by clicking a link (that is, using a GET operation), people could make simple requests to your site and delete your data.</span></span> <span data-ttu-id="f8807-145">Auch ein Suchmaschinen-Crawler, der die Indizierung konnte nur über Links versehentlich Daten löschen.</span><span class="sxs-lookup"><span data-stu-id="f8807-145">Even a search-engine crawler that's indexing your site could inadvertently delete data just by following links.</span></span>
> 
> <span data-ttu-id="f8807-146">Wenn Ihre app Personen, die einen Datensatz ändern kann, müssen Sie den Datensatz für den Benutzer anzuzeigen, für die Bearbeitung trotzdem.</span><span class="sxs-lookup"><span data-stu-id="f8807-146">When your app lets people change a record, you have to present the record to the user for editing anyway.</span></span> <span data-ttu-id="f8807-147">Aber Sie möchten, überspringen Sie diesen Schritt für das Löschen eines Datensatzes sein.</span><span class="sxs-lookup"><span data-stu-id="f8807-147">But you might be tempted to skip this step for deleting a record.</span></span> <span data-ttu-id="f8807-148">Überspringen Sie diesen Schritt jedoch nicht.</span><span class="sxs-lookup"><span data-stu-id="f8807-148">Don't skip that step, though.</span></span> <span data-ttu-id="f8807-149">(Es ist auch nützlich für Benutzer finden Sie unter den Datensatz und bestätigen Sie, dass sie den Datensatz löschen, den sie für den gedacht.)</span><span class="sxs-lookup"><span data-stu-id="f8807-149">(It's also helpful for users to see the record and confirm that they're deleting the record that they intended.)</span></span>
> 
> <span data-ttu-id="f8807-150">In einem späteren Lernprogramm Satz sehen Sie, wie Anmeldenamen-Funktionen hinzugefügt, damit ein Benutzer melden Sie sich vor dem Löschen eines Datensatzes müssten.</span><span class="sxs-lookup"><span data-stu-id="f8807-150">In a subsequent tutorial set, you'll see how to add login functionality so a user would have to log in before deleting a record.</span></span>


<span data-ttu-id="f8807-151">Erstellen Sie eine Seite mit dem Namen *DeleteMovie.cshtml* , und Ersetzen Sie die Neuigkeiten in der Datei durch Folgendes Markup:</span><span class="sxs-lookup"><span data-stu-id="f8807-151">Create a page named *DeleteMovie.cshtml* and replace what's in the file with the following markup:</span></span>

[!code-cshtml[Main](deleting-data/samples/sample4.cshtml)]

<span data-ttu-id="f8807-152">Dieses Markup ähnelt der *EditMovie* Seiten, außer dass anstelle von Textfeldern (`<input type="text">`), das Markup enthält `<span>` Elemente.</span><span class="sxs-lookup"><span data-stu-id="f8807-152">This markup is like the *EditMovie* pages, except that instead of using text boxes (`<input type="text">`), the markup includes `<span>` elements.</span></span> <span data-ttu-id="f8807-153">Es ist nichts hier bearbeiten.</span><span class="sxs-lookup"><span data-stu-id="f8807-153">There's nothing here to edit.</span></span> <span data-ttu-id="f8807-154">Alles, was ist die Filmdetails angezeigt werden, damit die Benutzer können Sie sicher, dass sie den richtigen Film löschen.</span><span class="sxs-lookup"><span data-stu-id="f8807-154">All you have to do is display the movie details so that users can make sure that they're deleting the right movie.</span></span>

<span data-ttu-id="f8807-155">Das Markup enthält bereits einen Link, der dem Benutzer, die auf der Seite "Movie-Angebot" zurückgeben kann.</span><span class="sxs-lookup"><span data-stu-id="f8807-155">The markup already contains a link that lets the user return to the movie listing page.</span></span>

<span data-ttu-id="f8807-156">Wie in der *EditMovie* Seite, die ID des ausgewählten Films in einem ausgeblendeten Feld gespeichert wird.</span><span class="sxs-lookup"><span data-stu-id="f8807-156">As in the *EditMovie* page, the ID of the selected movie is stored in a hidden field.</span></span> <span data-ttu-id="f8807-157">(Es wird auf der Seite ursprünglich als ein Wert der Abfragezeichenfolge übergeben.) Es ist ein `Html.ValidationSummary` aufrufen, die Validierungsfehler angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="f8807-157">(It's passed into the page in the first place as a query string value.) There's an `Html.ValidationSummary` call that will display validation errors.</span></span> <span data-ttu-id="f8807-158">In diesem Fall möglicherweise der Fehler, dass keine Film-ID auf der Seite "übergeben wurde oder die Film-ID ungültig ist.</span><span class="sxs-lookup"><span data-stu-id="f8807-158">In this case, the error might be that no movie ID was passed to the page or that the movie ID is invalid.</span></span> <span data-ttu-id="f8807-159">Diese Situation kann eintreten, wenn jemand auf dieser Seite ausgeführt wurde, ohne zuvor einen Film in der *Filme* Seite.</span><span class="sxs-lookup"><span data-stu-id="f8807-159">This situation could occur if someone ran this page without first selecting a movie in the *Movies* page.</span></span>

<span data-ttu-id="f8807-160">Die Beschriftung der Schaltfläche wird **löschen Film**, und ihr Namensattribut wird festgelegt, um `buttonDelete`.</span><span class="sxs-lookup"><span data-stu-id="f8807-160">The button caption is **Delete Movie**, and its name attribute is set to `buttonDelete`.</span></span> <span data-ttu-id="f8807-161">Die `name` Attribut wird im Code verwendet werden, um die Schaltfläche zu identifizieren, die das Formular gesendet.</span><span class="sxs-lookup"><span data-stu-id="f8807-161">The `name` attribute will be used in the code to identify the button that submitted the form.</span></span>

<span data-ttu-id="f8807-162">Sie müssen Code schreiben, um die (1) die Filmdetails gelesen, wenn die Seite erstmals angezeigt wird und den Film 2) tatsächlich gelöscht werden, wenn der Benutzer die Schaltfläche klickt.</span><span class="sxs-lookup"><span data-stu-id="f8807-162">You'll have to write code to 1) read the movie details when the page is first displayed and 2) actually delete the movie when the user clicks the button.</span></span>

## <a name="adding-code-to-read-a-single-movie"></a><span data-ttu-id="f8807-163">Hinzufügen von Code zu einen einzigen Film lesen</span><span class="sxs-lookup"><span data-stu-id="f8807-163">Adding Code to Read a Single Movie</span></span>

<span data-ttu-id="f8807-164">Am oberen Rand der *DeleteMovie.cshtml* Seite, fügen Sie den folgenden Codeblock:</span><span class="sxs-lookup"><span data-stu-id="f8807-164">At the top of the *DeleteMovie.cshtml* page, add the following code block:</span></span>

[!code-cshtml[Main](deleting-data/samples/sample5.cshtml)]

<span data-ttu-id="f8807-165">Dieses Markup ist identisch mit den entsprechenden Code in der *EditMovie* Seite.</span><span class="sxs-lookup"><span data-stu-id="f8807-165">This markup is the same as the corresponding code in the *EditMovie* page.</span></span> <span data-ttu-id="f8807-166">Er ruft die Film-ID aus der Abfragezeichenfolge ab und verwendet die ID, um einen Datensatz aus der Datenbank zu lesen.</span><span class="sxs-lookup"><span data-stu-id="f8807-166">It gets the movie ID out of the query string and uses the ID to read a record from the database.</span></span> <span data-ttu-id="f8807-167">Der Code enthält die Validierungstest (`IsInt()` und `row != null`) um sicherzustellen, dass die Film-ID übergeben wird, um die Seite ungültig ist.</span><span class="sxs-lookup"><span data-stu-id="f8807-167">The code includes the validation test (`IsInt()` and `row != null`) to make sure that the movie ID being passed to the page is valid.</span></span>

<span data-ttu-id="f8807-168">Denken Sie daran, dass dieser Code nur beim ersten ausgeführt werden soll, die die Seite ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="f8807-168">Remember that this code should only run the first time the page runs.</span></span> <span data-ttu-id="f8807-169">Sie möchten nicht die Film-Eintrag aus der Datenbank erneut zu lesen, klickt der Benutzer die **löschen Film** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="f8807-169">You don't want to re-read the movie record from the database when the user clicks the **Delete Movie** button.</span></span> <span data-ttu-id="f8807-170">Deshalb zum Lesen der Film befindet sich innerhalb eines Tests, die besagt, dass code `if(!IsPost)` &mdash; , also *ist die Anforderung keinen Post-Vorgang (Formularübermittlung)*.</span><span class="sxs-lookup"><span data-stu-id="f8807-170">Therefore, code to read the movie is inside a test that says `if(!IsPost)` &mdash; that is, *if the request is not a post operation (form submission)*.</span></span>

## <a name="adding-code-to-delete-the-selected-movie"></a><span data-ttu-id="f8807-171">Hinzufügen von Code zum Löschen des ausgewählten Films</span><span class="sxs-lookup"><span data-stu-id="f8807-171">Adding Code to Delete the Selected Movie</span></span>

<span data-ttu-id="f8807-172">Um den Film klickt der Benutzer die Schaltfläche "" zu löschen, fügen Sie den folgenden Code innerhalb der schließenden Klammer der der `@` blockieren:</span><span class="sxs-lookup"><span data-stu-id="f8807-172">To delete the movie when the user clicks the button, add the following code just inside the closing brace of the `@` block:</span></span>

[!code-csharp[Main](deleting-data/samples/sample6.cs)]

<span data-ttu-id="f8807-173">Dieser Code ist ähnlich wie der Code für einen vorhandenen Datensatz aktualisieren, aber einfacher.</span><span class="sxs-lookup"><span data-stu-id="f8807-173">This code is similar to the code for updating an existing record, but simpler.</span></span> <span data-ttu-id="f8807-174">Der Code führt im Wesentlichen eine SQL `Delete` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="f8807-174">The code basically runs a SQL `Delete` statement.</span></span>

 <span data-ttu-id="f8807-175">Wie in der *EditMovie* Seite der Code befindet sich in einem `if(IsPost)` Block.</span><span class="sxs-lookup"><span data-stu-id="f8807-175">As in the *EditMovie* page, the code is in an `if(IsPost)` block.</span></span> <span data-ttu-id="f8807-176">Dieses Mal die `if()` Bedingung ist etwas komplizierter:</span><span class="sxs-lookup"><span data-stu-id="f8807-176">This time, the `if()` condition is a little more complicated:</span></span> 

[!code-csharp[Main](deleting-data/samples/sample7.cs)]

<span data-ttu-id="f8807-177">Es gibt zwei Bedingungen hier ein.</span><span class="sxs-lookup"><span data-stu-id="f8807-177">There are two conditions here.</span></span> <span data-ttu-id="f8807-178">Die erste ist, dass die Seite übermittelt wird, wie Sie vor dem gesehen haben &mdash; `if(IsPost)`.</span><span class="sxs-lookup"><span data-stu-id="f8807-178">The first is that the page is being submitted, as you've seen before &mdash; `if(IsPost)`.</span></span>

<span data-ttu-id="f8807-179">Die zweite Bedingung ist `!Request["buttonDelete"].IsEmpty()`, was bedeutet, dass die Anforderung ein Objekt mit dem Namen verfügt über `buttonDelete`.</span><span class="sxs-lookup"><span data-stu-id="f8807-179">The second condition is `!Request["buttonDelete"].IsEmpty()`, meaning that the request has an object named `buttonDelete`.</span></span> <span data-ttu-id="f8807-180">Zugegeben, ist es ein indirektes Verfahren dar, auf welche Schaltfläche Senden des Formulars testen.</span><span class="sxs-lookup"><span data-stu-id="f8807-180">Admittedly, it's an indirect way of testing which button submitted the form.</span></span> <span data-ttu-id="f8807-181">Wenn ein Formular mehrere Schaltflächen "Senden" enthält, wird nur der Namen der Schaltfläche, auf die geklickt wurde, in der Anforderung.</span><span class="sxs-lookup"><span data-stu-id="f8807-181">If a form contains multiple submit buttons, only the name of the button that was clicked appears in the request.</span></span> <span data-ttu-id="f8807-182">Aus diesem Grund, logisch, wenn der Name einer bestimmten Schaltfläche wird, in der Anforderung angezeigt &mdash; oder wie im Code angegeben werden, wenn diese Schaltfläche nicht leer ist &mdash; , die die Schaltfläche, die das Formular gesendet wird.</span><span class="sxs-lookup"><span data-stu-id="f8807-182">Therefore, logically, if the name of a particular button appears in the request &mdash; or as stated in the code, if that button isn't empty &mdash; that's the button that submitted the form.</span></span>

<span data-ttu-id="f8807-183">Die `&&` Operator bedeutet "und" (logisches AND).</span><span class="sxs-lookup"><span data-stu-id="f8807-183">The `&&` operator means "and" (logical AND).</span></span> <span data-ttu-id="f8807-184">Daher die gesamte `if` Bedingung ist...</span><span class="sxs-lookup"><span data-stu-id="f8807-184">Therefore the entire `if` condition is ...</span></span>

<span data-ttu-id="f8807-185">*Diese Anforderung wird einer POST-Anforderung (keine Anforderung zum ersten Mal)*</span><span class="sxs-lookup"><span data-stu-id="f8807-185">*This request is a post (not a first-time request)*</span></span>  
  
 <span data-ttu-id="f8807-186">UND</span><span class="sxs-lookup"><span data-stu-id="f8807-186">AND</span></span>  
  
<span data-ttu-id="f8807-187">*Die* `buttonDelete` *Schaltfläche wurde die Schaltfläche, die das Formular gesendet.*</span><span class="sxs-lookup"><span data-stu-id="f8807-187">*The* `buttonDelete`*button was the button that submitted the form.*</span></span>

<span data-ttu-id="f8807-188">Dieses Formular (in tatsächlich auf dieser Seite) enthält nur ein Optionsfeld, sodass die zusätzliche Tests für `buttonDelete` ist technisch nicht erforderlich.</span><span class="sxs-lookup"><span data-stu-id="f8807-188">This form (in fact, this page) contains only one button, so the additional test for `buttonDelete` is technically not required.</span></span> <span data-ttu-id="f8807-189">Dennoch können Sie einen Vorgang ausführen, der Daten dauerhaft entfernt.</span><span class="sxs-lookup"><span data-stu-id="f8807-189">Still, you're about to perform an operation that will permanently remove data.</span></span> <span data-ttu-id="f8807-190">Sie sollten sich daher so sicher wie möglich sein, dass Sie den Vorgang durchführen, nur, wenn der Benutzer explizit angefordert hat.</span><span class="sxs-lookup"><span data-stu-id="f8807-190">So you want to be as sure as possible that you're performing the operation only when the user has explicitly requested it.</span></span> <span data-ttu-id="f8807-191">Nehmen wir beispielsweise an, dass Sie diese Seite später erweitert und andere Schaltflächen hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="f8807-191">For example, suppose that you expanded this page later and added other buttons to it.</span></span> <span data-ttu-id="f8807-192">Der Code, der den Film löscht wird selbst dann ausgeführt, wenn die `buttonDelete` Schaltfläche geklickt wurde.</span><span class="sxs-lookup"><span data-stu-id="f8807-192">Even then, the code that deletes the movie will run only if the `buttonDelete` button was clicked.</span></span>

<span data-ttu-id="f8807-193">Wie in der *EditMovie* Seite, die ID aus dem ausgeblendeten Feld abrufen und führen Sie den SQL-Befehl.</span><span class="sxs-lookup"><span data-stu-id="f8807-193">As in the *EditMovie* page, you get the ID from the hidden field and then run the SQL command.</span></span> <span data-ttu-id="f8807-194">Die Syntax für die `Delete` -Anweisung ist:</span><span class="sxs-lookup"><span data-stu-id="f8807-194">The syntax for the `Delete` statement is:</span></span>

`DELETE FROM table WHERE ID = value`

<span data-ttu-id="f8807-195">Schließen Sie unbedingt die `WHERE` -Klausel und die-ID.</span><span class="sxs-lookup"><span data-stu-id="f8807-195">It's vital to include the `WHERE` clause and the ID.</span></span> <span data-ttu-id="f8807-196">Wenn Sie die WHERE-Klausel auslassen *alle Datensätze in der Tabelle gelöscht werden*.</span><span class="sxs-lookup"><span data-stu-id="f8807-196">If you leave out the WHERE clause, *all the records in the table will be deleted*.</span></span> <span data-ttu-id="f8807-197">Wie Sie gesehen haben, übergeben Sie den ID-Wert an den SQL-Befehl, indem Sie mithilfe eines Platzhalters.</span><span class="sxs-lookup"><span data-stu-id="f8807-197">As you have seen, you pass the ID value to the SQL command by using a placeholder.</span></span>

## <a name="testing-the-movie-delete-process"></a><span data-ttu-id="f8807-198">Testen des Prozesses der Film löschen</span><span class="sxs-lookup"><span data-stu-id="f8807-198">Testing the Movie Delete Process</span></span>

<span data-ttu-id="f8807-199">Sie können jetzt testen.</span><span class="sxs-lookup"><span data-stu-id="f8807-199">Now you can test.</span></span> <span data-ttu-id="f8807-200">Führen Sie die *Filme* Seite, und klicken Sie auf **löschen** neben einen Film.</span><span class="sxs-lookup"><span data-stu-id="f8807-200">Run the *Movies* page, and click **Delete** next to a movie.</span></span> <span data-ttu-id="f8807-201">Wenn die *DeleteMovie* Seite angezeigt wird, klicken Sie auf **löschen Film**.</span><span class="sxs-lookup"><span data-stu-id="f8807-201">When the *DeleteMovie* page appears, click **Delete Movie**.</span></span>

![Löschen von filmdetailseite mit Schaltfläche "Löschen Film" hervorgehoben](deleting-data/_static/image4.png)

<span data-ttu-id="f8807-203">Wenn Sie die Schaltfläche klicken, wird der Code löscht die Filme und zur Auflistung Film gibt.</span><span class="sxs-lookup"><span data-stu-id="f8807-203">When you click the button, the code deletes the movies and returns to the movie listing.</span></span> <span data-ttu-id="f8807-204">Sie können es für den gelöschten Film suchen, und bestätigen Sie, dass sie gelöscht wurde, wird.</span><span class="sxs-lookup"><span data-stu-id="f8807-204">There you can search for the deleted movie and confirm that it's been deleted.</span></span>

## <a name="coming-up-next"></a><span data-ttu-id="f8807-205">Als Nächstes kommen</span><span class="sxs-lookup"><span data-stu-id="f8807-205">Coming Up Next</span></span>

<span data-ttu-id="f8807-206">Der nächste Lernprogrammen wird gezeigt, wie eine allgemeine Darstellung und Layout alle Seiten auf Ihrer Website zu gewähren.</span><span class="sxs-lookup"><span data-stu-id="f8807-206">The next tutorial shows you how to give all the pages on your site a common look and layout.</span></span>

## <a name="complete-listing-for-movie-page-updated-with-delete-links"></a><span data-ttu-id="f8807-207">Vollständige Auflistung für Filmdetailseite (inklusive löschen Links)</span><span class="sxs-lookup"><span data-stu-id="f8807-207">Complete Listing for Movie Page (Updated with Delete Links)</span></span>

[!code-cshtml[Main](deleting-data/samples/sample8.cshtml)]

## <a name="complete-listing-for-deletemovie-page"></a><span data-ttu-id="f8807-208">Vollständige Auflistung für die Seite "DeleteMovie"</span><span class="sxs-lookup"><span data-stu-id="f8807-208">Complete Listing for DeleteMovie Page</span></span>

[!code-cshtml[Main](deleting-data/samples/sample9.cshtml)]

## <a name="additional-resources"></a><span data-ttu-id="f8807-209">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="f8807-209">Additional Resources</span></span>

- [<span data-ttu-id="f8807-210">Einführung in ASP.NET-Webprogrammierung mithilfe der Razor-Syntax</span><span class="sxs-lookup"><span data-stu-id="f8807-210">Introduction to ASP.NET Web Programming by Using the Razor Syntax</span></span>](../introducing-razor-syntax-c.md)
- <span data-ttu-id="f8807-211">[DELETE-Anweisung von SQL](http://www.w3schools.com/sql/sql_delete.asp) auf der Website W3Schools</span><span class="sxs-lookup"><span data-stu-id="f8807-211">[SQL DELETE Statement](http://www.w3schools.com/sql/sql_delete.asp) on the W3Schools site</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="f8807-212">[Zurück](updating-data.md)
[Weiter](layouts.md)</span><span class="sxs-lookup"><span data-stu-id="f8807-212">[Previous](updating-data.md)
[Next](layouts.md)</span></span>
