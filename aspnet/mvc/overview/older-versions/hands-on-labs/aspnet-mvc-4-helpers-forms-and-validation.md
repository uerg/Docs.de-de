---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
title: ASP.NET MVC 4-Hilfsprogrammen, Formulare und Validierung | Microsoft Docs
author: rick-anderson
description: "In ASP.NET MVC 4-Modelle und Daten Zugriff praktische Übungseinheit haben Sie laden und Anzeigen von Daten aus der Datenbank wurde. In dieser praktischen Übungseinheit werden Sie zum Hinzufügen der..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 187ee9cd-bc70-479b-bfed-f568b8da96eb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
msc.type: authoredcontent
ms.openlocfilehash: 4dd10430778dc51fef1199315ee02eb2cd4970ba
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-4-helpers-forms-and-validation"></a><span data-ttu-id="0a1e8-104">ASP.NET MVC 4-Hilfsprogrammen, Formulare und Überprüfung</span><span class="sxs-lookup"><span data-stu-id="0a1e8-104">ASP.NET MVC 4 Helpers, Forms and Validation</span></span>
====================
<span data-ttu-id="0a1e8-105">durch [Web Lager Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="0a1e8-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="0a1e8-106">In **ASP.NET MVC 4-Modellen und Datenzugriff** praktische Übungseinheit, Sie wurden laden und Anzeigen von Daten aus der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-106">In **ASP.NET MVC 4 Models and Data Access** Hands-on Lab, you have been loading and displaying data from the database.</span></span> <span data-ttu-id="0a1e8-107">In dieser praktischen Übungseinheit, fügen Sie auf der **Music Store** Anwendung die Möglichkeit, diese Daten zu bearbeiten.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-107">In this Hands-on Lab, you will add to the **Music Store** application the ability to edit that data.</span></span>
> 
> <span data-ttu-id="0a1e8-108">Mit diesem Ziel Denken Sie daran erstellen Sie zuerst den Controller, der die Aktionen erstellen, lesen, aktualisieren und löschen (CRUD) von Alben unterstützt.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-108">With that goal in mind, you will first create the controller that will support the Create, Read, Update and Delete (CRUD) actions of albums.</span></span> <span data-ttu-id="0a1e8-109">Generieren Sie eine Indexansicht Vorlage nutzen ASP.NETs Gerüstbau-Funktion auf die Alben-Eigenschaften in einer HTML-Tabelle anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-109">You will generate an Index View template taking advantage of ASP.NET MVC's scaffolding feature to display the albums' properties in an HTML table.</span></span> <span data-ttu-id="0a1e8-110">Um die Ansicht zu verbessern, fügen Sie einer benutzerdefinierten HTML-Hilfsobjekt, der eine lange Beschreibung abgeschnitten wird.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-110">To enhance that view, you will add a custom HTML helper that will truncate long descriptions.</span></span>
> 
> <span data-ttu-id="0a1e8-111">Danach fügen Sie, dass die bearbeiten und Erstellen von Sichten, mit denen Sie die Alben in der Datenbank mit Hilfe des Form-Elemente wie Dropdownlisten ändern.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-111">Afterwards, you will add the Edit and Create Views that will let you alter the albums in the database, with the help of form elements like dropdowns.</span></span>
> 
> <span data-ttu-id="0a1e8-112">Abschließend können Sie Benutzer, der ein Album löschen und auch Sie hindert sie falsche Daten validieren von Eingaben eingeben.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-112">Lastly, you will let users delete an album and also you will prevent them from entering wrong data by validating their input.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="0a1e8-113">Diese praktische Übungseinheit wird vorausgesetzt, dass grundlegende Kenntnisse im **ASP.NET-MVC**.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-113">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="0a1e8-114">Wenn Sie nicht verwendet haben **ASP.NET-MVC** vorher, empfehlen wir Ihnen, durchlaufen **ASP.NET MVC-Grundlagen** praktische Übungseinheit.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-114">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC Fundamentals** Hands-on Lab.</span></span>
> 
> 
> <span data-ttu-id="0a1e8-115">Diese Übung führt Sie durch die Optimierungen und neuen Funktionen, die zuvor beschriebenen durch Anwenden von kleinere Änderungen auf eine Beispielwebanwendung im Quellordner bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-115">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>
> 
> <span data-ttu-id="0a1e8-116">Alle Beispielcode und Codeausschnitte sind im Web Lager Training Kit unter enthalten [https://www.microsoft.com/en-us/download/29843](https://www.microsoft.com/en-us/download/29843).</span><span class="sxs-lookup"><span data-stu-id="0a1e8-116">All sample code and snippets are included in the Web Camps Training Kit, available at [https://www.microsoft.com/en-us/download/29843](https://www.microsoft.com/en-us/download/29843).</span></span>


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="0a1e8-117">Ziele</span><span class="sxs-lookup"><span data-stu-id="0a1e8-117">Objectives</span></span>

<span data-ttu-id="0a1e8-118">In dieser praktischen Übungseinheit erfahren Sie, wie Sie:</span><span class="sxs-lookup"><span data-stu-id="0a1e8-118">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="0a1e8-119">Erstellen Sie einen Controller zur Unterstützung von CRUD-Vorgänge</span><span class="sxs-lookup"><span data-stu-id="0a1e8-119">Create a controller to support CRUD operations</span></span>
- <span data-ttu-id="0a1e8-120">Generieren Sie einen Index-Ansicht zum Anzeigen von Eigenschaften der Entität in einer HTML-Tabelle</span><span class="sxs-lookup"><span data-stu-id="0a1e8-120">Generate an Index View to display entity properties in an HTML table</span></span>
- <span data-ttu-id="0a1e8-121">Hinzufügen einer benutzerdefinierten HTML-Hilfsmethoden</span><span class="sxs-lookup"><span data-stu-id="0a1e8-121">Add a custom HTML helper</span></span>
- <span data-ttu-id="0a1e8-122">Erstellen und Anpassen einer Ansicht bearbeiten</span><span class="sxs-lookup"><span data-stu-id="0a1e8-122">Create and customize an Edit View</span></span>
- <span data-ttu-id="0a1e8-123">Unterscheiden Sie zwischen Aktionsmethoden, die auf HTTP-GET oder HTTP-POST-Aufrufe zu reagieren</span><span class="sxs-lookup"><span data-stu-id="0a1e8-123">Differentiate between action methods that react to either HTTP-GET or HTTP-POST calls</span></span>
- <span data-ttu-id="0a1e8-124">Hinzufügen und Anpassen einer Create View</span><span class="sxs-lookup"><span data-stu-id="0a1e8-124">Add and customize a Create View</span></span>
- <span data-ttu-id="0a1e8-125">Behandeln Sie das Löschen einer Entität</span><span class="sxs-lookup"><span data-stu-id="0a1e8-125">Handle the deletion of an entity</span></span>
- <span data-ttu-id="0a1e8-126">Validieren von Benutzereingaben</span><span class="sxs-lookup"><span data-stu-id="0a1e8-126">Validate user input</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="0a1e8-127">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="0a1e8-127">Prerequisites</span></span>

<span data-ttu-id="0a1e8-128">Sie benötigen zum Abschließen dieser testumgebung die folgenden Elemente:</span><span class="sxs-lookup"><span data-stu-id="0a1e8-128">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="0a1e8-129">[Microsoft Visual Studio Express 2012 für das Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) oder sogar eine höhere (Lesen [Anhang A](#AppendixA) Anleitungen zur Installation).</span><span class="sxs-lookup"><span data-stu-id="0a1e8-129">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="0a1e8-130">Setup</span><span class="sxs-lookup"><span data-stu-id="0a1e8-130">Setup</span></span>

<span data-ttu-id="0a1e8-131">**Installieren von Codeausschnitten**</span><span class="sxs-lookup"><span data-stu-id="0a1e8-131">**Installing Code Snippets**</span></span>

<span data-ttu-id="0a1e8-132">Der Einfachheit halber gilt Großteil des Codes, den Sie entlang dieser Übung verwalten als Visual Studio-Codeausschnitte verfügbar.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-132">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="0a1e8-133">So installieren Sie die Codeausschnitte ausführen **.\Source\Setup\CodeSnippets.vsi** Datei.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-133">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="0a1e8-134">Wenn Sie nicht mit den Visual Studio-Codeausschnitte und zu erfahren, wie deren Verwendung vertraut sind, Sie können finden Sie im Anhang in diesem Dokument &quot; [Anhang B: mithilfe von Code Snippets](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-134">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="0a1e8-135">Übungen</span><span class="sxs-lookup"><span data-stu-id="0a1e8-135">Exercises</span></span>

<span data-ttu-id="0a1e8-136">Die folgenden Übungen bilden diese praktische Übungseinheit:</span><span class="sxs-lookup"><span data-stu-id="0a1e8-136">The following exercises make up this Hands-On Lab:</span></span>

1. [<span data-ttu-id="0a1e8-137">Der Speicher-Manager-Controller und die Indexansicht erstellen</span><span class="sxs-lookup"><span data-stu-id="0a1e8-137">Creating the Store Manager controller and its Index view</span></span>](#Exercise1)
2. [<span data-ttu-id="0a1e8-138">Hinzufügen von HTML-Hilfsobjekt</span><span class="sxs-lookup"><span data-stu-id="0a1e8-138">Adding an HTML Helper</span></span>](#Exercise2)
3. [<span data-ttu-id="0a1e8-139">Erstellen die Bearbeitungsansicht</span><span class="sxs-lookup"><span data-stu-id="0a1e8-139">Creating the Edit View</span></span>](#Exercise3)
4. [<span data-ttu-id="0a1e8-140">Hinzufügen einer Ansicht erstellen</span><span class="sxs-lookup"><span data-stu-id="0a1e8-140">Adding a Create View</span></span>](#Exercise4)
5. [<span data-ttu-id="0a1e8-141">Behandlung von löschen</span><span class="sxs-lookup"><span data-stu-id="0a1e8-141">Handling Deletion</span></span>](#Exercise5)
6. [<span data-ttu-id="0a1e8-142">Hinzufügen der Validierung</span><span class="sxs-lookup"><span data-stu-id="0a1e8-142">Adding Validation</span></span>](#Exercise6)
7. [<span data-ttu-id="0a1e8-143">Mithilfe von unaufdringliche jQuery auf Clientseite</span><span class="sxs-lookup"><span data-stu-id="0a1e8-143">Using Unobtrusive jQuery at Client Side</span></span>](#Exercise7)

> [!NOTE]
> <span data-ttu-id="0a1e8-144">Jede Übung beiliegen ein **End** Ordner, der sich so ergebende Lösung, die Sie nach Abschluss der Übung abrufen soll.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-144">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="0a1e8-145">Sie können diese Lösung als Leitfaden verwenden, ggf. zusätzliche Hilfe bei der Übungen durcharbeiten.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-145">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="0a1e8-146">Geschätzte Zeit zum Abschließen dieser testumgebung: **60 Minuten**</span><span class="sxs-lookup"><span data-stu-id="0a1e8-146">Estimated time to complete this lab: **60 minutes**</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_the_Store_Manager_controller_and_its_Index_view"></a>
### <a name="exercise-1-creating-the-store-manager-controller-and-its-index-view"></a><span data-ttu-id="0a1e8-147">Übung 1: Erstellen der Speicher-Manager-Controller und die Indexansicht</span><span class="sxs-lookup"><span data-stu-id="0a1e8-147">Exercise 1: Creating the Store Manager controller and its Index view</span></span>

<span data-ttu-id="0a1e8-148">In dieser Übung erfahren Sie zum Erstellen eines neuen Controllers CRUD-Vorgänge zu unterstützen, passen dessen Index-Aktion-Methode, um eine Liste von Alben aus der Datenbank und schließlich generiert eine Indexansicht Vorlage nutzen ASP.NETs Gerüstbau zurückzugeben die Funktion auf die Alben-Eigenschaften in einer HTML-Tabelle anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-148">In this exercise, you will learn how to create a new controller to support CRUD operations, customize its Index action method to return a list of albums from the database and finally generating an Index View template taking advantage of ASP.NET MVC's scaffolding feature to display the albums' properties in an HTML table.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_StoreManagerController"></a>
#### <a name="task-1---creating-the-storemanagercontroller"></a><span data-ttu-id="0a1e8-149">Aufgabe 1: Erstellen der StoreManagerController</span><span class="sxs-lookup"><span data-stu-id="0a1e8-149">Task 1 - Creating the StoreManagerController</span></span>

<span data-ttu-id="0a1e8-150">In dieser Aufgabe erstellen Sie einen neuen Domänencontroller aufgerufen **StoreManagerController** CRUD-Vorgänge zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-150">In this task, you will create a new controller called **StoreManagerController** to support CRUD operations.</span></span>

1. <span data-ttu-id="0a1e8-151">Öffnen der **beginnen** Lösung finden Sie unter **Quell-/Ex1-CreatingTheStoreManagerController/Begin/** Ordner.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-151">Open the **Begin** solution located at **Source/Ex1-CreatingTheStoreManagerController/Begin/** folder.</span></span>

    1. <span data-ttu-id="0a1e8-152">Sie müssen einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-152">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="0a1e8-153">Klicken Sie hierzu auf die **Projekt** Menü **NuGet-Pakete verwalten**.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-153">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="0a1e8-154">In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-154">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="0a1e8-155">Schließlich erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-155">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="0a1e8-156">Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken, die sich in Ihrem Projekt liefern Verringern der Projektgröße.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-156">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="0a1e8-157">Mit NuGet-Powertools werden durch Angabe der Paketversionen in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, wenn, das Sie das Projekt ausführen, können.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-157">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="0a1e8-158">Deshalb wird müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-158">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="0a1e8-159">Fügen Sie einen neuen Domänencontroller hinzu.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-159">Add a new controller.</span></span> <span data-ttu-id="0a1e8-160">Zu diesem Zweck Maustaste die **Controller** Ordner im Projektmappen-Explorer, wählen Sie **hinzufügen** und dann die **Controller** Befehl.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-160">To do this, right-click the **Controllers** folder within the Solution Explorer, select **Add** and then the **Controller** command.</span></span> <span data-ttu-id="0a1e8-161">Ändern der **Controller** **Namen** auf **StoreManagerController** und stellen Sie sicher, dass die Option **MVC-Controller mit leeren Lese-/schreibaktionen**ausgewählt ist.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-161">Change the **Controller** **Name** to **StoreManagerController** and make sure the option **MVC controller with empty read/write actions** is selected.</span></span> <span data-ttu-id="0a1e8-162">Klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-162">Click **Add**.</span></span>

    <span data-ttu-id="0a1e8-163">![Dialogfeld "Controller hinzufügen"](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "Controller "hinzufügen"")</span><span class="sxs-lookup"><span data-stu-id="0a1e8-163">![Add controller dialog](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "Add controller dialog")</span></span>

    <span data-ttu-id="0a1e8-164">*Controller-Dialogfeld "hinzufügen"*</span><span class="sxs-lookup"><span data-stu-id="0a1e8-164">*Add Controller Dialog*</span></span>

    <span data-ttu-id="0a1e8-165">Es wird eine neue Domänencontroller-Klasse generiert.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-165">A new Controller class is generated.</span></span> <span data-ttu-id="0a1e8-166">Da Sie zum Hinzufügen von Aktionen für Lese-/Schreibzugriff Stubmethoden für diese angegeben werden häufig verwendete CRUD-Aktionen mit TODO-Kommentare ausgefüllt wird, fragt nach, die anwendungsspezifische Logik einschließen erstellt.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-166">Since you indicated to add actions for read/write, stub methods for those, common CRUD actions are created with TODO comments filled in, prompting to include the application specific logic.</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Customizing_the_StoreManager_Index"></a>
#### <a name="task-2---customizing-the-storemanager-index"></a><span data-ttu-id="0a1e8-167">Aufgabe 2: Anpassen der StoreManager-Index</span><span class="sxs-lookup"><span data-stu-id="0a1e8-167">Task 2 - Customizing the StoreManager Index</span></span>

<span data-ttu-id="0a1e8-168">In diesem Schritt passen Sie die Aktionsmethode StoreManager Index, um eine Ansicht mit der Liste der Alben aus der Datenbank zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-168">In this task, you will customize the StoreManager Index action method to return a View with the list of albums from the database.</span></span>

1. <span data-ttu-id="0a1e8-169">Fügen Sie die folgenden, in der Klasse StoreManagerController *mit* Direktiven.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-169">In the StoreManagerController class, add the following *using* directives.</span></span>

    <span data-ttu-id="0a1e8-170">(Codeausschnitt - *ASP.NET MVC 4-Hilfsprogrammen und Formulare und Validierung - Ex1 mit MvcMusicStore*)</span><span class="sxs-lookup"><span data-stu-id="0a1e8-170">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 using MvcMusicStore*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample1.cs)]
2. <span data-ttu-id="0a1e8-171">Hinzufügen eines Felds auf die **StoreManagerController** zum Speichern von einer Instanz von **MusicStoreEntities.**</span><span class="sxs-lookup"><span data-stu-id="0a1e8-171">Add a field to the **StoreManagerController** to hold an instance of **MusicStoreEntities.**</span></span>

    <span data-ttu-id="0a1e8-172">(Codeausschnitt - *ASP.NET MVC 4-Hilfsprogrammen und Formulare und Validierung - Ex1 MusicStoreEntities*)</span><span class="sxs-lookup"><span data-stu-id="0a1e8-172">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 MusicStoreEntities*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample2.cs)]
3. <span data-ttu-id="0a1e8-173">Implementieren Sie die Aktion StoreManagerController Index, um eine Ansicht mit der Liste der Alben zurückzugeben.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-173">Implement the StoreManagerController Index action to return a View with the list of albums.</span></span>

    <span data-ttu-id="0a1e8-174">Der Controller aktionslogik werden ähnelt der StoreController Index-Aktion, die zuvor geschrieben.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-174">The Controller action logic will be very similar to the StoreController's Index action written earlier.</span></span> <span data-ttu-id="0a1e8-175">Verwenden Sie LINQ, um alle Alben, einschließlich Informationen zu "Genre" und Interpreten für die Anzeige abzurufen.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-175">Use LINQ to retrieve all albums, including Genre and Artist information for display.</span></span>

    <span data-ttu-id="0a1e8-176">(Codeausschnitt - *ASP.NET MVC 4-Hilfsprogrammen und Formulare und Validierung - Ex1 StoreManagerController Index*)</span><span class="sxs-lookup"><span data-stu-id="0a1e8-176">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 StoreManagerController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample3.cs)]

<a id="Ex1Task3"></a>

<a id="strongTask_3_-_Creating_the_Index_Viewstrong"></a>
#### <a name="task-3---creating-the-index-view"></a><span data-ttu-id="0a1e8-177">Aufgabe 3: erstellen die Indexansicht</span><span class="sxs-lookup"><span data-stu-id="0a1e8-177">Task 3 - Creating the Index View</span></span>

<span data-ttu-id="0a1e8-178">In dieser Aufgabe erstellen Sie die Indexansicht-Vorlage zum Anzeigen der Liste von Alben zurückgegebenes der **StoreManager** Controller.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-178">In this task, you will create the Index View template to display the list of albums returned by the **StoreManager** Controller.</span></span>

1. <span data-ttu-id="0a1e8-179">Vor dem Erstellen der neuen Vorlage anzeigen, erstellen Sie das Projekt, damit die **Ansicht-Dialogfeld hinzufügen** kennt die **Album** -Klasse.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-179">Before creating the new View template, you should build the project so that the **Add View Dialog** knows about the **Album** class to use.</span></span> <span data-ttu-id="0a1e8-180">Wählen Sie **erstellen | Erstellen von MvcMusicStore** zum Erstellen des Projekts.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-180">Select **Build | Build MvcMusicStore** to build the project.</span></span>
2. <span data-ttu-id="0a1e8-181">Mit der rechten Maustaste innerhalb der **Index** Aktionsmethode, und wählen **Ansicht hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-181">Right-click inside the **Index** action method and select **Add View**.</span></span> <span data-ttu-id="0a1e8-182">Hierdurch erscheint der **Ansicht hinzufügen** Dialogfeld.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-182">This will bring up the **Add View** dialog.</span></span>

    <span data-ttu-id="0a1e8-183">![Fügen Sie die Ansicht](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "Ansicht hinzufügen")</span><span class="sxs-lookup"><span data-stu-id="0a1e8-183">![Add view](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "Add view")</span></span>

    <span data-ttu-id="0a1e8-184">*Hinzufügen einer Ansicht von innerhalb der Index-Methode*</span><span class="sxs-lookup"><span data-stu-id="0a1e8-184">*Adding a View from within the Index method*</span></span>
3. <span data-ttu-id="0a1e8-185">Stellen Sie sicher, dass der Ansichtsname ist, klicken Sie im Dialogfeld "Ansicht hinzufügen" **Index**.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-185">In the Add View dialog, verify that the View Name is **Index**.</span></span> <span data-ttu-id="0a1e8-186">Wählen Sie die **eine stark typisierte Ansicht erstellen** aus, und wählen Sie **Album (MvcMusicStore.Models)** aus der **Modellschemas** Dropdownliste aus.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-186">Select the **Create a strongly-typed view** option, and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down.</span></span> <span data-ttu-id="0a1e8-187">Wählen Sie **Liste** aus der **Gerüst Vorlage** Dropdownliste aus.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-187">Select **List** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="0a1e8-188">Lassen Sie die **Ansichtsmodul** auf **Razor** und die anderen Felder mit ihren Wert, und klicken Sie dann auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-188">Leave the **View engine** to **Razor** and the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="0a1e8-189">![Hinzufügen einer Indexansicht](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "Hinzufügen der Indexansicht für einen")</span><span class="sxs-lookup"><span data-stu-id="0a1e8-189">![Adding an index view](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "Adding an index view")</span></span>

    <span data-ttu-id="0a1e8-190">*Hinzufügen der Indexansicht für einen*</span><span class="sxs-lookup"><span data-stu-id="0a1e8-190">*Adding an Index View*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Customizing_the_scaffold_of_the_Index_View"></a>
#### <a name="task-4---customizing-the-scaffold-of-the-index-view"></a><span data-ttu-id="0a1e8-191">Aufgabe 4: Anpassen das Gerüst für die Indexansicht</span><span class="sxs-lookup"><span data-stu-id="0a1e8-191">Task 4 - Customizing the scaffold of the Index View</span></span>

<span data-ttu-id="0a1e8-192">In diesem Schritt passen Sie die einfache Ansicht-Vorlage, die mit ASP.NET MVC-Gerüstbau-Funktion so, dass sie die gewünschten Felder anzeigen, erstellt.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-192">In this task, you will adjust the simple View template created with ASP.NET MVC scaffolding feature to have it display the fields you want.</span></span>

> [!NOTE]
> <span data-ttu-id="0a1e8-193">Die **Gerüstbau** -Unterstützung in ASP.NET MVC generiert eine einfache Ansichtenvorlage, die alle Felder im Modell Album aufgelistet.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-193">The **scaffolding** support within ASP.NET MVC generates a simple View template which lists all fields in the Album model.</span></span> <span data-ttu-id="0a1e8-194">**Gerüstbau** bietet eine schnelle Möglichkeit zum Einstieg mit einer stark typisierten Ansicht: anstatt die Sicht Vorlage manuell zu schreiben, schnell Gerüstbau eine Standardvorlage generiert und dann können Sie den generierten Code ändern.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-194">**Scaffolding** provides a quick way to get started on a strongly typed view: rather than having to write the View template manually, scaffolding quickly generates a default template and then you can modify the generated code.</span></span>


1. <span data-ttu-id="0a1e8-195">Überprüfen Sie den Code erstellt.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-195">Review the code created.</span></span> <span data-ttu-id="0a1e8-196">Die generierten Liste von Feldern werden Teil des folgenden HTML Tabelle mit **Gerüstbau** zum Anzeigen von Tabellendaten zu verwenden ist.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-196">The generated list of fields will be part of the following HTML table that **Scaffolding** is using for displaying tabular data.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample4.cshtml)]
2. <span data-ttu-id="0a1e8-197">Ersetzen Sie die  **&lt;Tabelle&gt;**  Code durch den folgenden Code zur ausschließlichen Anzeige der **"Genre"**, **Interpreten**, **Albumtitel**, und **Preis** Felder.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-197">Replace the **&lt;table&gt;** code with the following code to display only the **Genre**, **Artist**, **Album Title**, and **Price** fields.</span></span> <span data-ttu-id="0a1e8-198">Dadurch werden gelöscht, die **AlbumId** und **Album Art URL** Spalten.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-198">This deletes the **AlbumId** and **Album Art URL** columns.</span></span> <span data-ttu-id="0a1e8-199">Es ändert sich auch, GenreId und ArtistId Spalten zum Anzeigen ihrer verknüpfte Klasseneigenschaften eines **Artist.Name** und **Genre.Name**, und entfernt die **Details** Link.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-199">Also, it changes GenreId and ArtistId columns to display their linked class properties of **Artist.Name** and **Genre.Name**, and removes the **Details** link.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample5.cshtml)]
3. <span data-ttu-id="0a1e8-200">Ändern Sie die folgenden Beschreibungen.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-200">Change the following descriptions.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample6.cshtml)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="0a1e8-201">Aufgabe 5: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="0a1e8-201">Task 5 - Running the Application</span></span>

<span data-ttu-id="0a1e8-202">In dieser Aufgabe testen Sie, die die **StoreManager** **Index** Vorlage anzeigen Zeigt eine Liste von Alben gemäß den Entwurf der vorigen Schritte.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-202">In this task, you will test that the **StoreManager** **Index** View template displays a list of albums according to the design of the previous steps.</span></span>

1. <span data-ttu-id="0a1e8-203">Drücken Sie **F5** um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-203">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="0a1e8-204">Das Projekt wird auf der Startseite gestartet.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-204">The project starts in the Home page.</span></span> <span data-ttu-id="0a1e8-205">Ändern Sie die URL zum **/StoreManager** zu überprüfen, ob eine Liste von Alben angezeigt wird, deren **Titel**, **Interpreten** und **"Genre"**.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-205">Change the URL to **/StoreManager** to verify that a list of albums is displayed, showing their **Title**, **Artist** and **Genre**.</span></span>

    <span data-ttu-id="0a1e8-206">![Durchsuchen die Liste der Alben](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "Durchsuchen der Liste von Alben")</span><span class="sxs-lookup"><span data-stu-id="0a1e8-206">![Browsing the list of albums](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "Browsing the list of albums")</span></span>

    <span data-ttu-id="0a1e8-207">*Durchsuchen die Liste der Alben*</span><span class="sxs-lookup"><span data-stu-id="0a1e8-207">*Browsing the list of albums*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Adding_an_HTML_Helper"></a>
### <a name="exercise-2-adding-an-html-helper"></a><span data-ttu-id="0a1e8-208">Übung 2: Hinzufügen von HTML-Hilfsobjekt</span><span class="sxs-lookup"><span data-stu-id="0a1e8-208">Exercise 2: Adding an HTML Helper</span></span>

<span data-ttu-id="0a1e8-209">Die Indexseite StoreManager hat ein mögliches Problem: Titel und den Namen der Interpret-Eigenschaften können sowohl werden lang genug ist, deaktivieren Sie die Formatierung der Tabelle ausgelöst werden soll.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-209">The StoreManager Index page has one potential issue: Title and Artist Name properties can both be long enough to throw off the table formatting.</span></span> <span data-ttu-id="0a1e8-210">In dieser Übung erfahren Sie, wie Sie eine benutzerdefinierte HTML-Hilfsobjekt, um das Abschneiden von Text hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-210">In this exercise you will learn how to add a custom HTML helper to truncate that text.</span></span>

<span data-ttu-id="0a1e8-211">In der folgenden Abbildung sehen Sie, wie das Format aufgrund der Länge des Texts geändert wird, wenn Sie eine kleines Browsergröße verwenden.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-211">In the following figure, you can see how the format is modified because of the length of the text when you use a small browser size.</span></span>

<span data-ttu-id="0a1e8-212">![Durchsuchen die Liste der Alben mit nicht abgeschnittener Text](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "Text Durchsuchen der Liste von Alben mit nicht abgeschnitten")</span><span class="sxs-lookup"><span data-stu-id="0a1e8-212">![Browsing the list of Albums with not truncated text](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "Browsing the list of Albums with not truncated text")</span></span>

<span data-ttu-id="0a1e8-213">*Durchsuchen die Liste der Alben mit abgeschnittener nicht text*</span><span class="sxs-lookup"><span data-stu-id="0a1e8-213">*Browsing the list of Albums with not truncated text*</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Extending_the_HTML_Helper"></a>
#### <a name="task-1---extending-the-html-helper"></a><span data-ttu-id="0a1e8-214">Aufgabe 1 – Erweitern von HTML-Hilfsobjekt.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-214">Task 1 - Extending the HTML Helper</span></span>

<span data-ttu-id="0a1e8-215">In dieser Aufgabe fügen Sie eine neue Methode **Truncate** auf die **HTML** in ASP.NET MVC-Ansichten verfügbar gemachtes Objekt zu.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-215">In this task, you will add a new method **Truncate** to the **HTML** object exposed within ASP.NET MVC Views.</span></span> <span data-ttu-id="0a1e8-216">Implementieren Sie zu diesem Zweck ein **Erweiterungsmethode** für die integrierte **System.Web.Mvc.HtmlHelper** von ASP.NET MVC bereitgestellte-Klasse.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-216">To do this, you will implement an **extension method** to the built-in **System.Web.Mvc.HtmlHelper** class provided by ASP.NET MVC.</span></span>

> [!NOTE]
> <span data-ttu-id="0a1e8-217">Weitere Informationen zu **Erweiterungsmethoden**, besuchen Sie die in diesem Msdn-Artikel.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-217">To read more about **Extension Methods**, please visit this msdn article.</span></span> <span data-ttu-id="0a1e8-218">[https://msdn.Microsoft.com/en-us/library/bb383977.aspx](https://msdn.microsoft.com/en-us/library/bb383977.aspx).</span><span class="sxs-lookup"><span data-stu-id="0a1e8-218">[https://msdn.microsoft.com/en-us/library/bb383977.aspx](https://msdn.microsoft.com/en-us/library/bb383977.aspx).</span></span>


1. <span data-ttu-id="0a1e8-219">Öffnen der **beginnen** Lösung finden Sie unter **Quell-/Ex2-AddingAnHTMLHelper/Begin/** Ordner.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-219">Open the **Begin** solution located at **Source/Ex2-AddingAnHTMLHelper/Begin/** folder.</span></span> <span data-ttu-id="0a1e8-220">Andernfalls möglicherweise weiterhin mithilfe der **End** Lösung abgerufen, von der vorherigen Übung abschließen.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-220">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="0a1e8-221">Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung müssen Sie einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-221">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="0a1e8-222">Klicken Sie hierzu auf die **Projekt** Menü **NuGet-Pakete verwalten**.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-222">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="0a1e8-223">In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-223">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="0a1e8-224">Schließlich erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-224">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="0a1e8-225">Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken, die sich in Ihrem Projekt liefern Verringern der Projektgröße.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-225">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="0a1e8-226">Mit NuGet-Powertools werden durch Angabe der Paketversionen in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, wenn, das Sie das Projekt ausführen, können.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-226">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="0a1e8-227">Deshalb wird müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-227">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="0a1e8-228">Öffnen des StoreManager Index anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-228">Open StoreManager's Index View.</span></span> <span data-ttu-id="0a1e8-229">Zu diesem Zweck im Projektmappen-Explorer erweitern die **Ansichten** Ordner, und klicken Sie dann die **StoreManager** , und öffnen Sie die **Index.cshtml** Datei.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-229">To do this, in the Solution Explorer expand the **Views** folder, then the **StoreManager** and open the **Index.cshtml** file.</span></span>
3. <span data-ttu-id="0a1e8-230">Fügen Sie den folgenden Code unter der  **@model**  zum Definieren der **Truncate** Hilfsmethode.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-230">Add the following code below the **@model** directive to define the **Truncate** helper method.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample7.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Truncating_Text_in_the_Page"></a>
#### <a name="task-2---truncating-text-in-the-page"></a><span data-ttu-id="0a1e8-231">Aufgabe 2 - Abschneiden Text auf der Seite</span><span class="sxs-lookup"><span data-stu-id="0a1e8-231">Task 2 - Truncating Text in the Page</span></span>

<span data-ttu-id="0a1e8-232">In dieser Aufgabe verwenden Sie die **Truncate** Methode zum Abschneiden von Text in der Vorlage anzeigen.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-232">In this task, you will use the **Truncate** method to truncate the text in the View template.</span></span>

1. <span data-ttu-id="0a1e8-233">Öffnen des StoreManager Index anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-233">Open StoreManager's Index View.</span></span> <span data-ttu-id="0a1e8-234">Zu diesem Zweck im Projektmappen-Explorer erweitern die **Ansichten** Ordner, und klicken Sie dann die **StoreManager** , und öffnen Sie die **Index.cshtml** Datei.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-234">To do this, in the Solution Explorer expand the **Views** folder, then the **StoreManager** and open the **Index.cshtml** file.</span></span>
2. <span data-ttu-id="0a1e8-235">Ersetzen Sie die Zeilen, die veranschaulichen die **Namen Künstlers** und des Albums **Titel**.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-235">Replace the lines that show the **Artist Name** and Album's **Title**.</span></span> <span data-ttu-id="0a1e8-236">Ersetzen Sie hierzu die folgenden Zeilen ein.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-236">To do this, replace the following lines.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample8.cshtml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="0a1e8-237">Aufgabe 3: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="0a1e8-237">Task 3 - Running the Application</span></span>

<span data-ttu-id="0a1e8-238">In dieser Aufgabe testen Sie, die die **StoreManager** **Index** Vorlage anzeigen abschneidet, Titel und Interpret-Name des Albums.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-238">In this task, you will test that the **StoreManager** **Index** View template truncates the Album's Title and Artist Name.</span></span>

1. <span data-ttu-id="0a1e8-239">Drücken Sie **F5** um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-239">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="0a1e8-240">Das Projekt wird auf der Startseite gestartet.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-240">The project starts in the Home page.</span></span> <span data-ttu-id="0a1e8-241">Ändern Sie die URL zum **/StoreManager** lange überprüfen Texte in der **Titel** und **Interpreten** Spalte abgeschnitten werden.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-241">Change the URL to **/StoreManager** to verify that long texts in the **Title** and **Artist** column are truncated.</span></span>

    <span data-ttu-id="0a1e8-242">![Titel und Künstler Namen gekürzt](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "Titel und Künstler Namen abgeschnitten")</span><span class="sxs-lookup"><span data-stu-id="0a1e8-242">![Truncated titles and artists names](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "Truncated titles and artists names")</span></span>

    <span data-ttu-id="0a1e8-243">*Abgeschnittene Titel und Künstler Namen*</span><span class="sxs-lookup"><span data-stu-id="0a1e8-243">*Truncated Titles and Artist Names*</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Creating_the_Edit_View"></a>
### <a name="exercise-3-creating-the-edit-view"></a><span data-ttu-id="0a1e8-244">Übung 3: Erstellen der Bearbeitungsansicht</span><span class="sxs-lookup"><span data-stu-id="0a1e8-244">Exercise 3: Creating the Edit View</span></span>

<span data-ttu-id="0a1e8-245">In dieser Übung erfahren Sie, zum Erstellen eines Formulars, um Speicher-Manager so bearbeiten Sie die Verwaltung zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-245">In this exercise, you will learn how to create a form to allow store managers to edit an Album.</span></span> <span data-ttu-id="0a1e8-246">Durchsuchen sie die **/StoreManager/Edit/id** URL (**Id** wird die eindeutige Id des Albums bearbeiten), wodurch einen HTTP-GET-Aufruf an den Server.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-246">They will browse the **/StoreManager/Edit/id** URL (**id** being the unique id of the album to edit), thus making an HTTP-GET call to the server.</span></span>

<span data-ttu-id="0a1e8-247">Die Aktionsmethode Controller bearbeiten wird das entsprechende Album aus der Datenbank abrufen, erstellen Sie eine **StoreManagerViewModel** Objekt, das (zusammen mit der eine Liste von Künstler und Genres) zu kapseln, und übergeben Sie ihn dann, um eine Ansicht aus, um Rendern von HTML-Seite an dem Benutzer.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-247">The Controller Edit action method will retrieve the appropriate Album from the database, create a **StoreManagerViewModel** object to encapsulate it (along with a list of Artists and Genres), and then pass it off to a View template to render the HTML page back to the user.</span></span> <span data-ttu-id="0a1e8-248">Diese Seite enthält eine  **&lt;Formular&gt;**  Element mit Textfelder und Dropdownlisten zur Bearbeitung der Eigenschaften Album.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-248">This page will contain a **&lt;form&gt;** element with textboxes and dropdowns for editing the Album properties.</span></span>

<span data-ttu-id="0a1e8-249">Sobald der Benutzer klickt und die Formularwerte Album aktualisiert der **speichern** Schaltfläche, die Änderungen werden gesendet, über HTTP-POST aufgerufen, **/StoreManager/Edit/id**. Obwohl die URL wie in den letzten Aufruf unverändert bleibt, ASP.NET MVC identifiziert, diesmal, es ist ein HTTP-POST und aus diesem Grund führt eine andere Methode des bearbeiten-Aktion (eine Angabe **[HttpPost]**).</span><span class="sxs-lookup"><span data-stu-id="0a1e8-249">Once the user updates the Album form values and clicks the **Save** button, the changes are submitted via an HTTP-POST call back to **/StoreManager/Edit/id**. Although the URL remains the same as in the last call, ASP.NET MVC identifies that this time it is an HTTP-POST and therefore executes a different Edit action method (one decorated with **[HttpPost]**).</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Edit_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-edit-action-method"></a><span data-ttu-id="0a1e8-250">Aufgabe 1 – Implementieren der HTTP-GET bearbeiten Action-Methode</span><span class="sxs-lookup"><span data-stu-id="0a1e8-250">Task 1 - Implementing the HTTP-GET Edit Action Method</span></span>

<span data-ttu-id="0a1e8-251">In dieser Aufgabe wird der HTTP-GET-Version der Aktionsmethode bearbeiten zum Abrufen der entsprechenden Album aus der Datenbank sowie eine Liste aller Genres und Künstler implementiert werden.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-251">In this task, you will implement the HTTP-GET version of the Edit action method to retrieve the appropriate Album from the database, as well as a list of all Genres and Artists.</span></span> <span data-ttu-id="0a1e8-252">Es wird verpackt diese Daten in der **StoreManagerViewModel** Objekt definiert, die im letzten Schritt, der dann an eine Ansichtenvorlage zum Rendern der Antwort mit übergeben wird.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-252">It will package this data up into the **StoreManagerViewModel** object defined in the last step, which will then be passed to a View template to render the response with.</span></span>

1. <span data-ttu-id="0a1e8-253">Öffnen der **beginnen** Lösung finden Sie unter **Quell-/Ex3-CreatingTheEditView/Begin/** Ordner.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-253">Open the **Begin** solution located at **Source/Ex3-CreatingTheEditView/Begin/** folder.</span></span> <span data-ttu-id="0a1e8-254">Andernfalls möglicherweise weiterhin mithilfe der **End** Lösung abgerufen, von der vorherigen Übung abschließen.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-254">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="0a1e8-255">Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung müssen Sie einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-255">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="0a1e8-256">Klicken Sie hierzu auf die **Projekt** Menü **NuGet-Pakete verwalten**.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-256">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="0a1e8-257">In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-257">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="0a1e8-258">Schließlich erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-258">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="0a1e8-259">Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken, die sich in Ihrem Projekt liefern Verringern der Projektgröße.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-259">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="0a1e8-260">Mit NuGet-Powertools werden durch Angabe der Paketversionen in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, wenn, das Sie das Projekt ausführen, können.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-260">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="0a1e8-261">Deshalb wird müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-261">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="0a1e8-262">Öffnen der **StoreManagerController** Klasse.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-262">Open the **StoreManagerController** class.</span></span> <span data-ttu-id="0a1e8-263">Erweitern Sie hierzu die **Controller** Ordner und doppelklicken Sie auf **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-263">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="0a1e8-264">Ersetzen Sie die **HTTP-GET bearbeiten** Aktionsmethode mit den folgenden Code zum Abrufen der entsprechenden **Album** als auch die **Genres** und **Künstler**aufgeführt.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-264">Replace the **HTTP-GET Edit** action method with the following code to retrieve the appropriate **Album** as well as the **Genres** and **Artists** lists.</span></span>

    <span data-ttu-id="0a1e8-265">(Codeausschnitt - *ASP.NET MVC 4-Hilfsprogrammen und Formulare und Validierung - Aktion bearbeiten zu Ex3 StoreManagerController HTTP-GET*)</span><span class="sxs-lookup"><span data-stu-id="0a1e8-265">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex3 StoreManagerController HTTP-GET Edit action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample9.cs)]

    > [!NOTE]
    > <span data-ttu-id="0a1e8-266">Verwenden Sie **System.Web.Mvc** **SelectList** für Künstler und Genres statt der **System.Collections.Generic** Liste.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-266">You are using **System.Web.Mvc** **SelectList** for Artists and Genres instead of the **System.Collections.Generic** List.</span></span>
    > 
    > <span data-ttu-id="0a1e8-267">**SelectList** ist eine sauberere Methode zum Auffüllen von HTML-Dropdownlisten und verwalten z. B. die aktuelle Auswahl.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-267">**SelectList** is a cleaner way to populate HTML dropdowns and manage things like current selection.</span></span> <span data-ttu-id="0a1e8-268">Instanziieren und später zum Einrichten dieser ViewModel-Objekte in der Controlleraktion treffen cleaner Szenario Formular bearbeiten.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-268">Instantiating and later setting up these ViewModel objects in the controller action will make the Edit form scenario cleaner.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Creating_the_Edit_View"></a>
#### <a name="task-2---creating-the-edit-view"></a><span data-ttu-id="0a1e8-269">Aufgabe 2: Erstellen der Bearbeitungsansicht</span><span class="sxs-lookup"><span data-stu-id="0a1e8-269">Task 2 - Creating the Edit View</span></span>

<span data-ttu-id="0a1e8-270">In dieser Aufgabe erstellen Sie eine Vorlage bearbeiten anzeigen, die später die Albumeigenschaften angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-270">In this task, you will create an Edit View template that will later display the album properties.</span></span>

1. <span data-ttu-id="0a1e8-271">Erstellen Sie die Bearbeitungsansicht.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-271">Create the Edit View.</span></span> <span data-ttu-id="0a1e8-272">Zu diesem Zweck Maustaste innerhalb der **bearbeiten** Aktionsmethode, und wählen **Ansicht hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-272">To do this, right-click inside the **Edit** action method and select **Add View**.</span></span>
2. <span data-ttu-id="0a1e8-273">Stellen Sie sicher, dass der Ansichtsname ist, klicken Sie im Dialogfeld "Ansicht hinzufügen" **bearbeiten**.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-273">In the Add View dialog, verify that the View Name is **Edit**.</span></span> <span data-ttu-id="0a1e8-274">Überprüfen Sie die **eine stark typisierte Ansicht erstellen** Kontrollkästchen, und wählen Sie **Album (MvcMusicStore.Models)** aus der **Datenklasse anzeigen** Dropdown-Menü.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-274">Check the **Create a strongly-typed view** checkbox and select **Album (MvcMusicStore.Models)** from the **View data class** drop-down.</span></span> <span data-ttu-id="0a1e8-275">Wählen Sie **bearbeiten** aus der **Gerüst Vorlage** Dropdownliste aus.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-275">Select **Edit** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="0a1e8-276">Lassen Sie die anderen Felder die Standardwerte, und klicken Sie dann auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-276">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="0a1e8-277">![Hinzufügen einer Bearbeitungsansicht](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "eine Bearbeitungsansicht hinzufügen")</span><span class="sxs-lookup"><span data-stu-id="0a1e8-277">![Adding an Edit view](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "Adding an Edit view")</span></span>

    <span data-ttu-id="0a1e8-278">*Hinzufügen einer Bearbeitungsansicht*</span><span class="sxs-lookup"><span data-stu-id="0a1e8-278">*Adding an Edit view*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="0a1e8-279">Aufgabe 3: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="0a1e8-279">Task 3 - Running the Application</span></span>

<span data-ttu-id="0a1e8-280">In dieser Aufgabe testen Sie, die die **StoreManager** **bearbeiten** Ansichtsseite zeigt die Eigenschaften-Werte für das Album als Parameter übergeben.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-280">In this task, you will test that the **StoreManager** **Edit** View page displays the properties' values for the album passed as parameter.</span></span>

1. <span data-ttu-id="0a1e8-281">Drücken Sie **F5** um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-281">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="0a1e8-282">Das Projekt wird auf der Startseite gestartet.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-282">The project starts in the Home page.</span></span> <span data-ttu-id="0a1e8-283">Ändern Sie die URL zum **/StoreManager/Edit/1** um sicherzustellen, dass die Eigenschaften-Werte für das übergeben Album angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-283">Change the URL to **/StoreManager/Edit/1** to verify that the properties' values for the album passed are displayed.</span></span>

    <span data-ttu-id="0a1e8-284">![Durchsuchen des Albums Bearbeitungsansicht](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "des Albums Bearbeitungsansicht durchsuchen")</span><span class="sxs-lookup"><span data-stu-id="0a1e8-284">![Browsing Album's Edit View](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "Browsing Album's Edit View")</span></span>

    <span data-ttu-id="0a1e8-285">*Durchsuchen des Albums Bearbeitungsansicht*</span><span class="sxs-lookup"><span data-stu-id="0a1e8-285">*Browsing Album's Edit view*</span></span>

<a id="Ex3Task4"></a>

<a id="Task_4_-_Implementing_drop-downs_on_the_Album_Editor_Template"></a>
#### <a name="task-4---implementing-drop-downs-on-the-album-editor-template"></a><span data-ttu-id="0a1e8-286">Aufgabe 4: Implementieren von Dropdownlisten in der Vorlage Album-Editor</span><span class="sxs-lookup"><span data-stu-id="0a1e8-286">Task 4 - Implementing drop-downs on the Album Editor Template</span></span>

<span data-ttu-id="0a1e8-287">In dieser Aufgabe fügen Sie Dropdownlisten der Vorlage anzeigen, die in der vorhergehenden Aufgabe erstellt hinzu, damit der Benutzer aus einer Liste von Künstler und Genres auswählen kann.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-287">In this task, you will add drop-downs to the View template created in the last task, so that the user can select from a list of Artists and Genres.</span></span>

1. <span data-ttu-id="0a1e8-288">Alle ersetzen die **Album** Fieldset-Code durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="0a1e8-288">Replace all the **Album** fieldset code with the following:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample10.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="0a1e8-289">Ein **Html.DropDownList** Helper Dropdownlisten für die Auswahl Künstler und Genres Rendern hinzugefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-289">An **Html.DropDownList** helper has been added to render drop-downs for choosing Artists and Genres.</span></span> <span data-ttu-id="0a1e8-290">Die Parameter zu übergeben, um **Html.DropDownList** sind:</span><span class="sxs-lookup"><span data-stu-id="0a1e8-290">The parameters passed to **Html.DropDownList** are:</span></span>
    > 
    > 1. <span data-ttu-id="0a1e8-291">Der Name des Formularfelds (**&quot;ArtistId&quot;**).</span><span class="sxs-lookup"><span data-stu-id="0a1e8-291">The name of the form field (**&quot;ArtistId&quot;**).</span></span>
    > 2. <span data-ttu-id="0a1e8-292">Die **SelectList** der Werte für die Dropdownliste aus.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-292">The **SelectList** of values for the drop-down.</span></span>

<a id="Ex3Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="0a1e8-293">Aufgabe 5: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="0a1e8-293">Task 5 - Running the Application</span></span>

<span data-ttu-id="0a1e8-294">In dieser Aufgabe testen Sie, die die **StoreManager** **bearbeiten** Ansichtsseite zeigt Dropdowns statt Interpreten und ID "Genre" Textfelder.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-294">In this task, you will test that the **StoreManager** **Edit** View page displays drop-downs instead of Artist and Genre ID text fields.</span></span>

1. <span data-ttu-id="0a1e8-295">Drücken Sie **F5** um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-295">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="0a1e8-296">Das Projekt wird auf der Startseite gestartet.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-296">The project starts in the Home page.</span></span> <span data-ttu-id="0a1e8-297">Ändern Sie die URL zum **/StoreManager/Edit/1** zu überprüfen, ob sie Dropdownlisten statt Interpreten und ID "Genre" Text-Felder angezeigt.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-297">Change the URL to **/StoreManager/Edit/1** to verify that it displays drop-downs instead of Artist and Genre ID text fields.</span></span>

    <span data-ttu-id="0a1e8-298">![Durchsuchen des Albums Ansicht bearbeiten mit Dropdownlisten](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "durchsuchen Album Bearbeitungsansicht mit Dropdownlisten")</span><span class="sxs-lookup"><span data-stu-id="0a1e8-298">![Browsing Album's Edit View with drop downs](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "Browsing Album's Edit View with drop downs")</span></span>

    <span data-ttu-id="0a1e8-299">*Durchsuchen des Albums Bearbeitungsansicht, diesmal mit Dropdownlisten*</span><span class="sxs-lookup"><span data-stu-id="0a1e8-299">*Browsing Album's Edit view, this time with dropdowns*</span></span>

<a id="Ex3Task6"></a>

<a id="Task_6_-_Implementing_the_HTTP-POST_Edit_action_method"></a>
#### <a name="task-6---implementing-the-http-post-edit-action-method"></a><span data-ttu-id="0a1e8-300">Aufgabe 6: Implementieren der HTTP-POST bearbeiten Action-Methode</span><span class="sxs-lookup"><span data-stu-id="0a1e8-300">Task 6 - Implementing the HTTP-POST Edit action method</span></span>

<span data-ttu-id="0a1e8-301">Nun, dass die Ansicht bearbeiten wie erwartet angezeigt wird, müssen Sie zur Implementierung der HTTP-POST-Aktion bearbeiten-Methode, um dem Album vorgenommenen Änderungen zu speichern.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-301">Now that the Edit View displays as expected, you need to implement the HTTP-POST Edit Action method to save the changes made to the Album.</span></span>

1. <span data-ttu-id="0a1e8-302">Schließen Sie den Browser, die ggf. zu Visual Studio-Fenster zurückzukehren.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-302">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="0a1e8-303">Open **StoreManagerController** aus der **Controller** Ordner.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-303">Open **StoreManagerController** from the **Controllers** folder.</span></span>
2. <span data-ttu-id="0a1e8-304">Ersetzen Sie **HTTP-POST bearbeiten** Aktion-Methode durch den folgenden Code (Beachten Sie, dass die Methode, die ersetzt werden müssen überladene Version, die zwei Parameter empfängt):</span><span class="sxs-lookup"><span data-stu-id="0a1e8-304">Replace **HTTP-POST Edit** action method code with the following (note that the method that must be replaced is overloaded version that receives two parameters):</span></span>

    <span data-ttu-id="0a1e8-305">(Codeausschnitt - *ASP.NET MVC 4-Hilfsprogrammen und Formulare und Validierung - Aktion bearbeiten zu Ex3 StoreManagerController HTTP-POST*)</span><span class="sxs-lookup"><span data-stu-id="0a1e8-305">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex3 StoreManagerController HTTP-POST Edit action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample11.cs)]

    > [!NOTE]
    > <span data-ttu-id="0a1e8-306">Diese Methode wird ausgeführt werden, wenn der Benutzer klickt auf die **speichern** Schaltfläche der Sicht und führt einen HTTP-POST für die Formularwerte zurück an den Server um dauerhaft in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-306">This method will be executed when the user clicks the **Save** button of the View and performs an HTTP-POST of the form values back to the server to persist them in the database.</span></span> <span data-ttu-id="0a1e8-307">Die Decorator **[HttpPost]** gibt an, dass die Methode für die HTTP-POST-Szenarien verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-307">The decorator **[HttpPost]** indicates that the method should be used for those HTTP-POST scenarios.</span></span> <span data-ttu-id="0a1e8-308">Die Methode nimmt ein **Album** Objekt.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-308">The method takes an **Album** object.</span></span> <span data-ttu-id="0a1e8-309">ASP.NET MVC erstellt automatisch das Album-Objekt aus der gebuchten &lt;Formular&gt; Werte.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-309">ASP.NET MVC will automatically create the Album object from the posted &lt;form&gt; values.</span></span>
    > 
    > <span data-ttu-id="0a1e8-310">Die Methode führt folgende Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="0a1e8-310">The method will perform these steps:</span></span>
    > 
    > 1. <span data-ttu-id="0a1e8-311">Wenn das Modell gültig ist:</span><span class="sxs-lookup"><span data-stu-id="0a1e8-311">If model is valid:</span></span>
    > 
    >     1. <span data-ttu-id="0a1e8-312">Aktualisieren Sie den Album-Eintrag im Kontext als eine geänderte Objekt kennzeichnen.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-312">Update the album entry in the context to mark it as a modified object.</span></span>
    >     2. <span data-ttu-id="0a1e8-313">Die Änderungen zu speichern und an die Indexansicht umleiten.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-313">Save the changes and redirect to the index view.</span></span>
    > 2. <span data-ttu-id="0a1e8-314">Wenn das Modell nicht gültig ist, füllt es das ViewBag-Objekt mit dem **GenreId** und **ArtistId**, wird die Ansicht mit dem empfangenen Album-Objekt, damit der Benutzer kann zurückgegeben führen Sie ein Update erforderliches.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-314">If the model is not valid, it will populate the ViewBag with the **GenreId** and **ArtistId**, then it will return the view with the received Album object to allow the user perform any required update.</span></span>

<a id="Ex3Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a><span data-ttu-id="0a1e8-315">Aufgabe 7: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="0a1e8-315">Task 7 - Running the Application</span></span>

<span data-ttu-id="0a1e8-316">In dieser Aufgabe testen Sie, die die **StoreManager bearbeiten** Ansichtsseite speichert die aktualisierten Album Daten tatsächlich in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-316">In this task, you will test that the **StoreManager Edit** View page actually saves the updated Album data in the database.</span></span>

1. <span data-ttu-id="0a1e8-317">Drücken Sie **F5** um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-317">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="0a1e8-318">Das Projekt wird auf der Startseite gestartet.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-318">The project starts in the Home page.</span></span> <span data-ttu-id="0a1e8-319">Ändern Sie die URL zum **/StoreManager/Edit/1**.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-319">Change the URL to **/StoreManager/Edit/1**.</span></span> <span data-ttu-id="0a1e8-320">Ändern Sie den Titel Album in **laden** , und klicken Sie auf **speichern**.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-320">Change the Album title to **Load** and click on **Save**.</span></span> <span data-ttu-id="0a1e8-321">Stellen Sie sicher, dass die Albumtitel tatsächlich in der Liste der Alben geändert.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-321">Verify that album's title actually changed in the list of albums.</span></span>

    <span data-ttu-id="0a1e8-322">![Aktualisieren ein Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "ein Album aktualisieren")</span><span class="sxs-lookup"><span data-stu-id="0a1e8-322">![Updating an album](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "Updating an album")</span></span>

    <span data-ttu-id="0a1e8-323">*Ein Album aktualisieren*</span><span class="sxs-lookup"><span data-stu-id="0a1e8-323">*Updating an Album*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Adding_a_Create_View"></a>
### <a name="exercise-4-adding-a-create-view"></a><span data-ttu-id="0a1e8-324">Übung 4: Hinzufügen einer Ansicht erstellen</span><span class="sxs-lookup"><span data-stu-id="0a1e8-324">Exercise 4: Adding a Create View</span></span>

<span data-ttu-id="0a1e8-325">Nun, dass der **StoreManagerController** unterstützt die **bearbeiten** Fähigkeit, in dieser Übung Sie erfahren, wie zum Hinzufügen einer Create View-Vorlage, damit Sie das Speichern von Managern neue Alben zur Anwendung hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-325">Now that the **StoreManagerController** supports the **Edit** ability, in this exercise you will learn how to add a Create View template to let store managers add new Albums to the application.</span></span>

<span data-ttu-id="0a1e8-326">Wie Sie mit der Bearbeitungsfunktion getan haben, implementieren Sie das Create-Szenario mit zwei separate Methoden innerhalb der **StoreManagerController** Klasse:</span><span class="sxs-lookup"><span data-stu-id="0a1e8-326">Like you did with the Edit functionality, you will implement the Create scenario using two separate methods within the **StoreManagerController** class:</span></span>

1. <span data-ttu-id="0a1e8-327">Eine Aktionsmethode wird ein leeres Formular angezeigt, wenn der Speicher-Manager zum ersten Mal besuchen der **/StoreManager/Create** URL.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-327">One action method will display an empty form when store managers first visit the **/StoreManager/Create** URL.</span></span>
2. <span data-ttu-id="0a1e8-328">Eine zweite Aktionsmethode behandelt das Szenario, in dem der Speicher-Manager klickt der **speichern** Schaltfläche im Formular und sendet die Werte zurück, an die **/StoreManager/Create** URL als eine HTTP-POST.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-328">A second action method will handle the scenario where the store manager clicks the **Save** button within the form and submits the values back to the **/StoreManager/Create** URL as an HTTP-POST.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Create_action_method"></a>
#### <a name="task-1---implementing-the-http-get-create-action-method"></a><span data-ttu-id="0a1e8-329">Aufgabe 1: Erstellen von HTTP-GET-Aktionsmethode implementieren</span><span class="sxs-lookup"><span data-stu-id="0a1e8-329">Task 1 - Implementing the HTTP-GET Create action method</span></span>

<span data-ttu-id="0a1e8-330">In diesem Schritt implementieren Sie die HTTP-GET-Version von der Create Action-Methode zum Abrufen einer Liste aller Genres und Künstler, verpackt diese Daten in einem **StoreManagerViewModel** -Objekt, das dann an eine Ansichtenvorlage übergeben wird.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-330">In this task, you will implement the HTTP-GET version of the Create action method to retrieve a list of all Genres and Artists, package this data up into a **StoreManagerViewModel** object, which will then be passed to a View template.</span></span>

1. <span data-ttu-id="0a1e8-331">Öffnen der **beginnen** Lösung finden Sie unter **Quell-/Ex4-AddingACreateView/Begin/** Ordner.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-331">Open the **Begin** solution located at **Source/Ex4-AddingACreateView/Begin/** folder.</span></span> <span data-ttu-id="0a1e8-332">Andernfalls möglicherweise weiterhin mithilfe der **End** Lösung abgerufen, von der vorherigen Übung abschließen.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-332">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="0a1e8-333">Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung müssen Sie einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-333">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="0a1e8-334">Klicken Sie hierzu auf die **Projekt** Menü **NuGet-Pakete verwalten**.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-334">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="0a1e8-335">In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-335">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="0a1e8-336">Schließlich erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-336">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="0a1e8-337">Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken, die sich in Ihrem Projekt liefern Verringern der Projektgröße.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-337">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="0a1e8-338">Mit NuGet-Powertools werden durch Angabe der Paketversionen in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, wenn, das Sie das Projekt ausführen, können.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-338">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="0a1e8-339">Deshalb wird müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-339">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="0a1e8-340">Open **StoreManagerController** Klasse.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-340">Open **StoreManagerController** class.</span></span> <span data-ttu-id="0a1e8-341">Erweitern Sie hierzu die **Controller** Ordner und doppelklicken Sie auf **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-341">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="0a1e8-342">Ersetzen Sie die **erstellen** Aktion-Methode durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="0a1e8-342">Replace the **Create** action method code with the following:</span></span>

    <span data-ttu-id="0a1e8-343">(Codeausschnitt - *ASP.NET MVC 4-Hilfsprogrammen und Formulare und Validierung - Aktion Ex4 StoreManagerController HTTP-GET erstellen*)</span><span class="sxs-lookup"><span data-stu-id="0a1e8-343">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex4 StoreManagerController HTTP-GET Create action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample12.cs)]

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_the_Create_View"></a>
#### <a name="task-2---adding-the-create-view"></a><span data-ttu-id="0a1e8-344">Aufgabe 2: Hinzufügen der Ansicht erstellen</span><span class="sxs-lookup"><span data-stu-id="0a1e8-344">Task 2 - Adding the Create View</span></span>

<span data-ttu-id="0a1e8-345">In dieser Aufgabe fügen Sie die Create View-Vorlage, die ein neues (leeres) Album Formular angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-345">In this task, you will add the Create View template that will display a new (empty) Album form.</span></span>

1. <span data-ttu-id="0a1e8-346">Mit der rechten Maustaste innerhalb der **erstellen** Aktionsmethode, und wählen **Ansicht hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-346">Right-click inside the **Create** action method and select **Add View**.</span></span> <span data-ttu-id="0a1e8-347">Hierdurch wird das Dialogfeld "Ansicht hinzufügen" angezeigt.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-347">This will bring up the Add View dialog.</span></span>
2. <span data-ttu-id="0a1e8-348">Stellen Sie sicher, dass der Ansichtsname ist, klicken Sie im Dialogfeld "Ansicht hinzufügen" **erstellen**.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-348">In the Add View dialog, verify that the View Name is **Create**.</span></span> <span data-ttu-id="0a1e8-349">Wählen Sie die **eine stark typisierte Ansicht erstellen** aus, und wählen Sie **Album (MvcMusicStore.Models)** aus der **Modellschemas** Dropdown-Menü und **erstellen** aus der **Gerüst Vorlage** Dropdownliste aus.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-349">Select the **Create a strongly-typed view** option and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down and **Create** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="0a1e8-350">Lassen Sie die anderen Felder die Standardwerte, und klicken Sie dann auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-350">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="0a1e8-351">![Hinzufügen einer Erstellungsansicht](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "hinzufügen-a-erstellen-view.png")</span><span class="sxs-lookup"><span data-stu-id="0a1e8-351">![Adding a create view](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "adding-a-create-view.png")</span></span>

    <span data-ttu-id="0a1e8-352">*Hinzufügen der Ansicht erstellen*</span><span class="sxs-lookup"><span data-stu-id="0a1e8-352">*Adding the Create View*</span></span>
3. <span data-ttu-id="0a1e8-353">Update der **GenreId** und **ArtistId** Felder, die eine Dropdown-Liste verwendet wird, wie unten dargestellt:</span><span class="sxs-lookup"><span data-stu-id="0a1e8-353">Update the **GenreId** and **ArtistId** fields to use a drop-down list as shown below:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample13.cshtml)]

<a id="Ex4Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="0a1e8-354">Aufgabe 3: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="0a1e8-354">Task 3 - Running the Application</span></span>

<span data-ttu-id="0a1e8-355">In dieser Aufgabe testen Sie, die die **StoreManager** **erstellen** Ansichtsseite zeigt ein leeres Formular ein Album.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-355">In this task, you will test that the **StoreManager** **Create** View page displays an empty Album form.</span></span>

1. <span data-ttu-id="0a1e8-356">Drücken Sie **F5** um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-356">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="0a1e8-357">Das Projekt wird auf der Startseite gestartet.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-357">The project starts in the Home page.</span></span> <span data-ttu-id="0a1e8-358">Ändern Sie die URL zum **/StoreManager/Create**.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-358">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="0a1e8-359">Stellen Sie sicher, dass ein leeres Formular zum Ausfüllen der Eigenschaften der neuen Album angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-359">Verify that an empty form is displayed for filling the new Album properties.</span></span>

    <span data-ttu-id="0a1e8-360">![Erstellen Sie die Ansicht mit einem leeren Formular](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "Create View mit einem leeren Formular")</span><span class="sxs-lookup"><span data-stu-id="0a1e8-360">![Create View with an empty form](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "Create View with an empty form")</span></span>

    <span data-ttu-id="0a1e8-361">*Ansicht mit einem leeren Formular erstellen*</span><span class="sxs-lookup"><span data-stu-id="0a1e8-361">*Create View with an empty form*</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_-_Implementing_the_HTTP-POST_Create_Action_Method"></a>
#### <a name="task-4---implementing-the-http-post-create-action-method"></a><span data-ttu-id="0a1e8-362">Aufgabe 4: Implementieren des HTTP-POST-Create Aktionsmethode</span><span class="sxs-lookup"><span data-stu-id="0a1e8-362">Task 4 - Implementing the HTTP-POST Create Action Method</span></span>

<span data-ttu-id="0a1e8-363">In diesem Schritt implementieren Sie die HTTP-POST-Version der Create-Aktionsmethode, die aufgerufen wird, wenn ein Benutzer klickt auf die **speichern** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-363">In this task, you will implement the HTTP-POST version of the Create action method that will be invoked when a user clicks the **Save** button.</span></span> <span data-ttu-id="0a1e8-364">Die Methode sollte das neue Album in der Datenbank speichern.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-364">The method should save the new album in the database.</span></span>

1. <span data-ttu-id="0a1e8-365">Schließen Sie den Browser, die ggf. zu Visual Studio-Fenster zurückzukehren.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-365">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="0a1e8-366">Open **StoreManagerController** Klasse.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-366">Open **StoreManagerController** class.</span></span> <span data-ttu-id="0a1e8-367">Erweitern Sie hierzu die **Controller** Ordner und doppelklicken Sie auf **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-367">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
2. <span data-ttu-id="0a1e8-368">Ersetzen Sie **Erstellen von HTTP-POST** Aktion-Methode durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="0a1e8-368">Replace **HTTP-POST Create** action method code with the following:</span></span>

    <span data-ttu-id="0a1e8-369">(Codeausschnitt - *ASP.NET MVC 4-Hilfsprogrammen und Formulare und Validierung - Ex4 StoreManagerController HTTP - POST Aktion erstellen*)</span><span class="sxs-lookup"><span data-stu-id="0a1e8-369">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex4 StoreManagerController HTTP- POST Create action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample14.cs)]

    > [!NOTE]
    > <span data-ttu-id="0a1e8-370">Die Erstellungsaktion ist ziemlich ähnelt der vorherigen bearbeiten-Aktionsmethode, aber statt der Festlegung des Objekts, geändert wurde, wird es zum Kontext hinzugefügt wird.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-370">The Create action is pretty similar to the previous Edit action method but instead of setting the object as modified, it is being added to the context.</span></span>

<a id="Ex4Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="0a1e8-371">Aufgabe 5: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="0a1e8-371">Task 5 - Running the Application</span></span>

<span data-ttu-id="0a1e8-372">In dieser Aufgabe testen Sie, die die **StoreManager erstellen** Ansichtsseite können Sie ein neues Album erstellen und leitet dann an die Indexansicht StoreManager.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-372">In this task, you will test that the **StoreManager Create** View page lets you create a new Album and then redirects to the StoreManager Index View.</span></span>

1. <span data-ttu-id="0a1e8-373">Drücken Sie **F5** um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-373">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="0a1e8-374">Das Projekt wird auf der Startseite gestartet.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-374">The project starts in the Home page.</span></span> <span data-ttu-id="0a1e8-375">Ändern Sie die URL zum **/StoreManager/Create**.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-375">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="0a1e8-376">Füllen Sie alle Felder des Formulars mit Daten für ein neues Album, wie Sie in der folgenden Abbildung:</span><span class="sxs-lookup"><span data-stu-id="0a1e8-376">Fill all the form fields with data for a new Album, like the one in the following figure:</span></span>

    <span data-ttu-id="0a1e8-377">![Erstellen ein Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "ein Album erstellen")</span><span class="sxs-lookup"><span data-stu-id="0a1e8-377">![Creating an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "Creating an Album")</span></span>

    <span data-ttu-id="0a1e8-378">*Erstellen ein Album*</span><span class="sxs-lookup"><span data-stu-id="0a1e8-378">*Creating an Album*</span></span>
3. <span data-ttu-id="0a1e8-379">Stellen Sie sicher, dass Sie die Indexansicht StoreManager umgeleitet abrufen, die soeben erstellte neue Album enthält.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-379">Verify that you get redirected to the StoreManager Index View that includes the new Album just created.</span></span>

    <span data-ttu-id="0a1e8-380">![Neues Album erstellt](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "neues Album erstellt")</span><span class="sxs-lookup"><span data-stu-id="0a1e8-380">![New Album Created](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "New Album Created")</span></span>

    <span data-ttu-id="0a1e8-381">*Neues Album erstellt*</span><span class="sxs-lookup"><span data-stu-id="0a1e8-381">*New Album Created*</span></span>

<a id="Exercise5"></a>

<a id="Exercise_5_Handling_Deletion"></a>
### <a name="exercise-5-handling-deletion"></a><span data-ttu-id="0a1e8-382">Übung 5: Behandlung von löschen</span><span class="sxs-lookup"><span data-stu-id="0a1e8-382">Exercise 5: Handling Deletion</span></span>

<span data-ttu-id="0a1e8-383">Die Möglichkeit, Alben löschen wurde noch nicht implementiert.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-383">The ability to delete albums is not yet implemented.</span></span> <span data-ttu-id="0a1e8-384">Dies ist zum werden in dieser Übung.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-384">This is what this exercise will be about.</span></span> <span data-ttu-id="0a1e8-385">Wie vorher, implementieren Sie die Delete-Szenario mit zwei separate Methoden innerhalb der **StoreManagerController** Klasse:</span><span class="sxs-lookup"><span data-stu-id="0a1e8-385">Like before, you will implement the Delete scenario using two separate methods within the **StoreManagerController** class:</span></span>

1. <span data-ttu-id="0a1e8-386">Eine Aktionsmethode wird eine Form der Bestätigung angezeigt.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-386">One action method will display a confirmation form</span></span>
2. <span data-ttu-id="0a1e8-387">Eine zweite Aktionsmethode verarbeitet der Übermittlung des Formulars</span><span class="sxs-lookup"><span data-stu-id="0a1e8-387">A second action method will handle the form submission</span></span>

<a id="Ex5Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Delete_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-delete-action-method"></a><span data-ttu-id="0a1e8-388">Aufgabe 1 – Implementieren der HTTP-GET-Delete-Aktion-Methode</span><span class="sxs-lookup"><span data-stu-id="0a1e8-388">Task 1 - Implementing the HTTP-GET Delete Action Method</span></span>

<span data-ttu-id="0a1e8-389">In dieser Aufgabe wird die HTTP-GET-Version von der Delete-Aktion-Methode zum Abrufen von Informationen für das Album implementiert werden.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-389">In this task, you will implement the HTTP-GET version of the Delete action method to retrieve the album's information.</span></span>

1. <span data-ttu-id="0a1e8-390">Öffnen der **beginnen** Lösung finden Sie unter **Quell-/Ex5-HandlingDeletion/Begin/** Ordner.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-390">Open the **Begin** solution located at **Source/Ex5-HandlingDeletion/Begin/** folder.</span></span> <span data-ttu-id="0a1e8-391">Andernfalls möglicherweise weiterhin mithilfe der **End** Lösung abgerufen, von der vorherigen Übung abschließen.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-391">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="0a1e8-392">Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung müssen Sie einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-392">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="0a1e8-393">Klicken Sie hierzu auf die **Projekt** Menü **NuGet-Pakete verwalten**.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-393">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="0a1e8-394">In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-394">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="0a1e8-395">Schließlich erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-395">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="0a1e8-396">Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken, die sich in Ihrem Projekt liefern Verringern der Projektgröße.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-396">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="0a1e8-397">Mit NuGet-Powertools werden durch Angabe der Paketversionen in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, wenn, das Sie das Projekt ausführen, können.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-397">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="0a1e8-398">Deshalb wird müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-398">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="0a1e8-399">Open **StoreManagerController** Klasse.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-399">Open **StoreManagerController** class.</span></span> <span data-ttu-id="0a1e8-400">Erweitern Sie hierzu die **Controller** Ordner und doppelklicken Sie auf **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-400">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="0a1e8-401">Die Löschaktion der Controller ist identisch mit der vorherigen Store Details Controlleraktion: anschließend fragt es die **Album** Objekt aus der Datenbank der **Id** bereitgestellt, die in der URL und gibt die entsprechende **Ansicht**.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-401">The Delete controller action is exactly the same as the previous Store Details controller action: it queries the **album** object from the database using the **id** provided in the URL and returns the appropriate **View**.</span></span> <span data-ttu-id="0a1e8-402">Ersetzen Sie hierzu die HTTP-GET **löschen** Aktion-Methode durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="0a1e8-402">To do this, replace the HTTP-GET **Delete** action method code with the following:</span></span>

    <span data-ttu-id="0a1e8-403">(Codeausschnitt - *ASP.NET MVC 4-Hilfsprogrammen und Formulare und Validierung - Aktion Ex5 löschen Behandlung von HTTP-GET löschen*)</span><span class="sxs-lookup"><span data-stu-id="0a1e8-403">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex5 Handling Deletion HTTP-GET Delete action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample15.cs)]
4. <span data-ttu-id="0a1e8-404">Mit der rechten Maustaste innerhalb der **löschen** Aktionsmethode, und wählen **Ansicht hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-404">Right-click inside the **Delete** action method and select **Add View**.</span></span> <span data-ttu-id="0a1e8-405">Hierdurch wird das Dialogfeld "Ansicht hinzufügen" angezeigt.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-405">This will bring up the Add View dialog.</span></span>
5. <span data-ttu-id="0a1e8-406">Stellen Sie sicher, dass der Ansichtsname ist, klicken Sie im Dialogfeld "Ansicht hinzufügen" **löschen**.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-406">In the Add View dialog, verify that the View name is **Delete**.</span></span> <span data-ttu-id="0a1e8-407">Wählen Sie die **eine stark typisierte Ansicht erstellen** aus, und wählen Sie **Album (MvcMusicStore.Models)** aus der **Modellschemas** Dropdown-Menü.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-407">Select the **Create a strongly-typed view** option and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down.</span></span> <span data-ttu-id="0a1e8-408">Wählen Sie **löschen** aus der **Gerüst Vorlage** Dropdownliste aus.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-408">Select **Delete** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="0a1e8-409">Lassen Sie die anderen Felder die Standardwerte, und klicken Sie dann auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-409">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="0a1e8-410">![Hinzufügen einer Löschansicht](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "Hinzufügen einer Löschansicht")</span><span class="sxs-lookup"><span data-stu-id="0a1e8-410">![Adding a Delete View](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "Adding a Delete View")</span></span>

    <span data-ttu-id="0a1e8-411">*Hinzufügen einer Löschansicht*</span><span class="sxs-lookup"><span data-stu-id="0a1e8-411">*Adding a Delete View*</span></span>
6. <span data-ttu-id="0a1e8-412">Die Vorlage "löschen" zeigt alle Felder aus dem Modell.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-412">The Delete template shows all the fields from the model.</span></span> <span data-ttu-id="0a1e8-413">Sie werden nur die Album Titel angezeigt.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-413">You will show only the album's title.</span></span> <span data-ttu-id="0a1e8-414">Ersetzen Sie dazu den Inhalt der Ansicht mit den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="0a1e8-414">To do this, replace the content of the view with the following code:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample16.cshtml)]

<a id="Ex05Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="0a1e8-415">Aufgabe 2: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="0a1e8-415">Task 2 - Running the Application</span></span>

<span data-ttu-id="0a1e8-416">In dieser Aufgabe testen Sie, die die **StoreManager** **löschen** Ansichtsseite zeigt eine Form der Bestätigung löschen.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-416">In this task, you will test that the **StoreManager** **Delete** View page displays a confirmation deletion form.</span></span>

1. <span data-ttu-id="0a1e8-417">Drücken Sie **F5** um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-417">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="0a1e8-418">Das Projekt wird auf der Startseite gestartet.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-418">The project starts in the Home page.</span></span> <span data-ttu-id="0a1e8-419">Ändern Sie die URL zum **/StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-419">Change the URL to **/StoreManager**.</span></span> <span data-ttu-id="0a1e8-420">Wählen Sie ein Album zu löschen, indem Sie auf **löschen** und stellen Sie sicher, dass die neue Ansicht hochgeladen wurde.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-420">Select one album to delete by clicking **Delete** and verify that the new view is uploaded.</span></span>

    <span data-ttu-id="0a1e8-421">![Löschen ein Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "ein Album löschen")</span><span class="sxs-lookup"><span data-stu-id="0a1e8-421">![Deleting an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "Deleting an Album")</span></span>

    <span data-ttu-id="0a1e8-422">*Löschen ein Album*</span><span class="sxs-lookup"><span data-stu-id="0a1e8-422">*Deleting an Album*</span></span>

<a id="Ex05Task3"></a>

<a id="Task_3-_Implementing_the_HTTP-POST_Delete_Action_Method"></a>
#### <a name="task-3--implementing-the-http-post-delete-action-method"></a><span data-ttu-id="0a1e8-423">Aufgabe 3 – Implementieren der HTTP-POST-Delete-Aktion-Methode</span><span class="sxs-lookup"><span data-stu-id="0a1e8-423">Task 3- Implementing the HTTP-POST Delete Action Method</span></span>

<span data-ttu-id="0a1e8-424">In diesem Schritt implementieren Sie die HTTP-POST-Version der Delete-Aktionsmethode, die aufgerufen wird, wenn ein Benutzer klickt auf die **löschen** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-424">In this task, you will implement the HTTP-POST version of the Delete action method that will be invoked when a user clicks the **Delete** button.</span></span> <span data-ttu-id="0a1e8-425">Die Methode sollte das Album in der Datenbank löschen.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-425">The method should delete the album in the database.</span></span>

1. <span data-ttu-id="0a1e8-426">Schließen Sie den Browser, die ggf. zu Visual Studio-Fenster zurückzukehren.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-426">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="0a1e8-427">Open **StoreManagerController** Klasse.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-427">Open **StoreManagerController** class.</span></span> <span data-ttu-id="0a1e8-428">Erweitern Sie hierzu die **Controller** Ordner und doppelklicken Sie auf **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-428">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
2. <span data-ttu-id="0a1e8-429">Ersetzen Sie **HTTP-POST löschen** Aktion-Methode durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="0a1e8-429">Replace **HTTP-POST Delete** action method code with the following:</span></span>

    <span data-ttu-id="0a1e8-430">(Codeausschnitt - *ASP.NET MVC 4-Hilfsprogrammen und Formulare und Validierung - Aktion Ex5 löschen Behandlung von HTTP-POST löschen*)</span><span class="sxs-lookup"><span data-stu-id="0a1e8-430">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex5 Handling Deletion HTTP-POST Delete action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample17.cs)]

<a id="Ex5Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="0a1e8-431">Aufgabe 4: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="0a1e8-431">Task 4 - Running the Application</span></span>

<span data-ttu-id="0a1e8-432">In dieser Aufgabe testen Sie, die die **StoreManager Delete** Ansichtsseite können Sie ein Album löschen und leitet dann an die Indexansicht StoreManager.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-432">In this task, you will test that the **StoreManager Delete** View page lets you delete an Album and then redirects to the StoreManager Index View.</span></span>

1. <span data-ttu-id="0a1e8-433">Drücken Sie **F5** um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-433">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="0a1e8-434">Das Projekt wird auf der Startseite gestartet.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-434">The project starts in the Home page.</span></span> <span data-ttu-id="0a1e8-435">Ändern Sie die URL zum **/StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-435">Change the URL to **/StoreManager**.</span></span> <span data-ttu-id="0a1e8-436">Wählen Sie ein Album zu löschen, indem Sie auf **löschen.**</span><span class="sxs-lookup"><span data-stu-id="0a1e8-436">Select one album to delete by clicking **Delete.**</span></span> <span data-ttu-id="0a1e8-437">Bestätigen Sie den Vorgang, indem Sie auf **löschen** Schaltfläche:</span><span class="sxs-lookup"><span data-stu-id="0a1e8-437">Confirm the deletion by clicking **Delete** button:</span></span>

    <span data-ttu-id="0a1e8-438">![Löschen ein Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "ein Album löschen")</span><span class="sxs-lookup"><span data-stu-id="0a1e8-438">![Deleting an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "Deleting an Album")</span></span>

    <span data-ttu-id="0a1e8-439">*Löschen ein Album*</span><span class="sxs-lookup"><span data-stu-id="0a1e8-439">*Deleting an Album*</span></span>
3. <span data-ttu-id="0a1e8-440">Stellen Sie sicher, dass Album gelöscht wurde, da sie nicht in erscheint die **Index** Seite.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-440">Verify that the album was deleted since it does not appear in the **Index** page.</span></span>

<a id="Exercise6"></a>

<a id="Exercise_6_Adding_Validation"></a>
### <a name="exercise-6-adding-validation"></a><span data-ttu-id="0a1e8-441">Übung 6: Hinzufügen einer Validierung</span><span class="sxs-lookup"><span data-stu-id="0a1e8-441">Exercise 6: Adding Validation</span></span>

<span data-ttu-id="0a1e8-442">Das Erstellen und Bearbeiten von Formulare, die erfüllt sein führen zurzeit keine Art von Validierung.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-442">Currently, the Create and Edit forms you have in place do not perform any kind of validation.</span></span> <span data-ttu-id="0a1e8-443">Wenn der Benutzer ein erforderliches Feld leer oder Typ Buchstaben im Feld "Preis" verlässt, wird der erste Fehler erhalten Sie aus der Datenbank sein.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-443">If the user leaves a required field blank or type letters in the price field, the first error you will get will be from the database.</span></span>

<span data-ttu-id="0a1e8-444">Sie können die Anwendung durch Hinzufügen von Datenanmerkungen zu Ihrer Modellklasse Validierung hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-444">You can add validation to the application by adding Data Annotations to your model class.</span></span> <span data-ttu-id="0a1e8-445">Datenanmerkungen ermöglichen es, beschreibt die gewünschten Regeln auf die Modelleigenschaften angewendet, und ASP.NET MVC übernimmt erzwingen und die entsprechende Meldung an Benutzer anzeigen.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-445">Data Annotations allow describing the rules you want applied to your model properties, and ASP.NET MVC will take care of enforcing and displaying appropriate message to users.</span></span>

<a id="Ex06Task1"></a>

<a id="Task_1_-_Adding_Data_Annotations"></a>
#### <a name="task-1---adding-data-annotations"></a><span data-ttu-id="0a1e8-446">Aufgabe 1: Hinzufügen von Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="0a1e8-446">Task 1 - Adding Data Annotations</span></span>

<span data-ttu-id="0a1e8-447">In dieser Aufgabe fügen Sie Datenanmerkungen dem Album-Modell, die die Seite "erstellen und Bearbeiten von" vornehmen, werden validierungsmeldungen gegebenenfalls angezeigt.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-447">In this task, you will add Data Annotations to the Album Model that will make the Create and Edit page display validation messages when appropriate.</span></span>

<span data-ttu-id="0a1e8-448">Für eine einfache Modellklasse, Hinzufügen einer Anmerkung Daten nur erfolgt durch Hinzufügen einer **mit** -Anweisung für **System.ComponentModel.DataAnnotation**, platzieren dann eine **[erforderlich]**Attribut in der entsprechenden Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-448">For a simple Model class, adding a Data Annotation is just handled by adding a **using** statement for **System.ComponentModel.DataAnnotation**, then placing a **[Required]** attribute on the appropriate properties.</span></span> <span data-ttu-id="0a1e8-449">Im folgende Beispiel würde machen die **Namen** Eigenschaft ein erforderliches Feld in der Ansicht.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-449">The following example would make the **Name** property a required field in the View.</span></span>

[!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample18.cs)]

<span data-ttu-id="0a1e8-450">Dies ist ein wenig komplexer in Fällen, wie diese Anwendung, in dem das Entity Data Model generiert wird.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-450">This is a little more complex in cases like this application where the Entity Data Model is generated.</span></span> <span data-ttu-id="0a1e8-451">Wenn Sie direkt mit der Modellklassen Datenanmerkungen hinzugefügt haben, würden sie überschrieben, wenn Sie das Modell aus der Datenbank zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-451">If you added Data Annotations directly to the model classes, they would be overwritten if you update the model from the database.</span></span> <span data-ttu-id="0a1e8-452">Sie können stattdessen machen Metadaten partiellen Klassen ist vorhanden, um die Anmerkungen zu speichern und mit dem Modell verknüpft sind Klassen, mit der **[MetadataType]** Attribut.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-452">Instead, you can make use of metadata partial classes which will exist to hold the annotations and are associated with the model classes using the **[MetadataType]** attribute.</span></span>

1. <span data-ttu-id="0a1e8-453">Öffnen der **beginnen** Lösung finden Sie unter **Quell-/Ex6-AddingValidation/Begin/** Ordner.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-453">Open the **Begin** solution located at **Source/Ex6-AddingValidation/Begin/** folder.</span></span> <span data-ttu-id="0a1e8-454">Andernfalls möglicherweise weiterhin mithilfe der **End** Lösung abgerufen, von der vorherigen Übung abschließen.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-454">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="0a1e8-455">Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung müssen Sie einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-455">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="0a1e8-456">Klicken Sie hierzu auf die **Projekt** Menü **NuGet-Pakete verwalten**.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-456">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="0a1e8-457">In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-457">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="0a1e8-458">Schließlich erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-458">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="0a1e8-459">Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken, die sich in Ihrem Projekt liefern Verringern der Projektgröße.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-459">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="0a1e8-460">Mit NuGet-Powertools werden durch Angabe der Paketversionen in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, wenn, das Sie das Projekt ausführen, können.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-460">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="0a1e8-461">Deshalb wird müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-461">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="0a1e8-462">Öffnen der **Album.cs** aus der **Modelle** Ordner.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-462">Open the **Album.cs** from the **Models** folder.</span></span>
3. <span data-ttu-id="0a1e8-463">Ersetzen Sie **Album.cs** Inhalt mit der hervorgehobene Code, damit er wie folgt aussieht:</span><span class="sxs-lookup"><span data-stu-id="0a1e8-463">Replace **Album.cs** content with the highlighted code, so that it looks like the following:</span></span>

    > [!NOTE]
    > <span data-ttu-id="0a1e8-464">Die Zeile **[DisplayFormat(ConvertEmptyStringToNull=false)]** gibt an, dass leere Zeichenfolgen aus dem Modell nicht auf null konvertiert werden, wenn das Feld "Daten" in der Datenquelle aktualisiert wird.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-464">The line **[DisplayFormat(ConvertEmptyStringToNull=false)]** indicates that empty strings from the model won't be converted to null when the data field is updated in the data source.</span></span> <span data-ttu-id="0a1e8-465">Diese Einstellung wird eine Ausnahme vermeiden, wenn das Entity Framework null-Werte für das Modell, weist bevor Datenanmerkung Felder überprüft.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-465">This setting will avoid an exception when the Entity Framework assigns null values to the model before Data Annotation validates the fields.</span></span>

    <span data-ttu-id="0a1e8-466">(Codeausschnitt - *ASP.NET MVC 4-Hilfsprogrammen und Formulare und Validierung - Ex6 Album Metadaten Teilklasse*)</span><span class="sxs-lookup"><span data-stu-id="0a1e8-466">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex6 Album metadata partial class*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample19.cs)]

    > [!NOTE]
    > <span data-ttu-id="0a1e8-467">Dies **Album** partielle Klasse verfügt über eine **MetadataType** Attribut verweist auf die **AlbumMetaData** Klasse für die Datenanmerkungen.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-467">This **Album** partial class has a **MetadataType** attribute which points to the **AlbumMetaData** class for the Data Annotations.</span></span> <span data-ttu-id="0a1e8-468">Dies sind einige der Datenanmerkung Attribute, die Sie verwenden, um das Modell Album versehen:</span><span class="sxs-lookup"><span data-stu-id="0a1e8-468">These are some of the Data Annotation attributes you are using to annotate the Album model:</span></span>
    > 
    > - <span data-ttu-id="0a1e8-469">Erforderlich. – gibt an, dass die Eigenschaft ein Pflichtfeld ist.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-469">Required - Indicates that the property is a required field</span></span>
    > - <span data-ttu-id="0a1e8-470">DisplayName - definiert den Text für Formularfelder und validierungsmeldungen verwendet werden soll</span><span class="sxs-lookup"><span data-stu-id="0a1e8-470">DisplayName - Defines the text to be used on form fields and validation messages</span></span>
    > - <span data-ttu-id="0a1e8-471">DisplayFormat - gibt an, wie Datenfelder angezeigt und formatiert sind.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-471">DisplayFormat - Specifies how data fields are displayed and formatted.</span></span>
    > - <span data-ttu-id="0a1e8-472">StringLength - definiert die maximale Länge für ein Zeichenfolgenfeld eingegeben</span><span class="sxs-lookup"><span data-stu-id="0a1e8-472">StringLength - Defines a maximum length for a string field</span></span>
    > - <span data-ttu-id="0a1e8-473">Bereich - weist den maximalen und minimalen Wert für ein numerisches Feld</span><span class="sxs-lookup"><span data-stu-id="0a1e8-473">Range - Gives a maximum and minimum value for a numeric field</span></span>
    > - <span data-ttu-id="0a1e8-474">ScaffoldColumn - ermöglicht das Ausblenden von Feldern aus Editor Formularen</span><span class="sxs-lookup"><span data-stu-id="0a1e8-474">ScaffoldColumn - Allows hiding fields from editor forms</span></span>

<a id="Ex06Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="0a1e8-475">Aufgabe 2: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="0a1e8-475">Task 2 - Running the Application</span></span>

<span data-ttu-id="0a1e8-476">In dieser Aufgabe testen Sie, dass die Seiten erstellen und Bearbeiten von Feldern, überprüfen, verwenden die Anzeigenamen in der vorhergehenden Aufgabe ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-476">In this task, you will test that the Create and Edit pages validate fields, using the display names chosen in the last task.</span></span>

1. <span data-ttu-id="0a1e8-477">Drücken Sie **F5** um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-477">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="0a1e8-478">Das Projekt wird auf der Startseite gestartet.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-478">The project starts in the Home page.</span></span> <span data-ttu-id="0a1e8-479">Ändern Sie die URL zum **/StoreManager/Create**.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-479">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="0a1e8-480">Stellen Sie sicher, dass die Anzeigenamen der in der partiellen Klasse entspricht (z. B. **Album Art URL** anstelle von **AlbumArtUrl**)</span><span class="sxs-lookup"><span data-stu-id="0a1e8-480">Verify that the display names match the ones in the partial class (like **Album Art URL** instead of **AlbumArtUrl**)</span></span>
3. <span data-ttu-id="0a1e8-481">Klicken Sie auf **erstellen**, ohne das Formular auszufüllen.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-481">Click **Create**, without filling the form.</span></span> <span data-ttu-id="0a1e8-482">Stellen Sie sicher, dass Sie die entsprechenden validierungsmeldungen erhalten.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-482">Verify that you get the corresponding validation messages.</span></span>

    <span data-ttu-id="0a1e8-483">![Überprüft die Felder in der Seite "erstellen"](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "überprüft die Felder in der Seite "erstellen"")</span><span class="sxs-lookup"><span data-stu-id="0a1e8-483">![Validated fields in the Create page](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "Validated fields in the Create page")</span></span>

    <span data-ttu-id="0a1e8-484">*Überprüfte Felder in der Seite "erstellen"*</span><span class="sxs-lookup"><span data-stu-id="0a1e8-484">*Validated fields in the Create page*</span></span>
4. <span data-ttu-id="0a1e8-485">Sie können überprüfen, ob das gleiche mit geschieht die **bearbeiten** Seite.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-485">You can verify that the same occurs with the **Edit** page.</span></span> <span data-ttu-id="0a1e8-486">Ändern Sie die URL zum **/StoreManager/Edit/1** und stellen Sie sicher, dass die Anzeigenamen der in der partiellen Klasse entspricht (z. B. **Album Art URL** anstelle von **AlbumArtUrl**).</span><span class="sxs-lookup"><span data-stu-id="0a1e8-486">Change the URL to **/StoreManager/Edit/1** and verify that the display names match the ones in the partial class (like **Album Art URL** instead of **AlbumArtUrl**).</span></span> <span data-ttu-id="0a1e8-487">Leere der **Titel** und **Preis** Felder, und klicken Sie auf **speichern**.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-487">Empty the **Title** and **Price** fields and click **Save**.</span></span> <span data-ttu-id="0a1e8-488">Stellen Sie sicher, dass Sie die entsprechenden validierungsmeldungen erhalten.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-488">Verify that you get the corresponding validation messages.</span></span>

    ![Überprüfte Felder bearbeiten (Seite)](aspnet-mvc-4-helpers-forms-and-validation/_static/image19.png)

    <span data-ttu-id="0a1e8-490">*Überprüfte Felder bearbeiten (Seite)*</span><span class="sxs-lookup"><span data-stu-id="0a1e8-490">*Validated fields in the Edit page*</span></span>

<a id="Exercise7"></a>

<a id="Exercise_7_Using_Unobtrusive_jQuery_at_Client_Side"></a>
### <a name="exercise-7-using-unobtrusive-jquery-at-client-side"></a><span data-ttu-id="0a1e8-491">Übung 7: Unter Verwendung von unaufdringliche jQuery auf Clientseite</span><span class="sxs-lookup"><span data-stu-id="0a1e8-491">Exercise 7: Using Unobtrusive jQuery at Client Side</span></span>

<span data-ttu-id="0a1e8-492">In dieser Übung erfahren Sie, wie MVC 4 unaufdringliche jQuery-Validierung auf Clientseite zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-492">In this exercise, you will learn how to enable MVC 4 Unobtrusive jQuery validation at client side.</span></span>

> [!NOTE]
> <span data-ttu-id="0a1e8-493">Die unaufdringliche jQuery verwendet Data-Ajax-Präfix JavaScript Aktionsmethoden auf den Server anstelle von intrusively Ausgeben von Inline-Clientskripts aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-493">The Unobtrusive jQuery uses data-ajax prefix JavaScript to invoke action methods on the server rather than intrusively emitting inline client scripts.</span></span>


<a id="Ex7Task1"></a>

<a id="Task_1_-_Running_the_Application_before_Enabling_Unobtrusive_jQuery"></a>
#### <a name="task-1---running-the-application-before-enabling-unobtrusive-jquery"></a><span data-ttu-id="0a1e8-494">Aufgabe 1: Ausführen der Anwendung vor dem Aktivieren unaufdringliche jQuery</span><span class="sxs-lookup"><span data-stu-id="0a1e8-494">Task 1 - Running the Application before Enabling Unobtrusive jQuery</span></span>

<span data-ttu-id="0a1e8-495">In dieser Aufgabe führen Sie die Anwendung vor dem Einfügen von jQuery damit beide zertifikatsvalidierungsmodelle verglichen werden kann.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-495">In this task, you will run the application before including jQuery in order to compare both validation models.</span></span>

1. <span data-ttu-id="0a1e8-496">Öffnen der **beginnen** Lösung finden Sie unter **Quell-/Ex7-UnobtrusivejQueryValidation/Begin/** Ordner.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-496">Open the **Begin** solution located at **Source/Ex7-UnobtrusivejQueryValidation/Begin/** folder.</span></span> <span data-ttu-id="0a1e8-497">Andernfalls möglicherweise weiterhin mithilfe der **End** Lösung abgerufen, von der vorherigen Übung abschließen.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-497">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="0a1e8-498">Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung müssen Sie einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-498">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="0a1e8-499">Klicken Sie hierzu auf die **Projekt** Menü **NuGet-Pakete verwalten**.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-499">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="0a1e8-500">In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-500">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="0a1e8-501">Schließlich erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-501">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="0a1e8-502">Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken, die sich in Ihrem Projekt liefern Verringern der Projektgröße.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-502">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="0a1e8-503">Mit NuGet-Powertools werden durch Angabe der Paketversionen in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, wenn, das Sie das Projekt ausführen, können.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-503">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="0a1e8-504">Deshalb wird müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-504">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="0a1e8-505">Drücken Sie **F5**, um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-505">Press **F5** to run the application.</span></span>
3. <span data-ttu-id="0a1e8-506">Das Projekt wird auf der Startseite gestartet.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-506">The project starts in the Home page.</span></span> <span data-ttu-id="0a1e8-507">Navigieren Sie **/StoreManager/Create** , und klicken Sie auf **erstellen** ohne Ausfüllen des Formulars, um sicherzustellen, dass Sie validierungsmeldungen abrufen:</span><span class="sxs-lookup"><span data-stu-id="0a1e8-507">Browse **/StoreManager/Create** and click **Create** without filling the form to verify that you get validation messages:</span></span>

    <span data-ttu-id="0a1e8-508">![Die Clientvalidierung deaktiviert](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "Clientvalidierung deaktiviert")</span><span class="sxs-lookup"><span data-stu-id="0a1e8-508">![Client validation disabled](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "Client validation disabled")</span></span>

    <span data-ttu-id="0a1e8-509">*Die Clientvalidierung deaktiviert*</span><span class="sxs-lookup"><span data-stu-id="0a1e8-509">*Client validation disabled*</span></span>
4. <span data-ttu-id="0a1e8-510">Öffnen Sie im Browser die HTML-Quellcode aus:</span><span class="sxs-lookup"><span data-stu-id="0a1e8-510">In the browser, open the HTML source code:</span></span>

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample20.html)]

<a id="Ex7Task2"></a>

<a id="Task_2_-_Enabling_Unobtrusive_Client_Validation"></a>
#### <a name="task-2---enabling-unobtrusive-client-validation"></a><span data-ttu-id="0a1e8-511">Aufgabe 2: Aktivieren der unaufdringlichen Clientvalidierung</span><span class="sxs-lookup"><span data-stu-id="0a1e8-511">Task 2 - Enabling Unobtrusive Client Validation</span></span>

<span data-ttu-id="0a1e8-512">In dieser Aufgabe aktivieren Sie jQuery **unaufdringliche Clientvalidierung** aus **"Web.config"** -Datei, die standardmäßig auf "false" in allen neuen ASP.NET MVC 4-Projekten festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-512">In this task, you will enable jQuery **unobtrusive client validation** from **Web.config** file, which is by default set to false in all new ASP.NET MVC 4 projects.</span></span> <span data-ttu-id="0a1e8-513">Fügen Sie außerdem, dass die erforderlichen Verweise zur jQuery unaufdringlichen Clientvalidierung Arbeit stellen Skripts.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-513">Additionally, you will add the necessary scripts references to make jQuery Unobtrusive Client Validation work.</span></span>

1. <span data-ttu-id="0a1e8-514">Open **"Web.config"** Projekt Stammverzeichnis der Datei, und stellen Sie sicher, dass die **ClientValidationEnabled** und **UnobtrusiveJavaScriptEnabled** SchlüsselWertefestgelegtsind**"true"**.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-514">Open **Web.Config** file at project root, and make sure that the **ClientValidationEnabled** and **UnobtrusiveJavaScriptEnabled** keys values are set to **true**.</span></span>

    [!code-xml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample21.xml)]

    > [!NOTE]
    > <span data-ttu-id="0a1e8-515">Sie können auch die Clientvalidierung durch Code am Global.asax.cs dieselben Ergebnisse abrufen aktivieren:</span><span class="sxs-lookup"><span data-stu-id="0a1e8-515">You can also enable client validation by code at Global.asax.cs to get the same results:</span></span>
    > 
    > <span data-ttu-id="0a1e8-516">**HtmlHelper.ClientValidationEnabled = True;**</span><span class="sxs-lookup"><span data-stu-id="0a1e8-516">**HtmlHelper.ClientValidationEnabled = true;**</span></span>
    > 
    > <span data-ttu-id="0a1e8-517">Darüber hinaus können Sie ClientValidationEnabled-Attributs in jedem Controller haben Sie ein benutzerdefiniertes Verhalten zuweisen.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-517">Additionally, you can assign ClientValidationEnabled attribute into any controller to have a custom behavior.</span></span>
2. <span data-ttu-id="0a1e8-518">Open **Create.cshtml** am **Views\StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-518">Open **Create.cshtml** at **Views\StoreManager**.</span></span>
3. <span data-ttu-id="0a1e8-519">Stellen Sie sicher, dass die folgenden Skriptdateien **jquery.validate** und **jquery.validate.unobtrusive**, verwiesen wird, in die Sicht kann die &quot; **~/bundles/jqueryval** &quot; Paket.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-519">Make sure the following script files, **jquery.validate** and **jquery.validate.unobtrusive**, are referenced in the view throught the &quot;**~/bundles/jqueryval**&quot; bundle.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample22.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="0a1e8-520">Alle diese jQuery-Bibliotheken sind in neuen MVC 4-Projekte enthalten.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-520">All these jQuery libraries are included in MVC 4 new projects.</span></span> <span data-ttu-id="0a1e8-521">Sie finden weitere Bibliotheken in der **/Skripts** Ordner "Projekt".</span><span class="sxs-lookup"><span data-stu-id="0a1e8-521">You can find more libraries in the **/Scripts** folder of you project.</span></span>
    > 
    > <span data-ttu-id="0a1e8-522">Clientbibliotheken funktionieren, müssen Sie einen Verweis auf die jQuery-Framework-Klassenbibliothek hinzufügen, um diese Überprüfung vornehmen zu können.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-522">In order to make this validation libraries work, you need to add a reference to the jQuery framework library.</span></span> <span data-ttu-id="0a1e8-523">Da dieser Referenz in bereits hinzugefügt wurde die  **\_Layout.cshtml** Datei, Sie müssen nicht in diese spezielle Ansicht hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-523">Since this reference is already added in the **\_Layout.cshtml** file, you do not need to add it in this particular view.</span></span>

<a id="Ex7Task3"></a>

<a id="Task_3_-_Running_the_Application_Using_Unobtrusive_jQuery_Validation"></a>
#### <a name="task-3---running-the-application-using-unobtrusive-jquery-validation"></a><span data-ttu-id="0a1e8-524">Aufgabe 3: Ausführen der Anwendung mithilfe von unaufdringliche jQuery-Überprüfung</span><span class="sxs-lookup"><span data-stu-id="0a1e8-524">Task 3 - Running the Application Using Unobtrusive jQuery Validation</span></span>

<span data-ttu-id="0a1e8-525">In dieser Aufgabe testen Sie, die die **StoreManager** Vorlage führt die clientseitige Validierung jQuery-Clientbibliotheken verwenden, wenn der Benutzer ein neues Album erstellt Ansicht erstellen.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-525">In this task, you will test that the **StoreManager** create view template performs client side validation using jQuery libraries when the user creates a new album.</span></span>

1. <span data-ttu-id="0a1e8-526">Drücken Sie **F5**, um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-526">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="0a1e8-527">Das Projekt wird auf der Startseite gestartet.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-527">The project starts in the Home page.</span></span> <span data-ttu-id="0a1e8-528">Navigieren Sie **/StoreManager/Create** , und klicken Sie auf **erstellen** ohne Ausfüllen des Formulars, um sicherzustellen, dass Sie validierungsmeldungen abrufen:</span><span class="sxs-lookup"><span data-stu-id="0a1e8-528">Browse **/StoreManager/Create** and click **Create** without filling the form to verify that you get validation messages:</span></span>

    <span data-ttu-id="0a1e8-529">![Die Clientvalidierung mit jQuery aktiviert](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "Clientvalidierung mit jQuery aktiviert")</span><span class="sxs-lookup"><span data-stu-id="0a1e8-529">![Client validation with jQuery enabled](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "Client validation with jQuery enabled")</span></span>

    <span data-ttu-id="0a1e8-530">*Die Clientvalidierung mit jQuery aktiviert*</span><span class="sxs-lookup"><span data-stu-id="0a1e8-530">*Client validation with jQuery enabled*</span></span>
3. <span data-ttu-id="0a1e8-531">Öffnen Sie im Browser den Quellcode für die Ansicht erstellen:</span><span class="sxs-lookup"><span data-stu-id="0a1e8-531">In the browser, open the source code for Create view:</span></span>

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample23.html)]

    > [!NOTE]
    > <span data-ttu-id="0a1e8-532">Für jede Clientvalidierungsregel unaufdringliche jQuery Fügt ein Attribut mit Daten-Val -*Rulename*=&quot;*Nachricht*&quot;.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-532">For each client validation rule, Unobtrusive jQuery adds an attribute with data-val-*rulename*=&quot;*message*&quot;.</span></span> <span data-ttu-id="0a1e8-533">Im folgenden finden Sie eine Liste der Tags, unauffälliger jQuery fügt die html-Eingabefeld Clientvalidierung ausführen:</span><span class="sxs-lookup"><span data-stu-id="0a1e8-533">Below is a list of tags that Unobtrusive jQuery inserts into the html input field to perform client validation:</span></span>
    > 
    > - <span data-ttu-id="0a1e8-534">Daten-val</span><span class="sxs-lookup"><span data-stu-id="0a1e8-534">Data-val</span></span>
    > - <span data-ttu-id="0a1e8-535">Daten-Val-Anzahl</span><span class="sxs-lookup"><span data-stu-id="0a1e8-535">Data-val-number</span></span>
    > - <span data-ttu-id="0a1e8-536">Val-Datenbereich</span><span class="sxs-lookup"><span data-stu-id="0a1e8-536">Data-val-range</span></span>
    > - <span data-ttu-id="0a1e8-537">Daten-Val-Range-min / Daten-Val-Range-max</span><span class="sxs-lookup"><span data-stu-id="0a1e8-537">Data-val-range-min / Data-val-range-max</span></span>
    > - <span data-ttu-id="0a1e8-538">Val erforderlichen Daten</span><span class="sxs-lookup"><span data-stu-id="0a1e8-538">Data-val-required</span></span>
    > - <span data-ttu-id="0a1e8-539">Val-Datenlänge</span><span class="sxs-lookup"><span data-stu-id="0a1e8-539">Data-val-length</span></span>
    > - <span data-ttu-id="0a1e8-540">Daten-Val-Länge-Max / Daten-Val-Länge-min</span><span class="sxs-lookup"><span data-stu-id="0a1e8-540">Data-val-length-max / Data-val-length-min</span></span>
    > 
    > <span data-ttu-id="0a1e8-541">Alle Datenwerte mit Modell gefüllt **-Datenanmerkung**.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-541">All the data values are filled with model **Data Annotation**.</span></span> <span data-ttu-id="0a1e8-542">Anschließend kann die gesamte Logik, die auf Serverseite arbeitet auf Clientseite ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-542">Then, all the logic that works at server side can be run at client side.</span></span> <span data-ttu-id="0a1e8-543">Price-Attribut hat beispielsweise die folgende datenanmerkung im Modell an:</span><span class="sxs-lookup"><span data-stu-id="0a1e8-543">For example, Price attribute has the following data annotation in the model:</span></span>
    > 
    > [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample24.cs)]
    > 
    > <span data-ttu-id="0a1e8-544">Ist nach der Verwendung unaufdringliche jQuery, der generierte Code ein:</span><span class="sxs-lookup"><span data-stu-id="0a1e8-544">After using Unobtrusive jQuery, the generated code is:</span></span>
    >  
    > [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample25.html)]

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="0a1e8-545">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="0a1e8-545">Summary</span></span>

