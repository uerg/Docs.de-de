---
title: Aktualisieren der generierten Seiten
author: rick-anderson
description: Aktualisieren der generierten Seiten mit besserer Anzeige.
keywords: ASP.NET Core, Razor-Seiten
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/razor-pages/da1
ms.openlocfilehash: dfe8136dccb0e98a9fc6b1395161ccb442392c76
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
# <a name="updating-the-generated-pages"></a><span data-ttu-id="aba59-104">Aktualisieren der generierten Seiten</span><span class="sxs-lookup"><span data-stu-id="aba59-104">Updating the generated pages</span></span>

<span data-ttu-id="aba59-105">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="aba59-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="aba59-106">Die „movie“-App ist schon recht ansprechend, doch die Präsentation ist nicht ideal.</span><span class="sxs-lookup"><span data-stu-id="aba59-106">We have a good start to the movie app, but the presentation is not ideal.</span></span> <span data-ttu-id="aba59-107">Wir wollen die Uhrzeit nicht sehen (12:00:00 Uhr in der nachstehenden Abbildung), und **ReleaseDate** als soll **Release Date** (zwei Wörter) angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="aba59-107">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be **Release Date** (two words).</span></span>

![In Chrome geöffnete Movie-Anwendung mit Filmdaten](sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="aba59-109">Aktualisieren des generierten Codes</span><span class="sxs-lookup"><span data-stu-id="aba59-109">Update the generated code</span></span>

<span data-ttu-id="aba59-110">Öffnen Sie die Datei *Models/Movie.cs*, und fügen Sie die im folgenden Code gezeigten markierten Zeilen hinzu:</span><span class="sxs-lookup"><span data-stu-id="aba59-110">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]

<span data-ttu-id="aba59-111">Klicken Sie mit der rechten Maustaste auf eine rote Wellenlinie > **Schnellaktionen und Refactorings**.</span><span class="sxs-lookup"><span data-stu-id="aba59-111">Right click on a red squiggly line > ** Quick Actions and Refactorings**.</span></span>

  ![Kontextmenü mit **> Schnellaktionen und Refactorings**.](da1/qa.png)

<span data-ttu-id="aba59-113">Wählen Sie `using System.ComponentModel.DataAnnotations;` aus.</span><span class="sxs-lookup"><span data-stu-id="aba59-113">Select `using System.ComponentModel.DataAnnotations;`</span></span>

  ![„using System.ComponentModel.DataAnnotations“ oben in der Liste](da1/da.png)

  <span data-ttu-id="aba59-115">Visual Studio fügt `using System.ComponentModel.DataAnnotations;` hinzu.</span><span class="sxs-lookup"><span data-stu-id="aba59-115">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

<span data-ttu-id="aba59-116">Wir behandeln [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) im nächsten Tutorial.</span><span class="sxs-lookup"><span data-stu-id="aba59-116">We'll cover [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) in the next tutorial.</span></span> <span data-ttu-id="aba59-117">Das [Display](https://docs.microsoft.com//aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata)-Attribut gibt an, was für den Namen eines Felds angezeigt werden soll (in diesem Fall „Release Date“ anstatt „ReleaseDate“).</span><span class="sxs-lookup"><span data-stu-id="aba59-117">The [Display](https://docs.microsoft.com//aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) attribute specifies what to display for the name of a field (in this case "Release Date" instead of "ReleaseDate").</span></span> <span data-ttu-id="aba59-118">Das [DataType](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter)-Attribut gibt den Typ der Daten (Date) an, damit die im Feld gespeicherten Uhrzeitinformationen nicht angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="aba59-118">The [DataType](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) attribute specifies the type of the data (Date), so the time information stored in the field is not displayed.</span></span>

<span data-ttu-id="aba59-119">Navigieren Sie zu „Pages/Movies“, und bewegen Sie den Mauszeiger über dem Link **Bearbeiten**, um die Ziel-URL anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="aba59-119">Browse to Pages/Movies and  hover over an **Edit** link to see the target URL.</span></span>

![Browserfenster mit Maus über dem Link „Bearbeiten“ und der Link-URL „http://localhost:1234/Movies/Edit/5“](da1/edit7.png)

<span data-ttu-id="aba59-121">Die Links **Edit**, **Details** und **Delete** werden mithilfe des [Hilfsprogramms für Ankertags](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) in der Datei *Pages/Movies/Index.cshtml* generiert.</span><span class="sxs-lookup"><span data-stu-id="aba59-121">The **Edit**, **Details**, and **Delete** links are generated by the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) in the *Pages/Movies/Index.cshtml* file.</span></span>

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=16-18&range=32-)]

