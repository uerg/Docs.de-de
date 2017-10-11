---
title: "Gerüstbau mit Razor-Seiten in ASP.NET Core"
author: rick-anderson
description: "Gibt nähere Informationen über durch Gerüstbau erstellte Razor-Seiten."
keywords: ASP.NET Core, Razor-Seiten, Razor, MVC
ms.author: riande
manager: wpickett
ms.date: 09/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/page
ms.openlocfilehash: 7ae83b9bdadf5ebf8846b0c09c585da406708d12
ms.sourcegitcommit: 94b7e0f95b92c98b182a93d2b3dc0287e5f97976
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2017
---
# <a name="scaffolded-razor-pages-in-aspnet-core"></a><span data-ttu-id="c2533-104">Gerüstbau mit Razor-Seiten in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c2533-104">Scaffolded Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="c2533-105">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c2533-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c2533-106">In diesem Tutorial werden die Razor-Seiten näher untersucht, die im vorherigen Tutorial [Hinzufügen eines Modells](xref:tutorials/razor-pages/model#scaffold-the-movie-model) durch Gerüstbau erstellt wurden.</span><span class="sxs-lookup"><span data-stu-id="c2533-106">This tutorial examines the Razor Pages created by scaffolding in the previous tutorial topic [Adding a model](xref:tutorials/razor-pages/model#scaffold-the-movie-model).</span></span> 

<span data-ttu-id="c2533-107">Beispiel [Anzeigen oder Herunterladen](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie).</span><span class="sxs-lookup"><span data-stu-id="c2533-107">[View or download](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) sample.</span></span>

## <a name="the-create-delete-details-and-edit-pages"></a><span data-ttu-id="c2533-108">Die Seiten „Create“, „Delete“, „Details“ und „Edit“.</span><span class="sxs-lookup"><span data-stu-id="c2533-108">The Create, Delete, Details, and Edit pages.</span></span>

<span data-ttu-id="c2533-109">Betrachten Sie die *Pages/Movies/Index.cshtml.cs*-CodeBehind-Datei: [!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]</span><span class="sxs-lookup"><span data-stu-id="c2533-109">Examine the *Pages/Movies/Index.cshtml.cs* code-behind file: [!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]</span></span>

<span data-ttu-id="c2533-110">Razor-Seiten werden von `PageModel` abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="c2533-110">Razor Pages are derived from `PageModel`.</span></span> <span data-ttu-id="c2533-111">Gemäß der Konvention wird die von `PageModel` abgeleitete Klasse `<PageName>Model` genannt.</span><span class="sxs-lookup"><span data-stu-id="c2533-111">By convention, the `PageModel`-derived class is called `<PageName>Model`.</span></span> <span data-ttu-id="c2533-112">Der Konstruktor verwendet [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection), um `MovieContext` zur Seite hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="c2533-112">The constructor uses [dependency injection](xref:fundamentals/dependency-injection) to add the `MovieContext` to the page.</span></span> <span data-ttu-id="c2533-113">Alle per Gerüstbau erstellten Seiten folgen diesem Muster.</span><span class="sxs-lookup"><span data-stu-id="c2533-113">All the scaffolded pages follow this pattern.</span></span>

<span data-ttu-id="c2533-114">Wenn eine Anforderung an die Seite erfolgt, gibt die Methode `OnGetAsync` eine Filmliste an die Razor-Seite zurück.</span><span class="sxs-lookup"><span data-stu-id="c2533-114">When a request is made for the page, the `OnGetAsync` method returns a list of movies to the Razor Page.</span></span> <span data-ttu-id="c2533-115">`OnGetAsync` oder `OnGet` werden für eine Razor-Seite aufgerufen, um den Zustand der Seite zu initialisieren.</span><span class="sxs-lookup"><span data-stu-id="c2533-115">`OnGetAsync` or `OnGet` is called on a Razor Page to initialize the state for the page.</span></span> <span data-ttu-id="c2533-116">In diesem Fall ruft `OnGetAsync` eine Liste von Filmen für die Anzeige ab.</span><span class="sxs-lookup"><span data-stu-id="c2533-116">In this case, `OnGetAsync` gets a list of movies to display.</span></span>

