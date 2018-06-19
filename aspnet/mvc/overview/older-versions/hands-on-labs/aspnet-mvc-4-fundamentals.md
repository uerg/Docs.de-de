---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
title: ASP.NET MVC 4-Grundlagen | Microsoft Docs
author: rick-anderson
description: Diese praktische Übungseinheit auf MVC (Model View Controller) Music Store eine lernprogrammanwendung basiert, eingeführt und erläutert schrittweise, wie ASP.NET MV verwenden...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: b7dba543-73c3-4534-a9a0-ba70fa2c6a8a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
msc.type: authoredcontent
ms.openlocfilehash: 225dff4663e0e556cfb8966f1078848b4c2b47a5
ms.sourcegitcommit: 3a893ae05f010656d99d6ddf55e82f1b5b6933bc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/18/2018
ms.locfileid: "34306766"
---
# <a name="aspnet-mvc-4-fundamentals"></a><span data-ttu-id="7d536-103">ASP.NET MVC 4-Grundlagen</span><span class="sxs-lookup"><span data-stu-id="7d536-103">ASP.NET MVC 4 Fundamentals</span></span>

<span data-ttu-id="7d536-104">Durch [Web Lager Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="7d536-104">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="7d536-105">Herunterladen von Web-Lager Training Kit</span><span class="sxs-lookup"><span data-stu-id="7d536-105">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="7d536-106">Diese praktische Übungseinheit basiert auf MVC (Model View Controller) Music Store eine lernprogrammanwendung, die werden vorgestellt und erläutert schrittweise, wie ASP.NET MVC und Visual Studio verwenden.</span><span class="sxs-lookup"><span data-stu-id="7d536-106">This Hands-On Lab is based on MVC (Model View Controller) Music Store, a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio.</span></span> <span data-ttu-id="7d536-107">Im Labor erfahren Sie der Einfachheit halber noch Stromversorgung, der die gemeinsame Verwendung von diesen Technologien.</span><span class="sxs-lookup"><span data-stu-id="7d536-107">Throughout the lab you will learn the simplicity, yet power of using these technologies together.</span></span> <span data-ttu-id="7d536-108">Sie werden mit einer einfachen Anwendung gestartet und realisiert werden erst stehen Ihnen eine voll funktionsfähige ASP.NET MVC 4-Webanwendung.</span><span class="sxs-lookup"><span data-stu-id="7d536-108">You will start with a simple application and will build it until you have a fully functional ASP.NET MVC 4 Web Application.</span></span>

<span data-ttu-id="7d536-109">In dieser Umgebung funktioniert mit ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="7d536-109">This Lab works with ASP.NET MVC 4.</span></span>

<span data-ttu-id="7d536-110">Wenn Sie die ASP.NET MVC 3-Version des Lernprogramms Anwendung untersuchen möchten, finden Sie in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="7d536-110">If you wish to explore the ASP.NET MVC 3 version of the tutorial application, you can find it in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>

<span data-ttu-id="7d536-111">Diese praktische Übungseinheit wird davon ausgegangen, dass der Entwickler Erfahrung in der Entwicklung von webtechnologien wie HTML und JavaScript hat.</span><span class="sxs-lookup"><span data-stu-id="7d536-111">This Hands-On Lab assumes that the developer has experience in Web development technologies, such as HTML and JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="7d536-112">Alle Beispielcode und Codeausschnitte sind im Web Lager Training Kit unter enthalten [Versionen von Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="7d536-112">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="7d536-113">Das Projekt, das speziell für diese Übung finden Sie unter [ASP.NET MVC 4-Grundlagen](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals).</span><span class="sxs-lookup"><span data-stu-id="7d536-113">The project specific to this lab is available at [ASP.NET MVC 4 Fundamentals](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals).</span></span>

<a id="The_Music_Store_application"></a>
### <a name="the-music-store-application"></a><span data-ttu-id="7d536-114">Die Music Store-Anwendung</span><span class="sxs-lookup"><span data-stu-id="7d536-114">The Music Store application</span></span>

<span data-ttu-id="7d536-115">Die Music Store-Webanwendung, die in dieser Umgebung erstellt werden besteht aus drei Hauptteilen zusammen: Warenkorb, Auschecken und Verwaltung.</span><span class="sxs-lookup"><span data-stu-id="7d536-115">The Music Store web application that will be built throughout this Lab comprises three main parts: shopping, checkout, and administration.</span></span> <span data-ttu-id="7d536-116">Besucher sind Lage Alben durchsuchen, indem "Genre" Einkaufswagen Alben hinzu, überprüfen ihre Auswahl, und fahren Sie schließlich mit Auschecken, um sich anzumelden, und führen Sie die Reihenfolge.</span><span class="sxs-lookup"><span data-stu-id="7d536-116">Visitors will be able to browse albums by genre, add albums to their cart, review their selection and finally proceed to checkout to login and complete the order.</span></span> <span data-ttu-id="7d536-117">Darüber hinaus werden Administratoren Store verfügbaren Alben sowie ihre wichtigsten Eigenschaften verwalten können.</span><span class="sxs-lookup"><span data-stu-id="7d536-117">Additionally, store administrators will be able to manage the available albums as well as their main properties.</span></span>

<span data-ttu-id="7d536-118">![Music Store Bildschirme](aspnet-mvc-4-fundamentals/_static/image1.png "Music Store Bildschirme")</span><span class="sxs-lookup"><span data-stu-id="7d536-118">![Music Store screens](aspnet-mvc-4-fundamentals/_static/image1.png "Music Store screens")</span></span>

<span data-ttu-id="7d536-119">*Music Store Bildschirme*</span><span class="sxs-lookup"><span data-stu-id="7d536-119">*Music Store screens*</span></span>

<a id="ASPNET_MVC_4_Essentials"></a>
### <a name="aspnet-mvc-4-essentials"></a><span data-ttu-id="7d536-120">ASP.NET MVC 4-Essentials</span><span class="sxs-lookup"><span data-stu-id="7d536-120">ASP.NET MVC 4 Essentials</span></span>

<span data-ttu-id="7d536-121">Music Store-Anwendung erstellt, die mit **Model View Controller (MVC)**, eine architektonische Muster, das trennt eine Anwendung in drei Hauptkomponenten:</span><span class="sxs-lookup"><span data-stu-id="7d536-121">Music Store application will be built using **Model View Controller (MVC)**, an architectural pattern that separates an application into three main components:</span></span>

- <span data-ttu-id="7d536-122">**Modelle**: Modellobjekte sind die Teile der Anwendung, die die Domänenlogik implementieren.</span><span class="sxs-lookup"><span data-stu-id="7d536-122">**Models**: Model objects are the parts of the application that implement the domain logic.</span></span> <span data-ttu-id="7d536-123">Modellobjekte wird häufig auch abrufen, und der Modellzustand in einer Datenbank speichern.</span><span class="sxs-lookup"><span data-stu-id="7d536-123">Often, model objects also retrieve and store model state in a database.</span></span>
- <span data-ttu-id="7d536-124">**Ansichten:** Ansichten sind die Komponenten, die Benutzeroberfläche (UI) der Anwendung anzeigen.</span><span class="sxs-lookup"><span data-stu-id="7d536-124">**Views:** Views are the components that display the application's user interface (UI).</span></span> <span data-ttu-id="7d536-125">In der Regel wird diese Benutzeroberfläche aus den Modelldaten erstellt.</span><span class="sxs-lookup"><span data-stu-id="7d536-125">Typically, this UI is created from the model data.</span></span> <span data-ttu-id="7d536-126">Ein Beispiel wäre der Bearbeitungsansicht von Alben, die Textfelder und eine Dropdown-Liste basierend auf den aktuellen Status eines Objekts Album anzeigt.</span><span class="sxs-lookup"><span data-stu-id="7d536-126">An example would be the edit view of Albums that displays text boxes and a drop-down list based on the current state of an Album object.</span></span>
- <span data-ttu-id="7d536-127">**Domänencontroller:** Controller sind die Komponenten behandeln Benutzerinteraktionen, bearbeiten das Modell und letztlich wählen Sie eine Ansicht zum Rendern der Benutzeroberflächenautomatisierungs.</span><span class="sxs-lookup"><span data-stu-id="7d536-127">**Controllers:** Controllers are the components that handle user interaction, manipulate the model, and ultimately select a view to render the UI.</span></span> <span data-ttu-id="7d536-128">In einer MVC-Anwendung zeigt die Ansicht nur Informationen an. Benutzereingaben und -interaktionen werden vom Controller verarbeitet und beantwortet.</span><span class="sxs-lookup"><span data-stu-id="7d536-128">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span>

<span data-ttu-id="7d536-129">Das MVC-Muster unterstützt Sie beim Erstellen von Anwendungen, die die verschiedenen Aspekte der Anwendung (Eingabelogik, Geschäftslogik und Benutzeroberflächen-Logik), und gleichzeitig eine lose Kopplung zwischen diesen Elementen liegen.</span><span class="sxs-lookup"><span data-stu-id="7d536-129">The MVC pattern helps you to create applications that separate the different aspects of the application (input logic, business logic, and UI logic), while providing a loose coupling between these elements.</span></span> <span data-ttu-id="7d536-130">Aufgrund dieser Trennung können Sie die Komplexität bei der Erstellung einer Anwendung verwalten, wie Sie auf einen Aspekt der Implementierung zu einem Zeitpunkt konzentrieren können.</span><span class="sxs-lookup"><span data-stu-id="7d536-130">This separation helps you manage complexity when you build an application, as it allows you to focus on one aspect of the implementation at a time.</span></span> <span data-ttu-id="7d536-131">Darüber hinaus erleichtert das MVC-Schema zum Testen von Anwendungen, auch wenn Sie die Verwendung von Test-driven Development (TDD) zum Erstellen von Anwendungen.</span><span class="sxs-lookup"><span data-stu-id="7d536-131">In addition, the MVC pattern makes it easy to test applications, also encouraging the use of test-driven development (TDD) for creating applications.</span></span>

<span data-ttu-id="7d536-132">Die **ASP.NET-MVC** Framework bietet eine Alternative zum ASP.NET Web Forms-Muster für das Erstellen von ASP.NET MVC-basierten Webanwendungen.</span><span class="sxs-lookup"><span data-stu-id="7d536-132">The **ASP.NET MVC** framework provides an alternative to the ASP.NET Web Forms pattern for creating ASP.NET MVC-based Web applications.</span></span> <span data-ttu-id="7d536-133">Die **ASP.NET-MVC** Framework ist eine kompakte, mitgliedschaftsbasierte zu testendes Präsentationsframework, (wie bei Web Forms-basierten Anwendungen) ist in vorhandene ASP.NET-Funktionen, z. B. Gestaltungsvorlagen und Mitgliedschaft basierende integriert Authentifizierung, sodass Sie die gesamte Leistungsfähigkeit von .NET Framework Core abrufen.</span><span class="sxs-lookup"><span data-stu-id="7d536-133">The **ASP.NET MVC** framework is a lightweight, highly testable presentation framework that (as with Web-forms-based applications) is integrated with existing ASP.NET features, such as master pages and membership-based authentication so you get all the power of the core .NET framework.</span></span> <span data-ttu-id="7d536-134">Dies ist hilfreich, wenn Sie bereits mit ASP.NET Web Forms vertraut sind, da alle Bibliotheken, die Sie bereits verwenden, ebenfalls in ASP.NET MVC 4 verfügbar sind.</span><span class="sxs-lookup"><span data-stu-id="7d536-134">This is useful if you are already familiar with ASP.NET Web Forms because all the libraries that you already use are available in ASP.NET MVC 4 as well.</span></span>

<span data-ttu-id="7d536-135">Der lose Kopplung zwischen den drei Hauptkomponenten einer MVC-Anwendung begünstigt darüber hinaus auch die parallele Entwicklung.</span><span class="sxs-lookup"><span data-stu-id="7d536-135">In addition, the loose coupling between the three main components of an MVC application also promotes parallel development.</span></span> <span data-ttu-id="7d536-136">Z. B. ein Entwickler kann für die Sicht arbeiten, ein zweiter Entwickler an der Controllerlogik arbeiten kann und ein dritter Entwickler auf die Geschäftslogik im Modell konzentrieren kann.</span><span class="sxs-lookup"><span data-stu-id="7d536-136">For instance, one developer can work on the view, a second developer can work on the controller logic, and a third developer can focus on the business logic in the model.</span></span>

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="7d536-137">Ziele</span><span class="sxs-lookup"><span data-stu-id="7d536-137">Objectives</span></span>

<span data-ttu-id="7d536-138">In dieser praktischen Übungseinheit erfahren Sie, wie Sie:</span><span class="sxs-lookup"><span data-stu-id="7d536-138">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="7d536-139">Erstellen einer ASP.NET MVC-Anwendung von Grund auf Grundlage des Lernprogramms Music Store-Anwendung</span><span class="sxs-lookup"><span data-stu-id="7d536-139">Create an ASP.NET MVC application from scratch based on the Music Store Application tutorial</span></span>
- <span data-ttu-id="7d536-140">Hinzufügen von Domänencontroller zum Behandeln von URLs zur Startseite des Standorts und seine wichtigsten Funktionen durchsuchen</span><span class="sxs-lookup"><span data-stu-id="7d536-140">Add Controllers to handle URLs to the Home page of the site and for browsing its main functionality</span></span>
- <span data-ttu-id="7d536-141">Fügen Sie eine Ansicht, um den Inhalt angezeigt, zusammen mit den Stil anzupassen</span><span class="sxs-lookup"><span data-stu-id="7d536-141">Add a View to customize the content displayed along with its style</span></span>
- <span data-ttu-id="7d536-142">Fügen Sie Modellklassen zum enthalten und Verwalten von Daten und Domäne Logik hinzu</span><span class="sxs-lookup"><span data-stu-id="7d536-142">Add Model classes to contain and manage data and domain logic</span></span>
- <span data-ttu-id="7d536-143">Verwenden Sie ViewModel-Muster, um Informationen aus Controlleraktionen an die Ansichtsvorlagen zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="7d536-143">Use View Model pattern to pass information from controller actions to the view templates</span></span>
- <span data-ttu-id="7d536-144">Untersuchen Sie die neue ASP.NET MVC 4-Vorlage für Internetanwendungen</span><span class="sxs-lookup"><span data-stu-id="7d536-144">Explore the ASP.NET MVC 4 new template for internet applications</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="7d536-145">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="7d536-145">Prerequisites</span></span>

<span data-ttu-id="7d536-146">Sie benötigen zum Abschließen dieser testumgebung die folgenden Elemente:</span><span class="sxs-lookup"><span data-stu-id="7d536-146">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="7d536-147">[Visual Studio 2012 Express für Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (Lesen [Anhang A](#AppendixA) Anleitungen zur Installation)</span><span class="sxs-lookup"><span data-stu-id="7d536-147">[Visual Studio 2012 Express for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (read [Appendix A](#AppendixA) for instructions on how to install it)</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="7d536-148">Setup</span><span class="sxs-lookup"><span data-stu-id="7d536-148">Setup</span></span>

<span data-ttu-id="7d536-149">**Installieren von Codeausschnitten**</span><span class="sxs-lookup"><span data-stu-id="7d536-149">**Installing Code Snippets**</span></span>

<span data-ttu-id="7d536-150">Der Einfachheit halber gilt Großteil des Codes, den Sie entlang dieser Übung verwalten als Visual Studio-Codeausschnitte verfügbar.</span><span class="sxs-lookup"><span data-stu-id="7d536-150">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="7d536-151">So installieren Sie die Codeausschnitte ausführen **.\Source\Setup\CodeSnippets.vsi** Datei.</span><span class="sxs-lookup"><span data-stu-id="7d536-151">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="7d536-152">Wenn Sie nicht mit den Visual Studio-Codeausschnitte und zu erfahren, wie deren Verwendung vertraut sind, Sie können finden Sie im Anhang in diesem Dokument &quot; [Anhang "c:" mithilfe von Code Snippets](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="7d536-152">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="7d536-153">Übungen</span><span class="sxs-lookup"><span data-stu-id="7d536-153">Exercises</span></span>

<span data-ttu-id="7d536-154">Diese praktische Übungseinheit wird durch den folgenden Übungen umfasst:</span><span class="sxs-lookup"><span data-stu-id="7d536-154">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="7d536-155">Übung 1: Erstellen von ASP.NET MVC-Webanwendungsprojekt MusicStore</span><span class="sxs-lookup"><span data-stu-id="7d536-155">Exercise 1: Creating MusicStore ASP.NET MVC Web Application Project</span></span>](#Exercise1)
2. [<span data-ttu-id="7d536-156">Übung 2: Erstellen eines Domänencontrollers</span><span class="sxs-lookup"><span data-stu-id="7d536-156">Exercise 2: Creating a Controller</span></span>](#Exercise2)
3. [<span data-ttu-id="7d536-157">Übung 3: Übergeben von Parametern zu einem Domänencontroller</span><span class="sxs-lookup"><span data-stu-id="7d536-157">Exercise 3: Passing parameters to a Controller</span></span>](#Exercise3)
4. [<span data-ttu-id="7d536-158">Übung 4: Erstellen einer Ansicht</span><span class="sxs-lookup"><span data-stu-id="7d536-158">Exercise 4: Creating a View</span></span>](#Exercise4)
5. [<span data-ttu-id="7d536-159">Übung 5: Erstellen eines Modells anzeigen</span><span class="sxs-lookup"><span data-stu-id="7d536-159">Exercise 5: Creating a View Model</span></span>](#Exercise5)
6. [<span data-ttu-id="7d536-160">Übung 6: Verwenden von Parametern in der Ansicht</span><span class="sxs-lookup"><span data-stu-id="7d536-160">Exercise 6: Using parameters in View</span></span>](#Exercise6)
7. [<span data-ttu-id="7d536-161">Übung 7: Lap um neue ASP.NET MVC 4-Projektvorlage</span><span class="sxs-lookup"><span data-stu-id="7d536-161">Exercise 7: A lap around ASP.NET MVC 4 New Template</span></span>](#Exercise7)

> [!NOTE]
> <span data-ttu-id="7d536-162">Jede Übung beiliegen ein **End** Ordner, der sich so ergebende Lösung, die Sie nach Abschluss der Übung abrufen soll.</span><span class="sxs-lookup"><span data-stu-id="7d536-162">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="7d536-163">Sie können diese Lösung als Leitfaden verwenden, ggf. zusätzliche Hilfe bei der Übungen durcharbeiten.</span><span class="sxs-lookup"><span data-stu-id="7d536-163">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="7d536-164">Geschätzte Zeit zum Abschließen dieser testumgebung: **60 Minuten**.</span><span class="sxs-lookup"><span data-stu-id="7d536-164">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_MusicStore_ASPNET_MVC_Web_Application_Project"></a>
### <a name="exercise-1-creating-musicstore-aspnet-mvc-web-application-project"></a><span data-ttu-id="7d536-165">Übung 1: Erstellen von ASP.NET MVC-Webanwendungsprojekt MusicStore</span><span class="sxs-lookup"><span data-stu-id="7d536-165">Exercise 1: Creating MusicStore ASP.NET MVC Web Application Project</span></span>

<span data-ttu-id="7d536-166">In dieser Übung erfahren Sie, wie eine ASP.NET MVC-Anwendung in Visual Studio 2012 Express für Web sowie seiner Organisation Hauptordner zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="7d536-166">In this exercise, you will learn how to create an ASP.NET MVC application in Visual Studio 2012 Express for Web as well as its main folder organization.</span></span> <span data-ttu-id="7d536-167">Darüber hinaus erfahren Sie, wie fügen einen neuen Domänencontroller, und stellen sie eine einfache Zeichenfolge auf der Startseite der Anwendung angezeigt.</span><span class="sxs-lookup"><span data-stu-id="7d536-167">Additionally, you will learn how to add a new Controller and make it display a simple string in the application's home page.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_ASPNET_MVC_Web_Application_Project"></a>
#### <a name="task-1---creating-the-aspnet-mvc-web-application-project"></a><span data-ttu-id="7d536-168">Aufgabe 1: Erstellen der ASP.NET MVC-Webanwendungsprojekts</span><span class="sxs-lookup"><span data-stu-id="7d536-168">Task 1 - Creating the ASP.NET MVC Web Application Project</span></span>

1. <span data-ttu-id="7d536-169">In dieser Aufgabe erstellen Sie eine leere ASP.NET MVC-Anwendungsprojekt mit der Visual Studio für MVC-Vorlage.</span><span class="sxs-lookup"><span data-stu-id="7d536-169">In this task, you will create an empty ASP.NET MVC application project using the MVC Visual Studio template.</span></span> <span data-ttu-id="7d536-170">Starten Sie **Visual Studio Express für Web**.</span><span class="sxs-lookup"><span data-stu-id="7d536-170">Start **VS Express for Web**.</span></span>
2. <span data-ttu-id="7d536-171">Klicken Sie im Menü **Datei** auf **Neues Projekt**.</span><span class="sxs-lookup"><span data-stu-id="7d536-171">On the **File** menu, click **New Project**.</span></span>
3. <span data-ttu-id="7d536-172">In der **neues Projekt** aktivieren Sie im Dialogfeld die **ASP.NET MVC 4-Webanwendung** Projekttyp, befindet sich im **Visual c#** **Web** Vorlage Liste.</span><span class="sxs-lookup"><span data-stu-id="7d536-172">In the **New Project** dialog box select the **ASP.NET MVC 4 Web Application** project type, located under **Visual C#,** **Web** template list.</span></span>
4. <span data-ttu-id="7d536-173">Ändern der **Namen** auf *MvcMusicStore*.</span><span class="sxs-lookup"><span data-stu-id="7d536-173">Change the **Name** to *MvcMusicStore*.</span></span>
5. <span data-ttu-id="7d536-174">Legen Sie den Speicherort für die Projektmappe in einem neuen **beginnen** Ordner in dieser Übung Quellordner, z. B. **[HOL-den-PATH] \Source\Ex01-CreatingMusicStoreProject\Begin**.</span><span class="sxs-lookup"><span data-stu-id="7d536-174">Set the location of the solution inside a new **Begin** folder in this Exercise's Source folder, for example **[YOUR-HOL-PATH]\Source\Ex01-CreatingMusicStoreProject\Begin**.</span></span> <span data-ttu-id="7d536-175">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="7d536-175">Click **OK**.</span></span>

    <span data-ttu-id="7d536-176">![Dialogfeld Neues Projekt erstellen](aspnet-mvc-4-fundamentals/_static/image2.png "Dialogfeld Neues Projekt erstellen")</span><span class="sxs-lookup"><span data-stu-id="7d536-176">![Create New Project Dialog Box](aspnet-mvc-4-fundamentals/_static/image2.png "Create New Project Dialog Box")</span></span>

    <span data-ttu-id="7d536-177">*Dialogfeld Neues Projekt erstellen*</span><span class="sxs-lookup"><span data-stu-id="7d536-177">*Create New Project Dialog Box*</span></span>
6. <span data-ttu-id="7d536-178">In der **neues ASP.NET MVC 4-Projekt** aktivieren Sie im Dialogfeld die **grundlegende** Vorlage und stellen Sie sicher, dass die **Ansichtsmodul** ausgewählt ist **Razor**.</span><span class="sxs-lookup"><span data-stu-id="7d536-178">In the **New ASP.NET MVC 4 Project** dialog box select the **Basic** template and make sure that the **View engine** selected is **Razor**.</span></span> <span data-ttu-id="7d536-179">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="7d536-179">Click **OK**.</span></span>

    <span data-ttu-id="7d536-180">![Neues ASP.NET MVC 4-Projekt (Dialogfeld)](aspnet-mvc-4-fundamentals/_static/image3.png "neue ASP.NET MVC 4-Projekt (Dialogfeld)")</span><span class="sxs-lookup"><span data-stu-id="7d536-180">![New ASP.NET MVC 4 Project Dialog Box](aspnet-mvc-4-fundamentals/_static/image3.png "New ASP.NET MVC 4 Project Dialog Box")</span></span>

    <span data-ttu-id="7d536-181">*Neues ASP.NET MVC 4-Projekt (Dialogfeld)*</span><span class="sxs-lookup"><span data-stu-id="7d536-181">*New ASP.NET MVC 4 Project Dialog Box*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Exploring_the_Solution_Structure"></a>
#### <a name="task-2---exploring-the-solution-structure"></a><span data-ttu-id="7d536-182">Aufgabe 2: Untersuchen der Projektmappenstruktur</span><span class="sxs-lookup"><span data-stu-id="7d536-182">Task 2 - Exploring the Solution Structure</span></span>

<span data-ttu-id="7d536-183">ASP.NET MVC-Framework enthält eine Visual Studio-Projektvorlage, mit die Sie die Erstellung von Webanwendungen ermöglichen das MVC-Muster unterstützen kann.</span><span class="sxs-lookup"><span data-stu-id="7d536-183">The ASP.NET MVC framework includes a Visual Studio project template that helps you create Web applications supporting the MVC pattern.</span></span> <span data-ttu-id="7d536-184">Diese Vorlage erstellt eine neue ASP.NET MVC-Webanwendung mit der erforderlichen Ordnern, Elementvorlagen und Konfigurationsdatei Einträge.</span><span class="sxs-lookup"><span data-stu-id="7d536-184">This template creates a new ASP.NET MVC Web application with the required folders, item templates, and configuration-file entries.</span></span>

<span data-ttu-id="7d536-185">In dieser Aufgabe untersuchen Sie die Lösungsstruktur werden die Elemente erläutert, die beteiligt sind und ihre Beziehungen.</span><span class="sxs-lookup"><span data-stu-id="7d536-185">In this task, you will examine the solution structure to understand the elements that are involved and their relationships.</span></span> <span data-ttu-id="7d536-186">Die folgenden Ordner sind in der ASP.NET MVC-Anwendung enthalten, da das ASP.NET MVC-Framework standardmäßig verwendet eine &quot;Konvention über Konfiguration&quot; Ansatz und lässt einige Standard-Annahmen basierend auf den Ordner benennen Konventionen.</span><span class="sxs-lookup"><span data-stu-id="7d536-186">The following folders are included in all the ASP.NET MVC application because the ASP.NET MVC framework by default uses a &quot;convention over configuration&quot; approach, and makes some default assumptions based on folder naming conventions.</span></span>

1. <span data-ttu-id="7d536-187">Nachdem das Projekt erstellt wurde, überprüfen Sie die Ordnerstruktur, die im Projektmappen-Explorer auf der rechten Seite erstellt wurde:</span><span class="sxs-lookup"><span data-stu-id="7d536-187">Once the project is created, review the folder structure that has been created in the Solution Explorer on the right side:</span></span>

    <span data-ttu-id="7d536-188">![ASP.NET MVC-Ordnerstruktur in Projektmappen-Explorer](aspnet-mvc-4-fundamentals/_static/image4.png "ASP.NET MVC-Ordnerstruktur in Projektmappen-Explorer")</span><span class="sxs-lookup"><span data-stu-id="7d536-188">![ASP.NET MVC Folder structure in Solution Explorer](aspnet-mvc-4-fundamentals/_static/image4.png "ASP.NET MVC Folder structure in Solution Explorer")</span></span>

    <span data-ttu-id="7d536-189">*ASP.NET MVC-Ordnerstruktur in Projektmappen-Explorer*</span><span class="sxs-lookup"><span data-stu-id="7d536-189">*ASP.NET MVC Folder structure in Solution Explorer*</span></span>

   1. <span data-ttu-id="7d536-190">**Controller**.</span><span class="sxs-lookup"><span data-stu-id="7d536-190">**Controllers**.</span></span> <span data-ttu-id="7d536-191">Dieser Ordner enthält die Controllerklassen.</span><span class="sxs-lookup"><span data-stu-id="7d536-191">This folder will contain the controller classes.</span></span> <span data-ttu-id="7d536-192">In einer MVC-basierten Anwendung sind Domänencontroller verantwortlich für die Behandlung von End-Benutzerinteraktion, bearbeiten das Modell und letztendlich eine Ansicht zum Rendern der Benutzeroberflächenautomatisierungs auswählen.</span><span class="sxs-lookup"><span data-stu-id="7d536-192">In an MVC based application, controllers are responsible for handling end user interaction, manipulating the model, and ultimately choosing a view to render the UI.</span></span>

       > [!NOTE]
       > <span data-ttu-id="7d536-193">Das MVC-Framework erfordert die Namen aller Controller endet nicht mit &quot;Controller&quot;– z. B. HomeController, LoginController oder ProductController.</span><span class="sxs-lookup"><span data-stu-id="7d536-193">The MVC framework requires the names of all controllers to end with &quot;Controller&quot;-for example, HomeController, LoginController, or ProductController.</span></span>
   2. <span data-ttu-id="7d536-194">**Modelle**.</span><span class="sxs-lookup"><span data-stu-id="7d536-194">**Models**.</span></span> <span data-ttu-id="7d536-195">Dieser Ordner wird für Klassen bereitgestellt, die das Anwendungsmodell für die MVC-Webanwendung darstellen.</span><span class="sxs-lookup"><span data-stu-id="7d536-195">This folder is provided for classes that represent the application model for the MVC Web application.</span></span> <span data-ttu-id="7d536-196">Dies schließt in der Regel Code, der Objekte und die Logik für die Interaktion mit dem Datenspeicher definiert.</span><span class="sxs-lookup"><span data-stu-id="7d536-196">This usually includes code that defines objects and the logic for interacting with the data store.</span></span> <span data-ttu-id="7d536-197">In der Regel werden die eigentlichen Modellobjekte in separaten Klassenbibliotheken.</span><span class="sxs-lookup"><span data-stu-id="7d536-197">Typically, the actual model objects will be in separate class libraries.</span></span> <span data-ttu-id="7d536-198">Wenn Sie eine neue Anwendung erstellen, können Sie allerdings umfassen Klassen und klicken Sie dann im Entwicklungszyklus in separate Klassenbibliotheken zu einem späteren Zeitpunkt verschieben.</span><span class="sxs-lookup"><span data-stu-id="7d536-198">However, when you create a new application, you might include classes and then move them into separate class libraries at a later point in the development cycle.</span></span>
   3. <span data-ttu-id="7d536-199">**Sichten**.</span><span class="sxs-lookup"><span data-stu-id="7d536-199">**Views**.</span></span> <span data-ttu-id="7d536-200">Dieser Ordner ist der empfohlene Speicherort für Ansichten, die Komponenten, die für die Anzeige der Benutzeroberfläche der Anwendung verantwortlich.</span><span class="sxs-lookup"><span data-stu-id="7d536-200">This folder is the recommended location for views, the components responsible for displaying the application's user interface.</span></span> <span data-ttu-id="7d536-201">Ansichten verwenden aspx, .ascx, cshtml und Master-Dateien zusätzlich zu anderen Dateien, die zum Rendern von Ansichten verknüpft werden.</span><span class="sxs-lookup"><span data-stu-id="7d536-201">Views use .aspx, .ascx, .cshtml and .master files, in addition to any other files that are related to rendering views.</span></span> <span data-ttu-id="7d536-202">Ordner Views enthält einen Ordner für jeden Controller. der Ordner den Namen mit dem Controller-Namenspräfix.</span><span class="sxs-lookup"><span data-stu-id="7d536-202">Views folder contains a folder for each controller; the folder is named with the controller-name prefix.</span></span> <span data-ttu-id="7d536-203">Angenommen, Sie haben einen Controller mit dem Namen **HomeController**, der Ordner Views enthält einen Ordner mit der Bezeichnung Home.</span><span class="sxs-lookup"><span data-stu-id="7d536-203">For example, if you have a controller named **HomeController**, the Views folder will contain a folder named Home.</span></span> <span data-ttu-id="7d536-204">Wird standardmäßig beim Laden von ASP.NET MVC-Framework einer Sicht sucht es nach einer ASPX-Datei mit dem angeforderten Namen im Ordner "Views\controllerName" (**Ansichten [ControllerName] [Aktion] aspx**) oder (**Ansichten [ControllerName] [Aktion] cshtml**) für die Razor-Ansichten.</span><span class="sxs-lookup"><span data-stu-id="7d536-204">By default, when the ASP.NET MVC framework loads a view, it looks for an .aspx file with the requested view name in the Views\controllerName folder (**Views[ControllerName][Action].aspx**) or (**Views[ControllerName][Action].cshtml**) for Razor Views.</span></span>

      > [!NOTE]
      > <span data-ttu-id="7d536-205">Zusätzlich zu den zuvor aufgeführten Ordnern verwendet eine MVC-Webanwendung die **"Global.asax"** standardmäßig Datei globale URL-routing und verwendet die **"Web.config"** Datei zum Konfigurieren der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="7d536-205">In addition to the folders listed previously, an MVC Web application uses the **Global.asax** file to set global URL routing defaults, and it uses the **Web.config** file to configure the application.</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Adding_a_HomeController"></a>
#### <a name="task-3---adding-a-homecontroller"></a><span data-ttu-id="7d536-206">Aufgabe 3: Hinzufügen einer HomeController</span><span class="sxs-lookup"><span data-stu-id="7d536-206">Task 3 - Adding a HomeController</span></span>

<span data-ttu-id="7d536-207">In ASP.NET-Anwendungen, die das MVC-Framework nicht verwenden, ist eine Benutzerinteraktion Seiten und herum durch das Auslösen und Behandeln von Ereignissen aus diesen Seiten angeordnet.</span><span class="sxs-lookup"><span data-stu-id="7d536-207">In ASP.NET applications that do not use the MVC framework, user interaction is organized around pages, and around raising and handling events from those pages.</span></span> <span data-ttu-id="7d536-208">Im Gegensatz dazu ist Benutzerinteraktion in ASP.NET-MVC-Anwendungen in Controllern und deren Aktionsmethoden organisiert.</span><span class="sxs-lookup"><span data-stu-id="7d536-208">In contrast, user interaction with ASP.NET MVC applications is organized around controllers and their action methods.</span></span>

<span data-ttu-id="7d536-209">ASP.NET MVC-Framework ordnet andererseits, URLs Klassen, die als Domänencontroller bezeichnet werden.</span><span class="sxs-lookup"><span data-stu-id="7d536-209">On the other hand, ASP.NET MVC framework maps URLs to classes that are referred to as controllers.</span></span> <span data-ttu-id="7d536-210">Controller verarbeiten eingehende Anforderungen, behandeln Benutzereingaben und Interaktionen, führen Sie die entsprechende Anwendungslogik und die Antwort zurück an den Client senden zu bestimmen (HTML-Seite anzeigen, eine Datei herunterladen, Umleiten zu einer anderen URL usw.).</span><span class="sxs-lookup"><span data-stu-id="7d536-210">Controllers process incoming requests, handle user input and interactions, execute appropriate application logic and determine the response to send back to the client (display HTML, download a file, redirect to a different URL, etc.).</span></span> <span data-ttu-id="7d536-211">Im Fall von HTML anzeigen, ruft eine Controllerklasse in der Regel eine separate Ansichtskomponente zum Generieren von HTML-Markup für die Anforderung.</span><span class="sxs-lookup"><span data-stu-id="7d536-211">In the case of displaying HTML, a controller class typically calls a separate view component to generate the HTML markup for the request.</span></span> <span data-ttu-id="7d536-212">In einer MVC-Anwendung zeigt die Ansicht nur Informationen an. Benutzereingaben und -interaktionen werden vom Controller verarbeitet und beantwortet.</span><span class="sxs-lookup"><span data-stu-id="7d536-212">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span>

<span data-ttu-id="7d536-213">In dieser Aufgabe fügen Sie eine Controllerklasse, die URLs zur Startseite des Standorts Music Store behandelt.</span><span class="sxs-lookup"><span data-stu-id="7d536-213">In this task, you will add a Controller class that will handle URLs to the Home page of the Music Store site.</span></span>

1. <span data-ttu-id="7d536-214">Mit der rechten Maustaste **Controller** Ordner im Projektmappen-Explorer wählen **hinzufügen** und dann **Controller** Befehl:</span><span class="sxs-lookup"><span data-stu-id="7d536-214">Right-click **Controllers** folder within the Solution Explorer, select **Add** and then **Controller** command:</span></span>

    <span data-ttu-id="7d536-215">![Hinzufügen eines Befehls Controller](aspnet-mvc-4-fundamentals/_static/image5.png "fügen Sie einen Controllerbefehl")</span><span class="sxs-lookup"><span data-stu-id="7d536-215">![Add a Controller Command](aspnet-mvc-4-fundamentals/_static/image5.png "Add a Controller Command")</span></span>

    <span data-ttu-id="7d536-216">*Controllerbefehl hinzufügen*</span><span class="sxs-lookup"><span data-stu-id="7d536-216">*Add Controller Command*</span></span>
2. <span data-ttu-id="7d536-217">Die **Controller hinzufügen** Dialogfeld wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="7d536-217">The **Add Controller** dialog appears.</span></span> <span data-ttu-id="7d536-218">Nennen Sie den Controller *HomeController* , und drücken Sie **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="7d536-218">Name the controller *HomeController* and press **Add**.</span></span>

    <span data-ttu-id="7d536-219">![Controller-Dialogfeld "hinzufügen"](aspnet-mvc-4-fundamentals/_static/image6.png "Controller-Dialogfeld \"hinzufügen\"")</span><span class="sxs-lookup"><span data-stu-id="7d536-219">![Add Controller Dialog](aspnet-mvc-4-fundamentals/_static/image6.png "Add Controller Dialog")</span></span>

    <span data-ttu-id="7d536-220">*Controller-Dialogfeld "hinzufügen"*</span><span class="sxs-lookup"><span data-stu-id="7d536-220">*Add Controller Dialog*</span></span>
3. <span data-ttu-id="7d536-221">Die Datei **HomeController.cs** wird erstellt, der **Controller** Ordner.</span><span class="sxs-lookup"><span data-stu-id="7d536-221">The file **HomeController.cs** is created in the **Controllers** folder.</span></span> <span data-ttu-id="7d536-222">Um die **HomeController** geben eine Zeichenfolge zurück, dessen Index-Aktion bei, ersetzen Sie die **Index** -Methode durch folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="7d536-222">In order to have the **HomeController** return a string on its Index action, replace the **Index** method with the following code:</span></span>

    <span data-ttu-id="7d536-223">(Codeausschnitt - *Grundlagen von ASP.NET MVC 4 - Ex1 HomeController Index*)</span><span class="sxs-lookup"><span data-stu-id="7d536-223">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex1 HomeController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample1.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="7d536-224">Aufgabe 4: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="7d536-224">Task 4 - Running the Application</span></span>

<span data-ttu-id="7d536-225">In dieser Aufgabe wird die Anwendung in einem Webbrowser testen werden.</span><span class="sxs-lookup"><span data-stu-id="7d536-225">In this task, you will try out the Application in a web browser.</span></span>

1. <span data-ttu-id="7d536-226">Drücken Sie **F5** um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="7d536-226">Press **F5** to run the Application.</span></span> <span data-ttu-id="7d536-227">Kompilieren des Projekts, und der lokalen IIS-Webserver startet.</span><span class="sxs-lookup"><span data-stu-id="7d536-227">The project is compiled and the local IIS Web Server starts.</span></span> <span data-ttu-id="7d536-228">Lokalen IIS-Webserver wird einen Webbrowser auf die URL des Webservers automatisch geöffnet.</span><span class="sxs-lookup"><span data-stu-id="7d536-228">The local IIS Web Server will automatically open a web browser pointing to the URL of the Web server.</span></span>

    <span data-ttu-id="7d536-229">![In einem Webbrowser ausgeführte Anwendung](aspnet-mvc-4-fundamentals/_static/image7.png "Anwendung in einem Webbrowser ausgeführt wird")</span><span class="sxs-lookup"><span data-stu-id="7d536-229">![Application running in a web browser](aspnet-mvc-4-fundamentals/_static/image7.png "Application running in a web browser")</span></span>

    <span data-ttu-id="7d536-230">*Anwendung in einem Webbrowser ausgeführt wird*</span><span class="sxs-lookup"><span data-stu-id="7d536-230">*Application running in a web browser*</span></span>

    > [!NOTE]
    > <span data-ttu-id="7d536-231">Dem lokalen IIS-Webserver wird eine zufällige freie Portnummer an der Website ausführen.</span><span class="sxs-lookup"><span data-stu-id="7d536-231">The local IIS Web Server will run the website on a random free port number.</span></span> <span data-ttu-id="7d536-232">In der obigen Abbildung dem Standort ausgeführt wird, am `http://localhost:50103/`, sodass Port 50103 verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="7d536-232">In the figure above, the site is running at `http://localhost:50103/`, so it's using port 50103.</span></span> <span data-ttu-id="7d536-233">Die Portnummer kann variieren.</span><span class="sxs-lookup"><span data-stu-id="7d536-233">Your port number may vary.</span></span>
2. <span data-ttu-id="7d536-234">Schließen Sie den Browser.</span><span class="sxs-lookup"><span data-stu-id="7d536-234">Close the browser.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Controller"></a>
### <a name="exercise-2-creating-a-controller"></a><span data-ttu-id="7d536-235">Übung 2: Erstellen eines Domänencontrollers</span><span class="sxs-lookup"><span data-stu-id="7d536-235">Exercise 2: Creating a Controller</span></span>

<span data-ttu-id="7d536-236">In dieser Übung erfahren Sie, wie beim Aktualisieren des Controllers, um eine einfache Funktionalität der Music Store-Anwendung zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="7d536-236">In this exercise, you will learn how to update the controller to implement simple functionality of the Music Store application.</span></span> <span data-ttu-id="7d536-237">Dieser Controller definieren Aktionsmethoden, um die folgenden spezifischen Anforderungen zu verarbeiten:</span><span class="sxs-lookup"><span data-stu-id="7d536-237">That controller will define action methods to handle each of the following specific requests:</span></span>

- <span data-ttu-id="7d536-238">Eine Auflistung-Seite mit der Musik Genres im Music Store</span><span class="sxs-lookup"><span data-stu-id="7d536-238">A listing page of the music genres in the Music Store</span></span>
- <span data-ttu-id="7d536-239">Seite "Durchsuchen", die alle Musikalben für einen bestimmten "Genre" aufgelistet sind</span><span class="sxs-lookup"><span data-stu-id="7d536-239">A browse page that lists all of the music albums for a particular genre</span></span>
- <span data-ttu-id="7d536-240">Seite "Details", das Informationen zu einem bestimmten Musikalbum anzeigt</span><span class="sxs-lookup"><span data-stu-id="7d536-240">A details page that shows information about a specific music album</span></span>

<span data-ttu-id="7d536-241">Für den Rahmen dieser Übung gibt diese Aktionen nun einfach eine Zeichenfolge zurück.</span><span class="sxs-lookup"><span data-stu-id="7d536-241">For the scope of this exercise, those actions will simply return a string by now.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Adding_a_New_StoreController_Class"></a>
#### <a name="task-1---adding-a-new-storecontroller-class"></a><span data-ttu-id="7d536-242">Aufgabe 1: Hinzufügen einer neuen StoreController-Klasse</span><span class="sxs-lookup"><span data-stu-id="7d536-242">Task 1 - Adding a New StoreController Class</span></span>

<span data-ttu-id="7d536-243">In dieser Aufgabe fügen Sie einen neuen Domänencontroller.</span><span class="sxs-lookup"><span data-stu-id="7d536-243">In this task, you will add a new Controller.</span></span>

1. <span data-ttu-id="7d536-244">Wenn nicht bereits geöffnet ist, starten Sie **Visual Studio Express für Web 2012**.</span><span class="sxs-lookup"><span data-stu-id="7d536-244">If not already open, start **VS Express for Web 2012**.</span></span>
2. <span data-ttu-id="7d536-245">In der **Datei** Menü wählen **geöffneten Projekt**.</span><span class="sxs-lookup"><span data-stu-id="7d536-245">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="7d536-246">Navigieren Sie zu, klicken Sie im Dialogfeld "Projekt öffnen" **Source\Ex02 CreatingAController\Begin**Option **Begin.sln** , und klicken Sie auf **öffnen**.</span><span class="sxs-lookup"><span data-stu-id="7d536-246">In the Open Project dialog, browse to **Source\Ex02-CreatingAController\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="7d536-247">Alternativ können Sie mit der Lösung fortfahren, dass Sie nach Abschluss der vorherigen Übung erhalten haben.</span><span class="sxs-lookup"><span data-stu-id="7d536-247">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="7d536-248">Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung müssen Sie einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren.</span><span class="sxs-lookup"><span data-stu-id="7d536-248">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="7d536-249">Klicken Sie hierzu auf die **Projekt** Menü **NuGet-Pakete verwalten**.</span><span class="sxs-lookup"><span data-stu-id="7d536-249">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="7d536-250">In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.</span><span class="sxs-lookup"><span data-stu-id="7d536-250">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="7d536-251">Schließlich erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.</span><span class="sxs-lookup"><span data-stu-id="7d536-251">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="7d536-252">Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken, die sich in Ihrem Projekt liefern Verringern der Projektgröße.</span><span class="sxs-lookup"><span data-stu-id="7d536-252">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="7d536-253">Mit NuGet-Powertools werden durch Angabe der Paketversionen in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, wenn, das Sie das Projekt ausführen, können.</span><span class="sxs-lookup"><span data-stu-id="7d536-253">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="7d536-254">Deshalb wird müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit.</span><span class="sxs-lookup"><span data-stu-id="7d536-254">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="7d536-255">Hinzufügen eines neuen Controllers.</span><span class="sxs-lookup"><span data-stu-id="7d536-255">Add the new controller.</span></span> <span data-ttu-id="7d536-256">Zu diesem Zweck Maustaste die **Controller** Ordner im Projektmappen-Explorer, wählen Sie **hinzufügen** und dann die **Controller** Befehl.</span><span class="sxs-lookup"><span data-stu-id="7d536-256">To do this, right-click the **Controllers** folder within the Solution Explorer, select **Add** and then the **Controller** command.</span></span> <span data-ttu-id="7d536-257">Ändern der **Controllernamen** auf *StoreController*, und klicken Sie auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="7d536-257">Change the **Controller Name** to *StoreController*, and click **Add**.</span></span>

    <span data-ttu-id="7d536-258">![Controller-Dialogfeld "hinzufügen"](aspnet-mvc-4-fundamentals/_static/image8.png "Controller-Dialogfeld \"hinzufügen\"")</span><span class="sxs-lookup"><span data-stu-id="7d536-258">![Add Controller Dialog](aspnet-mvc-4-fundamentals/_static/image8.png "Add Controller Dialog")</span></span>

    <span data-ttu-id="7d536-259">*Controller-Dialogfeld "hinzufügen"*</span><span class="sxs-lookup"><span data-stu-id="7d536-259">*Add Controller Dialog*</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_Modifying_the_StoreControllers_Actions"></a>
#### <a name="task-2---modifying-the-storecontrollers-actions"></a><span data-ttu-id="7d536-260">Aufgabe 2: Ändern der StoreController Aktionen</span><span class="sxs-lookup"><span data-stu-id="7d536-260">Task 2 - Modifying the StoreController's Actions</span></span>

<span data-ttu-id="7d536-261">In dieser Aufgabe ändern Sie die Controller-Methoden, die aufgerufen werden **Aktionen**.</span><span class="sxs-lookup"><span data-stu-id="7d536-261">In this task, you will modify the Controller methods that are called **actions**.</span></span> <span data-ttu-id="7d536-262">Aktionen sind verantwortlich für die Behandlung von URL-Anforderungen und bestimmen den Inhalt, der wieder an den Browser oder Benutzer, der die URL aufgerufen gesendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="7d536-262">Actions are responsible for handling URL requests and determining the content that should be sent back to the browser or user that invoked the URL.</span></span>

1. <span data-ttu-id="7d536-263">Die **StoreController** -Klasse verfügt bereits über ein **Index** Methode.</span><span class="sxs-lookup"><span data-stu-id="7d536-263">The **StoreController** class already has an **Index** method.</span></span> <span data-ttu-id="7d536-264">Sie verwenden es später in dieser Übung die Seite zu implementieren, die alle Genres von Music Store aufgelistet.</span><span class="sxs-lookup"><span data-stu-id="7d536-264">You will use it later in this Lab to implement the page that lists all genres of the music store.</span></span> <span data-ttu-id="7d536-265">Ersetzen Sie jetzt einfach die **Index** Methode durch den folgenden Code, der eine Zeichenfolge zurückgibt, &quot;Hello aus Store.Index()&quot;:</span><span class="sxs-lookup"><span data-stu-id="7d536-265">For now, just replace the **Index** method with the following code that returns a string &quot;Hello from Store.Index()&quot;:</span></span>

    <span data-ttu-id="7d536-266">(Codeausschnitt - *Grundlagen von ASP.NET MVC 4 - Ex2 StoreController Index*)</span><span class="sxs-lookup"><span data-stu-id="7d536-266">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex2 StoreController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample2.cs)]
2. <span data-ttu-id="7d536-267">Hinzufügen **Durchsuchen** und **Details** Methoden.</span><span class="sxs-lookup"><span data-stu-id="7d536-267">Add **Browse** and **Details** methods.</span></span> <span data-ttu-id="7d536-268">Zu diesem Zweck fügen Sie den folgenden Code aus, um die **StoreController**:</span><span class="sxs-lookup"><span data-stu-id="7d536-268">To do this, add the following code to the **StoreController**:</span></span>

    <span data-ttu-id="7d536-269">(Codeausschnitt - *Grundlagen von ASP.NET MVC 4 - Ex2 StoreController BrowseAndDetails*)</span><span class="sxs-lookup"><span data-stu-id="7d536-269">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex2 StoreController BrowseAndDetails*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample3.cs)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="7d536-270">Aufgabe 3: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="7d536-270">Task 3 - Running the Application</span></span>

<span data-ttu-id="7d536-271">In dieser Aufgabe wird die Anwendung in einem Webbrowser testen werden.</span><span class="sxs-lookup"><span data-stu-id="7d536-271">In this task, you will try out the Application in a web browser.</span></span>

1. <span data-ttu-id="7d536-272">Drücken Sie **F5** um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="7d536-272">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="7d536-273">Das Projekt gestartet wird, der **Home** Seite.</span><span class="sxs-lookup"><span data-stu-id="7d536-273">The project starts in the **Home** page.</span></span> <span data-ttu-id="7d536-274">Ändern Sie die URL, um zu überprüfen, ob jede Aktion-Implementierung.</span><span class="sxs-lookup"><span data-stu-id="7d536-274">Change the URL to verify each action's implementation.</span></span>

    1. <span data-ttu-id="7d536-275">**/ Speichern**.</span><span class="sxs-lookup"><span data-stu-id="7d536-275">**/Store**.</span></span> <span data-ttu-id="7d536-276">Sehen Sie  **&quot;Hello aus Store.Index()&quot;**.</span><span class="sxs-lookup"><span data-stu-id="7d536-276">You will see **&quot;Hello from Store.Index()&quot;**.</span></span>
    2. <span data-ttu-id="7d536-277">**/ Store/Durchsuchen**.</span><span class="sxs-lookup"><span data-stu-id="7d536-277">**/Store/Browse**.</span></span> <span data-ttu-id="7d536-278">Sehen Sie  **&quot;Hello aus Store.Browse()&quot;**.</span><span class="sxs-lookup"><span data-stu-id="7d536-278">You will see **&quot;Hello from Store.Browse()&quot;**.</span></span>
    3. <span data-ttu-id="7d536-279">**/ Store/Detail-**.</span><span class="sxs-lookup"><span data-stu-id="7d536-279">**/Store/Details**.</span></span> <span data-ttu-id="7d536-280">Sehen Sie  **&quot;Hello aus Store.Details()&quot;**.</span><span class="sxs-lookup"><span data-stu-id="7d536-280">You will see **&quot;Hello from Store.Details()&quot;**.</span></span>

        <span data-ttu-id="7d536-281">![Durchsuchen von StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "StoreBrowse durchsuchen")</span><span class="sxs-lookup"><span data-stu-id="7d536-281">![Browsing StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "Browsing StoreBrowse")</span></span>

        <span data-ttu-id="7d536-282">*/Store/Browse durchsuchen*</span><span class="sxs-lookup"><span data-stu-id="7d536-282">*Browsing /Store/Browse*</span></span>
3. <span data-ttu-id="7d536-283">Schließen Sie den Browser.</span><span class="sxs-lookup"><span data-stu-id="7d536-283">Close the browser.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Passing_parameters_to_a_Controller"></a>
### <a name="exercise-3-passing-parameters-to-a-controller"></a><span data-ttu-id="7d536-284">Übung 3: Übergeben von Parametern zu einem Domänencontroller</span><span class="sxs-lookup"><span data-stu-id="7d536-284">Exercise 3: Passing parameters to a Controller</span></span>

<span data-ttu-id="7d536-285">Bis jetzt haben Sie Konstante Zeichenfolgen aus Controller zurückgeben wurde.</span><span class="sxs-lookup"><span data-stu-id="7d536-285">Until now, you have been returning constant strings from the Controllers.</span></span> <span data-ttu-id="7d536-286">In dieser Übung erfahren Sie, wie Parameter auf einen Domänencontroller mithilfe der URL und Abfragezeichenfolge, und nehmen Sie dann die methodenaktionen reagiert mit Text an den Browser zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="7d536-286">In this exercise you will learn how to pass parameters to a Controller using the URL and querystring, and then making the method actions respond with text to the browser.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Adding_Genre_Parameter_to_StoreController"></a>
#### <a name="task-1---adding-genre-parameter-to-storecontroller"></a><span data-ttu-id="7d536-287">Aufgabe 1: Hinzufügen von "Genre"-Parameter StoreController</span><span class="sxs-lookup"><span data-stu-id="7d536-287">Task 1 - Adding Genre Parameter to StoreController</span></span>

<span data-ttu-id="7d536-288">In dieser Aufgabe verwenden Sie die **Querystring** Parameter zum Senden der **Durchsuchen** Aktionsmethode in der **StoreController**.</span><span class="sxs-lookup"><span data-stu-id="7d536-288">In this task, you will use the **querystring** to send parameters to the **Browse** action method in the **StoreController**.</span></span>

1. <span data-ttu-id="7d536-289">Wenn nicht bereits geöffnet ist, starten Sie **Visual Studio Express für Web**.</span><span class="sxs-lookup"><span data-stu-id="7d536-289">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="7d536-290">In der **Datei** Menü wählen **geöffneten Projekt**.</span><span class="sxs-lookup"><span data-stu-id="7d536-290">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="7d536-291">Navigieren Sie zu, klicken Sie im Dialogfeld "Projekt öffnen" **Source\Ex03 PassingParametersToAController\Begin**Option **Begin.sln** , und klicken Sie auf **öffnen**.</span><span class="sxs-lookup"><span data-stu-id="7d536-291">In the Open Project dialog, browse to **Source\Ex03-PassingParametersToAController\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="7d536-292">Alternativ können Sie mit der Lösung fortfahren, dass Sie nach Abschluss der vorherigen Übung erhalten haben.</span><span class="sxs-lookup"><span data-stu-id="7d536-292">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="7d536-293">Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung müssen Sie einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren.</span><span class="sxs-lookup"><span data-stu-id="7d536-293">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="7d536-294">Klicken Sie hierzu auf die **Projekt** Menü **NuGet-Pakete verwalten**.</span><span class="sxs-lookup"><span data-stu-id="7d536-294">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="7d536-295">In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.</span><span class="sxs-lookup"><span data-stu-id="7d536-295">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="7d536-296">Schließlich erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.</span><span class="sxs-lookup"><span data-stu-id="7d536-296">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="7d536-297">Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken, die sich in Ihrem Projekt liefern Verringern der Projektgröße.</span><span class="sxs-lookup"><span data-stu-id="7d536-297">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="7d536-298">Mit NuGet-Powertools werden durch Angabe der Paketversionen in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, wenn, das Sie das Projekt ausführen, können.</span><span class="sxs-lookup"><span data-stu-id="7d536-298">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="7d536-299">Deshalb wird müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit.</span><span class="sxs-lookup"><span data-stu-id="7d536-299">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="7d536-300">Open **StoreController** Klasse.</span><span class="sxs-lookup"><span data-stu-id="7d536-300">Open **StoreController** class.</span></span> <span data-ttu-id="7d536-301">Klicken Sie hierzu in der **Projektmappen-Explorer**, erweitern Sie die **Controller** Ordner und doppelklicken Sie auf **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="7d536-301">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
4. <span data-ttu-id="7d536-302">Ändern der **Durchsuchen** Methode, die einen Zeichenfolgenparameter an die Anforderung für einen bestimmten "Genre" hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="7d536-302">Change the **Browse** method, adding a string parameter to request for a specific genre.</span></span> <span data-ttu-id="7d536-303">ASP.NET MVC automatisch alle Querystring übergeben oder Parameter mit den Namen der formularbereitstellung **"Genre"** auf diese Aktionsmethode beim Aufrufen.</span><span class="sxs-lookup"><span data-stu-id="7d536-303">ASP.NET MVC will automatically pass any querystring or form post parameters named **genre** to this action method when invoked.</span></span> <span data-ttu-id="7d536-304">Ersetzen Sie hierzu die **Durchsuchen** -Methode durch folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="7d536-304">To do this, replace the **Browse** method with the following code:</span></span>

    <span data-ttu-id="7d536-305">(Codeausschnitt - *Grundlagen von ASP.NET MVC 4 - Ex3 StoreController BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="7d536-305">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex3 StoreController BrowseMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="7d536-306">Verwenden Sie die **HttpUtility.HtmlEncode durchführen** Hilfsprogrammmethode zum verhindert, dass Benutzer Räumen Javascript in der Ansicht mit einem Link wie   **/Store/durchsuchen? "Genre" =&lt;Skript&gt;window.location= "[http://hackersite.com](http://hackersite.com)"&lt;/script&gt;**.</span><span class="sxs-lookup"><span data-stu-id="7d536-306">You are using the **HttpUtility.HtmlEncode** utility method to prevents users from injecting Javascript into the View with a link like **/Store/Browse?Genre=&lt;script&gt;window.location='[http://hackersite.com](http://hackersite.com)'&lt;/script&gt;**.</span></span>
> 
> <span data-ttu-id="7d536-307">Weitere Hinweise finden Sie auf [diesem Msdn-Artikel](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).</span><span class="sxs-lookup"><span data-stu-id="7d536-307">For further explanation, please visit [this msdn article](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="7d536-308">Aufgabe 2: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="7d536-308">Task 2 - Running the Application</span></span>

<span data-ttu-id="7d536-309">In dieser Aufgabe wird die Anwendung in einem Webbrowser testen und verwenden Sie die **"Genre"** Parameter.</span><span class="sxs-lookup"><span data-stu-id="7d536-309">In this task, you will try out the Application in a web browser and use the **genre** parameter.</span></span>

1. <span data-ttu-id="7d536-310">Drücken Sie **F5** um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="7d536-310">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="7d536-311">Das Projekt gestartet wird, der **Home** Seite.</span><span class="sxs-lookup"><span data-stu-id="7d536-311">The project starts in the **Home** page.</span></span> <span data-ttu-id="7d536-312">Ändern Sie die URL zum   */speichern/suchen? "Genre" = Disco* um sicherzustellen, dass die Aktion den Parameter "Genre" empfängt.</span><span class="sxs-lookup"><span data-stu-id="7d536-312">Change the URL to */Store/Browse?Genre=Disco* to verify that the action receives the genre parameter.</span></span>

    <span data-ttu-id="7d536-313">![Durchsuchen von StoreBrowseGenre = Disco](aspnet-mvc-4-fundamentals/_static/image10.png "StoreBrowseGenre durchsuchen Disco =")</span><span class="sxs-lookup"><span data-stu-id="7d536-313">![Browsing StoreBrowseGenre=Disco](aspnet-mvc-4-fundamentals/_static/image10.png "Browsing StoreBrowseGenre=Disco")</span></span>

    <span data-ttu-id="7d536-314">*Durchsuchen Sie /Store/Browse? "Genre" Disco =*</span><span class="sxs-lookup"><span data-stu-id="7d536-314">*Browsing /Store/Browse?Genre=Disco*</span></span>
3. <span data-ttu-id="7d536-315">Schließen Sie den Browser.</span><span class="sxs-lookup"><span data-stu-id="7d536-315">Close the browser.</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Adding_an_Id_Parameter_Embedded_in_the_URL"></a>
#### <a name="task-3---adding-an-id-parameter-embedded-in-the-url"></a><span data-ttu-id="7d536-316">Aufgabe 3: Hinzufügen eines Id-Parameters, die in die URL eingebettet</span><span class="sxs-lookup"><span data-stu-id="7d536-316">Task 3 - Adding an Id Parameter Embedded in the URL</span></span>

<span data-ttu-id="7d536-317">In dieser Aufgabe verwenden Sie die **URL** übergeben ein **Id** Parameter an die **Details** Aktionsmethode von der **StoreController**.</span><span class="sxs-lookup"><span data-stu-id="7d536-317">In this task, you will use the **URL** to pass an **Id** parameter to the **Details** action method of the **StoreController**.</span></span> <span data-ttu-id="7d536-318">ASP.NETs standardmäßig routing-Konvention ist, das eine URL-Segment nach dem Methodennamen Aktion zu behandeln, als einen Parameter namens **Id**. Wenn der Aktionsmethode Parameter Id verfügt, wird ASP.NET MVC automatisch das URL-Segment für Sie als Parameter übergeben.</span><span class="sxs-lookup"><span data-stu-id="7d536-318">ASP.NET MVC's default routing convention is to treat the segment of a URL after the action method name as a parameter named **Id**. If your action method has parameter named Id, then ASP.NET MVC will automatically pass the URL segment to you as a parameter.</span></span> <span data-ttu-id="7d536-319">In der URL **Store/Informationen/5**, **Id** als interpretiert **5**.</span><span class="sxs-lookup"><span data-stu-id="7d536-319">In the URL **Store/Details/5**, **Id** will be interpreted as **5**.</span></span>

1. <span data-ttu-id="7d536-320">Ändern der **Details** Methode der **StoreController**, Hinzufügen von einer **Int** Parameter namens **Id**. Ersetzen Sie hierzu die **Details** -Methode durch folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="7d536-320">Change the **Details** method of the **StoreController**, adding an **int** parameter called **id**. To do this, replace the **Details** method with the following code:</span></span>

    <span data-ttu-id="7d536-321">(Codeausschnitt - *Grundlagen von ASP.NET MVC 4 - Ex3 StoreController DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="7d536-321">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex3 StoreController DetailsMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample5.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="7d536-322">Aufgabe 4: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="7d536-322">Task 4 - Running the Application</span></span>

<span data-ttu-id="7d536-323">In dieser Aufgabe wird die Anwendung in einem Webbrowser testen und verwenden Sie die **Id** Parameter.</span><span class="sxs-lookup"><span data-stu-id="7d536-323">In this task, you will try out the Application in a web browser and use the **Id** parameter.</span></span>

1. <span data-ttu-id="7d536-324">Drücken Sie **F5** um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="7d536-324">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="7d536-325">Das Projekt gestartet wird, der **Home** Seite.</span><span class="sxs-lookup"><span data-stu-id="7d536-325">The project starts in the **Home** page.</span></span> <span data-ttu-id="7d536-326">Ändern Sie die URL zum */Store/Details/5* um sicherzustellen, dass die Aktion der Id-Parameter erhält.</span><span class="sxs-lookup"><span data-stu-id="7d536-326">Change the URL to */Store/Details/5* to verify that the action receives the id parameter.</span></span>

    <span data-ttu-id="7d536-327">![Durchsuchen von StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "StoreDetails5 durchsuchen")</span><span class="sxs-lookup"><span data-stu-id="7d536-327">![Browsing StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "Browsing StoreDetails5")</span></span>

    <span data-ttu-id="7d536-328">*/Store/Details/5 durchsuchen*</span><span class="sxs-lookup"><span data-stu-id="7d536-328">*Browsing /Store/Details/5*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Creating_a_View"></a>
### <a name="exercise-4-creating-a-view"></a><span data-ttu-id="7d536-329">Übung 4: Erstellen einer Ansicht</span><span class="sxs-lookup"><span data-stu-id="7d536-329">Exercise 4: Creating a View</span></span>

<span data-ttu-id="7d536-330">Bisher haben Sie Zeichenfolgen aus Controlleraktionen zurückgegeben wurde.</span><span class="sxs-lookup"><span data-stu-id="7d536-330">So far you have been returning strings from controller actions.</span></span> <span data-ttu-id="7d536-331">Obwohl dies ist eine hilfreiche Möglichkeit zum Verständnis der Funktionsweise von Domänencontrollern, ist es nicht, wie Ihre echten Web-Anwendungen erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="7d536-331">Although that is a useful way of understanding how controllers work, it is not how your real Web applications are built.</span></span> <span data-ttu-id="7d536-332">Ansichten sind Komponenten, die einen besseren Ansatz zum Generieren von HTML-Codes zurück an den Browser mit der Verwendung von Vorlagendateien zu bieten.</span><span class="sxs-lookup"><span data-stu-id="7d536-332">Views are components that provide a better approach for generating HTML back to the browser with the use of template files.</span></span>

<span data-ttu-id="7d536-333">In dieser Übung erfahren Sie, wie eine Layout-Gestaltungsvorlage zum Einrichten einer Vorlage für allgemeine HTML-Inhalt, ein StyleSheet für das Aussehen und Verhalten der Website und schließlich eine Vorlage anzeigen HomeController zurückzugebenden HTML aktivieren verbessern hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="7d536-333">In this exercise you will learn how to add a layout master page to setup a template for common HTML content, a StyleSheet to enhance the look and feel of the site and finally a View template to enable HomeController to return HTML.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Modifying_the_file__layoutcshtml"></a>
#### <a name="task-1---modifying-the-file-layoutcshtml"></a><span data-ttu-id="7d536-334">Aufgabe 1: Ändern der Datei \_layout.cshtml</span><span class="sxs-lookup"><span data-stu-id="7d536-334">Task 1 - Modifying the file \_layout.cshtml</span></span>

<span data-ttu-id="7d536-335">Die Datei **~/Views/Shared/\_layout.cshtml** ermöglicht es Ihnen so richten Sie eine Vorlage für allgemeine HTML, über die gesamte Website zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="7d536-335">The file **~/Views/Shared/\_layout.cshtml** allows you to setup a template for common HTML to use across the entire website.</span></span> <span data-ttu-id="7d536-336">In dieser Aufgabe fügen Sie eine master-Layoutseite mit einer allgemeinen Header mit Links in den Bereich auf der Startseite und Speicher.</span><span class="sxs-lookup"><span data-stu-id="7d536-336">In this task you will add a layout master page with a common header with links to the Home page and Store area.</span></span>

1. <span data-ttu-id="7d536-337">Wenn nicht bereits geöffnet ist, starten Sie **Visual Studio Express für Web**.</span><span class="sxs-lookup"><span data-stu-id="7d536-337">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="7d536-338">In der **Datei** Menü wählen **geöffneten Projekt**.</span><span class="sxs-lookup"><span data-stu-id="7d536-338">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="7d536-339">Navigieren Sie zu, klicken Sie im Dialogfeld "Projekt öffnen" **Source\Ex04 CreatingAView\Begin**Option **Begin.sln** , und klicken Sie auf **öffnen**.</span><span class="sxs-lookup"><span data-stu-id="7d536-339">In the Open Project dialog, browse to **Source\Ex04-CreatingAView\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="7d536-340">Alternativ können Sie mit der Lösung fortfahren, dass Sie nach Abschluss der vorherigen Übung erhalten haben.</span><span class="sxs-lookup"><span data-stu-id="7d536-340">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="7d536-341">Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung müssen Sie einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren.</span><span class="sxs-lookup"><span data-stu-id="7d536-341">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="7d536-342">Klicken Sie hierzu auf die **Projekt** Menü **NuGet-Pakete verwalten**.</span><span class="sxs-lookup"><span data-stu-id="7d536-342">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="7d536-343">In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.</span><span class="sxs-lookup"><span data-stu-id="7d536-343">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="7d536-344">Schließlich erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.</span><span class="sxs-lookup"><span data-stu-id="7d536-344">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="7d536-345">Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken, die sich in Ihrem Projekt liefern Verringern der Projektgröße.</span><span class="sxs-lookup"><span data-stu-id="7d536-345">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="7d536-346">Mit NuGet-Powertools werden durch Angabe der Paketversionen in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, wenn, das Sie das Projekt ausführen, können.</span><span class="sxs-lookup"><span data-stu-id="7d536-346">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="7d536-347">Deshalb wird müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit.</span><span class="sxs-lookup"><span data-stu-id="7d536-347">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="7d536-348">Die Datei  <strong>\_layout.cshtml</strong> das HTML-Container-Layout für alle Seiten auf der Website enthält.</span><span class="sxs-lookup"><span data-stu-id="7d536-348">The file <strong>\_layout.cshtml</strong> contains the HTML container layout for all pages on the site.</span></span> <span data-ttu-id="7d536-349">Es enthält die <strong>&lt;html&gt;</strong> -Element für die HTML-Antwort als auch die <strong>&lt;Head&gt;</strong> und <strong>&lt;Text&gt;</strong> Elemente.</span><span class="sxs-lookup"><span data-stu-id="7d536-349">It includes the <strong>&lt;html&gt;</strong> element for the HTML response, as well as the <strong>&lt;head&gt;</strong> and <strong>&lt;body&gt;</strong> elements.</span></span> <span data-ttu-id="7d536-350"><strong>@RenderBody()</strong> innerhalb des HTML-Text zu identifizieren Regionen dieser Ansicht Vorlagen werden in der Lage, sich mit dynamischen Inhalten zu füllen.</span><span class="sxs-lookup"><span data-stu-id="7d536-350"><strong>@RenderBody()</strong> within the HTML body identify regions that view templates will be able to fill in with dynamic content.</span></span>
   <span data-ttu-id="7d536-351">(C#)</span><span class="sxs-lookup"><span data-stu-id="7d536-351">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample6.cshtml)]
4. <span data-ttu-id="7d536-352">Fügen Sie einen allgemeinen Header mit Links in den Bereich auf der Startseite und Speicher auf allen Seiten der Website hinzu.</span><span class="sxs-lookup"><span data-stu-id="7d536-352">Add a common header with links to the Home page and Store area on all pages in the site.</span></span> <span data-ttu-id="7d536-353">Zu diesem Zweck fügen Sie den folgenden Code, der unten &lt;Text&gt; Anweisung.</span><span class="sxs-lookup"><span data-stu-id="7d536-353">In order to do that, add the following code below &lt;body&gt; statement.</span></span>
   <span data-ttu-id="7d536-354">(C#)</span><span class="sxs-lookup"><span data-stu-id="7d536-354">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample7.cshtml)]
5. <span data-ttu-id="7d536-355">Enthalten Sie ein Div um Textabschnitt Rand jeder Seite zu rendern.</span><span class="sxs-lookup"><span data-stu-id="7d536-355">Include a div to render the body section of each page.</span></span> <span data-ttu-id="7d536-356">Ersetzen Sie  <strong>@RenderBody()</strong> durch den folgenden Code der Higlighted: (c#)</span><span class="sxs-lookup"><span data-stu-id="7d536-356">Replace <strong>@RenderBody()</strong> with the following higlighted code: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample8.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="7d536-357">Wussten Sie schon?</span><span class="sxs-lookup"><span data-stu-id="7d536-357">Did you know?</span></span> <span data-ttu-id="7d536-358">Visual Studio 2012 weist Ausschnitte, die zum Hinzufügen von häufig verwendeten Code in HTML, Codedateien und mehr erleichtern!</span><span class="sxs-lookup"><span data-stu-id="7d536-358">Visual Studio 2012 has snippets that make it easy to add commonly used code in HTML, code files and more!</span></span> <span data-ttu-id="7d536-359">Versuchen Sie es, indem Sie Folgendes eingeben **&lt;Div&gt;** und **Registerkarte** zweimal, um eine vollständige einfügen **Div** Tag.</span><span class="sxs-lookup"><span data-stu-id="7d536-359">Try it out by typing **&lt;div&gt;** and pressing **TAB** twice to insert a complete **div** tag.</span></span>

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_CSS_Stylesheet"></a>
#### <a name="task-2---adding-css-stylesheet"></a><span data-ttu-id="7d536-360">Aufgabe 2: Hinzufügen von CSS-Stylesheet</span><span class="sxs-lookup"><span data-stu-id="7d536-360">Task 2 - Adding CSS Stylesheet</span></span>

<span data-ttu-id="7d536-361">Die leere Projektvorlage enthält eine sehr optimierte CSS-Datei, die Stile, die zum Anzeigen von grundlegenden Formen und validierungsmeldungen enthält nur.</span><span class="sxs-lookup"><span data-stu-id="7d536-361">The empty project template includes a very streamlined CSS file which just includes styles used to display basic forms and validation messages.</span></span> <span data-ttu-id="7d536-362">Sie zusätzliche CSS-Images (die möglicherweise von einem Designer bereitgestellt werden) verwenden und damit das Aussehen und Verhalten des Standorts zu verbessern.</span><span class="sxs-lookup"><span data-stu-id="7d536-362">You will use additional CSS and images (potentially provided by a designer) in order to enhance the look and feel of the site.</span></span>

<span data-ttu-id="7d536-363">In dieser Aufgabe fügen Sie eine CSS-Stylesheet, um die Formatvorlagen der Site zu definieren.</span><span class="sxs-lookup"><span data-stu-id="7d536-363">In this task, you will add a CSS stylesheet to define the styles of the site.</span></span>

1. <span data-ttu-id="7d536-364">Die CSS-Datei und die Bilder befinden sich die **Source\Assets\Content** Ordner dieser Anleitung.</span><span class="sxs-lookup"><span data-stu-id="7d536-364">The CSS file and images are included in the **Source\Assets\Content** folder of this Lab.</span></span> <span data-ttu-id="7d536-365">Um diese an die Anwendung hinzuzufügen, ziehen Sie ihren Inhalt aus einem **Windows Explorer** Fenster in der **Projektmappen-Explorer** in Visual Studio Express für Web, wie unten dargestellt:</span><span class="sxs-lookup"><span data-stu-id="7d536-365">In order to add them to the application, drag their content from a **Windows Explorer** window into the **Solution Explorer** in Visual Studio Express for Web, as shown below:</span></span>

    <span data-ttu-id="7d536-366">![Ziehen die Formatvorlage Inhalt](aspnet-mvc-4-fundamentals/_static/image12.png "Stil Inhalt ziehen")</span><span class="sxs-lookup"><span data-stu-id="7d536-366">![Dragging style contents](aspnet-mvc-4-fundamentals/_static/image12.png "Dragging style contents")</span></span>

    <span data-ttu-id="7d536-367">*Ziehen die Formatvorlage Inhalt*</span><span class="sxs-lookup"><span data-stu-id="7d536-367">*Dragging style contents*</span></span>
2. <span data-ttu-id="7d536-368">Ein Warndialogfeld angezeigt nach einer Bestätigung ersetzen **"Site.CSS" ändern** Datei- und einige vorhandener Bilder.</span><span class="sxs-lookup"><span data-stu-id="7d536-368">A warning dialog will appear, asking for confirmation to replace **Site.css** file and some existing images.</span></span> <span data-ttu-id="7d536-369">Überprüfen Sie **für alle Elemente übernehmen** , und klicken Sie auf **Ja**.</span><span class="sxs-lookup"><span data-stu-id="7d536-369">Check **Apply to all items** and click **Yes**.</span></span>

<a id="Ex4Task3"></a>

<a id="Task_3_-_Adding_a_View_Template"></a>
#### <a name="task-3---adding-a-view-template"></a><span data-ttu-id="7d536-370">Aufgabe 3: Hinzufügen einer Vorlage für eine Sicht</span><span class="sxs-lookup"><span data-stu-id="7d536-370">Task 3 - Adding a View Template</span></span>

<span data-ttu-id="7d536-371">In dieser Aufgabe fügen Sie eine Vorlage anzeigen, um HTML-Antwort zu generieren, die das Layout-Gestaltungsvorlage verwendet wird, und CSS in dieser Übung hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="7d536-371">In this task, you will add a View template to generate the HTML response that will use the layout master page and CSS added in this exercise.</span></span>

1. <span data-ttu-id="7d536-372">Um eine Vorlage anzeigen beim Durchsuchen der Homepage der Website zu verwenden, müssen Sie zuerst an, dass eine Zeichenfolge zurückgegeben, sondern die **HomeController Index** Methode zurückgegeben wird ein **Ansicht**.</span><span class="sxs-lookup"><span data-stu-id="7d536-372">To use a View template when browsing the site's home page, you will first need to indicate that instead of returning a string, the **HomeController Index** method will return a **View**.</span></span> <span data-ttu-id="7d536-373">Öffnen **HomeController** Klasse, und Ändern seiner **Index** -Methode zur Rückgabe einer **ActionResult**, und zurückgeben **View()**.</span><span class="sxs-lookup"><span data-stu-id="7d536-373">Open **HomeController** class and change its **Index** method to return an **ActionResult**, and have it return **View()**.</span></span>

    <span data-ttu-id="7d536-374">(Codeausschnitt - *Grundlagen von ASP.NET MVC 4 - Ex4 HomeController Index*)</span><span class="sxs-lookup"><span data-stu-id="7d536-374">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex4 HomeController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample9.cs)]
2. <span data-ttu-id="7d536-375">Nun müssen Sie eine entsprechende Ansichtenvorlage hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="7d536-375">Now, you need to add an appropriate View template.</span></span> <span data-ttu-id="7d536-376">Hierzu **mit der rechten Maustaste** innerhalb der **Index** Aktionsmethode, und wählen **Ansicht hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="7d536-376">To do this, **right-click** inside the **Index** action method and select **Add View**.</span></span> <span data-ttu-id="7d536-377">Hierdurch erscheint der **Ansicht hinzufügen** Dialogfeld.</span><span class="sxs-lookup"><span data-stu-id="7d536-377">This will bring up the **Add View** dialog.</span></span>

    <span data-ttu-id="7d536-378">![Hinzufügen einer Ansicht von innerhalb der Methode Index](aspnet-mvc-4-fundamentals/_static/image13.png "Hinzufügen einer Ansicht von innerhalb der Index-Methode")</span><span class="sxs-lookup"><span data-stu-id="7d536-378">![Adding a View from within the Index method](aspnet-mvc-4-fundamentals/_static/image13.png "Adding a View from within the Index method")</span></span>

    <span data-ttu-id="7d536-379">*Hinzufügen einer Ansicht von innerhalb der Index-Methode*</span><span class="sxs-lookup"><span data-stu-id="7d536-379">*Adding a View from within the Index method*</span></span>
3. <span data-ttu-id="7d536-380">Die **Ansicht hinzufügen** wird ein Dialogfeld angezeigt, um eine Vorlagendatei Ansicht zu generieren.</span><span class="sxs-lookup"><span data-stu-id="7d536-380">The **Add View** Dialog will appear to generate a View template file.</span></span> <span data-ttu-id="7d536-381">Standardmäßig füllt dieses Dialogfeld den Namen der Vorlage anzeigen, damit diese die Aktionsmethode übereinstimmt, die verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="7d536-381">By default, this dialog pre-populates the name of the View template so that it matches the action method that will use it.</span></span> <span data-ttu-id="7d536-382">Da Sie verwendet die **Ansicht hinzufügen** Kontextmenü innerhalb der **Index** Aktionsmethode innerhalb der HomeController die **Ansicht hinzufügen** Dialogfeld Index mit dem Standardnamen für die Sicht hat.</span><span class="sxs-lookup"><span data-stu-id="7d536-382">Because you used the **Add View** context menu within the **Index** action method within the HomeController, the **Add View** dialog has Index as the default view name.</span></span> <span data-ttu-id="7d536-383">Klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="7d536-383">Click **Add**.</span></span>

    <span data-ttu-id="7d536-384">![View-Dialogfeld "hinzufügen"](aspnet-mvc-4-fundamentals/_static/image14.png "Ansicht-Dialogfeld \"hinzufügen\"")</span><span class="sxs-lookup"><span data-stu-id="7d536-384">![Add View Dialog](aspnet-mvc-4-fundamentals/_static/image14.png "Add View Dialog")</span></span>

    <span data-ttu-id="7d536-385">*View-Dialogfeld "hinzufügen"*</span><span class="sxs-lookup"><span data-stu-id="7d536-385">*Add View Dialog*</span></span>
4. <span data-ttu-id="7d536-386">Visual Studio generiert eine **Index.cshtml** Vorlage anzeigen, in der **Views\Home den** Ordner und dann geöffnet.</span><span class="sxs-lookup"><span data-stu-id="7d536-386">Visual Studio generates an **Index.cshtml** view template inside the **Views\Home** folder and then opens it.</span></span>

    <span data-ttu-id="7d536-387">![Home Indexansicht erstellt](aspnet-mvc-4-fundamentals/_static/image15.png "Home Indexansicht erstellt")</span><span class="sxs-lookup"><span data-stu-id="7d536-387">![Home Index view created](aspnet-mvc-4-fundamentals/_static/image15.png "Home Index view created")</span></span>

    <span data-ttu-id="7d536-388">*Home Indexansicht erstellt*</span><span class="sxs-lookup"><span data-stu-id="7d536-388">*Home Index view created*</span></span>

    > [!NOTE]
    > <span data-ttu-id="7d536-389">Name und Speicherort der der **Index.cshtml** Datei spielt und folgt die Standard-ASP.NET-MVC-Benennungskonventionen.</span><span class="sxs-lookup"><span data-stu-id="7d536-389">name and location of the **Index.cshtml** file is relevant and follows the default ASP.NET MVC naming conventions.</span></span>
    > 
    > <span data-ttu-id="7d536-390">Der Ordner \Views\**Home** den Controllernamen entspricht (**Home** Controller).</span><span class="sxs-lookup"><span data-stu-id="7d536-390">The folder \Views\**Home** matches the controller name (**Home** Controller).</span></span> <span data-ttu-id="7d536-391">Den Namen der Sicht (**Index**), entspricht der Aktionsmethode des Controllers der Ansicht anzeigen werden.</span><span class="sxs-lookup"><span data-stu-id="7d536-391">The View template name (**Index**), matches the controller action method which will be displaying the View.</span></span>
    > 
    > <span data-ttu-id="7d536-392">Auf diese Weise vermeidet ASP.NET-MVC den Namen oder Speicherort der Vorlage für eine Sicht explizit angeben, wenn Sie diese Namenskonvention mithilfe eine Sicht zurück.</span><span class="sxs-lookup"><span data-stu-id="7d536-392">This way, ASP.NET MVC avoids having to explicitly specify the name or location of a View template when using this naming convention to return a View.</span></span>
5. <span data-ttu-id="7d536-393">Generierte Ansichtenvorlage basiert auf der  **\_layout.cshtml** Vorlage, die zuvor definiert.</span><span class="sxs-lookup"><span data-stu-id="7d536-393">The generated View template is based on the **\_layout.cshtml** template earlier defined.</span></span> <span data-ttu-id="7d536-394">Aktualisieren Sie die ViewBag.Title-Eigenschaft, um **Home**, und ändern Sie den Hauptinhalt zu **Dies ist die Startseite**, wie im folgenden Code gezeigt:</span><span class="sxs-lookup"><span data-stu-id="7d536-394">Update the ViewBag.Title property to **Home**, and change the main content to **This is the Home Page**, as shown in the code below:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample10.cshtml)]
6. <span data-ttu-id="7d536-395">Wählen Sie **MvcMusicStore** Projekt im Projektmappen-Explorer und drücken Sie **F5** um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="7d536-395">Select **MvcMusicStore** project in the Solution Explorer and Press **F5** to run the Application.</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_Verification"></a>
#### <a name="task-4-verification"></a><span data-ttu-id="7d536-396">Aufgabe 4: Überprüfung</span><span class="sxs-lookup"><span data-stu-id="7d536-396">Task 4: Verification</span></span>

<span data-ttu-id="7d536-397">Um sicherzustellen, dass Sie alle Schritte richtig im vorherigen Schritt ausgeführt haben, gehen Sie wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="7d536-397">In order to verify that you have correctly performed all the steps in the previous exercise, proceed as follows:</span></span>

<span data-ttu-id="7d536-398">Mit der Anwendung in einem Browser geöffnet wird sollten Sie Folgendes beachten:</span><span class="sxs-lookup"><span data-stu-id="7d536-398">With the application opened in a browser, you should note that:</span></span>

1. <span data-ttu-id="7d536-399">Die HomeController Index Aktionsmethode gefunden und angezeigt der **\Views\Home\Index.cshtml** Vorlage anzeigen, auch wenn der Code aufgerufen **zurückgeben View()**, da gefolgt von der Vorlage Anzeigen der standardmäßige Benennungskonvention zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="7d536-399">The HomeController's Index action method found and displayed the **\Views\Home\Index.cshtml** View template, even though the code called **return View()**, because the View template followed the standard naming convention.</span></span>
2. <span data-ttu-id="7d536-400">Auf der Startseite zeigt die Begrüßungsnachricht, der definiert, die innerhalb der **\Views\Home\Index.cshtml** Vorlage anzeigen.</span><span class="sxs-lookup"><span data-stu-id="7d536-400">The Home Page displays the welcome message defined within the **\Views\Home\Index.cshtml** view template.</span></span>
3. <span data-ttu-id="7d536-401">Die Startseite wird mithilfe der  **\_layout.cshtml** Vorlage, und damit die Willkommensnachricht innerhalb der standardmäßigen Website HTML-Layout enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="7d536-401">The Home Page is using the **\_layout.cshtml** template, and so the welcome message is contained within the standard site HTML layout.</span></span>

    <span data-ttu-id="7d536-402">![Mit dem definierten LayoutPage und Stil Indexansicht Home](aspnet-mvc-4-fundamentals/_static/image16.png "Home Indexansicht, die mit dem definierten LayoutPage und Stil")</span><span class="sxs-lookup"><span data-stu-id="7d536-402">![Home Index View using the defined LayoutPage and style](aspnet-mvc-4-fundamentals/_static/image16.png "Home Index View using the defined LayoutPage and style")</span></span>

    <span data-ttu-id="7d536-403">*Home Indexansicht mit dem definierten LayoutPage und Stil*</span><span class="sxs-lookup"><span data-stu-id="7d536-403">*Home Index View using the defined LayoutPage and style*</span></span>

<a id="Exercise5"></a>

<a id="Exercise_5_Creating_a_View_Model"></a>
### <a name="exercise-5-creating-a-view-model"></a><span data-ttu-id="7d536-404">Übung 5: Erstellen eines Modells anzeigen</span><span class="sxs-lookup"><span data-stu-id="7d536-404">Exercise 5: Creating a View Model</span></span>

<span data-ttu-id="7d536-405">Bisher vorgenommenen Ihre Ansichten hartcodiert HTML angezeigt, aber damit dynamischer Webanwendungen erstellen können, sollten die Vorlage anzeigen Informationen vom Controller erhalten.</span><span class="sxs-lookup"><span data-stu-id="7d536-405">So far, you made your Views display hardcoded HTML, but, in order to create dynamic web applications, the View template should receive information from the Controller.</span></span> <span data-ttu-id="7d536-406">Ist ein allgemeines Verfahren für diesen Zweck verwendet werden soll die **ViewModel** Muster, welches einen Controller ermöglicht so verpacken Sie die Informationen, die zum Generieren der entsprechenden HTML-Antwort erforderlich.</span><span class="sxs-lookup"><span data-stu-id="7d536-406">One common technique to be used for that purpose is the **ViewModel** pattern, which allows a Controller to package up all the information needed to generate the appropriate HTML response.</span></span>

<span data-ttu-id="7d536-407">In dieser Übung erfahren Sie, wie eine ViewModel-Klasse erstellen, und fügen die erforderlichen Eigenschaften: die Anzahl der in den Speicher und eine Liste von diesen Genres Genres.</span><span class="sxs-lookup"><span data-stu-id="7d536-407">In this exercise, you will learn how to create a ViewModel class and add the required properties: the number of genres in the store and a list of those genres.</span></span> <span data-ttu-id="7d536-408">Sie werden auch die StoreController Verwendung erstellte ViewModel aktualisiert, und schließlich erstellen Sie eine neue Vorlage anzeigen, in denen die genannten Eigenschaften auf der Seite angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="7d536-408">You will also update the StoreController to use the created ViewModel, and finally, you will create a new View template that will display the mentioned properties in the page.</span></span>

<a id="Ex5Task1"></a>

<a id="Task_1_-_Creating_a_ViewModel_Class"></a>
#### <a name="task-1---creating-a-viewmodel-class"></a><span data-ttu-id="7d536-409">Aufgabe 1: Erstellen einer ViewModel-Klasse</span><span class="sxs-lookup"><span data-stu-id="7d536-409">Task 1 - Creating a ViewModel Class</span></span>

<span data-ttu-id="7d536-410">In dieser Aufgabe erstellen Sie eine ViewModel-Klasse, die im Speicher "Genre" Angebot Szenario implementiert wird.</span><span class="sxs-lookup"><span data-stu-id="7d536-410">In this task, you will create a ViewModel class that will implement the Store genre listing scenario.</span></span>

1. <span data-ttu-id="7d536-411">Wenn nicht bereits geöffnet ist, starten Sie **Visual Studio Express für Web**.</span><span class="sxs-lookup"><span data-stu-id="7d536-411">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="7d536-412">In der **Datei** Menü wählen **geöffneten Projekt**.</span><span class="sxs-lookup"><span data-stu-id="7d536-412">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="7d536-413">Navigieren Sie zu, klicken Sie im Dialogfeld "Projekt öffnen" **Source\Ex05 CreatingAViewModel\Begin**Option **Begin.sln** , und klicken Sie auf **öffnen**.</span><span class="sxs-lookup"><span data-stu-id="7d536-413">In the Open Project dialog, browse to **Source\Ex05-CreatingAViewModel\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="7d536-414">Alternativ können Sie mit der Lösung fortfahren, dass Sie nach Abschluss der vorherigen Übung erhalten haben.</span><span class="sxs-lookup"><span data-stu-id="7d536-414">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="7d536-415">Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung müssen Sie einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren.</span><span class="sxs-lookup"><span data-stu-id="7d536-415">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="7d536-416">Klicken Sie hierzu auf die **Projekt** Menü **NuGet-Pakete verwalten**.</span><span class="sxs-lookup"><span data-stu-id="7d536-416">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="7d536-417">In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.</span><span class="sxs-lookup"><span data-stu-id="7d536-417">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="7d536-418">Schließlich erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.</span><span class="sxs-lookup"><span data-stu-id="7d536-418">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="7d536-419">Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken, die sich in Ihrem Projekt liefern Verringern der Projektgröße.</span><span class="sxs-lookup"><span data-stu-id="7d536-419">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="7d536-420">Mit NuGet-Powertools werden durch Angabe der Paketversionen in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, wenn, das Sie das Projekt ausführen, können.</span><span class="sxs-lookup"><span data-stu-id="7d536-420">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="7d536-421">Deshalb wird müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit.</span><span class="sxs-lookup"><span data-stu-id="7d536-421">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="7d536-422">Erstellen einer **ViewModels** Ordner für das ViewModel.</span><span class="sxs-lookup"><span data-stu-id="7d536-422">Create a **ViewModels** folder to hold the ViewModel.</span></span> <span data-ttu-id="7d536-423">Zu diesem Zweck der obersten Ebene Maustaste **MvcMusicStore** -Projekt, wählen **hinzufügen** und dann **neuer Ordner**.</span><span class="sxs-lookup"><span data-stu-id="7d536-423">To do this, right-click the top-level **MvcMusicStore** project, select **Add** and then **New Folder**.</span></span>

    <span data-ttu-id="7d536-424">![Hinzufügen eines neuen Ordners](aspnet-mvc-4-fundamentals/_static/image17.png "einen neuen Ordner hinzufügen")</span><span class="sxs-lookup"><span data-stu-id="7d536-424">![Adding a new folder](aspnet-mvc-4-fundamentals/_static/image17.png "Adding a new folder")</span></span>

    <span data-ttu-id="7d536-425">*Hinzufügen eines neuen Ordners*</span><span class="sxs-lookup"><span data-stu-id="7d536-425">*Adding a new folder*</span></span>
4. <span data-ttu-id="7d536-426">Benennen Sie den Ordner *ViewModels*.</span><span class="sxs-lookup"><span data-stu-id="7d536-426">Name the folder *ViewModels*.</span></span>

    <span data-ttu-id="7d536-427">![ViewModels-Ordner im Projektmappen-Explorer](aspnet-mvc-4-fundamentals/_static/image18.png "ViewModels-Ordner im Projektmappen-Explorer")</span><span class="sxs-lookup"><span data-stu-id="7d536-427">![ViewModels folder in Solution Explorer](aspnet-mvc-4-fundamentals/_static/image18.png "ViewModels folder in Solution Explorer")</span></span>

    <span data-ttu-id="7d536-428">*ViewModels-Ordner im Projektmappen-Explorer*</span><span class="sxs-lookup"><span data-stu-id="7d536-428">*ViewModels folder in Solution Explorer*</span></span>
5. <span data-ttu-id="7d536-429">Erstellen einer **ViewModel** Klasse.</span><span class="sxs-lookup"><span data-stu-id="7d536-429">Create a **ViewModel** class.</span></span> <span data-ttu-id="7d536-430">Zu diesem Zweck mit der Maustaste auf die **ViewModels** Ordner vor kurzem erstellt wurde, wählen Sie **hinzufügen** und dann **neues Element**.</span><span class="sxs-lookup"><span data-stu-id="7d536-430">To do this, right-click on the **ViewModels** folder recently created, select **Add** and then **New Item**.</span></span> <span data-ttu-id="7d536-431">Klicken Sie unter **Code**, wählen Sie die **Klasse** -Element aus, und nennen Sie die Datei *StoreIndexViewModel.cs*, klicken Sie dann auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="7d536-431">Under **Code**, choose the **Class** item and name the file *StoreIndexViewModel.cs*, then click **Add**.</span></span>

    <span data-ttu-id="7d536-432">![Hinzufügen einer neuen Klasse](aspnet-mvc-4-fundamentals/_static/image19.png "Hinzufügen einer neuen Klasse")</span><span class="sxs-lookup"><span data-stu-id="7d536-432">![Adding a new Class](aspnet-mvc-4-fundamentals/_static/image19.png "Adding a new Class")</span></span>

    <span data-ttu-id="7d536-433">*Hinzufügen einer neuen Klasse*</span><span class="sxs-lookup"><span data-stu-id="7d536-433">*Adding a new Class*</span></span>

    <span data-ttu-id="7d536-434">![Erstellen von StoreIndexViewModel Klasse](aspnet-mvc-4-fundamentals/_static/image20.png "erstellen StoreIndexViewModel-Klasse")</span><span class="sxs-lookup"><span data-stu-id="7d536-434">![Creating StoreIndexViewModel class](aspnet-mvc-4-fundamentals/_static/image20.png "Creating StoreIndexViewModel class")</span></span>

    <span data-ttu-id="7d536-435">*Erstellen von StoreIndexViewModel-Klasse*</span><span class="sxs-lookup"><span data-stu-id="7d536-435">*Creating StoreIndexViewModel class*</span></span>

<a id="Ex5Task2"></a>

<a id="Task_2_-_Adding_Properties_to_the_ViewModel_class"></a>
#### <a name="task-2---adding-properties-to-the-viewmodel-class"></a><span data-ttu-id="7d536-436">Aufgabe 2: Hinzufügen von Eigenschaften für das ViewModel-Klasse</span><span class="sxs-lookup"><span data-stu-id="7d536-436">Task 2 - Adding Properties to the ViewModel class</span></span>

<span data-ttu-id="7d536-437">Es gibt zwei Parameter an die Vorlage anzeigen aus der StoreController übergeben werden, um die erwartete HTML-Antwort zu generieren: die Anzahl der in den Speicher und eine Liste von diesen Genres Genres.</span><span class="sxs-lookup"><span data-stu-id="7d536-437">There are two parameters to be passed from the StoreController to the View template in order to generate the expected HTML response: the number of genres in the store and a list of those genres.</span></span>

<span data-ttu-id="7d536-438">In dieser Aufgabe fügen Sie dieser 2 Eigenschaften der **StoreIndexViewModel** Klasse: **NumberOfGenres** (eine Ganzzahl) und **Genres** (eine Liste von Zeichenfolgen).</span><span class="sxs-lookup"><span data-stu-id="7d536-438">In this task, you will add those 2 properties to the **StoreIndexViewModel** class: **NumberOfGenres** (an integer) and **Genres** (a list of strings).</span></span>

1. <span data-ttu-id="7d536-439">Hinzufügen **NumberOfGenres** und **Genres** Eigenschaften der **StoreIndexViewModel** Klasse.</span><span class="sxs-lookup"><span data-stu-id="7d536-439">Add **NumberOfGenres** and **Genres** properties to the **StoreIndexViewModel** class.</span></span> <span data-ttu-id="7d536-440">Zu diesem Zweck fügen Sie der Klassendefinition die 2 folgenden Zeilen hinzu:</span><span class="sxs-lookup"><span data-stu-id="7d536-440">To do this, add the following 2 lines to the class definition:</span></span>

    <span data-ttu-id="7d536-441">(Codeausschnitt - *ASP.NET MVC 4-Grundlagen - Ex5 StoreIndexViewModel Eigenschaften*)</span><span class="sxs-lookup"><span data-stu-id="7d536-441">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreIndexViewModel properties*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample11.cs)]

> [!NOTE]
> <span data-ttu-id="7d536-442">Die **{abrufen; festlegen;}**  Notation nutzt # Funktion automatisch implementierte Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="7d536-442">The **{ get; set; }** notation makes use of C#'s auto-implemented properties feature.</span></span> <span data-ttu-id="7d536-443">Bietet die Vorteile einer Eigenschaft ohne uns einen dahinter liegende Feld nicht deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="7d536-443">It provides the benefits of a property without requiring us to declare a backing field.</span></span>

<a id="Ex5Task3"></a>

<a id="Task_3_-_Updating_StoreController_to_use_the_StoreIndexViewModel"></a>
#### <a name="task-3---updating-storecontroller-to-use-the-storeindexviewmodel"></a><span data-ttu-id="7d536-444">Aufgabe 3 - Update StoreController die StoreIndexViewModel verwenden</span><span class="sxs-lookup"><span data-stu-id="7d536-444">Task 3 - Updating StoreController to use the StoreIndexViewModel</span></span>

<span data-ttu-id="7d536-445">Die **StoreIndexViewModel** -Klasse kapselt die benötigten Informationen zum Übergeben von **StoreController**des **Index** Methode, um eine Vorlage anzeigen, um eine Antwort zu generieren. .</span><span class="sxs-lookup"><span data-stu-id="7d536-445">The **StoreIndexViewModel** class encapsulates the information needed to pass from **StoreController**'s **Index** method to a View template in order to generate a response.</span></span>

<span data-ttu-id="7d536-446">In dieser Aufgabe aktualisieren Sie die **StoreController** verwenden die **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="7d536-446">In this task, you will update the **StoreController** to use the **StoreIndexViewModel**.</span></span>

1. <span data-ttu-id="7d536-447">Open **StoreController** Klasse.</span><span class="sxs-lookup"><span data-stu-id="7d536-447">Open **StoreController** class.</span></span>

    <span data-ttu-id="7d536-448">![Öffnen StoreController Klasse](aspnet-mvc-4-fundamentals/_static/image21.png "öffnende StoreController-Klasse")</span><span class="sxs-lookup"><span data-stu-id="7d536-448">![Opening StoreController class](aspnet-mvc-4-fundamentals/_static/image21.png "Opening StoreController class")</span></span>

    <span data-ttu-id="7d536-449">*Öffnende StoreController-Klasse*</span><span class="sxs-lookup"><span data-stu-id="7d536-449">*Opening StoreController class*</span></span>
2. <span data-ttu-id="7d536-450">Zum Verwenden der **StoreIndexViewModel** -Klasse aus den **StoreController**, fügen Sie den folgenden Namespace am Anfang der **StoreController** Code:</span><span class="sxs-lookup"><span data-stu-id="7d536-450">In order to use the **StoreIndexViewModel** class from the **StoreController**, add the following namespace at the top of the **StoreController** code:</span></span>

    <span data-ttu-id="7d536-451">(Codeausschnitt - *ASP.NET MVC 4-Grundlagen - Ex5 StoreIndexViewModel mit ViewModels*)</span><span class="sxs-lookup"><span data-stu-id="7d536-451">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreIndexViewModel using ViewModels*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample12.cs)]
3. <span data-ttu-id="7d536-452">Ändern der **StoreController**des **Index** Aktionsmethode so, dass er erstellt und füllt eine **StoreIndexViewModel** -Objekt und übergibt Sie es dann, um eine Ansicht aus, um Generieren einer HTML-Antwort mit.</span><span class="sxs-lookup"><span data-stu-id="7d536-452">Change the **StoreController**'s **Index** action method so that it creates and populates a **StoreIndexViewModel** object and then passes it off to a View template to generate an HTML response with it.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7d536-453">Im Labor &quot;ASP.NET MVC-Modelle und Datenzugriff&quot; schreiben Sie Code, der die Liste der Store Genres aus einer Datenbank abruft.</span><span class="sxs-lookup"><span data-stu-id="7d536-453">In Lab &quot;ASP.NET MVC Models and Data Access&quot; you will write code that retrieves the list of store genres from a database.</span></span> <span data-ttu-id="7d536-454">In den folgenden Code, erstellen Sie eine **Liste** von Genres dummy-Daten, die mit Daten Auffüllen der **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="7d536-454">In the following code, you will create a **List** of dummy data genres that will populate the **StoreIndexViewModel**.</span></span>
    > 
    > <span data-ttu-id="7d536-455">Nach dem Erstellen und Einrichten der **StoreIndexViewModel** übergeben, es wird als Argument an die **Ansicht** Methode.</span><span class="sxs-lookup"><span data-stu-id="7d536-455">After creating and setting up the **StoreIndexViewModel** object, it will be passed as an argument to the **View** method.</span></span> <span data-ttu-id="7d536-456">Dies gibt an, dass die Vorlage anzeigen zum Generieren einer HTML-Antwort, damit dieses Objekt verwendet.</span><span class="sxs-lookup"><span data-stu-id="7d536-456">This indicates that the View template will use that object to generate an HTML response with it.</span></span>
4. <span data-ttu-id="7d536-457">Ersetzen Sie die **Index** -Methode durch folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="7d536-457">Replace the **Index** method with the following code:</span></span>

    <span data-ttu-id="7d536-458">(Codeausschnitt - *ASP.NET MVC 4-Grundlagen - Ex5 StoreController Index Methode*)</span><span class="sxs-lookup"><span data-stu-id="7d536-458">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreController Index method*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample13.cs)]

> [!NOTE]
> <span data-ttu-id="7d536-459">Wenn Sie nicht mit c# vertraut sind, Sie können davon ausgehen, dass die Verwendung **Var** bedeutet, dass die **ViewModel** Variable wird spät gebunden.</span><span class="sxs-lookup"><span data-stu-id="7d536-459">If you're unfamiliar with C#, you may assume that using **var** means that the **viewModel** variable is late-bound.</span></span> <span data-ttu-id="7d536-460">Das ist nicht richtig - Typrückschluss basierend auf, was Sie der Variable zuzuweisen ermitteln, ob der C#-Compiler verwendeten **ViewModel** ist vom Typ **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="7d536-460">That's not correct - the C# compiler is using type-inference based on what you assign to the variable to determine that **viewModel** is of type **StoreIndexViewModel**.</span></span> <span data-ttu-id="7d536-461">Auch durch das Kompilieren des lokalen **ViewModel** -Variable als ein **StoreIndexViewModel** Geben Sie Get kompilierzeitüberprüfung und Unterstützung von Visual Studio Code-Editors.</span><span class="sxs-lookup"><span data-stu-id="7d536-461">Also, by compiling the local **viewModel** variable as a **StoreIndexViewModel** type you get compile-time checking and Visual Studio code-editor support.</span></span>

<a id="Ex5Task4"></a>

<a id="Task_4_-_Creating_a_View_Template_that_Uses_StoreIndexViewModel"></a>
#### <a name="task-4---creating-a-view-template-that-uses-storeindexviewmodel"></a><span data-ttu-id="7d536-462">Aufgabe 4: Erstellen einer Ansichtenvorlage für die, die StoreIndexViewModel verwendet.</span><span class="sxs-lookup"><span data-stu-id="7d536-462">Task 4 - Creating a View Template that Uses StoreIndexViewModel</span></span>

<span data-ttu-id="7d536-463">In dieser Aufgabe erstellen Sie eine Vorlage anzeigen, die ein StoreIndexViewModel-Objekt, das vom Controller übergebenen nutzt, um eine Liste von Genres anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="7d536-463">In this task, you will create a View template that will use a StoreIndexViewModel object passed from the Controller to display a list of genres.</span></span>

1. <span data-ttu-id="7d536-464">Vor dem Erstellen der neuen Vorlage anzeigen, erstellen Sie das Projekt, damit die **Ansicht-Dialogfeld hinzufügen** kennt die **StoreIndexViewModel** Klasse.</span><span class="sxs-lookup"><span data-stu-id="7d536-464">Before creating the new View template, let's build the project so that the **Add View Dialog** knows about the **StoreIndexViewModel** class.</span></span> <span data-ttu-id="7d536-465">Erstellen des Projekts durch Auswahl der **erstellen** Menüelement und dann **MvcMusicStore erstellen**.</span><span class="sxs-lookup"><span data-stu-id="7d536-465">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>

    <span data-ttu-id="7d536-466">![Beim Erstellen des Projekts](aspnet-mvc-4-fundamentals/_static/image22.png "beim Erstellen des Projekts")</span><span class="sxs-lookup"><span data-stu-id="7d536-466">![Building the project](aspnet-mvc-4-fundamentals/_static/image22.png "Building the project")</span></span>

    <span data-ttu-id="7d536-467">*Beim Erstellen des Projekts*</span><span class="sxs-lookup"><span data-stu-id="7d536-467">*Building the project*</span></span>
2. <span data-ttu-id="7d536-468">Erstellen Sie eine neue Ansichtenvorlage für die ein.</span><span class="sxs-lookup"><span data-stu-id="7d536-468">Create a new View template.</span></span> <span data-ttu-id="7d536-469">Zu diesem Zweck Maustaste innerhalb der **Index** Methode, und wählen **Ansicht hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="7d536-469">To do that, right-click inside the **Index** method and select **Add View**.</span></span>

    <span data-ttu-id="7d536-470">![Hinzufügen einer Ansicht](aspnet-mvc-4-fundamentals/_static/image23.png "eine Sicht hinzufügen")</span><span class="sxs-lookup"><span data-stu-id="7d536-470">![Adding a View](aspnet-mvc-4-fundamentals/_static/image23.png "Adding a View")</span></span>

    <span data-ttu-id="7d536-471">*Hinzufügen einer Ansicht*</span><span class="sxs-lookup"><span data-stu-id="7d536-471">*Adding a View*</span></span>
3. <span data-ttu-id="7d536-472">Da die **Ansicht-Dialogfeld hinzufügen** aufgerufen wurde, aus der **StoreController**, wird standardmäßig in der Vorlage anzeigen hinzugefügt eine **\Views\Store\Index.cshtml** Datei.</span><span class="sxs-lookup"><span data-stu-id="7d536-472">Because the **Add View Dialog** was invoked from the **StoreController**, it will add the View template by default in a **\Views\Store\Index.cshtml** file.</span></span> <span data-ttu-id="7d536-473">Überprüfen Sie die **erstellen Sie eine stark typisierte-anzeigen** Kontrollkästchen, und wählen Sie dann **StoreIndexViewModel** als die **Modellschemas**.</span><span class="sxs-lookup"><span data-stu-id="7d536-473">Check the **Create a strongly-typed-view** checkbox and then select **StoreIndexViewModel** as the **Model class**.</span></span> <span data-ttu-id="7d536-474">Stellen Sie außerdem sicher, dass das Ansichtsmodul ausgewählt ist **Razor**.</span><span class="sxs-lookup"><span data-stu-id="7d536-474">Also, make sure that the View engine selected is **Razor**.</span></span> <span data-ttu-id="7d536-475">Klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="7d536-475">Click **Add**.</span></span>

    <span data-ttu-id="7d536-476">![View-Dialogfeld "hinzufügen"](aspnet-mvc-4-fundamentals/_static/image24.png "Ansicht-Dialogfeld \"hinzufügen\"")</span><span class="sxs-lookup"><span data-stu-id="7d536-476">![Add View Dialog](aspnet-mvc-4-fundamentals/_static/image24.png "Add View Dialog")</span></span>

    <span data-ttu-id="7d536-477">*View-Dialogfeld "hinzufügen"*</span><span class="sxs-lookup"><span data-stu-id="7d536-477">*Add View Dialog*</span></span>

    <span data-ttu-id="7d536-478">Die **\Views\Store\Index.cshtml** Vorlagendatei Ansicht erstellt und geöffnet wird.</span><span class="sxs-lookup"><span data-stu-id="7d536-478">The **\Views\Store\Index.cshtml** View template file is created and opened.</span></span> <span data-ttu-id="7d536-479">Anhand der Informationen bereitgestellt, um die **Ansicht hinzufügen** Dialogfeld im letzten Schritt die Ansicht, die Vorlage erwarten, dass ein **StoreIndexViewModel** -Instanz wie die Daten zum Generieren einer HTML-Antwort.</span><span class="sxs-lookup"><span data-stu-id="7d536-479">Based on the information provided to the **Add View** dialog in the last step, the View template will expect a **StoreIndexViewModel** instance as the data to use to generate an HTML response.</span></span> <span data-ttu-id="7d536-480">Sie werden feststellen, dass die Vorlage übernimmt eine `ViewPage<musicstore.viewmodels.storeindexviewmodel>` in C# geschrieben.</span><span class="sxs-lookup"><span data-stu-id="7d536-480">You will notice that the template inherits a `ViewPage<musicstore.viewmodels.storeindexviewmodel>` in C#.</span></span>

<a id="Ex5Task5"></a>

<a id="Task_5_-_Updating_the_View_Template"></a>
#### <a name="task-5---updating-the-view-template"></a><span data-ttu-id="7d536-481">Aufgabe 5: Aktualisieren der Vorlage anzeigen</span><span class="sxs-lookup"><span data-stu-id="7d536-481">Task 5 - Updating the View Template</span></span>

<span data-ttu-id="7d536-482">In dieser Aufgabe aktualisieren Sie die Vorlage anzeigen, die in der vorhergehenden Aufgabe zum Abrufen der Anzahl von Genres und deren Namen auf der Seite erstellt.</span><span class="sxs-lookup"><span data-stu-id="7d536-482">In this task, you will update the View template created in the last task to retrieve the number of genres and their names within the page.</span></span>

> [!NOTE]
> <span data-ttu-id="7d536-483">Verwenden Sie @ Syntax (so genannte &quot;Codeausdrücke&quot;) zum Ausführen von Code in der Vorlage anzeigen.</span><span class="sxs-lookup"><span data-stu-id="7d536-483">You will use @ syntax (often referred to as &quot;code nuggets&quot;) to execute code within the View template.</span></span>

1. <span data-ttu-id="7d536-484">In der **Index.cshtml** Datei, in der **Store** Ordner, ersetzen Sie den Code durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="7d536-484">In the **Index.cshtml** file, within the **Store** folder, replace its code with the following:</span></span>

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
2. <span data-ttu-id="7d536-485">Schleife in der Liste der "Genre" **StoreIndexViewModel** , und erstellen Sie ein HTML **&lt;Ul&gt;** Liste mit einer **Foreach** Schleife.</span><span class="sxs-lookup"><span data-stu-id="7d536-485">Loop over the genre list in **StoreIndexViewModel** and create an HTML **&lt;ul&gt;** list using a **foreach** loop.</span></span>
   <span data-ttu-id="7d536-486">(C#)</span><span class="sxs-lookup"><span data-stu-id="7d536-486">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample15.cshtml)]
3. <span data-ttu-id="7d536-487">Drücken Sie **F5** , führen Sie die Anwendung, und navigieren Sie **/speichern**.</span><span class="sxs-lookup"><span data-stu-id="7d536-487">Press **F5** to run the Application and browse **/Store**.</span></span> <span data-ttu-id="7d536-488">Sehen Sie die Liste von Genres übergeben der **StoreIndexViewModel** -Objekt aus der **StoreController** der Vorlage anzeigen.</span><span class="sxs-lookup"><span data-stu-id="7d536-488">You will see the list of genres passed in the **StoreIndexViewModel** object from the **StoreController** to the View template.</span></span>

    <span data-ttu-id="7d536-489">![Anzeigen einer Liste von Genres Ansicht](aspnet-mvc-4-fundamentals/_static/image26.png "anzeigen, die eine Liste von Genres anzeigen")</span><span class="sxs-lookup"><span data-stu-id="7d536-489">![View displaying a list of genres](aspnet-mvc-4-fundamentals/_static/image26.png "View displaying a list of genres")</span></span>

    <span data-ttu-id="7d536-490">*Anzeigen einer Liste von Genres anzeigen*</span><span class="sxs-lookup"><span data-stu-id="7d536-490">*View displaying a list of genres*</span></span>
4. <span data-ttu-id="7d536-491">Schließen Sie den Browser.</span><span class="sxs-lookup"><span data-stu-id="7d536-491">Close the browser.</span></span>

<a id="Exercise6"></a>

<a id="Exercise_6_Using_Parameters_in_View"></a>
### <a name="exercise-6-using-parameters-in-view"></a><span data-ttu-id="7d536-492">Übung 6: Verwenden von Parametern in der Ansicht</span><span class="sxs-lookup"><span data-stu-id="7d536-492">Exercise 6: Using Parameters in View</span></span>

<span data-ttu-id="7d536-493">Übung 3 haben Sie gelernt, wie Parameter an den Controller übergeben wird.</span><span class="sxs-lookup"><span data-stu-id="7d536-493">In Exercise 3 you learned how to pass parameters to the Controller.</span></span> <span data-ttu-id="7d536-494">In dieser Übung erfahren Sie, wie diese Parameter in der Vorlage anzeigen verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="7d536-494">In this exercise, you will learn how to use those parameters in the View template.</span></span> <span data-ttu-id="7d536-495">Zu diesem Zweck werden Sie zuerst mit Modellklassen bekannt gemacht werden, die Sie zum Verwalten Ihrer Daten und Domänenlogik unterstützt.</span><span class="sxs-lookup"><span data-stu-id="7d536-495">For that purpose, you will be introduced first to Model classes that will help you to manage your data and domain logic.</span></span> <span data-ttu-id="7d536-496">Darüber hinaus erfahren Sie, wie Links zu Seiten in der ASP.NET MVC-Anwendung erstellt wird, unabhängig von der Dinge URL-Pfade, die Codierung.</span><span class="sxs-lookup"><span data-stu-id="7d536-496">Additionally, you will learn how to create links to pages inside the ASP.NET MVC application without worrying of things like URL paths encoding.</span></span>

<a id="Ex6Task1"></a>

<a id="Task_1_-_Adding_Model_Classes"></a>
#### <a name="task-1---adding-model-classes"></a><span data-ttu-id="7d536-497">Aufgabe 1: Hinzufügen von Modellklassen</span><span class="sxs-lookup"><span data-stu-id="7d536-497">Task 1 - Adding Model Classes</span></span>

<span data-ttu-id="7d536-498">Im Gegensatz zu ViewModels, die erstellt werden, um Informationen vom Controller an die Ansicht zu übergeben, werden Modellklassen zum enthalten und Verwalten von Daten und Domänenlogik erstellt.</span><span class="sxs-lookup"><span data-stu-id="7d536-498">Unlike ViewModels, which are created just to pass information from the Controller to the View, Model classes are built to contain and manage data and domain logic.</span></span> <span data-ttu-id="7d536-499">In dieser Aufgabe fügen Sie zwei Modellklassen zur Darstellung dieser Konzepte: **"Genre"** und **Album**.</span><span class="sxs-lookup"><span data-stu-id="7d536-499">In this task you will add two model classes to represent these concepts: **Genre** and **Album**.</span></span>

1. <span data-ttu-id="7d536-500">Wenn nicht bereits geöffnet ist, starten Sie **Visual Studio Express für Web**</span><span class="sxs-lookup"><span data-stu-id="7d536-500">If not already open, start **VS Express for Web**</span></span>
2. <span data-ttu-id="7d536-501">In der **Datei** Menü wählen **geöffneten Projekt**.</span><span class="sxs-lookup"><span data-stu-id="7d536-501">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="7d536-502">Navigieren Sie zu, klicken Sie im Dialogfeld "Projekt öffnen" **Source\Ex06 UsingParametersInView\Begin**Option **Begin.sln** , und klicken Sie auf **öffnen**.</span><span class="sxs-lookup"><span data-stu-id="7d536-502">In the Open Project dialog, browse to **Source\Ex06-UsingParametersInView\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="7d536-503">Alternativ können Sie mit der Lösung fortfahren, dass Sie nach Abschluss der vorherigen Übung erhalten haben.</span><span class="sxs-lookup"><span data-stu-id="7d536-503">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="7d536-504">Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung müssen Sie einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren.</span><span class="sxs-lookup"><span data-stu-id="7d536-504">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="7d536-505">Klicken Sie hierzu auf die **Projekt** Menü **NuGet-Pakete verwalten**.</span><span class="sxs-lookup"><span data-stu-id="7d536-505">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="7d536-506">In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.</span><span class="sxs-lookup"><span data-stu-id="7d536-506">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="7d536-507">Schließlich erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.</span><span class="sxs-lookup"><span data-stu-id="7d536-507">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="7d536-508">Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken, die sich in Ihrem Projekt liefern Verringern der Projektgröße.</span><span class="sxs-lookup"><span data-stu-id="7d536-508">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="7d536-509">Mit NuGet-Powertools werden durch Angabe der Paketversionen in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, wenn, das Sie das Projekt ausführen, können.</span><span class="sxs-lookup"><span data-stu-id="7d536-509">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="7d536-510">Deshalb wird müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit.</span><span class="sxs-lookup"><span data-stu-id="7d536-510">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="7d536-511">Hinzufügen einer **"Genre"** Modellschemas.</span><span class="sxs-lookup"><span data-stu-id="7d536-511">Add a **Genre** Model class.</span></span> <span data-ttu-id="7d536-512">Zu diesem Zweck Maustaste die **Modelle** Ordner in der **Projektmappen-Explorer**Option **hinzufügen** und dann die **neues Element** Option.</span><span class="sxs-lookup"><span data-stu-id="7d536-512">To do this, right-click the **Models** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="7d536-513">Klicken Sie unter **Code**, wählen Sie die **Klasse** -Element aus, und nennen Sie die Datei *Genre.cs*, klicken Sie dann auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="7d536-513">Under **Code**, choose the **Class** item and name the file *Genre.cs*, then click **Add**.</span></span>

    <span data-ttu-id="7d536-514">![Hinzufügen einer Klasse](aspnet-mvc-4-fundamentals/_static/image27.png "Hinzufügen einer Klasse")</span><span class="sxs-lookup"><span data-stu-id="7d536-514">![Adding a class](aspnet-mvc-4-fundamentals/_static/image27.png "Adding a class")</span></span>

    <span data-ttu-id="7d536-515">*Ein neues Element hinzufügen*</span><span class="sxs-lookup"><span data-stu-id="7d536-515">*Adding a new item*</span></span>

    <span data-ttu-id="7d536-516">![Hinzufügen von "Genre" Modellklasse](aspnet-mvc-4-fundamentals/_static/image28.png "Modellklasse \"Genre\" hinzufügen")</span><span class="sxs-lookup"><span data-stu-id="7d536-516">![Add Genre Model Class](aspnet-mvc-4-fundamentals/_static/image28.png "Add Genre Model Class")</span></span>

    <span data-ttu-id="7d536-517">*Fügen Sie "Genre"-Modell-Klasse*</span><span class="sxs-lookup"><span data-stu-id="7d536-517">*Add Genre Model Class*</span></span>
4. <span data-ttu-id="7d536-518">Hinzufügen einer **Namen** Eigenschaft der Klasse "Genre".</span><span class="sxs-lookup"><span data-stu-id="7d536-518">Add a **Name** property to the Genre class.</span></span> <span data-ttu-id="7d536-519">Fügen Sie hierzu den folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="7d536-519">To do this, add the following code:</span></span>

    <span data-ttu-id="7d536-520">(Codeausschnitt - *Grundlagen von ASP.NET MVC 4 - Ex6 "Genre"*)</span><span class="sxs-lookup"><span data-stu-id="7d536-520">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 Genre*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample16.cs)]
5. <span data-ttu-id="7d536-521">Befolgen die gleiche Vorgehensweise wie zuvor, Hinzufügen einer **Album** Klasse.</span><span class="sxs-lookup"><span data-stu-id="7d536-521">Following the same procedure as before, add an **Album** class.</span></span> <span data-ttu-id="7d536-522">Zu diesem Zweck Maustaste die **Modelle** Ordner in der **Projektmappen-Explorer**Option **hinzufügen** und dann die **neues Element** Option.</span><span class="sxs-lookup"><span data-stu-id="7d536-522">To do this, right-click the **Models** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="7d536-523">Klicken Sie unter **Code**, wählen Sie die **Klasse** -Element aus, und nennen Sie die Datei *Album.cs*, klicken Sie dann auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="7d536-523">Under **Code**, choose the **Class** item and name the file *Album.cs*, then click **Add**.</span></span>
6. <span data-ttu-id="7d536-524">Fügen Sie zwei Eigenschaften zur Klasse Album: **"Genre"** und **Titel**.</span><span class="sxs-lookup"><span data-stu-id="7d536-524">Add two properties to the Album class: **Genre** and **Title**.</span></span> <span data-ttu-id="7d536-525">Fügen Sie hierzu den folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="7d536-525">To do this, add the following code:</span></span>

    <span data-ttu-id="7d536-526">(Codeausschnitt - *Grundlagen von ASP.NET MVC 4 - Ex6 Album*)</span><span class="sxs-lookup"><span data-stu-id="7d536-526">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 Album*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample17.cs)]

<a id="Ex6Task2"></a>

<a id="Task_2_-_Adding_a_StoreBrowseViewModel"></a>
#### <a name="task-2---adding-a-storebrowseviewmodel"></a><span data-ttu-id="7d536-527">Aufgabe 2: Hinzufügen einer StoreBrowseViewModel</span><span class="sxs-lookup"><span data-stu-id="7d536-527">Task 2 - Adding a StoreBrowseViewModel</span></span>

<span data-ttu-id="7d536-528">Ein **StoreBrowseViewModel** wird in dieser Aufgabe verwendet werden, um die Alben anzeigen, die einem ausgewählten "Genre" entsprechen.</span><span class="sxs-lookup"><span data-stu-id="7d536-528">A **StoreBrowseViewModel** will be used in this task to show the Albums that match a selected Genre.</span></span> <span data-ttu-id="7d536-529">In dieser Aufgabe wird diese Klasse erstellen und fügen Sie dann zwei Eigenschaften zum Behandeln der **"Genre"** und seine **Album**der Liste.</span><span class="sxs-lookup"><span data-stu-id="7d536-529">In this task, you will create this class and then add two properties to handle the **Genre** and its **Album**'s List.</span></span>

1. <span data-ttu-id="7d536-530">Hinzufügen einer **StoreBrowseViewModel** Klasse.</span><span class="sxs-lookup"><span data-stu-id="7d536-530">Add a **StoreBrowseViewModel** class.</span></span> <span data-ttu-id="7d536-531">Zu diesem Zweck Maustaste die **ViewModels** Ordner in der **Projektmappen-Explorer**Option **hinzufügen** und dann die **neues Element** Option.</span><span class="sxs-lookup"><span data-stu-id="7d536-531">To do this, right-click the **ViewModels** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="7d536-532">Klicken Sie unter **Code**, wählen Sie die **Klasse** -Element aus, und nennen Sie die Datei *StoreBrowseViewModel.cs*, klicken Sie dann auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="7d536-532">Under **Code**, choose the **Class** item and name the file *StoreBrowseViewModel.cs*, then click **Add**.</span></span>
2. <span data-ttu-id="7d536-533">Hinzufügen eines Verweises auf die Modelle in **StoreBrowseViewModel** Klasse.</span><span class="sxs-lookup"><span data-stu-id="7d536-533">Add a reference to the Models in **StoreBrowseViewModel** class.</span></span> <span data-ttu-id="7d536-534">Fügen Sie hierzu die folgenden Namespace verwenden:</span><span class="sxs-lookup"><span data-stu-id="7d536-534">To do this, add the following using namespace:</span></span>

    <span data-ttu-id="7d536-535">(Codeausschnitt - *Grundlagen von ASP.NET MVC 4 - Ex6 UsingModel*)</span><span class="sxs-lookup"><span data-stu-id="7d536-535">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 UsingModel*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample18.cs)]