<span data-ttu-id="aba59-122">[Taghilfsprogramme](xref:mvc/views/tag-helpers/intro) ermöglichen serverseitigem Code das Mitwirken am Erstellen und Rendern von HTML-Elementen in Razor-Dateien.</span><span class="sxs-lookup"><span data-stu-id="aba59-122">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="aba59-123">Im vorangehenden Code generiert das `AnchorTagHelper` dynamisch den Wert des HTML-Attributs `href` auf der Razor-Seite (die Route ist relativ), das `asp-page`-Element und die Routen-ID (`asp-route-id`).</span><span class="sxs-lookup"><span data-stu-id="aba59-123">In the preceding code, the `AnchorTagHelper` dynamically generates the HTML `href` attribute value from the Razor Page (the route is relative), the `asp-page`,  and the route id (`asp-route-id`).</span></span> <span data-ttu-id="aba59-124">Weitere Informationen finden Sie unter [URL-Generierung für Seiten](xref:mvc/razor-pages/index#url-generation-for-pages).</span><span class="sxs-lookup"><span data-stu-id="aba59-124">See [URL generation for Pages](xref:mvc/razor-pages/index#url-generation-for-pages) for more information.</span></span>

<span data-ttu-id="aba59-125">Rufen Sie in Ihrem bevorzugten Browser **Quelltext anzeigen** auf, um das generierte Markup zu untersuchen.</span><span class="sxs-lookup"><span data-stu-id="aba59-125">Use **View Source** from your favorite browser to examine the generated markup.</span></span> <span data-ttu-id="aba59-126">Ein Teil des generierten HTML-Codes wird unten gezeigt:</span><span class="sxs-lookup"><span data-stu-id="aba59-126">A portion of the generated HTML is shown below:</span></span>

```html
<td>
  <a href="/Movies/Edit?id=1">Edit</a> |
  <a href="/Movies/Details?id=1">Details</a> |
  <a href="/Movies/Delete?id=1">Delete</a>
</td>
```

<span data-ttu-id="aba59-127">Die dynamisch generierten Links übergeben die Film-ID mit einer Abfragezeichenfolge (z B. `http://localhost:5000/Movies/Details?id=2`).</span><span class="sxs-lookup"><span data-stu-id="aba59-127">The dynamically-generated links pass the movie ID with a query string (for example, `http://localhost:5000/Movies/Details?id=2` ).</span></span> 

<span data-ttu-id="aba59-128">Aktualisieren Sie die Razor-Seiten „Edit“, „Details“ und „Delete“ so, dass die Routenvorlage „{id:int}“ verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="aba59-128">Update the Edit, Details, and Delete Razor Pages to use the "{id:int}" route template.</span></span> <span data-ttu-id="aba59-129">Ändern Sie die „page“-Direktive für jede dieser Seiten aus `@page` in `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="aba59-129">Change the page directive for each of these pages from `@page` to `@page "{id:int}"`.</span></span> <span data-ttu-id="aba59-130">Führen Sie die App aus, und zeigen Sie dann den Quelltext an.</span><span class="sxs-lookup"><span data-stu-id="aba59-130">Run the app and then view source.</span></span> <span data-ttu-id="aba59-131">Der generierte HTML-Code fügt die ID dem Pfadteil der URL hinzu:</span><span class="sxs-lookup"><span data-stu-id="aba59-131">The generated HTML adds the ID to the path portion of the URL:</span></span>

```html
<td>
  <a href="/Movies/Edit/1">Edit</a> |
  <a href="/Movies/Details/1">Details</a> |
  <a href="/Movies/Delete/1">Delete</a>
</td>
```

<span data-ttu-id="aba59-132">Eine Anforderung an die Seite mit der Routenvorlage „{id:int}“, die **nicht** die ganze Zahl enthält, gibt den HTTP-Fehler 404 (Nicht gefunden) zurück.</span><span class="sxs-lookup"><span data-stu-id="aba59-132">A request to the page with the "{id:int}" route template that does **not** include the integer will return an HTTP 404 (not found) error.</span></span> <span data-ttu-id="aba59-133">`http://localhost:5000/Movies/Details` gibt beispielsweise den Fehler 404 zurück.</span><span class="sxs-lookup"><span data-stu-id="aba59-133">For example, `http://localhost:5000/Movies/Details` will return a 404 error.</span></span> <span data-ttu-id="aba59-134">Um die ID optional zu machen, fügen Sie `?` an die Routeneinschränkung an:</span><span class="sxs-lookup"><span data-stu-id="aba59-134">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

### <a name="update-concurrency-exception-handling"></a><span data-ttu-id="aba59-135">Aktualisieren der Behandlung von Ausnahmen bei vollständiger Parallelität</span><span class="sxs-lookup"><span data-stu-id="aba59-135">Update concurrency exception handling</span></span>

<span data-ttu-id="aba59-136">Aktualisieren Sie die `OnPostAsync`-Methode in der Datei *Pages/Movies/Edit.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="aba59-136">Update the `OnPostAsync` method in the *Pages/Movies/Edit.cshtml.cs* file.</span></span> <span data-ttu-id="aba59-137">Im folgenden markierten Code sind alle Änderungen dargestellt:</span><span class="sxs-lookup"><span data-stu-id="aba59-137">The following highlighted code shows the changes:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit.cshtml.cs?name=snippet1&highlight=16-23)]

