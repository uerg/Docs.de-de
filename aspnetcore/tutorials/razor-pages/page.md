---
title: Gerüstbau mit Razor Pages in ASP.NET Core
author: rick-anderson
description: Gibt nähere Informationen über durch Gerüstbau erstellte Razor Pages.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 12/4/2018
uid: tutorials/razor-pages/page
ms.openlocfilehash: acfc446732803c67714943fe3e5b7a31055ebcd7
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2018
ms.locfileid: "52862004"
---
# <a name="scaffolded-razor-pages-in-aspnet-core"></a><span data-ttu-id="fa6ea-103">Gerüstbau mit Razor Pages in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fa6ea-103">Scaffolded Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="fa6ea-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fa6ea-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="fa6ea-105">In diesem Tutorial werden die Razor Pages näher betrachtet, die durch Gerüstbau im vorherigen Tutorial erstellt wurden.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-105">This tutorial examines the Razor Pages created by scaffolding in the previous tutorial.</span></span>

<span data-ttu-id="fa6ea-106">Beispiel [Anzeigen oder Herunterladen](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22).</span><span class="sxs-lookup"><span data-stu-id="fa6ea-106">[View or download](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22) sample.</span></span>

## <a name="the-create-delete-details-and-edit-pages"></a><span data-ttu-id="fa6ea-107">Die Seiten „Create“, „Delete“, „Details“ und „Edit“</span><span class="sxs-lookup"><span data-stu-id="fa6ea-107">The Create, Delete, Details, and Edit pages</span></span>

<span data-ttu-id="fa6ea-108">Sehen Sie sich das Seitenmodell *Pages/Movies/Index.cshtml.cs* an:</span><span class="sxs-lookup"><span data-stu-id="fa6ea-108">Examine the *Pages/Movies/Index.cshtml.cs* Page Model:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]

<span data-ttu-id="fa6ea-109">Razor Pages werden von `PageModel` abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-109">Razor Pages are derived from `PageModel`.</span></span> <span data-ttu-id="fa6ea-110">Gemäß der Konvention wird die von `PageModel` abgeleitete Klasse `<PageName>Model` genannt.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-110">By convention, the `PageModel`-derived class is called `<PageName>Model`.</span></span> <span data-ttu-id="fa6ea-111">Der Konstruktor verwendet [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection), um `RazorPagesMovieContext` zur Seite hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-111">The constructor uses [dependency injection](xref:fundamentals/dependency-injection) to add the `RazorPagesMovieContext` to the page.</span></span> <span data-ttu-id="fa6ea-112">Alle per Gerüstbau erstellten Seiten folgen diesem Muster.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-112">All the scaffolded pages follow this pattern.</span></span> <span data-ttu-id="fa6ea-113">Weitere Informationen zum asynchronen Programmieren mit Entity Framework finden Sie unter [Asynchroner Code](xref:data/ef-rp/intro#asynchronous-code).</span><span class="sxs-lookup"><span data-stu-id="fa6ea-113">See [Asynchronous code](xref:data/ef-rp/intro#asynchronous-code) for more information on asynchronous programing with Entity Framework.</span></span>

<span data-ttu-id="fa6ea-114">Wenn eine Anforderung an die Seite erfolgt, gibt die Methode `OnGetAsync` eine Filmliste an die Razor Page zurück.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-114">When a request is made for the page, the `OnGetAsync` method returns a list of movies to the Razor Page.</span></span> <span data-ttu-id="fa6ea-115">`OnGetAsync` oder `OnGet` werden für eine Razor Page aufgerufen, um den Zustand der Seite zu initialisieren.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-115">`OnGetAsync` or `OnGet` is called on a Razor Page to initialize the state for the page.</span></span> <span data-ttu-id="fa6ea-116">In diesem Fall ruft `OnGetAsync` eine Liste von Filmen ab und zeigt diese an.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-116">In this case, `OnGetAsync` gets a list of movies and displays them.</span></span>

<span data-ttu-id="fa6ea-117">Wenn `OnGet` `void` und `OnGetAsync` `Task` zurückgibt, wird keine Rückgabemethode verwendet.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-117">When `OnGet` returns `void` or `OnGetAsync` returns`Task`, no return method is used.</span></span> <span data-ttu-id="fa6ea-118">Wenn der Rückgabetyp `IActionResult` oder `Task<IActionResult>` ist, muss eine Rückgabeanweisung bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-118">When the return type is `IActionResult` or `Task<IActionResult>`, a return statement must be provided.</span></span> <span data-ttu-id="fa6ea-119">Die kann z.B. die `OnPostAsync`-Methode *Pages/Movies/Create.cshtml.cs* sein:</span><span class="sxs-lookup"><span data-stu-id="fa6ea-119">For example, the *Pages/Movies/Create.cshtml.cs* `OnPostAsync` method:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml.cs?name=snippet)]

