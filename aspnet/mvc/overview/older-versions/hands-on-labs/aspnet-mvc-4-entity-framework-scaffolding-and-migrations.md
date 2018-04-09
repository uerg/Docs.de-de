---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
title: ASP.NET MVC 4 Entity Framework Gerüstbau und Migrationen | Microsoft Docs
author: rick-anderson
description: Wenn Sie mit ASP.NET MVC 4-Controllermethoden vertraut sind, oder ein abgeschlossener der &quot;-Hilfsprogrammen, Formulare und Validierung&quot; praktische Übungseinheit sollten beachtet werden...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 093c1362-f10b-407c-a708-be370f4b62b0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
msc.type: authoredcontent
ms.openlocfilehash: 548afe1926eed49841251832d54dc213da0cb753
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="aspnet-mvc-4-entity-framework-scaffolding-and-migrations"></a><span data-ttu-id="ac42f-103">ASP.NET MVC 4 Entity Framework Gerüstbau und Migrationen</span><span class="sxs-lookup"><span data-stu-id="ac42f-103">ASP.NET MVC 4 Entity Framework Scaffolding and Migrations</span></span>

<span data-ttu-id="ac42f-104">Durch [Web Lager Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="ac42f-104">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="ac42f-105">Herunterladen von Web-Lager Training Kit</span><span class="sxs-lookup"><span data-stu-id="ac42f-105">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="ac42f-106">Wenn Sie mit ASP.NET MVC 4-Controllermethoden vertraut sind, oder ein abgeschlossener der &quot;-Hilfsprogrammen, Formulare und Validierung&quot; praktische Übungseinheit, Sie sollten sich bewusst sein, dass der Großteil der Logik zum Erstellen, aktualisieren, auflisten und entfernen alle Datenentität, er wird wiederholt, zwischen der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="ac42f-106">If you are familiar with ASP.NET MVC 4 controller methods, or have completed the &quot;Helpers, Forms and Validation&quot; Hands-On lab, you should be aware that many of the logic to create, update, list and remove any data entity it is repeated among the application.</span></span> <span data-ttu-id="ac42f-107">Nicht, um, die verdeutlichen, hat Ihr Modell mehrere Klassen zum Bearbeiten, werden Sie wahrscheinlich eine sehr viel Zeit mit dem Schreiben von Aktionsmethoden POST- und GET für jeden Vorgang Entität sowie einzelnen Ansichten ausgeben.</span><span class="sxs-lookup"><span data-stu-id="ac42f-107">Not to mention that, if your model has several classes to manipulate, you will be likely to spend a considerable time writing the POST and GET action methods for each entity operation, as well as each of the views.</span></span>

<span data-ttu-id="ac42f-108">In dieser Übung erfahren Sie, wie Sie das Gerüst für ASP.NET MVC 4 verwenden, um die Baseline der Ihre Anwendung CRUD (Create, Read, Update und Delete) automatisch zu generieren.</span><span class="sxs-lookup"><span data-stu-id="ac42f-108">In this lab you will learn how to use the ASP.NET MVC 4 scaffolding to automatically generate the baseline of your application's CRUD (Create, Read, Update and Delete).</span></span> <span data-ttu-id="ac42f-109">Starten eine einfache Modellklasse, und ohne eine einzige Codezeile schreiben zu müssen, erstellen Sie einen Controller, der alle CRUD-Vorgänge sowie die alle erforderlichen Ansichten enthalten soll.</span><span class="sxs-lookup"><span data-stu-id="ac42f-109">Starting from a simple model class, and, without writing a single line of code, you will create a controller that will contain all the CRUD operations, as well as the all the necessary views.</span></span> <span data-ttu-id="ac42f-110">Nach dem Erstellen und Ausführen der einfache Lösung, müssen Sie die Anwendungsdatenbank generiert, zusammen mit der MVC-Logik und die Ansichten für die Bearbeitung von Daten.</span><span class="sxs-lookup"><span data-stu-id="ac42f-110">After building and running the simple solution, you will have the application database generated, together with the MVC logic and views for data manipulation.</span></span>

<span data-ttu-id="ac42f-111">Darüber hinaus erfahren Sie, wie einfach es ist mit Entity Framework Migrationen modellaktualisierungen in der gesamten Anwendung durchführen.</span><span class="sxs-lookup"><span data-stu-id="ac42f-111">In addition, you will learn how easy it is to use Entity Framework Migrations to perform model updates throughout your entire application.</span></span> <span data-ttu-id="ac42f-112">Migrationen von Entity Framework können Sie Ihre Datenbank zu ändern, nachdem das Modell mit einfachen Schritten geändert hat.</span><span class="sxs-lookup"><span data-stu-id="ac42f-112">Entity Framework Migrations will let you modify your database after the model has changed with simple steps.</span></span> <span data-ttu-id="ac42f-113">Mit allen diesen Denken Sie daran werden Sie möglicherweise zum Erstellen und Verwalten von Webanwendungen effizienter nutzen die neuesten Funktionen von ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="ac42f-113">With all these in mind, you will be able to build and maintain web applications more efficiently, taking advantage of the latest features of ASP.NET MVC 4.</span></span>

> [!NOTE]
> <span data-ttu-id="ac42f-114">Alle Beispielcode und Codeausschnitte sind im Web Lager Training Kit zur Nächten enthalten [Versionen von Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="ac42f-114">All sample code and snippets are included in the Web Camps Training Kit, available from at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="ac42f-115">Das Projekt, das speziell für diese Übung finden Sie unter [Migrationen und ASP.NET MVC 4 Entity Framework Gerüstbau](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).</span><span class="sxs-lookup"><span data-stu-id="ac42f-115">The project specific to this lab is available at [ASP.NET MVC 4 Entity Framework Scaffolding and Migrations](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="ac42f-116">Ziele</span><span class="sxs-lookup"><span data-stu-id="ac42f-116">Objectives</span></span>

<span data-ttu-id="ac42f-117">In dieser praktischen Übungseinheit erfahren Sie, wie Sie:</span><span class="sxs-lookup"><span data-stu-id="ac42f-117">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="ac42f-118">Verwenden Sie ASP.NET Gerüstbau für CRUD-Vorgänge im Controller.</span><span class="sxs-lookup"><span data-stu-id="ac42f-118">Use ASP.NET scaffolding for CRUD operations in controllers.</span></span>
- <span data-ttu-id="ac42f-119">Das Datenbankmodell mithilfe von Entity Framework-Migrationen zu ändern.</span><span class="sxs-lookup"><span data-stu-id="ac42f-119">Change the database model using Entity Framework Migrations.</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="ac42f-120">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="ac42f-120">Prerequisites</span></span>

<span data-ttu-id="ac42f-121">Sie benötigen zum Abschließen dieser testumgebung die folgenden Elemente:</span><span class="sxs-lookup"><span data-stu-id="ac42f-121">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="ac42f-122">[Microsoft Visual Studio Express 2012 für das Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) oder sogar eine höhere (Lesen [Anhang A](#AppendixA) Anleitungen zur Installation).</span><span class="sxs-lookup"><span data-stu-id="ac42f-122">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="ac42f-123">Setup</span><span class="sxs-lookup"><span data-stu-id="ac42f-123">Setup</span></span>

<span data-ttu-id="ac42f-124">**Installieren von Codeausschnitten**</span><span class="sxs-lookup"><span data-stu-id="ac42f-124">**Installing Code Snippets**</span></span>

<span data-ttu-id="ac42f-125">Der Einfachheit halber gilt Großteil des Codes, den Sie entlang dieser Übung verwalten als Visual Studio-Codeausschnitte verfügbar.</span><span class="sxs-lookup"><span data-stu-id="ac42f-125">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="ac42f-126">So installieren Sie die Codeausschnitte ausführen **.\Source\Setup\CodeSnippets.vsi** Datei.</span><span class="sxs-lookup"><span data-stu-id="ac42f-126">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="ac42f-127">Wenn Sie nicht mit den Visual Studio-Codeausschnitte und zu erfahren, wie deren Verwendung vertraut sind, Sie können finden Sie im Anhang in diesem Dokument &quot; [Anhang B: mithilfe von Code Snippets](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="ac42f-127">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="ac42f-128">Übungen</span><span class="sxs-lookup"><span data-stu-id="ac42f-128">Exercises</span></span>

<span data-ttu-id="ac42f-129">In der folgenden Übung bilden diese praktische Übungseinheit:</span><span class="sxs-lookup"><span data-stu-id="ac42f-129">The following exercise make up this Hands-On Lab:</span></span>

1. [<span data-ttu-id="ac42f-130">Mithilfe von ASP.NET MVC 4-Gerüstbau mit Entity Framework-Migrationen</span><span class="sxs-lookup"><span data-stu-id="ac42f-130">Using ASP.NET MVC 4 Scaffolding with Entity Framework Migrations</span></span>](#Exercise1)

> [!NOTE]
> <span data-ttu-id="ac42f-131">In dieser Übung beiliegen ein **End** Ordner, der sich so ergebende Lösung, die Sie nach Abschluss der Übung abrufen soll.</span><span class="sxs-lookup"><span data-stu-id="ac42f-131">This exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercise.</span></span> <span data-ttu-id="ac42f-132">Sie können diese Lösung als Leitfaden verwenden, wenn Sie zusätzliche Hilfe bei der Arbeit in der Übung benötigen.</span><span class="sxs-lookup"><span data-stu-id="ac42f-132">You can use this solution as a guide if you need additional help working through the exercise.</span></span>


<span data-ttu-id="ac42f-133">Geschätzte Zeit zum Abschließen dieser testumgebung: **30 Minuten**</span><span class="sxs-lookup"><span data-stu-id="ac42f-133">Estimated time to complete this lab: **30 minutes**</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Using_ASPNET_MVC_4_Scaffolding_with_Entity_Framework_Migrations"></a>
### <a name="exercise-1-using-aspnet-mvc-4-scaffolding-with-entity-framework-migrations"></a><span data-ttu-id="ac42f-134">Übung 1: Verwenden von ASP.NET MVC 4-Gerüstbau mit Entity Framework-Migrationen</span><span class="sxs-lookup"><span data-stu-id="ac42f-134">Exercise 1: Using ASP.NET MVC 4 Scaffolding with Entity Framework Migrations</span></span>

<span data-ttu-id="ac42f-135">ASP.NET MVC-Gerüstbau bietet eine schnelle Möglichkeit zum Generieren von CRUD-Vorgänge in eine standardisierte Möglichkeit, erstellen die erforderliche Logik, die Ihre Anwendung mit der Datenbankebene interagieren kann.</span><span class="sxs-lookup"><span data-stu-id="ac42f-135">ASP.NET MVC scaffolding provides a quick way to generate the CRUD operations in a standardized way, creating the necessary logic that lets your application interact with the database layer.</span></span>

<span data-ttu-id="ac42f-136">In dieser Übung erfahren Sie, wie mit ASP.NET MVC 4-Gerüstbau mit Code zuerst die CRUD-Methoden erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="ac42f-136">In this exercise, you will learn how to use ASP.NET MVC 4 scaffolding with code first to create the CRUD methods.</span></span> <span data-ttu-id="ac42f-137">Anschließend erfahren Sie, wie das Modell anwenden der Änderungen in der Datenbank mithilfe von Entity Framework Migrationen aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="ac42f-137">Then, you will learn how to update your model applying the changes in the database by using Entity Framework Migrations.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1-_Creating_a_new_ASPNET_MVC_4_project_using_Scaffolding"></a>
#### <a name="task-1--creating-a-new-aspnet-mvc-4-project-using-scaffolding"></a><span data-ttu-id="ac42f-138">Aufgabe 1 – Erstellen einer neuen ASP.NET MVC 4-Projekt mithilfe von Gerüstbau</span><span class="sxs-lookup"><span data-stu-id="ac42f-138">Task 1- Creating a new ASP.NET MVC 4 project using Scaffolding</span></span>

1. <span data-ttu-id="ac42f-139">Wenn nicht bereits geöffnet ist, starten Sie **Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="ac42f-139">If not already open, start **Visual Studio 2012**.</span></span>
2. <span data-ttu-id="ac42f-140">Wählen Sie **Datei | Neues Projekt**.</span><span class="sxs-lookup"><span data-stu-id="ac42f-140">Select **File | New Project**.</span></span> <span data-ttu-id="ac42f-141">In das neue Projekt im Dialogfeld unter die **Visual C# | Web** Abschnitt **ASP.NET MVC 4-Webanwendung**.</span><span class="sxs-lookup"><span data-stu-id="ac42f-141">In the New Project dialog, under the **Visual C# | Web** section, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="ac42f-142">Nennen Sie das Projekt zu **MVC4andEFMigrations** und legen Sie den Speicherort **Source\Ex1 UsingMVC4ScaffoldingEFMigrations** Ordner dieser Anleitung.</span><span class="sxs-lookup"><span data-stu-id="ac42f-142">Name the project to **MVC4andEFMigrations** and set the location to **Source\Ex1-UsingMVC4ScaffoldingEFMigrations** folder of this lab.</span></span> <span data-ttu-id="ac42f-143">Legen Sie die **Projektmappenname** auf **beginnen** und stellen Sie sicher **Projektmappenverzeichnis erstellen** aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="ac42f-143">Set the **Solution name** to **Begin** and ensure **Create directory for solution** is checked.</span></span> <span data-ttu-id="ac42f-144">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="ac42f-144">Click **OK**.</span></span>

    <span data-ttu-id="ac42f-145">![Neues ASP.NET MVC 4-Projekt (Dialogfeld)](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "neue ASP.NET MVC 4-Projekt (Dialogfeld)")</span><span class="sxs-lookup"><span data-stu-id="ac42f-145">![New ASP.NET MVC 4 Project Dialog Box](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "New ASP.NET MVC 4 Project Dialog Box")</span></span>

    <span data-ttu-id="ac42f-146">*Neues ASP.NET MVC 4-Projekt (Dialogfeld)*</span><span class="sxs-lookup"><span data-stu-id="ac42f-146">*New ASP.NET MVC 4 Project Dialog Box*</span></span>
3. <span data-ttu-id="ac42f-147">In der **neues ASP.NET MVC 4-Projekt** aktivieren Sie im Dialogfeld die **Internetanwendung** Vorlage, und stellen Sie sicher, dass **Razor** wird dem ausgewählten **Ansichtsmodul**.</span><span class="sxs-lookup"><span data-stu-id="ac42f-147">In the **New ASP.NET MVC 4 Project** dialog box select the **Internet Application** template, and make sure that **Razor** is the selected **View engine**.</span></span> <span data-ttu-id="ac42f-148">Klicken Sie auf **OK**, um das Projekt zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="ac42f-148">Click **OK** to create the project.</span></span>

    <span data-ttu-id="ac42f-149">![Neues ASP.NET MVC 4-Internetanwendung](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "neue ASP.NET MVC 4-Internetanwendung")</span><span class="sxs-lookup"><span data-stu-id="ac42f-149">![New ASP.NET MVC 4 Internet Application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "New ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="ac42f-150">*Neues ASP.NET MVC 4-Internetanwendung*</span><span class="sxs-lookup"><span data-stu-id="ac42f-150">*New ASP.NET MVC 4 Internet Application*</span></span>
4. <span data-ttu-id="ac42f-151">Klicken Sie im Projektmappen-Explorer mit der Maustaste **Modelle** , und wählen Sie **hinzufügen | Klasse** zum Erstellen einer einfachen Klasse Person (POCO).</span><span class="sxs-lookup"><span data-stu-id="ac42f-151">In the Solution Explorer, right-click **Models** and select **Add | Class** to create a simple class person (POCO).</span></span> <span data-ttu-id="ac42f-152">Nennen Sie sie **Person** , und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="ac42f-152">Name it **Person** and click **OK**.</span></span>
5. <span data-ttu-id="ac42f-153">Öffnen Sie die Person-Klasse, und fügen Sie die folgenden Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="ac42f-153">Open the Person class and insert the following properties.</span></span>

    <span data-ttu-id="ac42f-154">(Codeausschnitt - *ASP.NET MVC 4 und Entity Framework Migrationen - Ex1 Person Eigenschaften*)</span><span class="sxs-lookup"><span data-stu-id="ac42f-154">(Code Snippet - *ASP.NET MVC 4 and Entity Framework Migrations - Ex1 Person Properties*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample1.cs)]
~~~
6. <span data-ttu-id="ac42f-155">Klicken Sie auf **erstellen | Projektmappe** zum Speichern der Änderungen, und erstellen Sie das Projekt.</span><span class="sxs-lookup"><span data-stu-id="ac42f-155">Click **Build | Build Solution** to save the changes and build the project.</span></span>

    <span data-ttu-id="ac42f-156">![Erstellen der Anwendung](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "Erstellen der Anwendung")</span><span class="sxs-lookup"><span data-stu-id="ac42f-156">![Building the application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "Building the application")</span></span>

    <span data-ttu-id="ac42f-157">*Erstellen der Anwendung*</span><span class="sxs-lookup"><span data-stu-id="ac42f-157">*Building the Application*</span></span>
7. <span data-ttu-id="ac42f-158">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste in des Ordners Controller, und wählen **hinzufügen | Controller**.</span><span class="sxs-lookup"><span data-stu-id="ac42f-158">In the Solution Explorer, right-click the controllers folder and select **Add | Controller**.</span></span>
8. <span data-ttu-id="ac42f-159">Nennen Sie den Controller *PersonController* und schließen Sie die **gerüstoptionen** mit den folgenden Werten.</span><span class="sxs-lookup"><span data-stu-id="ac42f-159">Name the controller *PersonController* and complete the **Scaffolding options** with the following values.</span></span>

   1. <span data-ttu-id="ac42f-160">In der **Vorlage** Dropdown-Liste der **MVC-Controller mit Lese-/schreibaktionen und Ansichten unter Verwendung von Entity Framework** Option.</span><span class="sxs-lookup"><span data-stu-id="ac42f-160">In the **Template** drop-down list, select the **MVC controller with read/write actions and views, using Entity Framework** option.</span></span>
   2. <span data-ttu-id="ac42f-161">In der **Modellschemas** Dropdown-Liste der **Person** Klasse.</span><span class="sxs-lookup"><span data-stu-id="ac42f-161">In the **Model class** drop-down list, select the **Person** class.</span></span>
   3. <span data-ttu-id="ac42f-162">In der **Datenkontext Klasse** Liste  **&lt;neuen Datenkontext... &gt;**.</span><span class="sxs-lookup"><span data-stu-id="ac42f-162">In the **Data Context class** list, select **&lt;New data context...&gt;**.</span></span> <span data-ttu-id="ac42f-163">Wählen Sie einen beliebigen Namen, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="ac42f-163">Choose any name and click **OK**.</span></span>
   4. <span data-ttu-id="ac42f-164">In der **Ansichten** Dropdown-Liste, stellen Sie sicher, dass **Razor** ausgewählt ist.</span><span class="sxs-lookup"><span data-stu-id="ac42f-164">In the **Views** drop-down list, make sure that **Razor** is selected.</span></span>

      <span data-ttu-id="ac42f-165">![Die Person-Controller mit Gerüst hinzufügen](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "den Person-Controller mit Gerüst hinzufügen")</span><span class="sxs-lookup"><span data-stu-id="ac42f-165">![Adding the Person controller with scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "Adding the Person controller with scaffolding")</span></span>

      <span data-ttu-id="ac42f-166">*Die Person-Controller mit Gerüst hinzufügen*</span><span class="sxs-lookup"><span data-stu-id="ac42f-166">*Adding the Person controller with scaffolding*</span></span>
9. <span data-ttu-id="ac42f-167">Klicken Sie auf **hinzufügen** So erstellen Sie den neuen Controller für Personen mit Gerüstbau.</span><span class="sxs-lookup"><span data-stu-id="ac42f-167">Click **Add** to create the new controller for Person with scaffolding.</span></span> <span data-ttu-id="ac42f-168">Sie haben jetzt die Controlleraktionen sowie die Ansichten erstellt.</span><span class="sxs-lookup"><span data-stu-id="ac42f-168">You have now generated the controller actions as well as the views.</span></span>

    <span data-ttu-id="ac42f-169">![Nach dem Erstellen des Person-Controllers mit Gerüstbau](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "nach dem Erstellen des Person-Controllers mit Gerüstbau")</span><span class="sxs-lookup"><span data-stu-id="ac42f-169">![After creating the Person controller with scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "After creating the Person controller with scaffolding")</span></span>

    <span data-ttu-id="ac42f-170">*Nach dem Erstellen des Person-Controllers mit Gerüstbau*</span><span class="sxs-lookup"><span data-stu-id="ac42f-170">*After creating the Person controller with scaffolding*</span></span>
10. <span data-ttu-id="ac42f-171">Open **PersonController** Klasse.</span><span class="sxs-lookup"><span data-stu-id="ac42f-171">Open **PersonController** class.</span></span> <span data-ttu-id="ac42f-172">Beachten Sie, dass die vollständige CRUD-Aktionsmethoden automatisch generiert wurden.</span><span class="sxs-lookup"><span data-stu-id="ac42f-172">Notice that the full CRUD action methods have been generated automatically.</span></span>

   <span data-ttu-id="ac42f-173">![In den Person-Controller](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "innerhalb der Person-Controller")</span><span class="sxs-lookup"><span data-stu-id="ac42f-173">![Inside the Person controller](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "Inside the Person controller")</span></span>

   <span data-ttu-id="ac42f-174">*In den Person-controller*</span><span class="sxs-lookup"><span data-stu-id="ac42f-174">*Inside the Person controller*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2-_Running_the_application"></a>
#### <a name="task-2--running-the-application"></a><span data-ttu-id="ac42f-175">Aufgabe 2 ausgeführte Anwendung</span><span class="sxs-lookup"><span data-stu-id="ac42f-175">Task 2- Running the application</span></span>

<span data-ttu-id="ac42f-176">An diesem Punkt ist die Datenbank noch nicht erstellt.</span><span class="sxs-lookup"><span data-stu-id="ac42f-176">At this point, the database is not yet created.</span></span> <span data-ttu-id="ac42f-177">In dieser Aufgabe werden Sie die Anwendung zum ersten Mal ausführen und Testen von CRUD-Vorgänge.</span><span class="sxs-lookup"><span data-stu-id="ac42f-177">In this task, you will run the application for the first time and test the CRUD operations.</span></span> <span data-ttu-id="ac42f-178">Die Datenbank wird im Handumdrehen mit Code First erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="ac42f-178">The database will be created on the fly with Code First.</span></span>

1. <span data-ttu-id="ac42f-179">Drücken Sie **F5**, um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="ac42f-179">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="ac42f-180">Fügen Sie in den Browser **/Person** an die URL der Seite "Person" zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="ac42f-180">In the browser, add **/Person** to the URL to open the Person page.</span></span>

    <span data-ttu-id="ac42f-181">![Anwendung – erste Ausführung](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "Anwendung – erste Ausführung")</span><span class="sxs-lookup"><span data-stu-id="ac42f-181">![Application first run](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "Application first run")</span></span>

    <span data-ttu-id="ac42f-182">*Die Anwendung: Führen Sie zuerst*</span><span class="sxs-lookup"><span data-stu-id="ac42f-182">*Application: first run*</span></span>
3. <span data-ttu-id="ac42f-183">Sie werden jetzt Untersuchen der Person-Seiten und die CRUD-Vorgänge zu testen.</span><span class="sxs-lookup"><span data-stu-id="ac42f-183">You will now explore the Person pages and test the CRUD operations.</span></span>

    1. <span data-ttu-id="ac42f-184">Klicken Sie auf **neu erstellen** eine neue Person hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="ac42f-184">Click **Create New** to add a new person.</span></span> <span data-ttu-id="ac42f-185">Geben Sie einen Vornamen und einen Nachnamen enthalten, und klicken Sie auf **erstellen**.</span><span class="sxs-lookup"><span data-stu-id="ac42f-185">Enter a first name and a last name and click **Create**.</span></span>

        <span data-ttu-id="ac42f-186">![Eine neue Person hinzufügen](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "eine neue Person hinzufügen")</span><span class="sxs-lookup"><span data-stu-id="ac42f-186">![Adding a new person](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "Adding a new person")</span></span>

        <span data-ttu-id="ac42f-187">*Eine neue Person hinzufügen*</span><span class="sxs-lookup"><span data-stu-id="ac42f-187">*Adding a new person*</span></span>
    2. <span data-ttu-id="ac42f-188">In der Liste der Person Sie löschen, bearbeiten oder Hinzufügen von Elementen.</span><span class="sxs-lookup"><span data-stu-id="ac42f-188">In the person's list, you can delete, edit or add items.</span></span>

        <span data-ttu-id="ac42f-189">![Personenliste](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "Personenliste")</span><span class="sxs-lookup"><span data-stu-id="ac42f-189">![person list](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "person list")</span></span>

        <span data-ttu-id="ac42f-190">*Personenliste*</span><span class="sxs-lookup"><span data-stu-id="ac42f-190">*Person list*</span></span>
    3. <span data-ttu-id="ac42f-191">Klicken Sie auf **Details** zu der Person Details zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="ac42f-191">Click **Details** to open the person's details.</span></span>

        <span data-ttu-id="ac42f-192">![Details der Person](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "Person-Details")</span><span class="sxs-lookup"><span data-stu-id="ac42f-192">![Person's details](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "Person's details")</span></span>

        <span data-ttu-id="ac42f-193">*Der Person-details*</span><span class="sxs-lookup"><span data-stu-id="ac42f-193">*Person's details*</span></span>
4. <span data-ttu-id="ac42f-194">Schließen Sie den Browser, und kehren Sie zu Visual Studio zurück.</span><span class="sxs-lookup"><span data-stu-id="ac42f-194">Close the browser and return to Visual Studio.</span></span> <span data-ttu-id="ac42f-195">Beachten Sie, dass Sie die gesamte CRUD-Vorgänge für die Personenentität in der gesamten - vom Modell zu den Ansichten - Anwendung erstellt haben, ohne eine einzige Codezeile schreiben zu müssen.</span><span class="sxs-lookup"><span data-stu-id="ac42f-195">Notice that you have created the whole CRUD for the person entity throughout your application -from the model to the views- without having to write a single line of code!</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3-_Updating_the_database_using_Entity_Framework_Migrations"></a>
#### <a name="task-3--updating-the-database-using-entity-framework-migrations"></a><span data-ttu-id="ac42f-196">Aufgabe 3-Aktualisierung der Datenbank mithilfe von Entity Framework-Migrationen</span><span class="sxs-lookup"><span data-stu-id="ac42f-196">Task 3- Updating the database using Entity Framework Migrations</span></span>

<span data-ttu-id="ac42f-197">In dieser Aufgabe aktualisieren Sie die Datenbank mithilfe von Entity Framework Migrationen.</span><span class="sxs-lookup"><span data-stu-id="ac42f-197">In this task you will update the database using Entity Framework Migrations.</span></span> <span data-ttu-id="ac42f-198">Sie werden feststellen, wie einfach es ist, ändern Sie das Modell und Änderungen in den Datenbanken mithilfe der Entity Framework Migrationen-Funktion.</span><span class="sxs-lookup"><span data-stu-id="ac42f-198">You will discover how easy it is to change the model and reflect the changes in your databases by using the Entity Framework Migrations feature.</span></span>

1. <span data-ttu-id="ac42f-199">Öffnen Sie die Paket-Manager-Konsole.</span><span class="sxs-lookup"><span data-stu-id="ac42f-199">Open the Package Manager Console.</span></span> <span data-ttu-id="ac42f-200">Wählen Sie **Tools | Bibliothek-Paket-Manager | Paket-Manager-Konsole**.</span><span class="sxs-lookup"><span data-stu-id="ac42f-200">Select **Tools | Library Package Manager | Package Manager Console**.</span></span>
2. <span data-ttu-id="ac42f-201">Geben Sie in der Paket-Manager-Konsole den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="ac42f-201">In the Package Manager Console, enter the following command:</span></span>

    <span data-ttu-id="ac42f-202">PMC</span><span class="sxs-lookup"><span data-stu-id="ac42f-202">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample2.ps1)]

    <span data-ttu-id="ac42f-203">![Aktivieren von Migrationen](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "Migrationen aktivieren")</span><span class="sxs-lookup"><span data-stu-id="ac42f-203">![Enabling Migrations](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "Enabling Migrations")</span></span>

    <span data-ttu-id="ac42f-204">*Aktivieren von Migrationen*</span><span class="sxs-lookup"><span data-stu-id="ac42f-204">*Enabling migrations*</span></span>

    <span data-ttu-id="ac42f-205">Der Enable-Migrations-Befehl erstellt die **Migrationen** Ordner, in dem ein Skript zum Initialisieren der Datenbank enthalten.</span><span class="sxs-lookup"><span data-stu-id="ac42f-205">The Enable-Migration command creates the **Migrations** folder, which contains a script to initialize the database.</span></span>

    <span data-ttu-id="ac42f-206">![Ordner](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "Ordner")</span><span class="sxs-lookup"><span data-stu-id="ac42f-206">![Migrations folder](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "Migrations folder")</span></span>

    <span data-ttu-id="ac42f-207">*Ordner*</span><span class="sxs-lookup"><span data-stu-id="ac42f-207">*Migrations folder*</span></span>
3. <span data-ttu-id="ac42f-208">Öffnen der **"Configuration.cs"** -Datei in den Ordner.</span><span class="sxs-lookup"><span data-stu-id="ac42f-208">Open the **Configuration.cs** file in the Migrations folder.</span></span> <span data-ttu-id="ac42f-209">Suchen Sie den Klassenkonstruktor, und Ändern der **AutomaticMigrationsEnabled** Wert *"true"*.</span><span class="sxs-lookup"><span data-stu-id="ac42f-209">Locate the class constructor and change the **AutomaticMigrationsEnabled** value to *true*.</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample3.cs)]
~~~
4. <span data-ttu-id="ac42f-210">Öffnen Sie die Person-Klasse, und fügen Sie ein Attribut für den Vornamen der Person.</span><span class="sxs-lookup"><span data-stu-id="ac42f-210">Open the Person class and add an attribute for the person's middle name.</span></span> <span data-ttu-id="ac42f-211">Mit diesem neuen Attribut ändern Sie das Modell.</span><span class="sxs-lookup"><span data-stu-id="ac42f-211">With this new attribute, you are changing the model.</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample4.cs)]
~~~
5. <span data-ttu-id="ac42f-212">Wählen Sie **erstellen | Projektmappe** auf das Menü zum Erstellen der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="ac42f-212">Select **Build | Build Solution** on the menu to build the application.</span></span>

    <span data-ttu-id="ac42f-213">![Erstellen der Anwendung](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "Erstellen der Anwendung")</span><span class="sxs-lookup"><span data-stu-id="ac42f-213">![Building the application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "Building the application")</span></span>

    <span data-ttu-id="ac42f-214">*Erstellen der Anwendung*</span><span class="sxs-lookup"><span data-stu-id="ac42f-214">*Building the application*</span></span>
6. <span data-ttu-id="ac42f-215">Geben Sie in der Paket-Manager-Konsole den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="ac42f-215">In the Package Manager Console, enter the following command:</span></span>

    <span data-ttu-id="ac42f-216">PMC</span><span class="sxs-lookup"><span data-stu-id="ac42f-216">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample5.ps1)]

    <span data-ttu-id="ac42f-217">Mit diesem Befehl wird für Änderungen in den Datenobjekten suchen, und es wird fügen Sie die erforderlichen Befehle aus, um die Datenbank entsprechend ändern.</span><span class="sxs-lookup"><span data-stu-id="ac42f-217">This command will look for changes in the data objects, and then, it will add the necessary commands to modify the database accordingly.</span></span>

    <span data-ttu-id="ac42f-218">![Hinzufügen einer zweiten Vornamens](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "ein zweiter Vorname hinzufügen")</span><span class="sxs-lookup"><span data-stu-id="ac42f-218">![Adding a middle name](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "Adding a middle name")</span></span>

    <span data-ttu-id="ac42f-219">*Ein zweiter Vorname hinzufügen*</span><span class="sxs-lookup"><span data-stu-id="ac42f-219">*Adding a middle name*</span></span>
7. <span data-ttu-id="ac42f-220">(Optional) Sie können den folgenden Befehl zum Generieren eines SQL-Skripts, mit dem differenzielle Update ausführen.</span><span class="sxs-lookup"><span data-stu-id="ac42f-220">(Optional) You can run the following command to generate a SQL script with the differential update.</span></span> <span data-ttu-id="ac42f-221">Auf diese Weise können Sie die manuellen Aktualisieren der Datenbank (In diesem Fall ist es nicht erforderlich), oder übernehmen Sie die Änderungen in anderen Datenbanken:</span><span class="sxs-lookup"><span data-stu-id="ac42f-221">This will let you update the database manually (In this case it's not necessary), or apply the changes in other databases:</span></span>

    <span data-ttu-id="ac42f-222">PMC</span><span class="sxs-lookup"><span data-stu-id="ac42f-222">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample6.ps1)]

    <span data-ttu-id="ac42f-223">![Generieren eines SQL-Skripts](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "Generieren eines SQL-Skripts")</span><span class="sxs-lookup"><span data-stu-id="ac42f-223">![Generating a SQL script](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "Generating a SQL script")</span></span>

    <span data-ttu-id="ac42f-224">*Generieren eines SQL-Skripts*</span><span class="sxs-lookup"><span data-stu-id="ac42f-224">*Generating a SQL script*</span></span>

    <span data-ttu-id="ac42f-225">![SQL-Skript Update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "Update SQL-Skript")</span><span class="sxs-lookup"><span data-stu-id="ac42f-225">![SQL Script update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "SQL Script update")</span></span>

    <span data-ttu-id="ac42f-226">*Update für SQL-Skript*</span><span class="sxs-lookup"><span data-stu-id="ac42f-226">*SQL Script update*</span></span>
8. <span data-ttu-id="ac42f-227">Geben Sie den folgenden Befehl zum Aktualisieren der Datenbank, in der Paket-Manager-Konsole:</span><span class="sxs-lookup"><span data-stu-id="ac42f-227">In the Package Manager Console, enter the following command to update the database:</span></span>

    <span data-ttu-id="ac42f-228">PMC</span><span class="sxs-lookup"><span data-stu-id="ac42f-228">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample7.ps1)]

    <span data-ttu-id="ac42f-229">![Aktualisieren der Datenbank](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "Aktualisieren der Datenbank")</span><span class="sxs-lookup"><span data-stu-id="ac42f-229">![Updating the Database](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "Updating the Database")</span></span>

    <span data-ttu-id="ac42f-230">*Aktualisieren der Datenbank*</span><span class="sxs-lookup"><span data-stu-id="ac42f-230">*Updating the Database*</span></span>

    <span data-ttu-id="ac42f-231">Dadurch wird hinzugefügt der **MiddleName** Spalte in der **Personen** Tabelle entsprechend die aktuelle Definition des der **Person** Klasse.</span><span class="sxs-lookup"><span data-stu-id="ac42f-231">This will add the **MiddleName** column in the **People** table to match the current definition of the **Person** class.</span></span>
