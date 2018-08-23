---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
title: ASP.NET MVC 4 – Grundlagen | Microsoft-Dokumentation
author: rick-anderson
description: Diese praktische Übungseinheit wird MVC (Model View Controller) Music Store, eine lernprogrammanwendung basiert auf eingeführt und erläutert Schritt für Schritt, wie ASP.NET MV verwenden...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: b7dba543-73c3-4534-a9a0-ba70fa2c6a8a
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
msc.type: authoredcontent
ms.openlocfilehash: d8e837a5d56871d271590859c2e82336111cc87a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831931"
---
# <a name="aspnet-mvc-4-fundamentals"></a><span data-ttu-id="7bd6e-103">ASP.NET MVC 4 – Grundlagen</span><span class="sxs-lookup"><span data-stu-id="7bd6e-103">ASP.NET MVC 4 Fundamentals</span></span>

<span data-ttu-id="7bd6e-104">durch [Web Camps Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="7bd6e-104">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="7bd6e-105">Herunterladen Sie Web Camps Training Kit</span><span class="sxs-lookup"><span data-stu-id="7bd6e-105">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="7bd6e-106">Diese praktische Übungseinheit wird MVC (Model View Controller) Music Store, eine lernprogrammanwendung basiert auf eingeführt und erläutert Schritt für Schritt, wie ASP.NET MVC und Visual Studio verwenden.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-106">This Hands-On Lab is based on MVC (Model View Controller) Music Store, a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio.</span></span> <span data-ttu-id="7bd6e-107">Erfahren Sie in den Laboren die Einfachheit halber noch Power diese Technologien zusammen verwendet.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-107">Throughout the lab you will learn the simplicity, yet power of using these technologies together.</span></span> <span data-ttu-id="7bd6e-108">Sie werden mit einer einfachen Anwendung gestartet und werden es erstellt, bis Sie über eine voll funktionsfähige ASP.NET MVC 4-Webanwendung verfügen.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-108">You will start with a simple application and will build it until you have a fully functional ASP.NET MVC 4 Web Application.</span></span>

<span data-ttu-id="7bd6e-109">Dieses Lab funktioniert mit ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-109">This Lab works with ASP.NET MVC 4.</span></span>

<span data-ttu-id="7bd6e-110">Wenn Sie die ASP.NET MVC 3-Version der Anwendung Tutorial erkunden möchten, finden Sie in [MVC-Musik-Store](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="7bd6e-110">If you wish to explore the ASP.NET MVC 3 version of the tutorial application, you can find it in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>

<span data-ttu-id="7bd6e-111">Diese praktische Übungseinheit wird davon ausgegangen, dass der Entwickler Erfahrungen in der Entwicklung von webtechnologien wie HTML und JavaScript.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-111">This Hands-On Lab assumes that the developer has experience in Web development technologies, such as HTML and JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="7bd6e-112">Alle Beispielcode und Ausschnitte sind im Web Camps Training Kit unter enthalten [Versionen der Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="7bd6e-112">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="7bd6e-113">Das Projekt, das speziell für dieses Lab finden Sie unter [Grundlagen von ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals).</span><span class="sxs-lookup"><span data-stu-id="7bd6e-113">The project specific to this lab is available at [ASP.NET MVC 4 Fundamentals](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals).</span></span>

<a id="The_Music_Store_application"></a>
### <a name="the-music-store-application"></a><span data-ttu-id="7bd6e-114">Die Music Store-Anwendung</span><span class="sxs-lookup"><span data-stu-id="7bd6e-114">The Music Store application</span></span>

<span data-ttu-id="7bd6e-115">Die Music Store-Anwendung, die in dieser Übung erstellt wird, umfasst drei Hauptkomponenten: Warenkorb, Auschecken und Verwaltung.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-115">The Music Store web application that will be built throughout this Lab comprises three main parts: shopping, checkout, and administration.</span></span> <span data-ttu-id="7bd6e-116">Besucher werden Alben nach Genre durchsuchen, Alben zu Einkaufswagen hinzufügen, überprüfen ihre Auswahl und schließlich fortfahren, lesen Sie die Anmeldung und vervollständigen Sie die Bestellung.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-116">Visitors will be able to browse albums by genre, add albums to their cart, review their selection and finally proceed to checkout to login and complete the order.</span></span> <span data-ttu-id="7bd6e-117">Darüber hinaus werden Store-Administratoren verfügbaren Alben sowie ihre Haupteigenschaften verwalten können.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-117">Additionally, store administrators will be able to manage the available albums as well as their main properties.</span></span>

<span data-ttu-id="7bd6e-118">![Music Store-Bildschirme](aspnet-mvc-4-fundamentals/_static/image1.png "Music Store Bildschirme")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-118">![Music Store screens](aspnet-mvc-4-fundamentals/_static/image1.png "Music Store screens")</span></span>

<span data-ttu-id="7bd6e-119">*Music Store-Bildschirme*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-119">*Music Store screens*</span></span>

<a id="ASPNET_MVC_4_Essentials"></a>
### <a name="aspnet-mvc-4-essentials"></a><span data-ttu-id="7bd6e-120">ASP.NET MVC 4 Essentials</span><span class="sxs-lookup"><span data-stu-id="7bd6e-120">ASP.NET MVC 4 Essentials</span></span>

<span data-ttu-id="7bd6e-121">Music Store-Anwendung erstellt werden mit **Model View Controller (MVC)**, ein Architekturmuster, das trennt eine Anwendung in drei Hauptkomponenten:</span><span class="sxs-lookup"><span data-stu-id="7bd6e-121">Music Store application will be built using **Model View Controller (MVC)**, an architectural pattern that separates an application into three main components:</span></span>

- <span data-ttu-id="7bd6e-122">**Modelle**: Modellobjekte sind die Teile der Anwendung, die die Domänenlogik implementieren.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-122">**Models**: Model objects are the parts of the application that implement the domain logic.</span></span> <span data-ttu-id="7bd6e-123">Model-Objekte wird häufig auch abrufen, und der Modellzustand in einer Datenbank speichern.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-123">Often, model objects also retrieve and store model state in a database.</span></span>
- <span data-ttu-id="7bd6e-124">**Ansichten:** Ansichten sind die Komponenten, die der Anwendung die Benutzeroberfläche (UI) an.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-124">**Views:** Views are the components that display the application's user interface (UI).</span></span> <span data-ttu-id="7bd6e-125">In der Regel wird diese Benutzeroberfläche aus den Modelldaten erstellt.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-125">Typically, this UI is created from the model data.</span></span> <span data-ttu-id="7bd6e-126">Ein Beispiel wäre der Bearbeitungsansicht von Alben, die Textfelder und eine Dropdown-Liste basierend auf den aktuellen Status eines Objekts Album anzeigt.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-126">An example would be the edit view of Albums that displays text boxes and a drop-down list based on the current state of an Album object.</span></span>
- <span data-ttu-id="7bd6e-127">**Domänencontroller:** Controller sind Komponenten, die Benutzerinteraktionen verarbeiten, bearbeiten das Modell und letztlich eine Ansicht zum Rendern der Benutzeroberflächenautomatisierungs auswählen.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-127">**Controllers:** Controllers are the components that handle user interaction, manipulate the model, and ultimately select a view to render the UI.</span></span> <span data-ttu-id="7bd6e-128">In einer MVC-Anwendung zeigt die Ansicht nur Informationen an. Benutzereingaben und -interaktionen werden vom Controller verarbeitet und beantwortet.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-128">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span>

<span data-ttu-id="7bd6e-129">Das MVC-Muster unterstützt Sie beim Erstellen von Anwendungen, die die verschiedenen Aspekte der Anwendung (Eingabelogik, Geschäftslogik und UI-Logik), und gleichzeitig eine lose Kopplung zwischen diesen Elementen zu trennen.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-129">The MVC pattern helps you to create applications that separate the different aspects of the application (input logic, business logic, and UI logic), while providing a loose coupling between these elements.</span></span> <span data-ttu-id="7bd6e-130">Diese Trennung ermöglicht Ihnen das bewältigen von Komplexität beim Erstellen einer Anwendung, da Sie sich auf einen Aspekt der Implementierung zu einem Zeitpunkt konzentrieren können.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-130">This separation helps you manage complexity when you build an application, as it allows you to focus on one aspect of the implementation at a time.</span></span> <span data-ttu-id="7bd6e-131">Darüber hinaus erleichtert das MVC-Muster zum Testen von Anwendungen, da Sie auch die Verwendung der testgesteuerten Entwicklung (TDD) zum Erstellen von Anwendungen.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-131">In addition, the MVC pattern makes it easy to test applications, also encouraging the use of test-driven development (TDD) for creating applications.</span></span>

<span data-ttu-id="7bd6e-132">Die **ASP.NET MVC** Framework stellt eine Alternative für das ASP.NET Web Forms-Muster zum Erstellen von Webanwendungen mit ASP.NET MVC-basierten.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-132">The **ASP.NET MVC** framework provides an alternative to the ASP.NET Web Forms pattern for creating ASP.NET MVC-based Web applications.</span></span> <span data-ttu-id="7bd6e-133">Die **ASP.NET MVC** Framework ist eine einfache und leicht zu testendes Präsentationsframework, (wie bei Web Forms-basierte Anwendungen) ist in vorhandene ASP.NET-Funktionen, z. B. Gestaltungsvorlagen und mitgliedschaftsbasierte integriert Authentifizierung, um die Leistungsfähigkeit von .NET Core Framework zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-133">The **ASP.NET MVC** framework is a lightweight, highly testable presentation framework that (as with Web-forms-based applications) is integrated with existing ASP.NET features, such as master pages and membership-based authentication so you get all the power of the core .NET framework.</span></span> <span data-ttu-id="7bd6e-134">Dies ist nützlich, wenn Sie bereits mit ASP.NET Web Forms vertraut sind, da alle Bibliotheken, mit denen Sie auch in ASP.NET MVC 4 verfügbar sind.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-134">This is useful if you are already familiar with ASP.NET Web Forms because all the libraries that you already use are available in ASP.NET MVC 4 as well.</span></span>

<span data-ttu-id="7bd6e-135">Darüber hinaus wird die lose Kopplung zwischen den drei Hauptkomponenten einer MVC-Anwendung auch parallele Entwicklung.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-135">In addition, the loose coupling between the three main components of an MVC application also promotes parallel development.</span></span> <span data-ttu-id="7bd6e-136">Z. B. ein Entwickler kann die auf die Ansicht, ein zweiter Entwickler an der Controllerlogik arbeiten kann und ein dritter Entwickler kann sich auf die Geschäftslogik im Modell konzentrieren.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-136">For instance, one developer can work on the view, a second developer can work on the controller logic, and a third developer can focus on the business logic in the model.</span></span>

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="7bd6e-137">Ziele</span><span class="sxs-lookup"><span data-stu-id="7bd6e-137">Objectives</span></span>

<span data-ttu-id="7bd6e-138">In dieser praktischen Übungseinheit erfahren Sie, wie Sie:</span><span class="sxs-lookup"><span data-stu-id="7bd6e-138">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="7bd6e-139">Erstellen Sie eine ASP.NET MVC-Anwendung von Grund auf neu, die basierend auf dem Lernprogramm für die Music Store-Anwendung</span><span class="sxs-lookup"><span data-stu-id="7bd6e-139">Create an ASP.NET MVC application from scratch based on the Music Store Application tutorial</span></span>
- <span data-ttu-id="7bd6e-140">Hinzufügen von Controllern zum Behandeln von URLs zur Homepage der Website und seine wichtigsten Funktionen durchsuchen</span><span class="sxs-lookup"><span data-stu-id="7bd6e-140">Add Controllers to handle URLs to the Home page of the site and for browsing its main functionality</span></span>
- <span data-ttu-id="7bd6e-141">Hinzufügen einer Ansicht, um den Inhalt, der angezeigt wird, zusammen mit den Stil anzupassen</span><span class="sxs-lookup"><span data-stu-id="7bd6e-141">Add a View to customize the content displayed along with its style</span></span>
- <span data-ttu-id="7bd6e-142">Hinzufügen von Modellklassen um enthalten und Verwalten von Daten und Domänenlogik</span><span class="sxs-lookup"><span data-stu-id="7bd6e-142">Add Model classes to contain and manage data and domain logic</span></span>
- <span data-ttu-id="7bd6e-143">Verwenden Sie View Model-Muster, um Informationen aus Controlleraktionen an die Ansichtsvorlagen zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-143">Use View Model pattern to pass information from controller actions to the view templates</span></span>
- <span data-ttu-id="7bd6e-144">Erkunden der neuen ASP.NET MVC 4-Vorlage von Internetanwendungen</span><span class="sxs-lookup"><span data-stu-id="7bd6e-144">Explore the ASP.NET MVC 4 new template for internet applications</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="7bd6e-145">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="7bd6e-145">Prerequisites</span></span>

<span data-ttu-id="7bd6e-146">Sie benötigen Folgendes, um diese testumgebung abzuschließen:</span><span class="sxs-lookup"><span data-stu-id="7bd6e-146">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="7bd6e-147">[Visual Studio 2012 Express für Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (Lesen [Anhang A](#AppendixA) Anleitungen zur Installation)</span><span class="sxs-lookup"><span data-stu-id="7bd6e-147">[Visual Studio 2012 Express for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (read [Appendix A](#AppendixA) for instructions on how to install it)</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="7bd6e-148">Setup</span><span class="sxs-lookup"><span data-stu-id="7bd6e-148">Setup</span></span>

<span data-ttu-id="7bd6e-149">**Installieren von Codeausschnitten**</span><span class="sxs-lookup"><span data-stu-id="7bd6e-149">**Installing Code Snippets**</span></span>

<span data-ttu-id="7bd6e-150">Der Einfachheit halber ist Großteil des Codes, die entlang dieser Übungseinheit verwaltet werden soll als Codeausschnitte für Visual Studio verfügbar.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-150">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="7bd6e-151">So installieren Sie die Codeausschnitte ausführen **.\Source\Setup\CodeSnippets.vsi** Datei.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-151">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="7bd6e-152">Wenn Sie nicht mit dem Visual Studio Code Snippets und zu erfahren, wie Sie deren Verwendung vertraut sind, sehen Sie sich im Anhang in diesem Dokument &quot; [Anhang C: Verwenden von Code Snippets](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-152">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="7bd6e-153">Übungen</span><span class="sxs-lookup"><span data-stu-id="7bd6e-153">Exercises</span></span>

<span data-ttu-id="7bd6e-154">Diese praktische Übungseinheit besteht aus durch die folgenden Übungen:</span><span class="sxs-lookup"><span data-stu-id="7bd6e-154">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="7bd6e-155">Übung 1: Erstellen von ASP.NET MVC-Webanwendungsprojekt Music Store</span><span class="sxs-lookup"><span data-stu-id="7bd6e-155">Exercise 1: Creating MusicStore ASP.NET MVC Web Application Project</span></span>](#Exercise1)
2. [<span data-ttu-id="7bd6e-156">Übung 2: Erstellen eines Controllers</span><span class="sxs-lookup"><span data-stu-id="7bd6e-156">Exercise 2: Creating a Controller</span></span>](#Exercise2)
3. [<span data-ttu-id="7bd6e-157">Übung 3: Übergeben von Parametern an einen Controller</span><span class="sxs-lookup"><span data-stu-id="7bd6e-157">Exercise 3: Passing parameters to a Controller</span></span>](#Exercise3)
4. [<span data-ttu-id="7bd6e-158">Übung 4: Erstellen einer Ansicht</span><span class="sxs-lookup"><span data-stu-id="7bd6e-158">Exercise 4: Creating a View</span></span>](#Exercise4)
5. [<span data-ttu-id="7bd6e-159">Schritt 5: Erstellen von Anzeigemodellen</span><span class="sxs-lookup"><span data-stu-id="7bd6e-159">Exercise 5: Creating a View Model</span></span>](#Exercise5)
6. [<span data-ttu-id="7bd6e-160">Übung 6: Verwenden von Parametern in der Ansicht</span><span class="sxs-lookup"><span data-stu-id="7bd6e-160">Exercise 6: Using parameters in View</span></span>](#Exercise6)
7. [<span data-ttu-id="7bd6e-161">Übung 7: Eine Tour durch neue ASP.NET MVC 4-Vorlage</span><span class="sxs-lookup"><span data-stu-id="7bd6e-161">Exercise 7: A lap around ASP.NET MVC 4 New Template</span></span>](#Exercise7)

> [!NOTE]
> <span data-ttu-id="7bd6e-162">Jede Übung umfasst eine **End** Ordner mit der resultierenden Lösung, die Sie nach Abschluss der Übungen abrufen soll.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-162">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="7bd6e-163">Sie können diese Lösung als Leitfaden verwenden, bei Bedarf zusätzliche Hilfe bei der die Übungen durcharbeiten.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-163">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="7bd6e-164">Geschätzte Zeit für diese testumgebung abzuschließen: **60 Minuten**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-164">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_MusicStore_ASPNET_MVC_Web_Application_Project"></a>
### <a name="exercise-1-creating-musicstore-aspnet-mvc-web-application-project"></a><span data-ttu-id="7bd6e-165">Übung 1: Erstellen von ASP.NET MVC-Webanwendungsprojekt Music Store</span><span class="sxs-lookup"><span data-stu-id="7bd6e-165">Exercise 1: Creating MusicStore ASP.NET MVC Web Application Project</span></span>

<span data-ttu-id="7bd6e-166">In dieser Übung lernen Sie, wie Sie eine ASP.NET MVC-Anwendung in Visual Studio 2012 Express für Web als auch der Ordner "main"-Organisation zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-166">In this exercise, you will learn how to create an ASP.NET MVC application in Visual Studio 2012 Express for Web as well as its main folder organization.</span></span> <span data-ttu-id="7bd6e-167">Darüber hinaus erfahren Sie, wie zum Hinzufügen eines neuen Controllers, und stellen sie eine einfache Zeichenfolge auf der Startseite der Anwendung angezeigt.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-167">Additionally, you will learn how to add a new Controller and make it display a simple string in the application's home page.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_ASPNET_MVC_Web_Application_Project"></a>
#### <a name="task-1---creating-the-aspnet-mvc-web-application-project"></a><span data-ttu-id="7bd6e-168">Aufgabe 1: Erstellen das ASP.NET MVC-Webanwendungsprojekt</span><span class="sxs-lookup"><span data-stu-id="7bd6e-168">Task 1 - Creating the ASP.NET MVC Web Application Project</span></span>

1. <span data-ttu-id="7bd6e-169">In dieser Aufgabe erstellen Sie eine leere ASP.NET MVC-Anwendungsprojekt mit der Visual Studio für MVC-Vorlage.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-169">In this task, you will create an empty ASP.NET MVC application project using the MVC Visual Studio template.</span></span> <span data-ttu-id="7bd6e-170">Starten Sie **Visual Studio Express für Web**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-170">Start **VS Express for Web**.</span></span>
2. <span data-ttu-id="7bd6e-171">Klicken Sie im Menü **Datei** auf **Neues Projekt**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-171">On the **File** menu, click **New Project**.</span></span>
3. <span data-ttu-id="7bd6e-172">In der **neues Projekt** aktivieren Sie im Dialogfeld die **ASP.NET MVC 4-Webanwendung** Projekttyp, befindet sich im **Visual c#** **Web** Vorlage Liste.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-172">In the **New Project** dialog box select the **ASP.NET MVC 4 Web Application** project type, located under **Visual C#,** **Web** template list.</span></span>
4. <span data-ttu-id="7bd6e-173">Ändern der **Namen** zu *MvcMusicStore*.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-173">Change the **Name** to *MvcMusicStore*.</span></span>
5. <span data-ttu-id="7bd6e-174">Legen Sie den Speicherort der Lösung in einem neuen **beginnen** im Ordner "in dieser Übung die Quelle", z. B. **[YOUR-HOL-PATH] \Source\Ex01-CreatingMusicStoreProject\Begin**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-174">Set the location of the solution inside a new **Begin** folder in this Exercise's Source folder, for example **[YOUR-HOL-PATH]\Source\Ex01-CreatingMusicStoreProject\Begin**.</span></span> <span data-ttu-id="7bd6e-175">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-175">Click **OK**.</span></span>

    <span data-ttu-id="7bd6e-176">![Dialogfeld Neues Projekt erstellen](aspnet-mvc-4-fundamentals/_static/image2.png "Dialogfeld Neues Projekt erstellen")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-176">![Create New Project Dialog Box](aspnet-mvc-4-fundamentals/_static/image2.png "Create New Project Dialog Box")</span></span>

    <span data-ttu-id="7bd6e-177">*Dialogfeld Neues Projekt erstellen*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-177">*Create New Project Dialog Box*</span></span>
6. <span data-ttu-id="7bd6e-178">In der **neues ASP.NET MVC 4-Projekt** aktivieren Sie im Dialogfeld die **grundlegende** Vorlage und stellen Sie sicher, dass die **Ansichts-Engine** ausgewählt **Razor**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-178">In the **New ASP.NET MVC 4 Project** dialog box select the **Basic** template and make sure that the **View engine** selected is **Razor**.</span></span> <span data-ttu-id="7bd6e-179">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-179">Click **OK**.</span></span>

    <span data-ttu-id="7bd6e-180">![Dialogfeld Neues ASP.NET MVC 4-Projekt](aspnet-mvc-4-fundamentals/_static/image3.png "neue ASP.NET MVC 4-Projekt (Dialogfeld)")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-180">![New ASP.NET MVC 4 Project Dialog Box](aspnet-mvc-4-fundamentals/_static/image3.png "New ASP.NET MVC 4 Project Dialog Box")</span></span>

    <span data-ttu-id="7bd6e-181">*Neues ASP.NET MVC 4-Projekt (Dialogfeld)*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-181">*New ASP.NET MVC 4 Project Dialog Box*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Exploring_the_Solution_Structure"></a>
#### <a name="task-2---exploring-the-solution-structure"></a><span data-ttu-id="7bd6e-182">Aufgabe 2: untersuchen die Lösungsstruktur</span><span class="sxs-lookup"><span data-stu-id="7bd6e-182">Task 2 - Exploring the Solution Structure</span></span>

<span data-ttu-id="7bd6e-183">ASP.NET MVC-Framework enthält eine Visual Studio-Projektvorlage, die Ihnen das Erstellen von Webanwendungen unterstützen das MVC-Muster hilft.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-183">The ASP.NET MVC framework includes a Visual Studio project template that helps you create Web applications supporting the MVC pattern.</span></span> <span data-ttu-id="7bd6e-184">Diese Vorlage erstellt eine neue ASP.NET MVC-Webanwendung, mit der erforderlichen Ordnern, Elementvorlagen und Konfigurationsdatei Einträge.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-184">This template creates a new ASP.NET MVC Web application with the required folders, item templates, and configuration-file entries.</span></span>

<span data-ttu-id="7bd6e-185">In dieser Aufgabe untersuchen Sie die Projektmappenstruktur, die Elemente zu verstehen, die beteiligt sind und deren Beziehungen.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-185">In this task, you will examine the solution structure to understand the elements that are involved and their relationships.</span></span> <span data-ttu-id="7bd6e-186">Die folgenden Ordner sind in der ASP.NET MVC-Anwendung enthalten, da das ASP.NET MVC-Framework standardmäßig verwendet eine &quot;Konvention geht vor Konfiguration&quot; Ansatz und legt einige Annahmen basiert, für die Benennung von Ordner Konventionen.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-186">The following folders are included in all the ASP.NET MVC application because the ASP.NET MVC framework by default uses a &quot;convention over configuration&quot; approach, and makes some default assumptions based on folder naming conventions.</span></span>

1. <span data-ttu-id="7bd6e-187">Nachdem das Projekt erstellt wurde, überprüfen Sie die Ordnerstruktur, die im Projektmappen-Explorer auf der rechten Seite erstellt wurde:</span><span class="sxs-lookup"><span data-stu-id="7bd6e-187">Once the project is created, review the folder structure that has been created in the Solution Explorer on the right side:</span></span>

    <span data-ttu-id="7bd6e-188">![ASP.NET MVC-Ordnerstruktur in Projektmappen-Explorer](aspnet-mvc-4-fundamentals/_static/image4.png "ASP.NET MVC-Ordnerstruktur in Projektmappen-Explorer")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-188">![ASP.NET MVC Folder structure in Solution Explorer](aspnet-mvc-4-fundamentals/_static/image4.png "ASP.NET MVC Folder structure in Solution Explorer")</span></span>

    <span data-ttu-id="7bd6e-189">*ASP.NET MVC-Ordnerstruktur in Projektmappen-Explorer*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-189">*ASP.NET MVC Folder structure in Solution Explorer*</span></span>

   1. <span data-ttu-id="7bd6e-190">**Controller**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-190">**Controllers**.</span></span> <span data-ttu-id="7bd6e-191">Dieser Ordner enthält die Controllerklassen.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-191">This folder will contain the controller classes.</span></span> <span data-ttu-id="7bd6e-192">In einer MVC-basierten Anwendung dienen Controller Verarbeiten von End-Benutzerinteraktionen, das Modell bearbeiten und auswählen letztlich eine Ansicht für die Benutzeroberfläche zu rendern.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-192">In an MVC based application, controllers are responsible for handling end user interaction, manipulating the model, and ultimately choosing a view to render the UI.</span></span>

       > [!NOTE]
       > <span data-ttu-id="7bd6e-193">Das MVC-Framework erfordert die Namen aller Controller endet nicht mit &quot;Controller&quot;– z. B. HomeController, LoginController oder ProductController.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-193">The MVC framework requires the names of all controllers to end with &quot;Controller&quot;-for example, HomeController, LoginController, or ProductController.</span></span>
   2. <span data-ttu-id="7bd6e-194">**Modelle**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-194">**Models**.</span></span> <span data-ttu-id="7bd6e-195">Dieser Ordner wird für Klassen bereitgestellt, die das Anwendungsmodell für die MVC-Webanwendung darstellen.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-195">This folder is provided for classes that represent the application model for the MVC Web application.</span></span> <span data-ttu-id="7bd6e-196">Dies schließt in der Regel Code, der Objekte und die Logik für die Interaktion mit dem Datenspeicher definiert.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-196">This usually includes code that defines objects and the logic for interacting with the data store.</span></span> <span data-ttu-id="7bd6e-197">In der Regel werden die eigentlichen Modellobjekte in separaten Klassenbibliotheken.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-197">Typically, the actual model objects will be in separate class libraries.</span></span> <span data-ttu-id="7bd6e-198">Wenn Sie eine neue Anwendung erstellen, können Sie jedoch umfassen Klassen und Sie dann im Entwicklungszyklus in separate Klassenbibliotheken zu einem späteren Zeitpunkt verschieben.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-198">However, when you create a new application, you might include classes and then move them into separate class libraries at a later point in the development cycle.</span></span>
   3. <span data-ttu-id="7bd6e-199">**Ansichten**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-199">**Views**.</span></span> <span data-ttu-id="7bd6e-200">Dieser Ordner ist der empfohlene Speicherort für Ansichten, die Komponenten, die für die Anzeige der Benutzeroberfläche der Anwendung verantwortlich.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-200">This folder is the recommended location for views, the components responsible for displaying the application's user interface.</span></span> <span data-ttu-id="7bd6e-201">Ansichten verwenden aspx, ASCX, .cshtml und Master-Dateien zusätzlich zu anderen Dateien, die zum Rendern von Ansichten verknüpft werden.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-201">Views use .aspx, .ascx, .cshtml and .master files, in addition to any other files that are related to rendering views.</span></span> <span data-ttu-id="7bd6e-202">Ordner "Views" enthält einen Ordner für jeden Controller. der Ordner den Namen mit dem Controller-Namenspräfix.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-202">Views folder contains a folder for each controller; the folder is named with the controller-name prefix.</span></span> <span data-ttu-id="7bd6e-203">Wenn Sie einen Controller mit dem Namen haben z. B. **HomeController**, den Ordner "Views" enthält einen Ordner mit der Bezeichnung Home.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-203">For example, if you have a controller named **HomeController**, the Views folder will contain a folder named Home.</span></span> <span data-ttu-id="7bd6e-204">In der Standardeinstellung beim Laden von ASP.NET MVC-Framework einer Ansicht, gesucht, der eine ASPX-Datei mit dem angeforderten Namen im Ordner "Views\controllerName" (**Ansichten [Namedescontrollers] [Action] aspx**) oder (**Ansichten [Namedescontrollers] [Aktion] .cshtml**) für Razor-Ansichten.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-204">By default, when the ASP.NET MVC framework loads a view, it looks for an .aspx file with the requested view name in the Views\controllerName folder (**Views[ControllerName][Action].aspx**) or (**Views[ControllerName][Action].cshtml**) for Razor Views.</span></span>

      > [!NOTE]
      > <span data-ttu-id="7bd6e-205">Zusätzlich zu den zuvor aufgeführten Ordnern verwendet eine MVC-Web-Anwendung die **"Global.asax"** standardmäßig hinzu, um globale URL-routing einzurichten, und es verwendet die **"Web.config"** Datei zum Konfigurieren der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-205">In addition to the folders listed previously, an MVC Web application uses the **Global.asax** file to set global URL routing defaults, and it uses the **Web.config** file to configure the application.</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Adding_a_HomeController"></a>
#### <a name="task-3---adding-a-homecontroller"></a><span data-ttu-id="7bd6e-206">Aufgabe 3: hinzufügen einen HomeController</span><span class="sxs-lookup"><span data-stu-id="7bd6e-206">Task 3 - Adding a HomeController</span></span>

<span data-ttu-id="7bd6e-207">In ASP.NET-Anwendungen, die das MVC-Framework nicht verwenden, wird in eine Benutzerinteraktion nach Seiten und Auslösen und Behandeln von Ereignissen aus diesen Seiten angeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-207">In ASP.NET applications that do not use the MVC framework, user interaction is organized around pages, and around raising and handling events from those pages.</span></span> <span data-ttu-id="7bd6e-208">Im Gegensatz dazu ist Benutzerinteraktion in ASP.NET-MVC-Anwendungen in Controllern und ihre Aktionsmethoden organisiert.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-208">In contrast, user interaction with ASP.NET MVC applications is organized around controllers and their action methods.</span></span>

<span data-ttu-id="7bd6e-209">ASP.NET MVC-Framework ordnet URLs auf der anderen Seite zu Klassen, die als Domänencontroller bezeichnet werden.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-209">On the other hand, ASP.NET MVC framework maps URLs to classes that are referred to as controllers.</span></span> <span data-ttu-id="7bd6e-210">Controller verarbeiten eingehende Anforderungen, behandeln Benutzereingaben und Interaktionen, führen Sie die zugehörige Anwendungslogik und die Antwort zurück an den Client senden zu bestimmen (HTML-Seite anzeigen, eine Datei herunterzuladen, Umleitung an eine andere URL usw.).</span><span class="sxs-lookup"><span data-stu-id="7bd6e-210">Controllers process incoming requests, handle user input and interactions, execute appropriate application logic and determine the response to send back to the client (display HTML, download a file, redirect to a different URL, etc.).</span></span> <span data-ttu-id="7bd6e-211">Im Fall von HTML anzeigen, ruft eine Controller-Klasse in der Regel eine separate Ansichtskomponente, um das HTML-Markup für die Anforderung zu generieren.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-211">In the case of displaying HTML, a controller class typically calls a separate view component to generate the HTML markup for the request.</span></span> <span data-ttu-id="7bd6e-212">In einer MVC-Anwendung zeigt die Ansicht nur Informationen an. Benutzereingaben und -interaktionen werden vom Controller verarbeitet und beantwortet.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-212">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span>

<span data-ttu-id="7bd6e-213">In dieser Aufgabe fügen Sie eine Controller-Klasse, die URLs, die auf der Startseite der Music Store-Website behandelt.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-213">In this task, you will add a Controller class that will handle URLs to the Home page of the Music Store site.</span></span>

1. <span data-ttu-id="7bd6e-214">Mit der rechten Maustaste **Controller** Ordner im Projektmappen-Explorer, wählen Sie **hinzufügen** und dann **Controller** Befehl:</span><span class="sxs-lookup"><span data-stu-id="7bd6e-214">Right-click **Controllers** folder within the Solution Explorer, select **Add** and then **Controller** command:</span></span>

    <span data-ttu-id="7bd6e-215">![Fügen Sie einen Controllerbefehl](aspnet-mvc-4-fundamentals/_static/image5.png "fügen einen Controllerbefehl hinzu")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-215">![Add a Controller Command](aspnet-mvc-4-fundamentals/_static/image5.png "Add a Controller Command")</span></span>

    <span data-ttu-id="7bd6e-216">*Controllerbefehl "hinzufügen"*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-216">*Add Controller Command*</span></span>
2. <span data-ttu-id="7bd6e-217">Die **Controller hinzufügen** Dialogfeld wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-217">The **Add Controller** dialog appears.</span></span> <span data-ttu-id="7bd6e-218">Nennen Sie den Controller *HomeController* , und drücken Sie **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-218">Name the controller *HomeController* and press **Add**.</span></span>

    <span data-ttu-id="7bd6e-219">![Controller-Dialogfeld "hinzufügen"](aspnet-mvc-4-fundamentals/_static/image6.png "Controller-Dialogfeld \"hinzufügen\"")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-219">![Add Controller Dialog](aspnet-mvc-4-fundamentals/_static/image6.png "Add Controller Dialog")</span></span>

    <span data-ttu-id="7bd6e-220">*Controller-Dialogfeld "hinzufügen"*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-220">*Add Controller Dialog*</span></span>
3. <span data-ttu-id="7bd6e-221">Die Datei **"HomeController.cs"** wird erstellt, der **Controller** Ordner.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-221">The file **HomeController.cs** is created in the **Controllers** folder.</span></span> <span data-ttu-id="7bd6e-222">Damit die **HomeController** geben eine Zeichenfolge zurück, auf die Index-Aktion, ersetzen Sie die **Index** Methode durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="7bd6e-222">In order to have the **HomeController** return a string on its Index action, replace the **Index** method with the following code:</span></span>

    <span data-ttu-id="7bd6e-223">(Codeausschnitt - *Grundlagen von ASP.NET MVC 4 - Ex1 HomeController Index*)</span><span class="sxs-lookup"><span data-stu-id="7bd6e-223">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex1 HomeController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample1.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="7bd6e-224">Aufgabe 4: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="7bd6e-224">Task 4 - Running the Application</span></span>

<span data-ttu-id="7bd6e-225">In dieser Aufgabe werden Sie die Anwendung in einem Webbrowser testen.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-225">In this task, you will try out the Application in a web browser.</span></span>

1. <span data-ttu-id="7bd6e-226">Drücken Sie **F5** zum Ausführen der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-226">Press **F5** to run the Application.</span></span> <span data-ttu-id="7bd6e-227">Das Projekt kompiliert wird, und die lokale IIS-Webserver gestartet wird.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-227">The project is compiled and the local IIS Web Server starts.</span></span> <span data-ttu-id="7bd6e-228">Die lokale IIS-Webserver wird einen Webbrowser zur URL des Webservers automatisch geöffnet.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-228">The local IIS Web Server will automatically open a web browser pointing to the URL of the Web server.</span></span>

    <span data-ttu-id="7bd6e-229">![In einem Webbrowser ausgeführte Anwendung](aspnet-mvc-4-fundamentals/_static/image7.png "Anwendung, die in einem Webbrowser ausgeführt wird")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-229">![Application running in a web browser](aspnet-mvc-4-fundamentals/_static/image7.png "Application running in a web browser")</span></span>

    <span data-ttu-id="7bd6e-230">*Anwendung, die in einem Webbrowser ausgeführt wird*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-230">*Application running in a web browser*</span></span>

    > [!NOTE]
    > <span data-ttu-id="7bd6e-231">Lokale IIS-Webservers wird die Website auf eine zufällige freie Portnummer ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-231">The local IIS Web Server will run the website on a random free port number.</span></span> <span data-ttu-id="7bd6e-232">In der obigen Abbildung die Website ausgeführt wird, am `http://localhost:50103/`, sodass sie Port 50103 verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-232">In the figure above, the site is running at `http://localhost:50103/`, so it's using port 50103.</span></span> <span data-ttu-id="7bd6e-233">Ihre Portnummer kann variieren.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-233">Your port number may vary.</span></span>
2. <span data-ttu-id="7bd6e-234">Schließen Sie den Browser.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-234">Close the browser.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Controller"></a>
### <a name="exercise-2-creating-a-controller"></a><span data-ttu-id="7bd6e-235">Übung 2: Erstellen eines Controllers</span><span class="sxs-lookup"><span data-stu-id="7bd6e-235">Exercise 2: Creating a Controller</span></span>

<span data-ttu-id="7bd6e-236">In dieser Übung lernen Sie, wie beim Aktualisieren des Controllers zur einfachen Funktionalität die Music Store-Anwendung zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-236">In this exercise, you will learn how to update the controller to implement simple functionality of the Music Store application.</span></span> <span data-ttu-id="7bd6e-237">Dieser Controller definieren Aktionsmethoden anwenden, um die folgenden spezifischen Anforderungen zu verarbeiten:</span><span class="sxs-lookup"><span data-stu-id="7bd6e-237">That controller will define action methods to handle each of the following specific requests:</span></span>

- <span data-ttu-id="7bd6e-238">Eine Angebotsseite des Genres Musik in die Music Store</span><span class="sxs-lookup"><span data-stu-id="7bd6e-238">A listing page of the music genres in the Music Store</span></span>
- <span data-ttu-id="7bd6e-239">Seite "Durchsuchen", die alle die Alben für eine bestimmte "Genre" aufgeführt werden</span><span class="sxs-lookup"><span data-stu-id="7bd6e-239">A browse page that lists all of the music albums for a particular genre</span></span>
- <span data-ttu-id="7bd6e-240">Eine Seite, die Informationen zu einem bestimmten Musikalbum anzeigt</span><span class="sxs-lookup"><span data-stu-id="7bd6e-240">A details page that shows information about a specific music album</span></span>

<span data-ttu-id="7bd6e-241">Für den Bereich in dieser Übung gibt diese Aktionen jetzt einfach eine Zeichenfolge zurück.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-241">For the scope of this exercise, those actions will simply return a string by now.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Adding_a_New_StoreController_Class"></a>
#### <a name="task-1---adding-a-new-storecontroller-class"></a><span data-ttu-id="7bd6e-242">Aufgabe 1: Hinzufügen einer neuen StoreController-Klasse</span><span class="sxs-lookup"><span data-stu-id="7bd6e-242">Task 1 - Adding a New StoreController Class</span></span>

<span data-ttu-id="7bd6e-243">In dieser Aufgabe fügen Sie einen neuen Controller hinzu.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-243">In this task, you will add a new Controller.</span></span>

1. <span data-ttu-id="7bd6e-244">Wenn nicht bereits geöffnet ist, starten Sie **Visual Studio Express für Web 2012**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-244">If not already open, start **VS Express for Web 2012**.</span></span>
2. <span data-ttu-id="7bd6e-245">In der **Datei** Menü wählen **geöffneten Projekt**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-245">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="7bd6e-246">Navigieren Sie in das Dialogfeld "Projekt öffnen" zum **Source\Ex02-CreatingAController\Begin**Option **Begin.sln** , und klicken Sie auf **öffnen**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-246">In the Open Project dialog, browse to **Source\Ex02-CreatingAController\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="7bd6e-247">Alternativ kann der Projektmappe fortgesetzt werden, dass Sie nach Abschluss der vorherigen Übung abgerufen.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-247">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="7bd6e-248">Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung, Sie müssen einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-248">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="7bd6e-249">Zu diesem Zweck klicken Sie auf die **Projekt** Menü **NuGet-Pakete verwalten**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-249">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="7bd6e-250">In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-250">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="7bd6e-251">Abschließend erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-251">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="7bd6e-252">Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken in Ihrem Projekt, Versand Verringern der Projektgröße.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-252">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="7bd6e-253">Mit NuGet Power Tools können werden durch Angabe von Versionen des Pakets in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, die, das Sie das Projekt ausführen, können.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-253">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="7bd6e-254">Deshalb müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit wird.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-254">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="7bd6e-255">Fügen Sie dem neuen Controller hinzu.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-255">Add the new controller.</span></span> <span data-ttu-id="7bd6e-256">Dazu, mit der Maustaste der **Controller** Ordner im Projektmappen-Explorer, wählen Sie **hinzufügen** und klicken Sie dann die **Controller** Befehl.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-256">To do this, right-click the **Controllers** folder within the Solution Explorer, select **Add** and then the **Controller** command.</span></span> <span data-ttu-id="7bd6e-257">Ändern der **Controllername** zu *StoreController*, und klicken Sie auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-257">Change the **Controller Name** to *StoreController*, and click **Add**.</span></span>

    <span data-ttu-id="7bd6e-258">![Controller-Dialogfeld "hinzufügen"](aspnet-mvc-4-fundamentals/_static/image8.png "Controller-Dialogfeld \"hinzufügen\"")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-258">![Add Controller Dialog](aspnet-mvc-4-fundamentals/_static/image8.png "Add Controller Dialog")</span></span>

    <span data-ttu-id="7bd6e-259">*Controller-Dialogfeld "hinzufügen"*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-259">*Add Controller Dialog*</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_Modifying_the_StoreControllers_Actions"></a>
#### <a name="task-2---modifying-the-storecontrollers-actions"></a><span data-ttu-id="7bd6e-260">Aufgabe 2: Ändern der StoreController Aktionen</span><span class="sxs-lookup"><span data-stu-id="7bd6e-260">Task 2 - Modifying the StoreController's Actions</span></span>

<span data-ttu-id="7bd6e-261">In dieser Aufgabe ändern Sie die Controller-Methoden, die aufgerufen werden **Aktionen**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-261">In this task, you will modify the Controller methods that are called **actions**.</span></span> <span data-ttu-id="7bd6e-262">Aktionen sind verantwortlich für die Verarbeitung von URL-Anforderungen, und bestimmen den Inhalt, der zurück an den Browser oder die Benutzer, die die URL aufgerufen gesendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-262">Actions are responsible for handling URL requests and determining the content that should be sent back to the browser or user that invoked the URL.</span></span>

1. <span data-ttu-id="7bd6e-263">Die **StoreController** Klasse verfügt bereits über ein **Index** Methode.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-263">The **StoreController** class already has an **Index** method.</span></span> <span data-ttu-id="7bd6e-264">Sie verwenden es später in diesem Kurs auf um die Seite zu implementieren, die alle Genres im Music Store auflistet.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-264">You will use it later in this Lab to implement the page that lists all genres of the music store.</span></span> <span data-ttu-id="7bd6e-265">Ersetzen Sie vorerst einfach die **Index** Methode durch den folgenden Code, der eine Zeichenfolge zurückgibt, &quot;Grüße aus Store.Index()&quot;:</span><span class="sxs-lookup"><span data-stu-id="7bd6e-265">For now, just replace the **Index** method with the following code that returns a string &quot;Hello from Store.Index()&quot;:</span></span>

    <span data-ttu-id="7bd6e-266">(Codeausschnitt - *Grundlagen von ASP.NET MVC 4 - Ex2 StoreController Index*)</span><span class="sxs-lookup"><span data-stu-id="7bd6e-266">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex2 StoreController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample2.cs)]
2. <span data-ttu-id="7bd6e-267">Hinzufügen **Durchsuchen** und **Details** Methoden.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-267">Add **Browse** and **Details** methods.</span></span> <span data-ttu-id="7bd6e-268">Zu diesem Zweck fügen Sie den folgenden Code der **StoreController**:</span><span class="sxs-lookup"><span data-stu-id="7bd6e-268">To do this, add the following code to the **StoreController**:</span></span>

    <span data-ttu-id="7bd6e-269">(Codeausschnitt - *Grundlagen von ASP.NET MVC 4 - Ex2 StoreController BrowseAndDetails*)</span><span class="sxs-lookup"><span data-stu-id="7bd6e-269">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex2 StoreController BrowseAndDetails*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample3.cs)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="7bd6e-270">Aufgabe 3: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="7bd6e-270">Task 3 - Running the Application</span></span>

<span data-ttu-id="7bd6e-271">In dieser Aufgabe werden Sie die Anwendung in einem Webbrowser testen.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-271">In this task, you will try out the Application in a web browser.</span></span>

1. <span data-ttu-id="7bd6e-272">Drücken Sie **F5** zum Ausführen der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-272">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="7bd6e-273">Das Projekt startet der **Startseite** Seite.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-273">The project starts in the **Home** page.</span></span> <span data-ttu-id="7bd6e-274">Ändern Sie die URL, um die Implementierung der einzelnen Aktionen zu überprüfen.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-274">Change the URL to verify each action's implementation.</span></span>

    1. <span data-ttu-id="7bd6e-275">**/ Store**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-275">**/Store**.</span></span> <span data-ttu-id="7bd6e-276">Sehen Sie  **&quot;Grüße aus Store.Index()&quot;**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-276">You will see **&quot;Hello from Store.Index()&quot;**.</span></span>
    2. <span data-ttu-id="7bd6e-277">**/ Store/durchsuchen**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-277">**/Store/Browse**.</span></span> <span data-ttu-id="7bd6e-278">Sehen Sie  **&quot;Grüße aus Store.Browse()&quot;**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-278">You will see **&quot;Hello from Store.Browse()&quot;**.</span></span>
    3. <span data-ttu-id="7bd6e-279">**/ Store/Details**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-279">**/Store/Details**.</span></span> <span data-ttu-id="7bd6e-280">Sehen Sie  **&quot;Grüße aus Store.Details()&quot;**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-280">You will see **&quot;Hello from Store.Details()&quot;**.</span></span>

        <span data-ttu-id="7bd6e-281">![Durchsuchen von StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "StoreBrowse durchsuchen")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-281">![Browsing StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "Browsing StoreBrowse")</span></span>

        <span data-ttu-id="7bd6e-282">*/Store/Browse durchsuchen*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-282">*Browsing /Store/Browse*</span></span>
3. <span data-ttu-id="7bd6e-283">Schließen Sie den Browser.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-283">Close the browser.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Passing_parameters_to_a_Controller"></a>
### <a name="exercise-3-passing-parameters-to-a-controller"></a><span data-ttu-id="7bd6e-284">Übung 3: Übergeben von Parametern an einen Controller</span><span class="sxs-lookup"><span data-stu-id="7bd6e-284">Exercise 3: Passing parameters to a Controller</span></span>

<span data-ttu-id="7bd6e-285">Bis jetzt haben Sie durch den Controller, Konstante Zeichenfolgen zurück, wurde.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-285">Until now, you have been returning constant strings from the Controllers.</span></span> <span data-ttu-id="7bd6e-286">In dieser Übung lernen Sie, wie zum Übergeben von Parametern an einen Controller mithilfe der URL und Abfragezeichenfolge, und nehmen Sie dann die methodenaktionen, die mit dem Text an den Browser zu reagieren.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-286">In this exercise you will learn how to pass parameters to a Controller using the URL and querystring, and then making the method actions respond with text to the browser.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Adding_Genre_Parameter_to_StoreController"></a>
#### <a name="task-1---adding-genre-parameter-to-storecontroller"></a><span data-ttu-id="7bd6e-287">Aufgabe 1: Hinzufügen von "Genre"-Parameter, um StoreController</span><span class="sxs-lookup"><span data-stu-id="7bd6e-287">Task 1 - Adding Genre Parameter to StoreController</span></span>

<span data-ttu-id="7bd6e-288">In dieser Aufgabe verwenden Sie die **Querystring** Senden von Parametern, die **Durchsuchen** Aktionsmethode in der **StoreController**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-288">In this task, you will use the **querystring** to send parameters to the **Browse** action method in the **StoreController**.</span></span>

1. <span data-ttu-id="7bd6e-289">Wenn nicht bereits geöffnet ist, starten Sie **Visual Studio Express für Web**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-289">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="7bd6e-290">In der **Datei** Menü wählen **geöffneten Projekt**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-290">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="7bd6e-291">Navigieren Sie in das Dialogfeld "Projekt öffnen" zum **Source\Ex03-PassingParametersToAController\Begin**Option **Begin.sln** , und klicken Sie auf **öffnen**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-291">In the Open Project dialog, browse to **Source\Ex03-PassingParametersToAController\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="7bd6e-292">Alternativ kann der Projektmappe fortgesetzt werden, dass Sie nach Abschluss der vorherigen Übung abgerufen.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-292">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="7bd6e-293">Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung, Sie müssen einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-293">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="7bd6e-294">Zu diesem Zweck klicken Sie auf die **Projekt** Menü **NuGet-Pakete verwalten**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-294">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="7bd6e-295">In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-295">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="7bd6e-296">Abschließend erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-296">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="7bd6e-297">Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken in Ihrem Projekt, Versand Verringern der Projektgröße.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-297">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="7bd6e-298">Mit NuGet Power Tools können werden durch Angabe von Versionen des Pakets in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, die, das Sie das Projekt ausführen, können.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-298">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="7bd6e-299">Deshalb müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit wird.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-299">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="7bd6e-300">Open **StoreController** Klasse.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-300">Open **StoreController** class.</span></span> <span data-ttu-id="7bd6e-301">Klicken Sie hierzu in der **Projektmappen-Explorer**, erweitern Sie die **Controller** Ordner und doppelklicken Sie auf **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-301">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
4. <span data-ttu-id="7bd6e-302">Ändern der **Durchsuchen** Methode, die einen Zeichenfolgenparameter zum Anfordern von für eine spezifische Genre hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-302">Change the **Browse** method, adding a string parameter to request for a specific genre.</span></span> <span data-ttu-id="7bd6e-303">ASP.NET MVC automatisch alle Abfragezeichenfolge übergeben oder FormPost-Parameter, die mit dem Namen **"Genre"** beim Aufrufen dieser Aktion-Methode.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-303">ASP.NET MVC will automatically pass any querystring or form post parameters named **genre** to this action method when invoked.</span></span> <span data-ttu-id="7bd6e-304">Ersetzen Sie hierzu die **Durchsuchen** Methode durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="7bd6e-304">To do this, replace the **Browse** method with the following code:</span></span>

    <span data-ttu-id="7bd6e-305">(Codeausschnitt - *Grundlagen von ASP.NET MVC 4 - Ex3 StoreController BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="7bd6e-305">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex3 StoreController BrowseMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="7bd6e-306">Sie verwenden die **HttpUtility.HtmlEncode durchführen** Utility-Methode, um Benutzer daran gehindert Einfügen von Javascript in der Ansicht mit einem Link wie   **/Store/durchsuchen? Genre =&lt;Skript&gt;Window.Location = "[http://hackersite.com](http://hackersite.com)"&lt;/script&gt;**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-306">You are using the **HttpUtility.HtmlEncode** utility method to prevents users from injecting Javascript into the View with a link like **/Store/Browse?Genre=&lt;script&gt;window.location='[http://hackersite.com](http://hackersite.com)'&lt;/script&gt;**.</span></span>
> 
> <span data-ttu-id="7bd6e-307">Weitere Informationen finden Sie unter [diesem Msdn-Artikel](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).</span><span class="sxs-lookup"><span data-stu-id="7bd6e-307">For further explanation, please visit [this msdn article](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="7bd6e-308">Aufgabe 2: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="7bd6e-308">Task 2 - Running the Application</span></span>

<span data-ttu-id="7bd6e-309">In dieser Aufgabe Sie die Anwendung in einem Webbrowser testen und Verwenden der **"Genre"** Parameter.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-309">In this task, you will try out the Application in a web browser and use the **genre** parameter.</span></span>

1. <span data-ttu-id="7bd6e-310">Drücken Sie **F5** zum Ausführen der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-310">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="7bd6e-311">Das Projekt startet der **Startseite** Seite.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-311">The project starts in the **Home** page.</span></span> <span data-ttu-id="7bd6e-312">Ändern Sie die URL zum   */Store/durchsuchen? Genre = Disco* um sicherzustellen, dass die Aktion den genreparameter empfängt.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-312">Change the URL to */Store/Browse?Genre=Disco* to verify that the action receives the genre parameter.</span></span>

    <span data-ttu-id="7bd6e-313">![Durchsuchen von StoreBrowseGenre = Disco](aspnet-mvc-4-fundamentals/_static/image10.png "durchsuchen StoreBrowseGenre = Disco")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-313">![Browsing StoreBrowseGenre=Disco](aspnet-mvc-4-fundamentals/_static/image10.png "Browsing StoreBrowseGenre=Disco")</span></span>

    <span data-ttu-id="7bd6e-314">*Durchsuchen Sie /Store/Browse? Genre = Disco*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-314">*Browsing /Store/Browse?Genre=Disco*</span></span>
3. <span data-ttu-id="7bd6e-315">Schließen Sie den Browser.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-315">Close the browser.</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Adding_an_Id_Parameter_Embedded_in_the_URL"></a>
#### <a name="task-3---adding-an-id-parameter-embedded-in-the-url"></a><span data-ttu-id="7bd6e-316">Aufgabe 3: Hinzufügen eines Id-Parameters, die in der URL eingebettet</span><span class="sxs-lookup"><span data-stu-id="7bd6e-316">Task 3 - Adding an Id Parameter Embedded in the URL</span></span>

<span data-ttu-id="7bd6e-317">In dieser Aufgabe verwenden Sie die **URL** übergeben eine **Id** Parameter, um die **Details** Action-Methode der der **StoreController**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-317">In this task, you will use the **URL** to pass an **Id** parameter to the **Details** action method of the **StoreController**.</span></span> <span data-ttu-id="7bd6e-318">Routing-Konvention wird das Segment der URL nach dem Namen der Aktionsmethode behandelt, als Parameter mit dem Namen ASP.NETs-Standard **Id**. Wenn Ihrer Aktionsmethode Parameter Id verfügt, wird ASP.NET MVC automatisch das URL-Segment für Sie als Parameter übergeben.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-318">ASP.NET MVC's default routing convention is to treat the segment of a URL after the action method name as a parameter named **Id**. If your action method has parameter named Id, then ASP.NET MVC will automatically pass the URL segment to you as a parameter.</span></span> <span data-ttu-id="7bd6e-319">In der URL **Store/Details/5**, **Id** wird als interpretiert **5**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-319">In the URL **Store/Details/5**, **Id** will be interpreted as **5**.</span></span>

1. <span data-ttu-id="7bd6e-320">Ändern der **Details** -Methode der der **StoreController**, Hinzufügen einer **Int** Parameter namens **Id**. Ersetzen Sie hierzu die **Details** Methode durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="7bd6e-320">Change the **Details** method of the **StoreController**, adding an **int** parameter called **id**. To do this, replace the **Details** method with the following code:</span></span>

    <span data-ttu-id="7bd6e-321">(Codeausschnitt - *Grundlagen von ASP.NET MVC 4 - Ex3 StoreController DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="7bd6e-321">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex3 StoreController DetailsMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample5.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="7bd6e-322">Aufgabe 4: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="7bd6e-322">Task 4 - Running the Application</span></span>

<span data-ttu-id="7bd6e-323">In dieser Aufgabe Sie die Anwendung in einem Webbrowser testen und Verwenden der **Id** Parameter.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-323">In this task, you will try out the Application in a web browser and use the **Id** parameter.</span></span>

1. <span data-ttu-id="7bd6e-324">Drücken Sie **F5** zum Ausführen der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-324">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="7bd6e-325">Das Projekt startet der **Startseite** Seite.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-325">The project starts in the **Home** page.</span></span> <span data-ttu-id="7bd6e-326">Ändern Sie die URL zum */Store/Details/5* um sicherzustellen, dass die Aktion mit den Id-Parameter empfängt.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-326">Change the URL to */Store/Details/5* to verify that the action receives the id parameter.</span></span>

    <span data-ttu-id="7bd6e-327">![Durchsuchen von StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "StoreDetails5 durchsuchen")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-327">![Browsing StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "Browsing StoreDetails5")</span></span>

    <span data-ttu-id="7bd6e-328">*/Store/Details/5 durchsuchen*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-328">*Browsing /Store/Details/5*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Creating_a_View"></a>
### <a name="exercise-4-creating-a-view"></a><span data-ttu-id="7bd6e-329">Übung 4: Erstellen einer Ansicht</span><span class="sxs-lookup"><span data-stu-id="7bd6e-329">Exercise 4: Creating a View</span></span>

<span data-ttu-id="7bd6e-330">Bisher haben Sie Zeichenfolgen aus Controlleraktionen zurückgegeben wurde.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-330">So far you have been returning strings from controller actions.</span></span> <span data-ttu-id="7bd6e-331">Obwohl dies ist eine gute Möglichkeit für das Verständnis der Funktionsweise von Controllern, ist es nicht, wie Ihre echten Webanwendungen erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-331">Although that is a useful way of understanding how controllers work, it is not how your real Web applications are built.</span></span> <span data-ttu-id="7bd6e-332">Ansichten sind Komponenten, die einen besseren Ansatz zum Generieren von HTML zurück an den Browser mit der Verwendung von Vorlagendateien bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-332">Views are components that provide a better approach for generating HTML back to the browser with the use of template files.</span></span>

<span data-ttu-id="7bd6e-333">In dieser Übung lernen Sie das Hinzufügen eine Masterseite "Layout" zum Einrichten einer Vorlage für allgemeine HTML-Inhalt, ein StyleSheet, um das Aussehen und Verhalten der Website und schließlich eine ansichtsvorlage zu HomeController HTML zurückgeben zu verbessern.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-333">In this exercise you will learn how to add a layout master page to setup a template for common HTML content, a StyleSheet to enhance the look and feel of the site and finally a View template to enable HomeController to return HTML.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Modifying_the_file__layoutcshtml"></a>
#### <a name="task-1---modifying-the-file-layoutcshtml"></a><span data-ttu-id="7bd6e-334">Aufgabe 1: Ändern der Datei \_layout.cshtml</span><span class="sxs-lookup"><span data-stu-id="7bd6e-334">Task 1 - Modifying the file \_layout.cshtml</span></span>

<span data-ttu-id="7bd6e-335">Die Datei **~/Views/Shared/\_layout.cshtml** können Sie eine Vorlage für gemeinsamer HTML-Code für die Verwendung in der gesamten Website einrichten.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-335">The file **~/Views/Shared/\_layout.cshtml** allows you to setup a template for common HTML to use across the entire website.</span></span> <span data-ttu-id="7bd6e-336">In dieser Aufgabe fügen Sie im Bereich auf der Startseite "und" Store eine Layout-Masterseite mit ein common-Header mit Links hinzu.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-336">In this task you will add a layout master page with a common header with links to the Home page and Store area.</span></span>

1. <span data-ttu-id="7bd6e-337">Wenn nicht bereits geöffnet ist, starten Sie **Visual Studio Express für Web**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-337">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="7bd6e-338">In der **Datei** Menü wählen **geöffneten Projekt**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-338">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="7bd6e-339">Navigieren Sie in das Dialogfeld "Projekt öffnen" zum **Source\Ex04-CreatingAView\Begin**Option **Begin.sln** , und klicken Sie auf **öffnen**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-339">In the Open Project dialog, browse to **Source\Ex04-CreatingAView\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="7bd6e-340">Alternativ kann der Projektmappe fortgesetzt werden, dass Sie nach Abschluss der vorherigen Übung abgerufen.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-340">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="7bd6e-341">Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung, Sie müssen einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-341">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="7bd6e-342">Zu diesem Zweck klicken Sie auf die **Projekt** Menü **NuGet-Pakete verwalten**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-342">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="7bd6e-343">In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-343">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="7bd6e-344">Abschließend erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-344">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="7bd6e-345">Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken in Ihrem Projekt, Versand Verringern der Projektgröße.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-345">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="7bd6e-346">Mit NuGet Power Tools können werden durch Angabe von Versionen des Pakets in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, die, das Sie das Projekt ausführen, können.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-346">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="7bd6e-347">Deshalb müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit wird.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-347">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="7bd6e-348">Die Datei  <strong>\_layout.cshtml</strong> enthält die HTML-Containerlayout für alle Seiten auf der Website.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-348">The file <strong>\_layout.cshtml</strong> contains the HTML container layout for all pages on the site.</span></span> <span data-ttu-id="7bd6e-349">Es enthält die <strong>&lt;html&gt;</strong> -Element für die HTML-Antwort als auch die <strong>&lt;Head&gt;</strong> und <strong>&lt;Text&gt;</strong> Elemente.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-349">It includes the <strong>&lt;html&gt;</strong> element for the HTML response, as well as the <strong>&lt;head&gt;</strong> and <strong>&lt;body&gt;</strong> elements.</span></span> <span data-ttu-id="7bd6e-350"><strong>@RenderBody()</strong> im HTML-Code identifizieren Text Regionen diese Ansicht, die Vorlagen werden in der Lage, sich mit dynamischen Inhalten zu füllen.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-350"><strong>@RenderBody()</strong> within the HTML body identify regions that view templates will be able to fill in with dynamic content.</span></span>
   <span data-ttu-id="7bd6e-351">(C#)</span><span class="sxs-lookup"><span data-stu-id="7bd6e-351">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample6.cshtml)]
4. <span data-ttu-id="7bd6e-352">Fügen Sie einen allgemeinen Header mit Links in den Bereich "Startseite" und "Store" auf allen Seiten der Website hinzu.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-352">Add a common header with links to the Home page and Store area on all pages in the site.</span></span> <span data-ttu-id="7bd6e-353">Zu diesem Zweck fügen Sie den folgenden Code unter &lt;Text&gt; Anweisung.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-353">In order to do that, add the following code below &lt;body&gt; statement.</span></span>
   <span data-ttu-id="7bd6e-354">(C#)</span><span class="sxs-lookup"><span data-stu-id="7bd6e-354">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample7.cshtml)]
5. <span data-ttu-id="7bd6e-355">Enthalten Sie ein DIV-Element um das Body-Bereich jeder Seite zu rendern.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-355">Include a div to render the body section of each page.</span></span> <span data-ttu-id="7bd6e-356">Ersetzen Sie dies  <strong>@RenderBody()</strong> durch den folgenden Code der Higlighted: (c#)</span><span class="sxs-lookup"><span data-stu-id="7bd6e-356">Replace <strong>@RenderBody()</strong> with the following higlighted code: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample8.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="7bd6e-357">Wussten Sie schon?</span><span class="sxs-lookup"><span data-stu-id="7bd6e-357">Did you know?</span></span> <span data-ttu-id="7bd6e-358">Visual Studio 2012 stellt Ausschnitte, die häufig verwendeten Code hinzufügen, in HTML, Codedateien und mehr erleichtern.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-358">Visual Studio 2012 has snippets that make it easy to add commonly used code in HTML, code files and more!</span></span> <span data-ttu-id="7bd6e-359">Versuchen Sie es, indem Sie eingeben **&lt;Div&gt;** und **Registerkarte** zweimal, um eine vollständige einfügen **Div** Tag.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-359">Try it out by typing **&lt;div&gt;** and pressing **TAB** twice to insert a complete **div** tag.</span></span>

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_CSS_Stylesheet"></a>
#### <a name="task-2---adding-css-stylesheet"></a><span data-ttu-id="7bd6e-360">Aufgabe 2: Hinzufügen von CSS-Stylesheet</span><span class="sxs-lookup"><span data-stu-id="7bd6e-360">Task 2 - Adding CSS Stylesheet</span></span>

<span data-ttu-id="7bd6e-361">Die leere Projektvorlage enthält eine sehr optimierte CSS-Datei, die Stile, die zum Anzeigen von grundlegenden Formen und validierungsmeldungen enthält nur.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-361">The empty project template includes a very streamlined CSS file which just includes styles used to display basic forms and validation messages.</span></span> <span data-ttu-id="7bd6e-362">Sie werden zusätzliche CSS- und -Images (die möglicherweise von einem Designer bereitgestellt werden) verwenden, um das Aussehen und Verhalten der Website zu verbessern.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-362">You will use additional CSS and images (potentially provided by a designer) in order to enhance the look and feel of the site.</span></span>

<span data-ttu-id="7bd6e-363">In dieser Aufgabe fügen Sie eine CSS-Stylesheet an, um die Formatvorlagen der Site zu definieren.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-363">In this task, you will add a CSS stylesheet to define the styles of the site.</span></span>

1. <span data-ttu-id="7bd6e-364">Die CSS-Datei und die Bilder befinden sich der **Source\Assets\Content** Ordner dieser Anleitung.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-364">The CSS file and images are included in the **Source\Assets\Content** folder of this Lab.</span></span> <span data-ttu-id="7bd6e-365">Um diese an die Anwendung hinzuzufügen, ziehen Sie in ihrer Inhalte mit einer **Windows Explorer** Einblick in die **Projektmappen-Explorer** in Visual Studio Express für Web, wie unten dargestellt:</span><span class="sxs-lookup"><span data-stu-id="7bd6e-365">In order to add them to the application, drag their content from a **Windows Explorer** window into the **Solution Explorer** in Visual Studio Express for Web, as shown below:</span></span>

    <span data-ttu-id="7bd6e-366">![Style-Inhalte ziehen](aspnet-mvc-4-fundamentals/_static/image12.png "Style-Inhalte ziehen")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-366">![Dragging style contents](aspnet-mvc-4-fundamentals/_static/image12.png "Dragging style contents")</span></span>

    <span data-ttu-id="7bd6e-367">*Ziehen die Style-Inhalt*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-367">*Dragging style contents*</span></span>
2. <span data-ttu-id="7bd6e-368">Eine Warnmeldung angezeigt, in der Sie gefragt, ersetzen Sie dies zu bestätigen **"Site.CSS"** Datei- und einige vorhandene Images.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-368">A warning dialog will appear, asking for confirmation to replace **Site.css** file and some existing images.</span></span> <span data-ttu-id="7bd6e-369">Überprüfen Sie **anwenden, um alle Elemente** , und klicken Sie auf **Ja**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-369">Check **Apply to all items** and click **Yes**.</span></span>

<a id="Ex4Task3"></a>

<a id="Task_3_-_Adding_a_View_Template"></a>
#### <a name="task-3---adding-a-view-template"></a><span data-ttu-id="7bd6e-370">Aufgabe 3: Hinzufügen einer Vorlage anzeigen</span><span class="sxs-lookup"><span data-stu-id="7bd6e-370">Task 3 - Adding a View Template</span></span>

<span data-ttu-id="7bd6e-371">In dieser Aufgabe fügen Sie eine Vorlage anzeigen, um die HTML-Antwort zu generieren, die die Masterseite Layout verwendet werden, und CSS in dieser Übung hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-371">In this task, you will add a View template to generate the HTML response that will use the layout master page and CSS added in this exercise.</span></span>

1. <span data-ttu-id="7bd6e-372">Um eine ansichtsvorlage beim Durchsuchen der Homepage der Website zu verwenden, müssen Sie zuerst an, dass anstelle einer Zeichenfolge, die **HomeController Index** Methode gibt zurück, eine **Ansicht**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-372">To use a View template when browsing the site's home page, you will first need to indicate that instead of returning a string, the **HomeController Index** method will return a **View**.</span></span> <span data-ttu-id="7bd6e-373">Open **HomeController** Klasse, und ändern die **Index** -Methode zur Rückgabe einer **ActionResult**, und zurückliefern **View()**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-373">Open **HomeController** class and change its **Index** method to return an **ActionResult**, and have it return **View()**.</span></span>

    <span data-ttu-id="7bd6e-374">(Codeausschnitt - *Grundlagen von ASP.NET MVC 4 - Ex4 HomeController Index*)</span><span class="sxs-lookup"><span data-stu-id="7bd6e-374">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex4 HomeController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample9.cs)]
2. <span data-ttu-id="7bd6e-375">Nun müssen Sie eine geeignete ansichtsvorlage hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-375">Now, you need to add an appropriate View template.</span></span> <span data-ttu-id="7bd6e-376">Zu diesem Zweck **mit der rechten Maustaste** innerhalb der **Index** Aktionsmethode, und wählen **Ansicht hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-376">To do this, **right-click** inside the **Index** action method and select **Add View**.</span></span> <span data-ttu-id="7bd6e-377">Hierdurch wird die **Ansicht hinzufügen** Dialogfeld.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-377">This will bring up the **Add View** dialog.</span></span>

    <span data-ttu-id="7bd6e-378">![Hinzufügen einer Ansicht aus, in die Indexmethode](aspnet-mvc-4-fundamentals/_static/image13.png "Hinzufügen einer Ansicht aus, in der Index-Methode")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-378">![Adding a View from within the Index method](aspnet-mvc-4-fundamentals/_static/image13.png "Adding a View from within the Index method")</span></span>

    <span data-ttu-id="7bd6e-379">*Hinzufügen einer Ansicht aus, in der Index-Methode*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-379">*Adding a View from within the Index method*</span></span>
3. <span data-ttu-id="7bd6e-380">Die **Ansicht hinzufügen** Dialogfeld wird angezeigt, um eine ansichtsvorlagendatei zu generieren.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-380">The **Add View** Dialog will appear to generate a View template file.</span></span> <span data-ttu-id="7bd6e-381">Standardmäßig füllt dieses Dialogfeld den Namen der Vorlage anzeigen, damit diese die Aktionsmethode entspricht, die dazu verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-381">By default, this dialog pre-populates the name of the View template so that it matches the action method that will use it.</span></span> <span data-ttu-id="7bd6e-382">Da Sie verwendet die **Ansicht hinzufügen** Kontextmenü innerhalb der **Index** Action-Methode im HomeController, die **Ansicht hinzufügen** Dialogfeld hat den Index als den Standardnamen für die Ansicht.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-382">Because you used the **Add View** context menu within the **Index** action method within the HomeController, the **Add View** dialog has Index as the default view name.</span></span> <span data-ttu-id="7bd6e-383">Klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-383">Click **Add**.</span></span>

    <span data-ttu-id="7bd6e-384">![Ansicht-Dialogfeld "hinzufügen"](aspnet-mvc-4-fundamentals/_static/image14.png "anzeigen-Dialogfeld \"hinzufügen\"")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-384">![Add View Dialog](aspnet-mvc-4-fundamentals/_static/image14.png "Add View Dialog")</span></span>

    <span data-ttu-id="7bd6e-385">*Ansicht-Dialogfeld "hinzufügen"*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-385">*Add View Dialog*</span></span>
4. <span data-ttu-id="7bd6e-386">Visual Studio generiert eine **"Index.cshtml"** ansichtsvorlage innerhalb der **Views\Home** Ordner und anschließend geöffnet.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-386">Visual Studio generates an **Index.cshtml** view template inside the **Views\Home** folder and then opens it.</span></span>

    <span data-ttu-id="7bd6e-387">![Startseite der Indexansicht erstellt](aspnet-mvc-4-fundamentals/_static/image15.png "Startseite Indexansicht erstellt")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-387">![Home Index view created](aspnet-mvc-4-fundamentals/_static/image15.png "Home Index view created")</span></span>

    <span data-ttu-id="7bd6e-388">*Home Indexansicht erstellt*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-388">*Home Index view created*</span></span>

    > [!NOTE]
    > <span data-ttu-id="7bd6e-389">Name und Speicherort der der **"Index.cshtml"** Datei relevant ist und die standardmäßige ASP.NET-MVC-Namenskonventionen entspricht.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-389">name and location of the **Index.cshtml** file is relevant and follows the default ASP.NET MVC naming conventions.</span></span>
    > 
    > <span data-ttu-id="7bd6e-390">Der Ordner \Views\**Startseite** den Controllernamen entspricht (**Startseite** Controller).</span><span class="sxs-lookup"><span data-stu-id="7bd6e-390">The folder \Views\**Home** matches the controller name (**Home** Controller).</span></span> <span data-ttu-id="7bd6e-391">Namen der Ansicht (**Index**), entspricht der Aktionsmethode des Controllers der Ansicht anzeigen lassen.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-391">The View template name (**Index**), matches the controller action method which will be displaying the View.</span></span>
    > 
    > <span data-ttu-id="7bd6e-392">Auf diese Weise wird ASP.NET MVC vermieden, müssen den Namen oder Speicherort, der eine ansichtsvorlage explizit angeben, wenn diese Benennungskonvention zu verwenden, um eine Ansicht zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-392">This way, ASP.NET MVC avoids having to explicitly specify the name or location of a View template when using this naming convention to return a View.</span></span>
5. <span data-ttu-id="7bd6e-393">Die generierte ansichtsvorlage basiert auf der  **\_layout.cshtml** Vorlage, die zuvor definierten.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-393">The generated View template is based on the **\_layout.cshtml** template earlier defined.</span></span> <span data-ttu-id="7bd6e-394">Aktualisieren Sie die ViewBag.Title-Eigenschaft, um **Startseite**, und ändern Sie den Hauptinhalt **Dies ist die Startseite**, wie im folgenden Code gezeigt:</span><span class="sxs-lookup"><span data-stu-id="7bd6e-394">Update the ViewBag.Title property to **Home**, and change the main content to **This is the Home Page**, as shown in the code below:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample10.cshtml)]
6. <span data-ttu-id="7bd6e-395">Wählen Sie **MvcMusicStore** -Projekt in der Projektmappen-Explorer, und drücken Sie **F5** zum Ausführen der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-395">Select **MvcMusicStore** project in the Solution Explorer and Press **F5** to run the Application.</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_Verification"></a>
#### <a name="task-4-verification"></a><span data-ttu-id="7bd6e-396">Aufgabe 4: Überprüfung</span><span class="sxs-lookup"><span data-stu-id="7bd6e-396">Task 4: Verification</span></span>

<span data-ttu-id="7bd6e-397">Um sicherzustellen, dass Sie alle Schritte in der vorherigen Übung ordnungsgemäß ausgeführt haben, gehen Sie wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="7bd6e-397">In order to verify that you have correctly performed all the steps in the previous exercise, proceed as follows:</span></span>

<span data-ttu-id="7bd6e-398">Mit der Anwendung, die in einem Browser geöffnet wird sollten Sie Folgendes beachten:</span><span class="sxs-lookup"><span data-stu-id="7bd6e-398">With the application opened in a browser, you should note that:</span></span>

1. <span data-ttu-id="7bd6e-399">Des HomeController Index-Aktionsmethode gefunden und angezeigt, die **\Views\Home\Index.cshtml** Vorlage anzeigen, auch wenn der Code aufgerufen **View() zurückgeben**, da die ansichtsvorlage gefolgt der standardmäßige Benennungskonvention zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-399">The HomeController's Index action method found and displayed the **\Views\Home\Index.cshtml** View template, even though the code called **return View()**, because the View template followed the standard naming convention.</span></span>
2. <span data-ttu-id="7bd6e-400">Auf der Startseite zeigt die Willkommensnachricht in definiert die **\Views\Home\Index.cshtml** Vorlage anzeigen.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-400">The Home Page displays the welcome message defined within the **\Views\Home\Index.cshtml** view template.</span></span>
3. <span data-ttu-id="7bd6e-401">Auf der Startseite wird mithilfe der  **\_layout.cshtml** Vorlage, und daher die Willkommensnachricht in der Standardwebsite HTML-Layout enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-401">The Home Page is using the **\_layout.cshtml** template, and so the welcome message is contained within the standard site HTML layout.</span></span>

    <span data-ttu-id="7bd6e-402">![Indexansicht mit dem definierten LayoutPage und Stil Home](aspnet-mvc-4-fundamentals/_static/image16.png "Startseite Ansicht \"Index\" mit dem definierten LayoutPage und Stil")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-402">![Home Index View using the defined LayoutPage and style](aspnet-mvc-4-fundamentals/_static/image16.png "Home Index View using the defined LayoutPage and style")</span></span>

    <span data-ttu-id="7bd6e-403">*Home Ansicht "Index" mit dem definierten LayoutPage und Stil*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-403">*Home Index View using the defined LayoutPage and style*</span></span>

<a id="Exercise5"></a>

<a id="Exercise_5_Creating_a_View_Model"></a>
### <a name="exercise-5-creating-a-view-model"></a><span data-ttu-id="7bd6e-404">Schritt 5: Erstellen von Anzeigemodellen</span><span class="sxs-lookup"><span data-stu-id="7bd6e-404">Exercise 5: Creating a View Model</span></span>

<span data-ttu-id="7bd6e-405">Bis jetzt vorgenommenen Ihre Ansichten, die hartcodierte HTML anzeigen, aber zum Erstellen dynamischer Webanwendungen die ansichtsvorlage erhalten Informationen über den Controller.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-405">So far, you made your Views display hardcoded HTML, but, in order to create dynamic web applications, the View template should receive information from the Controller.</span></span> <span data-ttu-id="7bd6e-406">Ein gängiges Verfahren, die für diesen Zweck verwendet werden wird die **"ViewModel"** Muster, das einen Domänencontroller, um alle Informationen, die zum Generieren der entsprechenden HTML-Antwort benötigt Verpacken kann.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-406">One common technique to be used for that purpose is the **ViewModel** pattern, which allows a Controller to package up all the information needed to generate the appropriate HTML response.</span></span>

<span data-ttu-id="7bd6e-407">In dieser Übung erfahren Sie, wie erstellen eine ViewModel-Klasse, und fügen Sie die erforderlichen Eigenschaften: die Anzahl von Genres in den Speicher und eine Liste mit diesen Genres.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-407">In this exercise, you will learn how to create a ViewModel class and add the required properties: the number of genres in the store and a list of those genres.</span></span> <span data-ttu-id="7bd6e-408">Aktualisieren Sie auch die StoreController zur Verwendung von "ViewModel" erstellt, und schließlich erstellen Sie eine neue Vorlage anzeigen, die die genannten Eigenschaften auf der Seite angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-408">You will also update the StoreController to use the created ViewModel, and finally, you will create a new View template that will display the mentioned properties in the page.</span></span>

<a id="Ex5Task1"></a>

<a id="Task_1_-_Creating_a_ViewModel_Class"></a>
#### <a name="task-1---creating-a-viewmodel-class"></a><span data-ttu-id="7bd6e-409">Aufgabe 1: erstellen eine ViewModel-Klasse</span><span class="sxs-lookup"><span data-stu-id="7bd6e-409">Task 1 - Creating a ViewModel Class</span></span>

<span data-ttu-id="7bd6e-410">In dieser Aufgabe erstellen Sie eine ViewModel-Klasse, die das Store "Genre" Auflistung Szenario implementiert wird.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-410">In this task, you will create a ViewModel class that will implement the Store genre listing scenario.</span></span>

1. <span data-ttu-id="7bd6e-411">Wenn nicht bereits geöffnet ist, starten Sie **Visual Studio Express für Web**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-411">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="7bd6e-412">In der **Datei** Menü wählen **geöffneten Projekt**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-412">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="7bd6e-413">Navigieren Sie in das Dialogfeld "Projekt öffnen" zum **Source\Ex05-CreatingAViewModel\Begin**Option **Begin.sln** , und klicken Sie auf **öffnen**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-413">In the Open Project dialog, browse to **Source\Ex05-CreatingAViewModel\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="7bd6e-414">Alternativ kann der Projektmappe fortgesetzt werden, dass Sie nach Abschluss der vorherigen Übung abgerufen.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-414">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="7bd6e-415">Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung, Sie müssen einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-415">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="7bd6e-416">Zu diesem Zweck klicken Sie auf die **Projekt** Menü **NuGet-Pakete verwalten**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-416">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="7bd6e-417">In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-417">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="7bd6e-418">Abschließend erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-418">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="7bd6e-419">Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken in Ihrem Projekt, Versand Verringern der Projektgröße.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-419">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="7bd6e-420">Mit NuGet Power Tools können werden durch Angabe von Versionen des Pakets in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, die, das Sie das Projekt ausführen, können.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-420">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="7bd6e-421">Deshalb müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit wird.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-421">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="7bd6e-422">Erstellen Sie eine **ViewModels** Ordner für das "ViewModel".</span><span class="sxs-lookup"><span data-stu-id="7bd6e-422">Create a **ViewModels** folder to hold the ViewModel.</span></span> <span data-ttu-id="7bd6e-423">Dazu, mit der Maustaste der obersten Ebene **MvcMusicStore** -Projekt, wählen **hinzufügen** und dann **neuer Ordner**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-423">To do this, right-click the top-level **MvcMusicStore** project, select **Add** and then **New Folder**.</span></span>

    <span data-ttu-id="7bd6e-424">![Hinzufügen eines neuen Ordners](aspnet-mvc-4-fundamentals/_static/image17.png "einen neuen Ordner hinzufügen")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-424">![Adding a new folder](aspnet-mvc-4-fundamentals/_static/image17.png "Adding a new folder")</span></span>

    <span data-ttu-id="7bd6e-425">*Hinzufügen eines neuen Ordners*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-425">*Adding a new folder*</span></span>
4. <span data-ttu-id="7bd6e-426">Nennen Sie den Ordner *ViewModels*.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-426">Name the folder *ViewModels*.</span></span>

    <span data-ttu-id="7bd6e-427">![Ordner "ViewModels" im Projektmappen-Explorer](aspnet-mvc-4-fundamentals/_static/image18.png "Ordner \"ViewModels\" im Projektmappen-Explorer")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-427">![ViewModels folder in Solution Explorer](aspnet-mvc-4-fundamentals/_static/image18.png "ViewModels folder in Solution Explorer")</span></span>

    <span data-ttu-id="7bd6e-428">*Ordner "ViewModels" im Projektmappen-Explorer*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-428">*ViewModels folder in Solution Explorer*</span></span>
5. <span data-ttu-id="7bd6e-429">Erstellen Sie eine **"ViewModel"** Klasse.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-429">Create a **ViewModel** class.</span></span> <span data-ttu-id="7bd6e-430">Zu diesem Zweck mit der Maustaste auf die **ViewModels** Ordner vor kurzem erstellt haben, wählen Sie **hinzufügen** und dann **neues Element**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-430">To do this, right-click on the **ViewModels** folder recently created, select **Add** and then **New Item**.</span></span> <span data-ttu-id="7bd6e-431">Klicken Sie unter **Code**, wählen Sie die **Klasse** Element aus, und nennen Sie die Datei *StoreIndexViewModel.cs*, klicken Sie dann auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-431">Under **Code**, choose the **Class** item and name the file *StoreIndexViewModel.cs*, then click **Add**.</span></span>

    <span data-ttu-id="7bd6e-432">![Hinzufügen einer neuen Klasse](aspnet-mvc-4-fundamentals/_static/image19.png "eine neue Klasse hinzufügen")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-432">![Adding a new Class](aspnet-mvc-4-fundamentals/_static/image19.png "Adding a new Class")</span></span>

    <span data-ttu-id="7bd6e-433">*Hinzufügen einer neuen Klasse*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-433">*Adding a new Class*</span></span>

    <span data-ttu-id="7bd6e-434">![Erstellen von StoreIndexViewModel Klasse](aspnet-mvc-4-fundamentals/_static/image20.png "erstellen StoreIndexViewModel-Klasse")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-434">![Creating StoreIndexViewModel class](aspnet-mvc-4-fundamentals/_static/image20.png "Creating StoreIndexViewModel class")</span></span>

    <span data-ttu-id="7bd6e-435">*Erstellen von StoreIndexViewModel-Klasse*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-435">*Creating StoreIndexViewModel class*</span></span>

<a id="Ex5Task2"></a>

<a id="Task_2_-_Adding_Properties_to_the_ViewModel_class"></a>
#### <a name="task-2---adding-properties-to-the-viewmodel-class"></a><span data-ttu-id="7bd6e-436">Aufgabe 2: Hinzufügen von Eigenschaften, die ViewModel-Klasse</span><span class="sxs-lookup"><span data-stu-id="7bd6e-436">Task 2 - Adding Properties to the ViewModel class</span></span>

<span data-ttu-id="7bd6e-437">Es gibt zwei Parameter an die ansichtsvorlage aus der StoreController übergeben werden, um die erwartete HTML-Antwort zu generieren: die Anzahl von Genres in den Speicher und eine Liste mit diesen Genres.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-437">There are two parameters to be passed from the StoreController to the View template in order to generate the expected HTML response: the number of genres in the store and a list of those genres.</span></span>

<span data-ttu-id="7bd6e-438">In dieser Aufgabe fügen Sie dieser 2 Eigenschaften der **StoreIndexViewModel** Klasse: **NumberOfGenres** (eine ganze Zahl) und **Genres** (eine Liste von Zeichenfolgen).</span><span class="sxs-lookup"><span data-stu-id="7bd6e-438">In this task, you will add those 2 properties to the **StoreIndexViewModel** class: **NumberOfGenres** (an integer) and **Genres** (a list of strings).</span></span>

1. <span data-ttu-id="7bd6e-439">Hinzufügen **NumberOfGenres** und **Genres** Eigenschaften, die die **StoreIndexViewModel** Klasse.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-439">Add **NumberOfGenres** and **Genres** properties to the **StoreIndexViewModel** class.</span></span> <span data-ttu-id="7bd6e-440">Zu diesem Zweck fügen Sie die folgenden 2 Zeilen an die Klassendefinition hinzu:</span><span class="sxs-lookup"><span data-stu-id="7bd6e-440">To do this, add the following 2 lines to the class definition:</span></span>

    <span data-ttu-id="7bd6e-441">(Codeausschnitt - *ASP.NET MVC 4-Grundlagen – Ex5 StoreIndexViewModel Eigenschaften*)</span><span class="sxs-lookup"><span data-stu-id="7bd6e-441">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreIndexViewModel properties*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample11.cs)]

> [!NOTE]
> <span data-ttu-id="7bd6e-442">Die **{get; festlegen;}**  Notation nutzt # Funktion für automatisch implementierte Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-442">The **{ get; set; }** notation makes use of C#'s auto-implemented properties feature.</span></span> <span data-ttu-id="7bd6e-443">Es bietet die Vorteile einer Eigenschaft, ohne uns um ein dahinter liegendes Feld zu deklarieren.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-443">It provides the benefits of a property without requiring us to declare a backing field.</span></span>

<a id="Ex5Task3"></a>

<a id="Task_3_-_Updating_StoreController_to_use_the_StoreIndexViewModel"></a>
#### <a name="task-3---updating-storecontroller-to-use-the-storeindexviewmodel"></a><span data-ttu-id="7bd6e-444">Aufgabe 3: Aktualisieren StoreController der StoreIndexViewModel verwenden</span><span class="sxs-lookup"><span data-stu-id="7bd6e-444">Task 3 - Updating StoreController to use the StoreIndexViewModel</span></span>

<span data-ttu-id="7bd6e-445">Die **StoreIndexViewModel** -Klasse kapselt die erforderlichen Informationen zum Übergeben von **StoreController**des **Index** Methode, um eine Vorlage anzeigen, um eine Antwort zu generieren. .</span><span class="sxs-lookup"><span data-stu-id="7bd6e-445">The **StoreIndexViewModel** class encapsulates the information needed to pass from **StoreController**'s **Index** method to a View template in order to generate a response.</span></span>

<span data-ttu-id="7bd6e-446">In dieser Aufgabe aktualisieren Sie die **StoreController** verwenden die **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-446">In this task, you will update the **StoreController** to use the **StoreIndexViewModel**.</span></span>

1. <span data-ttu-id="7bd6e-447">Open **StoreController** Klasse.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-447">Open **StoreController** class.</span></span>

    <span data-ttu-id="7bd6e-448">![Öffnen StoreController Klasse](aspnet-mvc-4-fundamentals/_static/image21.png "öffnen StoreController-Klasse")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-448">![Opening StoreController class](aspnet-mvc-4-fundamentals/_static/image21.png "Opening StoreController class")</span></span>

    <span data-ttu-id="7bd6e-449">*Öffnende StoreController-Klasse*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-449">*Opening StoreController class*</span></span>
2. <span data-ttu-id="7bd6e-450">Zum Verwenden der **StoreIndexViewModel** -Klasse aus der **StoreController**, den folgenden Namespace hinzufügen, am oberen Rand der **StoreController** Code:</span><span class="sxs-lookup"><span data-stu-id="7bd6e-450">In order to use the **StoreIndexViewModel** class from the **StoreController**, add the following namespace at the top of the **StoreController** code:</span></span>

    <span data-ttu-id="7bd6e-451">(Codeausschnitt - *ASP.NET MVC 4-Grundlagen – Ex5 StoreIndexViewModel Verwenden von ViewModels*)</span><span class="sxs-lookup"><span data-stu-id="7bd6e-451">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreIndexViewModel using ViewModels*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample12.cs)]
3. <span data-ttu-id="7bd6e-452">Ändern der **StoreController**des **Index** Action-Methode so, dass die It erstellt und füllt eine **StoreIndexViewModel** Objekt aus, und leitet diese dann an eine ansichtsvorlage deaktivieren Generieren einer HTML-Antwort mit.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-452">Change the **StoreController**'s **Index** action method so that it creates and populates a **StoreIndexViewModel** object and then passes it off to a View template to generate an HTML response with it.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7bd6e-453">In Übungseinheit &quot;ASP.NET MVC-Modelle und Datenzugriff&quot; schreiben Sie Code, der die Liste der Store Genres aus einer Datenbank abruft.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-453">In Lab &quot;ASP.NET MVC Models and Data Access&quot; you will write code that retrieves the list of store genres from a database.</span></span> <span data-ttu-id="7bd6e-454">In den folgenden Code, erstellen Sie eine **Liste** Dummydaten Genres, das Auffüllen der **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-454">In the following code, you will create a **List** of dummy data genres that will populate the **StoreIndexViewModel**.</span></span>
    > 
    > <span data-ttu-id="7bd6e-455">Nach dem Erstellen und Einrichten der **StoreIndexViewModel** Objekt, übergeben sie als Argument an die **Ansicht** Methode.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-455">After creating and setting up the **StoreIndexViewModel** object, it will be passed as an argument to the **View** method.</span></span> <span data-ttu-id="7bd6e-456">Dies gibt an, dass die ansichtsvorlage zum Generieren einer HTML-Antwort, damit dieses Objekt verwendet.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-456">This indicates that the View template will use that object to generate an HTML response with it.</span></span>
4. <span data-ttu-id="7bd6e-457">Ersetzen Sie die **Index** Methode durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="7bd6e-457">Replace the **Index** method with the following code:</span></span>

    <span data-ttu-id="7bd6e-458">(Codeausschnitt - *ASP.NET MVC 4-Grundlagen – Ex5 StoreController Indexmethode*)</span><span class="sxs-lookup"><span data-stu-id="7bd6e-458">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreController Index method*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample13.cs)]

> [!NOTE]
> <span data-ttu-id="7bd6e-459">Wenn Sie nicht mit c# vertraut sind, Sie können davon ausgehen, dass die Verwendung **Var** bedeutet, dass die **"ViewModel"** Variable ist spät gebunden.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-459">If you're unfamiliar with C#, you may assume that using **var** means that the **viewModel** variable is late-bound.</span></span> <span data-ttu-id="7bd6e-460">Das ist nicht falsch – c#-Compiler verwendet den Typrückschluss basierend auf was Sie auf die Variable zuweisen ermitteln, ob **"ViewModel"** ist vom Typ **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-460">That's not correct - the C# compiler is using type-inference based on what you assign to the variable to determine that **viewModel** is of type **StoreIndexViewModel**.</span></span> <span data-ttu-id="7bd6e-461">Darüber hinaus durch das Kompilieren des lokalen **"ViewModel"** -Variable als ein **StoreIndexViewModel** Geben Sie Get kompilierzeitüberprüfung und Visual Studio Code-Editor-Unterstützung.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-461">Also, by compiling the local **viewModel** variable as a **StoreIndexViewModel** type you get compile-time checking and Visual Studio code-editor support.</span></span>

<a id="Ex5Task4"></a>

<a id="Task_4_-_Creating_a_View_Template_that_Uses_StoreIndexViewModel"></a>
#### <a name="task-4---creating-a-view-template-that-uses-storeindexviewmodel"></a><span data-ttu-id="7bd6e-462">Aufgabe 4: Erstellen einer Vorlage anzeigen, die StoreIndexViewModel verwendet</span><span class="sxs-lookup"><span data-stu-id="7bd6e-462">Task 4 - Creating a View Template that Uses StoreIndexViewModel</span></span>

<span data-ttu-id="7bd6e-463">In dieser Aufgabe erstellen Sie eine Vorlage anzeigen, die ein StoreIndexViewModel-Objekt, das vom Controller übergebenen verwendet, um eine Liste der Genres anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-463">In this task, you will create a View template that will use a StoreIndexViewModel object passed from the Controller to display a list of genres.</span></span>

1. <span data-ttu-id="7bd6e-464">Vor dem Erstellen der neuen Vorlage anzeigen, erstellen Sie das Projekt, damit die **anzeigen-Dialogfeld hinzufügen** kennt die **StoreIndexViewModel** Klasse.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-464">Before creating the new View template, let's build the project so that the **Add View Dialog** knows about the **StoreIndexViewModel** class.</span></span> <span data-ttu-id="7bd6e-465">Erstellen Sie das Projekt durch Auswählen der **erstellen** Menüelement und dann **erstellen MvcMusicStore**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-465">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>

    <span data-ttu-id="7bd6e-466">![Beim Erstellen des Projekts](aspnet-mvc-4-fundamentals/_static/image22.png "beim Erstellen des Projekts")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-466">![Building the project](aspnet-mvc-4-fundamentals/_static/image22.png "Building the project")</span></span>

    <span data-ttu-id="7bd6e-467">*Beim Erstellen des Projekts*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-467">*Building the project*</span></span>
2. <span data-ttu-id="7bd6e-468">Erstellen Sie eine neue ansichtsvorlage für die ein.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-468">Create a new View template.</span></span> <span data-ttu-id="7bd6e-469">Klicken Sie dazu auf, auf die **Index** Methode, und wählen **Ansicht hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-469">To do that, right-click inside the **Index** method and select **Add View**.</span></span>

    <span data-ttu-id="7bd6e-470">![Hinzufügen einer Ansicht](aspnet-mvc-4-fundamentals/_static/image23.png "Hinzufügen einer Ansicht")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-470">![Adding a View](aspnet-mvc-4-fundamentals/_static/image23.png "Adding a View")</span></span>

    <span data-ttu-id="7bd6e-471">*Hinzufügen einer Ansicht*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-471">*Adding a View*</span></span>
3. <span data-ttu-id="7bd6e-472">Da die **anzeigen-Dialogfeld hinzufügen** aufgerufen wurde, aus der **StoreController**, fügen sie die ansichtsvorlage standardmäßig in eine **\Views\Store\Index.cshtml** Datei.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-472">Because the **Add View Dialog** was invoked from the **StoreController**, it will add the View template by default in a **\Views\Store\Index.cshtml** file.</span></span> <span data-ttu-id="7bd6e-473">Überprüfen Sie die **erstellen Sie eine stark typisierte-anzeigen** Kontrollkästchen und wählen Sie dann **StoreIndexViewModel** als die **Modellklasse**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-473">Check the **Create a strongly-typed-view** checkbox and then select **StoreIndexViewModel** as the **Model class**.</span></span> <span data-ttu-id="7bd6e-474">Stellen Sie außerdem sicher, dass die Ansichts-Engine ausgewählt ist **Razor**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-474">Also, make sure that the View engine selected is **Razor**.</span></span> <span data-ttu-id="7bd6e-475">Klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-475">Click **Add**.</span></span>

    <span data-ttu-id="7bd6e-476">![Ansicht-Dialogfeld "hinzufügen"](aspnet-mvc-4-fundamentals/_static/image24.png "anzeigen-Dialogfeld \"hinzufügen\"")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-476">![Add View Dialog](aspnet-mvc-4-fundamentals/_static/image24.png "Add View Dialog")</span></span>

    <span data-ttu-id="7bd6e-477">*Ansicht-Dialogfeld "hinzufügen"*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-477">*Add View Dialog*</span></span>

    <span data-ttu-id="7bd6e-478">Die **\Views\Store\Index.cshtml** ansichtsvorlagendatei wird erstellt und geöffnet.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-478">The **\Views\Store\Index.cshtml** View template file is created and opened.</span></span> <span data-ttu-id="7bd6e-479">Anhand der Informationen bereitgestellt, um die **Ansicht hinzufügen** Dialogfeld im letzten Schritt die Ansicht, die Vorlage werden erwarten, dass eine **StoreIndexViewModel** -Instanz wie die Daten zu verwenden, um eine HTML-Antwort zu generieren.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-479">Based on the information provided to the **Add View** dialog in the last step, the View template will expect a **StoreIndexViewModel** instance as the data to use to generate an HTML response.</span></span> <span data-ttu-id="7bd6e-480">Sie werden feststellen, dass die Vorlage übernimmt eine `ViewPage<musicstore.viewmodels.storeindexviewmodel>` in C# geschrieben.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-480">You will notice that the template inherits a `ViewPage<musicstore.viewmodels.storeindexviewmodel>` in C#.</span></span>

<a id="Ex5Task5"></a>

<a id="Task_5_-_Updating_the_View_Template"></a>
#### <a name="task-5---updating-the-view-template"></a><span data-ttu-id="7bd6e-481">Aufgabe 5: Aktualisieren der Vorlage anzeigen</span><span class="sxs-lookup"><span data-stu-id="7bd6e-481">Task 5 - Updating the View Template</span></span>

<span data-ttu-id="7bd6e-482">In dieser Aufgabe aktualisieren Sie die Vorlage anzeigen, die in der vorhergehenden Aufgabe zum Abrufen der Anzahl von Genres und deren Namen auf der Seite erstellt.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-482">In this task, you will update the View template created in the last task to retrieve the number of genres and their names within the page.</span></span>

> [!NOTE]
> <span data-ttu-id="7bd6e-483">Verwenden Sie @-Syntax (bezeichnet als &quot;Codeausdrücke&quot;) zum Ausführen von Code in der Vorlage anzeigen.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-483">You will use @ syntax (often referred to as &quot;code nuggets&quot;) to execute code within the View template.</span></span>

1. <span data-ttu-id="7bd6e-484">In der **"Index.cshtml"** -Datei, in der **Store** Ordner, ersetzen Sie den Code durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="7bd6e-484">In the **Index.cshtml** file, within the **Store** folder, replace its code with the following:</span></span>

[!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample14.cshtml)]

    > [!NOTE]
    > As soon as you finish typing the period after the word **Model**, Visual Studio's Intellisense will show a list of possible properties and methods to choose from.
    > 
    > ![](aspnet-mvc-4-fundamentals/_static/image25.png)
    > 
    > *Getting Model properties and methods with Visual Studio's IntelliSense*
    > 
    > The **Model** property references the **StoreIndexViewModel** object that the Controller passed to the View template. This means that you can access all of the data passed from the Controller to the View template via the **Model** property, and format it into an appropriate HTML response within the View template.
    > 
    > You can just select the **NumberOfGenres** property from the Intellisense list rather than typing it in and then it will auto-complete it by pressing the **tab key**.
2. <span data-ttu-id="7bd6e-485">Schleife über der Liste "Genre" in **StoreIndexViewModel** , und erstellen Sie eine HTML **&lt;Ul&gt;** auflisten mithilfe einer **Foreach** Schleife.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-485">Loop over the genre list in **StoreIndexViewModel** and create an HTML **&lt;ul&gt;** list using a **foreach** loop.</span></span>
   <span data-ttu-id="7bd6e-486">(C#)</span><span class="sxs-lookup"><span data-stu-id="7bd6e-486">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample15.cshtml)]
3. <span data-ttu-id="7bd6e-487">Drücken Sie **F5** , führen Sie die Anwendung, und navigieren Sie **/Store**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-487">Press **F5** to run the Application and browse **/Store**.</span></span> <span data-ttu-id="7bd6e-488">Sehen Sie die Liste der Genres übergeben die **StoreIndexViewModel** -Objekt aus der **StoreController** der Vorlage anzeigen.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-488">You will see the list of genres passed in the **StoreIndexViewModel** object from the **StoreController** to the View template.</span></span>

    <span data-ttu-id="7bd6e-489">![Ansicht zum Anzeigen einer Liste der Genres](aspnet-mvc-4-fundamentals/_static/image26.png "Ansicht zum Anzeigen einer Liste der Genres")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-489">![View displaying a list of genres](aspnet-mvc-4-fundamentals/_static/image26.png "View displaying a list of genres")</span></span>

    <span data-ttu-id="7bd6e-490">*Ansicht zum Anzeigen einer Liste der genres*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-490">*View displaying a list of genres*</span></span>
4. <span data-ttu-id="7bd6e-491">Schließen Sie den Browser.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-491">Close the browser.</span></span>

<a id="Exercise6"></a>

<a id="Exercise_6_Using_Parameters_in_View"></a>
### <a name="exercise-6-using-parameters-in-view"></a><span data-ttu-id="7bd6e-492">Übung 6: Verwenden von Parametern in der Ansicht</span><span class="sxs-lookup"><span data-stu-id="7bd6e-492">Exercise 6: Using Parameters in View</span></span>

<span data-ttu-id="7bd6e-493">In Übung 3 haben Sie gelernt, wie Parameter an den Controller übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-493">In Exercise 3 you learned how to pass parameters to the Controller.</span></span> <span data-ttu-id="7bd6e-494">In dieser Übung lernen Sie, wie Sie diese Parameter in die ansichtsvorlage zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-494">In this exercise, you will learn how to use those parameters in the View template.</span></span> <span data-ttu-id="7bd6e-495">Zu diesem Zweck werden Sie zuerst auf ViewModel-Klassen vorgestellt, mit denen Sie Ihre Daten und Domänenlogik zu verwalten.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-495">For that purpose, you will be introduced first to Model classes that will help you to manage your data and domain logic.</span></span> <span data-ttu-id="7bd6e-496">Darüber hinaus lernen Sie, wie Sie Links zu Seiten in ASP.NET MVC-Anwendung zu erstellen, ohne sich Gedanken machen Dinge wie URL-Pfade, die Codierung.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-496">Additionally, you will learn how to create links to pages inside the ASP.NET MVC application without worrying of things like URL paths encoding.</span></span>

<a id="Ex6Task1"></a>

<a id="Task_1_-_Adding_Model_Classes"></a>
#### <a name="task-1---adding-model-classes"></a><span data-ttu-id="7bd6e-497">Aufgabe 1: Hinzufügen von Modellklassen</span><span class="sxs-lookup"><span data-stu-id="7bd6e-497">Task 1 - Adding Model Classes</span></span>

<span data-ttu-id="7bd6e-498">Im Gegensatz zu ViewModels, die erstellt werden, um Informationen vom Controller an die Ansicht zu übergeben, werden ViewModel-Klassen erstellt, um enthalten und Verwalten von Daten und Domänenlogik.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-498">Unlike ViewModels, which are created just to pass information from the Controller to the View, Model classes are built to contain and manage data and domain logic.</span></span> <span data-ttu-id="7bd6e-499">In dieser Aufgabe fügen Sie zwei Modellklassen zur Darstellung dieser Konzepte: **"Genre"** und **Album**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-499">In this task you will add two model classes to represent these concepts: **Genre** and **Album**.</span></span>

1. <span data-ttu-id="7bd6e-500">Wenn nicht bereits geöffnet ist, starten Sie **Visual Studio Express für Web**</span><span class="sxs-lookup"><span data-stu-id="7bd6e-500">If not already open, start **VS Express for Web**</span></span>
2. <span data-ttu-id="7bd6e-501">In der **Datei** Menü wählen **geöffneten Projekt**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-501">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="7bd6e-502">Navigieren Sie in das Dialogfeld "Projekt öffnen" zum **Source\Ex06-UsingParametersInView\Begin**Option **Begin.sln** , und klicken Sie auf **öffnen**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-502">In the Open Project dialog, browse to **Source\Ex06-UsingParametersInView\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="7bd6e-503">Alternativ kann der Projektmappe fortgesetzt werden, dass Sie nach Abschluss der vorherigen Übung abgerufen.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-503">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="7bd6e-504">Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung, Sie müssen einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-504">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="7bd6e-505">Zu diesem Zweck klicken Sie auf die **Projekt** Menü **NuGet-Pakete verwalten**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-505">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="7bd6e-506">In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-506">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="7bd6e-507">Abschließend erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-507">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="7bd6e-508">Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken in Ihrem Projekt, Versand Verringern der Projektgröße.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-508">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="7bd6e-509">Mit NuGet Power Tools können werden durch Angabe von Versionen des Pakets in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, die, das Sie das Projekt ausführen, können.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-509">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="7bd6e-510">Deshalb müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit wird.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-510">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="7bd6e-511">Hinzufügen einer **"Genre"** Modellklasse.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-511">Add a **Genre** Model class.</span></span> <span data-ttu-id="7bd6e-512">Dazu, mit der Maustaste der **Modelle** Ordner in der **Projektmappen-Explorer**Option **hinzufügen** und klicken Sie dann die **neues Element** Option.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-512">To do this, right-click the **Models** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="7bd6e-513">Klicken Sie unter **Code**, wählen Sie die **Klasse** Element aus, und nennen Sie die Datei *Genre.cs*, klicken Sie dann auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-513">Under **Code**, choose the **Class** item and name the file *Genre.cs*, then click **Add**.</span></span>

    <span data-ttu-id="7bd6e-514">![Hinzufügen einer Klasse](aspnet-mvc-4-fundamentals/_static/image27.png "Hinzufügen einer Klasse")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-514">![Adding a class](aspnet-mvc-4-fundamentals/_static/image27.png "Adding a class")</span></span>

    <span data-ttu-id="7bd6e-515">*Hinzufügen eines neuen Elements*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-515">*Adding a new item*</span></span>

    <span data-ttu-id="7bd6e-516">![Hinzufügen der Modellklasse für "Genre"](aspnet-mvc-4-fundamentals/_static/image28.png "\"Genre\" Modellklasse hinzufügen")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-516">![Add Genre Model Class](aspnet-mvc-4-fundamentals/_static/image28.png "Add Genre Model Class")</span></span>

    <span data-ttu-id="7bd6e-517">*Hinzufügen von "Genre" Model-Klasse*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-517">*Add Genre Model Class*</span></span>
4. <span data-ttu-id="7bd6e-518">Hinzufügen einer **Namen** Eigenschaft der Klasse "Genre".</span><span class="sxs-lookup"><span data-stu-id="7bd6e-518">Add a **Name** property to the Genre class.</span></span> <span data-ttu-id="7bd6e-519">Zu diesem Zweck fügen Sie den folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="7bd6e-519">To do this, add the following code:</span></span>

    <span data-ttu-id="7bd6e-520">(Codeausschnitt - *Grundlagen von ASP.NET MVC 4 - Ex6 "Genre"*)</span><span class="sxs-lookup"><span data-stu-id="7bd6e-520">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 Genre*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample16.cs)]
5. <span data-ttu-id="7bd6e-521">Befolgen die gleiche Vorgehensweise wie zuvor Hinzufügen einer **Album** Klasse.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-521">Following the same procedure as before, add an **Album** class.</span></span> <span data-ttu-id="7bd6e-522">Dazu, mit der Maustaste der **Modelle** Ordner in der **Projektmappen-Explorer**Option **hinzufügen** und klicken Sie dann die **neues Element** Option.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-522">To do this, right-click the **Models** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="7bd6e-523">Klicken Sie unter **Code**, wählen Sie die **Klasse** Element aus, und nennen Sie die Datei *Album.cs*, klicken Sie dann auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-523">Under **Code**, choose the **Class** item and name the file *Album.cs*, then click **Add**.</span></span>
6. <span data-ttu-id="7bd6e-524">Fügen Sie zwei Eigenschaften zur Klasse Album: **"Genre"** und **Titel**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-524">Add two properties to the Album class: **Genre** and **Title**.</span></span> <span data-ttu-id="7bd6e-525">Zu diesem Zweck fügen Sie den folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="7bd6e-525">To do this, add the following code:</span></span>

    <span data-ttu-id="7bd6e-526">(Codeausschnitt - *Grundlagen von ASP.NET MVC 4 - Ex6 Album*)</span><span class="sxs-lookup"><span data-stu-id="7bd6e-526">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 Album*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample17.cs)]

<a id="Ex6Task2"></a>

<a id="Task_2_-_Adding_a_StoreBrowseViewModel"></a>
#### <a name="task-2---adding-a-storebrowseviewmodel"></a><span data-ttu-id="7bd6e-527">Aufgabe 2: Hinzufügen einer StoreBrowseViewModel</span><span class="sxs-lookup"><span data-stu-id="7bd6e-527">Task 2 - Adding a StoreBrowseViewModel</span></span>

<span data-ttu-id="7bd6e-528">Ein **StoreBrowseViewModel** wird in dieser Aufgabe verwendet werden, um Alben anzuzeigen, die eine ausgewählte Genre entsprechen.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-528">A **StoreBrowseViewModel** will be used in this task to show the Albums that match a selected Genre.</span></span> <span data-ttu-id="7bd6e-529">In dieser Aufgabe Sie diese Klasse erstellen und dann fügen Sie zwei Eigenschaften zum Behandeln der **"Genre"** und die zugehörige **Album**der Liste.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-529">In this task, you will create this class and then add two properties to handle the **Genre** and its **Album**'s List.</span></span>

1. <span data-ttu-id="7bd6e-530">Hinzufügen einer **StoreBrowseViewModel** Klasse.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-530">Add a **StoreBrowseViewModel** class.</span></span> <span data-ttu-id="7bd6e-531">Dazu, mit der Maustaste der **ViewModels** Ordner in der **Projektmappen-Explorer**Option **hinzufügen** und klicken Sie dann die **neues Element** Option.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-531">To do this, right-click the **ViewModels** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="7bd6e-532">Klicken Sie unter **Code**, wählen Sie die **Klasse** Element aus, und nennen Sie die Datei *StoreBrowseViewModel.cs*, klicken Sie dann auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-532">Under **Code**, choose the **Class** item and name the file *StoreBrowseViewModel.cs*, then click **Add**.</span></span>
2. <span data-ttu-id="7bd6e-533">Hinzufügen eines Verweises auf die Modelle in **StoreBrowseViewModel** Klasse.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-533">Add a reference to the Models in **StoreBrowseViewModel** class.</span></span> <span data-ttu-id="7bd6e-534">Zu diesem Zweck fügen Sie die folgenden Namespace verwenden:</span><span class="sxs-lookup"><span data-stu-id="7bd6e-534">To do this, add the following using namespace:</span></span>

    <span data-ttu-id="7bd6e-535">(Codeausschnitt - *Grundlagen von ASP.NET MVC 4 - Ex6 UsingModel*)</span><span class="sxs-lookup"><span data-stu-id="7bd6e-535">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 UsingModel*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample18.cs)]