3. <span data-ttu-id="7d536-536">Zwei Eigenschaften hinzufügen **StoreBrowseViewModel** Klasse: **"Genre"** und **Alben**.</span><span class="sxs-lookup"><span data-stu-id="7d536-536">Add two properties to **StoreBrowseViewModel** class: **Genre** and **Albums**.</span></span> <span data-ttu-id="7d536-537">Fügen Sie hierzu den folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="7d536-537">To do this, add the following code:</span></span>

    <span data-ttu-id="7d536-538">(Codeausschnitt - *Grundlagen von ASP.NET MVC 4 - Ex6 ModelProperties*)</span><span class="sxs-lookup"><span data-stu-id="7d536-538">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 ModelProperties*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample19.cs)]

> [!NOTE]
> <span data-ttu-id="7d536-539">Was ist **Liste&lt;Album&gt;**  ?: dieser Definition wird mithilfe der **Liste&lt;T&gt;**  Typ, in dem **T** schränkt die auf welche Elemente dieses Typs **Liste** gehören, in diesem Fall **Album** (oder eines seiner Nachfolger).</span><span class="sxs-lookup"><span data-stu-id="7d536-539">What is **List&lt;Album&gt;** ?: This definition is using the **List&lt;T&gt;** type, where **T** constrains the type to which elements of this **List** belong to, in this case **Album** (or any of its descendants).</span></span>
> 
> <span data-ttu-id="7d536-540">Diese Fähigkeit zum Entwerfen von Klassen und Methoden, die die Angabe von einem oder mehreren Typen verzögern, bis die Klasse oder Methode deklariert und instanziiert, indem Clientcode ein Feature von C#-Sprache ist namens **Generika**.</span><span class="sxs-lookup"><span data-stu-id="7d536-540">This ability to design classes and methods that defer the specification of one or more types until the class or method is declared and instantiated by client code is a feature of the C# language called **Generics**.</span></span>
> 
> <span data-ttu-id="7d536-541">**Liste&lt;T&gt;**  entspricht dem generischen der **ArrayList** geben und steht in den **System.Collections.Generic** Namespace.</span><span class="sxs-lookup"><span data-stu-id="7d536-541">**List&lt;T&gt;** is the generic equivalent of the **ArrayList** type and is available in the **System.Collections.Generic** namespace.</span></span> <span data-ttu-id="7d536-542">Einer der Vorteile der Verwendung von **Generika** ist, da der Typ angegeben wird, Sie nicht überprüfen Vorgänge wie die Elemente in die Umwandlung von Typen kümmern müssen **Album** wie mit einer **ArrayList**.</span><span class="sxs-lookup"><span data-stu-id="7d536-542">One of the benefits of using **generics** is that since the type is specified, you do not need to take care of type checking operations such as casting the elements into **Album** as you would do with an **ArrayList**.</span></span>

