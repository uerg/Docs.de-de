---
title: Migrieren von ASP.NET MVC zu ASP.NET Core MVC
author: ardalis
description: 
keywords: ASP.NET Core, MVC, migrieren
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: article
ms.assetid: 3155cc9e-d0c9-424b-886c-35c0ec6f9f4e
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/mvc
ms.openlocfilehash: 2bd689626e867e0ea82fbebdf92447a6029aa35b
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2017
---
# <a name="migrating-from-aspnet-mvc-to-aspnet-core-mvc"></a><span data-ttu-id="8c094-103">Migrieren von ASP.NET MVC zu ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="8c094-103">Migrating From ASP.NET MVC to ASP.NET Core MVC</span></span>

<span data-ttu-id="8c094-104">Durch [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), und [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="8c094-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="8c094-105">In diesem Artikel wird gezeigt, wie für den Einstieg in die Migration von einer ASP.NET MVC-Projekts zu [Core ASP.NET-MVC](../mvc/overview.md).</span><span class="sxs-lookup"><span data-stu-id="8c094-105">This article shows how to get started migrating an ASP.NET MVC project to [ASP.NET Core MVC](../mvc/overview.md).</span></span> <span data-ttu-id="8c094-106">In der Prozess wird hervorgehoben viele der Aufgaben, die von ASP.NET MVC geändert haben.</span><span class="sxs-lookup"><span data-stu-id="8c094-106">In the process, it highlights many of the things that have changed from ASP.NET MVC.</span></span> <span data-ttu-id="8c094-107">Migrieren von ASP.NET MVC umfasst mehrere Schritte, und dieser Artikel behandelt die Anfangssetup, grundlegende Controller und Ansichten, statische Inhalte und Client-Side-Abhängigkeiten.</span><span class="sxs-lookup"><span data-stu-id="8c094-107">Migrating from ASP.NET MVC is a multiple step process and this article covers the initial setup, basic controllers and views, static content, and client-side dependencies.</span></span> <span data-ttu-id="8c094-108">Zusätzliche Artikel behandelt Migrieren der Konfiguration und ID-Code in ASP.NET MVC-Projekte, die viele gefunden.</span><span class="sxs-lookup"><span data-stu-id="8c094-108">Additional articles cover migrating configuration and identity code found in many ASP.NET MVC projects.</span></span>

> [!NOTE]
> <span data-ttu-id="8c094-109">Die Versionsnummern in die Beispiele sind möglicherweise keine aktuellen.</span><span class="sxs-lookup"><span data-stu-id="8c094-109">The version numbers in the samples might not be current.</span></span> <span data-ttu-id="8c094-110">Sie müssen möglicherweise Ihre Projekte entsprechend aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="8c094-110">You may need to update your projects accordingly.</span></span>

## <a name="create-the-starter-aspnet-mvc-project"></a><span data-ttu-id="8c094-111">Erstellen von Starter ASP.NET MVC-Projekt</span><span class="sxs-lookup"><span data-stu-id="8c094-111">Create the starter ASP.NET MVC project</span></span>

<span data-ttu-id="8c094-112">Um das Upgrade zu demonstrieren, beginnen wir mit eine ASP.NET MVC-app erstellen.</span><span class="sxs-lookup"><span data-stu-id="8c094-112">To demonstrate the upgrade, we'll start by creating a ASP.NET MVC app.</span></span> <span data-ttu-id="8c094-113">Erstellen sie mit dem Namen *WebApp1* daher der Namespace des Projekts ASP.NET Core überein werden wir im nächsten Schritt erstellen.</span><span class="sxs-lookup"><span data-stu-id="8c094-113">Create it with the name *WebApp1* so the namespace will match the ASP.NET Core project we create in the next step.</span></span>

![Visual Studio-Projekt ein neues Dialogfeld "](mvc/_static/new-project.png)

![Dialogfeld "Neues Webanwendung": MVC-Projektvorlage in ASP.NET Vorlagen Bereich ausgewählt](mvc/_static/new-project-select-mvc-template.png)

<span data-ttu-id="8c094-116">*Optional:* ändern Sie den Namen der Projektmappe aus *WebApp1* auf *Mvc5*.</span><span class="sxs-lookup"><span data-stu-id="8c094-116">*Optional:* Change the name of the Solution from *WebApp1* to *Mvc5*.</span></span> <span data-ttu-id="8c094-117">Visual Studio zeigt den Namen der neuen Projektmappe (*Mvc5*), dem können sie einfacher, teilen Sie das Projekt über das nächste Projekt.</span><span class="sxs-lookup"><span data-stu-id="8c094-117">Visual Studio will display the new solution name (*Mvc5*), which will make it easier to tell this project from the next project.</span></span>

## <a name="create-the-aspnet-core-project"></a><span data-ttu-id="8c094-118">Erstellen des Projekts ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8c094-118">Create the ASP.NET Core project</span></span>

<span data-ttu-id="8c094-119">Erstellen Sie ein neues *leere* ASP.NET Core Web-app mit dem gleichen Namen wie das vorherige Projekt (*WebApp1*), damit die Namespaces in beiden Projekten übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="8c094-119">Create a new *empty* ASP.NET Core web app with the same name as the previous project (*WebApp1*) so the namespaces in the two projects match.</span></span> <span data-ttu-id="8c094-120">Mit dem gleichen Namespace erleichtert es, Code zwischen den zwei Projekten kopieren.</span><span class="sxs-lookup"><span data-stu-id="8c094-120">Having the same namespace makes it easier to copy code between the two projects.</span></span> <span data-ttu-id="8c094-121">Sie müssen zum Erstellen des Projekts in einem anderen Verzeichnis als das vorherige Projekt, das den gleichen Namen verwenden.</span><span class="sxs-lookup"><span data-stu-id="8c094-121">You'll have to create this project in a different directory than the previous project to use the same name.</span></span>

![Dialogfeld "Neues Projekt"](mvc/_static/new_core.png)

![Dialogfeld "neues ASP.NET Web-Anwendung": leere Projektvorlage, die im Bereich der Vorlagen für ASP.NET Core ausgewählt](mvc/_static/new-project-select-empty-aspnet5-template.png)

* <span data-ttu-id="8c094-124">*Optional:* Erstellen eines neuen ASP.NET Core app mithilfe der *Webanwendung* -Projektvorlage.</span><span class="sxs-lookup"><span data-stu-id="8c094-124">*Optional:* Create a new ASP.NET Core app using the *Web Application* project template.</span></span> <span data-ttu-id="8c094-125">Nennen Sie das Projekt *WebApp1*, und wählen Sie eine Authentifizierungsoption in **einzelne Benutzerkonten**.</span><span class="sxs-lookup"><span data-stu-id="8c094-125">Name the project *WebApp1*, and select an authentication option of **Individual User Accounts**.</span></span> <span data-ttu-id="8c094-126">Benennen Sie diese app *FullAspNetCore*.</span><span class="sxs-lookup"><span data-stu-id="8c094-126">Rename this app to *FullAspNetCore*.</span></span> <span data-ttu-id="8c094-127">Erstellen bei der Konvertierung dieses Projekts werden Sie Zeit sparen.</span><span class="sxs-lookup"><span data-stu-id="8c094-127">Creating this project will save you time in the conversion.</span></span> <span data-ttu-id="8c094-128">Betrachten Sie die Vorlage generiertem Code auf das Endergebnis finden Sie unter und im Code in das Konvertierungsprojekt zu kopieren.</span><span class="sxs-lookup"><span data-stu-id="8c094-128">You can look at the template-generated code to see the end result or to copy code to the conversion project.</span></span> <span data-ttu-id="8c094-129">Es ist auch hilfreich, wenn Sie auf einen Konvertierungsschritt für den Vergleich mit der Vorlage generiert Projekt festsitzt.</span><span class="sxs-lookup"><span data-stu-id="8c094-129">It's also helpful when you get stuck on a conversion step to compare with the template-generated project.</span></span>

## <a name="configure-the-site-to-use-mvc"></a><span data-ttu-id="8c094-130">Konfigurieren Sie den Standort für MVC verwenden</span><span class="sxs-lookup"><span data-stu-id="8c094-130">Configure the site to use MVC</span></span>

* <span data-ttu-id="8c094-131">Installieren der `Microsoft.AspNetCore.Mvc` und `Microsoft.AspNetCore.StaticFiles` NuGet-Pakete.</span><span class="sxs-lookup"><span data-stu-id="8c094-131">Install the `Microsoft.AspNetCore.Mvc` and `Microsoft.AspNetCore.StaticFiles` NuGet packages.</span></span>

  <span data-ttu-id="8c094-132">`Microsoft.AspNetCore.Mvc`ist das ASP.NET-MVC-Framework Core.</span><span class="sxs-lookup"><span data-stu-id="8c094-132">`Microsoft.AspNetCore.Mvc` is the ASP.NET Core MVC framework.</span></span> <span data-ttu-id="8c094-133">`Microsoft.AspNetCore.StaticFiles`ist der Handler für statische Dateien.</span><span class="sxs-lookup"><span data-stu-id="8c094-133">`Microsoft.AspNetCore.StaticFiles` is the static file handler.</span></span> <span data-ttu-id="8c094-134">Die ASP.NET-Laufzeit ist modular aufgebaut, und Sie müssen explizit entscheiden Sie sich für statische Dateien dienen (finden Sie unter [arbeiten mit statischen Dateien](../fundamentals/static-files.md)).</span><span class="sxs-lookup"><span data-stu-id="8c094-134">The ASP.NET runtime is modular, and you must explicitly opt in to serve static files (see [Working with Static Files](../fundamentals/static-files.md)).</span></span>

* <span data-ttu-id="8c094-135">Öffnen der *csproj* Datei (mit der rechten Maustaste des Projekts im **Projektmappen-Explorer** , und wählen Sie **bearbeiten WebApp1.csproj**) und fügen eine `PrepareForPublish` Ziel:</span><span class="sxs-lookup"><span data-stu-id="8c094-135">Open the *.csproj* file (right-click the project in **Solution Explorer** and select **Edit WebApp1.csproj**) and add a `PrepareForPublish` target:</span></span>

  <span data-ttu-id="8c094-136">[!code-xml[Main](mvc/sample/WebApp1.csproj?range=21-23)]</span><span class="sxs-lookup"><span data-stu-id="8c094-136">[!code-xml[Main](mvc/sample/WebApp1.csproj?range=21-23)]</span></span>

  <span data-ttu-id="8c094-137">Die `PrepareForPublish` Ziel für den Erwerb von Client-Side-Bibliotheken über Bower erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="8c094-137">The `PrepareForPublish` target is needed for acquiring client-side libraries via Bower.</span></span> <span data-ttu-id="8c094-138">Die müssen weiter unten besprochen.</span><span class="sxs-lookup"><span data-stu-id="8c094-138">We'll talk about that later.</span></span>

* <span data-ttu-id="8c094-139">Öffnen der *Startup.cs* Datei und ändern Sie den Code entsprechend der folgenden:</span><span class="sxs-lookup"><span data-stu-id="8c094-139">Open the *Startup.cs* file and change the code to match the following:</span></span>

  <span data-ttu-id="8c094-140">[!code-csharp[Main](mvc/sample/Startup.cs?highlight=14,27-34)]</span><span class="sxs-lookup"><span data-stu-id="8c094-140">[!code-csharp[Main](mvc/sample/Startup.cs?highlight=14,27-34)]</span></span>

  <span data-ttu-id="8c094-141">Die `UseStaticFiles` Erweiterungsmethode fügt Handler für statische Dateien.</span><span class="sxs-lookup"><span data-stu-id="8c094-141">The `UseStaticFiles` extension method adds the static file handler.</span></span> <span data-ttu-id="8c094-142">Wie bereits erwähnt, ist die ASP.NET-Laufzeit modular aufgebaut, und Sie müssen explizit entscheiden Sie sich für statische Dateien dienen.</span><span class="sxs-lookup"><span data-stu-id="8c094-142">As mentioned previously, the ASP.NET runtime is modular, and you must explicitly opt in to serve static files.</span></span> <span data-ttu-id="8c094-143">Die `UseMvc` Erweiterungsmethode fügt routing.</span><span class="sxs-lookup"><span data-stu-id="8c094-143">The `UseMvc` extension method adds routing.</span></span> <span data-ttu-id="8c094-144">Weitere Informationen finden Sie unter [Anwendungsstart](../fundamentals/startup.md) und [Routing](../fundamentals/routing.md).</span><span class="sxs-lookup"><span data-stu-id="8c094-144">For more information, see [Application Startup](../fundamentals/startup.md) and [Routing](../fundamentals/routing.md).</span></span>

## <a name="add-a-controller-and-view"></a><span data-ttu-id="8c094-145">Fügen Sie einen Controller und Ansicht hinzu.</span><span class="sxs-lookup"><span data-stu-id="8c094-145">Add a controller and view</span></span>

<span data-ttu-id="8c094-146">In diesem Abschnitt fügen Sie einem minimalen Controller und Ansicht dienen als Platzhalter für den ASP.NET MVC-Controller und Ansichten werden im nächsten Abschnitt migrieren.</span><span class="sxs-lookup"><span data-stu-id="8c094-146">In this section, you'll add a minimal controller and view to serve as placeholders for the ASP.NET MVC controller and views you'll migrate in the next section.</span></span>

* <span data-ttu-id="8c094-147">Hinzufügen einer *Controller* Ordner.</span><span class="sxs-lookup"><span data-stu-id="8c094-147">Add a *Controllers* folder.</span></span>

* <span data-ttu-id="8c094-148">Hinzufügen einer **MVC-Controller-Klasse** mit dem Namen *HomeController.cs* auf die *Controller* Ordner.</span><span class="sxs-lookup"><span data-stu-id="8c094-148">Add an **MVC controller class** with the name *HomeController.cs* to the *Controllers* folder.</span></span>

![Dialogfeld Neues Element hinzufügen](mvc/_static/add_mvc_ctl.png)

* <span data-ttu-id="8c094-150">Hinzufügen einer *Ansichten* Ordner.</span><span class="sxs-lookup"><span data-stu-id="8c094-150">Add a *Views* folder.</span></span>

* <span data-ttu-id="8c094-151">Hinzufügen einer *Ansichten/Start* Ordner.</span><span class="sxs-lookup"><span data-stu-id="8c094-151">Add a *Views/Home* folder.</span></span>

* <span data-ttu-id="8c094-152">Hinzufügen einer *Index.cshtml* MVC-Ansichtsseite fьr die *Ansichten/Start* Ordner.</span><span class="sxs-lookup"><span data-stu-id="8c094-152">Add an *Index.cshtml* MVC view page to the *Views/Home* folder.</span></span>

![Dialogfeld Neues Element hinzufügen](mvc/_static/view.png)

<span data-ttu-id="8c094-154">Die Projektstruktur wird unten gezeigt:</span><span class="sxs-lookup"><span data-stu-id="8c094-154">The project structure is shown below:</span></span>

![Projektmappen-Explorer mit Dateien und Ordner von WebApp1](mvc/_static/project-structure-controller-view.png)

<span data-ttu-id="8c094-156">Ersetzen Sie den Inhalt von der *Views/Home/Index.cshtml* Datei durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="8c094-156">Replace the contents of the *Views/Home/Index.cshtml* file with the following:</span></span>

```html
<h1>Hello world!</h1>
```

<span data-ttu-id="8c094-157">Führen Sie die app an.</span><span class="sxs-lookup"><span data-stu-id="8c094-157">Run the app.</span></span>

![Öffnen Sie in Microsoft Edge-Webanwendung](mvc/_static/hello-world.png)

<span data-ttu-id="8c094-159">Finden Sie unter [Controller](../mvc/controllers/index.md) und [Ansichten](../mvc/views/index.md) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="8c094-159">See [Controllers](../mvc/controllers/index.md) and [Views](../mvc/views/index.md) for more information.</span></span>

<span data-ttu-id="8c094-160">Nun, da wir ein minimales funktionierenden ASP.NET Core-Projekt haben, beginnen wir Migrieren von Funktionen von ASP.NET MVC-Projekt.</span><span class="sxs-lookup"><span data-stu-id="8c094-160">Now that we have a minimal working ASP.NET Core project, we can start migrating functionality from the ASP.NET MVC project.</span></span> <span data-ttu-id="8c094-161">Wir müssen die folgenden verschieben:</span><span class="sxs-lookup"><span data-stu-id="8c094-161">We will need to move the following:</span></span>

* <span data-ttu-id="8c094-162">die clientseitige Inhalt (CSS, Schriftarten und Skripts)</span><span class="sxs-lookup"><span data-stu-id="8c094-162">client-side content (CSS, fonts, and scripts)</span></span>

* <span data-ttu-id="8c094-163">Controller</span><span class="sxs-lookup"><span data-stu-id="8c094-163">controllers</span></span>

* <span data-ttu-id="8c094-164">Ansichten</span><span class="sxs-lookup"><span data-stu-id="8c094-164">views</span></span>

* <span data-ttu-id="8c094-165">Modelle</span><span class="sxs-lookup"><span data-stu-id="8c094-165">models</span></span>

* <span data-ttu-id="8c094-166">Bundling</span><span class="sxs-lookup"><span data-stu-id="8c094-166">bundling</span></span>

* <span data-ttu-id="8c094-167">Filter</span><span class="sxs-lookup"><span data-stu-id="8c094-167">filters</span></span>

* <span data-ttu-id="8c094-168">Melden Sie sich in/out Identität (Dies wird in den nächsten Lernprogrammen erfolgen.)</span><span class="sxs-lookup"><span data-stu-id="8c094-168">Log in/out, identity (This will be done in the next tutorial.)</span></span>

## <a name="controllers-and-views"></a><span data-ttu-id="8c094-169">Controller und Ansichten</span><span class="sxs-lookup"><span data-stu-id="8c094-169">Controllers and views</span></span>

* <span data-ttu-id="8c094-170">Kopieren Sie jede der Methoden von ASP.NET MVC `HomeController` mit dem neuen `HomeController`.</span><span class="sxs-lookup"><span data-stu-id="8c094-170">Copy each of the methods from the ASP.NET MVC `HomeController` to the new `HomeController`.</span></span> <span data-ttu-id="8c094-171">Beachten Sie, dass in ASP.NET-MVC integrierten Vorlage Controller Aktion Methodenrückgabetyp [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); in ASP.NET Core MVC die Aktionsmethoden return `IActionResult` stattdessen.</span><span class="sxs-lookup"><span data-stu-id="8c094-171">Note that in ASP.NET MVC, the built-in template's controller action method return type is [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); in ASP.NET Core MVC, the action methods return `IActionResult` instead.</span></span> <span data-ttu-id="8c094-172">`ActionResult`implementiert `IActionResult`, daher keine Notwendigkeit besteht, den Rückgabetyp der Aktionsmethoden zu ändern.</span><span class="sxs-lookup"><span data-stu-id="8c094-172">`ActionResult` implements `IActionResult`, so there is no need to change the return type of your action methods.</span></span>

* <span data-ttu-id="8c094-173">Kopieren der *About.cshtml*, *Contact.cshtml*, und *Index.cshtml* Razor-Ansicht-Dateien aus dem ASP.NET MVC-Projekt dem ASP.NET Core-Projekt.</span><span class="sxs-lookup"><span data-stu-id="8c094-173">Copy the *About.cshtml*, *Contact.cshtml*, and *Index.cshtml* Razor view files from the ASP.NET MVC project to the ASP.NET Core project.</span></span>

* <span data-ttu-id="8c094-174">Führen Sie die ASP.NET Core-app, und Testen Sie jede Methode zu.</span><span class="sxs-lookup"><span data-stu-id="8c094-174">Run the ASP.NET Core app and test each method.</span></span> <span data-ttu-id="8c094-175">Wir noch nicht die Layoutdatei oder Stile migriert noch, damit die gerenderten Ansichten nur der Inhalt in den Dateien anzeigen enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="8c094-175">We haven't migrated the layout file or styles yet, so the rendered views will only contain the content in the view files.</span></span> <span data-ttu-id="8c094-176">Sie erhalten die Layout generierte dateilinks für keinen der `About` und `Contact` anzeigt, daher Sie diese über den Browser aufrufen müssen (ersetzen Sie **4492** mit der Portnummer, die in Ihrem Projekt verwendet).</span><span class="sxs-lookup"><span data-stu-id="8c094-176">You won't have the layout file generated links for the `About` and `Contact` views, so you'll have to invoke them from the browser (replace **4492** with the port number used in your project).</span></span>

  * `http://localhost:4492/home/about`

  * `http://localhost:4492/home/contact`

![Seite "Kontakte"](mvc/_static/contact-page.png)

<span data-ttu-id="8c094-178">Beachten Sie die fehlende formatieren und Menüelementen aus.</span><span class="sxs-lookup"><span data-stu-id="8c094-178">Note the lack of styling and menu items.</span></span> <span data-ttu-id="8c094-179">Wir beheben, die im nächsten Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="8c094-179">We'll fix that in the next section.</span></span>

## <a name="static-content"></a><span data-ttu-id="8c094-180">Statischer Inhalt</span><span class="sxs-lookup"><span data-stu-id="8c094-180">Static content</span></span>

<span data-ttu-id="8c094-181">In früheren Versionen von ASP.NET MVC statischen Inhalte wurde aus dem Stammverzeichnis des Webprojekts gehostet und wurde mit serverseitigen Dateien zusammensetzt.</span><span class="sxs-lookup"><span data-stu-id="8c094-181">In previous versions of  ASP.NET MVC, static content was hosted from the root of the web project and was intermixed with server-side files.</span></span> <span data-ttu-id="8c094-182">In ASP.NET Core, statischer Inhalt gehostet wird, der *"Wwwroot"* Ordner.</span><span class="sxs-lookup"><span data-stu-id="8c094-182">In ASP.NET Core, static content is hosted in the *wwwroot* folder.</span></span> <span data-ttu-id="8c094-183">Sie möchten den statischen Inhalt von alten einer ASP.NET MVC-Anwendung zu kopieren der *"Wwwroot"* Projektordners ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8c094-183">You'll want to copy the static content from your old ASP.NET MVC app to the *wwwroot* folder in your ASP.NET Core project.</span></span> <span data-ttu-id="8c094-184">In diesem Beispiel Konvertierung:</span><span class="sxs-lookup"><span data-stu-id="8c094-184">In this sample conversion:</span></span>

* <span data-ttu-id="8c094-185">Kopieren der *favicon.ico* -Datei aus dem alten MVC-Projekt in der *"Wwwroot"* Ordner des Projekts ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8c094-185">Copy the *favicon.ico* file from the old MVC project to the *wwwroot* folder in the ASP.NET Core project.</span></span>

<span data-ttu-id="8c094-186">Die alte ASP.NET MVC-Projekt verwendet [Bootstrap](http://getbootstrap.com/) für seine Erstellen von Formaten und speichert die Bootstrap-in Dateien den *Content* und *Skripts* Ordner.</span><span class="sxs-lookup"><span data-stu-id="8c094-186">The old ASP.NET MVC project uses [Bootstrap](http://getbootstrap.com/) for its styling and stores the Bootstrap files in the *Content* and *Scripts* folders.</span></span> <span data-ttu-id="8c094-187">Die Vorlage, die mit das alte ASP.NET MVC-Projekt generiert wird, verweist auf Bootstrap in der Layoutdatei (*Views/Shared/_Layout.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="8c094-187">The template, which generated the old ASP.NET MVC project, references Bootstrap in the layout file (*Views/Shared/_Layout.cshtml*).</span></span> <span data-ttu-id="8c094-188">Sie kopieren konnte die *bootstrap.js* und *bootstrap.css* Dateien von ASP.NET MVC-Projekt auf die *"Wwwroot"* Ordner in das neue Projekt, aber dieser Ansatz nicht verwenden die Verbesserte Mechanismus zum Verwalten von Client-Side-Abhängigkeiten in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8c094-188">You could copy the *bootstrap.js* and *bootstrap.css* files from the ASP.NET MVC project to the *wwwroot* folder in the new project, but that approach doesn't use the improved mechanism for managing client-side dependencies in ASP.NET Core.</span></span>

<span data-ttu-id="8c094-189">In das neue Projekt, fügen wir Unterstützung für Bootstrap (und andere clientseitige Bibliotheken) mit [Bower](https://bower.io/):</span><span class="sxs-lookup"><span data-stu-id="8c094-189">In the new project, we'll add support for Bootstrap (and other client-side libraries) using [Bower](https://bower.io/):</span></span>

* <span data-ttu-id="8c094-190">Hinzufügen einer [Bower](https://bower.io/) Konfigurationsdatei mit dem Namen *"bower.JSON"* in das Projektstammverzeichnis (mit der rechten Maustaste auf das Projekt, und klicken Sie dann **hinzufügen > Neues Element > Bower Konfigurationsdatei**).</span><span class="sxs-lookup"><span data-stu-id="8c094-190">Add a [Bower](https://bower.io/) configuration file named *bower.json* to the project root (Right-click on the project, and then **Add > New Item > Bower Configuration File**).</span></span> <span data-ttu-id="8c094-191">Hinzufügen [Bootstrap](http://getbootstrap.com/) und [jQuery](https://jquery.com/) in der Datei (siehe die folgenden hervorgehobenen Zeilen).</span><span class="sxs-lookup"><span data-stu-id="8c094-191">Add [Bootstrap](http://getbootstrap.com/) and [jQuery](https://jquery.com/) to the file (see the highlighted lines below).</span></span>

  <span data-ttu-id="8c094-192">[!code-json[Main](mvc/sample/bower.json?highlight=5-6)]</span><span class="sxs-lookup"><span data-stu-id="8c094-192">[!code-json[Main](mvc/sample/bower.json?highlight=5-6)]</span></span>

<span data-ttu-id="8c094-193">Beim Speichern der Datei herunterladen Bower automatisch die Abhängigkeiten zu den *"Wwwroot" / Lib* Ordner.</span><span class="sxs-lookup"><span data-stu-id="8c094-193">Upon saving the file, Bower will automatically download the dependencies to the *wwwroot/lib* folder.</span></span> <span data-ttu-id="8c094-194">Sie können die **Projektmappen-Explorer Durchsuchen** Feld, um den Pfad der Anlagen finden:</span><span class="sxs-lookup"><span data-stu-id="8c094-194">You can use the **Search Solution Explorer** box to find the path of the assets:</span></span>

![jQuery-Objekte, die in den Suchergebnissen der Projektmappen-Explorer angezeigt](mvc/_static/search.png)

<span data-ttu-id="8c094-196">Finden Sie unter [Client-Side-Pakete verwalten, mit Bower](../client-side/bower.md) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="8c094-196">See [Manage Client-Side Packages with Bower](../client-side/bower.md) for more information.</span></span>

<a name=migrate-layout-file></a>

## <a name="migrate-the-layout-file"></a><span data-ttu-id="8c094-197">Migrieren Sie die Layoutdatei</span><span class="sxs-lookup"><span data-stu-id="8c094-197">Migrate the layout file</span></span>

* <span data-ttu-id="8c094-198">Kopieren der *Ansichten "_viewstart.cshtml"* Datei aus des alten ASP.NET MVC-Projekts *Ansichten* Ordner, in des ASP.NET Core Projekts *Ansichten* Ordner.</span><span class="sxs-lookup"><span data-stu-id="8c094-198">Copy the *_ViewStart.cshtml* file from the old ASP.NET MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="8c094-199">Die *Ansichten "_viewstart.cshtml"* Datei Core ASP.NET-MVC nicht geändert.</span><span class="sxs-lookup"><span data-stu-id="8c094-199">The *_ViewStart.cshtml* file has not changed in ASP.NET Core MVC.</span></span>

* <span data-ttu-id="8c094-200">Erstellen einer *Ansichten/freigegeben* Ordner.</span><span class="sxs-lookup"><span data-stu-id="8c094-200">Create a *Views/Shared* folder.</span></span>

* <span data-ttu-id="8c094-201">*Optional:* Kopie *_ViewImports.cshtml* aus der *FullAspNetCore* MVC-Projekt *Ansichten* Ordner, in des ASP.NET Core Projekts *Ansichten* Ordner.</span><span class="sxs-lookup"><span data-stu-id="8c094-201">*Optional:* Copy *_ViewImports.cshtml* from the *FullAspNetCore* MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="8c094-202">Entfernen Sie alle Namespacedeklaration in der *_ViewImports.cshtml* Datei.</span><span class="sxs-lookup"><span data-stu-id="8c094-202">Remove any namespace declaration in the *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="8c094-203">Die *_ViewImports.cshtml* Datei bietet Namespaces für alle Dateien anzeigen und bringt in [Tag Hilfsprogramme](../mvc/views/tag-helpers/index.md).</span><span class="sxs-lookup"><span data-stu-id="8c094-203">The *_ViewImports.cshtml* file provides namespaces for all the view files and brings in [Tag Helpers](../mvc/views/tag-helpers/index.md).</span></span> <span data-ttu-id="8c094-204">Tag-Hilfsmethoden werden in das Neue Layoutdatei verwendet.</span><span class="sxs-lookup"><span data-stu-id="8c094-204">Tag Helpers are used in the new layout file.</span></span> <span data-ttu-id="8c094-205">Die *_ViewImports.cshtml* Datei ist neu in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8c094-205">The *_ViewImports.cshtml* file is new for ASP.NET Core.</span></span>

* <span data-ttu-id="8c094-206">Kopieren der *_Layout.cshtml* Datei aus des alten ASP.NET MVC-Projekts *Ansichten/freigegeben* Ordner, in des ASP.NET Core Projekts *Ansichten/freigegeben* Ordner.</span><span class="sxs-lookup"><span data-stu-id="8c094-206">Copy the *_Layout.cshtml* file from the old ASP.NET MVC project's *Views/Shared* folder into the ASP.NET Core project's *Views/Shared* folder.</span></span>

<span data-ttu-id="8c094-207">Open *_Layout.cshtml* Datei, und nehmen Sie die folgenden Änderungen (der fertigen Code wird unten dargestellt):</span><span class="sxs-lookup"><span data-stu-id="8c094-207">Open *_Layout.cshtml* file and make the following changes (the completed code is shown below):</span></span>

   * <span data-ttu-id="8c094-208">Ersetzen Sie `@Styles.Render("~/Content/css")` mit einem `<link>` Element laden *bootstrap.css* (siehe unten).</span><span class="sxs-lookup"><span data-stu-id="8c094-208">Replace `@Styles.Render("~/Content/css")` with a `<link>` element to load *bootstrap.css* (see below).</span></span>

   * <span data-ttu-id="8c094-209">Entfernen Sie `@Scripts.Render("~/bundles/modernizr")`.</span><span class="sxs-lookup"><span data-stu-id="8c094-209">Remove `@Scripts.Render("~/bundles/modernizr")`.</span></span>

   * <span data-ttu-id="8c094-210">Kommentieren Sie Sie aus der `@Html.Partial("_LoginPartial")` Zeile (die Zeile mit umschließen `@*...*@`).</span><span class="sxs-lookup"><span data-stu-id="8c094-210">Comment out the `@Html.Partial("_LoginPartial")` line (surround the line with `@*...*@`).</span></span> <span data-ttu-id="8c094-211">Wir müssen in einem späteren Lernprogramm darauf zurück.</span><span class="sxs-lookup"><span data-stu-id="8c094-211">We'll return to it in a future tutorial.</span></span>

   * <span data-ttu-id="8c094-212">Ersetzen Sie `@Scripts.Render("~/bundles/jquery")` mit einem `<script>` Element (siehe unten).</span><span class="sxs-lookup"><span data-stu-id="8c094-212">Replace `@Scripts.Render("~/bundles/jquery")` with a `<script>` element (see below).</span></span>

   * <span data-ttu-id="8c094-213">Ersetzen Sie `@Scripts.Render("~/bundles/bootstrap")` mit einem `<script>` Element (siehe unten)...</span><span class="sxs-lookup"><span data-stu-id="8c094-213">Replace `@Scripts.Render("~/bundles/bootstrap")` with a `<script>` element (see below)..</span></span>

<span data-ttu-id="8c094-214">Die Ersetzung verknüpfen CSS:</span><span class="sxs-lookup"><span data-stu-id="8c094-214">The replacement CSS link:</span></span>

```html
<link rel="stylesheet" href="~/lib/bootstrap/dist/css/bootstrap.css" />
```

<span data-ttu-id="8c094-215">Die Ersetzung Skripttags:</span><span class="sxs-lookup"><span data-stu-id="8c094-215">The replacement script tags:</span></span>

```html
<script src="~/lib/jquery/dist/jquery.js"></script>
<script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
```

<span data-ttu-id="8c094-216">Die aktualisierte *_Layout.cshtml* Datei wird unten gezeigt:</span><span class="sxs-lookup"><span data-stu-id="8c094-216">The updated *_Layout.cshtml* file is shown below:</span></span>

<span data-ttu-id="8c094-217">[!code-html[Main](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7,27,39-40)]</span><span class="sxs-lookup"><span data-stu-id="8c094-217">[!code-html[Main](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7,27,39-40)]</span></span>

<span data-ttu-id="8c094-218">Zeigen Sie die Website im Browser.</span><span class="sxs-lookup"><span data-stu-id="8c094-218">View the site in the browser.</span></span> <span data-ttu-id="8c094-219">Es sollte jetzt ordnungsgemäß mit den erwarteten Formatvorlagen direktes Laden.</span><span class="sxs-lookup"><span data-stu-id="8c094-219">It should now load correctly, with the expected styles in place.</span></span>

* <span data-ttu-id="8c094-220">*Optional:* möglicherweise möchten Sie versuchen, mithilfe der neuen Layoutdatei.</span><span class="sxs-lookup"><span data-stu-id="8c094-220">*Optional:* You might want to try using the new layout file.</span></span> <span data-ttu-id="8c094-221">Für dieses Projekt kopieren Sie die Layoutdatei aus der *FullAspNetCore* Projekt.</span><span class="sxs-lookup"><span data-stu-id="8c094-221">For this project you can copy the layout file from the *FullAspNetCore* project.</span></span> <span data-ttu-id="8c094-222">Verwendet die Neue Layoutdatei [Tag Hilfsprogramme](../mvc/views/tag-helpers/index.md) und hat den weiteren Verbesserungen.</span><span class="sxs-lookup"><span data-stu-id="8c094-222">The new layout file uses [Tag Helpers](../mvc/views/tag-helpers/index.md) and has other improvements.</span></span>

## <a name="configure-bundling--minification"></a><span data-ttu-id="8c094-223">Konfigurieren der Bündelung & Minimierung</span><span class="sxs-lookup"><span data-stu-id="8c094-223">Configure Bundling & Minification</span></span>

<span data-ttu-id="8c094-224">Informationen zum Konfigurieren von Bündelung und Minimierung finden Sie unter [Bündelung und Minimierung](../client-side/bundling-and-minification.md).</span><span class="sxs-lookup"><span data-stu-id="8c094-224">For information about how to configure bundling and minification, see [Bundling and Minification](../client-side/bundling-and-minification.md).</span></span>

## <a name="solving-http-500-errors"></a><span data-ttu-id="8c094-225">Beheben von HTTP 500-Fehler</span><span class="sxs-lookup"><span data-stu-id="8c094-225">Solving HTTP 500 errors</span></span>

<span data-ttu-id="8c094-226">Es gibt viele Probleme, die eine HTTP 500-Fehlermeldung verursachen können, die keine Informationen für die Quelle des Problems enthalten.</span><span class="sxs-lookup"><span data-stu-id="8c094-226">There are many problems that can cause a HTTP 500 error message that contain no information on the source of the problem.</span></span> <span data-ttu-id="8c094-227">Beispielsweise, wenn die *Views/_ViewImports.cshtml* -Datei enthält einen Namespace, der in Ihrem Projekt nicht vorhanden ist, erhalten Sie einen HTTP 500-Fehler.</span><span class="sxs-lookup"><span data-stu-id="8c094-227">For example, if the *Views/_ViewImports.cshtml* file contains a namespace that doesn't exist in your project, you'll get a HTTP 500 error.</span></span> <span data-ttu-id="8c094-228">Um eine ausführliche Fehlermeldung zu erhalten, fügen Sie den folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="8c094-228">To get a detailed error message, add the following code:</span></span>

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

<span data-ttu-id="8c094-229">Finden Sie unter **Developer Ausnahme auf der Dienstkontoseite** in [Fehlerbehandlung](../fundamentals/error-handling.md) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="8c094-229">See **Using the Developer Exception Page** in [Error Handling](../fundamentals/error-handling.md) for more information.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8c094-230">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="8c094-230">Additional Resources</span></span>

* [<span data-ttu-id="8c094-231">Clientbasierte Entwicklung</span><span class="sxs-lookup"><span data-stu-id="8c094-231">Client-Side Development</span></span>](../client-side/index.md)

* [<span data-ttu-id="8c094-232">Tag Helpers (Taghilfsprogramme)</span><span class="sxs-lookup"><span data-stu-id="8c094-232">Tag Helpers</span></span>](../mvc/views/tag-helpers/index.md)