<span data-ttu-id="c2533-117">Betrachten Sie die *Pages/Movies/Index.cshtml*-Razor-Seite:</span><span class="sxs-lookup"><span data-stu-id="c2533-117">Examine the *Pages/Movies/Index.cshtml* Razor Page:</span></span>

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

<span data-ttu-id="c2533-118">Razor kann aus HTML in C# oder Razor-spezifisches Markup übergehen.</span><span class="sxs-lookup"><span data-stu-id="c2533-118">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="c2533-119">Wenn auf ein `@`-Symbol ein für Razor reserviertes Schlüsselwort ([Razor reserved keyword](xref:mvc/views/razor#razor-reserved-keywords)) folgt, wechselt es in Razor-spezifisches Markup. Andernfalls geht es in C# über.</span><span class="sxs-lookup"><span data-stu-id="c2533-119">When an `@` symbol is followed by a [Razor reserved keyword](xref:mvc/views/razor#razor-reserved-keywords), it transitions into Razor-specific markup, otherwise it transitions into C#.</span></span>

<span data-ttu-id="c2533-120">Die Razor-Anweisung `@page` ändert die Datei in eine &mdash;-MVC-Aktion. Das bedeutet, dass sie Anforderungen verarbeiten kann.</span><span class="sxs-lookup"><span data-stu-id="c2533-120">The `@page` Razor directive makes the file into an MVC action &mdash; which means that it can handle requests.</span></span> <span data-ttu-id="c2533-121">`@page` muss die erste Razor-Anweisung auf einer Seite sein.</span><span class="sxs-lookup"><span data-stu-id="c2533-121">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="c2533-122">`@page` ist ein Beispiel für einen Übergang in Razor-spezifisches Markup.</span><span class="sxs-lookup"><span data-stu-id="c2533-122">`@page` is an example of transitioning into Razor-specific markup.</span></span> <span data-ttu-id="c2533-123">Unter [Razor-Syntax](xref:mvc/views/razor#razor-syntax) finden Sie weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="c2533-123">See [Razor syntax](xref:mvc/views/razor#razor-syntax) for more information.</span></span>

<span data-ttu-id="c2533-124">Überprüfen Sie den Lambdaausdruck, der in der folgenden HTML-Hilfsfunktion verwendet wird:</span><span class="sxs-lookup"><span data-stu-id="c2533-124">Examine the lambda expression used in the following HTML Helper:</span></span>

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

<span data-ttu-id="c2533-125">Das HTML-Hilfsprogramm `DisplayNameFor` überprüft die Eigenschaft `Title`, auf die im Lambdaausdruck verwiesen wird, um den Anzeigenamen zu bestimmen.</span><span class="sxs-lookup"><span data-stu-id="c2533-125">The `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="c2533-126">Der Lambdaausdruck wird überprüft und nicht ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="c2533-126">The lambda expression is inspected rather than evaluated.</span></span> <span data-ttu-id="c2533-127">Das bedeutet, dass keine Zugriffsverletzung auftritt, wenn `model`, `model.Movie` oder `model.Movie[0]` `null` oder leer sind.</span><span class="sxs-lookup"><span data-stu-id="c2533-127">That means there is no access violation when `model`, `model.Movie`, or `model.Movie[0]` are `null` or empty.</span></span> <span data-ttu-id="c2533-128">Wenn der Lambdaausdruck ausgewertet wird, (z.B. mit `@Html.DisplayFor(modelItem => item.Title)`), werden die Eigenschaftswerte ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="c2533-128">When the lambda expression is evaluated (for example, with `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<a name="md"></a>
### <a name="the-model-directive"></a><span data-ttu-id="c2533-129">Die @model-Direktive</span><span class="sxs-lookup"><span data-stu-id="c2533-129">The @model directive</span></span>

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

<span data-ttu-id="c2533-130">Die `@model`-Anweisung gibt den Typ des Modells an, das an die Razor-Seite weitergegeben wird.</span><span class="sxs-lookup"><span data-stu-id="c2533-130">The `@model` directive specifies the type of the model passed to the Razor Page.</span></span> <span data-ttu-id="c2533-131">Im vorherigen Beispiel macht die `@model`-Linie die von `PageModel` abgeleitete Klasse der Razor-Seite verfügbar.</span><span class="sxs-lookup"><span data-stu-id="c2533-131">In the preceding example, the `@model` line makes the `PageModel`-derived class available to the Razor Page.</span></span> <span data-ttu-id="c2533-132">Das Modell wird in den [HTML Helpers (HTML-Hilfsprogrammen)](https://docs.microsoft.com/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) `@Html.DisplayNameFor` und `@Html.DisplayName` auf der Seite verwendet.</span><span class="sxs-lookup"><span data-stu-id="c2533-132">The model is used in the `@Html.DisplayNameFor` and `@Html.DisplayName` [HTML Helpers](https://docs.microsoft.com/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) on the page.</span></span>

<span data-ttu-id="c2533-133"><!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
### ViewData und Layout</span><span class="sxs-lookup"><span data-stu-id="c2533-133"><!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
### ViewData and layout</span></span>

<span data-ttu-id="c2533-134">Betrachten Sie folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="c2533-134">Consider the following code:</span></span>

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-)]