<span data-ttu-id="aba59-138">Der vorherige Code erkennt nur Ausnahmen bei vollständiger Parallelität, wenn der erste gleichzeitige Client den Film löscht und der zweite gleichzeitige Client Änderungen am Film vornimmt.</span><span class="sxs-lookup"><span data-stu-id="aba59-138">The previous code only detects concurrency exceptions when the first concurrent client deletes the movie, and the second concurrent client posts changes to the movie.</span></span>

<span data-ttu-id="aba59-139">So testen Sie den Block `catch`</span><span class="sxs-lookup"><span data-stu-id="aba59-139">To test the `catch` block:</span></span>

* <span data-ttu-id="aba59-140">Legen Sie einen Haltepunkt auf `catch (DbUpdateConcurrencyException)` fest.</span><span class="sxs-lookup"><span data-stu-id="aba59-140">Set a breakpoint on `catch (DbUpdateConcurrencyException)`</span></span>
* <span data-ttu-id="aba59-141">Bearbeiten Sie einen Film.</span><span class="sxs-lookup"><span data-stu-id="aba59-141">Edit a movie.</span></span>
* <span data-ttu-id="aba59-142">Klicken Sie in einem anderen Browserfenster auf den Link **Delete** für denselben Film, und löschen Sie dann den Film.</span><span class="sxs-lookup"><span data-stu-id="aba59-142">In another browser window, select the **Delete** link for the same movie, and then delete the movie.</span></span>
* <span data-ttu-id="aba59-143">Übermitteln Sie im vorherigen Browserfenster Änderungen am Film.</span><span class="sxs-lookup"><span data-stu-id="aba59-143">In the previous browser window, post changes to the movie.</span></span>

<span data-ttu-id="aba59-144">Code in Produktionsumgebungen erkennt normalerweise Parallelitätskonflikte, wenn zwei oder mehrere Clients einen Datensatz gleichzeitig aktualisiert haben.</span><span class="sxs-lookup"><span data-stu-id="aba59-144">Production code would generally detect concurrency conflicts when two or more clients concurrently updated a record.</span></span> <span data-ttu-id="aba59-145">Weitere Informationen finden Sie unter [Behandlung von Parallelitätskonflikten](xref:data/ef-mvc/concurrency).</span><span class="sxs-lookup"><span data-stu-id="aba59-145">See [Handling concurrency conflicts](xref:data/ef-mvc/concurrency) for more information.</span></span>

### <a name="posting-and-binding-review"></a><span data-ttu-id="aba59-146">Überprüfen der Bereitstellung und Bindung</span><span class="sxs-lookup"><span data-stu-id="aba59-146">Posting and binding review</span></span>

<span data-ttu-id="aba59-147">Untersuchen Sie die Datei *Pages/Movies/Edit.cshtml.cs*: [!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit.cshtml.cs?name=snippet2)]</span><span class="sxs-lookup"><span data-stu-id="aba59-147">Examine the *Pages/Movies/Edit.cshtml.cs* file: [!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit.cshtml.cs?name=snippet2)]</span></span>

