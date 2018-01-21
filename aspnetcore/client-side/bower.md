---
title: Mithilfe von Bower in ASP.NET Core
author: rick-anderson
description: Verwalten von Client-Side-Paketen mit Bower.
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/bower
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 57a6941155c60e2769636fd4abc98531266c206c
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
---
# <a name="manage-client-side-packages-with-bower-in-aspnet-core"></a><span data-ttu-id="e372a-103">Verwalten von Client-Side-Paketen mit Bower in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e372a-103">Manage client-side packages with Bower in ASP.NET Core</span></span>

<span data-ttu-id="e372a-104">Durch [Rick Anderson](https://twitter.com/RickAndMSFT), [Noel Reis](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/), und [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="e372a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Noel Rice](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/), and [Scott Addie](https://scottaddie.com)</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="e372a-105">Auch wenn Bower beibehalten wird, werden sie mit einer anderen Projektmappe.</span><span class="sxs-lookup"><span data-stu-id="e372a-105">While Bower is maintained, they recommend using a different solution.</span></span> <span data-ttu-id="e372a-106">Yarn mit Webpaketdatei ist eine beliebte Alternative für die [migrationsanweisungen](https://bower.io/blog/2017/how-to-migrate-away-from-bower/) verfügbar sind.</span><span class="sxs-lookup"><span data-stu-id="e372a-106">Yarn with Webpack is one popular alternative for which [migration instructions](https://bower.io/blog/2017/how-to-migrate-away-from-bower/) are available.</span></span>

<span data-ttu-id="e372a-107">[Bower](https://bower.io/) ruft sich selbst "Eine Paketmanager für das Web".</span><span class="sxs-lookup"><span data-stu-id="e372a-107">[Bower](https://bower.io/) calls itself "A package manager for the web".</span></span> <span data-ttu-id="e372a-108">Innerhalb der Umgebung .NET füllt die "void", um NuGet Funktion zum Übermitteln von Dateien mit statischer Inhalt nach links.</span><span class="sxs-lookup"><span data-stu-id="e372a-108">Within the .NET ecosystem, it fills the void left by NuGet’s inability to deliver static content files.</span></span> <span data-ttu-id="e372a-109">Für ASP.NET Core, sind dies statischen Dateien clientseitige Bibliotheken wie inhärenten [jQuery](http://jquery.com/) und [Bootstrap](http://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="e372a-109">For ASP.NET Core projects, these static files are inherent to client-side libraries like [jQuery](http://jquery.com/) and [Bootstrap](http://getbootstrap.com/).</span></span> <span data-ttu-id="e372a-110">Für die .NET-Bibliotheken, verwenden Sie immer noch [NuGet](https://www.nuget.org/) Paket-Manager.</span><span class="sxs-lookup"><span data-stu-id="e372a-110">For .NET libraries, you still use [NuGet](https://www.nuget.org/) package manager.</span></span>

<span data-ttu-id="e372a-111">Prozess zur Erstellung neuer Projekte, die mit den richten Sie die clientseitige ASP.NET Core-Projektvorlagen erstellt.</span><span class="sxs-lookup"><span data-stu-id="e372a-111">New projects created with the ASP.NET Core project templates set up the client-side build process.</span></span> <span data-ttu-id="e372a-112">[jQuery](http://jquery.com/) und [Bootstrap](http://getbootstrap.com/) installiert sind, und Bower wird unterstützt.</span><span class="sxs-lookup"><span data-stu-id="e372a-112">[jQuery](http://jquery.com/) and [Bootstrap](http://getbootstrap.com/) are installed, and Bower is supported.</span></span>

<span data-ttu-id="e372a-113">Die clientseitige Pakete werden aufgeführt, der *"bower.JSON"* Datei.</span><span class="sxs-lookup"><span data-stu-id="e372a-113">Client-side packages are listed in the *bower.json* file.</span></span> <span data-ttu-id="e372a-114">Konfiguriert die Projektvorlagen für ASP.NET Core *"bower.JSON"* mit jQuery, jQuery-Validierung und Bootstrap.</span><span class="sxs-lookup"><span data-stu-id="e372a-114">The ASP.NET Core project templates configures *bower.json* with jQuery, jQuery validation, and Bootstrap.</span></span>

<span data-ttu-id="e372a-115">In diesem Lernprogramm fügen wir Unterstützung für [Schriftart Awesome](http://fontawesome.io).</span><span class="sxs-lookup"><span data-stu-id="e372a-115">In this tutorial, we'll add support for [Font Awesome](http://fontawesome.io).</span></span> <span data-ttu-id="e372a-116">Bower Pakete können installiert werden, wobei die **Bower-Pakete verwalten** UI oder manuell in die *"bower.JSON"* Datei.</span><span class="sxs-lookup"><span data-stu-id="e372a-116">Bower packages can be installed with the **Manage Bower Packages** UI or manually in the *bower.json* file.</span></span>

### <a name="installation-via-manage-bower-packages-ui"></a><span data-ttu-id="e372a-117">Installation über Bower-Pakete verwalten UI</span><span class="sxs-lookup"><span data-stu-id="e372a-117">Installation via Manage Bower Packages UI</span></span>

* <span data-ttu-id="e372a-118">Erstellen Sie eine neue ASP.NET Core Web-app mit der **ASP.NET-Webanwendung für Core (.NET Core)** Vorlage.</span><span class="sxs-lookup"><span data-stu-id="e372a-118">Create a new ASP.NET Core Web app with the **ASP.NET Core Web Application (.NET Core)** template.</span></span> <span data-ttu-id="e372a-119">Wählen Sie **-Webanwendung** und **keine Authentifizierung**.</span><span class="sxs-lookup"><span data-stu-id="e372a-119">Select **Web Application** and **No Authentication**.</span></span>

* <span data-ttu-id="e372a-120">Mit der rechten Maustaste des Projekts im Projektmappen-Explorer, und wählen Sie **Bower-Pakete verwalten** (Alternativ im Hauptmenü **Projekt** > **Bower-Pakete verwalten**).</span><span class="sxs-lookup"><span data-stu-id="e372a-120">Right-click the project in Solution Explorer and select **Manage Bower Packages** (alternatively from the main menu, **Project** > **Manage Bower Packages**).</span></span>

* <span data-ttu-id="e372a-121">In der **Bower: \<Projektname\>**  , klicken Sie auf der Registerkarte "Durchsuchen", und klicken Sie dann die Pakete durch Eingabe Filterliste `font-awesome` in das Suchfeld:</span><span class="sxs-lookup"><span data-stu-id="e372a-121">In the **Bower: \<project name\>** window, click the "Browse" tab, and then filter the packages list by entering `font-awesome` in the search box:</span></span>

 ![Bower-Pakete verwalten](bower/_static/manage-bower-packages.png)

* <span data-ttu-id="e372a-123">Überprüfen Sie, ob die "Speichern Sie die Änderungen *" bower.JSON "*" aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="e372a-123">Confirm that the "Save changes to *bower.json*" checkbox is checked.</span></span> <span data-ttu-id="e372a-124">Wählen Sie eine Version aus der Dropdown-Liste, und klicken Sie auf die **installieren** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="e372a-124">Select a version from the drop-down list and click the **Install** button.</span></span> <span data-ttu-id="e372a-125">Die **Ausgabe** Fenster zeigt die Details zur Installation.</span><span class="sxs-lookup"><span data-stu-id="e372a-125">The **Output** window shows the installation details.</span></span>

### <a name="manual-installation-in-bowerjson"></a><span data-ttu-id="e372a-126">Manuelle Installation in "bower.JSON"</span><span class="sxs-lookup"><span data-stu-id="e372a-126">Manual installation in bower.json</span></span>

<span data-ttu-id="e372a-127">Öffnen der *"bower.JSON"* Datei und die Abhängigkeiten hinzuzufügen "Schriftart awesome".</span><span class="sxs-lookup"><span data-stu-id="e372a-127">Open the *bower.json* file and add "font-awesome" to the dependencies.</span></span> <span data-ttu-id="e372a-128">IntelliSense zeigt die verfügbaren Pakete.</span><span class="sxs-lookup"><span data-stu-id="e372a-128">IntelliSense shows the available packages.</span></span> <span data-ttu-id="e372a-129">Wenn ein Paket ausgewählt ist, werden die verfügbaren Versionen angezeigt.</span><span class="sxs-lookup"><span data-stu-id="e372a-129">When a package is selected, the available versions are displayed.</span></span> <span data-ttu-id="e372a-130">Die folgenden Bilder sind ältere und stimmen nicht überein, was Sie sehen.</span><span class="sxs-lookup"><span data-stu-id="e372a-130">The images below are older and will not match what you see.</span></span>

![IntelliSense von Bower Paket-explorer](bower/_static/add-package.png)

![Bower Version IntelliSense](bower/_static/version-intelliSense.png)

<span data-ttu-id="e372a-133">Bower verwendet [semantischer versionsverwaltung](http://semver.org/) , Abhängigkeiten zu organisieren.</span><span class="sxs-lookup"><span data-stu-id="e372a-133">Bower uses [semantic versioning](http://semver.org/) to organize dependencies.</span></span> <span data-ttu-id="e372a-134">Semantische versionsverwaltung, auch bekannt als SemVer, identifiziert die Pakete mit den Nummerierungsschema \<wichtigen >.\< kleinere >. \<Patch >.</span><span class="sxs-lookup"><span data-stu-id="e372a-134">Semantic versioning, also known as SemVer, identifies packages with the numbering scheme \<major>.\<minor>.\<patch>.</span></span> <span data-ttu-id="e372a-135">IntelliSense vereinfacht semantischen versionsverwaltung, indem nur einige allgemeine Optionen angezeigt.</span><span class="sxs-lookup"><span data-stu-id="e372a-135">IntelliSense simplifies semantic versioning by showing only a few common choices.</span></span> <span data-ttu-id="e372a-136">Das oberste Element in der IntelliSense-Liste (4.6.3 im obigen Beispiel) wird die neueste stabile Version des Pakets betrachtet.</span><span class="sxs-lookup"><span data-stu-id="e372a-136">The top item in the IntelliSense list (4.6.3 in the example above) is considered the latest stable version of the package.</span></span> <span data-ttu-id="e372a-137">Das Caretzeichen (^) Symbol entspricht der aktuellsten Hauptversion und die Tilde (~) entspricht der aktuellsten Nebenversion.</span><span class="sxs-lookup"><span data-stu-id="e372a-137">The caret (^) symbol matches the most recent major version and the tilde (~) matches the most recent minor version.</span></span>

<span data-ttu-id="e372a-138">Speichern Sie die *"bower.JSON"* Datei.</span><span class="sxs-lookup"><span data-stu-id="e372a-138">Save the *bower.json* file.</span></span> <span data-ttu-id="e372a-139">Visual Studio wird überwacht, ob die *"bower.JSON"* -Datei für die Änderungen.</span><span class="sxs-lookup"><span data-stu-id="e372a-139">Visual Studio watches the *bower.json* file for changes.</span></span> <span data-ttu-id="e372a-140">Beim Speichern der *Bower Installation* Befehl ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="e372a-140">Upon saving, the *bower install* command is executed.</span></span> <span data-ttu-id="e372a-141">Finden Sie im Ausgabefenster **Bower/Npm** Ansicht für den exakten Befehl ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="e372a-141">See the Output window's **Bower/npm** view for the exact command executed.</span></span>

<span data-ttu-id="e372a-142">Öffnen der *".bowerrc"* Datei unter *"bower.JSON"*.</span><span class="sxs-lookup"><span data-stu-id="e372a-142">Open the *.bowerrc* file under *bower.json*.</span></span> <span data-ttu-id="e372a-143">Die `directory` -Eigenschaftensatz auf *"Wwwroot" / Lib* gibt den Speicherort Bower an die Medienobjekte Paket installiert.</span><span class="sxs-lookup"><span data-stu-id="e372a-143">The `directory` property is set to *wwwroot/lib* which indicates the location Bower will install the package assets.</span></span>

```json
{
 "directory": "wwwroot/lib"
}
```

<span data-ttu-id="e372a-144">Sie können das Suchfeld im Projektmappen-Explorer verwenden, suchen und Anzeigen der Schriftart awesome Paket.</span><span class="sxs-lookup"><span data-stu-id="e372a-144">You can use the search box in Solution Explorer to find and display the font-awesome package.</span></span>

<span data-ttu-id="e372a-145">Öffnen der *Views\Shared\_Layout.cshtml* Datei, und fügen Sie die Schriftart awesome CSS-Datei mit der Umgebung [Tag Helper](xref:mvc/views/tag-helpers/intro) für `Development`.</span><span class="sxs-lookup"><span data-stu-id="e372a-145">Open the *Views\Shared\_Layout.cshtml* file and add the font-awesome CSS file to the environment [Tag Helper](xref:mvc/views/tag-helpers/intro) for `Development`.</span></span> <span data-ttu-id="e372a-146">Klicken Sie im Projektmappen-Explorer ziehen, und legen Sie *Schriftart awesome.css* innerhalb der `<environment names="Development">` Element.</span><span class="sxs-lookup"><span data-stu-id="e372a-146">From Solution Explorer, drag and drop *font-awesome.css* inside the `<environment names="Development">` element.</span></span>

[!code-html[Main](bower/sample/_Layout.cshtml?highlight=4&range=9-13)]

<span data-ttu-id="e372a-147">Fügen Sie in einer Produktions-app *Schriftart awesome.min.css* für die Umgebung Tags Hilfe für `Staging,Production`.</span><span class="sxs-lookup"><span data-stu-id="e372a-147">In a production app you would add *font-awesome.min.css* to the environment tag helper for `Staging,Production`.</span></span>

<span data-ttu-id="e372a-148">Ersetzen Sie den Inhalt von der *Views\Home\About.cshtml* Razor-Datei durch Folgendes Markup:</span><span class="sxs-lookup"><span data-stu-id="e372a-148">Replace the contents of the *Views\Home\About.cshtml* Razor file with the following markup:</span></span>

[!code-html[Main](bower/sample/About.cshtml)]

<span data-ttu-id="e372a-149">Führen Sie die app, und navigieren Sie zu der Info-Ansicht, um zu überprüfen, ob die Schriftart awesome Paket funktioniert.</span><span class="sxs-lookup"><span data-stu-id="e372a-149">Run the app and navigate to the About view to verify the font-awesome package works.</span></span>

## <a name="exploring-the-client-side-build-process"></a><span data-ttu-id="e372a-150">Untersuchen des clientseitigen Build-Prozesses</span><span class="sxs-lookup"><span data-stu-id="e372a-150">Exploring the client-side build process</span></span>

<span data-ttu-id="e372a-151">Die meisten ASP.NET Core-Projektvorlagen sind bereits mithilfe von Bower konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="e372a-151">Most ASP.NET Core project templates are already configured to use Bower.</span></span> <span data-ttu-id="e372a-152">Diese weiter Exemplarische Vorgehensweise beginnt mit einem leeren ASP.NET Core-Projekt und Transformationsprozesses manuell hinzugefügt, sodass Sie einen Eindruck darüber abrufen können, wie Bower in einem Projekt verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="e372a-152">This next walkthrough starts with an empty ASP.NET Core project and adds each piece manually, so you can get a feel for how Bower is used in a project.</span></span> <span data-ttu-id="e372a-153">Sie können sehen, was geschieht mit der Projektstruktur und die Common Language Runtime, die Ausgabe wie für jede konfigurationsänderung vorgenommen wird.</span><span class="sxs-lookup"><span data-stu-id="e372a-153">You can see what happens to the project structure and the runtime output as each configuration change is made.</span></span>

<span data-ttu-id="e372a-154">Die allgemeinen Schritte des Buildprozesses für die clientseitige mit Bower verwendet werden:</span><span class="sxs-lookup"><span data-stu-id="e372a-154">The general steps to use the client-side build process with Bower are:</span></span>

* <span data-ttu-id="e372a-155">Definieren Sie die Pakete im Projekt verwendet.</span><span class="sxs-lookup"><span data-stu-id="e372a-155">Define packages used in your project.</span></span> <!-- once defined, you don't need to download them, VS does -->
* <span data-ttu-id="e372a-156">Referenzpaketen von Webseiten.</span><span class="sxs-lookup"><span data-stu-id="e372a-156">Reference packages from your web pages.</span></span>

### <a name="define-packages"></a><span data-ttu-id="e372a-157">Definieren von Paketen</span><span class="sxs-lookup"><span data-stu-id="e372a-157">Define packages</span></span>

<span data-ttu-id="e372a-158">Nachdem Sie Pakete in Auflisten der *"bower.JSON"* Visual Studio-Datei laden sie herunter.</span><span class="sxs-lookup"><span data-stu-id="e372a-158">Once you list packages in the *bower.json* file, Visual Studio will download them.</span></span> <span data-ttu-id="e372a-159">Im folgenden Beispiel wird Bower jQuery und Bootstrap zum Laden der *"Wwwroot"* Ordner.</span><span class="sxs-lookup"><span data-stu-id="e372a-159">The following example uses Bower to load jQuery and Bootstrap to the *wwwroot* folder.</span></span>

* <span data-ttu-id="e372a-160">Erstellen Sie eine neue ASP.NET Core Web-app mit der **ASP.NET-Webanwendung für Core (.NET Core)** Vorlage.</span><span class="sxs-lookup"><span data-stu-id="e372a-160">Create a new ASP.NET Core Web app with the **ASP.NET Core Web Application (.NET Core)** template.</span></span> <span data-ttu-id="e372a-161">Wählen Sie die **leere** -Projektvorlage, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="e372a-161">Select the **Empty** project template and click **OK**.</span></span>

* <span data-ttu-id="e372a-162">Im Projektmappen-Explorer mit der Maustaste des Projekts > **neues Element hinzufügen** , und wählen Sie **Bower Konfigurationsdatei**.</span><span class="sxs-lookup"><span data-stu-id="e372a-162">In Solution Explorer, right-click the project > **Add New Item** and select **Bower Configuration File**.</span></span> <span data-ttu-id="e372a-163">Hinweis: Ein *".bowerrc"* Datei wird ebenfalls hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="e372a-163">Note: A *.bowerrc* file is also added.</span></span>

* <span data-ttu-id="e372a-164">Open *"bower.JSON"*, Jquery und fügen auf das Bootstrapping der `dependencies` Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="e372a-164">Open *bower.json*, and add jquery and bootstrap to the `dependencies` section.</span></span> <span data-ttu-id="e372a-165">Das resultierende *"bower.JSON"* Datei wird im folgenden Beispiel aussehen.</span><span class="sxs-lookup"><span data-stu-id="e372a-165">The resulting *bower.json* file will look like the following example.</span></span> <span data-ttu-id="e372a-166">Die Versionen mit der Zeit ändert und die folgenden Abbildung möglicherweise nicht überein.</span><span class="sxs-lookup"><span data-stu-id="e372a-166">The versions will change over time and may not match the image below.</span></span>

[!code-json[Main](bower/sample/bower.json?highlight=5,6)]

* <span data-ttu-id="e372a-167">Speichern Sie die *"bower.JSON"* Datei.</span><span class="sxs-lookup"><span data-stu-id="e372a-167">Save the *bower.json* file.</span></span>

 <span data-ttu-id="e372a-168">Überprüfen Sie das Projekt enthält die *bootstrap* und *jQuery* Verzeichnisse in *"Wwwroot" / Lib*.</span><span class="sxs-lookup"><span data-stu-id="e372a-168">Verify the project includes the *bootstrap* and *jQuery* directories in *wwwroot/lib*.</span></span> <span data-ttu-id="e372a-169">Bower verwendet die *".bowerrc"* -Datei zum Installieren von der Objekten in *"Wwwroot" / Lib*.</span><span class="sxs-lookup"><span data-stu-id="e372a-169">Bower uses the *.bowerrc* file to install the assets in *wwwroot/lib*.</span></span>

 <span data-ttu-id="e372a-170">Hinweis: Die Benutzeroberfläche "Bower-Pakete verwalten" bietet eine Alternative zum manuellen bearbeiten.</span><span class="sxs-lookup"><span data-stu-id="e372a-170">Note: The "Manage Bower Packages" UI provides an alternative to manual file editing.</span></span>

### <a name="enable-static-files"></a><span data-ttu-id="e372a-171">Statische Dateien aktivieren</span><span class="sxs-lookup"><span data-stu-id="e372a-171">Enable static files</span></span>

* <span data-ttu-id="e372a-172">Hinzufügen der `Microsoft.AspNetCore.StaticFiles` NuGet-Paket zum Projekt.</span><span class="sxs-lookup"><span data-stu-id="e372a-172">Add the `Microsoft.AspNetCore.StaticFiles` NuGet package to the project.</span></span>
* <span data-ttu-id="e372a-173">Aktivieren Sie die statische Dateien mit versorgt werden die [Middleware für statische Dateien](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions).</span><span class="sxs-lookup"><span data-stu-id="e372a-173">Enable static files to be served with the [Static file middleware](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions).</span></span> <span data-ttu-id="e372a-174">Fügen Sie einen Aufruf von [UseStaticFiles](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions) auf die `Configure` Methode `Startup`.</span><span class="sxs-lookup"><span data-stu-id="e372a-174">Add a call to [UseStaticFiles](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions) to the `Configure` method of `Startup`.</span></span>

[!code-csharp[Main](bower/sample/Startup.cs?highlight=9)]

### <a name="reference-packages"></a><span data-ttu-id="e372a-175">Referenzpaketen</span><span class="sxs-lookup"><span data-stu-id="e372a-175">Reference packages</span></span>

<span data-ttu-id="e372a-176">In diesem Abschnitt erstellen Sie eine HTML-Seite, um sicherzustellen, dass er die bereitgestellten Pakete zugreifen kann.</span><span class="sxs-lookup"><span data-stu-id="e372a-176">In this section, you will create an HTML page to verify it can access the deployed packages.</span></span>

* <span data-ttu-id="e372a-177">Fügen Sie eine neue HTML-Seite mit dem Namen *"Index.HTML"* auf die *"Wwwroot"* Ordner.</span><span class="sxs-lookup"><span data-stu-id="e372a-177">Add a new HTML page named *Index.html* to the *wwwroot* folder.</span></span> <span data-ttu-id="e372a-178">Hinweis: Sie müssen die HTML-Datei zum Hinzufügen der *"Wwwroot"* Ordner.</span><span class="sxs-lookup"><span data-stu-id="e372a-178">Note: You must add the HTML file to the *wwwroot* folder.</span></span> <span data-ttu-id="e372a-179">Standardmäßig kann nicht außerhalb statische Inhalte bereitgestellt werden *"Wwwroot"*.</span><span class="sxs-lookup"><span data-stu-id="e372a-179">By default, static content cannot be served outside *wwwroot*.</span></span> <span data-ttu-id="e372a-180">Finden Sie unter [arbeiten mit statischen Dateien](xref:fundamentals/static-files) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="e372a-180">See [Working with static files](xref:fundamentals/static-files) for more information.</span></span>

 <span data-ttu-id="e372a-181">Ersetzen Sie den Inhalt des *"Index.HTML"* durch Folgendes Markup:</span><span class="sxs-lookup"><span data-stu-id="e372a-181">Replace the contents of *Index.html* with the following markup:</span></span>

[!code-html[Main](bower/sample/Index.html)]

* <span data-ttu-id="e372a-182">Führen Sie die app, und navigieren Sie zu `http://localhost:<port>/Index.html`.</span><span class="sxs-lookup"><span data-stu-id="e372a-182">Run the app and navigate to `http://localhost:<port>/Index.html`.</span></span> <span data-ttu-id="e372a-183">Alternativ können Sie mit *"Index.HTML"* geöffnet haben, drücken Sie `Ctrl+Shift+W`.</span><span class="sxs-lookup"><span data-stu-id="e372a-183">Alternatively, with *Index.html* opened, press `Ctrl+Shift+W`.</span></span> <span data-ttu-id="e372a-184">Stellen Sie sicher, dass die Formatvorlage Jumbotron angewendet wird, die jQuery-Code reagiert werden soll, wenn die Schaltfläche geklickt wird, und die Schaltfläche "Bootstrap" Zustand ändert.</span><span class="sxs-lookup"><span data-stu-id="e372a-184">Verify that the jumbotron styling is applied, the jQuery code responds when the button is clicked, and that the Bootstrap button changes state.</span></span>

 ![Jumbotron-Formatvorlage](bower/_static/jumbotron.png)