3. <span data-ttu-id="7bd6e-536">Fügen Sie zwei Eigenschaften, **StoreBrowseViewModel** Klasse: **"Genre"** und **Alben**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-536">Add two properties to **StoreBrowseViewModel** class: **Genre** and **Albums**.</span></span> <span data-ttu-id="7bd6e-537">Zu diesem Zweck fügen Sie den folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="7bd6e-537">To do this, add the following code:</span></span>

    <span data-ttu-id="7bd6e-538">(Codeausschnitt - *Grundlagen von ASP.NET MVC 4 - Ex6 ModelProperties*)</span><span class="sxs-lookup"><span data-stu-id="7bd6e-538">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 ModelProperties*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample19.cs)]

> [!NOTE]
> <span data-ttu-id="7bd6e-539">Was ist **Liste&lt;Album&gt;**  ?: Diese Definition ist die Verwendung der **Liste&lt;T&gt;**  Typ, in denen **T** schränkt die Typ der Elemente dieser **Liste** gehören, in diesem Fall **Album** (oder eines seiner Nachfolger).</span><span class="sxs-lookup"><span data-stu-id="7bd6e-539">What is **List&lt;Album&gt;** ?: This definition is using the **List&lt;T&gt;** type, where **T** constrains the type to which elements of this **List** belong to, in this case **Album** (or any of its descendants).</span></span>
> 
> <span data-ttu-id="7bd6e-540">Diese Möglichkeit zum Entwerfen von Klassen und Methoden, die die Spezifikation des einen oder mehrere Typen verzögern, bis die Klasse oder Methode deklariert und instanziiert, indem der Client-Code ein Feature von c#-Sprache ist namens **Generika**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-540">This ability to design classes and methods that defer the specification of one or more types until the class or method is declared and instantiated by client code is a feature of the C# language called **Generics**.</span></span>
> 
> <span data-ttu-id="7bd6e-541">**Liste&lt;T&gt;**  ist die generische Entsprechung, der die **ArrayList** geben und steht in der **System.Collections.Generic** Namespace.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-541">**List&lt;T&gt;** is the generic equivalent of the **ArrayList** type and is available in the **System.Collections.Generic** namespace.</span></span> <span data-ttu-id="7bd6e-542">Einer der Vorteile der Verwendung von **Generika** , da der Typ angegeben ist, müssen Sie keine typüberprüfung für Vorgänge wie das Umwandeln der Elemente in kümmern **Album** wie mit einer **ArrayList**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-542">One of the benefits of using **generics** is that since the type is specified, you do not need to take care of type checking operations such as casting the elements into **Album** as you would do with an **ArrayList**.</span></span>