9. <span data-ttu-id="ac42f-232">Sobald die Datenbank aktualisiert wird, mit der rechten Maustaste in des Ordners Controller, und wählen Sie **hinzufügen | Controller** den Person Controller erneut (vollständig mit den gleichen Werten) hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="ac42f-232">Once the database is updated, right-click the Controller folder and select **Add | Controller** to add the Person controller again (Complete with the same values).</span></span> <span data-ttu-id="ac42f-233">Hiermit wird die vorhandene Methoden und Ansichten, die das neue Attribut hinzufügen aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="ac42f-233">This will update the existing methods and views adding the new attribute.</span></span>

    <span data-ttu-id="ac42f-234">![Hinzufügen eines Controllers Updates](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "ein controllerupdate hinzufügen")</span><span class="sxs-lookup"><span data-stu-id="ac42f-234">![Adding a controller update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "Adding a controller update")</span></span>

    <span data-ttu-id="ac42f-235">*Update des Controllers*</span><span class="sxs-lookup"><span data-stu-id="ac42f-235">*Updating the controller*</span></span>
10. <span data-ttu-id="ac42f-236">Klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="ac42f-236">Click **Add**.</span></span> <span data-ttu-id="ac42f-237">Wählen Sie dann die Werte **überschreiben PersonController.cs** und **überschreiben zugeordnete Ansichten** , und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="ac42f-237">Then, select the values **Overwrite PersonController.cs** and the **Overwrite associated views** and click **OK**.</span></span>

   ![Hinzufügen von einem Domänencontroller überschreiben](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image19.png)

   <span data-ttu-id="ac42f-239">*Update des Controllers*</span><span class="sxs-lookup"><span data-stu-id="ac42f-239">*Updating the controller*</span></span>