<a id="Ex6Task3"></a>

<a id="Task_3_-_Using_the_New_ViewModel_in_the_StoreController"></a>
#### <a name="task-3---using-the-new-viewmodel-in-the-storecontroller"></a><span data-ttu-id="7d536-543">Aufgabe 3: verwenden das neue ViewModel in der StoreController</span><span class="sxs-lookup"><span data-stu-id="7d536-543">Task 3 - Using the New ViewModel in the StoreController</span></span>

<span data-ttu-id="7d536-544">In dieser Aufgabe ändern Sie die **StoreController**des **Durchsuchen** und **Details** Aktionsmethoden der neuen **StoreBrowseViewModel** .</span><span class="sxs-lookup"><span data-stu-id="7d536-544">In this task, you will modify the **StoreController**'s **Browse** and **Details** action methods to use the new **StoreBrowseViewModel**.</span></span>

1. <span data-ttu-id="7d536-545">Hinzufügen eines Verweises auf den Ordner Models in **StoreController** Klasse.</span><span class="sxs-lookup"><span data-stu-id="7d536-545">Add a reference to the Models folder in **StoreController** class.</span></span> <span data-ttu-id="7d536-546">Zu diesem Zweck, erweitern Sie die **Controller** Ordner in der **Projektmappen-Explorer** , und öffnen Sie die **StoreController** Klasse.</span><span class="sxs-lookup"><span data-stu-id="7d536-546">To do this, expand the **Controllers** folder in the **Solution Explorer** and open the **StoreController** class.</span></span> <span data-ttu-id="7d536-547">Fügen Sie dann den folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="7d536-547">Then add the following code:</span></span>

    <span data-ttu-id="7d536-548">(Codeausschnitt - *Grundlagen von ASP.NET MVC 4 - Ex6 UsingModelInController*)</span><span class="sxs-lookup"><span data-stu-id="7d536-548">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 UsingModelInController*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample20.cs)]