<span data-ttu-id="aba59-148">Wenn eine HTTP GET-Anforderung an die Seite „Movies/Edit“ gerichtet wird (z.B. `http://localhost:5000/Movies/Edit/2`):</span><span class="sxs-lookup"><span data-stu-id="aba59-148">When an HTTP GET request is made to the Movies/Edit page (for example, `http://localhost:5000/Movies/Edit/2`):</span></span>

* <span data-ttu-id="aba59-149">Die `OnGetAsync`-Methode ruft den Film aus der Datenbank ab und gibt die `Page`-Methode zurück.</span><span class="sxs-lookup"><span data-stu-id="aba59-149">The `OnGetAsync` method fetches the movie from the database and returns the `Page` method.</span></span> 
* <span data-ttu-id="aba59-150">Die `Page`-Methode rendert die Razor-Seite *Pages/Movies/Edit.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="aba59-150">The `Page` method renders the *Pages/Movies/Edit.cshtml* Razor Page.</span></span> <span data-ttu-id="aba59-151">Die Datei *Pages/Movies/Edit.cshtml* enthält die Modelldirektive (`@model RazorPagesMovie.Pages.Movies.EditModel`), die das Filmmodell auf der Seite verfügbar macht.</span><span class="sxs-lookup"><span data-stu-id="aba59-151">The *Pages/Movies/Edit.cshtml* file contains the model directive (`@model RazorPagesMovie.Pages.Movies.EditModel`), which makes the the movie model available on the page.</span></span>
* <span data-ttu-id="aba59-152">Das Bearbeitungsformular wird mit den Werten aus dem Film angezeigt.</span><span class="sxs-lookup"><span data-stu-id="aba59-152">The Edit form is displayed with the values from the movie.</span></span>

<span data-ttu-id="aba59-153">Wenn die Seite „Filme/Bearbeiten“ bereitgestellt wird:</span><span class="sxs-lookup"><span data-stu-id="aba59-153">When the Movies/Edit page is posted:</span></span>

* <span data-ttu-id="aba59-154">Die Formularwerte auf der Seite sind an die `Movie`-Eigenschaft gebunden.</span><span class="sxs-lookup"><span data-stu-id="aba59-154">The form values on the page are bound to the `Movie` property.</span></span> <span data-ttu-id="aba59-155">Das `[BindProperty]`-Attribut ermöglicht die [Modellbindung](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="aba59-155">The `[BindProperty]` attribute enables [Model binding](xref:mvc/models/model-binding).</span></span>

  ```csharp
  [BindProperty]
  public Movie Movie { get; set; }
  ```

* <span data-ttu-id="aba59-156">Bei Fehlern beim Modellstatus (Beispiel: `ReleaseDate` kann nicht in ein Datum konvertiert werden), wird das Formular mit den übermittelten Werten erneut bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="aba59-156">If there are errors in the model state (for example, `ReleaseDate` cannot be converted to a date), the form is posted again with the submitted values.</span></span>
* <span data-ttu-id="aba59-157">Wenn keine Modellfehler vorhanden sind, wird der Film gespeichert.</span><span class="sxs-lookup"><span data-stu-id="aba59-157">If there are no model errors, the movie is saved.</span></span>

<span data-ttu-id="aba59-158">Die HTTP GET-Methoden auf den Razor-Seiten „Index“, „Create“ und „Delete“ folgen einem ähnlichen Muster.</span><span class="sxs-lookup"><span data-stu-id="aba59-158">The HTTP GET methods in the Index, Create, and Delete Razor pages follow a similar pattern.</span></span> <span data-ttu-id="aba59-159">Die HTTP-POST-Methode `OnPostAsync` auf der Razor-Seite „Create“ folgt einem ähnlichen Muster wie die `OnPostAsync`-Methode auf der Razor-Seite „Edit“.</span><span class="sxs-lookup"><span data-stu-id="aba59-159">The HTTP POST `OnPostAsync` method in the Create Razor Page follows a similar pattern to the `OnPostAsync` method in the Edit Razor Page.</span></span>

<span data-ttu-id="aba59-160">Die Suche wird im nächsten Tutorial hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="aba59-160">Search is added in the next tutorial.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="aba59-161">[Zurück: Arbeiten mit SQL Server LocalDB](xref:tutorials/razor-pages/sql)
[Hinzufügen der Suche](xref:tutorials/razor-pages/search)</span><span class="sxs-lookup"><span data-stu-id="aba59-161">[Previous: Working with SQL Server LocalDB](xref:tutorials/razor-pages/sql)
[Adding Search](xref:tutorials/razor-pages/search)</span></span>