<a id="Ex6Task3"></a>

<a id="Task_3_-_Using_the_New_ViewModel_in_the_StoreController"></a>
#### <a name="task-3---using-the-new-viewmodel-in-the-storecontroller"></a><span data-ttu-id="7bd6e-543">Aufgabe 3: verwenden das neue "ViewModel" in der StoreController</span><span class="sxs-lookup"><span data-stu-id="7bd6e-543">Task 3 - Using the New ViewModel in the StoreController</span></span>

<span data-ttu-id="7bd6e-544">In dieser Aufgabe ändern Sie die **StoreController**des **Durchsuchen** und **Details** Aktionsmethoden anwenden, um die Verwendung der neuen **StoreBrowseViewModel** .</span><span class="sxs-lookup"><span data-stu-id="7bd6e-544">In this task, you will modify the **StoreController**'s **Browse** and **Details** action methods to use the new **StoreBrowseViewModel**.</span></span>

1. <span data-ttu-id="7bd6e-545">Hinzufügen eines Verweises auf den Ordner "Models" im **StoreController** Klasse.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-545">Add a reference to the Models folder in **StoreController** class.</span></span> <span data-ttu-id="7bd6e-546">Zu diesem Zweck, erweitern Sie die **Controller** Ordner in der **Projektmappen-Explorer** , und öffnen Sie die **StoreController** Klasse.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-546">To do this, expand the **Controllers** folder in the **Solution Explorer** and open the **StoreController** class.</span></span> <span data-ttu-id="7bd6e-547">Fügen Sie dann den folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="7bd6e-547">Then add the following code:</span></span>

    <span data-ttu-id="7bd6e-548">(Codeausschnitt - *Grundlagen von ASP.NET MVC 4 - Ex6 UsingModelInController*)</span><span class="sxs-lookup"><span data-stu-id="7bd6e-548">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 UsingModelInController*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample20.cs)]