<span data-ttu-id="c2533-135">Der oben markierte Code ist ein Beispiel vom Übergang von Razor in C#.</span><span class="sxs-lookup"><span data-stu-id="c2533-135">The preceding highlighted code is an example of Razor transitioning into C#.</span></span> <span data-ttu-id="c2533-136">Die Zeichen `{` und `}` schließen einen Block mit C#-Code ein.</span><span class="sxs-lookup"><span data-stu-id="c2533-136">The `{` and `}` characters enclose a block of C# code.</span></span>

<span data-ttu-id="c2533-137">Die Basisklasse `PageModel` verfügt über eine `ViewData`-Wörterbucheigenschaft, die zum Hinzufügen von Daten verwendet werden kann, die an eine Ansicht übergeben werden sollen.</span><span class="sxs-lookup"><span data-stu-id="c2533-137">The `PageModel` base class has a `ViewData` dictionary property that can be used to add data that you want to pass to a View.</span></span> <span data-ttu-id="c2533-138">Mithilfe eines Schlüssel-Wert-Musters können dem `ViewData`-Wörterbuch Objekte hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="c2533-138">You add objects into the `ViewData` dictionary using a key/value pattern.</span></span> <span data-ttu-id="c2533-139">Im vorherigen Beispiel wurde dem `ViewData`-Wörterbuch die Eigenschaft „Title“ hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="c2533-139">In the preceding sample, the "Title" property is added to the `ViewData` dictionary.</span></span> <span data-ttu-id="c2533-140">Die Eigenschaft „Title“ wird in der *Pages/_Layout.cshtml*-Datei verwendet.</span><span class="sxs-lookup"><span data-stu-id="c2533-140">The "Title" property is used in the *Pages/_Layout.cshtml* file.</span></span> <span data-ttu-id="c2533-141">Das folgende Markup zeigt die ersten Zeilen der *Pages/_Layout.cshtml*-Datei.</span><span class="sxs-lookup"><span data-stu-id="c2533-141">The following markup shows the first few lines of the *Pages/_Layout.cshtml* file.</span></span>

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout1.cshtml?highlight=6-)]

<span data-ttu-id="c2533-142">Die Zeile `@*Markup removed for brevity.*@` ist ein Razor-Kommentar.</span><span class="sxs-lookup"><span data-stu-id="c2533-142">The line `@*Markup removed for brevity.*@` is a Razor comment.</span></span> <span data-ttu-id="c2533-143">Im Gegensatz zu HTML-Kommentaren (`<!-- -->`) werden Razor-Kommentare nicht an den Client gesendet.</span><span class="sxs-lookup"><span data-stu-id="c2533-143">Unlike HTML comments (`<!-- -->`), Razor comments are not sent to the client.</span></span>

