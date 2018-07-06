---
uid: web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
title: Erstellen von RESTful-APIs mit ASP.NET Web-API | Microsoft-Dokumentation
author: rick-anderson
description: In den letzten Jahren hat er deutlich werden, dass HTTP nicht ist nur für HTML-Seiten bereitstellt. Es ist auch eine leistungsstarke Plattform zum Erstellen von Web-APIs, die über eine Handvoll o...
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: 87daa99f-3810-407e-b969-dd28a192959d
msc.legacyurl: /web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 9866b5f75771c633165587ba04e694f72a1e626c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835598"
---
<a name="build-restful-apis-with-aspnet-web-api"></a><span data-ttu-id="f8ccb-104">Erstellen von RESTful-APIs mit ASP.NET Web-API</span><span class="sxs-lookup"><span data-stu-id="f8ccb-104">Build RESTful APIs with ASP.NET Web API</span></span>
====================
<span data-ttu-id="f8ccb-105">durch [Web Camps Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="f8ccb-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="f8ccb-106">In den letzten Jahren hat er deutlich werden, dass HTTP nicht ist nur für HTML-Seiten bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-106">In recent years, it has become clear that HTTP is not just for serving up HTML pages.</span></span> <span data-ttu-id="f8ccb-107">Es ist auch eine leistungsstarke Plattform zum Erstellen von Web-APIs, mit ein paar Verben (GET, POST usw.) sowie einige einfache Konzepte wie *URIs* und *Header*.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-107">It is also a powerful platform for building Web APIs, using a handful of verbs (GET, POST, and so forth) plus a few simple concepts such as *URIs* and *headers*.</span></span> <span data-ttu-id="f8ccb-108">ASP.NET Web-API ist eine Reihe von Komponenten, die HTTP-Programmiermodell zu vereinfachen.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-108">ASP.NET Web API is a set of components that simplify HTTP programming.</span></span> <span data-ttu-id="f8ccb-109">Da es auf die ASP.NET MVC-Laufzeit aufgebaut ist, verarbeitet Web-API automatisch die Low-Level Transportdetails von HTTP.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-109">Because it is built on top of the ASP.NET MVC runtime, Web API automatically handles the low-level transport details of HTTP.</span></span> <span data-ttu-id="f8ccb-110">Zur gleichen Zeit stellt Web-API auf natürliche Weise das HTTP-Programmiermodell.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-110">At the same time, Web API naturally exposes the HTTP programming model.</span></span> <span data-ttu-id="f8ccb-111">Ein Ziel der Web-API in der Tat ist es, *nicht* abstrahieren der Verwendung von HTTP.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-111">In fact, one goal of Web API is to *not* abstract away the reality of HTTP.</span></span> <span data-ttu-id="f8ccb-112">Daher ist Web-API, flexibel und einfach zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-112">As a result, Web API is both flexible and easy to extend.</span></span> <span data-ttu-id="f8ccb-113">In dieser praktischen Übungseinheit verwenden Sie die Web-API, um eine einfache REST-API für eine Kontakt-Manager-Anwendung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-113">In this hands-on lab, you will use Web API to build a simple REST API for a contact manager application.</span></span> <span data-ttu-id="f8ccb-114">Erstellen Sie auch einen Client für die API verwenden.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-114">You will also build a client to consume the API.</span></span> <span data-ttu-id="f8ccb-115">Architektonische REST-Stil erwiesenermaßen eine effektive Methode zur HTTP - Nutzen sein, obwohl es sicherlich nicht der einzige gültige Ansatz für HTTP ist.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-115">The REST architectural style has proven to be an effective way to leverage HTTP - although it is certainly not the only valid approach to HTTP.</span></span> <span data-ttu-id="f8ccb-116">Der Kontakte-Manager wird die RESTful zum Auflisten, hinzufügen und Entfernen von Kontakten, unter anderem verfügbar machen.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-116">The contact manager will expose the RESTful for listing, adding and removing contacts, among others.</span></span> <span data-ttu-id="f8ccb-117">Dieses erfordert ein grundlegendes Verständnis von HTTP, REST, und setzt voraus, dass Sie über grundlegende Kenntnisse von HTML, JavaScript und jQuery verfügen.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-117">This lab requires a basic understanding of HTTP, REST, and assumes you have a basic working knowledge of HTML, JavaScript, and jQuery.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="f8ccb-118">Die ASP.NET-Website verfügt über einen speziellen Bereich für die ASP.NET Web-API-Framework unter [ https://asp.net/web-api ](https://asp.net/web-api).</span><span class="sxs-lookup"><span data-stu-id="f8ccb-118">The ASP.NET Web site has an area dedicated to the ASP.NET Web API framework at [https://asp.net/web-api](https://asp.net/web-api).</span></span> <span data-ttu-id="f8ccb-119">Dieser Standort wird weiterhin bieten aktuelle Informationen, Beispiele und Nachrichten, die im Zusammenhang mit der Web-API so überprüfen Sie sie häufig, wenn Sie tiefer in die Kunst gerne der benutzerdefinierten Web-APIs auf praktisch jedem Gerät oder Entwicklung Framework erstellen.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-119">This site will continue to provide late-breaking information, samples, and news related to Web API, so check it frequently if you'd like to delve deeper into the art of creating custom Web APIs available to virtually any device or development framework.</span></span>
> > 
> > <span data-ttu-id="f8ccb-120">ASP.NET Web-API, wie ASP.NET MVC 4 verfügt über mehr Flexibilität im Hinblick auf die Trennung von der Dienstebene aus den Controllern, und Sie können mehrere Frameworks verfügbar Dependency Injection recht einfach zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-120">ASP.NET Web API, similar to ASP.NET MVC 4, has great flexibility in terms of separating the service layer from the controllers allowing you to use several of the available Dependency Injection frameworks fairly easy.</span></span> <span data-ttu-id="f8ccb-121">Ist es ein gutes Beispiel in MSDN, die zeigt, wie Ninject für Dependency Injection in ASP.NET Web-API-Projekt, das Sie es aus herunterladen [hier](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7).</span><span class="sxs-lookup"><span data-stu-id="f8ccb-121">There is a good sample in MSDN that shows how to use Ninject for dependency injection in an ASP.NET Web API project that you can download it from [here](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7).</span></span>
> 
> 
> <span data-ttu-id="f8ccb-122">Alle Beispielcode und Ausschnitte sind im Web Camps Training Kit unter enthalten [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="f8ccb-122">All sample code and snippets are included in the Web Camps Training Kit, available at [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span></span>


<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="f8ccb-123">Ziele</span><span class="sxs-lookup"><span data-stu-id="f8ccb-123">Objectives</span></span>

<span data-ttu-id="f8ccb-124">In dieser praktischen Übungseinheit erfahren Sie, wie Sie:</span><span class="sxs-lookup"><span data-stu-id="f8ccb-124">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="f8ccb-125">Implementieren einer RESTful-Web-API</span><span class="sxs-lookup"><span data-stu-id="f8ccb-125">Implement a RESTful Web API</span></span>
- <span data-ttu-id="f8ccb-126">Aufrufen der API aus einem HTML-client</span><span class="sxs-lookup"><span data-stu-id="f8ccb-126">Call the API from an HTML client</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="f8ccb-127">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="f8ccb-127">Prerequisites</span></span>

<span data-ttu-id="f8ccb-128">Folgendes ist erforderlich, um diese praktische Übungseinheit auszuführen:</span><span class="sxs-lookup"><span data-stu-id="f8ccb-128">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="f8ccb-129">[Microsoft Visual Studio Express 2012 für Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) oder sogar eine höhere (Lesen [Anhang B](#AppendixB) Anleitungen zur Installation).</span><span class="sxs-lookup"><span data-stu-id="f8ccb-129">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix B](#AppendixB) for instructions on how to install it).</span></span>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="f8ccb-130">Setup</span><span class="sxs-lookup"><span data-stu-id="f8ccb-130">Setup</span></span>

<span data-ttu-id="f8ccb-131">**Installieren von Codeausschnitten**</span><span class="sxs-lookup"><span data-stu-id="f8ccb-131">**Installing Code Snippets**</span></span>

<span data-ttu-id="f8ccb-132">Der Einfachheit halber ist Großteil des Codes, die entlang dieser Übungseinheit verwaltet werden soll als Codeausschnitte für Visual Studio verfügbar.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-132">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="f8ccb-133">So installieren Sie die Codeausschnitte ausführen **.\Source\Setup\CodeSnippets.vsi** Datei.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-133">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="f8ccb-134">Wenn Sie nicht mit dem Visual Studio Code Snippets und zu erfahren, wie Sie deren Verwendung vertraut sind, sehen Sie sich im Anhang in diesem Dokument &quot; [Anhang A: Verwenden von Code Snippets](#AppendixA)&quot;.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-134">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix A: Using Code Snippets](#AppendixA)&quot;.</span></span>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="f8ccb-135">Übungen</span><span class="sxs-lookup"><span data-stu-id="f8ccb-135">Exercises</span></span>

<span data-ttu-id="f8ccb-136">Dieser praktischen Übungseinheit enthält in der folgenden Übung werden:</span><span class="sxs-lookup"><span data-stu-id="f8ccb-136">This hands-on lab includes the following exercise:</span></span>

1. [<span data-ttu-id="f8ccb-137">Übung 1: Erstellen einer nur-Lese Webs-API</span><span class="sxs-lookup"><span data-stu-id="f8ccb-137">Exercise 1: Create a Read-Only Web API</span></span>](#Exercise1)
2. [<span data-ttu-id="f8ccb-138">Übung 2: Erstellen einer Lese-/Schreibzugriff-Webs-API</span><span class="sxs-lookup"><span data-stu-id="f8ccb-138">Exercise 2: Create a Read/Write Web API</span></span>](#Exercise2)
3. [<span data-ttu-id="f8ccb-139">Übung 3: Nutzen Sie die Web-API aus einem HTML-Client</span><span class="sxs-lookup"><span data-stu-id="f8ccb-139">Exercise 3: Consume the Web API from an HTML Client</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="f8ccb-140">Jede Übung umfasst eine **End** Ordner mit der resultierenden Lösung, die Sie nach Abschluss der Übungen abrufen soll.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-140">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="f8ccb-141">Sie können diese Lösung als Leitfaden verwenden, bei Bedarf zusätzliche Hilfe bei der die Übungen durcharbeiten.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-141">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="f8ccb-142">Geschätzte Zeit für diese testumgebung abzuschließen: **60 Minuten**.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-142">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Create_a_Read-Only_Web_API"></a>
### <a name="exercise-1-create-a-read-only-web-api"></a><span data-ttu-id="f8ccb-143">Übung 1: Erstellen einer nur-Lese Webs-API</span><span class="sxs-lookup"><span data-stu-id="f8ccb-143">Exercise 1: Create a Read-Only Web API</span></span>

<span data-ttu-id="f8ccb-144">In dieser Übung implementieren Sie die ReadOnly-GET-Methoden für den Kontakt-Manager.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-144">In this exercise, you will implement the read-only GET methods for the contact manager.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_API_Project"></a>
#### <a name="task-1---creating-the-api-project"></a><span data-ttu-id="f8ccb-145">Aufgabe 1: Erstellen das API-Projekt</span><span class="sxs-lookup"><span data-stu-id="f8ccb-145">Task 1 - Creating the API Project</span></span>

<span data-ttu-id="f8ccb-146">In dieser Aufgabe verwenden Sie die neuen ASP.NET Web-Projektvorlagen zum Erstellen einer Web-API-Web-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-146">In this task, you will use the new ASP.NET web project templates to create a Web API web application.</span></span>

1. <span data-ttu-id="f8ccb-147">Führen Sie **Visual Studio-2012 Express für Web**, Sie gehen Sie dazu zur **starten** , und geben **Visual Studio Express für Web** drücken Sie dann die **EINGABETASTE**.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-147">Run **Visual Studio 2012 Express for Web**, to do this go to **Start** and type **VS Express for Web** then press **Enter**.</span></span>
2. <span data-ttu-id="f8ccb-148">Von der **Datei** , wählen Sie im Menü **neues Projekt**.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-148">From the **File** menu, select **New Project**.</span></span> <span data-ttu-id="f8ccb-149">Wählen Sie die **Visual c# | Web** Projekttyp aus der Strukturansicht des Projekt-Typ aus, und wählen Sie dann die **ASP.NET MVC 4-Webanwendung** Projekttyp.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-149">Select the **Visual C# | Web** project type from the project type tree view, then select the **ASP.NET MVC 4 Web Application** project type.</span></span> <span data-ttu-id="f8ccb-150">Legen Sie die **Namen** zu *ContactManager* und **Projektmappenname** zu *beginnen*, klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-150">Set the project's **Name** to *ContactManager* and the **Solution name** to *Begin*, then click **OK**.</span></span>

    <span data-ttu-id="f8ccb-151">![Erstellen einer neuen ASP.NET MVC 4.0-Webanwendungsprojekt](build-restful-apis-with-aspnet-web-api/_static/image1.png "erstellen eine neue ASP.NET MVC 4.0-Webanwendungsprojekt")</span><span class="sxs-lookup"><span data-stu-id="f8ccb-151">![Creating a new ASP.NET MVC 4.0 Web Application Project](build-restful-apis-with-aspnet-web-api/_static/image1.png "Creating a new ASP.NET MVC 4.0 Web Application Project")</span></span>

    <span data-ttu-id="f8ccb-152">*Erstellen einer neuen ASP.NET MVC 4.0-Webanwendungsprojekt*</span><span class="sxs-lookup"><span data-stu-id="f8ccb-152">*Creating a new ASP.NET MVC 4.0 Web Application Project*</span></span>
3. <span data-ttu-id="f8ccb-153">Wählen Sie im Dialogfeld für den Typ ASP.NET MVC 4-Projekt die **Web-API-** Projekttyp.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-153">In the ASP.NET MVC 4 project type dialog, select the **Web API** project type.</span></span> <span data-ttu-id="f8ccb-154">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-154">Click **OK**.</span></span>

    <span data-ttu-id="f8ccb-155">![Gibt den Typ der Web-API-Projekt](build-restful-apis-with-aspnet-web-api/_static/image2.png ", die den Typ der Web-API-Projekt")</span><span class="sxs-lookup"><span data-stu-id="f8ccb-155">![Specifying the Web API project type](build-restful-apis-with-aspnet-web-api/_static/image2.png "Specifying the Web API project type")</span></span>

    <span data-ttu-id="f8ccb-156">*Gibt den Typ der Web-API-Projekt*</span><span class="sxs-lookup"><span data-stu-id="f8ccb-156">*Specifying the Web API project type*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_the_Contact_Manager_API_Controllers"></a>
#### <a name="task-2---creating-the-contact-manager-api-controllers"></a><span data-ttu-id="f8ccb-157">Aufgabe 2: Erstellen der Kontakt-Manager-API-Controller</span><span class="sxs-lookup"><span data-stu-id="f8ccb-157">Task 2 - Creating the Contact Manager API Controllers</span></span>

<span data-ttu-id="f8ccb-158">In dieser Aufgabe erstellen Sie die Controllerklassen in denen API-Methoden befinden soll.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-158">In this task, you will create the controller classes in which API methods will reside.</span></span>

1. <span data-ttu-id="f8ccb-159">Löschen Sie die Datei mit dem Namen **ValuesController.cs** in **Controller** Ordner aus dem Projekt.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-159">Delete the file named **ValuesController.cs** within **Controllers** folder from the project.</span></span>
2. <span data-ttu-id="f8ccb-160">Mit der rechten Maustaste die **Controller** Ordner im Projekt, und wählen Sie **hinzufügen | Controller** aus dem Kontextmenü.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-160">Right-click the **Controllers** folder in the project and select **Add | Controller** from the context menu.</span></span>

    <span data-ttu-id="f8ccb-161">![Hinzufügen eines neuen Controllers zum Projekt](build-restful-apis-with-aspnet-web-api/_static/image3.png "Hinzufügen eines neuen Controllers zum Projekt")</span><span class="sxs-lookup"><span data-stu-id="f8ccb-161">![Adding a new controller to the project](build-restful-apis-with-aspnet-web-api/_static/image3.png "Adding a new controller to the project")</span></span>

    <span data-ttu-id="f8ccb-162">*Hinzufügen eines neuen Controllers zum Projekt*</span><span class="sxs-lookup"><span data-stu-id="f8ccb-162">*Adding a new controller to the project*</span></span>
3. <span data-ttu-id="f8ccb-163">In der **Controller hinzufügen** Dialogfeld, das angezeigt wird, wählen Sie **leerer API-Controller** Menü der Vorlage.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-163">In the **Add Controller** dialog that appears, select **Empty API Controller** from the Template menu.</span></span> <span data-ttu-id="f8ccb-164">Nennen Sie die Controllerklasse **ContactController**.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-164">Name the controller class **ContactController**.</span></span> <span data-ttu-id="f8ccb-165">Klicken Sie auf **hinzufügen.**</span><span class="sxs-lookup"><span data-stu-id="f8ccb-165">Then, click **Add.**</span></span>

    <span data-ttu-id="f8ccb-166">![Verwenden das Dialogfeld "Controller hinzufügen" zum Erstellen eines neuen Web-API-Controllers](build-restful-apis-with-aspnet-web-api/_static/image4.png "verwenden das Dialogfeld \"Controller hinzufügen\" zum Erstellen eines neuen Web-API-Controllers")</span><span class="sxs-lookup"><span data-stu-id="f8ccb-166">![Using the Add Controller dialog to create a new Web API controller](build-restful-apis-with-aspnet-web-api/_static/image4.png "Using the Add Controller dialog to create a new Web API controller")</span></span>

    <span data-ttu-id="f8ccb-167">*Verwenden das Dialogfeld "Controller hinzufügen" zum Erstellen eines neuen Web-API-Controllers*</span><span class="sxs-lookup"><span data-stu-id="f8ccb-167">*Using the Add Controller dialog to create a new Web API controller*</span></span>
4. <span data-ttu-id="f8ccb-168">Fügen Sie den folgenden Code der **ContactController**.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-168">Add the following code to the **ContactController**.</span></span>

    <span data-ttu-id="f8ccb-169">(Codeausschnitt - *Web-API-Lab - Ex01 - Get-API-Methode*)</span><span class="sxs-lookup"><span data-stu-id="f8ccb-169">(Code Snippet - *Web API Lab - Ex01 - Get API Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample1.cs)]
5. <span data-ttu-id="f8ccb-170">Drücken Sie **F5** zum Debuggen der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-170">Press **F5** to debug the application.</span></span> <span data-ttu-id="f8ccb-171">Die Standardstartseite für eine Web-API-Projekt sollte angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-171">The default home page for a Web API project should appear.</span></span>

    <span data-ttu-id="f8ccb-172">![Standard-Startseite einer ASP.NET Web-API-Anwendung](build-restful-apis-with-aspnet-web-api/_static/image5.png "Standardstartseite einer ASP.NET Web-API-Anwendung")</span><span class="sxs-lookup"><span data-stu-id="f8ccb-172">![The default home page of an ASP.NET Web API application](build-restful-apis-with-aspnet-web-api/_static/image5.png "The default home page of an ASP.NET Web API application")</span></span>

    <span data-ttu-id="f8ccb-173">*Standard-Startseite einer ASP.NET Web-API-Anwendung*</span><span class="sxs-lookup"><span data-stu-id="f8ccb-173">*The default home page of an ASP.NET Web API application*</span></span>
6. <span data-ttu-id="f8ccb-174">Drücken Sie in Internet Explorer-Fenster, das **F12** Schlüssel zum Öffnen der **Entwicklertools** Fenster.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-174">In the Internet Explorer window, press the **F12** key to open the **Developer Tools** window.</span></span> <span data-ttu-id="f8ccb-175">Klicken Sie auf die **Netzwerk** Registerkarte, und klicken Sie dann auf die **erfassen starten** Schaltfläche beginnt die Erfassung von Netzwerkdatenverkehr in das Fenster.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-175">Click the **Network** tab, and then click the **Start Capturing** button to begin capturing network traffic into the window.</span></span>

    <span data-ttu-id="f8ccb-176">![Öffnen die Registerkarte "Netzwerk" und das Initiieren der netzwerkerfassung](build-restful-apis-with-aspnet-web-api/_static/image6.png "öffnen die Registerkarte \"Netzwerk\" und das Initiieren der netzwerkerfassung")</span><span class="sxs-lookup"><span data-stu-id="f8ccb-176">![Opening the network tab and initiating network capture](build-restful-apis-with-aspnet-web-api/_static/image6.png "Opening the network tab and initiating network capture")</span></span>

    <span data-ttu-id="f8ccb-177">*Öffnen die Registerkarte "Netzwerk" und dem Initiieren der netzwerkerfassung*</span><span class="sxs-lookup"><span data-stu-id="f8ccb-177">*Opening the network tab and initiating network capture*</span></span>
7. <span data-ttu-id="f8ccb-178">Fügen Sie die URL in die Sie in der Adressleiste des Browsers mit **/api/Contact** , und drücken Sie die EINGABETASTE.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-178">Append the URL in the browser's address bar with **/api/contact** and press enter.</span></span> <span data-ttu-id="f8ccb-179">Die Details für die Übertragung werden im Netzwerk erfassen-Fenster angezeigt.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-179">The transmission details will appear in the network capture window.</span></span> <span data-ttu-id="f8ccb-180">Beachten Sie, dass die Antwort des MIME-Typ **Application/Json**.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-180">Note that the response's MIME type is **application/json**.</span></span> <span data-ttu-id="f8ccb-181">Dieses Beispiel veranschaulicht, wie das Standardausgabeformat JSON ist.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-181">This demonstrates how the default output format is JSON.</span></span>

    <span data-ttu-id="f8ccb-182">![Anzeigen der Ausgabe der Web-API-Anforderung in der Netzwerkansicht](build-restful-apis-with-aspnet-web-api/_static/image7.png "Anzeigen der Ausgabe der Web-API-Anforderung in der Netzwerk-Ansicht")</span><span class="sxs-lookup"><span data-stu-id="f8ccb-182">![Viewing the output of the Web API request in the Network view](build-restful-apis-with-aspnet-web-api/_static/image7.png "Viewing the output of the Web API request in the Network view")</span></span>

    <span data-ttu-id="f8ccb-183">*Die Ausgabe der Web-API-Anforderung anzeigen in der Netzwerk-Ansicht*</span><span class="sxs-lookup"><span data-stu-id="f8ccb-183">*Viewing the output of the Web API request in the Network view*</span></span>

    > [!NOTE]
    > <span data-ttu-id="f8ccb-184">Internet Explorer 10-Standardverhalten werden nun gefragt, ob die Benutzer sollen, speichern oder Öffnen den Stream, aus der Web-API-Aufruf.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-184">Internet Explorer 10's default behavior at this point will be to ask if the user would like to save or open the stream resulting from the Web API call.</span></span> <span data-ttu-id="f8ccb-185">Die Ausgabe wird eine Textdatei mit dem JSON-Ergebnis des Aufrufs Web-API-URL sein.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-185">The output will be a text file containing the JSON result of the Web API URL call.</span></span> <span data-ttu-id="f8ccb-186">Brechen Sie nicht das Dialogfeld, um Inhalte von der Antwort durch Entwickler Toolfenster ansehen zu können.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-186">Do not cancel the dialog in order to be able to watch the response's content through Developers Tool window.</span></span>
8. <span data-ttu-id="f8ccb-187">Klicken Sie auf die **wechseln Sie zur detaillierten Ansicht** Schaltfläche, um weitere Details zu dieser API-Aufruf die Antwort anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-187">Click the **Go to detailed view** button to see more details about the response of this API call.</span></span>

    <span data-ttu-id="f8ccb-188">![Wechseln Sie zur detaillierten Ansicht](build-restful-apis-with-aspnet-web-api/_static/image8.png "wechseln Sie zur Detailansicht")</span><span class="sxs-lookup"><span data-stu-id="f8ccb-188">![Switch to Detailed View](build-restful-apis-with-aspnet-web-api/_static/image8.png "Switch to Details View")</span></span>

    <span data-ttu-id="f8ccb-189">*Wechseln Sie zur detaillierten Ansicht*</span><span class="sxs-lookup"><span data-stu-id="f8ccb-189">*Switch to Detailed View*</span></span>
9. <span data-ttu-id="f8ccb-190">Klicken Sie auf die **Antworttext** Registerkarte, um den tatsächlichen Text der JSON-Antwort anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-190">Click the **Response body** tab to view the actual JSON response text.</span></span>

    <span data-ttu-id="f8ccb-191">![Anzeigen des JSON-Codes in der Microsoft-Netzwerkmonitor Ausgabetext](build-restful-apis-with-aspnet-web-api/_static/image9.png "Ausgabetext Anzeigen des JSON-Codes in der Microsoft-Netzwerkmonitor")</span><span class="sxs-lookup"><span data-stu-id="f8ccb-191">![Viewing the JSON output text in the network monitor](build-restful-apis-with-aspnet-web-api/_static/image9.png "Viewing the JSON output text in the network monitor")</span></span>

    <span data-ttu-id="f8ccb-192">*Anzeigen von JSON-Ausgabetext in den Netzwerkmonitor*</span><span class="sxs-lookup"><span data-stu-id="f8ccb-192">*Viewing the JSON output text in the network monitor*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Creating_the_Contact_Models_and_Augment_the_Contact_Controller"></a>
#### <a name="task-3---creating-the-contact-models-and-augment-the-contact-controller"></a><span data-ttu-id="f8ccb-193">Aufgabe 3: beim Erstellen der Modelle wenden Sie sich an, und Erweitern der Contact-Controller</span><span class="sxs-lookup"><span data-stu-id="f8ccb-193">Task 3 - Creating the Contact Models and Augment the Contact Controller</span></span>

<span data-ttu-id="f8ccb-194">In dieser Aufgabe erstellen Sie die Controllerklassen in denen API-Methoden befinden soll.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-194">In this task, you will create the controller classes in which API methods will reside.</span></span>

1. <span data-ttu-id="f8ccb-195">Mit der rechten Maustaste die **Modelle** Ordner, und wählen **hinzufügen | Klasse...**  aus dem Kontextmenü.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-195">Right-click the **Models** folder and select **Add | Class...** from the context menu.</span></span>

    <span data-ttu-id="f8ccb-196">![Hinzufügen eines neuen Modells an die Webanwendung](build-restful-apis-with-aspnet-web-api/_static/image10.png "Hinzufügen eines neuen Modells an die Webanwendung")</span><span class="sxs-lookup"><span data-stu-id="f8ccb-196">![Adding a new model to the web application](build-restful-apis-with-aspnet-web-api/_static/image10.png "Adding a new model to the web application")</span></span>

    <span data-ttu-id="f8ccb-197">*Hinzufügen eines neuen Modells an die Webanwendung*</span><span class="sxs-lookup"><span data-stu-id="f8ccb-197">*Adding a new model to the web application*</span></span>
2. <span data-ttu-id="f8ccb-198">In der **neues Element hinzufügen** Dialogfeld benennen Sie die neue Datei **Contact.cs** , und klicken Sie auf **hinzufügen.**</span><span class="sxs-lookup"><span data-stu-id="f8ccb-198">In the **Add New Item** dialog, name the new file **Contact.cs** and click **Add.**</span></span>

    <span data-ttu-id="f8ccb-199">![Erstellen der neuen Kontakt-Klassendatei](build-restful-apis-with-aspnet-web-api/_static/image11.png "-Datei der neue Kontakt erstellen")</span><span class="sxs-lookup"><span data-stu-id="f8ccb-199">![Creating the new Contact class file](build-restful-apis-with-aspnet-web-api/_static/image11.png "Creating the new Contact class file")</span></span>

    <span data-ttu-id="f8ccb-200">*Erstellen die neue Klassendatei in Kontakt*</span><span class="sxs-lookup"><span data-stu-id="f8ccb-200">*Creating the new Contact class file*</span></span>
3. <span data-ttu-id="f8ccb-201">Fügen Sie den folgenden hervorgehobenen Code in die **wenden Sie sich an** Klasse.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-201">Add the following highlighted code to the **Contact** class.</span></span>

    <span data-ttu-id="f8ccb-202">(Codeausschnitt - *Web-API-Lab - Ex01 - Kontakt Klasse*)</span><span class="sxs-lookup"><span data-stu-id="f8ccb-202">(Code Snippet - *Web API Lab - Ex01 - Contact Class*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample2.cs)]
4. <span data-ttu-id="f8ccb-203">In der **ContactController** Klasse, markieren Sie das Wort **Zeichenfolge** in der Definition der Methode die **erhalten** -Methode, und geben Sie das Wort *wenden Sie sich an*.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-203">In the **ContactController** class, select the word **string** in method definition of the **Get** method, and type the word *Contact*.</span></span> <span data-ttu-id="f8ccb-204">Sobald Sie in das Wort eingeben, erscheint ein Indikator am Anfang des Worts **wenden Sie sich an**.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-204">Once the word is typed in, an indicator will appear at the beginning of the word **Contact**.</span></span> <span data-ttu-id="f8ccb-205">Entweder halten Sie die **STRG** Schlüssel, und drücken Sie auf den Punkt (.), oder klicken Sie auf das Symbol mit der Maus, um das Dialogfeld "Hilfe" im Code-Editor automatisch ausfüllen zu öffnen der **mit** Direktive für die Modelle Namespace.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-205">Either hold down the **Ctrl** key and press the period (.) key or click the icon using your mouse to open up the assistance dialog in the code editor, to automatically fill in the **using** directive for the Models namespace.</span></span>

    ![Verwenden Intellisense-Unterstützung für Namespacedeklarationen](build-restful-apis-with-aspnet-web-api/_static/image12.png)

    <span data-ttu-id="f8ccb-207">*Verwenden Intellisense-Unterstützung für Namespacedeklarationen*</span><span class="sxs-lookup"><span data-stu-id="f8ccb-207">*Using Intellisense assistance for namespace declarations*</span></span>
5. <span data-ttu-id="f8ccb-208">Ändern Sie den Code für die **erhalten** Methode so, dass die It ein Array von Instanzen der Kontakt-Modell zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-208">Modify the code for the **Get** method so that it returns an array of Contact model instances.</span></span>

    <span data-ttu-id="f8ccb-209">(Codeausschnitt - *Web-API-Lab - Ex01 - Rückgabe einer Liste von Kontakten*)</span><span class="sxs-lookup"><span data-stu-id="f8ccb-209">(Code Snippet - *Web API Lab - Ex01 - Returning a list of contacts*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample3.cs)]
6. <span data-ttu-id="f8ccb-210">Drücken Sie **F5** zum Debuggen der Webanwendung im Browser.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-210">Press **F5** to debug the web application in the browser.</span></span> <span data-ttu-id="f8ccb-211">Um die an die antwortausgabe der API vorgenommenen Änderungen anzuzeigen, führen Sie die folgenden Schritte aus.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-211">To view the changes made to the response output of the API, perform the following steps.</span></span>

   1. <span data-ttu-id="f8ccb-212">Wenn der Browser geöffnet wird, drücken Sie die **F12** , wenn die Developer Tools noch nicht geöffnet sind.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-212">Once the browser opens, press **F12** if the developer tools are not open yet.</span></span>
   2. <span data-ttu-id="f8ccb-213">Klicken Sie auf die **Netzwerk** Registerkarte.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-213">Click the **Network** tab.</span></span>
   3. <span data-ttu-id="f8ccb-214">Drücken Sie die **erfassen starten** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-214">Press the **Start Capturing** button.</span></span>
   4. <span data-ttu-id="f8ccb-215">Fügen Sie das URL-Suffix **/api/Contact** an die URL in die Adressleiste ein und drücken Sie die **EINGABETASTE** Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-215">Add the URL suffix **/api/contact** to the URL in the address bar and press the **Enter** key.</span></span>
   5. <span data-ttu-id="f8ccb-216">Drücken Sie die **wechseln Sie zur detaillierten Ansicht** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-216">Press the **Go to detailed view** button.</span></span>
   6. <span data-ttu-id="f8ccb-217">Wählen Sie die **Antworttext** Registerkarte. Daraufhin sollte eine JSON-Zeichenfolge, die die serialisierte Form eines Arrays von wenden Sie sich an Instanzen darstellt.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-217">Select the **Response body** tab. You should see a JSON string representing the serialized form of an array of Contact instances.</span></span>

      <span data-ttu-id="f8ccb-218">![JSON-serialisierte Ausgabe von komplexer Aufruf einer Web-API-Methode](build-restful-apis-with-aspnet-web-api/_static/image13.png "JSON serialisierte Ausgabe von komplexer Aufruf einer Web-API-Methode")</span><span class="sxs-lookup"><span data-stu-id="f8ccb-218">![JSON serialized output of a complex Web API method call](build-restful-apis-with-aspnet-web-api/_static/image13.png "JSON serialized output of a complex Web API method call")</span></span>

      <span data-ttu-id="f8ccb-219">*JSON-serialisierte Ausgabe von komplexer Aufruf einer Web-API-Methode*</span><span class="sxs-lookup"><span data-stu-id="f8ccb-219">*JSON serialized output of a complex Web API method call*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Extracting_Functionality_into_a_Service_Layer"></a>
#### <a name="task-4---extracting-functionality-into-a-service-layer"></a><span data-ttu-id="f8ccb-220">Aufgabe 4: Extrahieren von Funktionen in einer Dienstebene</span><span class="sxs-lookup"><span data-stu-id="f8ccb-220">Task 4 - Extracting Functionality into a Service Layer</span></span>

<span data-ttu-id="f8ccb-221">Diese Aufgabe zeigen, wie Funktionen in einer Dienstschicht erleichtert Entwicklern das Trennen ihrer Dienstfunktionalität von der Controller-Ebene, wodurch die wiederverwendbarkeit der Dienste, die eigentliche Arbeit ausführen zu extrahieren.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-221">This task will demonstrate how to extract functionality into a Service layer to make it easy for developers to separate their service functionality from the controller layer, thereby allowing reusability of the services that actually do the work.</span></span>

1. <span data-ttu-id="f8ccb-222">Erstellen Sie einen neuen Ordner in den Projektmappenstamm, und nennen Sie sie **Services**.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-222">Create a new folder in the solution root and name it **Services**.</span></span> <span data-ttu-id="f8ccb-223">Dazu, mit der Maustaste **ContactManager** -Projekt, wählen **hinzufügen** | **neuer Ordner**, nennen Sie sie *Services*.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-223">To do this, right-click **ContactManager** project, select **Add** | **New Folder**, name it *Services*.</span></span>

    <span data-ttu-id="f8ccb-224">![Erstellen der Ordner "Dienste"](build-restful-apis-with-aspnet-web-api/_static/image14.png "diensteordner erstellen")</span><span class="sxs-lookup"><span data-stu-id="f8ccb-224">![Creating Services folder](build-restful-apis-with-aspnet-web-api/_static/image14.png "Creating Services folder")</span></span>

    <span data-ttu-id="f8ccb-225">*Erstellen Ordner "Dienste"*</span><span class="sxs-lookup"><span data-stu-id="f8ccb-225">*Creating Services folder*</span></span>
2. <span data-ttu-id="f8ccb-226">Mit der rechten Maustaste die **Services** Ordner, und wählen **hinzufügen | Klasse...**  aus dem Kontextmenü.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-226">Right-click the **Services** folder and select **Add | Class...** from the context menu.</span></span>

    <span data-ttu-id="f8ccb-227">![Hinzufügen einer neuen Klasse zum Ordner "Dienste"](build-restful-apis-with-aspnet-web-api/_static/image15.png "eine neue Klasse hinzufügen, um den Ordner \"Dienste\"")</span><span class="sxs-lookup"><span data-stu-id="f8ccb-227">![Adding a new class to the Services folder](build-restful-apis-with-aspnet-web-api/_static/image15.png "Adding a new class to the Services folder")</span></span>

    <span data-ttu-id="f8ccb-228">*Hinzufügen einer neuen Klasse zum Ordner "Dienste"*</span><span class="sxs-lookup"><span data-stu-id="f8ccb-228">*Adding a new class to the Services folder*</span></span>
3. <span data-ttu-id="f8ccb-229">Wenn die **neues Element hinzufügen** Dialogfeld angezeigt wird, benennen Sie die neue Klasse **ContactRepository** , und klicken Sie auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-229">When the **Add New Item** dialog appears, name the new class **ContactRepository** and click **Add**.</span></span>

    <span data-ttu-id="f8ccb-230">![Erstellen eine Klassendatei, um den Code für die Dienstebene der Kontakt-Repository enthält](build-restful-apis-with-aspnet-web-api/_static/image16.png "erstellen eine Klassendatei, um den Code für die Dienstebene der Kontakt-Repository enthält")</span><span class="sxs-lookup"><span data-stu-id="f8ccb-230">![Creating a class file to contain the code for the Contact Repository service layer](build-restful-apis-with-aspnet-web-api/_static/image16.png "Creating a class file to contain the code for the Contact Repository service layer")</span></span>

    <span data-ttu-id="f8ccb-231">*Erstellen eine Klassendatei, um den Code für die Dienstebene der Kontakt-Repository enthält*</span><span class="sxs-lookup"><span data-stu-id="f8ccb-231">*Creating a class file to contain the code for the Contact Repository service layer*</span></span>
4. <span data-ttu-id="f8ccb-232">Hinzufügen einer using-Direktive hinzu der **ContactRepository.cs** Datei, die den Modellen-Namespace enthalten.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-232">Add a using directive to the **ContactRepository.cs** file to include the models namespace.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample4.cs)]
5. <span data-ttu-id="f8ccb-233">Fügen Sie den folgenden hervorgehobenen Code in die **ContactRepository.cs** Datei GetAllContacts-Methode implementieren.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-233">Add the following highlighted code to the **ContactRepository.cs** file to implement GetAllContacts method.</span></span>

    <span data-ttu-id="f8ccb-234">(Codeausschnitt - *Web-API-Lab - Ex01 - Kontaktrepository*)</span><span class="sxs-lookup"><span data-stu-id="f8ccb-234">(Code Snippet - *Web API Lab - Ex01 - Contact Repository*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample5.cs)]
6. <span data-ttu-id="f8ccb-235">Öffnen Sie die **ContactController.cs** Datei, wenn es nicht bereits geöffnet ist.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-235">Open the **ContactController.cs** file if it is not already open.</span></span>
7. <span data-ttu-id="f8ccb-236">Fügen Sie die folgenden using-Anweisung mit dem Namespace-Deklaration-Abschnitt der Datei.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-236">Add the following using statement to the namespace declaration section of the file.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample6.cs)]
8. <span data-ttu-id="f8ccb-237">Fügen Sie den folgenden hervorgehobenen Code in die **ContactController.cs** Klasse, um ein privates Feld für die Darstellung der Instanz des Repositorys, hinzufügen, damit der Rest der Klasse, die Mitglieder ausführen können der dienstimplementierung.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-237">Add the following highlighted code to the **ContactController.cs** class to add a private field to represent the instance of the repository, so that the rest of the class members can make use of the service implementation.</span></span>

    <span data-ttu-id="f8ccb-238">(Codeausschnitt - *Web-API-Lab - Ex01 - Contact-Controller*)</span><span class="sxs-lookup"><span data-stu-id="f8ccb-238">(Code Snippet - *Web API Lab - Ex01 - Contact Controller*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample7.cs)]
9. <span data-ttu-id="f8ccb-239">Ändern der **erhalten** Methode, sodass die It stellt das Kontaktrepository-Dienst verwenden.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-239">Change the **Get** method so that it makes use of the contact repository service.</span></span>

    <span data-ttu-id="f8ccb-240">(Codeausschnitt - *Web-API-Lab - Ex01 - Zurückgeben einer Liste von Kontakten über das Repository*)</span><span class="sxs-lookup"><span data-stu-id="f8ccb-240">(Code Snippet - *Web API Lab - Ex01 - Returning a list of contacts via the repository*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample8.cs)]
10. <span data-ttu-id="f8ccb-241">Fügen Sie einen Haltepunkt auf der **ContactController**des **erhalten** Methodendefinition.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-241">Put a breakpoint on the **ContactController**'s **Get** method definition.</span></span>

   <span data-ttu-id="f8ccb-242">![Haltepunkte hinzufügen, um die Contact-Controller](build-restful-apis-with-aspnet-web-api/_static/image17.png "Haltepunkte auf der Contact-Controller hinzufügen")</span><span class="sxs-lookup"><span data-stu-id="f8ccb-242">![Adding breakpoints to the contact controller](build-restful-apis-with-aspnet-web-api/_static/image17.png "Adding breakpoints to the contact controller")</span></span>

   <span data-ttu-id="f8ccb-243">*Haltepunkte auf der Contact-Controller hinzufügen*</span><span class="sxs-lookup"><span data-stu-id="f8ccb-243">*Adding breakpoints to the contact controller*</span></span>
11. <span data-ttu-id="f8ccb-244">Drücken Sie **F5**, um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-244">Press **F5** to run the application.</span></span>
12. <span data-ttu-id="f8ccb-245">Wenn der Browser geöffnet wird, drücken Sie die **F12** zu den Entwicklertools zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-245">When the browser opens, press **F12** to open the developer tools.</span></span>
13. <span data-ttu-id="f8ccb-246">Klicken Sie auf die **Netzwerk** Registerkarte.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-246">Click the **Network** tab.</span></span>
14. <span data-ttu-id="f8ccb-247">Klicken Sie auf die **erfassen starten** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-247">Click the **Start Capturing** button.</span></span>
15. <span data-ttu-id="f8ccb-248">Fügen Sie die URL in die Adressleiste ein, mit dem Suffix **/api/Contact** , und drücken Sie **EINGABETASTE** API-Controller zu laden.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-248">Append the URL in the address bar with the suffix **/api/contact** and press **Enter** to load the API controller.</span></span>
16. <span data-ttu-id="f8ccb-249">Visual Studio 2012 sollte ein Halt **erhalten** Methode mit der Ausführung beginnt.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-249">Visual Studio 2012 should break once **Get** method begins execution.</span></span>

   <span data-ttu-id="f8ccb-250">![In der Get-Methode wichtige](build-restful-apis-with-aspnet-web-api/_static/image18.png "wichtige in der Get-Methode")</span><span class="sxs-lookup"><span data-stu-id="f8ccb-250">![Breaking within the Get method](build-restful-apis-with-aspnet-web-api/_static/image18.png "Breaking within the Get method")</span></span>

   <span data-ttu-id="f8ccb-251">*Wichtige in der Get-Methode*</span><span class="sxs-lookup"><span data-stu-id="f8ccb-251">*Breaking within the Get method*</span></span>
17. <span data-ttu-id="f8ccb-252">Drücken Sie **F5**, um fortzufahren.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-252">Press **F5** to continue.</span></span>
18. <span data-ttu-id="f8ccb-253">Wechseln Sie zurück zu Internet Explorer, wenn sie noch nicht im Fokus ist.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-253">Go back to Internet Explorer if it is not already in focus.</span></span> <span data-ttu-id="f8ccb-254">Beachten Sie das Netzwerk erfassen-Fenster.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-254">Note the network capture window.</span></span>

    <span data-ttu-id="f8ccb-255">![Netzwerk-Ansicht in Internet Explorer zeigt die Ergebnisse des Web-API-Aufrufs](build-restful-apis-with-aspnet-web-api/_static/image19.png "Netzwerk anzeigen in Internet Explorer, die Ergebnisse des Web-API-Aufrufs angezeigt.")</span><span class="sxs-lookup"><span data-stu-id="f8ccb-255">![Network view in Internet Explorer showing results of the Web API call](build-restful-apis-with-aspnet-web-api/_static/image19.png "Network view in Internet Explorer showing results of the Web API call")</span></span>

    <span data-ttu-id="f8ccb-256">*Netzwerkansicht in Internet Explorer, die Ergebnisse des Web-API-Aufrufs angezeigt.*</span><span class="sxs-lookup"><span data-stu-id="f8ccb-256">*Network view in Internet Explorer showing results of the Web API call*</span></span>
19. <span data-ttu-id="f8ccb-257">Klicken Sie auf die **wechseln Sie zur detaillierten Ansicht** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-257">Click the **Go to detailed view** button.</span></span>
20. <span data-ttu-id="f8ccb-258">Klicken Sie auf die **Antworttext** Registerkarte. Beachten Sie die JSON-Ausgabe von den API-Aufruf, und wie sie die zwei Kontakte abgerufen, indem die Dienstschicht darstellt.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-258">Click the **Response body** tab. Note the JSON output of the API call, and how it represents the two contacts retrieved by the service layer.</span></span>

    <span data-ttu-id="f8ccb-259">![Anzeigen von der JSON-Ausgabe aus der Web-API im Fenster mit Entwicklertools](build-restful-apis-with-aspnet-web-api/_static/image20.png "Anzeigen der JSON-Ausgabe aus der Web-API im Fenster mit Entwicklertools")</span><span class="sxs-lookup"><span data-stu-id="f8ccb-259">![Viewing the JSON output from the Web API in the developer tools window](build-restful-apis-with-aspnet-web-api/_static/image20.png "Viewing the JSON output from the Web API in the developer tools window")</span></span>

    <span data-ttu-id="f8ccb-260">*Anzeigen von der JSON-Ausgabe aus der Web-API im Fenster mit Entwicklertools*</span><span class="sxs-lookup"><span data-stu-id="f8ccb-260">*Viewing the JSON output from the Web API in the developer tools window*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Create_a_ReadWrite_Web_API"></a>
### <a name="exercise-2-create-a-readwrite-web-api"></a><span data-ttu-id="f8ccb-261">Übung 2: Erstellen einer Lese-/Schreibzugriff-Webs-API</span><span class="sxs-lookup"><span data-stu-id="f8ccb-261">Exercise 2: Create a Read/Write Web API</span></span>

<span data-ttu-id="f8ccb-262">In dieser Übung implementieren Sie die POST und PUT-Methoden für die der Kontakt-Manager, um sie mit Features Bearbeiten von Daten zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-262">In this exercise, you will implement POST and PUT methods for the contact manager to enable it with data-editing features.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Opening_the_Web_API_Project"></a>
#### <a name="task-1---opening-the-web-api-project"></a><span data-ttu-id="f8ccb-263">Aufgabe 1: Öffnen die Web-API-Projekt</span><span class="sxs-lookup"><span data-stu-id="f8ccb-263">Task 1 - Opening the Web API Project</span></span>

<span data-ttu-id="f8ccb-264">In dieser Aufgabe bereiten Sie zur Verbesserung der Web-API-Projekts in Übung 1 erstellt werden, sodass sie Benutzereingaben akzeptieren kann.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-264">In this task, you will prepare to enhance the Web API project created in Exercise 1 so that it can accept user input.</span></span>

1. <span data-ttu-id="f8ccb-265">Führen Sie **Visual Studio-2012 Express für Web**, Sie gehen Sie dazu zur **starten** , und geben **Visual Studio Express für Web** drücken Sie dann die **EINGABETASTE**.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-265">Run **Visual Studio 2012 Express for Web**, to do this go to **Start** and type **VS Express for Web** then press **Enter**.</span></span>
2. <span data-ttu-id="f8ccb-266">Öffnen der **beginnen** Lösung controllerarbeitsverzeichnis **Quelle/Ex02-ReadWriteWebAPI/Anfang/** Ordner.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-266">Open the **Begin** solution located at **Source/Ex02-ReadWriteWebAPI/Begin/** folder.</span></span> <span data-ttu-id="f8ccb-267">Andernfalls können Sie weiterhin, verwenden die **End** Lösung abgerufen wird, indem Sie der vorherige Übung abschließen können.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-267">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="f8ccb-268">Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung, Sie müssen einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-268">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="f8ccb-269">Zu diesem Zweck klicken Sie auf die **Projekt** Menü **NuGet-Pakete verwalten**.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-269">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="f8ccb-270">In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-270">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="f8ccb-271">Abschließend erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-271">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="f8ccb-272">Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken in Ihrem Projekt, Versand Verringern der Projektgröße.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-272">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="f8ccb-273">Mit NuGet Power Tools können werden durch Angabe von Versionen des Pakets in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, die, das Sie das Projekt ausführen, können.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-273">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="f8ccb-274">Deshalb müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit wird.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-274">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="f8ccb-275">Öffnen der **Services/ContactRepository.cs** Datei.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-275">Open the **Services/ContactRepository.cs** file.</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_Adding_Data-Persistence_Features_to_the_Contact_Repository_Implementation"></a>
#### <a name="task-2---adding-data-persistence-features-to-the-contact-repository-implementation"></a><span data-ttu-id="f8ccb-276">Aufgabe 2: Hinzufügen von Datenpersistenz Features für die Implementierung Kontaktrepository</span><span class="sxs-lookup"><span data-stu-id="f8ccb-276">Task 2 - Adding Data-Persistence Features to the Contact Repository Implementation</span></span>

<span data-ttu-id="f8ccb-277">In dieser Aufgabe werden Sie die ContactRepository-Klasse des Web-API-Projekts, damit es beibehalten und akzeptieren von Benutzereingaben und neuer Kontakt-Instanzen kann die in Übung 1 erstellten erweitern.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-277">In this task, you will augment the ContactRepository class of the Web API project created in Exercise 1 so that it can persist and accept user input and new Contact instances.</span></span>

1. <span data-ttu-id="f8ccb-278">Fügen Sie die folgende Konstante, die die **ContactRepository** Klasse, um den Namen des Cache Element Schlüssel den Namen des Webservers später in dieser Übung darzustellen.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-278">Add the following constant to the **ContactRepository** class to represent the name of the web server cache item key name later in this exercise.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample9.cs)]
2. <span data-ttu-id="f8ccb-279">Fügen Sie einen Konstruktor, der **ContactRepository** mit dem folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-279">Add a constructor to the **ContactRepository** containing the following code.</span></span>

    <span data-ttu-id="f8ccb-280">(Codeausschnitt - *Web-API-Lab - Ex02 - Kontaktrepository Konstruktor*)</span><span class="sxs-lookup"><span data-stu-id="f8ccb-280">(Code Snippet - *Web API Lab - Ex02 - Contact Repository Constructor*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample10.cs)]
3. <span data-ttu-id="f8ccb-281">Ändern Sie den Code für die **GetAllContacts** Methode wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-281">Modify the code for the **GetAllContacts** method as demonstrated below.</span></span>

    <span data-ttu-id="f8ccb-282">(Codeausschnitt - *Web-API-Lab - Ex02 - Get All Contacts*)</span><span class="sxs-lookup"><span data-stu-id="f8ccb-282">(Code Snippet - *Web API Lab - Ex02 - Get All Contacts*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample11.cs)]

    > [!NOTE]
    > <span data-ttu-id="f8ccb-283">In diesem Beispiel dient zu Demonstrationszwecken und verwendet die Webserver Cache als einem Speichermedium, damit die Werte werden verfügbar sein, um mehrere Clients gleichzeitig, anstatt einen Speichermechanismus für die Sitzung oder eine Anforderung Storage Lebensdauer verwenden.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-283">This example is for demonstration purposes and will use the web server's cache as a storage medium, so that the values will be available to multiple clients simultaneously, rather than use a Session storage mechanism or a Request storage lifetime.</span></span> <span data-ttu-id="f8ccb-284">Eine kann Entity Framework, XML-Speicherung oder jeder anderen Sorte anstelle der Web-Server-Cache verwenden.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-284">One could use Entity Framework, XML storage, or any other variety in place of the web server cache.</span></span>
4. <span data-ttu-id="f8ccb-285">Implementieren Sie eine neue Methode namens **SaveContact** auf die **ContactRepository** Klasse, um die Arbeit ein Kontakts zu speichern.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-285">Implement a new method named **SaveContact** to the **ContactRepository** class to do the work of saving a contact.</span></span> <span data-ttu-id="f8ccb-286">Die **SaveContact** Methode dauert ein einzelnes **wenden Sie sich an** Parameter und Rückgabetypen ein boolescher Wert, der angibt, Erfolg oder Fehler.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-286">The **SaveContact** method should take a single **Contact** parameter and return a Boolean value indicating success or failure.</span></span>

    <span data-ttu-id="f8ccb-287">(Codeausschnitt - *Web-API-Lab - Ex02 - Implementierung der Methode SaveContact*)</span><span class="sxs-lookup"><span data-stu-id="f8ccb-287">(Code Snippet - *Web API Lab - Ex02 - Implementing the SaveContact Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample12.cs)]

<a id="Exercise3"></a>

<a id="Exercise_3_Consume_the_Web_API_from_an_HTML_Client"></a>
### <a name="exercise-3-consume-the-web-api-from-an-html-client"></a><span data-ttu-id="f8ccb-288">Übung 3: Nutzen Sie die Web-API aus einem HTML-Client</span><span class="sxs-lookup"><span data-stu-id="f8ccb-288">Exercise 3: Consume the Web API from an HTML Client</span></span>

<span data-ttu-id="f8ccb-289">In dieser Übung erstellen Sie einen HTML-Client zum Aufrufen der Web-API.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-289">In this exercise, you will create an HTML client to call the Web API.</span></span> <span data-ttu-id="f8ccb-290">Dieser Client wird zu den Datenaustausch mit der Web-API mit JavaScript vereinfachen und die Ergebnisse in einem Webbrowser mit HTML-Markup angezeigt.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-290">This client will facilitate data exchange with the Web API using JavaScript and will display the results in a web browser using HTML markup.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Displaying_Contacts"></a>
#### <a name="task-1---modifying-the-index-view-to-provide-a-gui-for-displaying-contacts"></a><span data-ttu-id="f8ccb-291">Aufgabe 1: Ändern der Ansicht "Index", um eine grafische Benutzeroberfläche zu bieten, für die Anzeige von Kontakten</span><span class="sxs-lookup"><span data-stu-id="f8ccb-291">Task 1 - Modifying the Index View to Provide a GUI for Displaying Contacts</span></span>

<span data-ttu-id="f8ccb-292">In dieser Aufgabe ändern Sie die Standardansicht der Index der Webanwendung zur Unterstützung der Anforderung die Liste der vorhandenen Kontakte in einem HTML-Browser anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-292">In this task, you will modify the default Index view of the web application to support the requirement of displaying the list of existing contacts in an HTML browser.</span></span>

1. <span data-ttu-id="f8ccb-293">Öffnen Sie **Visual Studio-2012 Express für Web** , wenn es nicht bereits geöffnet ist.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-293">Open **Visual Studio 2012 Express for Web** if it is not already open.</span></span>
2. <span data-ttu-id="f8ccb-294">Öffnen der **beginnen** Lösung controllerarbeitsverzeichnis **Quelle/Ex03-ConsumingWebAPI/Anfang/** Ordner.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-294">Open the **Begin** solution located at **Source/Ex03-ConsumingWebAPI/Begin/** folder.</span></span> <span data-ttu-id="f8ccb-295">Andernfalls können Sie weiterhin, verwenden die **End** Lösung abgerufen wird, indem Sie der vorherige Übung abschließen können.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-295">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="f8ccb-296">Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung, Sie müssen einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-296">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="f8ccb-297">Zu diesem Zweck klicken Sie auf die **Projekt** Menü **NuGet-Pakete verwalten**.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-297">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="f8ccb-298">In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-298">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="f8ccb-299">Abschließend erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-299">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="f8ccb-300">Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken in Ihrem Projekt, Versand Verringern der Projektgröße.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-300">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="f8ccb-301">Mit NuGet Power Tools können werden durch Angabe von Versionen des Pakets in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, die, das Sie das Projekt ausführen, können.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-301">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="f8ccb-302">Deshalb müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit wird.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-302">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="f8ccb-303">Öffnen der **"Index.cshtml"** Datei **Views/Home** Ordner.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-303">Open the **Index.cshtml** file located at **Views/Home** folder.</span></span>
4. <span data-ttu-id="f8ccb-304">Ersetzen Sie den HTML-Code in das Div-Element mit der Id **Text** , damit sie sich wie der folgende Code aussieht.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-304">Replace the HTML code within the div element with id **body** so that it looks like the following code.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample13.html)]
5. <span data-ttu-id="f8ccb-305">Fügen Sie den folgenden Javascript-Code am Ende der Datei zum Ausführen der HTTP-Anforderung an die Web-API.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-305">Add the following Javascript code at the bottom of the file to perform the HTTP request to the Web API.</span></span>

    [!code-cshtml[Main](build-restful-apis-with-aspnet-web-api/samples/sample14.cshtml)]
6. <span data-ttu-id="f8ccb-306">Öffnen Sie die **ContactController.cs** Datei, wenn es nicht bereits geöffnet ist.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-306">Open the **ContactController.cs** file if it is not already open.</span></span>
7. <span data-ttu-id="f8ccb-307">Fügen Sie einen Haltepunkt auf der **erhalten** Methode der **ContactController** Klasse.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-307">Place a breakpoint on the **Get** method of the **ContactController** class.</span></span>

    <span data-ttu-id="f8ccb-308">![Platzieren einen Haltepunkt für die Get-Methode der API-Controller](build-restful-apis-with-aspnet-web-api/_static/image21.png "einen Haltepunkt für die Get-Methode der API-Controller einfügen")</span><span class="sxs-lookup"><span data-stu-id="f8ccb-308">![Placing a breakpoint on the Get method of the API controller](build-restful-apis-with-aspnet-web-api/_static/image21.png "Placing a breakpoint on the Get method of the API controller")</span></span>

    <span data-ttu-id="f8ccb-309">*Platzieren einen Haltepunkt für die Get-Methode der API-controller*</span><span class="sxs-lookup"><span data-stu-id="f8ccb-309">*Placing a breakpoint on the Get method of the API controller*</span></span>
8. <span data-ttu-id="f8ccb-310">Drücken Sie **F5** um das Projekt auszuführen.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-310">Press **F5** to run the project.</span></span> <span data-ttu-id="f8ccb-311">Der Browser lädt das HTML-Dokument.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-311">The browser will load the HTML document.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f8ccb-312">Stellen Sie sicher, dass Sie auf die Stamm-URL Ihrer Anwendung navigieren.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-312">Ensure that you are browsing to the root URL of your application.</span></span>
9. <span data-ttu-id="f8ccb-313">Sobald die Seite geladen wird, und der JavaScript-Code ausgeführt wird, wird der Haltepunkt erreicht, und die Ausführung von Code im Controller hält.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-313">Once the page loads and the JavaScript executes, the breakpoint will be hit and the code execution will pause in the controller.</span></span>

    <span data-ttu-id="f8ccb-314">![Debuggen in der Web-API-Aufrufe, die mithilfe von Visual Studio Express für Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "Debuggen in der Web-API-Aufrufe, die mithilfe von Visual Studio Express für Web")</span><span class="sxs-lookup"><span data-stu-id="f8ccb-314">![Debugging into the Web API calls using VS Express for Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "Debugging into the Web API calls using VS Express for Web")</span></span>

    <span data-ttu-id="f8ccb-315">*Debuggen in der Web-API-Aufruf mithilfe von Visual Studio 2012 Express für Web*</span><span class="sxs-lookup"><span data-stu-id="f8ccb-315">*Debugging into the Web API call using Visual Studio 2012 Express for Web*</span></span>
10. <span data-ttu-id="f8ccb-316">Entfernen Sie den Haltepunkt, und drücken Sie **F5** oder der debugging-Symbolleiste **Weiter** Schaltfläche, um den Vorgang fortzusetzen, laden die Ansicht im Browser.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-316">Remove the breakpoint and press **F5** or the debugging toolbar's **Continue** button to continue loading the view in the browser.</span></span> <span data-ttu-id="f8ccb-317">Nach Abschluss der Web-API-Aufruf sollte im Browser dargestellt als von Listenelementen rufen Sie aus der Web-API zurückgegebenen Kontakte angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-317">Once the Web API call completes you should see the contacts returned from the Web API call displayed as list items in the browser.</span></span>

    <span data-ttu-id="f8ccb-318">![Ergebnisse von den API-Aufruf im Browser angezeigt wird, als Listenelemente](build-restful-apis-with-aspnet-web-api/_static/image23.png "Ergebnisse von den API-Aufruf im Browser angezeigt wird, als Listenelemente")</span><span class="sxs-lookup"><span data-stu-id="f8ccb-318">![Results of the API call displayed in the browser as list items](build-restful-apis-with-aspnet-web-api/_static/image23.png "Results of the API call displayed in the browser as list items")</span></span>

    <span data-ttu-id="f8ccb-319">*Ergebnisse von den API-Aufruf im Browser angezeigt wird, als Listenelemente*</span><span class="sxs-lookup"><span data-stu-id="f8ccb-319">*Results of the API call displayed in the browser as list items*</span></span>
11. <span data-ttu-id="f8ccb-320">Beenden des Debuggens.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-320">Stop debugging.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Creating_Contacts"></a>
#### <a name="task-2---modifying-the-index-view-to-provide-a-gui-for-creating-contacts"></a><span data-ttu-id="f8ccb-321">Aufgabe 2: Ändern der Ansicht "Index", um eine grafische Benutzeroberfläche für die Erstellung von Contacts bereitzustellen</span><span class="sxs-lookup"><span data-stu-id="f8ccb-321">Task 2 - Modifying the Index View to Provide a GUI for Creating Contacts</span></span>

<span data-ttu-id="f8ccb-322">In dieser Aufgabe werden Sie weiterhin so ändern Sie die Ansicht "Index", von der MVC-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-322">In this task, you will continue to modify the Index view of the MVC application.</span></span> <span data-ttu-id="f8ccb-323">Die HTML-Seite, die Erfassen von Benutzereingaben und senden Sie sie an die Web-API zum Erstellen eines neuen Kontakts wird ein Formular hinzugefügt werden, und eine neue Web-API-Controller-Methode erstellt wird, um das Datum über die grafische Benutzeroberfläche zu sammeln.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-323">A form will be added to the HTML page that will capture user input and send it to the Web API to create a new Contact, and a new Web API controller method will be created to collect date from the GUI.</span></span>

1. <span data-ttu-id="f8ccb-324">Öffnen der **ContactController.cs** Datei.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-324">Open the **ContactController.cs** file.</span></span>
2. <span data-ttu-id="f8ccb-325">Fügen Sie eine neue Methode für die Controllerklasse, die mit dem Namen **Post** wie im folgenden Code gezeigt.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-325">Add a new method to the controller class named **Post** as shown in the following code.</span></span>

    <span data-ttu-id="f8ccb-326">(Codeausschnitt - *Web-API-Lab - Ex03 - Post-Methode*)</span><span class="sxs-lookup"><span data-stu-id="f8ccb-326">(Code Snippet - *Web API Lab - Ex03 - Post Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample15.cs)]
3. <span data-ttu-id="f8ccb-327">Öffnen Sie die **"Index.cshtml"** Datei in Visual Studio, wenn es nicht bereits geöffnet ist.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-327">Open the **Index.cshtml** file in Visual Studio if it is not already open.</span></span>
4. <span data-ttu-id="f8ccb-328">Fügen Sie den folgenden HTML-Code in die Datei direkt nach der unsortierten Liste, die Sie in der vorherigen Aufgabe hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-328">Add the HTML code below to the file just after the unordered list you added in the previous task.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample16.html)]
5. <span data-ttu-id="f8ccb-329">Fügen Sie in der Script-Element, am unteren Rand des Dokuments werden soll folgenden hervorgehobenen Code zum Behandeln von Schaltfläche – Click-Ereignisse, die die Daten an die Web-API sendet, über eine HTTP POST-Aufruf hinzu.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-329">Within the script element at the bottom of the document, add the following highlighted code to handle button-click events, which will post the data to the Web API using an HTTP POST call.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample17.html)]
6. <span data-ttu-id="f8ccb-330">In **ContactController.cs**, platzieren Sie einen Haltepunkt auf der **Post** Methode.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-330">In **ContactController.cs**, place a breakpoint on the **Post** method.</span></span>
7. <span data-ttu-id="f8ccb-331">Drücken Sie **F5** zum Ausführen der Anwendung im Browser.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-331">Press **F5** to run the application in the browser.</span></span>
8. <span data-ttu-id="f8ccb-332">Sobald die Seite im Browser geladen wird, geben Sie einen neuen Kontaktnamen und -Id und klicken Sie auf die **speichern** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-332">Once the page is loaded in the browser, type in a new contact name and Id and click the **Save** button.</span></span>

    <span data-ttu-id="f8ccb-333">![Das Client-HTML-Dokument geladen wird, im Browser](build-restful-apis-with-aspnet-web-api/_static/image24.png "das Client-HTML-Dokument geladen wird, im Browser")</span><span class="sxs-lookup"><span data-stu-id="f8ccb-333">![The client HTML document loaded in the browser](build-restful-apis-with-aspnet-web-api/_static/image24.png "The client HTML document loaded in the browser")</span></span>

    <span data-ttu-id="f8ccb-334">*Das Client-HTML-Dokument im Browser geladen.*</span><span class="sxs-lookup"><span data-stu-id="f8ccb-334">*The client HTML document loaded in the browser*</span></span>
9. <span data-ttu-id="f8ccb-335">Wenn das Debuggerfenster Zeilenumbrüche, in der **Post** -Methode, verschaffen Sie sich einen Blick auf die Eigenschaften des der **wenden Sie sich an** Parameter.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-335">When the debugger window breaks in the **Post** method, take a look at the properties of the **contact** parameter.</span></span> <span data-ttu-id="f8ccb-336">Die Werte sollten mit den Daten übereinstimmen, die Sie in das Formular eingegeben haben.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-336">The values should match the data you entered in the form.</span></span>

    <span data-ttu-id="f8ccb-337">![Der Contact-Objekt, das an die Web-API vom Client gesendeten](build-restful-apis-with-aspnet-web-api/_static/image25.png "der Contact-Objekt, an die Web-API vom Client gesendeten")</span><span class="sxs-lookup"><span data-stu-id="f8ccb-337">![The Contact object being sent to the Web API from the client](build-restful-apis-with-aspnet-web-api/_static/image25.png "The Contact object being sent to the Web API from the client")</span></span>

    <span data-ttu-id="f8ccb-338">*Der Contact-Objekt, das an die Web-API vom Client gesendeten*</span><span class="sxs-lookup"><span data-stu-id="f8ccb-338">*The Contact object being sent to the Web API from the client*</span></span>
10. <span data-ttu-id="f8ccb-339">Durchlaufen Sie die Methode im Debugger bis der **Antwort** Variable erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-339">Step through the method in the debugger until the **response** variable has been created.</span></span> <span data-ttu-id="f8ccb-340">Bei näherer Untersuchung in der **"lokal"** Fenster im Debugger, werden Sie feststellen, dass alle Eigenschaften festgelegt wurden.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-340">Upon inspection in the **Locals** window in the debugger, you'll see that all the properties have been set.</span></span>

   <span data-ttu-id="f8ccb-341">![Die Antwort nach der Erstellung im Debugger](build-restful-apis-with-aspnet-web-api/_static/image26.png "die Antwort nach der Erstellung im Debugger")</span><span class="sxs-lookup"><span data-stu-id="f8ccb-341">![The response following creation in the debugger](build-restful-apis-with-aspnet-web-api/_static/image26.png "The response following creation in the debugger")</span></span>

   <span data-ttu-id="f8ccb-342">*Die Antwort nach der Erstellung im debugger*</span><span class="sxs-lookup"><span data-stu-id="f8ccb-342">*The response following creation in the debugger*</span></span>
11. <span data-ttu-id="f8ccb-343">Wenn Sie drücken **F5** oder klicken Sie auf **Weiter** im Debugger die Anforderung abgeschlossen wird.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-343">If you press **F5** or click **Continue** in the debugger the request will complete.</span></span> <span data-ttu-id="f8ccb-344">Nachdem Sie sich an den Browser wechseln, der neue Kontakt hinzugefügt wurde die Liste der Kontakte, die von gespeicherten der **ContactRepository** Implementierung.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-344">Once you switch back to the browser, the new contact has been added to the list of contacts stored by the **ContactRepository** implementation.</span></span>

   <span data-ttu-id="f8ccb-345">![Der Browser gibt erfolgreichen Erstellung der neuen Instanz wenden Sie sich an](build-restful-apis-with-aspnet-web-api/_static/image27.png "Browser gibt erfolgreichen Erstellung der neuen Instanz wenden Sie sich an")</span><span class="sxs-lookup"><span data-stu-id="f8ccb-345">![The browser reflects successful creation of the new contact instance](build-restful-apis-with-aspnet-web-api/_static/image27.png "The browser reflects successful creation of the new contact instance")</span></span>

   <span data-ttu-id="f8ccb-346">*Der Browser gibt erfolgreichen Erstellung der neuen Instanz wenden Sie sich an*</span><span class="sxs-lookup"><span data-stu-id="f8ccb-346">*The browser reflects successful creation of the new contact instance*</span></span>

> [!NOTE]
> <span data-ttu-id="f8ccb-347">Darüber hinaus können Sie diese Anwendung in Azure Folgendes bereitstellen [Anhang C: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web Deploy](#AppendixC).</span><span class="sxs-lookup"><span data-stu-id="f8ccb-347">Additionally, you can deploy this application to Azure following [Appendix C: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixC).</span></span>


* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="f8ccb-348">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="f8ccb-348">Summary</span></span>

<span data-ttu-id="f8ccb-349">Dieses Lab wurde das neue ASP.NET Web-API-Framework und die Implementierung von RESTful Web-APIs, die mit dem Framework eingeführt.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-349">This lab has introduced you to the new ASP.NET Web API framework and to the implementation of RESTful Web APIs using the framework.</span></span> <span data-ttu-id="f8ccb-350">Von hier aus können Sie ein neues Repository, das Dauerhaftigkeit von Daten mit einer Reihe verschiedener Mechanismen erleichtert erstellen und socialsharing.Share dieses Diensts anstelle der einfachen in dieser Übungseinheit als Beispiel bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-350">From here, you could create a new repository that facilitates data persistence using any number of mechanisms and wire that service up rather than the simple one provided as an example in this lab.</span></span> <span data-ttu-id="f8ccb-351">Web-API unterstützt zahlreiche zusätzliche Funktionen, z. B. das Aktivieren der Kommunikation mit nicht-HTML-Clients, die in jeder beliebigen Sprache, die von HTTP und JSON oder XML unterstützt.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-351">Web API supports a number of additional features, such as enabling communication from non-HTML clients written in any language that supports HTTP and JSON or XML.</span></span> <span data-ttu-id="f8ccb-352">Die Möglichkeit zum Hosten einer Web-API außerhalb einer typischen Webanwendung ist auch möglich, als auch ist die Möglichkeit, Ihre eigenen Serialisierungsformate zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-352">The ability to host a Web API outside of a typical web application is also possible, as well as is the ability to create your own serialization formats.</span></span>

<span data-ttu-id="f8ccb-353">Die ASP.NET-Website verfügt über einen speziellen Bereich für die ASP.NET Web-API-Framework unter [ [ https://asp.net/web-api ](https://asp.net/web-api) ](https://asp.net/web-api).</span><span class="sxs-lookup"><span data-stu-id="f8ccb-353">The ASP.NET Web site has an area dedicated to the ASP.NET Web API framework at [[https://asp.net/web-api](https://asp.net/web-api)](https://asp.net/web-api).</span></span> <span data-ttu-id="f8ccb-354">Dieser Standort wird weiterhin bieten aktuelle Informationen, Beispiele und Nachrichten, die im Zusammenhang mit der Web-API so überprüfen Sie sie häufig, wenn Sie tiefer in die Kunst gerne der benutzerdefinierten Web-APIs auf praktisch jedem Gerät oder Entwicklung Framework erstellen.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-354">This site will continue to provide late-breaking information, samples, and news related to Web API, so check it frequently if you'd like to delve deeper into the art of creating custom Web APIs available to virtually any device or development framework.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a><span data-ttu-id="f8ccb-355">Anhang A: Verwenden von Codeausschnitten</span><span class="sxs-lookup"><span data-stu-id="f8ccb-355">Appendix A: Using Code Snippets</span></span>

<span data-ttu-id="f8ccb-356">Mit Codeausschnitten müssen Sie den Code zur Hand benötigten.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-356">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="f8ccb-357">Das Lab-Dokument informiert Sie genau wann sie verwendet werden kann wie in der folgenden Abbildung dargestellt.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-357">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="f8ccb-358">![Verwenden von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihrem Projekt](build-restful-apis-with-aspnet-web-api/_static/image28.png "mithilfe von Visual Studio-Codeausschnitten, die Code in das Projekt einfügen.")</span><span class="sxs-lookup"><span data-stu-id="f8ccb-358">![Using Visual Studio code snippets to insert code into your project](build-restful-apis-with-aspnet-web-api/_static/image28.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="f8ccb-359">*Verwenden von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihrem Projekt*</span><span class="sxs-lookup"><span data-stu-id="f8ccb-359">*Using Visual Studio code snippets to insert code into your project*</span></span>

<a id="CodeSnippetUsingKeyBoard"></a>

<a id="To_add_a_code_snippet_using_the_keyboard_C_only"></a>
### <a name="to-add-a-code-snippet-using-the-keyboard-c-only"></a><span data-ttu-id="f8ccb-360">Hinzufügen ein Codeausschnitts, die über die Tastatur (nur c#)</span><span class="sxs-lookup"><span data-stu-id="f8ccb-360">To add a code snippet using the keyboard (C# only)</span></span>

1. <span data-ttu-id="f8ccb-361">Platzieren Sie den Cursor, wo Sie möchten den Code einfügen.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-361">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="f8ccb-362">Starten Sie den codeausschnittsnamen (ohne Leerzeichen oder Bindestriche) eingeben.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-362">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="f8ccb-363">Beobachten Sie, wie IntelliSense zeigt übereinstimmende Codeausschnitte Namen.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-363">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="f8ccb-364">Wählen Sie den richtigen Codeausschnitt (oder halten Sie eingeben, bis die gesamte codeausschnittsnamen ausgewählt ist).</span><span class="sxs-lookup"><span data-stu-id="f8ccb-364">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="f8ccb-365">Drücken Sie die Tab-Taste zweimal auf Einfügen des Codeausschnitts an der Cursorposition ein.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-365">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

    <span data-ttu-id="f8ccb-366">![Geben Sie den Namen des Ausschnitts](build-restful-apis-with-aspnet-web-api/_static/image29.png "Geben Sie den Namen des Ausschnitts")</span><span class="sxs-lookup"><span data-stu-id="f8ccb-366">![Start typing the snippet name](build-restful-apis-with-aspnet-web-api/_static/image29.png "Start typing the snippet name")</span></span>

    <span data-ttu-id="f8ccb-367">*Geben Sie den Namen des Ausschnitts*</span><span class="sxs-lookup"><span data-stu-id="f8ccb-367">*Start typing the snippet name*</span></span>

    <span data-ttu-id="f8ccb-368">![Tabstopp drücken, um den hervorgehobenen Codeausschnitt auswählen](build-restful-apis-with-aspnet-web-api/_static/image30.png "Tab drücken, um den hervorgehobenen Codeausschnitt auswählen")</span><span class="sxs-lookup"><span data-stu-id="f8ccb-368">![Press Tab to select the highlighted snippet](build-restful-apis-with-aspnet-web-api/_static/image30.png "Press Tab to select the highlighted snippet")</span></span>

    <span data-ttu-id="f8ccb-369">*Drücken Sie Tabstopp, um den hervorgehobenen Codeausschnitt auswählen*</span><span class="sxs-lookup"><span data-stu-id="f8ccb-369">*Press Tab to select the highlighted snippet*</span></span>

    <span data-ttu-id="f8ccb-370">![Drücken Sie die Tabulatortaste erneut, und der Ausschnitt erweitert](build-restful-apis-with-aspnet-web-api/_static/image31.png "drücken Sie die Tabulatortaste erneut, und der Ausschnitt werden erweitert.")</span><span class="sxs-lookup"><span data-stu-id="f8ccb-370">![Press Tab again and the snippet will expand](build-restful-apis-with-aspnet-web-api/_static/image31.png "Press Tab again and the snippet will expand")</span></span>

    <span data-ttu-id="f8ccb-371">*Drücken Sie die Tabulatortaste erneut, und der Ausschnitt werden erweitert.*</span><span class="sxs-lookup"><span data-stu-id="f8ccb-371">*Press Tab again and the snippet will expand*</span></span>

<a id="CodeSnippetUsingMouse"></a>

<a id="To_add_a_code_snippet_using_the_mouse_C_Visual_Basic_and_XML"></a>
### <a name="to-add-a-code-snippet-using-the-mouse-c-visual-basic-and-xml"></a><span data-ttu-id="f8ccb-372">Hinzufügen ein Codeausschnitts, die mit der Maus (c#, Visual Basic und XML)</span><span class="sxs-lookup"><span data-stu-id="f8ccb-372">To add a code snippet using the mouse (C#, Visual Basic and XML)</span></span>

1. <span data-ttu-id="f8ccb-373">Mit der rechten Maustaste, in dem den Codeausschnitt eingefügt werden soll.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-373">Right-click where you want to insert the code snippet.</span></span>
2. <span data-ttu-id="f8ccb-374">Wählen Sie **Ausschnitt einfügen** gefolgt von **Meine Codeausschnitte**.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-374">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
3. <span data-ttu-id="f8ccb-375">Wählen Sie die relevante Codeausschnitte in der Liste, indem Sie darauf klicken.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-375">Pick the relevant snippet from the list, by clicking on it.</span></span>

    <span data-ttu-id="f8ccb-376">![Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten](build-restful-apis-with-aspnet-web-api/_static/image32.png "mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten,")</span><span class="sxs-lookup"><span data-stu-id="f8ccb-376">![Right-click where you want to insert the code snippet and select Insert Snippet](build-restful-apis-with-aspnet-web-api/_static/image32.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

    <span data-ttu-id="f8ccb-377">*Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten*</span><span class="sxs-lookup"><span data-stu-id="f8ccb-377">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

    <span data-ttu-id="f8ccb-378">![Wählen Sie die relevante Codeausschnitte in der Liste, indem Sie darauf klicken](build-restful-apis-with-aspnet-web-api/_static/image33.png "die relevante Codeausschnitte in der Liste auswählen, indem Sie darauf klicken")</span><span class="sxs-lookup"><span data-stu-id="f8ccb-378">![Pick the relevant snippet from the list, by clicking on it](build-restful-apis-with-aspnet-web-api/_static/image33.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

    <span data-ttu-id="f8ccb-379">*Wählen Sie die relevante Codeausschnitte in der Liste, indem Sie darauf klicken*</span><span class="sxs-lookup"><span data-stu-id="f8ccb-379">*Pick the relevant snippet from the list, by clicking on it*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="f8ccb-380">Anhang B: Installieren von Visual Studio Express 2012 für Web</span><span class="sxs-lookup"><span data-stu-id="f8ccb-380">Appendix B: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="f8ccb-381">Sie installieren können **Microsoft Visual Studio Express 2012 für Web** oder einem anderen &quot;Express&quot; Version verwenden, die **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-381">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="f8ccb-382">Die folgenden Anweisungen führen Sie über die erforderlichen Schritte zum Installieren *Visual Studio Express 2012 für Web* mit *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-382">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="f8ccb-383">Wechseln Sie zu [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="f8ccb-383">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="f8ccb-384">Auch wenn Sie bereits Webplattform-Installer installiert haben, können Sie öffnen ihn, und suchen Sie nach dem Produkt &quot; <em>Visual Studio Express 2012 für das Web mit Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-384">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="f8ccb-385">Klicken Sie auf **jetzt installieren**.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-385">Click on **Install Now**.</span></span> <span data-ttu-id="f8ccb-386">Wenn Sie keine **Webplattform-Installer** gelangen Sie zum Herunterladen und installieren Sie diese zuerst.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-386">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="f8ccb-387">Einmal **Webplattform-Installer** geöffnet ist, klicken Sie auf **installieren** um das Setup zu starten.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-387">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="f8ccb-388">![Installieren von Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "Installieren von Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="f8ccb-388">![Install Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="f8ccb-389">*Installieren von Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="f8ccb-389">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="f8ccb-390">Lesen Sie die Produkte Lizenzen und Begriffe, und klicken Sie auf **akzeptieren** um den Vorgang fortzusetzen.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-390">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Akzeptieren der Lizenzbedingungen](build-restful-apis-with-aspnet-web-api/_static/image35.png)

    <span data-ttu-id="f8ccb-392">*Akzeptieren der Lizenzbedingungen*</span><span class="sxs-lookup"><span data-stu-id="f8ccb-392">*Accepting the license terms*</span></span>
5. <span data-ttu-id="f8ccb-393">Warten Sie, bis der Prozess zum Herunterladen und die Installation abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-393">Wait until the downloading and installation process completes.</span></span>

    ![Installationsstatus](build-restful-apis-with-aspnet-web-api/_static/image36.png)

    <span data-ttu-id="f8ccb-395">*Installationsstatus*</span><span class="sxs-lookup"><span data-stu-id="f8ccb-395">*Installation progress*</span></span>
6. <span data-ttu-id="f8ccb-396">Wenn die Installation abgeschlossen ist, klicken Sie auf **Fertig stellen**.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-396">When the installation completes, click **Finish**.</span></span>

    ![Die Installation wurde abgeschlossen](build-restful-apis-with-aspnet-web-api/_static/image37.png)

    <span data-ttu-id="f8ccb-398">*Die Installation wurde abgeschlossen*</span><span class="sxs-lookup"><span data-stu-id="f8ccb-398">*Installation completed*</span></span>
7. <span data-ttu-id="f8ccb-399">Klicken Sie auf **beenden** Webplattform-Installer zu schließen.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-399">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="f8ccb-400">Um Visual Studio Express für Web zu öffnen, wechseln Sie zu der **starten** schreiben zu starten und Bildschirm &quot; **VS Express**&quot;, klicken Sie dann auf die **Visual Studio Express für Web** die Kachel.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-400">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Visual Studio Express für Web-Kachel](build-restful-apis-with-aspnet-web-api/_static/image38.png)

    <span data-ttu-id="f8ccb-402">*Visual Studio Express für Web-Kachel*</span><span class="sxs-lookup"><span data-stu-id="f8ccb-402">*VS Express for Web tile*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-c-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="f8ccb-403">Anhang C: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web Deploy</span><span class="sxs-lookup"><span data-stu-id="f8ccb-403">Appendix C: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="f8ccb-404">In diesem Anhang erfahren Sie, wie eine neue Website aus dem Azure-Portal erstellen und Veröffentlichen der Anwendung, die Sie erhalten haben, indem der testumgebung können die Nutzung der Web Deploy-publishing-Funktion von Azure bereitgestellten.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-404">This appendix will show you how to create a new web site from the Azure Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Azure.</span></span>

<a id="ApxCTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a><span data-ttu-id="f8ccb-405">Aufgabe 1: Erstellen einer neuen Website über das Azure-Portal</span><span class="sxs-lookup"><span data-stu-id="f8ccb-405">Task 1 - Creating a New Web Site from the Azure Portal</span></span>

1. <span data-ttu-id="f8ccb-406">Wechseln Sie zu der [Azure-Verwaltungsportal](https://manage.windowsazure.com/) und melden Sie sich mit den Microsoft-Anmeldeinformationen, die Ihrem Abonnement zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-406">Go to the [Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f8ccb-407">Mit Azure können Sie 10 ASP.NET-Websites kostenlos hosten und dann zu skalieren, wenn Ihr Datenverkehr zunimmt.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-407">With Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="f8ccb-408">Sie können registrieren [hier](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="f8ccb-408">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="f8ccb-409">![Melden Sie sich bei Windows Azure-Portal](build-restful-apis-with-aspnet-web-api/_static/image39.png "melden Sie sich bei Windows Azure-Portal")</span><span class="sxs-lookup"><span data-stu-id="f8ccb-409">![Log on to Windows Azure portal](build-restful-apis-with-aspnet-web-api/_static/image39.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="f8ccb-410">*Melden Sie sich bei Verwaltungsportal*</span><span class="sxs-lookup"><span data-stu-id="f8ccb-410">*Log on to Portal*</span></span>
2. <span data-ttu-id="f8ccb-411">Klicken Sie auf **neu** auf der Befehlsleiste.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-411">Click **New** on the command bar.</span></span>

    <span data-ttu-id="f8ccb-412">![Erstellen einer neuen Website](build-restful-apis-with-aspnet-web-api/_static/image40.png "Erstellen einer neuen Website")</span><span class="sxs-lookup"><span data-stu-id="f8ccb-412">![Creating a new Web Site](build-restful-apis-with-aspnet-web-api/_static/image40.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="f8ccb-413">*Erstellen einer neuen Website*</span><span class="sxs-lookup"><span data-stu-id="f8ccb-413">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="f8ccb-414">Klicken Sie auf **Compute** | **Website**.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-414">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="f8ccb-415">Wählen Sie dann **Schnellerfassung** Option.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-415">Then select **Quick Create** option.</span></span> <span data-ttu-id="f8ccb-416">Geben Sie eine verfügbare URL für die neue Website, und klicken Sie auf **Website erstellen**.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-416">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f8ccb-417">Azure ist der Host für eine Webanwendung, die in der Cloud, die Sie steuern und verwalten können.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-417">Azure is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="f8ccb-418">Die Option "Schnellerfassung" können Sie eine vollständige Webanwendung in Azure von außerhalb des Portals bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-418">The Quick Create option allows you to deploy a completed web application to the Azure from outside the portal.</span></span> <span data-ttu-id="f8ccb-419">Er umfasst keine Informationen zum Einrichten einer Datenbank.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-419">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="f8ccb-420">![Erstellen einer neuen Website mithilfe der Schnellerfassung](build-restful-apis-with-aspnet-web-api/_static/image41.png "Erstellen einer neuen Website mithilfe der Schnellerfassung")</span><span class="sxs-lookup"><span data-stu-id="f8ccb-420">![Creating a new Web Site using Quick Create](build-restful-apis-with-aspnet-web-api/_static/image41.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="f8ccb-421">*Erstellen einer neuen Website mithilfe der Schnellerfassung*</span><span class="sxs-lookup"><span data-stu-id="f8ccb-421">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="f8ccb-422">Warten Sie, bis die neue **Website** erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-422">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="f8ccb-423">Nachdem die Website erstellt wurde, klicken Sie auf den Link unter der **URL** Spalte.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-423">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="f8ccb-424">Überprüfen Sie, dass die neue Website ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-424">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="f8ccb-425">![Navigieren zu der neuen Website](build-restful-apis-with-aspnet-web-api/_static/image42.png "auf die neue Website durchsuchen")</span><span class="sxs-lookup"><span data-stu-id="f8ccb-425">![Browsing to the new web site](build-restful-apis-with-aspnet-web-api/_static/image42.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="f8ccb-426">*Um die neue Website durchsuchen*</span><span class="sxs-lookup"><span data-stu-id="f8ccb-426">*Browsing to the new web site*</span></span>

    <span data-ttu-id="f8ccb-427">![Website](build-restful-apis-with-aspnet-web-api/_static/image43.png "Website ausgeführt wird")</span><span class="sxs-lookup"><span data-stu-id="f8ccb-427">![Web site running](build-restful-apis-with-aspnet-web-api/_static/image43.png "Web site running")</span></span>

    <span data-ttu-id="f8ccb-428">*Die Website ausgeführt wird*</span><span class="sxs-lookup"><span data-stu-id="f8ccb-428">*Web site running*</span></span>
6. <span data-ttu-id="f8ccb-429">Wechseln Sie zurück zum Portal, und klicken Sie auf den Namen der Website unter der **Namen** Spalte die Verwaltungsseiten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-429">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="f8ccb-430">![Öffnen die Website-Verwaltungsseiten](build-restful-apis-with-aspnet-web-api/_static/image44.png "öffnen die Website-Verwaltungsseiten")</span><span class="sxs-lookup"><span data-stu-id="f8ccb-430">![Opening the web site management pages](build-restful-apis-with-aspnet-web-api/_static/image44.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="f8ccb-431">*Öffnen die Website-Verwaltungsseiten*</span><span class="sxs-lookup"><span data-stu-id="f8ccb-431">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="f8ccb-432">In der **Dashboard** Seite die **Blick** auf die **Veröffentlichungsprofil herunterladen** Link.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-432">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f8ccb-433">Die *Veröffentlichungsprofil* enthält alle Informationen zum Veröffentlichen einer Webanwendung auf Azure für die einzelnen aktivierten Veröffentlichungsmethoden erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-433">The *publish profile* contains all of the information required to publish a web application to a Azure for each enabled publication method.</span></span> <span data-ttu-id="f8ccb-434">Das Veröffentlichungsprofil enthält die URLs, Benutzeranmeldeinformationen und datenbankzeichenfolgen, die zum Herstellen einer Verbindung mit und die Authentifizierung bei jedem der Endpunkte für die eine Veröffentlichungsmethode aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-434">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="f8ccb-435">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express für Web** und **Microsoft Visual Studio 2012** Unterstützung, die beim Lesen von veröffentlichungsprofilen Automatisieren der Konfiguration von diesen Programmen Veröffentlichung von Webanwendungen in Azure.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-435">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Azure.</span></span>

    <span data-ttu-id="f8ccb-436">![Herunterladen der Website-Veröffentlichungsprofil](build-restful-apis-with-aspnet-web-api/_static/image45.png "herunterladen den Website-Veröffentlichungsprofil")</span><span class="sxs-lookup"><span data-stu-id="f8ccb-436">![Downloading the web site publish profile](build-restful-apis-with-aspnet-web-api/_static/image45.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="f8ccb-437">*Herunterladen der Website-Veröffentlichungsprofil*</span><span class="sxs-lookup"><span data-stu-id="f8ccb-437">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="f8ccb-438">Laden Sie das Veröffentlichungsprofil an einem bekannten Speicherort herunter.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-438">Download the publish profile file to a known location.</span></span> <span data-ttu-id="f8ccb-439">In dieser Übung wird weiter wie Sie diese Datei verwenden, um das Veröffentlichen einer Webanwendung in Azure aus Visual Studio angezeigt.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-439">Further in this exercise you will see how to use this file to publish a web application to Azure from Visual Studio.</span></span>

    <span data-ttu-id="f8ccb-440">![Speichern das Veröffentlichungsprofil](build-restful-apis-with-aspnet-web-api/_static/image46.png "speichern das Veröffentlichungsprofil")</span><span class="sxs-lookup"><span data-stu-id="f8ccb-440">![Saving the publish profile file](build-restful-apis-with-aspnet-web-api/_static/image46.png "Saving the publish profile")</span></span>

    <span data-ttu-id="f8ccb-441">*Speichern das Veröffentlichungsprofil*</span><span class="sxs-lookup"><span data-stu-id="f8ccb-441">*Saving the publish profile file*</span></span>

<a id="ApxCTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="f8ccb-442">Aufgabe 2: Konfigurieren des Datenbank-Servers</span><span class="sxs-lookup"><span data-stu-id="f8ccb-442">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="f8ccb-443">Wenn Ihre Anwendung nutzt SQL Server Datenbanken, die Sie zum Erstellen eines SQL-Datenbank-Servers benötigen.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-443">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="f8ccb-444">Wenn zum Bereitstellen einer einfachen Anwendung, die keine SQL Server verwendet werden sollen, können Sie diese Aufgabe überspringen.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-444">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="f8ccb-445">Sie benötigen einen SQL-Datenbank-Server zum Speichern der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-445">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="f8ccb-446">Sehen Sie die SQL-Datenbank-Server aus Ihrem Abonnement im Azure-Verwaltungsportal auf **Sql-Datenbanken** | **Server** | **Dashboard des Servers**.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-446">You can view the SQL Database servers from your subscription in the Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="f8ccb-447">Wenn Sie nicht mit eine Server-Instanz verfügen, können Sie erstellen, mithilfe der **hinzufügen** auf der Befehlsleiste auf die Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-447">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="f8ccb-448">Notieren Sie sich die **Servername und -URL, Administrator-Anmeldenamen und Kennwort**, wie Sie sie in den nächsten Aufgaben verwenden.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-448">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="f8ccb-449">Erstellen Sie die Datenbank nicht noch, wie es in einem späteren Zeitpunkt erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-449">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="f8ccb-450">![SQL Server-Datenbank-Dashboard](build-restful-apis-with-aspnet-web-api/_static/image47.png "Dashboard der SQL-Datenbank-Server")</span><span class="sxs-lookup"><span data-stu-id="f8ccb-450">![SQL Database Server Dashboard](build-restful-apis-with-aspnet-web-api/_static/image47.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="f8ccb-451">*SQL Server-Datenbank-Dashboard*</span><span class="sxs-lookup"><span data-stu-id="f8ccb-451">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="f8ccb-452">In der nächsten Aufgabe testen Sie die Verbindung mit der Datenbank, in Visual Studio aus diesem Grund Sie Ihre lokale IP-Adresse in der Liste des Servers der einschließen müssen **zulässige IP-Adressen**.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-452">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="f8ccb-453">Zu diesem Zweck klicken Sie auf **konfigurieren**, wählen Sie die IP-Adresse von **Current Client IP Address** und fügen Sie ihn auf die **IP-Startadresse** und **IP-Endadresse** Textfelder, und klicken Sie auf die ![add-client-ip-address-ok-button](build-restful-apis-with-aspnet-web-api/_static/image48.png) Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-453">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](build-restful-apis-with-aspnet-web-api/_static/image48.png) button.</span></span>

    ![Client-IP-Adresse hinzufügen](build-restful-apis-with-aspnet-web-api/_static/image49.png)

    <span data-ttu-id="f8ccb-455">*Client-IP-Adresse hinzufügen*</span><span class="sxs-lookup"><span data-stu-id="f8ccb-455">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="f8ccb-456">Einmal die **Client-IP-Adresse** wird hinzugefügt, um die zulässigen IP-Adressen aufzulisten, klicken Sie auf **speichern** um die Änderungen zu bestätigen.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-456">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Bestätigen von Änderungen](build-restful-apis-with-aspnet-web-api/_static/image50.png)

    <span data-ttu-id="f8ccb-458">*Bestätigen von Änderungen*</span><span class="sxs-lookup"><span data-stu-id="f8ccb-458">*Confirm Changes*</span></span>

<a id="ApxCTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="f8ccb-459">Aufgabe 3: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web Deploy</span><span class="sxs-lookup"><span data-stu-id="f8ccb-459">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="f8ccb-460">Wechseln Sie zurück, der ASP.NET MVC 4-Projektmappe.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-460">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="f8ccb-461">In der **Projektmappen-Explorer**mit der rechten Maustaste auf das Websiteprojekt, und wählen Sie **veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-461">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="f8ccb-462">![Veröffentlichen der Anwendung](build-restful-apis-with-aspnet-web-api/_static/image51.png "Veröffentlichen der Anwendung")</span><span class="sxs-lookup"><span data-stu-id="f8ccb-462">![Publishing the Application](build-restful-apis-with-aspnet-web-api/_static/image51.png "Publishing the Application")</span></span>

    <span data-ttu-id="f8ccb-463">*Veröffentlichen der Website*</span><span class="sxs-lookup"><span data-stu-id="f8ccb-463">*Publishing the web site*</span></span>
2. <span data-ttu-id="f8ccb-464">Importieren Sie das Veröffentlichungsprofil, die, das Sie in der ersten Aufgabe gespeichert.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-464">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="f8ccb-465">![Importieren das Veröffentlichungsprofil](build-restful-apis-with-aspnet-web-api/_static/image52.png "Importieren des Veröffentlichungsprofils")</span><span class="sxs-lookup"><span data-stu-id="f8ccb-465">![Importing the publish profile](build-restful-apis-with-aspnet-web-api/_static/image52.png "Importing the publish profile")</span></span>

    <span data-ttu-id="f8ccb-466">*Importieren Sie das Veröffentlichungsprofil*</span><span class="sxs-lookup"><span data-stu-id="f8ccb-466">*Importing publish profile*</span></span>
3. <span data-ttu-id="f8ccb-467">Klicken Sie auf **überprüft, ob Verbindung**.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-467">Click **Validate Connection**.</span></span> <span data-ttu-id="f8ccb-468">Klicken Sie nach Abschluss der Überprüfung auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-468">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f8ccb-469">Die Überprüfung ist abgeschlossen, wenn Sie sehen, dass ein grünes Häkchen neben der Schaltfläche "Verbindung überprüfen" angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-469">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="f8ccb-470">![Überprüfen der Verbindung](build-restful-apis-with-aspnet-web-api/_static/image53.png "Überprüfen der Verbindung")</span><span class="sxs-lookup"><span data-stu-id="f8ccb-470">![Validating connection](build-restful-apis-with-aspnet-web-api/_static/image53.png "Validating connection")</span></span>

    <span data-ttu-id="f8ccb-471">*Überprüfen der Verbindung*</span><span class="sxs-lookup"><span data-stu-id="f8ccb-471">*Validating connection*</span></span>
4. <span data-ttu-id="f8ccb-472">In der **Einstellungen** Seite die **Datenbanken** auf die Schaltfläche neben dem Textfeld für die datenbankverbindung (d. h. **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="f8ccb-472">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="f8ccb-473">![Web deploy-Konfiguration](build-restful-apis-with-aspnet-web-api/_static/image54.png "Web deploy-Konfiguration")</span><span class="sxs-lookup"><span data-stu-id="f8ccb-473">![Web deploy configuration](build-restful-apis-with-aspnet-web-api/_static/image54.png "Web deploy configuration")</span></span>

    <span data-ttu-id="f8ccb-474">*Web deploy-Konfiguration*</span><span class="sxs-lookup"><span data-stu-id="f8ccb-474">*Web deploy configuration*</span></span>
5. <span data-ttu-id="f8ccb-475">Konfigurieren Sie die Verbindung mit der Datenbank wie folgt:</span><span class="sxs-lookup"><span data-stu-id="f8ccb-475">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="f8ccb-476">In der **Servernamen** Geben Sie Ihre SQL-Datenbankserver URL mit der *Tcp:* Präfix.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-476">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="f8ccb-477">In **Benutzernamen** Geben Sie Ihre Server Administrator-Anmeldenamen.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-477">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="f8ccb-478">In **Kennwort** Geben Sie Ihre Server Administrator-Anmeldekennwort.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-478">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="f8ccb-479">Geben Sie einen neuen Datenbanknamen ein, z. B.: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-479">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="f8ccb-480">![Konfigurieren von Ziel-Verbindungszeichenfolge](build-restful-apis-with-aspnet-web-api/_static/image55.png "Zielverbindungszeichenfolge konfigurieren")</span><span class="sxs-lookup"><span data-stu-id="f8ccb-480">![Configuring destination connection string](build-restful-apis-with-aspnet-web-api/_static/image55.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="f8ccb-481">*Konfigurieren von Ziel-Verbindungszeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="f8ccb-481">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="f8ccb-482">Klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-482">Then click **OK**.</span></span> <span data-ttu-id="f8ccb-483">Bei der Aufforderung zum Erstellen der Datenbank auf **Ja**.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-483">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="f8ccb-484">![Erstellen der Datenbank](build-restful-apis-with-aspnet-web-api/_static/image56.png "erstellen die datenbankzeichenfolge")</span><span class="sxs-lookup"><span data-stu-id="f8ccb-484">![Creating the database](build-restful-apis-with-aspnet-web-api/_static/image56.png "Creating the database string")</span></span>

    <span data-ttu-id="f8ccb-485">*Erstellen der Datenbank*</span><span class="sxs-lookup"><span data-stu-id="f8ccb-485">*Creating the database*</span></span>
7. <span data-ttu-id="f8ccb-486">Die Verbindungszeichenfolge, die Sie für die Verbindung mit SQL-Datenbank in Windows Azure verwendet werden, ist innerhalb der Verbindungstyp Standard Textfeld angezeigt.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-486">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="f8ccb-487">Klicken Sie dann auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-487">Then click **Next**.</span></span>

    <span data-ttu-id="f8ccb-488">![Auf der SQL-Datenbank-Verbindungszeichenfolge](build-restful-apis-with-aspnet-web-api/_static/image57.png "auf SQL-Datenbank-Verbindungszeichenfolge")</span><span class="sxs-lookup"><span data-stu-id="f8ccb-488">![Connection string pointing to SQL Database](build-restful-apis-with-aspnet-web-api/_static/image57.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="f8ccb-489">*Auf der SQL-Datenbank-Verbindungszeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="f8ccb-489">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="f8ccb-490">In der **Vorschau** auf **veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-490">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="f8ccb-491">![Veröffentlichen der Webanwendung](build-restful-apis-with-aspnet-web-api/_static/image58.png "Veröffentlichen der Webanwendung")</span><span class="sxs-lookup"><span data-stu-id="f8ccb-491">![Publishing the web application](build-restful-apis-with-aspnet-web-api/_static/image58.png "Publishing the web application")</span></span>

    <span data-ttu-id="f8ccb-492">*Veröffentlichen der Webanwendung*</span><span class="sxs-lookup"><span data-stu-id="f8ccb-492">*Publishing the web application*</span></span>
9. <span data-ttu-id="f8ccb-493">Sobald der Veröffentlichungsprozess abgeschlossen ist, öffnet Ihr Standardbrowser die veröffentlichte Website.</span><span class="sxs-lookup"><span data-stu-id="f8ccb-493">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="f8ccb-494">![Anwendung auf Windows Azure veröffentlicht](build-restful-apis-with-aspnet-web-api/_static/image59.png "Anwendung auf Windows Azure veröffentlicht")</span><span class="sxs-lookup"><span data-stu-id="f8ccb-494">![Application published to Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image59.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="f8ccb-495">*Anwendung in Azure veröffentlicht*</span><span class="sxs-lookup"><span data-stu-id="f8ccb-495">*Application published to Azure*</span></span>