2. <span data-ttu-id="7d536-549">Ersetzen Sie die **Durchsuchen** Aktionsmethode mit den **StoreViewBrowseController** Klasse.</span><span class="sxs-lookup"><span data-stu-id="7d536-549">Replace the **Browse** action method to use the **StoreViewBrowseController** class.</span></span> <span data-ttu-id="7d536-550">Erstellen Sie eine "Genre" und zwei neue Alben Objekte mit dummy-Daten (in der nächsten praktische Übungseinheit verwenden Sie echte Daten von einer Datenbank).</span><span class="sxs-lookup"><span data-stu-id="7d536-550">You will create a Genre and two new Albums objects with dummy data (in the next Hands-on Lab you will consume real data from a database).</span></span> <span data-ttu-id="7d536-551">Ersetzen Sie hierzu die **Durchsuchen** -Methode durch folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="7d536-551">To do this, replace the **Browse** method with the following code:</span></span>

    <span data-ttu-id="7d536-552">(Codeausschnitt - *Grundlagen von ASP.NET MVC 4 - Ex6 BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="7d536-552">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 BrowseMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample21.cs)]
3. <span data-ttu-id="7d536-553">Ersetzen Sie die **Details** Aktionsmethode mit den **StoreViewBrowseController** Klasse.</span><span class="sxs-lookup"><span data-stu-id="7d536-553">Replace the **Details** action method to use the **StoreViewBrowseController** class.</span></span> <span data-ttu-id="7d536-554">Erstellen Sie ein neues **Album** zu zurückzugebenden Objekt zu den **Ansicht**.</span><span class="sxs-lookup"><span data-stu-id="7d536-554">You will create a new **Album** object to be returned to the **View**.</span></span> <span data-ttu-id="7d536-555">Ersetzen Sie hierzu die **Details** -Methode durch folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="7d536-555">To do this, replace the **Details** method with the following code:</span></span>

    <span data-ttu-id="7d536-556">(Codeausschnitt - *Grundlagen von ASP.NET MVC 4 - Ex6 DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="7d536-556">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 DetailsMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample22.cs)]

