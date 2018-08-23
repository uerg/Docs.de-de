---
uid: web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
title: Erstellen von Hilfeseiten für ASP.NET Web-API | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: riande
ms.date: 04/01/2013
ms.assetid: 0150e67b-c50d-4613-83ea-7b4ef8cacc5a
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
msc.type: authoredcontent
ms.openlocfilehash: 2d8758034ca4339ed7e9699cf2f2643bfab87ba4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832376"
---
<a name="creating-help-pages-for-aspnet-web-api"></a><span data-ttu-id="12646-102">Erstellen von Hilfeseiten für ASP.NET Web-API</span><span class="sxs-lookup"><span data-stu-id="12646-102">Creating Help Pages for ASP.NET Web API</span></span>
====================
<span data-ttu-id="12646-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="12646-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="12646-104">Bei der Erstellung einer Webs-API ist es oft sinnvoll, eine Seite zu erstellen, sodass andere Entwickler zum Aufrufen Ihrer API veranschaulicht werden.</span><span class="sxs-lookup"><span data-stu-id="12646-104">When you create a web API, it is often useful to create a help page, so that other developers will know how to call your API.</span></span> <span data-ttu-id="12646-105">Konnten Sie gesamte Dokumentation manuell erstellen, aber es ist besser, automatisch so weit wie möglich.</span><span class="sxs-lookup"><span data-stu-id="12646-105">You could create all of the documentation manually, but it is better to autogenerate as much as possible.</span></span>

<span data-ttu-id="12646-106">Um diese Aufgabe erleichtern, bietet ASP.NET Web-API für die automatische Generierung von Hilfeseiten eine Bibliothek zur Laufzeit an.</span><span class="sxs-lookup"><span data-stu-id="12646-106">To make this task easier, ASP.NET Web API provides a library for auto-generating help pages at run time.</span></span>

![](creating-api-help-pages/_static/image1.png)

## <a name="creating-api-help-pages"></a><span data-ttu-id="12646-107">Erstellen von API-Hilfeseiten</span><span class="sxs-lookup"><span data-stu-id="12646-107">Creating API Help Pages</span></span>

