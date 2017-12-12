---
uid: web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
title: "Erstellen von Hilfeseiten für ASP.NET Web-API | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2013
ms.topic: article
ms.assetid: 0150e67b-c50d-4613-83ea-7b4ef8cacc5a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
msc.type: authoredcontent
ms.openlocfilehash: 18d04492529e96b6c0e14f1d7a30378b4832f4c8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="creating-help-pages-for-aspnet-web-api"></a><span data-ttu-id="0d5df-102">Erstellen von Hilfeseiten für ASP.NET Web-API</span><span class="sxs-lookup"><span data-stu-id="0d5df-102">Creating Help Pages for ASP.NET Web API</span></span>
====================
<span data-ttu-id="0d5df-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="0d5df-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="0d5df-104">Wenn Sie eine Web-API erstellen, ist es oft hilfreich, erstellen Sie eine Hilfeseite, damit andere Entwickler wissen, wie Ihre API aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="0d5df-104">When you create a web API, it is often useful to create a help page, so that other developers will know how to call your API.</span></span> <span data-ttu-id="0d5df-105">Gesamte Dokumentation konnte manuell erstellen, aber es ist besser, automatisch so weit wie möglich.</span><span class="sxs-lookup"><span data-stu-id="0d5df-105">You could create all of the documentation manually, but it is better to autogenerate as much as possible.</span></span>

<span data-ttu-id="0d5df-106">Um diese Aufgabe zu vereinfachen, enthält der ASP.NET Web-API eine Bibliothek für die automatische Generierung von Hilfeseiten zur Laufzeit.</span><span class="sxs-lookup"><span data-stu-id="0d5df-106">To make this task easier, ASP.NET Web API provides a library for auto-generating help pages at run time.</span></span>

![](creating-api-help-pages/_static/image1.png)

## <a name="creating-api-help-pages"></a><span data-ttu-id="0d5df-107">API-Hilfeseiten erstellen</span><span class="sxs-lookup"><span data-stu-id="0d5df-107">Creating API Help Pages</span></span>