<a id="Ex6Task4"></a>

<a id="Task_4_-_Adding_a_Browse_View_Template"></a>
#### <a name="task-4---adding-a-browse-view-template"></a><span data-ttu-id="7d536-557">Aufgabe 4: Hinzufügen eines Sicht zum Durchsuchen einer Vorlage</span><span class="sxs-lookup"><span data-stu-id="7d536-557">Task 4 - Adding a Browse View Template</span></span>

<span data-ttu-id="7d536-558">In dieser Aufgabe fügen Sie eine **Durchsuchen** Anzeigen von Alben für einen bestimmten "Genre" gefunden.</span><span class="sxs-lookup"><span data-stu-id="7d536-558">In this task, you will add a **Browse** View to show the Albums found for a specific Genre.</span></span>

1. <span data-ttu-id="7d536-559">Vor dem Erstellen der neuen Vorlage anzeigen, erstellen Sie das Projekt, damit die **Ansicht hinzufügen** Dialogfeld kennt die **ViewModel** -Klasse.</span><span class="sxs-lookup"><span data-stu-id="7d536-559">Before creating the new View template, you should build the project so that the **Add View** Dialog knows about the **ViewModel** class to use.</span></span> <span data-ttu-id="7d536-560">Erstellen des Projekts durch Auswahl der **erstellen** Menüelement und dann **MvcMusicStore erstellen**.</span><span class="sxs-lookup"><span data-stu-id="7d536-560">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>
2. <span data-ttu-id="7d536-561">Hinzufügen einer **Durchsuchen** anzeigen.</span><span class="sxs-lookup"><span data-stu-id="7d536-561">Add a **Browse** View.</span></span> <span data-ttu-id="7d536-562">Zu diesem Zweck mit der Maustaste in die **Durchsuchen** Aktionsmethode von der **StoreController** , und klicken Sie auf **Ansicht hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="7d536-562">To do this, right-click in the **Browse** action method of the **StoreController** and click **Add View**.</span></span>
3. <span data-ttu-id="7d536-563">In der **Ansicht hinzufügen** Dialogfeld Vergewissern Sie sich, dass der Ansichtsname ist **Durchsuchen**.</span><span class="sxs-lookup"><span data-stu-id="7d536-563">In the **Add View** dialog box, verify that the View Name is **Browse**.</span></span> <span data-ttu-id="7d536-564">Überprüfen Sie die **eine stark typisierte Ansicht erstellen** Kontrollkästchen, und wählen Sie **StoreBrowseViewModel** aus der **Modellschemas** Dropdownliste.</span><span class="sxs-lookup"><span data-stu-id="7d536-564">Check the **Create a strongly-typed view** checkbox and select **StoreBrowseViewModel** from the **Model class** dropdown.</span></span> <span data-ttu-id="7d536-565">Lassen Sie die anderen Felder, die Standardwerte.</span><span class="sxs-lookup"><span data-stu-id="7d536-565">Leave the other fields with their default value.</span></span> <span data-ttu-id="7d536-566">Klicken Sie anschließend auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="7d536-566">Then click **Add**.</span></span>

    <span data-ttu-id="7d536-567">![Hinzufügen einer Ansicht durchsuchen](aspnet-mvc-4-fundamentals/_static/image29.png "Hinzufügen einer Ansicht durchsuchen")</span><span class="sxs-lookup"><span data-stu-id="7d536-567">![Adding a Browse View](aspnet-mvc-4-fundamentals/_static/image29.png "Adding a Browse View")</span></span>

    <span data-ttu-id="7d536-568">*Hinzufügen einer Ansicht durchsuchen*</span><span class="sxs-lookup"><span data-stu-id="7d536-568">*Adding a Browse View*</span></span>