<span data-ttu-id="12646-108">Installieren Sie [ASP.NET und Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span><span class="sxs-lookup"><span data-stu-id="12646-108">Install [ASP.NET and Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span></span> <span data-ttu-id="12646-109">Dieses Update ist Hilfeseiten in der Web-API-Projektvorlage integriert.</span><span class="sxs-lookup"><span data-stu-id="12646-109">This update integrates help pages into the Web API project template.</span></span>

<span data-ttu-id="12646-110">Als Nächstes erstellen Sie ein neues ASP.NET MVC 4-Projekt, und wählen Sie die Web-API-Projektvorlage aus.</span><span class="sxs-lookup"><span data-stu-id="12646-110">Next, create a new ASP.NET MVC 4 project and select the Web API project template.</span></span> <span data-ttu-id="12646-111">Die Projektvorlage erstellt einen Beispiel-API-Controller mit dem Namen `ValuesController`.</span><span class="sxs-lookup"><span data-stu-id="12646-111">The project template creates an example API controller named `ValuesController`.</span></span> <span data-ttu-id="12646-112">Die Vorlage erstellt auch die API-Hilfeseiten.</span><span class="sxs-lookup"><span data-stu-id="12646-112">The template also creates the API help pages.</span></span> <span data-ttu-id="12646-113">Alle Codedateien für die Hilfeseite befinden sich im Ordner "Areas" des Projekts.</span><span class="sxs-lookup"><span data-stu-id="12646-113">All of the code files for the help page are placed in the Areas folder of the project.</span></span>

![](creating-api-help-pages/_static/image2.png)

<span data-ttu-id="12646-114">Wenn Sie die Anwendung ausführen, enthält der Startseite eine Verknüpfung mit der API-Hilfeseite an.</span><span class="sxs-lookup"><span data-stu-id="12646-114">When you run the application, the home page contains a link to the API help page.</span></span> <span data-ttu-id="12646-115">Auf der Startseite ist der relative Pfad ' / help '.</span><span class="sxs-lookup"><span data-stu-id="12646-115">From the home page, the relative path is /Help.</span></span>

![](creating-api-help-pages/_static/image3.png)

<span data-ttu-id="12646-116">Diesen Link gelangen Sie zu einer Seite "API-Zusammenfassung".</span><span class="sxs-lookup"><span data-stu-id="12646-116">This link brings you to an API summary page.</span></span>

![](creating-api-help-pages/_static/image4.png)

<span data-ttu-id="12646-117">Die MVC-Ansicht für diese Seite wird im Areas/HelpPage/Views/Help/Index.cshtml definiert.</span><span class="sxs-lookup"><span data-stu-id="12646-117">The MVC view for this page is defined in Areas/HelpPage/Views/Help/Index.cshtml.</span></span> <span data-ttu-id="12646-118">Sie können diese Seite, um das Layout, Einführung, Titel, Stile usw. ändern bearbeiten.</span><span class="sxs-lookup"><span data-stu-id="12646-118">You can edit this page to modify the layout, introduction, title, styles, and so forth.</span></span>

<span data-ttu-id="12646-119">Der Hauptteil der Seite ist eine Tabelle mit APIs, die vom Netzwerkcontroller gruppiert.</span><span class="sxs-lookup"><span data-stu-id="12646-119">The main part of the page is a table of APIs, grouped by controller.</span></span> <span data-ttu-id="12646-120">Die Tabelleneinträge dynamisch generiert werden, mithilfe der **IApiExplorer** Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="12646-120">The table entries are generated dynamically, using the **IApiExplorer** interface.</span></span> <span data-ttu-id="12646-121">(Werde ich mehr über diese Schnittstelle später eingehen.) Wenn Sie einen neuen API-Controller hinzufügen, wird die Tabelle automatisch zur Laufzeit aktualisiert werden.</span><span class="sxs-lookup"><span data-stu-id="12646-121">(I'll talk more about this interface later.) If you add a new API controller, the table is automatically updated at run time.</span></span>

<span data-ttu-id="12646-122">Die "API"-Spalte enthält den HTTP-Methode und des relativen URI.</span><span class="sxs-lookup"><span data-stu-id="12646-122">The "API" column lists the HTTP method and relative URI.</span></span> <span data-ttu-id="12646-123">Die Spalte "Beschreibung" enthält die Dokumentation für jede API.</span><span class="sxs-lookup"><span data-stu-id="12646-123">The "Description" column contains documentation for each API.</span></span> <span data-ttu-id="12646-124">Die Dokumentation ist zunächst nur Platzhaltertext.</span><span class="sxs-lookup"><span data-stu-id="12646-124">Initially, the documentation is just placeholder text.</span></span> <span data-ttu-id="12646-125">Im nächsten Abschnitt zeige ich Ihnen wie Dokumentation aus XML-Kommentare hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="12646-125">In the next section, I'll show you how to add documentation from XML comments.</span></span>

<span data-ttu-id="12646-126">Jede API verfügt über einen Link zu einer Seite mit ausführlicheren Informationen, einschließlich Beispiel Anforderungs- und Antworttext.</span><span class="sxs-lookup"><span data-stu-id="12646-126">Each API has a link to a page with more detailed information, including example request and response bodies.</span></span>

![](creating-api-help-pages/_static/image5.png)

## <a name="adding-help-pages-to-an-existing-project"></a><span data-ttu-id="12646-127">Hilfeseiten zu einem vorhandenen Projekt hinzufügen</span><span class="sxs-lookup"><span data-stu-id="12646-127">Adding Help Pages to an Existing Project</span></span>

<span data-ttu-id="12646-128">Sie können Hilfeseiten zu einem vorhandenen Web-API-Projekt mithilfe von NuGet-Paket-Manager hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="12646-128">You can add help pages to an existing Web API project by using NuGet Package Manager.</span></span> <span data-ttu-id="12646-129">Diese Option ist nützlich, die Sie von einer anderen Vorlage als die Vorlage "Web-API" beginnen.</span><span class="sxs-lookup"><span data-stu-id="12646-129">This option is useful you start from a different project template than the "Web API" template.</span></span>

<span data-ttu-id="12646-130">Von der **Tools** , wählen Sie im Menü **Bibliothekspaket-Manager**, und wählen Sie dann **-Paket-Manager-Konsole**.</span><span class="sxs-lookup"><span data-stu-id="12646-130">From the **Tools** menu, select **Library Package Manager**, and then select **Package Manager Console**.</span></span> <span data-ttu-id="12646-131">In der [-Paket-Manager-Konsole](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) Fenster, geben Sie einen der folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="12646-131">In the [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) window, type one of the following commands:</span></span>

<span data-ttu-id="12646-132">Für eine **c#** Anwendung: `Install-Package Microsoft.AspNet.WebApi.HelpPage`</span><span class="sxs-lookup"><span data-stu-id="12646-132">For a **C#** application: `Install-Package Microsoft.AspNet.WebApi.HelpPage`</span></span>

<span data-ttu-id="12646-133">Für eine **Visual Basic** Anwendung: `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`</span><span class="sxs-lookup"><span data-stu-id="12646-133">For a **Visual Basic** application: `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`</span></span>

<span data-ttu-id="12646-134">Es gibt zwei Pakete, die für C#- und 1 für Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="12646-134">There are two packages, one for C# and one for Visual Basic.</span></span> <span data-ttu-id="12646-135">Stellen Sie sicher, dass das Konto verwendet, das Ihr Projekt entspricht.</span><span class="sxs-lookup"><span data-stu-id="12646-135">Make sure to use the one that matches your project.</span></span>

<span data-ttu-id="12646-136">Dieser Befehl installiert die erforderlichen Assemblys und fügt die MVC-Ansichten für die Hilfeseiten (befindet sich im Ordner "Bereiche/HelpPage").</span><span class="sxs-lookup"><span data-stu-id="12646-136">This command installs the necessary assemblies and adds the MVC views for the help pages (located in the Areas/HelpPage folder).</span></span> <span data-ttu-id="12646-137">Sie müssen manuell einen Link zur Hilfeseite für die hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="12646-137">You'll need to manually add a link to the Help page.</span></span> <span data-ttu-id="12646-138">Der URI ist ' / help '.</span><span class="sxs-lookup"><span data-stu-id="12646-138">The URI is /Help.</span></span> <span data-ttu-id="12646-139">Um einen Link in einer Razor-Ansicht zu erstellen, fügen Sie Folgendes hinzu:</span><span class="sxs-lookup"><span data-stu-id="12646-139">To create a link in a razor view, add the following:</span></span>

[!code-cshtml[Main](creating-api-help-pages/samples/sample1.cshtml)]

<span data-ttu-id="12646-140">Stellen Sie außerdem sicher, um Bereiche zu registrieren.</span><span class="sxs-lookup"><span data-stu-id="12646-140">Also, make sure to register areas.</span></span> <span data-ttu-id="12646-141">Fügen Sie in der Datei "Global.asax" den folgenden Code, der **Anwendung\_starten** -Methode, sofern sie nicht bereits angezeigt wird:</span><span class="sxs-lookup"><span data-stu-id="12646-141">In the Global.asax file, add the following code to the **Application\_Start** method, if it is not there already:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample2.cs?highlight=4)]

## <a name="adding-api-documentation"></a><span data-ttu-id="12646-142">Hinzufügen der API-Dokumentation</span><span class="sxs-lookup"><span data-stu-id="12646-142">Adding API Documentation</span></span>

<span data-ttu-id="12646-143">Standardmäßig werden in der Hilfe Seiten sind Platzhalter-Zeichenfolgen für die Dokumentation.</span><span class="sxs-lookup"><span data-stu-id="12646-143">By default, the help pages have placeholder strings for documentation.</span></span> <span data-ttu-id="12646-144">Sie können [XML-Dokumentationskommentare](https://msdn.microsoft.com/library/b2s063f7.aspx) der Dokumentation zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="12646-144">You can use [XML documentation comments](https://msdn.microsoft.com/library/b2s063f7.aspx) to create the documentation.</span></span> <span data-ttu-id="12646-145">Um dieses Feature zu aktivieren, öffnen Sie die Bereiche/HelpPage/App-Datei\_Start/HelpPageConfig.cs und kommentieren Sie die folgende Zeile:</span><span class="sxs-lookup"><span data-stu-id="12646-145">To enable this feature, open the file Areas/HelpPage/App\_Start/HelpPageConfig.cs and uncomment the following line:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample3.cs)]

<span data-ttu-id="12646-146">Jetzt aktivieren Sie XML-Dokumentation.</span><span class="sxs-lookup"><span data-stu-id="12646-146">Now enable XML documentation.</span></span> <span data-ttu-id="12646-147">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste in des Projekts, und wählen **Eigenschaften**.</span><span class="sxs-lookup"><span data-stu-id="12646-147">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="12646-148">Wählen Sie die **erstellen** Seite.</span><span class="sxs-lookup"><span data-stu-id="12646-148">Select the **Build** page.</span></span>

![](creating-api-help-pages/_static/image6.png)

<span data-ttu-id="12646-149">Klicken Sie unter **Ausgabe**, überprüfen Sie **XML-Dokumentationsdatei**.</span><span class="sxs-lookup"><span data-stu-id="12646-149">Under **Output**, check **XML documentation file**.</span></span> <span data-ttu-id="12646-150">Geben Sie in das Bearbeitungsfeld "App\_Data/XmlDocument.xml".</span><span class="sxs-lookup"><span data-stu-id="12646-150">In the edit box, type "App\_Data/XmlDocument.xml".</span></span>

![](creating-api-help-pages/_static/image7.png)

<span data-ttu-id="12646-151">Öffnen Sie als Nächstes den Code für die `ValuesController` -API-Controller, der im /Controllers/ValuesControler.cs definiert ist.</span><span class="sxs-lookup"><span data-stu-id="12646-151">Next, open the code for the `ValuesController` API controller, which is defined in /Controllers/ValuesControler.cs.</span></span> <span data-ttu-id="12646-152">Die Controllermethoden einige Dokumentationskommentare hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="12646-152">Add some documentation comments to the controller methods.</span></span> <span data-ttu-id="12646-153">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="12646-153">For example:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="12646-154">Tipp: Wenn Sie positionieren den Textcursor in der Zeile über der Methode, und geben Sie drei Schrägstriche, fügt Visual Studio automatisch die XML-Elemente.</span><span class="sxs-lookup"><span data-stu-id="12646-154">Tip: If you position the caret on the line above the method and type three forward slashes, Visual Studio automatically inserts the XML elements.</span></span> <span data-ttu-id="12646-155">Sie können dann in die leeren Felder ausfüllen.</span><span class="sxs-lookup"><span data-stu-id="12646-155">Then you can fill in the blanks.</span></span>


<span data-ttu-id="12646-156">Jetzt erstellen Sie und führen Sie die Anwendung erneut aus, und navigieren Sie zu den Hilfeseiten.</span><span class="sxs-lookup"><span data-stu-id="12646-156">Now build and run the application again, and navigate to the help pages.</span></span> <span data-ttu-id="12646-157">Die Dokumentationszeichenfolgen sollte in der API-Tabelle angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="12646-157">The documentation strings should appear in the API table.</span></span>

![](creating-api-help-pages/_static/image8.png)

<span data-ttu-id="12646-158">Die Hilfeseite liest die Zeichenfolgen aus der XML-Datei zur Laufzeit.</span><span class="sxs-lookup"><span data-stu-id="12646-158">The help page reads the strings from the XML file at run time.</span></span> <span data-ttu-id="12646-159">(Wenn Sie die Anwendung bereitstellen, stellen Sie sicher, dass die XML-Datei bereitstellen.)</span><span class="sxs-lookup"><span data-stu-id="12646-159">(When you deploy the application, make sure to deploy the XML file.)</span></span>

## <a name="under-the-hood"></a><span data-ttu-id="12646-160">Im Hintergrund</span><span class="sxs-lookup"><span data-stu-id="12646-160">Under the Hood</span></span>

<span data-ttu-id="12646-161">Hilfeseiten basieren auf der die **ApiExplorer** Klasse, die Bestandteil des Web-API-Framework ist.</span><span class="sxs-lookup"><span data-stu-id="12646-161">The help pages are built on top of the **ApiExplorer** class, which is part of the Web API framework.</span></span> <span data-ttu-id="12646-162">Die **ApiExplorer** Klasse stellt das Material für das Erstellen einer Hilfeseite ein.</span><span class="sxs-lookup"><span data-stu-id="12646-162">The **ApiExplorer** class provides the raw material for creating a help page.</span></span> <span data-ttu-id="12646-163">Für jede API **ApiExplorer** enthält ein **ApiDescription** , die die API beschreibt.</span><span class="sxs-lookup"><span data-stu-id="12646-163">For each API, **ApiExplorer** contains an **ApiDescription** that describes the API.</span></span> <span data-ttu-id="12646-164">Zu diesem Zweck wird eine "API" als die Kombination von HTTP-Methode und des relativen URI definiert.</span><span class="sxs-lookup"><span data-stu-id="12646-164">For this purpose, an "API" is defined as the combination of HTTP method and relative URI.</span></span> <span data-ttu-id="12646-165">Hier sind z. B. einige unterschiedliche APIs:</span><span class="sxs-lookup"><span data-stu-id="12646-165">For example, here are some distinct APIs:</span></span>

- <span data-ttu-id="12646-166">/Api/Products abrufen</span><span class="sxs-lookup"><span data-stu-id="12646-166">GET /api/Products</span></span>
- <span data-ttu-id="12646-167">Abrufen Sie/API/Produkte / {Id}</span><span class="sxs-lookup"><span data-stu-id="12646-167">GET /api/Products/{id}</span></span>
- <span data-ttu-id="12646-168">Veröffentlichen von Produkten/api /</span><span class="sxs-lookup"><span data-stu-id="12646-168">POST /api/Products</span></span>

<span data-ttu-id="12646-169">Wenn eine Controlleraktion mehrere HTTP-Methoden, unterstützt die **ApiExplorer** behandelt jede Methode als distinct-API.</span><span class="sxs-lookup"><span data-stu-id="12646-169">If a controller action supports multiple HTTP methods, the **ApiExplorer** treats each method as a distinct API.</span></span>

<span data-ttu-id="12646-170">Ausblenden eine API aus dem **ApiExplorer**, Hinzufügen der **ApiExplorerSettings** -Attribut auf die Aktion und den Satz *IgnoreApi* auf "true".</span><span class="sxs-lookup"><span data-stu-id="12646-170">To hide an API from the **ApiExplorer**, add the **ApiExplorerSettings** attribute to the action and set *IgnoreApi* to true.</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample5.cs)]