<span data-ttu-id="0d5df-108">Installieren Sie [ASP.NET und Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span><span class="sxs-lookup"><span data-stu-id="0d5df-108">Install [ASP.NET and Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span></span> <span data-ttu-id="0d5df-109">Dieses Update wird Hilfeseiten in der Web-API-Projektvorlage integriert.</span><span class="sxs-lookup"><span data-stu-id="0d5df-109">This update integrates help pages into the Web API project template.</span></span>

<span data-ttu-id="0d5df-110">Als Nächstes erstellen Sie ein neues ASP.NET MVC 4-Projekt, und wählen Sie die Web-API-Projektvorlage.</span><span class="sxs-lookup"><span data-stu-id="0d5df-110">Next, create a new ASP.NET MVC 4 project and select the Web API project template.</span></span> <span data-ttu-id="0d5df-111">Die Projektvorlage erstellt einen Beispiel-API-Controller mit dem Namen `ValuesController`.</span><span class="sxs-lookup"><span data-stu-id="0d5df-111">The project template creates an example API controller named `ValuesController`.</span></span> <span data-ttu-id="0d5df-112">Die Projektvorlage erstellt auch die API-Hilfeseiten.</span><span class="sxs-lookup"><span data-stu-id="0d5df-112">The template also creates the API help pages.</span></span> <span data-ttu-id="0d5df-113">Alle von die Codedateien für die Hilfeseite befinden sich im Ordner "Bereiche" des Projekts.</span><span class="sxs-lookup"><span data-stu-id="0d5df-113">All of the code files for the help page are placed in the Areas folder of the project.</span></span>

![](creating-api-help-pages/_static/image2.png)

<span data-ttu-id="0d5df-114">Wenn Sie die Anwendung ausführen, enthält der Homepage einen Link, um die API-Hilfeseite an.</span><span class="sxs-lookup"><span data-stu-id="0d5df-114">When you run the application, the home page contains a link to the API help page.</span></span> <span data-ttu-id="0d5df-115">Über die Startseite ist der relative Pfad/Help.</span><span class="sxs-lookup"><span data-stu-id="0d5df-115">From the home page, the relative path is /Help.</span></span>

![](creating-api-help-pages/_static/image3.png)

<span data-ttu-id="0d5df-116">Dieser Link stellt eine Seite "Zusammenfassung API".</span><span class="sxs-lookup"><span data-stu-id="0d5df-116">This link brings you to an API summary page.</span></span>

![](creating-api-help-pages/_static/image4.png)

<span data-ttu-id="0d5df-117">Das MVC-Ansicht für diese Seite wird in Areas/HelpPage/Views/Help/Index.cshtml definiert.</span><span class="sxs-lookup"><span data-stu-id="0d5df-117">The MVC view for this page is defined in Areas/HelpPage/Views/Help/Index.cshtml.</span></span> <span data-ttu-id="0d5df-118">Sie können diese Seite zum Ändern des Layouts, Einführung, Titel, Formatvorlagen und usw. bearbeiten.</span><span class="sxs-lookup"><span data-stu-id="0d5df-118">You can edit this page to modify the layout, introduction, title, styles, and so forth.</span></span>

<span data-ttu-id="0d5df-119">Der Hauptteil der Seite ist eine Tabelle von APIs, die vom Netzwerkcontroller gruppiert.</span><span class="sxs-lookup"><span data-stu-id="0d5df-119">The main part of the page is a table of APIs, grouped by controller.</span></span> <span data-ttu-id="0d5df-120">Die Tabelleneinträge dynamisch generiert werden, mithilfe der **IApiExplorer** Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="0d5df-120">The table entries are generated dynamically, using the **IApiExplorer** interface.</span></span> <span data-ttu-id="0d5df-121">(Weitere Informationen über diese Schnittstelle später werde ich.) Wenn Sie einen neue API-Controller hinzufügen, wird die Tabelle automatisch zur Laufzeit aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="0d5df-121">(I'll talk more about this interface later.) If you add a new API controller, the table is automatically updated at run time.</span></span>

<span data-ttu-id="0d5df-122">Die Spalte "API" listet die HTTP-Methode und die relative URI.</span><span class="sxs-lookup"><span data-stu-id="0d5df-122">The "API" column lists the HTTP method and relative URI.</span></span> <span data-ttu-id="0d5df-123">Die Spalte "Beschreibung" enthält die Dokumentation für jede API.</span><span class="sxs-lookup"><span data-stu-id="0d5df-123">The "Description" column contains documentation for each API.</span></span> <span data-ttu-id="0d5df-124">Zu Beginn wird die Dokumentation nur Platzhaltertext.</span><span class="sxs-lookup"><span data-stu-id="0d5df-124">Initially, the documentation is just placeholder text.</span></span> <span data-ttu-id="0d5df-125">Im nächsten Abschnitt zeige ich Ihnen wie Dokumentation aus XML-Kommentare hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="0d5df-125">In the next section, I'll show you how to add documentation from XML comments.</span></span>

<span data-ttu-id="0d5df-126">Jede API verfügt über einen Link zu einer Seite mit ausführlicheren Informationen, einschließlich Beispiel Anfrage- und Antworttexte.</span><span class="sxs-lookup"><span data-stu-id="0d5df-126">Each API has a link to a page with more detailed information, including example request and response bodies.</span></span>

![](creating-api-help-pages/_static/image5.png)

## <a name="adding-help-pages-to-an-existing-project"></a><span data-ttu-id="0d5df-127">Hilfeseiten zu einem vorhandenen Projekt hinzufügen</span><span class="sxs-lookup"><span data-stu-id="0d5df-127">Adding Help Pages to an Existing Project</span></span>

<span data-ttu-id="0d5df-128">Sie können Hilfeseiten zu einem vorhandenen Web-API-Projekt mithilfe von NuGet-Paket-Manager hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="0d5df-128">You can add help pages to an existing Web API project by using NuGet Package Manager.</span></span> <span data-ttu-id="0d5df-129">Diese Option ist nützlich, die Sie von einer anderen Vorlage als der Vorlage "Web-API" beginnen.</span><span class="sxs-lookup"><span data-stu-id="0d5df-129">This option is useful you start from a different project template than the "Web API" template.</span></span>

<span data-ttu-id="0d5df-130">Aus der **Tools** klicken Sie im Menü **Bibliothekspaket-Manager**, und wählen Sie dann **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="0d5df-130">From the **Tools** menu, select **Library Package Manager**, and then select **Package Manager Console**.</span></span> <span data-ttu-id="0d5df-131">In der [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) Fenster, geben Sie einen der folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="0d5df-131">In the [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) window, type one of the following commands:</span></span>

<span data-ttu-id="0d5df-132">Für eine **c#** Anwendung:`Install-Package Microsoft.AspNet.WebApi.HelpPage`</span><span class="sxs-lookup"><span data-stu-id="0d5df-132">For a **C#** application: `Install-Package Microsoft.AspNet.WebApi.HelpPage`</span></span>

<span data-ttu-id="0d5df-133">Für eine **Visual Basic** Anwendung:`Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`</span><span class="sxs-lookup"><span data-stu-id="0d5df-133">For a **Visual Basic** application: `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`</span></span>

<span data-ttu-id="0d5df-134">Es gibt zwei Pakete, für C#- und eine für Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="0d5df-134">There are two packages, one for C# and one for Visual Basic.</span></span> <span data-ttu-id="0d5df-135">Stellen Sie sicher, dass die zu verwenden, die Ihr Projekt entspricht.</span><span class="sxs-lookup"><span data-stu-id="0d5df-135">Make sure to use the one that matches your project.</span></span>

<span data-ttu-id="0d5df-136">Mit diesem Befehl werden die erforderlichen Assemblys und fügt die MVC-Ansichten für die Hilfeseiten (befindet sich im Ordner "Bereiche/HelpPage").</span><span class="sxs-lookup"><span data-stu-id="0d5df-136">This command installs the necessary assemblies and adds the MVC views for the help pages (located in the Areas/HelpPage folder).</span></span> <span data-ttu-id="0d5df-137">Sie müssen manuell einen Link auf der Seite "Hilfe" hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="0d5df-137">You'll need to manually add a link to the Help page.</span></span> <span data-ttu-id="0d5df-138">Der URI ist/Help.</span><span class="sxs-lookup"><span data-stu-id="0d5df-138">The URI is /Help.</span></span> <span data-ttu-id="0d5df-139">Um einen Link in einer Razor-Ansicht zu erstellen, fügen Sie Folgendes hinzu:</span><span class="sxs-lookup"><span data-stu-id="0d5df-139">To create a link in a razor view, add the following:</span></span>

[!code-cshtml[Main](creating-api-help-pages/samples/sample1.cshtml)]

<span data-ttu-id="0d5df-140">Stellen Sie außerdem sicher, dass Bereiche zu registrieren.</span><span class="sxs-lookup"><span data-stu-id="0d5df-140">Also, make sure to register areas.</span></span> <span data-ttu-id="0d5df-141">Fügen Sie in der Datei "Global.asax" den folgenden Code, der **Anwendung\_starten** -Methode, wenn sie nicht bereits angezeigt wird:</span><span class="sxs-lookup"><span data-stu-id="0d5df-141">In the Global.asax file, add the following code to the **Application\_Start** method, if it is not there already:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample2.cs?highlight=4)]

## <a name="adding-api-documentation"></a><span data-ttu-id="0d5df-142">Hinzufügen von API-Dokumentation</span><span class="sxs-lookup"><span data-stu-id="0d5df-142">Adding API Documentation</span></span>

<span data-ttu-id="0d5df-143">Standardmäßig wird die Hilfe über Seiten verfügen Platzhalter-Zeichenfolgen für die Dokumentation.</span><span class="sxs-lookup"><span data-stu-id="0d5df-143">By default, the help pages have placeholder strings for documentation.</span></span> <span data-ttu-id="0d5df-144">Sie können [XML-Dokumentationskommentare](https://msdn.microsoft.com/en-us/library/b2s063f7.aspx) die Dokumentation zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="0d5df-144">You can use [XML documentation comments](https://msdn.microsoft.com/en-us/library/b2s063f7.aspx) to create the documentation.</span></span> <span data-ttu-id="0d5df-145">Um dieses Feature zu aktivieren, öffnen Sie die Datei Bereiche/HelpPage/App\_Start/HelpPageConfig.cs und die auskommentierung der folgenden Zeile:</span><span class="sxs-lookup"><span data-stu-id="0d5df-145">To enable this feature, open the file Areas/HelpPage/App\_Start/HelpPageConfig.cs and uncomment the following line:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample3.cs)]

<span data-ttu-id="0d5df-146">Aktivieren Sie jetzt die XML-Dokumentation.</span><span class="sxs-lookup"><span data-stu-id="0d5df-146">Now enable XML documentation.</span></span> <span data-ttu-id="0d5df-147">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste des Projekts, und wählen **Eigenschaften**.</span><span class="sxs-lookup"><span data-stu-id="0d5df-147">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="0d5df-148">Wählen Sie die **erstellen** Seite.</span><span class="sxs-lookup"><span data-stu-id="0d5df-148">Select the **Build** page.</span></span>

![](creating-api-help-pages/_static/image6.png)

<span data-ttu-id="0d5df-149">Klicken Sie unter **Ausgabe**, überprüfen Sie **XML-Dokumentationsdatei**.</span><span class="sxs-lookup"><span data-stu-id="0d5df-149">Under **Output**, check **XML documentation file**.</span></span> <span data-ttu-id="0d5df-150">Geben Sie im Bearbeitungsfeld "App\_Data/XmlDocument.xml".</span><span class="sxs-lookup"><span data-stu-id="0d5df-150">In the edit box, type "App\_Data/XmlDocument.xml".</span></span>

![](creating-api-help-pages/_static/image7.png)

<span data-ttu-id="0d5df-151">Als Nächstes öffnen Sie den Code für die `ValuesController` -API-Controller, der im /Controllers/ValuesControler.cs definiert ist.</span><span class="sxs-lookup"><span data-stu-id="0d5df-151">Next, open the code for the `ValuesController` API controller, which is defined in /Controllers/ValuesControler.cs.</span></span> <span data-ttu-id="0d5df-152">Fügen Sie einige Dokumentationskommentare, auf die Controllermethoden.</span><span class="sxs-lookup"><span data-stu-id="0d5df-152">Add some documentation comments to the controller methods.</span></span> <span data-ttu-id="0d5df-153">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="0d5df-153">For example:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="0d5df-154">Tipp: Wenn Sie positionieren die Einfügemarke in der Zeile über der Methode, und geben Sie drei Schrägstriche, fügt Visual Studio automatisch die XML-Elemente.</span><span class="sxs-lookup"><span data-stu-id="0d5df-154">Tip: If you position the caret on the line above the method and type three forward slashes, Visual Studio automatically inserts the XML elements.</span></span> <span data-ttu-id="0d5df-155">Sie können Sie die Leerzeichen eingeben.</span><span class="sxs-lookup"><span data-stu-id="0d5df-155">Then you can fill in the blanks.</span></span>


<span data-ttu-id="0d5df-156">Erstellen Sie nun und die Anwendung erneut auszuführen, und navigieren Sie zu der Hilfeseiten.</span><span class="sxs-lookup"><span data-stu-id="0d5df-156">Now build and run the application again, and navigate to the help pages.</span></span> <span data-ttu-id="0d5df-157">Die Dokumentationszeichenfolgen sollte in der API-Tabelle angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="0d5df-157">The documentation strings should appear in the API table.</span></span>

![](creating-api-help-pages/_static/image8.png)

<span data-ttu-id="0d5df-158">Die Hilfeseite liest die Zeichenfolgen aus der XML-Datei zur Laufzeit.</span><span class="sxs-lookup"><span data-stu-id="0d5df-158">The help page reads the strings from the XML file at run time.</span></span> <span data-ttu-id="0d5df-159">(Wenn Sie die Anwendung bereitstellen, stellen Sie sicher, zum Bereitstellen der XML-Datei.)</span><span class="sxs-lookup"><span data-stu-id="0d5df-159">(When you deploy the application, make sure to deploy the XML file.)</span></span>

## <a name="under-the-hood"></a><span data-ttu-id="0d5df-160">Hinter den Kulissen</span><span class="sxs-lookup"><span data-stu-id="0d5df-160">Under the Hood</span></span>

<span data-ttu-id="0d5df-161">Die Hilfeseiten basieren auf der Basis von der **ApiExplorer** Klasse, die Teil des Web-API-Framework ist.</span><span class="sxs-lookup"><span data-stu-id="0d5df-161">The help pages are built on top of the **ApiExplorer** class, which is part of the Web API framework.</span></span> <span data-ttu-id="0d5df-162">Die **ApiExplorer** Klasse bietet das Material für eine Hilfeseite erstellen.</span><span class="sxs-lookup"><span data-stu-id="0d5df-162">The **ApiExplorer** class provides the raw material for creating a help page.</span></span> <span data-ttu-id="0d5df-163">Für jede API **ApiExplorer** enthält ein **ApiDescription** , beschreibt die API.</span><span class="sxs-lookup"><span data-stu-id="0d5df-163">For each API, **ApiExplorer** contains an **ApiDescription** that describes the API.</span></span> <span data-ttu-id="0d5df-164">Zu diesem Zweck wird eine "API" als Kombination aus HTTP-Methode und der relativen URI definiert.</span><span class="sxs-lookup"><span data-stu-id="0d5df-164">For this purpose, an "API" is defined as the combination of HTTP method and relative URI.</span></span> <span data-ttu-id="0d5df-165">Hier sind z. B. einige distinct-APIs:</span><span class="sxs-lookup"><span data-stu-id="0d5df-165">For example, here are some distinct APIs:</span></span>

- <span data-ttu-id="0d5df-166">/Api/Products abrufen</span><span class="sxs-lookup"><span data-stu-id="0d5df-166">GET /api/Products</span></span>
- <span data-ttu-id="0d5df-167">Abrufen Sie/API/Produkte / {Id}</span><span class="sxs-lookup"><span data-stu-id="0d5df-167">GET /api/Products/{id}</span></span>
- <span data-ttu-id="0d5df-168">Bereitstellen Sie/api /-Produkte</span><span class="sxs-lookup"><span data-stu-id="0d5df-168">POST /api/Products</span></span>

<span data-ttu-id="0d5df-169">Wenn eine Controlleraktion mehrere HTTP-Methoden unterstützt die **ApiExplorer** behandelt jede Methode als eine distinct-API.</span><span class="sxs-lookup"><span data-stu-id="0d5df-169">If a controller action supports multiple HTTP methods, the **ApiExplorer** treats each method as a distinct API.</span></span>

<span data-ttu-id="0d5df-170">Ausblenden eine API aus dem **ApiExplorer**, Hinzufügen der **ApiExplorerSettings** -Attribut auf die Aktion, und legen *IgnoreApi* auf "true".</span><span class="sxs-lookup"><span data-stu-id="0d5df-170">To hide an API from the **ApiExplorer**, add the **ApiExplorerSettings** attribute to the action and set *IgnoreApi* to true.</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample5.cs)]