<span data-ttu-id="0a1e8-546">Durch diese praktische Übungseinheit haben Sie gelernt so ändern Sie die in der Datenbank mit der Verwendung der folgenden gespeicherten Daten von Benutzern aktiviert:</span><span class="sxs-lookup"><span data-stu-id="0a1e8-546">By completing this Hands-On Lab you have learned how to enable users to change the data stored in the database with the use of the following:</span></span>

- <span data-ttu-id="0a1e8-547">Controlleraktionen wie Index, erstellen, bearbeiten, löschen</span><span class="sxs-lookup"><span data-stu-id="0a1e8-547">Controller actions like Index, Create, Edit, Delete</span></span>
- <span data-ttu-id="0a1e8-548">ASP.NETs Gerüstbau-Funktion zum Anzeigen von Eigenschaften in einer HTML-Tabelle</span><span class="sxs-lookup"><span data-stu-id="0a1e8-548">ASP.NET MVC's scaffolding feature for displaying properties in an HTML table</span></span>
- <span data-ttu-id="0a1e8-549">Auftreten von benutzerdefinierte HTML-Hilfsmethoden, um Benutzer zu verbessern.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-549">Custom HTML helpers to improve user experience</span></span>
- <span data-ttu-id="0a1e8-550">Aktionsmethoden, die auf HTTP-GET oder HTTP-POST-Aufrufe zu reagieren</span><span class="sxs-lookup"><span data-stu-id="0a1e8-550">Action methods that react to either HTTP-GET or HTTP-POST calls</span></span>
- <span data-ttu-id="0a1e8-551">Eine freigegebene Editor-Vorlage für ähnliche Ansichtsvorlagen wie erstellen und bearbeiten</span><span class="sxs-lookup"><span data-stu-id="0a1e8-551">A shared editor template for similar View templates like Create and Edit</span></span>
- <span data-ttu-id="0a1e8-552">Form-Elemente wie Dropdownlisten</span><span class="sxs-lookup"><span data-stu-id="0a1e8-552">Form elements like drop-downs</span></span>
- <span data-ttu-id="0a1e8-553">Datenanmerkungen zur modellvalidierung</span><span class="sxs-lookup"><span data-stu-id="0a1e8-553">Data annotations for Model validation</span></span>
- <span data-ttu-id="0a1e8-554">Clientseitige Validierung mit unaufdringliche jQuery-Bibliothek</span><span class="sxs-lookup"><span data-stu-id="0a1e8-554">Client Side Validation using jQuery Unobtrusive library</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="0a1e8-555">Anhang A: Installieren von Visual Studio Express 2012 für das Web</span><span class="sxs-lookup"><span data-stu-id="0a1e8-555">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="0a1e8-556">Sie installieren können **Microsoft Visual Studio Express 2012 für das Web** oder ein anderes &quot;Express&quot; Version mithilfe der  **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="0a1e8-556">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="0a1e8-557">Die folgenden Anweisungen führen Sie durch die erforderlichen Schritte zum Installieren *Visual Studio Express 2012 für das Web* mit *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-557">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="0a1e8-558">Wechseln Sie zu [ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="0a1e8-558">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="0a1e8-559">Auch wenn Sie bereits Webplattform-Installer installiert haben, können Sie öffnen es, und suchen Sie nach dem Produkt &quot; *Visual Studio Express 2012 für das Web mit Windows Azure SDK*&quot;.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-559">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*Visual Studio Express 2012 for Web with Windows Azure SDK*&quot;.</span></span>
2. <span data-ttu-id="0a1e8-560">Klicken Sie auf **jetzt installieren**.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-560">Click on **Install Now**.</span></span> <span data-ttu-id="0a1e8-561">Wenn Sie keine **Webplattform-Installer** Sie Informationen zum Herunterladen und installieren Sie diese zuerst umgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-561">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="0a1e8-562">Einmal **Webplattform-Installer** geöffnet ist, klicken Sie auf **installieren** um das Setup zu starten.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-562">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="0a1e8-563">![Visual Studio Express installieren](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "Visual Studio Express installieren")</span><span class="sxs-lookup"><span data-stu-id="0a1e8-563">![Install Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="0a1e8-564">*Installieren Sie Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="0a1e8-564">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="0a1e8-565">Lesen Sie die Produkte, Lizenzen und Begriffe, und klicken Sie auf **ich stimme** um den Vorgang fortzusetzen.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-565">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Akzeptieren der Lizenzbedingungen](aspnet-mvc-4-helpers-forms-and-validation/_static/image23.png)

    <span data-ttu-id="0a1e8-567">*Akzeptieren der Lizenzbedingungen*</span><span class="sxs-lookup"><span data-stu-id="0a1e8-567">*Accepting the license terms*</span></span>