<span data-ttu-id="c2533-144">Führen Sie die App aus, und testen Sie die Links im Projekt (**Home**, **Details**, **Contact** (Kontakt), **Create** (Erstellen), **Edit** (Bearbeiten) und **Delete** (Löschen)).</span><span class="sxs-lookup"><span data-stu-id="c2533-144">Run the app and test the links in the project (**Home**, **About**, **Contact**, **Create**, **Edit**, and **Delete**).</span></span> <span data-ttu-id="c2533-145">Jede Seite legt den Titel fest, der in der Browserregisterkarte angezeigt wird. Wenn Sie eine Seite mit einem Lesezeichen versehen, wird dieser Titel für das Lesezeichen verwendet.</span><span class="sxs-lookup"><span data-stu-id="c2533-145">Each page sets the title, which you can see in the browser tab. When you bookmark a page, the title is used for the bookmark.</span></span> <span data-ttu-id="c2533-146">*Pages/Index.cshtml* und *Pages/Movies/Index.cshtml* haben momentan denselben Titel, sie können aber verändert werden, um ihnen verschiedene Werte zu geben.</span><span class="sxs-lookup"><span data-stu-id="c2533-146">*Pages/Index.cshtml* and *Pages/Movies/Index.cshtml* currently have the same title, but you can modify them to have different values.</span></span>

<span data-ttu-id="c2533-147">Die Eigenschaft `Layout` wird in der Datei *Pages/_ViewStart.cshtml* festgelegt:</span><span class="sxs-lookup"><span data-stu-id="c2533-147">The `Layout` property is set in the *Pages/_ViewStart.cshtml* file:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_ViewStart.cshtml)]

<span data-ttu-id="c2533-148">Das vorhergehende Markup legt die Layoutdatei für alle Razor-Dateien unter dem Ordner *Seiten* auf *Pages/_Layout.cshtml* fest.</span><span class="sxs-lookup"><span data-stu-id="c2533-148">The preceding markup sets the layout file to *Pages/_Layout.cshtml* for all Razor files under the *Pages* folder.</span></span> <span data-ttu-id="c2533-149">Weitere Informationen finden Sie unter [Layout](xref:mvc/razor-pages/index#layout).</span><span class="sxs-lookup"><span data-stu-id="c2533-149">See [Layout](xref:mvc/razor-pages/index#layout) for more information.</span></span>

### <a name="update-the-layout"></a><span data-ttu-id="c2533-150">Aktualisieren des Layouts</span><span class="sxs-lookup"><span data-stu-id="c2533-150">Update the layout</span></span>

<span data-ttu-id="c2533-151">Ändern Sie das Element `<title>` in der Datei *Pages/_Layout.cshtml*, damit es eine kürzere Zeichenfolge verwendet.</span><span class="sxs-lookup"><span data-stu-id="c2533-151">Change the `<title>` element in the *Pages/_Layout.cshtml* file to use a shorter string.</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=1-6&highlight=6)]

<span data-ttu-id="c2533-152">Suchen Sie in der Datei *Pages/_Layout.cshtml* das folgende Ankerelement.</span><span class="sxs-lookup"><span data-stu-id="c2533-152">Find the following anchor element in the *Pages/_Layout.cshtml* file.</span></span>

```cshtml
<a asp-page="/Index" class="navbar-brand">RazorPagesMovie</a>
```
<span data-ttu-id="c2533-153">Ersetzen Sie das vorhergehende Element durch das folgende Markup.</span><span class="sxs-lookup"><span data-stu-id="c2533-153">Replace the preceding element with the following markup.</span></span>

```cshtml
<a asp-page="/Movies/Index" class="navbar-brand">RpMovie</a>
```

