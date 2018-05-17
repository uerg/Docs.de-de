---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
title: ASP.NET MVC 4-Modellen und Datenzugriff | Microsoft Docs
author: rick-anderson
description: 'Hinweis: Diese praktische Übungseinheit wird davon ausgegangen, dass Sie über grundlegende Kenntnisse von ASP.NET MVC haben. Wenn Sie nicht ASP.NET MVC, bevor Sie verwendet haben, empfehlen wir Sie ASP.NET MVC 4 besprochen...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 634ea84b-f904-4afe-b71b-49cccef4d9cc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
msc.type: authoredcontent
ms.openlocfilehash: 57477cf15bf6755523f28356d5384517bea24982
ms.sourcegitcommit: 5ae0c125ee3bbd324edef3818d1d160f4dd84602
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/17/2018
---
# <a name="aspnet-mvc-4-models-and-data-access"></a><span data-ttu-id="958a2-104">ASP.NET MVC 4-Modellen und Datenzugriff</span><span class="sxs-lookup"><span data-stu-id="958a2-104">ASP.NET MVC 4 Models and Data Access</span></span>

<span data-ttu-id="958a2-105">Durch [Web Lager Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="958a2-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="958a2-106">Herunterladen von Web-Lager Training Kit</span><span class="sxs-lookup"><span data-stu-id="958a2-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="958a2-107">Diese praktische Übungseinheit wird vorausgesetzt, dass grundlegende Kenntnisse im **ASP.NET-MVC**.</span><span class="sxs-lookup"><span data-stu-id="958a2-107">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="958a2-108">Wenn Sie nicht verwendet haben **ASP.NET-MVC** vorher, empfehlen wir Ihnen, durchlaufen **ASP.NET MVC 4-Grundlagen** praktische Übungseinheit.</span><span class="sxs-lookup"><span data-stu-id="958a2-108">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC 4 Fundamentals** Hands-on Lab.</span></span>

<span data-ttu-id="958a2-109">Diese Übung führt Sie durch die Optimierungen und neuen Funktionen, die zuvor beschriebenen durch Anwenden von kleinere Änderungen auf eine Beispielwebanwendung im Quellordner bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="958a2-109">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>

> [!NOTE]
> <span data-ttu-id="958a2-110">Alle Beispielcode und Codeausschnitte sind im Web Lager Training Kit unter enthalten [Versionen von Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="958a2-110">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="958a2-111">Das Projekt, das speziell für diese Übung finden Sie unter [ASP.NET MVC 4-Modellen und Datenzugriff](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess).</span><span class="sxs-lookup"><span data-stu-id="958a2-111">The project specific to this lab is available at [ASP.NET MVC 4 Models and Data Access](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess).</span></span>

<span data-ttu-id="958a2-112">In **ASP.NET MVC-Grundlagen** praktische Übungseinheit, Sie haben wurde übergeben hartcodierte Daten durch den Controller, um die Vorlagen anzeigen.</span><span class="sxs-lookup"><span data-stu-id="958a2-112">In **ASP.NET MVC Fundamentals** Hands-on Lab, you have been passing hard-coded data from the Controllers to the View templates.</span></span> <span data-ttu-id="958a2-113">Allerdings um eine echte Webanwendung zu erstellen, sollten Sie eine echte Datenbank verwendet.</span><span class="sxs-lookup"><span data-stu-id="958a2-113">But, in order to build a real Web application, you might want to use a real database.</span></span>

<span data-ttu-id="958a2-114">Diese praktische Übungseinheit wird gezeigt, wie ein Datenbankmodul zum Speichern und Abrufen der erforderlichen Daten für die Music Store-Anwendung verwenden.</span><span class="sxs-lookup"><span data-stu-id="958a2-114">This Hands-on Lab will show you how to use a database engine in order to store and retrieve the data needed for the Music Store application.</span></span> <span data-ttu-id="958a2-115">Um zu erreichen, dass Sie mit einer vorhandenen Datenbank startet und das Entity Data Model daraus erstellen.</span><span class="sxs-lookup"><span data-stu-id="958a2-115">To accomplish that, you will start with an existing database and create the Entity Data Model from it.</span></span> <span data-ttu-id="958a2-116">In dieser Übungseinheit erfüllen Sie die **Database First** Ansatz als auch die **Code First** Ansatz.</span><span class="sxs-lookup"><span data-stu-id="958a2-116">Throughout this lab, you will meet the **Database First** approach as well as the **Code First** approach.</span></span>

<span data-ttu-id="958a2-117">Aber Sie können auch die **Model First** Ansatz, erstellen Sie das gleiche Modell mit den Tools und die Datenbank dann daraus zu generieren.</span><span class="sxs-lookup"><span data-stu-id="958a2-117">However, you can also use the **Model First** approach, create the same model using the tools, and then generate the database from it.</span></span>

<span data-ttu-id="958a2-118">![Datenbank erste im Vergleich zu Model First](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First Vs. Model First")</span><span class="sxs-lookup"><span data-stu-id="958a2-118">![Database First vs. Model First](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First vs. Model First")</span></span>

<span data-ttu-id="958a2-119">*Datenbank erste im Vergleich zu Model First*</span><span class="sxs-lookup"><span data-stu-id="958a2-119">*Database First vs. Model First*</span></span>

<span data-ttu-id="958a2-120">Nach dem Generieren des Modells, sind Sie die entsprechenden Korrekturen der StoreController die Store-Ansichten mit den Daten aus der Datenbank, anstelle von hartcodierten Daten bereitstellen werden.</span><span class="sxs-lookup"><span data-stu-id="958a2-120">After generating the Model, you will make the proper adjustments in the StoreController to provide the Store Views with the data taken from the database, instead of using hard-coded data.</span></span> <span data-ttu-id="958a2-121">Sie müssen keine die Ansichtsvorlagen ändern, da die StoreController an den Vorlagen anzeigen, die gleichen ViewModels zurückgegeben werden sollen jedoch dieses Mal die Daten aus der Datenbank stammen.</span><span class="sxs-lookup"><span data-stu-id="958a2-121">You will not need to make any change to the View templates because the StoreController will be returning the same ViewModels to the View templates, although this time the data will come from the database.</span></span>

<span data-ttu-id="958a2-122">**Der erste Ansatz von Code**</span><span class="sxs-lookup"><span data-stu-id="958a2-122">**The Code First Approach**</span></span>

<span data-ttu-id="958a2-123">Der Code First-Ansatz erlaubt es uns so definieren Sie das Modell aus dem Code, ohne das Generieren von Klassen, die in der Regel mit dem Framework verbunden sind.</span><span class="sxs-lookup"><span data-stu-id="958a2-123">The Code First approach allows us to define the model from the code without generating classes that are generally coupled with the framework.</span></span>

<span data-ttu-id="958a2-124">Im Code zuerst Modellobjekte mit POCOs, definiert werden &quot;Plain Old CLR Objects&quot;.</span><span class="sxs-lookup"><span data-stu-id="958a2-124">In code first, model objects are defined with POCOs, &quot;Plain Old CLR Objects&quot;.</span></span> <span data-ttu-id="958a2-125">POCOs sind einfache einfache Klassen, die keine Vererbung und Schnittstellen nicht implementieren.</span><span class="sxs-lookup"><span data-stu-id="958a2-125">POCOs are simple plain classes that have no inheritance and do not implement interfaces.</span></span> <span data-ttu-id="958a2-126">Wir können die Datenbank automatisch generieren, von ihnen oder wir können eine vorhandene Datenbank verwenden und die Zuordnung für die Klasse aus dem Code zu generieren.</span><span class="sxs-lookup"><span data-stu-id="958a2-126">We can automatically generate the database from them, or we can use an existing database and generate the class mapping from the code.</span></span>

<span data-ttu-id="958a2-127">Die Vorteile der Verwendung dieser Ansatz ist, dass das Modell wie die POCOs Klassen nicht mit der Zuordnung Framework gekoppelt sind unabhängig von der Persistenz-Framework (in diesem Fall Entity Framework), bleibt.</span><span class="sxs-lookup"><span data-stu-id="958a2-127">The benefits of using this approach is that the Model remains independent from the persistence framework (in this case, Entity Framework), as the POCOs classes are not coupled with the mapping framework.</span></span>

> [!NOTE]
> <span data-ttu-id="958a2-128">In dieser Umgebung basiert auf ASP.NET MVC 4 und eine Version der beispielanwendung Music Store angepasst und nur die Funktionen, die in dieser praktischen Übungseinheit gezeigt entsprechend minimiert.</span><span class="sxs-lookup"><span data-stu-id="958a2-128">This Lab is based on ASP.NET MVC 4 and a version of the Music Store sample application customized and minimized to fit only the features shown in this Hands-On Lab.</span></span>
> 
> <span data-ttu-id="958a2-129">Wenn Sie die gesamte untersuchen möchten **Music Store** lernprogrammanwendung Sie in finden [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="958a2-129">If you wish to explore the whole **Music Store** tutorial application you can find it in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>


<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="958a2-130">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="958a2-130">Prerequisites</span></span>

<span data-ttu-id="958a2-131">Sie benötigen zum Abschließen dieser testumgebung die folgenden Elemente:</span><span class="sxs-lookup"><span data-stu-id="958a2-131">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="958a2-132">[Microsoft Visual Studio Express 2012 für das Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) oder sogar eine höhere (Lesen [Anhang A](#AppendixA) Anleitungen zur Installation).</span><span class="sxs-lookup"><span data-stu-id="958a2-132">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="958a2-133">Setup</span><span class="sxs-lookup"><span data-stu-id="958a2-133">Setup</span></span>

<span data-ttu-id="958a2-134">**Installieren von Codeausschnitten**</span><span class="sxs-lookup"><span data-stu-id="958a2-134">**Installing Code Snippets**</span></span>

<span data-ttu-id="958a2-135">Der Einfachheit halber gilt Großteil des Codes, den Sie entlang dieser Übung verwalten als Visual Studio-Codeausschnitte verfügbar.</span><span class="sxs-lookup"><span data-stu-id="958a2-135">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="958a2-136">So installieren Sie die Codeausschnitte ausführen **.\Source\Setup\CodeSnippets.vsi** Datei.</span><span class="sxs-lookup"><span data-stu-id="958a2-136">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="958a2-137">Wenn Sie nicht mit den Visual Studio-Codeausschnitte und zu erfahren, wie deren Verwendung vertraut sind, Sie können finden Sie im Anhang in diesem Dokument &quot; [Anhang "c:" mithilfe von Code Snippets](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="958a2-137">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="958a2-138">Übungen</span><span class="sxs-lookup"><span data-stu-id="958a2-138">Exercises</span></span>

<span data-ttu-id="958a2-139">Diese praktische Übungseinheit wird durch den folgenden Übungen umfasst:</span><span class="sxs-lookup"><span data-stu-id="958a2-139">This Hands-on Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="958a2-140">Übung 1: Hinzufügen einer Datenbank</span><span class="sxs-lookup"><span data-stu-id="958a2-140">Exercise 1: Adding a Database</span></span>](#Exercise1)
2. [<span data-ttu-id="958a2-141">Übung 2: Erstellen einer Datenbank mithilfe von Code First</span><span class="sxs-lookup"><span data-stu-id="958a2-141">Exercise 2: Creating a Database using Code First</span></span>](#Exercise2)
3. [<span data-ttu-id="958a2-142">Übung 3: Abfragen der Datenbank mit Parametern</span><span class="sxs-lookup"><span data-stu-id="958a2-142">Exercise 3: Querying the Database with Parameters</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="958a2-143">Jede Übung beiliegen ein **End** Ordner, der sich so ergebende Lösung, die Sie nach Abschluss der Übung abrufen soll.</span><span class="sxs-lookup"><span data-stu-id="958a2-143">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="958a2-144">Sie können diese Lösung als Leitfaden verwenden, ggf. zusätzliche Hilfe bei der Übungen durcharbeiten.</span><span class="sxs-lookup"><span data-stu-id="958a2-144">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="958a2-145">Geschätzte Zeit zum Abschließen dieser testumgebung: **35 Minuten**.</span><span class="sxs-lookup"><span data-stu-id="958a2-145">Estimated time to complete this lab: **35 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Adding_a_Database"></a>
### <a name="exercise-1-adding-a-database"></a><span data-ttu-id="958a2-146">Übung 1: Hinzufügen einer Datenbank</span><span class="sxs-lookup"><span data-stu-id="958a2-146">Exercise 1: Adding a Database</span></span>

<span data-ttu-id="958a2-147">In dieser Übung erfahren Sie, wie zum Hinzufügen einer Datenbank mit den Tabellen der Anwendung MusicStore der Projektmappe um die Daten zu nutzen.</span><span class="sxs-lookup"><span data-stu-id="958a2-147">In this exercise, you will learn how to add a database with the tables of the MusicStore application to the solution in order to consume its data.</span></span> <span data-ttu-id="958a2-148">Sobald die Datenbank mit dem Modell generiert, und der Projektmappe hinzugefügt, ändern Sie die StoreController-Klasse, um die Vorlage für die Ansicht mit den Daten aus der Datenbank, anstelle von hartcodierten Werte geben.</span><span class="sxs-lookup"><span data-stu-id="958a2-148">Once the database is generated with the model, and added to the solution, you will modify the StoreController class to provide the View template with the data taken from the database, instead of using hard-coded values.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Adding_a_Database"></a>
#### <a name="task-1---adding-a-database"></a><span data-ttu-id="958a2-149">Aufgabe 1: Hinzufügen einer Datenbank</span><span class="sxs-lookup"><span data-stu-id="958a2-149">Task 1 - Adding a Database</span></span>

<span data-ttu-id="958a2-150">In dieser Aufgabe fügen Sie eine bereits erstellte Datenbank mit den wichtigsten Tabellen der Anwendung MusicStore zur Projektmappe hinzu.</span><span class="sxs-lookup"><span data-stu-id="958a2-150">In this task, you will add an already created database with the main tables of the MusicStore application to the solution.</span></span>

1. <span data-ttu-id="958a2-151">Öffnen der **beginnen** Lösung finden Sie unter **Quell-/Ex1-AddingADatabaseDBFirst/Begin/** Ordner.</span><span class="sxs-lookup"><span data-stu-id="958a2-151">Open the **Begin** solution located at **Source/Ex1-AddingADatabaseDBFirst/Begin/** folder.</span></span>

   1. <span data-ttu-id="958a2-152">Sie müssen einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren.</span><span class="sxs-lookup"><span data-stu-id="958a2-152">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="958a2-153">Klicken Sie hierzu auf die **Projekt** Menü **NuGet-Pakete verwalten**.</span><span class="sxs-lookup"><span data-stu-id="958a2-153">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="958a2-154">In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.</span><span class="sxs-lookup"><span data-stu-id="958a2-154">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="958a2-155">Schließlich erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.</span><span class="sxs-lookup"><span data-stu-id="958a2-155">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="958a2-156">Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken, die sich in Ihrem Projekt liefern Verringern der Projektgröße.</span><span class="sxs-lookup"><span data-stu-id="958a2-156">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="958a2-157">Mit NuGet-Powertools werden durch Angabe der Paketversionen in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, wenn, das Sie das Projekt ausführen, können.</span><span class="sxs-lookup"><span data-stu-id="958a2-157">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="958a2-158">Deshalb wird müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit.</span><span class="sxs-lookup"><span data-stu-id="958a2-158">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="958a2-159">Hinzufügen **MvcMusicStore** Datenbankdatei.</span><span class="sxs-lookup"><span data-stu-id="958a2-159">Add **MvcMusicStore** database file.</span></span> <span data-ttu-id="958a2-160">In dieser praktischen Übungseinheit verwenden Sie eine bereits erstellte Datenbank mit dem Namen **MvcMusicStore.mdf**.</span><span class="sxs-lookup"><span data-stu-id="958a2-160">In this Hands-on Lab, you will use an already created database called **MvcMusicStore.mdf**.</span></span> <span data-ttu-id="958a2-161">Zu diesem Zweck Maustaste **App\_Daten** Ordner, zeigen Sie auf **hinzufügen** , und klicken Sie dann auf **vorhandenes Element**.</span><span class="sxs-lookup"><span data-stu-id="958a2-161">To do that, right-click **App\_Data** folder, point to **Add** and then click **Existing Item**.</span></span> <span data-ttu-id="958a2-162">Navigieren Sie zu **\Source\Assets** , und wählen Sie die **MvcMusicStore.mdf** Datei.</span><span class="sxs-lookup"><span data-stu-id="958a2-162">Browse to **\Source\Assets** and select the **MvcMusicStore.mdf** file.</span></span>

    <span data-ttu-id="958a2-163">![Hinzufügen eines vorhandenen Elements](aspnet-mvc-4-models-and-data-access/_static/image2.png "ein vorhandenes Element hinzufügen")</span><span class="sxs-lookup"><span data-stu-id="958a2-163">![Adding an Existing Item](aspnet-mvc-4-models-and-data-access/_static/image2.png "Adding an Existing Item")</span></span>

    <span data-ttu-id="958a2-164">*Ein vorhandenes Element hinzufügen*</span><span class="sxs-lookup"><span data-stu-id="958a2-164">*Adding an Existing Item*</span></span>

    <span data-ttu-id="958a2-165">![Datenbankdatei MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf-Datenbankdatei")</span><span class="sxs-lookup"><span data-stu-id="958a2-165">![MvcMusicStore.mdf database file](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf database file")</span></span>

    <span data-ttu-id="958a2-166">*MvcMusicStore.mdf-Datenbankdatei*</span><span class="sxs-lookup"><span data-stu-id="958a2-166">*MvcMusicStore.mdf database file*</span></span>

    <span data-ttu-id="958a2-167">Die Datenbank wurde dem Projekt hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="958a2-167">The database has been added to the project.</span></span> <span data-ttu-id="958a2-168">Selbst wenn die Datenbank innerhalb der Lösung befindet, können Sie Abfragen und aktualisieren Sie es an, wie er in einen anderen Datenbankserver gehostet wurde.</span><span class="sxs-lookup"><span data-stu-id="958a2-168">Even when the database is located inside the solution, you can query and update it as it was hosted in a different database server.</span></span>

    <span data-ttu-id="958a2-169">![MvcMusicStore Datenbank im Projektmappen-Explorer](aspnet-mvc-4-models-and-data-access/_static/image4.png "MvcMusicStore Datenbank im Projektmappen-Explorer")</span><span class="sxs-lookup"><span data-stu-id="958a2-169">![MvcMusicStore database in Solution Explorer](aspnet-mvc-4-models-and-data-access/_static/image4.png "MvcMusicStore database in Solution Explorer")</span></span>

    <span data-ttu-id="958a2-170">*MvcMusicStore Datenbank im Projektmappen-Explorer*</span><span class="sxs-lookup"><span data-stu-id="958a2-170">*MvcMusicStore database in Solution Explorer*</span></span>
3. <span data-ttu-id="958a2-171">Überprüfen Sie die Verbindung mit der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="958a2-171">Verify the connection to the database.</span></span> <span data-ttu-id="958a2-172">Doppelklicken Sie hierzu auf **MvcMusicStore.mdf** zum Herstellen einer Verbindung.</span><span class="sxs-lookup"><span data-stu-id="958a2-172">To do this, double-click **MvcMusicStore.mdf** to establish a connection.</span></span>

    <span data-ttu-id="958a2-173">![Herstellen einer Verbindung mit MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "Herstellen einer Verbindung mit MvcMusicStore.mdf")</span><span class="sxs-lookup"><span data-stu-id="958a2-173">![Connecting to MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "Connecting to MvcMusicStore.mdf")</span></span>

    <span data-ttu-id="958a2-174">*Herstellen einer Verbindung mit MvcMusicStore.mdf*</span><span class="sxs-lookup"><span data-stu-id="958a2-174">*Connecting to MvcMusicStore.mdf*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_a_Data_Model"></a>
#### <a name="task-2---creating-a-data-model"></a><span data-ttu-id="958a2-175">Aufgabe 2: erstellen ein Datenmodell</span><span class="sxs-lookup"><span data-stu-id="958a2-175">Task 2 - Creating a Data Model</span></span>

<span data-ttu-id="958a2-176">In dieser Aufgabe erstellen Sie ein Datenmodell, um die Interaktion mit der Datenbank, die in der vorherigen Aufgabe hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="958a2-176">In this task, you will create a data model to interact with the database added in the previous task.</span></span>

1. <span data-ttu-id="958a2-177">Erstellen Sie ein Datenmodell, die die Datenbank darstellen.</span><span class="sxs-lookup"><span data-stu-id="958a2-177">Create a data model that will represent the database.</span></span> <span data-ttu-id="958a2-178">Klicken Sie dazu im Projektmappen-Explorer mit der rechten Maustaste die **Modelle** Ordner, zeigen Sie auf **hinzufügen** , und klicken Sie dann auf **neues Element**.</span><span class="sxs-lookup"><span data-stu-id="958a2-178">To do this, in Solution Explorer right-click the **Models** folder, point to **Add** and then click **New Item**.</span></span> <span data-ttu-id="958a2-179">In der **neues Element hinzufügen** wählen Sie im Dialogfeld die **Daten** Vorlage und dann die **ADO.NET Entity Data Model** Element.</span><span class="sxs-lookup"><span data-stu-id="958a2-179">In the **Add New Item** dialog, select the **Data** template and then the **ADO.NET Entity Data Model** item.</span></span> <span data-ttu-id="958a2-180">Ändern Sie den Namen des Daten-Modell zu **StoreDB.edmx** , und klicken Sie auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="958a2-180">Change the data model name to **StoreDB.edmx** and click **Add**.</span></span>

    <span data-ttu-id="958a2-181">![Hinzufügen von StoreDB ADO.NET Entity Data Model](aspnet-mvc-4-models-and-data-access/_static/image6.png "StoreDB ADO.NET Entity Data Model hinzufügen")</span><span class="sxs-lookup"><span data-stu-id="958a2-181">![Adding the StoreDB ADO.NET Entity Data Model](aspnet-mvc-4-models-and-data-access/_static/image6.png "Adding the StoreDB ADO.NET Entity Data Model")</span></span>

    <span data-ttu-id="958a2-182">*Hinzufügen von StoreDB ADO.NET Entity Data Model*</span><span class="sxs-lookup"><span data-stu-id="958a2-182">*Adding the StoreDB ADO.NET Entity Data Model*</span></span>
2. <span data-ttu-id="958a2-183">Die **Entity Data Model-Assistenten** wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="958a2-183">The **Entity Data Model Wizard** will appear.</span></span> <span data-ttu-id="958a2-184">Dieser Assistent führt Sie durch die Erstellung der Modellebene.</span><span class="sxs-lookup"><span data-stu-id="958a2-184">This wizard will guide you through the creation of the model layer.</span></span> <span data-ttu-id="958a2-185">Da das Modell basierend auf der vorhandenen Datenbank Recentyl hinzugefügt erstellt werden soll, wählen **aus Datenbank generieren** , und klicken Sie auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="958a2-185">Since the model should be created based on the existing database recentyl added, select **Generate from database** and click **Next**.</span></span>

    <span data-ttu-id="958a2-186">![Auswählen des Modellinhalts](aspnet-mvc-4-models-and-data-access/_static/image7.png "den Modellinhalt auswählen")</span><span class="sxs-lookup"><span data-stu-id="958a2-186">![Choosing the model content](aspnet-mvc-4-models-and-data-access/_static/image7.png "Choosing the model content")</span></span>

    <span data-ttu-id="958a2-187">*Den Modellinhalt auswählen*</span><span class="sxs-lookup"><span data-stu-id="958a2-187">*Choosing the model content*</span></span>
3. <span data-ttu-id="958a2-188">Da zum Generieren eines Modells aus einer Datenbank müssen Sie die zu verwendende Verbindung an.</span><span class="sxs-lookup"><span data-stu-id="958a2-188">Since you are generating a model from a database, you will need to specify the connection to use.</span></span> <span data-ttu-id="958a2-189">Klicken Sie auf **neue Verbindung**.</span><span class="sxs-lookup"><span data-stu-id="958a2-189">Click **New Connection**.</span></span>
4. <span data-ttu-id="958a2-190">Wählen Sie **Microsoft SQL Server-Datenbankdatei** , und klicken Sie auf **Fortfahren**.</span><span class="sxs-lookup"><span data-stu-id="958a2-190">Select **Microsoft SQL Server Database File** and click **Continue**.</span></span>

    <span data-ttu-id="958a2-191">![Wählen Sie die Datenquelle](aspnet-mvc-4-models-and-data-access/_static/image8.png "Datenquelle auswählen")</span><span class="sxs-lookup"><span data-stu-id="958a2-191">![Choose data source](aspnet-mvc-4-models-and-data-access/_static/image8.png "Choose data source")</span></span>

    <span data-ttu-id="958a2-192">*Dialogfeld "Quelle" Daten auswählen*</span><span class="sxs-lookup"><span data-stu-id="958a2-192">*Choose data source dialog*</span></span>
5. <span data-ttu-id="958a2-193">Klicken Sie auf **Durchsuchen** und wählen Sie die Datenbank **MvcMusicStore.mdf** Sie befindet sich der **App\_Daten** Ordner, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="958a2-193">Click **Browse** and select the database **MvcMusicStore.mdf** you located in the **App\_Data** folder and click **OK**.</span></span>

    <span data-ttu-id="958a2-194">![Verbindungseigenschaften](aspnet-mvc-4-models-and-data-access/_static/image9.png "Verbindungseigenschaften")</span><span class="sxs-lookup"><span data-stu-id="958a2-194">![Connection properties](aspnet-mvc-4-models-and-data-access/_static/image9.png "Connection properties")</span></span>

    <span data-ttu-id="958a2-195">*Verbindungseigenschaften*</span><span class="sxs-lookup"><span data-stu-id="958a2-195">*Connection properties*</span></span>
6. <span data-ttu-id="958a2-196">Die generierte Klasse sollte haben den gleichen Namen wie die Verbindungszeichenfolge für die Entität, so ändern Sie den Namen in **MusicStoreEntities** , und klicken Sie auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="958a2-196">The generated class should have the same name as the entity connection string, so change its name to **MusicStoreEntities** and click **Next**.</span></span>

    <span data-ttu-id="958a2-197">![Wählen die Datenverbindung](aspnet-mvc-4-models-and-data-access/_static/image10.png "die Datenverbindung auswählen")</span><span class="sxs-lookup"><span data-stu-id="958a2-197">![Choosing the data connection](aspnet-mvc-4-models-and-data-access/_static/image10.png "Choosing the data connection")</span></span>

    <span data-ttu-id="958a2-198">*Die Datenverbindung auswählen*</span><span class="sxs-lookup"><span data-stu-id="958a2-198">*Choosing the data connection*</span></span>
7. <span data-ttu-id="958a2-199">Wählen Sie die Datenbankobjekte zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="958a2-199">Choose the database objects to use.</span></span> <span data-ttu-id="958a2-200">Wie das Entitätsmodell nur die Tabellen der Datenbank verwenden, wählen Sie die **Tabellen** aus, und stellen Sie sicher, dass die **Fremdschlüsselspalten in das Modell einbeziehen** und **in den Singular oder Plural setzen generiert Objektnamen** -Optionen sind auch ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="958a2-200">As the Entity Model will use just the database's tables, select the **Tables** option, and make sure that the **Include foreign key columns in the model** and **Pluralize or singularize generated object names** options are also selected.</span></span> <span data-ttu-id="958a2-201">Ändern Sie den Modell-Namespace zu **MvcMusicStore.Model** , und klicken Sie auf **Fertig stellen**.</span><span class="sxs-lookup"><span data-stu-id="958a2-201">Change the Model Namespace to **MvcMusicStore.Model** and click **Finish**.</span></span>

    <span data-ttu-id="958a2-202">![Wählen die Datenbankobjekte](aspnet-mvc-4-models-and-data-access/_static/image11.png "Datenbankobjekte auswählen")</span><span class="sxs-lookup"><span data-stu-id="958a2-202">![Choosing the database objects](aspnet-mvc-4-models-and-data-access/_static/image11.png "Choosing the database objects")</span></span>

    <span data-ttu-id="958a2-203">*Datenbankobjekte auswählen*</span><span class="sxs-lookup"><span data-stu-id="958a2-203">*Choosing the database objects*</span></span>

    > [!NOTE]
    > <span data-ttu-id="958a2-204">Wenn eine Sicherheitswarnungs-Dialogfeld angezeigt wird, klicken Sie auf **OK** führen Sie die Vorlage und die Klassen für die Modellelemente zu generieren.</span><span class="sxs-lookup"><span data-stu-id="958a2-204">If a Security Warning dialog is shown, click **OK** to run the template and generate the classes for the model entities.</span></span>
8. <span data-ttu-id="958a2-205">Eine Entitätsdiagramm für die Datenbank wird angezeigt, während eine separate Klasse, die ordnet jede Tabelle in der Datenbank erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="958a2-205">An entity diagram for the database will appear, while a separate class that maps each table to the database will be created.</span></span> <span data-ttu-id="958a2-206">Z. B. die **Alben** Tabelle dargestellte ein **Album** -Klasse, in dem jede Spalte in der Tabelle an Klasseneigenschaften zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="958a2-206">For example, the **Albums** table will be represented by an **Album** class, where each column in the table will map to a class property.</span></span> <span data-ttu-id="958a2-207">Dadurch können Sie Abfragen und Arbeiten mit Objekten, die Zeilen in der Datenbank darstellen.</span><span class="sxs-lookup"><span data-stu-id="958a2-207">This will allow you to query and work with objects that represent rows in the database.</span></span>

    <span data-ttu-id="958a2-208">![Entitätsdiagramm](aspnet-mvc-4-models-and-data-access/_static/image12.png "Entität-Diagramm")</span><span class="sxs-lookup"><span data-stu-id="958a2-208">![Entity diagram](aspnet-mvc-4-models-and-data-access/_static/image12.png "Entity diagram")</span></span>

    <span data-ttu-id="958a2-209">*Entitätsdiagramm*</span><span class="sxs-lookup"><span data-stu-id="958a2-209">*Entity diagram*</span></span>

    > [!NOTE]
    > <span data-ttu-id="958a2-210">Die T4-Vorlagen (TT) Code ausführen, um die Entitäten Klassen zu generieren und überschreibt die vorhandenen Klassen mit dem gleichen Namen.</span><span class="sxs-lookup"><span data-stu-id="958a2-210">The T4 templates (.tt) run code to generate the entities classes and will overwrite the existing classes with the same name.</span></span> <span data-ttu-id="958a2-211">In diesem Beispiel die Klassen &quot;Album&quot;, &quot;"Genre"&quot; und &quot;Interpreten&quot; durch den generierten Code außer Kraft gesetzt wurden.</span><span class="sxs-lookup"><span data-stu-id="958a2-211">In this example, the classes &quot;Album&quot;, &quot;Genre&quot; and &quot;Artist&quot; were overwritten with the generated code.</span></span>


<a id="Ex1Task3"></a>

<a id="Task_3_-_Building_the_Application"></a>
#### <a name="task-3---building-the-application"></a><span data-ttu-id="958a2-212">Aufgabe 3: Erstellen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="958a2-212">Task 3 - Building the Application</span></span>

<span data-ttu-id="958a2-213">In dieser Aufgabe werden überprüfen Sie, dass zwar die modellgenerierung wurden entfernt. die **Album**, **"Genre"** und **Interpreten** Modellklassen, das Projekt erfolgreich erstellt mit das neue Modell Datenklassen.</span><span class="sxs-lookup"><span data-stu-id="958a2-213">In this task, you will check that, although the model generation have removed the **Album**, **Genre** and **Artist** model classes, the project builds successfully by using the new data model classes.</span></span>

1. <span data-ttu-id="958a2-214">Erstellen des Projekts durch Auswahl der **erstellen** Menüelement und dann **MvcMusicStore erstellen**.</span><span class="sxs-lookup"><span data-stu-id="958a2-214">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>

    <span data-ttu-id="958a2-215">![Beim Erstellen des Projekts](aspnet-mvc-4-models-and-data-access/_static/image13.png "beim Erstellen des Projekts")</span><span class="sxs-lookup"><span data-stu-id="958a2-215">![Building the project](aspnet-mvc-4-models-and-data-access/_static/image13.png "Building the project")</span></span>

    <span data-ttu-id="958a2-216">*Beim Erstellen des Projekts*</span><span class="sxs-lookup"><span data-stu-id="958a2-216">*Building the project*</span></span>
2. <span data-ttu-id="958a2-217">Das Projekt erstellt erfolgreich.</span><span class="sxs-lookup"><span data-stu-id="958a2-217">The project builds successfully.</span></span> <span data-ttu-id="958a2-218">Warum ist es weiterhin funktionsfähig?</span><span class="sxs-lookup"><span data-stu-id="958a2-218">Why does it still work?</span></span> <span data-ttu-id="958a2-219">Es funktioniert, da die Datenbanktabellen Felder verfügen, die die Eigenschaften, die Sie verwendet haben, in den entfernten Klassen **Album** und **"Genre"**.</span><span class="sxs-lookup"><span data-stu-id="958a2-219">It works because the database tables have fields that include the properties that you were using in the removed classes **Album** and **Genre**.</span></span>

    <span data-ttu-id="958a2-220">![Builds erfolgreich](aspnet-mvc-4-models-and-data-access/_static/image14.png "Builds erfolgreich war")</span><span class="sxs-lookup"><span data-stu-id="958a2-220">![Builds succeeded](aspnet-mvc-4-models-and-data-access/_static/image14.png "Builds succeeded")</span></span>

    <span data-ttu-id="958a2-221">*Builds erfolgreich war*</span><span class="sxs-lookup"><span data-stu-id="958a2-221">*Builds succeeded*</span></span>
3. <span data-ttu-id="958a2-222">Während der Designer die Entitäten in einem Diagramm Format angezeigt wird, sind sie wirklich über C#-Klassen.</span><span class="sxs-lookup"><span data-stu-id="958a2-222">While the designer displays the entities in a diagram format, they are really C# classes.</span></span> <span data-ttu-id="958a2-223">Erweitern Sie die **StoreDB.edmx** Knoten im Projektmappen-Explorer und dann **StoreDB.tt**, sehen Sie die neue erzeugten Entitäten.</span><span class="sxs-lookup"><span data-stu-id="958a2-223">Expand the **StoreDB.edmx** node in the Solution Explorer and then **StoreDB.tt**, you will see the new generated entities.</span></span>

    <span data-ttu-id="958a2-224">![Generierte Dateien](aspnet-mvc-4-models-and-data-access/_static/image15.png "generierte Dateien")</span><span class="sxs-lookup"><span data-stu-id="958a2-224">![Generated files](aspnet-mvc-4-models-and-data-access/_static/image15.png "Generated files")</span></span>

    <span data-ttu-id="958a2-225">*Generierte Dateien*</span><span class="sxs-lookup"><span data-stu-id="958a2-225">*Generated files*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a><span data-ttu-id="958a2-226">Aufgabe 4: Abfragen der Datenbank</span><span class="sxs-lookup"><span data-stu-id="958a2-226">Task 4 - Querying the Database</span></span>

<span data-ttu-id="958a2-227">In dieser Aufgabe aktualisieren Sie die StoreController-Klasse, anstatt Sie hartcodierte Daten verwenden, es die Datenbank zum Abruf der Informationen abfragt.</span><span class="sxs-lookup"><span data-stu-id="958a2-227">In this task, you will update the StoreController class so that, instead of using hardcoded data, it will query the database to retrieve the information.</span></span>

1. <span data-ttu-id="958a2-228">Open **Controllers\StoreController.cs** und fügen Sie das folgende Feld der Klasse zum Halten von einer Instanz von der **MusicStoreEntities** Klasse, mit dem Namen **StoreDB**:</span><span class="sxs-lookup"><span data-stu-id="958a2-228">Open **Controllers\StoreController.cs** and add the following field to the class to hold an instance of the **MusicStoreEntities** class, named **storeDB**:</span></span>

    <span data-ttu-id="958a2-229">(Codeausschnitt - *Modelle und des Datenzugriffs - Ex1 StoreDB*)</span><span class="sxs-lookup"><span data-stu-id="958a2-229">(Code Snippet - *Models And Data Access - Ex1 storeDB*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample1.cs)]
~~~
2. <span data-ttu-id="958a2-230">Die **MusicStoreEntities** Klasse macht eine Auflistungseigenschaft für jede Tabelle in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="958a2-230">The **MusicStoreEntities** class exposes a collection property for each table in the database.</span></span> <span data-ttu-id="958a2-231">Update **Durchsuchen** Aktionsmethode einen "Genre" mit allen Abrufen der **Alben**.</span><span class="sxs-lookup"><span data-stu-id="958a2-231">Update **Browse** action method to retrieve a Genre with all of the **Albums**.</span></span>

    <span data-ttu-id="958a2-232">(Codeausschnitt - *Modelle und Datenzugriff - Ex1 Store durchsuchen*)</span><span class="sxs-lookup"><span data-stu-id="958a2-232">(Code Snippet - *Models And Data Access - Ex1 Store Browse*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample2.cs)]
~~~

> [!NOTE]
> <span data-ttu-id="958a2-233">Verwenden Sie eine Funktion von .NET aufgerufen **LINQ** (Language-integrated Query), stark typisierte Abfrageausdrücke anhand dieser Sammlungen - schreiben Code für die Datenbank ausgeführt und zurückgegeben-Objekten, die Sie programmieren können vor.</span><span class="sxs-lookup"><span data-stu-id="958a2-233">You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.</span></span>
> 
> <span data-ttu-id="958a2-234">Weitere Informationen über LINQ finden Sie auf der [Msdn-Website](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).</span><span class="sxs-lookup"><span data-stu-id="958a2-234">For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).</span></span>


3. <span data-ttu-id="958a2-235">Update **Index** Aktionsmethode, um alle Genres abzurufen.</span><span class="sxs-lookup"><span data-stu-id="958a2-235">Update **Index** action method to retrieve all the genres.</span></span>

    <span data-ttu-id="958a2-236">(Codeausschnitt - *Modelle und Datenzugriff - Ex1 Columnstore-Index*)</span><span class="sxs-lookup"><span data-stu-id="958a2-236">(Code Snippet - *Models And Data Access - Ex1 Store Index*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample3.cs)]
~~~
4. <span data-ttu-id="958a2-237">Update **Index** Aktionsmethode alle Genres abrufen und Transformieren von der Auflistung auf eine Liste.</span><span class="sxs-lookup"><span data-stu-id="958a2-237">Update **Index** action method to retrieve all the genres and transform the collection to a list.</span></span>

    <span data-ttu-id="958a2-238">(Codeausschnitt - *Modelle und Datenzugriff - Ex1 Store GenreMenu*)</span><span class="sxs-lookup"><span data-stu-id="958a2-238">(Code Snippet - *Models And Data Access - Ex1 Store GenreMenu*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample4.cs)]
~~~

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="958a2-239">Aufgabe 5: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="958a2-239">Task 5 - Running the Application</span></span>

<span data-ttu-id="958a2-240">In dieser Aufgabe überprüfen Sie, dass die Columnstore-Index-Seite die in der Datenbank anstelle der hartcodiert diejenigen gespeicherten Genres jetzt angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="958a2-240">In this task, you will check that the Store Index page will now display the Genres stored in the database instead of the hardcoded ones.</span></span> <span data-ttu-id="958a2-241">Besteht keine Notwendigkeit zum Ändern der Vorlage anzeigen, da die **StoreController** ist Zurückgeben der gleichen Entitäten wie zuvor zwar diesmal die Daten aus der Datenbank stammen.</span><span class="sxs-lookup"><span data-stu-id="958a2-241">There is no need to change the View template because the **StoreController** is returning the same entities as before, although this time the data will come from the database.</span></span>

1. <span data-ttu-id="958a2-242">Erneutes Erstellen von Projektmappen und drücken Sie **F5** um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="958a2-242">Rebuild the solution and press **F5** to run the Application.</span></span>
2. <span data-ttu-id="958a2-243">Das Projekt wird auf der Startseite gestartet.</span><span class="sxs-lookup"><span data-stu-id="958a2-243">The project starts in the Home page.</span></span> <span data-ttu-id="958a2-244">Überprüfen Sie, ob das Menü des **Genres** ist nicht mehr eine hartcodierte Liste, und die Daten direkt aus der Datenbank abgerufen.</span><span class="sxs-lookup"><span data-stu-id="958a2-244">Verify that the menu of **Genres** is no longer a hardcoded list, and the data is directly retrieved from the database.</span></span>

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image16.png)

    <span data-ttu-id="958a2-246">*Durchsuchen von Genres aus der Datenbank*</span><span class="sxs-lookup"><span data-stu-id="958a2-246">*Browsing Genres from the database*</span></span>
3. <span data-ttu-id="958a2-247">Jetzt alle "Genre" und stellen Sie sicher, dass die Alben aus der Datenbank aufgefüllt werden.</span><span class="sxs-lookup"><span data-stu-id="958a2-247">Now browse to any genre and verify the albums are populated from database.</span></span>

    <span data-ttu-id="958a2-248">![Durchsuchen von Alben aus der Datenbank](aspnet-mvc-4-models-and-data-access/_static/image17.png "durchsuchen Alben aus der Datenbank")</span><span class="sxs-lookup"><span data-stu-id="958a2-248">![Browsing Albums from the database](aspnet-mvc-4-models-and-data-access/_static/image17.png "Browsing Albums from the database")</span></span>

    <span data-ttu-id="958a2-249">*Durchsuchen von Alben aus der Datenbank*</span><span class="sxs-lookup"><span data-stu-id="958a2-249">*Browsing Albums from the database*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Database_Using_Code_First"></a>
### <a name="exercise-2-creating-a-database-using-code-first"></a><span data-ttu-id="958a2-250">Übung 2: Erstellen einer Datenbank zunächst mithilfe von Code</span><span class="sxs-lookup"><span data-stu-id="958a2-250">Exercise 2: Creating a Database Using Code First</span></span>

<span data-ttu-id="958a2-251">In dieser Übung erfahren Sie, wie die Code First-Ansatz zu verwenden, um eine Datenbank mit den Tabellen der MusicStore Anwendung erstellen und zum Zugreifen auf ihre Daten.</span><span class="sxs-lookup"><span data-stu-id="958a2-251">In this exercise, you will learn how to use the Code First approach to create a database with the tables of the MusicStore application, and how to access its data.</span></span>

<span data-ttu-id="958a2-252">Nachdem das Modell generiert wird, ändern Sie die StoreController, um die Vorlage für die Ansicht mit den Daten aus der Datenbank, anstelle von hartcodierten Werte bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="958a2-252">Once the model is generated, you will modify the StoreController to provide the View template with the data taken from the database, instead of using hardcoded values.</span></span>

> [!NOTE]
> <span data-ttu-id="958a2-253">Wenn Sie Übung 1 abgeschlossen haben und bereits mit dem Database First-Ansatz funktioniert haben, erfahren Sie jetzt die gleichen Ergebnisse mit einem anderen Prozess abrufen.</span><span class="sxs-lookup"><span data-stu-id="958a2-253">If you have completed Exercise 1 and have already worked with the Database First approach, you will now learn how to get the same results with a different process.</span></span> <span data-ttu-id="958a2-254">Aufgaben, die auch in zahlreichen Übung 1 sind wurden gekennzeichnet, um die Lesbarkeit zu erhöhen.</span><span class="sxs-lookup"><span data-stu-id="958a2-254">The tasks that are in common with Exercise 1 have been marked to make your reading easier.</span></span> <span data-ttu-id="958a2-255">Wenn Sie Übung 1 wurden nicht abgeschlossen, aber die Code First-Ansatz erfahren möchten, können Sie aus dieser Übung beginnen und erhalten eine vollständige Abdeckung des Themas.</span><span class="sxs-lookup"><span data-stu-id="958a2-255">If you have not completed Exercise 1 but would like to learn the Code First approach, you can start from this exercise and get a full coverage of the topic.</span></span>


<a id="Ex2Task1"></a>

<a id="Task_1_-_Populating_Sample_Data"></a>
#### <a name="task-1---populating-sample-data"></a><span data-ttu-id="958a2-256">Aufgabe 1: Auffüllen von Beispieldaten</span><span class="sxs-lookup"><span data-stu-id="958a2-256">Task 1 - Populating Sample Data</span></span>

<span data-ttu-id="958a2-257">In dieser Aufgabe werden Sie die Datenbank mit Beispieldaten aufgefüllt, wenn sie mithilfe von Code First gestufte erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="958a2-257">In this task, you will populate the database with sample data when it is intially created using Code-First.</span></span>

1. <span data-ttu-id="958a2-258">Öffnen der **beginnen** Lösung finden Sie unter **Quell-/Ex2-CreatingADatabaseCodeFirst/Begin/** Ordner.</span><span class="sxs-lookup"><span data-stu-id="958a2-258">Open the **Begin** solution located at **Source/Ex2-CreatingADatabaseCodeFirst/Begin/** folder.</span></span> <span data-ttu-id="958a2-259">Andernfalls möglicherweise weiterhin mithilfe der **End** Lösung abgerufen, von der vorherigen Übung abschließen.</span><span class="sxs-lookup"><span data-stu-id="958a2-259">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="958a2-260">Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung müssen Sie einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren.</span><span class="sxs-lookup"><span data-stu-id="958a2-260">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="958a2-261">Klicken Sie hierzu auf die **Projekt** Menü **NuGet-Pakete verwalten**.</span><span class="sxs-lookup"><span data-stu-id="958a2-261">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="958a2-262">In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.</span><span class="sxs-lookup"><span data-stu-id="958a2-262">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="958a2-263">Schließlich erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.</span><span class="sxs-lookup"><span data-stu-id="958a2-263">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="958a2-264">Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken, die sich in Ihrem Projekt liefern Verringern der Projektgröße.</span><span class="sxs-lookup"><span data-stu-id="958a2-264">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="958a2-265">Mit NuGet-Powertools werden durch Angabe der Paketversionen in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, wenn, das Sie das Projekt ausführen, können.</span><span class="sxs-lookup"><span data-stu-id="958a2-265">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="958a2-266">Deshalb wird müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit.</span><span class="sxs-lookup"><span data-stu-id="958a2-266">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="958a2-267">Hinzufügen der **SampleData.cs** Datei wird in der **Modelle** Ordner.</span><span class="sxs-lookup"><span data-stu-id="958a2-267">Add the **SampleData.cs** file to the **Models** folder.</span></span> <span data-ttu-id="958a2-268">Zu diesem Zweck Maustaste **Modelle** Ordner, zeigen Sie auf **hinzufügen** , und klicken Sie dann auf **vorhandenes Element**.</span><span class="sxs-lookup"><span data-stu-id="958a2-268">To do that, right-click **Models** folder, point to **Add** and then click **Existing Item**.</span></span> <span data-ttu-id="958a2-269">Navigieren Sie zu **\Source\Assets** , und wählen Sie die **SampleData.cs** Datei.</span><span class="sxs-lookup"><span data-stu-id="958a2-269">Browse to **\Source\Assets** and select the **SampleData.cs** file.</span></span>

    <span data-ttu-id="958a2-270">![Beispieldaten aufzufüllen Code](aspnet-mvc-4-models-and-data-access/_static/image18.png "Beispieldaten aufzufüllen, Code")</span><span class="sxs-lookup"><span data-stu-id="958a2-270">![Sample data populate code](aspnet-mvc-4-models-and-data-access/_static/image18.png "Sample data populate code")</span></span>

    <span data-ttu-id="958a2-271">*Beispieldaten aufzufüllen, code*</span><span class="sxs-lookup"><span data-stu-id="958a2-271">*Sample data populate code*</span></span>
3. <span data-ttu-id="958a2-272">Öffnen der **Global.asax.cs** Datei, und fügen Sie die folgenden *mit* Anweisungen.</span><span class="sxs-lookup"><span data-stu-id="958a2-272">Open the **Global.asax.cs** file and add the following *using* statements.</span></span>

    <span data-ttu-id="958a2-273">(Codeausschnitt - *Modelle und Datenzugriff - Ex2 globale Asax Using-Direktiven*)</span><span class="sxs-lookup"><span data-stu-id="958a2-273">(Code Snippet - *Models And Data Access - Ex2 Global Asax Usings*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample5.cs)]
~~~
4. <span data-ttu-id="958a2-274">In der **Anwendung\_Start()** Methode fügen Sie die folgende Zeile zum Festlegen der datenbankinitialisierers hinzu.</span><span class="sxs-lookup"><span data-stu-id="958a2-274">In the **Application\_Start()** method add the following line to set the database initializer.</span></span>

    <span data-ttu-id="958a2-275">(Codeausschnitt - *Modelle und Datenzugriff - Ex2 globale Asax SetInitializer*)</span><span class="sxs-lookup"><span data-stu-id="958a2-275">(Code Snippet - *Models And Data Access - Ex2 Global Asax SetInitializer*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample6.cs)]
~~~

<a id="Ex2Task2"></a>

<a id="Task_2_-_Configuring_the_connection_to_the_Database"></a>
#### <a name="task-2---configuring-the-connection-to-the-database"></a><span data-ttu-id="958a2-276">Aufgabe 2: Konfigurieren der Verbindung mit der Datenbank</span><span class="sxs-lookup"><span data-stu-id="958a2-276">Task 2 - Configuring the connection to the Database</span></span>

<span data-ttu-id="958a2-277">Nun, dass Sie unseren Projekt bereits eine Datenbank hinzugefügt haben, Schreiben Sie der **"Web.config"** Datei die Verbindungszeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="958a2-277">Now that you have already added a database to our project, you will write in the **Web.config** file the connection string.</span></span>

1. <span data-ttu-id="958a2-278">Fügen Sie eine Verbindungszeichenfolge zur **"Web.config"**. Öffnen Sie hierzu **"Web.config"** am Projektstamm und Ersetzen Sie die Verbindungszeichenfolge mit dem Namen DefaultConnection durch diese Zeile in der **&lt;ConnectionStrings&gt;** Abschnitt:</span><span class="sxs-lookup"><span data-stu-id="958a2-278">Add a connection string at **Web.config**. To do that, open **Web.config** at project root and replace the connection string named DefaultConnection with this line in the **&lt;connectionStrings&gt;** section:</span></span>

    <span data-ttu-id="958a2-279">![Speicherort der Datei "Web.config"](aspnet-mvc-4-models-and-data-access/_static/image19.png "Speicherort der Datei "Web.config"")</span><span class="sxs-lookup"><span data-stu-id="958a2-279">![Web.config file location](aspnet-mvc-4-models-and-data-access/_static/image19.png "Web.config file location")</span></span>

    <span data-ttu-id="958a2-280">*Speicherort der Datei "Web.config"*</span><span class="sxs-lookup"><span data-stu-id="958a2-280">*Web.config file location*</span></span>


~~~
[!code-xml[Main](aspnet-mvc-4-models-and-data-access/samples/sample7.xml)]
~~~

<a id="Ex2Task3"></a>

<a id="Task_3_-_Working_with_the_Model"></a>
#### <a name="task-3---working-with-the-model"></a><span data-ttu-id="958a2-281">Aufgabe 3: Arbeiten mit dem Modell</span><span class="sxs-lookup"><span data-stu-id="958a2-281">Task 3 - Working with the Model</span></span>

<span data-ttu-id="958a2-282">Nun, dass Sie die Verbindung mit der Datenbank bereits konfiguriert haben, verknüpfen Sie das Modell mit Tabellen der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="958a2-282">Now that you have already configured the connection to the database, you will link the model with the database tables.</span></span> <span data-ttu-id="958a2-283">In dieser Aufgabe erstellen Sie eine Klasse, die die Datenbank mit dem Code First verknüpft werden soll.</span><span class="sxs-lookup"><span data-stu-id="958a2-283">In this task, you will create a class that will be linked to the database with Code First.</span></span> <span data-ttu-id="958a2-284">Denken Sie daran, dass eine vorhandene POCO-Modell-Klasse, die geändert werden soll, vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="958a2-284">Remember that there is an existent POCO model class that should be modified.</span></span>

   > [!NOTE]
> <span data-ttu-id="958a2-285">Wenn Sie Übung 1 abgeschlossen haben, werden Sie bemerken, dass dieser Schritt von einem Assistenten durchgeführt wurde.</span><span class="sxs-lookup"><span data-stu-id="958a2-285">If you completed Exercise 1, you will note that this step was performed by a wizard.</span></span> <span data-ttu-id="958a2-286">Indem Sie Code First, erstellen Sie manuell Klassen, die Datenentitäten verknüpft werden soll.</span><span class="sxs-lookup"><span data-stu-id="958a2-286">By doing Code First, you will manually create classes that will be linked to data entities.</span></span>


1. <span data-ttu-id="958a2-287">Öffnen Sie die POCO-Modellklasse **"Genre"** aus **Modelle** Projektordner und enthalten eine-ID.</span><span class="sxs-lookup"><span data-stu-id="958a2-287">Open the POCO model class **Genre** from **Models** project folder and include an ID.</span></span> <span data-ttu-id="958a2-288">Verwenden Sie eine Int-Eigenschaft mit dem Namen **GenreId**.</span><span class="sxs-lookup"><span data-stu-id="958a2-288">Use an int property with the name **GenreId**.</span></span>

    <span data-ttu-id="958a2-289">(Codeausschnitt - *Modelle und Datenzugriff - Ex2 Code erste "Genre"*)</span><span class="sxs-lookup"><span data-stu-id="958a2-289">(Code Snippet - *Models And Data Access - Ex2 Code First Genre*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample8.cs)]

> [!NOTE]
> To work with Code First conventions, the class Genre must have a primary key property that will be automatically detected.
> 
> You can read more about Code First Conventions in this [msdn article](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).
~~~
2. <span data-ttu-id="958a2-290">Öffnen Sie das Modell POCO-Klasse jetzt **Album** aus **Modelle** Projektordner und die Fremdschlüssel enthalten, Erstellen von Eigenschaften mit den Namen **GenreId** und  **ArtistId**.</span><span class="sxs-lookup"><span data-stu-id="958a2-290">Now, open the POCO model class **Album** from **Models** project folder and include the foreign keys, create properties with the names **GenreId** and **ArtistId**.</span></span> <span data-ttu-id="958a2-291">Diese Klasse bereits haben die **GenreId** für den Primärschlüssel.</span><span class="sxs-lookup"><span data-stu-id="958a2-291">This class already have the **GenreId** for the primary key.</span></span>

    <span data-ttu-id="958a2-292">(Codeausschnitt - *Modelle und Datenzugriff - Ex2 Code erste Album*)</span><span class="sxs-lookup"><span data-stu-id="958a2-292">(Code Snippet - *Models And Data Access - Ex2 Code First Album*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample9.cs)]
~~~
3. <span data-ttu-id="958a2-293">Öffnen Sie die POCO-Modellklasse **Interpreten** und enthalten die **ArtistId** Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="958a2-293">Open the POCO model class **Artist** and include the **ArtistId** property.</span></span>

    <span data-ttu-id="958a2-294">(Codeausschnitt - *Modelle und Datenzugriff - Ex2 Code erste Interpreten*)</span><span class="sxs-lookup"><span data-stu-id="958a2-294">(Code Snippet - *Models And Data Access - Ex2 Code First Artist*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample10.cs)]
~~~
4. <span data-ttu-id="958a2-295">Mit der rechten Maustaste die **Modelle** Projektordner, und wählen **hinzufügen | Klasse**.</span><span class="sxs-lookup"><span data-stu-id="958a2-295">Right-click the **Models** project folder and select **Add | Class**.</span></span> <span data-ttu-id="958a2-296">Nennen Sie die Datei **MusicStoreEntities.cs**.</span><span class="sxs-lookup"><span data-stu-id="958a2-296">Name the file **MusicStoreEntities.cs**.</span></span> <span data-ttu-id="958a2-297">Klicken Sie auf **hinzufügen.**</span><span class="sxs-lookup"><span data-stu-id="958a2-297">Then, click **Add.**</span></span>

    <span data-ttu-id="958a2-298">![Hinzufügen einer Klasse](aspnet-mvc-4-models-and-data-access/_static/image20.png "Hinzufügen einer Klasse")</span><span class="sxs-lookup"><span data-stu-id="958a2-298">![Adding a class](aspnet-mvc-4-models-and-data-access/_static/image20.png "Adding a class")</span></span>

    <span data-ttu-id="958a2-299">*Ein neues Element hinzufügen*</span><span class="sxs-lookup"><span data-stu-id="958a2-299">*Adding a new item*</span></span>

    <span data-ttu-id="958a2-300">![Hinzufügen von class2](aspnet-mvc-4-models-and-data-access/_static/image21.png "class2 hinzufügen")</span><span class="sxs-lookup"><span data-stu-id="958a2-300">![Adding a class2](aspnet-mvc-4-models-and-data-access/_static/image21.png "Adding a class2")</span></span>

    <span data-ttu-id="958a2-301">*Hinzufügen einer Klasse*</span><span class="sxs-lookup"><span data-stu-id="958a2-301">*Adding a class*</span></span>
5. <span data-ttu-id="958a2-302">Öffnen Sie die Klasse, die Sie soeben erstellt haben, **MusicStoreEntities.cs**, und fügen Sie die Namespaces **System.Data.Entity** und **System.Data.Entity.Infrastructure**.</span><span class="sxs-lookup"><span data-stu-id="958a2-302">Open the class you have just created, **MusicStoreEntities.cs**, and include the namespaces **System.Data.Entity** and **System.Data.Entity.Infrastructure**.</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample11.cs)]
~~~
6. <span data-ttu-id="958a2-303">Ersetzen Sie die Deklaration der Klasse zum Erweitern der **DbContext** Klasse: deklarieren einen öffentlichen **DBSet** und überschreiben **OnModelCreating** Methode.</span><span class="sxs-lookup"><span data-stu-id="958a2-303">Replace the class declaration to extend the **DbContext** class: declare a public **DBSet** and override **OnModelCreating** method.</span></span> <span data-ttu-id="958a2-304">Nach diesem Schritt erhalten Sie eine Domänenklasse, die das Modell mit dem Entity Framework verknüpft wird.</span><span class="sxs-lookup"><span data-stu-id="958a2-304">After this step you will get a domain class that will link your model with the Entity Framework.</span></span> <span data-ttu-id="958a2-305">Ersetzen Sie zu diesem Zweck des Klasse Codes durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="958a2-305">In order to do that, replace the class code with the following:</span></span>

    <span data-ttu-id="958a2-306">(Codeausschnitt - *Modelle und Datenzugriff - Ex2 Code erste MusicStoreEntities*)</span><span class="sxs-lookup"><span data-stu-id="958a2-306">(Code Snippet - *Models And Data Access - Ex2 Code First MusicStoreEntities*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample12.cs)]

> [!NOTE]
> With Entity Framework **DbContext** and **DBSet** you will be able to query the POCO class Genre. By extending **OnModelCreating** method, you are specifying in the **code** how Genre will be mapped to a database table. You can find more information about DBContext and DBSet in this msdn article: [link](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)
~~~

<a id="Ex2Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a><span data-ttu-id="958a2-307">Aufgabe 4: Abfragen der Datenbank</span><span class="sxs-lookup"><span data-stu-id="958a2-307">Task 4 - Querying the Database</span></span>

<span data-ttu-id="958a2-308">In dieser Aufgabe aktualisieren Sie die StoreController-Klasse so, dass anstelle von hartcodierten Daten sie es aus der Datenbank abgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="958a2-308">In this task, you will update the StoreController class so that, instead of using hardcoded data, it will retrieve it from the database.</span></span>

> [!NOTE]
> <span data-ttu-id="958a2-309">Diese Aufgabe ist auch in zahlreichen Übung 1.</span><span class="sxs-lookup"><span data-stu-id="958a2-309">This task is in common with Exercise 1.</span></span>
> 
> <span data-ttu-id="958a2-310">Wenn Sie Übung 1 abgeschlossen werden Sie bemerken, diese Schritte sind identisch in beiden Ansätzen (Datenbank zunächst oder Code first).</span><span class="sxs-lookup"><span data-stu-id="958a2-310">If you completed Exercise 1 you will note these steps are the same in both approaches (Database first or Code first).</span></span> <span data-ttu-id="958a2-311">Sie unterscheiden sich in wie die Daten mit dem Modell verknüpft ist, der Zugriff auf Datenentitäten ist jedoch noch transparent vom Controller.</span><span class="sxs-lookup"><span data-stu-id="958a2-311">They are different in how the data is linked with the model, but the access to data entities is yet transparent from the controller.</span></span>


1. <span data-ttu-id="958a2-312">Open **Controllers\StoreController.cs** und fügen Sie das folgende Feld der Klasse zum Halten von einer Instanz von der **MusicStoreEntities** Klasse, mit dem Namen **StoreDB**:</span><span class="sxs-lookup"><span data-stu-id="958a2-312">Open **Controllers\StoreController.cs** and add the following field to the class to hold an instance of the **MusicStoreEntities** class, named **storeDB**:</span></span>

    <span data-ttu-id="958a2-313">(Codeausschnitt - *Modelle und des Datenzugriffs - Ex1 StoreDB*)</span><span class="sxs-lookup"><span data-stu-id="958a2-313">(Code Snippet - *Models And Data Access - Ex1 storeDB*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample13.cs)]
~~~
2. <span data-ttu-id="958a2-314">Die **MusicStoreEntities** Klasse macht eine Auflistungseigenschaft für jede Tabelle in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="958a2-314">The **MusicStoreEntities** class exposes a collection property for each table in the database.</span></span> <span data-ttu-id="958a2-315">Update **Durchsuchen** Aktionsmethode einen "Genre" mit allen Abrufen der **Alben**.</span><span class="sxs-lookup"><span data-stu-id="958a2-315">Update **Browse** action method to retrieve a Genre with all of the **Albums**.</span></span>

    <span data-ttu-id="958a2-316">(Codeausschnitt - *Modelle und Datenzugriff - Ex2 Store durchsuchen*)</span><span class="sxs-lookup"><span data-stu-id="958a2-316">(Code Snippet - *Models And Data Access - Ex2 Store Browse*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample14.cs)]

> [!NOTE]
> You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.
> 
> For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).
~~~
3. <span data-ttu-id="958a2-317">Update **Index** Aktionsmethode, um alle Genres abzurufen.</span><span class="sxs-lookup"><span data-stu-id="958a2-317">Update **Index** action method to retrieve all the genres.</span></span>

    <span data-ttu-id="958a2-318">(Codeausschnitt - *Modelle und Datenzugriff - Ex2 Columnstore-Index*)</span><span class="sxs-lookup"><span data-stu-id="958a2-318">(Code Snippet - *Models And Data Access - Ex2 Store Index*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample15.cs)]
~~~
4. <span data-ttu-id="958a2-319">Update **Index** Aktionsmethode alle Genres abrufen und Transformieren von der Auflistung auf eine Liste.</span><span class="sxs-lookup"><span data-stu-id="958a2-319">Update **Index** action method to retrieve all the genres and transform the collection to a list.</span></span>

    <span data-ttu-id="958a2-320">(Codeausschnitt - *Modelle und Datenzugriff - Ex2 Store GenreMenu*)</span><span class="sxs-lookup"><span data-stu-id="958a2-320">(Code Snippet - *Models And Data Access - Ex2 Store GenreMenu*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample16.cs)]
~~~

<a id="Ex2Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="958a2-321">Aufgabe 5: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="958a2-321">Task 5 - Running the Application</span></span>

<span data-ttu-id="958a2-322">In dieser Aufgabe überprüfen Sie, dass die Columnstore-Index-Seite die in der Datenbank anstelle der hartcodiert diejenigen gespeicherten Genres jetzt angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="958a2-322">In this task, you will check that the Store Index page will now display the Genres stored in the database instead of the hardcoded ones.</span></span> <span data-ttu-id="958a2-323">Besteht keine Notwendigkeit zum Ändern der Vorlage anzeigen, da die **StoreController** ist die gleiche zurückgeben **StoreIndexViewModel** wie zuvor, dieses Mal jedoch die Daten aus der Datenbank stammen.</span><span class="sxs-lookup"><span data-stu-id="958a2-323">There is no need to change the View template because the **StoreController** is returning the same **StoreIndexViewModel** as before, but this time the data will come from the database.</span></span>

1. <span data-ttu-id="958a2-324">Erneutes Erstellen von Projektmappen und drücken Sie **F5** um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="958a2-324">Rebuild the solution and press **F5** to run the Application.</span></span>
2. <span data-ttu-id="958a2-325">Das Projekt wird auf der Startseite gestartet.</span><span class="sxs-lookup"><span data-stu-id="958a2-325">The project starts in the Home page.</span></span> <span data-ttu-id="958a2-326">Überprüfen Sie, ob das Menü des **Genres** ist nicht mehr eine hartcodierte Liste, und die Daten direkt aus der Datenbank abgerufen.</span><span class="sxs-lookup"><span data-stu-id="958a2-326">Verify that the menu of **Genres** is no longer a hardcoded list, and the data is directly retrieved from the database.</span></span>

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image22.png)

    <span data-ttu-id="958a2-328">*Durchsuchen von Genres aus der Datenbank*</span><span class="sxs-lookup"><span data-stu-id="958a2-328">*Browsing Genres from the database*</span></span>
3. <span data-ttu-id="958a2-329">Jetzt alle "Genre" und stellen Sie sicher, dass die Alben aus der Datenbank aufgefüllt werden.</span><span class="sxs-lookup"><span data-stu-id="958a2-329">Now browse to any genre and verify the albums are populated from database.</span></span>

    <span data-ttu-id="958a2-330">![Durchsuchen von Alben aus der Datenbank](aspnet-mvc-4-models-and-data-access/_static/image23.png "durchsuchen Alben aus der Datenbank")</span><span class="sxs-lookup"><span data-stu-id="958a2-330">![Browsing Albums from the database](aspnet-mvc-4-models-and-data-access/_static/image23.png "Browsing Albums from the database")</span></span>

    <span data-ttu-id="958a2-331">*Durchsuchen von Alben aus der Datenbank*</span><span class="sxs-lookup"><span data-stu-id="958a2-331">*Browsing Albums from the database*</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Querying_the_Database_with_Parameters"></a>
### <a name="exercise-3-querying-the-database-with-parameters"></a><span data-ttu-id="958a2-332">Übung 3: Abfragen der Datenbank mit Parametern</span><span class="sxs-lookup"><span data-stu-id="958a2-332">Exercise 3: Querying the Database with Parameters</span></span>

<span data-ttu-id="958a2-333">In dieser Übung erfahren Sie, zum Abfragen der Datenbank mithilfe von Parametern und Gewusst wie: Verwenden Sie die Abfrage Ergebnis strukturiert werden, eine Funktion, die die Zahl Datenbank reduziert Abrufen von Daten in eine effizientere Methode zugreift.</span><span class="sxs-lookup"><span data-stu-id="958a2-333">In this exercise, you will learn how to query the database using parameters, and how to use Query Result Shaping, a feature that reduces the number database accesses retrieving data in a more efficient way.</span></span>

> [!NOTE]
> <span data-ttu-id="958a2-334">Weitere Informationen zu Abfrage Ergebnis strukturiert werden, finden Sie auf der folgenden [Msdn-Artikel](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).</span><span class="sxs-lookup"><span data-stu-id="958a2-334">For further information on Query Result Shaping, visit the following [msdn article](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).</span></span>


<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_StoreController_to_Retrieve_Albums_from_Database"></a>
#### <a name="task-1---modifying-storecontroller-to-retrieve-albums-from-database"></a><span data-ttu-id="958a2-335">Aufgabe 1: Ändern von StoreController Alben aus Datenbank abrufen</span><span class="sxs-lookup"><span data-stu-id="958a2-335">Task 1 - Modifying StoreController to Retrieve Albums from Database</span></span>

<span data-ttu-id="958a2-336">In dieser Aufgabe ändern Sie die **StoreController** Klasse Zugriff auf die Datenbank zum Abrufen von Alben aus einem bestimmten "Genre".</span><span class="sxs-lookup"><span data-stu-id="958a2-336">In this task, you will change the **StoreController** class to access the database to retrieve albums from a specific genre.</span></span>

1. <span data-ttu-id="958a2-337">Öffnen der **beginnen** Lösung finden Sie unter der **Source\Ex3 QueryingTheDatabaseWithParametersCodeFirst\Begin** Ordner, wenn Sie Code First-Ansatz verwenden möchten oder **Source\ Ex3 QueryingTheDatabaseWithParametersDBFirst\Begin** Benutzerordner, falls Sie Database First-Ansatz verwenden möchten.</span><span class="sxs-lookup"><span data-stu-id="958a2-337">Open the **Begin** solution located at the **Source\Ex3-QueryingTheDatabaseWithParametersCodeFirst\Begin** folder if you want to use Code-First approach or **Source\Ex3-QueryingTheDatabaseWithParametersDBFirst\Begin** folder if you want to use Database-First approach.</span></span> <span data-ttu-id="958a2-338">Andernfalls möglicherweise weiterhin mithilfe der **End** Lösung abgerufen, von der vorherigen Übung abschließen.</span><span class="sxs-lookup"><span data-stu-id="958a2-338">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="958a2-339">Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung müssen Sie einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren.</span><span class="sxs-lookup"><span data-stu-id="958a2-339">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="958a2-340">Klicken Sie hierzu auf die **Projekt** Menü **NuGet-Pakete verwalten**.</span><span class="sxs-lookup"><span data-stu-id="958a2-340">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="958a2-341">In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.</span><span class="sxs-lookup"><span data-stu-id="958a2-341">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="958a2-342">Schließlich erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.</span><span class="sxs-lookup"><span data-stu-id="958a2-342">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="958a2-343">Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken, die sich in Ihrem Projekt liefern Verringern der Projektgröße.</span><span class="sxs-lookup"><span data-stu-id="958a2-343">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="958a2-344">Mit NuGet-Powertools werden durch Angabe der Paketversionen in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, wenn, das Sie das Projekt ausführen, können.</span><span class="sxs-lookup"><span data-stu-id="958a2-344">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="958a2-345">Deshalb wird müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit.</span><span class="sxs-lookup"><span data-stu-id="958a2-345">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="958a2-346">Öffnen der **StoreController** -Klasse zum Ändern der **Durchsuchen** Aktionsmethode.</span><span class="sxs-lookup"><span data-stu-id="958a2-346">Open the **StoreController** class to change the **Browse** action method.</span></span> <span data-ttu-id="958a2-347">Klicken Sie hierzu in der **Projektmappen-Explorer**, erweitern Sie die **Controller** Ordner und doppelklicken Sie auf **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="958a2-347">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
3. <span data-ttu-id="958a2-348">Ändern der **Durchsuchen** Aktionsmethode Alben für einen bestimmten "Genre" abgerufen.</span><span class="sxs-lookup"><span data-stu-id="958a2-348">Change the **Browse** action method to retrieve albums for a specific genre.</span></span> <span data-ttu-id="958a2-349">Ersetzen Sie hierzu den folgenden Code ein:</span><span class="sxs-lookup"><span data-stu-id="958a2-349">To do this, replace the following code:</span></span>

    <span data-ttu-id="958a2-350">(Codeausschnitt - *Modelle und Datenzugriff - Ex3 StoreController BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="958a2-350">(Code Snippet - *Models And Data Access - Ex3 StoreController BrowseMethod*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample17.cs)]

> [!NOTE]
> To populate a collection of the entity, you need to use the **Include** method to specify you want to retrieve the albums too. You can use the .**Single()** extension in LINQ because in this case only one genre is expected for an album. The **Single()** method takes a Lambda expression as a parameter, which in this case specifies a single Genre object such that its name matches the value defined.
> 
> You will take advantage of a feature that allows you to indicate other related entities you want loaded as well when the Genre object is retrieved. This feature is called **Query Result Shaping**, and enables you to reduce the number of times needed to access the database to retrieve information. In this scenario, you will want to pre-fetch the Albums for the Genre you retrieve.
> 
> The query includes **Genres.Include(&quot;Albums&quot;)** to indicate that you want related albums as well. This will result in a more efficient application, since it will retrieve both Genre and Album data in a single database request.
~~~

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="958a2-351">Aufgabe 2: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="958a2-351">Task 2 - Running the Application</span></span>

<span data-ttu-id="958a2-352">In dieser Aufgabe werden Sie die Anwendung auszuführen und Alben mit einem bestimmten "Genre" aus der Datenbank abrufen.</span><span class="sxs-lookup"><span data-stu-id="958a2-352">In this task, you will run the application and retrieve albums of a specific genre from the database.</span></span>

1. <span data-ttu-id="958a2-353">Drücken Sie **F5** um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="958a2-353">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="958a2-354">Das Projekt wird auf der Startseite gestartet.</span><span class="sxs-lookup"><span data-stu-id="958a2-354">The project starts in the Home page.</span></span> <span data-ttu-id="958a2-355">Ändern Sie die URL zum **/Store/durchsuchen? "Genre" Pop =** um sicherzustellen, dass die Ergebnisse aus der Datenbank abgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="958a2-355">Change the URL to **/Store/Browse?genre=Pop** to verify that the results are being retrieved from the database.</span></span>

    <span data-ttu-id="958a2-356">![Durchsuchen nach "Genre"](aspnet-mvc-4-models-and-data-access/_static/image24.png "durchsuchen nach "Genre"")</span><span class="sxs-lookup"><span data-stu-id="958a2-356">![Browsing by genre](aspnet-mvc-4-models-and-data-access/_static/image24.png "Browsing by genre")</span></span>

    <span data-ttu-id="958a2-357">*Durchsuchen/Store/durchsuchen? "Genre" Pop =*</span><span class="sxs-lookup"><span data-stu-id="958a2-357">*Browsing /Store/Browse?genre=Pop*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Accessing_Albums_by_Id"></a>
#### <a name="task-3---accessing-albums-by-id"></a><span data-ttu-id="958a2-358">Aufgabe 3: Zugreifen auf Alben nach Id</span><span class="sxs-lookup"><span data-stu-id="958a2-358">Task 3 - Accessing Albums by Id</span></span>

<span data-ttu-id="958a2-359">In dieser Aufgabe werden Sie in der vorherigen Prozedur zum Abrufen von Alben anhand ihrer Id. wiederholen.</span><span class="sxs-lookup"><span data-stu-id="958a2-359">In this task, you will repeat the previous procedure to get albums by their Id.</span></span>

1. <span data-ttu-id="958a2-360">Schließen Sie den Browser, die ggf. zu Visual Studio zurückzukehren.</span><span class="sxs-lookup"><span data-stu-id="958a2-360">Close the browser if needed, to return to Visual Studio.</span></span> <span data-ttu-id="958a2-361">Öffnen der **StoreController** -Klasse zum Ändern der **Details** Aktionsmethode.</span><span class="sxs-lookup"><span data-stu-id="958a2-361">Open the **StoreController** class to change the **Details** action method.</span></span> <span data-ttu-id="958a2-362">Klicken Sie hierzu in der **Projektmappen-Explorer**, erweitern Sie die **Controller** Ordner und doppelklicken Sie auf **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="958a2-362">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
2. <span data-ttu-id="958a2-363">Ändern der **Details** Aktionsmethode beim Abrufen der Details Alben basierend auf ihren **Id**. Ersetzen Sie hierzu den folgenden Code ein:</span><span class="sxs-lookup"><span data-stu-id="958a2-363">Change the **Details** action method to retrieve albums details based on their **Id**. To do this, replace the following code:</span></span>

    <span data-ttu-id="958a2-364">(Codeausschnitt - *Modelle und Datenzugriff - Ex3 StoreController DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="958a2-364">(Code Snippet - *Models And Data Access - Ex3 StoreController DetailsMethod*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample18.cs)]
~~~

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="958a2-365">Aufgabe 4: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="958a2-365">Task 4 - Running the Application</span></span>

<span data-ttu-id="958a2-366">In dieser Aufgabe wird die Anwendung in einem Webbrowser ausführen und erhalten Album anhand ihrer Id.</span><span class="sxs-lookup"><span data-stu-id="958a2-366">In this task, you will run the Application in a web browser and obtain album details by their Id.</span></span>

1. <span data-ttu-id="958a2-367">Drücken Sie **F5** um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="958a2-367">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="958a2-368">Das Projekt wird auf der Startseite gestartet.</span><span class="sxs-lookup"><span data-stu-id="958a2-368">The project starts in the Home page.</span></span> <span data-ttu-id="958a2-369">Ändern Sie die URL zum **/Store/Details/51** oder die Genres durchsuchen, und wählen Sie ein Album, um sicherzustellen, dass die Ergebnisse aus der Datenbank abgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="958a2-369">Change the URL to **/Store/Details/51** or browse the genres and select an album to verify that the results are being retrieved from the database.</span></span>

    <span data-ttu-id="958a2-370">![Durchsuchen von Details](aspnet-mvc-4-models-and-data-access/_static/image25.png "Details durchsuchen")</span><span class="sxs-lookup"><span data-stu-id="958a2-370">![Browsing Details](aspnet-mvc-4-models-and-data-access/_static/image25.png "Browsing Details")</span></span>

    <span data-ttu-id="958a2-371">*/Store/Details/51 durchsuchen*</span><span class="sxs-lookup"><span data-stu-id="958a2-371">*Browsing /Store/Details/51*</span></span>

> [!NOTE]
> <span data-ttu-id="958a2-372">Darüber hinaus können Sie die Bereitstellung dieser Anwendung, die Windows Azure-Websites folgenden [Anhang B: Veröffentlichen einer ASP.NET MVC 4-Anwendung mithilfe von Web Deploy](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="958a2-372">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="958a2-373">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="958a2-373">Summary</span></span>

<span data-ttu-id="958a2-374">Durch diese praktische Übungseinheit haben Sie gelernt, die Grundlagen von ASP.NET MVC-Modelle und Datenzugriff abschließen, indem Sie die **Database First** Ansatz als auch die **Code First** Ansatz:</span><span class="sxs-lookup"><span data-stu-id="958a2-374">By completing this Hands-on Lab you have learned the fundamentals of ASP.NET MVC Models and Data Access, using the **Database First** approach as well as the **Code First** Approach:</span></span>

- <span data-ttu-id="958a2-375">Wie die Lösung eine Datenbank hinzugefügt, um die Daten zu nutzen.</span><span class="sxs-lookup"><span data-stu-id="958a2-375">How to add a database to the solution in order to consume its data</span></span>
- <span data-ttu-id="958a2-376">Gewusst wie: Aktualisieren der Verwaltungscontroller um Ansichtsvorlagen mit den Daten aus der Datenbank statt hartcodierten bereitzustellen</span><span class="sxs-lookup"><span data-stu-id="958a2-376">How to update Controllers to provide View templates with the data taken from the database instead of hard-coded one</span></span>
- <span data-ttu-id="958a2-377">Gewusst wie: Abfragen der Datenbank, die mit Parametern</span><span class="sxs-lookup"><span data-stu-id="958a2-377">How to query the database using parameters</span></span>
- <span data-ttu-id="958a2-378">Gewusst wie: Verwenden Sie die Abfrage Ergebnis strukturiert werden, eine Funktion, die reduziert die Anzahl der Zugriff auf die Datenbank, Abrufen von Daten in eine effizientere Methode</span><span class="sxs-lookup"><span data-stu-id="958a2-378">How to use the Query Result Shaping, a feature that reduces the number of database accesses, retrieving data in a more efficient way</span></span>
- <span data-ttu-id="958a2-379">Gewusst wie: Database First und Code First Ansätze in Microsoft Entity Framework zu verwenden, um die Datenbank mit dem Modell verknüpfen</span><span class="sxs-lookup"><span data-stu-id="958a2-379">How to use both Database First and Code First approaches in Microsoft Entity Framework to link the database with the model</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="958a2-380">Anhang A: Installieren von Visual Studio Express 2012 für das Web</span><span class="sxs-lookup"><span data-stu-id="958a2-380">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="958a2-381">Sie installieren können **Microsoft Visual Studio Express 2012 für das Web** oder ein anderes &quot;Express&quot; Version mithilfe der **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="958a2-381">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="958a2-382">Die folgenden Anweisungen führen Sie durch die erforderlichen Schritte zum Installieren *Visual Studio Express 2012 für das Web* mit *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="958a2-382">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="958a2-383">Wechseln Sie zu [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="958a2-383">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="958a2-384">Auch wenn Sie bereits Webplattform-Installer installiert haben, können Sie öffnen es, und suchen Sie nach dem Produkt &quot; <em>Visual Studio Express 2012 für das Web mit Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="958a2-384">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="958a2-385">Klicken Sie auf **jetzt installieren**.</span><span class="sxs-lookup"><span data-stu-id="958a2-385">Click on **Install Now**.</span></span> <span data-ttu-id="958a2-386">Wenn Sie keine **Webplattform-Installer** Sie Informationen zum Herunterladen und installieren Sie diese zuerst umgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="958a2-386">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="958a2-387">Einmal **Webplattform-Installer** geöffnet ist, klicken Sie auf **installieren** um das Setup zu starten.</span><span class="sxs-lookup"><span data-stu-id="958a2-387">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="958a2-388">![Visual Studio Express installieren](aspnet-mvc-4-models-and-data-access/_static/image26.png "Visual Studio Express installieren")</span><span class="sxs-lookup"><span data-stu-id="958a2-388">![Install Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="958a2-389">*Installieren Sie Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="958a2-389">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="958a2-390">Lesen Sie die Produkte, Lizenzen und Begriffe, und klicken Sie auf **ich stimme** um den Vorgang fortzusetzen.</span><span class="sxs-lookup"><span data-stu-id="958a2-390">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Akzeptieren der Lizenzbedingungen](aspnet-mvc-4-models-and-data-access/_static/image27.png)

    <span data-ttu-id="958a2-392">*Akzeptieren der Lizenzbedingungen*</span><span class="sxs-lookup"><span data-stu-id="958a2-392">*Accepting the license terms*</span></span>
5. <span data-ttu-id="958a2-393">Warten Sie, bis der Prozess herunterladen und die Installation abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="958a2-393">Wait until the downloading and installation process completes.</span></span>

    ![Installationsstatus](aspnet-mvc-4-models-and-data-access/_static/image28.png)

    <span data-ttu-id="958a2-395">*Installationsstatus*</span><span class="sxs-lookup"><span data-stu-id="958a2-395">*Installation progress*</span></span>
6. <span data-ttu-id="958a2-396">Nach Abschluss der Installation klicken Sie auf **Fertig stellen**.</span><span class="sxs-lookup"><span data-stu-id="958a2-396">When the installation completes, click **Finish**.</span></span>

    ![Installation wurde abgeschlossen](aspnet-mvc-4-models-and-data-access/_static/image29.png)

    <span data-ttu-id="958a2-398">*Installation wurde abgeschlossen*</span><span class="sxs-lookup"><span data-stu-id="958a2-398">*Installation completed*</span></span>
7. <span data-ttu-id="958a2-399">Klicken Sie auf **beenden** Webplattform-Installer zu schließen.</span><span class="sxs-lookup"><span data-stu-id="958a2-399">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="958a2-400">Um Visual Studio Express für Web zu öffnen, wechseln Sie zu der **starten** Startseite ein, und starten Sie das Schreiben von &quot; **Visual Studio Express**&quot;, klicken Sie dann auf die **Visual Studio Express für Web** Kachel.</span><span class="sxs-lookup"><span data-stu-id="958a2-400">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Visual Studio Express für Web-Kachel](aspnet-mvc-4-models-and-data-access/_static/image30.png)

    <span data-ttu-id="958a2-402">*Visual Studio Express für Web-Kachel*</span><span class="sxs-lookup"><span data-stu-id="958a2-402">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="958a2-403">Anhang B: Veröffentlichen einer ASP.NET MVC 4-Anwendung mithilfe von Web Deploy</span><span class="sxs-lookup"><span data-stu-id="958a2-403">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="958a2-404">In diesem Anhang wird gezeigt, wie eine neue Website aus dem Windows Azure-Verwaltungsportal erstellen und veröffentlichen Sie die Anwendung, die Sie erhalten haben anhand der testumgebung profitieren von der Web Deploy Veröffentlichungsfunktion von Windows Azure bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="958a2-404">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="958a2-405">Aufgabe 1: Erstellen einer neuen Website aus dem Windows Azure-Portal</span><span class="sxs-lookup"><span data-stu-id="958a2-405">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="958a2-406">Wechseln Sie zu der [Windows Azure-Verwaltungsportal](https://manage.windowsazure.com/) und melden Sie sich mit den Microsoft-Anmeldeinformationen, die Ihrem Abonnement zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="958a2-406">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="958a2-407">Sie können mit Windows Azure hosten 10 ASP.NET-Websites kostenlos und skalieren Sie, wenn Ihr Datenverkehr zunimmt.</span><span class="sxs-lookup"><span data-stu-id="958a2-407">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="958a2-408">Sie können registrieren [hier](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="958a2-408">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="958a2-409">![Melden Sie sich bei Windows Azure-Portal](aspnet-mvc-4-models-and-data-access/_static/image31.png "melden Sie sich bei Windows Azure-Portal")</span><span class="sxs-lookup"><span data-stu-id="958a2-409">![Log on to Windows Azure portal](aspnet-mvc-4-models-and-data-access/_static/image31.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="958a2-410">*Melden Sie sich bei Windows Azure-Verwaltungsportal*</span><span class="sxs-lookup"><span data-stu-id="958a2-410">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="958a2-411">Klicken Sie auf **neu** in der Befehlszeile.</span><span class="sxs-lookup"><span data-stu-id="958a2-411">Click **New** on the command bar.</span></span>

    <span data-ttu-id="958a2-412">![Erstellen einer neuen Website](aspnet-mvc-4-models-and-data-access/_static/image32.png "Erstellen einer neuen Website")</span><span class="sxs-lookup"><span data-stu-id="958a2-412">![Creating a new Web Site](aspnet-mvc-4-models-and-data-access/_static/image32.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="958a2-413">*Erstellen einer neuen Website*</span><span class="sxs-lookup"><span data-stu-id="958a2-413">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="958a2-414">Klicken Sie auf **berechnen** | **Website**.</span><span class="sxs-lookup"><span data-stu-id="958a2-414">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="958a2-415">Wählen Sie dann **Schnellerfassung** Option.</span><span class="sxs-lookup"><span data-stu-id="958a2-415">Then select **Quick Create** option.</span></span> <span data-ttu-id="958a2-416">Verfügbare URL für die neue Website bereitstellen, und klicken Sie auf **Website erstellen**.</span><span class="sxs-lookup"><span data-stu-id="958a2-416">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="958a2-417">Eine Windows Azure-Website ist der Host für eine Webanwendung, die ausgeführt wird, in der Cloud, die Sie steuern und verwalten können.</span><span class="sxs-lookup"><span data-stu-id="958a2-417">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="958a2-418">Die Option Schnellerfassung können Sie eine vollständige Webanwendung, die Windows Azure-Website von außerhalb des Portals bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="958a2-418">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="958a2-419">Es umfasst nicht die Schritte zum Einrichten einer Datenbank.</span><span class="sxs-lookup"><span data-stu-id="958a2-419">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="958a2-420">![Erstellen eine neue Website, die mit der Schnellerfassung](aspnet-mvc-4-models-and-data-access/_static/image33.png "erstellen eine neue Website, die mit der Schnellerfassung")</span><span class="sxs-lookup"><span data-stu-id="958a2-420">![Creating a new Web Site using Quick Create](aspnet-mvc-4-models-and-data-access/_static/image33.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="958a2-421">*Erstellen eine neue Website, die mit der Schnellerfassung*</span><span class="sxs-lookup"><span data-stu-id="958a2-421">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="958a2-422">Warten Sie, bis die neue **Website** wird erstellt.</span><span class="sxs-lookup"><span data-stu-id="958a2-422">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="958a2-423">Nachdem die Website erstellt wurde, klicken Sie auf den Link unter der **URL** Spalte.</span><span class="sxs-lookup"><span data-stu-id="958a2-423">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="958a2-424">Überprüfen Sie, dass die neue Website funktioniert.</span><span class="sxs-lookup"><span data-stu-id="958a2-424">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="958a2-425">![Um die neue Website durchsuchen](aspnet-mvc-4-models-and-data-access/_static/image34.png "durchsuchen, um die neue Website")</span><span class="sxs-lookup"><span data-stu-id="958a2-425">![Browsing to the new web site](aspnet-mvc-4-models-and-data-access/_static/image34.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="958a2-426">*Um die neue Website durchsuchen*</span><span class="sxs-lookup"><span data-stu-id="958a2-426">*Browsing to the new web site*</span></span>

    <span data-ttu-id="958a2-427">![Website](aspnet-mvc-4-models-and-data-access/_static/image35.png "Website ausgeführt wird")</span><span class="sxs-lookup"><span data-stu-id="958a2-427">![Web site running](aspnet-mvc-4-models-and-data-access/_static/image35.png "Web site running")</span></span>

    <span data-ttu-id="958a2-428">*Die Website ausgeführt wird*</span><span class="sxs-lookup"><span data-stu-id="958a2-428">*Web site running*</span></span>
6. <span data-ttu-id="958a2-429">Wechseln Sie zurück zum Portal, und klicken Sie auf den Namen der Website unter der **Namen** Spalte die Verwaltungsseiten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="958a2-429">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="958a2-430">![Öffnen die Website-Verwaltungsseiten](aspnet-mvc-4-models-and-data-access/_static/image36.png "öffnen die Website-Verwaltungsseiten")</span><span class="sxs-lookup"><span data-stu-id="958a2-430">![Opening the web site management pages](aspnet-mvc-4-models-and-data-access/_static/image36.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="958a2-431">*Öffnen die Website-Verwaltungsseiten*</span><span class="sxs-lookup"><span data-stu-id="958a2-431">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="958a2-432">In der **Dashboard** Seite der **kurzer Blick** auf die **Herunterladen eines Veröffentlichungsprofils** Link.</span><span class="sxs-lookup"><span data-stu-id="958a2-432">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="958a2-433">Die *Veröffentlichungsprofil* enthält alle Informationen zum Veröffentlichen einer Webanwendung in einer Windows Azure-Website für die einzelnen aktivierten Veröffentlichungsmethoden erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="958a2-433">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="958a2-434">Das Veröffentlichungsprofil enthält die URLs, Benutzeranmeldeinformationen und datenbankzeichenfolgen, die erforderlich sind, eine Verbindung herstellen und die Authentifizierung für alle Endpunkte für die eine Veröffentlichungsmethode aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="958a2-434">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="958a2-435">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express für Web** und **Microsoft Visual Studio 2012** Unterstützung beim Lesen von veröffentlichungsprofilen zum Automatisieren der Konfiguration dieser Programme für Veröffentlichung von Webanwendungen auf Windows Azure-Websites.</span><span class="sxs-lookup"><span data-stu-id="958a2-435">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="958a2-436">![Herunterladen der Website-Veröffentlichungsprofil](aspnet-mvc-4-models-and-data-access/_static/image37.png "der Website herunterladen eines Veröffentlichungsprofils")</span><span class="sxs-lookup"><span data-stu-id="958a2-436">![Downloading the web site publish profile](aspnet-mvc-4-models-and-data-access/_static/image37.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="958a2-437">*Die Website herunterladen eines Veröffentlichungsprofils*</span><span class="sxs-lookup"><span data-stu-id="958a2-437">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="958a2-438">Laden Sie das Veröffentlichungsprofil an einem bekannten Speicherort ein.</span><span class="sxs-lookup"><span data-stu-id="958a2-438">Download the publish profile file to a known location.</span></span> <span data-ttu-id="958a2-439">In dieser Übung wird weiter Gewusst wie: Verwenden Sie diese Datei zum Veröffentlichen einer Webanwendung auf einem Windows Azure-Websites in Visual Studio angezeigt.</span><span class="sxs-lookup"><span data-stu-id="958a2-439">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="958a2-440">![Speichern das Veröffentlichungsprofil](aspnet-mvc-4-models-and-data-access/_static/image38.png "speichern das Veröffentlichungsprofil")</span><span class="sxs-lookup"><span data-stu-id="958a2-440">![Saving the publish profile file](aspnet-mvc-4-models-and-data-access/_static/image38.png "Saving the publish profile")</span></span>

    <span data-ttu-id="958a2-441">*Speichern das Veröffentlichungsprofil*</span><span class="sxs-lookup"><span data-stu-id="958a2-441">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="958a2-442">Aufgabe 2: konfigurieren den Datenbankserver</span><span class="sxs-lookup"><span data-stu-id="958a2-442">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="958a2-443">Wenn die Anwendung durchführt, verwenden Sie SQL Server Datenbanken Sie einen SQL-Datenbankserver erstellen müssen.</span><span class="sxs-lookup"><span data-stu-id="958a2-443">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="958a2-444">Wenn eine einfache Anwendung bereitstellen, die keine SQL Server verwendet werden sollen, können Sie diese Aufgabe überspringen.</span><span class="sxs-lookup"><span data-stu-id="958a2-444">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="958a2-445">Sie benötigen einen SQL-Datenbankserver zum Speichern der Anwendungsdatenbank.</span><span class="sxs-lookup"><span data-stu-id="958a2-445">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="958a2-446">Sehen Sie die SQL-Datenbankservern über Ihr Abonnement in Windows Azure-Verwaltungsportal am **Sql-Datenbanken** | **Server** | **des Servers Dashboard**.</span><span class="sxs-lookup"><span data-stu-id="958a2-446">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="958a2-447">Wenn Sie keinen Server erstellt haben, können Sie erstellen, mit der **hinzufügen** auf der Befehlsleiste auf die Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="958a2-447">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="958a2-448">Notieren Sie die **Servername und -URL, Administrator-Benutzername und Kennwort**, wie Sie sie in den nächsten Aufgaben verwenden.</span><span class="sxs-lookup"><span data-stu-id="958a2-448">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="958a2-449">Erstellen Sie die Datenbank nicht noch, wie er in einer späteren Phase erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="958a2-449">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="958a2-450">![SQL-Datenbank-Dashboard](aspnet-mvc-4-models-and-data-access/_static/image39.png "SQL-Datenbank-Dashboard")</span><span class="sxs-lookup"><span data-stu-id="958a2-450">![SQL Database Server Dashboard](aspnet-mvc-4-models-and-data-access/_static/image39.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="958a2-451">*SQL-Datenbank-Dashboard*</span><span class="sxs-lookup"><span data-stu-id="958a2-451">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="958a2-452">In der nächsten Aufgabe testen Sie die Verbindung mit der Datenbank, aus Visual Studio aus diesem Grund Sie die lokale IP-Adresse in der Serverliste des müssen **zulässigen IP-Adressen**.</span><span class="sxs-lookup"><span data-stu-id="958a2-452">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="958a2-453">Klicken Sie hierzu auf **konfigurieren**, wählen Sie die IP-Adresse von **aktuelle Client-IP-Adresse** und fügen Sie ihn auf die **IP-Startadresse** und **IP-Endadresse** Textfelder, und klicken Sie auf die ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png) Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="958a2-453">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png) button.</span></span>

    ![Client-IP-Adresse hinzufügen](aspnet-mvc-4-models-and-data-access/_static/image41.png)

    <span data-ttu-id="958a2-455">*Client-IP-Adresse hinzufügen*</span><span class="sxs-lookup"><span data-stu-id="958a2-455">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="958a2-456">Einmal die **IP-Clientadresse** wird zu den zulässigen IP-Adressen hinzugefügt Liste, klicken Sie auf **speichern** um die Änderungen zu bestätigen.</span><span class="sxs-lookup"><span data-stu-id="958a2-456">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Bestätigen von Änderungen](aspnet-mvc-4-models-and-data-access/_static/image42.png)

    <span data-ttu-id="958a2-458">*Bestätigen von Änderungen*</span><span class="sxs-lookup"><span data-stu-id="958a2-458">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="958a2-459">Aufgabe 3: Veröffentlichen einer ASP.NET MVC 4-Anwendung mithilfe von Web Deploy</span><span class="sxs-lookup"><span data-stu-id="958a2-459">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="958a2-460">Wechseln Sie zurück, die ASP.NET MVC 4-Projektmappe.</span><span class="sxs-lookup"><span data-stu-id="958a2-460">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="958a2-461">In der **Projektmappen-Explorer**mit der rechten Maustaste auf das Websiteprojekt, und wählen Sie **veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="958a2-461">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="958a2-462">![Veröffentlichen der Anwendung](aspnet-mvc-4-models-and-data-access/_static/image43.png "Veröffentlichen der Anwendung")</span><span class="sxs-lookup"><span data-stu-id="958a2-462">![Publishing the Application](aspnet-mvc-4-models-and-data-access/_static/image43.png "Publishing the Application")</span></span>

    <span data-ttu-id="958a2-463">*Veröffentlichen der Website*</span><span class="sxs-lookup"><span data-stu-id="958a2-463">*Publishing the web site*</span></span>
2. <span data-ttu-id="958a2-464">Importieren Sie das Veröffentlichungsprofil, das Sie in der ersten Aufgabe gespeichert.</span><span class="sxs-lookup"><span data-stu-id="958a2-464">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="958a2-465">![Importieren des Veröffentlichungsprofils](aspnet-mvc-4-models-and-data-access/_static/image44.png "Importieren des Veröffentlichungsprofils")</span><span class="sxs-lookup"><span data-stu-id="958a2-465">![Importing the publish profile](aspnet-mvc-4-models-and-data-access/_static/image44.png "Importing the publish profile")</span></span>

    <span data-ttu-id="958a2-466">*Importieren Sie das Veröffentlichungsprofil*</span><span class="sxs-lookup"><span data-stu-id="958a2-466">*Importing publish profile*</span></span>
3. <span data-ttu-id="958a2-467">Klicken Sie auf **überprüft, ob Verbindung**.</span><span class="sxs-lookup"><span data-stu-id="958a2-467">Click **Validate Connection**.</span></span> <span data-ttu-id="958a2-468">Klicken Sie nach Abschluss der Überprüfung auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="958a2-468">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="958a2-469">Überprüfung ist beendet, sobald Sie sehen, dass ein grünes Häkchen neben der Schaltfläche "Validate Connection" angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="958a2-469">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="958a2-470">![Überprüfen der Verbindung](aspnet-mvc-4-models-and-data-access/_static/image45.png "Überprüfen der Verbindung")</span><span class="sxs-lookup"><span data-stu-id="958a2-470">![Validating connection](aspnet-mvc-4-models-and-data-access/_static/image45.png "Validating connection")</span></span>

    <span data-ttu-id="958a2-471">*Überprüfen der Verbindung*</span><span class="sxs-lookup"><span data-stu-id="958a2-471">*Validating connection*</span></span>
4. <span data-ttu-id="958a2-472">In der **Einstellungen** Seite der **Datenbanken** auf die Schaltfläche neben dem Textfeld für die datenbankverbindung (d. h. **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="958a2-472">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="958a2-473">![Web deploy-Konfiguration](aspnet-mvc-4-models-and-data-access/_static/image46.png "Web deploy-Konfiguration")</span><span class="sxs-lookup"><span data-stu-id="958a2-473">![Web deploy configuration](aspnet-mvc-4-models-and-data-access/_static/image46.png "Web deploy configuration")</span></span>

    <span data-ttu-id="958a2-474">*Web deploy-Konfiguration*</span><span class="sxs-lookup"><span data-stu-id="958a2-474">*Web deploy configuration*</span></span>
5. <span data-ttu-id="958a2-475">Konfigurieren Sie die Verbindung mit der Datenbank wie folgt:</span><span class="sxs-lookup"><span data-stu-id="958a2-475">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="958a2-476">In der **Servernamen** Geben Sie Ihre SQL-Datenbank Server-URL unter Verwendung der *Tcp:* Präfix.</span><span class="sxs-lookup"><span data-stu-id="958a2-476">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="958a2-477">In **Benutzername** Geben Sie Ihre Administrator Serveranmeldenamen an.</span><span class="sxs-lookup"><span data-stu-id="958a2-477">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="958a2-478">In **Kennwort** Geben Sie Ihre Server-Administratorkennwort.</span><span class="sxs-lookup"><span data-stu-id="958a2-478">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="958a2-479">Geben Sie einen neuen Datenbanknamen ein.</span><span class="sxs-lookup"><span data-stu-id="958a2-479">Type a new database name.</span></span>

     <span data-ttu-id="958a2-480">![Konfigurieren von Zielverbindungszeichenfolge](aspnet-mvc-4-models-and-data-access/_static/image47.png "Zielverbindungszeichenfolge konfigurieren")</span><span class="sxs-lookup"><span data-stu-id="958a2-480">![Configuring destination connection string](aspnet-mvc-4-models-and-data-access/_static/image47.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="958a2-481">*Konfigurieren von Ziel-Verbindungszeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="958a2-481">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="958a2-482">Klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="958a2-482">Then click **OK**.</span></span> <span data-ttu-id="958a2-483">Bei der Aufforderung zum Erstellen des Datenbank auf **Ja**.</span><span class="sxs-lookup"><span data-stu-id="958a2-483">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="958a2-484">![Erstellen der Datenbank](aspnet-mvc-4-models-and-data-access/_static/image48.png "erstellen die Datenbank-Zeichenfolge")</span><span class="sxs-lookup"><span data-stu-id="958a2-484">![Creating the database](aspnet-mvc-4-models-and-data-access/_static/image48.png "Creating the database string")</span></span>

    <span data-ttu-id="958a2-485">*Erstellen der Datenbank*</span><span class="sxs-lookup"><span data-stu-id="958a2-485">*Creating the database*</span></span>
7. <span data-ttu-id="958a2-486">Die Verbindungszeichenfolge, die Sie verwenden für die Verbindung mit SQL-Datenbank in Windows Azure ist innerhalb der Verbindung standardmäßig Textfeld angezeigt.</span><span class="sxs-lookup"><span data-stu-id="958a2-486">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="958a2-487">Klicken Sie dann auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="958a2-487">Then click **Next**.</span></span>

    <span data-ttu-id="958a2-488">![Verbindungszeichenfolge zur SQL-Datenbank zeigt](aspnet-mvc-4-models-and-data-access/_static/image49.png "Verbindungszeichenfolge zur SQL-Datenbank zeigt")</span><span class="sxs-lookup"><span data-stu-id="958a2-488">![Connection string pointing to SQL Database](aspnet-mvc-4-models-and-data-access/_static/image49.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="958a2-489">*Verbindungszeichenfolge, die auf der SQL-Datenbank*</span><span class="sxs-lookup"><span data-stu-id="958a2-489">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="958a2-490">In der **Vorschau** auf **veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="958a2-490">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="958a2-491">![Veröffentlichen der Webanwendung](aspnet-mvc-4-models-and-data-access/_static/image50.png "Veröffentlichen der Webanwendung")</span><span class="sxs-lookup"><span data-stu-id="958a2-491">![Publishing the web application](aspnet-mvc-4-models-and-data-access/_static/image50.png "Publishing the web application")</span></span>

    <span data-ttu-id="958a2-492">*Veröffentlichen der Webanwendung*</span><span class="sxs-lookup"><span data-stu-id="958a2-492">*Publishing the web application*</span></span>
9. <span data-ttu-id="958a2-493">Sobald der Veröffentlichungsprozess abgeschlossen ist, wird der Standardbrowser veröffentlichten Website geöffnet.</span><span class="sxs-lookup"><span data-stu-id="958a2-493">Once the publishing process finishes, your default browser will open the published web site.</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="958a2-494">Anhang C: Verwendung von Codeausschnitten</span><span class="sxs-lookup"><span data-stu-id="958a2-494">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="958a2-495">Mit Codeausschnitten müssen Sie den Code, den Sie jederzeit griffbereit benötigen.</span><span class="sxs-lookup"><span data-stu-id="958a2-495">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="958a2-496">Das Lab-Dokument erfahren Sie, wenn Sie genau, verwenden können wie in der folgenden Abbildung dargestellt.</span><span class="sxs-lookup"><span data-stu-id="958a2-496">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="958a2-497">![Verwendung von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihr Projekt](aspnet-mvc-4-models-and-data-access/_static/image51.png "Codeausschnitte zum Einfügen von Code in Ihr Projekt mit Visual Studio")</span><span class="sxs-lookup"><span data-stu-id="958a2-497">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-models-and-data-access/_static/image51.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="958a2-498">*Verwendung von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihr Projekt*</span><span class="sxs-lookup"><span data-stu-id="958a2-498">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="958a2-499">***Hinzufügen ein Codeausschnitts, die mithilfe der Tastatur (nur c#)***</span><span class="sxs-lookup"><span data-stu-id="958a2-499">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="958a2-500">Platzieren Sie den Cursor, wo möchten Sie den Code einfügen.</span><span class="sxs-lookup"><span data-stu-id="958a2-500">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="958a2-501">Starten Sie den Namen des Ausschnitts (ohne Leerzeichen oder Bindestriche) eingeben.</span><span class="sxs-lookup"><span data-stu-id="958a2-501">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="958a2-502">Beobachten Sie, wie IntelliSense zeigt übereinstimmenden Namen Ausschnitte.</span><span class="sxs-lookup"><span data-stu-id="958a2-502">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="958a2-503">Wählen Sie den richtigen Ausschnitt (oder behalten Sie eingeben, bis der gesamte Ausschnitt Name ausgewählt ist).</span><span class="sxs-lookup"><span data-stu-id="958a2-503">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="958a2-504">Drücken Sie die Tab-Taste zweimal, um den Ausschnitt an der Cursorposition eingefügt.</span><span class="sxs-lookup"><span data-stu-id="958a2-504">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="958a2-505">![Geben Sie den Namen des Ausschnitts](aspnet-mvc-4-models-and-data-access/_static/image52.png "starten Sie den Namen des Ausschnitts eingeben")</span><span class="sxs-lookup"><span data-stu-id="958a2-505">![Start typing the snippet name](aspnet-mvc-4-models-and-data-access/_static/image52.png "Start typing the snippet name")</span></span>

<span data-ttu-id="958a2-506">*Starten Sie den Namen des Ausschnitts eingeben*</span><span class="sxs-lookup"><span data-stu-id="958a2-506">*Start typing the snippet name*</span></span>

<span data-ttu-id="958a2-507">![Drücken Sie Tab, auf den hervorgehobenen Ausschnitt](aspnet-mvc-4-models-and-data-access/_static/image53.png "drücken Sie die Tab hervorgehobenen Ausschnitt auswählen")</span><span class="sxs-lookup"><span data-stu-id="958a2-507">![Press Tab to select the highlighted snippet](aspnet-mvc-4-models-and-data-access/_static/image53.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="958a2-508">*Drücken Sie Tab, um den markierten Ausschnitt auszuwählen*</span><span class="sxs-lookup"><span data-stu-id="958a2-508">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="958a2-509">![Drücken Sie erneut die Tab und den Ausschnitt erweitert](aspnet-mvc-4-models-and-data-access/_static/image54.png "drücken Sie erneut die Tab und den Ausschnitt erweitert")</span><span class="sxs-lookup"><span data-stu-id="958a2-509">![Press Tab again and the snippet will expand](aspnet-mvc-4-models-and-data-access/_static/image54.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="958a2-510">*Drücken Sie erneut die Tab und den Ausschnitt erweitert*</span><span class="sxs-lookup"><span data-stu-id="958a2-510">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="958a2-511">***Hinzufügen ein Codeausschnitts, die mit der Maus (c#, Visual Basic und XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="958a2-511">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="958a2-512">Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen möchten.</span><span class="sxs-lookup"><span data-stu-id="958a2-512">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="958a2-513">Wählen Sie **Ausschnitt einfügen** gefolgt von **eigene Codeausschnitte**.</span><span class="sxs-lookup"><span data-stu-id="958a2-513">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="958a2-514">Wählen Sie die relevanten Ausschnitt aus der Liste, indem Sie darauf klicken.</span><span class="sxs-lookup"><span data-stu-id="958a2-514">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="958a2-515">![Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten](aspnet-mvc-4-models-and-data-access/_static/image55.png "mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten,")</span><span class="sxs-lookup"><span data-stu-id="958a2-515">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-models-and-data-access/_static/image55.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="958a2-516">*Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten*</span><span class="sxs-lookup"><span data-stu-id="958a2-516">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="958a2-517">![Wählen Sie den relevanten Ausschnitt aus der Liste, indem Sie darauf klicken](aspnet-mvc-4-models-and-data-access/_static/image56.png "den relevanten Ausschnitt aus der Liste auswählen, indem Sie darauf klicken")</span><span class="sxs-lookup"><span data-stu-id="958a2-517">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-models-and-data-access/_static/image56.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="958a2-518">*Wählen Sie den relevanten Ausschnitt aus der Liste, indem Sie darauf klicken*</span><span class="sxs-lookup"><span data-stu-id="958a2-518">*Pick the relevant snippet from the list, by clicking on it*</span></span>