2. <span data-ttu-id="7bd6e-549">Ersetzen Sie die **Durchsuchen** Aktionsmethode verwendet die **StoreViewBrowseController** Klasse.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-549">Replace the **Browse** action method to use the **StoreViewBrowseController** class.</span></span> <span data-ttu-id="7bd6e-550">Erstellen Sie ein Genre und zwei neue Alben Objekte mit unechten Daten (in der nächsten praktische Übungseinheit verwenden Sie echte Daten von einer Datenbank).</span><span class="sxs-lookup"><span data-stu-id="7bd6e-550">You will create a Genre and two new Albums objects with dummy data (in the next Hands-on Lab you will consume real data from a database).</span></span> <span data-ttu-id="7bd6e-551">Ersetzen Sie hierzu die **Durchsuchen** Methode durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="7bd6e-551">To do this, replace the **Browse** method with the following code:</span></span>

    <span data-ttu-id="7bd6e-552">(Codeausschnitt - *Grundlagen von ASP.NET MVC 4 - Ex6 BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="7bd6e-552">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 BrowseMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample21.cs)]
3. <span data-ttu-id="7bd6e-553">Ersetzen Sie die **Details** Aktionsmethode verwendet die **StoreViewBrowseController** Klasse.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-553">Replace the **Details** action method to use the **StoreViewBrowseController** class.</span></span> <span data-ttu-id="7bd6e-554">Erstellen Sie ein neues **Album** zu zurückzugebenden Objekts der **Ansicht**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-554">You will create a new **Album** object to be returned to the **View**.</span></span> <span data-ttu-id="7bd6e-555">Ersetzen Sie hierzu die **Details** Methode durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="7bd6e-555">To do this, replace the **Details** method with the following code:</span></span>

    <span data-ttu-id="7bd6e-556">(Codeausschnitt - *Grundlagen von ASP.NET MVC 4 - Ex6 DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="7bd6e-556">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 DetailsMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample22.cs)]