<a name="index"></a> <span data-ttu-id="fa6ea-120">Sehen Sie sich die Razor-Seite *Pages/Movies/Index.cshtml* an:</span><span class="sxs-lookup"><span data-stu-id="fa6ea-120">Examine the *Pages/Movies/Index.cshtml* Razor Page:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

<span data-ttu-id="fa6ea-121">Razor kann aus HTML in C# oder Razor-spezifisches Markup übergehen.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-121">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="fa6ea-122">Wenn auf ein `@`-Symbol ein für Razor reserviertes Schlüsselwort ([Razor reserved keyword](xref:mvc/views/razor#razor-reserved-keywords)) folgt, wechselt es in Razor-spezifisches Markup. Andernfalls geht es in C# über.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-122">When an `@` symbol is followed by a [Razor reserved keyword](xref:mvc/views/razor#razor-reserved-keywords), it transitions into Razor-specific markup, otherwise it transitions into C#.</span></span>

<span data-ttu-id="fa6ea-123">Die Razor-Anweisung `@page` wandelt die Datei in eine MVC-Aktion um. Das bedeutet, dass sie Anforderungen verarbeiten kann.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-123">The `@page` Razor directive makes the file into an MVC action, which means that it can handle requests.</span></span> <span data-ttu-id="fa6ea-124">`@page` muss die erste Razor-Anweisung auf einer Seite sein.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-124">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="fa6ea-125">`@page` ist ein Beispiel für einen Übergang in Razor-spezifisches Markup.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-125">`@page` is an example of transitioning into Razor-specific markup.</span></span> <span data-ttu-id="fa6ea-126">Unter [Razor-Syntax](xref:mvc/views/razor#razor-syntax) finden Sie weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-126">See [Razor syntax](xref:mvc/views/razor#razor-syntax) for more information.</span></span>

<span data-ttu-id="fa6ea-127">Überprüfen Sie den Lambdaausdruck, der in der folgenden HTML-Hilfsfunktion verwendet wird:</span><span class="sxs-lookup"><span data-stu-id="fa6ea-127">Examine the lambda expression used in the following HTML Helper:</span></span>

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

<span data-ttu-id="fa6ea-128">Das HTML-Hilfsprogramm `DisplayNameFor` überprüft die Eigenschaft `Title`, auf die im Lambdaausdruck verwiesen wird, um den Anzeigenamen zu bestimmen.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-128">The `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="fa6ea-129">Der Lambdaausdruck wird überprüft und nicht ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-129">The lambda expression is inspected rather than evaluated.</span></span> <span data-ttu-id="fa6ea-130">Das bedeutet, dass keine Zugriffsverletzung auftritt, wenn `model`, `model.Movie` oder `model.Movie[0]` `null` oder leer sind.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-130">That means there is no access violation when `model`, `model.Movie`, or `model.Movie[0]` are `null` or empty.</span></span> <span data-ttu-id="fa6ea-131">Wenn der Lambdaausdruck ausgewertet wird, (z.B. mit `@Html.DisplayFor(modelItem => item.Title)`), werden die Eigenschaftswerte ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-131">When the lambda expression is evaluated (for example, with `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<a name="md"></a>
### <a name="the-model-directive"></a><span data-ttu-id="fa6ea-132">Die @model -Direktive</span><span class="sxs-lookup"><span data-stu-id="fa6ea-132">The @model directive</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

<span data-ttu-id="fa6ea-133">Die `@model`-Anweisung gibt den Typ des Modells an, das an die Razor Page weitergegeben wird.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-133">The `@model` directive specifies the type of the model passed to the Razor Page.</span></span> <span data-ttu-id="fa6ea-134">Im vorherigen Beispiel macht die `@model`-Linie die von `PageModel` abgeleitete Klasse der Razor Page verfügbar.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-134">In the preceding example, the `@model` line makes the `PageModel`-derived class available to the Razor Page.</span></span> <span data-ttu-id="fa6ea-135">Das Modell wird in den [HTML Helpers (HTML-Hilfsprogrammen)](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) `@Html.DisplayNameFor` und `@Html.DisplayFor` auf der Seite verwendet.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-135">The model is used in the `@Html.DisplayNameFor` and `@Html.DisplayFor` [HTML Helpers](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) on the page.</span></span>

<a name="vd"></a>
### <a name="viewdata-and-layout"></a><span data-ttu-id="fa6ea-136">ViewData und Layout</span><span class="sxs-lookup"><span data-stu-id="fa6ea-136">ViewData and layout</span></span>

<span data-ttu-id="fa6ea-137">Sehen Sie sich den folgenden Code aus der Datei *Pages/Movies/Index.cshtml* an:</span><span class="sxs-lookup"><span data-stu-id="fa6ea-137">Consider the following code from the *Pages/Movies/Index.cshtml* file:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

<span data-ttu-id="fa6ea-138">Der oben markierte Code ist ein Beispiel vom Übergang von Razor in C#.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-138">The preceding highlighted code is an example of Razor transitioning into C#.</span></span> <span data-ttu-id="fa6ea-139">Die Zeichen `{` und `}` schließen einen Block mit C#-Code ein.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-139">The `{` and `}` characters enclose a block of C# code.</span></span>

<span data-ttu-id="fa6ea-140">Die Basisklasse `PageModel` verfügt über eine `ViewData`-Wörterbucheigenschaft, die zum Hinzufügen von Daten verwendet werden kann, die an eine Ansicht übergeben werden sollen.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-140">The `PageModel` base class has a `ViewData` dictionary property that can be used to add data that you want to pass to a View.</span></span> <span data-ttu-id="fa6ea-141">Mithilfe eines Schlüssel-Wert-Musters können dem `ViewData`-Wörterbuch Objekte hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-141">You add objects into the `ViewData` dictionary using a key/value pattern.</span></span> <span data-ttu-id="fa6ea-142">Im vorherigen Beispiel wurde dem `ViewData`-Wörterbuch die Eigenschaft „Title“ hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-142">In the preceding sample, the "Title" property is added to the `ViewData` dictionary.</span></span> 

<span data-ttu-id="fa6ea-143">Die Eigenschaft „Title“ wird in der Datei *Pages/Shared/_Layout.cshtml* verwendet.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-143">The "Title" property is used in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="fa6ea-144">Das folgende Markup zeigt die ersten Zeilen der Datei *Pages/_Layout.cshtml* an.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-144">The following markup shows the first few lines of the *_Layout.cshtml* file.</span></span>

<!-- we need a snapshot copy of layout because we are
changing in in the next step. 
-->
[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout.cshtml?highlight=6-99)]

<span data-ttu-id="fa6ea-145">Die Zeile `@*Markup removed for brevity.*@` ist ein Razor-Kommentar, der in Ihrer Layoutdatei nicht angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-145">The line `@*Markup removed for brevity.*@` is a Razor comment which doesn't appear in your layout file.</span></span> <span data-ttu-id="fa6ea-146">Im Gegensatz zu HTML-Kommentaren (`<!-- -->`) werden Razor-Kommentare nicht an den Client gesendet.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-146">Unlike HTML comments (`<!-- -->`), Razor comments are not sent to the client.</span></span>

### <a name="update-the-layout"></a><span data-ttu-id="fa6ea-147">Aktualisieren des Layouts</span><span class="sxs-lookup"><span data-stu-id="fa6ea-147">Update the layout</span></span>

<span data-ttu-id="fa6ea-148">Ändern Sie das `<title>`-Element in der Datei *Pages/Shared/_Layout.cshtml*, um **Movie** anstelle von **RazorPagesMovie** anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-148">Change the `<title>` element in the *Pages/Shared/_Layout.cshtml* file display **Movie** rather than **RazorPagesMovie**.</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml?range=1-6&highlight=6)]


<span data-ttu-id="fa6ea-149">Suchen Sie in der Datei *Pages/Shared/_Layout.cshtml* das folgende Ankerelement.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-149">Find the following anchor element in the *Pages/Shared/_Layout.cshtml* file.</span></span>

```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Index">RazorPagesMovie</a>
```

<span data-ttu-id="fa6ea-150">Ersetzen Sie das vorhergehende Element durch das folgende Markup.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-150">Replace the preceding element with the following markup.</span></span>

```cshtml
<a class="navbar-brand" asp-page="/Movies/Index">RpMovie</a>
```

<span data-ttu-id="fa6ea-151">Das vorangehende Ankerelement ist ein [Tag Helper (Taghilfsprogramm)](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="fa6ea-151">The preceding anchor element is a [Tag Helper](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="fa6ea-152">In diesem Fall handelt es sich um ein [Anchor Tag Helper (Anchor-Taghilfsprogramm)](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="fa6ea-152">In this case, it's the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span></span> <span data-ttu-id="fa6ea-153">Das Taghilfsattribut und der -wert `asp-page="/Movies/Index"` erstellt einen Link zur Razor Page `/Movies/Index`.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-153">The `asp-page="/Movies/Index"` Tag Helper attribute and value creates a link to the `/Movies/Index` Razor Page.</span></span> <span data-ttu-id="fa6ea-154">Das `asp-area`-Attribut ist leer, daher wird der Bereich im Link nicht verwendet.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-154">The `asp-area` attribute value is empty, so the area isn't used in the link.</span></span> <span data-ttu-id="fa6ea-155">Weitere Informationen finden Sie unter [Bereiche](xref:mvc/controllers/areas).</span><span class="sxs-lookup"><span data-stu-id="fa6ea-155">See [Areas](xref:mvc/controllers/areas) for more information.</span></span>

<span data-ttu-id="fa6ea-156">Speichern Sie Ihre Änderungen, und testen Sie die App, indem Sie auf den Link **RpMovie** klicken.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-156">Save your changes, and test the app by clicking on the **RpMovie** link.</span></span> <span data-ttu-id="fa6ea-157">Wenn Probleme auftreten, sehen Sie sich die Datei [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml) in GitHub an.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-157">See the [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml) file in GitHub if you have any problems.</span></span>

<span data-ttu-id="fa6ea-158">Testen Sie die anderen Links – **Home** (Startseite), **RpMovie** (Razor Pages-Film), **Create** (Erstellen), **Edit** (Bearbeiten) und **Delete** (Löschen).</span><span class="sxs-lookup"><span data-stu-id="fa6ea-158">Test the other links (**Home**, **RpMovie**, **Create**, **Edit**, and **Delete**).</span></span> <span data-ttu-id="fa6ea-159">Jede Seite legt den Titel fest, der in der Browserregisterkarte angezeigt wird. Wenn Sie eine Seite mit einem Lesezeichen versehen, wird dieser Titel für das Lesezeichen verwendet.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-159">Each page sets the title, which you can see in the browser tab. When you bookmark a page, the title is used for the bookmark.</span></span> <span data-ttu-id="fa6ea-160">*Pages/Index.cshtml* und *Pages/Movies/Index.cshtml* haben momentan denselben Titel, sie können aber verändert werden, um ihnen verschiedene Werte zu geben.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-160">*Pages/Index.cshtml* and *Pages/Movies/Index.cshtml* currently have the same title, but you can modify them to have different values.</span></span>

> [!NOTE]
> <span data-ttu-id="fa6ea-161">Sie können unter Umständen in das Feld `Price` keine Kommas als Dezimaltrennzeichen eingeben.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-161">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="fa6ea-162">Zur Unterstützung der [jQuery-Validierung](https://jqueryvalidation.org/) für nicht englische Gebietsschemas, in denen ein Komma („,“) als Dezimaltrennzeichen verwendet wird, und Nicht-US-englische Datums- und Uhrzeitformate müssen Sie Schritte zur Globalisierung Ihrer App ausführen.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-162">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="fa6ea-163">In diesem [GitHub-Problem 4076](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420) finden Sie Anweisungen zum Hinzufügen von Kommas als Dezimaltrennzeichen.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-163">This [GitHub issue 4076](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420) for instructions on adding decimal comma.</span></span>

<span data-ttu-id="fa6ea-164">Die Eigenschaft `Layout` wird in der Datei *Pages/_ViewStart.cshtml* festgelegt:</span><span class="sxs-lookup"><span data-stu-id="fa6ea-164">The `Layout` property is set in the *Pages/_ViewStart.cshtml* file:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/_ViewStart.cshtml)]

<span data-ttu-id="fa6ea-165">Das vorhergehende Markup legt die Layoutdatei für alle Razor-Dateien unter dem Ordner *Pages* auf *Pages/Shared/_Layout.cshtml* fest.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-165">The preceding markup sets the layout file to *Pages/Shared/_Layout.cshtml* for all Razor files under the *Pages* folder.</span></span> <span data-ttu-id="fa6ea-166">Weitere Informationen finden Sie unter [Layout](xref:razor-pages/index#layout).</span><span class="sxs-lookup"><span data-stu-id="fa6ea-166">See [Layout](xref:razor-pages/index#layout) for more information.</span></span>

### <a name="the-create-page-model"></a><span data-ttu-id="fa6ea-167">Das Seitenmodell „Create“</span><span class="sxs-lookup"><span data-stu-id="fa6ea-167">The Create page model</span></span>

<span data-ttu-id="fa6ea-168">Überprüfen Sie das Seitenmodell *Pages/Movies/Create.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="fa6ea-168">Examine the *Pages/Movies/Create.cshtml.cs* page model:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

<span data-ttu-id="fa6ea-169">Die Methode `OnGet` initialisiert einen beliebigen Zustand, der für die Seite erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-169">The `OnGet` method initializes any state needed for the page.</span></span> <span data-ttu-id="fa6ea-170">Die Seite „Create“ (Erstellen) verfügt nicht über einen initialisierbaren Zustand. Deshalb wird `Page` zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-170">The Create page doesn't have any state to initialize, so `Page` is returned.</span></span> <span data-ttu-id="fa6ea-171">Im späteren Verlauf des Tutorials initialisiert die Methode `OnGet` einen Zustand.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-171">Later in the tutorial you see `OnGet` method initialize state.</span></span> <span data-ttu-id="fa6ea-172">Die Methode `Page` erstellt ein `PageResult`-Objekt, das die Seite *Create.cshtml* rendert.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-172">The `Page` method creates a `PageResult` object that renders the *Create.cshtml* page.</span></span>

<span data-ttu-id="fa6ea-173">Die Eigenschaft `Movie` verwendet das `[BindProperty]`-Attribut, um die [Modellbindung](xref:mvc/models/model-binding) zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-173">The `Movie` property uses the `[BindProperty]` attribute to opt-in to [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="fa6ea-174">Wenn das Formular „Create“ die Formularwerte bereitstellt, bindet die ASP.NET Core-Laufzeit die bereitgestellten Werte an das `Movie`-Modell.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-174">When the Create form posts the form values, the ASP.NET Core runtime binds the posted values to the `Movie` model.</span></span>

<span data-ttu-id="fa6ea-175">Die Methode `OnPostAsync` wird ausgeführt, wenn die Seite Formulardaten bereitstellt:</span><span class="sxs-lookup"><span data-stu-id="fa6ea-175">The `OnPostAsync` method is run when the page posts form data:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

<span data-ttu-id="fa6ea-176">Wenn es zu Modellfehlern kommt, wird das Formular mit allen bereitgestellten Formulardaten erneut angezeigt.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-176">If there are any model errors, the form is redisplayed, along with any form data posted.</span></span> <span data-ttu-id="fa6ea-177">Die meisten Modellfehler können clientseitig abgefangen werden, bevor das Formular bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-177">Most model errors can be caught on the client-side before the form is posted.</span></span> <span data-ttu-id="fa6ea-178">Ein Beispiel für einen Modellfehler ist die Bereitstellung eines Werts für das Datumsfeld, der nicht in ein Datum konvertiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-178">An example of a model error is posting a value for the date field that cannot be converted to a date.</span></span> <span data-ttu-id="fa6ea-179">Die clientseitige Validierung und die Modellvalidierung werden später in diesem Tutorial erläutert.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-179">Client-side validation and model validation are discussed later in the tutorial.</span></span>

<span data-ttu-id="fa6ea-180">Wenn keine Modellfehler auftreten, werden die Daten gespeichert und der Browser zur Indexseite umgeleitet.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-180">If there are no model errors, the data is saved, and the browser is redirected to the Index page.</span></span>

### <a name="the-create-razor-page"></a><span data-ttu-id="fa6ea-181">Die Razor Page „Create“</span><span class="sxs-lookup"><span data-stu-id="fa6ea-181">The Create Razor Page</span></span>

<span data-ttu-id="fa6ea-182">Betrachten Sie die Datei *Pages/Movies/Create.cshtml* der Razor Page:</span><span class="sxs-lookup"><span data-stu-id="fa6ea-182">Examine the *Pages/Movies/Create.cshtml* Razor Page file:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fa6ea-183">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fa6ea-183">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="fa6ea-184">Visual Studio zeigt das `<form method="post">`-Tag in einer bestimmten Schriftart an, die für Taghilfsprogramme verwendet wird:</span><span class="sxs-lookup"><span data-stu-id="fa6ea-184">Visual Studio displays the `<form method="post">` tag in a distinctive bold font used for Tag Helpers:</span></span>

<span data-ttu-id="fa6ea-185">![VS17-Ansicht der Seite „Create.cshtml“](page/_static/th.png)
<!-- Code --------------------------></span><span class="sxs-lookup"><span data-stu-id="fa6ea-185">![VS17 view of Create.cshtml page](page/_static/th.png)
<!-- Code --------------------------></span></span>
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="fa6ea-186">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="fa6ea-186">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="fa6ea-187">Weitere Informationen zu Taghilfsprogrammen wie `<form method="post">` finden Sie unter [Taghilfsprogramme in ASP.NET Core](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="fa6ea-187">For more information on Tag Helpers such as `<form method="post">`, see [Tag Helpers in ASP.NET Core](xref:mvc/views/tag-helpers/intro).</span></span>

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="fa6ea-188">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="fa6ea-188">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="fa6ea-189">Visual Studio für Mac zeigt das `<form method="post">`-Tag in einer bestimmten Schriftart an, die für Taghilfsprogramme verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-189">Visual Studio for Mac displays the `<form method="post">` tag in a distinctive bold font used for Tag Helpers.</span></span>

---  
<!-- End of VS tabs -->

<span data-ttu-id="fa6ea-190">Das Element `<form method="post">` ist ein [Form Tag Helper (Hilfsprogramm für Formulartags)](xref:mvc/views/working-with-forms#the-form-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="fa6ea-190">The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="fa6ea-191">Das Hilfsprogramm für Formulartags enthält automatisch ein [antiforgery token (Antifälschungstoken)](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="fa6ea-191">The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).</span></span>

<span data-ttu-id="fa6ea-192">Die Gerüstbau-Engine erstellt Razor-Markup für jedes Feld im Modell (ausgenommen die ID) ähnlich wie folgt:</span><span class="sxs-lookup"><span data-stu-id="fa6ea-192">The scaffolding engine creates Razor markup for each field in the model (except the ID) similar to the following:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=15-20)]

<span data-ttu-id="fa6ea-193">Das [Hilfsprogramm für Validierungstags](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` und ` <span asp-validation-for`) zeigt Validierungsfehler.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-193">The [Validation Tag Helpers](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` and ` <span asp-validation-for`) display validation errors.</span></span> <span data-ttu-id="fa6ea-194">Die Validierung wird an späterer Stelle in dieser Reihe detaillierter beschrieben.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-194">Validation is covered in more detail later in this series.</span></span>

<span data-ttu-id="fa6ea-195">Das [Label Tag Helper (Taghilfsprogramm für Label)](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) generiert den Titel des Labels und das `for`-Attribut für die Eigenschaft `Title`.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-195">The [Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) generates the label caption and `for` attribute for the `Title` property.</span></span>

<span data-ttu-id="fa6ea-196">Das [Hilfsprogramm für Eingabetags](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control" />`) verwendet die Attribute von [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) und generiert HTML-Attribute, die auf der Clientseite für die jQuery-Validierung erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="fa6ea-196">The [Input Tag Helper](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control" />`) uses the [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) attributes and produces HTML attributes needed for jQuery Validation on the client-side.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="fa6ea-197">[Zurück: Hinzufügen eines Modells](xref:tutorials/razor-pages/model)
> [Weiter: Datenbank](xref:tutorials/razor-pages/sql)</span><span class="sxs-lookup"><span data-stu-id="fa6ea-197">[Previous: Adding a model](xref:tutorials/razor-pages/model)
[Next: Data Base](xref:tutorials/razor-pages/sql)</span></span>