<a id="Ex1Task4"></a>

<a id="Task4-_Running_the_application"></a>
#### <a name="task4--running-the-application"></a><span data-ttu-id="ac42f-240">Task4 - Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="ac42f-240">Task4- Running the application</span></span>

1. <span data-ttu-id="ac42f-241">Drücken Sie **F5**, um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="ac42f-241">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="ac42f-242">Open **/Person**.</span><span class="sxs-lookup"><span data-stu-id="ac42f-242">Open **/Person**.</span></span> <span data-ttu-id="ac42f-243">Beachten Sie, dass die Daten beibehalten wurde, während die Vornamen-Spalte hinzugefügt wurde, ein.</span><span class="sxs-lookup"><span data-stu-id="ac42f-243">Notice that the data was preserved, while the middle name column was added.</span></span>

    <span data-ttu-id="ac42f-244">![Zweiter Vorname hinzugefügt](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "Vornamen hinzugefügt")</span><span class="sxs-lookup"><span data-stu-id="ac42f-244">![Middle Name added](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "Middle Name added")</span></span>

    <span data-ttu-id="ac42f-245">*Zweiter Vorname hinzugefügt*</span><span class="sxs-lookup"><span data-stu-id="ac42f-245">*Middle Name added*</span></span>
3. <span data-ttu-id="ac42f-246">Wenn Sie auf **bearbeiten**, Sie werden möglicherweise ein zweiter Vorname an die aktuelle Person hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="ac42f-246">If you click **Edit**, you will be able to add a middle name to the current person.</span></span>

    <span data-ttu-id="ac42f-247">![Zweiter Vorname Edition](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "Vornamen Edition")</span><span class="sxs-lookup"><span data-stu-id="ac42f-247">![Middle Name edition](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "Middle Name edition")</span></span>

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="ac42f-248">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="ac42f-248">Summary</span></span>

