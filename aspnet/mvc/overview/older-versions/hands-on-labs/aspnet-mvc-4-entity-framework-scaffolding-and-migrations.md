---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
title: ASP.NET MVC 4 Entity Framework – Gerüstbau und Migrationen | Microsoft-Dokumentation
author: rick-anderson
description: Wenn Sie mit ASP.NET MVC 4-Controllermethoden vertraut sind, oder haben die &quot;Hilfsprogramme, Formulare und Überprüfung&quot; praktische Übungseinheiten durchführen, sollten Sie bedenken...
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: 093c1362-f10b-407c-a708-be370f4b62b0
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
msc.type: authoredcontent
ms.openlocfilehash: 31f593f294c4865f621a8556cb43d0d9c42f2660
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814117"
---
# <a name="aspnet-mvc-4-entity-framework-scaffolding-and-migrations"></a><span data-ttu-id="b1ecf-103">ASP.NET MVC 4 Entity Framework – Gerüstbau und Migrationen</span><span class="sxs-lookup"><span data-stu-id="b1ecf-103">ASP.NET MVC 4 Entity Framework Scaffolding and Migrations</span></span>

<span data-ttu-id="b1ecf-104">durch [Web Camps Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="b1ecf-104">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="b1ecf-105">Herunterladen Sie Web Camps Training Kit</span><span class="sxs-lookup"><span data-stu-id="b1ecf-105">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="b1ecf-106">Wenn Sie mit ASP.NET MVC 4-Controllermethoden vertraut sind, oder haben die &quot;Hilfsprogramme, Formulare und Überprüfung&quot; praktische Übungseinheiten durchführen, sollten Sie bedenken, dass viele der Logik zu erstellen, aktualisieren, auflisten und entfernen alle Datenentität, er wird wiederholt, zwischen der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-106">If you are familiar with ASP.NET MVC 4 controller methods, or have completed the &quot;Helpers, Forms and Validation&quot; Hands-On lab, you should be aware that many of the logic to create, update, list and remove any data entity it is repeated among the application.</span></span> <span data-ttu-id="b1ecf-107">Ganz zu schweigen davon, dass, wenn Ihr Modell mehrere Klassen zum Bearbeiten von verfügt, Sie wahrscheinlich eine beträchtliche Zeit schreiben die POST- und GET-Aktionsmethoden für jeden Vorgang für die Entität, als auch jede der Ansichten können.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-107">Not to mention that, if your model has several classes to manipulate, you will be likely to spend a considerable time writing the POST and GET action methods for each entity operation, as well as each of the views.</span></span>

<span data-ttu-id="b1ecf-108">In dieser Übung lernen Sie, wie Sie das ASP.NET MVC 4-Gerüst zu verwenden, zum automatischen Generieren von der Baseline der Ihrer Anwendung CRUD (Create, Read, Update und Delete).</span><span class="sxs-lookup"><span data-stu-id="b1ecf-108">In this lab you will learn how to use the ASP.NET MVC 4 scaffolding to automatically generate the baseline of your application's CRUD (Create, Read, Update and Delete).</span></span> <span data-ttu-id="b1ecf-109">Starten über eine einfache Modellklasse und, ohne eine einzige Codezeile schreiben zu müssen, erstellen Sie einen Controller, der die CRUD-Vorgänge sowie die alle erforderlichen Ansichten enthält.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-109">Starting from a simple model class, and, without writing a single line of code, you will create a controller that will contain all the CRUD operations, as well as the all the necessary views.</span></span> <span data-ttu-id="b1ecf-110">Nach dem Erstellen und eine einfache Lösung ausführen, müssen Sie die Datenbank generiert werden, zusammen mit dem MVC-Logik und die Ansichten für die datenbearbeitung.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-110">After building and running the simple solution, you will have the application database generated, together with the MVC logic and views for data manipulation.</span></span>

<span data-ttu-id="b1ecf-111">Darüber hinaus erfahren Sie, wie einfach es ist, die Entity Framework-Migrationen zu verwenden, um Modelle von Aktualisierungen in der gesamten Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-111">In addition, you will learn how easy it is to use Entity Framework Migrations to perform model updates throughout your entire application.</span></span> <span data-ttu-id="b1ecf-112">Entity Framework-Migrationen können Sie Ihre Datenbank zu ändern, nachdem das Modell mit einfachen Schritten geändert wurde.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-112">Entity Framework Migrations will let you modify your database after the model has changed with simple steps.</span></span> <span data-ttu-id="b1ecf-113">Mit all diese Denken Sie daran werden Sie zum Erstellen und Verwalten von Webanwendungen effizienter, nutzen die neuesten Funktionen von ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-113">With all these in mind, you will be able to build and maintain web applications more efficiently, taking advantage of the latest features of ASP.NET MVC 4.</span></span>

> [!NOTE]
> <span data-ttu-id="b1ecf-114">Alle Beispielcode und Ausschnitte sind im Web Camps Trainingskit, erhältlich über enthalten [Versionen der Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="b1ecf-114">All sample code and snippets are included in the Web Camps Training Kit, available from at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="b1ecf-115">Das Projekt, das speziell für dieses Lab finden Sie unter [ASP.NET MVC 4 Entity Framework – Gerüstbau und Migrationen](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).</span><span class="sxs-lookup"><span data-stu-id="b1ecf-115">The project specific to this lab is available at [ASP.NET MVC 4 Entity Framework Scaffolding and Migrations](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="b1ecf-116">Ziele</span><span class="sxs-lookup"><span data-stu-id="b1ecf-116">Objectives</span></span>

<span data-ttu-id="b1ecf-117">In dieser praktischen Übungseinheit erfahren Sie, wie Sie:</span><span class="sxs-lookup"><span data-stu-id="b1ecf-117">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="b1ecf-118">Verwenden Sie ASP.NET-Gerüstbau für CRUD-Vorgänge in Controllern.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-118">Use ASP.NET scaffolding for CRUD operations in controllers.</span></span>
- <span data-ttu-id="b1ecf-119">Ändern Sie das Datenbankmodell mit Entity Framework-Migrationen.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-119">Change the database model using Entity Framework Migrations.</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="b1ecf-120">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="b1ecf-120">Prerequisites</span></span>

<span data-ttu-id="b1ecf-121">Sie benötigen Folgendes, um diese testumgebung abzuschließen:</span><span class="sxs-lookup"><span data-stu-id="b1ecf-121">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="b1ecf-122">[Microsoft Visual Studio Express 2012 für Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) oder sogar eine höhere (Lesen [Anhang A](#AppendixA) Anleitungen zur Installation).</span><span class="sxs-lookup"><span data-stu-id="b1ecf-122">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="b1ecf-123">Setup</span><span class="sxs-lookup"><span data-stu-id="b1ecf-123">Setup</span></span>

<span data-ttu-id="b1ecf-124">**Installieren von Codeausschnitten**</span><span class="sxs-lookup"><span data-stu-id="b1ecf-124">**Installing Code Snippets**</span></span>

<span data-ttu-id="b1ecf-125">Der Einfachheit halber ist Großteil des Codes, die entlang dieser Übungseinheit verwaltet werden soll als Codeausschnitte für Visual Studio verfügbar.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-125">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="b1ecf-126">So installieren Sie die Codeausschnitte ausführen **.\Source\Setup\CodeSnippets.vsi** Datei.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-126">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="b1ecf-127">Wenn Sie nicht mit dem Visual Studio Code Snippets und zu erfahren, wie Sie deren Verwendung vertraut sind, sehen Sie sich im Anhang in diesem Dokument &quot; [Anhang B: Verwenden von Code Snippets](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-127">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="b1ecf-128">Übungen</span><span class="sxs-lookup"><span data-stu-id="b1ecf-128">Exercises</span></span>

<span data-ttu-id="b1ecf-129">In der folgenden Übung besteht aus diesem Hands-On Lab:</span><span class="sxs-lookup"><span data-stu-id="b1ecf-129">The following exercise make up this Hands-On Lab:</span></span>

1. [<span data-ttu-id="b1ecf-130">Verwenden von ASP.NET MVC 4-Gerüstbau mit Entity Framework-Migrationen</span><span class="sxs-lookup"><span data-stu-id="b1ecf-130">Using ASP.NET MVC 4 Scaffolding with Entity Framework Migrations</span></span>](#Exercise1)

> [!NOTE]
> <span data-ttu-id="b1ecf-131">In dieser Übung wird zusammen mit einem **End** Ordner mit der resultierenden Lösung, die Sie nach Abschluss der Übung abrufen soll.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-131">This exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercise.</span></span> <span data-ttu-id="b1ecf-132">Sie können diese Lösung als Leitfaden verwenden, wenn Sie zusätzliche Hilfe bei der Arbeit mit der Aufgabe benötigen.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-132">You can use this solution as a guide if you need additional help working through the exercise.</span></span>


<span data-ttu-id="b1ecf-133">Geschätzte Zeit für diese testumgebung abzuschließen: **30 Minuten**</span><span class="sxs-lookup"><span data-stu-id="b1ecf-133">Estimated time to complete this lab: **30 minutes**</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Using_ASPNET_MVC_4_Scaffolding_with_Entity_Framework_Migrations"></a>
### <a name="exercise-1-using-aspnet-mvc-4-scaffolding-with-entity-framework-migrations"></a><span data-ttu-id="b1ecf-134">Übung 1: Verwenden von ASP.NET MVC 4-Gerüstbau mit Entity Framework-Migrationen</span><span class="sxs-lookup"><span data-stu-id="b1ecf-134">Exercise 1: Using ASP.NET MVC 4 Scaffolding with Entity Framework Migrations</span></span>

<span data-ttu-id="b1ecf-135">ASP.NET MVC-Gerüstbau bietet eine schnelle Möglichkeit zum Generieren der CRUD-Vorgänge auf standardisierte Weise erstellen die erforderliche Logik, mit dem Ihre Anwendung mit der Datenbankschicht zu interagieren.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-135">ASP.NET MVC scaffolding provides a quick way to generate the CRUD operations in a standardized way, creating the necessary logic that lets your application interact with the database layer.</span></span>

<span data-ttu-id="b1ecf-136">In dieser Übung lernen Sie, wie mit ASP.NET MVC 4-Gerüstbau mit Code zuerst die CRUD-Methoden erstellen.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-136">In this exercise, you will learn how to use ASP.NET MVC 4 scaffolding with code first to create the CRUD methods.</span></span> <span data-ttu-id="b1ecf-137">Anschließend erfahren Sie, wie Sie das Modell anwenden der Änderungen in der Datenbank mit Entity Framework-Migrationen aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-137">Then, you will learn how to update your model applying the changes in the database by using Entity Framework Migrations.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1-_Creating_a_new_ASPNET_MVC_4_project_using_Scaffolding"></a>
#### <a name="task-1--creating-a-new-aspnet-mvc-4-project-using-scaffolding"></a><span data-ttu-id="b1ecf-138">Aufgabe 1: Erstellen einer neuen ASP.NET MVC 4-Projekt mithilfe von Gerüstbau</span><span class="sxs-lookup"><span data-stu-id="b1ecf-138">Task 1- Creating a new ASP.NET MVC 4 project using Scaffolding</span></span>

1. <span data-ttu-id="b1ecf-139">Wenn nicht bereits geöffnet ist, starten Sie **Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-139">If not already open, start **Visual Studio 2012**.</span></span>
2. <span data-ttu-id="b1ecf-140">Wählen Sie **Datei | Neues Projekt**.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-140">Select **File | New Project**.</span></span> <span data-ttu-id="b1ecf-141">In das neue Projekt im Dialogfeld unter die **Visual c# | Web** wählen Sie im Abschnitt **ASP.NET MVC 4-Webanwendung**.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-141">In the New Project dialog, under the **Visual C# | Web** section, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="b1ecf-142">Nennen Sie das Projekt zu **MVC4andEFMigrations** , und legen Sie den Speicherort auf **Source\Ex1-UsingMVC4ScaffoldingEFMigrations** Ordner dieser Anleitung.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-142">Name the project to **MVC4andEFMigrations** and set the location to **Source\Ex1-UsingMVC4ScaffoldingEFMigrations** folder of this lab.</span></span> <span data-ttu-id="b1ecf-143">Legen Sie die **Projektmappenname** zu **beginnen** und stellen Sie sicher **Projektmappenverzeichnis erstellen** aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-143">Set the **Solution name** to **Begin** and ensure **Create directory for solution** is checked.</span></span> <span data-ttu-id="b1ecf-144">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-144">Click **OK**.</span></span>

    <span data-ttu-id="b1ecf-145">![Dialogfeld Neues ASP.NET MVC 4-Projekt](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "neue ASP.NET MVC 4-Projekt (Dialogfeld)")</span><span class="sxs-lookup"><span data-stu-id="b1ecf-145">![New ASP.NET MVC 4 Project Dialog Box](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "New ASP.NET MVC 4 Project Dialog Box")</span></span>

    <span data-ttu-id="b1ecf-146">*Neues ASP.NET MVC 4-Projekt (Dialogfeld)*</span><span class="sxs-lookup"><span data-stu-id="b1ecf-146">*New ASP.NET MVC 4 Project Dialog Box*</span></span>
3. <span data-ttu-id="b1ecf-147">In der **neues ASP.NET MVC 4-Projekt** aktivieren Sie im Dialogfeld die **Internetanwendung** Vorlage, und stellen Sie sicher, dass **Razor** die ausgewählte **Ansichts-Engine**.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-147">In the **New ASP.NET MVC 4 Project** dialog box select the **Internet Application** template, and make sure that **Razor** is the selected **View engine**.</span></span> <span data-ttu-id="b1ecf-148">Klicken Sie auf **OK**, um das Projekt zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-148">Click **OK** to create the project.</span></span>

    <span data-ttu-id="b1ecf-149">![Neue ASP.NET MVC 4-Internetanwendung](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "neue ASP.NET MVC 4-Internetanwendung")</span><span class="sxs-lookup"><span data-stu-id="b1ecf-149">![New ASP.NET MVC 4 Internet Application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "New ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="b1ecf-150">*Neue ASP.NET MVC 4-Internetanwendung*</span><span class="sxs-lookup"><span data-stu-id="b1ecf-150">*New ASP.NET MVC 4 Internet Application*</span></span>
4. <span data-ttu-id="b1ecf-151">Klicken Sie im Projektmappen-Explorer mit der Maustaste **Modelle** , und wählen Sie **hinzufügen | Klasse** um eine einfache Klasse Person (POCO) zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-151">In the Solution Explorer, right-click **Models** and select **Add | Class** to create a simple class person (POCO).</span></span> <span data-ttu-id="b1ecf-152">Nennen Sie sie **Person** , und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-152">Name it **Person** and click **OK**.</span></span>
5. <span data-ttu-id="b1ecf-153">Öffnen Sie die Person-Klasse und fügen Sie die folgenden Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-153">Open the Person class and insert the following properties.</span></span>

    <span data-ttu-id="b1ecf-154">(Codeausschnitt - *ASP.NET MVC 4 und Entity Framework-Migrationen - Ex1 Personeneigenschaften*)</span><span class="sxs-lookup"><span data-stu-id="b1ecf-154">(Code Snippet - *ASP.NET MVC 4 and Entity Framework Migrations - Ex1 Person Properties*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample1.cs)]
6. <span data-ttu-id="b1ecf-155">Klicken Sie auf **erstellen | Erstellen Sie Projektmappe** zum Speichern der Änderungen, und erstellen Sie das Projekt.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-155">Click **Build | Build Solution** to save the changes and build the project.</span></span>

    <span data-ttu-id="b1ecf-156">![Erstellen der Anwendung](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "Erstellen der Anwendung")</span><span class="sxs-lookup"><span data-stu-id="b1ecf-156">![Building the application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "Building the application")</span></span>

    <span data-ttu-id="b1ecf-157">*Erstellen der Anwendung*</span><span class="sxs-lookup"><span data-stu-id="b1ecf-157">*Building the Application*</span></span>
7. <span data-ttu-id="b1ecf-158">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste in den Ordner "Controllers", und wählen **hinzufügen | Controller**.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-158">In the Solution Explorer, right-click the controllers folder and select **Add | Controller**.</span></span>
8. <span data-ttu-id="b1ecf-159">Nennen Sie den Controller *PersonController* und führen Sie die **gerüstoptionen** mit den folgenden Werten.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-159">Name the controller *PersonController* and complete the **Scaffolding options** with the following values.</span></span>

   1. <span data-ttu-id="b1ecf-160">In der **Vorlage** Dropdown-Liste die **MVC-Controller mit Lese-/schreibaktionen und Ansichten unter Verwendung von Entity Framework** Option.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-160">In the **Template** drop-down list, select the **MVC controller with read/write actions and views, using Entity Framework** option.</span></span>
   2. <span data-ttu-id="b1ecf-161">In der **Modellklasse** Dropdown-Liste die **Person** Klasse.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-161">In the **Model class** drop-down list, select the **Person** class.</span></span>
   3. <span data-ttu-id="b1ecf-162">In der **Datenkontext Klasse** Liste  **&lt;neuer Datenkontext... &gt;**.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-162">In the **Data Context class** list, select **&lt;New data context...&gt;**.</span></span> <span data-ttu-id="b1ecf-163">Wählen Sie einen beliebigen Namen ein, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-163">Choose any name and click **OK**.</span></span>
   4. <span data-ttu-id="b1ecf-164">In der **Ansichten** Dropdown-Liste, stellen Sie sicher, dass **Razor** ausgewählt ist.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-164">In the **Views** drop-down list, make sure that **Razor** is selected.</span></span>

      <span data-ttu-id="b1ecf-165">![Die Person-Controller mit Gerüst hinzufügen](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "den Person-Controller mit Gerüst hinzufügen")</span><span class="sxs-lookup"><span data-stu-id="b1ecf-165">![Adding the Person controller with scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "Adding the Person controller with scaffolding")</span></span>

      <span data-ttu-id="b1ecf-166">*Die Person-Controller mit Gerüst hinzufügen*</span><span class="sxs-lookup"><span data-stu-id="b1ecf-166">*Adding the Person controller with scaffolding*</span></span>
9. <span data-ttu-id="b1ecf-167">Klicken Sie auf **hinzufügen** auf den neuen Controller für Personen mit Gerüst zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-167">Click **Add** to create the new controller for Person with scaffolding.</span></span> <span data-ttu-id="b1ecf-168">Sie haben jetzt die Controlleraktionen sowie die Ansichten erstellt.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-168">You have now generated the controller actions as well as the views.</span></span>

    <span data-ttu-id="b1ecf-169">![Nach dem Erstellen des Person-Controllers mit Gerüstbau](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "nach dem Erstellen des Person-Controllers mit Gerüstbau")</span><span class="sxs-lookup"><span data-stu-id="b1ecf-169">![After creating the Person controller with scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "After creating the Person controller with scaffolding")</span></span>

    <span data-ttu-id="b1ecf-170">*Nach dem Erstellen des Person-Controllers mit Gerüstbau*</span><span class="sxs-lookup"><span data-stu-id="b1ecf-170">*After creating the Person controller with scaffolding*</span></span>
10. <span data-ttu-id="b1ecf-171">Open **PersonController** Klasse.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-171">Open **PersonController** class.</span></span> <span data-ttu-id="b1ecf-172">Beachten Sie, dass die vollständige CRUD-Aktionsmethoden automatisch generiert wurden.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-172">Notice that the full CRUD action methods have been generated automatically.</span></span>

   <span data-ttu-id="b1ecf-173">![In den Person-Controller](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "innerhalb der Person-Controller")</span><span class="sxs-lookup"><span data-stu-id="b1ecf-173">![Inside the Person controller](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "Inside the Person controller")</span></span>

   <span data-ttu-id="b1ecf-174">*In den Person-controller*</span><span class="sxs-lookup"><span data-stu-id="b1ecf-174">*Inside the Person controller*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2-_Running_the_application"></a>
#### <a name="task-2--running-the-application"></a><span data-ttu-id="b1ecf-175">Aufgabe 2: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="b1ecf-175">Task 2- Running the application</span></span>

<span data-ttu-id="b1ecf-176">An diesem Punkt ist die Datenbank noch nicht erstellt.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-176">At this point, the database is not yet created.</span></span> <span data-ttu-id="b1ecf-177">In dieser Aufgabe werden die Anwendung zum ersten Mal ausführen und Testen Sie die CRUD-Vorgänge.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-177">In this task, you will run the application for the first time and test the CRUD operations.</span></span> <span data-ttu-id="b1ecf-178">Die Datenbank wird im laufenden Betrieb mit Code First erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-178">The database will be created on the fly with Code First.</span></span>

1. <span data-ttu-id="b1ecf-179">Drücken Sie **F5**, um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-179">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="b1ecf-180">Fügen Sie im Browser **/Person** an die URL der Seite "Person" zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-180">In the browser, add **/Person** to the URL to open the Person page.</span></span>

    <span data-ttu-id="b1ecf-181">![Anwendung – erste Ausführung](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "Anwendung – erste Ausführung")</span><span class="sxs-lookup"><span data-stu-id="b1ecf-181">![Application first run](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "Application first run")</span></span>

    <span data-ttu-id="b1ecf-182">*Der Anwendung: ersten Ausführen*</span><span class="sxs-lookup"><span data-stu-id="b1ecf-182">*Application: first run*</span></span>
3. <span data-ttu-id="b1ecf-183">Jetzt Untersuchen der Person-Seiten und testen die CRUD-Vorgänge.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-183">You will now explore the Person pages and test the CRUD operations.</span></span>

    1. <span data-ttu-id="b1ecf-184">Klicken Sie auf **neu erstellen** eine neue Person hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-184">Click **Create New** to add a new person.</span></span> <span data-ttu-id="b1ecf-185">Geben Sie einen Vornamen und einen Nachnamen ein, und klicken Sie auf **erstellen**.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-185">Enter a first name and a last name and click **Create**.</span></span>

        <span data-ttu-id="b1ecf-186">![Eine neue Person hinzufügen](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "eine neue Person hinzufügen")</span><span class="sxs-lookup"><span data-stu-id="b1ecf-186">![Adding a new person](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "Adding a new person")</span></span>

        <span data-ttu-id="b1ecf-187">*Eine neue Person hinzufügen*</span><span class="sxs-lookup"><span data-stu-id="b1ecf-187">*Adding a new person*</span></span>
    2. <span data-ttu-id="b1ecf-188">In der Liste der Person können Sie zu löschen, bearbeiten oder Hinzufügen von Elementen.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-188">In the person's list, you can delete, edit or add items.</span></span>

        <span data-ttu-id="b1ecf-189">![Personenliste](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "Person-Liste")</span><span class="sxs-lookup"><span data-stu-id="b1ecf-189">![person list](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "person list")</span></span>

        <span data-ttu-id="b1ecf-190">*Person-Liste*</span><span class="sxs-lookup"><span data-stu-id="b1ecf-190">*Person list*</span></span>
    3. <span data-ttu-id="b1ecf-191">Klicken Sie auf **Details** um der Person zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-191">Click **Details** to open the person's details.</span></span>

        <span data-ttu-id="b1ecf-192">![Person Details](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "Person Details")</span><span class="sxs-lookup"><span data-stu-id="b1ecf-192">![Person's details](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "Person's details")</span></span>

        <span data-ttu-id="b1ecf-193">*Person-details*</span><span class="sxs-lookup"><span data-stu-id="b1ecf-193">*Person's details*</span></span>
4. <span data-ttu-id="b1ecf-194">Schließen Sie den Browser, und kehren Sie zu Visual Studio zurück.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-194">Close the browser and return to Visual Studio.</span></span> <span data-ttu-id="b1ecf-195">Beachten Sie, dass Sie in der gesamten Anwendung – von des Modells in den Ansichten: die gesamte CRUD-Vorgänge für die Personenentität erstellt haben, ohne eine einzige Codezeile schreiben zu müssen.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-195">Notice that you have created the whole CRUD for the person entity throughout your application -from the model to the views- without having to write a single line of code!</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3-_Updating_the_database_using_Entity_Framework_Migrations"></a>
#### <a name="task-3--updating-the-database-using-entity-framework-migrations"></a><span data-ttu-id="b1ecf-196">Aufgabe 3: Aktualisieren der Datenbank mithilfe von Entity Framework-Migrationen</span><span class="sxs-lookup"><span data-stu-id="b1ecf-196">Task 3- Updating the database using Entity Framework Migrations</span></span>

<span data-ttu-id="b1ecf-197">In dieser Aufgabe aktualisieren Sie die Datenbank mithilfe von Entity Framework-Migrationen.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-197">In this task you will update the database using Entity Framework Migrations.</span></span> <span data-ttu-id="b1ecf-198">Sie werden feststellen, wie einfach es ist, ändern Sie das Modell und die Änderungen widerzuspiegeln, in Ihren Datenbanken mithilfe der Funktion von Entity Framework-Migrationen.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-198">You will discover how easy it is to change the model and reflect the changes in your databases by using the Entity Framework Migrations feature.</span></span>

1. <span data-ttu-id="b1ecf-199">Öffnen Sie die Paket-Manager-Konsole.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-199">Open the Package Manager Console.</span></span> <span data-ttu-id="b1ecf-200">Wählen Sie **Tools | Bibliothekspaket-Manager | Paket-Manager-Konsole**.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-200">Select **Tools | Library Package Manager | Package Manager Console**.</span></span>
2. <span data-ttu-id="b1ecf-201">Geben Sie in der Paket-Manager-Konsole den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="b1ecf-201">In the Package Manager Console, enter the following command:</span></span>

    <span data-ttu-id="b1ecf-202">PMC</span><span class="sxs-lookup"><span data-stu-id="b1ecf-202">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample2.ps1)]

    <span data-ttu-id="b1ecf-203">![Aktivieren von Migrationen](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "Migrationen aktivieren")</span><span class="sxs-lookup"><span data-stu-id="b1ecf-203">![Enabling Migrations](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "Enabling Migrations")</span></span>

    <span data-ttu-id="b1ecf-204">*Aktivieren von Migrationen*</span><span class="sxs-lookup"><span data-stu-id="b1ecf-204">*Enabling migrations*</span></span>

    <span data-ttu-id="b1ecf-205">Der Aktivierung der Migration Befehl erstellt die **Migrationen** Ordner, in dem ein Skript zum Initialisieren der Datenbank enthalten.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-205">The Enable-Migration command creates the **Migrations** folder, which contains a script to initialize the database.</span></span>

    <span data-ttu-id="b1ecf-206">![Ordner "Migrations"](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "Ordner \"Migrations\"")</span><span class="sxs-lookup"><span data-stu-id="b1ecf-206">![Migrations folder](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "Migrations folder")</span></span>

    <span data-ttu-id="b1ecf-207">*Ordner "Migrations"*</span><span class="sxs-lookup"><span data-stu-id="b1ecf-207">*Migrations folder*</span></span>
3. <span data-ttu-id="b1ecf-208">Öffnen der **Configuration.cs** -Datei im Ordner "Migrations".</span><span class="sxs-lookup"><span data-stu-id="b1ecf-208">Open the **Configuration.cs** file in the Migrations folder.</span></span> <span data-ttu-id="b1ecf-209">Der Konstruktor der Klasse zu suchen und ändern Sie die **AutomaticMigrationsEnabled** Wert *"true"*.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-209">Locate the class constructor and change the **AutomaticMigrationsEnabled** value to *true*.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample3.cs)]
4. <span data-ttu-id="b1ecf-210">Öffnen Sie die Person-Klasse, und fügen Sie ein Attribut für den Vornamen einer Person hinzu.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-210">Open the Person class and add an attribute for the person's middle name.</span></span> <span data-ttu-id="b1ecf-211">Mit diesem neuen Attribut ändern Sie das Modell.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-211">With this new attribute, you are changing the model.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample4.cs)]
5. <span data-ttu-id="b1ecf-212">Wählen Sie **erstellen | Erstellen Sie Projektmappe** im Menü, um die Anwendung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-212">Select **Build | Build Solution** on the menu to build the application.</span></span>

    <span data-ttu-id="b1ecf-213">![Erstellen der Anwendung](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "Erstellen der Anwendung")</span><span class="sxs-lookup"><span data-stu-id="b1ecf-213">![Building the application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "Building the application")</span></span>

    <span data-ttu-id="b1ecf-214">*Erstellen der Anwendung*</span><span class="sxs-lookup"><span data-stu-id="b1ecf-214">*Building the application*</span></span>
6. <span data-ttu-id="b1ecf-215">Geben Sie in der Paket-Manager-Konsole den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="b1ecf-215">In the Package Manager Console, enter the following command:</span></span>

    <span data-ttu-id="b1ecf-216">PMC</span><span class="sxs-lookup"><span data-stu-id="b1ecf-216">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample5.ps1)]

    <span data-ttu-id="b1ecf-217">Dieser Befehl sucht nach Änderungen in den Datenobjekten, und klicken Sie dann, fügen sie die erforderlichen Befehle, die Datenbank entsprechend zu ändern.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-217">This command will look for changes in the data objects, and then, it will add the necessary commands to modify the database accordingly.</span></span>

    <span data-ttu-id="b1ecf-218">![Hinzufügen einer zweiten Vornamens](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "einen Zweitnamen hinzufügen")</span><span class="sxs-lookup"><span data-stu-id="b1ecf-218">![Adding a middle name](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "Adding a middle name")</span></span>

    <span data-ttu-id="b1ecf-219">*Ein zweiter Vorname hinzufügen*</span><span class="sxs-lookup"><span data-stu-id="b1ecf-219">*Adding a middle name*</span></span>
7. <span data-ttu-id="b1ecf-220">(Optional) Sie können den folgenden Befehl zum Generieren eines SQL-Skripts mit dem Update, differenzielle ausführen.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-220">(Optional) You can run the following command to generate a SQL script with the differential update.</span></span> <span data-ttu-id="b1ecf-221">Auf diese Weise können Sie die Datenbank manuell zu aktualisieren (In diesem Fall es ist nicht erforderlich), oder wenden Sie die Änderungen in anderen Datenbanken:</span><span class="sxs-lookup"><span data-stu-id="b1ecf-221">This will let you update the database manually (In this case it's not necessary), or apply the changes in other databases:</span></span>

    <span data-ttu-id="b1ecf-222">PMC</span><span class="sxs-lookup"><span data-stu-id="b1ecf-222">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample6.ps1)]

    <span data-ttu-id="b1ecf-223">![Generieren eines SQL-Skripts](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "Generieren eines SQL-Skripts")</span><span class="sxs-lookup"><span data-stu-id="b1ecf-223">![Generating a SQL script](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "Generating a SQL script")</span></span>

    <span data-ttu-id="b1ecf-224">*Generieren eines SQL-Skripts*</span><span class="sxs-lookup"><span data-stu-id="b1ecf-224">*Generating a SQL script*</span></span>

    <span data-ttu-id="b1ecf-225">![Update für SQL-Skript](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "Update für SQL-Skript")</span><span class="sxs-lookup"><span data-stu-id="b1ecf-225">![SQL Script update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "SQL Script update")</span></span>

    <span data-ttu-id="b1ecf-226">*Update für SQL-Skript*</span><span class="sxs-lookup"><span data-stu-id="b1ecf-226">*SQL Script update*</span></span>