<span data-ttu-id="0d5df-171">Sie können dieses Attribut auch auf dem Controller an, um den gesamten Controller auszuschließen hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="0d5df-171">You can also add this attribute to the controller, to exclude the entire controller.</span></span>

<span data-ttu-id="0d5df-172">Die Klasse ApiExplorer ruft Dokumentationszeichenfolgen aus dem **IDocumentationProvider** Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="0d5df-172">The ApiExplorer class gets documentation strings from the **IDocumentationProvider** interface.</span></span> <span data-ttu-id="0d5df-173">Wie weiter oben gezeigt, um die Hilfeseiten-Bibliothek stellt eine **IDocumentationProvider** Dokumentation von XML-Dokumentationszeichenfolgen abruft.</span><span class="sxs-lookup"><span data-stu-id="0d5df-173">As you saw earlier, the Help Pages library provides an **IDocumentationProvider** that gets documentation from XML documentation strings.</span></span> <span data-ttu-id="0d5df-174">Der Code befindet sich im /Areas/HelpPage/XmlDocumentationProvider.cs.</span><span class="sxs-lookup"><span data-stu-id="0d5df-174">The code is located in /Areas/HelpPage/XmlDocumentationProvider.cs.</span></span> <span data-ttu-id="0d5df-175">Erhalten Sie Dokumentation aus einer anderen Quelle durch Schreiben von eigenen **IDocumentationProvider**.</span><span class="sxs-lookup"><span data-stu-id="0d5df-175">You can get documentation from another source by writing your own **IDocumentationProvider**.</span></span> <span data-ttu-id="0d5df-176">Aufrufen, um es von Netzwerkdaten, die **SetDocumentationProvider** in definierte Erweiterungsmethode **HelpPageConfigurationExtensions**</span><span class="sxs-lookup"><span data-stu-id="0d5df-176">To wire it up, call the **SetDocumentationProvider** extension method, defined in **HelpPageConfigurationExtensions**</span></span>