<span data-ttu-id="ac42f-249">In dieser praktischen Übungseinheit haben Sie einfache Schritte zum Erstellen von CRUD-Vorgänge mit ASP.NET MVC 4-Gerüstbau über jede beliebige Modellklasse gelernt.</span><span class="sxs-lookup"><span data-stu-id="ac42f-249">In this Hands-On lab, you have learned simple steps to create CRUD operations with ASP.NET MVC 4 Scaffolding using any model class.</span></span> <span data-ttu-id="ac42f-250">Anschließend haben Sie gelernt ein End-to-End-Update in der Anwendung – aus der Datenbank, die den Ansichten – mit Entity Framework-Migrationen ausführen.</span><span class="sxs-lookup"><span data-stu-id="ac42f-250">Then, you have learned how to perform an end to end update in your application -from the database to the views- by using Entity Framework Migrations.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="ac42f-251">Anhang A: Installieren von Visual Studio Express 2012 für das Web</span><span class="sxs-lookup"><span data-stu-id="ac42f-251">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="ac42f-252">Sie installieren können **Microsoft Visual Studio Express 2012 für das Web** oder ein anderes &quot;Express&quot; Version mithilfe der **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="ac42f-252">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="ac42f-253">Die folgenden Anweisungen führen Sie durch die erforderlichen Schritte zum Installieren *Visual Studio Express 2012 für das Web* mit *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="ac42f-253">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="ac42f-254">Wechseln Sie zu [ [ https://go.microsoft.com/? Linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="ac42f-254">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="ac42f-255">Auch wenn Sie bereits Webplattform-Installer installiert haben, können Sie öffnen es, und suchen Sie nach dem Produkt &quot; <em>Visual Studio Express 2012 für das Web mit Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="ac42f-255">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="ac42f-256">Klicken Sie auf **jetzt installieren**.</span><span class="sxs-lookup"><span data-stu-id="ac42f-256">Click on **Install Now**.</span></span> <span data-ttu-id="ac42f-257">Wenn Sie keine **Webplattform-Installer** Sie Informationen zum Herunterladen und installieren Sie diese zuerst umgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="ac42f-257">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="ac42f-258">Einmal **Webplattform-Installer** geöffnet ist, klicken Sie auf **installieren** um das Setup zu starten.</span><span class="sxs-lookup"><span data-stu-id="ac42f-258">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="ac42f-259">![Visual Studio Express installieren](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "Visual Studio Express installieren")</span><span class="sxs-lookup"><span data-stu-id="ac42f-259">![Install Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="ac42f-260">*Installieren Sie Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="ac42f-260">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="ac42f-261">Lesen Sie die Produkte, Lizenzen und Begriffe, und klicken Sie auf **ich stimme** um den Vorgang fortzusetzen.</span><span class="sxs-lookup"><span data-stu-id="ac42f-261">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Akzeptieren der Lizenzbedingungen](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image23.png)

    <span data-ttu-id="ac42f-263">*Akzeptieren der Lizenzbedingungen*</span><span class="sxs-lookup"><span data-stu-id="ac42f-263">*Accepting the license terms*</span></span>
5. <span data-ttu-id="ac42f-264">Warten Sie, bis der Prozess herunterladen und die Installation abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="ac42f-264">Wait until the downloading and installation process completes.</span></span>

    ![Installationsstatus](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image24.png)

    <span data-ttu-id="ac42f-266">*Installationsstatus*</span><span class="sxs-lookup"><span data-stu-id="ac42f-266">*Installation progress*</span></span>
6. <span data-ttu-id="ac42f-267">Nach Abschluss der Installation klicken Sie auf **Fertig stellen**.</span><span class="sxs-lookup"><span data-stu-id="ac42f-267">When the installation completes, click **Finish**.</span></span>

    ![Installation wurde abgeschlossen](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image25.png)

    <span data-ttu-id="ac42f-269">*Installation wurde abgeschlossen*</span><span class="sxs-lookup"><span data-stu-id="ac42f-269">*Installation completed*</span></span>
7. <span data-ttu-id="ac42f-270">Klicken Sie auf **beenden** Webplattform-Installer zu schließen.</span><span class="sxs-lookup"><span data-stu-id="ac42f-270">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="ac42f-271">Um Visual Studio Express für Web zu öffnen, wechseln Sie zu der **starten** Startseite ein, und starten Sie das Schreiben von &quot; **Visual Studio Express**&quot;, klicken Sie dann auf die **Visual Studio Express für Web** Kachel.</span><span class="sxs-lookup"><span data-stu-id="ac42f-271">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Visual Studio Express für Web-Kachel](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image26.png)

    <span data-ttu-id="ac42f-273">*Visual Studio Express für Web-Kachel*</span><span class="sxs-lookup"><span data-stu-id="ac42f-273">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="ac42f-274">Anhang B: Verwendung von Codeausschnitten</span><span class="sxs-lookup"><span data-stu-id="ac42f-274">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="ac42f-275">Mit Codeausschnitten müssen Sie den Code, den Sie jederzeit griffbereit benötigen.</span><span class="sxs-lookup"><span data-stu-id="ac42f-275">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="ac42f-276">Das Lab-Dokument erfahren Sie, wenn Sie genau, verwenden können wie in der folgenden Abbildung dargestellt.</span><span class="sxs-lookup"><span data-stu-id="ac42f-276">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="ac42f-277">![Verwendung von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihr Projekt](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "Codeausschnitte zum Einfügen von Code in Ihr Projekt mit Visual Studio")</span><span class="sxs-lookup"><span data-stu-id="ac42f-277">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="ac42f-278">*Verwendung von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihr Projekt*</span><span class="sxs-lookup"><span data-stu-id="ac42f-278">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="ac42f-279">***Hinzufügen ein Codeausschnitts, die mithilfe der Tastatur (nur c#)***</span><span class="sxs-lookup"><span data-stu-id="ac42f-279">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="ac42f-280">Platzieren Sie den Cursor, wo möchten Sie den Code einfügen.</span><span class="sxs-lookup"><span data-stu-id="ac42f-280">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="ac42f-281">Starten Sie den Namen des Ausschnitts (ohne Leerzeichen oder Bindestriche) eingeben.</span><span class="sxs-lookup"><span data-stu-id="ac42f-281">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="ac42f-282">Beobachten Sie, wie IntelliSense zeigt übereinstimmenden Namen Ausschnitte.</span><span class="sxs-lookup"><span data-stu-id="ac42f-282">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="ac42f-283">Wählen Sie den richtigen Ausschnitt (oder behalten Sie eingeben, bis der gesamte Ausschnitt Name ausgewählt ist).</span><span class="sxs-lookup"><span data-stu-id="ac42f-283">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="ac42f-284">Drücken Sie die Tab-Taste zweimal, um den Ausschnitt an der Cursorposition eingefügt.</span><span class="sxs-lookup"><span data-stu-id="ac42f-284">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="ac42f-285">![Geben Sie den Namen des Ausschnitts](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "starten Sie den Namen des Ausschnitts eingeben")</span><span class="sxs-lookup"><span data-stu-id="ac42f-285">![Start typing the snippet name](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "Start typing the snippet name")</span></span>

<span data-ttu-id="ac42f-286">*Starten Sie den Namen des Ausschnitts eingeben*</span><span class="sxs-lookup"><span data-stu-id="ac42f-286">*Start typing the snippet name*</span></span>

<span data-ttu-id="ac42f-287">![Drücken Sie Tab, auf den hervorgehobenen Ausschnitt](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "drücken Sie die Tab hervorgehobenen Ausschnitt auswählen")</span><span class="sxs-lookup"><span data-stu-id="ac42f-287">![Press Tab to select the highlighted snippet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="ac42f-288">*Drücken Sie Tab, um den markierten Ausschnitt auszuwählen*</span><span class="sxs-lookup"><span data-stu-id="ac42f-288">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="ac42f-289">![Drücken Sie erneut die Tab und den Ausschnitt erweitert](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "drücken Sie erneut die Tab und den Ausschnitt erweitert")</span><span class="sxs-lookup"><span data-stu-id="ac42f-289">![Press Tab again and the snippet will expand](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="ac42f-290">*Drücken Sie erneut die Tab und den Ausschnitt erweitert*</span><span class="sxs-lookup"><span data-stu-id="ac42f-290">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="ac42f-291">***Hinzufügen ein Codeausschnitts, die mit der Maus (c#, Visual Basic und XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="ac42f-291">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="ac42f-292">Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen möchten.</span><span class="sxs-lookup"><span data-stu-id="ac42f-292">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="ac42f-293">Wählen Sie **Ausschnitt einfügen** gefolgt von **eigene Codeausschnitte**.</span><span class="sxs-lookup"><span data-stu-id="ac42f-293">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="ac42f-294">Wählen Sie die relevanten Ausschnitt aus der Liste, indem Sie darauf klicken.</span><span class="sxs-lookup"><span data-stu-id="ac42f-294">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="ac42f-295">![Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten,")</span><span class="sxs-lookup"><span data-stu-id="ac42f-295">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="ac42f-296">*Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten*</span><span class="sxs-lookup"><span data-stu-id="ac42f-296">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="ac42f-297">![Wählen Sie den relevanten Ausschnitt aus der Liste, indem Sie darauf klicken](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "den relevanten Ausschnitt aus der Liste auswählen, indem Sie darauf klicken")</span><span class="sxs-lookup"><span data-stu-id="ac42f-297">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="ac42f-298">*Wählen Sie den relevanten Ausschnitt aus der Liste, indem Sie darauf klicken*</span><span class="sxs-lookup"><span data-stu-id="ac42f-298">*Pick the relevant snippet from the list, by clicking on it*</span></span>