8. <span data-ttu-id="b1ecf-227">Geben Sie den folgenden Befehl zum Aktualisieren der Datenbank, in der Paket-Manager-Konsole:</span><span class="sxs-lookup"><span data-stu-id="b1ecf-227">In the Package Manager Console, enter the following command to update the database:</span></span>

    <span data-ttu-id="b1ecf-228">PMC</span><span class="sxs-lookup"><span data-stu-id="b1ecf-228">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample7.ps1)]

    <span data-ttu-id="b1ecf-229">![Aktualisieren der Datenbank](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "Aktualisieren der Datenbank")</span><span class="sxs-lookup"><span data-stu-id="b1ecf-229">![Updating the Database](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "Updating the Database")</span></span>

    <span data-ttu-id="b1ecf-230">*Aktualisieren der Datenbank*</span><span class="sxs-lookup"><span data-stu-id="b1ecf-230">*Updating the Database*</span></span>

    <span data-ttu-id="b1ecf-231">Hiermit wird die **MiddleName** -Spalte in der **Personen** Tabelle entsprechend die aktuelle Definition der der **Person** Klasse.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-231">This will add the **MiddleName** column in the **People** table to match the current definition of the **Person** class.</span></span>
9. <span data-ttu-id="b1ecf-232">Nachdem die Datenbank aktualisiert wurde, mit der rechten Maustaste in den Ordner "Controller", und wählen Sie **hinzufügen | Controller** zum Hinzufügen der Person Controllers erneut (vollständig mit den gleichen Werten).</span><span class="sxs-lookup"><span data-stu-id="b1ecf-232">Once the database is updated, right-click the Controller folder and select **Add | Controller** to add the Person controller again (Complete with the same values).</span></span> <span data-ttu-id="b1ecf-233">Dadurch werden die vorhandenen Methoden und Ansichten, die das neue Attribut hinzufügen aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-233">This will update the existing methods and views adding the new attribute.</span></span>

    <span data-ttu-id="b1ecf-234">![Hinzufügen von einem controllerupdate](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "Hinzufügen von einem controllerupdate")</span><span class="sxs-lookup"><span data-stu-id="b1ecf-234">![Adding a controller update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "Adding a controller update")</span></span>

    <span data-ttu-id="b1ecf-235">*Update des Controllers*</span><span class="sxs-lookup"><span data-stu-id="b1ecf-235">*Updating the controller*</span></span>