<span data-ttu-id="0d5df-177">**ApiExplorer** ruft automatisch in die **IDocumentationProvider** Schnittstelle, um Dokumentationszeichenfolgen für jede API abzurufen.</span><span class="sxs-lookup"><span data-stu-id="0d5df-177">**ApiExplorer** automatically calls into the **IDocumentationProvider** interface to get documentation strings for each API.</span></span> <span data-ttu-id="0d5df-178">Er speichert sie in der **Dokumentation** Eigenschaft von der **ApiDescription** und **ApiParameterDescription** Objekte.</span><span class="sxs-lookup"><span data-stu-id="0d5df-178">It stores them in the **Documentation** property of the **ApiDescription** and **ApiParameterDescription** objects.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0d5df-179">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="0d5df-179">Next Steps</span></span>

<span data-ttu-id="0d5df-180">Sie sind nicht auf die hier gezeigten Hilfeseiten beschränkt.</span><span class="sxs-lookup"><span data-stu-id="0d5df-180">You aren't limited to the help pages shown here.</span></span> <span data-ttu-id="0d5df-181">In der Tat **ApiExplorer** ist nicht auf das Erstellen von Hilfeseiten beschränkt.</span><span class="sxs-lookup"><span data-stu-id="0d5df-181">In fact, **ApiExplorer** is not limited to creating help pages.</span></span> <span data-ttu-id="0d5df-182">Yao Huang Lin wurde geschrieben, dass einige hervorragende Blogeinträge von jovan um erhalten Sie denken ausgegeben:</span><span class="sxs-lookup"><span data-stu-id="0d5df-182">Yao Huang Lin has written some great blog posts to get you thinking out of the box:</span></span>