<a id="Ex6Task4"></a>

<a id="Task_4_-_Adding_a_Browse_View_Template"></a>
#### <a name="task-4---adding-a-browse-view-template"></a><span data-ttu-id="7bd6e-557">Aufgabe 4: Hinzufügen eines durchsuchen-Vorlage anzeigen</span><span class="sxs-lookup"><span data-stu-id="7bd6e-557">Task 4 - Adding a Browse View Template</span></span>

<span data-ttu-id="7bd6e-558">In dieser Aufgabe fügen Sie eine **Durchsuchen** zum Anzeigen der Alben für eine spezifische Genre gefunden.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-558">In this task, you will add a **Browse** View to show the Albums found for a specific Genre.</span></span>

1. <span data-ttu-id="7bd6e-559">Vor dem Erstellen der neuen Vorlage anzeigen, sollten Sie das Projekt erstellen, damit die **Ansicht hinzufügen** Dialogfeld kennt die **"ViewModel"** zu verwendende Klasse an.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-559">Before creating the new View template, you should build the project so that the **Add View** Dialog knows about the **ViewModel** class to use.</span></span> <span data-ttu-id="7bd6e-560">Erstellen Sie das Projekt durch Auswählen der **erstellen** Menüelement und dann **erstellen MvcMusicStore**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-560">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>
2. <span data-ttu-id="7bd6e-561">Hinzufügen einer **Durchsuchen** anzeigen.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-561">Add a **Browse** View.</span></span> <span data-ttu-id="7bd6e-562">Zu diesem Zweck mit der Maustaste in den **Durchsuchen** Action-Methode der der **StoreController** , und klicken Sie auf **Ansicht hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-562">To do this, right-click in the **Browse** action method of the **StoreController** and click **Add View**.</span></span>
3. <span data-ttu-id="7bd6e-563">In der **Ansicht hinzufügen** Dialogfeld Vergewissern Sie sich, dass der Ansichtsname ist **Durchsuchen**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-563">In the **Add View** dialog box, verify that the View Name is **Browse**.</span></span> <span data-ttu-id="7bd6e-564">Überprüfen Sie die **eine stark typisierte Ansicht erstellen** Kontrollkästchen, und wählen Sie **StoreBrowseViewModel** aus der **Modellklasse** Dropdownliste.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-564">Check the **Create a strongly-typed view** checkbox and select **StoreBrowseViewModel** from the **Model class** dropdown.</span></span> <span data-ttu-id="7bd6e-565">Lassen Sie die anderen Felder auch die Standardwerte.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-565">Leave the other fields with their default value.</span></span> <span data-ttu-id="7bd6e-566">Klicken Sie anschließend auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-566">Then click **Add**.</span></span>

    <span data-ttu-id="7bd6e-567">![Hinzufügen einer Ansicht durchsuchen](aspnet-mvc-4-fundamentals/_static/image29.png "Hinzufügen einer Ansicht durchsuchen")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-567">![Adding a Browse View](aspnet-mvc-4-fundamentals/_static/image29.png "Adding a Browse View")</span></span>

    <span data-ttu-id="7bd6e-568">*Hinzufügen einer Ansicht durchsuchen*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-568">*Adding a Browse View*</span></span>
