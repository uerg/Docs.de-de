---
title: "Einführung in Razor-Seiten in ASP.NET Core"
author: Rick-Anderson
description: "ASP.NET Core-Lernprogramm für Razor-Seiten. Enthält MVC Core, ASP.NET Core 2.x, Visual Studio 2017 sowie eine Einführung in die Webentwicklung. Diese Dokumentation stellt eine Übersicht über die Verwendung von Razor-Seiten in ASP.NET Core bereit, um die Entwicklung von auf Seiten bezogene Szenarios zu vereinfachen."
ms.author: riande
manager: wpickett
ms.date: 09/12/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/razor-pages/index
ms.openlocfilehash: a08c1b59c7be3a27fc11e6737a1cb4b4208f2901
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a><span data-ttu-id="70e77-105">Einführung in Razor-Seiten in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="70e77-105">Introduction to Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="70e77-106">Von [Rick Anderson](https://twitter.com/RickAndMSFT) und [Ryan Nowak](https://github.com/rynowak)</span><span class="sxs-lookup"><span data-stu-id="70e77-106">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="70e77-107">Razor-Seiten ist ein neues Feature von ASP.NET Core MVC, mit dem codierungsseitige Szenarios einfacher und produktiver werden.</span><span class="sxs-lookup"><span data-stu-id="70e77-107">Razor Pages is a new feature of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="70e77-108">Weitere Informationen zu einem Tutorial, in dem der Model-View-Controller-Ansatz verwendet wird, finden Sie unter [Getting started with ASP.NET Core MVC (Erste Schritte mit ASP.NET Core MVC)](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="70e77-108">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Getting started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="70e77-109">Dieses Dokument bietet eine Einführung in Razor-Seiten.</span><span class="sxs-lookup"><span data-stu-id="70e77-109">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="70e77-110">Es handelt sich nicht um ein Schritt-für-Schritt-Tutorial.</span><span class="sxs-lookup"><span data-stu-id="70e77-110">It's not a step by step tutorial.</span></span> <span data-ttu-id="70e77-111">Wenn es Ihnen Probleme bereitet, die Ausführungen in einigen Abschnitten nachzuvollziehen, lesen Sie [Erste Schritte mit Razor-Seiten](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="70e77-111">If you find some of the sections difficult to follow, see [Getting started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span>

<a name="prerequisites"></a>

## <a name="aspnet-core-20-prerequisites"></a><span data-ttu-id="70e77-112">Anforderungen von ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="70e77-112">ASP.NET Core 2.0 prerequisites</span></span>

<span data-ttu-id="70e77-113">Installieren Sie [.NET Core](https://www.microsoft.com/net/core) 2.0.0 oder höher.</span><span class="sxs-lookup"><span data-stu-id="70e77-113">Install [.NET Core](https://www.microsoft.com/net/core) 2.0.0 or later.</span></span>

<span data-ttu-id="70e77-114">Wenn Sie Visual Studio verwenden, installieren Sie [Visual Studio](https://www.visualstudio.com/vs/) 2017 Version 15.3 oder höher mit folgenden Workloads:</span><span class="sxs-lookup"><span data-stu-id="70e77-114">If you're using Visual Studio, install [Visual Studio](https://www.visualstudio.com/vs/) 2017 version 15.3 or later with the following workloads:</span></span>

* <span data-ttu-id="70e77-115">**ASP.NET und Webentwicklung**</span><span class="sxs-lookup"><span data-stu-id="70e77-115">**ASP.NET and web development**</span></span>
* <span data-ttu-id="70e77-116">**Plattformübergreifende .NET Core-Entwicklung**</span><span class="sxs-lookup"><span data-stu-id="70e77-116">**.NET Core cross-platform development**</span></span>

<a name="rpvs17"></a>

## <a name="creating-a-razor-pages-project"></a><span data-ttu-id="70e77-117">Erstellen eines Projekts mit Razor-Seiten</span><span class="sxs-lookup"><span data-stu-id="70e77-117">Creating a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="70e77-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="70e77-118">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="70e77-119">Weitere Informationen zum Erstellen eines Projekts mit Razor-Seiten mithilfe von Visual Studio finden Sie unter [Getting started with Razor Pages (Erste Schritte mit Razor-Seiten)](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="70e77-119">See [Getting started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project using Visual Studio.</span></span>

#   <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="70e77-120">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="70e77-120">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="70e77-121">Führen Sie `dotnet new razor` über die Befehlszeile aus.</span><span class="sxs-lookup"><span data-stu-id="70e77-121">Run `dotnet new razor` from the command line.</span></span>

<span data-ttu-id="70e77-122">Öffnen Sie die generierte Datei *.csproj* aus Visual Studio für Mac.</span><span class="sxs-lookup"><span data-stu-id="70e77-122">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="70e77-123">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="70e77-123">Visual Studio Code</span></span>](#tab/visual-studio-code) 

<span data-ttu-id="70e77-124">Führen Sie `dotnet new razor` über die Befehlszeile aus.</span><span class="sxs-lookup"><span data-stu-id="70e77-124">Run `dotnet new razor` from the command line.</span></span>

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="70e77-125">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="70e77-125">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="70e77-126">Führen Sie `dotnet new razor` über die Befehlszeile aus.</span><span class="sxs-lookup"><span data-stu-id="70e77-126">Run `dotnet new razor` from the command line.</span></span>

---

## <a name="razor-pages"></a><span data-ttu-id="70e77-127">Razor-Seiten</span><span class="sxs-lookup"><span data-stu-id="70e77-127">Razor Pages</span></span>

<span data-ttu-id="70e77-128">Razor-Seiten ist in *Startup.cs* aktiviert:</span><span class="sxs-lookup"><span data-stu-id="70e77-128">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[main](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

<span data-ttu-id="70e77-129">Sehen Sie sich diese einfache Seite an: <a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="70e77-129">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

<span data-ttu-id="70e77-130">Der vorherige Code ähnelt stark einer Razor-Umgebungsdatei.</span><span class="sxs-lookup"><span data-stu-id="70e77-130">The preceding code looks a lot like a Razor view file.</span></span> <span data-ttu-id="70e77-131">Der Unterschied besteht in der `@page`-Anweisung.</span><span class="sxs-lookup"><span data-stu-id="70e77-131">What makes it different is the `@page` directive.</span></span> <span data-ttu-id="70e77-132">`@page` macht die Datei zu einer MVC-Aktion, d.h. dass Anfragen direkt ohne einen Controller verarbeitet werden.</span><span class="sxs-lookup"><span data-stu-id="70e77-132">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="70e77-133">`@page` muss die erste Razor-Anweisung auf einer Seite sein.</span><span class="sxs-lookup"><span data-stu-id="70e77-133">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="70e77-134">`@page` wirkt sich auf das Verhalten aller anderen Razor-Konstrukte aus.</span><span class="sxs-lookup"><span data-stu-id="70e77-134">`@page` affects the behavior of other Razor constructs.</span></span>

<span data-ttu-id="70e77-135">Eine ähnliche Seite, die die `PageModel`-Klasse verwendet, wird in den folgenden zwei Dateien angezeigt.</span><span class="sxs-lookup"><span data-stu-id="70e77-135">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="70e77-136">Die Datei *Pages/Index2.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="70e77-136">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="70e77-137">Die CodeBehind-Datei *Pages/Index2.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="70e77-137">The *Pages/Index2.cshtml.cs* "code-behind" file:</span></span>

[!code-cs[main](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="70e77-138">Die `PageModel`-Klassendatei hat standardmäßig den gleichen Namen wie die Datei mit Razor-Seiten, nur dass außerdem *cs* angefügt wird.</span><span class="sxs-lookup"><span data-stu-id="70e77-138">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="70e77-139">Die vorherige Datei mit Razor-Seiten lautet beispielsweise *Pages/Index2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="70e77-139">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="70e77-140">Die Datei mit der `PageModel`-Klasse heißt *Pages/Index2.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="70e77-140">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="70e77-141">Die Zuordnungen von URL-Pfaden zu Seiten werden durch den Speicherort der Seite im Dateisystem bestimmt.</span><span class="sxs-lookup"><span data-stu-id="70e77-141">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="70e77-142">Die folgende Tabelle zeigt einen Pfad zu Razor-Seiten und die entsprechende URL:</span><span class="sxs-lookup"><span data-stu-id="70e77-142">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="70e77-143">Dateiname und Pfad</span><span class="sxs-lookup"><span data-stu-id="70e77-143">File name and path</span></span>               | <span data-ttu-id="70e77-144">Entsprechende URL</span><span class="sxs-lookup"><span data-stu-id="70e77-144">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="70e77-145">*/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="70e77-145">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="70e77-146">`/` oder `/Index`</span><span class="sxs-lookup"><span data-stu-id="70e77-146">`/` or `/Index`</span></span> |
| <span data-ttu-id="70e77-147">*/Pages/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="70e77-147">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="70e77-148">*/Pages/Store/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="70e77-148">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="70e77-149">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="70e77-149">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="70e77-150">`/Store` oder `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="70e77-150">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="70e77-151">Notizen:</span><span class="sxs-lookup"><span data-stu-id="70e77-151">Notes:</span></span>

* <span data-ttu-id="70e77-152">Die Runtime sucht standardmäßig im Ordner *Pages* (Seiten) nach Dateien mit Razor-Seiten.</span><span class="sxs-lookup"><span data-stu-id="70e77-152">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="70e77-153">Wenn eine Seite nicht in einer URL enthalten ist, ist `Index` die Standardseite.</span><span class="sxs-lookup"><span data-stu-id="70e77-153">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="writing-a-basic-form"></a><span data-ttu-id="70e77-154">Schreiben eines einfachen Formulars</span><span class="sxs-lookup"><span data-stu-id="70e77-154">Writing a basic form</span></span>

<span data-ttu-id="70e77-155">Features von Razor-Seiten erstellen allgemeine Muster, die einfach mit Webbrowsern verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="70e77-155">Razor Pages features are designed to make common patterns used with web browsers easy.</span></span> <span data-ttu-id="70e77-156">Die [Modellbindung](xref:mvc/models/model-binding), [Taghilfsprogramme](xref:mvc/views/tag-helpers/intro) und alle HTML-Hilfsprogramme *funktionieren nur* mit den Eigenschaften, die in einer Klasse der Razor-Seiten definiert wurden.</span><span class="sxs-lookup"><span data-stu-id="70e77-156">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="70e77-157">Nehmen wir z.B. eine Seite, die ein allgemeines Kontaktformular für das `Contact`-Modell implementiert:</span><span class="sxs-lookup"><span data-stu-id="70e77-157">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="70e77-158">Für die Beispiele in diesem Dokument wird `DbContext` in der Datei [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) initialisiert.</span><span class="sxs-lookup"><span data-stu-id="70e77-158">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) file.</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

<span data-ttu-id="70e77-159">Das Datenmodell:</span><span class="sxs-lookup"><span data-stu-id="70e77-159">The data model:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Data/Customer.cs)]

<span data-ttu-id="70e77-160">Der db-Kontext:</span><span class="sxs-lookup"><span data-stu-id="70e77-160">The db context:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

<span data-ttu-id="70e77-161">Die Umgebungsdatei *Pages/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="70e77-161">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

<span data-ttu-id="70e77-162">Die CodeBehind-Datei *Pages/Create.cshtml.cs* für die Ansicht:</span><span class="sxs-lookup"><span data-stu-id="70e77-162">The *Pages/Create.cshtml.cs* code-behind file for the view:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="70e77-163">Die `PageModel`-Klasse heißt standardmäßig `<PageName>Model` und befindet sich im selben Namespace wie die Seite.</span><span class="sxs-lookup"><span data-stu-id="70e77-163">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="70e77-164">Mit der Klasse `PageModel` kann die Logik einer Seite von deren Darstellung getrennt werden.</span><span class="sxs-lookup"><span data-stu-id="70e77-164">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="70e77-165">Sie definiert Seitenhandler für Anforderungen, die an die Seite geschickt wurden, und für zum Rendern der Seite verwendete Daten.</span><span class="sxs-lookup"><span data-stu-id="70e77-165">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="70e77-166">Durch diese Trennung können Sie Seitenabhängigkeiten durch [Abhängigkeiteneinschleusung](xref:fundamentals/dependency-injection) verwalten und [Komponententests](xref:testing/razor-pages-testing) für die Seiten durchführen.</span><span class="sxs-lookup"><span data-stu-id="70e77-166">This separation allows you to manage page dependencies through [dependency injection](xref:fundamentals/dependency-injection) and to [unit test](xref:testing/razor-pages-testing) the pages.</span></span>

<span data-ttu-id="70e77-167">Die Seite hat eine `OnPostAsync`-*Handlermethode*, die auf `POST`-Anforderungen ausgeführt wird (wenn ein Benutzer das Formular sendet).</span><span class="sxs-lookup"><span data-stu-id="70e77-167">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="70e77-168">Sie können Handlermethoden für alle HTTP-Verben hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="70e77-168">You can add handler methods for any HTTP verb.</span></span> <span data-ttu-id="70e77-169">Die am häufigsten verwendeten Handler sind:</span><span class="sxs-lookup"><span data-stu-id="70e77-169">The most common handlers are:</span></span>

* <span data-ttu-id="70e77-170">`OnGet`, um den für eine Seite erforderlichen Status zu initialisieren.</span><span class="sxs-lookup"><span data-stu-id="70e77-170">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="70e77-171">[OnGet](#OnGet)-Beispiel</span><span class="sxs-lookup"><span data-stu-id="70e77-171">[OnGet](#OnGet) sample.</span></span>
* <span data-ttu-id="70e77-172">`OnPost`, um Formularübermittlungen zu behandeln</span><span class="sxs-lookup"><span data-stu-id="70e77-172">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="70e77-173">Das Namenssuffix `Async` ist optional. Es wird jedoch standardmäßig häufig für asynchrone Funktionen verwendet.</span><span class="sxs-lookup"><span data-stu-id="70e77-173">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="70e77-174">Der `OnPostAsync`-Code im vorhergehenden Beispiel ähnelt dem, was Sie normalerweise in einem Controller schreiben würden.</span><span class="sxs-lookup"><span data-stu-id="70e77-174">The `OnPostAsync` code in the preceding example looks similar to what you would normally write in a controller.</span></span> <span data-ttu-id="70e77-175">Der vorhergehende Code ist typisch für Razor-Seiten.</span><span class="sxs-lookup"><span data-stu-id="70e77-175">The preceding code is typical for Razor Pages.</span></span> <span data-ttu-id="70e77-176">Die meisten primitiven MVC-Typen wie die [Modellbindung](xref:mvc/models/model-binding), die [Validierung](xref:mvc/models/validation) und Aktionsergebnisse werden freigegeben.</span><span class="sxs-lookup"><span data-stu-id="70e77-176">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results are shared.</span></span>  <!-- Review: Ryan, can we get a list of what is shared and what isn't? -->

<span data-ttu-id="70e77-177">Die vorherige `OnPostAsync`-Methode:</span><span class="sxs-lookup"><span data-stu-id="70e77-177">The previous `OnPostAsync` method:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="70e77-178">Der grundlegende Ablauf von `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="70e77-178">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="70e77-179">Prüfen auf Validierungsfehler</span><span class="sxs-lookup"><span data-stu-id="70e77-179">Check for validation errors.</span></span>

*  <span data-ttu-id="70e77-180">Wenn keine Fehler vorliegen, werden die Daten gespeichert und weitergeleitet.</span><span class="sxs-lookup"><span data-stu-id="70e77-180">If there are no errors, save the data and redirect.</span></span>
*  <span data-ttu-id="70e77-181">Wenn es Fehler gibt, zeigen Sie die Seite erneut mit den Validierungsmeldungen an.</span><span class="sxs-lookup"><span data-stu-id="70e77-181">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="70e77-182">Die clientseitige Validierung ist identisch mit herkömmlichen ASP.NET Core MVC-Anwendungen,</span><span class="sxs-lookup"><span data-stu-id="70e77-182">Client-side validation is identical to traditional ASP.NET Core MVC applications.</span></span> <span data-ttu-id="70e77-183">denn Validierungsfehler werden oftmals auf dem Client erkannt und nie an den Server übermittelt.</span><span class="sxs-lookup"><span data-stu-id="70e77-183">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="70e77-184">Wenn die Daten erfolgreich eingegeben wurden, ruft die `OnPostAsync`-Handlermethode die `RedirectToPage`-Hilfsmethode auf, um eine Instanz von `RedirectToPageResult` zurückzugeben.</span><span class="sxs-lookup"><span data-stu-id="70e77-184">When the data is entered successfully, the `OnPostAsync` handler method calls the `RedirectToPage` helper method to return an instance of `RedirectToPageResult`.</span></span> <span data-ttu-id="70e77-185">`RedirectToPage` ist ein neues Aktionsergebnis und ähnelt `RedirectToAction` oder `RedirectToRoute`, ist aber für Seiten angepasst.</span><span class="sxs-lookup"><span data-stu-id="70e77-185">`RedirectToPage` is a new action result, similar to `RedirectToAction` or `RedirectToRoute`, but customized for pages.</span></span> <span data-ttu-id="70e77-186">Im vorhergehenden Beispiel leitet es an die Stammindexseite (`/Index`) weiter.</span><span class="sxs-lookup"><span data-stu-id="70e77-186">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="70e77-187">Informationen zu `RedirectToPage` finden Sie im Abschnitt [URL-Generierung für Seiten](#url_gen).</span><span class="sxs-lookup"><span data-stu-id="70e77-187">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

<span data-ttu-id="70e77-188">Wenn das übermittelte Formular Validierungsfehler enthält (die an den Server übergeben wurden), ruft die `OnPostAsync`-Handlermethode die `Page`-Hilfsmethode auf.</span><span class="sxs-lookup"><span data-stu-id="70e77-188">When the submitted form has validation errors (that are passed to the server), the`OnPostAsync` handler method calls the `Page` helper method.</span></span> <span data-ttu-id="70e77-189">`Page` gibt eine Instanz von `PageResult` zurück.</span><span class="sxs-lookup"><span data-stu-id="70e77-189">`Page` returns an instance of `PageResult`.</span></span> <span data-ttu-id="70e77-190">Der Vorgang, bei dem `Page` zurückgegeben wird, ähnelt dem Vorgang, bei dem Aktionen im Controller `View` zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="70e77-190">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="70e77-191">`PageResult` ist der standardmäßige <!-- Review  -->-Rückgabetyp für eine Handlermethode.</span><span class="sxs-lookup"><span data-stu-id="70e77-191">`PageResult` is the default <!-- Review  --> return type for a handler method.</span></span> <span data-ttu-id="70e77-192">Eine Handlermethode, die `void` zurückgibt, rendert die Seite.</span><span class="sxs-lookup"><span data-stu-id="70e77-192">A handler method that returns `void` renders the page.</span></span>

<span data-ttu-id="70e77-193">Die Eigenschaft `Customer` verwendet das `[BindProperty]`-Attribut, um die Modellbindung zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="70e77-193">The `Customer` property uses `[BindProperty]` attribute to opt in to model binding.</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

<span data-ttu-id="70e77-194">Razor-Seiten binden Eigenschaften standardmäßig nur an Nicht-GET-Verben.</span><span class="sxs-lookup"><span data-stu-id="70e77-194">Razor Pages, by default, bind properties only with non-GET verbs.</span></span> <span data-ttu-id="70e77-195">Durch die Bindung an Eigenschaften können Sie den Umfang von Codes reduzieren, den Sie schreiben müssen.</span><span class="sxs-lookup"><span data-stu-id="70e77-195">Binding to properties can reduce the amount of code you have to write.</span></span> <span data-ttu-id="70e77-196">Die Bindung reduziert den Code mithilfe der gleichen Eigenschaft, um Formularfelder (`<input asp-for="Customer.Name" />`) zu rendern und die Eingabe zu akzeptieren.</span><span class="sxs-lookup"><span data-stu-id="70e77-196">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name" />`) and accept the input.</span></span>

<span data-ttu-id="70e77-197">Die Startseite (*Index.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="70e77-197">The home page (*Index.cshtml*):</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

<span data-ttu-id="70e77-198">Die CodeBehind-Datei *Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="70e77-198">The code behind *Index.cshtml.cs* file:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

<span data-ttu-id="70e77-199">Die Datei *Index.cshtml* enthält das folgende Markup, um einen Bearbeitungslink für jeden Kontakt zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="70e77-199">The *Index.cshtml* file contains the following markup to create an edit link for each contact:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

<span data-ttu-id="70e77-200">Das [Anchor-Taghilfsprogramm](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) verwendet das Attribut `asp-route-{value}`, um einen Link zur Bearbeitungsseite zu generieren.</span><span class="sxs-lookup"><span data-stu-id="70e77-200">The [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="70e77-201">Der Link enthält die Routendaten mit der Kontakt-ID.</span><span class="sxs-lookup"><span data-stu-id="70e77-201">The link contains route data with the contact ID.</span></span> <span data-ttu-id="70e77-202">Beispielsweise `http://localhost:5000/Edit/1`.</span><span class="sxs-lookup"><span data-stu-id="70e77-202">For example, `http://localhost:5000/Edit/1`.</span></span>

<span data-ttu-id="70e77-203">Die Datei *Pages/Edit.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="70e77-203">The *Pages/Edit.cshtml* file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

<span data-ttu-id="70e77-204">Die erste Zeile enthält die `@page "{id:int}"`-Anweisung.</span><span class="sxs-lookup"><span data-stu-id="70e77-204">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="70e77-205">Die Routingbeschränkung `"{id:int}"` weist die Seite an, die Anforderungen für die Seite zu akzeptieren, die `int`-Routingdaten enthalten.</span><span class="sxs-lookup"><span data-stu-id="70e77-205">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="70e77-206">Wenn eine Anforderung an die Seite bestimmte Routingdaten nicht enthält, die in einen `int` konvertiert werden können, gibt die Runtime einen Fehler vom Typ „HTTP 404: Nicht gefunden“ zurück.</span><span class="sxs-lookup"><span data-stu-id="70e77-206">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span>

<span data-ttu-id="70e77-207">Die Datei *Pages/Edit.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="70e77-207">The *Pages/Edit.cshtml.cs* file:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

<span data-ttu-id="70e77-208">Die Datei *index.cshtml* enthält auch Markup zum Erstellen der Schaltfläche „Löschen“ für jeden benutzerdefinierten Kontakt:</span><span class="sxs-lookup"><span data-stu-id="70e77-208">The *Index.cshtml* file also contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

<span data-ttu-id="70e77-209">Wenn die „Löschen“-Schaltfläche in HTML gerendert wird, enthält ihr `formaction`-Element Parameter für Folgendes:</span><span class="sxs-lookup"><span data-stu-id="70e77-209">When the delete button is rendered in HTML, its `formaction` includes parameters for:</span></span>

* <span data-ttu-id="70e77-210">Die benutzerdefinierte Kontakt-ID, die vom `asp-route-id`-Attribut angegeben wird</span><span class="sxs-lookup"><span data-stu-id="70e77-210">The customer contact ID specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="70e77-211">Der `handler`, der vom `asp-page-handler`-Attribut angegeben wird</span><span class="sxs-lookup"><span data-stu-id="70e77-211">The `handler` specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="70e77-212">Hier sehen Sie ein Beispiel für eine gerenderte „Löschen“-Schaltfläche mit einer benutzerdefinierten Kontakt-ID von `1`:</span><span class="sxs-lookup"><span data-stu-id="70e77-212">Here is an example of a rendered delete button with a customer contact ID of `1`:</span></span>

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="70e77-213">Wenn die Schaltfläche ausgewählt wird, wird eine `POST`-Anforderung an den Server gesendet.</span><span class="sxs-lookup"><span data-stu-id="70e77-213">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="70e77-214">Durch Konvention wird der Name der Handlermethode auf Grundlage des Werts des `handler`-Parameters entsprechend des Schemas `OnPost[handler]Async` ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="70e77-214">By convention, the name of the handler method is selected based the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="70e77-215">Da der `handler` in diesem Beispiel `delete` ist, wird die Handlermethode `OnPostDeleteAsync` verwendet, um die `POST`-Anforderung zu verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="70e77-215">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="70e77-216">Wenn der `asp-page-handler` auf einen anderen Wert festgelegt wird, wie z.B. `remove`, wird eine Seitenhandlermethode mit dem Namen `OnPostRemoveAsync` ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="70e77-216">If the `asp-page-handler` is set to a different value, such as `remove`, a page handler method with the name `OnPostRemoveAsync` is selected.</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

<span data-ttu-id="70e77-217">Die `OnPostDeleteAsync`-Methode:</span><span class="sxs-lookup"><span data-stu-id="70e77-217">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="70e77-218">Akzeptiert die `id` der Abfragezeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="70e77-218">Accepts the `id` from the query string.</span></span>
* <span data-ttu-id="70e77-219">Fragt mit `FindAsync` die Datenbank nach dem Kundenkontakt ab.</span><span class="sxs-lookup"><span data-stu-id="70e77-219">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="70e77-220">Wenn der Kundenkontakt gefunden wird, wird er aus der Liste der Kundenkontakte entfernt.</span><span class="sxs-lookup"><span data-stu-id="70e77-220">If the customer contact is found, they're removed from the list of customer contacts.</span></span> <span data-ttu-id="70e77-221">Die Datenbank wurde aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="70e77-221">The database is updated.</span></span>
* <span data-ttu-id="70e77-222">Ruft `RedirectToPage` auf, um die Stammindexseite (`/Index`) umzuleiten.</span><span class="sxs-lookup"><span data-stu-id="70e77-222">Calls `RedirectToPage` to redirect to the root Index page (`/Index`).</span></span>

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="70e77-223">XSRF/CSRF und Razor-Seiten</span><span class="sxs-lookup"><span data-stu-id="70e77-223">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="70e77-224">Sie müssen keinen Code für die [Antifälschungsvalidierung](xref:security/anti-request-forgery) schreiben.</span><span class="sxs-lookup"><span data-stu-id="70e77-224">You don't have to write any code for [antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="70e77-225">Die Generierung und Validierung von Antifälschungstoken ist automatisch in Razor-Seiten enthalten.</span><span class="sxs-lookup"><span data-stu-id="70e77-225">Antiforgery token generation and validation are automatically included in Razor Pages.</span></span>

<a name="layout"></a>
## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="70e77-226">Verwenden von Layouts, Teilansichten, Vorlagen und Taghilfsprogrammen mit Razor-Seiten</span><span class="sxs-lookup"><span data-stu-id="70e77-226">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="70e77-227">Razor-Seiten funktionieren mit allen Funktionen des Razor-Anzeigemoduls.</span><span class="sxs-lookup"><span data-stu-id="70e77-227">Pages work with all the features of the Razor view engine.</span></span> <span data-ttu-id="70e77-228">Layouts, Teilansichten, Vorlagen, Taghilfsprogramme, *_ViewStart.cshtml*, *_ViewImports.cshtml* funktionieren auf die gleiche Weise wie für herkömmliche Razor-Ansichten.</span><span class="sxs-lookup"><span data-stu-id="70e77-228">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="70e77-229">Lassen Sie uns diese Seite mithilfe einiger dieser Funktionen entwirren.</span><span class="sxs-lookup"><span data-stu-id="70e77-229">Let's declutter this page by taking advantage of some of those features.</span></span>

<span data-ttu-id="70e77-230">Fügen Sie einer *Pages/_Layout.cshtml* eine [Layoutseite](xref:mvc/views/layout) hinzu:</span><span class="sxs-lookup"><span data-stu-id="70e77-230">Add a [layout page](xref:mvc/views/layout) to *Pages/_Layout.cshtml*:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

<span data-ttu-id="70e77-231">Das [Layout](xref:mvc/views/layout):</span><span class="sxs-lookup"><span data-stu-id="70e77-231">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="70e77-232">Steuert das Layout der einzelnen Seiten, es sei denn, das Layout wird für eine Seite deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="70e77-232">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="70e77-233">Importiert HTML-Strukturen, z.B. JavaScript und Stylesheets.</span><span class="sxs-lookup"><span data-stu-id="70e77-233">Imports HTML structures such as JavaScript and stylesheets.</span></span>

<span data-ttu-id="70e77-234">Weitere Informationen finden Sie unter [Layoutseite](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="70e77-234">See [layout page](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="70e77-235">Die Eigenschaft [Layout](xref:mvc/views/layout#specifying-a-layout) wird in *Pages/_ViewStart.cshtml* festgelegt:</span><span class="sxs-lookup"><span data-stu-id="70e77-235">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

<span data-ttu-id="70e77-236">**Hinweis:** Das Layout befindet sich im Ordner *Pages* (Seiten).</span><span class="sxs-lookup"><span data-stu-id="70e77-236">**Note:** The layout is in the *Pages* folder.</span></span> <span data-ttu-id="70e77-237">Seiten suchen hierarchisch nach anderen Ansichten (Layouts, Vorlagen oder Teilansichten) und beginnen im gleichen Ordner wie die aktuelle Seite.</span><span class="sxs-lookup"><span data-stu-id="70e77-237">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="70e77-238">Ein Layout im Ordner *Seiten* kann aus jeder Razor-Seite unter dem Ordner *Pages* verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="70e77-238">A layout in the *Pages* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="70e77-239">Wir empfehlen Ihnen, die Layoutdatei **nicht** im Ordner *Views/Shared* (Ansichten/Freigegeben) zu platzieren.</span><span class="sxs-lookup"><span data-stu-id="70e77-239">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="70e77-240">*Views/Shared* ist ein MVC-Ansichtsmuster.</span><span class="sxs-lookup"><span data-stu-id="70e77-240">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="70e77-241">Razor-Seiten basieren auf der Ordnerhierarchie, nicht auf Pfadkonventionen.</span><span class="sxs-lookup"><span data-stu-id="70e77-241">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="70e77-242">Die Ansichtensuche in einer Razor-Seite enthält den Ordner *Pages*.</span><span class="sxs-lookup"><span data-stu-id="70e77-242">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="70e77-243">Die Layouts, Vorlagen und Teilansichten, die Sie mit MVC-Controllern und herkömmlichen Razor-Ansichten verwenden, *funktionieren einfach so*.</span><span class="sxs-lookup"><span data-stu-id="70e77-243">The layouts, templates, and partials you're using with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="70e77-244">Fügen Sie eine Datei *Pages/_ViewImports.cshtml* hinzu:</span><span class="sxs-lookup"><span data-stu-id="70e77-244">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="70e77-245">`@namespace` wird weiter unten im Tutorial erläutert.</span><span class="sxs-lookup"><span data-stu-id="70e77-245">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="70e77-246">Die `@addTagHelper`-Anweisung bringt die [integrierten Taghilfsprogramme](xref:mvc/views/tag-helpers/builtin-th/Index) zu allen Seiten in der Ordner *Pages*.</span><span class="sxs-lookup"><span data-stu-id="70e77-246">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="70e77-247">Wenn die `@namespace`-Anweisung explizit auf eine Seite angewendet wird:</span><span class="sxs-lookup"><span data-stu-id="70e77-247">When the `@namespace` directive is used explicitly on a page:</span></span>

[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="70e77-248">Die Anweisung legt den Namespace für die Seite fest.</span><span class="sxs-lookup"><span data-stu-id="70e77-248">The directive sets the namespace for the page.</span></span> <span data-ttu-id="70e77-249">Die `@model`-Anweisung muss den Namespace nicht enthalten.</span><span class="sxs-lookup"><span data-stu-id="70e77-249">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="70e77-250">Wenn sich die `@namespace`-Anweisung in *_ViewImports.cshtml* befindet, stellt der angegebene Namespace das Präfix für den generierten Namespace auf der Seite bereit, die die `@namespace`-Anweisung importiert.</span><span class="sxs-lookup"><span data-stu-id="70e77-250">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="70e77-251">Der Rest der generierten Namespaces (der Suffixteil) ist der durch Punkte getrennte relative Pfad zwischen dem Ordner mit *_ViewImports.cshtml* und dem Ordner, der die Seite enthält.</span><span class="sxs-lookup"><span data-stu-id="70e77-251">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="70e77-252">Die CodeBehind-Datei *Pages/Customers/Edit.cshtml.cs* legt den Namespace z.B. explizit fest:</span><span class="sxs-lookup"><span data-stu-id="70e77-252">For example, the code behind file *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="70e77-253">Die Datei *Pages/_ViewImports.cshtml* legt den folgenden Namespace fest:</span><span class="sxs-lookup"><span data-stu-id="70e77-253">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="70e77-254">Der generierte Namespace für die Razor-Seite *Pages/Customers/Edit.cshtml* ist identisch mit der CodeBehind-Datei.</span><span class="sxs-lookup"><span data-stu-id="70e77-254">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the code behind file.</span></span> <span data-ttu-id="70e77-255">Die `@namespace`-Anweisung wurde entworfen, damit die einem Projekt hinzugefügten C#-Klassen und mit Seiten generierter Code *einfach so funktionieren*, ohne dass eine `@using`-Anweisung für die CodeBehind-Datei hinzugefügt werden muss.</span><span class="sxs-lookup"><span data-stu-id="70e77-255">The `@namespace` directive was designed so the C# classes added to a project and pages-generated code *just work* without having to add an `@using` directive for the code behind file.</span></span>

<span data-ttu-id="70e77-256">**Hinweis:** `@namespace` funktioniert auch mit konventionellen Razor-Ansichten.</span><span class="sxs-lookup"><span data-stu-id="70e77-256">**Note:** `@namespace` also works with conventional Razor views.</span></span>

<span data-ttu-id="70e77-257">Die ursprüngliche Umgebungsdatei *Pages/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="70e77-257">The original *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

<span data-ttu-id="70e77-258">Die aktualisierte Umgebungsdatei *Pages/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="70e77-258">The updated *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

<span data-ttu-id="70e77-259">Die [Razor-Seiten-Startprojekt](#rpvs17) enthält die Seite *Pages/_ValidationScriptsPartial.cshtml*, die die clientseitige Validierung bindet.</span><span class="sxs-lookup"><span data-stu-id="70e77-259">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="70e77-260">URL-Generierung für Seiten</span><span class="sxs-lookup"><span data-stu-id="70e77-260">URL generation for Pages</span></span>

<span data-ttu-id="70e77-261">Die zuvor gezeigte `Create`-Seite verwendet `RedirectToPage`:</span><span class="sxs-lookup"><span data-stu-id="70e77-261">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

<span data-ttu-id="70e77-262">Die App hat die folgende Datei/Ordner-Struktur:</span><span class="sxs-lookup"><span data-stu-id="70e77-262">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="70e77-263">*/Pages*</span><span class="sxs-lookup"><span data-stu-id="70e77-263">*/Pages*</span></span>

  * <span data-ttu-id="70e77-264">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="70e77-264">*Index.cshtml*</span></span>
  * <span data-ttu-id="70e77-265">*/Customer*</span><span class="sxs-lookup"><span data-stu-id="70e77-265">*/Customer*</span></span>

    * <span data-ttu-id="70e77-266">*Create.cshtml*</span><span class="sxs-lookup"><span data-stu-id="70e77-266">*Create.cshtml*</span></span>
    * <span data-ttu-id="70e77-267">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="70e77-267">*Edit.cshtml*</span></span>
    * <span data-ttu-id="70e77-268">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="70e77-268">*Index.cshtml*</span></span>

<span data-ttu-id="70e77-269">Die Seiten *Pages/Customers/Create.cshtml* und *Pages/Customers/Edit.cshtml* leiten bei Erfolg an *Pages/Index.cshtml* weiter.</span><span class="sxs-lookup"><span data-stu-id="70e77-269">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Index.cshtml* after success.</span></span> <span data-ttu-id="70e77-270">Die Zeichenfolge `/Index` ist Teil des URI, der auf die vorhergehende Seite zugreifen soll.</span><span class="sxs-lookup"><span data-stu-id="70e77-270">The string `/Index` is part of the URI to access the preceding page.</span></span> <span data-ttu-id="70e77-271">Die Zeichenfolge `/Index` kann für das Generieren von URIs für die Seite *Pages/Index.cshtml* verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="70e77-271">The string `/Index` can be used to generate URIs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="70e77-272">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="70e77-272">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="70e77-273">Der Seitenname ist der Pfad zu der Seite vom Stammordner */Pages* (einschließlich eines vorangestellten `/`, z.B. `/Index`).</span><span class="sxs-lookup"><span data-stu-id="70e77-273">The page name is the path to the page from the root */Pages* folder (including a leading `/`, for example `/Index`).</span></span> <span data-ttu-id="70e77-274">Die vorherigen Beispiele für die URL-Generierung sind viel umfangreicher an Features als eine einfache hartcodierte URL.</span><span class="sxs-lookup"><span data-stu-id="70e77-274">The preceding URL generation samples are much more feature rich than just hardcoding a URL.</span></span> <span data-ttu-id="70e77-275">Bei der URL-Generierung wird [Routing](xref:mvc/controllers/routing) verwendet. Außerdem können damit Parameter generiert und entsprechend der Definition der Route im Zielpfad codiert werden.</span><span class="sxs-lookup"><span data-stu-id="70e77-275">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="70e77-276">Die URL-Generierung für Seiten unterstützt relative Namen.</span><span class="sxs-lookup"><span data-stu-id="70e77-276">URL generation for pages supports relative names.</span></span> <span data-ttu-id="70e77-277">In der folgenden Tabelle wird dargestellt, welche Indexseite für verschiedene `RedirectToPage`-Parameter aus *Pages/Customers/Create.cshtml* ausgewählt wird:</span><span class="sxs-lookup"><span data-stu-id="70e77-277">The following table shows which Index page is selected with different `RedirectToPage` parameters from *Pages/Customers/Create.cshtml*:</span></span>

| <span data-ttu-id="70e77-278">RedirectToPage(x)</span><span class="sxs-lookup"><span data-stu-id="70e77-278">RedirectToPage(x)</span></span>| <span data-ttu-id="70e77-279">Seite</span><span class="sxs-lookup"><span data-stu-id="70e77-279">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="70e77-280">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="70e77-280">RedirectToPage("/Index")</span></span> | <span data-ttu-id="70e77-281">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="70e77-281">*Pages/Index*</span></span> |
| <span data-ttu-id="70e77-282">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="70e77-282">RedirectToPage("./Index");</span></span> | <span data-ttu-id="70e77-283">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="70e77-283">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="70e77-284">RedirectToPage("../Index")</span><span class="sxs-lookup"><span data-stu-id="70e77-284">RedirectToPage("../Index")</span></span> | <span data-ttu-id="70e77-285">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="70e77-285">*Pages/Index*</span></span> |
| <span data-ttu-id="70e77-286">RedirectToPage("Index")</span><span class="sxs-lookup"><span data-stu-id="70e77-286">RedirectToPage("Index")</span></span>  | <span data-ttu-id="70e77-287">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="70e77-287">*Pages/Customers/Index*</span></span> |

<span data-ttu-id="70e77-288">`RedirectToPage("Index")`, `RedirectToPage("./Index")` und `RedirectToPage("../Index")` sind *relative Namen*.</span><span class="sxs-lookup"><span data-stu-id="70e77-288">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")`  are *relative names*.</span></span> <span data-ttu-id="70e77-289">Der `RedirectToPage`-Parameter wird mit dem Pfad der aktuellen Seite *kombiniert*, um den Namen der Zielseite zu berechnen.</span><span class="sxs-lookup"><span data-stu-id="70e77-289">The `RedirectToPage` parameter is *combined* with the path of the current page to compute the name of the destination page.</span></span>  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page. -- page name, not page path -->

<span data-ttu-id="70e77-290">Das Verknüpfen relativer Namen eignet sich beim Erstellen von Websites mit einer komplexen Struktur.</span><span class="sxs-lookup"><span data-stu-id="70e77-290">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="70e77-291">Wenn Sie relative Namen verwenden, um Seiten in einem Ordner zu verknüpfen, können Sie diesen Ordner umbenennen.</span><span class="sxs-lookup"><span data-stu-id="70e77-291">If you use relative names to link between pages in a folder, you can rename that folder.</span></span> <span data-ttu-id="70e77-292">Alle Links funktionieren weiterhin, da sie nicht den Namen des Ordners enthalten.</span><span class="sxs-lookup"><span data-stu-id="70e77-292">All the links still work (because they didn't include the folder name).</span></span>

## <a name="tempdata"></a><span data-ttu-id="70e77-293">TempData</span><span class="sxs-lookup"><span data-stu-id="70e77-293">TempData</span></span>

<span data-ttu-id="70e77-294">ASP.NET Core macht die Eigenschaft [TempData](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) auf einem [Controller](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller) verfügbar.</span><span class="sxs-lookup"><span data-stu-id="70e77-294">ASP.NET Core exposes the [TempData](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="70e77-295">Diese Eigenschaft speichert Daten, bis sie gelesen wurden.</span><span class="sxs-lookup"><span data-stu-id="70e77-295">This property stores data until it's read.</span></span> <span data-ttu-id="70e77-296">Die Methoden `Keep` und `Peek` können verwendet werden, um die Daten zu überprüfen, ohne sie zu löschen.</span><span class="sxs-lookup"><span data-stu-id="70e77-296">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="70e77-297">`TempData` eignet sich für die Weiterleitung, wenn Daten für mehr als eine Anforderung benötigt werden.</span><span class="sxs-lookup"><span data-stu-id="70e77-297">`TempData` is  useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="70e77-298">Das `[TempData]`-Attribut ist neu in ASP.NET Core 2.0 und wird auf Domänencontrollern und Seiten unterstützt.</span><span class="sxs-lookup"><span data-stu-id="70e77-298">The `[TempData]` attribute is new in ASP.NET Core 2.0 and is supported on controllers and pages.</span></span>

<span data-ttu-id="70e77-299">Im folgenden Code wird der Wert von `Message` mit `TempData` festgelegt:</span><span class="sxs-lookup"><span data-stu-id="70e77-299">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="70e77-300">Das folgende Markup in der Datei *Pages/Customers/Index.cshtml* zeigt den Wert von `Message` mit `TempData` an.</span><span class="sxs-lookup"><span data-stu-id="70e77-300">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="70e77-301">Die CodeBehind-Datei *Pages/Customers/Index.cshtml.cs* wendet das `[TempData]`-Attribut auf die Eigenschaft `Message`.</span><span class="sxs-lookup"><span data-stu-id="70e77-301">The *Pages/Customers/Index.cshtml.cs* code-behind file applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="70e77-302">Weitere Informationen finden Sie unter [TempData](xref:fundamentals/app-state#temp).</span><span class="sxs-lookup"><span data-stu-id="70e77-302">See [TempData](xref:fundamentals/app-state#temp) for more information.</span></span>

<a name="mhpp"></a>
## <a name="multiple-handlers-per-page"></a><span data-ttu-id="70e77-303">Mehrere Handler pro Seite</span><span class="sxs-lookup"><span data-stu-id="70e77-303">Multiple handlers per page</span></span>

<span data-ttu-id="70e77-304">Die folgende Seite generiert mit dem `asp-page-handler`-Taghilfsprogramm Markup für zwei Handler:</span><span class="sxs-lookup"><span data-stu-id="70e77-304">The following page generates markup for two page handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there's no `asp-` attribute   -->

<span data-ttu-id="70e77-305">Das Formular im vorherigen Beispiel hat zwei Sendeschaltflächen, und jede verwendet `FormActionTagHelper`, um an eine andere URL zu übermitteln.</span><span class="sxs-lookup"><span data-stu-id="70e77-305">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="70e77-306">Das `asp-page-handler`-Attribut ist eine Ergänzung für `asp-page`.</span><span class="sxs-lookup"><span data-stu-id="70e77-306">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="70e77-307">`asp-page-handler` generiert URLs, die als Übermittlungsziel jeweils die durch eine Seite festgelegte Handlermethode verwenden.</span><span class="sxs-lookup"><span data-stu-id="70e77-307">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="70e77-308">`asp-page` wird nicht angegeben, weil das Beispiel mit der aktuellen Seite verknüpft.</span><span class="sxs-lookup"><span data-stu-id="70e77-308">`asp-page` isn't specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="70e77-309">Die CodeBehind-Datei:</span><span class="sxs-lookup"><span data-stu-id="70e77-309">The code-behind file:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="70e77-310">Der vorherige Code verwendet *benannte Handlermethoden*.</span><span class="sxs-lookup"><span data-stu-id="70e77-310">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="70e77-311">Benannte Handlermethoden werden aus dem Text im Namen nach `On<HTTP Verb>` und vor `Async` (falls vorhanden) erstellt.</span><span class="sxs-lookup"><span data-stu-id="70e77-311">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="70e77-312">Im vorherigen Beispiel sind OnPost**JoinList**Async und OnPost**JoinListUC**Async die Seitenmethoden.</span><span class="sxs-lookup"><span data-stu-id="70e77-312">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="70e77-313">Wenn Sie *OnPost* und *Async* entfernen, lauten die Handlernamen `JoinList` und `JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="70e77-313">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="70e77-314">Mit dem vorherigen Code lautet der URL-Pfad, der an `OnPostJoinListAsync` übermittelt, `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span><span class="sxs-lookup"><span data-stu-id="70e77-314">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="70e77-315">Der URL-Pfad, der an `OnPostJoinListUCAsync` übermittelt, lautet `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="70e77-315">The URL path that submits to `OnPostJoinListUCAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="customizing-routing"></a><span data-ttu-id="70e77-316">Anpassen des Routings</span><span class="sxs-lookup"><span data-stu-id="70e77-316">Customizing Routing</span></span>

<span data-ttu-id="70e77-317">Wenn Sie nicht möchten, dass die Abfragezeichenfolge `?handler=JoinList` in der URL enthalten ist, können Sie die Route ändern, damit der Handlername im Pfadteil der URL eingefügt wird.</span><span class="sxs-lookup"><span data-stu-id="70e77-317">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="70e77-318">Sie können die Route anpassen, indem Sie eine Routenvorlage in doppelten Anführungszeichen nach der `@page`-Anweisung hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="70e77-318">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="70e77-319">Die vorherige Route platziert den Handlernamen im URL-Pfad statt in die Abfragezeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="70e77-319">The preceding route puts the handler name in the URL path instead of the query string.</span></span> <span data-ttu-id="70e77-320">Das `?` nach `handler` bedeutet, dass der Routenparameter optional ist.</span><span class="sxs-lookup"><span data-stu-id="70e77-320">The `?` following `handler` means the route parameter is optional.</span></span>

<span data-ttu-id="70e77-321">Sie können einer Seitenroute mit `@page` weitere Segmente und Parameter hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="70e77-321">You can use `@page` to add additional segments and parameters to a page's route.</span></span> <span data-ttu-id="70e77-322">Alles, was hier angegeben wird, wird der Standardroute der Seite **angefügt**.</span><span class="sxs-lookup"><span data-stu-id="70e77-322">Whatever's there's **appended** to the default route of the page.</span></span> <span data-ttu-id="70e77-323">Die Verwendung eines absoluten oder virtuellen Pfads, um die Seitenroute (z.B. `"~/Some/Other/Path"`) zu ändern, wird nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="70e77-323">Using an absolute or virtual path to change the page's route (like `"~/Some/Other/Path"`) isn't supported.</span></span>

## <a name="configuration-and-settings"></a><span data-ttu-id="70e77-324">Konfiguration und Einstellungen</span><span class="sxs-lookup"><span data-stu-id="70e77-324">Configuration and settings</span></span>

<span data-ttu-id="70e77-325">Um die erweiterten Optionen zu konfigurieren, verwenden Sie die Erweiterungsmethode `AddRazorPagesOptions` auf dem MVC-Generator:</span><span class="sxs-lookup"><span data-stu-id="70e77-325">To configure advanced options, use the extension method `AddRazorPagesOptions` on the MVC builder:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

<span data-ttu-id="70e77-326">Derzeit können Sie `RazorPagesOptions` verwenden, um das Stammverzeichnis für Seiten festzulegen oder Anwendungsmodellkonventionen für Seiten hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="70e77-326">Currently you can use the `RazorPagesOptions` to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="70e77-327">Auf diese Weise wird in Zukunft eine höhere Erweiterbarkeit erreicht.</span><span class="sxs-lookup"><span data-stu-id="70e77-327">We'll enable more extensibility this way in the future.</span></span>

<span data-ttu-id="70e77-328">Informationen zum Vorkompilieren von Ansichten finden Sie unter [Razor view compilation and precompilation in ASP.NET Core (Razor-Ansichtenkompilierung und Vorkompilierung in ASP.NET)](xref:mvc/views/view-compilation).</span><span class="sxs-lookup"><span data-stu-id="70e77-328">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation) .</span></span>

<span data-ttu-id="70e77-329">[Laden Sie Beispielcode herunter, oder zeigen Sie ihn an](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span><span class="sxs-lookup"><span data-stu-id="70e77-329">[Download or view sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span></span>

<span data-ttu-id="70e77-330">Lesen Sie auch den Artikel [Getting started with Razor Pages in ASP.NET Core (Erste Schritte mit Razor-Seiten in ASP.NET Core)](xref:tutorials/razor-pages/razor-pages-start), der auf diese Einführung aufbaut.</span><span class="sxs-lookup"><span data-stu-id="70e77-330">See [Getting started with Razor Pages in ASP.NET Core](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="70e77-331">Festlegen des Inhaltsstammverzeichnisses für Razor-Seiten</span><span class="sxs-lookup"><span data-stu-id="70e77-331">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="70e77-332">Standardmäßig lautet das Stammverzeichnis für Razor-Seiten */Pages*.</span><span class="sxs-lookup"><span data-stu-id="70e77-332">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="70e77-333">Fügen Sie [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) zu [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) hinzu, um anzugeben, dass sich Ihre Razor-Dateien im Inhaltsstammverzeichnis ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) der App befinden:</span><span class="sxs-lookup"><span data-stu-id="70e77-333">Add [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at the content root ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) of the app:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="70e77-334">Festlegen eines benutzerdefinierten Stammverzeichnisses für Razor-Seiten</span><span class="sxs-lookup"><span data-stu-id="70e77-334">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="70e77-335">Fügen Sie [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) zu [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) hinzu, um anzugeben, dass Ihre Razor-Seiten sich in einem benutzerdefinierten Stammverzeichnis in der App befinden (geben Sie einen relativen Pfad an):</span><span class="sxs-lookup"><span data-stu-id="70e77-335">Add [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="see-also"></a><span data-ttu-id="70e77-336">Siehe auch</span><span class="sxs-lookup"><span data-stu-id="70e77-336">See also</span></span>

* [<span data-ttu-id="70e77-337">Getting started with Razor Pages (Erste Schritte mit Razor-Seiten)</span><span class="sxs-lookup"><span data-stu-id="70e77-337">Getting started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="70e77-338">Autorisierungskonventionen für Razor-Seiten</span><span class="sxs-lookup"><span data-stu-id="70e77-338">Razor Pages authorization conventions</span></span>](xref:security/authorization/razor-pages-authorization)
* [<span data-ttu-id="70e77-339">Benutzerdefinierte Routen- und Seitenmodellanbieter für Razor-Seiten</span><span class="sxs-lookup"><span data-stu-id="70e77-339">Razor Pages custom route and page model providers</span></span>](xref:mvc/razor-pages/razor-pages-convention-features)
* [<span data-ttu-id="70e77-340">Unit- und Integrationstests für Razor-Seiten</span><span class="sxs-lookup"><span data-stu-id="70e77-340">Razor Pages unit and integration testing</span></span>](xref:testing/razor-pages-testing)
