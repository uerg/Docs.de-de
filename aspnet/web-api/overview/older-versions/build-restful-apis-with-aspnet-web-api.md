---
uid: web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
title: Erstellen von RESTful-APIs mit ASP.NET Web-API | Microsoft Docs
author: rick-anderson
description: In den vergangenen Jahren hat es klar sein, dass HTTP nicht ist nur für HTML-Seiten bedient. Es ist auch eine leistungsstarke Plattform zum Erstellen von Web-APIs, die mit einer Handvoll o...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 87daa99f-3810-407e-b969-dd28a192959d
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: cb02288e93be801a1e55852741ed1443d8d3617d
ms.sourcegitcommit: 3a893ae05f010656d99d6ddf55e82f1b5b6933bc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/18/2018
---
<a name="build-restful-apis-with-aspnet-web-api"></a><span data-ttu-id="6cbc9-104">Erstellen von RESTful-APIs mit ASP.NET Web-API</span><span class="sxs-lookup"><span data-stu-id="6cbc9-104">Build RESTful APIs with ASP.NET Web API</span></span>
====================
<span data-ttu-id="6cbc9-105">Durch [Web Lager Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="6cbc9-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="6cbc9-106">In den vergangenen Jahren hat es klar sein, dass HTTP nicht ist nur für HTML-Seiten bedient.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-106">In recent years, it has become clear that HTTP is not just for serving up HTML pages.</span></span> <span data-ttu-id="6cbc9-107">Es ist auch eine leistungsstarke Plattform zum Erstellen von Web-APIs, mit ein paar Verben (GET, POST usw.) sowie einige einfache Konzepte wie *URIs* und *Header*.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-107">It is also a powerful platform for building Web APIs, using a handful of verbs (GET, POST, and so forth) plus a few simple concepts such as *URIs* and *headers*.</span></span> <span data-ttu-id="6cbc9-108">ASP.NET-Web-API ist eine Reihe von Komponenten, die HTTP-Programmiermodell zu vereinfachen.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-108">ASP.NET Web API is a set of components that simplify HTTP programming.</span></span> <span data-ttu-id="6cbc9-109">Da es auf Grundlage der ASP.NET MVC-Laufzeit erstellt wurde, verarbeitet Web-API automatisch die Transportdetails auf niedriger Ebene HTTP.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-109">Because it is built on top of the ASP.NET MVC runtime, Web API automatically handles the low-level transport details of HTTP.</span></span> <span data-ttu-id="6cbc9-110">Zur gleichen Zeit macht das HTTP-Programmiermodell natürlich von Web-API verfügbar.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-110">At the same time, Web API naturally exposes the HTTP programming model.</span></span> <span data-ttu-id="6cbc9-111">Tatsächlich ist eines der Ziele von Web-API zum *nicht* Abstraktion für die Verwendung von HTTP.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-111">In fact, one goal of Web API is to *not* abstract away the reality of HTTP.</span></span> <span data-ttu-id="6cbc9-112">Deshalb ist Web-API flexibel und einfach zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-112">As a result, Web API is both flexible and easy to extend.</span></span> <span data-ttu-id="6cbc9-113">In dieser praktischen Übungseinheit verwenden Sie die Web-API, um eine einfache REST-API für einen Kontakt-Manager-Anwendung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-113">In this hands-on lab, you will use Web API to build a simple REST API for a contact manager application.</span></span> <span data-ttu-id="6cbc9-114">Erstellen Sie auch einen Client für die API nutzen.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-114">You will also build a client to consume the API.</span></span> <span data-ttu-id="6cbc9-115">Architektonische REST-Stil hat eine effektive Möglichkeit für die HTTP - genutzt werden zwar sicherlich nicht der einzige gültige Ansatz in HTTP-erwiesen.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-115">The REST architectural style has proven to be an effective way to leverage HTTP - although it is certainly not the only valid approach to HTTP.</span></span> <span data-ttu-id="6cbc9-116">Der Kontakte-Manager wird die RESTful zum Auflisten, hinzufügen und Entfernen von Kontakten, u. a. verfügbar machen.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-116">The contact manager will expose the RESTful for listing, adding and removing contacts, among others.</span></span> <span data-ttu-id="6cbc9-117">In dieser Umgebung erfordert ein grundlegendes Verständnis von HTTP, REST, und setzt voraus, dass Sie über grundlegende Kenntnisse von HTML, JavaScript und jQuery haben.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-117">This lab requires a basic understanding of HTTP, REST, and assumes you have a basic working knowledge of HTML, JavaScript, and jQuery.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="6cbc9-118">Die ASP.NET-Website verfügt über einen speziellen Bereich für die ASP.NET Web API-Framework unter [ https://asp.net/web-api ](https://asp.net/web-api).</span><span class="sxs-lookup"><span data-stu-id="6cbc9-118">The ASP.NET Web site has an area dedicated to the ASP.NET Web API framework at [https://asp.net/web-api](https://asp.net/web-api).</span></span> <span data-ttu-id="6cbc9-119">Dieser Standort wird weiterhin bereitstellen, hochaktuelle Informationen, Beispiele und Nachrichten, die im Zusammenhang mit der Web-API so checken Sie es häufig, wenn Sie tiefer in die Kunst zum Erstellen von benutzerdefinierten Web-APIs auf praktisch jedem Gerät oder einer Entwicklungsumgebung Framework vertiefen möchten.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-119">This site will continue to provide late-breaking information, samples, and news related to Web API, so check it frequently if you'd like to delve deeper into the art of creating custom Web APIs available to virtually any device or development framework.</span></span>
> > 
> > <span data-ttu-id="6cbc9-120">ASP.NET Web-API, ähnlich wie in ASP.NET MVC 4 hat hohe Flexibilität im Hinblick auf die Dienstebene trennen, durch den Controller, und Sie können einige der verfügbaren Abhängigkeitsinjektion Frameworks relativ einfach zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-120">ASP.NET Web API, similar to ASP.NET MVC 4, has great flexibility in terms of separating the service layer from the controllers allowing you to use several of the available Dependency Injection frameworks fairly easy.</span></span> <span data-ttu-id="6cbc9-121">Es ist ein gutes Beispiel in MSDN, die zeigt, wie Ninject für Abhängigkeitsinjektion in ASP.NET Web-API-Projekt, das Sie ihn von herunterladen können [hier](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7).</span><span class="sxs-lookup"><span data-stu-id="6cbc9-121">There is a good sample in MSDN that shows how to use Ninject for dependency injection in an ASP.NET Web API project that you can download it from [here](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7).</span></span>
> 
> 
> <span data-ttu-id="6cbc9-122">Alle Beispielcode und Codeausschnitte sind im Web Lager Training Kit unter enthalten [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="6cbc9-122">All sample code and snippets are included in the Web Camps Training Kit, available at [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span></span>


<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="6cbc9-123">Ziele</span><span class="sxs-lookup"><span data-stu-id="6cbc9-123">Objectives</span></span>

<span data-ttu-id="6cbc9-124">In dieser praktischen Übungseinheit erfahren Sie, wie Sie:</span><span class="sxs-lookup"><span data-stu-id="6cbc9-124">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="6cbc9-125">Implementieren Sie eine RESTful-Web-API</span><span class="sxs-lookup"><span data-stu-id="6cbc9-125">Implement a RESTful Web API</span></span>
- <span data-ttu-id="6cbc9-126">Aufrufen der API aus einem HTML-client</span><span class="sxs-lookup"><span data-stu-id="6cbc9-126">Call the API from an HTML client</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="6cbc9-127">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="6cbc9-127">Prerequisites</span></span>

<span data-ttu-id="6cbc9-128">Folgendes ist erforderlich, um diese praktische Übungseinheit:</span><span class="sxs-lookup"><span data-stu-id="6cbc9-128">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="6cbc9-129">[Microsoft Visual Studio Express 2012 für das Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) oder sogar eine höhere (Lesen [Anhang B](#AppendixB) Anleitungen zur Installation).</span><span class="sxs-lookup"><span data-stu-id="6cbc9-129">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix B](#AppendixB) for instructions on how to install it).</span></span>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="6cbc9-130">Setup</span><span class="sxs-lookup"><span data-stu-id="6cbc9-130">Setup</span></span>

<span data-ttu-id="6cbc9-131">**Installieren von Codeausschnitten**</span><span class="sxs-lookup"><span data-stu-id="6cbc9-131">**Installing Code Snippets**</span></span>

<span data-ttu-id="6cbc9-132">Der Einfachheit halber gilt Großteil des Codes, den Sie entlang dieser Übung verwalten als Visual Studio-Codeausschnitte verfügbar.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-132">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="6cbc9-133">So installieren Sie die Codeausschnitte ausführen **.\Source\Setup\CodeSnippets.vsi** Datei.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-133">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="6cbc9-134">Wenn Sie nicht mit den Visual Studio-Codeausschnitte und zu erfahren, wie deren Verwendung vertraut sind, Sie können finden Sie im Anhang in diesem Dokument &quot; [Anhang A: Verwenden von Code Snippets](#AppendixA)&quot;.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-134">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix A: Using Code Snippets](#AppendixA)&quot;.</span></span>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="6cbc9-135">Übungen</span><span class="sxs-lookup"><span data-stu-id="6cbc9-135">Exercises</span></span>

<span data-ttu-id="6cbc9-136">Diese praktische Übungseinheit enthält in der folgenden Übung:</span><span class="sxs-lookup"><span data-stu-id="6cbc9-136">This hands-on lab includes the following exercise:</span></span>

1. [<span data-ttu-id="6cbc9-137">Übung 1: Erstellen einer nur-Lese Web-API</span><span class="sxs-lookup"><span data-stu-id="6cbc9-137">Exercise 1: Create a Read-Only Web API</span></span>](#Exercise1)
2. [<span data-ttu-id="6cbc9-138">Übung 2: Erstellen einer Lese-/Schreibzugriff-Webs-API</span><span class="sxs-lookup"><span data-stu-id="6cbc9-138">Exercise 2: Create a Read/Write Web API</span></span>](#Exercise2)
3. [<span data-ttu-id="6cbc9-139">Übung 3: Verwenden der Webs-API aus einem HTML-Client</span><span class="sxs-lookup"><span data-stu-id="6cbc9-139">Exercise 3: Consume the Web API from an HTML Client</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="6cbc9-140">Jede Übung beiliegen ein **End** Ordner, der sich so ergebende Lösung, die Sie nach Abschluss der Übung abrufen soll.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-140">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="6cbc9-141">Sie können diese Lösung als Leitfaden verwenden, ggf. zusätzliche Hilfe bei der Übungen durcharbeiten.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-141">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="6cbc9-142">Geschätzte Zeit zum Abschließen dieser testumgebung: **60 Minuten**.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-142">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Create_a_Read-Only_Web_API"></a>
### <a name="exercise-1-create-a-read-only-web-api"></a><span data-ttu-id="6cbc9-143">Übung 1: Erstellen einer nur-Lese Web-API</span><span class="sxs-lookup"><span data-stu-id="6cbc9-143">Exercise 1: Create a Read-Only Web API</span></span>

<span data-ttu-id="6cbc9-144">In dieser Übung wird die nur-Lese GET-Methoden für den Kontakt-Manager implementiert werden.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-144">In this exercise, you will implement the read-only GET methods for the contact manager.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_API_Project"></a>
#### <a name="task-1---creating-the-api-project"></a><span data-ttu-id="6cbc9-145">Aufgabe 1: Erstellen des Projekts-API</span><span class="sxs-lookup"><span data-stu-id="6cbc9-145">Task 1 - Creating the API Project</span></span>

<span data-ttu-id="6cbc9-146">In dieser Aufgabe verwenden Sie die neue ASP.NET Web-Projektvorlagen zum Erstellen einer Web-API-Webanwendung.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-146">In this task, you will use the new ASP.NET web project templates to create a Web API web application.</span></span>

1. <span data-ttu-id="6cbc9-147">Führen Sie **zuerst Visual Studio 2012 Express für Web**, wechseln Sie zu diesem möchten **starten** und Typ **Visual Studio Express für Web** drücken Sie dann die **EINGABETASTE**.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-147">Run **Visual Studio 2012 Express for Web**, to do this go to **Start** and type **VS Express for Web** then press **Enter**.</span></span>
2. <span data-ttu-id="6cbc9-148">Aus der **Datei** klicken Sie im Menü **neues Projekt**.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-148">From the **File** menu, select **New Project**.</span></span> <span data-ttu-id="6cbc9-149">Wählen Sie die **Visual C# | Web** Projekttyp aus der Strukturansicht der Projekt-Typ aus, und wählen Sie dann die **ASP.NET MVC 4-Webanwendung** Projekttyp.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-149">Select the **Visual C# | Web** project type from the project type tree view, then select the **ASP.NET MVC 4 Web Application** project type.</span></span> <span data-ttu-id="6cbc9-150">Legen Sie das Projekt **Namen** auf *ContactManager* und **Projektmappenname** auf *beginnen*, klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-150">Set the project's **Name** to *ContactManager* and the **Solution name** to *Begin*, then click **OK**.</span></span>

    <span data-ttu-id="6cbc9-151">![Erstellen eine neue ASP.NET MVC 4.0 Webanwendungsprojekt](build-restful-apis-with-aspnet-web-api/_static/image1.png "Erstellen eines neuen ASP.NET MVC 4.0 Web-Anwendungsprojekts")</span><span class="sxs-lookup"><span data-stu-id="6cbc9-151">![Creating a new ASP.NET MVC 4.0 Web Application Project](build-restful-apis-with-aspnet-web-api/_static/image1.png "Creating a new ASP.NET MVC 4.0 Web Application Project")</span></span>

    <span data-ttu-id="6cbc9-152">*Erstellen eines neuen ASP.NET MVC 4.0 Web-Anwendungsprojekts*</span><span class="sxs-lookup"><span data-stu-id="6cbc9-152">*Creating a new ASP.NET MVC 4.0 Web Application Project*</span></span>
3. <span data-ttu-id="6cbc9-153">Wählen Sie im Dialogfeld für den Typ ASP.NET MVC 4-Projekt die **Web-API** Projekttyp.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-153">In the ASP.NET MVC 4 project type dialog, select the **Web API** project type.</span></span> <span data-ttu-id="6cbc9-154">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-154">Click **OK**.</span></span>

    <span data-ttu-id="6cbc9-155">![Angeben den Web-API-Projekttyp](build-restful-apis-with-aspnet-web-api/_static/image2.png "Angabe des Typs der Web-API-Projekt")</span><span class="sxs-lookup"><span data-stu-id="6cbc9-155">![Specifying the Web API project type](build-restful-apis-with-aspnet-web-api/_static/image2.png "Specifying the Web API project type")</span></span>

    <span data-ttu-id="6cbc9-156">*Angeben den Web-API-Projekttyp*</span><span class="sxs-lookup"><span data-stu-id="6cbc9-156">*Specifying the Web API project type*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_the_Contact_Manager_API_Controllers"></a>
#### <a name="task-2---creating-the-contact-manager-api-controllers"></a><span data-ttu-id="6cbc9-157">Aufgabe 2: erstellen die Kontakt-Manager-API-Controller</span><span class="sxs-lookup"><span data-stu-id="6cbc9-157">Task 2 - Creating the Contact Manager API Controllers</span></span>

<span data-ttu-id="6cbc9-158">In dieser Aufgabe erstellen Sie die Controllerklassen in denen API-Methoden gespeichert werden soll.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-158">In this task, you will create the controller classes in which API methods will reside.</span></span>

1. <span data-ttu-id="6cbc9-159">Löschen Sie die Datei mit dem Namen **ValuesController.cs** in **Controller** Ordner aus dem Projekt.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-159">Delete the file named **ValuesController.cs** within **Controllers** folder from the project.</span></span>
2. <span data-ttu-id="6cbc9-160">Mit der rechten Maustaste die **Controller** Ordner im Projekt, und wählen Sie **hinzufügen | Controller** aus dem Kontextmenü.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-160">Right-click the **Controllers** folder in the project and select **Add | Controller** from the context menu.</span></span>

    <span data-ttu-id="6cbc9-161">![Hinzufügen eines neuen Controllers zum Projekt](build-restful-apis-with-aspnet-web-api/_static/image3.png "Hinzufügen eines neuen Controllers zum Projekt")</span><span class="sxs-lookup"><span data-stu-id="6cbc9-161">![Adding a new controller to the project](build-restful-apis-with-aspnet-web-api/_static/image3.png "Adding a new controller to the project")</span></span>

    <span data-ttu-id="6cbc9-162">*Hinzufügen eines neuen Controllers zum Projekt*</span><span class="sxs-lookup"><span data-stu-id="6cbc9-162">*Adding a new controller to the project*</span></span>
3. <span data-ttu-id="6cbc9-163">In der **Controller hinzufügen** Dialogfeld, das angezeigt wird, wählen Sie **leerer API-Controller** aus dem Menü Vorlage.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-163">In the **Add Controller** dialog that appears, select **Empty API Controller** from the Template menu.</span></span> <span data-ttu-id="6cbc9-164">Benennen Sie die Controllerklasse **ContactController**.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-164">Name the controller class **ContactController**.</span></span> <span data-ttu-id="6cbc9-165">Klicken Sie auf **hinzufügen.**</span><span class="sxs-lookup"><span data-stu-id="6cbc9-165">Then, click **Add.**</span></span>

    <span data-ttu-id="6cbc9-166">![Verwenden das Dialogfeld "Controller hinzufügen" zum Erstellen eines neuen Web-API-Controllers](build-restful-apis-with-aspnet-web-api/_static/image4.png "verwenden das Dialogfeld \"Controller hinzufügen\" zum Erstellen eines neuen Web-API-Controllers")</span><span class="sxs-lookup"><span data-stu-id="6cbc9-166">![Using the Add Controller dialog to create a new Web API controller](build-restful-apis-with-aspnet-web-api/_static/image4.png "Using the Add Controller dialog to create a new Web API controller")</span></span>

    <span data-ttu-id="6cbc9-167">*Verwenden das Dialogfeld "Controller hinzufügen" zum Erstellen eines neuen Web-API-Controllers*</span><span class="sxs-lookup"><span data-stu-id="6cbc9-167">*Using the Add Controller dialog to create a new Web API controller*</span></span>
4. <span data-ttu-id="6cbc9-168">Fügen Sie folgenden Code, der **ContactController**.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-168">Add the following code to the **ContactController**.</span></span>

    <span data-ttu-id="6cbc9-169">(Codeausschnitt - *Web-API-Lab - Ex01 - Get-API-Methode*)</span><span class="sxs-lookup"><span data-stu-id="6cbc9-169">(Code Snippet - *Web API Lab - Ex01 - Get API Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample1.cs)]
5. <span data-ttu-id="6cbc9-170">Drücken Sie **F5** zum Debuggen der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-170">Press **F5** to debug the application.</span></span> <span data-ttu-id="6cbc9-171">Die Standardstartseite für eine Web-API-Projekt sollte angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-171">The default home page for a Web API project should appear.</span></span>

    <span data-ttu-id="6cbc9-172">![Die Standardstartseite einer ASP.NET Web-API-Anwendung](build-restful-apis-with-aspnet-web-api/_static/image5.png "die Standardstartseite einer ASP.NET Web-API-Anwendung")</span><span class="sxs-lookup"><span data-stu-id="6cbc9-172">![The default home page of an ASP.NET Web API application](build-restful-apis-with-aspnet-web-api/_static/image5.png "The default home page of an ASP.NET Web API application")</span></span>

    <span data-ttu-id="6cbc9-173">*Die Standardstartseite einer ASP.NET Web-API-Anwendung*</span><span class="sxs-lookup"><span data-stu-id="6cbc9-173">*The default home page of an ASP.NET Web API application*</span></span>
6. <span data-ttu-id="6cbc9-174">Drücken Sie in Internet Explorer-Fenster die **F12** Schlüssel zum Öffnen der **Entwicklertools** Fenster.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-174">In the Internet Explorer window, press the **F12** key to open the **Developer Tools** window.</span></span> <span data-ttu-id="6cbc9-175">Klicken Sie auf die **Netzwerk** Registerkarte, und klicken Sie dann auf die **starten erfassen** zu beginnen, Erfassen von Netzwerkdatenverkehr in das Fenster.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-175">Click the **Network** tab, and then click the **Start Capturing** button to begin capturing network traffic into the window.</span></span>

    <span data-ttu-id="6cbc9-176">![Öffnen die Registerkarte "Netzwerk" und das Initiieren von netzwerkerfassung](build-restful-apis-with-aspnet-web-api/_static/image6.png "öffnen die Registerkarte \"Netzwerk\" und das Initiieren von netzwerkerfassung")</span><span class="sxs-lookup"><span data-stu-id="6cbc9-176">![Opening the network tab and initiating network capture](build-restful-apis-with-aspnet-web-api/_static/image6.png "Opening the network tab and initiating network capture")</span></span>

    <span data-ttu-id="6cbc9-177">*Öffnen die Registerkarte "Netzwerk" und das Initiieren von netzwerkerfassung*</span><span class="sxs-lookup"><span data-stu-id="6cbc9-177">*Opening the network tab and initiating network capture*</span></span>
7. <span data-ttu-id="6cbc9-178">Fügen Sie die URL in die Adressleiste des Browsers, mit **/api/Kontakte** und drücken Sie die EINGABETASTE.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-178">Append the URL in the browser's address bar with **/api/contact** and press enter.</span></span> <span data-ttu-id="6cbc9-179">Die Übertragung wird im Fenster erfassen Netzwerks angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-179">The transmission details will appear in the network capture window.</span></span> <span data-ttu-id="6cbc9-180">Beachten Sie, dass die Antwort MIME-Typ **Application/Json**.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-180">Note that the response's MIME type is **application/json**.</span></span> <span data-ttu-id="6cbc9-181">Dies zeigt, wie das Standardausgabeformat JSON ist.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-181">This demonstrates how the default output format is JSON.</span></span>

    <span data-ttu-id="6cbc9-182">![Anzeigen der Ausgabe der Web-API-Anforderung in der Netzwerkansicht](build-restful-apis-with-aspnet-web-api/_static/image7.png "die Ausgabe der Web-API-Anforderung in der Ansicht anzeigen")</span><span class="sxs-lookup"><span data-stu-id="6cbc9-182">![Viewing the output of the Web API request in the Network view](build-restful-apis-with-aspnet-web-api/_static/image7.png "Viewing the output of the Web API request in the Network view")</span></span>

    <span data-ttu-id="6cbc9-183">*Die Ausgabe der Web-API-Anforderung anzeigen in der Ansicht*</span><span class="sxs-lookup"><span data-stu-id="6cbc9-183">*Viewing the output of the Web API request in the Network view*</span></span>

    > [!NOTE]
    > <span data-ttu-id="6cbc9-184">Internet Explorer 10-Standardverhalten werden an diesem Punkt gefragt, ob der Benutzer, speichern oder den Stream, aus dem Web-API-Aufruf resultierende öffnen möchten.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-184">Internet Explorer 10's default behavior at this point will be to ask if the user would like to save or open the stream resulting from the Web API call.</span></span> <span data-ttu-id="6cbc9-185">Die Ausgabe wird eine Textdatei mit dem JSON-Ergebnis des Aufrufs Web-API-URL sein.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-185">The output will be a text file containing the JSON result of the Web API URL call.</span></span> <span data-ttu-id="6cbc9-186">Brechen Sie das Dialogfeld "nicht um Antwortinhalt durch Entwickler Toolfenster beobachten können.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-186">Do not cancel the dialog in order to be able to watch the response's content through Developers Tool window.</span></span>
8. <span data-ttu-id="6cbc9-187">Klicken Sie auf die **wechseln Sie zur detaillierten Ansicht** Schaltfläche, um weitere Details über die API-Aufrufs Antwort anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-187">Click the **Go to detailed view** button to see more details about the response of this API call.</span></span>

    <span data-ttu-id="6cbc9-188">![Wechseln Sie zur detaillierten Ansicht](build-restful-apis-with-aspnet-web-api/_static/image8.png "wechseln Sie in der Detailansicht")</span><span class="sxs-lookup"><span data-stu-id="6cbc9-188">![Switch to Detailed View](build-restful-apis-with-aspnet-web-api/_static/image8.png "Switch to Details View")</span></span>

    <span data-ttu-id="6cbc9-189">*Wechseln Sie in der Detailansicht*</span><span class="sxs-lookup"><span data-stu-id="6cbc9-189">*Switch to Detailed View*</span></span>
9. <span data-ttu-id="6cbc9-190">Klicken Sie auf die **Antworttext** Registerkarte, um den tatsächlichen Text der JSON-Antwort anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-190">Click the **Response body** tab to view the actual JSON response text.</span></span>

    <span data-ttu-id="6cbc9-191">![Anzeigen von JSON output Text in den Netzwerkmonitor](build-restful-apis-with-aspnet-web-api/_static/image9.png "Text in den Netzwerkmonitor Ausgabe anzeigen von JSON")</span><span class="sxs-lookup"><span data-stu-id="6cbc9-191">![Viewing the JSON output text in the network monitor](build-restful-apis-with-aspnet-web-api/_static/image9.png "Viewing the JSON output text in the network monitor")</span></span>

    <span data-ttu-id="6cbc9-192">*Den Text der JSON-Ausgabe anzeigen in die Netzwerkmonitor*</span><span class="sxs-lookup"><span data-stu-id="6cbc9-192">*Viewing the JSON output text in the network monitor*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Creating_the_Contact_Models_and_Augment_the_Contact_Controller"></a>
#### <a name="task-3---creating-the-contact-models-and-augment-the-contact-controller"></a><span data-ttu-id="6cbc9-193">Aufgabe 3 - Kontakt Modelle erstellen, und erweitern Sie den Kontakten Controller</span><span class="sxs-lookup"><span data-stu-id="6cbc9-193">Task 3 - Creating the Contact Models and Augment the Contact Controller</span></span>

<span data-ttu-id="6cbc9-194">In dieser Aufgabe erstellen Sie die Controllerklassen in denen API-Methoden gespeichert werden soll.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-194">In this task, you will create the controller classes in which API methods will reside.</span></span>

1. <span data-ttu-id="6cbc9-195">Mit der rechten Maustaste die **Modelle** Ordner, und wählen **hinzufügen | Klasse...**  aus dem Kontextmenü.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-195">Right-click the **Models** folder and select **Add | Class...** from the context menu.</span></span>

    <span data-ttu-id="6cbc9-196">![Hinzufügen eines neuen Modells an die Webanwendung](build-restful-apis-with-aspnet-web-api/_static/image10.png "der Webanwendung ein neues Modell hinzufügen")</span><span class="sxs-lookup"><span data-stu-id="6cbc9-196">![Adding a new model to the web application](build-restful-apis-with-aspnet-web-api/_static/image10.png "Adding a new model to the web application")</span></span>

    <span data-ttu-id="6cbc9-197">*Hinzufügen eines neuen Modells an die Web-Anwendung*</span><span class="sxs-lookup"><span data-stu-id="6cbc9-197">*Adding a new model to the web application*</span></span>
2. <span data-ttu-id="6cbc9-198">In der **neues Element hinzufügen** Dialogfeld, benennen Sie die neue Datei **Contact.cs** , und klicken Sie auf **hinzufügen.**</span><span class="sxs-lookup"><span data-stu-id="6cbc9-198">In the **Add New Item** dialog, name the new file **Contact.cs** and click **Add.**</span></span>

    <span data-ttu-id="6cbc9-199">![Erstellen die neue Klassendatei Kontakt](build-restful-apis-with-aspnet-web-api/_static/image11.png "die neue Klassendatei Kontakt erstellen")</span><span class="sxs-lookup"><span data-stu-id="6cbc9-199">![Creating the new Contact class file](build-restful-apis-with-aspnet-web-api/_static/image11.png "Creating the new Contact class file")</span></span>

    <span data-ttu-id="6cbc9-200">*Die neue Klassendatei Kontakt erstellen*</span><span class="sxs-lookup"><span data-stu-id="6cbc9-200">*Creating the new Contact class file*</span></span>
3. <span data-ttu-id="6cbc9-201">Fügen Sie den folgenden hervorgehobenen Code in die **Kontakt** Klasse.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-201">Add the following highlighted code to the **Contact** class.</span></span>

    <span data-ttu-id="6cbc9-202">(Codeausschnitt - *Web-API-Lab - Ex01 - Kontakt Klasse*)</span><span class="sxs-lookup"><span data-stu-id="6cbc9-202">(Code Snippet - *Web API Lab - Ex01 - Contact Class*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample2.cs)]
4. <span data-ttu-id="6cbc9-203">In der **ContactController** Klasse, wählen Sie das Wort **Zeichenfolge** in der Definition des der **abrufen** -Methode, und geben Sie das Wort *Kontakt*.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-203">In the **ContactController** class, select the word **string** in method definition of the **Get** method, and type the word *Contact*.</span></span> <span data-ttu-id="6cbc9-204">Sobald das Wort im eingegeben wurde, erscheint ein Indikator am Anfang des Worts **Kontakt**.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-204">Once the word is typed in, an indicator will appear at the beginning of the word **Contact**.</span></span> <span data-ttu-id="6cbc9-205">Entweder halten Sie die **STRG** -Taste und drücken Sie die Taste Punkt (.) oder klicken Sie auf das Symbol mit der Maus, um das Dialogfeld "Hilfe" im Code-Editor automatisch ausfüllen zu öffnen die **mit** Richtlinie für die Modelle Namespace.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-205">Either hold down the **Ctrl** key and press the period (.) key or click the icon using your mouse to open up the assistance dialog in the code editor, to automatically fill in the **using** directive for the Models namespace.</span></span>

    ![Verwenden von Intellisense-Unterstützung für Namespacedeklarationen](build-restful-apis-with-aspnet-web-api/_static/image12.png)

    <span data-ttu-id="6cbc9-207">*Verwenden von Intellisense-Unterstützung für Namespacedeklarationen*</span><span class="sxs-lookup"><span data-stu-id="6cbc9-207">*Using Intellisense assistance for namespace declarations*</span></span>
5. <span data-ttu-id="6cbc9-208">Ändern Sie den Code für die **abrufen** Methode, sodass diese ein Array von Instanzen der Kontakt-Modell zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-208">Modify the code for the **Get** method so that it returns an array of Contact model instances.</span></span>

    <span data-ttu-id="6cbc9-209">(Codeausschnitt - *Web-API-Lab - Ex01 - Zurückgeben einer Liste von Kontakten*)</span><span class="sxs-lookup"><span data-stu-id="6cbc9-209">(Code Snippet - *Web API Lab - Ex01 - Returning a list of contacts*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample3.cs)]
6. <span data-ttu-id="6cbc9-210">Drücken Sie **F5** zum Debuggen der Webanwendung im Browser.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-210">Press **F5** to debug the web application in the browser.</span></span> <span data-ttu-id="6cbc9-211">Um die an die antwortausgabe der API vorgenommenen Änderungen anzuzeigen, führen Sie die folgenden Schritte aus.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-211">To view the changes made to the response output of the API, perform the following steps.</span></span>

   1. <span data-ttu-id="6cbc9-212">Nachdem der Browser geöffnet wird, drücken Sie **F12** , wenn die Entwicklertools noch nicht geöffnet sind.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-212">Once the browser opens, press **F12** if the developer tools are not open yet.</span></span>
   2. <span data-ttu-id="6cbc9-213">Klicken Sie auf die **Netzwerk** Registerkarte.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-213">Click the **Network** tab.</span></span>
   3. <span data-ttu-id="6cbc9-214">Drücken Sie die **starten erfassen** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-214">Press the **Start Capturing** button.</span></span>
   4. <span data-ttu-id="6cbc9-215">Fügen Sie die URL-Suffix **/api/Kontakt** an die URL in die Adressleiste ein und drücken Sie die **EINGABETASTE** Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-215">Add the URL suffix **/api/contact** to the URL in the address bar and press the **Enter** key.</span></span>
   5. <span data-ttu-id="6cbc9-216">Drücken Sie die **wechseln Sie zur detaillierten Ansicht** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-216">Press the **Go to detailed view** button.</span></span>
   6. <span data-ttu-id="6cbc9-217">Wählen Sie die **Antworttext** Registerkarte. Daraufhin sollte eine JSON-Zeichenfolge, die die serialisierte Form eines Arrays von Kontakt Instanzen darstellt.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-217">Select the **Response body** tab. You should see a JSON string representing the serialized form of an array of Contact instances.</span></span>

      <span data-ttu-id="6cbc9-218">![JSON-serialisierten Ausgabe eines komplexen Web-API-Methodenaufrufs](build-restful-apis-with-aspnet-web-api/_static/image13.png "JSON serialisierten Ausgabe des einen komplexen Web-API-Methodenaufruf")</span><span class="sxs-lookup"><span data-stu-id="6cbc9-218">![JSON serialized output of a complex Web API method call](build-restful-apis-with-aspnet-web-api/_static/image13.png "JSON serialized output of a complex Web API method call")</span></span>

      <span data-ttu-id="6cbc9-219">*JSON-serialisierten Ausgabe des einen komplexen Web-API-Methodenaufruf*</span><span class="sxs-lookup"><span data-stu-id="6cbc9-219">*JSON serialized output of a complex Web API method call*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Extracting_Functionality_into_a_Service_Layer"></a>
#### <a name="task-4---extracting-functionality-into-a-service-layer"></a><span data-ttu-id="6cbc9-220">Aufgabe 4 - extrahieren Funktionalität in einer Dienstebene</span><span class="sxs-lookup"><span data-stu-id="6cbc9-220">Task 4 - Extracting Functionality into a Service Layer</span></span>

<span data-ttu-id="6cbc9-221">Diese Aufgabe zeigen, wie Funktionen in einer Dienstebene zu erleichtern Entwicklern, trennen ihre Service-Funktionalität von der Controller-Ebene, wodurch die Wiederverwendung der Dienste, die die Verarbeitung durch erfolgt extrahieren.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-221">This task will demonstrate how to extract functionality into a Service layer to make it easy for developers to separate their service functionality from the controller layer, thereby allowing reusability of the services that actually do the work.</span></span>

1. <span data-ttu-id="6cbc9-222">Erstellen Sie einen neuen Ordner im Projektmappen-Stammverzeichnis und nennen sie **Services**.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-222">Create a new folder in the solution root and name it **Services**.</span></span> <span data-ttu-id="6cbc9-223">Zu diesem Zweck Maustaste **ContactManager** -Projekt, wählen **hinzufügen** | **neuer Ordner**, nennen Sie sie *Services*.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-223">To do this, right-click **ContactManager** project, select **Add** | **New Folder**, name it *Services*.</span></span>

    <span data-ttu-id="6cbc9-224">![Erstellen der Ordner "Dienste"](build-restful-apis-with-aspnet-web-api/_static/image14.png "Ordner \"Dienste erstellen\"")</span><span class="sxs-lookup"><span data-stu-id="6cbc9-224">![Creating Services folder](build-restful-apis-with-aspnet-web-api/_static/image14.png "Creating Services folder")</span></span>

    <span data-ttu-id="6cbc9-225">*Erstellen Ordner "Dienste"*</span><span class="sxs-lookup"><span data-stu-id="6cbc9-225">*Creating Services folder*</span></span>
2. <span data-ttu-id="6cbc9-226">Mit der rechten Maustaste die **Services** Ordner, und wählen **hinzufügen | Klasse...**  aus dem Kontextmenü.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-226">Right-click the **Services** folder and select **Add | Class...** from the context menu.</span></span>

    <span data-ttu-id="6cbc9-227">![Eine neue Klasse hinzufügen, um den Ordner "Dienste"](build-restful-apis-with-aspnet-web-api/_static/image15.png "eine neue Klasse hinzufügen, um den Ordner \"Dienste\"")</span><span class="sxs-lookup"><span data-stu-id="6cbc9-227">![Adding a new class to the Services folder](build-restful-apis-with-aspnet-web-api/_static/image15.png "Adding a new class to the Services folder")</span></span>

    <span data-ttu-id="6cbc9-228">*Eine neue Klasse hinzufügen, um den Ordner "Dienste"*</span><span class="sxs-lookup"><span data-stu-id="6cbc9-228">*Adding a new class to the Services folder*</span></span>
3. <span data-ttu-id="6cbc9-229">Wenn die **neues Element hinzufügen** Dialogfeld angezeigt wird, benennen Sie die neue Klasse **ContactRepository** , und klicken Sie auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-229">When the **Add New Item** dialog appears, name the new class **ContactRepository** and click **Add**.</span></span>

    <span data-ttu-id="6cbc9-230">![Erstellen eine Klassendatei, um den Code für die Dienstebene Kontakt Repository enthalten](build-restful-apis-with-aspnet-web-api/_static/image16.png "erstellen eine Klassendatei, um den Code für die Dienstebene Kontakt Repository enthalten")</span><span class="sxs-lookup"><span data-stu-id="6cbc9-230">![Creating a class file to contain the code for the Contact Repository service layer](build-restful-apis-with-aspnet-web-api/_static/image16.png "Creating a class file to contain the code for the Contact Repository service layer")</span></span>

    <span data-ttu-id="6cbc9-231">*Erstellen eine Klassendatei, um den Code für die Dienstebene Kontakt Repository enthalten*</span><span class="sxs-lookup"><span data-stu-id="6cbc9-231">*Creating a class file to contain the code for the Contact Repository service layer*</span></span>
4. <span data-ttu-id="6cbc9-232">Fügen Sie eine using-Direktive hinzu der **ContactRepository.cs** Datei, die den Modellen-Namespace enthalten.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-232">Add a using directive to the **ContactRepository.cs** file to include the models namespace.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample4.cs)]
5. <span data-ttu-id="6cbc9-233">Fügen Sie den folgenden hervorgehobenen Code in die **ContactRepository.cs** Datei GetAllContacts Methode implementiert.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-233">Add the following highlighted code to the **ContactRepository.cs** file to implement GetAllContacts method.</span></span>

    <span data-ttu-id="6cbc9-234">(Codeausschnitt - *Web-API-Lab - Ex01 - Kontaktrepository*)</span><span class="sxs-lookup"><span data-stu-id="6cbc9-234">(Code Snippet - *Web API Lab - Ex01 - Contact Repository*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample5.cs)]
6. <span data-ttu-id="6cbc9-235">Öffnen Sie die **ContactController.cs** Datei, sofern dieser nicht bereits geöffnet ist.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-235">Open the **ContactController.cs** file if it is not already open.</span></span>
7. <span data-ttu-id="6cbc9-236">Fügen Sie die folgende Anweisung verwenden, mit dem Namespace-Deklaration-Abschnitt der Datei.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-236">Add the following using statement to the namespace declaration section of the file.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample6.cs)]
8. <span data-ttu-id="6cbc9-237">Fügen Sie den folgenden hervorgehobenen Code in die **ContactController.cs** Klasse, um ein privates Feld für die Darstellung der Instanz des Repositorys, hinzuzufügen, sodass der Rest der Klasse, die Mitglieder ausführen können mithilfe der dienstimplementierung.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-237">Add the following highlighted code to the **ContactController.cs** class to add a private field to represent the instance of the repository, so that the rest of the class members can make use of the service implementation.</span></span>

    <span data-ttu-id="6cbc9-238">(Codeausschnitt - *Web-API-Lab - Ex01 - Kontakt Controller*)</span><span class="sxs-lookup"><span data-stu-id="6cbc9-238">(Code Snippet - *Web API Lab - Ex01 - Contact Controller*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample7.cs)]
9. <span data-ttu-id="6cbc9-239">Ändern der **abrufen** Methode, sodass die It stellt den Kontaktrepository-Dienst verwenden.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-239">Change the **Get** method so that it makes use of the contact repository service.</span></span>

    <span data-ttu-id="6cbc9-240">(Codeausschnitt - *Web-API-Lab - Ex01 - Zurückgeben einer Liste von Kontakten über das Repository*)</span><span class="sxs-lookup"><span data-stu-id="6cbc9-240">(Code Snippet - *Web API Lab - Ex01 - Returning a list of contacts via the repository*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample8.cs)]
10. <span data-ttu-id="6cbc9-241">Fügen Sie einen Haltepunkt in der **ContactController**des **abrufen** Methodendefinition.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-241">Put a breakpoint on the **ContactController**'s **Get** method definition.</span></span>

   <span data-ttu-id="6cbc9-242">![Haltepunkte hinzufügen, mit dem Kontakt Controller](build-restful-apis-with-aspnet-web-api/_static/image17.png "Haltepunkte mit dem Kontakt Controller hinzufügen")</span><span class="sxs-lookup"><span data-stu-id="6cbc9-242">![Adding breakpoints to the contact controller](build-restful-apis-with-aspnet-web-api/_static/image17.png "Adding breakpoints to the contact controller")</span></span>

   <span data-ttu-id="6cbc9-243">*Haltepunkte mit dem Kontakt Controller hinzufügen*</span><span class="sxs-lookup"><span data-stu-id="6cbc9-243">*Adding breakpoints to the contact controller*</span></span>
11. <span data-ttu-id="6cbc9-244">Drücken Sie **F5**, um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-244">Press **F5** to run the application.</span></span>
12. <span data-ttu-id="6cbc9-245">Wenn der Browser geöffnet wird, drücken Sie die **F12** zu den Entwicklertools zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-245">When the browser opens, press **F12** to open the developer tools.</span></span>
13. <span data-ttu-id="6cbc9-246">Klicken Sie auf die **Netzwerk** Registerkarte.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-246">Click the **Network** tab.</span></span>
14. <span data-ttu-id="6cbc9-247">Klicken Sie auf die **starten erfassen** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-247">Click the **Start Capturing** button.</span></span>
15. <span data-ttu-id="6cbc9-248">Fügen Sie die URL in die Adressleiste ein, mit dem Suffix **/api/Kontakt** , und drücken Sie **EINGABETASTE** beim Laden der API-Controller.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-248">Append the URL in the address bar with the suffix **/api/contact** and press **Enter** to load the API controller.</span></span>
16. <span data-ttu-id="6cbc9-249">Visual Studio 2012 sollte einmal unterbrechen **abrufen** Methode startet die Ausführung.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-249">Visual Studio 2012 should break once **Get** method begins execution.</span></span>

   <span data-ttu-id="6cbc9-250">![Innerhalb der Methode Get wichtige](build-restful-apis-with-aspnet-web-api/_static/image18.png "wichtige innerhalb der Get-Methode")</span><span class="sxs-lookup"><span data-stu-id="6cbc9-250">![Breaking within the Get method](build-restful-apis-with-aspnet-web-api/_static/image18.png "Breaking within the Get method")</span></span>

   <span data-ttu-id="6cbc9-251">*Wichtige in der Get-Methode*</span><span class="sxs-lookup"><span data-stu-id="6cbc9-251">*Breaking within the Get method*</span></span>
17. <span data-ttu-id="6cbc9-252">Drücken Sie **F5**, um fortzufahren.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-252">Press **F5** to continue.</span></span>
18. <span data-ttu-id="6cbc9-253">Wechseln Sie zurück in Internet Explorer, wenn es nicht bereits im Fokus ist.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-253">Go back to Internet Explorer if it is not already in focus.</span></span> <span data-ttu-id="6cbc9-254">Beachten Sie das Fenster "Netzwerk Capture" aus.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-254">Note the network capture window.</span></span>

    <span data-ttu-id="6cbc9-255">![Netzwerk-Ansicht in Internet Explorer, dabei werden die Ergebnisse des Web-API-Aufrufs](build-restful-apis-with-aspnet-web-api/_static/image19.png "Netzwerk in Internet Explorer, dabei werden die Ergebnisse des Web-API-Aufrufs anzeigen")</span><span class="sxs-lookup"><span data-stu-id="6cbc9-255">![Network view in Internet Explorer showing results of the Web API call](build-restful-apis-with-aspnet-web-api/_static/image19.png "Network view in Internet Explorer showing results of the Web API call")</span></span>

    <span data-ttu-id="6cbc9-256">*Ansicht in Internet Explorer, dabei werden die Ergebnisse der Web-API-Aufruf*</span><span class="sxs-lookup"><span data-stu-id="6cbc9-256">*Network view in Internet Explorer showing results of the Web API call*</span></span>
19. <span data-ttu-id="6cbc9-257">Klicken Sie auf die **wechseln Sie zur detaillierten Ansicht** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-257">Click the **Go to detailed view** button.</span></span>
20. <span data-ttu-id="6cbc9-258">Klicken Sie auf die **Antworttext** Registerkarte. Beachten Sie die JSON-Ausgabe der API-Aufruf, und wie die beiden Kontakte abgerufen, indem die Dienstebene dar.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-258">Click the **Response body** tab. Note the JSON output of the API call, and how it represents the two contacts retrieved by the service layer.</span></span>

    <span data-ttu-id="6cbc9-259">![Anzeigen von JSON-Ausgabe aus der Web-API in das Fenster "Developer"](build-restful-apis-with-aspnet-web-api/_static/image20.png "die JSON-Ausgabe aus der Web-API in das Fenster \"Developer\" anzeigen")</span><span class="sxs-lookup"><span data-stu-id="6cbc9-259">![Viewing the JSON output from the Web API in the developer tools window](build-restful-apis-with-aspnet-web-api/_static/image20.png "Viewing the JSON output from the Web API in the developer tools window")</span></span>

    <span data-ttu-id="6cbc9-260">*Die JSON-Ausgabe aus der Web-API anzeigen in das Fenster "Developer"*</span><span class="sxs-lookup"><span data-stu-id="6cbc9-260">*Viewing the JSON output from the Web API in the developer tools window*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Create_a_ReadWrite_Web_API"></a>
### <a name="exercise-2-create-a-readwrite-web-api"></a><span data-ttu-id="6cbc9-261">Übung 2: Erstellen einer Lese-/Schreibzugriff-Webs-API</span><span class="sxs-lookup"><span data-stu-id="6cbc9-261">Exercise 2: Create a Read/Write Web API</span></span>

<span data-ttu-id="6cbc9-262">In dieser Übung implementieren Sie POST und PUT-Methoden für den Kontakt-Manager zum Bearbeiten von Daten von Funktionen zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-262">In this exercise, you will implement POST and PUT methods for the contact manager to enable it with data-editing features.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Opening_the_Web_API_Project"></a>
#### <a name="task-1---opening-the-web-api-project"></a><span data-ttu-id="6cbc9-263">Aufgabe 1: Öffnen das Web-API-Projekt</span><span class="sxs-lookup"><span data-stu-id="6cbc9-263">Task 1 - Opening the Web API Project</span></span>

<span data-ttu-id="6cbc9-264">In dieser Aufgabe bereiten Sie verbessern die Web-API-Projekt in Übung 1 erstellt werden, sodass sie Benutzereingaben akzeptieren kann.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-264">In this task, you will prepare to enhance the Web API project created in Exercise 1 so that it can accept user input.</span></span>

1. <span data-ttu-id="6cbc9-265">Führen Sie **zuerst Visual Studio 2012 Express für Web**, wechseln Sie zu diesem möchten **starten** und Typ **Visual Studio Express für Web** drücken Sie dann die **EINGABETASTE**.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-265">Run **Visual Studio 2012 Express for Web**, to do this go to **Start** and type **VS Express for Web** then press **Enter**.</span></span>
2. <span data-ttu-id="6cbc9-266">Öffnen der **beginnen** Lösung finden Sie unter **Quell-/Ex02-ReadWriteWebAPI/Begin/** Ordner.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-266">Open the **Begin** solution located at **Source/Ex02-ReadWriteWebAPI/Begin/** folder.</span></span> <span data-ttu-id="6cbc9-267">Andernfalls möglicherweise weiterhin mithilfe der **End** Lösung abgerufen, von der vorherigen Übung abschließen.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-267">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="6cbc9-268">Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung müssen Sie einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-268">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="6cbc9-269">Klicken Sie hierzu auf die **Projekt** Menü **NuGet-Pakete verwalten**.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-269">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="6cbc9-270">In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-270">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="6cbc9-271">Schließlich erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-271">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="6cbc9-272">Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken, die sich in Ihrem Projekt liefern Verringern der Projektgröße.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-272">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="6cbc9-273">Mit NuGet-Powertools werden durch Angabe der Paketversionen in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, wenn, das Sie das Projekt ausführen, können.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-273">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="6cbc9-274">Deshalb wird müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-274">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="6cbc9-275">Öffnen der **Services/ContactRepository.cs** Datei.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-275">Open the **Services/ContactRepository.cs** file.</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_Adding_Data-Persistence_Features_to_the_Contact_Repository_Implementation"></a>
#### <a name="task-2---adding-data-persistence-features-to-the-contact-repository-implementation"></a><span data-ttu-id="6cbc9-276">Aufgabe 2: Hinzufügen von Datenpersistenz Funktionen für die Implementierung Kontaktrepository</span><span class="sxs-lookup"><span data-stu-id="6cbc9-276">Task 2 - Adding Data-Persistence Features to the Contact Repository Implementation</span></span>

<span data-ttu-id="6cbc9-277">In dieser Aufgabe wird die ContactRepository-Klasse, der das Web-API-Projekt, die in Übung 1 erstellt haben, damit es beibehalten und von Benutzereingaben und neuer Kontakt-Instanzen annehmen kann erweitert werden.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-277">In this task, you will augment the ContactRepository class of the Web API project created in Exercise 1 so that it can persist and accept user input and new Contact instances.</span></span>

1. <span data-ttu-id="6cbc9-278">Die folgende Konstante zum Hinzufügen der **ContactRepository** Klasse, um den Namen des Cache Element Schlüssel den Namen des Webservers weiter unten in dieser Übung darzustellen.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-278">Add the following constant to the **ContactRepository** class to represent the name of the web server cache item key name later in this exercise.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample9.cs)]
2. <span data-ttu-id="6cbc9-279">Fügen Sie einen Konstruktor, um die **ContactRepository** mit dem folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-279">Add a constructor to the **ContactRepository** containing the following code.</span></span>

    <span data-ttu-id="6cbc9-280">(Codeausschnitt - *Web-API-Lab - Ex02 - Kontaktrepository Konstruktor*)</span><span class="sxs-lookup"><span data-stu-id="6cbc9-280">(Code Snippet - *Web API Lab - Ex02 - Contact Repository Constructor*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample10.cs)]
3. <span data-ttu-id="6cbc9-281">Ändern Sie den Code für die **GetAllContacts** Methode wie nachstehend gezeigt.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-281">Modify the code for the **GetAllContacts** method as demonstrated below.</span></span>

    <span data-ttu-id="6cbc9-282">(Codeausschnitt - *Web-API-Lab - Ex02 - Abrufen aller Kontakte*)</span><span class="sxs-lookup"><span data-stu-id="6cbc9-282">(Code Snippet - *Web API Lab - Ex02 - Get All Contacts*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample11.cs)]

    > [!NOTE]
    > <span data-ttu-id="6cbc9-283">In diesem Beispiel wird zu Demonstrationszwecken und verwendet die Webserver Cache als einem Speichermedium, damit die Werte werden gleichzeitig an mehrere Clients verfügbar sein, anstatt eine Sitzung Speichermechanismus oder eine Anforderung Speicher Lebensdauer verwenden.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-283">This example is for demonstration purposes and will use the web server's cache as a storage medium, so that the values will be available to multiple clients simultaneously, rather than use a Session storage mechanism or a Request storage lifetime.</span></span> <span data-ttu-id="6cbc9-284">Eine konnte Entity Framework, XML-Speicherung oder anderen verschiedener anstelle der Cache des Webservers verwenden.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-284">One could use Entity Framework, XML storage, or any other variety in place of the web server cache.</span></span>
4. <span data-ttu-id="6cbc9-285">Implementieren Sie eine neue Methode mit dem Namen **SaveContact** auf die **ContactRepository** Klasse zum Durchführen der Aufgaben ein Kontakts zu speichern.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-285">Implement a new method named **SaveContact** to the **ContactRepository** class to do the work of saving a contact.</span></span> <span data-ttu-id="6cbc9-286">Die **SaveContact** -Methode sollte ein einzelnes annehmen **Kontakt** Parameter- und Rückgabetypen ein boolescher Wert, Erfolg oder Fehler angibt.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-286">The **SaveContact** method should take a single **Contact** parameter and return a Boolean value indicating success or failure.</span></span>

    <span data-ttu-id="6cbc9-287">(Codeausschnitt - *Web-API-Lab - Ex02 - Implementierung der SaveContact-Methode*)</span><span class="sxs-lookup"><span data-stu-id="6cbc9-287">(Code Snippet - *Web API Lab - Ex02 - Implementing the SaveContact Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample12.cs)]

<a id="Exercise3"></a>

<a id="Exercise_3_Consume_the_Web_API_from_an_HTML_Client"></a>
### <a name="exercise-3-consume-the-web-api-from-an-html-client"></a><span data-ttu-id="6cbc9-288">Übung 3: Verwenden der Webs-API aus einem HTML-Client</span><span class="sxs-lookup"><span data-stu-id="6cbc9-288">Exercise 3: Consume the Web API from an HTML Client</span></span>

<span data-ttu-id="6cbc9-289">In dieser Übung erstellen Sie einen HTML-Client, um die Web-API aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-289">In this exercise, you will create an HTML client to call the Web API.</span></span> <span data-ttu-id="6cbc9-290">Dieser Client wird Datenaustausch mit der Web-API, die mit JavaScript vereinfachen und zeigt die Ergebnisse in einem Webbrowser mit HTML-Markup.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-290">This client will facilitate data exchange with the Web API using JavaScript and will display the results in a web browser using HTML markup.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Displaying_Contacts"></a>
#### <a name="task-1---modifying-the-index-view-to-provide-a-gui-for-displaying-contacts"></a><span data-ttu-id="6cbc9-291">Aufgabe 1 – ändern die Indexansicht, um eine GUI bereitstellen, für die Anzeige von Kontakten</span><span class="sxs-lookup"><span data-stu-id="6cbc9-291">Task 1 - Modifying the Index View to Provide a GUI for Displaying Contacts</span></span>

<span data-ttu-id="6cbc9-292">In dieser Aufgabe ändern Sie die Standardansicht des Index der Webanwendung, die die Anforderung zum Anzeigen von der Liste der existierenden Kontakte in einem HTML-Browser zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-292">In this task, you will modify the default Index view of the web application to support the requirement of displaying the list of existing contacts in an HTML browser.</span></span>

1. <span data-ttu-id="6cbc9-293">Öffnen Sie **zuerst Visual Studio 2012 Express für Web** , wenn er nicht bereits geöffnet ist.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-293">Open **Visual Studio 2012 Express for Web** if it is not already open.</span></span>
2. <span data-ttu-id="6cbc9-294">Öffnen der **beginnen** Lösung finden Sie unter **Quell-/Ex03-ConsumingWebAPI/Begin/** Ordner.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-294">Open the **Begin** solution located at **Source/Ex03-ConsumingWebAPI/Begin/** folder.</span></span> <span data-ttu-id="6cbc9-295">Andernfalls möglicherweise weiterhin mithilfe der **End** Lösung abgerufen, von der vorherigen Übung abschließen.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-295">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="6cbc9-296">Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung müssen Sie einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-296">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="6cbc9-297">Klicken Sie hierzu auf die **Projekt** Menü **NuGet-Pakete verwalten**.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-297">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="6cbc9-298">In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-298">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="6cbc9-299">Schließlich erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-299">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="6cbc9-300">Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken, die sich in Ihrem Projekt liefern Verringern der Projektgröße.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-300">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="6cbc9-301">Mit NuGet-Powertools werden durch Angabe der Paketversionen in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, wenn, das Sie das Projekt ausführen, können.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-301">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="6cbc9-302">Deshalb wird müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-302">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="6cbc9-303">Öffnen der **Index.cshtml** Datei **Ansichten/Start** Ordner.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-303">Open the **Index.cshtml** file located at **Views/Home** folder.</span></span>
4. <span data-ttu-id="6cbc9-304">Ersetzen Sie den HTML-Code innerhalb des Div-Elements mit der Id **Text** , damit sie den folgenden Code ähnelt.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-304">Replace the HTML code within the div element with id **body** so that it looks like the following code.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample13.html)]
5. <span data-ttu-id="6cbc9-305">Fügen Sie den folgenden Javascript-Code am Ende der Datei zum Ausführen der HTTP-Anforderung an die Web-API.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-305">Add the following Javascript code at the bottom of the file to perform the HTTP request to the Web API.</span></span>

    [!code-cshtml[Main](build-restful-apis-with-aspnet-web-api/samples/sample14.cshtml)]
6. <span data-ttu-id="6cbc9-306">Öffnen Sie die **ContactController.cs** Datei, sofern dieser nicht bereits geöffnet ist.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-306">Open the **ContactController.cs** file if it is not already open.</span></span>
7. <span data-ttu-id="6cbc9-307">Fügen Sie einen Haltepunkt auf der **abrufen** Methode der **ContactController** Klasse.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-307">Place a breakpoint on the **Get** method of the **ContactController** class.</span></span>

    <span data-ttu-id="6cbc9-308">![Platzieren einen Haltepunkt für die Get-Methode von der API-Controller](build-restful-apis-with-aspnet-web-api/_static/image21.png "einen Haltepunkt einfügen, auf die Get-Methode von der API-Controller")</span><span class="sxs-lookup"><span data-stu-id="6cbc9-308">![Placing a breakpoint on the Get method of the API controller](build-restful-apis-with-aspnet-web-api/_static/image21.png "Placing a breakpoint on the Get method of the API controller")</span></span>

    <span data-ttu-id="6cbc9-309">*Platzieren einen Haltepunkt für die Get-Methode von der API-controller*</span><span class="sxs-lookup"><span data-stu-id="6cbc9-309">*Placing a breakpoint on the Get method of the API controller*</span></span>
8. <span data-ttu-id="6cbc9-310">Drücken Sie **F5** um das Projekt auszuführen.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-310">Press **F5** to run the project.</span></span> <span data-ttu-id="6cbc9-311">Das HTML-Dokument wird im Browser geladen werden.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-311">The browser will load the HTML document.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6cbc9-312">Stellen Sie sicher, dass Sie an die Stamm-URL Ihrer Anwendung durchsuchen möchten.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-312">Ensure that you are browsing to the root URL of your application.</span></span>
9. <span data-ttu-id="6cbc9-313">Nach dem Laden der Seite, und der JavaScript-Code ausführt, wird der Haltepunkt erreicht, und hält die Ausführung des Codes im Controller.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-313">Once the page loads and the JavaScript executes, the breakpoint will be hit and the code execution will pause in the controller.</span></span>

    <span data-ttu-id="6cbc9-314">![Debuggen in der Web-API-Aufrufe, die mit Visual Studio Express für Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "Debuggen in der Web-API-Aufrufe, die mit Visual Studio Express für Web")</span><span class="sxs-lookup"><span data-stu-id="6cbc9-314">![Debugging into the Web API calls using VS Express for Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "Debugging into the Web API calls using VS Express for Web")</span></span>

    <span data-ttu-id="6cbc9-315">*Debuggen in der Web-API-Aufruf mithilfe von Visual Studio 2012 Express für Web*</span><span class="sxs-lookup"><span data-stu-id="6cbc9-315">*Debugging into the Web API call using Visual Studio 2012 Express for Web*</span></span>
10. <span data-ttu-id="6cbc9-316">Entfernen Sie den Haltepunkt, und drücken Sie **F5** oder der Symbolleiste Debuggen **Fortfahren** um den Vorgang fortzusetzen, laden die Ansicht im Browser.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-316">Remove the breakpoint and press **F5** or the debugging toolbar's **Continue** button to continue loading the view in the browser.</span></span> <span data-ttu-id="6cbc9-317">Nach Abschluss der Web-API-Aufruf sollte aus der Web-API zurückgegebenen Kontakte aufrufen, dargestellt als Listenelemente im Browser angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-317">Once the Web API call completes you should see the contacts returned from the Web API call displayed as list items in the browser.</span></span>

    <span data-ttu-id="6cbc9-318">![Ergebnisse von der API-Aufruf im Browser angezeigt wird, als Listenelemente](build-restful-apis-with-aspnet-web-api/_static/image23.png "Ergebnisse dem API-Aufruf im Browser angezeigt wird, als Listenelemente")</span><span class="sxs-lookup"><span data-stu-id="6cbc9-318">![Results of the API call displayed in the browser as list items](build-restful-apis-with-aspnet-web-api/_static/image23.png "Results of the API call displayed in the browser as list items")</span></span>

    <span data-ttu-id="6cbc9-319">*Ergebnisse von der API-Aufruf im Browser angezeigt wird, als Listenelemente*</span><span class="sxs-lookup"><span data-stu-id="6cbc9-319">*Results of the API call displayed in the browser as list items*</span></span>
11. <span data-ttu-id="6cbc9-320">Beenden des Debuggens.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-320">Stop debugging.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Creating_Contacts"></a>
#### <a name="task-2---modifying-the-index-view-to-provide-a-gui-for-creating-contacts"></a><span data-ttu-id="6cbc9-321">Aufgabe 2: ändern die Indexansicht, um eine GUI zu bieten, zum Erstellen von Kontakten</span><span class="sxs-lookup"><span data-stu-id="6cbc9-321">Task 2 - Modifying the Index View to Provide a GUI for Creating Contacts</span></span>

<span data-ttu-id="6cbc9-322">In dieser Aufgabe werden Sie weiterhin die Indexansicht MVC-Anwendung zu ändern.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-322">In this task, you will continue to modify the Index view of the MVC application.</span></span> <span data-ttu-id="6cbc9-323">Die HTML-Seite, die Benutzereingaben erfassen und senden sie an die Web-API zum Erstellen eines neuen Kontakts wird ein Formular hinzugefügt werden, und eine neue Web-API-Controller-Methode wird erstellt, um das Datum aus der GUI zu sammeln.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-323">A form will be added to the HTML page that will capture user input and send it to the Web API to create a new Contact, and a new Web API controller method will be created to collect date from the GUI.</span></span>

1. <span data-ttu-id="6cbc9-324">Öffnen der **ContactController.cs** Datei.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-324">Open the **ContactController.cs** file.</span></span>
2. <span data-ttu-id="6cbc9-325">Fügen Sie eine neue Methode für die Controllerklasse, die mit dem Namen **Post** wie im folgenden Code gezeigt.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-325">Add a new method to the controller class named **Post** as shown in the following code.</span></span>

    <span data-ttu-id="6cbc9-326">(Codeausschnitt - *Web-API-Lab - Ex03 - Post-Methode*)</span><span class="sxs-lookup"><span data-stu-id="6cbc9-326">(Code Snippet - *Web API Lab - Ex03 - Post Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample15.cs)]
3. <span data-ttu-id="6cbc9-327">Öffnen Sie die **Index.cshtml** Datei in Visual Studio, wenn er nicht bereits geöffnet ist.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-327">Open the **Index.cshtml** file in Visual Studio if it is not already open.</span></span>
4. <span data-ttu-id="6cbc9-328">Fügen Sie den folgenden HTML-Code in die Datei direkt hinter dem ungeordnete Liste, die Sie in der vorherigen Aufgabe hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-328">Add the HTML code below to the file just after the unordered list you added in the previous task.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample16.html)]
5. <span data-ttu-id="6cbc9-329">Fügen Sie in das Script-Element am unteren Rand des Dokuments um Schaltfläche – Click-Ereignisse zu behandeln, die die Daten an die Web-API bereitstellen werden, über eine HTTP POST-Aufruf die folgenden hervorgehobenen Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-329">Within the script element at the bottom of the document, add the following highlighted code to handle button-click events, which will post the data to the Web API using an HTTP POST call.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample17.html)]
6. <span data-ttu-id="6cbc9-330">In **ContactController.cs**, fügen Sie einen Haltepunkt auf der **Post** Methode.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-330">In **ContactController.cs**, place a breakpoint on the **Post** method.</span></span>
7. <span data-ttu-id="6cbc9-331">Drücken Sie **F5** zum Ausführen der Anwendung im Browser.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-331">Press **F5** to run the application in the browser.</span></span>
8. <span data-ttu-id="6cbc9-332">Sobald die Seite im Browser geladen wird, geben Sie in einem neuen Kontaktnamen und -Id und klicken Sie auf die **speichern** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-332">Once the page is loaded in the browser, type in a new contact name and Id and click the **Save** button.</span></span>

    <span data-ttu-id="6cbc9-333">![Das Client-HTML-Dokument im Browser geladen](build-restful-apis-with-aspnet-web-api/_static/image24.png "das Client-HTML-Dokument im Browser geladen.")</span><span class="sxs-lookup"><span data-stu-id="6cbc9-333">![The client HTML document loaded in the browser](build-restful-apis-with-aspnet-web-api/_static/image24.png "The client HTML document loaded in the browser")</span></span>

    <span data-ttu-id="6cbc9-334">*Die Client-HTML-Dokument im Browser geladen.*</span><span class="sxs-lookup"><span data-stu-id="6cbc9-334">*The client HTML document loaded in the browser*</span></span>
9. <span data-ttu-id="6cbc9-335">Wenn im Debuggerfenster Zeilenumbrüche, in der **Post** -Methode, erstellen Sie eine Blick auf die Eigenschaften des der **wenden Sie sich an** Parameter.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-335">When the debugger window breaks in the **Post** method, take a look at the properties of the **contact** parameter.</span></span> <span data-ttu-id="6cbc9-336">Die Werte übereinstimmen, die Daten, die Sie in das Formular eingegeben haben.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-336">The values should match the data you entered in the form.</span></span>

    <span data-ttu-id="6cbc9-337">![Die Kontakt-Objekt, das an die Web-API vom Client gesendeten](build-restful-apis-with-aspnet-web-api/_static/image25.png "die Kontakt-Objekt, die an die Web-API vom Client gesendet werden")</span><span class="sxs-lookup"><span data-stu-id="6cbc9-337">![The Contact object being sent to the Web API from the client](build-restful-apis-with-aspnet-web-api/_static/image25.png "The Contact object being sent to the Web API from the client")</span></span>

    <span data-ttu-id="6cbc9-338">*Die Kontakt-Objekt, das an die Web-API vom Client gesendeten*</span><span class="sxs-lookup"><span data-stu-id="6cbc9-338">*The Contact object being sent to the Web API from the client*</span></span>
10. <span data-ttu-id="6cbc9-339">Durchlaufen Sie die Methode im Debugger bis der **Antwort** Variable erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-339">Step through the method in the debugger until the **response** variable has been created.</span></span> <span data-ttu-id="6cbc9-340">Bei der Überprüfung in der **"lokal"** Fenster im Debugger, werden Sie feststellen, dass alle Eigenschaften festgelegt wurden.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-340">Upon inspection in the **Locals** window in the debugger, you'll see that all the properties have been set.</span></span>

   <span data-ttu-id="6cbc9-341">![Die Antwort nach der Erstellung im Debugger](build-restful-apis-with-aspnet-web-api/_static/image26.png "die Antwort nach der Erstellung im Debugger")</span><span class="sxs-lookup"><span data-stu-id="6cbc9-341">![The response following creation in the debugger](build-restful-apis-with-aspnet-web-api/_static/image26.png "The response following creation in the debugger")</span></span>

   <span data-ttu-id="6cbc9-342">*Die Antwort nach der Erstellung im debugger*</span><span class="sxs-lookup"><span data-stu-id="6cbc9-342">*The response following creation in the debugger*</span></span>
11. <span data-ttu-id="6cbc9-343">Wenn Sie drücken **F5** oder klicken Sie auf **Fortfahren** im Debugger die Anforderung abgeschlossen wird.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-343">If you press **F5** or click **Continue** in the debugger the request will complete.</span></span> <span data-ttu-id="6cbc9-344">Sobald Sie wieder an den Browser wechseln, der neue Kontakt hinzugefügt wurde der Liste der Kontakte, die vom gespeicherten der **ContactRepository** Implementierung.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-344">Once you switch back to the browser, the new contact has been added to the list of contacts stored by the **ContactRepository** implementation.</span></span>

   <span data-ttu-id="6cbc9-345">![Der Browser gibt bei erfolgreicher Erstellung des neuen Kontakts Instanz wieder](build-restful-apis-with-aspnet-web-api/_static/image27.png "der Browser gibt bei erfolgreicher Erstellung des neuen Kontakts Instanz wieder")</span><span class="sxs-lookup"><span data-stu-id="6cbc9-345">![The browser reflects successful creation of the new contact instance](build-restful-apis-with-aspnet-web-api/_static/image27.png "The browser reflects successful creation of the new contact instance")</span></span>

   <span data-ttu-id="6cbc9-346">*Der Browser gibt bei erfolgreicher Erstellung des neuen Kontakts Instanz wieder*</span><span class="sxs-lookup"><span data-stu-id="6cbc9-346">*The browser reflects successful creation of the new contact instance*</span></span>

> [!NOTE]
> <span data-ttu-id="6cbc9-347">Darüber hinaus können Sie die Bereitstellung dieser Anwendung auf Azure Folgendes [Anhang C: Veröffentlichen einer ASP.NET MVC 4-Anwendung mithilfe von Web Deploy](#AppendixC).</span><span class="sxs-lookup"><span data-stu-id="6cbc9-347">Additionally, you can deploy this application to Azure following [Appendix C: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixC).</span></span>


* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="6cbc9-348">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="6cbc9-348">Summary</span></span>

<span data-ttu-id="6cbc9-349">In dieser Umgebung wurde das neue ASP.NET Web API-Framework und die Implementierung von RESTful Web-APIs, die mithilfe des Frameworks eingeführt.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-349">This lab has introduced you to the new ASP.NET Web API framework and to the implementation of RESTful Web APIs using the framework.</span></span> <span data-ttu-id="6cbc9-350">Von hier aus können Sie ein neues Repository, das die Dauerhaftigkeit von Daten, die eine beliebige Anzahl von Mechanismen vereinfacht erstellen und verknüpfen diesen Dienst statt der einfachen eine in dieser Übungseinheit als Beispiel bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-350">From here, you could create a new repository that facilitates data persistence using any number of mechanisms and wire that service up rather than the simple one provided as an example in this lab.</span></span> <span data-ttu-id="6cbc9-351">Web-API unterstützt eine Reihe zusätzlicher Funktionen, z. B. das Aktivieren der Kommunikation von nicht-HTML-Clients, die in einer beliebigen Sprache, die von HTTP und JSON oder XML unterstützt geschrieben wurden.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-351">Web API supports a number of additional features, such as enabling communication from non-HTML clients written in any language that supports HTTP and JSON or XML.</span></span> <span data-ttu-id="6cbc9-352">Die Möglichkeit, eine Web-API außerhalb einer typischen Webanwendung gehostet ist auch möglich, als auch ist die Möglichkeit, eigene Serialisierungsformate erstellen.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-352">The ability to host a Web API outside of a typical web application is also possible, as well as is the ability to create your own serialization formats.</span></span>

<span data-ttu-id="6cbc9-353">Die ASP.NET-Website verfügt über einen speziellen Bereich für die ASP.NET Web API-Framework unter [ [ https://asp.net/web-api ](https://asp.net/web-api) ](https://asp.net/web-api).</span><span class="sxs-lookup"><span data-stu-id="6cbc9-353">The ASP.NET Web site has an area dedicated to the ASP.NET Web API framework at [[https://asp.net/web-api](https://asp.net/web-api)](https://asp.net/web-api).</span></span> <span data-ttu-id="6cbc9-354">Dieser Standort wird weiterhin bereitstellen, hochaktuelle Informationen, Beispiele und Nachrichten, die im Zusammenhang mit der Web-API so checken Sie es häufig, wenn Sie tiefer in die Kunst zum Erstellen von benutzerdefinierten Web-APIs auf praktisch jedem Gerät oder einer Entwicklungsumgebung Framework vertiefen möchten.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-354">This site will continue to provide late-breaking information, samples, and news related to Web API, so check it frequently if you'd like to delve deeper into the art of creating custom Web APIs available to virtually any device or development framework.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a><span data-ttu-id="6cbc9-355">Anhang A: Verwenden von Codeausschnitten</span><span class="sxs-lookup"><span data-stu-id="6cbc9-355">Appendix A: Using Code Snippets</span></span>

<span data-ttu-id="6cbc9-356">Mit Codeausschnitten müssen Sie den Code, den Sie jederzeit griffbereit benötigen.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-356">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="6cbc9-357">Das Lab-Dokument erfahren Sie, wenn Sie genau, verwenden können wie in der folgenden Abbildung dargestellt.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-357">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="6cbc9-358">![Verwendung von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihr Projekt](build-restful-apis-with-aspnet-web-api/_static/image28.png "Codeausschnitte zum Einfügen von Code in Ihr Projekt mit Visual Studio")</span><span class="sxs-lookup"><span data-stu-id="6cbc9-358">![Using Visual Studio code snippets to insert code into your project](build-restful-apis-with-aspnet-web-api/_static/image28.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="6cbc9-359">*Verwendung von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihr Projekt*</span><span class="sxs-lookup"><span data-stu-id="6cbc9-359">*Using Visual Studio code snippets to insert code into your project*</span></span>

<a id="CodeSnippetUsingKeyBoard"></a>

<a id="To_add_a_code_snippet_using_the_keyboard_C_only"></a>
### <a name="to-add-a-code-snippet-using-the-keyboard-c-only"></a><span data-ttu-id="6cbc9-360">Hinzufügen ein Codeausschnitts, die mithilfe der Tastatur (nur c#)</span><span class="sxs-lookup"><span data-stu-id="6cbc9-360">To add a code snippet using the keyboard (C# only)</span></span>

1. <span data-ttu-id="6cbc9-361">Platzieren Sie den Cursor, wo möchten Sie den Code einfügen.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-361">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="6cbc9-362">Starten Sie den Namen des Ausschnitts (ohne Leerzeichen oder Bindestriche) eingeben.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-362">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="6cbc9-363">Beobachten Sie, wie IntelliSense zeigt übereinstimmenden Namen Ausschnitte.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-363">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="6cbc9-364">Wählen Sie den richtigen Ausschnitt (oder behalten Sie eingeben, bis der gesamte Ausschnitt Name ausgewählt ist).</span><span class="sxs-lookup"><span data-stu-id="6cbc9-364">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="6cbc9-365">Drücken Sie die Tab-Taste zweimal, um den Ausschnitt an der Cursorposition eingefügt.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-365">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

    <span data-ttu-id="6cbc9-366">![Geben Sie den Namen des Ausschnitts](build-restful-apis-with-aspnet-web-api/_static/image29.png "starten Sie den Namen des Ausschnitts eingeben")</span><span class="sxs-lookup"><span data-stu-id="6cbc9-366">![Start typing the snippet name](build-restful-apis-with-aspnet-web-api/_static/image29.png "Start typing the snippet name")</span></span>

    <span data-ttu-id="6cbc9-367">*Starten Sie den Namen des Ausschnitts eingeben*</span><span class="sxs-lookup"><span data-stu-id="6cbc9-367">*Start typing the snippet name*</span></span>

    <span data-ttu-id="6cbc9-368">![Drücken Sie Tab, auf den hervorgehobenen Ausschnitt](build-restful-apis-with-aspnet-web-api/_static/image30.png "drücken Sie die Tab hervorgehobenen Ausschnitt auswählen")</span><span class="sxs-lookup"><span data-stu-id="6cbc9-368">![Press Tab to select the highlighted snippet](build-restful-apis-with-aspnet-web-api/_static/image30.png "Press Tab to select the highlighted snippet")</span></span>

    <span data-ttu-id="6cbc9-369">*Drücken Sie Tab, um den markierten Ausschnitt auszuwählen*</span><span class="sxs-lookup"><span data-stu-id="6cbc9-369">*Press Tab to select the highlighted snippet*</span></span>

    <span data-ttu-id="6cbc9-370">![Drücken Sie erneut die Tab und den Ausschnitt erweitert](build-restful-apis-with-aspnet-web-api/_static/image31.png "drücken Sie erneut die Tab und den Ausschnitt erweitert")</span><span class="sxs-lookup"><span data-stu-id="6cbc9-370">![Press Tab again and the snippet will expand](build-restful-apis-with-aspnet-web-api/_static/image31.png "Press Tab again and the snippet will expand")</span></span>

    <span data-ttu-id="6cbc9-371">*Drücken Sie erneut die Tab und den Ausschnitt erweitert*</span><span class="sxs-lookup"><span data-stu-id="6cbc9-371">*Press Tab again and the snippet will expand*</span></span>

<a id="CodeSnippetUsingMouse"></a>

<a id="To_add_a_code_snippet_using_the_mouse_C_Visual_Basic_and_XML"></a>
### <a name="to-add-a-code-snippet-using-the-mouse-c-visual-basic-and-xml"></a><span data-ttu-id="6cbc9-372">Hinzufügen ein Codeausschnitts, die mit der Maus (c#, Visual Basic und XML)</span><span class="sxs-lookup"><span data-stu-id="6cbc9-372">To add a code snippet using the mouse (C#, Visual Basic and XML)</span></span>

1. <span data-ttu-id="6cbc9-373">Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen möchten.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-373">Right-click where you want to insert the code snippet.</span></span>
2. <span data-ttu-id="6cbc9-374">Wählen Sie **Ausschnitt einfügen** gefolgt von **eigene Codeausschnitte**.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-374">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
3. <span data-ttu-id="6cbc9-375">Wählen Sie die relevanten Ausschnitt aus der Liste, indem Sie darauf klicken.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-375">Pick the relevant snippet from the list, by clicking on it.</span></span>

    <span data-ttu-id="6cbc9-376">![Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten](build-restful-apis-with-aspnet-web-api/_static/image32.png "mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten,")</span><span class="sxs-lookup"><span data-stu-id="6cbc9-376">![Right-click where you want to insert the code snippet and select Insert Snippet](build-restful-apis-with-aspnet-web-api/_static/image32.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

    <span data-ttu-id="6cbc9-377">*Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten*</span><span class="sxs-lookup"><span data-stu-id="6cbc9-377">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

    <span data-ttu-id="6cbc9-378">![Wählen Sie den relevanten Ausschnitt aus der Liste, indem Sie darauf klicken](build-restful-apis-with-aspnet-web-api/_static/image33.png "den relevanten Ausschnitt aus der Liste auswählen, indem Sie darauf klicken")</span><span class="sxs-lookup"><span data-stu-id="6cbc9-378">![Pick the relevant snippet from the list, by clicking on it](build-restful-apis-with-aspnet-web-api/_static/image33.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

    <span data-ttu-id="6cbc9-379">*Wählen Sie den relevanten Ausschnitt aus der Liste, indem Sie darauf klicken*</span><span class="sxs-lookup"><span data-stu-id="6cbc9-379">*Pick the relevant snippet from the list, by clicking on it*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="6cbc9-380">Anhang B: Installieren von Visual Studio Express 2012 für das Web</span><span class="sxs-lookup"><span data-stu-id="6cbc9-380">Appendix B: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="6cbc9-381">Sie installieren können **Microsoft Visual Studio Express 2012 für das Web** oder ein anderes &quot;Express&quot; Version mithilfe der **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-381">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="6cbc9-382">Die folgenden Anweisungen führen Sie durch die erforderlichen Schritte zum Installieren *Visual Studio Express 2012 für das Web* mit *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-382">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="6cbc9-383">Wechseln Sie zu [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="6cbc9-383">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="6cbc9-384">Auch wenn Sie bereits Webplattform-Installer installiert haben, können Sie öffnen es, und suchen Sie nach dem Produkt &quot; <em>Visual Studio Express 2012 für das Web mit Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-384">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="6cbc9-385">Klicken Sie auf **jetzt installieren**.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-385">Click on **Install Now**.</span></span> <span data-ttu-id="6cbc9-386">Wenn Sie keine **Webplattform-Installer** Sie Informationen zum Herunterladen und installieren Sie diese zuerst umgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-386">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="6cbc9-387">Einmal **Webplattform-Installer** geöffnet ist, klicken Sie auf **installieren** um das Setup zu starten.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-387">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="6cbc9-388">![Visual Studio Express installieren](build-restful-apis-with-aspnet-web-api/_static/image34.png "Visual Studio Express installieren")</span><span class="sxs-lookup"><span data-stu-id="6cbc9-388">![Install Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="6cbc9-389">*Installieren Sie Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="6cbc9-389">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="6cbc9-390">Lesen Sie die Produkte, Lizenzen und Begriffe, und klicken Sie auf **ich stimme** um den Vorgang fortzusetzen.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-390">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Akzeptieren der Lizenzbedingungen](build-restful-apis-with-aspnet-web-api/_static/image35.png)

    <span data-ttu-id="6cbc9-392">*Akzeptieren der Lizenzbedingungen*</span><span class="sxs-lookup"><span data-stu-id="6cbc9-392">*Accepting the license terms*</span></span>
5. <span data-ttu-id="6cbc9-393">Warten Sie, bis der Prozess herunterladen und die Installation abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-393">Wait until the downloading and installation process completes.</span></span>

    ![Installationsstatus](build-restful-apis-with-aspnet-web-api/_static/image36.png)

    <span data-ttu-id="6cbc9-395">*Installationsstatus*</span><span class="sxs-lookup"><span data-stu-id="6cbc9-395">*Installation progress*</span></span>
6. <span data-ttu-id="6cbc9-396">Nach Abschluss der Installation klicken Sie auf **Fertig stellen**.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-396">When the installation completes, click **Finish**.</span></span>

    ![Installation wurde abgeschlossen](build-restful-apis-with-aspnet-web-api/_static/image37.png)

    <span data-ttu-id="6cbc9-398">*Installation wurde abgeschlossen*</span><span class="sxs-lookup"><span data-stu-id="6cbc9-398">*Installation completed*</span></span>
7. <span data-ttu-id="6cbc9-399">Klicken Sie auf **beenden** Webplattform-Installer zu schließen.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-399">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="6cbc9-400">Um Visual Studio Express für Web zu öffnen, wechseln Sie zu der **starten** Startseite ein, und starten Sie das Schreiben von &quot; **Visual Studio Express**&quot;, klicken Sie dann auf die **Visual Studio Express für Web** Kachel.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-400">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Visual Studio Express für Web-Kachel](build-restful-apis-with-aspnet-web-api/_static/image38.png)

    <span data-ttu-id="6cbc9-402">*Visual Studio Express für Web-Kachel*</span><span class="sxs-lookup"><span data-stu-id="6cbc9-402">*VS Express for Web tile*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-c-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="6cbc9-403">Anhang C: Veröffentlichen einer ASP.NET MVC 4-Anwendung mithilfe von Web Deploy</span><span class="sxs-lookup"><span data-stu-id="6cbc9-403">Appendix C: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="6cbc9-404">In diesem Anhang wird gezeigt, wie eine neue Website aus dem Azure-Portal erstellen und Veröffentlichen der Anwendung, die Sie erhalten haben anhand der testumgebung profitieren von der Web Deploy Veröffentlichungsfunktion von Azure bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-404">This appendix will show you how to create a new web site from the Azure Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Azure.</span></span>

<a id="ApxCTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a><span data-ttu-id="6cbc9-405">Aufgabe 1: Erstellen einer neuen Website aus dem Azure-Portal</span><span class="sxs-lookup"><span data-stu-id="6cbc9-405">Task 1 - Creating a New Web Site from the Azure Portal</span></span>

1. <span data-ttu-id="6cbc9-406">Wechseln Sie zu der [Azure-Verwaltungsportal](https://manage.windowsazure.com/) und melden Sie sich mit den Microsoft-Anmeldeinformationen, die Ihrem Abonnement zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-406">Go to the [Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6cbc9-407">Sie können mit Azure hosten 10 ASP.NET-Websites kostenlos und skalieren Sie, wenn Ihr Datenverkehr zunimmt.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-407">With Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="6cbc9-408">Sie können registrieren [hier](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="6cbc9-408">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="6cbc9-409">![Melden Sie sich bei Windows Azure-Portal](build-restful-apis-with-aspnet-web-api/_static/image39.png "melden Sie sich bei Windows Azure-Portal")</span><span class="sxs-lookup"><span data-stu-id="6cbc9-409">![Log on to Windows Azure portal](build-restful-apis-with-aspnet-web-api/_static/image39.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="6cbc9-410">*Melden Sie sich beim Portal*</span><span class="sxs-lookup"><span data-stu-id="6cbc9-410">*Log on to Portal*</span></span>
2. <span data-ttu-id="6cbc9-411">Klicken Sie auf **neu** in der Befehlszeile.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-411">Click **New** on the command bar.</span></span>

    <span data-ttu-id="6cbc9-412">![Erstellen einer neuen Website](build-restful-apis-with-aspnet-web-api/_static/image40.png "Erstellen einer neuen Website")</span><span class="sxs-lookup"><span data-stu-id="6cbc9-412">![Creating a new Web Site](build-restful-apis-with-aspnet-web-api/_static/image40.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="6cbc9-413">*Erstellen einer neuen Website*</span><span class="sxs-lookup"><span data-stu-id="6cbc9-413">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="6cbc9-414">Klicken Sie auf **berechnen** | **Website**.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-414">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="6cbc9-415">Wählen Sie dann **Schnellerfassung** Option.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-415">Then select **Quick Create** option.</span></span> <span data-ttu-id="6cbc9-416">Verfügbare URL für die neue Website bereitstellen, und klicken Sie auf **Website erstellen**.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-416">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6cbc9-417">Azure ist der Host für eine Webanwendung, die ausgeführt wird, in der Cloud, die Sie steuern und verwalten können.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-417">Azure is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="6cbc9-418">Die Option Schnellerfassung können Sie eine vollständige Webanwendung in Azure von außerhalb des Portals bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-418">The Quick Create option allows you to deploy a completed web application to the Azure from outside the portal.</span></span> <span data-ttu-id="6cbc9-419">Es umfasst nicht die Schritte zum Einrichten einer Datenbank.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-419">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="6cbc9-420">![Erstellen eine neue Website, die mit der Schnellerfassung](build-restful-apis-with-aspnet-web-api/_static/image41.png "erstellen eine neue Website, die mit der Schnellerfassung")</span><span class="sxs-lookup"><span data-stu-id="6cbc9-420">![Creating a new Web Site using Quick Create](build-restful-apis-with-aspnet-web-api/_static/image41.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="6cbc9-421">*Erstellen eine neue Website, die mit der Schnellerfassung*</span><span class="sxs-lookup"><span data-stu-id="6cbc9-421">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="6cbc9-422">Warten Sie, bis die neue **Website** wird erstellt.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-422">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="6cbc9-423">Nachdem die Website erstellt wurde, klicken Sie auf den Link unter der **URL** Spalte.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-423">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="6cbc9-424">Überprüfen Sie, dass die neue Website funktioniert.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-424">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="6cbc9-425">![Um die neue Website durchsuchen](build-restful-apis-with-aspnet-web-api/_static/image42.png "durchsuchen, um die neue Website")</span><span class="sxs-lookup"><span data-stu-id="6cbc9-425">![Browsing to the new web site](build-restful-apis-with-aspnet-web-api/_static/image42.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="6cbc9-426">*Um die neue Website durchsuchen*</span><span class="sxs-lookup"><span data-stu-id="6cbc9-426">*Browsing to the new web site*</span></span>

    <span data-ttu-id="6cbc9-427">![Website](build-restful-apis-with-aspnet-web-api/_static/image43.png "Website ausgeführt wird")</span><span class="sxs-lookup"><span data-stu-id="6cbc9-427">![Web site running](build-restful-apis-with-aspnet-web-api/_static/image43.png "Web site running")</span></span>

    <span data-ttu-id="6cbc9-428">*Die Website ausgeführt wird*</span><span class="sxs-lookup"><span data-stu-id="6cbc9-428">*Web site running*</span></span>
6. <span data-ttu-id="6cbc9-429">Wechseln Sie zurück zum Portal, und klicken Sie auf den Namen der Website unter der **Namen** Spalte die Verwaltungsseiten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-429">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="6cbc9-430">![Öffnen die Website-Verwaltungsseiten](build-restful-apis-with-aspnet-web-api/_static/image44.png "öffnen die Website-Verwaltungsseiten")</span><span class="sxs-lookup"><span data-stu-id="6cbc9-430">![Opening the web site management pages](build-restful-apis-with-aspnet-web-api/_static/image44.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="6cbc9-431">*Öffnen die Website-Verwaltungsseiten*</span><span class="sxs-lookup"><span data-stu-id="6cbc9-431">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="6cbc9-432">In der **Dashboard** Seite der **kurzer Blick** auf die **Herunterladen eines Veröffentlichungsprofils** Link.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-432">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6cbc9-433">Die *Veröffentlichungsprofil* enthält alle Informationen zum Veröffentlichen einer Webanwendung in einem von Azure für die einzelnen aktivierten Veröffentlichungsmethoden erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-433">The *publish profile* contains all of the information required to publish a web application to a Azure for each enabled publication method.</span></span> <span data-ttu-id="6cbc9-434">Das Veröffentlichungsprofil enthält die URLs, Benutzeranmeldeinformationen und datenbankzeichenfolgen, die erforderlich sind, eine Verbindung herstellen und die Authentifizierung für alle Endpunkte für die eine Veröffentlichungsmethode aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-434">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="6cbc9-435">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express für Web** und **Microsoft Visual Studio 2012** Unterstützung beim Lesen von veröffentlichungsprofilen zum Automatisieren der Konfiguration dieser Programme für Veröffentlichung von Webanwendungen in Azure.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-435">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Azure.</span></span>

    <span data-ttu-id="6cbc9-436">![Herunterladen der Website-Veröffentlichungsprofil](build-restful-apis-with-aspnet-web-api/_static/image45.png "der Website herunterladen eines Veröffentlichungsprofils")</span><span class="sxs-lookup"><span data-stu-id="6cbc9-436">![Downloading the web site publish profile](build-restful-apis-with-aspnet-web-api/_static/image45.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="6cbc9-437">*Die Website herunterladen eines Veröffentlichungsprofils*</span><span class="sxs-lookup"><span data-stu-id="6cbc9-437">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="6cbc9-438">Laden Sie das Veröffentlichungsprofil an einem bekannten Speicherort ein.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-438">Download the publish profile file to a known location.</span></span> <span data-ttu-id="6cbc9-439">In dieser Übung wird weiter Gewusst wie: Verwenden Sie diese Datei zum Veröffentlichen einer Webanwendung in Azure aus Visual Studio angezeigt.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-439">Further in this exercise you will see how to use this file to publish a web application to Azure from Visual Studio.</span></span>

    <span data-ttu-id="6cbc9-440">![Speichern das Veröffentlichungsprofil](build-restful-apis-with-aspnet-web-api/_static/image46.png "speichern das Veröffentlichungsprofil")</span><span class="sxs-lookup"><span data-stu-id="6cbc9-440">![Saving the publish profile file](build-restful-apis-with-aspnet-web-api/_static/image46.png "Saving the publish profile")</span></span>

    <span data-ttu-id="6cbc9-441">*Speichern das Veröffentlichungsprofil*</span><span class="sxs-lookup"><span data-stu-id="6cbc9-441">*Saving the publish profile file*</span></span>

<a id="ApxCTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="6cbc9-442">Aufgabe 2: konfigurieren den Datenbankserver</span><span class="sxs-lookup"><span data-stu-id="6cbc9-442">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="6cbc9-443">Wenn die Anwendung durchführt, verwenden Sie SQL Server Datenbanken Sie einen SQL-Datenbankserver erstellen müssen.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-443">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="6cbc9-444">Wenn eine einfache Anwendung bereitstellen, die keine SQL Server verwendet werden sollen, können Sie diese Aufgabe überspringen.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-444">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="6cbc9-445">Sie benötigen einen SQL-Datenbankserver zum Speichern der Anwendungsdatenbank.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-445">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="6cbc9-446">Sehen Sie die SQL-Datenbankservern über Ihr Abonnement im Azure-Verwaltungsportal am **Sql-Datenbanken** | **Server** | **Dashboard des Servers**.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-446">You can view the SQL Database servers from your subscription in the Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="6cbc9-447">Wenn Sie keinen Server erstellt haben, können Sie erstellen, mit der **hinzufügen** auf der Befehlsleiste auf die Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-447">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="6cbc9-448">Notieren Sie die **Servername und -URL, Administrator-Benutzername und Kennwort**, wie Sie sie in den nächsten Aufgaben verwenden.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-448">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="6cbc9-449">Erstellen Sie die Datenbank nicht noch, wie er in einer späteren Phase erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-449">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="6cbc9-450">![SQL-Datenbank-Dashboard](build-restful-apis-with-aspnet-web-api/_static/image47.png "SQL-Datenbank-Dashboard")</span><span class="sxs-lookup"><span data-stu-id="6cbc9-450">![SQL Database Server Dashboard](build-restful-apis-with-aspnet-web-api/_static/image47.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="6cbc9-451">*SQL-Datenbank-Dashboard*</span><span class="sxs-lookup"><span data-stu-id="6cbc9-451">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="6cbc9-452">In der nächsten Aufgabe testen Sie die Verbindung mit der Datenbank, aus Visual Studio aus diesem Grund Sie die lokale IP-Adresse in der Serverliste des müssen **zulässigen IP-Adressen**.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-452">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="6cbc9-453">Klicken Sie hierzu auf **konfigurieren**, wählen Sie die IP-Adresse von **aktuelle Client-IP-Adresse** und fügen Sie ihn auf die **IP-Startadresse** und **IP-Endadresse** Textfelder, und klicken Sie auf die ![add-client-ip-address-ok-button](build-restful-apis-with-aspnet-web-api/_static/image48.png) Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-453">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](build-restful-apis-with-aspnet-web-api/_static/image48.png) button.</span></span>

    ![Client-IP-Adresse hinzufügen](build-restful-apis-with-aspnet-web-api/_static/image49.png)

    <span data-ttu-id="6cbc9-455">*Client-IP-Adresse hinzufügen*</span><span class="sxs-lookup"><span data-stu-id="6cbc9-455">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="6cbc9-456">Einmal die **IP-Clientadresse** wird zu den zulässigen IP-Adressen hinzugefügt Liste, klicken Sie auf **speichern** um die Änderungen zu bestätigen.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-456">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Bestätigen von Änderungen](build-restful-apis-with-aspnet-web-api/_static/image50.png)

    <span data-ttu-id="6cbc9-458">*Bestätigen von Änderungen*</span><span class="sxs-lookup"><span data-stu-id="6cbc9-458">*Confirm Changes*</span></span>

<a id="ApxCTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="6cbc9-459">Aufgabe 3: Veröffentlichen einer ASP.NET MVC 4-Anwendung mithilfe von Web Deploy</span><span class="sxs-lookup"><span data-stu-id="6cbc9-459">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="6cbc9-460">Wechseln Sie zurück, die ASP.NET MVC 4-Projektmappe.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-460">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="6cbc9-461">In der **Projektmappen-Explorer**mit der rechten Maustaste auf das Websiteprojekt, und wählen Sie **veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-461">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="6cbc9-462">![Veröffentlichen der Anwendung](build-restful-apis-with-aspnet-web-api/_static/image51.png "Veröffentlichen der Anwendung")</span><span class="sxs-lookup"><span data-stu-id="6cbc9-462">![Publishing the Application](build-restful-apis-with-aspnet-web-api/_static/image51.png "Publishing the Application")</span></span>

    <span data-ttu-id="6cbc9-463">*Veröffentlichen der Website*</span><span class="sxs-lookup"><span data-stu-id="6cbc9-463">*Publishing the web site*</span></span>
2. <span data-ttu-id="6cbc9-464">Importieren Sie das Veröffentlichungsprofil, das Sie in der ersten Aufgabe gespeichert.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-464">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="6cbc9-465">![Importieren des Veröffentlichungsprofils](build-restful-apis-with-aspnet-web-api/_static/image52.png "Importieren des Veröffentlichungsprofils")</span><span class="sxs-lookup"><span data-stu-id="6cbc9-465">![Importing the publish profile](build-restful-apis-with-aspnet-web-api/_static/image52.png "Importing the publish profile")</span></span>

    <span data-ttu-id="6cbc9-466">*Importieren Sie das Veröffentlichungsprofil*</span><span class="sxs-lookup"><span data-stu-id="6cbc9-466">*Importing publish profile*</span></span>
3. <span data-ttu-id="6cbc9-467">Klicken Sie auf **überprüft, ob Verbindung**.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-467">Click **Validate Connection**.</span></span> <span data-ttu-id="6cbc9-468">Klicken Sie nach Abschluss der Überprüfung auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-468">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6cbc9-469">Überprüfung ist beendet, sobald Sie sehen, dass ein grünes Häkchen neben der Schaltfläche "Validate Connection" angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-469">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="6cbc9-470">![Überprüfen der Verbindung](build-restful-apis-with-aspnet-web-api/_static/image53.png "Überprüfen der Verbindung")</span><span class="sxs-lookup"><span data-stu-id="6cbc9-470">![Validating connection](build-restful-apis-with-aspnet-web-api/_static/image53.png "Validating connection")</span></span>

    <span data-ttu-id="6cbc9-471">*Überprüfen der Verbindung*</span><span class="sxs-lookup"><span data-stu-id="6cbc9-471">*Validating connection*</span></span>
4. <span data-ttu-id="6cbc9-472">In der **Einstellungen** Seite der **Datenbanken** auf die Schaltfläche neben dem Textfeld für die datenbankverbindung (d. h. **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="6cbc9-472">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="6cbc9-473">![Web deploy-Konfiguration](build-restful-apis-with-aspnet-web-api/_static/image54.png "Web deploy-Konfiguration")</span><span class="sxs-lookup"><span data-stu-id="6cbc9-473">![Web deploy configuration](build-restful-apis-with-aspnet-web-api/_static/image54.png "Web deploy configuration")</span></span>

    <span data-ttu-id="6cbc9-474">*Web deploy-Konfiguration*</span><span class="sxs-lookup"><span data-stu-id="6cbc9-474">*Web deploy configuration*</span></span>
5. <span data-ttu-id="6cbc9-475">Konfigurieren Sie die Verbindung mit der Datenbank wie folgt:</span><span class="sxs-lookup"><span data-stu-id="6cbc9-475">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="6cbc9-476">In der **Servernamen** Geben Sie Ihre SQL-Datenbank Server-URL unter Verwendung der *Tcp:* Präfix.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-476">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="6cbc9-477">In **Benutzername** Geben Sie Ihre Administrator Serveranmeldenamen an.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-477">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="6cbc9-478">In **Kennwort** Geben Sie Ihre Server-Administratorkennwort.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-478">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="6cbc9-479">Geben Sie einen neuen Datenbanknamen ein, z. B.: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-479">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="6cbc9-480">![Konfigurieren von Zielverbindungszeichenfolge](build-restful-apis-with-aspnet-web-api/_static/image55.png "Zielverbindungszeichenfolge konfigurieren")</span><span class="sxs-lookup"><span data-stu-id="6cbc9-480">![Configuring destination connection string](build-restful-apis-with-aspnet-web-api/_static/image55.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="6cbc9-481">*Konfigurieren von Ziel-Verbindungszeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="6cbc9-481">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="6cbc9-482">Klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-482">Then click **OK**.</span></span> <span data-ttu-id="6cbc9-483">Bei der Aufforderung zum Erstellen des Datenbank auf **Ja**.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-483">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="6cbc9-484">![Erstellen der Datenbank](build-restful-apis-with-aspnet-web-api/_static/image56.png "erstellen die Datenbank-Zeichenfolge")</span><span class="sxs-lookup"><span data-stu-id="6cbc9-484">![Creating the database](build-restful-apis-with-aspnet-web-api/_static/image56.png "Creating the database string")</span></span>

    <span data-ttu-id="6cbc9-485">*Erstellen der Datenbank*</span><span class="sxs-lookup"><span data-stu-id="6cbc9-485">*Creating the database*</span></span>
7. <span data-ttu-id="6cbc9-486">Die Verbindungszeichenfolge, die Sie verwenden für die Verbindung mit SQL-Datenbank in Windows Azure ist innerhalb der Verbindung standardmäßig Textfeld angezeigt.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-486">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="6cbc9-487">Klicken Sie dann auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-487">Then click **Next**.</span></span>

    <span data-ttu-id="6cbc9-488">![Verbindungszeichenfolge zur SQL-Datenbank zeigt](build-restful-apis-with-aspnet-web-api/_static/image57.png "Verbindungszeichenfolge zur SQL-Datenbank zeigt")</span><span class="sxs-lookup"><span data-stu-id="6cbc9-488">![Connection string pointing to SQL Database](build-restful-apis-with-aspnet-web-api/_static/image57.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="6cbc9-489">*Verbindungszeichenfolge, die auf der SQL-Datenbank*</span><span class="sxs-lookup"><span data-stu-id="6cbc9-489">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="6cbc9-490">In der **Vorschau** auf **veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-490">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="6cbc9-491">![Veröffentlichen der Webanwendung](build-restful-apis-with-aspnet-web-api/_static/image58.png "Veröffentlichen der Webanwendung")</span><span class="sxs-lookup"><span data-stu-id="6cbc9-491">![Publishing the web application](build-restful-apis-with-aspnet-web-api/_static/image58.png "Publishing the web application")</span></span>

    <span data-ttu-id="6cbc9-492">*Veröffentlichen der Webanwendung*</span><span class="sxs-lookup"><span data-stu-id="6cbc9-492">*Publishing the web application*</span></span>
9. <span data-ttu-id="6cbc9-493">Sobald der Veröffentlichungsprozess abgeschlossen ist, wird der Standardbrowser veröffentlichten Website geöffnet.</span><span class="sxs-lookup"><span data-stu-id="6cbc9-493">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="6cbc9-494">![Anwendung in Windows Azure veröffentlicht](build-restful-apis-with-aspnet-web-api/_static/image59.png "Veröffentlichung der Anwendung in Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="6cbc9-494">![Application published to Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image59.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="6cbc9-495">*Anwendung in Azure veröffentlicht*</span><span class="sxs-lookup"><span data-stu-id="6cbc9-495">*Application published to Azure*</span></span>