4. <span data-ttu-id="7bd6e-569">Ändern der **Browse.cshtml** zur Anzeige von Informationen für die "Genre", den Zugriff auf die **StoreBrowseViewModel** -Objekt, das die ansichtsvorlage übergeben wird.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-569">Modify the **Browse.cshtml** to display the Genre's information, accessing the **StoreBrowseViewModel** object that is passed to the view template.</span></span> <span data-ttu-id="7bd6e-570">Zu diesem Zweck ersetzen Sie den Inhalt durch Folgendes: (c#)</span><span class="sxs-lookup"><span data-stu-id="7bd6e-570">To do this, replace the content with the following: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample23.cshtml)]

<a id="Ex6Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="7bd6e-571">Aufgabe 5: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="7bd6e-571">Task 5 - Running the Application</span></span>

<span data-ttu-id="7bd6e-572">In dieser Aufgabe testen Sie, die die **Durchsuchen** Methode ruft Alben aus der **Durchsuchen** Methodenaktion.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-572">In this task, you will test that the **Browse** method retrieves Albums from the **Browse** method action.</span></span>

1. <span data-ttu-id="7bd6e-573">Drücken Sie **F5** zum Ausführen der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-573">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="7bd6e-574">Das Projekt, die auf der Startseite wird gestartet.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-574">The project starts in the Home page.</span></span> <span data-ttu-id="7bd6e-575">Ändern Sie die URL zum   **/Store/durchsuchen? Genre = Disco** zu überprüfen, ob die Aktion zwei Alben zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-575">Change the URL to **/Store/Browse?Genre=Disco** to verify that the action returns two Albums.</span></span>

    <span data-ttu-id="7bd6e-576">![Durchsuchen Store Disco Alben](aspnet-mvc-4-fundamentals/_static/image30.png "Disco Alben Store durchsuchen")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-576">![Browsing Store Disco Albums](aspnet-mvc-4-fundamentals/_static/image30.png "Browsing Store Disco Albums")</span></span>

    <span data-ttu-id="7bd6e-577">*Disco Alben Store durchsuchen*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-577">*Browsing Store Disco Albums*</span></span>

<a id="Ex6Task6"></a>

<a id="Task_6_-_Displaying_information_About_a_Specific_Album"></a>
#### <a name="task-6---displaying-information-about-a-specific-album"></a><span data-ttu-id="7bd6e-578">Aufgabe 6: Anzeigen von Informationen über ein bestimmtes Album</span><span class="sxs-lookup"><span data-stu-id="7bd6e-578">Task 6 - Displaying information About a Specific Album</span></span>

<span data-ttu-id="7bd6e-579">In dieser Aufgabe implementieren Sie die **Store/Details** können Sie Informationen über ein bestimmtes Album anzeigen.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-579">In this task, you will implement the **Store/Details** view to display information about a specific album.</span></span> <span data-ttu-id="7bd6e-580">In dieser praktischen Übungseinheit alles, was Sie über das Album zeigt ist bereits Bestandteil der **Ansicht** Vorlage.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-580">In this Hands-on Lab, everything you will display about the album is already contained in the **View** template.</span></span> <span data-ttu-id="7bd6e-581">In diesem Fall statt einer **StoreDetailsViewModel** -Klasse, verwenden Sie die aktuelle **StoreBrowseViewModel** Vorlage, die das Album an sie übergibt.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-581">So, instead of creating a **StoreDetailsViewModel** class, you will use the current **StoreBrowseViewModel** template passing the Album to it.</span></span>

1. <span data-ttu-id="7bd6e-582">Schließen Sie den Browser, die ggf. zu Visual Studio-Fenster zurückzukehren.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-582">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="7bd6e-583">Fügen Sie einen neuen **Details** anzeigen für die **StoreController**des **Details** Aktionsmethode.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-583">Add a new **Details** view for the **StoreController**'s **Details** action method.</span></span> <span data-ttu-id="7bd6e-584">Dazu, mit der Maustaste der **Details** -Methode in der die **StoreController** Klasse, und klicken Sie auf **Ansicht hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-584">To do this, right-click the **Details** method in the **StoreController** class and click **Add View**.</span></span>
2. <span data-ttu-id="7bd6e-585">In der **Ansicht hinzufügen** Dialogfeld, überprüfen Sie, ob die **Sichtname** ist **Details**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-585">In the **Add View** dialog, verify that the **View Name** is **Details**.</span></span> <span data-ttu-id="7bd6e-586">Überprüfen Sie die **eine stark typisierte Ansicht erstellen,** Kontrollkästchen, und wählen Sie **Album** aus der **Modellklasse** Dropdown-Liste.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-586">Check the **Create a strongly-typed view** checkbox and select **Album** from the **Model class** drop-down.</span></span> <span data-ttu-id="7bd6e-587">Lassen Sie die anderen Felder auch die Standardwerte.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-587">Leave the other fields with their default value.</span></span> <span data-ttu-id="7bd6e-588">Klicken Sie anschließend auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-588">Then click **Add**.</span></span> <span data-ttu-id="7bd6e-589">Wird erstellt, und öffnen Sie eine **\Views\Store\Details.cshtml** Datei.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-589">This will create and open a **\Views\Store\Details.cshtml** file.</span></span>

    <span data-ttu-id="7bd6e-590">![Hinzufügen einer Detailansicht](aspnet-mvc-4-fundamentals/_static/image31.png "Hinzufügen einer Detailansicht")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-590">![Adding a Details View](aspnet-mvc-4-fundamentals/_static/image31.png "Adding a Details View")</span></span>

    <span data-ttu-id="7bd6e-591">*Hinzufügen einer Detailansicht*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-591">*Adding a Details View*</span></span>
3. <span data-ttu-id="7bd6e-592">Ändern der **Details.cshtml** Datei zur Anzeige von Informationen des Albums, den Zugriff auf die **Album** -Objekt, das die ansichtsvorlage übergeben wird.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-592">Modify the **Details.cshtml** file to display the Album's information, accessing the **Album** object that is passed to the view template.</span></span> <span data-ttu-id="7bd6e-593">Zu diesem Zweck ersetzen Sie den Inhalt durch Folgendes: (c#)</span><span class="sxs-lookup"><span data-stu-id="7bd6e-593">To do this, replace the content with the following: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample24.cshtml)]

<a id="Ex6Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a><span data-ttu-id="7bd6e-594">Aufgabe 7: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="7bd6e-594">Task 7 - Running the Application</span></span>

<span data-ttu-id="7bd6e-595">In dieser Aufgabe testen Sie, die die **Details** Ansicht abruft Albuminformationen aus der **Details Aktion** Methode.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-595">In this task, you will test that the **Details** View retrieves Album's information from the **Details action** method.</span></span>

1. <span data-ttu-id="7bd6e-596">Drücken Sie **F5** zum Ausführen der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-596">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="7bd6e-597">Das Projekt startet der **Startseite** Seite.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-597">The project starts in the **Home** page.</span></span> <span data-ttu-id="7bd6e-598">Ändern Sie die URL zum **/Store/Details/5** des Albums Informationen überprüfen.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-598">Change the URL to **/Store/Details/5** to verify the album's information.</span></span>

    <span data-ttu-id="7bd6e-599">![Durchsuchen Alben Detail](aspnet-mvc-4-fundamentals/_static/image32.png "Alben Detail durchsuchen")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-599">![Browsing Albums Detail](aspnet-mvc-4-fundamentals/_static/image32.png "Browsing Albums Detail")</span></span>

    <span data-ttu-id="7bd6e-600">*Durchsuchen des Albums-Details*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-600">*Browsing Album's Detail*</span></span>

<a id="Ex6Task8"></a>

<a id="Task_8_-_Adding_Links_Between_Pages"></a>
#### <a name="task-8---adding-links-between-pages"></a><span data-ttu-id="7bd6e-601">Aufgabe 8: Hinzufügen von Verknüpfungen zwischen Seiten</span><span class="sxs-lookup"><span data-stu-id="7bd6e-601">Task 8 - Adding Links Between Pages</span></span>

<span data-ttu-id="7bd6e-602">In dieser Aufgabe fügen Sie einen Link in der Ansicht der Store über einen Link in die Namen aller "Genre" in den entsprechenden **/Store/durchsuchen** URL.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-602">In this task, you will add a link in the Store View to have a link in every Genre name to the appropriate **/Store/Browse** URL.</span></span> <span data-ttu-id="7bd6e-603">Auf diese Weise beim Klicken auf ein Genre, z. B. **Disco**, navigiert er zur **/Store/durchsuchen? Genre = Disco** URL.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-603">This way, when you click on a Genre, for instance **Disco**, it will navigate to **/Store/Browse?genre=Disco** URL.</span></span>

1. <span data-ttu-id="7bd6e-604">Schließen Sie den Browser, die ggf. zu Visual Studio-Fenster zurückzukehren.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-604">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="7bd6e-605">Update der **Index** einen Link zum Hinzufügen der **Durchsuchen** Seite.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-605">Update the **Index** page to add a link to the **Browse** page.</span></span> <span data-ttu-id="7bd6e-606">Klicken Sie hierzu in der **Projektmappen-Explorer** erweitern Sie die **Ansichten** Ordner die **Store** Ordner und doppelklicken Sie auf die **"Index.cshtml"** Seite.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-606">To do this, in the **Solution Explorer** expand the **Views** folder, then the **Store** folder and double-click the **Index.cshtml** page.</span></span>
2. <span data-ttu-id="7bd6e-607">Die Ansicht durchsuchen, der angibt, des Genres ausgewählt haben, wird keine Verknüpfung hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-607">Add a link to the Browse view indicating the genre selected.</span></span> <span data-ttu-id="7bd6e-608">Ersetzen Sie hierzu den folgenden hervorgehobenen Code innerhalb der **&lt;li&gt;** Tags: (c#)</span><span class="sxs-lookup"><span data-stu-id="7bd6e-608">To do this, replace the following highlighted code within the **&lt;li&gt;** tags: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample25.cshtml)]

   > [!NOTE]
   > <span data-ttu-id="7bd6e-609">ein anderer Ansatz würde direkt auf der Seite mit einem Code wie folgt verknüpft werden:</span><span class="sxs-lookup"><span data-stu-id="7bd6e-609">another approach would be linking directly to the page, with a code like the following:</span></span>
   > 
   > <span data-ttu-id="7bd6e-610">&lt;ein Href =&quot;/Store/durchsuchen? Genre =@genreName&quot;&gt;@genreName &lt; /a&gt;</span><span class="sxs-lookup"><span data-stu-id="7bd6e-610">&lt;a href=&quot;/Store/Browse?genre=@genreName&quot;&gt;@genreName&lt;/a&gt;</span></span>
   > 
   > <span data-ttu-id="7bd6e-611">Obwohl dieser Ansatz funktioniert, hängt es eine hartcodierte Zeichenfolge ein.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-611">Although this approach works, it depends on a hardcoded string.</span></span> <span data-ttu-id="7bd6e-612">Wenn Sie später den Controller umbenennen, müssen Sie diese Anweisung manuell ändern.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-612">If you later rename the Controller, you will have to change this instruction manually.</span></span> <span data-ttu-id="7bd6e-613">Eine bessere Alternative ist die Verwendung einer **HTML-Hilfsobjekt** Methode.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-613">A better alternative is to use an **HTML Helper** method.</span></span> <span data-ttu-id="7bd6e-614">ASP.NET MVC umfasst eine HTML-Hilfsobjekt-Methode, die für die Ausführung von Aufgaben verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-614">ASP.NET MVC includes an HTML Helper method which is available for such tasks.</span></span> <span data-ttu-id="7bd6e-615">Die **Html.ActionLink()** Hilfsmethode erleichtert Ihnen die Erstellung von HTML **&lt;eine&gt;** Links, sodass Sie sicher, dass die URL-Pfade sind ordnungsgemäß URL-codiert.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-615">The **Html.ActionLink()** helper method makes it easy to build HTML **&lt;a&gt;** links, making sure URL paths are properly URL encoded.</span></span>
   > 
   > <span data-ttu-id="7bd6e-616">Htlm.ActionLink weist mehrere Überladungen.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-616">Htlm.ActionLink has several overloads.</span></span> <span data-ttu-id="7bd6e-617">In dieser Übung werden Sie eine verwenden, die drei Parameter akzeptiert:</span><span class="sxs-lookup"><span data-stu-id="7bd6e-617">In this exercise you will use one that takes three parameters:</span></span>
   > 
   > 1. <span data-ttu-id="7bd6e-618">Text des Links, der den Namen "Genre" angezeigt wird</span><span class="sxs-lookup"><span data-stu-id="7bd6e-618">Link text, which will display the Genre name</span></span>
   > 2. <span data-ttu-id="7bd6e-619">Name des Domänencontrollers Aktion (**Durchsuchen**)</span><span class="sxs-lookup"><span data-stu-id="7bd6e-619">Controller action name (**Browse**)</span></span>
   > 3. <span data-ttu-id="7bd6e-620">Weiterleiten von Parameterwerten, die sowohl den Namen angeben (**"Genre"**) und der Wert (**Name des Genres**)</span><span class="sxs-lookup"><span data-stu-id="7bd6e-620">Route parameter values, specifying both the name (**Genre**) and the value (**Genre name**)</span></span>

<a id="Ex6Task9"></a>

<a id="Task_9_-_Running_the_Application"></a>
#### <a name="task-9---running-the-application"></a><span data-ttu-id="7bd6e-621">Aufgabe 9: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="7bd6e-621">Task 9 - Running the Application</span></span>

<span data-ttu-id="7bd6e-622">In dieser Aufgabe testen Sie, dass jedes Genre, mit einem Link zu den entsprechenden angezeigt wird **/Store/durchsuchen** URL.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-622">In this task, you will test that each Genre is displayed with a link to the appropriate **/Store/Browse** URL.</span></span>

1. <span data-ttu-id="7bd6e-623">Drücken Sie **F5** zum Ausführen der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-623">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="7bd6e-624">Das Projekt, die auf der Startseite wird gestartet.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-624">The project starts in the Home page.</span></span> <span data-ttu-id="7bd6e-625">Ändern Sie die URL zum **/Store** um sicherzustellen, dass jedes Genres an den entsprechenden links **/Store/durchsuchen** URL.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-625">Change the URL to **/Store** to verify that each Genre links to the appropriate **/Store/Browse** URL.</span></span>

    <span data-ttu-id="7bd6e-626">![Durchsuchen von Genres mit Links zu der Seite "Durchsuchen"](aspnet-mvc-4-fundamentals/_static/image33.png "Genres durchsuchen, mit Links zu der Seite \"Durchsuchen\"")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-626">![Browsing Genres with links to Browse page](aspnet-mvc-4-fundamentals/_static/image33.png "Browsing Genres with links to Browse page")</span></span>

    <span data-ttu-id="7bd6e-627">*Durchsuchen von Genres mit Links zu der Seite "Durchsuchen"*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-627">*Browsing Genres with links to Browse page*</span></span>

<a id="Ex6Task10"></a>

<a id="Task_10_-_Using_Dynamic_ViewModel_Collection_to_Pass_Values"></a>
#### <a name="task-10---using-dynamic-viewmodel-collection-to-pass-values"></a><span data-ttu-id="7bd6e-628">Aufgabe 10 – mit dynamischen ViewModel-Auflistung zum Übergeben der Werte</span><span class="sxs-lookup"><span data-stu-id="7bd6e-628">Task 10 - Using Dynamic ViewModel Collection to Pass Values</span></span>

<span data-ttu-id="7bd6e-629">In dieser Aufgabe lernen Sie, eine einfache und leistungsstarke Methode zum Übergeben von Werten zwischen dem Controller und die Ansicht, ohne dass Änderungen im Modell.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-629">In this task, you will learn a simple and powerful method to pass values between the Controller and the View without making any changes in the Model.</span></span> <span data-ttu-id="7bd6e-630">ASP.NET MVC 4 stellt die Auflistung &quot;"ViewModel"&quot;, die auf einen dynamischen Wert zugewiesen und innerhalb von Controllern und Ansichten ebenfalls aufgerufen werden können.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-630">ASP.NET MVC 4 provides the collection &quot;ViewModel&quot;, which can be assigned to any dynamic value and accessed inside controllers and views as well.</span></span>

<span data-ttu-id="7bd6e-631">Sie verwenden nun die dynamische ViewBag-Auflistung, übergeben eine Liste der &quot; **Sterne erhalten Genres** &quot; vom Controller an die Ansicht.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-631">You will now use the ViewBag dynamic collection to pass a list of &quot;**Starred genres**&quot; from the controller to the view.</span></span> <span data-ttu-id="7bd6e-632">Die Store Indexansicht wird Zugriff auf **"ViewModel"** und die Informationen anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-632">The Store Index view will access to **ViewModel** and display the information.</span></span>

1. <span data-ttu-id="7bd6e-633">Schließen Sie den Browser, die ggf. zu Visual Studio-Fenster zurückzukehren.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-633">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="7bd6e-634">Open **StoreController.cs** und Ändern von **Index** Methode zum Erstellen einer Liste der Sterne Genres in ViewModel-Auflistung erhalten:</span><span class="sxs-lookup"><span data-stu-id="7bd6e-634">Open **StoreController.cs** and modify **Index** method to create a list of starred genres into ViewModel collection :</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample26.cs)]

    > [!NOTE]
    > <span data-ttu-id="7bd6e-635">Außerdem können Sie die Syntax **"ViewBag" [&quot;Starred&quot;]** auf die Eigenschaften zugreifen.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-635">You could also use the syntax **ViewBag[&quot;Starred&quot;]** to access the properties.</span></span>