<span data-ttu-id="12646-171">Sie können dieses Attribut auch auf den Controller, für den gesamten Controller ausschließen hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="12646-171">You can also add this attribute to the controller, to exclude the entire controller.</span></span>

<span data-ttu-id="12646-172">Die ApiExplorer Klasse ruft Dokumentationszeichenfolgen aus der **IDocumentationProvider** Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="12646-172">The ApiExplorer class gets documentation strings from the **IDocumentationProvider** interface.</span></span> <span data-ttu-id="12646-173">Wie Sie gesehen haben, um die Hilfeseiten-Bibliothek bietet eine **IDocumentationProvider** Dokumentation von XML-Dokumentationszeichenfolgen abruft.</span><span class="sxs-lookup"><span data-stu-id="12646-173">As you saw earlier, the Help Pages library provides an **IDocumentationProvider** that gets documentation from XML documentation strings.</span></span> <span data-ttu-id="12646-174">Der Code befindet sich im /Areas/HelpPage/XmlDocumentationProvider.cs.</span><span class="sxs-lookup"><span data-stu-id="12646-174">The code is located in /Areas/HelpPage/XmlDocumentationProvider.cs.</span></span> <span data-ttu-id="12646-175">Erhalten Sie Dokumentation aus einer anderen Quelle durch Schreiben eigener **IDocumentationProvider**.</span><span class="sxs-lookup"><span data-stu-id="12646-175">You can get documentation from another source by writing your own **IDocumentationProvider**.</span></span> <span data-ttu-id="12646-176">Aufrufen, um diese verknüpfen, die **SetDocumentationProvider** Erweiterungsmethode, die in definierten **HelpPageConfigurationExtensions**</span><span class="sxs-lookup"><span data-stu-id="12646-176">To wire it up, call the **SetDocumentationProvider** extension method, defined in **HelpPageConfigurationExtensions**</span></span>

