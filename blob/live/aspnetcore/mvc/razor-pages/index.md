---
title: "Einführung in Razor-Seiten in ASP.NET Core"
author: Rick-Anderson
description: "Diese Dokumentation stellt eine Übersicht über die Verwendung von Razor-Seiten in ASP.NET Core bereit, um die Entwicklung von auf Seiten bezogene Szenarios zu vereinfachen."
keywords: ASP.NET Core, Razor-Seiten
ms.author: riande
manager: wpickett
ms.date: 09/12/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/razor-pages/index
ms.openlocfilehash: 36dd2ad01f93ab1093bad84a58504a150c70ea16
ms.sourcegitcommit: 4925a91ef4130ddb333f187ab13defe66f2c6cef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/03/2017
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a><span data-ttu-id="3d79b-104">Einführung in Razor-Seiten in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3d79b-104">Introduction to Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="3d79b-105">Von [Rick Anderson](https://twitter.com/RickAndMSFT) und [Ryan Nowak](https://github.com/rynowak)</span><span class="sxs-lookup"><span data-stu-id="3d79b-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="3d79b-106">Razor-Seiten ist ein neues Feature von ASP.NET Core MVC, mit dem codierungsseitige Szenarios einfacher und produktiver werden.</span><span class="sxs-lookup"><span data-stu-id="3d79b-106">Razor Pages is a new feature of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="3d79b-107">Weitere Informationen zu einem Tutorial, in dem der Model-View-Controller-Ansatz verwendet wird, finden Sie unter [Erste Schritte mit ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="3d79b-107">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Getting started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="3d79b-108">Dieses Dokument bietet eine Einführung in Razor-Seiten.</span><span class="sxs-lookup"><span data-stu-id="3d79b-108">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="3d79b-109">Es handelt sich nicht um ein Schritt-für-Schritt-Tutorial.</span><span class="sxs-lookup"><span data-stu-id="3d79b-109">It's not a step by step tutorial.</span></span> <span data-ttu-id="3d79b-110">Wenn es Ihnen Probleme bereitet, die Ausführungen in einigen Abschnitten nachzuvollziehen, lesen Sie [Erste Schritte mit Razor-Seiten](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="3d79b-110">If you find some of the sections difficult to follow, see [Getting started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span>

<a name="prerequisites"></a>

## <a name="aspnet-core-20-prerequisites"></a><span data-ttu-id="3d79b-111">Anforderungen von ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="3d79b-111">ASP.NET Core 2.0 prerequisites</span></span>

<span data-ttu-id="3d79b-112">Installieren Sie [.NET Core](https://www.microsoft.com/net/core) 2.0.0 oder höher.</span><span class="sxs-lookup"><span data-stu-id="3d79b-112">Install [.NET Core](https://www.microsoft.com/net/core) 2.0.0 or later.</span></span>

<span data-ttu-id="3d79b-113">Wenn Sie Visual Studio verwenden, installieren Sie [Visual Studio](https://www.visualstudio.com/vs/) 2017 Version 15.3 oder höher mit folgenden Workloads:</span><span class="sxs-lookup"><span data-stu-id="3d79b-113">If you're using Visual Studio, install [Visual Studio](https://www.visualstudio.com/vs/) 2017 version 15.3 or later with the following workloads:</span></span>

* <span data-ttu-id="3d79b-114">**ASP.NET und Webentwicklung**</span><span class="sxs-lookup"><span data-stu-id="3d79b-114">**ASP.NET and web development**</span></span>
* <span data-ttu-id="3d79b-115">**Plattformübergreifende .NET Core-Entwicklung**</span><span class="sxs-lookup"><span data-stu-id="3d79b-115">**.NET Core cross-platform development**</span></span>

<a name="rpvs17"></a>

## <a name="creating-a-razor-pages-project"></a><span data-ttu-id="3d79b-116">Erstellen eines Projekts mit Razor-Seiten</span><span class="sxs-lookup"><span data-stu-id="3d79b-116">Creating a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3d79b-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3d79b-117">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="3d79b-118">Weitere Informationen zum Erstellen eines Projekts mit Razor-Seiten mithilfe von Visual Studio finden Sie unter [Erste Schritte mit Razor-Seiten](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="3d79b-118">See [Getting started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project using Visual Studio.</span></span>

#   <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="3d79b-119">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="3d79b-119">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="3d79b-120">Führen Sie `dotnet new razor` über die Befehlszeile aus.</span><span class="sxs-lookup"><span data-stu-id="3d79b-120">Run `dotnet new razor` from the command line.</span></span>

<span data-ttu-id="3d79b-121">Öffnen Sie die generierte Datei *.csproj* aus Visual Studio für Mac.</span><span class="sxs-lookup"><span data-stu-id="3d79b-121">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3d79b-122">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3d79b-122">Visual Studio Code</span></span>](#tab/visual-studio-code) 

<span data-ttu-id="3d79b-123">Führen Sie `dotnet new razor` über die Befehlszeile aus.</span><span class="sxs-lookup"><span data-stu-id="3d79b-123">Run `dotnet new razor` from the command line.</span></span>

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="3d79b-124">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="3d79b-124">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="3d79b-125">Führen Sie `dotnet new razor` über die Befehlszeile aus.</span><span class="sxs-lookup"><span data-stu-id="3d79b-125">Run `dotnet new razor` from the command line.</span></span>

---

## <a name="razor-pages"></a><span data-ttu-id="3d79b-126">Razor-Seiten</span><span class="sxs-lookup"><span data-stu-id="3d79b-126">Razor Pages</span></span>

<span data-ttu-id="3d79b-127">Razor-Seiten ist in *Startup.cs* aktiviert:</span><span class="sxs-lookup"><span data-stu-id="3d79b-127">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[main](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

<span data-ttu-id="3d79b-128">Sehen Sie sich diese einfache Seite an: <a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="3d79b-128">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

<span data-ttu-id="3d79b-129">Der vorherige Code ähnelt stark einer Razor-Umgebungsdatei.</span><span class="sxs-lookup"><span data-stu-id="3d79b-129">The preceding code looks a lot like a Razor view file.</span></span> <span data-ttu-id="3d79b-130">Der Unterschied besteht in der `@page`-Anweisung.</span><span class="sxs-lookup"><span data-stu-id="3d79b-130">What makes it different is the `@page` directive.</span></span> <span data-ttu-id="3d79b-131">`@page` macht die Datei zu einer MVC-Aktion, d.h. dass Anfragen direkt ohne einen Controller verarbeitet werden.</span><span class="sxs-lookup"><span data-stu-id="3d79b-131">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="3d79b-132">`@page` muss die erste Razor-Anweisung auf einer Seite sein.</span><span class="sxs-lookup"><span data-stu-id="3d79b-132">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="3d79b-133">`@page` wirkt sich auf das Verhalten aller anderen Razor-Konstrukte aus.</span><span class="sxs-lookup"><span data-stu-id="3d79b-133">`@page` affects the behavior of other Razor constructs.</span></span>

<span data-ttu-id="3d79b-134">Eine ähnliche Seite, die die `PageModel`-Klasse verwendet, wird in den folgenden zwei Dateien angezeigt.</span><span class="sxs-lookup"><span data-stu-id="3d79b-134">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="3d79b-135">Die Datei *Pages/Index2.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="3d79b-135">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="3d79b-136">Die CodeBehind-Datei *Pages/Index2.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="3d79b-136">The *Pages/Index2.cshtml.cs* "code-behind" file:</span></span>

[!code-cs[main](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="3d79b-137">Die `PageModel`-Klassendatei hat standardmäßig den gleichen Namen wie die Datei mit Razor-Seiten, nur dass außerdem *cs* angefügt wird.</span><span class="sxs-lookup"><span data-stu-id="3d79b-137">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="3d79b-138">Die vorherige Datei mit Razor-Seiten lautet beispielsweise *Pages/Index2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="3d79b-138">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="3d79b-139">Die Datei mit der `PageModel`-Klasse heißt *Pages/Index2.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="3d79b-139">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="3d79b-140">Die Zuordnungen von URL-Pfaden zu Seiten werden durch den Speicherort der Seite im Dateisystem bestimmt.</span><span class="sxs-lookup"><span data-stu-id="3d79b-140">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="3d79b-141">Die folgende Tabelle zeigt einen Pfad zu Razor-Seiten und die entsprechende URL:</span><span class="sxs-lookup"><span data-stu-id="3d79b-141">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="3d79b-142">Dateiname und Pfad</span><span class="sxs-lookup"><span data-stu-id="3d79b-142">File name and path</span></span>               | <span data-ttu-id="3d79b-143">Entsprechende URL</span><span class="sxs-lookup"><span data-stu-id="3d79b-143">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="3d79b-144">*/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="3d79b-144">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="3d79b-145">`/` oder `/Index`</span><span class="sxs-lookup"><span data-stu-id="3d79b-145">`/` or `/Index`</span></span> |
| <span data-ttu-id="3d79b-146">*/Pages/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="3d79b-146">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="3d79b-147">*/Pages/Store/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="3d79b-147">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="3d79b-148">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="3d79b-148">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="3d79b-149">`/Store` oder `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="3d79b-149">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="3d79b-150">Notizen:</span><span class="sxs-lookup"><span data-stu-id="3d79b-150">Notes:</span></span>

* <span data-ttu-id="3d79b-151">Die Runtime sucht standardmäßig im Ordner *Pages* (Seiten) nach Dateien mit Razor-Seiten.</span><span class="sxs-lookup"><span data-stu-id="3d79b-151">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="3d79b-152">Wenn eine Seite nicht in einer URL enthalten ist, ist `Index` die Standardseite.</span><span class="sxs-lookup"><span data-stu-id="3d79b-152">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="writing-a-basic-form"></a><span data-ttu-id="3d79b-153">Schreiben eines einfachen Formulars</span><span class="sxs-lookup"><span data-stu-id="3d79b-153">Writing a basic form</span></span>

<span data-ttu-id="3d79b-154">Features von Razor-Seiten erstellen allgemeine Muster, die einfach mit Webbrowsern verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="3d79b-154">Razor Pages features are designed to make common patterns used with web browsers easy.</span></span> <span data-ttu-id="3d79b-155">Die [Modellbindung](xref:mvc/models/model-binding), [Taghilfsprogramme](xref:mvc/views/tag-helpers/intro) und alle HTML-Hilfsprogramme *funktionieren nur* mit den Eigenschaften, die in einer Klasse der Razor-Seiten definiert wurden.</span><span class="sxs-lookup"><span data-stu-id="3d79b-155">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="3d79b-156">Nehmen wir z.B. eine Seite, die ein allgemeines Kontaktformular für das `Contact`-Modell implementiert:</span><span class="sxs-lookup"><span data-stu-id="3d79b-156">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="3d79b-157">Für die Beispiele in diesem Dokument wird `DbContext` in der Datei [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) initialisiert.</span><span class="sxs-lookup"><span data-stu-id="3d79b-157">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) file.</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

<span data-ttu-id="3d79b-158">Das Datenmodell:</span><span class="sxs-lookup"><span data-stu-id="3d79b-158">The data model:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Data/Customer.cs)]

<span data-ttu-id="3d79b-159">Der db-Kontext:</span><span class="sxs-lookup"><span data-stu-id="3d79b-159">The db context:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

<span data-ttu-id="3d79b-160">Die Umgebungsdatei *Pages/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="3d79b-160">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

<span data-ttu-id="3d79b-161">Die CodeBehind-Datei *Pages/Create.cshtml.cs* für die Ansicht:</span><span class="sxs-lookup"><span data-stu-id="3d79b-161">The *Pages/Create.cshtml.cs* code-behind file for the view:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="3d79b-162">Die `PageModel`-Klasse heißt standardmäßig `<PageName>Model` und befindet sich im selben Namespace wie die Seite.</span><span class="sxs-lookup"><span data-stu-id="3d79b-162">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="3d79b-163">Für die Verwendung einer `PageModel`-CodeBehind-Datei werden Unittests unterstützt. Sie müssen aber einen expliziten Konstruktor und Klasse schreiben.</span><span class="sxs-lookup"><span data-stu-id="3d79b-163">Using a `PageModel` code-behind file supports unit testing, but requires you to write an explicit constructor and class.</span></span> <span data-ttu-id="3d79b-164">Seiten ohne `PageModel`-CodeBehind-Dateien unterstützen die Runtimekompilierung. Dies kann ein Vorteil bei der Entwicklung sein.</span><span class="sxs-lookup"><span data-stu-id="3d79b-164">Pages without `PageModel` code-behind files support runtime compilation, which can be an advantage in development.</span></span>  <!-- review: advantage because you can make changes and refresh the browser without explicitly compiling the app -->

<span data-ttu-id="3d79b-165">Die Seite hat eine `OnPostAsync`-*Handlermethode*, die auf `POST`-Anforderungen ausgeführt wird (wenn ein Benutzer das Formular sendet).</span><span class="sxs-lookup"><span data-stu-id="3d79b-165">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="3d79b-166">Sie können Handlermethoden für alle HTTP-Verben hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="3d79b-166">You can add handler methods for any HTTP verb.</span></span> <span data-ttu-id="3d79b-167">Die am häufigsten verwendeten Handler sind:</span><span class="sxs-lookup"><span data-stu-id="3d79b-167">The most common handlers are:</span></span>

* <span data-ttu-id="3d79b-168">`OnGet`, um den für eine Seite erforderlichen Status zu initialisieren.</span><span class="sxs-lookup"><span data-stu-id="3d79b-168">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="3d79b-169">[OnGet](#OnGet)-Beispiel</span><span class="sxs-lookup"><span data-stu-id="3d79b-169">[OnGet](#OnGet) sample.</span></span>
* <span data-ttu-id="3d79b-170">`OnPost`, um Formularübermittlungen zu behandeln</span><span class="sxs-lookup"><span data-stu-id="3d79b-170">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="3d79b-171">Das Namenssuffix `Async` ist optional. Es wird jedoch standardmäßig häufig für asynchrone Funktionen verwendet.</span><span class="sxs-lookup"><span data-stu-id="3d79b-171">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="3d79b-172">Der `OnPostAsync`-Code im vorhergehenden Beispiel ähnelt dem, was Sie normalerweise in einem Controller schreiben würden.</span><span class="sxs-lookup"><span data-stu-id="3d79b-172">The `OnPostAsync` code in the preceding example looks similar to what you would normally write in a controller.</span></span> <span data-ttu-id="3d79b-173">Der vorhergehende Code ist typisch für Razor-Seiten.</span><span class="sxs-lookup"><span data-stu-id="3d79b-173">The preceding code is typical for Razor Pages.</span></span> <span data-ttu-id="3d79b-174">Die meisten primitiven MVC-Typen wie die [Modellbindung](xref:mvc/models/model-binding), die [Validierung](xref:mvc/models/validation) und Aktionsergebnisse werden freigegeben.</span><span class="sxs-lookup"><span data-stu-id="3d79b-174">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results are shared.</span></span>  <!-- Review: Ryan, can we get a list of what is shared and what isn't? -->

<span data-ttu-id="3d79b-175">Die vorherige `OnPostAsync`-Methode:</span><span class="sxs-lookup"><span data-stu-id="3d79b-175">The previous `OnPostAsync` method:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="3d79b-176">Der grundlegende Ablauf von `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="3d79b-176">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="3d79b-177">Prüfen auf Validierungsfehler</span><span class="sxs-lookup"><span data-stu-id="3d79b-177">Check for validation errors.</span></span>

*  <span data-ttu-id="3d79b-178">Wenn keine Fehler vorliegen, werden die Daten gespeichert und weitergeleitet.</span><span class="sxs-lookup"><span data-stu-id="3d79b-178">If there are no errors, save the data and redirect.</span></span>
*  <span data-ttu-id="3d79b-179">Wenn es Fehler gibt, zeigen Sie die Seite erneut mit den Validierungsmeldungen an.</span><span class="sxs-lookup"><span data-stu-id="3d79b-179">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="3d79b-180">Die clientseitige Validierung ist identisch mit herkömmlichen ASP.NET Core MVC-Anwendungen,</span><span class="sxs-lookup"><span data-stu-id="3d79b-180">Client-side validation is identical to traditional ASP.NET Core MVC applications.</span></span> <span data-ttu-id="3d79b-181">denn Validierungsfehler werden oftmals auf dem Client erkannt und nie an den Server übermittelt.</span><span class="sxs-lookup"><span data-stu-id="3d79b-181">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="3d79b-182">Wenn die Daten erfolgreich eingegeben wurden, ruft die `OnPostAsync`-Handlermethode die `RedirectToPage`-Hilfsmethode auf, um eine Instanz von `RedirectToPageResult` zurückzugeben.</span><span class="sxs-lookup"><span data-stu-id="3d79b-182">When the data is entered successfully, the `OnPostAsync` handler method calls the `RedirectToPage` helper method to return an instance of `RedirectToPageResult`.</span></span> <span data-ttu-id="3d79b-183">`RedirectToPage` ist ein neues Aktionsergebnis und ähnelt `RedirectToAction` oder `RedirectToRoute`, ist aber für Seiten angepasst.</span><span class="sxs-lookup"><span data-stu-id="3d79b-183">`RedirectToPage` is a new action result, similar to `RedirectToAction` or `RedirectToRoute`, but customized for pages.</span></span> <span data-ttu-id="3d79b-184">Im vorhergehenden Beispiel leitet es an die Stammindexseite (`/Index`) weiter.</span><span class="sxs-lookup"><span data-stu-id="3d79b-184">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="3d79b-185">Informationen zu `RedirectToPage` finden Sie im Abschnitt [URL-Generierung für Seiten](#url_gen).</span><span class="sxs-lookup"><span data-stu-id="3d79b-185">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

<span data-ttu-id="3d79b-186">Wenn das übermittelte Formular Validierungsfehler enthält (die an den Server übergeben wurden), ruft die `OnPostAsync`-Handlermethode die `Page`-Hilfsmethode auf.</span><span class="sxs-lookup"><span data-stu-id="3d79b-186">When the submitted form has validation errors (that are passed to the server), the`OnPostAsync` handler method calls the `Page` helper method.</span></span> <span data-ttu-id="3d79b-187">`Page` gibt eine Instanz von `PageResult` zurück.</span><span class="sxs-lookup"><span data-stu-id="3d79b-187">`Page` returns an instance of `PageResult`.</span></span> <span data-ttu-id="3d79b-188">Der Vorgang, bei dem `Page` zurückgegeben wird, ähnelt dem Vorgang, bei dem Aktionen im Controller `View` zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="3d79b-188">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="3d79b-189">`PageResult` ist der standardmäßige <!-- Review  -->-Rückgabetyp für eine Handlermethode.</span><span class="sxs-lookup"><span data-stu-id="3d79b-189">`PageResult` is the default <!-- Review  --> return type for a handler method.</span></span> <span data-ttu-id="3d79b-190">Eine Handlermethode, die `void` zurückgibt, rendert die Seite.</span><span class="sxs-lookup"><span data-stu-id="3d79b-190">A handler method that returns `void` renders the page.</span></span>

<span data-ttu-id="3d79b-191">Die Eigenschaft `Customer` verwendet das `[BindProperty]`-Attribut, um die Modellbindung zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="3d79b-191">The `Customer` property uses `[BindProperty]` attribute to opt in to model binding.</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

<span data-ttu-id="3d79b-192">Razor-Seiten binden Eigenschaften standardmäßig nur an Nicht-GET-Verben.</span><span class="sxs-lookup"><span data-stu-id="3d79b-192">Razor Pages, by default, bind properties only with non-GET verbs.</span></span> <span data-ttu-id="3d79b-193">Durch die Bindung an Eigenschaften können Sie den Umfang von Codes reduzieren, den Sie schreiben müssen.</span><span class="sxs-lookup"><span data-stu-id="3d79b-193">Binding to properties can reduce the amount of code you have to write.</span></span> <span data-ttu-id="3d79b-194">Die Bindung reduziert den Code mithilfe der gleichen Eigenschaft, um Formularfelder (`<input asp-for="Customer.Name" />`) zu rendern und die Eingabe zu akzeptieren.</span><span class="sxs-lookup"><span data-stu-id="3d79b-194">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name" />`) and accept the input.</span></span>

<span data-ttu-id="3d79b-195">Die Startseite (*Index.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="3d79b-195">The home page (*Index.cshtml*):</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

<span data-ttu-id="3d79b-196">Die CodeBehind-Datei *Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="3d79b-196">The code behind *Index.cshtml.cs* file:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

<span data-ttu-id="3d79b-197">Die Datei *Index.cshtml* enthält das folgende Markup, um einen Bearbeitungslink für jeden Kontakt zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="3d79b-197">The *Index.cshtml* file contains the following markup to create an edit link for each contact:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

<span data-ttu-id="3d79b-198">Das [Anchor-Taghilfsprogramm](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) verwendet das Attribut [asp-route-{Wert}](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#route), um einen Link zur Bearbeitungsseite zu generieren.</span><span class="sxs-lookup"><span data-stu-id="3d79b-198">The [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the [asp-route-{value}](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#route) attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="3d79b-199">Der Link enthält die Routendaten mit der Kontakt-ID.</span><span class="sxs-lookup"><span data-stu-id="3d79b-199">The link contains route data with the contact ID.</span></span> <span data-ttu-id="3d79b-200">Beispielsweise `http://localhost:5000/Edit/1`.</span><span class="sxs-lookup"><span data-stu-id="3d79b-200">For example, `http://localhost:5000/Edit/1`.</span></span>

<span data-ttu-id="3d79b-201">Die Datei *Pages/Edit.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="3d79b-201">The *Pages/Edit.cshtml* file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

<span data-ttu-id="3d79b-202">Die erste Zeile enthält die `@page "{id:int}"`-Anweisung.</span><span class="sxs-lookup"><span data-stu-id="3d79b-202">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="3d79b-203">Die Routingbeschränkung `"{id:int}"` weist die Seite an, die Anforderungen für die Seite zu akzeptieren, die `int`-Routingdaten enthalten.</span><span class="sxs-lookup"><span data-stu-id="3d79b-203">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="3d79b-204">Wenn eine Anforderung an die Seite bestimmte Routingdaten nicht enthält, die in einen `int` konvertiert werden können, gibt die Runtime einen Fehler vom Typ „HTTP 404: Nicht gefunden“ zurück.</span><span class="sxs-lookup"><span data-stu-id="3d79b-204">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span>

<span data-ttu-id="3d79b-205">Die Datei *Pages/Edit.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="3d79b-205">The *Pages/Edit.cshtml.cs* file:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

<span data-ttu-id="3d79b-206">Die Datei *index.cshtml* enthält auch Markup zum Erstellen der Schaltfläche „Löschen“ für jeden benutzerdefinierten Kontakt:</span><span class="sxs-lookup"><span data-stu-id="3d79b-206">The *Index.cshtml* file also contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

<span data-ttu-id="3d79b-207">Wenn die „Löschen“-Schaltfläche in HTML gerendert wird, enthält ihr `formaction`-Element Parameter für Folgendes:</span><span class="sxs-lookup"><span data-stu-id="3d79b-207">When the delete button is rendered in HTML, its `formaction` includes parameters for:</span></span>

* <span data-ttu-id="3d79b-208">Die benutzerdefinierte Kontakt-ID, die vom `asp-route-id`-Attribut angegeben wird</span><span class="sxs-lookup"><span data-stu-id="3d79b-208">The customer contact ID specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="3d79b-209">Der `handler`, der vom `asp-page-handler`-Attribut angegeben wird</span><span class="sxs-lookup"><span data-stu-id="3d79b-209">The `handler` specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="3d79b-210">Hier sehen Sie ein Beispiel für eine gerenderte „Löschen“-Schaltfläche mit einer benutzerdefinierten Kontakt-ID von `1`:</span><span class="sxs-lookup"><span data-stu-id="3d79b-210">Here is an example of a rendered delete button with a customer contact ID of `1`:</span></span>

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="3d79b-211">Wenn die Schaltfläche ausgewählt wird, wird eine `POST`-Anforderung an den Server gesendet.</span><span class="sxs-lookup"><span data-stu-id="3d79b-211">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="3d79b-212">Durch Konvention wird der Name der Handlermethode auf Grundlage des Werts des `handler`-Parameters entsprechend des Schemas `OnPost[handler]Async` ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="3d79b-212">By convention, the name of the handler method is selected based the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="3d79b-213">Da der `handler` in diesem Beispiel `delete` ist, wird die Handlermethode `OnPostDeleteAsync` verwendet, um die `POST`-Anforderung zu verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="3d79b-213">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="3d79b-214">Wenn der `asp-page-handler` auf einen anderen Wert festgelegt wird, wie z.B. `remove`, wird eine Seitenhandlermethode mit dem Namen `OnPostRemoveAsync` ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="3d79b-214">If the `asp-page-handler` is set to a different value, such as `remove`, a page handler method with the name `OnPostRemoveAsync` is selected.</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

<span data-ttu-id="3d79b-215">Die `OnPostDeleteAsync`-Methode:</span><span class="sxs-lookup"><span data-stu-id="3d79b-215">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="3d79b-216">Akzeptiert die `id` der Abfragezeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="3d79b-216">Accepts the `id` from the query string.</span></span>
* <span data-ttu-id="3d79b-217">Fragt mit `FindAsync` die Datenbank nach dem Kundenkontakt ab.</span><span class="sxs-lookup"><span data-stu-id="3d79b-217">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="3d79b-218">Wenn der Kundenkontakt gefunden wird, wird er aus der Liste der Kundenkontakte entfernt.</span><span class="sxs-lookup"><span data-stu-id="3d79b-218">If the customer contact is found, they're removed from the list of customer contacts.</span></span> <span data-ttu-id="3d79b-219">Die Datenbank wurde aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="3d79b-219">The database is updated.</span></span>
* <span data-ttu-id="3d79b-220">Ruft `RedirectToPage` auf, um die Stammindexseite (`/Index`) umzuleiten.</span><span class="sxs-lookup"><span data-stu-id="3d79b-220">Calls `RedirectToPage` to redirect to the root Index page (`/Index`).</span></span>

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="3d79b-221">XSRF/CSRF und Razor-Seiten</span><span class="sxs-lookup"><span data-stu-id="3d79b-221">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="3d79b-222">Sie müssen keinen Code für die [Antifälschungsvalidierung](xref:security/anti-request-forgery) schreiben.</span><span class="sxs-lookup"><span data-stu-id="3d79b-222">You don't have to write any code for [antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="3d79b-223">Die Generierung und Validierung von Antifälschungstoken ist automatisch in Razor-Seiten enthalten.</span><span class="sxs-lookup"><span data-stu-id="3d79b-223">Antiforgery token generation and validation are automatically included in Razor Pages.</span></span>

<a name="layout"></a>
## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="3d79b-224">Verwenden von Layouts, Teilansichten, Vorlagen und Taghilfsprogrammen mit Razor-Seiten</span><span class="sxs-lookup"><span data-stu-id="3d79b-224">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="3d79b-225">Razor-Seiten funktionieren mit allen Funktionen des Razor-Anzeigemoduls.</span><span class="sxs-lookup"><span data-stu-id="3d79b-225">Pages work with all the features of the Razor view engine.</span></span> <span data-ttu-id="3d79b-226">Layouts, Teilansichten, Vorlagen, Taghilfsprogramme, *_ViewStart.cshtml*, *_ViewImports.cshtml* funktionieren auf die gleiche Weise wie für herkömmliche Razor-Ansichten.</span><span class="sxs-lookup"><span data-stu-id="3d79b-226">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="3d79b-227">Lassen Sie uns diese Seite mithilfe einiger dieser Funktionen entwirren.</span><span class="sxs-lookup"><span data-stu-id="3d79b-227">Let's declutter this page by taking advantage of some of those features.</span></span>

<span data-ttu-id="3d79b-228">Fügen Sie einer *Pages/_Layout.cshtml* eine [Layoutseite](xref:mvc/views/layout) hinzu:</span><span class="sxs-lookup"><span data-stu-id="3d79b-228">Add a [layout page](xref:mvc/views/layout) to *Pages/_Layout.cshtml*:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

<span data-ttu-id="3d79b-229">Das [Layout](xref:mvc/views/layout):</span><span class="sxs-lookup"><span data-stu-id="3d79b-229">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="3d79b-230">Steuert das Layout der einzelnen Seiten, es sei denn, das Layout wird für eine Seite deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="3d79b-230">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="3d79b-231">Importiert HTML-Strukturen, z.B. JavaScript und Stylesheets.</span><span class="sxs-lookup"><span data-stu-id="3d79b-231">Imports HTML structures such as JavaScript and stylesheets.</span></span>

<span data-ttu-id="3d79b-232">Weitere Informationen finden Sie unter [Layoutseite](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="3d79b-232">See [layout page](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="3d79b-233">Die Eigenschaft [Layout](xref:mvc/views/layout#specifying-a-layout) wird in *Pages/_ViewStart.cshtml* festgelegt:</span><span class="sxs-lookup"><span data-stu-id="3d79b-233">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

<span data-ttu-id="3d79b-234">**Hinweis:** Das Layout befindet sich im Ordner *Pages* (Seiten).</span><span class="sxs-lookup"><span data-stu-id="3d79b-234">**Note:** The layout is in the *Pages* folder.</span></span> <span data-ttu-id="3d79b-235">Seiten suchen hierarchisch nach anderen Ansichten (Layouts, Vorlagen oder Teilansichten) und beginnen im gleichen Ordner wie die aktuelle Seite.</span><span class="sxs-lookup"><span data-stu-id="3d79b-235">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="3d79b-236">Ein Layout im Ordner *Seiten* kann aus jeder Razor-Seite unter dem Ordner *Pages* verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="3d79b-236">A layout in the *Pages* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="3d79b-237">Wir empfehlen Ihnen, die Layoutdatei **nicht** im Ordner *Views/Shared* (Ansichten/Freigegeben) zu platzieren.</span><span class="sxs-lookup"><span data-stu-id="3d79b-237">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="3d79b-238">*Views/Shared* ist ein MVC-Ansichtsmuster.</span><span class="sxs-lookup"><span data-stu-id="3d79b-238">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="3d79b-239">Razor-Seiten basieren auf der Ordnerhierarchie, nicht auf Pfadkonventionen.</span><span class="sxs-lookup"><span data-stu-id="3d79b-239">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="3d79b-240">Die Ansichtensuche in einer Razor-Seite enthält den Ordner *Pages*.</span><span class="sxs-lookup"><span data-stu-id="3d79b-240">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="3d79b-241">Die Layouts, Vorlagen und Teilansichten, die Sie mit MVC-Controllern und herkömmlichen Razor-Ansichten verwenden, *funktionieren einfach so*.</span><span class="sxs-lookup"><span data-stu-id="3d79b-241">The layouts, templates, and partials you're using with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="3d79b-242">Fügen Sie eine Datei *Pages/_ViewImports.cshtml* hinzu:</span><span class="sxs-lookup"><span data-stu-id="3d79b-242">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="3d79b-243">`@namespace` wird weiter unten im Tutorial erläutert.</span><span class="sxs-lookup"><span data-stu-id="3d79b-243">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="3d79b-244">Die `@addTagHelper`-Anweisung bringt die [integrierten Taghilfsprogramme](xref:mvc/views/tag-helpers/builtin-th/Index) zu allen Seiten in der Ordner *Pages*.</span><span class="sxs-lookup"><span data-stu-id="3d79b-244">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="3d79b-245">Wenn die `@namespace`-Anweisung explizit auf eine Seite angewendet wird:</span><span class="sxs-lookup"><span data-stu-id="3d79b-245">When the `@namespace` directive is used explicitly on a page:</span></span>

[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="3d79b-246">Die Anweisung legt den Namespace für die Seite fest.</span><span class="sxs-lookup"><span data-stu-id="3d79b-246">The directive sets the namespace for the page.</span></span> <span data-ttu-id="3d79b-247">Die `@model`-Anweisung muss den Namespace nicht enthalten.</span><span class="sxs-lookup"><span data-stu-id="3d79b-247">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="3d79b-248">Wenn sich die `@namespace`-Anweisung in *_ViewImports.cshtml* befindet, stellt der angegebene Namespace das Präfix für den generierten Namespace auf der Seite bereit, die die `@namespace`-Anweisung importiert.</span><span class="sxs-lookup"><span data-stu-id="3d79b-248">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="3d79b-249">Der Rest der generierten Namespaces (der Suffixteil) ist der durch Punkte getrennte relative Pfad zwischen dem Ordner mit *_ViewImports.cshtml* und dem Ordner, der die Seite enthält.</span><span class="sxs-lookup"><span data-stu-id="3d79b-249">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="3d79b-250">Die CodeBehind-Datei *Pages/Customers/Edit.cshtml.cs* legt den Namespace z.B. explizit fest:</span><span class="sxs-lookup"><span data-stu-id="3d79b-250">For example, the code behind file *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="3d79b-251">Die Datei *Pages/_ViewImports.cshtml* legt den folgenden Namespace fest:</span><span class="sxs-lookup"><span data-stu-id="3d79b-251">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="3d79b-252">Der generierte Namespace für die Razor-Seite *Pages/Customers/Edit.cshtml* ist identisch mit der CodeBehind-Datei.</span><span class="sxs-lookup"><span data-stu-id="3d79b-252">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the code behind file.</span></span> <span data-ttu-id="3d79b-253">Die `@namespace`-Anweisung wurde entworfen, damit die einem Projekt hinzugefügten C#-Klassen und mit Seiten generierter Code *einfach so funktionieren*, ohne dass eine `@using`-Anweisung für die CodeBehind-Datei hinzugefügt werden muss.</span><span class="sxs-lookup"><span data-stu-id="3d79b-253">The `@namespace` directive was designed so the C# classes added to a project and pages-generated code *just work* without having to add an `@using` directive for the code behind file.</span></span>

<span data-ttu-id="3d79b-254">**Hinweis:** `@namespace` funktioniert auch mit konventionellen Razor-Ansichten.</span><span class="sxs-lookup"><span data-stu-id="3d79b-254">**Note:** `@namespace` also works with conventional Razor views.</span></span>

<span data-ttu-id="3d79b-255">Die ursprüngliche Umgebungsdatei *Pages/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="3d79b-255">The original *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

<span data-ttu-id="3d79b-256">Die aktualisierte Umgebungsdatei *Pages/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="3d79b-256">The updated *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

<span data-ttu-id="3d79b-257">Die [Razor-Seiten-Startprojekt](#rpvs17) enthält die Seite *Pages/_ValidationScriptsPartial.cshtml*, die die clientseitige Validierung bindet.</span><span class="sxs-lookup"><span data-stu-id="3d79b-257">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="3d79b-258">URL-Generierung für Seiten</span><span class="sxs-lookup"><span data-stu-id="3d79b-258">URL generation for Pages</span></span>

<span data-ttu-id="3d79b-259">Die zuvor gezeigte `Create`-Seite verwendet `RedirectToPage`:</span><span class="sxs-lookup"><span data-stu-id="3d79b-259">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

<span data-ttu-id="3d79b-260">Die App hat die folgende Datei/Ordner-Struktur:</span><span class="sxs-lookup"><span data-stu-id="3d79b-260">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="3d79b-261">*/Pages*</span><span class="sxs-lookup"><span data-stu-id="3d79b-261">*/Pages*</span></span>

  * <span data-ttu-id="3d79b-262">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="3d79b-262">*Index.cshtml*</span></span>
  * <span data-ttu-id="3d79b-263">*/Customer*</span><span class="sxs-lookup"><span data-stu-id="3d79b-263">*/Customer*</span></span>

    * <span data-ttu-id="3d79b-264">*Create.cshtml*</span><span class="sxs-lookup"><span data-stu-id="3d79b-264">*Create.cshtml*</span></span>
    * <span data-ttu-id="3d79b-265">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="3d79b-265">*Edit.cshtml*</span></span>
    * <span data-ttu-id="3d79b-266">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="3d79b-266">*Index.cshtml*</span></span>

<span data-ttu-id="3d79b-267">Die Seiten *Pages/Customers/Create.cshtml* und *Pages/Customers/Edit.cshtml* leiten bei Erfolg an *Pages/Index.cshtml* weiter.</span><span class="sxs-lookup"><span data-stu-id="3d79b-267">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Index.cshtml* after success.</span></span> <span data-ttu-id="3d79b-268">Die Zeichenfolge `/Index` ist Teil des URI, der auf die vorhergehende Seite zugreifen soll.</span><span class="sxs-lookup"><span data-stu-id="3d79b-268">The string `/Index` is part of the URI to access the preceding page.</span></span> <span data-ttu-id="3d79b-269">Die Zeichenfolge `/Index` kann für das Generieren von URIs für die Seite *Pages/Index.cshtml* verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="3d79b-269">The string `/Index` can be used to generate URIs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="3d79b-270">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="3d79b-270">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="3d79b-271">Der Seitenname ist der Pfad zu der Seite vom Stammordner */Pages* (einschließlich eines vorangestellten `/`, z.B. `/Index`).</span><span class="sxs-lookup"><span data-stu-id="3d79b-271">The page name is the path to the page from the root */Pages* folder (including a leading `/`, for example `/Index`).</span></span> <span data-ttu-id="3d79b-272">Die vorherigen Beispiele für die URL-Generierung sind viel umfangreicher an Features als eine einfache hartcodierte URL.</span><span class="sxs-lookup"><span data-stu-id="3d79b-272">The preceding URL generation samples are much more feature rich than just hardcoding a URL.</span></span> <span data-ttu-id="3d79b-273">Bei der URL-Generierung wird [Routing](xref:mvc/controllers/routing) verwendet. Außerdem können damit Parameter generiert und entsprechend der Definition der Route im Zielpfad codiert werden.</span><span class="sxs-lookup"><span data-stu-id="3d79b-273">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="3d79b-274">Die URL-Generierung für Seiten unterstützt relative Namen.</span><span class="sxs-lookup"><span data-stu-id="3d79b-274">URL generation for pages supports relative names.</span></span> <span data-ttu-id="3d79b-275">In der folgenden Tabelle wird dargestellt, welche Indexseite für verschiedene `RedirectToPage`-Parameter aus *Pages/Customers/Create.cshtml* ausgewählt wird:</span><span class="sxs-lookup"><span data-stu-id="3d79b-275">The following table shows which Index page is selected with different `RedirectToPage` parameters from *Pages/Customers/Create.cshtml*:</span></span>

| <span data-ttu-id="3d79b-276">RedirectToPage(x)</span><span class="sxs-lookup"><span data-stu-id="3d79b-276">RedirectToPage(x)</span></span>| <span data-ttu-id="3d79b-277">Seite</span><span class="sxs-lookup"><span data-stu-id="3d79b-277">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="3d79b-278">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="3d79b-278">RedirectToPage("/Index")</span></span> | <span data-ttu-id="3d79b-279">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="3d79b-279">*Pages/Index*</span></span> |
| <span data-ttu-id="3d79b-280">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="3d79b-280">RedirectToPage("./Index");</span></span> | <span data-ttu-id="3d79b-281">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="3d79b-281">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="3d79b-282">RedirectToPage("../Index")</span><span class="sxs-lookup"><span data-stu-id="3d79b-282">RedirectToPage("../Index")</span></span> | <span data-ttu-id="3d79b-283">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="3d79b-283">*Pages/Index*</span></span> |
| <span data-ttu-id="3d79b-284">RedirectToPage("Index")</span><span class="sxs-lookup"><span data-stu-id="3d79b-284">RedirectToPage("Index")</span></span>  | <span data-ttu-id="3d79b-285">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="3d79b-285">*Pages/Customers/Index*</span></span> |

<span data-ttu-id="3d79b-286">`RedirectToPage("Index")`, `RedirectToPage("./Index")` und `RedirectToPage("../Index")` sind *relative Namen*.</span><span class="sxs-lookup"><span data-stu-id="3d79b-286">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")`  are *relative names*.</span></span> <span data-ttu-id="3d79b-287">Der `RedirectToPage`-Parameter wird mit dem Pfad der aktuellen Seite *kombiniert*, um den Namen der Zielseite zu berechnen.</span><span class="sxs-lookup"><span data-stu-id="3d79b-287">The `RedirectToPage` parameter is *combined* with the path of the current page to compute the name of the destination page.</span></span>  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page. -- page name, not page path -->

<span data-ttu-id="3d79b-288">Das Verknüpfen relativer Namen eignet sich beim Erstellen von Websites mit einer komplexen Struktur.</span><span class="sxs-lookup"><span data-stu-id="3d79b-288">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="3d79b-289">Wenn Sie relative Namen verwenden, um Seiten in einem Ordner zu verknüpfen, können Sie diesen Ordner umbenennen.</span><span class="sxs-lookup"><span data-stu-id="3d79b-289">If you use relative names to link between pages in a folder, you can rename that folder.</span></span> <span data-ttu-id="3d79b-290">Alle Links funktionieren weiterhin, da sie nicht den Namen des Ordners enthalten.</span><span class="sxs-lookup"><span data-stu-id="3d79b-290">All the links still work (because they didn't include the folder name).</span></span>

## <a name="tempdata"></a><span data-ttu-id="3d79b-291">TempData</span><span class="sxs-lookup"><span data-stu-id="3d79b-291">TempData</span></span>

<span data-ttu-id="3d79b-292">ASP.NET Core macht die Eigenschaft [TempData](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) auf einem [Controller](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller) verfügbar.</span><span class="sxs-lookup"><span data-stu-id="3d79b-292">ASP.NET Core exposes the [TempData](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="3d79b-293">Diese Eigenschaft speichert Daten, bis sie gelesen wurden.</span><span class="sxs-lookup"><span data-stu-id="3d79b-293">This property stores data until it is read.</span></span> <span data-ttu-id="3d79b-294">Die Methoden `Keep` und `Peek` können verwendet werden, um die Daten zu überprüfen, ohne sie zu löschen.</span><span class="sxs-lookup"><span data-stu-id="3d79b-294">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="3d79b-295">`TempData` eignet sich für die Weiterleitung, wenn Daten für mehr als eine Anforderung benötigt werden.</span><span class="sxs-lookup"><span data-stu-id="3d79b-295">`TempData` is  useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="3d79b-296">Das `[TempData]`-Attribut ist neu in ASP.NET Core 2.0 und wird auf Domänencontrollern und Seiten unterstützt.</span><span class="sxs-lookup"><span data-stu-id="3d79b-296">The `[TempData]` attribute is new in ASP.NET Core 2.0 and is supported on controllers and pages.</span></span>

<span data-ttu-id="3d79b-297">Im folgenden Code wird der Wert von `Message` mit `TempData` festgelegt:</span><span class="sxs-lookup"><span data-stu-id="3d79b-297">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="3d79b-298">Das folgende Markup in der Datei *Pages/Customers/Index.cshtml* zeigt den Wert von `Message` mit `TempData` an.</span><span class="sxs-lookup"><span data-stu-id="3d79b-298">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="3d79b-299">Die CodeBehind-Datei *Pages/Customers/Index.cshtml.cs* wendet das `[TempData]`-Attribut auf die Eigenschaft `Message`.</span><span class="sxs-lookup"><span data-stu-id="3d79b-299">The *Pages/Customers/Index.cshtml.cs* code-behind file applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="3d79b-300">Weitere Informationen finden Sie unter [TempData](xref:fundamentals/app-state#temp).</span><span class="sxs-lookup"><span data-stu-id="3d79b-300">See [TempData](xref:fundamentals/app-state#temp) for more information.</span></span>

<a name="mhpp"></a>
## <a name="multiple-handlers-per-page"></a><span data-ttu-id="3d79b-301">Mehrere Handler pro Seite</span><span class="sxs-lookup"><span data-stu-id="3d79b-301">Multiple handlers per page</span></span>

<span data-ttu-id="3d79b-302">Die folgende Seite generiert mit dem `asp-page-handler`-Taghilfsprogramm Markup für zwei Handler:</span><span class="sxs-lookup"><span data-stu-id="3d79b-302">The following page generates markup for two page handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there is no `asp-` attribute   -->

<span data-ttu-id="3d79b-303">Das Formular im vorherigen Beispiel hat zwei Sendeschaltflächen, und jede verwendet `FormActionTagHelper`, um an eine andere URL zu übermitteln.</span><span class="sxs-lookup"><span data-stu-id="3d79b-303">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="3d79b-304">Das `asp-page-handler`-Attribut ist eine Ergänzung für `asp-page`.</span><span class="sxs-lookup"><span data-stu-id="3d79b-304">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="3d79b-305">`asp-page-handler` generiert URLs, die als Übermittlungsziel jeweils die durch eine Seite festgelegte Handlermethode verwenden.</span><span class="sxs-lookup"><span data-stu-id="3d79b-305">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="3d79b-306">`asp-page` wird nicht angegeben, weil das Beispiel mit der aktuellen Seite verknüpft.</span><span class="sxs-lookup"><span data-stu-id="3d79b-306">`asp-page` is not specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="3d79b-307">Die CodeBehind-Datei:</span><span class="sxs-lookup"><span data-stu-id="3d79b-307">The code-behind file:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="3d79b-308">Der vorherige Code verwendet *benannte Handlermethoden*.</span><span class="sxs-lookup"><span data-stu-id="3d79b-308">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="3d79b-309">Benannte Handlermethoden werden aus dem Text im Namen nach `On<HTTP Verb>` und vor `Async` (falls vorhanden) erstellt.</span><span class="sxs-lookup"><span data-stu-id="3d79b-309">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="3d79b-310">Im vorherigen Beispiel sind OnPost**JoinList**Async und OnPost**JoinListUC**Async die Seitenmethoden.</span><span class="sxs-lookup"><span data-stu-id="3d79b-310">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="3d79b-311">Wenn Sie *OnPost* und *Async* entfernen, lauten die Handlernamen `JoinList` und `JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="3d79b-311">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="3d79b-312">Mit dem vorherigen Code lautet der URL-Pfad, der an `OnPostJoinListAsync` übermittelt, `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span><span class="sxs-lookup"><span data-stu-id="3d79b-312">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="3d79b-313">Der URL-Pfad, der an `OnPostJoinListUCAsync` übermittelt, lautet `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="3d79b-313">The URL path that submits to `OnPostJoinListUCAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="customizing-routing"></a><span data-ttu-id="3d79b-314">Anpassen des Routings</span><span class="sxs-lookup"><span data-stu-id="3d79b-314">Customizing Routing</span></span>

<span data-ttu-id="3d79b-315">Wenn Sie nicht möchten, dass die Abfragezeichenfolge `?handler=JoinList` in der URL enthalten ist, können Sie die Route ändern, damit der Handlername im Pfadteil der URL eingefügt wird.</span><span class="sxs-lookup"><span data-stu-id="3d79b-315">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="3d79b-316">Sie können die Route anpassen, indem Sie eine Routenvorlage in doppelten Anführungszeichen nach der `@page`-Anweisung hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="3d79b-316">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="3d79b-317">Die vorherige Route platziert den Handlernamen im URL-Pfad statt in die Abfragezeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="3d79b-317">The preceding route puts the handler name in the URL path instead of the query string.</span></span> <span data-ttu-id="3d79b-318">Das `?` nach `handler` bedeutet, dass der Routenparameter optional ist.</span><span class="sxs-lookup"><span data-stu-id="3d79b-318">The `?` following `handler` means the route parameter is optional.</span></span>

<span data-ttu-id="3d79b-319">Sie können einer Seitenroute mit `@page` weitere Segmente und Parameter hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="3d79b-319">You can use `@page` to add additional segments and parameters to a page's route.</span></span> <span data-ttu-id="3d79b-320">Alles, was hier angegeben wird, wird der Standardroute der Seite **angefügt**.</span><span class="sxs-lookup"><span data-stu-id="3d79b-320">Whatever's there is **appended** to the default route of the page.</span></span> <span data-ttu-id="3d79b-321">Die Verwendung eines absoluten oder des virtuellen Pfads, um die Seitenroute (z.B. `"~/Some/Other/Path"`) zu ändern, wird nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="3d79b-321">Using an absolute or virtual path to change the page's route (like `"~/Some/Other/Path"`) is not supported.</span></span>

## <a name="configuration-and-settings"></a><span data-ttu-id="3d79b-322">Konfiguration und Einstellungen</span><span class="sxs-lookup"><span data-stu-id="3d79b-322">Configuration and settings</span></span>

<span data-ttu-id="3d79b-323">Um die erweiterten Optionen zu konfigurieren, verwenden Sie die Erweiterungsmethode `AddRazorPagesOptions` auf dem MVC-Generator:</span><span class="sxs-lookup"><span data-stu-id="3d79b-323">To configure advanced options, use the extension method `AddRazorPagesOptions` on the MVC builder:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

<span data-ttu-id="3d79b-324">Derzeit können Sie `RazorPagesOptions` verwenden, um das Stammverzeichnis für Seiten festzulegen oder Anwendungsmodellkonventionen für Seiten hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="3d79b-324">Currently you can use the `RazorPagesOptions` to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="3d79b-325">Auf diese Weise wird in Zukunft eine höhere Erweiterbarkeit erreicht.</span><span class="sxs-lookup"><span data-stu-id="3d79b-325">We'll enable more extensibility this way in the future.</span></span>

<span data-ttu-id="3d79b-326">Informationen zum Vorkompilieren von Ansichten finden Sie unter [Razor-Ansichtenkompilierung und Vorkompilierung in ASP.NET](xref:mvc/views/view-compilation).</span><span class="sxs-lookup"><span data-stu-id="3d79b-326">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation) .</span></span>

<span data-ttu-id="3d79b-327">[Laden Sie Beispielcode herunter, oder zeigen Sie ihn an](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span><span class="sxs-lookup"><span data-stu-id="3d79b-327">[Download or view sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span></span>

<span data-ttu-id="3d79b-328">Lesen Sie auch den Artikel [Erste Schritte mit Razor-Seiten in ASP.NET Core](xref:tutorials/razor-pages/razor-pages-start), der auf diese Einführung aufbaut.</span><span class="sxs-lookup"><span data-stu-id="3d79b-328">See [Getting started with Razor Pages in ASP.NET Core](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="3d79b-329">Festlegen des Inhaltsstammverzeichnisses für Razor-Seiten</span><span class="sxs-lookup"><span data-stu-id="3d79b-329">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="3d79b-330">Standardmäßig lautet das Stammverzeichnis für Razor-Seiten */Pages*.</span><span class="sxs-lookup"><span data-stu-id="3d79b-330">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="3d79b-331">Fügen Sie [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) zu [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) hinzu, um anzugeben, dass sich Ihre Razor-Dateien im Inhaltsstammverzeichnis ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) der App befinden:</span><span class="sxs-lookup"><span data-stu-id="3d79b-331">Add [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at the content root ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) of the app:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="3d79b-332">Festlegen eines benutzerdefinierten Stammverzeichnisses für Razor-Seiten</span><span class="sxs-lookup"><span data-stu-id="3d79b-332">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="3d79b-333">Fügen Sie [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) zu [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) hinzu, um anzugeben, dass Ihre Razor-Seiten sich in einem benutzerdefinierten Stammverzeichnis in der App befinden (geben Sie einen relativen Pfad an):</span><span class="sxs-lookup"><span data-stu-id="3d79b-333">Add [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="see-also"></a><span data-ttu-id="3d79b-334">Siehe auch</span><span class="sxs-lookup"><span data-stu-id="3d79b-334">See also</span></span>

* [<span data-ttu-id="3d79b-335">Erste Schritte mit Razor-Seiten</span><span class="sxs-lookup"><span data-stu-id="3d79b-335">Getting started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="3d79b-336">Autorisierungskonventionen für Razor-Seiten</span><span class="sxs-lookup"><span data-stu-id="3d79b-336">Razor Pages authorization conventions</span></span>](xref:security/authorization/razor-pages-authorization)
* [<span data-ttu-id="3d79b-337">Benutzerdefinierte Routen- und Seitenmodellanbieter für Razor-Seiten</span><span class="sxs-lookup"><span data-stu-id="3d79b-337">Razor Pages custom route and page model providers</span></span>](xref:mvc/razor-pages/razor-pages-convention-features)
* [<span data-ttu-id="3d79b-338">Unit- und Integrationstests für Razor-Seiten</span><span class="sxs-lookup"><span data-stu-id="3d79b-338">Razor Pages unit and integration testing</span></span>](xref:testing/razor-pages-testing)