2. <span data-ttu-id="7bd6e-636">Das Sternsymbol **&quot;starred.png&quot;** befindet sich auf die **Source\Assets\Images** Ordner dieser Anleitung.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-636">The star icon **&quot;starred.png&quot;** is included in the **Source\Assets\Images** folder of this lab.</span></span> <span data-ttu-id="7bd6e-637">Um sie an die Anwendung hinzuzufügen, ziehen Sie in ihrer Inhalte mit einem **Windows Explorer** Einblick in die **Projektmappen-Explorer** in Visual Web Developer Express, wie unten dargestellt:</span><span class="sxs-lookup"><span data-stu-id="7bd6e-637">In order to add it to the application, drag their content from a **Windows Explorer** window into the **Solution Explorer** in Visual Web Developer Express, as shown below:</span></span>

    <span data-ttu-id="7bd6e-638">![Stern Image hinzufügen, mit der Lösung](aspnet-mvc-4-fundamentals/_static/image34.png "Stern Image hinzufügen, mit der Lösung")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-638">![Adding star image to the solution](aspnet-mvc-4-fundamentals/_static/image34.png "Adding star image to the solution")</span></span>

    <span data-ttu-id="7bd6e-639">*Stern Image Hinzufügen zur Projektmappe*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-639">*Adding star image to the solution*</span></span>
