---
title: "Einführung in Razor-Seiten in ASP.NET Core"
author: Rick-Anderson
description: "Überblick über Razor-Seiten in ASP.NET Core"
keywords: ASP.NET Core, Razor-Seiten
ms.author: riande
manager: wpickett
ms.date: 08/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/razor-pages/index
ms.openlocfilehash: 9301b99aed8fcb3bef91abf0fb269c4052cdb7e2
ms.sourcegitcommit: 87900dffec8ad84a0f74357b23343e215f354dcb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/01/2017
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a><span data-ttu-id="c1027-104">Einführung in Razor-Seiten in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c1027-104">Introduction to Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="c1027-105">Von [Rick Anderson](https://twitter.com/RickAndMSFT) und [Ryan Nowak](https://github.com/rynowak)</span><span class="sxs-lookup"><span data-stu-id="c1027-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="c1027-106">Razor-Seiten ist ein neues Feature von ASP.NET Core MVC, mit dem codierungsseitige Szenarios einfacher und produktiver werden.</span><span class="sxs-lookup"><span data-stu-id="c1027-106">Razor Pages is a new feature of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="c1027-107">Weitere Informationen zu einem Tutorial, in dem der Model-View-Controller-Ansatz verwendet wird, finden Sie unter [Getting started with ASP.NET Core MVC (Erste Schritte mit ASP.NET Core MVC)](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="c1027-107">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Getting started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<a name="prerequisites"></a>

## <a name="aspnet-core-20-prerequisites"></a><span data-ttu-id="c1027-108">Anforderungen von ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="c1027-108">ASP.NET Core 2.0 prerequisites</span></span>

<span data-ttu-id="c1027-109">Installieren Sie [.NET Core](https://dot.net/core) 2.0.0 oder höher.</span><span class="sxs-lookup"><span data-stu-id="c1027-109">Install [.NET Core](https://dot.net/core) 2.0.0 or later.</span></span>

<span data-ttu-id="c1027-110">Wenn Sie Visual Studio verwenden, installieren Sie [Visual Studio](https://www.visualstudio.com/vs/) 15.3 oder höher mit folgenden Workloads:</span><span class="sxs-lookup"><span data-stu-id="c1027-110">If you're using Visual Studio, install [Visual Studio](https://www.visualstudio.com/vs/) 15.3 or later with the following workloads:</span></span>

* <span data-ttu-id="c1027-111">**ASP.NET und Webentwicklung**</span><span class="sxs-lookup"><span data-stu-id="c1027-111">**ASP.NET and web development**</span></span>
* <span data-ttu-id="c1027-112">**Plattformübergreifende .NET Core-Entwicklung**</span><span class="sxs-lookup"><span data-stu-id="c1027-112">**.NET Core cross-platform development**</span></span>

<a name="rpvs17"></a>

## <a name="creating-a-razor-pages-project"></a><span data-ttu-id="c1027-113">Erstellen eines Projekts mit Razor-Seiten</span><span class="sxs-lookup"><span data-stu-id="c1027-113">Creating a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c1027-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c1027-114">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="c1027-115">Weitere Informationen zum Erstellen eines Projekts mit Razor-Seiten mithilfe von Visual Studio finden Sie unter [Getting started with Razor Pages (Erste Schritte mit Razor-Seiten)](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="c1027-115">See [Getting started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project using Visual Studio.</span></span>

#   <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c1027-116">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="c1027-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="c1027-117">Führen Sie `dotnet new razor` über die Befehlszeile aus.</span><span class="sxs-lookup"><span data-stu-id="c1027-117">Run `dotnet new razor` from the command line.</span></span>

<span data-ttu-id="c1027-118">Öffnen Sie die generierte Datei *.csproj* aus Visual Studio für Mac.</span><span class="sxs-lookup"><span data-stu-id="c1027-118">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c1027-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c1027-119">Visual Studio Code</span></span>](#tab/visual-studio-code) 

<span data-ttu-id="c1027-120">Führen Sie `dotnet new razor` über die Befehlszeile aus.</span><span class="sxs-lookup"><span data-stu-id="c1027-120">Run `dotnet new razor` from the command line.</span></span>

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="c1027-121">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="c1027-121">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="c1027-122">Führen Sie `dotnet new razor` über die Befehlszeile aus.</span><span class="sxs-lookup"><span data-stu-id="c1027-122">Run `dotnet new razor` from the command line.</span></span>

---

## <a name="razor-pages"></a><span data-ttu-id="c1027-123">Razor-Seiten</span><span class="sxs-lookup"><span data-stu-id="c1027-123">Razor Pages</span></span>

<span data-ttu-id="c1027-124">Razor-Seiten ist in *Startup.cs* aktiviert:</span><span class="sxs-lookup"><span data-stu-id="c1027-124">Razor Pages is enabled in *Startup.cs*:</span></span>

<span data-ttu-id="c1027-125">[!code-cs[main](index/sample/RazorPagesIntro/Startup.cs?name=Startup)]</span><span class="sxs-lookup"><span data-stu-id="c1027-125">[!code-cs[main](index/sample/RazorPagesIntro/Startup.cs?name=Startup)]</span></span>

<span data-ttu-id="c1027-126">Sehen Sie sich diese einfache Seite an: <a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="c1027-126">Consider a basic page: <a name="OnGet"></a></span></span>

<span data-ttu-id="c1027-127">[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Index.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="c1027-127">[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Index.cshtml)]</span></span>

<span data-ttu-id="c1027-128">Der vorherige Code ähnelt stark einer Razor-Umgebungsdatei.</span><span class="sxs-lookup"><span data-stu-id="c1027-128">The preceding code looks a lot like a Razor view file.</span></span> <span data-ttu-id="c1027-129">Der Unterschied besteht in der `@page`-Anweisung.</span><span class="sxs-lookup"><span data-stu-id="c1027-129">What makes it different is the `@page` directive.</span></span> <span data-ttu-id="c1027-130">`@page` macht die Datei zu einer MVC-Aktion, d.h. dass Anfragen direkt ohne einen Controller verarbeitet werden.</span><span class="sxs-lookup"><span data-stu-id="c1027-130">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="c1027-131">`@page` muss die erste Razor-Anweisung auf einer Seite sein.</span><span class="sxs-lookup"><span data-stu-id="c1027-131">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="c1027-132">`@page` wirkt sich auf das Verhalten aller anderen Razor-Konstrukte aus.</span><span class="sxs-lookup"><span data-stu-id="c1027-132">`@page` affects the behavior of other Razor constructs.</span></span> <span data-ttu-id="c1027-133">Die [@functions](xref:mvc/views/razor#functions)-Anweisung aktiviert Inhalt auf Funktionsebene.</span><span class="sxs-lookup"><span data-stu-id="c1027-133">The [@functions](xref:mvc/views/razor#functions) directive enables function-level content.</span></span>

<span data-ttu-id="c1027-134">Eine ähnliche Seite, mit `PageModel` in einer separaten Datei, wird in den folgenden zwei Dateien angezeigt.</span><span class="sxs-lookup"><span data-stu-id="c1027-134">A similar page, with the `PageModel` in a separate file, is shown in the following two files.</span></span> <span data-ttu-id="c1027-135">Die Datei *Pages/Index2.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="c1027-135">The *Pages/Index2.cshtml* file:</span></span>

<span data-ttu-id="c1027-136">[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="c1027-136">[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]</span></span>

<span data-ttu-id="c1027-137">Die CodeBehind-Datei *Pages/Index2.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="c1027-137">The *Pages/Index2.cshtml.cs* 'code-behind' file:</span></span>

<span data-ttu-id="c1027-138">[!code-cs[main](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]</span><span class="sxs-lookup"><span data-stu-id="c1027-138">[!code-cs[main](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]</span></span>

<span data-ttu-id="c1027-139">Die `PageModel`-Klassendatei hat standardmäßig den gleichen Namen wie die Datei mit Razor-Seiten, nur dass außerdem *cs* angefügt wird.</span><span class="sxs-lookup"><span data-stu-id="c1027-139">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="c1027-140">Die vorherige Datei mit Razor-Seiten lautet beispielsweise *Pages/Index2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="c1027-140">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="c1027-141">Die Datei mit der `PageModel`-Klasse heißt *Pages/Index2.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="c1027-141">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="c1027-142">Bei einfachen Seiten ist es in Ordnung, die `PageModel`-Klasse mit dem Razor-Markup zu mischen.</span><span class="sxs-lookup"><span data-stu-id="c1027-142">For simple pages, mixing the `PageModel` class with the Razor markup is fine.</span></span> <span data-ttu-id="c1027-143">Bei komplexerem Code ist es eine bewährte Methode, den Seitenmodelcode zu trennen.</span><span class="sxs-lookup"><span data-stu-id="c1027-143">For more complex code, it's a best practice to keep the page model code separate.</span></span>

<span data-ttu-id="c1027-144">Die Zuordnungen von URL-Pfaden zu Seiten werden durch den Speicherort der Seite im Dateisystem bestimmt.</span><span class="sxs-lookup"><span data-stu-id="c1027-144">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="c1027-145">Die folgende Tabelle zeigt einen Pfad zu Razor-Seiten und die entsprechende URL:</span><span class="sxs-lookup"><span data-stu-id="c1027-145">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="c1027-146">Dateiname und Pfad</span><span class="sxs-lookup"><span data-stu-id="c1027-146">File name and path</span></span>               | <span data-ttu-id="c1027-147">Entsprechende URL</span><span class="sxs-lookup"><span data-stu-id="c1027-147">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="c1027-148">*/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="c1027-148">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="c1027-149">`/` oder `/Index`</span><span class="sxs-lookup"><span data-stu-id="c1027-149">`/` or `/Index`</span></span> |
| <span data-ttu-id="c1027-150">*/Pages/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="c1027-150">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="c1027-151">*/Pages/Store/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="c1027-151">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="c1027-152">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="c1027-152">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="c1027-153">`/Store` oder `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="c1027-153">`/Store` or `/Store/Index`</span></span>  |

<span data-ttu-id="c1027-154">Notizen:</span><span class="sxs-lookup"><span data-stu-id="c1027-154">Notes:</span></span>

* <span data-ttu-id="c1027-155">Die Runtime sucht standardmäßig im Ordner *Pages* (Seiten) nach Dateien mit Razor-Seiten.</span><span class="sxs-lookup"><span data-stu-id="c1027-155">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="c1027-156">Wenn eine Seite nicht in einer URL enthalten ist, ist `Index` die Standardseite.</span><span class="sxs-lookup"><span data-stu-id="c1027-156">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="writing-a-basic-form"></a><span data-ttu-id="c1027-157">Schreiben eines einfachen Formulars</span><span class="sxs-lookup"><span data-stu-id="c1027-157">Writing a basic form</span></span>

<span data-ttu-id="c1027-158">Features von Razor-Seiten erstellen allgemeine Muster, die einfach mit Webbrowsern verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="c1027-158">Razor Pages features are designed to make common patterns used with web browsers easy.</span></span> <span data-ttu-id="c1027-159">Die [Modellbindung](xref:mvc/models/model-binding), [Taghilfsprogramme](xref:mvc/views/tag-helpers/intro) und alle HTML-Hilfsprogramme *funktionieren nur* mit den Eigenschaften, die in einer Klasse der Razor-Seiten definiert wurden.</span><span class="sxs-lookup"><span data-stu-id="c1027-159">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="c1027-160">Nehmen wir z.B. eine Seite, die ein allgemeines Kontaktformular für das `Contact`-Modell implementiert:</span><span class="sxs-lookup"><span data-stu-id="c1027-160">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="c1027-161">Für die Beispiele in diesem Dokument wird `DbContext` in der Datei [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) initialisiert.</span><span class="sxs-lookup"><span data-stu-id="c1027-161">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) file.</span></span>

<span data-ttu-id="c1027-162">[!code-cs[main](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]</span><span class="sxs-lookup"><span data-stu-id="c1027-162">[!code-cs[main](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]</span></span>

<span data-ttu-id="c1027-163">Das Datenmodell:</span><span class="sxs-lookup"><span data-stu-id="c1027-163">The data model:</span></span>

<span data-ttu-id="c1027-164">[!code-cs[main](index/sample/RazorPagesContacts/Data/Customer.cs)]</span><span class="sxs-lookup"><span data-stu-id="c1027-164">[!code-cs[main](index/sample/RazorPagesContacts/Data/Customer.cs)]</span></span>

<span data-ttu-id="c1027-165">Die Umgebungsdatei *Pages/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="c1027-165">The *Pages/Create.cshtml* view file:</span></span>

<span data-ttu-id="c1027-166">[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Create.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="c1027-166">[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Create.cshtml)]</span></span>

<span data-ttu-id="c1027-167">Die CodeBehind-Datei *Pages/Create.cshtml.cs* für die Ansicht:</span><span class="sxs-lookup"><span data-stu-id="c1027-167">The *Pages/Create.cshtml.cs* code-behind file for the view:</span></span>

<span data-ttu-id="c1027-168">[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=ALL)]</span><span class="sxs-lookup"><span data-stu-id="c1027-168">[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=ALL)]</span></span>

<span data-ttu-id="c1027-169">Die `PageModel`-Klasse heißt standardmäßig `<PageName>Model` und befindet sich im selben Namespace wie die Seite.</span><span class="sxs-lookup"><span data-stu-id="c1027-169">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span> <span data-ttu-id="c1027-170">Um von einer Seite mit `@functions` zu konvertieren und so Handler und eine Seite mit einer `PageModel`-Klasse zu definieren, sind keine großen Änderungen erforderlich.</span><span class="sxs-lookup"><span data-stu-id="c1027-170">Not much change is needed to convert from a page using `@functions` to define handlers and a page using a `PageModel` class.</span></span>

<span data-ttu-id="c1027-171">Für die Verwendung einer `PageModel`-CodeBehind-Datei werden Unittests unterstützt. Sie müssen aber einen expliziten Konstruktor und Klasse schreiben.</span><span class="sxs-lookup"><span data-stu-id="c1027-171">Using a `PageModel` code-behind file supports unit testing, but requires you to write an explicit constructor and class.</span></span> <span data-ttu-id="c1027-172">Seiten ohne `PageModel`-CodeBehind-Dateien unterstützen die Runtimekompilierung. Dies kann ein Vorteil bei der Entwicklung sein.</span><span class="sxs-lookup"><span data-stu-id="c1027-172">Pages without `PageModel` code-behind files support runtime compilation, which can be an advantage in development.</span></span>  <!-- review: advantage because you can make changes and refresh the browser without explicitly compiling the app -->

<span data-ttu-id="c1027-173">Die Seite hat eine `OnPostAsync`-*Handlermethode*, die auf `POST`-Anforderungen ausgeführt wird (wenn ein Benutzer das Formular sendet).</span><span class="sxs-lookup"><span data-stu-id="c1027-173">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="c1027-174">Sie können Handlermethoden für alle HTTP-Verben hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="c1027-174">You can add handler methods for any HTTP verb.</span></span> <span data-ttu-id="c1027-175">Die am häufigsten verwendeten Handler sind:</span><span class="sxs-lookup"><span data-stu-id="c1027-175">The most common handlers are:</span></span>

* <span data-ttu-id="c1027-176">`OnGet`, um den für eine Seite erforderlichen Status zu initialisieren.</span><span class="sxs-lookup"><span data-stu-id="c1027-176">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="c1027-177">[OnGet](#OnGet)-Beispiel</span><span class="sxs-lookup"><span data-stu-id="c1027-177">[OnGet](#OnGet) sample.</span></span>
* <span data-ttu-id="c1027-178">`OnPost`, um Formularübermittlungen zu behandeln</span><span class="sxs-lookup"><span data-stu-id="c1027-178">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="c1027-179">Das Namenssuffix `Async` ist optional. Es wird jedoch standardmäßig häufig für asynchrone Funktionen verwendet.</span><span class="sxs-lookup"><span data-stu-id="c1027-179">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="c1027-180">Der `OnPostAsync`-Code im vorhergehenden Beispiel ähnelt dem, was Sie normalerweise in einem Controller schreiben würden.</span><span class="sxs-lookup"><span data-stu-id="c1027-180">The `OnPostAsync` code in the preceding example looks similar to what you would normally write in a controller.</span></span> <span data-ttu-id="c1027-181">Der vorhergehende Code ist typisch für Razor-Seiten.</span><span class="sxs-lookup"><span data-stu-id="c1027-181">The preceding code is typical for Razor Pages.</span></span> <span data-ttu-id="c1027-182">Die meisten primitiven MVC-Typen wie die [Modellbindung](xref:mvc/models/model-binding), die [Validierung](xref:mvc/models/validation) und Aktionsergebnisse werden freigegeben.</span><span class="sxs-lookup"><span data-stu-id="c1027-182">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results are shared.</span></span>  <!-- Review: Ryan, can we get a list of what is shared and what isn't? -->

<span data-ttu-id="c1027-183">Die vorherige `OnPostAsync`-Methode:</span><span class="sxs-lookup"><span data-stu-id="c1027-183">The previous `OnPostAsync` method:</span></span>

<span data-ttu-id="c1027-184">[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=OnPostAsync)]</span><span class="sxs-lookup"><span data-stu-id="c1027-184">[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=OnPostAsync)]</span></span>

<span data-ttu-id="c1027-185">Der grundlegende Ablauf von `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="c1027-185">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="c1027-186">Prüfen auf Validierungsfehler</span><span class="sxs-lookup"><span data-stu-id="c1027-186">Check for validation errors.</span></span>

*  <span data-ttu-id="c1027-187">Wenn keine Fehler vorliegen, werden die Daten gespeichert und weitergeleitet.</span><span class="sxs-lookup"><span data-stu-id="c1027-187">If there are no errors, save the data and redirect.</span></span>
*  <span data-ttu-id="c1027-188">Wenn es Fehler gibt, zeigen Sie die Seite erneut mit den Validierungsmeldungen an.</span><span class="sxs-lookup"><span data-stu-id="c1027-188">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="c1027-189">Die clientseitige Validierung ist identisch mit herkömmlichen ASP.NET Core MVC-Anwendungen,</span><span class="sxs-lookup"><span data-stu-id="c1027-189">Client-side validation is identical to traditional ASP.NET Core MVC applications.</span></span> <span data-ttu-id="c1027-190">denn Validierungsfehler werden oftmals auf dem Client erkannt und nie an den Server übermittelt.</span><span class="sxs-lookup"><span data-stu-id="c1027-190">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="c1027-191">Wenn die Daten erfolgreich eingegeben wurden, ruft die `OnPostAsync`-Handlermethode die `RedirectToPage`-Hilfsmethode auf, um eine Instanz von `RedirectToPageResult` zurückzugeben.</span><span class="sxs-lookup"><span data-stu-id="c1027-191">When the data is entered successfully, the `OnPostAsync` handler method calls the `RedirectToPage` helper method to return an instance of `RedirectToPageResult`.</span></span> <span data-ttu-id="c1027-192">`RedirectToPage` ist ein neues Aktionsergebnis und ähnelt `RedirectToAction` oder `RedirectToRoute`, ist aber für Seiten angepasst.</span><span class="sxs-lookup"><span data-stu-id="c1027-192">`RedirectToPage` is a new action result, similar to `RedirectToAction` or `RedirectToRoute`, but customized for pages.</span></span> <span data-ttu-id="c1027-193">Im vorhergehenden Beispiel leitet es an die Stammindexseite (`/Index`) weiter.</span><span class="sxs-lookup"><span data-stu-id="c1027-193">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="c1027-194">Informationen zu `RedirectToPage` finden Sie im Abschnitt [URL-Generierung für Seiten](#url_gen).</span><span class="sxs-lookup"><span data-stu-id="c1027-194">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

<span data-ttu-id="c1027-195">Wenn das übermittelte Formular Validierungsfehler enthält (die an den Server übergeben wurden), ruft die `OnPostAsync`-Handlermethode die `Page`-Hilfsmethode auf.</span><span class="sxs-lookup"><span data-stu-id="c1027-195">When the submitted form has validation errors (that are passed to the server), the`OnPostAsync` handler method calls the `Page` helper method.</span></span> <span data-ttu-id="c1027-196">`Page` gibt eine Instanz von `PageResult` zurück.</span><span class="sxs-lookup"><span data-stu-id="c1027-196">`Page` returns an instance of `PageResult`.</span></span> <span data-ttu-id="c1027-197">Der Vorgang, bei dem `Page` zurückgegeben wird, ähnelt dem Vorgang, bei dem Aktionen im Controller `View` zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="c1027-197">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="c1027-198">`PageResult` ist der standardmäßige <!-- Review  -->-Rückgabetyp für eine Handlermethode.</span><span class="sxs-lookup"><span data-stu-id="c1027-198">`PageResult` is the default <!-- Review  --> return type for a handler method.</span></span> <span data-ttu-id="c1027-199">Eine Handlermethode, die `void` zurückgibt, rendert die Seite.</span><span class="sxs-lookup"><span data-stu-id="c1027-199">A handler method that returns `void` renders the page.</span></span>

<span data-ttu-id="c1027-200">Die Eigenschaft `Customer` verwendet das `[BindProperty]`-Attribut, um die Modellbindung zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="c1027-200">The `Customer` property uses `[BindProperty]` attribute to opt in to model binding.</span></span>

<span data-ttu-id="c1027-201">[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=PageModel&highlight=10-11)]</span><span class="sxs-lookup"><span data-stu-id="c1027-201">[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=PageModel&highlight=10-11)]</span></span>

<span data-ttu-id="c1027-202">Razor-Seiten binden Eigenschaften standardmäßig nur an Nicht-GET-Verben.</span><span class="sxs-lookup"><span data-stu-id="c1027-202">Razor Pages, by default, bind properties only with non-GET verbs.</span></span> <span data-ttu-id="c1027-203">Durch die Bindung an Eigenschaften können Sie den Umfang von Codes reduzieren, den Sie schreiben müssen.</span><span class="sxs-lookup"><span data-stu-id="c1027-203">Binding to properties can reduce the amount of code you have to write.</span></span> <span data-ttu-id="c1027-204">Die Bindung reduziert den Code mithilfe der gleichen Eigenschaft, um Formularfelder (`<input asp-for="Customer.Name" />`) zu rendern und die Eingabe zu akzeptieren.</span><span class="sxs-lookup"><span data-stu-id="c1027-204">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name" />`) and accept the input.</span></span>

<span data-ttu-id="c1027-205">Der folgende Code zeigt die kombinierte Version der Erstellungsseite:</span><span class="sxs-lookup"><span data-stu-id="c1027-205">The following code shows the combined version of the create page:</span></span>

<span data-ttu-id="c1027-206">[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/CreateCombined.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="c1027-206">[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/CreateCombined.cshtml)]</span></span>

<span data-ttu-id="c1027-207">Statt `@model` zu verwenden, nutzen wir ein neues Feature von Seiten.</span><span class="sxs-lookup"><span data-stu-id="c1027-207">Rather than using `@model`, we're taking advantage of a new feature for Pages.</span></span> <span data-ttu-id="c1027-208">Standardmäßig *ist* die generierte, von `Page` abgeleitete Klasse das Modell.</span><span class="sxs-lookup"><span data-stu-id="c1027-208">By default, the generated `Page`-derived class *is* the model.</span></span> <span data-ttu-id="c1027-209">Eine bewährte Methode ist die Verwendung eines *Ansichtsmodells* mit Razor-Ansichten.</span><span class="sxs-lookup"><span data-stu-id="c1027-209">Using a *view model* with Razor views is a best practice.</span></span> <span data-ttu-id="c1027-210">Mit Seiten erhalten Sie *automatisch* ein Ansichtsmodell.</span><span class="sxs-lookup"><span data-stu-id="c1027-210">With Pages, you get a view model *automatically*.</span></span>

<span data-ttu-id="c1027-211">Die Hauptänderung besteht im Ersetzen der Konstruktorinjektion durch eingefügte (`@inject`) Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="c1027-211">The main change is replacing constructor injection with injected (`@inject`) properties.</span></span> <span data-ttu-id="c1027-212">Diese Seite verwendet [@inject](xref:mvc/views/razor#inject) für die [konstruktorbasierte Abhängigkeitsinjektion](xref:mvc/controllers/dependency-injection#constructor-injection).</span><span class="sxs-lookup"><span data-stu-id="c1027-212">This page uses [@inject](xref:mvc/views/razor#inject) for [constructor dependency injection](xref:mvc/controllers/dependency-injection#constructor-injection).</span></span> <span data-ttu-id="c1027-213">Die `@inject`-Anweisung generiert und initialisiert die Eigenschaft `Db`, die in `OnPostAsync` verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="c1027-213">The `@inject` statement generates and initializes the `Db` property that is used in `OnPostAsync`.</span></span> <span data-ttu-id="c1027-214">Eingefügte (`@inject`) Eigenschaften werden festgelegt, bevor Handlermethoden ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="c1027-214">Injected (`@inject`) properties are set before handler methods run.</span></span>


<span data-ttu-id="c1027-215">Die Startseite (*Index.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="c1027-215">The home page (*Index.cshtml*):</span></span>

<span data-ttu-id="c1027-216">[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Index.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="c1027-216">[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Index.cshtml)]</span></span>

<span data-ttu-id="c1027-217">Die CodeBehind-Datei *Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="c1027-217">The code behind *Index.cshtml.cs* file:</span></span>

<span data-ttu-id="c1027-218">[!code-cs[main](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]</span><span class="sxs-lookup"><span data-stu-id="c1027-218">[!code-cs[main](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]</span></span>

<span data-ttu-id="c1027-219">Die Datei *Index.cshtml* enthält das folgende Markup, um einen Bearbeitungslink für jeden Kontakt zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="c1027-219">The *Index.cshtml* file contains the following markup to create an edit link for each contact:</span></span>

```html
<a asp-page="./Edit" asp-route-id="@contact.Id">edit</a>
```

<span data-ttu-id="c1027-220">Das [Anchor-Taghilfsprogramm](xref:mvc/views/tag-helpers/builtin-th/AnchorTagHelper) verwendet das Attribut [asp-route-{Wert}](xref:mvc/views/tag-helpers/builtin-th/AnchorTagHelper#route), um einen Link zur Bearbeitungsseite zu generieren.</span><span class="sxs-lookup"><span data-stu-id="c1027-220">The [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/AnchorTagHelper) used the [asp-route-{value}](xref:mvc/views/tag-helpers/builtin-th/AnchorTagHelper#route) attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="c1027-221">Der Link enthält die Routendaten mit der Kontakt-ID.</span><span class="sxs-lookup"><span data-stu-id="c1027-221">The link contains route data with the contact ID.</span></span> <span data-ttu-id="c1027-222">Beispielsweise `http://localhost:5000/Edit/1`.</span><span class="sxs-lookup"><span data-stu-id="c1027-222">For example, `http://localhost:5000/Edit/1`.</span></span>

<span data-ttu-id="c1027-223">Die Datei *Pages/Edit.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="c1027-223">The *Pages/Edit.cshtml* file:</span></span>

<span data-ttu-id="c1027-224">[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]</span><span class="sxs-lookup"><span data-stu-id="c1027-224">[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]</span></span>

<span data-ttu-id="c1027-225">Die erste Zeile enthält die `@page "{id:int}"`-Anweisung.</span><span class="sxs-lookup"><span data-stu-id="c1027-225">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="c1027-226">Die Routingbeschränkung `"{id:int}"` weist die Seite an, die Anforderungen für die Seite zu akzeptieren, die `int`-Routingdaten enthalten.</span><span class="sxs-lookup"><span data-stu-id="c1027-226">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="c1027-227">Wenn eine Anforderung an die Seite bestimmte Routingdaten nicht enthält, die in einen `int` konvertiert werden können, gibt die Runtime einen Fehler vom Typ „HTTP 404: Nicht gefunden“ zurück.</span><span class="sxs-lookup"><span data-stu-id="c1027-227">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span>

<span data-ttu-id="c1027-228">Die Datei *Pages/Edit.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="c1027-228">The *Pages/Edit.cshtml.cs* file:</span></span>

<span data-ttu-id="c1027-229">[!code-cs[main](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]</span><span class="sxs-lookup"><span data-stu-id="c1027-229">[!code-cs[main](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]</span></span>

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="c1027-230">XSRF/CSRF und Razor-Seiten</span><span class="sxs-lookup"><span data-stu-id="c1027-230">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="c1027-231">Sie müssen keinen Code für die [Antifälschungsvalidierung](xref:security/anti-request-forgery) schreiben.</span><span class="sxs-lookup"><span data-stu-id="c1027-231">You don't have to write any code for [antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="c1027-232">Die Generierung und Validierung von Antifälschungstoken ist automatisch in Razor-Seiten enthalten.</span><span class="sxs-lookup"><span data-stu-id="c1027-232">Antiforgery token generation and validation are automatically included in Razor Pages.</span></span>

<a name="layout"></a>
## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="c1027-233">Verwenden von Layouts, Teilansichten, Vorlagen und Taghilfsprogrammen mit Razor-Seiten</span><span class="sxs-lookup"><span data-stu-id="c1027-233">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="c1027-234">Razor-Seiten funktionieren mit allen Funktionen des Razor-Anzeigemoduls.</span><span class="sxs-lookup"><span data-stu-id="c1027-234">Pages work with all the features of the Razor view engine.</span></span> <span data-ttu-id="c1027-235">Layouts, Teilansichten, Vorlagen, Taghilfsprogramme, *_ViewStart.cshtml*, *_ViewImports.cshtml* funktionieren auf die gleiche Weise wie für herkömmliche Razor-Ansichten.</span><span class="sxs-lookup"><span data-stu-id="c1027-235">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="c1027-236">Lassen Sie uns diese Seite mithilfe einiger dieser Funktionen entwirren.</span><span class="sxs-lookup"><span data-stu-id="c1027-236">Let's declutter this page by taking advantage of some of those features.</span></span>

<span data-ttu-id="c1027-237">Fügen Sie einer *Pages/_Layout.cshtml* eine [Layoutseite](xref:mvc/views/layout) hinzu:</span><span class="sxs-lookup"><span data-stu-id="c1027-237">Add a [layout page](xref:mvc/views/layout) to *Pages/_Layout.cshtml*:</span></span>

<span data-ttu-id="c1027-238">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="c1027-238">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]</span></span>

<span data-ttu-id="c1027-239">Das [Layout](xref:mvc/views/layout):</span><span class="sxs-lookup"><span data-stu-id="c1027-239">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="c1027-240">Steuert das Layout der einzelnen Seiten, es sei denn, das Layout wird für eine Seite deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="c1027-240">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="c1027-241">Importiert HTML-Strukturen, z.B. JavaScript und Stylesheets.</span><span class="sxs-lookup"><span data-stu-id="c1027-241">Imports HTML structures such as JavaScript and stylesheets.</span></span>

<span data-ttu-id="c1027-242">Weitere Informationen finden Sie unter [Layoutseite](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="c1027-242">See [layout page](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="c1027-243">Die Eigenschaft [Layout](xref:mvc/views/layout#specifying-a-layout) wird in *Pages/_ViewStart.cshtml* festgelegt:</span><span class="sxs-lookup"><span data-stu-id="c1027-243">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

<span data-ttu-id="c1027-244">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="c1027-244">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]</span></span>

<span data-ttu-id="c1027-245">Hinweis: Das Layout befindet sich im Ordner *Pages* (Seiten).</span><span class="sxs-lookup"><span data-stu-id="c1027-245">Note: The layout is in the *Pages* folder.</span></span> <span data-ttu-id="c1027-246">Seiten suchen hierarchisch nach anderen Ansichten (Layouts, Vorlagen oder Teilansichten) und beginnen im gleichen Ordner wie die aktuelle Seite.</span><span class="sxs-lookup"><span data-stu-id="c1027-246">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="c1027-247">Ein Layout im Ordner *Seiten* kann aus jeder Razor-Seite unter dem Ordner *Pages* verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="c1027-247">A layout in the *Pages* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="c1027-248">Wir empfehlen Ihnen, die Layoutdatei **nicht** im Ordner *Views/Shared* (Ansichten/Freigegeben) zu platzieren.</span><span class="sxs-lookup"><span data-stu-id="c1027-248">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="c1027-249">*Views/Shared* ist ein MVC-Ansichtsmuster.</span><span class="sxs-lookup"><span data-stu-id="c1027-249">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="c1027-250">Razor-Seiten basieren auf der Ordnerhierarchie, nicht auf Pfadkonventionen.</span><span class="sxs-lookup"><span data-stu-id="c1027-250">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="c1027-251">Die Ansichtensuche in einer Razor-Seite enthält den Ordner *Pages*.</span><span class="sxs-lookup"><span data-stu-id="c1027-251">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="c1027-252">Die Layouts, Vorlagen und Teilansichten, die Sie mit MVC-Controllern und herkömmlichen Razor-Ansichten verwenden, *funktionieren einfach so*.</span><span class="sxs-lookup"><span data-stu-id="c1027-252">The layouts, templates, and partials you're using with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="c1027-253">Fügen Sie eine Datei *Pages/_ViewImports.cshtml* hinzu:</span><span class="sxs-lookup"><span data-stu-id="c1027-253">Add a *Pages/_ViewImports.cshtml* file:</span></span>

<span data-ttu-id="c1027-254">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="c1027-254">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]</span></span>

<span data-ttu-id="c1027-255">`@namespace` wird weiter unten im Tutorial erläutert.</span><span class="sxs-lookup"><span data-stu-id="c1027-255">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="c1027-256">Die `@addTagHelper`-Anweisung bringt die [integrierten Taghilfsprogramme](https://docs.microsoft.com/aspnet/core/mvc/views/tag-helpers/built-in/) zu allen Seiten in der Ordner *Pages*.</span><span class="sxs-lookup"><span data-stu-id="c1027-256">The `@addTagHelper` directive brings in the [built-in Tag Helpers](https://docs.microsoft.com/aspnet/core/mvc/views/tag-helpers/built-in/) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="c1027-257">Wenn die `@namespace`-Anweisung explizit auf eine Seite angewendet wird:</span><span class="sxs-lookup"><span data-stu-id="c1027-257">When the `@namespace` directive is used explicitly on a page:</span></span>

<span data-ttu-id="c1027-258">[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="c1027-258">[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]</span></span>

<span data-ttu-id="c1027-259">Die Anweisung legt den Namespace für die Seite fest.</span><span class="sxs-lookup"><span data-stu-id="c1027-259">The directive sets the namespace for the page.</span></span> <span data-ttu-id="c1027-260">Die `@model`-Anweisung muss den Namespace nicht enthalten.</span><span class="sxs-lookup"><span data-stu-id="c1027-260">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="c1027-261">Wenn sich die `@namespace`-Anweisung in *_ViewImports.cshtml* befindet, stellt der angegebene Namespace das Präfix für den generierten Namespace auf der Seite bereit, die die `@namespace`-Anweisung importiert.</span><span class="sxs-lookup"><span data-stu-id="c1027-261">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="c1027-262">Der Rest der generierten Namespaces (der Suffixteil) ist der durch Punkte getrennte relative Pfad zwischen dem Ordner mit *_ViewImports.cshtml* und dem Ordner, der die Seite enthält.</span><span class="sxs-lookup"><span data-stu-id="c1027-262">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="c1027-263">Die CodeBehind-Datei *Pages/Customers/Edit.cshtml.cs* legt den Namespace z.B. explizit fest:</span><span class="sxs-lookup"><span data-stu-id="c1027-263">For example, the code behind file *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

<span data-ttu-id="c1027-264">[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=namespace)]</span><span class="sxs-lookup"><span data-stu-id="c1027-264">[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=namespace)]</span></span>

<span data-ttu-id="c1027-265">Die Datei *Pages/_ViewImports.cshtml* legt den folgenden Namespace fest:</span><span class="sxs-lookup"><span data-stu-id="c1027-265">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

<span data-ttu-id="c1027-266">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]</span><span class="sxs-lookup"><span data-stu-id="c1027-266">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]</span></span>

<span data-ttu-id="c1027-267">Der generierte Namespace für die Razor-Seite *Pages/Customers/Edit.cshtml* ist identisch mit der CodeBehind-Datei.</span><span class="sxs-lookup"><span data-stu-id="c1027-267">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the code behind file.</span></span> <span data-ttu-id="c1027-268">Die `@namespace`-Anweisung wurde entworfen, damit die einem Projekt hinzugefügten C#-Klassen und mit Seiten generierter Code *einfach so funktionieren*, ohne dass eine `@using`-Anweisung für die CodeBehind-Datei hinzugefügt werden muss.</span><span class="sxs-lookup"><span data-stu-id="c1027-268">The `@namespace` directive was designed so the C# classes added to a project and pages-generated code *just work* without having to add an `@using` directive for the code behind file.</span></span>

<span data-ttu-id="c1027-269">Hinweis: `@namespace` funktioniert auch mit konventionellen Razor-Ansichten.</span><span class="sxs-lookup"><span data-stu-id="c1027-269">Note: `@namespace` also works with conventional Razor views.</span></span>

<span data-ttu-id="c1027-270">Die ursprüngliche Umgebungsdatei *Pages/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="c1027-270">The original *Pages/Create.cshtml* view file:</span></span>

<span data-ttu-id="c1027-271">[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="c1027-271">[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]</span></span>

<span data-ttu-id="c1027-272">Die aktualisierte Seite:</span><span class="sxs-lookup"><span data-stu-id="c1027-272">The updated page:</span></span>

<span data-ttu-id="c1027-273">Die Umgebungsdatei *Pages/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="c1027-273">The *Pages/Create.cshtml* view file:</span></span>

<span data-ttu-id="c1027-274">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="c1027-274">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]</span></span>

<span data-ttu-id="c1027-275">Die [Razor-Seiten-Startprojekt](#rpvs17) enthält die Seite *Pages/_ValidationScriptsPartial.cshtml*, die die clientseitige Validierung bindet.</span><span class="sxs-lookup"><span data-stu-id="c1027-275">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="c1027-276">URL-Generierung für Seiten</span><span class="sxs-lookup"><span data-stu-id="c1027-276">URL generation for Pages</span></span>

<span data-ttu-id="c1027-277">Die zuvor gezeigte `Create`-Seite verwendet `RedirectToPage`:</span><span class="sxs-lookup"><span data-stu-id="c1027-277">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

<span data-ttu-id="c1027-278">[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=OnPostAsync&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="c1027-278">[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=OnPostAsync&highlight=10)]</span></span>

<span data-ttu-id="c1027-279">Die App hat die folgende Datei/Ordner-Struktur</span><span class="sxs-lookup"><span data-stu-id="c1027-279">The app has the following file/folder structure</span></span>

* <span data-ttu-id="c1027-280">*/Pages*</span><span class="sxs-lookup"><span data-stu-id="c1027-280">*/Pages*</span></span>

  * <span data-ttu-id="c1027-281">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="c1027-281">*Index.cshtml*</span></span>
  * <span data-ttu-id="c1027-282">*/Customer*</span><span class="sxs-lookup"><span data-stu-id="c1027-282">*/Customer*</span></span>

    * <span data-ttu-id="c1027-283">*Create.cshtml*</span><span class="sxs-lookup"><span data-stu-id="c1027-283">*Create.cshtml*</span></span>
    * <span data-ttu-id="c1027-284">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="c1027-284">*Edit.cshtml*</span></span>
    * <span data-ttu-id="c1027-285">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="c1027-285">*Index.cshtml*</span></span>

<span data-ttu-id="c1027-286">Die Seiten *Pages/Customers/Create.cshtml* und *Pages/Customers/Edit.cshtml* leiten bei Erfolg an *Pages/Index.cshtml* weiter.</span><span class="sxs-lookup"><span data-stu-id="c1027-286">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Index.cshtml* after success.</span></span> <span data-ttu-id="c1027-287">Die Zeichenfolge `/Index` ist Teil des URI, der auf die vorhergehende Seite zugreifen soll.</span><span class="sxs-lookup"><span data-stu-id="c1027-287">The string `/Index` is part of the URI to access the preceding page.</span></span> <span data-ttu-id="c1027-288">Die Zeichenfolge `/Index` kann für das Generieren von URIs für die Seite *Pages/Index.cshtml* verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="c1027-288">The string `/Index` can be used to generate URIs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="c1027-289">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="c1027-289">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="c1027-290">Der Seitenname ist der Pfad zu der Seite vom Stammordner */Pages* (einschließlich eines vorangestellten `/`, z.B. `/Index`).</span><span class="sxs-lookup"><span data-stu-id="c1027-290">The page name is the path to the page from the root */Pages* folder (including a leading `/`, for example `/Index`).</span></span> <span data-ttu-id="c1027-291">Die vorherigen Beispiele für die URL-Generierung sind viel umfangreicher an Features als eine einfache hartcodierte URL.</span><span class="sxs-lookup"><span data-stu-id="c1027-291">The preceding URL generation samples are much more feature rich than just hardcoding a URL.</span></span> <span data-ttu-id="c1027-292">Bei der URL-Generierung wird [Routing](xref:mvc/controllers/routing) verwendet. Außerdem können damit Parameter generiert und entsprechend der Definition der Route im Zielpfad codiert werden.</span><span class="sxs-lookup"><span data-stu-id="c1027-292">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="c1027-293">Die URL-Generierung für Seiten unterstützt relative Namen.</span><span class="sxs-lookup"><span data-stu-id="c1027-293">URL generation for pages supports relative names.</span></span> <span data-ttu-id="c1027-294">In der folgenden Tabelle wird dargestellt, welche Indexseite für verschiedene `RedirectToPage`-Parameter aus *Pages/Customers/Create.cshtml* ausgewählt wird:</span><span class="sxs-lookup"><span data-stu-id="c1027-294">The following table shows which Index page is selected with different `RedirectToPage` parameters from *Pages/Customers/Create.cshtml*:</span></span>

| <span data-ttu-id="c1027-295">RedirectToPage(x)</span><span class="sxs-lookup"><span data-stu-id="c1027-295">RedirectToPage(x)</span></span>| <span data-ttu-id="c1027-296">Seite</span><span class="sxs-lookup"><span data-stu-id="c1027-296">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="c1027-297">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="c1027-297">RedirectToPage("/Index")</span></span> | <span data-ttu-id="c1027-298">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="c1027-298">*Pages/Index*</span></span> |
| <span data-ttu-id="c1027-299">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="c1027-299">RedirectToPage("./Index");</span></span> | <span data-ttu-id="c1027-300">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="c1027-300">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="c1027-301">RedirectToPage("../Index")</span><span class="sxs-lookup"><span data-stu-id="c1027-301">RedirectToPage("../Index")</span></span> | <span data-ttu-id="c1027-302">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="c1027-302">*Pages/Index*</span></span> |
| <span data-ttu-id="c1027-303">RedirectToPage("Index")</span><span class="sxs-lookup"><span data-stu-id="c1027-303">RedirectToPage("Index")</span></span>  | <span data-ttu-id="c1027-304">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="c1027-304">*Pages/Customers/Index*</span></span> |

<span data-ttu-id="c1027-305">`RedirectToPage("Index")`, `RedirectToPage("./Index")` und `RedirectToPage("../Index")` sind *relative Namen*.</span><span class="sxs-lookup"><span data-stu-id="c1027-305">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")`  are *relative names*.</span></span> <span data-ttu-id="c1027-306">Der `RedirectToPage`-Parameter wird mit dem Pfad der aktuellen Seite *kombiniert*, um den Namen der Zielseite zu berechnen.</span><span class="sxs-lookup"><span data-stu-id="c1027-306">The `RedirectToPage` parameter is *combined* with the path of the current page to compute the name of the destination page.</span></span>  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page. -- page name, not page path -->

<span data-ttu-id="c1027-307">Das Verknüpfen relativer Namen eignet sich beim Erstellen von Websites mit einer komplexen Struktur.</span><span class="sxs-lookup"><span data-stu-id="c1027-307">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="c1027-308">Wenn Sie relative Namen verwenden, um Seiten in einem Ordner zu verknüpfen, können Sie diesen Ordner umbenennen.</span><span class="sxs-lookup"><span data-stu-id="c1027-308">If you use relative names to link between pages in a folder, you can rename that folder.</span></span> <span data-ttu-id="c1027-309">Alle Links funktionieren weiterhin, da sie nicht den Namen des Ordners enthalten.</span><span class="sxs-lookup"><span data-stu-id="c1027-309">All the links still work (because they didn't include the folder name).</span></span>

## <a name="tempdata"></a><span data-ttu-id="c1027-310">TempData</span><span class="sxs-lookup"><span data-stu-id="c1027-310">TempData</span></span>

<span data-ttu-id="c1027-311">ASP.NET Core macht die Eigenschaft [TempData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller#Microsoft_AspNetCore_Mvc_Controller_TempData) auf einem [Controller](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller) verfügbar.</span><span class="sxs-lookup"><span data-stu-id="c1027-311">ASP.NET Core exposes the [TempData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="c1027-312">Diese Eigenschaft speichert Daten, bis sie gelesen wurden.</span><span class="sxs-lookup"><span data-stu-id="c1027-312">This property stores data until it is read.</span></span> <span data-ttu-id="c1027-313">Die Methoden `Keep` und `Peek` können verwendet werden, um die Daten zu überprüfen, ohne sie zu löschen.</span><span class="sxs-lookup"><span data-stu-id="c1027-313">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="c1027-314">`TempData` eignet sich für die Weiterleitung, wenn Daten für mehr als eine Anforderung benötigt werden.</span><span class="sxs-lookup"><span data-stu-id="c1027-314">`TempData` is  useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="c1027-315">Das `[TempData]`-Attribut ist neu in ASP.NET Core 2.0 und wird auf Domänencontrollern und Seiten unterstützt.</span><span class="sxs-lookup"><span data-stu-id="c1027-315">The `[TempData]` attribute is new in ASP.NET Core 2.0 and is supported on controllers and pages.</span></span>

<span data-ttu-id="c1027-316">Im folgenden Code wird der Wert von `Message` mit `TempData` festgelegt.</span><span class="sxs-lookup"><span data-stu-id="c1027-316">The following code sets the value of `Message` using `TempData`.</span></span>
<span data-ttu-id="c1027-317">[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,27-28&name=snippetTemp)]</span><span class="sxs-lookup"><span data-stu-id="c1027-317">[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,27-28&name=snippetTemp)]</span></span>

<span data-ttu-id="c1027-318">Das folgende Markup in der Datei *Pages/Customers/Index.cshtml* zeigt den Wert von `Message` mit `TempData` an.</span><span class="sxs-lookup"><span data-stu-id="c1027-318">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="c1027-319">Die CodeBehind-Datei *Pages/Customers/Index.cshtml.cs* wendet das `[TempData]`-Attribut auf die Eigenschaft `Message`.</span><span class="sxs-lookup"><span data-stu-id="c1027-319">The *Pages/Customers/Index.cshtml.cs* code-behind file applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="c1027-320">Weitere Informationen finden Sie unter [TempData](xref:fundamentals/app-state#temp).</span><span class="sxs-lookup"><span data-stu-id="c1027-320">See [TempData](xref:fundamentals/app-state#temp) for more information.</span></span>

<a name="mhpp"></a>
## <a name="multiple-handlers-per-page"></a><span data-ttu-id="c1027-321">Mehrere Handler pro Seite</span><span class="sxs-lookup"><span data-stu-id="c1027-321">Multiple handlers per page</span></span>

<span data-ttu-id="c1027-322">Die folgende Seite generiert mit dem `asp-page-handler`-Taghilfsprogramm Markup für zwei Handler:</span><span class="sxs-lookup"><span data-stu-id="c1027-322">The following page generates markup for two page handlers using the `asp-page-handler` Tag Helper:</span></span>

<span data-ttu-id="c1027-323">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]</span><span class="sxs-lookup"><span data-stu-id="c1027-323">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]</span></span>

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there is no `asp-` attribute   -->

<span data-ttu-id="c1027-324">Das Formular im vorherigen Beispiel hat zwei Sendeschaltflächen, und jede verwendet `FormActionTagHelper`, um an eine andere URL zu übermitteln.</span><span class="sxs-lookup"><span data-stu-id="c1027-324">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="c1027-325">Das `asp-page-handler`-Attribut ist eine Ergänzung für `asp-page`.</span><span class="sxs-lookup"><span data-stu-id="c1027-325">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="c1027-326">`asp-page-handler` generiert URLs, die als Übermittlungsziel jeweils die durch eine Seite festgelegte Handlermethode verwenden.</span><span class="sxs-lookup"><span data-stu-id="c1027-326">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="c1027-327">`asp-page` wird nicht angegeben, weil das Beispiel mit der aktuellen Seite verknüpft.</span><span class="sxs-lookup"><span data-stu-id="c1027-327">`asp-page` is not specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="c1027-328">Die CodeBehind-Datei:</span><span class="sxs-lookup"><span data-stu-id="c1027-328">The code-behind file:</span></span>

<span data-ttu-id="c1027-329">[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]</span><span class="sxs-lookup"><span data-stu-id="c1027-329">[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]</span></span>

<span data-ttu-id="c1027-330">Der vorherige Code verwendet *benannte Handlermethoden*.</span><span class="sxs-lookup"><span data-stu-id="c1027-330">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="c1027-331">Benannte Handlermethoden werden aus dem Text im Namen nach `On<HTTP Verb>` und vor `Async` (falls vorhanden) erstellt.</span><span class="sxs-lookup"><span data-stu-id="c1027-331">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="c1027-332">Im vorherigen Beispiel sind OnPost**JoinList**Async und OnPost**JoinListUC**Async die Seitenmethoden.</span><span class="sxs-lookup"><span data-stu-id="c1027-332">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="c1027-333">Wenn Sie *OnPost* und *Async* entfernen, lauten die Handlernamen `JoinList` und `JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="c1027-333">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

<span data-ttu-id="c1027-334">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]</span><span class="sxs-lookup"><span data-stu-id="c1027-334">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]</span></span>

<span data-ttu-id="c1027-335">Mit dem vorherigen Code lautet der URL-Pfad, der an `OnPostJoinListAsync` übermittelt, `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span><span class="sxs-lookup"><span data-stu-id="c1027-335">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="c1027-336">Der URL-Pfad, der an `OnPostJoinListUCAsync` übermittelt, lautet `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="c1027-336">The URL path that submits to `OnPostJoinListUCAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="customizing-routing"></a><span data-ttu-id="c1027-337">Anpassen des Routings</span><span class="sxs-lookup"><span data-stu-id="c1027-337">Customizing Routing</span></span>

<span data-ttu-id="c1027-338">Wenn Sie nicht möchten, dass die Abfragezeichenfolge `?handler=JoinList` in der URL enthalten ist, können Sie die Route ändern, damit der Handlername im Pfadteil der URL eingefügt wird.</span><span class="sxs-lookup"><span data-stu-id="c1027-338">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="c1027-339">Sie können die Route anpassen, indem Sie eine Routenvorlage in doppelten Anführungszeichen nach der `@page`-Anweisung hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="c1027-339">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

<span data-ttu-id="c1027-340">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]</span><span class="sxs-lookup"><span data-stu-id="c1027-340">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]</span></span>


<span data-ttu-id="c1027-341">Die vorherige Route platziert den Handlernamen im URL-Pfad statt in die Abfragezeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="c1027-341">The preceding route puts the handler name in the URL path instead of the query string.</span></span> <span data-ttu-id="c1027-342">Das `?` nach `handler` bedeutet, dass der Routenparameter optional ist.</span><span class="sxs-lookup"><span data-stu-id="c1027-342">The `?` following `handler` means the route parameter is optional.</span></span>

<span data-ttu-id="c1027-343">Sie können einer Seitenroute mit `@page` weitere Segmente und Parameter hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="c1027-343">You can use `@page` to add additional segments and parameters to a page's route.</span></span> <span data-ttu-id="c1027-344">Alles, was hier angegeben wird, wird der Standardroute der Seite **angefügt**.</span><span class="sxs-lookup"><span data-stu-id="c1027-344">Whatever's there is **appended** to the default route of the page.</span></span> <span data-ttu-id="c1027-345">Die Verwendung eines absoluten oder des virtuellen Pfads, um die Seitenroute (z.B. `"~/Some/Other/Path"`) zu ändern, wird nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="c1027-345">Using an absolute or virtual path to change the page's route (like `"~/Some/Other/Path"`) is not supported.</span></span>

## <a name="configuration-and-settings"></a><span data-ttu-id="c1027-346">Konfiguration und Einstellungen</span><span class="sxs-lookup"><span data-stu-id="c1027-346">Configuration and settings</span></span>

<span data-ttu-id="c1027-347">Um die erweiterten Optionen zu konfigurieren, verwenden Sie die Erweiterungsmethode `AddRazorPagesOptions` auf dem MVC-Generator:</span><span class="sxs-lookup"><span data-stu-id="c1027-347">To configure advanced options, use the extension method `AddRazorPagesOptions` on the MVC builder:</span></span>

<span data-ttu-id="c1027-348">[!code-cs[main](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="c1027-348">[!code-cs[main](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet1)]</span></span>

<span data-ttu-id="c1027-349">Derzeit können Sie `RazorPagesOptions` verwenden, um das Stammverzeichnis für Seiten festzulegen oder Anwendungsmodellkonventionen für Seiten hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="c1027-349">Currently you can use the `RazorPagesOptions` to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="c1027-350">So möchten wir in Zukunft eine höhere Erweiterbarkeit erreichen.</span><span class="sxs-lookup"><span data-stu-id="c1027-350">We hope to enable more extensibility this way in the future.</span></span>

<span data-ttu-id="c1027-351">Informationen zum Vorkompilieren von Ansichten finden Sie unter [Razor view compilation and precompilation in ASP.NET Core (Razor-Ansichtenkompilierung und Vorkompilierung in ASP.NET)](xref:mvc/views/view-compilation).</span><span class="sxs-lookup"><span data-stu-id="c1027-351">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation) .</span></span>

<span data-ttu-id="c1027-352">[Laden Sie Beispielcode herunter, oder zeigen Sie ihn an](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span><span class="sxs-lookup"><span data-stu-id="c1027-352">[Download or view sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span></span>

<span data-ttu-id="c1027-353">Lesen Sie auch den Artikel [Getting started with Razor Pages in ASP.NET Core (Erste Schritte mit Razor-Seiten in ASP.NET Core)](xref:tutorials/razor-pages/razor-pages-start), der auf diese Einführung aufbaut.</span><span class="sxs-lookup"><span data-stu-id="c1027-353">See [Getting started with Razor Pages in ASP.NET Core](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>