<span data-ttu-id="c2533-154">Das vorangehende Ankerelement ist ein [Tag Helper (Taghilfsprogramm)](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="c2533-154">The preceding anchor element is a [Tag Helper](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="c2533-155">In diesem Fall handelt es sich um ein [Anchor Tag Helper (Anchor-Taghilfsprogramm)](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="c2533-155">In this case, it's the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span></span> <span data-ttu-id="c2533-156">Das Taghilfsattribut und der -wert `asp-page="/Movies/Index"` erstellt einen Link zur Razor-Seite `/Movies/Index`.</span><span class="sxs-lookup"><span data-stu-id="c2533-156">The `asp-page="/Movies/Index"` Tag Helper attribute and value creates a link to the `/Movies/Index` Razor Page.</span></span>

<span data-ttu-id="c2533-157">Speichern Sie Ihre Änderungen, und testen Sie die App, indem Sie auf den Link **RpMovie** klicken.</span><span class="sxs-lookup"><span data-stu-id="c2533-157">Save your changes, and test the app by clicking on the **RpMovie** link.</span></span> <span data-ttu-id="c2533-158">Weitere Informationen finden Sie in der Datei [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml) in GitHub.</span><span class="sxs-lookup"><span data-stu-id="c2533-158">See the [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml) file in GitHub.</span></span>

### <a name="the-create-code-behind-page"></a><span data-ttu-id="c2533-159">Die CodeBehind-Seite „Create“</span><span class="sxs-lookup"><span data-stu-id="c2533-159">The Create code-behind page</span></span>

<span data-ttu-id="c2533-160">Betrachten Sie die *Pages/Movies/Create.cshtml.cs*-CodeBehind-Datei:</span><span class="sxs-lookup"><span data-stu-id="c2533-160">Examine the *Pages/Movies/Create.cshtml.cs* code-behind file:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

<span data-ttu-id="c2533-161">Die Methode `OnGet` initialisiert einen beliebigen Zustand, der für die Seite erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="c2533-161">The `OnGet` method initializes any state needed for the page.</span></span> <span data-ttu-id="c2533-162">Die Seite „Create“ verfügt nicht über einen initialisierbaren Zustand.</span><span class="sxs-lookup"><span data-stu-id="c2533-162">The Create page doesn't have any state to initialize.</span></span> <span data-ttu-id="c2533-163">Die Methode `Page` erstellt ein `PageResult`-Objekt, das die Seite *Create.cshtml* rendert.</span><span class="sxs-lookup"><span data-stu-id="c2533-163">The `Page` method creates a `PageResult` object that renders the *Create.cshtml* page.</span></span>

<span data-ttu-id="c2533-164">Die Eigenschaft `Movie` verwendet das `[BindProperty]`-Attribut, um die [Modellbindung](xref:mvc/models/model-binding) zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="c2533-164">The `Movie` property uses the `[BindProperty]` attribute to opt-in to [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="c2533-165">Wenn das Formular „Create“ die Formularwerte bereitstellt, bindet die ASP.NET Core-Laufzeit die bereitgestellten Werte an das `Movie`-Modell.</span><span class="sxs-lookup"><span data-stu-id="c2533-165">When the Create form posts the form values, the ASP.NET Core runtime binds the posted values to the `Movie` model.</span></span>

<span data-ttu-id="c2533-166">Die Methode `OnPostAsync` wird ausgeführt, wenn die Seite Formulardaten bereitstellt:</span><span class="sxs-lookup"><span data-stu-id="c2533-166">The `OnPostAsync` method is run when the page posts form data:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

<span data-ttu-id="c2533-167">Wenn es zu Modellfehlern kommt, wird das Formular mit allen bereitgestellten Formulardaten erneut angezeigt.</span><span class="sxs-lookup"><span data-stu-id="c2533-167">If there are any model errors, the form is redisplayed, along with any form data posted.</span></span> <span data-ttu-id="c2533-168">Die meisten Modellfehler können clientseitig abgefangen werden, bevor das Formular bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="c2533-168">Most model errors can be caught on the client-side before the form is posted.</span></span> <span data-ttu-id="c2533-169">Ein Beispiel für einen Modellfehler ist die Bereitstellung eines Werts für das Datumsfeld, der nicht in ein Datum konvertiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="c2533-169">An example of a model error is posting a value for the date field that cannot be converted to a date.</span></span> <span data-ttu-id="c2533-170">Später in diesem Tutorial erfahren Sie mehr über die clientseitige Validierung und Modellvalidierung.</span><span class="sxs-lookup"><span data-stu-id="c2533-170">We'll talk more about client-side validation and model validation later in the tutorial.</span></span>

<span data-ttu-id="c2533-171">Wenn keine Modellfehler auftreten, werden die Daten gespeichert und der Browser zur Indexseite umgeleitet.</span><span class="sxs-lookup"><span data-stu-id="c2533-171">If there are no model errors, the data is saved, and the browser is redirected to the Index page.</span></span>

### <a name="the-create-razor-page"></a><span data-ttu-id="c2533-172">Die Razor-Seite „Create“</span><span class="sxs-lookup"><span data-stu-id="c2533-172">The Create Razor Page</span></span>

<span data-ttu-id="c2533-173">Betrachten Sie die Datei *Pages/Movies/Create.cshtml* der Razor-Seite:</span><span class="sxs-lookup"><span data-stu-id="c2533-173">Examine the *Pages/Movies/Create.cshtml* Razor Page file:</span></span>

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

<span data-ttu-id="c2533-174">Visual Studio zeigt den Tag `<form method="post">` in einer bestimmten Schriftart an, die für Taghilfsprogramme verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="c2533-174">Visual Studio displays the `<form method="post">` tag in a distinctive font used for Tag Helpers.</span></span> <span data-ttu-id="c2533-175">Das Element `<form method="post">` ist ein [Form Tag Helper (Hilfsprogramm für Formulartags)](xref:mvc/views/working-with-forms#the-form-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="c2533-175">The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="c2533-176">Das Hilfsprogramm für Formulartags enthält automatisch ein [antiforgery token (Antifälschungstoken)](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="c2533-176">The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).</span></span>

![VS17-Ansicht der Create.cshtml-Seite](page/_static/th.png)

<span data-ttu-id="c2533-178">Das Gerüstbaumodul erstellt Razor-Markup für jedes Feld im Modell (ausgenommen die ID) ähnlich wie folgt:</span><span class="sxs-lookup"><span data-stu-id="c2533-178">The scaffolding engine creates Razor markup for each field in the model (except the ID) similar to the following:</span></span>

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=15-20)]

<span data-ttu-id="c2533-179">Das [Hilfsprogramm für Validierungstags](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` und ` <span asp-validation-for`) zeigt Validierungsfehler.</span><span class="sxs-lookup"><span data-stu-id="c2533-179">The [Validation Tag Helpers](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` and ` <span asp-validation-for`) display validation errors.</span></span> <span data-ttu-id="c2533-180">Die Validierung wird an späterer Stelle in dieser Reihe detaillierter beschrieben.</span><span class="sxs-lookup"><span data-stu-id="c2533-180">Validation is covered in more detail later in this series.</span></span>

<span data-ttu-id="c2533-181">Das [Label Tag Helper (Taghilfsprogramm für Label)](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) generiert den Titel des Labels und das `for`-Attribut für die Eigenschaft `Title`.</span><span class="sxs-lookup"><span data-stu-id="c2533-181">The [Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) generates the label caption and `for` attribute for the `Title` property.</span></span>

<span data-ttu-id="c2533-182">Das [Hilfsprogramm für Eingabetags](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control" />`) verwendet die Attribute von [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) und generiert HTML-Attribute, die auf der Clientseite für die jQuery-Validierung erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="c2533-182">The [Input Tag Helper](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control" />`) uses the [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) attributes and produces HTML attributes needed for jQuery Validation on the client-side.</span></span>

<span data-ttu-id="c2533-183">Im nächsten Tutorial werden SQL Server LocalDB und das Seeding der Datenbank erläutert.</span><span class="sxs-lookup"><span data-stu-id="c2533-183">The next tutorial explains SQL Server LocalDB and seeding the database.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="c2533-184">[Vorheriges Thema: Adding a model (Hinzufügen eines Modells)](xref:tutorials/razor-pages/model)
[Nächstes Thema: SQL Server LocalDB](xref:tutorials/razor-pages/sql)</span><span class="sxs-lookup"><span data-stu-id="c2533-184">[Previous: Adding a model](xref:tutorials/razor-pages/model)
[Next: SQL Server LocalDB](xref:tutorials/razor-pages/sql)</span></span>