10. <span data-ttu-id="b1ecf-236">Klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-236">Click **Add**.</span></span> <span data-ttu-id="b1ecf-237">Wählen Sie dann die Werte **überschreiben PersonController.cs** und **überschreiben zugeordnete Ansichten** , und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-237">Then, select the values **Overwrite PersonController.cs** and the **Overwrite associated views** and click **OK**.</span></span>

   ![Das Überschreiben einer Controller hinzufügen](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image19.png)

   <span data-ttu-id="b1ecf-239">*Update des Controllers*</span><span class="sxs-lookup"><span data-stu-id="b1ecf-239">*Updating the controller*</span></span>

<a id="Ex1Task4"></a>

<a id="Task4-_Running_the_application"></a>
#### <a name="task4--running-the-application"></a><span data-ttu-id="b1ecf-240">Task4 - Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="b1ecf-240">Task4- Running the application</span></span>

1. <span data-ttu-id="b1ecf-241">Drücken Sie **F5**, um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-241">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="b1ecf-242">Open **/Person**.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-242">Open **/Person**.</span></span> <span data-ttu-id="b1ecf-243">Beachten Sie, dass die Daten beibehalten wurde, während die zweite Vorname-Spalte hinzugefügt wurde, ein.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-243">Notice that the data was preserved, while the middle name column was added.</span></span>

    <span data-ttu-id="b1ecf-244">![Zweiter Vorname hinzugefügt](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "Vornamen hinzugefügt")</span><span class="sxs-lookup"><span data-stu-id="b1ecf-244">![Middle Name added](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "Middle Name added")</span></span>

    <span data-ttu-id="b1ecf-245">*Zweiter Vorname hinzugefügt*</span><span class="sxs-lookup"><span data-stu-id="b1ecf-245">*Middle Name added*</span></span>