4. <span data-ttu-id="7d536-569">Ändern der **Browse.cshtml** zum Anzeigen von Informationen für die "Genre", den Zugriff auf die **StoreBrowseViewModel** -Objekt, das an die Ansichtenvorlage übergeben wird.</span><span class="sxs-lookup"><span data-stu-id="7d536-569">Modify the **Browse.cshtml** to display the Genre's information, accessing the **StoreBrowseViewModel** object that is passed to the view template.</span></span> <span data-ttu-id="7d536-570">Ersetzen Sie dazu den Inhalt durch Folgendes: (c#)</span><span class="sxs-lookup"><span data-stu-id="7d536-570">To do this, replace the content with the following: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample23.cshtml)]

<a id="Ex6Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="7d536-571">Aufgabe 5: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="7d536-571">Task 5 - Running the Application</span></span>

<span data-ttu-id="7d536-572">In dieser Aufgabe testen Sie, die die **Durchsuchen** Methode ruft Alben aus der **Durchsuchen** Methodenaktion.</span><span class="sxs-lookup"><span data-stu-id="7d536-572">In this task, you will test that the **Browse** method retrieves Albums from the **Browse** method action.</span></span>

1. <span data-ttu-id="7d536-573">Drücken Sie **F5** um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="7d536-573">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="7d536-574">Das Projekt wird auf der Startseite gestartet.</span><span class="sxs-lookup"><span data-stu-id="7d536-574">The project starts in the Home page.</span></span> <span data-ttu-id="7d536-575">Ändern Sie die URL zum   **/speichern/suchen? "Genre" = Disco** , stellen Sie sicher, dass die Aktion zwei Alben zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="7d536-575">Change the URL to **/Store/Browse?Genre=Disco** to verify that the action returns two Albums.</span></span>

    <span data-ttu-id="7d536-576">![Disco Alben Store durchsuchen](aspnet-mvc-4-fundamentals/_static/image30.png "Disco Alben Store durchsuchen")</span><span class="sxs-lookup"><span data-stu-id="7d536-576">![Browsing Store Disco Albums](aspnet-mvc-4-fundamentals/_static/image30.png "Browsing Store Disco Albums")</span></span>

    <span data-ttu-id="7d536-577">*Disco Alben Store durchsuchen*</span><span class="sxs-lookup"><span data-stu-id="7d536-577">*Browsing Store Disco Albums*</span></span>

<a id="Ex6Task6"></a>

<a id="Task_6_-_Displaying_information_About_a_Specific_Album"></a>
#### <a name="task-6---displaying-information-about-a-specific-album"></a><span data-ttu-id="7d536-578">Aufgabe 6: Anzeigen von Informationen zu bestimmten Album</span><span class="sxs-lookup"><span data-stu-id="7d536-578">Task 6 - Displaying information About a Specific Album</span></span>

<span data-ttu-id="7d536-579">In diesem Schritt implementieren Sie die **Store/Detail-** können Sie Informationen zu einem bestimmten Album anzeigen.</span><span class="sxs-lookup"><span data-stu-id="7d536-579">In this task, you will implement the **Store/Details** view to display information about a specific album.</span></span> <span data-ttu-id="7d536-580">In dieser praktischen Übungseinheit alles, was Sie über das Album zeigt ist bereits Bestandteil der **Ansicht** Vorlage.</span><span class="sxs-lookup"><span data-stu-id="7d536-580">In this Hands-on Lab, everything you will display about the album is already contained in the **View** template.</span></span> <span data-ttu-id="7d536-581">In diesem Fall erstellen Sie anstelle einer **StoreDetailsViewModel** -Klasse, verwenden Sie die aktuelle **StoreBrowseViewModel** Vorlage Album an sie übergibt.</span><span class="sxs-lookup"><span data-stu-id="7d536-581">So, instead of creating a **StoreDetailsViewModel** class, you will use the current **StoreBrowseViewModel** template passing the Album to it.</span></span>

1. <span data-ttu-id="7d536-582">Schließen Sie den Browser, die ggf. zu Visual Studio-Fenster zurückzukehren.</span><span class="sxs-lookup"><span data-stu-id="7d536-582">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="7d536-583">Fügen Sie einen neuen **Details** anzeigen für die **StoreController**des **Details** Aktionsmethode.</span><span class="sxs-lookup"><span data-stu-id="7d536-583">Add a new **Details** view for the **StoreController**'s **Details** action method.</span></span> <span data-ttu-id="7d536-584">Zu diesem Zweck Maustaste die **Details** Methode in der **StoreController** Klasse, und klicken Sie auf **Ansicht hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="7d536-584">To do this, right-click the **Details** method in the **StoreController** class and click **Add View**.</span></span>
2. <span data-ttu-id="7d536-585">In der **Ansicht hinzufügen** Dialogfeld, überprüfen Sie, ob die **Sichtname** ist **Details**.</span><span class="sxs-lookup"><span data-stu-id="7d536-585">In the **Add View** dialog, verify that the **View Name** is **Details**.</span></span> <span data-ttu-id="7d536-586">Überprüfen der **eine stark typisierte Ansicht erstellen** Kontrollkästchen, und wählen Sie **Album** aus der **Modellschemas** Dropdownliste aus.</span><span class="sxs-lookup"><span data-stu-id="7d536-586">Check the **Create a strongly-typed view** checkbox and select **Album** from the **Model class** drop-down.</span></span> <span data-ttu-id="7d536-587">Lassen Sie die anderen Felder, die Standardwerte.</span><span class="sxs-lookup"><span data-stu-id="7d536-587">Leave the other fields with their default value.</span></span> <span data-ttu-id="7d536-588">Klicken Sie anschließend auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="7d536-588">Then click **Add**.</span></span> <span data-ttu-id="7d536-589">Dies erstellt und öffnet eine **\Views\Store\Details.cshtml** Datei.</span><span class="sxs-lookup"><span data-stu-id="7d536-589">This will create and open a **\Views\Store\Details.cshtml** file.</span></span>

    <span data-ttu-id="7d536-590">![Hinzufügen einer Detailansicht](aspnet-mvc-4-fundamentals/_static/image31.png "eine Detailansicht hinzufügen")</span><span class="sxs-lookup"><span data-stu-id="7d536-590">![Adding a Details View](aspnet-mvc-4-fundamentals/_static/image31.png "Adding a Details View")</span></span>

    <span data-ttu-id="7d536-591">*Hinzufügen einer Detailansicht*</span><span class="sxs-lookup"><span data-stu-id="7d536-591">*Adding a Details View*</span></span>
3. <span data-ttu-id="7d536-592">Ändern der **Details.cshtml** Datei auf dem Album Informationen anzuzeigen, den Zugriff auf die **Album** -Objekt, das an die Ansichtenvorlage übergeben wird.</span><span class="sxs-lookup"><span data-stu-id="7d536-592">Modify the **Details.cshtml** file to display the Album's information, accessing the **Album** object that is passed to the view template.</span></span> <span data-ttu-id="7d536-593">Ersetzen Sie dazu den Inhalt durch Folgendes: (c#)</span><span class="sxs-lookup"><span data-stu-id="7d536-593">To do this, replace the content with the following: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample24.cshtml)]

<a id="Ex6Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a><span data-ttu-id="7d536-594">Aufgabe 7: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="7d536-594">Task 7 - Running the Application</span></span>

<span data-ttu-id="7d536-595">In dieser Aufgabe testen Sie, die die **Details** Sicht Ruft Informationen zu den Album aus der **Details Aktion** Methode.</span><span class="sxs-lookup"><span data-stu-id="7d536-595">In this task, you will test that the **Details** View retrieves Album's information from the **Details action** method.</span></span>

1. <span data-ttu-id="7d536-596">Drücken Sie **F5** um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="7d536-596">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="7d536-597">Das Projekt gestartet wird, der **Home** Seite.</span><span class="sxs-lookup"><span data-stu-id="7d536-597">The project starts in the **Home** page.</span></span> <span data-ttu-id="7d536-598">Ändern Sie die URL zum **/Store/Details/5** das Album Informationen überprüfen.</span><span class="sxs-lookup"><span data-stu-id="7d536-598">Change the URL to **/Store/Details/5** to verify the album's information.</span></span>

    <span data-ttu-id="7d536-599">![Durchsuchen von Alben Detail](aspnet-mvc-4-fundamentals/_static/image32.png "Alben Detail durchsuchen")</span><span class="sxs-lookup"><span data-stu-id="7d536-599">![Browsing Albums Detail](aspnet-mvc-4-fundamentals/_static/image32.png "Browsing Albums Detail")</span></span>

    <span data-ttu-id="7d536-600">*Durchsuchen des Albums Detail*</span><span class="sxs-lookup"><span data-stu-id="7d536-600">*Browsing Album's Detail*</span></span>

<a id="Ex6Task8"></a>

<a id="Task_8_-_Adding_Links_Between_Pages"></a>
#### <a name="task-8---adding-links-between-pages"></a><span data-ttu-id="7d536-601">Aufgabe 8: Hinzufügen von Verknüpfungen zwischen Seiten</span><span class="sxs-lookup"><span data-stu-id="7d536-601">Task 8 - Adding Links Between Pages</span></span>

<span data-ttu-id="7d536-602">In dieser Aufgabe fügen Sie einen Link in der Ansicht speichern, haben einen Link in jedem Namen "Genre" in das entsprechende **/Store/durchsuchen** URL.</span><span class="sxs-lookup"><span data-stu-id="7d536-602">In this task, you will add a link in the Store View to have a link in every Genre name to the appropriate **/Store/Browse** URL.</span></span> <span data-ttu-id="7d536-603">Wenn Sie z. B. auf eine "Genre", klicken auf diese Weise **Disco**, wird es navigieren Sie zu **/Store/durchsuchen? "Genre" = Disco** URL.</span><span class="sxs-lookup"><span data-stu-id="7d536-603">This way, when you click on a Genre, for instance **Disco**, it will navigate to **/Store/Browse?genre=Disco** URL.</span></span>

1. <span data-ttu-id="7d536-604">Schließen Sie den Browser, die ggf. zu Visual Studio-Fenster zurückzukehren.</span><span class="sxs-lookup"><span data-stu-id="7d536-604">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="7d536-605">Update der **Index** Seite, um einen Link zum Hinzufügen der **Durchsuchen** Seite.</span><span class="sxs-lookup"><span data-stu-id="7d536-605">Update the **Index** page to add a link to the **Browse** page.</span></span> <span data-ttu-id="7d536-606">Klicken Sie hierzu in der **Projektmappen-Explorer** erweitern Sie die **Ansichten** Ordner, und dann die **Store** Ordner und doppelklicken Sie auf die **Index.cshtml** Seite.</span><span class="sxs-lookup"><span data-stu-id="7d536-606">To do this, in the **Solution Explorer** expand the **Views** folder, then the **Store** folder and double-click the **Index.cshtml** page.</span></span>
2. <span data-ttu-id="7d536-607">Fügen Sie einen Link auf die Ansicht durchsuchen, der angibt, die "Genre" ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="7d536-607">Add a link to the Browse view indicating the genre selected.</span></span> <span data-ttu-id="7d536-608">Ersetzen Sie hierzu die folgenden hervorgehobenen Code innerhalb der **&lt;li&gt;** Tags: (c#)</span><span class="sxs-lookup"><span data-stu-id="7d536-608">To do this, replace the following highlighted code within the **&lt;li&gt;** tags: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample25.cshtml)]

   > [!NOTE]
   > <span data-ttu-id="7d536-609">ein anderer Ansatz würde direkt auf der Seite mit einem Code wie den folgenden verknüpfen:</span><span class="sxs-lookup"><span data-stu-id="7d536-609">another approach would be linking directly to the page, with a code like the following:</span></span>
   > 
   > <span data-ttu-id="7d536-610">&lt;ein Href =&quot;/Store/durchsuchen? "Genre" =@genreName&quot;&gt;@genreName &lt; /a&gt;</span><span class="sxs-lookup"><span data-stu-id="7d536-610">&lt;a href=&quot;/Store/Browse?genre=@genreName&quot;&gt;@genreName&lt;/a&gt;</span></span>
   > 
   > <span data-ttu-id="7d536-611">Obwohl dieser Ansatz funktioniert, hängt es eine hartcodierte Zeichenfolge ein.</span><span class="sxs-lookup"><span data-stu-id="7d536-611">Although this approach works, it depends on a hardcoded string.</span></span> <span data-ttu-id="7d536-612">Wenn Sie später den Controller umbenennen, müssen Sie diese Anweisung manuell ändern.</span><span class="sxs-lookup"><span data-stu-id="7d536-612">If you later rename the Controller, you will have to change this instruction manually.</span></span> <span data-ttu-id="7d536-613">Eine bessere Alternative ist die Verwendung einer **HTML-Hilfsobjekt** Methode.</span><span class="sxs-lookup"><span data-stu-id="7d536-613">A better alternative is to use an **HTML Helper** method.</span></span> <span data-ttu-id="7d536-614">ASP.NET MVC umfasst eine HTML-Hilfsobjekt-Methode die für Aufgaben verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="7d536-614">ASP.NET MVC includes an HTML Helper method which is available for such tasks.</span></span> <span data-ttu-id="7d536-615">Die **Html.ActionLink()** Hilfsmethode erleichtert das Erstellen von HTML **&lt;eine&gt;** Links, die sicherstellen, dass URL-Pfade sind ordnungsgemäß URL-codiert.</span><span class="sxs-lookup"><span data-stu-id="7d536-615">The **Html.ActionLink()** helper method makes it easy to build HTML **&lt;a&gt;** links, making sure URL paths are properly URL encoded.</span></span>
   > 
   > <span data-ttu-id="7d536-616">Htlm.ActionLink weist mehrere Überladungen.</span><span class="sxs-lookup"><span data-stu-id="7d536-616">Htlm.ActionLink has several overloads.</span></span> <span data-ttu-id="7d536-617">In dieser Übung werden Sie eine verwenden, die drei Parameter akzeptiert:</span><span class="sxs-lookup"><span data-stu-id="7d536-617">In this exercise you will use one that takes three parameters:</span></span>
   > 
   > 1. <span data-ttu-id="7d536-618">Text des Links, der den Namen "Genre" angezeigt wird</span><span class="sxs-lookup"><span data-stu-id="7d536-618">Link text, which will display the Genre name</span></span>
   > 2. <span data-ttu-id="7d536-619">Controller Aktionsname (**Durchsuchen**)</span><span class="sxs-lookup"><span data-stu-id="7d536-619">Controller action name (**Browse**)</span></span>
   > 3. <span data-ttu-id="7d536-620">Weiterleiten von Parameterwerten, die beide den Namen angeben (**"Genre"**) und dem Wert (**"Genre" Namen**)</span><span class="sxs-lookup"><span data-stu-id="7d536-620">Route parameter values, specifying both the name (**Genre**) and the value (**Genre name**)</span></span>

<a id="Ex6Task9"></a>

<a id="Task_9_-_Running_the_Application"></a>
#### <a name="task-9---running-the-application"></a><span data-ttu-id="7d536-621">Aufgabe 9: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="7d536-621">Task 9 - Running the Application</span></span>

<span data-ttu-id="7d536-622">In dieser Aufgabe testen Sie, dass jede "Genre", mit einem Link zu der entsprechenden angezeigt wird **/Store/durchsuchen** URL.</span><span class="sxs-lookup"><span data-stu-id="7d536-622">In this task, you will test that each Genre is displayed with a link to the appropriate **/Store/Browse** URL.</span></span>

1. <span data-ttu-id="7d536-623">Drücken Sie **F5** um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="7d536-623">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="7d536-624">Das Projekt wird auf der Startseite gestartet.</span><span class="sxs-lookup"><span data-stu-id="7d536-624">The project starts in the Home page.</span></span> <span data-ttu-id="7d536-625">Ändern Sie die URL zum **/speichern** um sicherzustellen, dass jedes Genres an die entsprechende links **/Store/durchsuchen** URL.</span><span class="sxs-lookup"><span data-stu-id="7d536-625">Change the URL to **/Store** to verify that each Genre links to the appropriate **/Store/Browse** URL.</span></span>

    <span data-ttu-id="7d536-626">![Durchsuchen von Genres mit Links auf der Seite "Durchsuchen"](aspnet-mvc-4-fundamentals/_static/image33.png "durchsuchen Genres mit Links auf der Seite \"Durchsuchen\"")</span><span class="sxs-lookup"><span data-stu-id="7d536-626">![Browsing Genres with links to Browse page](aspnet-mvc-4-fundamentals/_static/image33.png "Browsing Genres with links to Browse page")</span></span>

    <span data-ttu-id="7d536-627">*Durchsuchen von Genres mit Links auf der Seite "Durchsuchen"*</span><span class="sxs-lookup"><span data-stu-id="7d536-627">*Browsing Genres with links to Browse page*</span></span>

<a id="Ex6Task10"></a>

<a id="Task_10_-_Using_Dynamic_ViewModel_Collection_to_Pass_Values"></a>
#### <a name="task-10---using-dynamic-viewmodel-collection-to-pass-values"></a><span data-ttu-id="7d536-628">Aufgabe 10 - mit dynamischen ViewModel Auflistung zum Übergeben von Werten</span><span class="sxs-lookup"><span data-stu-id="7d536-628">Task 10 - Using Dynamic ViewModel Collection to Pass Values</span></span>

<span data-ttu-id="7d536-629">In dieser Aufgabe erfahren Sie, eine einfache und leistungsstarke Methode zum Übergeben von Werten zwischen dem Controller und die Ansicht, ohne Änderungen im Modell.</span><span class="sxs-lookup"><span data-stu-id="7d536-629">In this task, you will learn a simple and powerful method to pass values between the Controller and the View without making any changes in the Model.</span></span> <span data-ttu-id="7d536-630">ASP.NET MVC 4 stellt die Auflistung &quot;ViewModel&quot;, dynamische ausnahmslos zugewiesen und innerhalb der Controller und Ansichten ebenfalls zugegriffen werden können.</span><span class="sxs-lookup"><span data-stu-id="7d536-630">ASP.NET MVC 4 provides the collection &quot;ViewModel&quot;, which can be assigned to any dynamic value and accessed inside controllers and views as well.</span></span>

<span data-ttu-id="7d536-631">Verwenden Sie jetzt die ViewBag dynamische Auflistung zum Übergeben von &quot; **mit Stern Genres** &quot; auf dem Controller auf die Ansicht.</span><span class="sxs-lookup"><span data-stu-id="7d536-631">You will now use the ViewBag dynamic collection to pass a list of &quot;**Starred genres**&quot; from the controller to the view.</span></span> <span data-ttu-id="7d536-632">Die Columnstore-Index-Ansicht wird der Zugriff auf **ViewModel** und die Informationen anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="7d536-632">The Store Index view will access to **ViewModel** and display the information.</span></span>

1. <span data-ttu-id="7d536-633">Schließen Sie den Browser, die ggf. zu Visual Studio-Fenster zurückzukehren.</span><span class="sxs-lookup"><span data-stu-id="7d536-633">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="7d536-634">Open **StoreController.cs** und Ändern von **Index** Methode zum Erstellen einer Liste von Genres in ViewModel Auflistung markiert:</span><span class="sxs-lookup"><span data-stu-id="7d536-634">Open **StoreController.cs** and modify **Index** method to create a list of starred genres into ViewModel collection :</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample26.cs)]

    > [!NOTE]
    > <span data-ttu-id="7d536-635">Sie können auch die Syntax **ViewBag [&quot;Starred&quot;]** Zugriff auf die Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="7d536-635">You could also use the syntax **ViewBag[&quot;Starred&quot;]** to access the properties.</span></span>
2. <span data-ttu-id="7d536-636">Das Sternsymbol **&quot;starred.png&quot;** dient in der **Source\Assets\Images** Ordner dieser Anleitung.</span><span class="sxs-lookup"><span data-stu-id="7d536-636">The star icon **&quot;starred.png&quot;** is included in the **Source\Assets\Images** folder of this lab.</span></span> <span data-ttu-id="7d536-637">Um die Anwendung hinzugefügt haben, ziehen Sie ihren Inhalt aus einem **Windows Explorer** Fenster in der **Projektmappen-Explorer** in Visual Web Developer Express, wie unten dargestellt:</span><span class="sxs-lookup"><span data-stu-id="7d536-637">In order to add it to the application, drag their content from a **Windows Explorer** window into the **Solution Explorer** in Visual Web Developer Express, as shown below:</span></span>

    <span data-ttu-id="7d536-638">![Stern Bild hinzufügen, der Projektmappe](aspnet-mvc-4-fundamentals/_static/image34.png "Stern Bild der Projektmappe hinzufügen")</span><span class="sxs-lookup"><span data-stu-id="7d536-638">![Adding star image to the solution](aspnet-mvc-4-fundamentals/_static/image34.png "Adding star image to the solution")</span></span>

    <span data-ttu-id="7d536-639">*Stern Bild hinzufügen zur Projektmappe*</span><span class="sxs-lookup"><span data-stu-id="7d536-639">*Adding star image to the solution*</span></span>
