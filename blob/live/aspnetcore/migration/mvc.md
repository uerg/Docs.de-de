---
title: Migrieren von ASP.NET MVC zu ASP.NET Core MVC
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/mvc
ms.openlocfilehash: 88e5b7575930434e291a7aa4daef429306c653e0
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
---
# <a name="migrating-from-aspnet-mvc-to-aspnet-core-mvc"></a><span data-ttu-id="ac90d-102">Migrieren von ASP.NET MVC zu ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="ac90d-102">Migrating From ASP.NET MVC to ASP.NET Core MVC</span></span>

<span data-ttu-id="ac90d-103">Durch [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), und [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="ac90d-103">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="ac90d-104">In diesem Artikel wird gezeigt, wie für den Einstieg in die Migration von einer ASP.NET MVC-Projekts zu [Core ASP.NET-MVC](../mvc/overview.md).</span><span class="sxs-lookup"><span data-stu-id="ac90d-104">This article shows how to get started migrating an ASP.NET MVC project to [ASP.NET Core MVC](../mvc/overview.md).</span></span> <span data-ttu-id="ac90d-105">In der Prozess wird hervorgehoben viele der Aufgaben, die von ASP.NET MVC geändert haben.</span><span class="sxs-lookup"><span data-stu-id="ac90d-105">In the process, it highlights many of the things that have changed from ASP.NET MVC.</span></span> <span data-ttu-id="ac90d-106">Migrieren von ASP.NET MVC umfasst mehrere Schritte, und dieser Artikel behandelt die Anfangssetup, grundlegende Controller und Ansichten, statische Inhalte und Client-Side-Abhängigkeiten.</span><span class="sxs-lookup"><span data-stu-id="ac90d-106">Migrating from ASP.NET MVC is a multiple step process and this article covers the initial setup, basic controllers and views, static content, and client-side dependencies.</span></span> <span data-ttu-id="ac90d-107">Zusätzliche Artikel behandelt Migrieren der Konfiguration und ID-Code in ASP.NET MVC-Projekte, die viele gefunden.</span><span class="sxs-lookup"><span data-stu-id="ac90d-107">Additional articles cover migrating configuration and identity code found in many ASP.NET MVC projects.</span></span>

> [!NOTE]
> <span data-ttu-id="ac90d-108">Die Versionsnummern in die Beispiele sind möglicherweise keine aktuellen.</span><span class="sxs-lookup"><span data-stu-id="ac90d-108">The version numbers in the samples might not be current.</span></span> <span data-ttu-id="ac90d-109">Sie müssen möglicherweise Ihre Projekte entsprechend aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="ac90d-109">You may need to update your projects accordingly.</span></span>

## <a name="create-the-starter-aspnet-mvc-project"></a><span data-ttu-id="ac90d-110">Erstellen von Starter ASP.NET MVC-Projekt</span><span class="sxs-lookup"><span data-stu-id="ac90d-110">Create the starter ASP.NET MVC project</span></span>

<span data-ttu-id="ac90d-111">Um das Upgrade zu demonstrieren, beginnen wir mit eine ASP.NET MVC-app erstellen.</span><span class="sxs-lookup"><span data-stu-id="ac90d-111">To demonstrate the upgrade, we'll start by creating a ASP.NET MVC app.</span></span> <span data-ttu-id="ac90d-112">Erstellen sie mit dem Namen *WebApp1* daher der Namespace des Projekts ASP.NET Core überein werden wir im nächsten Schritt erstellen.</span><span class="sxs-lookup"><span data-stu-id="ac90d-112">Create it with the name *WebApp1* so the namespace will match the ASP.NET Core project we create in the next step.</span></span>

![Visual Studio-Projekt ein neues Dialogfeld "](mvc/_static/new-project.png)

![Dialogfeld "Neues Webanwendung": MVC-Projektvorlage in ASP.NET Vorlagen Bereich ausgewählt](mvc/_static/new-project-select-mvc-template.png)

<span data-ttu-id="ac90d-115">*Optional:* ändern Sie den Namen der Projektmappe aus *WebApp1* auf *Mvc5*.</span><span class="sxs-lookup"><span data-stu-id="ac90d-115">*Optional:* Change the name of the Solution from *WebApp1* to *Mvc5*.</span></span> <span data-ttu-id="ac90d-116">Visual Studio zeigt den Namen der neuen Projektmappe (*Mvc5*), dem können sie einfacher, teilen Sie das Projekt über das nächste Projekt.</span><span class="sxs-lookup"><span data-stu-id="ac90d-116">Visual Studio will display the new solution name (*Mvc5*), which will make it easier to tell this project from the next project.</span></span>

## <a name="create-the-aspnet-core-project"></a><span data-ttu-id="ac90d-117">Erstellen des Projekts ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ac90d-117">Create the ASP.NET Core project</span></span>

<span data-ttu-id="ac90d-118">Erstellen Sie ein neues *leere* ASP.NET Core Web-app mit dem gleichen Namen wie das vorherige Projekt (*WebApp1*), damit die Namespaces in beiden Projekten übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="ac90d-118">Create a new *empty* ASP.NET Core web app with the same name as the previous project (*WebApp1*) so the namespaces in the two projects match.</span></span> <span data-ttu-id="ac90d-119">Mit dem gleichen Namespace erleichtert es, Code zwischen den zwei Projekten kopieren.</span><span class="sxs-lookup"><span data-stu-id="ac90d-119">Having the same namespace makes it easier to copy code between the two projects.</span></span> <span data-ttu-id="ac90d-120">Sie müssen zum Erstellen des Projekts in einem anderen Verzeichnis als das vorherige Projekt, das den gleichen Namen verwenden.</span><span class="sxs-lookup"><span data-stu-id="ac90d-120">You'll have to create this project in a different directory than the previous project to use the same name.</span></span>

![Dialogfeld "Neues Projekt"](mvc/_static/new_core.png)

![Dialogfeld "neues ASP.NET Web-Anwendung": leere Projektvorlage, die im Bereich der Vorlagen für ASP.NET Core ausgewählt](mvc/_static/new-project-select-empty-aspnet5-template.png)

* <span data-ttu-id="ac90d-123">*Optional:* Erstellen eines neuen ASP.NET Core app mithilfe der *Webanwendung* -Projektvorlage.</span><span class="sxs-lookup"><span data-stu-id="ac90d-123">*Optional:* Create a new ASP.NET Core app using the *Web Application* project template.</span></span> <span data-ttu-id="ac90d-124">Nennen Sie das Projekt *WebApp1*, und wählen Sie eine Authentifizierungsoption in **einzelne Benutzerkonten**.</span><span class="sxs-lookup"><span data-stu-id="ac90d-124">Name the project *WebApp1*, and select an authentication option of **Individual User Accounts**.</span></span> <span data-ttu-id="ac90d-125">Benennen Sie diese app *FullAspNetCore*.</span><span class="sxs-lookup"><span data-stu-id="ac90d-125">Rename this app to *FullAspNetCore*.</span></span> <span data-ttu-id="ac90d-126">Erstellen bei der Konvertierung dieses Projekts werden Sie Zeit sparen.</span><span class="sxs-lookup"><span data-stu-id="ac90d-126">Creating this project will save you time in the conversion.</span></span> <span data-ttu-id="ac90d-127">Betrachten Sie die Vorlage generiertem Code auf das Endergebnis finden Sie unter und im Code in das Konvertierungsprojekt zu kopieren.</span><span class="sxs-lookup"><span data-stu-id="ac90d-127">You can look at the template-generated code to see the end result or to copy code to the conversion project.</span></span> <span data-ttu-id="ac90d-128">Es ist auch hilfreich, wenn Sie auf einen Konvertierungsschritt für den Vergleich mit der Vorlage generiert Projekt festsitzt.</span><span class="sxs-lookup"><span data-stu-id="ac90d-128">It's also helpful when you get stuck on a conversion step to compare with the template-generated project.</span></span>

## <a name="configure-the-site-to-use-mvc"></a><span data-ttu-id="ac90d-129">Konfigurieren Sie den Standort für MVC verwenden</span><span class="sxs-lookup"><span data-stu-id="ac90d-129">Configure the site to use MVC</span></span>

* <span data-ttu-id="ac90d-130">Installieren der `Microsoft.AspNetCore.Mvc` und `Microsoft.AspNetCore.StaticFiles` NuGet-Pakete.</span><span class="sxs-lookup"><span data-stu-id="ac90d-130">Install the `Microsoft.AspNetCore.Mvc` and `Microsoft.AspNetCore.StaticFiles` NuGet packages.</span></span>

  <span data-ttu-id="ac90d-131">`Microsoft.AspNetCore.Mvc`ist das ASP.NET-MVC-Framework Core.</span><span class="sxs-lookup"><span data-stu-id="ac90d-131">`Microsoft.AspNetCore.Mvc` is the ASP.NET Core MVC framework.</span></span> <span data-ttu-id="ac90d-132">`Microsoft.AspNetCore.StaticFiles`ist der Handler für statische Dateien.</span><span class="sxs-lookup"><span data-stu-id="ac90d-132">`Microsoft.AspNetCore.StaticFiles` is the static file handler.</span></span> <span data-ttu-id="ac90d-133">Die ASP.NET-Laufzeit ist modular aufgebaut, und Sie müssen explizit entscheiden Sie sich für statische Dateien dienen (finden Sie unter [arbeiten mit statischen Dateien](../fundamentals/static-files.md)).</span><span class="sxs-lookup"><span data-stu-id="ac90d-133">The ASP.NET runtime is modular, and you must explicitly opt in to serve static files (see [Working with Static Files](../fundamentals/static-files.md)).</span></span>

* <span data-ttu-id="ac90d-134">Öffnen der *csproj* Datei (mit der rechten Maustaste des Projekts im **Projektmappen-Explorer** , und wählen Sie **bearbeiten WebApp1.csproj**) und fügen eine `PrepareForPublish` Ziel:</span><span class="sxs-lookup"><span data-stu-id="ac90d-134">Open the *.csproj* file (right-click the project in **Solution Explorer** and select **Edit WebApp1.csproj**) and add a `PrepareForPublish` target:</span></span>

  [!code-xml[Main](mvc/sample/WebApp1.csproj?range=21-23)]

  <span data-ttu-id="ac90d-135">Die `PrepareForPublish` Ziel für den Erwerb von Client-Side-Bibliotheken über Bower erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="ac90d-135">The `PrepareForPublish` target is needed for acquiring client-side libraries via Bower.</span></span> <span data-ttu-id="ac90d-136">Die müssen weiter unten besprochen.</span><span class="sxs-lookup"><span data-stu-id="ac90d-136">We'll talk about that later.</span></span>

* <span data-ttu-id="ac90d-137">Öffnen der *Startup.cs* Datei und ändern Sie den Code entsprechend der folgenden:</span><span class="sxs-lookup"><span data-stu-id="ac90d-137">Open the *Startup.cs* file and change the code to match the following:</span></span>

  [!code-csharp[Main](mvc/sample/Startup.cs?highlight=14,27-34)]

  <span data-ttu-id="ac90d-138">Die `UseStaticFiles` Erweiterungsmethode fügt Handler für statische Dateien.</span><span class="sxs-lookup"><span data-stu-id="ac90d-138">The `UseStaticFiles` extension method adds the static file handler.</span></span> <span data-ttu-id="ac90d-139">Wie bereits erwähnt, ist die ASP.NET-Laufzeit modular aufgebaut, und Sie müssen explizit entscheiden Sie sich für statische Dateien dienen.</span><span class="sxs-lookup"><span data-stu-id="ac90d-139">As mentioned previously, the ASP.NET runtime is modular, and you must explicitly opt in to serve static files.</span></span> <span data-ttu-id="ac90d-140">Die `UseMvc` Erweiterungsmethode fügt routing.</span><span class="sxs-lookup"><span data-stu-id="ac90d-140">The `UseMvc` extension method adds routing.</span></span> <span data-ttu-id="ac90d-141">Weitere Informationen finden Sie unter [Anwendungsstart](../fundamentals/startup.md) und [Routing](../fundamentals/routing.md).</span><span class="sxs-lookup"><span data-stu-id="ac90d-141">For more information, see [Application Startup](../fundamentals/startup.md) and [Routing](../fundamentals/routing.md).</span></span>

## <a name="add-a-controller-and-view"></a><span data-ttu-id="ac90d-142">Fügen Sie einen Controller und Ansicht hinzu.</span><span class="sxs-lookup"><span data-stu-id="ac90d-142">Add a controller and view</span></span>

<span data-ttu-id="ac90d-143">In diesem Abschnitt fügen Sie einem minimalen Controller und Ansicht dienen als Platzhalter für den ASP.NET MVC-Controller und Ansichten werden im nächsten Abschnitt migrieren.</span><span class="sxs-lookup"><span data-stu-id="ac90d-143">In this section, you'll add a minimal controller and view to serve as placeholders for the ASP.NET MVC controller and views you'll migrate in the next section.</span></span>

* <span data-ttu-id="ac90d-144">Hinzufügen einer *Controller* Ordner.</span><span class="sxs-lookup"><span data-stu-id="ac90d-144">Add a *Controllers* folder.</span></span>

* <span data-ttu-id="ac90d-145">Hinzufügen einer **MVC-Controller-Klasse** mit dem Namen *HomeController.cs* auf die *Controller* Ordner.</span><span class="sxs-lookup"><span data-stu-id="ac90d-145">Add an **MVC controller class** with the name *HomeController.cs* to the *Controllers* folder.</span></span>

![Dialogfeld „Neues Element hinzufügen“](mvc/_static/add_mvc_ctl.png)

* <span data-ttu-id="ac90d-147">Hinzufügen einer *Ansichten* Ordner.</span><span class="sxs-lookup"><span data-stu-id="ac90d-147">Add a *Views* folder.</span></span>

* <span data-ttu-id="ac90d-148">Hinzufügen einer *Ansichten/Start* Ordner.</span><span class="sxs-lookup"><span data-stu-id="ac90d-148">Add a *Views/Home* folder.</span></span>

* <span data-ttu-id="ac90d-149">Hinzufügen einer *Index.cshtml* MVC-Ansichtsseite fьr die *Ansichten/Start* Ordner.</span><span class="sxs-lookup"><span data-stu-id="ac90d-149">Add an *Index.cshtml* MVC view page to the *Views/Home* folder.</span></span>

![Dialogfeld „Neues Element hinzufügen“](mvc/_static/view.png)

<span data-ttu-id="ac90d-151">Die Projektstruktur wird unten gezeigt:</span><span class="sxs-lookup"><span data-stu-id="ac90d-151">The project structure is shown below:</span></span>

![Projektmappen-Explorer mit Dateien und Ordner von WebApp1](mvc/_static/project-structure-controller-view.png)

<span data-ttu-id="ac90d-153">Ersetzen Sie den Inhalt von der *Views/Home/Index.cshtml* Datei durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="ac90d-153">Replace the contents of the *Views/Home/Index.cshtml* file with the following:</span></span>

```html
<h1>Hello world!</h1>
```

<span data-ttu-id="ac90d-154">Führen Sie die App aus.</span><span class="sxs-lookup"><span data-stu-id="ac90d-154">Run the app.</span></span>

![Öffnen Sie in Microsoft Edge-Webanwendung](mvc/_static/hello-world.png)

<span data-ttu-id="ac90d-156">Finden Sie unter [Controller](../mvc/controllers/index.md) und [Ansichten](../mvc/views/index.md) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="ac90d-156">See [Controllers](../mvc/controllers/index.md) and [Views](../mvc/views/index.md) for more information.</span></span>

<span data-ttu-id="ac90d-157">Nun, da wir ein minimales funktionierenden ASP.NET Core-Projekt haben, beginnen wir Migrieren von Funktionen von ASP.NET MVC-Projekt.</span><span class="sxs-lookup"><span data-stu-id="ac90d-157">Now that we have a minimal working ASP.NET Core project, we can start migrating functionality from the ASP.NET MVC project.</span></span> <span data-ttu-id="ac90d-158">Wir müssen die folgenden verschieben:</span><span class="sxs-lookup"><span data-stu-id="ac90d-158">We will need to move the following:</span></span>

* <span data-ttu-id="ac90d-159">die clientseitige Inhalt (CSS, Schriftarten und Skripts)</span><span class="sxs-lookup"><span data-stu-id="ac90d-159">client-side content (CSS, fonts, and scripts)</span></span>

* <span data-ttu-id="ac90d-160">Controller</span><span class="sxs-lookup"><span data-stu-id="ac90d-160">controllers</span></span>

* <span data-ttu-id="ac90d-161">Ansichten</span><span class="sxs-lookup"><span data-stu-id="ac90d-161">views</span></span>

* <span data-ttu-id="ac90d-162">Modelle</span><span class="sxs-lookup"><span data-stu-id="ac90d-162">models</span></span>

* <span data-ttu-id="ac90d-163">Bundling</span><span class="sxs-lookup"><span data-stu-id="ac90d-163">bundling</span></span>

* <span data-ttu-id="ac90d-164">Filter</span><span class="sxs-lookup"><span data-stu-id="ac90d-164">filters</span></span>

* <span data-ttu-id="ac90d-165">Melden Sie sich in/out Identität (Dies wird in den nächsten Lernprogrammen erfolgen.)</span><span class="sxs-lookup"><span data-stu-id="ac90d-165">Log in/out, identity (This will be done in the next tutorial.)</span></span>

## <a name="controllers-and-views"></a><span data-ttu-id="ac90d-166">Controller und Ansichten</span><span class="sxs-lookup"><span data-stu-id="ac90d-166">Controllers and views</span></span>

* <span data-ttu-id="ac90d-167">Kopieren Sie jede der Methoden von ASP.NET MVC `HomeController` mit dem neuen `HomeController`.</span><span class="sxs-lookup"><span data-stu-id="ac90d-167">Copy each of the methods from the ASP.NET MVC `HomeController` to the new `HomeController`.</span></span> <span data-ttu-id="ac90d-168">Beachten Sie, dass in ASP.NET-MVC integrierten Vorlage Controller Aktion Methodenrückgabetyp [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); in ASP.NET Core MVC die Aktionsmethoden return `IActionResult` stattdessen.</span><span class="sxs-lookup"><span data-stu-id="ac90d-168">Note that in ASP.NET MVC, the built-in template's controller action method return type is [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); in ASP.NET Core MVC, the action methods return `IActionResult` instead.</span></span> <span data-ttu-id="ac90d-169">`ActionResult`implementiert `IActionResult`, daher keine Notwendigkeit besteht, den Rückgabetyp der Aktionsmethoden zu ändern.</span><span class="sxs-lookup"><span data-stu-id="ac90d-169">`ActionResult` implements `IActionResult`, so there is no need to change the return type of your action methods.</span></span>

* <span data-ttu-id="ac90d-170">Kopieren der *About.cshtml*, *Contact.cshtml*, und *Index.cshtml* Razor-Ansicht-Dateien aus dem ASP.NET MVC-Projekt dem ASP.NET Core-Projekt.</span><span class="sxs-lookup"><span data-stu-id="ac90d-170">Copy the *About.cshtml*, *Contact.cshtml*, and *Index.cshtml* Razor view files from the ASP.NET MVC project to the ASP.NET Core project.</span></span>

* <span data-ttu-id="ac90d-171">Führen Sie die ASP.NET Core-app, und Testen Sie jede Methode zu.</span><span class="sxs-lookup"><span data-stu-id="ac90d-171">Run the ASP.NET Core app and test each method.</span></span> <span data-ttu-id="ac90d-172">Wir noch nicht die Layoutdatei oder Stile migriert noch, damit die gerenderten Ansichten nur der Inhalt in den Dateien anzeigen enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="ac90d-172">We haven't migrated the layout file or styles yet, so the rendered views will only contain the content in the view files.</span></span> <span data-ttu-id="ac90d-173">Sie erhalten die Layout generierte dateilinks für keinen der `About` und `Contact` anzeigt, daher Sie diese über den Browser aufrufen müssen (ersetzen Sie **4492** mit der Portnummer, die in Ihrem Projekt verwendet).</span><span class="sxs-lookup"><span data-stu-id="ac90d-173">You won't have the layout file generated links for the `About` and `Contact` views, so you'll have to invoke them from the browser (replace **4492** with the port number used in your project).</span></span>

  * `http://localhost:4492/home/about`

  * `http://localhost:4492/home/contact`

![Seite "Kontakte"](mvc/_static/contact-page.png)

<span data-ttu-id="ac90d-175">Beachten Sie die fehlende formatieren und Menüelementen aus.</span><span class="sxs-lookup"><span data-stu-id="ac90d-175">Note the lack of styling and menu items.</span></span> <span data-ttu-id="ac90d-176">Wir beheben, die im nächsten Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="ac90d-176">We'll fix that in the next section.</span></span>

## <a name="static-content"></a><span data-ttu-id="ac90d-177">Statischer Inhalt</span><span class="sxs-lookup"><span data-stu-id="ac90d-177">Static content</span></span>

<span data-ttu-id="ac90d-178">In früheren Versionen von ASP.NET MVC statischen Inhalte wurde aus dem Stammverzeichnis des Webprojekts gehostet und wurde mit serverseitigen Dateien zusammensetzt.</span><span class="sxs-lookup"><span data-stu-id="ac90d-178">In previous versions of  ASP.NET MVC, static content was hosted from the root of the web project and was intermixed with server-side files.</span></span> <span data-ttu-id="ac90d-179">In ASP.NET Core, statischer Inhalt gehostet wird, der *"Wwwroot"* Ordner.</span><span class="sxs-lookup"><span data-stu-id="ac90d-179">In ASP.NET Core, static content is hosted in the *wwwroot* folder.</span></span> <span data-ttu-id="ac90d-180">Sie möchten den statischen Inhalt von alten einer ASP.NET MVC-Anwendung zu kopieren der *"Wwwroot"* Projektordners ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ac90d-180">You'll want to copy the static content from your old ASP.NET MVC app to the *wwwroot* folder in your ASP.NET Core project.</span></span> <span data-ttu-id="ac90d-181">In diesem Beispiel Konvertierung:</span><span class="sxs-lookup"><span data-stu-id="ac90d-181">In this sample conversion:</span></span>

* <span data-ttu-id="ac90d-182">Kopieren der *favicon.ico* -Datei aus dem alten MVC-Projekt in der *"Wwwroot"* Ordner des Projekts ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ac90d-182">Copy the *favicon.ico* file from the old MVC project to the *wwwroot* folder in the ASP.NET Core project.</span></span>

<span data-ttu-id="ac90d-183">Die alte ASP.NET MVC-Projekt verwendet [Bootstrap](http://getbootstrap.com/) für seine Erstellen von Formaten und speichert die Bootstrap-in Dateien den *Content* und *Skripts* Ordner.</span><span class="sxs-lookup"><span data-stu-id="ac90d-183">The old ASP.NET MVC project uses [Bootstrap](http://getbootstrap.com/) for its styling and stores the Bootstrap files in the *Content* and *Scripts* folders.</span></span> <span data-ttu-id="ac90d-184">Die Vorlage, die mit das alte ASP.NET MVC-Projekt generiert wird, verweist auf Bootstrap in der Layoutdatei (*Views/Shared/_Layout.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="ac90d-184">The template, which generated the old ASP.NET MVC project, references Bootstrap in the layout file (*Views/Shared/_Layout.cshtml*).</span></span> <span data-ttu-id="ac90d-185">Sie kopieren konnte die *bootstrap.js* und *bootstrap.css* Dateien von ASP.NET MVC-Projekt auf die *"Wwwroot"* Ordner in das neue Projekt, aber dieser Ansatz nicht verwenden die Verbesserte Mechanismus zum Verwalten von Client-Side-Abhängigkeiten in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ac90d-185">You could copy the *bootstrap.js* and *bootstrap.css* files from the ASP.NET MVC project to the *wwwroot* folder in the new project, but that approach doesn't use the improved mechanism for managing client-side dependencies in ASP.NET Core.</span></span>

<span data-ttu-id="ac90d-186">In das neue Projekt, fügen wir Unterstützung für Bootstrap (und andere clientseitige Bibliotheken) mit [Bower](https://bower.io/):</span><span class="sxs-lookup"><span data-stu-id="ac90d-186">In the new project, we'll add support for Bootstrap (and other client-side libraries) using [Bower](https://bower.io/):</span></span>

* <span data-ttu-id="ac90d-187">Hinzufügen einer [Bower](https://bower.io/) Konfigurationsdatei mit dem Namen *"bower.JSON"* in das Projektstammverzeichnis (mit der rechten Maustaste auf das Projekt, und klicken Sie dann **hinzufügen > Neues Element > Bower Konfigurationsdatei**).</span><span class="sxs-lookup"><span data-stu-id="ac90d-187">Add a [Bower](https://bower.io/) configuration file named *bower.json* to the project root (Right-click on the project, and then **Add > New Item > Bower Configuration File**).</span></span> <span data-ttu-id="ac90d-188">Hinzufügen [Bootstrap](http://getbootstrap.com/) und [jQuery](https://jquery.com/) in der Datei (siehe die folgenden hervorgehobenen Zeilen).</span><span class="sxs-lookup"><span data-stu-id="ac90d-188">Add [Bootstrap](http://getbootstrap.com/) and [jQuery](https://jquery.com/) to the file (see the highlighted lines below).</span></span>

  [!code-json[Main](mvc/sample/bower.json?highlight=5-6)]

<span data-ttu-id="ac90d-189">Beim Speichern der Datei herunterladen Bower automatisch die Abhängigkeiten zu den *"Wwwroot" / Lib* Ordner.</span><span class="sxs-lookup"><span data-stu-id="ac90d-189">Upon saving the file, Bower will automatically download the dependencies to the *wwwroot/lib* folder.</span></span> <span data-ttu-id="ac90d-190">Sie können die **Projektmappen-Explorer Durchsuchen** Feld, um den Pfad der Anlagen finden:</span><span class="sxs-lookup"><span data-stu-id="ac90d-190">You can use the **Search Solution Explorer** box to find the path of the assets:</span></span>

![jQuery-Objekte, die in den Suchergebnissen der Projektmappen-Explorer angezeigt](mvc/_static/search.png)

<span data-ttu-id="ac90d-192">Finden Sie unter [Client-Side-Pakete verwalten, mit Bower](../client-side/bower.md) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="ac90d-192">See [Manage Client-Side Packages with Bower](../client-side/bower.md) for more information.</span></span>

<a name="migrate-layout-file"></a>

## <a name="migrate-the-layout-file"></a><span data-ttu-id="ac90d-193">Migrieren Sie die Layoutdatei</span><span class="sxs-lookup"><span data-stu-id="ac90d-193">Migrate the layout file</span></span>

* <span data-ttu-id="ac90d-194">Kopieren der *Ansichten "_viewstart.cshtml"* Datei aus des alten ASP.NET MVC-Projekts *Ansichten* Ordner, in des ASP.NET Core Projekts *Ansichten* Ordner.</span><span class="sxs-lookup"><span data-stu-id="ac90d-194">Copy the *_ViewStart.cshtml* file from the old ASP.NET MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="ac90d-195">Die *Ansichten "_viewstart.cshtml"* Datei Core ASP.NET-MVC nicht geändert.</span><span class="sxs-lookup"><span data-stu-id="ac90d-195">The *_ViewStart.cshtml* file has not changed in ASP.NET Core MVC.</span></span>

* <span data-ttu-id="ac90d-196">Erstellen einer *Ansichten/freigegeben* Ordner.</span><span class="sxs-lookup"><span data-stu-id="ac90d-196">Create a *Views/Shared* folder.</span></span>

* <span data-ttu-id="ac90d-197">*Optional:* Kopie *_ViewImports.cshtml* aus der *FullAspNetCore* MVC-Projekt *Ansichten* Ordner, in des ASP.NET Core Projekts *Ansichten* Ordner.</span><span class="sxs-lookup"><span data-stu-id="ac90d-197">*Optional:* Copy *_ViewImports.cshtml* from the *FullAspNetCore* MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="ac90d-198">Entfernen Sie alle Namespacedeklaration in der *_ViewImports.cshtml* Datei.</span><span class="sxs-lookup"><span data-stu-id="ac90d-198">Remove any namespace declaration in the *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="ac90d-199">Die *_ViewImports.cshtml* Datei bietet Namespaces für alle Dateien anzeigen und bringt in [Tag Hilfsprogramme](../mvc/views/tag-helpers/index.md).</span><span class="sxs-lookup"><span data-stu-id="ac90d-199">The *_ViewImports.cshtml* file provides namespaces for all the view files and brings in [Tag Helpers](../mvc/views/tag-helpers/index.md).</span></span> <span data-ttu-id="ac90d-200">Tag-Hilfsmethoden werden in das Neue Layoutdatei verwendet.</span><span class="sxs-lookup"><span data-stu-id="ac90d-200">Tag Helpers are used in the new layout file.</span></span> <span data-ttu-id="ac90d-201">Die *_ViewImports.cshtml* Datei ist neu in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ac90d-201">The *_ViewImports.cshtml* file is new for ASP.NET Core.</span></span>

* <span data-ttu-id="ac90d-202">Kopieren der *_Layout.cshtml* Datei aus des alten ASP.NET MVC-Projekts *Ansichten/freigegeben* Ordner, in des ASP.NET Core Projekts *Ansichten/freigegeben* Ordner.</span><span class="sxs-lookup"><span data-stu-id="ac90d-202">Copy the *_Layout.cshtml* file from the old ASP.NET MVC project's *Views/Shared* folder into the ASP.NET Core project's *Views/Shared* folder.</span></span>

<span data-ttu-id="ac90d-203">Open *_Layout.cshtml* Datei, und nehmen Sie die folgenden Änderungen (der fertigen Code wird unten dargestellt):</span><span class="sxs-lookup"><span data-stu-id="ac90d-203">Open *_Layout.cshtml* file and make the following changes (the completed code is shown below):</span></span>

   * <span data-ttu-id="ac90d-204">Ersetzen Sie `@Styles.Render("~/Content/css")` mit einem `<link>` Element laden *bootstrap.css* (siehe unten).</span><span class="sxs-lookup"><span data-stu-id="ac90d-204">Replace `@Styles.Render("~/Content/css")` with a `<link>` element to load *bootstrap.css* (see below).</span></span>

   * <span data-ttu-id="ac90d-205">Entfernen Sie `@Scripts.Render("~/bundles/modernizr")`.</span><span class="sxs-lookup"><span data-stu-id="ac90d-205">Remove `@Scripts.Render("~/bundles/modernizr")`.</span></span>

   * <span data-ttu-id="ac90d-206">Kommentieren Sie Sie aus der `@Html.Partial("_LoginPartial")` Zeile (die Zeile mit umschließen `@*...*@`).</span><span class="sxs-lookup"><span data-stu-id="ac90d-206">Comment out the `@Html.Partial("_LoginPartial")` line (surround the line with `@*...*@`).</span></span> <span data-ttu-id="ac90d-207">Wir müssen in einem späteren Lernprogramm darauf zurück.</span><span class="sxs-lookup"><span data-stu-id="ac90d-207">We'll return to it in a future tutorial.</span></span>

   * <span data-ttu-id="ac90d-208">Ersetzen Sie `@Scripts.Render("~/bundles/jquery")` mit einem `<script>` Element (siehe unten).</span><span class="sxs-lookup"><span data-stu-id="ac90d-208">Replace `@Scripts.Render("~/bundles/jquery")` with a `<script>` element (see below).</span></span>

   * <span data-ttu-id="ac90d-209">Ersetzen Sie `@Scripts.Render("~/bundles/bootstrap")` mit einem `<script>` Element (siehe unten)...</span><span class="sxs-lookup"><span data-stu-id="ac90d-209">Replace `@Scripts.Render("~/bundles/bootstrap")` with a `<script>` element (see below)..</span></span>

<span data-ttu-id="ac90d-210">Die Ersetzung verknüpfen CSS:</span><span class="sxs-lookup"><span data-stu-id="ac90d-210">The replacement CSS link:</span></span>

```html
<link rel="stylesheet" href="~/lib/bootstrap/dist/css/bootstrap.css" />
```

<span data-ttu-id="ac90d-211">Die Ersetzung Skripttags:</span><span class="sxs-lookup"><span data-stu-id="ac90d-211">The replacement script tags:</span></span>

```html
<script src="~/lib/jquery/dist/jquery.js"></script>
<script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
```

<span data-ttu-id="ac90d-212">Die aktualisierte *_Layout.cshtml* Datei wird unten gezeigt:</span><span class="sxs-lookup"><span data-stu-id="ac90d-212">The updated *_Layout.cshtml* file is shown below:</span></span>

[!code-html[Main](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7,27,39-40)]

<span data-ttu-id="ac90d-213">Zeigen Sie die Website im Browser.</span><span class="sxs-lookup"><span data-stu-id="ac90d-213">View the site in the browser.</span></span> <span data-ttu-id="ac90d-214">Es sollte jetzt ordnungsgemäß mit den erwarteten Formatvorlagen direktes Laden.</span><span class="sxs-lookup"><span data-stu-id="ac90d-214">It should now load correctly, with the expected styles in place.</span></span>

* <span data-ttu-id="ac90d-215">*Optional:* möglicherweise möchten Sie versuchen, mithilfe der neuen Layoutdatei.</span><span class="sxs-lookup"><span data-stu-id="ac90d-215">*Optional:* You might want to try using the new layout file.</span></span> <span data-ttu-id="ac90d-216">Für dieses Projekt kopieren Sie die Layoutdatei aus der *FullAspNetCore* Projekt.</span><span class="sxs-lookup"><span data-stu-id="ac90d-216">For this project you can copy the layout file from the *FullAspNetCore* project.</span></span> <span data-ttu-id="ac90d-217">Verwendet die Neue Layoutdatei [Tag Hilfsprogramme](../mvc/views/tag-helpers/index.md) und hat den weiteren Verbesserungen.</span><span class="sxs-lookup"><span data-stu-id="ac90d-217">The new layout file uses [Tag Helpers](../mvc/views/tag-helpers/index.md) and has other improvements.</span></span>

## <a name="configure-bundling--minification"></a><span data-ttu-id="ac90d-218">Konfigurieren der Bündelung & Minimierung</span><span class="sxs-lookup"><span data-stu-id="ac90d-218">Configure Bundling & Minification</span></span>

<span data-ttu-id="ac90d-219">Informationen zum Konfigurieren von Bündelung und Minimierung finden Sie unter [Bündelung und Minimierung](../client-side/bundling-and-minification.md).</span><span class="sxs-lookup"><span data-stu-id="ac90d-219">For information about how to configure bundling and minification, see [Bundling and Minification](../client-side/bundling-and-minification.md).</span></span>

## <a name="solving-http-500-errors"></a><span data-ttu-id="ac90d-220">Beheben von HTTP 500-Fehler</span><span class="sxs-lookup"><span data-stu-id="ac90d-220">Solving HTTP 500 errors</span></span>

<span data-ttu-id="ac90d-221">Es gibt viele Probleme, die eine HTTP 500-Fehlermeldung verursachen können, die keine Informationen für die Quelle des Problems enthalten.</span><span class="sxs-lookup"><span data-stu-id="ac90d-221">There are many problems that can cause a HTTP 500 error message that contain no information on the source of the problem.</span></span> <span data-ttu-id="ac90d-222">Beispielsweise, wenn die *Views/_ViewImports.cshtml* -Datei enthält einen Namespace, der in Ihrem Projekt nicht vorhanden ist, erhalten Sie einen HTTP 500-Fehler.</span><span class="sxs-lookup"><span data-stu-id="ac90d-222">For example, if the *Views/_ViewImports.cshtml* file contains a namespace that doesn't exist in your project, you'll get a HTTP 500 error.</span></span> <span data-ttu-id="ac90d-223">Um eine ausführliche Fehlermeldung zu erhalten, fügen Sie den folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="ac90d-223">To get a detailed error message, add the following code:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
{
    if (env.IsDevelopment())
    {
         app.UseDeveloperExceptionPage();
    }

    app.UseStaticFiles();

    app.UseMvc(routes =>
    {
        routes.MapRoute(
            name: "default",
            template: "{controller=Home}/{action=Index}/{id?}");
    });
}
```

<span data-ttu-id="ac90d-224">Finden Sie unter **Developer Ausnahme auf der Dienstkontoseite** in [Fehlerbehandlung](../fundamentals/error-handling.md) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="ac90d-224">See **Using the Developer Exception Page** in [Error Handling](../fundamentals/error-handling.md) for more information.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ac90d-225">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="ac90d-225">Additional Resources</span></span>

* [<span data-ttu-id="ac90d-226">Clientbasierte Entwicklung</span><span class="sxs-lookup"><span data-stu-id="ac90d-226">Client-Side Development</span></span>](../client-side/index.md)

* [<span data-ttu-id="ac90d-227">Taghilfsprogramme</span><span class="sxs-lookup"><span data-stu-id="ac90d-227">Tag Helpers</span></span>](../mvc/views/tag-helpers/index.md)