- [<span data-ttu-id="0d5df-183">Hinzufügen von einer einfachen Testclient zu ASP.NET Web API-Hilfeseite</span><span class="sxs-lookup"><span data-stu-id="0d5df-183">Adding a simple Test Client to ASP.NET Web API Help Page</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/02/adding-a-simple-test-client-to-asp-net-web-api-help-page.aspx)
- [<span data-ttu-id="0d5df-184">Erstellen von ASP.NET Web-API Hilfeseite für selbst gehostete Dienste arbeiten</span><span class="sxs-lookup"><span data-stu-id="0d5df-184">Making ASP.NET Web API Help Page work on self-hosted services</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/20/making-asp-net-web-api-help-page-work-on-self-hosted-services.aspx)
- [<span data-ttu-id="0d5df-185">Während der Entwurfszeit-Generierung von Hilfe zur Seite "(oder clientarbeitsauslastungen) für ASP.NET Web-API</span><span class="sxs-lookup"><span data-stu-id="0d5df-185">Design-time generation of help page (or client) for ASP.NET Web API</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2013/01/20/design-time-generation-of-help-page-or-proxy-for-asp-net-web-api.aspx)
- [<span data-ttu-id="0d5df-186">Erweiterte Hilfeseite-Anpassungen</span><span class="sxs-lookup"><span data-stu-id="0d5df-186">Advanced Help Page customizations</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/10/asp-net-web-api-help-page-part-3-advanced-help-page-customizations.aspx)