3. <span data-ttu-id="7d536-640">Öffnen Sie die Ansicht **Store/Index.cshtml** und ändern Sie den Inhalt.</span><span class="sxs-lookup"><span data-stu-id="7d536-640">Open the view **Store/Index.cshtml** and modify the content.</span></span> <span data-ttu-id="7d536-641">Sie liest die &quot;mit Stern&quot; Eigenschaft in der **ViewBag** -Auflistung, und gefragt, ob der Name der aktuellen "Genre" in der Liste befindet.</span><span class="sxs-lookup"><span data-stu-id="7d536-641">You will read the &quot;starred&quot; property in the **ViewBag** collection, and ask if the current genre name is in the list.</span></span> <span data-ttu-id="7d536-642">In diesem Fall zeigt das einem Sternsymbol rechts auf den Link "Genre".</span><span class="sxs-lookup"><span data-stu-id="7d536-642">In that case you will show a star icon right to the genre link.</span></span>
   <span data-ttu-id="7d536-643">(C#)</span><span class="sxs-lookup"><span data-stu-id="7d536-643">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample27.cshtml)]

<a id="Ex6Task11"></a>

<a id="Task_11_-_Running_the_Application"></a>
#### <a name="task-11---running-the-application"></a><span data-ttu-id="7d536-644">Aufgabe 11: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="7d536-644">Task 11 - Running the Application</span></span>

<span data-ttu-id="7d536-645">In dieser Aufgabe testen Sie, dass die mit Sternchen Genres einem Sternsymbol angezeigt.</span><span class="sxs-lookup"><span data-stu-id="7d536-645">In this task, you will test that the starred genres display a star icon.</span></span>

1. <span data-ttu-id="7d536-646">Drücken Sie **F5** um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="7d536-646">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="7d536-647">Das Projekt gestartet wird, der **Home** Seite.</span><span class="sxs-lookup"><span data-stu-id="7d536-647">The project starts in the **Home** page.</span></span> <span data-ttu-id="7d536-648">Ändern Sie die URL zum **/speichern** überprüfen, ob jede ausgewählte "Genre" respektieren Bezeichnung verfügt:</span><span class="sxs-lookup"><span data-stu-id="7d536-648">Change the URL to **/Store** to verify that each featured genre has the respecting label:</span></span>

    <span data-ttu-id="7d536-649">![Durchsuchen von Genres mit Elementen mit Sternchen](aspnet-mvc-4-fundamentals/_static/image35.png "Genres mit Elementen mit Sternchen durchsuchen")</span><span class="sxs-lookup"><span data-stu-id="7d536-649">![Browsing Genres with starred elements](aspnet-mvc-4-fundamentals/_static/image35.png "Browsing Genres with starred elements")</span></span>

    <span data-ttu-id="7d536-650">*Durchsuchen von Genres mit mit Sternchen Elementen*</span><span class="sxs-lookup"><span data-stu-id="7d536-650">*Browsing Genres with starred elements*</span></span>

<a id="Exercise7"></a>

<a id="Exercise_7_A_lap_around_ASPNET_MVC_4_new_template"></a>
### <a name="exercise-7-a-lap-around-aspnet-mvc-4-new-template"></a><span data-ttu-id="7d536-651">Übung 7: Lap um neue ASP.NET MVC 4-Vorlage</span><span class="sxs-lookup"><span data-stu-id="7d536-651">Exercise 7: A lap around ASP.NET MVC 4 new template</span></span>

<span data-ttu-id="7d536-652">In dieser Übung untersuchen Sie die hier enthaltenen Erweiterungen der ASP.NET MVC 4-Projektvorlagen erstellen einem Blick die wichtigsten Features der neuen Vorlage.</span><span class="sxs-lookup"><span data-stu-id="7d536-652">In this exercise, you will explore the enhancements in the ASP.NET MVC 4 project templates, taking a look at the most relevant features of the new template.</span></span>

<a id="Ex7Task1"></a>

<a id="Task_1_Exploring_the_ASPNET_MVC_4_Internet_Application_Template"></a>
#### <a name="task-1-exploring-the-aspnet-mvc-4-internet-application-template"></a><span data-ttu-id="7d536-653">Aufgabe 1: Untersuchen der ASP.NET MVC 4 Internet Application-Vorlage</span><span class="sxs-lookup"><span data-stu-id="7d536-653">Task 1: Exploring the ASP.NET MVC 4 Internet Application Template</span></span>

1. <span data-ttu-id="7d536-654">Wenn nicht bereits geöffnet ist, starten Sie **Visual Studio Express für Web**</span><span class="sxs-lookup"><span data-stu-id="7d536-654">If not already open, start **VS Express for Web**</span></span>
2. <span data-ttu-id="7d536-655">Wählen Sie die **Datei | Neue | Projekt** Menübefehl.</span><span class="sxs-lookup"><span data-stu-id="7d536-655">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="7d536-656">In der **neues Projekt** wählen Sie im Dialogfeld die **Visual C# | Web** Vorlage auf der linken Struktur, und wählen Sie die **ASP.NET MVC 4-Webanwendung**.</span><span class="sxs-lookup"><span data-stu-id="7d536-656">In the **New Project** dialog, select the **Visual C#|Web** template on the left pane tree, and choose the **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="7d536-657">**Namen** des Projekts *MusicStore* und Aktualisieren der **Projektmappenname** auf *beginnen*, dann wählen Sie einen Speicherort (oder übernehmen Sie den Standardwert), und klicken Sie auf **OK** .</span><span class="sxs-lookup"><span data-stu-id="7d536-657">**Name** the project *MusicStore* and update the **solution name** to *Begin*, then select a location (or leave the default) and click **OK**.</span></span>

    <span data-ttu-id="7d536-658">![Erstellen ein neues ASP.NET MVC 4-Projekt](aspnet-mvc-4-fundamentals/_static/image36.png "erstellen ein neues ASP.NET MVC 4-Projekt")</span><span class="sxs-lookup"><span data-stu-id="7d536-658">![Creating a new ASP.NET MVC 4 Project](aspnet-mvc-4-fundamentals/_static/image36.png "Creating a new ASP.NET MVC 4 Project")</span></span>

    <span data-ttu-id="7d536-659">*Erstellen ein neues ASP.NET MVC 4-Projekt*</span><span class="sxs-lookup"><span data-stu-id="7d536-659">*Creating a new ASP.NET MVC 4 Project*</span></span>
3. <span data-ttu-id="7d536-660">In der **neues ASP.NET MVC 4-Projekt** wählen Sie im Dialogfeld die **Internetanwendung** -Projektvorlage, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="7d536-660">In the **New ASP.NET MVC 4 Project** dialog, select the **Internet Application** project template and click **OK**.</span></span> <span data-ttu-id="7d536-661">Beachten Sie, dass Sie das Ansichtsmodul Razor oder ASPX auswählen können.</span><span class="sxs-lookup"><span data-stu-id="7d536-661">Notice you can select either Razor or ASPX as the view engine.</span></span>

    <span data-ttu-id="7d536-662">![Erstellen einer neuen ASP.NET MVC 4 Internetanwendung](aspnet-mvc-4-fundamentals/_static/image37.png "Erstellen einer neuen ASP.NET MVC 4 Internet-Anwendung")</span><span class="sxs-lookup"><span data-stu-id="7d536-662">![Creating a new ASP.NET MVC 4 Internet Application](aspnet-mvc-4-fundamentals/_static/image37.png "Creating a new ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="7d536-663">*Erstellen einer neuen ASP.NET MVC 4 Internet-Anwendung*</span><span class="sxs-lookup"><span data-stu-id="7d536-663">*Creating a new ASP.NET MVC 4 Internet Application*</span></span>

    > [!NOTE]
    > <span data-ttu-id="7d536-664">Razor-Syntax wurde in ASP.NET MVC 3 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="7d536-664">Razor syntax has been introduced in ASP.NET MVC 3.</span></span> <span data-ttu-id="7d536-665">Das Ziel besteht darin, die Anzahl von Zeichen und Tastatureingaben in einer Datei erforderlich, eine schnelle und Fluid Codierung Workflow aktivieren zu minimieren.</span><span class="sxs-lookup"><span data-stu-id="7d536-665">Its goal is to minimize the number of characters and keystrokes required in a file, enabling a fast and fluid coding workflow.</span></span> <span data-ttu-id="7d536-666">Razor nutzt die vorhandenen C#/VB (oder andere) Sprachkenntnisse und übermittelt Sie eine Vorlage Markup-Syntax, die einen awesome HTML-Konstruktion-Workflow ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="7d536-666">Razor leverages existing C#/VB (or other) language skills and delivers a template markup syntax that enables an awesome HTML construction workflow.</span></span>
4. <span data-ttu-id="7d536-667">Drücken Sie **F5** führen Sie die Projektmappe, und die erneuerte Vorlage angezeigt.</span><span class="sxs-lookup"><span data-stu-id="7d536-667">Press **F5** to run the solution and see the renewed template.</span></span> <span data-ttu-id="7d536-668">Sie können die folgenden Funktionen Auschecken:</span><span class="sxs-lookup"><span data-stu-id="7d536-668">You can check out the following features:</span></span>

    1. <span data-ttu-id="7d536-669">**Modernes Vorlagen**</span><span class="sxs-lookup"><span data-stu-id="7d536-669">**Modern-style templates**</span></span>

        <span data-ttu-id="7d536-670">Die Vorlagen haben erneuert wurde, mehr Modern aussehende Arten bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="7d536-670">The templates have been renewed, providing more modern-looking styles.</span></span>

        <span data-ttu-id="7d536-671">![Vorlagen für ASP.NET MVC 4 restyled](aspnet-mvc-4-fundamentals/_static/image38.png "ASP.NET MVC 4 restyled Vorlagen")</span><span class="sxs-lookup"><span data-stu-id="7d536-671">![ASP.NET MVC 4 restyled templates](aspnet-mvc-4-fundamentals/_static/image38.png "ASP.NET MVC 4 restyled templates")</span></span>

        <span data-ttu-id="7d536-672">*ASP.NET MVC 4 restyled Vorlagen*</span><span class="sxs-lookup"><span data-stu-id="7d536-672">*ASP.NET MVC 4 restyled templates*</span></span>
    2. <span data-ttu-id="7d536-673">**Adaptives Rendering**</span><span class="sxs-lookup"><span data-stu-id="7d536-673">**Adaptive Rendering**</span></span>

        <span data-ttu-id="7d536-674">Sehen Sie sich die Größe des Browserfensters, und beachten Sie, wie das Seitenlayout dynamisch an die Größe des neuen Fensters anpasst.</span><span class="sxs-lookup"><span data-stu-id="7d536-674">Check out resizing the browser window and notice how the page layout dynamically adapts to the new window size.</span></span> <span data-ttu-id="7d536-675">Diese Vorlagen mithilfe der adaptiven Renderingtechnik um ordnungsgemäß auf Desktop- und mobile Plattformen ohne Anpassung zu rendern.</span><span class="sxs-lookup"><span data-stu-id="7d536-675">These templates use the adaptive rendering technique to render properly in both desktop and mobile platforms without any customization.</span></span>

        <span data-ttu-id="7d536-676">![ASP.NET MVC 4-Projektvorlage in anderen Browser Größen](aspnet-mvc-4-fundamentals/_static/image39.png "ASP.NET MVC 4-Projektvorlage in anderen Browser Größen")</span><span class="sxs-lookup"><span data-stu-id="7d536-676">![ASP.NET MVC 4 project template in different browser sizes](aspnet-mvc-4-fundamentals/_static/image39.png "ASP.NET MVC 4 project template in different browser sizes")</span></span>

        <span data-ttu-id="7d536-677">*ASP.NET MVC 4-Projektvorlage in anderen Browser Größen*</span><span class="sxs-lookup"><span data-stu-id="7d536-677">*ASP.NET MVC 4 project template in different browser sizes*</span></span>
5. <span data-ttu-id="7d536-678">Schließen Sie den Browser, um den Debugger beenden und zurück zu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7d536-678">Close the browser to stop the debugger and return to Visual Studio.</span></span>
6. <span data-ttu-id="7d536-679">Nun können Sie in der Lage, untersuchen die Projektmappe, und sehen Sie sich einige der neuen Funktionen, die in der Projektvorlage von ASP.NET MVC 4 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="7d536-679">Now you are able to explore the solution and check out some of the new features introduced by ASP.NET MVC 4 in the project template.</span></span>

    <span data-ttu-id="7d536-680">![ASP.NET MVC 4-Internet-Anwendung –--Projektvorlage](aspnet-mvc-4-fundamentals/_static/image40.png "der Projektvorlage Internet ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="7d536-680">![ASP.NET MVC4-internet-application-project-template](aspnet-mvc-4-fundamentals/_static/image40.png "The ASP.NET MVC 4 Internet Application Project Template")</span></span>

    <span data-ttu-id="7d536-681">*Die ASP.NET MVC 4 Internet Application-Projektvorlage*</span><span class="sxs-lookup"><span data-stu-id="7d536-681">*The ASP.NET MVC 4 Internet Application Project Template*</span></span>

   1. <span data-ttu-id="7d536-682">**HTML5-markup**</span><span class="sxs-lookup"><span data-stu-id="7d536-682">**HTML5 markup**</span></span>

       <span data-ttu-id="7d536-683">Durchsuchen Sie die Ansichten der Vorlagen so ermitteln Sie das neue Design Markup, z. B. open **About.cshtml** in anzeigen **Home** Ordner.</span><span class="sxs-lookup"><span data-stu-id="7d536-683">Browse template views to find out the new theme markup, for example open **About.cshtml** view within **Home** folder.</span></span>

       <span data-ttu-id="7d536-684">![Neue Vorlage für die Verwendung von Razor und HTML5-Markups](aspnet-mvc-4-fundamentals/_static/image41.png "neue Vorlage für die Verwendung von Razor und HTML5-Markups")</span><span class="sxs-lookup"><span data-stu-id="7d536-684">![New template, using Razor and HTML5 markup](aspnet-mvc-4-fundamentals/_static/image41.png "New template, using Razor and HTML5 markup")</span></span>

       <span data-ttu-id="7d536-685">*Neue Vorlage für die Verwendung von Razor und HTML5-Markups*</span><span class="sxs-lookup"><span data-stu-id="7d536-685">*New template, using Razor and HTML5 markup*</span></span>
   2. <span data-ttu-id="7d536-686">**JavaScript-Bibliotheken enthalten**</span><span class="sxs-lookup"><span data-stu-id="7d536-686">**JavaScript libraries included**</span></span>

      1. <span data-ttu-id="7d536-687">**jQuery**: jQuery vereinfacht die HTML-Dokument durchlaufen, Ereignisbehandlung, Animation und Ajax-Aktivitäten.</span><span class="sxs-lookup"><span data-stu-id="7d536-687">**jQuery**: jQuery simplifies HTML document traversing, event handling, animating, and Ajax interactions.</span></span>
      2. <span data-ttu-id="7d536-688">**jQuery UI**: Diese Bibliothek bietet Abstraktionen für die Low-Level-Interaktion und Animation, erweiterte Auswirkungen und Design beeinflusst Widgets, baut auf dem jQuery JavaScript-Bibliothek.</span><span class="sxs-lookup"><span data-stu-id="7d536-688">**jQuery UI**: This library provides abstractions for low-level interaction and animation, advanced effects and themeable widgets, built on top of the jQuery JavaScript Library.</span></span>

         > [!NOTE]
         > <span data-ttu-id="7d536-689">Sie können erfahren, jQuery und jQuery UI in [ [ http://docs.jquery.com/ ](http://docs.jquery.com/) ](http://docs.jquery.com/).</span><span class="sxs-lookup"><span data-stu-id="7d536-689">You can learn about jQuery and jQuery UI in [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span></span>
      3. <span data-ttu-id="7d536-690">**KnockoutJS**: der ASP.NET MVC 4-Standardvorlage enthält jetzt **KnockoutJS**, ein MVVM JavaScript-Framework, das umfangreiche und extrem reaktionsschnellen Webanwendungen, die mit JavaScript und HTML erstellen kann.</span><span class="sxs-lookup"><span data-stu-id="7d536-690">**KnockoutJS**: The ASP.NET MVC 4 default template now includes **KnockoutJS**, a JavaScript MVVM framework that lets you create rich and highly responsive web applications using JavaScript and HTML.</span></span> <span data-ttu-id="7d536-691">Wie werden in ASP.NET MVC 3, jQuery und jQuery UI-Bibliotheken ebenfalls in ASP.NET MVC 4 enthalten.</span><span class="sxs-lookup"><span data-stu-id="7d536-691">Like in ASP.NET MVC 3, jQuery and jQuery UI libraries are also included in ASP.NET MVC 4.</span></span>

          > [!NOTE]
          > <span data-ttu-id="7d536-692">Sie erhalten weitere Informationen zur KnockOutJS Library unter diesem Link: [ http://learn.knockoutjs.com/ ](http://learn.knockoutjs.com/).</span><span class="sxs-lookup"><span data-stu-id="7d536-692">You can get more information about KnockOutJS library in this link: [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/).</span></span>
      4. <span data-ttu-id="7d536-693">**Modernizr**: Diese Bibliothek automatisch ausgeführt, macht Ihre Website mit älteren Browsern kompatibel, bei Verwendung von HTML5- und CSS3-Technologien.</span><span class="sxs-lookup"><span data-stu-id="7d536-693">**Modernizr**: This library runs automatically, making your site compatible with older browsers when using HTML5 and CSS3 technologies.</span></span>

          > [!NOTE]
          > <span data-ttu-id="7d536-694">Sie erhalten weitere Informationen zu Modernizr-Bibliothek unter diesem Link: [ http://www.modernizr.com/ ](http://www.modernizr.com/).</span><span class="sxs-lookup"><span data-stu-id="7d536-694">You can get more information about Modernizr library in this link: [http://www.modernizr.com/](http://www.modernizr.com/).</span></span>
   3. <span data-ttu-id="7d536-695">**In der Projektmappe enthaltenen SimpleMembership**</span><span class="sxs-lookup"><span data-stu-id="7d536-695">**SimpleMembership included in the solution**</span></span>

       <span data-ttu-id="7d536-696">SimpleMembership wurde als Ersatz für das vorherige ASP.NET Rollen- und Mitgliedschaftseinstellungen anbietersystem entwickelt.</span><span class="sxs-lookup"><span data-stu-id="7d536-696">SimpleMembership has been designed as a replacement for the previous ASP.NET Role and Membership provider system.</span></span> <span data-ttu-id="7d536-697">Er verfügt über viele neue Features, die für den Entwickler sichere Webseiten in ein flexibleres erleichtern.</span><span class="sxs-lookup"><span data-stu-id="7d536-697">It has many new features that make it easier for the developer to secure web pages in a more flexible way.</span></span>

       <span data-ttu-id="7d536-698">Die Internet-Vorlage bereits eingerichtet hat einige Punkte, die SimpleMembership integrieren, z. B. die AccountController OAuthWebSecurity (für OAuth kontoregistrierung, Login, Verwaltung, usw.) und -Websicherheit verwenden vorbereitet wird.</span><span class="sxs-lookup"><span data-stu-id="7d536-698">The Internet template already has set up a few things to integrate SimpleMembership, for example, the AccountController is prepared to use OAuthWebSecurity (for OAuth account registration, login, management, etc.) and Web Security.</span></span>

       <span data-ttu-id="7d536-699">![SimpleMembership in der Projektmappe enthaltenen](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership in der Projektmappe enthaltenen")</span><span class="sxs-lookup"><span data-stu-id="7d536-699">![SimpleMembership Included in the solution](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership Included in the solution")</span></span>

       <span data-ttu-id="7d536-700">*SimpleMembership in der Projektmappe enthaltenen*</span><span class="sxs-lookup"><span data-stu-id="7d536-700">*SimpleMembership Included in the solution*</span></span>

       > [!NOTE]
       > <span data-ttu-id="7d536-701">Weitere Informationen finden Sie Informationen zu [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) in MSDN.</span><span class="sxs-lookup"><span data-stu-id="7d536-701">Find more information about [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) in MSDN.</span></span>

> [!NOTE]
> <span data-ttu-id="7d536-702">Darüber hinaus können Sie die Bereitstellung dieser Anwendung, die Windows Azure-Websites folgenden [Anhang B: Veröffentlichen einer ASP.NET MVC 4-Anwendung mithilfe von Web Deploy](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="7d536-702">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="7d536-703">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="7d536-703">Summary</span></span>

<span data-ttu-id="7d536-704">Durch diese praktische Übungseinheit haben Sie gelernt, dass die Grundlagen von ASP.NET MVC:</span><span class="sxs-lookup"><span data-stu-id="7d536-704">By completing this Hands-On Lab you have learned the fundamentals of ASP.NET MVC:</span></span>

- <span data-ttu-id="7d536-705">Kernelemente des MVC-Anwendung und ihre Interaktion</span><span class="sxs-lookup"><span data-stu-id="7d536-705">The core elements of an MVC application and how they interact</span></span>
- <span data-ttu-id="7d536-706">Vorgehensweise: Erstellen einer ASP.NET MVC-Anwendung</span><span class="sxs-lookup"><span data-stu-id="7d536-706">How to create an ASP.NET MVC Application</span></span>
- <span data-ttu-id="7d536-707">Zum Hinzufügen und Konfigurieren von Domänencontrollern, um Parameter zu behandeln, übergeben die URL und Abfragezeichenfolge</span><span class="sxs-lookup"><span data-stu-id="7d536-707">How to add and configure Controllers to handle parameters passed through the URL and querystring</span></span>
- <span data-ttu-id="7d536-708">Zum Hinzufügen einer Layoutseite master zum Einrichten einer Vorlage für allgemeine HTML-Inhalt, ein StyleSheet, um das Aussehen und Verhalten und Ansichtenvorlage zum Anzeigen von HTML-Inhalte zu verbessern</span><span class="sxs-lookup"><span data-stu-id="7d536-708">How to add a layout master page to setup a template for common HTML content, a StyleSheet to enhance the look and feel and a View template to display HTML content</span></span>
- <span data-ttu-id="7d536-709">Wie das ViewModel-Muster zur Weitergabe der Eigenschaften der Vorlage anzeigen dynamischer Informationen anzuzeigen</span><span class="sxs-lookup"><span data-stu-id="7d536-709">How to use the ViewModel pattern for passing properties to the View template to display dynamic information</span></span>
- <span data-ttu-id="7d536-710">Verwendung von Parametern übergeben auf Domänencontroller in der Vorlage anzeigen</span><span class="sxs-lookup"><span data-stu-id="7d536-710">How to use parameters passed to Controllers in the View template</span></span>
- <span data-ttu-id="7d536-711">Zum Hinzufügen von Links zu Seiten in der ASP.NET MVC-Anwendung</span><span class="sxs-lookup"><span data-stu-id="7d536-711">How to add links to pages inside the ASP.NET MVC application</span></span>
- <span data-ttu-id="7d536-712">Gewusst wie: Hinzufügen und Verwenden von dynamischen Eigenschaften in einer Ansicht</span><span class="sxs-lookup"><span data-stu-id="7d536-712">How to add and use dynamic properties in a View</span></span>
- <span data-ttu-id="7d536-713">Die Verbesserungen in ASP.NET MVC 4-Projektvorlagen</span><span class="sxs-lookup"><span data-stu-id="7d536-713">The enhancements in the ASP.NET MVC 4 project templates</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="7d536-714">Anhang A: Installieren von Visual Studio Express 2012 für das Web</span><span class="sxs-lookup"><span data-stu-id="7d536-714">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="7d536-715">Sie installieren können **Microsoft Visual Studio Express 2012 für das Web** oder ein anderes &quot;Express&quot; Version mithilfe der **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="7d536-715">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="7d536-716">Die folgenden Anweisungen führen Sie durch die erforderlichen Schritte zum Installieren *Visual Studio Express 2012 für das Web* mit *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="7d536-716">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="7d536-717">Wechseln Sie zu [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="7d536-717">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="7d536-718">Auch wenn Sie bereits Webplattform-Installer installiert haben, können Sie öffnen es, und suchen Sie nach dem Produkt &quot; <em>Visual Studio Express 2012 für das Web mit Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="7d536-718">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="7d536-719">Klicken Sie auf **jetzt installieren**.</span><span class="sxs-lookup"><span data-stu-id="7d536-719">Click on **Install Now**.</span></span> <span data-ttu-id="7d536-720">Wenn Sie keine **Webplattform-Installer** Sie Informationen zum Herunterladen und installieren Sie diese zuerst umgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="7d536-720">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="7d536-721">Einmal **Webplattform-Installer** geöffnet ist, klicken Sie auf **installieren** um das Setup zu starten.</span><span class="sxs-lookup"><span data-stu-id="7d536-721">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="7d536-722">![Visual Studio Express installieren](aspnet-mvc-4-fundamentals/_static/image43.png "Visual Studio Express installieren")</span><span class="sxs-lookup"><span data-stu-id="7d536-722">![Install Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="7d536-723">*Installieren Sie Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="7d536-723">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="7d536-724">Lesen Sie die Produkte, Lizenzen und Begriffe, und klicken Sie auf **ich stimme** um den Vorgang fortzusetzen.</span><span class="sxs-lookup"><span data-stu-id="7d536-724">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Akzeptieren der Lizenzbedingungen](aspnet-mvc-4-fundamentals/_static/image44.png)

    <span data-ttu-id="7d536-726">*Akzeptieren der Lizenzbedingungen*</span><span class="sxs-lookup"><span data-stu-id="7d536-726">*Accepting the license terms*</span></span>
5. <span data-ttu-id="7d536-727">Warten Sie, bis der Prozess herunterladen und die Installation abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="7d536-727">Wait until the downloading and installation process completes.</span></span>

    ![Installationsstatus](aspnet-mvc-4-fundamentals/_static/image45.png)

    <span data-ttu-id="7d536-729">*Installationsstatus*</span><span class="sxs-lookup"><span data-stu-id="7d536-729">*Installation progress*</span></span>
6. <span data-ttu-id="7d536-730">Nach Abschluss der Installation klicken Sie auf **Fertig stellen**.</span><span class="sxs-lookup"><span data-stu-id="7d536-730">When the installation completes, click **Finish**.</span></span>

    ![Installation wurde abgeschlossen](aspnet-mvc-4-fundamentals/_static/image46.png)

    <span data-ttu-id="7d536-732">*Installation wurde abgeschlossen*</span><span class="sxs-lookup"><span data-stu-id="7d536-732">*Installation completed*</span></span>
7. <span data-ttu-id="7d536-733">Klicken Sie auf **beenden** Webplattform-Installer zu schließen.</span><span class="sxs-lookup"><span data-stu-id="7d536-733">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="7d536-734">Um Visual Studio Express für Web zu öffnen, wechseln Sie zu der **starten** Startseite ein, und starten Sie das Schreiben von &quot; **Visual Studio Express**&quot;, klicken Sie dann auf die **Visual Studio Express für Web** Kachel.</span><span class="sxs-lookup"><span data-stu-id="7d536-734">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Visual Studio Express für Web-Kachel](aspnet-mvc-4-fundamentals/_static/image47.png)

    <span data-ttu-id="7d536-736">*Visual Studio Express für Web-Kachel*</span><span class="sxs-lookup"><span data-stu-id="7d536-736">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="7d536-737">Anhang B: Veröffentlichen einer ASP.NET MVC 4-Anwendung mithilfe von Web Deploy</span><span class="sxs-lookup"><span data-stu-id="7d536-737">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="7d536-738">In diesem Anhang wird gezeigt, wie eine neue Website aus dem Windows Azure-Verwaltungsportal erstellen und veröffentlichen Sie die Anwendung, die Sie erhalten haben anhand der testumgebung profitieren von der Web Deploy Veröffentlichungsfunktion von Windows Azure bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="7d536-738">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="7d536-739">Aufgabe 1: Erstellen einer neuen Website aus dem Windows Azure-Portal</span><span class="sxs-lookup"><span data-stu-id="7d536-739">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="7d536-740">Wechseln Sie zu der [Windows Azure-Verwaltungsportal](https://manage.windowsazure.com/) und melden Sie sich mit den Microsoft-Anmeldeinformationen, die Ihrem Abonnement zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="7d536-740">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7d536-741">Sie können mit Windows Azure hosten 10 ASP.NET-Websites kostenlos und skalieren Sie, wenn Ihr Datenverkehr zunimmt.</span><span class="sxs-lookup"><span data-stu-id="7d536-741">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="7d536-742">Sie können registrieren [hier](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="7d536-742">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="7d536-743">![Melden Sie sich bei Windows Azure-Portal](aspnet-mvc-4-fundamentals/_static/image48.png "melden Sie sich bei Windows Azure-Portal")</span><span class="sxs-lookup"><span data-stu-id="7d536-743">![Log on to Windows Azure portal](aspnet-mvc-4-fundamentals/_static/image48.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="7d536-744">*Melden Sie sich bei Windows Azure-Verwaltungsportal*</span><span class="sxs-lookup"><span data-stu-id="7d536-744">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="7d536-745">Klicken Sie auf **neu** in der Befehlszeile.</span><span class="sxs-lookup"><span data-stu-id="7d536-745">Click **New** on the command bar.</span></span>

    <span data-ttu-id="7d536-746">![Erstellen einer neuen Website](aspnet-mvc-4-fundamentals/_static/image49.png "Erstellen einer neuen Website")</span><span class="sxs-lookup"><span data-stu-id="7d536-746">![Creating a new Web Site](aspnet-mvc-4-fundamentals/_static/image49.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="7d536-747">*Erstellen einer neuen Website*</span><span class="sxs-lookup"><span data-stu-id="7d536-747">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="7d536-748">Klicken Sie auf **berechnen** | **Website**.</span><span class="sxs-lookup"><span data-stu-id="7d536-748">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="7d536-749">Wählen Sie dann **Schnellerfassung** Option.</span><span class="sxs-lookup"><span data-stu-id="7d536-749">Then select **Quick Create** option.</span></span> <span data-ttu-id="7d536-750">Verfügbare URL für die neue Website bereitstellen, und klicken Sie auf **Website erstellen**.</span><span class="sxs-lookup"><span data-stu-id="7d536-750">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7d536-751">Eine Windows Azure-Website ist der Host für eine Webanwendung, die ausgeführt wird, in der Cloud, die Sie steuern und verwalten können.</span><span class="sxs-lookup"><span data-stu-id="7d536-751">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="7d536-752">Die Option Schnellerfassung können Sie eine vollständige Webanwendung, die Windows Azure-Website von außerhalb des Portals bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="7d536-752">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="7d536-753">Es umfasst nicht die Schritte zum Einrichten einer Datenbank.</span><span class="sxs-lookup"><span data-stu-id="7d536-753">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="7d536-754">![Erstellen eine neue Website, die mit der Schnellerfassung](aspnet-mvc-4-fundamentals/_static/image50.png "erstellen eine neue Website, die mit der Schnellerfassung")</span><span class="sxs-lookup"><span data-stu-id="7d536-754">![Creating a new Web Site using Quick Create](aspnet-mvc-4-fundamentals/_static/image50.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="7d536-755">*Erstellen eine neue Website, die mit der Schnellerfassung*</span><span class="sxs-lookup"><span data-stu-id="7d536-755">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="7d536-756">Warten Sie, bis die neue **Website** wird erstellt.</span><span class="sxs-lookup"><span data-stu-id="7d536-756">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="7d536-757">Nachdem die Website erstellt wurde, klicken Sie auf den Link unter der **URL** Spalte.</span><span class="sxs-lookup"><span data-stu-id="7d536-757">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="7d536-758">Überprüfen Sie, dass die neue Website funktioniert.</span><span class="sxs-lookup"><span data-stu-id="7d536-758">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="7d536-759">![Um die neue Website durchsuchen](aspnet-mvc-4-fundamentals/_static/image51.png "durchsuchen, um die neue Website")</span><span class="sxs-lookup"><span data-stu-id="7d536-759">![Browsing to the new web site](aspnet-mvc-4-fundamentals/_static/image51.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="7d536-760">*Um die neue Website durchsuchen*</span><span class="sxs-lookup"><span data-stu-id="7d536-760">*Browsing to the new web site*</span></span>

    <span data-ttu-id="7d536-761">![Website](aspnet-mvc-4-fundamentals/_static/image52.png "Website ausgeführt wird")</span><span class="sxs-lookup"><span data-stu-id="7d536-761">![Web site running](aspnet-mvc-4-fundamentals/_static/image52.png "Web site running")</span></span>

    <span data-ttu-id="7d536-762">*Die Website ausgeführt wird*</span><span class="sxs-lookup"><span data-stu-id="7d536-762">*Web site running*</span></span>
6. <span data-ttu-id="7d536-763">Wechseln Sie zurück zum Portal, und klicken Sie auf den Namen der Website unter der **Namen** Spalte die Verwaltungsseiten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="7d536-763">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="7d536-764">![Öffnen die Website-Verwaltungsseiten](aspnet-mvc-4-fundamentals/_static/image53.png "öffnen die Website-Verwaltungsseiten")</span><span class="sxs-lookup"><span data-stu-id="7d536-764">![Opening the web site management pages](aspnet-mvc-4-fundamentals/_static/image53.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="7d536-765">*Öffnen die Website-Verwaltungsseiten*</span><span class="sxs-lookup"><span data-stu-id="7d536-765">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="7d536-766">In der **Dashboard** Seite der **kurzer Blick** auf die **Herunterladen eines Veröffentlichungsprofils** Link.</span><span class="sxs-lookup"><span data-stu-id="7d536-766">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7d536-767">Die *Veröffentlichungsprofil* enthält alle Informationen zum Veröffentlichen einer Webanwendung in einer Windows Azure-Website für die einzelnen aktivierten Veröffentlichungsmethoden erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="7d536-767">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="7d536-768">Das Veröffentlichungsprofil enthält die URLs, Benutzeranmeldeinformationen und datenbankzeichenfolgen, die erforderlich sind, eine Verbindung herstellen und die Authentifizierung für alle Endpunkte für die eine Veröffentlichungsmethode aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="7d536-768">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="7d536-769">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express für Web** und **Microsoft Visual Studio 2012** Unterstützung beim Lesen von veröffentlichungsprofilen zum Automatisieren der Konfiguration dieser Programme für Veröffentlichung von Webanwendungen auf Windows Azure-Websites.</span><span class="sxs-lookup"><span data-stu-id="7d536-769">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="7d536-770">![Herunterladen der Website-Veröffentlichungsprofil](aspnet-mvc-4-fundamentals/_static/image54.png "der Website herunterladen eines Veröffentlichungsprofils")</span><span class="sxs-lookup"><span data-stu-id="7d536-770">![Downloading the web site publish profile](aspnet-mvc-4-fundamentals/_static/image54.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="7d536-771">*Die Website herunterladen eines Veröffentlichungsprofils*</span><span class="sxs-lookup"><span data-stu-id="7d536-771">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="7d536-772">Laden Sie das Veröffentlichungsprofil an einem bekannten Speicherort ein.</span><span class="sxs-lookup"><span data-stu-id="7d536-772">Download the publish profile file to a known location.</span></span> <span data-ttu-id="7d536-773">In dieser Übung wird weiter Gewusst wie: Verwenden Sie diese Datei zum Veröffentlichen einer Webanwendung auf einem Windows Azure-Websites in Visual Studio angezeigt.</span><span class="sxs-lookup"><span data-stu-id="7d536-773">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="7d536-774">![Speichern das Veröffentlichungsprofil](aspnet-mvc-4-fundamentals/_static/image55.png "speichern das Veröffentlichungsprofil")</span><span class="sxs-lookup"><span data-stu-id="7d536-774">![Saving the publish profile file](aspnet-mvc-4-fundamentals/_static/image55.png "Saving the publish profile")</span></span>

    <span data-ttu-id="7d536-775">*Speichern das Veröffentlichungsprofil*</span><span class="sxs-lookup"><span data-stu-id="7d536-775">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="7d536-776">Aufgabe 2: konfigurieren den Datenbankserver</span><span class="sxs-lookup"><span data-stu-id="7d536-776">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="7d536-777">Wenn die Anwendung durchführt, verwenden Sie SQL Server Datenbanken Sie einen SQL-Datenbankserver erstellen müssen.</span><span class="sxs-lookup"><span data-stu-id="7d536-777">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="7d536-778">Wenn eine einfache Anwendung bereitstellen, die keine SQL Server verwendet werden sollen, können Sie diese Aufgabe überspringen.</span><span class="sxs-lookup"><span data-stu-id="7d536-778">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="7d536-779">Sie benötigen einen SQL-Datenbankserver zum Speichern der Anwendungsdatenbank.</span><span class="sxs-lookup"><span data-stu-id="7d536-779">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="7d536-780">Sehen Sie die SQL-Datenbankservern über Ihr Abonnement in Windows Azure-Verwaltungsportal am **Sql-Datenbanken** | **Server** | **des Servers Dashboard**.</span><span class="sxs-lookup"><span data-stu-id="7d536-780">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="7d536-781">Wenn Sie keinen Server erstellt haben, können Sie erstellen, mit der **hinzufügen** auf der Befehlsleiste auf die Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="7d536-781">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="7d536-782">Notieren Sie die **Servername und -URL, Administrator-Benutzername und Kennwort**, wie Sie sie in den nächsten Aufgaben verwenden.</span><span class="sxs-lookup"><span data-stu-id="7d536-782">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="7d536-783">Erstellen Sie die Datenbank nicht noch, wie er in einer späteren Phase erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="7d536-783">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="7d536-784">![SQL-Datenbank-Dashboard](aspnet-mvc-4-fundamentals/_static/image56.png "SQL-Datenbank-Dashboard")</span><span class="sxs-lookup"><span data-stu-id="7d536-784">![SQL Database Server Dashboard](aspnet-mvc-4-fundamentals/_static/image56.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="7d536-785">*SQL-Datenbank-Dashboard*</span><span class="sxs-lookup"><span data-stu-id="7d536-785">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="7d536-786">In der nächsten Aufgabe testen Sie die Verbindung mit der Datenbank, aus Visual Studio aus diesem Grund Sie die lokale IP-Adresse in der Serverliste des müssen **zulässigen IP-Adressen**.</span><span class="sxs-lookup"><span data-stu-id="7d536-786">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="7d536-787">Klicken Sie hierzu auf **konfigurieren**, wählen Sie die IP-Adresse von **aktuelle Client-IP-Adresse** und fügen Sie ihn auf die **IP-Startadresse** und **IP-Endadresse** Textfelder, und klicken Sie auf die ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="7d536-787">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) button.</span></span>

    ![Client-IP-Adresse hinzufügen](aspnet-mvc-4-fundamentals/_static/image58.png)

    <span data-ttu-id="7d536-789">*Client-IP-Adresse hinzufügen*</span><span class="sxs-lookup"><span data-stu-id="7d536-789">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="7d536-790">Einmal die **IP-Clientadresse** wird zu den zulässigen IP-Adressen hinzugefügt Liste, klicken Sie auf **speichern** um die Änderungen zu bestätigen.</span><span class="sxs-lookup"><span data-stu-id="7d536-790">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Bestätigen von Änderungen](aspnet-mvc-4-fundamentals/_static/image59.png)

    <span data-ttu-id="7d536-792">*Bestätigen von Änderungen*</span><span class="sxs-lookup"><span data-stu-id="7d536-792">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="7d536-793">Aufgabe 3: Veröffentlichen einer ASP.NET MVC 4-Anwendung mithilfe von Web Deploy</span><span class="sxs-lookup"><span data-stu-id="7d536-793">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="7d536-794">Wechseln Sie zurück, die ASP.NET MVC 4-Projektmappe.</span><span class="sxs-lookup"><span data-stu-id="7d536-794">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="7d536-795">In der **Projektmappen-Explorer**mit der rechten Maustaste auf das Websiteprojekt, und wählen Sie **veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="7d536-795">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="7d536-796">![Veröffentlichen der Anwendung](aspnet-mvc-4-fundamentals/_static/image60.png "Veröffentlichen der Anwendung")</span><span class="sxs-lookup"><span data-stu-id="7d536-796">![Publishing the Application](aspnet-mvc-4-fundamentals/_static/image60.png "Publishing the Application")</span></span>

    <span data-ttu-id="7d536-797">*Veröffentlichen der Website*</span><span class="sxs-lookup"><span data-stu-id="7d536-797">*Publishing the web site*</span></span>
2. <span data-ttu-id="7d536-798">Importieren Sie das Veröffentlichungsprofil, das Sie in der ersten Aufgabe gespeichert.</span><span class="sxs-lookup"><span data-stu-id="7d536-798">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="7d536-799">![Importieren des Veröffentlichungsprofils](aspnet-mvc-4-fundamentals/_static/image61.png "Importieren des Veröffentlichungsprofils")</span><span class="sxs-lookup"><span data-stu-id="7d536-799">![Importing the publish profile](aspnet-mvc-4-fundamentals/_static/image61.png "Importing the publish profile")</span></span>

    <span data-ttu-id="7d536-800">*Importieren Sie das Veröffentlichungsprofil*</span><span class="sxs-lookup"><span data-stu-id="7d536-800">*Importing publish profile*</span></span>
3. <span data-ttu-id="7d536-801">Klicken Sie auf **überprüft, ob Verbindung**.</span><span class="sxs-lookup"><span data-stu-id="7d536-801">Click **Validate Connection**.</span></span> <span data-ttu-id="7d536-802">Klicken Sie nach Abschluss der Überprüfung auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="7d536-802">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7d536-803">Überprüfung ist beendet, sobald Sie sehen, dass ein grünes Häkchen neben der Schaltfläche "Validate Connection" angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="7d536-803">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="7d536-804">![Überprüfen der Verbindung](aspnet-mvc-4-fundamentals/_static/image62.png "Überprüfen der Verbindung")</span><span class="sxs-lookup"><span data-stu-id="7d536-804">![Validating connection](aspnet-mvc-4-fundamentals/_static/image62.png "Validating connection")</span></span>

    <span data-ttu-id="7d536-805">*Überprüfen der Verbindung*</span><span class="sxs-lookup"><span data-stu-id="7d536-805">*Validating connection*</span></span>
4. <span data-ttu-id="7d536-806">In der **Einstellungen** Seite der **Datenbanken** auf die Schaltfläche neben dem Textfeld für die datenbankverbindung (d. h. **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="7d536-806">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="7d536-807">![Web deploy-Konfiguration](aspnet-mvc-4-fundamentals/_static/image63.png "Web deploy-Konfiguration")</span><span class="sxs-lookup"><span data-stu-id="7d536-807">![Web deploy configuration](aspnet-mvc-4-fundamentals/_static/image63.png "Web deploy configuration")</span></span>

    <span data-ttu-id="7d536-808">*Web deploy-Konfiguration*</span><span class="sxs-lookup"><span data-stu-id="7d536-808">*Web deploy configuration*</span></span>
5. <span data-ttu-id="7d536-809">Konfigurieren Sie die Verbindung mit der Datenbank wie folgt:</span><span class="sxs-lookup"><span data-stu-id="7d536-809">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="7d536-810">In der **Servernamen** Geben Sie Ihre SQL-Datenbank Server-URL unter Verwendung der *Tcp:* Präfix.</span><span class="sxs-lookup"><span data-stu-id="7d536-810">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="7d536-811">In **Benutzername** Geben Sie Ihre Administrator Serveranmeldenamen an.</span><span class="sxs-lookup"><span data-stu-id="7d536-811">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="7d536-812">In **Kennwort** Geben Sie Ihre Server-Administratorkennwort.</span><span class="sxs-lookup"><span data-stu-id="7d536-812">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="7d536-813">Geben Sie einen neuen Datenbanknamen ein, z. B.: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="7d536-813">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="7d536-814">![Konfigurieren von Zielverbindungszeichenfolge](aspnet-mvc-4-fundamentals/_static/image64.png "Zielverbindungszeichenfolge konfigurieren")</span><span class="sxs-lookup"><span data-stu-id="7d536-814">![Configuring destination connection string](aspnet-mvc-4-fundamentals/_static/image64.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="7d536-815">*Konfigurieren von Ziel-Verbindungszeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="7d536-815">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="7d536-816">Klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="7d536-816">Then click **OK**.</span></span> <span data-ttu-id="7d536-817">Bei der Aufforderung zum Erstellen des Datenbank auf **Ja**.</span><span class="sxs-lookup"><span data-stu-id="7d536-817">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="7d536-818">![Erstellen der Datenbank](aspnet-mvc-4-fundamentals/_static/image65.png "erstellen die Datenbank-Zeichenfolge")</span><span class="sxs-lookup"><span data-stu-id="7d536-818">![Creating the database](aspnet-mvc-4-fundamentals/_static/image65.png "Creating the database string")</span></span>

    <span data-ttu-id="7d536-819">*Erstellen der Datenbank*</span><span class="sxs-lookup"><span data-stu-id="7d536-819">*Creating the database*</span></span>
7. <span data-ttu-id="7d536-820">Die Verbindungszeichenfolge, die Sie verwenden für die Verbindung mit SQL-Datenbank in Windows Azure ist innerhalb der Verbindung standardmäßig Textfeld angezeigt.</span><span class="sxs-lookup"><span data-stu-id="7d536-820">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="7d536-821">Klicken Sie dann auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="7d536-821">Then click **Next**.</span></span>

    <span data-ttu-id="7d536-822">![Verbindungszeichenfolge zur SQL-Datenbank zeigt](aspnet-mvc-4-fundamentals/_static/image66.png "Verbindungszeichenfolge zur SQL-Datenbank zeigt")</span><span class="sxs-lookup"><span data-stu-id="7d536-822">![Connection string pointing to SQL Database](aspnet-mvc-4-fundamentals/_static/image66.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="7d536-823">*Verbindungszeichenfolge, die auf der SQL-Datenbank*</span><span class="sxs-lookup"><span data-stu-id="7d536-823">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="7d536-824">In der **Vorschau** auf **veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="7d536-824">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="7d536-825">![Veröffentlichen der Webanwendung](aspnet-mvc-4-fundamentals/_static/image67.png "Veröffentlichen der Webanwendung")</span><span class="sxs-lookup"><span data-stu-id="7d536-825">![Publishing the web application](aspnet-mvc-4-fundamentals/_static/image67.png "Publishing the web application")</span></span>

    <span data-ttu-id="7d536-826">*Veröffentlichen der Webanwendung*</span><span class="sxs-lookup"><span data-stu-id="7d536-826">*Publishing the web application*</span></span>
9. <span data-ttu-id="7d536-827">Sobald der Veröffentlichungsprozess abgeschlossen ist, wird der Standardbrowser veröffentlichten Website geöffnet.</span><span class="sxs-lookup"><span data-stu-id="7d536-827">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="7d536-828">![Anwendung in Windows Azure veröffentlicht](aspnet-mvc-4-fundamentals/_static/image68.png "Veröffentlichung der Anwendung in Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="7d536-828">![Application published to Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="7d536-829">*Anwendung, die auf Windows Azure veröffentlicht werden soll*</span><span class="sxs-lookup"><span data-stu-id="7d536-829">*Application published to Windows Azure*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="7d536-830">Anhang C: Verwendung von Codeausschnitten</span><span class="sxs-lookup"><span data-stu-id="7d536-830">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="7d536-831">Mit Codeausschnitten müssen Sie den Code, den Sie jederzeit griffbereit benötigen.</span><span class="sxs-lookup"><span data-stu-id="7d536-831">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="7d536-832">Das Lab-Dokument erfahren Sie, wenn Sie genau, verwenden können wie in der folgenden Abbildung dargestellt.</span><span class="sxs-lookup"><span data-stu-id="7d536-832">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="7d536-833">![Verwendung von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihr Projekt](aspnet-mvc-4-fundamentals/_static/image69.png "Codeausschnitte zum Einfügen von Code in Ihr Projekt mit Visual Studio")</span><span class="sxs-lookup"><span data-stu-id="7d536-833">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-fundamentals/_static/image69.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="7d536-834">*Verwendung von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihr Projekt*</span><span class="sxs-lookup"><span data-stu-id="7d536-834">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="7d536-835">***Hinzufügen ein Codeausschnitts, die mithilfe der Tastatur (nur c#)***</span><span class="sxs-lookup"><span data-stu-id="7d536-835">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="7d536-836">Platzieren Sie den Cursor, wo möchten Sie den Code einfügen.</span><span class="sxs-lookup"><span data-stu-id="7d536-836">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="7d536-837">Starten Sie den Namen des Ausschnitts (ohne Leerzeichen oder Bindestriche) eingeben.</span><span class="sxs-lookup"><span data-stu-id="7d536-837">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="7d536-838">Beobachten Sie, wie IntelliSense zeigt übereinstimmenden Namen Ausschnitte.</span><span class="sxs-lookup"><span data-stu-id="7d536-838">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="7d536-839">Wählen Sie den richtigen Ausschnitt (oder behalten Sie eingeben, bis der gesamte Ausschnitt Name ausgewählt ist).</span><span class="sxs-lookup"><span data-stu-id="7d536-839">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="7d536-840">Drücken Sie die Tab-Taste zweimal, um den Ausschnitt an der Cursorposition eingefügt.</span><span class="sxs-lookup"><span data-stu-id="7d536-840">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="7d536-841">![Geben Sie den Namen des Ausschnitts](aspnet-mvc-4-fundamentals/_static/image70.png "starten Sie den Namen des Ausschnitts eingeben")</span><span class="sxs-lookup"><span data-stu-id="7d536-841">![Start typing the snippet name](aspnet-mvc-4-fundamentals/_static/image70.png "Start typing the snippet name")</span></span>

<span data-ttu-id="7d536-842">*Starten Sie den Namen des Ausschnitts eingeben*</span><span class="sxs-lookup"><span data-stu-id="7d536-842">*Start typing the snippet name*</span></span>

<span data-ttu-id="7d536-843">![Drücken Sie Tab, auf den hervorgehobenen Ausschnitt](aspnet-mvc-4-fundamentals/_static/image71.png "drücken Sie die Tab hervorgehobenen Ausschnitt auswählen")</span><span class="sxs-lookup"><span data-stu-id="7d536-843">![Press Tab to select the highlighted snippet](aspnet-mvc-4-fundamentals/_static/image71.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="7d536-844">*Drücken Sie Tab, um den markierten Ausschnitt auszuwählen*</span><span class="sxs-lookup"><span data-stu-id="7d536-844">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="7d536-845">![Drücken Sie erneut die Tab und den Ausschnitt erweitert](aspnet-mvc-4-fundamentals/_static/image72.png "drücken Sie erneut die Tab und den Ausschnitt erweitert")</span><span class="sxs-lookup"><span data-stu-id="7d536-845">![Press Tab again and the snippet will expand](aspnet-mvc-4-fundamentals/_static/image72.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="7d536-846">*Drücken Sie erneut die Tab und den Ausschnitt erweitert*</span><span class="sxs-lookup"><span data-stu-id="7d536-846">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="7d536-847">***Hinzufügen ein Codeausschnitts, die mit der Maus (c#, Visual Basic und XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="7d536-847">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="7d536-848">Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen möchten.</span><span class="sxs-lookup"><span data-stu-id="7d536-848">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="7d536-849">Wählen Sie **Ausschnitt einfügen** gefolgt von **eigene Codeausschnitte**.</span><span class="sxs-lookup"><span data-stu-id="7d536-849">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="7d536-850">Wählen Sie die relevanten Ausschnitt aus der Liste, indem Sie darauf klicken.</span><span class="sxs-lookup"><span data-stu-id="7d536-850">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="7d536-851">![Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten](aspnet-mvc-4-fundamentals/_static/image73.png "mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten,")</span><span class="sxs-lookup"><span data-stu-id="7d536-851">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-fundamentals/_static/image73.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="7d536-852">*Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten*</span><span class="sxs-lookup"><span data-stu-id="7d536-852">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="7d536-853">![Wählen Sie den relevanten Ausschnitt aus der Liste, indem Sie darauf klicken](aspnet-mvc-4-fundamentals/_static/image74.png "den relevanten Ausschnitt aus der Liste auswählen, indem Sie darauf klicken")</span><span class="sxs-lookup"><span data-stu-id="7d536-853">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-fundamentals/_static/image74.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="7d536-854">*Wählen Sie den relevanten Ausschnitt aus der Liste, indem Sie darauf klicken*</span><span class="sxs-lookup"><span data-stu-id="7d536-854">*Pick the relevant snippet from the list, by clicking on it*</span></span>