<span data-ttu-id="12646-177">**ApiExplorer** ruft automatisch in die **IDocumentationProvider** Schnittstelle zum Abrufen von Dokumentationszeichenfolgen für jede API.</span><span class="sxs-lookup"><span data-stu-id="12646-177">**ApiExplorer** automatically calls into the **IDocumentationProvider** interface to get documentation strings for each API.</span></span> <span data-ttu-id="12646-178">Er speichert sie in der **Dokumentation** Eigenschaft der **ApiDescription** und **ApiParameterDescription** Objekte.</span><span class="sxs-lookup"><span data-stu-id="12646-178">It stores them in the **Documentation** property of the **ApiDescription** and **ApiParameterDescription** objects.</span></span>

## <a name="next-steps"></a><span data-ttu-id="12646-179">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="12646-179">Next Steps</span></span>

<span data-ttu-id="12646-180">Sie sind nicht auf den hier gezeigten Hilfeseiten beschränkt.</span><span class="sxs-lookup"><span data-stu-id="12646-180">You aren't limited to the help pages shown here.</span></span> <span data-ttu-id="12646-181">In der Tat **ApiExplorer** ist nicht zum Erstellen von Hilfeseiten beschränkt.</span><span class="sxs-lookup"><span data-stu-id="12646-181">In fact, **ApiExplorer** is not limited to creating help pages.</span></span> <span data-ttu-id="12646-182">Yao Huang Lin verfügt über einige großartige Blogbeiträge, rufen Sie vielleicht zum Nachdenken standardmäßig geschrieben:</span><span class="sxs-lookup"><span data-stu-id="12646-182">Yao Huang Lin has written some great blog posts to get you thinking out of the box:</span></span>