3. <span data-ttu-id="7bd6e-640">Öffnen Sie die Ansicht **Store/Index.cshtml** und ändern Sie den Inhalt.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-640">Open the view **Store/Index.cshtml** and modify the content.</span></span> <span data-ttu-id="7bd6e-641">Sie liest die &quot;Sterne erhalten&quot; -Eigenschaft in der **"ViewBag"** -Auflistung, und bitten Sie den Namen des aktuellen "Genre" in der Liste ist.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-641">You will read the &quot;starred&quot; property in the **ViewBag** collection, and ask if the current genre name is in the list.</span></span> <span data-ttu-id="7bd6e-642">In diesem Fall werden Sie einem Sternsymbol direkt auf den Link "Genre" angezeigt.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-642">In that case you will show a star icon right to the genre link.</span></span>
   <span data-ttu-id="7bd6e-643">(C#)</span><span class="sxs-lookup"><span data-stu-id="7bd6e-643">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample27.cshtml)]

<a id="Ex6Task11"></a>

<a id="Task_11_-_Running_the_Application"></a>
#### <a name="task-11---running-the-application"></a><span data-ttu-id="7bd6e-644">Aufgabe 11: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="7bd6e-644">Task 11 - Running the Application</span></span>

<span data-ttu-id="7bd6e-645">In dieser Aufgabe testen Sie, dass die mit Sternen versehenen Genres einem Sternsymbol angezeigt.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-645">In this task, you will test that the starred genres display a star icon.</span></span>

1. <span data-ttu-id="7bd6e-646">Drücken Sie **F5** zum Ausführen der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-646">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="7bd6e-647">Das Projekt startet der **Startseite** Seite.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-647">The project starts in the **Home** page.</span></span> <span data-ttu-id="7bd6e-648">Ändern Sie die URL zum **/Store** um sicherzustellen, dass jedes ausgewählte Genre die respektieren Bezeichnung aufweist:</span><span class="sxs-lookup"><span data-stu-id="7bd6e-648">Change the URL to **/Store** to verify that each featured genre has the respecting label:</span></span>

    <span data-ttu-id="7bd6e-649">![Durchsuchen von Genres mit Sternen versehenen Elemente](aspnet-mvc-4-fundamentals/_static/image35.png "Genres mit Sternen versehenen Elemente durchsuchen")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-649">![Browsing Genres with starred elements](aspnet-mvc-4-fundamentals/_static/image35.png "Browsing Genres with starred elements")</span></span>

    <span data-ttu-id="7bd6e-650">*Durchsuchen von Genres mit Sternen versehenen Elemente*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-650">*Browsing Genres with starred elements*</span></span>

<a id="Exercise7"></a>

<a id="Exercise_7_A_lap_around_ASPNET_MVC_4_new_template"></a>
### <a name="exercise-7-a-lap-around-aspnet-mvc-4-new-template"></a><span data-ttu-id="7bd6e-651">Übung 7: Eine Tour durch neue ASP.NET MVC 4-Vorlage</span><span class="sxs-lookup"><span data-stu-id="7bd6e-651">Exercise 7: A lap around ASP.NET MVC 4 new template</span></span>

<span data-ttu-id="7bd6e-652">In dieser Übung untersuchen Sie die Verbesserungen in den ASP.NET MVC 4-Projektvorlagen, Blick auf die wichtigsten Features der neuen Vorlage.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-652">In this exercise, you will explore the enhancements in the ASP.NET MVC 4 project templates, taking a look at the most relevant features of the new template.</span></span>

<a id="Ex7Task1"></a>

<a id="Task_1_Exploring_the_ASPNET_MVC_4_Internet_Application_Template"></a>
#### <a name="task-1-exploring-the-aspnet-mvc-4-internet-application-template"></a><span data-ttu-id="7bd6e-653">Aufgabe 1: Untersuchen der Internetanwendungsvorlage in ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="7bd6e-653">Task 1: Exploring the ASP.NET MVC 4 Internet Application Template</span></span>

1. <span data-ttu-id="7bd6e-654">Wenn nicht bereits geöffnet ist, starten Sie **Visual Studio Express für Web**</span><span class="sxs-lookup"><span data-stu-id="7bd6e-654">If not already open, start **VS Express for Web**</span></span>
2. <span data-ttu-id="7bd6e-655">Wählen Sie die **Datei | Neue | Projekt** Menübefehl.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-655">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="7bd6e-656">In der **neues Projekt** wählen Sie im Dialogfeld die **Visual c# | Web** Vorlage auf der linken Struktur, und wählen Sie die **ASP.NET MVC 4-Webanwendung**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-656">In the **New Project** dialog, select the **Visual C#|Web** template on the left pane tree, and choose the **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="7bd6e-657">**Namen** des Projekts *Music Store* und aktualisieren Sie die **Projektmappenname** zu *beginnen*, und wählen Sie einen Standort (oder übernehmen Sie den Standardwert), und klicken Sie auf **OK** .</span><span class="sxs-lookup"><span data-stu-id="7bd6e-657">**Name** the project *MusicStore* and update the **solution name** to *Begin*, then select a location (or leave the default) and click **OK**.</span></span>

    <span data-ttu-id="7bd6e-658">![Erstellen ein neues ASP.NET MVC 4-Projekt](aspnet-mvc-4-fundamentals/_static/image36.png "erstellen ein neues ASP.NET MVC 4-Projekt")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-658">![Creating a new ASP.NET MVC 4 Project](aspnet-mvc-4-fundamentals/_static/image36.png "Creating a new ASP.NET MVC 4 Project")</span></span>

    <span data-ttu-id="7bd6e-659">*Erstellen ein neues ASP.NET MVC 4-Projekt*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-659">*Creating a new ASP.NET MVC 4 Project*</span></span>
3. <span data-ttu-id="7bd6e-660">In der **neues ASP.NET MVC 4-Projekt** wählen Sie im Dialogfeld die **Internetanwendung** Projektvorlage aus, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-660">In the **New ASP.NET MVC 4 Project** dialog, select the **Internet Application** project template and click **OK**.</span></span> <span data-ttu-id="7bd6e-661">Beachten Sie, dass Sie Razor oder ASPX als Ansichtsmodul auswählen können.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-661">Notice you can select either Razor or ASPX as the view engine.</span></span>

    <span data-ttu-id="7bd6e-662">![Erstellen einer neuen ASP.NET MVC 4 Internetanwendung](aspnet-mvc-4-fundamentals/_static/image37.png "Erstellen einer neuen ASP.NET MVC 4 Internet-Anwendung")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-662">![Creating a new ASP.NET MVC 4 Internet Application](aspnet-mvc-4-fundamentals/_static/image37.png "Creating a new ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="7bd6e-663">*Erstellen einer neuen ASP.NET MVC 4 Internet-Anwendung*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-663">*Creating a new ASP.NET MVC 4 Internet Application*</span></span>

    > [!NOTE]
    > <span data-ttu-id="7bd6e-664">Razor-Syntax wurde in ASP.NET MVC 3 eingeführt wurde.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-664">Razor syntax has been introduced in ASP.NET MVC 3.</span></span> <span data-ttu-id="7bd6e-665">Das Ziel ist, die die Anzahl von Zeichen und Tastaturanschläge erforderlich sind in einer Datei, und Aktivieren einer schnell und fließend, die Codierung von Workflows zu minimieren.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-665">Its goal is to minimize the number of characters and keystrokes required in a file, enabling a fast and fluid coding workflow.</span></span> <span data-ttu-id="7bd6e-666">Razor nutzt die vorhandenen C#/VB (oder andere) Fähigkeiten und bietet eine Vorlage Markupsyntax, die einen tollen HTML-Konstruktion Workflow ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-666">Razor leverages existing C#/VB (or other) language skills and delivers a template markup syntax that enables an awesome HTML construction workflow.</span></span>
4. <span data-ttu-id="7bd6e-667">Drücken Sie **F5** führen Sie die Projektmappe, und finden in der Vorlage erneuerten.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-667">Press **F5** to run the solution and see the renewed template.</span></span> <span data-ttu-id="7bd6e-668">Sie können Sie die folgenden Features überprüfen:</span><span class="sxs-lookup"><span data-stu-id="7bd6e-668">You can check out the following features:</span></span>

    1. <span data-ttu-id="7bd6e-669">**Modernes-Vorlagen**</span><span class="sxs-lookup"><span data-stu-id="7bd6e-669">**Modern-style templates**</span></span>

        <span data-ttu-id="7bd6e-670">Die Vorlagen haben erneuert wurde, weitere Modern aussehende Formate bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-670">The templates have been renewed, providing more modern-looking styles.</span></span>

        <span data-ttu-id="7bd6e-671">![Vorlagen für ASP.NET MVC 4 neu gestaltet](aspnet-mvc-4-fundamentals/_static/image38.png "neu gestaltet Vorlagen von ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-671">![ASP.NET MVC 4 restyled templates](aspnet-mvc-4-fundamentals/_static/image38.png "ASP.NET MVC 4 restyled templates")</span></span>

        <span data-ttu-id="7bd6e-672">*Vorlagen für ASP.NET MVC 4 neu gestaltet*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-672">*ASP.NET MVC 4 restyled templates*</span></span>
    2. <span data-ttu-id="7bd6e-673">**Adaptive Rendering**</span><span class="sxs-lookup"><span data-stu-id="7bd6e-673">**Adaptive Rendering**</span></span>

        <span data-ttu-id="7bd6e-674">Sehen Sie sich die Größe des Browserfensters, und beachten Sie, wie das Seitenlayout passen diese dynamisch auf die neue Fenstergröße an.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-674">Check out resizing the browser window and notice how the page layout dynamically adapts to the new window size.</span></span> <span data-ttu-id="7bd6e-675">Diese Vorlagen mithilfe der adaptiven Renderingtechnik um Desktop- und mobile Plattformen ohne Anpassung ordnungsgemäß zu rendern.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-675">These templates use the adaptive rendering technique to render properly in both desktop and mobile platforms without any customization.</span></span>

        <span data-ttu-id="7bd6e-676">![ASP.NET MVC 4-Projektvorlage in Größen von anderen Browser](aspnet-mvc-4-fundamentals/_static/image39.png "ASP.NET MVC 4-Projektvorlage in Größen von anderen Browser.")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-676">![ASP.NET MVC 4 project template in different browser sizes](aspnet-mvc-4-fundamentals/_static/image39.png "ASP.NET MVC 4 project template in different browser sizes")</span></span>

        <span data-ttu-id="7bd6e-677">*ASP.NET MVC 4-Projektvorlage in Größen von anderen Browser.*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-677">*ASP.NET MVC 4 project template in different browser sizes*</span></span>
5. <span data-ttu-id="7bd6e-678">Schließen Sie den Browser, um den Debugger beenden und zurück zu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-678">Close the browser to stop the debugger and return to Visual Studio.</span></span>
6. <span data-ttu-id="7bd6e-679">Jetzt können Sie in der Lage, untersuchen die Projektmappe, und sehen sich die neuen Funktionen in der Projektvorlage von ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-679">Now you are able to explore the solution and check out some of the new features introduced by ASP.NET MVC 4 in the project template.</span></span>

    <span data-ttu-id="7bd6e-680">![ASP.NET MVC4-Internet-Anwendung--Projektvorlage](aspnet-mvc-4-fundamentals/_static/image40.png "der Projekt Internetanwendungsvorlage in ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-680">![ASP.NET MVC4-internet-application-project-template](aspnet-mvc-4-fundamentals/_static/image40.png "The ASP.NET MVC 4 Internet Application Project Template")</span></span>

    <span data-ttu-id="7bd6e-681">*Der Projekt Internetanwendungsvorlage in ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-681">*The ASP.NET MVC 4 Internet Application Project Template*</span></span>

   1. <span data-ttu-id="7bd6e-682">**HTML5-markup**</span><span class="sxs-lookup"><span data-stu-id="7bd6e-682">**HTML5 markup**</span></span>

       <span data-ttu-id="7bd6e-683">Vorlage-Ansichten, um zu ermitteln, das neue Design Markup, z. B. open suchen **"About.cshtml"** anzeigen in **Startseite** Ordner.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-683">Browse template views to find out the new theme markup, for example open **About.cshtml** view within **Home** folder.</span></span>

       <span data-ttu-id="7bd6e-684">![Neue Vorlage für die Verwendung von Razor und HTML5-Markup](aspnet-mvc-4-fundamentals/_static/image41.png "neue Vorlage für die Verwendung von Razor und HTML5-Markup")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-684">![New template, using Razor and HTML5 markup](aspnet-mvc-4-fundamentals/_static/image41.png "New template, using Razor and HTML5 markup")</span></span>

       <span data-ttu-id="7bd6e-685">*Neue Vorlage für die Verwendung von Razor und HTML5-markup*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-685">*New template, using Razor and HTML5 markup*</span></span>
   2. <span data-ttu-id="7bd6e-686">**JavaScript-Bibliotheken enthalten**</span><span class="sxs-lookup"><span data-stu-id="7bd6e-686">**JavaScript libraries included**</span></span>

      1. <span data-ttu-id="7bd6e-687">**jQuery**: jQuery vereinfacht die HTML-Dokument durchlaufen, Ereignisbehandlung, Animation und Ajax-Interaktionen.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-687">**jQuery**: jQuery simplifies HTML document traversing, event handling, animating, and Ajax interactions.</span></span>
      2. <span data-ttu-id="7bd6e-688">**jQuery UI**: Diese Bibliothek bietet Abstraktionen für die Low-Level-Interaktion und Animationen, Erweiterte Effekte und Design beeinflusst Widgets, baut auf die jQuery-Bibliothek für JavaScript.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-688">**jQuery UI**: This library provides abstractions for low-level interaction and animation, advanced effects and themeable widgets, built on top of the jQuery JavaScript Library.</span></span>

         > [!NOTE]
         > <span data-ttu-id="7bd6e-689">Sie erfahren, jQuery und jQuery UI in [ [ http://docs.jquery.com/ ](http://docs.jquery.com/) ](http://docs.jquery.com/).</span><span class="sxs-lookup"><span data-stu-id="7bd6e-689">You can learn about jQuery and jQuery UI in [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span></span>
      3. <span data-ttu-id="7bd6e-690">**KnockoutJS**: die ASP.NET MVC 4-Standardvorlage enthält jetzt **KnockoutJS**, ein JavaScript-MVVM-Framework, das Sie umfassende und reaktionsschnelle Web-Anwendungen, die mit JavaScript und HTML-Elemente erstellen kann.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-690">**KnockoutJS**: The ASP.NET MVC 4 default template now includes **KnockoutJS**, a JavaScript MVVM framework that lets you create rich and highly responsive web applications using JavaScript and HTML.</span></span> <span data-ttu-id="7bd6e-691">Wie sind in ASP.NET MVC 3, jQuery und jQuery UI-Bibliotheken ebenfalls in ASP.NET MVC 4 enthalten.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-691">Like in ASP.NET MVC 3, jQuery and jQuery UI libraries are also included in ASP.NET MVC 4.</span></span>

          > [!NOTE]
          > <span data-ttu-id="7bd6e-692">Erhalten Sie weitere Informationen zur KnockOutJS-Bibliothek unter diesem Link: [ http://learn.knockoutjs.com/ ](http://learn.knockoutjs.com/).</span><span class="sxs-lookup"><span data-stu-id="7bd6e-692">You can get more information about KnockOutJS library in this link: [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/).</span></span>
      4. <span data-ttu-id="7bd6e-693">**Modernizr**: Diese Bibliothek automatisch ausgeführt, Ihre Website kompatibel mit älteren Browsern HTML5 und CSS3-Technologien mit herstellen.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-693">**Modernizr**: This library runs automatically, making your site compatible with older browsers when using HTML5 and CSS3 technologies.</span></span>

          > [!NOTE]
          > <span data-ttu-id="7bd6e-694">Erhalten Sie weitere Informationen zu Modernizr-Bibliothek unter diesem Link: [ http://www.modernizr.com/ ](http://www.modernizr.com/).</span><span class="sxs-lookup"><span data-stu-id="7bd6e-694">You can get more information about Modernizr library in this link: [http://www.modernizr.com/](http://www.modernizr.com/).</span></span>
   3. <span data-ttu-id="7bd6e-695">**In der Projektmappe enthalten SimpleMembership**</span><span class="sxs-lookup"><span data-stu-id="7bd6e-695">**SimpleMembership included in the solution**</span></span>

       <span data-ttu-id="7bd6e-696">SimpleMembership wurde als Ersatz für das vorherige ASP.NET Rollen- und Mitgliedschaftseinstellungen anbietersystem entwickelt.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-696">SimpleMembership has been designed as a replacement for the previous ASP.NET Role and Membership provider system.</span></span> <span data-ttu-id="7bd6e-697">Es verfügt über viele neue Features, die für den Entwickler sichere Webseiten auf eine flexiblere Art und Weise zu vereinfachen.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-697">It has many new features that make it easier for the developer to secure web pages in a more flexible way.</span></span>

       <span data-ttu-id="7bd6e-698">Die Internet-Vorlage hat bereits ein paar Dinge SimpleMembership integrieren festgelegt ist, z. B. AccountController-Komponente wird vorbereitet "oauthwebsecurity" (für OAuth-kontoregistrierung "," Login "," Verwaltung "," usw. ") und Internet-Sicherheit verwenden.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-698">The Internet template already has set up a few things to integrate SimpleMembership, for example, the AccountController is prepared to use OAuthWebSecurity (for OAuth account registration, login, management, etc.) and Web Security.</span></span>

       <span data-ttu-id="7bd6e-699">![SimpleMembership in der Projektmappe enthalten](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership in der Projektmappe enthalten.")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-699">![SimpleMembership Included in the solution](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership Included in the solution")</span></span>

       <span data-ttu-id="7bd6e-700">*SimpleMembership in der Projektmappe enthalten.*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-700">*SimpleMembership Included in the solution*</span></span>

       > [!NOTE]
       > <span data-ttu-id="7bd6e-701">Weitere Informationen finden Sie Informationen zu ["oauthwebsecurity"](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) in MSDN.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-701">Find more information about [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) in MSDN.</span></span>

> [!NOTE]
> <span data-ttu-id="7bd6e-702">Darüber hinaus können Sie diese Anwendung für Windows Azure-Websites Folgendes bereitstellen [Anhang B: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web Deploy](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="7bd6e-702">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="7bd6e-703">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="7bd6e-703">Summary</span></span>

<span data-ttu-id="7bd6e-704">Von dieser praktischen Übungseinheit haben Sie die Grundlagen von ASP.NET MVC gelernt:</span><span class="sxs-lookup"><span data-stu-id="7bd6e-704">By completing this Hands-On Lab you have learned the fundamentals of ASP.NET MVC:</span></span>

- <span data-ttu-id="7bd6e-705">Die Kernelemente einer MVC-Anwendung und deren Interaktion</span><span class="sxs-lookup"><span data-stu-id="7bd6e-705">The core elements of an MVC application and how they interact</span></span>
- <span data-ttu-id="7bd6e-706">Vorgehensweise: Erstellen einer ASP.NET MVC-Anwendung</span><span class="sxs-lookup"><span data-stu-id="7bd6e-706">How to create an ASP.NET MVC Application</span></span>
- <span data-ttu-id="7bd6e-707">Das Hinzufügen und Konfigurieren von Controllern zum Verarbeiten von Parametern übergeben wird, über die URL und Abfragezeichenfolge</span><span class="sxs-lookup"><span data-stu-id="7bd6e-707">How to add and configure Controllers to handle parameters passed through the URL and querystring</span></span>
- <span data-ttu-id="7bd6e-708">Gewusst wie: hinzufügen eine Masterseite "Layout" zum Einrichten einer Vorlage für allgemeine HTML-Inhalt, ein StyleSheet, um das Aussehen und Verhalten und eine ansichtsvorlage zum Anzeigen von HTML-Inhalten zu verbessern</span><span class="sxs-lookup"><span data-stu-id="7bd6e-708">How to add a layout master page to setup a template for common HTML content, a StyleSheet to enhance the look and feel and a View template to display HTML content</span></span>
- <span data-ttu-id="7bd6e-709">Gewusst wie: Verwenden Sie das ViewModel-Muster für die Übergabe von Eigenschaften an die ansichtsvorlage zum Anzeigen dynamischer Informationen</span><span class="sxs-lookup"><span data-stu-id="7bd6e-709">How to use the ViewModel pattern for passing properties to the View template to display dynamic information</span></span>
- <span data-ttu-id="7bd6e-710">Verwendung von Parametern an Controller übergeben werden, in der Vorlage anzeigen</span><span class="sxs-lookup"><span data-stu-id="7bd6e-710">How to use parameters passed to Controllers in the View template</span></span>
- <span data-ttu-id="7bd6e-711">Gewusst wie: Hinzufügen von Links zu Seiten, die in der ASP.NET MVC-Anwendung</span><span class="sxs-lookup"><span data-stu-id="7bd6e-711">How to add links to pages inside the ASP.NET MVC application</span></span>
- <span data-ttu-id="7bd6e-712">Das Hinzufügen und Verwenden von dynamischen Eigenschaften in einer Sicht</span><span class="sxs-lookup"><span data-stu-id="7bd6e-712">How to add and use dynamic properties in a View</span></span>
- <span data-ttu-id="7bd6e-713">Die Verbesserungen in ASP.NET MVC 4-Projektvorlagen</span><span class="sxs-lookup"><span data-stu-id="7bd6e-713">The enhancements in the ASP.NET MVC 4 project templates</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="7bd6e-714">Anhang A: Installieren von Visual Studio Express 2012 für Web</span><span class="sxs-lookup"><span data-stu-id="7bd6e-714">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="7bd6e-715">Sie installieren können **Microsoft Visual Studio Express 2012 für Web** oder einem anderen &quot;Express&quot; Version verwenden, die **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-715">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="7bd6e-716">Die folgenden Anweisungen führen Sie über die erforderlichen Schritte zum Installieren *Visual Studio Express 2012 für Web* mit *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-716">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="7bd6e-717">Wechseln Sie zu [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="7bd6e-717">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="7bd6e-718">Auch wenn Sie bereits Webplattform-Installer installiert haben, können Sie öffnen ihn, und suchen Sie nach dem Produkt &quot; <em>Visual Studio Express 2012 für das Web mit Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-718">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="7bd6e-719">Klicken Sie auf **jetzt installieren**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-719">Click on **Install Now**.</span></span> <span data-ttu-id="7bd6e-720">Wenn Sie keine **Webplattform-Installer** gelangen Sie zum Herunterladen und installieren Sie diese zuerst.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-720">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="7bd6e-721">Einmal **Webplattform-Installer** geöffnet ist, klicken Sie auf **installieren** um das Setup zu starten.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-721">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="7bd6e-722">![Installieren von Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "Installieren von Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-722">![Install Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="7bd6e-723">*Installieren von Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-723">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="7bd6e-724">Lesen Sie die Produkte Lizenzen und Begriffe, und klicken Sie auf **akzeptieren** um den Vorgang fortzusetzen.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-724">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Akzeptieren der Lizenzbedingungen](aspnet-mvc-4-fundamentals/_static/image44.png)

    <span data-ttu-id="7bd6e-726">*Akzeptieren der Lizenzbedingungen*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-726">*Accepting the license terms*</span></span>
5. <span data-ttu-id="7bd6e-727">Warten Sie, bis der Prozess zum Herunterladen und die Installation abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-727">Wait until the downloading and installation process completes.</span></span>

    ![Installationsstatus](aspnet-mvc-4-fundamentals/_static/image45.png)

    <span data-ttu-id="7bd6e-729">*Installationsstatus*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-729">*Installation progress*</span></span>
6. <span data-ttu-id="7bd6e-730">Wenn die Installation abgeschlossen ist, klicken Sie auf **Fertig stellen**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-730">When the installation completes, click **Finish**.</span></span>

    ![Die Installation wurde abgeschlossen](aspnet-mvc-4-fundamentals/_static/image46.png)

    <span data-ttu-id="7bd6e-732">*Die Installation wurde abgeschlossen*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-732">*Installation completed*</span></span>
7. <span data-ttu-id="7bd6e-733">Klicken Sie auf **beenden** Webplattform-Installer zu schließen.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-733">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="7bd6e-734">Um Visual Studio Express für Web zu öffnen, wechseln Sie zu der **starten** schreiben zu starten und Bildschirm &quot; **VS Express**&quot;, klicken Sie dann auf die **Visual Studio Express für Web** die Kachel.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-734">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Visual Studio Express für Web-Kachel](aspnet-mvc-4-fundamentals/_static/image47.png)

    <span data-ttu-id="7bd6e-736">*Visual Studio Express für Web-Kachel*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-736">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="7bd6e-737">Anhang B: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web Deploy</span><span class="sxs-lookup"><span data-stu-id="7bd6e-737">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="7bd6e-738">In diesem Anhang wird veranschaulicht, wie erstellen eine neue Website aus dem Windows Azure-Verwaltungsportal, und Veröffentlichen der Anwendung, die Sie erhalten haben, indem der testumgebung können die Nutzung der Web Deploy-publishing-Funktion von Windows Azure bereitgestellte.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-738">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="7bd6e-739">Aufgabe 1: Erstellen einer neuen Website aus dem Windows Azure-Portal</span><span class="sxs-lookup"><span data-stu-id="7bd6e-739">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="7bd6e-740">Wechseln Sie zu der [Windows Azure-Verwaltungsportal](https://manage.windowsazure.com/) und melden Sie sich mit den Microsoft-Anmeldeinformationen, die Ihrem Abonnement zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-740">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7bd6e-741">Mit Windows Azure können Sie 10 ASP.NET-Websites kostenlos hosten und dann zu skalieren, wenn Ihr Datenverkehr zunimmt.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-741">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="7bd6e-742">Sie können registrieren [hier](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="7bd6e-742">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="7bd6e-743">![Melden Sie sich bei Windows Azure-Portal](aspnet-mvc-4-fundamentals/_static/image48.png "melden Sie sich bei Windows Azure-Portal")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-743">![Log on to Windows Azure portal](aspnet-mvc-4-fundamentals/_static/image48.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="7bd6e-744">*Melden Sie sich bei Windows Azure-Verwaltungsportal*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-744">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="7bd6e-745">Klicken Sie auf **neu** auf der Befehlsleiste.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-745">Click **New** on the command bar.</span></span>

    <span data-ttu-id="7bd6e-746">![Erstellen einer neuen Website](aspnet-mvc-4-fundamentals/_static/image49.png "Erstellen einer neuen Website")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-746">![Creating a new Web Site](aspnet-mvc-4-fundamentals/_static/image49.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="7bd6e-747">*Erstellen einer neuen Website*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-747">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="7bd6e-748">Klicken Sie auf **Compute** | **Website**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-748">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="7bd6e-749">Wählen Sie dann **Schnellerfassung** Option.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-749">Then select **Quick Create** option.</span></span> <span data-ttu-id="7bd6e-750">Geben Sie eine verfügbare URL für die neue Website, und klicken Sie auf **Website erstellen**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-750">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7bd6e-751">Ein Windows Azure-Website ist der Host für eine Webanwendung, die in der Cloud, die Sie steuern und verwalten können.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-751">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="7bd6e-752">Die Option "Schnellerfassung" können Sie eine vollständige Webanwendung auf dem Windows Azure-Website von außerhalb des Portals bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-752">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="7bd6e-753">Er umfasst keine Informationen zum Einrichten einer Datenbank.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-753">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="7bd6e-754">![Erstellen einer neuen Website mithilfe der Schnellerfassung](aspnet-mvc-4-fundamentals/_static/image50.png "Erstellen einer neuen Website mithilfe der Schnellerfassung")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-754">![Creating a new Web Site using Quick Create](aspnet-mvc-4-fundamentals/_static/image50.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="7bd6e-755">*Erstellen einer neuen Website mithilfe der Schnellerfassung*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-755">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="7bd6e-756">Warten Sie, bis die neue **Website** erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-756">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="7bd6e-757">Nachdem die Website erstellt wurde, klicken Sie auf den Link unter der **URL** Spalte.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-757">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="7bd6e-758">Überprüfen Sie, dass die neue Website ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-758">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="7bd6e-759">![Navigieren zu der neuen Website](aspnet-mvc-4-fundamentals/_static/image51.png "auf die neue Website durchsuchen")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-759">![Browsing to the new web site](aspnet-mvc-4-fundamentals/_static/image51.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="7bd6e-760">*Um die neue Website durchsuchen*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-760">*Browsing to the new web site*</span></span>

    <span data-ttu-id="7bd6e-761">![Website](aspnet-mvc-4-fundamentals/_static/image52.png "Website ausgeführt wird")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-761">![Web site running](aspnet-mvc-4-fundamentals/_static/image52.png "Web site running")</span></span>

    <span data-ttu-id="7bd6e-762">*Die Website ausgeführt wird*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-762">*Web site running*</span></span>
6. <span data-ttu-id="7bd6e-763">Wechseln Sie zurück zum Portal, und klicken Sie auf den Namen der Website unter der **Namen** Spalte die Verwaltungsseiten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-763">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="7bd6e-764">![Öffnen die Website-Verwaltungsseiten](aspnet-mvc-4-fundamentals/_static/image53.png "öffnen die Website-Verwaltungsseiten")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-764">![Opening the web site management pages](aspnet-mvc-4-fundamentals/_static/image53.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="7bd6e-765">*Öffnen die Website-Verwaltungsseiten*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-765">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="7bd6e-766">In der **Dashboard** Seite die **Blick** auf die **Veröffentlichungsprofil herunterladen** Link.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-766">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7bd6e-767">Die *Veröffentlichungsprofil* enthält alle Informationen zum Veröffentlichen einer Webanwendung in einer Windows Azure-Website für die einzelnen aktivierten Veröffentlichungsmethoden erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-767">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="7bd6e-768">Das Veröffentlichungsprofil enthält die URLs, Benutzeranmeldeinformationen und datenbankzeichenfolgen, die zum Herstellen einer Verbindung mit und die Authentifizierung bei jedem der Endpunkte für die eine Veröffentlichungsmethode aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-768">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="7bd6e-769">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express für Web** und **Microsoft Visual Studio 2012** Unterstützung, die beim Lesen von veröffentlichungsprofilen Automatisieren der Konfiguration von diesen Programmen Veröffentlichung von Webanwendungen in Windows Azure-Websites.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-769">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="7bd6e-770">![Herunterladen der Website-Veröffentlichungsprofil](aspnet-mvc-4-fundamentals/_static/image54.png "herunterladen den Website-Veröffentlichungsprofil")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-770">![Downloading the web site publish profile](aspnet-mvc-4-fundamentals/_static/image54.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="7bd6e-771">*Herunterladen der Website-Veröffentlichungsprofil*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-771">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="7bd6e-772">Laden Sie das Veröffentlichungsprofil an einem bekannten Speicherort herunter.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-772">Download the publish profile file to a known location.</span></span> <span data-ttu-id="7bd6e-773">In dieser Übung wird weiter wie Sie diese Datei verwenden, um das Veröffentlichen einer Webanwendung auf einem Windows Azure-Websites in Visual Studio angezeigt.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-773">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="7bd6e-774">![Speichern das Veröffentlichungsprofil](aspnet-mvc-4-fundamentals/_static/image55.png "speichern das Veröffentlichungsprofil")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-774">![Saving the publish profile file](aspnet-mvc-4-fundamentals/_static/image55.png "Saving the publish profile")</span></span>

    <span data-ttu-id="7bd6e-775">*Speichern das Veröffentlichungsprofil*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-775">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="7bd6e-776">Aufgabe 2: Konfigurieren des Datenbank-Servers</span><span class="sxs-lookup"><span data-stu-id="7bd6e-776">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="7bd6e-777">Wenn Ihre Anwendung nutzt SQL Server Datenbanken, die Sie zum Erstellen eines SQL-Datenbank-Servers benötigen.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-777">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="7bd6e-778">Wenn zum Bereitstellen einer einfachen Anwendung, die keine SQL Server verwendet werden sollen, können Sie diese Aufgabe überspringen.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-778">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="7bd6e-779">Sie benötigen einen SQL-Datenbank-Server zum Speichern der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-779">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="7bd6e-780">Sehen Sie die SQL-Datenbank-Server aus Ihrem Abonnement in das Windows Azure-Verwaltungsportal unter **Sql-Datenbanken** | **Server** | **des Servers Dashboard**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-780">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="7bd6e-781">Wenn Sie nicht mit eine Server-Instanz verfügen, können Sie erstellen, mithilfe der **hinzufügen** auf der Befehlsleiste auf die Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-781">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="7bd6e-782">Notieren Sie sich die **Servername und -URL, Administrator-Anmeldenamen und Kennwort**, wie Sie sie in den nächsten Aufgaben verwenden.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-782">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="7bd6e-783">Erstellen Sie die Datenbank nicht noch, wie es in einem späteren Zeitpunkt erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-783">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="7bd6e-784">![SQL Server-Datenbank-Dashboard](aspnet-mvc-4-fundamentals/_static/image56.png "Dashboard der SQL-Datenbank-Server")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-784">![SQL Database Server Dashboard](aspnet-mvc-4-fundamentals/_static/image56.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="7bd6e-785">*SQL Server-Datenbank-Dashboard*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-785">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="7bd6e-786">In der nächsten Aufgabe testen Sie die Verbindung mit der Datenbank, in Visual Studio aus diesem Grund Sie Ihre lokale IP-Adresse in der Liste des Servers der einschließen müssen **zulässige IP-Adressen**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-786">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="7bd6e-787">Zu diesem Zweck klicken Sie auf **konfigurieren**, wählen Sie die IP-Adresse von **Current Client IP Address** und fügen Sie ihn auf die **IP-Startadresse** und **IP-Endadresse** Textfelder, und klicken Sie auf die ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-787">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) button.</span></span>

    ![Client-IP-Adresse hinzufügen](aspnet-mvc-4-fundamentals/_static/image58.png)

    <span data-ttu-id="7bd6e-789">*Client-IP-Adresse hinzufügen*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-789">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="7bd6e-790">Einmal die **Client-IP-Adresse** wird hinzugefügt, um die zulässigen IP-Adressen aufzulisten, klicken Sie auf **speichern** um die Änderungen zu bestätigen.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-790">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Bestätigen von Änderungen](aspnet-mvc-4-fundamentals/_static/image59.png)

    <span data-ttu-id="7bd6e-792">*Bestätigen von Änderungen*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-792">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="7bd6e-793">Aufgabe 3: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web Deploy</span><span class="sxs-lookup"><span data-stu-id="7bd6e-793">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="7bd6e-794">Wechseln Sie zurück, der ASP.NET MVC 4-Projektmappe.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-794">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="7bd6e-795">In der **Projektmappen-Explorer**mit der rechten Maustaste auf das Websiteprojekt, und wählen Sie **veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-795">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="7bd6e-796">![Veröffentlichen der Anwendung](aspnet-mvc-4-fundamentals/_static/image60.png "Veröffentlichen der Anwendung")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-796">![Publishing the Application](aspnet-mvc-4-fundamentals/_static/image60.png "Publishing the Application")</span></span>

    <span data-ttu-id="7bd6e-797">*Veröffentlichen der Website*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-797">*Publishing the web site*</span></span>
2. <span data-ttu-id="7bd6e-798">Importieren Sie das Veröffentlichungsprofil, die, das Sie in der ersten Aufgabe gespeichert.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-798">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="7bd6e-799">![Importieren das Veröffentlichungsprofil](aspnet-mvc-4-fundamentals/_static/image61.png "Importieren des Veröffentlichungsprofils")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-799">![Importing the publish profile](aspnet-mvc-4-fundamentals/_static/image61.png "Importing the publish profile")</span></span>

    <span data-ttu-id="7bd6e-800">*Importieren Sie das Veröffentlichungsprofil*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-800">*Importing publish profile*</span></span>
3. <span data-ttu-id="7bd6e-801">Klicken Sie auf **überprüft, ob Verbindung**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-801">Click **Validate Connection**.</span></span> <span data-ttu-id="7bd6e-802">Klicken Sie nach Abschluss der Überprüfung auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-802">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7bd6e-803">Die Überprüfung ist abgeschlossen, wenn Sie sehen, dass ein grünes Häkchen neben der Schaltfläche "Verbindung überprüfen" angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-803">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="7bd6e-804">![Überprüfen der Verbindung](aspnet-mvc-4-fundamentals/_static/image62.png "Überprüfen der Verbindung")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-804">![Validating connection](aspnet-mvc-4-fundamentals/_static/image62.png "Validating connection")</span></span>

    <span data-ttu-id="7bd6e-805">*Überprüfen der Verbindung*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-805">*Validating connection*</span></span>
4. <span data-ttu-id="7bd6e-806">In der **Einstellungen** Seite die **Datenbanken** auf die Schaltfläche neben dem Textfeld für die datenbankverbindung (d. h. **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="7bd6e-806">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="7bd6e-807">![Web deploy-Konfiguration](aspnet-mvc-4-fundamentals/_static/image63.png "Web deploy-Konfiguration")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-807">![Web deploy configuration](aspnet-mvc-4-fundamentals/_static/image63.png "Web deploy configuration")</span></span>

    <span data-ttu-id="7bd6e-808">*Web deploy-Konfiguration*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-808">*Web deploy configuration*</span></span>
5. <span data-ttu-id="7bd6e-809">Konfigurieren Sie die Verbindung mit der Datenbank wie folgt:</span><span class="sxs-lookup"><span data-stu-id="7bd6e-809">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="7bd6e-810">In der **Servernamen** Geben Sie Ihre SQL-Datenbankserver URL mit der *Tcp:* Präfix.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-810">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="7bd6e-811">In **Benutzernamen** Geben Sie Ihre Server Administrator-Anmeldenamen.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-811">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="7bd6e-812">In **Kennwort** Geben Sie Ihre Server Administrator-Anmeldekennwort.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-812">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="7bd6e-813">Geben Sie einen neuen Datenbanknamen ein, z. B.: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-813">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="7bd6e-814">![Konfigurieren von Ziel-Verbindungszeichenfolge](aspnet-mvc-4-fundamentals/_static/image64.png "Zielverbindungszeichenfolge konfigurieren")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-814">![Configuring destination connection string](aspnet-mvc-4-fundamentals/_static/image64.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="7bd6e-815">*Konfigurieren von Ziel-Verbindungszeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-815">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="7bd6e-816">Klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-816">Then click **OK**.</span></span> <span data-ttu-id="7bd6e-817">Bei der Aufforderung zum Erstellen der Datenbank auf **Ja**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-817">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="7bd6e-818">![Erstellen der Datenbank](aspnet-mvc-4-fundamentals/_static/image65.png "erstellen die datenbankzeichenfolge")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-818">![Creating the database](aspnet-mvc-4-fundamentals/_static/image65.png "Creating the database string")</span></span>

    <span data-ttu-id="7bd6e-819">*Erstellen der Datenbank*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-819">*Creating the database*</span></span>
7. <span data-ttu-id="7bd6e-820">Die Verbindungszeichenfolge, die Sie für die Verbindung mit SQL-Datenbank in Windows Azure verwendet werden, ist innerhalb der Verbindungstyp Standard Textfeld angezeigt.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-820">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="7bd6e-821">Klicken Sie dann auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-821">Then click **Next**.</span></span>

    <span data-ttu-id="7bd6e-822">![Auf der SQL-Datenbank-Verbindungszeichenfolge](aspnet-mvc-4-fundamentals/_static/image66.png "auf SQL-Datenbank-Verbindungszeichenfolge")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-822">![Connection string pointing to SQL Database](aspnet-mvc-4-fundamentals/_static/image66.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="7bd6e-823">*Auf der SQL-Datenbank-Verbindungszeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-823">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="7bd6e-824">In der **Vorschau** auf **veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-824">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="7bd6e-825">![Veröffentlichen der Webanwendung](aspnet-mvc-4-fundamentals/_static/image67.png "Veröffentlichen der Webanwendung")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-825">![Publishing the web application](aspnet-mvc-4-fundamentals/_static/image67.png "Publishing the web application")</span></span>

    <span data-ttu-id="7bd6e-826">*Veröffentlichen der Webanwendung*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-826">*Publishing the web application*</span></span>
9. <span data-ttu-id="7bd6e-827">Sobald der Veröffentlichungsprozess abgeschlossen ist, öffnet Ihr Standardbrowser die veröffentlichte Website.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-827">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="7bd6e-828">![Anwendung auf Windows Azure veröffentlicht](aspnet-mvc-4-fundamentals/_static/image68.png "Anwendung auf Windows Azure veröffentlicht")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-828">![Application published to Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="7bd6e-829">*Anwendung in Windows Azure veröffentlicht*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-829">*Application published to Windows Azure*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="7bd6e-830">Anhang C: Verwenden von Codeausschnitten</span><span class="sxs-lookup"><span data-stu-id="7bd6e-830">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="7bd6e-831">Mit Codeausschnitten müssen Sie den Code zur Hand benötigten.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-831">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="7bd6e-832">Das Lab-Dokument informiert Sie genau wann sie verwendet werden kann wie in der folgenden Abbildung dargestellt.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-832">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="7bd6e-833">![Verwenden von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihrem Projekt](aspnet-mvc-4-fundamentals/_static/image69.png "mithilfe von Visual Studio-Codeausschnitten, die Code in das Projekt einfügen.")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-833">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-fundamentals/_static/image69.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="7bd6e-834">*Verwenden von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihrem Projekt*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-834">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="7bd6e-835">***Hinzufügen ein Codeausschnitts, die über die Tastatur (nur c#)***</span><span class="sxs-lookup"><span data-stu-id="7bd6e-835">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="7bd6e-836">Platzieren Sie den Cursor, wo Sie möchten den Code einfügen.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-836">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="7bd6e-837">Starten Sie den codeausschnittsnamen (ohne Leerzeichen oder Bindestriche) eingeben.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-837">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="7bd6e-838">Beobachten Sie, wie IntelliSense zeigt übereinstimmende Codeausschnitte Namen.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-838">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="7bd6e-839">Wählen Sie den richtigen Codeausschnitt (oder halten Sie eingeben, bis die gesamte codeausschnittsnamen ausgewählt ist).</span><span class="sxs-lookup"><span data-stu-id="7bd6e-839">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="7bd6e-840">Drücken Sie die Tab-Taste zweimal auf Einfügen des Codeausschnitts an der Cursorposition ein.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-840">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="7bd6e-841">![Geben Sie den Namen des Ausschnitts](aspnet-mvc-4-fundamentals/_static/image70.png "Geben Sie den Namen des Ausschnitts")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-841">![Start typing the snippet name](aspnet-mvc-4-fundamentals/_static/image70.png "Start typing the snippet name")</span></span>

<span data-ttu-id="7bd6e-842">*Geben Sie den Namen des Ausschnitts*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-842">*Start typing the snippet name*</span></span>

<span data-ttu-id="7bd6e-843">![Tabstopp drücken, um den hervorgehobenen Codeausschnitt auswählen](aspnet-mvc-4-fundamentals/_static/image71.png "Tab drücken, um den hervorgehobenen Codeausschnitt auswählen")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-843">![Press Tab to select the highlighted snippet](aspnet-mvc-4-fundamentals/_static/image71.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="7bd6e-844">*Drücken Sie Tabstopp, um den hervorgehobenen Codeausschnitt auswählen*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-844">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="7bd6e-845">![Drücken Sie die Tabulatortaste erneut, und der Ausschnitt erweitert](aspnet-mvc-4-fundamentals/_static/image72.png "drücken Sie die Tabulatortaste erneut, und der Ausschnitt werden erweitert.")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-845">![Press Tab again and the snippet will expand](aspnet-mvc-4-fundamentals/_static/image72.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="7bd6e-846">*Drücken Sie die Tabulatortaste erneut, und der Ausschnitt werden erweitert.*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-846">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="7bd6e-847">***Hinzufügen ein Codeausschnitts, die mit der Maus (c#, Visual Basic und XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-847">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="7bd6e-848">Mit der rechten Maustaste, in dem den Codeausschnitt eingefügt werden soll.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-848">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="7bd6e-849">Wählen Sie **Ausschnitt einfügen** gefolgt von **Meine Codeausschnitte**.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-849">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="7bd6e-850">Wählen Sie die relevante Codeausschnitte in der Liste, indem Sie darauf klicken.</span><span class="sxs-lookup"><span data-stu-id="7bd6e-850">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="7bd6e-851">![Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten](aspnet-mvc-4-fundamentals/_static/image73.png "mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten,")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-851">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-fundamentals/_static/image73.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="7bd6e-852">*Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-852">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="7bd6e-853">![Wählen Sie die relevante Codeausschnitte in der Liste, indem Sie darauf klicken](aspnet-mvc-4-fundamentals/_static/image74.png "die relevante Codeausschnitte in der Liste auswählen, indem Sie darauf klicken")</span><span class="sxs-lookup"><span data-stu-id="7bd6e-853">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-fundamentals/_static/image74.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="7bd6e-854">*Wählen Sie die relevante Codeausschnitte in der Liste, indem Sie darauf klicken*</span><span class="sxs-lookup"><span data-stu-id="7bd6e-854">*Pick the relevant snippet from the list, by clicking on it*</span></span>