5. <span data-ttu-id="0a1e8-568">Warten Sie, bis der Prozess herunterladen und die Installation abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-568">Wait until the downloading and installation process completes.</span></span>

    ![Installationsstatus](aspnet-mvc-4-helpers-forms-and-validation/_static/image24.png)

    <span data-ttu-id="0a1e8-570">*Installationsstatus*</span><span class="sxs-lookup"><span data-stu-id="0a1e8-570">*Installation progress*</span></span>
6. <span data-ttu-id="0a1e8-571">Nach Abschluss der Installation klicken Sie auf **Fertig stellen**.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-571">When the installation completes, click **Finish**.</span></span>

    ![Installation wurde abgeschlossen](aspnet-mvc-4-helpers-forms-and-validation/_static/image25.png)

    <span data-ttu-id="0a1e8-573">*Installation wurde abgeschlossen*</span><span class="sxs-lookup"><span data-stu-id="0a1e8-573">*Installation completed*</span></span>
7. <span data-ttu-id="0a1e8-574">Klicken Sie auf **beenden** Webplattform-Installer zu schließen.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-574">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="0a1e8-575">Um Visual Studio Express für Web zu öffnen, wechseln Sie zu der **starten** Startseite ein, und starten Sie das Schreiben von &quot; **Visual Studio Express**&quot;, klicken Sie dann auf die **Visual Studio Express für Web** Kachel.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-575">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Visual Studio Express für Web-Kachel](aspnet-mvc-4-helpers-forms-and-validation/_static/image26.png)

    <span data-ttu-id="0a1e8-577">*Visual Studio Express für Web-Kachel*</span><span class="sxs-lookup"><span data-stu-id="0a1e8-577">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="0a1e8-578">Anhang B: Verwendung von Codeausschnitten</span><span class="sxs-lookup"><span data-stu-id="0a1e8-578">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="0a1e8-579">Mit Codeausschnitten müssen Sie den Code, den Sie jederzeit griffbereit benötigen.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-579">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="0a1e8-580">Das Lab-Dokument erfahren Sie, wenn Sie genau, verwenden können wie in der folgenden Abbildung dargestellt.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-580">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="0a1e8-581">![Verwendung von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihr Projekt](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "Codeausschnitte zum Einfügen von Code in Ihr Projekt mit Visual Studio")</span><span class="sxs-lookup"><span data-stu-id="0a1e8-581">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="0a1e8-582">*Verwendung von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihr Projekt*</span><span class="sxs-lookup"><span data-stu-id="0a1e8-582">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="0a1e8-583">***Hinzufügen ein Codeausschnitts, die mithilfe der Tastatur (nur c#)***</span><span class="sxs-lookup"><span data-stu-id="0a1e8-583">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="0a1e8-584">Platzieren Sie den Cursor, wo möchten Sie den Code einfügen.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-584">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="0a1e8-585">Starten Sie den Namen des Ausschnitts (ohne Leerzeichen oder Bindestriche) eingeben.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-585">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="0a1e8-586">Beobachten Sie, wie IntelliSense zeigt übereinstimmenden Namen Ausschnitte.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-586">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="0a1e8-587">Wählen Sie den richtigen Ausschnitt (oder behalten Sie eingeben, bis der gesamte Ausschnitt Name ausgewählt ist).</span><span class="sxs-lookup"><span data-stu-id="0a1e8-587">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="0a1e8-588">Drücken Sie die Tab-Taste zweimal, um den Ausschnitt an der Cursorposition eingefügt.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-588">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="0a1e8-589">![Geben Sie den Namen des Ausschnitts](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "starten Sie den Namen des Ausschnitts eingeben")</span><span class="sxs-lookup"><span data-stu-id="0a1e8-589">![Start typing the snippet name](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "Start typing the snippet name")</span></span>

<span data-ttu-id="0a1e8-590">*Starten Sie den Namen des Ausschnitts eingeben*</span><span class="sxs-lookup"><span data-stu-id="0a1e8-590">*Start typing the snippet name*</span></span>

<span data-ttu-id="0a1e8-591">![Drücken Sie Tab, auf den hervorgehobenen Ausschnitt](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "drücken Sie die Tab hervorgehobenen Ausschnitt auswählen")</span><span class="sxs-lookup"><span data-stu-id="0a1e8-591">![Press Tab to select the highlighted snippet](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="0a1e8-592">*Drücken Sie Tab, um den markierten Ausschnitt auszuwählen*</span><span class="sxs-lookup"><span data-stu-id="0a1e8-592">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="0a1e8-593">![Drücken Sie erneut die Tab und den Ausschnitt erweitert](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "drücken Sie erneut die Tab und den Ausschnitt erweitert")</span><span class="sxs-lookup"><span data-stu-id="0a1e8-593">![Press Tab again and the snippet will expand](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="0a1e8-594">*Drücken Sie erneut die Tab und den Ausschnitt erweitert*</span><span class="sxs-lookup"><span data-stu-id="0a1e8-594">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="0a1e8-595">***Hinzufügen ein Codeausschnitts, die mit der Maus (c#, Visual Basic und XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-595">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="0a1e8-596">Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen möchten.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-596">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="0a1e8-597">Wählen Sie **Ausschnitt einfügen** gefolgt von **eigene Codeausschnitte**.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-597">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="0a1e8-598">Wählen Sie die relevanten Ausschnitt aus der Liste, indem Sie darauf klicken.</span><span class="sxs-lookup"><span data-stu-id="0a1e8-598">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="0a1e8-599">![Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten,")</span><span class="sxs-lookup"><span data-stu-id="0a1e8-599">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="0a1e8-600">*Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten*</span><span class="sxs-lookup"><span data-stu-id="0a1e8-600">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="0a1e8-601">![Wählen Sie den relevanten Ausschnitt aus der Liste, indem Sie darauf klicken](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "den relevanten Ausschnitt aus der Liste auswählen, indem Sie darauf klicken")</span><span class="sxs-lookup"><span data-stu-id="0a1e8-601">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="0a1e8-602">*Wählen Sie den relevanten Ausschnitt aus der Liste, indem Sie darauf klicken*</span><span class="sxs-lookup"><span data-stu-id="0a1e8-602">*Pick the relevant snippet from the list, by clicking on it*</span></span>