3. <span data-ttu-id="b1ecf-246">Wenn Sie auf **bearbeiten**, ein zweiter Vorname an die aktuelle Person hinzufügen können.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-246">If you click **Edit**, you will be able to add a middle name to the current person.</span></span>

    <span data-ttu-id="b1ecf-247">![Zweiter Vorname Edition](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "Vornamen-Edition")</span><span class="sxs-lookup"><span data-stu-id="b1ecf-247">![Middle Name edition](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "Middle Name edition")</span></span>

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="b1ecf-248">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="b1ecf-248">Summary</span></span>

<span data-ttu-id="b1ecf-249">In dieser praktischen Übungseinheit haben Sie einfache Schritte zum Erstellen von CRUD-Vorgänge mit ASP.NET MVC 4-Gerüstbau jede beliebige Modellklasse verwenden.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-249">In this Hands-On lab, you have learned simple steps to create CRUD operations with ASP.NET MVC 4 Scaffolding using any model class.</span></span> <span data-ttu-id="b1ecf-250">Klicken Sie dann haben Sie gelernt, ein Update für die End-to-End in Ihrer Anwendung – aus der Datenbank, die den Ansichten – führen Sie mithilfe von Entity Framework-Migrationen.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-250">Then, you have learned how to perform an end to end update in your application -from the database to the views- by using Entity Framework Migrations.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="b1ecf-251">Anhang A: Installieren von Visual Studio Express 2012 für Web</span><span class="sxs-lookup"><span data-stu-id="b1ecf-251">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="b1ecf-252">Sie installieren können **Microsoft Visual Studio Express 2012 für Web** oder einem anderen &quot;Express&quot; Version verwenden, die **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-252">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="b1ecf-253">Die folgenden Anweisungen führen Sie über die erforderlichen Schritte zum Installieren *Visual Studio Express 2012 für Web* mit *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-253">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="b1ecf-254">Wechseln Sie zu [ [ https://go.microsoft.com/? Linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="b1ecf-254">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="b1ecf-255">Auch wenn Sie bereits Webplattform-Installer installiert haben, können Sie öffnen ihn, und suchen Sie nach dem Produkt &quot; <em>Visual Studio Express 2012 für das Web mit Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-255">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="b1ecf-256">Klicken Sie auf **jetzt installieren**.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-256">Click on **Install Now**.</span></span> <span data-ttu-id="b1ecf-257">Wenn Sie keine **Webplattform-Installer** gelangen Sie zum Herunterladen und installieren Sie diese zuerst.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-257">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="b1ecf-258">Einmal **Webplattform-Installer** geöffnet ist, klicken Sie auf **installieren** um das Setup zu starten.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-258">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="b1ecf-259">![Installieren von Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "Installieren von Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="b1ecf-259">![Install Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="b1ecf-260">*Installieren von Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="b1ecf-260">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="b1ecf-261">Lesen Sie die Produkte Lizenzen und Begriffe, und klicken Sie auf **akzeptieren** um den Vorgang fortzusetzen.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-261">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Akzeptieren der Lizenzbedingungen](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image23.png)

    <span data-ttu-id="b1ecf-263">*Akzeptieren der Lizenzbedingungen*</span><span class="sxs-lookup"><span data-stu-id="b1ecf-263">*Accepting the license terms*</span></span>
5. <span data-ttu-id="b1ecf-264">Warten Sie, bis der Prozess zum Herunterladen und die Installation abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-264">Wait until the downloading and installation process completes.</span></span>

    ![Installationsstatus](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image24.png)

    <span data-ttu-id="b1ecf-266">*Installationsstatus*</span><span class="sxs-lookup"><span data-stu-id="b1ecf-266">*Installation progress*</span></span>
6. <span data-ttu-id="b1ecf-267">Wenn die Installation abgeschlossen ist, klicken Sie auf **Fertig stellen**.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-267">When the installation completes, click **Finish**.</span></span>

    ![Die Installation wurde abgeschlossen](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image25.png)

    <span data-ttu-id="b1ecf-269">*Die Installation wurde abgeschlossen*</span><span class="sxs-lookup"><span data-stu-id="b1ecf-269">*Installation completed*</span></span>
7. <span data-ttu-id="b1ecf-270">Klicken Sie auf **beenden** Webplattform-Installer zu schließen.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-270">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="b1ecf-271">Um Visual Studio Express für Web zu öffnen, wechseln Sie zu der **starten** schreiben zu starten und Bildschirm &quot; **VS Express**&quot;, klicken Sie dann auf die **Visual Studio Express für Web** die Kachel.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-271">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Visual Studio Express für Web-Kachel](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image26.png)

    <span data-ttu-id="b1ecf-273">*Visual Studio Express für Web-Kachel*</span><span class="sxs-lookup"><span data-stu-id="b1ecf-273">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="b1ecf-274">Anhang B: Verwenden von Codeausschnitten</span><span class="sxs-lookup"><span data-stu-id="b1ecf-274">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="b1ecf-275">Mit Codeausschnitten müssen Sie den Code zur Hand benötigten.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-275">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="b1ecf-276">Das Lab-Dokument informiert Sie genau wann sie verwendet werden kann wie in der folgenden Abbildung dargestellt.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-276">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="b1ecf-277">![Verwenden von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihrem Projekt](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "mithilfe von Visual Studio-Codeausschnitten, die Code in das Projekt einfügen.")</span><span class="sxs-lookup"><span data-stu-id="b1ecf-277">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="b1ecf-278">*Verwenden von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihrem Projekt*</span><span class="sxs-lookup"><span data-stu-id="b1ecf-278">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="b1ecf-279">***Hinzufügen ein Codeausschnitts, die über die Tastatur (nur c#)***</span><span class="sxs-lookup"><span data-stu-id="b1ecf-279">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="b1ecf-280">Platzieren Sie den Cursor, wo Sie möchten den Code einfügen.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-280">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="b1ecf-281">Starten Sie den codeausschnittsnamen (ohne Leerzeichen oder Bindestriche) eingeben.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-281">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="b1ecf-282">Beobachten Sie, wie IntelliSense zeigt übereinstimmende Codeausschnitte Namen.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-282">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="b1ecf-283">Wählen Sie den richtigen Codeausschnitt (oder halten Sie eingeben, bis die gesamte codeausschnittsnamen ausgewählt ist).</span><span class="sxs-lookup"><span data-stu-id="b1ecf-283">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="b1ecf-284">Drücken Sie die Tab-Taste zweimal auf Einfügen des Codeausschnitts an der Cursorposition ein.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-284">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="b1ecf-285">![Geben Sie den Namen des Ausschnitts](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "Geben Sie den Namen des Ausschnitts")</span><span class="sxs-lookup"><span data-stu-id="b1ecf-285">![Start typing the snippet name](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "Start typing the snippet name")</span></span>

<span data-ttu-id="b1ecf-286">*Geben Sie den Namen des Ausschnitts*</span><span class="sxs-lookup"><span data-stu-id="b1ecf-286">*Start typing the snippet name*</span></span>

<span data-ttu-id="b1ecf-287">![Tabstopp drücken, um den hervorgehobenen Codeausschnitt auswählen](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "Tab drücken, um den hervorgehobenen Codeausschnitt auswählen")</span><span class="sxs-lookup"><span data-stu-id="b1ecf-287">![Press Tab to select the highlighted snippet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="b1ecf-288">*Drücken Sie Tabstopp, um den hervorgehobenen Codeausschnitt auswählen*</span><span class="sxs-lookup"><span data-stu-id="b1ecf-288">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="b1ecf-289">![Drücken Sie die Tabulatortaste erneut, und der Ausschnitt erweitert](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "drücken Sie die Tabulatortaste erneut, und der Ausschnitt werden erweitert.")</span><span class="sxs-lookup"><span data-stu-id="b1ecf-289">![Press Tab again and the snippet will expand](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="b1ecf-290">*Drücken Sie die Tabulatortaste erneut, und der Ausschnitt werden erweitert.*</span><span class="sxs-lookup"><span data-stu-id="b1ecf-290">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="b1ecf-291">***Hinzufügen ein Codeausschnitts, die mit der Maus (c#, Visual Basic und XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-291">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="b1ecf-292">Mit der rechten Maustaste, in dem den Codeausschnitt eingefügt werden soll.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-292">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="b1ecf-293">Wählen Sie **Ausschnitt einfügen** gefolgt von **Meine Codeausschnitte**.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-293">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="b1ecf-294">Wählen Sie die relevante Codeausschnitte in der Liste, indem Sie darauf klicken.</span><span class="sxs-lookup"><span data-stu-id="b1ecf-294">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="b1ecf-295">![Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten,")</span><span class="sxs-lookup"><span data-stu-id="b1ecf-295">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="b1ecf-296">*Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten*</span><span class="sxs-lookup"><span data-stu-id="b1ecf-296">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="b1ecf-297">![Wählen Sie die relevante Codeausschnitte in der Liste, indem Sie darauf klicken](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "die relevante Codeausschnitte in der Liste auswählen, indem Sie darauf klicken")</span><span class="sxs-lookup"><span data-stu-id="b1ecf-297">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="b1ecf-298">*Wählen Sie die relevante Codeausschnitte in der Liste, indem Sie darauf klicken*</span><span class="sxs-lookup"><span data-stu-id="b1ecf-298">*Pick the relevant snippet from the list, by clicking on it*</span></span>
