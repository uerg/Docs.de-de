---
title: Migrieren von ASP.NET MVC zu ASP.NET Core MVC
author: ardalis
description: Informationen Sie zum Migrieren eines ASP.NET MVC-Projekts zu ASP.NET Core MVC beginnen.
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/mvc
ms.openlocfilehash: b8c913c0a6f47a1c993d508f9baae54981327957
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2018
ms.locfileid: "33851026"
---
# <a name="migrate-from-aspnet-mvc-to-aspnet-core-mvc"></a><span data-ttu-id="ddc3f-103">Migrieren von ASP.NET MVC zu ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="ddc3f-103">Migrate from ASP.NET MVC to ASP.NET Core MVC</span></span>

<span data-ttu-id="ddc3f-104">Durch [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), und [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="ddc3f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="ddc3f-105">In diesem Artikel wird gezeigt, wie für den Einstieg in die Migration von einer ASP.NET MVC-Projekts zu [Core ASP.NET-MVC](../mvc/overview.md).</span><span class="sxs-lookup"><span data-stu-id="ddc3f-105">This article shows how to get started migrating an ASP.NET MVC project to [ASP.NET Core MVC](../mvc/overview.md).</span></span> <span data-ttu-id="ddc3f-106">In der Prozess wird hervorgehoben viele der Aufgaben, die von ASP.NET MVC geändert haben.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-106">In the process, it highlights many of the things that have changed from ASP.NET MVC.</span></span> <span data-ttu-id="ddc3f-107">Migrieren von ASP.NET MVC umfasst mehrere Schritte, und dieser Artikel behandelt die Anfangssetup, grundlegende Controller und Ansichten, statische Inhalte und Client-Side-Abhängigkeiten.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-107">Migrating from ASP.NET MVC is a multiple step process and this article covers the initial setup, basic controllers and views, static content, and client-side dependencies.</span></span> <span data-ttu-id="ddc3f-108">Zusätzliche Artikel behandelt Migrieren der Konfiguration und ID-Code in ASP.NET MVC-Projekte, die viele gefunden.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-108">Additional articles cover migrating configuration and identity code found in many ASP.NET MVC projects.</span></span>

> [!NOTE]
> <span data-ttu-id="ddc3f-109">Die Versionsnummern in die Beispiele sind möglicherweise keine aktuellen.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-109">The version numbers in the samples might not be current.</span></span> <span data-ttu-id="ddc3f-110">Sie müssen möglicherweise Ihre Projekte entsprechend aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-110">You may need to update your projects accordingly.</span></span>

## <a name="create-the-starter-aspnet-mvc-project"></a><span data-ttu-id="ddc3f-111">Erstellen von Starter ASP.NET MVC-Projekt</span><span class="sxs-lookup"><span data-stu-id="ddc3f-111">Create the starter ASP.NET MVC project</span></span>

<span data-ttu-id="ddc3f-112">Um das Upgrade zu demonstrieren, beginnen wir mit eine ASP.NET MVC-app erstellen.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-112">To demonstrate the upgrade, we'll start by creating a ASP.NET MVC app.</span></span> <span data-ttu-id="ddc3f-113">Erstellen sie mit dem Namen *WebApp1* , damit der Namespace des Projekts ASP.NET Core entspricht wir im nächsten Schritt erstellen.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-113">Create it with the name *WebApp1* so the namespace matches the ASP.NET Core project we create in the next step.</span></span>

![Visual Studio-Projekt ein neues Dialogfeld "](mvc/_static/new-project.png)

![Dialogfeld "Neues Webanwendung": MVC-Projektvorlage in ASP.NET Vorlagen Bereich ausgewählt](mvc/_static/new-project-select-mvc-template.png)

<span data-ttu-id="ddc3f-116">*Optional:* ändern Sie den Namen der Projektmappe aus *WebApp1* auf *Mvc5*.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-116">*Optional:* Change the name of the Solution from *WebApp1* to *Mvc5*.</span></span> <span data-ttu-id="ddc3f-117">Zeigt den Namen der neuen Projektmappe von Visual Studio (*Mvc5*), die erleichtert es, teilen Sie das Projekt über das nächste Projekt.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-117">Visual Studio displays the new solution name (*Mvc5*), which makes it easier to tell this project from the next project.</span></span>

## <a name="create-the-aspnet-core-project"></a><span data-ttu-id="ddc3f-118">Erstellen des Projekts ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ddc3f-118">Create the ASP.NET Core project</span></span>

<span data-ttu-id="ddc3f-119">Erstellen Sie ein neues *leere* ASP.NET Core Web-app mit dem gleichen Namen wie das vorherige Projekt (*WebApp1*), damit die Namespaces in beiden Projekten übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-119">Create a new *empty* ASP.NET Core web app with the same name as the previous project (*WebApp1*) so the namespaces in the two projects match.</span></span> <span data-ttu-id="ddc3f-120">Mit dem gleichen Namespace erleichtert es, Code zwischen den zwei Projekten kopieren.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-120">Having the same namespace makes it easier to copy code between the two projects.</span></span> <span data-ttu-id="ddc3f-121">Sie müssen zum Erstellen des Projekts in einem anderen Verzeichnis als das vorherige Projekt, das den gleichen Namen verwenden.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-121">You'll have to create this project in a different directory than the previous project to use the same name.</span></span>

![Dialogfeld "Neues Projekt"](mvc/_static/new_core.png)

![Dialogfeld "neues ASP.NET Web-Anwendung": leere Projektvorlage, die im Bereich der Vorlagen für ASP.NET Core ausgewählt](mvc/_static/new-project-select-empty-aspnet5-template.png)

* <span data-ttu-id="ddc3f-124">*Optional:* Erstellen eines neuen ASP.NET Core app mithilfe der *Webanwendung* -Projektvorlage.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-124">*Optional:* Create a new ASP.NET Core app using the *Web Application* project template.</span></span> <span data-ttu-id="ddc3f-125">Nennen Sie das Projekt *WebApp1*, und wählen Sie eine Authentifizierungsoption in **einzelne Benutzerkonten**.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-125">Name the project *WebApp1*, and select an authentication option of **Individual User Accounts**.</span></span> <span data-ttu-id="ddc3f-126">Benennen Sie diese app *FullAspNetCore*.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-126">Rename this app to *FullAspNetCore*.</span></span> <span data-ttu-id="ddc3f-127">Erstellen dieses Projekt sparen Sie Zeit bei der Konvertierung.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-127">Creating this project saves you time in the conversion.</span></span> <span data-ttu-id="ddc3f-128">Betrachten Sie die Vorlage generiertem Code auf das Endergebnis finden Sie unter und im Code in das Konvertierungsprojekt zu kopieren.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-128">You can look at the template-generated code to see the end result or to copy code to the conversion project.</span></span> <span data-ttu-id="ddc3f-129">Es ist auch hilfreich, wenn Sie auf einen Konvertierungsschritt für den Vergleich mit der Vorlage generiert Projekt festsitzt.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-129">It's also helpful when you get stuck on a conversion step to compare with the template-generated project.</span></span>

## <a name="configure-the-site-to-use-mvc"></a><span data-ttu-id="ddc3f-130">Konfigurieren Sie den Standort für MVC verwenden</span><span class="sxs-lookup"><span data-stu-id="ddc3f-130">Configure the site to use MVC</span></span>

* <span data-ttu-id="ddc3f-131">Wenn .NET Core verwenden möchten, wird der ASP.NET Core Metapackage auf das Projekt mit der Bezeichnung hinzugefügt `Microsoft.AspNetCore.All` standardmäßig.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-131">When targeting .NET Core, the ASP.NET Core metapackage is added to the project, called `Microsoft.AspNetCore.All` by default.</span></span> <span data-ttu-id="ddc3f-132">Dieses Paket enthält Pakete wie `Microsoft.AspNetCore.Mvc` und `Microsoft.AspNetCore.StaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-132">This package contains packages like `Microsoft.AspNetCore.Mvc` and `Microsoft.AspNetCore.StaticFiles`.</span></span> <span data-ttu-id="ddc3f-133">Wenn .NET Framework abzielen, müssen paketverweisen in der Datei \*.csproj einzeln aufgelistet werden.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-133">If targeting .NET Framework, package references need to be listed individually in the \*.csproj file.</span></span>

<span data-ttu-id="ddc3f-134">`Microsoft.AspNetCore.Mvc` ist das ASP.NET-MVC-Framework Core.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-134">`Microsoft.AspNetCore.Mvc` is the ASP.NET Core MVC framework.</span></span> <span data-ttu-id="ddc3f-135">`Microsoft.AspNetCore.StaticFiles` ist der Handler für statische Dateien.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-135">`Microsoft.AspNetCore.StaticFiles` is the static file handler.</span></span> <span data-ttu-id="ddc3f-136">Die Laufzeit ASP.NET Core ist modular aufgebaut, und Sie müssen explizit entscheiden Sie sich für statische Dateien dienen (siehe [statische Dateien](xref:fundamentals/static-files)).</span><span class="sxs-lookup"><span data-stu-id="ddc3f-136">The ASP.NET Core runtime is modular, and you must explicitly opt in to serve static files (see [Static files](xref:fundamentals/static-files)).</span></span>

* <span data-ttu-id="ddc3f-137">Öffnen der *Startup.cs* Datei und ändern Sie den Code entsprechend der folgenden:</span><span class="sxs-lookup"><span data-stu-id="ddc3f-137">Open the *Startup.cs* file and change the code to match the following:</span></span>

  [!code-csharp[](mvc/sample/Startup.cs?highlight=13,26-31)]

<span data-ttu-id="ddc3f-138">Die `UseStaticFiles` Erweiterungsmethode fügt Handler für statische Dateien.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-138">The `UseStaticFiles` extension method adds the static file handler.</span></span> <span data-ttu-id="ddc3f-139">Wie bereits erwähnt, ist die ASP.NET-Laufzeit modular aufgebaut, und Sie müssen explizit entscheiden Sie sich für statische Dateien dienen.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-139">As mentioned previously, the ASP.NET runtime is modular, and you must explicitly opt in to serve static files.</span></span> <span data-ttu-id="ddc3f-140">Die `UseMvc` Erweiterungsmethode fügt routing.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-140">The `UseMvc` extension method adds routing.</span></span> <span data-ttu-id="ddc3f-141">Weitere Informationen finden Sie unter [Anwendungsstart](xref:fundamentals/startup) und [Routing](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="ddc3f-141">For more information, see [Application Startup](xref:fundamentals/startup) and [Routing](xref:fundamentals/routing).</span></span>

## <a name="add-a-controller-and-view"></a><span data-ttu-id="ddc3f-142">Fügen Sie einen Controller und Ansicht hinzu.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-142">Add a controller and view</span></span>

<span data-ttu-id="ddc3f-143">In diesem Abschnitt fügen Sie einem minimalen Controller und Ansicht dienen als Platzhalter für den ASP.NET MVC-Controller und Ansichten werden im nächsten Abschnitt migrieren.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-143">In this section, you'll add a minimal controller and view to serve as placeholders for the ASP.NET MVC controller and views you'll migrate in the next section.</span></span>

* <span data-ttu-id="ddc3f-144">Hinzufügen einer *Controller* Ordner.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-144">Add a *Controllers* folder.</span></span>

* <span data-ttu-id="ddc3f-145">Hinzufügen einer **Controllerklasse** mit dem Namen *HomeController.cs* auf die *Controller* Ordner.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-145">Add a **Controller Class** named *HomeController.cs* to the *Controllers* folder.</span></span>

![Dialogfeld „Neues Element hinzufügen“](mvc/_static/add_mvc_ctl.png)

* <span data-ttu-id="ddc3f-147">Hinzufügen einer *Ansichten* Ordner.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-147">Add a *Views* folder.</span></span>

* <span data-ttu-id="ddc3f-148">Hinzufügen einer *Ansichten/Start* Ordner.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-148">Add a *Views/Home* folder.</span></span>

* <span data-ttu-id="ddc3f-149">Hinzufügen einer **Razor-Ansicht** mit dem Namen *Index.cshtml* auf die *Ansichten/Start* Ordner.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-149">Add a **Razor View** named *Index.cshtml* to the *Views/Home* folder.</span></span>

![Dialogfeld „Neues Element hinzufügen“](mvc/_static/view.png)

<span data-ttu-id="ddc3f-151">Die Projektstruktur wird unten gezeigt:</span><span class="sxs-lookup"><span data-stu-id="ddc3f-151">The project structure is shown below:</span></span>

![Projektmappen-Explorer mit Dateien und Ordner von WebApp1](mvc/_static/project-structure-controller-view.png)

<span data-ttu-id="ddc3f-153">Ersetzen Sie den Inhalt von der *Views/Home/Index.cshtml* Datei durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="ddc3f-153">Replace the contents of the *Views/Home/Index.cshtml* file with the following:</span></span>

```html
<h1>Hello world!</h1>
```

<span data-ttu-id="ddc3f-154">Führen Sie die App aus.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-154">Run the app.</span></span>

![Öffnen Sie in Microsoft Edge-Web-app](mvc/_static/hello-world.png)

<span data-ttu-id="ddc3f-156">Finden Sie unter [Controller](xref:mvc/controllers/actions) und [Ansichten](xref:mvc/views/overview) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-156">See [Controllers](xref:mvc/controllers/actions) and [Views](xref:mvc/views/overview) for more information.</span></span>

<span data-ttu-id="ddc3f-157">Nun, da wir ein minimales funktionierenden ASP.NET Core-Projekt haben, beginnen wir Migrieren von Funktionen von ASP.NET MVC-Projekt.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-157">Now that we have a minimal working ASP.NET Core project, we can start migrating functionality from the ASP.NET MVC project.</span></span> <span data-ttu-id="ddc3f-158">Wir müssen die folgenden verschieben:</span><span class="sxs-lookup"><span data-stu-id="ddc3f-158">We need to move the following:</span></span>

* <span data-ttu-id="ddc3f-159">die clientseitige Inhalt (CSS, Schriftarten und Skripts)</span><span class="sxs-lookup"><span data-stu-id="ddc3f-159">client-side content (CSS, fonts, and scripts)</span></span>

* <span data-ttu-id="ddc3f-160">Controller</span><span class="sxs-lookup"><span data-stu-id="ddc3f-160">controllers</span></span>

* <span data-ttu-id="ddc3f-161">Ansichten</span><span class="sxs-lookup"><span data-stu-id="ddc3f-161">views</span></span>

* <span data-ttu-id="ddc3f-162">Modelle</span><span class="sxs-lookup"><span data-stu-id="ddc3f-162">models</span></span>

* <span data-ttu-id="ddc3f-163">Bundling</span><span class="sxs-lookup"><span data-stu-id="ddc3f-163">bundling</span></span>

* <span data-ttu-id="ddc3f-164">Filter</span><span class="sxs-lookup"><span data-stu-id="ddc3f-164">filters</span></span>

* <span data-ttu-id="ddc3f-165">Melden Sie sich in/out Identität (Dies erfolgt in den nächsten Lernprogrammen.)</span><span class="sxs-lookup"><span data-stu-id="ddc3f-165">Log in/out, Identity (This is done in the next tutorial.)</span></span>

## <a name="controllers-and-views"></a><span data-ttu-id="ddc3f-166">Controller und Ansichten</span><span class="sxs-lookup"><span data-stu-id="ddc3f-166">Controllers and views</span></span>

* <span data-ttu-id="ddc3f-167">Kopieren Sie jede der Methoden von ASP.NET MVC `HomeController` mit dem neuen `HomeController`.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-167">Copy each of the methods from the ASP.NET MVC `HomeController` to the new `HomeController`.</span></span> <span data-ttu-id="ddc3f-168">Beachten Sie, dass in ASP.NET-MVC integrierten Vorlage Controller Aktion Methodenrückgabetyp [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); in ASP.NET Core MVC die Aktionsmethoden return `IActionResult` stattdessen.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-168">Note that in ASP.NET MVC, the built-in template's controller action method return type is [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); in ASP.NET Core MVC, the action methods return `IActionResult` instead.</span></span> <span data-ttu-id="ddc3f-169">`ActionResult` implementiert `IActionResult`, daher keine Notwendigkeit besteht, den Rückgabetyp der Aktionsmethoden zu ändern.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-169">`ActionResult` implements `IActionResult`, so there's no need to change the return type of your action methods.</span></span>

* <span data-ttu-id="ddc3f-170">Kopieren der *About.cshtml*, *Contact.cshtml*, und *Index.cshtml* Razor-Ansicht-Dateien aus dem ASP.NET MVC-Projekt dem ASP.NET Core-Projekt.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-170">Copy the *About.cshtml*, *Contact.cshtml*, and *Index.cshtml* Razor view files from the ASP.NET MVC project to the ASP.NET Core project.</span></span>

* <span data-ttu-id="ddc3f-171">Führen Sie die ASP.NET Core-app, und Testen Sie jede Methode zu.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-171">Run the ASP.NET Core app and test each method.</span></span> <span data-ttu-id="ddc3f-172">Wir noch nicht die Layoutdatei oder Stile migriert noch, damit die gerenderten Ansichten nur den Inhalt in den Dateien anzeigen enthalten.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-172">We haven't migrated the layout file or styles yet, so the rendered views only contain the content in the view files.</span></span> <span data-ttu-id="ddc3f-173">Sie erhalten die Layout generierte dateilinks für keinen der `About` und `Contact` anzeigt, daher Sie diese über den Browser aufrufen müssen (ersetzen Sie **4492** mit der Portnummer, die in Ihrem Projekt verwendet).</span><span class="sxs-lookup"><span data-stu-id="ddc3f-173">You won't have the layout file generated links for the `About` and `Contact` views, so you'll have to invoke them from the browser (replace **4492** with the port number used in your project).</span></span>

  * `http://localhost:4492/home/about`

  * `http://localhost:4492/home/contact`

![Seite "Kontakte"](mvc/_static/contact-page.png)

<span data-ttu-id="ddc3f-175">Beachten Sie die fehlende formatieren und Menüelementen aus.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-175">Note the lack of styling and menu items.</span></span> <span data-ttu-id="ddc3f-176">Dies soll im nächsten Abschnitt behoben werden.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-176">We'll fix that in the next section.</span></span>

## <a name="static-content"></a><span data-ttu-id="ddc3f-177">Statischer Inhalt</span><span class="sxs-lookup"><span data-stu-id="ddc3f-177">Static content</span></span>

<span data-ttu-id="ddc3f-178">In früheren Versionen von ASP.NET MVC statischen Inhalte wurde aus dem Stammverzeichnis des Webprojekts gehostet und wurde mit serverseitigen Dateien zusammensetzt.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-178">In previous versions of ASP.NET MVC, static content was hosted from the root of the web project and was intermixed with server-side files.</span></span> <span data-ttu-id="ddc3f-179">In ASP.NET Core, statischer Inhalt gehostet wird, der *"Wwwroot"* Ordner.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-179">In ASP.NET Core, static content is hosted in the *wwwroot* folder.</span></span> <span data-ttu-id="ddc3f-180">Sie möchten den statischen Inhalt von alten einer ASP.NET MVC-Anwendung zu kopieren der *"Wwwroot"* Projektordners ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-180">You'll want to copy the static content from your old ASP.NET MVC app to the *wwwroot* folder in your ASP.NET Core project.</span></span> <span data-ttu-id="ddc3f-181">In diesem Beispiel Konvertierung:</span><span class="sxs-lookup"><span data-stu-id="ddc3f-181">In this sample conversion:</span></span>

* <span data-ttu-id="ddc3f-182">Kopieren der *favicon.ico* -Datei aus dem alten MVC-Projekt in der *"Wwwroot"* Ordner des Projekts ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-182">Copy the *favicon.ico* file from the old MVC project to the *wwwroot* folder in the ASP.NET Core project.</span></span>

<span data-ttu-id="ddc3f-183">Die alte ASP.NET MVC-Projekt verwendet [Bootstrap](https://getbootstrap.com/) für seine Erstellen von Formaten und speichert die Bootstrap-in Dateien den *Content* und *Skripts* Ordner.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-183">The old ASP.NET MVC project uses [Bootstrap](https://getbootstrap.com/) for its styling and stores the Bootstrap files in the *Content* and *Scripts* folders.</span></span> <span data-ttu-id="ddc3f-184">Die Vorlage, die mit das alte ASP.NET MVC-Projekt generiert wird, verweist auf Bootstrap in der Layoutdatei (*Views/Shared/_Layout.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="ddc3f-184">The template, which generated the old ASP.NET MVC project, references Bootstrap in the layout file (*Views/Shared/_Layout.cshtml*).</span></span> <span data-ttu-id="ddc3f-185">Sie kopieren konnte die *bootstrap.js* und *bootstrap.css* Dateien von ASP.NET MVC-Projekt auf die *"Wwwroot"* Ordner in das neue Projekt.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-185">You could copy the *bootstrap.js* and *bootstrap.css* files from the ASP.NET MVC project to the *wwwroot* folder in the new project.</span></span> <span data-ttu-id="ddc3f-186">Stattdessen fügen wir Unterstützung für Bootstrap (und andere clientseitige Bibliotheken) mithilfe des CDNs im nächsten Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-186">Instead, we'll add support for Bootstrap (and other client-side libraries) using CDNs in the next section.</span></span>

## <a name="migrate-the-layout-file"></a><span data-ttu-id="ddc3f-187">Migrieren Sie die Layoutdatei</span><span class="sxs-lookup"><span data-stu-id="ddc3f-187">Migrate the layout file</span></span>

* <span data-ttu-id="ddc3f-188">Kopieren der *Ansichten "_viewstart.cshtml"* Datei aus des alten ASP.NET MVC-Projekts *Ansichten* Ordner, in des ASP.NET Core Projekts *Ansichten* Ordner.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-188">Copy the *_ViewStart.cshtml* file from the old ASP.NET MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="ddc3f-189">Die *Ansichten "_viewstart.cshtml"* Datei Core ASP.NET-MVC nicht geändert.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-189">The *_ViewStart.cshtml* file has not changed in ASP.NET Core MVC.</span></span>

* <span data-ttu-id="ddc3f-190">Erstellen einer *Ansichten/freigegeben* Ordner.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-190">Create a *Views/Shared* folder.</span></span>

* <span data-ttu-id="ddc3f-191">*Optional:* Kopie *_ViewImports.cshtml* aus der *FullAspNetCore* MVC-Projekt *Ansichten* Ordner, in des ASP.NET Core Projekts  *Sichten* Ordner.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-191">*Optional:* Copy *_ViewImports.cshtml* from the *FullAspNetCore* MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="ddc3f-192">Entfernen Sie alle Namespacedeklaration in der *_ViewImports.cshtml* Datei.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-192">Remove any namespace declaration in the *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="ddc3f-193">Die *_ViewImports.cshtml* Datei bietet Namespaces für alle Dateien anzeigen und bringt in [Tag Hilfsprogramme](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="ddc3f-193">The *_ViewImports.cshtml* file provides namespaces for all the view files and brings in [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="ddc3f-194">Tag-Hilfsmethoden werden in das Neue Layoutdatei verwendet.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-194">Tag Helpers are used in the new layout file.</span></span> <span data-ttu-id="ddc3f-195">Die *_ViewImports.cshtml* Datei ist neu in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-195">The *_ViewImports.cshtml* file is new for ASP.NET Core.</span></span>

* <span data-ttu-id="ddc3f-196">Kopieren der *_Layout.cshtml* Datei aus des alten ASP.NET MVC-Projekts *Ansichten/freigegeben* Ordner, in des ASP.NET Core Projekts *Ansichten/freigegeben* Ordner.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-196">Copy the *_Layout.cshtml* file from the old ASP.NET MVC project's *Views/Shared* folder into the ASP.NET Core project's *Views/Shared* folder.</span></span>

<span data-ttu-id="ddc3f-197">Open *_Layout.cshtml* Datei, und nehmen Sie die folgenden Änderungen (der fertigen Code wird unten dargestellt):</span><span class="sxs-lookup"><span data-stu-id="ddc3f-197">Open *_Layout.cshtml* file and make the following changes (the completed code is shown below):</span></span>

* <span data-ttu-id="ddc3f-198">Ersetzen Sie `@Styles.Render("~/Content/css")` mit einem `<link>` Element laden *bootstrap.css* (siehe unten).</span><span class="sxs-lookup"><span data-stu-id="ddc3f-198">Replace `@Styles.Render("~/Content/css")` with a `<link>` element to load *bootstrap.css* (see below).</span></span>

* <span data-ttu-id="ddc3f-199">Entfernen Sie `@Scripts.Render("~/bundles/modernizr")`.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-199">Remove `@Scripts.Render("~/bundles/modernizr")`.</span></span>

* <span data-ttu-id="ddc3f-200">Kommentieren Sie Sie aus der `@Html.Partial("_LoginPartial")` Zeile (die Zeile mit umschließen `@*...*@`).</span><span class="sxs-lookup"><span data-stu-id="ddc3f-200">Comment out the `@Html.Partial("_LoginPartial")` line (surround the line with `@*...*@`).</span></span> <span data-ttu-id="ddc3f-201">Wir müssen in einem späteren Lernprogramm darauf zurück.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-201">We'll return to it in a future tutorial.</span></span>

* <span data-ttu-id="ddc3f-202">Ersetzen Sie `@Scripts.Render("~/bundles/jquery")` mit einem `<script>` Element (siehe unten).</span><span class="sxs-lookup"><span data-stu-id="ddc3f-202">Replace `@Scripts.Render("~/bundles/jquery")` with a `<script>` element (see below).</span></span>

* <span data-ttu-id="ddc3f-203">Ersetzen Sie `@Scripts.Render("~/bundles/bootstrap")` mit einem `<script>` Element (siehe unten).</span><span class="sxs-lookup"><span data-stu-id="ddc3f-203">Replace `@Scripts.Render("~/bundles/bootstrap")` with a `<script>` element (see below).</span></span>

<span data-ttu-id="ddc3f-204">Das Ersatz-Markup für die Aufnahme der Bootstrap-CSS:</span><span class="sxs-lookup"><span data-stu-id="ddc3f-204">The replacement markup for Bootstrap CSS inclusion:</span></span>

```html
<link rel="stylesheet"
    href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css"
    integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u"
    crossorigin="anonymous">
```

<span data-ttu-id="ddc3f-205">Das Ersatz-Markup für jQuery oder Bootstrap JavaScript einschließen:</span><span class="sxs-lookup"><span data-stu-id="ddc3f-205">The replacement markup for jQuery and Bootstrap JavaScript inclusion:</span></span>

```html
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"
    integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa" crossorigin="anonymous"></script>
```

<span data-ttu-id="ddc3f-206">Die aktualisierte *_Layout.cshtml* Datei wird unten gezeigt:</span><span class="sxs-lookup"><span data-stu-id="ddc3f-206">The updated *_Layout.cshtml* file is shown below:</span></span>

[!code-cshtml[](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7-10,29,41-44)]

<span data-ttu-id="ddc3f-207">Zeigen Sie die Website im Browser.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-207">View the site in the browser.</span></span> <span data-ttu-id="ddc3f-208">Es sollte jetzt ordnungsgemäß mit den erwarteten Formatvorlagen direktes Laden.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-208">It should now load correctly, with the expected styles in place.</span></span>

* <span data-ttu-id="ddc3f-209">*Optional:* möglicherweise möchten Sie versuchen, mithilfe der neuen Layoutdatei.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-209">*Optional:* You might want to try using the new layout file.</span></span> <span data-ttu-id="ddc3f-210">Für dieses Projekt kopieren Sie die Layoutdatei aus der *FullAspNetCore* Projekt.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-210">For this project you can copy the layout file from the *FullAspNetCore* project.</span></span> <span data-ttu-id="ddc3f-211">Verwendet die Neue Layoutdatei [Tag Hilfsprogramme](xref:mvc/views/tag-helpers/intro) und hat den weiteren Verbesserungen.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-211">The new layout file uses [Tag Helpers](xref:mvc/views/tag-helpers/intro) and has other improvements.</span></span>

## <a name="configure-bundling-and-minification"></a><span data-ttu-id="ddc3f-212">Konfigurieren von Bündelung und Minimierung</span><span class="sxs-lookup"><span data-stu-id="ddc3f-212">Configure bundling and minification</span></span>

<span data-ttu-id="ddc3f-213">Informationen zum Konfigurieren von Bündelung und Minimierung finden Sie unter [Bündelung und Minimierung](../client-side/bundling-and-minification.md).</span><span class="sxs-lookup"><span data-stu-id="ddc3f-213">For information about how to configure bundling and minification, see [Bundling and Minification](../client-side/bundling-and-minification.md).</span></span>

## <a name="solve-http-500-errors"></a><span data-ttu-id="ddc3f-214">Beheben von HTTP 500-Fehler</span><span class="sxs-lookup"><span data-stu-id="ddc3f-214">Solve HTTP 500 errors</span></span>

<span data-ttu-id="ddc3f-215">Es gibt viele Probleme, die eine HTTP 500-Fehlermeldung verursachen können, die keine Informationen für die Quelle des Problems enthalten.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-215">There are many problems that can cause a HTTP 500 error message that contain no information on the source of the problem.</span></span> <span data-ttu-id="ddc3f-216">Beispielsweise, wenn die *Views/_ViewImports.cshtml* -Datei enthält einen Namespace, der in Ihrem Projekt nicht vorhanden ist, erhalten Sie einen HTTP 500-Fehler.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-216">For example, if the *Views/_ViewImports.cshtml* file contains a namespace that doesn't exist in your project, you'll get a HTTP 500 error.</span></span> <span data-ttu-id="ddc3f-217">In ASP.NET Core-apps standardmäßig die `UseDeveloperExceptionPage` Erweiterung wird hinzugefügt, um die `IApplicationBuilder` ausgeführt, sobald die Konfiguration ist *Entwicklung*.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-217">By default in ASP.NET Core apps, the `UseDeveloperExceptionPage` extension is added to the `IApplicationBuilder` and executed when the configuration is *Development*.</span></span> <span data-ttu-id="ddc3f-218">Dies ist in den folgenden Code beschrieben:</span><span class="sxs-lookup"><span data-stu-id="ddc3f-218">This is detailed in the following code:</span></span>

[!code-csharp[](mvc/sample/Startup.cs?highlight=19-22)]

<span data-ttu-id="ddc3f-219">ASP.NET Core konvertiert nicht behandelte Ausnahmen in einer Web-app in HTTP 500-Fehlerantworten.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-219">ASP.NET Core converts unhandled exceptions in a web app into HTTP 500 error responses.</span></span> <span data-ttu-id="ddc3f-220">Fehlerdetails werden nicht in der Regel wird in diese Antworten auf die Offenlegung von potenziell vertrauliche Informationen über den Server zu verhindern, dass aufgenommen.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-220">Normally, error details aren't included in these responses to prevent disclosure of potentially sensitive information about the server.</span></span> <span data-ttu-id="ddc3f-221">Finden Sie unter **Developer Ausnahme auf der Dienstkontoseite** in [Fehler behandeln](../fundamentals/error-handling.md) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="ddc3f-221">See **Using the Developer Exception Page** in [Handle errors](../fundamentals/error-handling.md) for more information.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ddc3f-222">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="ddc3f-222">Additional resources</span></span>

* [<span data-ttu-id="ddc3f-223">Clientseitige Entwicklung</span><span class="sxs-lookup"><span data-stu-id="ddc3f-223">Client-side development</span></span>](xref:client-side/index)
* [<span data-ttu-id="ddc3f-224">Taghilfsprogramme</span><span class="sxs-lookup"><span data-stu-id="ddc3f-224">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