- [<span data-ttu-id="12646-183">ASP.NET Web API-Hilfeseite hinzugefügt einen einfachen Test-Client</span><span class="sxs-lookup"><span data-stu-id="12646-183">Adding a simple Test Client to ASP.NET Web API Help Page</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/02/adding-a-simple-test-client-to-asp-net-web-api-help-page.aspx)
- [<span data-ttu-id="12646-184">Vornehmen von ASP.NET Web API-Hilfeseite für selbst gehostete Dienste arbeiten</span><span class="sxs-lookup"><span data-stu-id="12646-184">Making ASP.NET Web API Help Page work on self-hosted services</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/20/making-asp-net-web-api-help-page-work-on-self-hosted-services.aspx)
- [<span data-ttu-id="12646-185">Generieren zur Entwurfszeit von Hilfe Seite (oder clientarbeitsauslastungen) für ASP.NET Web-API</span><span class="sxs-lookup"><span data-stu-id="12646-185">Design-time generation of help page (or client) for ASP.NET Web API</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2013/01/20/design-time-generation-of-help-page-or-proxy-for-asp-net-web-api.aspx)
- [<span data-ttu-id="12646-186">Erweiterte Hilfeseite-Anpassungen</span><span class="sxs-lookup"><span data-stu-id="12646-186">Advanced Help Page customizations</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/10/asp-net-web-api-help-page-part-3-advanced-help-page-customizations.aspx)
