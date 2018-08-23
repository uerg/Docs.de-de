---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
title: ASP.NET MVC 4 – Modelle und Datenzugriff | Microsoft-Dokumentation
author: rick-anderson
description: 'Hinweis: Dieser praktischen Übungseinheit wird davon ausgegangen, dass Sie über grundlegende Kenntnisse von ASP.NET MVC verfügen. Wenn Sie nicht ASP.NET MVC, bevor Sie verwendet haben, empfehlen wir Ihnen über ASP.NET MVC 4 wechseln...'
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 634ea84b-f904-4afe-b71b-49cccef4d9cc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
msc.type: authoredcontent
ms.openlocfilehash: 26896e6ee3c02e8f939296ecbfb8b7d500940765
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835584"
---
# <a name="aspnet-mvc-4-models-and-data-access"></a><span data-ttu-id="0ca0b-104">ASP.NET MVC 4 – Modelle und Datenzugriff</span><span class="sxs-lookup"><span data-stu-id="0ca0b-104">ASP.NET MVC 4 Models and Data Access</span></span>

<span data-ttu-id="0ca0b-105">durch [Web Camps Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="0ca0b-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="0ca0b-106">Herunterladen Sie Web Camps Training Kit</span><span class="sxs-lookup"><span data-stu-id="0ca0b-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="0ca0b-107">Diese praktische Übungseinheit wird vorausgesetzt, dass grundlegende Kenntnisse der **ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-107">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="0ca0b-108">Wenn Sie nicht verwendet haben **ASP.NET MVC** vor, wir empfehlen Ihnen, sich mit Windows Live ID **Grundlagen von ASP.NET MVC 4** Hands-On Lab.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-108">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC 4 Fundamentals** Hands-on Lab.</span></span>

<span data-ttu-id="0ca0b-109">Dieses Lab führt Sie durch die Verbesserungen und neue Funktionen, die zuvor beschriebenen durch Anwenden von geringen Änderungen mit einer Beispiel-Webanwendung, die im Quellordner bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-109">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>

> [!NOTE]
> <span data-ttu-id="0ca0b-110">Alle Beispielcode und Ausschnitte sind im Web Camps Training Kit unter enthalten [Versionen der Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="0ca0b-110">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="0ca0b-111">Das Projekt, das speziell für dieses Lab finden Sie unter [ASP.NET MVC 4-Modelle und Datenzugriff](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess).</span><span class="sxs-lookup"><span data-stu-id="0ca0b-111">The project specific to this lab is available at [ASP.NET MVC 4 Models and Data Access](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess).</span></span>

<span data-ttu-id="0ca0b-112">In **Grundlagen von ASP.NET MVC** praktische Übungseinheit, Sie haben wurde übergeben hartcodierten Daten vom Controller an die Ansichtsvorlagen.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-112">In **ASP.NET MVC Fundamentals** Hands-on Lab, you have been passing hard-coded data from the Controllers to the View templates.</span></span> <span data-ttu-id="0ca0b-113">Um eine echte Webanwendung zu erstellen, Sie sollten jedoch eine echte Datenbank verwenden.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-113">But, in order to build a real Web application, you might want to use a real database.</span></span>

<span data-ttu-id="0ca0b-114">Diese praktische Übungseinheit erfahren Sie, wie mit einer Datenbank-Engine zum Speichern und Abrufen der Daten, die für die Music Store-Anwendung erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-114">This Hands-on Lab will show you how to use a database engine in order to store and retrieve the data needed for the Music Store application.</span></span> <span data-ttu-id="0ca0b-115">Damit dies erreicht wird, Sie beginnen mit einer vorhandenen Datenbank und das Entity Data Model daraus erstellen.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-115">To accomplish that, you will start with an existing database and create the Entity Data Model from it.</span></span> <span data-ttu-id="0ca0b-116">In dieser Übung werden Sie erfüllen die **Database First** Ansatz als auch die **Code First** Ansatz.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-116">Throughout this lab, you will meet the **Database First** approach as well as the **Code First** approach.</span></span>

<span data-ttu-id="0ca0b-117">Allerdings können Sie auch verwenden die **Model First** Ansatz, erstellen Sie das gleiche Modell mit den Tools und klicken Sie dann die Datenbank aus zu generieren.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-117">However, you can also use the **Model First** approach, create the same model using the tools, and then generate the database from it.</span></span>

<span data-ttu-id="0ca0b-118">![Datenbank erste im Vergleich zu Model First](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First Visual Studio. Model First")</span><span class="sxs-lookup"><span data-stu-id="0ca0b-118">![Database First vs. Model First](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First vs. Model First")</span></span>

<span data-ttu-id="0ca0b-119">*Datenbank erste im Vergleich zu Model First*</span><span class="sxs-lookup"><span data-stu-id="0ca0b-119">*Database First vs. Model First*</span></span>

<span data-ttu-id="0ca0b-120">Nach dem Generieren des Modells, werden Sie die entsprechenden Anpassungen in die StoreController können Sie die Store-Ansichten mit den Daten aus der Datenbank anstelle von hartcodierten Daten bereitstellen vornehmen.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-120">After generating the Model, you will make the proper adjustments in the StoreController to provide the Store Views with the data taken from the database, instead of using hard-coded data.</span></span> <span data-ttu-id="0ca0b-121">Sie benötigen keine Änderung an die Ansichtsvorlagen vornehmen, da die StoreController die gleichen ViewModels, die Ansichtsvorlagen zurückgegeben wird zwar dieses Mal die Daten aus der Datenbank stammen.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-121">You will not need to make any change to the View templates because the StoreController will be returning the same ViewModels to the View templates, although this time the data will come from the database.</span></span>

<span data-ttu-id="0ca0b-122">**Code First-Ansatz**</span><span class="sxs-lookup"><span data-stu-id="0ca0b-122">**The Code First Approach**</span></span>

<span data-ttu-id="0ca0b-123">Der Code First-Ansatz ermöglicht uns, definieren das Modell aus dem Code, ohne das Generieren von Klassen, die in der Regel mit dem Framework gekoppelt sind.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-123">The Code First approach allows us to define the model from the code without generating classes that are generally coupled with the framework.</span></span>

<span data-ttu-id="0ca0b-124">Im Code zuerst Modellobjekte werden mit definiert POCOs, &quot;Plain Old CLR Objects&quot;.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-124">In code first, model objects are defined with POCOs, &quot;Plain Old CLR Objects&quot;.</span></span> <span data-ttu-id="0ca0b-125">POCOs sind einfache einfache Klassen, die keine Vererbung und implementieren keine Schnittstellen.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-125">POCOs are simple plain classes that have no inheritance and do not implement interfaces.</span></span> <span data-ttu-id="0ca0b-126">Wir können die Datenbank automatisch generieren, von ihnen oder wir können eine vorhandene Datenbank verwenden und die klassenzuordnung aus dem Code zu generieren.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-126">We can automatically generate the database from them, or we can use an existing database and generate the class mapping from the code.</span></span>

<span data-ttu-id="0ca0b-127">Die Vorteile der Verwendung dieses Ansatzes ist, dass das Modell unabhängig von das persistenzframework (in diesem Fall Entity Framework), bleibt, wie die POCOs Klassen nicht mit der Mapping-Framework gekoppelt sind.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-127">The benefits of using this approach is that the Model remains independent from the persistence framework (in this case, Entity Framework), as the POCOs classes are not coupled with the mapping framework.</span></span>

> [!NOTE]
> <span data-ttu-id="0ca0b-128">Diese Testumgebung basiert auf ASP.NET MVC 4 und eine Version der Music Store-beispielanwendung angepasst und entsprechend nur die Funktionen, die in dieser praktischen Übungseinheit gezeigt minimiert.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-128">This Lab is based on ASP.NET MVC 4 and a version of the Music Store sample application customized and minimized to fit only the features shown in this Hands-On Lab.</span></span>
> 
> <span data-ttu-id="0ca0b-129">Wenn Sie die gesamte kennenlernen möchten **Music Store** lernprogrammanwendung, die Sie es im finden [MVC-Musik-Store](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="0ca0b-129">If you wish to explore the whole **Music Store** tutorial application you can find it in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>


<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="0ca0b-130">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="0ca0b-130">Prerequisites</span></span>

<span data-ttu-id="0ca0b-131">Sie benötigen Folgendes, um diese testumgebung abzuschließen:</span><span class="sxs-lookup"><span data-stu-id="0ca0b-131">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="0ca0b-132">[Microsoft Visual Studio Express 2012 für Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) oder sogar eine höhere (Lesen [Anhang A](#AppendixA) Anleitungen zur Installation).</span><span class="sxs-lookup"><span data-stu-id="0ca0b-132">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="0ca0b-133">Setup</span><span class="sxs-lookup"><span data-stu-id="0ca0b-133">Setup</span></span>

<span data-ttu-id="0ca0b-134">**Installieren von Codeausschnitten**</span><span class="sxs-lookup"><span data-stu-id="0ca0b-134">**Installing Code Snippets**</span></span>

<span data-ttu-id="0ca0b-135">Der Einfachheit halber ist Großteil des Codes, die entlang dieser Übungseinheit verwaltet werden soll als Codeausschnitte für Visual Studio verfügbar.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-135">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="0ca0b-136">So installieren Sie die Codeausschnitte ausführen **.\Source\Setup\CodeSnippets.vsi** Datei.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-136">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="0ca0b-137">Wenn Sie nicht mit dem Visual Studio Code Snippets und zu erfahren, wie Sie deren Verwendung vertraut sind, sehen Sie sich im Anhang in diesem Dokument &quot; [Anhang C: Verwenden von Code Snippets](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-137">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="0ca0b-138">Übungen</span><span class="sxs-lookup"><span data-stu-id="0ca0b-138">Exercises</span></span>

<span data-ttu-id="0ca0b-139">Diese praktische Übungseinheit besteht aus durch die folgenden Übungen:</span><span class="sxs-lookup"><span data-stu-id="0ca0b-139">This Hands-on Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="0ca0b-140">Übung 1: Hinzufügen einer Datenbank</span><span class="sxs-lookup"><span data-stu-id="0ca0b-140">Exercise 1: Adding a Database</span></span>](#Exercise1)
2. [<span data-ttu-id="0ca0b-141">Übung 2: Erstellen einer Datenbank mithilfe von Code First</span><span class="sxs-lookup"><span data-stu-id="0ca0b-141">Exercise 2: Creating a Database using Code First</span></span>](#Exercise2)
3. [<span data-ttu-id="0ca0b-142">Übung 3: Abfragen der Datenbank mit Parametern</span><span class="sxs-lookup"><span data-stu-id="0ca0b-142">Exercise 3: Querying the Database with Parameters</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="0ca0b-143">Jede Übung umfasst eine **End** Ordner mit der resultierenden Lösung, die Sie nach Abschluss der Übungen abrufen soll.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-143">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="0ca0b-144">Sie können diese Lösung als Leitfaden verwenden, bei Bedarf zusätzliche Hilfe bei der die Übungen durcharbeiten.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-144">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="0ca0b-145">Geschätzte Zeit für diese testumgebung abzuschließen: **35 Minuten**.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-145">Estimated time to complete this lab: **35 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Adding_a_Database"></a>
### <a name="exercise-1-adding-a-database"></a><span data-ttu-id="0ca0b-146">Übung 1: Hinzufügen einer Datenbank</span><span class="sxs-lookup"><span data-stu-id="0ca0b-146">Exercise 1: Adding a Database</span></span>

<span data-ttu-id="0ca0b-147">In dieser Übung lernen Sie, wie Sie eine Datenbank mit den Tabellen der Music Store-Anwendung, die Lösung zum Nutzen der Daten hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-147">In this exercise, you will learn how to add a database with the tables of the MusicStore application to the solution in order to consume its data.</span></span> <span data-ttu-id="0ca0b-148">Sobald die Datenbank mit dem Modell generiert, und der Projektmappe hinzugefügt, ändern Sie die StoreController-Klasse, um die ansichtsvorlage mit den Daten aus der Datenbank, anstatt hartcodierte Werte bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-148">Once the database is generated with the model, and added to the solution, you will modify the StoreController class to provide the View template with the data taken from the database, instead of using hard-coded values.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Adding_a_Database"></a>
#### <a name="task-1---adding-a-database"></a><span data-ttu-id="0ca0b-149">Aufgabe 1: Hinzufügen einer Datenbank</span><span class="sxs-lookup"><span data-stu-id="0ca0b-149">Task 1 - Adding a Database</span></span>

<span data-ttu-id="0ca0b-150">In dieser Aufgabe fügen Sie eine bereits erstellte Datenbank mit die Haupttabellen der Music Store-Anwendung, die Lösung.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-150">In this task, you will add an already created database with the main tables of the MusicStore application to the solution.</span></span>

1. <span data-ttu-id="0ca0b-151">Öffnen der **beginnen** Lösung controllerarbeitsverzeichnis **Quelle/Ex1-AddingADatabaseDBFirst/Anfang/** Ordner.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-151">Open the **Begin** solution located at **Source/Ex1-AddingADatabaseDBFirst/Begin/** folder.</span></span>

   1. <span data-ttu-id="0ca0b-152">Sie müssen einige fehlende NuGet-Pakete herunterladen. bevor Sie fortfahren.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-152">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="0ca0b-153">Zu diesem Zweck klicken Sie auf die **Projekt** Menü **NuGet-Pakete verwalten**.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-153">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="0ca0b-154">In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-154">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="0ca0b-155">Abschließend erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-155">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="0ca0b-156">Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken in Ihrem Projekt, Versand Verringern der Projektgröße.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-156">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="0ca0b-157">Mit NuGet Power Tools können werden durch Angabe von Versionen des Pakets in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, die, das Sie das Projekt ausführen, können.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-157">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="0ca0b-158">Deshalb müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit wird.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-158">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="0ca0b-159">Hinzufügen **MvcMusicStore** Datenbankdatei.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-159">Add **MvcMusicStore** database file.</span></span> <span data-ttu-id="0ca0b-160">In dieser praktischen Übungseinheit verwenden Sie eine bereits erstellte Datenbank mit dem Namen **MvcMusicStore.mdf**.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-160">In this Hands-on Lab, you will use an already created database called **MvcMusicStore.mdf**.</span></span> <span data-ttu-id="0ca0b-161">Zu diesem Zweck Maustaste **App\_Daten** Ordner, zeigen Sie auf **hinzufügen** , und klicken Sie dann auf **vorhandenes Element**.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-161">To do that, right-click **App\_Data** folder, point to **Add** and then click **Existing Item**.</span></span> <span data-ttu-id="0ca0b-162">Navigieren Sie zu **\Source\Assets** , und wählen Sie die **MvcMusicStore.mdf** Datei.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-162">Browse to **\Source\Assets** and select the **MvcMusicStore.mdf** file.</span></span>

    <span data-ttu-id="0ca0b-163">![Hinzufügen eines vorhandenen Elements](aspnet-mvc-4-models-and-data-access/_static/image2.png "ein vorhandenes Element hinzufügen")</span><span class="sxs-lookup"><span data-stu-id="0ca0b-163">![Adding an Existing Item](aspnet-mvc-4-models-and-data-access/_static/image2.png "Adding an Existing Item")</span></span>

    <span data-ttu-id="0ca0b-164">*Hinzufügen eines vorhandenen Elements*</span><span class="sxs-lookup"><span data-stu-id="0ca0b-164">*Adding an Existing Item*</span></span>

    <span data-ttu-id="0ca0b-165">![Datenbankdatei MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf-Datenbankdatei")</span><span class="sxs-lookup"><span data-stu-id="0ca0b-165">![MvcMusicStore.mdf database file](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf database file")</span></span>

    <span data-ttu-id="0ca0b-166">*MvcMusicStore.mdf-Datenbankdatei*</span><span class="sxs-lookup"><span data-stu-id="0ca0b-166">*MvcMusicStore.mdf database file*</span></span>

    <span data-ttu-id="0ca0b-167">Die Datenbank wurde dem Projekt hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-167">The database has been added to the project.</span></span> <span data-ttu-id="0ca0b-168">Auch wenn die Datenbank in der Projektmappe befindet, können Sie Abfragen und aktualisieren Sie sie, wie es in einen anderen Datenbankserver gehostet wurde.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-168">Even when the database is located inside the solution, you can query and update it as it was hosted in a different database server.</span></span>

    <span data-ttu-id="0ca0b-169">![MvcMusicStore-Datenbank im Projektmappen-Explorer](aspnet-mvc-4-models-and-data-access/_static/image4.png "MvcMusicStore-Datenbank im Projektmappen-Explorer")</span><span class="sxs-lookup"><span data-stu-id="0ca0b-169">![MvcMusicStore database in Solution Explorer](aspnet-mvc-4-models-and-data-access/_static/image4.png "MvcMusicStore database in Solution Explorer")</span></span>

    <span data-ttu-id="0ca0b-170">*MvcMusicStore-Datenbank im Projektmappen-Explorer*</span><span class="sxs-lookup"><span data-stu-id="0ca0b-170">*MvcMusicStore database in Solution Explorer*</span></span>
3. <span data-ttu-id="0ca0b-171">Überprüfen Sie die Verbindung mit der Datenbank aus.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-171">Verify the connection to the database.</span></span> <span data-ttu-id="0ca0b-172">Zu diesem Zweck doppelklicken Sie auf **MvcMusicStore.mdf** zum Herstellen einer Verbindung.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-172">To do this, double-click **MvcMusicStore.mdf** to establish a connection.</span></span>

    <span data-ttu-id="0ca0b-173">![Herstellen einer Verbindung mit MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "MvcMusicStore.mdf mit")</span><span class="sxs-lookup"><span data-stu-id="0ca0b-173">![Connecting to MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "Connecting to MvcMusicStore.mdf")</span></span>

    <span data-ttu-id="0ca0b-174">*Herstellen einer Verbindung mit MvcMusicStore.mdf*</span><span class="sxs-lookup"><span data-stu-id="0ca0b-174">*Connecting to MvcMusicStore.mdf*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_a_Data_Model"></a>
#### <a name="task-2---creating-a-data-model"></a><span data-ttu-id="0ca0b-175">Aufgabe 2: Erstellen eines Datenmodells</span><span class="sxs-lookup"><span data-stu-id="0ca0b-175">Task 2 - Creating a Data Model</span></span>

<span data-ttu-id="0ca0b-176">In dieser Aufgabe erstellen Sie ein Datenmodell für die Interaktion mit der Datenbank, die in der vorherigen Aufgabe hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-176">In this task, you will create a data model to interact with the database added in the previous task.</span></span>

1. <span data-ttu-id="0ca0b-177">Erstellen Sie ein Datenmodell, die die Datenbank darstellt.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-177">Create a data model that will represent the database.</span></span> <span data-ttu-id="0ca0b-178">Dazu klicken Sie im Projektmappen-Explorer mit der rechten Maustaste die **Modelle** Ordner, zeigen Sie auf **hinzufügen** , und klicken Sie dann auf **neues Element**.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-178">To do this, in Solution Explorer right-click the **Models** folder, point to **Add** and then click **New Item**.</span></span> <span data-ttu-id="0ca0b-179">In der **neues Element hinzufügen** wählen Sie im Dialogfeld die **Daten** Vorlage und dann die **ADO.NET Entity Data Model** Element.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-179">In the **Add New Item** dialog, select the **Data** template and then the **ADO.NET Entity Data Model** item.</span></span> <span data-ttu-id="0ca0b-180">Ändern Sie den Namen des Daten-Modell zu **StoreDB.edmx** , und klicken Sie auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-180">Change the data model name to **StoreDB.edmx** and click **Add**.</span></span>

    <span data-ttu-id="0ca0b-181">![Hinzufügen von StoreDB ADO.NET Entity Data Model](aspnet-mvc-4-models-and-data-access/_static/image6.png "StoreDB ADO.NET Entity Data Model hinzufügen")</span><span class="sxs-lookup"><span data-stu-id="0ca0b-181">![Adding the StoreDB ADO.NET Entity Data Model](aspnet-mvc-4-models-and-data-access/_static/image6.png "Adding the StoreDB ADO.NET Entity Data Model")</span></span>

    <span data-ttu-id="0ca0b-182">*Hinzufügen von StoreDB ADO.NET Entity Data Model*</span><span class="sxs-lookup"><span data-stu-id="0ca0b-182">*Adding the StoreDB ADO.NET Entity Data Model*</span></span>
2. <span data-ttu-id="0ca0b-183">Die **Entity Data Model-Assistenten** wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-183">The **Entity Data Model Wizard** will appear.</span></span> <span data-ttu-id="0ca0b-184">Dieser Assistent führt Sie durch die Erstellung der Modellschicht.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-184">This wizard will guide you through the creation of the model layer.</span></span> <span data-ttu-id="0ca0b-185">Da das Modell basierend auf der vorhandenen Datenbank Recentyl hinzugefügt erstellt werden soll, wählen Sie **aus Datenbank generieren** , und klicken Sie auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-185">Since the model should be created based on the existing database recentyl added, select **Generate from database** and click **Next**.</span></span>

    <span data-ttu-id="0ca0b-186">![Auswählen des Modellinhalts](aspnet-mvc-4-models-and-data-access/_static/image7.png "Modellinhalt auswählen")</span><span class="sxs-lookup"><span data-stu-id="0ca0b-186">![Choosing the model content](aspnet-mvc-4-models-and-data-access/_static/image7.png "Choosing the model content")</span></span>

    <span data-ttu-id="0ca0b-187">*Den Modellinhalt auswählen*</span><span class="sxs-lookup"><span data-stu-id="0ca0b-187">*Choosing the model content*</span></span>
3. <span data-ttu-id="0ca0b-188">Da Sie ein Modell aus einer Datenbank generieren, müssen Sie die zu verwendende Verbindung an.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-188">Since you are generating a model from a database, you will need to specify the connection to use.</span></span> <span data-ttu-id="0ca0b-189">Klicken Sie auf **neue Verbindung**.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-189">Click **New Connection**.</span></span>
4. <span data-ttu-id="0ca0b-190">Wählen Sie **Microsoft SQL Server-Datenbankdatei** , und klicken Sie auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-190">Select **Microsoft SQL Server Database File** and click **Continue**.</span></span>

    <span data-ttu-id="0ca0b-191">![Wählen Sie die Datenquelle](aspnet-mvc-4-models-and-data-access/_static/image8.png "Datenquelle auswählen")</span><span class="sxs-lookup"><span data-stu-id="0ca0b-191">![Choose data source](aspnet-mvc-4-models-and-data-access/_static/image8.png "Choose data source")</span></span>

    <span data-ttu-id="0ca0b-192">*Wählen Sie die Daten-Dialogfeld "Quelle"*</span><span class="sxs-lookup"><span data-stu-id="0ca0b-192">*Choose data source dialog*</span></span>
5. <span data-ttu-id="0ca0b-193">Klicken Sie auf **Durchsuchen** , und wählen Sie die Datenbank **MvcMusicStore.mdf** Sie befindet sich der **App\_Daten** Ordner, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-193">Click **Browse** and select the database **MvcMusicStore.mdf** you located in the **App\_Data** folder and click **OK**.</span></span>

    <span data-ttu-id="0ca0b-194">![Verbindungseigenschaften](aspnet-mvc-4-models-and-data-access/_static/image9.png "Verbindungseigenschaften")</span><span class="sxs-lookup"><span data-stu-id="0ca0b-194">![Connection properties](aspnet-mvc-4-models-and-data-access/_static/image9.png "Connection properties")</span></span>

    <span data-ttu-id="0ca0b-195">*Verbindungseigenschaften*</span><span class="sxs-lookup"><span data-stu-id="0ca0b-195">*Connection properties*</span></span>
6. <span data-ttu-id="0ca0b-196">Die generierte Klasse sollten den gleichen Namen wie die Verbindungszeichenfolge für die Entität haben, so ändern Sie den Namen **MusicStoreEntities** , und klicken Sie auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-196">The generated class should have the same name as the entity connection string, so change its name to **MusicStoreEntities** and click **Next**.</span></span>

    <span data-ttu-id="0ca0b-197">![Wählen die Datenverbindung](aspnet-mvc-4-models-and-data-access/_static/image10.png "die Datenverbindung auswählen")</span><span class="sxs-lookup"><span data-stu-id="0ca0b-197">![Choosing the data connection](aspnet-mvc-4-models-and-data-access/_static/image10.png "Choosing the data connection")</span></span>

    <span data-ttu-id="0ca0b-198">*Die Datenverbindung auswählen*</span><span class="sxs-lookup"><span data-stu-id="0ca0b-198">*Choosing the data connection*</span></span>
7. <span data-ttu-id="0ca0b-199">Wählen Sie die Datenbankobjekte verwenden.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-199">Choose the database objects to use.</span></span> <span data-ttu-id="0ca0b-200">Da das Entity Model nur die Tabellen der Datenbank verwendet wird, wählen Sie die **Tabellen** aus, und stellen Sie sicher, dass die **Fremdschlüsselspalten in das Modell einzuschließen** und **in den Singular oder Plural setzen generiert Objektnamen** Optionen werden ebenfalls ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-200">As the Entity Model will use just the database's tables, select the **Tables** option, and make sure that the **Include foreign key columns in the model** and **Pluralize or singularize generated object names** options are also selected.</span></span> <span data-ttu-id="0ca0b-201">Ändern Sie den Modell-Namespace zu **MvcMusicStore.Model** , und klicken Sie auf **Fertig stellen**.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-201">Change the Model Namespace to **MvcMusicStore.Model** and click **Finish**.</span></span>

    <span data-ttu-id="0ca0b-202">![Auswählen der Datenbankobjekte](aspnet-mvc-4-models-and-data-access/_static/image11.png "Datenbankobjekte auswählen")</span><span class="sxs-lookup"><span data-stu-id="0ca0b-202">![Choosing the database objects](aspnet-mvc-4-models-and-data-access/_static/image11.png "Choosing the database objects")</span></span>

    <span data-ttu-id="0ca0b-203">*Datenbankobjekte auswählen*</span><span class="sxs-lookup"><span data-stu-id="0ca0b-203">*Choosing the database objects*</span></span>

    > [!NOTE]
    > <span data-ttu-id="0ca0b-204">Wenn eine Sicherheitswarnung-Dialogfeld angezeigt wird, klicken Sie auf **OK** führen Sie die Vorlage und die Klassen für die Modellelemente zu generieren.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-204">If a Security Warning dialog is shown, click **OK** to run the template and generate the classes for the model entities.</span></span>
8. <span data-ttu-id="0ca0b-205">Einem Entitätsdiagramm für die Datenbank wird angezeigt, während eine separate Klasse, die ordnet jede Tabelle in der Datenbank erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-205">An entity diagram for the database will appear, while a separate class that maps each table to the database will be created.</span></span> <span data-ttu-id="0ca0b-206">Z. B. die **Alben** Tabelle wird dargestellt werden, indem ein **Album** -Klasse, in dem jede Spalte in der Tabelle einer Klasseneigenschaft zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-206">For example, the **Albums** table will be represented by an **Album** class, where each column in the table will map to a class property.</span></span> <span data-ttu-id="0ca0b-207">Dadurch können Sie Abfragen und Arbeiten mit Objekten, die Zeilen in der Datenbank darstellen.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-207">This will allow you to query and work with objects that represent rows in the database.</span></span>

    <span data-ttu-id="0ca0b-208">![Entitätsdiagramm](aspnet-mvc-4-models-and-data-access/_static/image12.png "Entitätsdiagramm")</span><span class="sxs-lookup"><span data-stu-id="0ca0b-208">![Entity diagram](aspnet-mvc-4-models-and-data-access/_static/image12.png "Entity diagram")</span></span>

    <span data-ttu-id="0ca0b-209">*Entitätsdiagramm*</span><span class="sxs-lookup"><span data-stu-id="0ca0b-209">*Entity diagram*</span></span>

    > [!NOTE]
    > <span data-ttu-id="0ca0b-210">Die T4-Vorlagen (.tt) führen Sie Code zum Generieren der Entitäten und überschreibt die vorhandenen Klassen mit dem gleichen Namen.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-210">The T4 templates (.tt) run code to generate the entities classes and will overwrite the existing classes with the same name.</span></span> <span data-ttu-id="0ca0b-211">In diesem Beispiel die Klassen &quot;Album&quot;, &quot;"Genre"&quot; und &quot;Interpreten&quot; überschrieben wurden, mit dem generierten Code.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-211">In this example, the classes &quot;Album&quot;, &quot;Genre&quot; and &quot;Artist&quot; were overwritten with the generated code.</span></span>


<a id="Ex1Task3"></a>

<a id="Task_3_-_Building_the_Application"></a>
#### <a name="task-3---building-the-application"></a><span data-ttu-id="0ca0b-212">Aufgabe 3: Erstellen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="0ca0b-212">Task 3 - Building the Application</span></span>

<span data-ttu-id="0ca0b-213">In dieser Aufgabe Sie prüft, dass zwar der Erstellung der Modelle wurden entfernt. die **Album**, **"Genre"** und **Interpreten** Modellklassen und das Projekt erstellt wurde erfolgreich mithilfe von die neuen Data Model-Klassen.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-213">In this task, you will check that, although the model generation have removed the **Album**, **Genre** and **Artist** model classes, the project builds successfully by using the new data model classes.</span></span>

1. <span data-ttu-id="0ca0b-214">Erstellen Sie das Projekt durch Auswählen der **erstellen** Menüelement und dann **erstellen MvcMusicStore**.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-214">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>

    <span data-ttu-id="0ca0b-215">![Beim Erstellen des Projekts](aspnet-mvc-4-models-and-data-access/_static/image13.png "beim Erstellen des Projekts")</span><span class="sxs-lookup"><span data-stu-id="0ca0b-215">![Building the project](aspnet-mvc-4-models-and-data-access/_static/image13.png "Building the project")</span></span>

    <span data-ttu-id="0ca0b-216">*Beim Erstellen des Projekts*</span><span class="sxs-lookup"><span data-stu-id="0ca0b-216">*Building the project*</span></span>
2. <span data-ttu-id="0ca0b-217">Das Projekt wird erfolgreich erstellt.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-217">The project builds successfully.</span></span> <span data-ttu-id="0ca0b-218">Warum funktioniert es weiterhin?</span><span class="sxs-lookup"><span data-stu-id="0ca0b-218">Why does it still work?</span></span> <span data-ttu-id="0ca0b-219">Es funktioniert, da die Datenbanktabellen Felder verfügen, die die Eigenschaften, die Sie verwendet haben, in den entfernten Klassen **Album** und **"Genre"**.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-219">It works because the database tables have fields that include the properties that you were using in the removed classes **Album** and **Genre**.</span></span>

    <span data-ttu-id="0ca0b-220">![Builds erfolgreich](aspnet-mvc-4-models-and-data-access/_static/image14.png "Builds erfolgreich")</span><span class="sxs-lookup"><span data-stu-id="0ca0b-220">![Builds succeeded](aspnet-mvc-4-models-and-data-access/_static/image14.png "Builds succeeded")</span></span>

    <span data-ttu-id="0ca0b-221">*Builds erfolgreich*</span><span class="sxs-lookup"><span data-stu-id="0ca0b-221">*Builds succeeded*</span></span>
3. <span data-ttu-id="0ca0b-222">Der Designer die Entitäten in einem Diagramm-Format angezeigt wird, sind eigentlich c#-Klassen.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-222">While the designer displays the entities in a diagram format, they are really C# classes.</span></span> <span data-ttu-id="0ca0b-223">Erweitern Sie die **StoreDB.edmx** Knoten im Projektmappen-Explorer und dann **StoreDB.tt**, sehen Sie die neuen generierten Entitäten.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-223">Expand the **StoreDB.edmx** node in the Solution Explorer and then **StoreDB.tt**, you will see the new generated entities.</span></span>

    <span data-ttu-id="0ca0b-224">![Generierte Dateien](aspnet-mvc-4-models-and-data-access/_static/image15.png "generierte Dateien")</span><span class="sxs-lookup"><span data-stu-id="0ca0b-224">![Generated files](aspnet-mvc-4-models-and-data-access/_static/image15.png "Generated files")</span></span>

    <span data-ttu-id="0ca0b-225">*Generierte Dateien*</span><span class="sxs-lookup"><span data-stu-id="0ca0b-225">*Generated files*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a><span data-ttu-id="0ca0b-226">Aufgabe 4: Abfragen der Datenbank</span><span class="sxs-lookup"><span data-stu-id="0ca0b-226">Task 4 - Querying the Database</span></span>

<span data-ttu-id="0ca0b-227">In dieser Aufgabe aktualisieren Sie die StoreController-Klasse, anstatt hartcodierte Daten, es die Datenbank zum Abrufen der Informationen abfragt.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-227">In this task, you will update the StoreController class so that, instead of using hardcoded data, it will query the database to retrieve the information.</span></span>

1. <span data-ttu-id="0ca0b-228">Open **Controllers\StoreController.cs** und fügen Sie das folgende Feld hinzu, auf die Klasse, um eine Instanz von aufzunehmen der **MusicStoreEntities** Klasse, mit dem Namen **StoreDB**:</span><span class="sxs-lookup"><span data-stu-id="0ca0b-228">Open **Controllers\StoreController.cs** and add the following field to the class to hold an instance of the **MusicStoreEntities** class, named **storeDB**:</span></span>

    <span data-ttu-id="0ca0b-229">(Codeausschnitt - *Modelle und Datenzugriff – Ex1 StoreDB*)</span><span class="sxs-lookup"><span data-stu-id="0ca0b-229">(Code Snippet - *Models And Data Access - Ex1 storeDB*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample1.cs)]
2. <span data-ttu-id="0ca0b-230">Die **MusicStoreEntities** Klasse macht eine Auflistungseigenschaft für jede Tabelle in der Datenbank verfügbar.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-230">The **MusicStoreEntities** class exposes a collection property for each table in the database.</span></span> <span data-ttu-id="0ca0b-231">Update **Durchsuchen** Aktionsmethode zum Abrufen einer "Genre" mit allen dem **Alben**.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-231">Update **Browse** action method to retrieve a Genre with all of the **Albums**.</span></span>

    <span data-ttu-id="0ca0b-232">(Codeausschnitt - *Modelle und Datenzugriff - Ex1 Store durchsuchen*)</span><span class="sxs-lookup"><span data-stu-id="0ca0b-232">(Code Snippet - *Models And Data Access - Ex1 Store Browse*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample2.cs)]

    > [!NOTE]
    > <span data-ttu-id="0ca0b-233">Verwenden Sie eine Funktion von .NET namens **LINQ** (Language integrated Query) zum Schreiben von stark typisierten-Abfrageausdrücken für diese Sammlungen - zurück und führen Sie Code für die Datenbank-Objekten, die Sie programmieren können, vor.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-233">You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.</span></span>
    > 
    > <span data-ttu-id="0ca0b-234">Weitere Informationen über LINQ finden Sie auf die [Msdn-Website](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).</span><span class="sxs-lookup"><span data-stu-id="0ca0b-234">For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).</span></span>
3. <span data-ttu-id="0ca0b-235">Update **Index** Aktionsmethode, die alle Genres abrufen.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-235">Update **Index** action method to retrieve all the genres.</span></span>

    <span data-ttu-id="0ca0b-236">(Codeausschnitt - *Modelle und Datenzugriff - Ex1 Store Index*)</span><span class="sxs-lookup"><span data-stu-id="0ca0b-236">(Code Snippet - *Models And Data Access - Ex1 Store Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample3.cs)]
4. <span data-ttu-id="0ca0b-237">Update **Index** Aktionsmethode, die alle Genres abrufen und Transformieren der Auflistung in einer Liste.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-237">Update **Index** action method to retrieve all the genres and transform the collection to a list.</span></span>

    <span data-ttu-id="0ca0b-238">(Codeausschnitt - *Modelle und Datenzugriff - Ex1 Store GenreMenu*)</span><span class="sxs-lookup"><span data-stu-id="0ca0b-238">(Code Snippet - *Models And Data Access - Ex1 Store GenreMenu*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample4.cs)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="0ca0b-239">Aufgabe 5: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="0ca0b-239">Task 5 - Running the Application</span></span>

<span data-ttu-id="0ca0b-240">In dieser Aufgabe überprüfen, dass die Indexseite der Store jetzt Genres in die Datenbank anstatt durch die hartcodierte diejenigen gespeichert angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-240">In this task, you will check that the Store Index page will now display the Genres stored in the database instead of the hardcoded ones.</span></span> <span data-ttu-id="0ca0b-241">Es ist nicht erforderlich, um die ansichtsvorlage zu ändern, da die **StoreController** gibt die gleichen Entitäten wie zuvor zwar dieses Mal die Daten aus der Datenbank stammen.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-241">There is no need to change the View template because the **StoreController** is returning the same entities as before, although this time the data will come from the database.</span></span>

1. <span data-ttu-id="0ca0b-242">Erneutes Erstellen von Projektmappen und drücken Sie **F5** zum Ausführen der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-242">Rebuild the solution and press **F5** to run the Application.</span></span>
2. <span data-ttu-id="0ca0b-243">Das Projekt, die auf der Startseite wird gestartet.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-243">The project starts in the Home page.</span></span> <span data-ttu-id="0ca0b-244">Überprüfen Sie, ob Sie im Menü des **Genres** ist nicht mehr eine feste Liste, und die Daten direkt aus der Datenbank abgerufen.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-244">Verify that the menu of **Genres** is no longer a hardcoded list, and the data is directly retrieved from the database.</span></span>

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image16.png)

    <span data-ttu-id="0ca0b-246">*Durchsuchen von Genres aus der Datenbank*</span><span class="sxs-lookup"><span data-stu-id="0ca0b-246">*Browsing Genres from the database*</span></span>
3. <span data-ttu-id="0ca0b-247">Nun wechseln Sie zu jedem "Genre", und stellen Sie sicher, dass die Alben aus der Datenbank aufgefüllt werden.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-247">Now browse to any genre and verify the albums are populated from database.</span></span>

    <span data-ttu-id="0ca0b-248">![Durchsuchen Sie Alben aus der Datenbank](aspnet-mvc-4-models-and-data-access/_static/image17.png "durchsuchen Alben aus der Datenbank")</span><span class="sxs-lookup"><span data-stu-id="0ca0b-248">![Browsing Albums from the database](aspnet-mvc-4-models-and-data-access/_static/image17.png "Browsing Albums from the database")</span></span>

    <span data-ttu-id="0ca0b-249">*Durchsuchen Alben aus der Datenbank*</span><span class="sxs-lookup"><span data-stu-id="0ca0b-249">*Browsing Albums from the database*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Database_Using_Code_First"></a>
### <a name="exercise-2-creating-a-database-using-code-first"></a><span data-ttu-id="0ca0b-250">Übung 2: Erstellen einer Datenbank, die mithilfe von Code First</span><span class="sxs-lookup"><span data-stu-id="0ca0b-250">Exercise 2: Creating a Database Using Code First</span></span>

<span data-ttu-id="0ca0b-251">In dieser Übung lernen Sie, wie Sie die Code First-Ansatz zu verwenden, um eine Datenbank mit den Tabellen der Music Store-Anwendung zu erstellen, und wie auf ihre Daten zugreifen.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-251">In this exercise, you will learn how to use the Code First approach to create a database with the tables of the MusicStore application, and how to access its data.</span></span>

<span data-ttu-id="0ca0b-252">Nachdem das Modell generiert wird, ändern Sie die StoreController, um die ansichtsvorlage mit den Daten aus der Datenbank, anstatt hartcodierte Werte bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-252">Once the model is generated, you will modify the StoreController to provide the View template with the data taken from the database, instead of using hardcoded values.</span></span>

> [!NOTE]
> <span data-ttu-id="0ca0b-253">Wenn Sie die Übung 1 abgeschlossen haben und bereits mit dem Database First-Ansatz gearbeitet haben, lernen Sie jetzt die gleichen Ergebnisse mit einem anderen Prozess abrufen.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-253">If you have completed Exercise 1 and have already worked with the Database First approach, you will now learn how to get the same results with a different process.</span></span> <span data-ttu-id="0ca0b-254">Aufgaben, die gemeinsamen Übung 1 sind wurden gekennzeichnet, um Ihre Lesbarkeit zu erhöhen.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-254">The tasks that are in common with Exercise 1 have been marked to make your reading easier.</span></span> <span data-ttu-id="0ca0b-255">Wenn Sie Übung 1 nicht abgeschlossen, aber der Code First-Ansatz erfahren möchten, können Sie aus dieser Übung beginnen und erhalten eine vollständige Abdeckung des Themas.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-255">If you have not completed Exercise 1 but would like to learn the Code First approach, you can start from this exercise and get a full coverage of the topic.</span></span>


<a id="Ex2Task1"></a>

<a id="Task_1_-_Populating_Sample_Data"></a>
#### <a name="task-1---populating-sample-data"></a><span data-ttu-id="0ca0b-256">Aufgabe 1: Auffüllen der Beispieldaten</span><span class="sxs-lookup"><span data-stu-id="0ca0b-256">Task 1 - Populating Sample Data</span></span>

<span data-ttu-id="0ca0b-257">In dieser Aufgabe füllen Sie die Datenbank mit Beispieldaten bei der gestufte mit Code First-Erstellung.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-257">In this task, you will populate the database with sample data when it is intially created using Code-First.</span></span>

1. <span data-ttu-id="0ca0b-258">Öffnen der **beginnen** Lösung controllerarbeitsverzeichnis **Quelle/Ex2-CreatingADatabaseCodeFirst/Anfang/** Ordner.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-258">Open the **Begin** solution located at **Source/Ex2-CreatingADatabaseCodeFirst/Begin/** folder.</span></span> <span data-ttu-id="0ca0b-259">Andernfalls können Sie weiterhin, verwenden die **End** Lösung abgerufen wird, indem Sie der vorherige Übung abschließen können.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-259">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="0ca0b-260">Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung, Sie müssen einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-260">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="0ca0b-261">Zu diesem Zweck klicken Sie auf die **Projekt** Menü **NuGet-Pakete verwalten**.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-261">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="0ca0b-262">In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-262">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="0ca0b-263">Abschließend erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-263">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="0ca0b-264">Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken in Ihrem Projekt, Versand Verringern der Projektgröße.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-264">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="0ca0b-265">Mit NuGet Power Tools können werden durch Angabe von Versionen des Pakets in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, die, das Sie das Projekt ausführen, können.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-265">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="0ca0b-266">Deshalb müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit wird.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-266">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="0ca0b-267">Hinzufügen der **SampleData.cs** -Datei in die **Modelle** Ordner.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-267">Add the **SampleData.cs** file to the **Models** folder.</span></span> <span data-ttu-id="0ca0b-268">Zu diesem Zweck Maustaste **Modelle** Ordner, zeigen Sie auf **hinzufügen** , und klicken Sie dann auf **vorhandenes Element**.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-268">To do that, right-click **Models** folder, point to **Add** and then click **Existing Item**.</span></span> <span data-ttu-id="0ca0b-269">Navigieren Sie zu **\Source\Assets** , und wählen Sie die **SampleData.cs** Datei.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-269">Browse to **\Source\Assets** and select the **SampleData.cs** file.</span></span>

    <span data-ttu-id="0ca0b-270">![Beispieldaten Auffüllen Code](aspnet-mvc-4-models-and-data-access/_static/image18.png "Beispieldaten Auffüllen Code")</span><span class="sxs-lookup"><span data-stu-id="0ca0b-270">![Sample data populate code](aspnet-mvc-4-models-and-data-access/_static/image18.png "Sample data populate code")</span></span>

    <span data-ttu-id="0ca0b-271">*Beispieldaten Auffüllen code*</span><span class="sxs-lookup"><span data-stu-id="0ca0b-271">*Sample data populate code*</span></span>
3. <span data-ttu-id="0ca0b-272">Öffnen der **"Global.asax.cs"** Datei, und fügen Sie die folgenden *mit* Anweisungen.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-272">Open the **Global.asax.cs** file and add the following *using* statements.</span></span>

    <span data-ttu-id="0ca0b-273">(Codeausschnitt - *Modelle und Datenzugriff - Ex2 globale Asax Using-Direktiven*)</span><span class="sxs-lookup"><span data-stu-id="0ca0b-273">(Code Snippet - *Models And Data Access - Ex2 Global Asax Usings*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample5.cs)]
4. <span data-ttu-id="0ca0b-274">In der **Anwendung\_Start()** Methode hinzufügen, die folgende Zeile hinzu, legen Sie den Datenbankinitialisierer.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-274">In the **Application\_Start()** method add the following line to set the database initializer.</span></span>

    <span data-ttu-id="0ca0b-275">(Codeausschnitt - *Modelle und Datenzugriff - Ex2 globale Asax SetInitializer*)</span><span class="sxs-lookup"><span data-stu-id="0ca0b-275">(Code Snippet - *Models And Data Access - Ex2 Global Asax SetInitializer*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample6.cs)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Configuring_the_connection_to_the_Database"></a>
#### <a name="task-2---configuring-the-connection-to-the-database"></a><span data-ttu-id="0ca0b-276">Aufgabe 2: Konfigurieren der Verbindung mit der Datenbank</span><span class="sxs-lookup"><span data-stu-id="0ca0b-276">Task 2 - Configuring the connection to the Database</span></span>

<span data-ttu-id="0ca0b-277">Nun, da Sie bereits eine Datenbank zu Ihrem Projekt hinzugefügt haben, Schreiben Sie der **"Web.config"** Datei die Verbindungszeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-277">Now that you have already added a database to our project, you will write in the **Web.config** file the connection string.</span></span>

1. <span data-ttu-id="0ca0b-278">Fügen Sie eine Verbindungszeichenfolge zur **"Web.config"**. Öffnen Sie zu diesem Zweck **"Web.config"** am Stammverzeichnis des Projekts, und ersetzen, die die Verbindungszeichenfolge mit dem Namen DefaultConnection mit der folgenden Zeile in der **&lt;ConnectionStrings&gt;** Abschnitt:</span><span class="sxs-lookup"><span data-stu-id="0ca0b-278">Add a connection string at **Web.config**. To do that, open **Web.config** at project root and replace the connection string named DefaultConnection with this line in the **&lt;connectionStrings&gt;** section:</span></span>

    <span data-ttu-id="0ca0b-279">![Speicherort der Datei "Web.config"](aspnet-mvc-4-models-and-data-access/_static/image19.png "Speicherort der Datei \"Web.config\"")</span><span class="sxs-lookup"><span data-stu-id="0ca0b-279">![Web.config file location](aspnet-mvc-4-models-and-data-access/_static/image19.png "Web.config file location")</span></span>

    <span data-ttu-id="0ca0b-280">*Speicherort der Datei "Web.config"*</span><span class="sxs-lookup"><span data-stu-id="0ca0b-280">*Web.config file location*</span></span>

    [!code-xml[Main](aspnet-mvc-4-models-and-data-access/samples/sample7.xml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Working_with_the_Model"></a>
#### <a name="task-3---working-with-the-model"></a><span data-ttu-id="0ca0b-281">Aufgabe 3: mit dem Modell arbeiten</span><span class="sxs-lookup"><span data-stu-id="0ca0b-281">Task 3 - Working with the Model</span></span>

<span data-ttu-id="0ca0b-282">Nun, dass Sie die Verbindung mit der Datenbank bereits konfiguriert haben, verknüpfen Sie das Modell mit Tabellen der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-282">Now that you have already configured the connection to the database, you will link the model with the database tables.</span></span> <span data-ttu-id="0ca0b-283">In dieser Aufgabe erstellen Sie eine Klasse, die für die Datenbank mit Code First verknüpft werden soll.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-283">In this task, you will create a class that will be linked to the database with Code First.</span></span> <span data-ttu-id="0ca0b-284">Denken Sie daran, dass eine vorhandene POCO-Modell-Klasse, die geändert werden soll, vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-284">Remember that there is an existent POCO model class that should be modified.</span></span>

> [!NOTE]
> <span data-ttu-id="0ca0b-285">Wenn Sie Übung 1 abgeschlossen haben, werden Sie feststellen, dass dieser Schritt von einem Assistenten durchgeführt wurde.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-285">If you completed Exercise 1, you will note that this step was performed by a wizard.</span></span> <span data-ttu-id="0ca0b-286">Indem Sie Code First, erstellen Sie manuell Klassen, die auf Datenentitäten verknüpft wird.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-286">By doing Code First, you will manually create classes that will be linked to data entities.</span></span>

1. <span data-ttu-id="0ca0b-287">Öffnen Sie die POCO-Klasse **"Genre"** aus **Modelle** Projektordner, und eine-ID einschließen</span><span class="sxs-lookup"><span data-stu-id="0ca0b-287">Open the POCO model class **Genre** from **Models** project folder and include an ID.</span></span> <span data-ttu-id="0ca0b-288">Verwenden Sie eine Int-Eigenschaft mit dem Namen **GenreId**.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-288">Use an int property with the name **GenreId**.</span></span>

    <span data-ttu-id="0ca0b-289">(Codeausschnitt - *Modelle und Datenzugriff - Ex2 Code zuerst "Genre"*)</span><span class="sxs-lookup"><span data-stu-id="0ca0b-289">(Code Snippet - *Models And Data Access - Ex2 Code First Genre*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample8.cs)]

    > [!NOTE]
    > <span data-ttu-id="0ca0b-290">Zum Arbeiten mit Code First-Konventionen müssen die Klasse "Genre" eine Primärschlüsseleigenschaft, die automatisch erkannt wird.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-290">To work with Code First conventions, the class Genre must have a primary key property that will be automatically detected.</span></span>
    > 
    > <span data-ttu-id="0ca0b-291">Erfahren Sie mehr über Code First-Konventionen in diesem [Msdn-Artikel](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).</span><span class="sxs-lookup"><span data-stu-id="0ca0b-291">You can read more about Code First Conventions in this [msdn article](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).</span></span>
2. <span data-ttu-id="0ca0b-292">Öffnen Sie nun die POCO-Klasse **Album** aus **Modelle** Projektordner und die Fremdschlüssel enthalten, Erstellen von Eigenschaften mit den Namen **GenreId** und  **ArtistId**.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-292">Now, open the POCO model class **Album** from **Models** project folder and include the foreign keys, create properties with the names **GenreId** and **ArtistId**.</span></span> <span data-ttu-id="0ca0b-293">Diese Klasse bereits haben die **GenreId** für den Primärschlüssel.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-293">This class already have the **GenreId** for the primary key.</span></span>

    <span data-ttu-id="0ca0b-294">(Codeausschnitt - *Modelle und Datenzugriff - Ex2 Code zuerst Albums*)</span><span class="sxs-lookup"><span data-stu-id="0ca0b-294">(Code Snippet - *Models And Data Access - Ex2 Code First Album*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample9.cs)]
3. <span data-ttu-id="0ca0b-295">Öffnen Sie die POCO-Klasse **Interpreten** und enthalten die **ArtistId** Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-295">Open the POCO model class **Artist** and include the **ArtistId** property.</span></span>

    <span data-ttu-id="0ca0b-296">(Codeausschnitt - *Modelle und Datenzugriff - Ex2 Code erste Interpreten*)</span><span class="sxs-lookup"><span data-stu-id="0ca0b-296">(Code Snippet - *Models And Data Access - Ex2 Code First Artist*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample10.cs)]
4. <span data-ttu-id="0ca0b-297">Mit der rechten Maustaste die **Modelle** Projektordner, und wählen **hinzufügen | Klasse**.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-297">Right-click the **Models** project folder and select **Add | Class**.</span></span> <span data-ttu-id="0ca0b-298">Nennen Sie die Datei **MusicStoreEntities.cs**.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-298">Name the file **MusicStoreEntities.cs**.</span></span> <span data-ttu-id="0ca0b-299">Klicken Sie auf **hinzufügen.**</span><span class="sxs-lookup"><span data-stu-id="0ca0b-299">Then, click **Add.**</span></span>

    <span data-ttu-id="0ca0b-300">![Hinzufügen einer Klasse](aspnet-mvc-4-models-and-data-access/_static/image20.png "Hinzufügen einer Klasse")</span><span class="sxs-lookup"><span data-stu-id="0ca0b-300">![Adding a class](aspnet-mvc-4-models-and-data-access/_static/image20.png "Adding a class")</span></span>

    <span data-ttu-id="0ca0b-301">*Hinzufügen eines neuen Elements*</span><span class="sxs-lookup"><span data-stu-id="0ca0b-301">*Adding a new item*</span></span>

    <span data-ttu-id="0ca0b-302">![Hinzufügen von class2](aspnet-mvc-4-models-and-data-access/_static/image21.png "class2 hinzufügen")</span><span class="sxs-lookup"><span data-stu-id="0ca0b-302">![Adding a class2](aspnet-mvc-4-models-and-data-access/_static/image21.png "Adding a class2")</span></span>

    <span data-ttu-id="0ca0b-303">*Hinzufügen einer Klasse*</span><span class="sxs-lookup"><span data-stu-id="0ca0b-303">*Adding a class*</span></span>
5. <span data-ttu-id="0ca0b-304">Öffnen Sie die Klasse, die Sie soeben erstellt haben, **MusicStoreEntities.cs**, und fügen Sie die Namespaces **System.Data.Entity** und **System.Data.Entity.Infrastructure**.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-304">Open the class you have just created, **MusicStoreEntities.cs**, and include the namespaces **System.Data.Entity** and **System.Data.Entity.Infrastructure**.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample11.cs)]
6. <span data-ttu-id="0ca0b-305">Ersetzen Sie die Klassendeklaration in erweitern die **"DbContext"** Klasse: deklarieren eine öffentlichen **"DbSet"** und überschreiben **"onmodelcreating"** Methode.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-305">Replace the class declaration to extend the **DbContext** class: declare a public **DBSet** and override **OnModelCreating** method.</span></span> <span data-ttu-id="0ca0b-306">Nach diesem Schritt erhalten Sie eine Domänenklasse, die das Modell mit dem Entity Framework verknüpft wird.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-306">After this step you will get a domain class that will link your model with the Entity Framework.</span></span> <span data-ttu-id="0ca0b-307">Zu diesem Zweck ersetzen Sie den Code der Klasse durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="0ca0b-307">In order to do that, replace the class code with the following:</span></span>

    <span data-ttu-id="0ca0b-308">(Codeausschnitt - *Modelle und Datenzugriff - Ex2 Code erste MusicStoreEntities*)</span><span class="sxs-lookup"><span data-stu-id="0ca0b-308">(Code Snippet - *Models And Data Access - Ex2 Code First MusicStoreEntities*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample12.cs)]

> [!NOTE]
> <span data-ttu-id="0ca0b-309">Mit Entity Framework **"DbContext"** und **"DbSet"** können zum Abfragen der POCO-Klasse "Genre".</span><span class="sxs-lookup"><span data-stu-id="0ca0b-309">With Entity Framework **DbContext** and **DBSet** you will be able to query the POCO class Genre.</span></span> <span data-ttu-id="0ca0b-310">Durch die Erweiterung **"onmodelcreating"** -Methode, die Sie angeben werden, in der **Code** wie "Genre" in einer Datenbanktabelle zugeordnet werden soll.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-310">By extending **OnModelCreating** method, you are specifying in the **code** how Genre will be mapped to a database table.</span></span> <span data-ttu-id="0ca0b-311">Weitere Informationen zu "DbContext" und "DbSet" finden Sie in diesem Msdn-Artikel: [Link](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)</span><span class="sxs-lookup"><span data-stu-id="0ca0b-311">You can find more information about DBContext and DBSet in this msdn article: [link](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a><span data-ttu-id="0ca0b-312">Aufgabe 4: Abfragen der Datenbank</span><span class="sxs-lookup"><span data-stu-id="0ca0b-312">Task 4 - Querying the Database</span></span>

<span data-ttu-id="0ca0b-313">In dieser Aufgabe aktualisieren Sie die Klasse StoreController, anstatt hartcodierte Daten, damit sie aus der Datenbank abgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-313">In this task, you will update the StoreController class so that, instead of using hardcoded data, it will retrieve it from the database.</span></span>

> [!NOTE]
> <span data-ttu-id="0ca0b-314">Diese Aufgabe ist, enthält, die auch Übung 1.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-314">This task is in common with Exercise 1.</span></span>
> 
> <span data-ttu-id="0ca0b-315">Wenn Sie die Übung 1 abgeschlossen sehen Sie diese Schritte sind bei beiden Ansätzen identisch (Datenbank zunächst oder Code first).</span><span class="sxs-lookup"><span data-stu-id="0ca0b-315">If you completed Exercise 1 you will note these steps are the same in both approaches (Database first or Code first).</span></span> <span data-ttu-id="0ca0b-316">Sie unterscheiden sich in wie die Daten mit dem Modell verknüpft werden, aber der Zugriff auf Datenentitäten ist noch transparent vom Controller.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-316">They are different in how the data is linked with the model, but the access to data entities is yet transparent from the controller.</span></span>


1. <span data-ttu-id="0ca0b-317">Open **Controllers\StoreController.cs** und fügen Sie das folgende Feld hinzu, auf die Klasse, um eine Instanz von aufzunehmen der **MusicStoreEntities** Klasse, mit dem Namen **StoreDB**:</span><span class="sxs-lookup"><span data-stu-id="0ca0b-317">Open **Controllers\StoreController.cs** and add the following field to the class to hold an instance of the **MusicStoreEntities** class, named **storeDB**:</span></span>

    <span data-ttu-id="0ca0b-318">(Codeausschnitt - *Modelle und Datenzugriff – Ex1 StoreDB*)</span><span class="sxs-lookup"><span data-stu-id="0ca0b-318">(Code Snippet - *Models And Data Access - Ex1 storeDB*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample13.cs)]
2. <span data-ttu-id="0ca0b-319">Die **MusicStoreEntities** Klasse macht eine Auflistungseigenschaft für jede Tabelle in der Datenbank verfügbar.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-319">The **MusicStoreEntities** class exposes a collection property for each table in the database.</span></span> <span data-ttu-id="0ca0b-320">Update **Durchsuchen** Aktionsmethode zum Abrufen einer "Genre" mit allen dem **Alben**.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-320">Update **Browse** action method to retrieve a Genre with all of the **Albums**.</span></span>

    <span data-ttu-id="0ca0b-321">(Codeausschnitt - *Modelle und Datenzugriff - Ex2 Store durchsuchen*)</span><span class="sxs-lookup"><span data-stu-id="0ca0b-321">(Code Snippet - *Models And Data Access - Ex2 Store Browse*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample14.cs)]

    > [!NOTE]
    > <span data-ttu-id="0ca0b-322">Verwenden Sie eine Funktion von .NET namens **LINQ** (Language integrated Query) zum Schreiben von stark typisierten-Abfrageausdrücken für diese Sammlungen - zurück und führen Sie Code für die Datenbank-Objekten, die Sie programmieren können, vor.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-322">You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.</span></span>
    > 
    > <span data-ttu-id="0ca0b-323">Weitere Informationen über LINQ finden Sie auf die [Msdn-Website](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).</span><span class="sxs-lookup"><span data-stu-id="0ca0b-323">For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).</span></span>
3. <span data-ttu-id="0ca0b-324">Update **Index** Aktionsmethode, die alle Genres abrufen.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-324">Update **Index** action method to retrieve all the genres.</span></span>

    <span data-ttu-id="0ca0b-325">(Codeausschnitt - *Modelle und Datenzugriff - Ex2 Store Index*)</span><span class="sxs-lookup"><span data-stu-id="0ca0b-325">(Code Snippet - *Models And Data Access - Ex2 Store Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample15.cs)]
4. <span data-ttu-id="0ca0b-326">Update **Index** Aktionsmethode, die alle Genres abrufen und Transformieren der Auflistung in einer Liste.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-326">Update **Index** action method to retrieve all the genres and transform the collection to a list.</span></span>

    <span data-ttu-id="0ca0b-327">(Codeausschnitt - *Modelle und Datenzugriff - Ex2 Store GenreMenu*)</span><span class="sxs-lookup"><span data-stu-id="0ca0b-327">(Code Snippet - *Models And Data Access - Ex2 Store GenreMenu*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample16.cs)]

<a id="Ex2Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="0ca0b-328">Aufgabe 5: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="0ca0b-328">Task 5 - Running the Application</span></span>

<span data-ttu-id="0ca0b-329">In dieser Aufgabe überprüfen, dass die Indexseite der Store jetzt Genres in die Datenbank anstatt durch die hartcodierte diejenigen gespeichert angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-329">In this task, you will check that the Store Index page will now display the Genres stored in the database instead of the hardcoded ones.</span></span> <span data-ttu-id="0ca0b-330">Besteht keine Notwendigkeit, die die ansichtsvorlage zu ändern, da die **StoreController** gibt dasselbe **StoreIndexViewModel** wie zuvor, aber dieses Mal werden die Daten aus der Datenbank stammen.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-330">There is no need to change the View template because the **StoreController** is returning the same **StoreIndexViewModel** as before, but this time the data will come from the database.</span></span>

1. <span data-ttu-id="0ca0b-331">Erneutes Erstellen von Projektmappen und drücken Sie **F5** zum Ausführen der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-331">Rebuild the solution and press **F5** to run the Application.</span></span>
2. <span data-ttu-id="0ca0b-332">Das Projekt, die auf der Startseite wird gestartet.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-332">The project starts in the Home page.</span></span> <span data-ttu-id="0ca0b-333">Überprüfen Sie, ob Sie im Menü des **Genres** ist nicht mehr eine feste Liste, und die Daten direkt aus der Datenbank abgerufen.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-333">Verify that the menu of **Genres** is no longer a hardcoded list, and the data is directly retrieved from the database.</span></span>

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image22.png)

    <span data-ttu-id="0ca0b-335">*Durchsuchen von Genres aus der Datenbank*</span><span class="sxs-lookup"><span data-stu-id="0ca0b-335">*Browsing Genres from the database*</span></span>
3. <span data-ttu-id="0ca0b-336">Nun wechseln Sie zu jedem "Genre", und stellen Sie sicher, dass die Alben aus der Datenbank aufgefüllt werden.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-336">Now browse to any genre and verify the albums are populated from database.</span></span>

    <span data-ttu-id="0ca0b-337">![Durchsuchen Sie Alben aus der Datenbank](aspnet-mvc-4-models-and-data-access/_static/image23.png "durchsuchen Alben aus der Datenbank")</span><span class="sxs-lookup"><span data-stu-id="0ca0b-337">![Browsing Albums from the database](aspnet-mvc-4-models-and-data-access/_static/image23.png "Browsing Albums from the database")</span></span>

    <span data-ttu-id="0ca0b-338">*Durchsuchen Alben aus der Datenbank*</span><span class="sxs-lookup"><span data-stu-id="0ca0b-338">*Browsing Albums from the database*</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Querying_the_Database_with_Parameters"></a>
### <a name="exercise-3-querying-the-database-with-parameters"></a><span data-ttu-id="0ca0b-339">Übung 3: Abfragen der Datenbank mit Parametern</span><span class="sxs-lookup"><span data-stu-id="0ca0b-339">Exercise 3: Querying the Database with Parameters</span></span>

<span data-ttu-id="0ca0b-340">Klicken Sie in dieser Übung lernen Sie Gewusst wie: Abfragen der Datenbank, die mit Parametern und Vorgehensweise verwenden Sie die Abfrage Ergebnis strukturieren, ein Feature, das die Anzahl Datenbank reduziert Abrufen von Daten in eine effizientere Methode zugreift.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-340">In this exercise, you will learn how to query the database using parameters, and how to use Query Result Shaping, a feature that reduces the number database accesses retrieving data in a more efficient way.</span></span>

> [!NOTE]
> <span data-ttu-id="0ca0b-341">Weitere Informationen über die Abfrage Ergebnis strukturieren, finden Sie auf die folgenden [Msdn-Artikel](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).</span><span class="sxs-lookup"><span data-stu-id="0ca0b-341">For further information on Query Result Shaping, visit the following [msdn article](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_StoreController_to_Retrieve_Albums_from_Database"></a>
#### <a name="task-1---modifying-storecontroller-to-retrieve-albums-from-database"></a><span data-ttu-id="0ca0b-342">Aufgabe 1: Ändern von StoreController Alben aus der Datenbank abrufen</span><span class="sxs-lookup"><span data-stu-id="0ca0b-342">Task 1 - Modifying StoreController to Retrieve Albums from Database</span></span>

<span data-ttu-id="0ca0b-343">In dieser Aufgabe ändern Sie die **StoreController** Klasse Zugriff auf die Datenbank zum Abrufen von Alben aus einem bestimmten Genre.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-343">In this task, you will change the **StoreController** class to access the database to retrieve albums from a specific genre.</span></span>

1. <span data-ttu-id="0ca0b-344">Öffnen der **beginnen** Lösung befindet sich auf die **Source\Ex3-QueryingTheDatabaseWithParametersCodeFirst\Begin** Ordner aus, wenn Sie Code First-Ansatz verwenden möchten oder **Source\ Ex3-QueryingTheDatabaseWithParametersDBFirst\Begin** Ordner aus, wenn Sie Database First-Ansatz verwenden möchten.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-344">Open the **Begin** solution located at the **Source\Ex3-QueryingTheDatabaseWithParametersCodeFirst\Begin** folder if you want to use Code-First approach or **Source\Ex3-QueryingTheDatabaseWithParametersDBFirst\Begin** folder if you want to use Database-First approach.</span></span> <span data-ttu-id="0ca0b-345">Andernfalls können Sie weiterhin, verwenden die **End** Lösung abgerufen wird, indem Sie der vorherige Übung abschließen können.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-345">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="0ca0b-346">Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung, Sie müssen einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-346">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="0ca0b-347">Zu diesem Zweck klicken Sie auf die **Projekt** Menü **NuGet-Pakete verwalten**.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-347">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="0ca0b-348">In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-348">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="0ca0b-349">Abschließend erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-349">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="0ca0b-350">Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken in Ihrem Projekt, Versand Verringern der Projektgröße.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-350">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="0ca0b-351">Mit NuGet Power Tools können werden durch Angabe von Versionen des Pakets in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, die, das Sie das Projekt ausführen, können.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-351">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="0ca0b-352">Deshalb müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit wird.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-352">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="0ca0b-353">Öffnen der **StoreController** Klasse so ändern Sie die **Durchsuchen** Aktionsmethode.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-353">Open the **StoreController** class to change the **Browse** action method.</span></span> <span data-ttu-id="0ca0b-354">Klicken Sie hierzu in der **Projektmappen-Explorer**, erweitern Sie die **Controller** Ordner und doppelklicken Sie auf **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-354">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
3. <span data-ttu-id="0ca0b-355">Ändern der **Durchsuchen** Aktionsmethode Alben für eine spezifische Genre abrufen.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-355">Change the **Browse** action method to retrieve albums for a specific genre.</span></span> <span data-ttu-id="0ca0b-356">Ersetzen Sie hierzu den folgenden Code ein:</span><span class="sxs-lookup"><span data-stu-id="0ca0b-356">To do this, replace the following code:</span></span>

    <span data-ttu-id="0ca0b-357">(Codeausschnitt - *Modelle und Datenzugriff - Ex3 StoreController BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="0ca0b-357">(Code Snippet - *Models And Data Access - Ex3 StoreController BrowseMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample17.cs)]

> [!NOTE]
> <span data-ttu-id="0ca0b-358">Um eine Auflistung der Entität aufzufüllen, müssen Sie die **Include** Methode, um anzugeben, die Alben zu abgerufen werden soll.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-358">To populate a collection of the entity, you need to use the **Include** method to specify you want to retrieve the albums too.</span></span> <span data-ttu-id="0ca0b-359">Sie können die. **Single()** -Erweiterung in LINQ, da in diesem Fall nur eine "Genre" für ein Album erwartet wird.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-359">You can use the .**Single()** extension in LINQ because in this case only one genre is expected for an album.</span></span> <span data-ttu-id="0ca0b-360">Die **Single()** Methode nimmt einen Lambdaausdruck als Parameter, die in diesem Fall ein einzelnes Objekt für "Genre" gibt an, so an, dass der Name der definierten Wert entspricht.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-360">The **Single()** method takes a Lambda expression as a parameter, which in this case specifies a single Genre object such that its name matches the value defined.</span></span>
> 
> <span data-ttu-id="0ca0b-361">Sie werden eine Funktion nutzen, mit dem Sie an andere verknüpften Entitäten, die ebenfalls geladen werden sollen, wenn das Objekt "Genre" abgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-361">You will take advantage of a feature that allows you to indicate other related entities you want loaded as well when the Genre object is retrieved.</span></span> <span data-ttu-id="0ca0b-362">Dieses Feature heißt **Abfrage Ergebnis strukturieren**, und ermöglicht es Ihnen, wie oft erforderlich, um Zugriff auf die Datenbank zum Abrufen von Informationen zu reduzieren.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-362">This feature is called **Query Result Shaping**, and enables you to reduce the number of times needed to access the database to retrieve information.</span></span> <span data-ttu-id="0ca0b-363">In diesem Szenario werden Sie Alben für das Genre vorab abzurufen, die Sie abrufen möchten.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-363">In this scenario, you will want to pre-fetch the Albums for the Genre you retrieve.</span></span>
> 
> <span data-ttu-id="0ca0b-364">Die Abfrage enthält **Genres.Include (&quot;Alben&quot;)** an, wenn auch verwandte Alben werden sollen.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-364">The query includes **Genres.Include(&quot;Albums&quot;)** to indicate that you want related albums as well.</span></span> <span data-ttu-id="0ca0b-365">Dies führt zu einer Anwendung effizienter, da es sowohl "Genre" und "Album Daten in einer einzelnen Datenbank-Anforderung abgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-365">This will result in a more efficient application, since it will retrieve both Genre and Album data in a single database request.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="0ca0b-366">Aufgabe 2: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="0ca0b-366">Task 2 - Running the Application</span></span>

<span data-ttu-id="0ca0b-367">In dieser Aufgabe, die Sie die Anwendung ausgeführt werden und Alben von einer bestimmten "Genre" aus der Datenbank abrufen.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-367">In this task, you will run the application and retrieve albums of a specific genre from the database.</span></span>

1. <span data-ttu-id="0ca0b-368">Drücken Sie **F5** zum Ausführen der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-368">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="0ca0b-369">Das Projekt, die auf der Startseite wird gestartet.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-369">The project starts in the Home page.</span></span> <span data-ttu-id="0ca0b-370">Ändern Sie die URL zum **/Store/durchsuchen? Genre = Pop** um sicherzustellen, dass die Ergebnisse aus der Datenbank abgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-370">Change the URL to **/Store/Browse?genre=Pop** to verify that the results are being retrieved from the database.</span></span>

    <span data-ttu-id="0ca0b-371">![Durchsuchen nach Genre](aspnet-mvc-4-models-and-data-access/_static/image24.png "durchsuchen nach Genre")</span><span class="sxs-lookup"><span data-stu-id="0ca0b-371">![Browsing by genre](aspnet-mvc-4-models-and-data-access/_static/image24.png "Browsing by genre")</span></span>

    <span data-ttu-id="0ca0b-372">*Durchsuchen/Store/durchsuchen? Genre = Pop*</span><span class="sxs-lookup"><span data-stu-id="0ca0b-372">*Browsing /Store/Browse?genre=Pop*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Accessing_Albums_by_Id"></a>
#### <a name="task-3---accessing-albums-by-id"></a><span data-ttu-id="0ca0b-373">Aufgabe 3: Zugreifen auf Alben nach Id</span><span class="sxs-lookup"><span data-stu-id="0ca0b-373">Task 3 - Accessing Albums by Id</span></span>

<span data-ttu-id="0ca0b-374">In dieser Aufgabe werden Sie in der vorherigen Prozedur zum Abrufen von Alben anhand ihrer Id. wiederholen.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-374">In this task, you will repeat the previous procedure to get albums by their Id.</span></span>

1. <span data-ttu-id="0ca0b-375">Schließen Sie den Browser, die ggf. zu Visual Studio zurückzukehren.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-375">Close the browser if needed, to return to Visual Studio.</span></span> <span data-ttu-id="0ca0b-376">Öffnen der **StoreController** Klasse so ändern Sie die **Details** Aktionsmethode.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-376">Open the **StoreController** class to change the **Details** action method.</span></span> <span data-ttu-id="0ca0b-377">Klicken Sie hierzu in der **Projektmappen-Explorer**, erweitern Sie die **Controller** Ordner und doppelklicken Sie auf **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-377">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
2. <span data-ttu-id="0ca0b-378">Ändern der **Details** Aktionsmethode beim Abrufen der Details von Alben basierend auf ihren **Id**. Ersetzen Sie hierzu den folgenden Code ein:</span><span class="sxs-lookup"><span data-stu-id="0ca0b-378">Change the **Details** action method to retrieve albums details based on their **Id**. To do this, replace the following code:</span></span>

    <span data-ttu-id="0ca0b-379">(Codeausschnitt - *Modelle und Datenzugriff - Ex3 StoreController DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="0ca0b-379">(Code Snippet - *Models And Data Access - Ex3 StoreController DetailsMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample18.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="0ca0b-380">Aufgabe 4: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="0ca0b-380">Task 4 - Running the Application</span></span>

<span data-ttu-id="0ca0b-381">In dieser Aufgabe wird die Anwendung in einem Webbrowser ausführen und erfragen Sie Album anhand ihrer Id.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-381">In this task, you will run the Application in a web browser and obtain album details by their Id.</span></span>

1. <span data-ttu-id="0ca0b-382">Drücken Sie **F5** zum Ausführen der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-382">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="0ca0b-383">Das Projekt, die auf der Startseite wird gestartet.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-383">The project starts in the Home page.</span></span> <span data-ttu-id="0ca0b-384">Ändern Sie die URL zum **/Store/Details/51** oder Genres durchsuchen, und wählen Sie ein Album, um sicherzustellen, dass die Ergebnisse aus der Datenbank abgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-384">Change the URL to **/Store/Details/51** or browse the genres and select an album to verify that the results are being retrieved from the database.</span></span>

    <span data-ttu-id="0ca0b-385">![Durchsuchen Sie die Details](aspnet-mvc-4-models-and-data-access/_static/image25.png "Informationen durchsuchen")</span><span class="sxs-lookup"><span data-stu-id="0ca0b-385">![Browsing Details](aspnet-mvc-4-models-and-data-access/_static/image25.png "Browsing Details")</span></span>

    <span data-ttu-id="0ca0b-386">*/Store/Details/51 durchsuchen*</span><span class="sxs-lookup"><span data-stu-id="0ca0b-386">*Browsing /Store/Details/51*</span></span>

> [!NOTE]
> <span data-ttu-id="0ca0b-387">Darüber hinaus können Sie diese Anwendung für Windows Azure-Websites Folgendes bereitstellen [Anhang B: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web Deploy](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="0ca0b-387">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="0ca0b-388">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="0ca0b-388">Summary</span></span>

<span data-ttu-id="0ca0b-389">Von dieser praktischen Übungseinheit haben Sie gelernt, die Grundlagen von ASP.NET MVC-Modelle und Datenzugriff, indem die **Database First** Ansatz als auch die **Code First** Ansatz:</span><span class="sxs-lookup"><span data-stu-id="0ca0b-389">By completing this Hands-on Lab you have learned the fundamentals of ASP.NET MVC Models and Data Access, using the **Database First** approach as well as the **Code First** Approach:</span></span>

- <span data-ttu-id="0ca0b-390">Wie Sie die Lösung eine Datenbank hinzugefügt, um die Daten zu verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-390">How to add a database to the solution in order to consume its data</span></span>
- <span data-ttu-id="0ca0b-391">Gewusst wie: Aktualisieren der Verwaltungscontroller zum Anzeigen von Vorlagen mit den Daten aus der Datenbank statt einer hartcodierten bereitstellen</span><span class="sxs-lookup"><span data-stu-id="0ca0b-391">How to update Controllers to provide View templates with the data taken from the database instead of hard-coded one</span></span>
- <span data-ttu-id="0ca0b-392">Gewusst wie: Abfragen der Datenbank, die mit Parametern</span><span class="sxs-lookup"><span data-stu-id="0ca0b-392">How to query the database using parameters</span></span>
- <span data-ttu-id="0ca0b-393">Gewusst wie: Verwenden Sie die Abfrage Ergebnis strukturieren, ein Feature, das die Anzahl von Datenbankzugriffe reduziert das Abrufen von Daten in eine effizientere Methode</span><span class="sxs-lookup"><span data-stu-id="0ca0b-393">How to use the Query Result Shaping, a feature that reduces the number of database accesses, retrieving data in a more efficient way</span></span>
- <span data-ttu-id="0ca0b-394">Wie Sie Database First und Code First-Ansätze in Microsoft Entity Framework zu verwenden, um die Datenbank mit dem Modell verknüpfen</span><span class="sxs-lookup"><span data-stu-id="0ca0b-394">How to use both Database First and Code First approaches in Microsoft Entity Framework to link the database with the model</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="0ca0b-395">Anhang A: Installieren von Visual Studio Express 2012 für Web</span><span class="sxs-lookup"><span data-stu-id="0ca0b-395">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="0ca0b-396">Sie installieren können **Microsoft Visual Studio Express 2012 für Web** oder einem anderen &quot;Express&quot; Version verwenden, die **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-396">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="0ca0b-397">Die folgenden Anweisungen führen Sie über die erforderlichen Schritte zum Installieren *Visual Studio Express 2012 für Web* mit *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-397">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="0ca0b-398">Wechseln Sie zu [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="0ca0b-398">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="0ca0b-399">Auch wenn Sie bereits Webplattform-Installer installiert haben, können Sie öffnen ihn, und suchen Sie nach dem Produkt &quot; <em>Visual Studio Express 2012 für das Web mit Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-399">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="0ca0b-400">Klicken Sie auf **jetzt installieren**.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-400">Click on **Install Now**.</span></span> <span data-ttu-id="0ca0b-401">Wenn Sie keine **Webplattform-Installer** gelangen Sie zum Herunterladen und installieren Sie diese zuerst.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-401">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="0ca0b-402">Einmal **Webplattform-Installer** geöffnet ist, klicken Sie auf **installieren** um das Setup zu starten.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-402">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="0ca0b-403">![Installieren von Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "Installieren von Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="0ca0b-403">![Install Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="0ca0b-404">*Installieren von Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="0ca0b-404">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="0ca0b-405">Lesen Sie die Produkte Lizenzen und Begriffe, und klicken Sie auf **akzeptieren** um den Vorgang fortzusetzen.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-405">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Akzeptieren der Lizenzbedingungen](aspnet-mvc-4-models-and-data-access/_static/image27.png)

    <span data-ttu-id="0ca0b-407">*Akzeptieren der Lizenzbedingungen*</span><span class="sxs-lookup"><span data-stu-id="0ca0b-407">*Accepting the license terms*</span></span>
5. <span data-ttu-id="0ca0b-408">Warten Sie, bis der Prozess zum Herunterladen und die Installation abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-408">Wait until the downloading and installation process completes.</span></span>

    ![Installationsstatus](aspnet-mvc-4-models-and-data-access/_static/image28.png)

    <span data-ttu-id="0ca0b-410">*Installationsstatus*</span><span class="sxs-lookup"><span data-stu-id="0ca0b-410">*Installation progress*</span></span>
6. <span data-ttu-id="0ca0b-411">Wenn die Installation abgeschlossen ist, klicken Sie auf **Fertig stellen**.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-411">When the installation completes, click **Finish**.</span></span>

    ![Die Installation wurde abgeschlossen](aspnet-mvc-4-models-and-data-access/_static/image29.png)

    <span data-ttu-id="0ca0b-413">*Die Installation wurde abgeschlossen*</span><span class="sxs-lookup"><span data-stu-id="0ca0b-413">*Installation completed*</span></span>
7. <span data-ttu-id="0ca0b-414">Klicken Sie auf **beenden** Webplattform-Installer zu schließen.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-414">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="0ca0b-415">Um Visual Studio Express für Web zu öffnen, wechseln Sie zu der **starten** schreiben zu starten und Bildschirm &quot; **VS Express**&quot;, klicken Sie dann auf die **Visual Studio Express für Web** die Kachel.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-415">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Visual Studio Express für Web-Kachel](aspnet-mvc-4-models-and-data-access/_static/image30.png)

    <span data-ttu-id="0ca0b-417">*Visual Studio Express für Web-Kachel*</span><span class="sxs-lookup"><span data-stu-id="0ca0b-417">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="0ca0b-418">Anhang B: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web Deploy</span><span class="sxs-lookup"><span data-stu-id="0ca0b-418">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="0ca0b-419">In diesem Anhang wird veranschaulicht, wie erstellen eine neue Website aus dem Windows Azure-Verwaltungsportal, und Veröffentlichen der Anwendung, die Sie erhalten haben, indem der testumgebung können die Nutzung der Web Deploy-publishing-Funktion von Windows Azure bereitgestellte.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-419">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="0ca0b-420">Aufgabe 1: Erstellen einer neuen Website aus dem Windows Azure-Portal</span><span class="sxs-lookup"><span data-stu-id="0ca0b-420">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="0ca0b-421">Wechseln Sie zu der [Windows Azure-Verwaltungsportal](https://manage.windowsazure.com/) und melden Sie sich mit den Microsoft-Anmeldeinformationen, die Ihrem Abonnement zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-421">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="0ca0b-422">Mit Windows Azure können Sie 10 ASP.NET-Websites kostenlos hosten und dann zu skalieren, wenn Ihr Datenverkehr zunimmt.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-422">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="0ca0b-423">Sie können registrieren [hier](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="0ca0b-423">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="0ca0b-424">![Melden Sie sich bei Windows Azure-Portal](aspnet-mvc-4-models-and-data-access/_static/image31.png "melden Sie sich bei Windows Azure-Portal")</span><span class="sxs-lookup"><span data-stu-id="0ca0b-424">![Log on to Windows Azure portal](aspnet-mvc-4-models-and-data-access/_static/image31.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="0ca0b-425">*Melden Sie sich bei Windows Azure-Verwaltungsportal*</span><span class="sxs-lookup"><span data-stu-id="0ca0b-425">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="0ca0b-426">Klicken Sie auf **neu** auf der Befehlsleiste.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-426">Click **New** on the command bar.</span></span>

    <span data-ttu-id="0ca0b-427">![Erstellen einer neuen Website](aspnet-mvc-4-models-and-data-access/_static/image32.png "Erstellen einer neuen Website")</span><span class="sxs-lookup"><span data-stu-id="0ca0b-427">![Creating a new Web Site](aspnet-mvc-4-models-and-data-access/_static/image32.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="0ca0b-428">*Erstellen einer neuen Website*</span><span class="sxs-lookup"><span data-stu-id="0ca0b-428">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="0ca0b-429">Klicken Sie auf **Compute** | **Website**.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-429">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="0ca0b-430">Wählen Sie dann **Schnellerfassung** Option.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-430">Then select **Quick Create** option.</span></span> <span data-ttu-id="0ca0b-431">Geben Sie eine verfügbare URL für die neue Website, und klicken Sie auf **Website erstellen**.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-431">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="0ca0b-432">Ein Windows Azure-Website ist der Host für eine Webanwendung, die in der Cloud, die Sie steuern und verwalten können.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-432">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="0ca0b-433">Die Option "Schnellerfassung" können Sie eine vollständige Webanwendung auf dem Windows Azure-Website von außerhalb des Portals bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-433">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="0ca0b-434">Er umfasst keine Informationen zum Einrichten einer Datenbank.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-434">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="0ca0b-435">![Erstellen einer neuen Website mithilfe der Schnellerfassung](aspnet-mvc-4-models-and-data-access/_static/image33.png "Erstellen einer neuen Website mithilfe der Schnellerfassung")</span><span class="sxs-lookup"><span data-stu-id="0ca0b-435">![Creating a new Web Site using Quick Create](aspnet-mvc-4-models-and-data-access/_static/image33.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="0ca0b-436">*Erstellen einer neuen Website mithilfe der Schnellerfassung*</span><span class="sxs-lookup"><span data-stu-id="0ca0b-436">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="0ca0b-437">Warten Sie, bis die neue **Website** erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-437">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="0ca0b-438">Nachdem die Website erstellt wurde, klicken Sie auf den Link unter der **URL** Spalte.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-438">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="0ca0b-439">Überprüfen Sie, dass die neue Website ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-439">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="0ca0b-440">![Navigieren zu der neuen Website](aspnet-mvc-4-models-and-data-access/_static/image34.png "auf die neue Website durchsuchen")</span><span class="sxs-lookup"><span data-stu-id="0ca0b-440">![Browsing to the new web site](aspnet-mvc-4-models-and-data-access/_static/image34.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="0ca0b-441">*Um die neue Website durchsuchen*</span><span class="sxs-lookup"><span data-stu-id="0ca0b-441">*Browsing to the new web site*</span></span>

    <span data-ttu-id="0ca0b-442">![Website](aspnet-mvc-4-models-and-data-access/_static/image35.png "Website ausgeführt wird")</span><span class="sxs-lookup"><span data-stu-id="0ca0b-442">![Web site running](aspnet-mvc-4-models-and-data-access/_static/image35.png "Web site running")</span></span>

    <span data-ttu-id="0ca0b-443">*Die Website ausgeführt wird*</span><span class="sxs-lookup"><span data-stu-id="0ca0b-443">*Web site running*</span></span>
6. <span data-ttu-id="0ca0b-444">Wechseln Sie zurück zum Portal, und klicken Sie auf den Namen der Website unter der **Namen** Spalte die Verwaltungsseiten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-444">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="0ca0b-445">![Öffnen die Website-Verwaltungsseiten](aspnet-mvc-4-models-and-data-access/_static/image36.png "öffnen die Website-Verwaltungsseiten")</span><span class="sxs-lookup"><span data-stu-id="0ca0b-445">![Opening the web site management pages](aspnet-mvc-4-models-and-data-access/_static/image36.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="0ca0b-446">*Öffnen die Website-Verwaltungsseiten*</span><span class="sxs-lookup"><span data-stu-id="0ca0b-446">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="0ca0b-447">In der **Dashboard** Seite die **Blick** auf die **Veröffentlichungsprofil herunterladen** Link.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-447">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="0ca0b-448">Die *Veröffentlichungsprofil* enthält alle Informationen zum Veröffentlichen einer Webanwendung in einer Windows Azure-Website für die einzelnen aktivierten Veröffentlichungsmethoden erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-448">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="0ca0b-449">Das Veröffentlichungsprofil enthält die URLs, Benutzeranmeldeinformationen und datenbankzeichenfolgen, die zum Herstellen einer Verbindung mit und die Authentifizierung bei jedem der Endpunkte für die eine Veröffentlichungsmethode aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-449">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="0ca0b-450">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express für Web** und **Microsoft Visual Studio 2012** Unterstützung, die beim Lesen von veröffentlichungsprofilen Automatisieren der Konfiguration von diesen Programmen Veröffentlichung von Webanwendungen in Windows Azure-Websites.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-450">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="0ca0b-451">![Herunterladen der Website-Veröffentlichungsprofil](aspnet-mvc-4-models-and-data-access/_static/image37.png "herunterladen den Website-Veröffentlichungsprofil")</span><span class="sxs-lookup"><span data-stu-id="0ca0b-451">![Downloading the web site publish profile](aspnet-mvc-4-models-and-data-access/_static/image37.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="0ca0b-452">*Herunterladen der Website-Veröffentlichungsprofil*</span><span class="sxs-lookup"><span data-stu-id="0ca0b-452">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="0ca0b-453">Laden Sie das Veröffentlichungsprofil an einem bekannten Speicherort herunter.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-453">Download the publish profile file to a known location.</span></span> <span data-ttu-id="0ca0b-454">In dieser Übung wird weiter wie Sie diese Datei verwenden, um das Veröffentlichen einer Webanwendung auf einem Windows Azure-Websites in Visual Studio angezeigt.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-454">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="0ca0b-455">![Speichern das Veröffentlichungsprofil](aspnet-mvc-4-models-and-data-access/_static/image38.png "speichern das Veröffentlichungsprofil")</span><span class="sxs-lookup"><span data-stu-id="0ca0b-455">![Saving the publish profile file](aspnet-mvc-4-models-and-data-access/_static/image38.png "Saving the publish profile")</span></span>

    <span data-ttu-id="0ca0b-456">*Speichern das Veröffentlichungsprofil*</span><span class="sxs-lookup"><span data-stu-id="0ca0b-456">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="0ca0b-457">Aufgabe 2: Konfigurieren des Datenbank-Servers</span><span class="sxs-lookup"><span data-stu-id="0ca0b-457">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="0ca0b-458">Wenn Ihre Anwendung nutzt SQL Server Datenbanken, die Sie zum Erstellen eines SQL-Datenbank-Servers benötigen.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-458">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="0ca0b-459">Wenn zum Bereitstellen einer einfachen Anwendung, die keine SQL Server verwendet werden sollen, können Sie diese Aufgabe überspringen.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-459">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="0ca0b-460">Sie benötigen einen SQL-Datenbank-Server zum Speichern der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-460">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="0ca0b-461">Sehen Sie die SQL-Datenbank-Server aus Ihrem Abonnement in das Windows Azure-Verwaltungsportal unter **Sql-Datenbanken** | **Server** | **des Servers Dashboard**.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-461">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="0ca0b-462">Wenn Sie nicht mit eine Server-Instanz verfügen, können Sie erstellen, mithilfe der **hinzufügen** auf der Befehlsleiste auf die Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-462">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="0ca0b-463">Notieren Sie sich die **Servername und -URL, Administrator-Anmeldenamen und Kennwort**, wie Sie sie in den nächsten Aufgaben verwenden.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-463">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="0ca0b-464">Erstellen Sie die Datenbank nicht noch, wie es in einem späteren Zeitpunkt erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-464">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="0ca0b-465">![SQL Server-Datenbank-Dashboard](aspnet-mvc-4-models-and-data-access/_static/image39.png "Dashboard der SQL-Datenbank-Server")</span><span class="sxs-lookup"><span data-stu-id="0ca0b-465">![SQL Database Server Dashboard](aspnet-mvc-4-models-and-data-access/_static/image39.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="0ca0b-466">*SQL Server-Datenbank-Dashboard*</span><span class="sxs-lookup"><span data-stu-id="0ca0b-466">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="0ca0b-467">In der nächsten Aufgabe testen Sie die Verbindung mit der Datenbank, in Visual Studio aus diesem Grund Sie Ihre lokale IP-Adresse in der Liste des Servers der einschließen müssen **zulässige IP-Adressen**.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-467">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="0ca0b-468">Zu diesem Zweck klicken Sie auf **konfigurieren**, wählen Sie die IP-Adresse von **Current Client IP Address** und fügen Sie ihn auf die **IP-Startadresse** und **IP-Endadresse** Textfelder, und klicken Sie auf die ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png) Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-468">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png) button.</span></span>

    ![Client-IP-Adresse hinzufügen](aspnet-mvc-4-models-and-data-access/_static/image41.png)

    <span data-ttu-id="0ca0b-470">*Client-IP-Adresse hinzufügen*</span><span class="sxs-lookup"><span data-stu-id="0ca0b-470">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="0ca0b-471">Einmal die **Client-IP-Adresse** wird hinzugefügt, um die zulässigen IP-Adressen aufzulisten, klicken Sie auf **speichern** um die Änderungen zu bestätigen.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-471">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Bestätigen von Änderungen](aspnet-mvc-4-models-and-data-access/_static/image42.png)

    <span data-ttu-id="0ca0b-473">*Bestätigen von Änderungen*</span><span class="sxs-lookup"><span data-stu-id="0ca0b-473">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="0ca0b-474">Aufgabe 3: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web Deploy</span><span class="sxs-lookup"><span data-stu-id="0ca0b-474">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="0ca0b-475">Wechseln Sie zurück, der ASP.NET MVC 4-Projektmappe.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-475">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="0ca0b-476">In der **Projektmappen-Explorer**mit der rechten Maustaste auf das Websiteprojekt, und wählen Sie **veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-476">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="0ca0b-477">![Veröffentlichen der Anwendung](aspnet-mvc-4-models-and-data-access/_static/image43.png "Veröffentlichen der Anwendung")</span><span class="sxs-lookup"><span data-stu-id="0ca0b-477">![Publishing the Application](aspnet-mvc-4-models-and-data-access/_static/image43.png "Publishing the Application")</span></span>

    <span data-ttu-id="0ca0b-478">*Veröffentlichen der Website*</span><span class="sxs-lookup"><span data-stu-id="0ca0b-478">*Publishing the web site*</span></span>
2. <span data-ttu-id="0ca0b-479">Importieren Sie das Veröffentlichungsprofil, die, das Sie in der ersten Aufgabe gespeichert.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-479">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="0ca0b-480">![Importieren das Veröffentlichungsprofil](aspnet-mvc-4-models-and-data-access/_static/image44.png "Importieren des Veröffentlichungsprofils")</span><span class="sxs-lookup"><span data-stu-id="0ca0b-480">![Importing the publish profile](aspnet-mvc-4-models-and-data-access/_static/image44.png "Importing the publish profile")</span></span>

    <span data-ttu-id="0ca0b-481">*Importieren Sie das Veröffentlichungsprofil*</span><span class="sxs-lookup"><span data-stu-id="0ca0b-481">*Importing publish profile*</span></span>
3. <span data-ttu-id="0ca0b-482">Klicken Sie auf **überprüft, ob Verbindung**.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-482">Click **Validate Connection**.</span></span> <span data-ttu-id="0ca0b-483">Klicken Sie nach Abschluss der Überprüfung auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-483">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="0ca0b-484">Die Überprüfung ist abgeschlossen, wenn Sie sehen, dass ein grünes Häkchen neben der Schaltfläche "Verbindung überprüfen" angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-484">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="0ca0b-485">![Überprüfen der Verbindung](aspnet-mvc-4-models-and-data-access/_static/image45.png "Überprüfen der Verbindung")</span><span class="sxs-lookup"><span data-stu-id="0ca0b-485">![Validating connection](aspnet-mvc-4-models-and-data-access/_static/image45.png "Validating connection")</span></span>

    <span data-ttu-id="0ca0b-486">*Überprüfen der Verbindung*</span><span class="sxs-lookup"><span data-stu-id="0ca0b-486">*Validating connection*</span></span>
4. <span data-ttu-id="0ca0b-487">In der **Einstellungen** Seite die **Datenbanken** auf die Schaltfläche neben dem Textfeld für die datenbankverbindung (d. h. **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="0ca0b-487">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="0ca0b-488">![Web deploy-Konfiguration](aspnet-mvc-4-models-and-data-access/_static/image46.png "Web deploy-Konfiguration")</span><span class="sxs-lookup"><span data-stu-id="0ca0b-488">![Web deploy configuration](aspnet-mvc-4-models-and-data-access/_static/image46.png "Web deploy configuration")</span></span>

    <span data-ttu-id="0ca0b-489">*Web deploy-Konfiguration*</span><span class="sxs-lookup"><span data-stu-id="0ca0b-489">*Web deploy configuration*</span></span>
5. <span data-ttu-id="0ca0b-490">Konfigurieren Sie die Verbindung mit der Datenbank wie folgt:</span><span class="sxs-lookup"><span data-stu-id="0ca0b-490">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="0ca0b-491">In der **Servernamen** Geben Sie Ihre SQL-Datenbankserver URL mit der *Tcp:* Präfix.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-491">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="0ca0b-492">In **Benutzernamen** Geben Sie Ihre Server Administrator-Anmeldenamen.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-492">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="0ca0b-493">In **Kennwort** Geben Sie Ihre Server Administrator-Anmeldekennwort.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-493">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="0ca0b-494">Geben Sie einen neuen Datenbanknamen ein.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-494">Type a new database name.</span></span>

     <span data-ttu-id="0ca0b-495">![Konfigurieren von Ziel-Verbindungszeichenfolge](aspnet-mvc-4-models-and-data-access/_static/image47.png "Zielverbindungszeichenfolge konfigurieren")</span><span class="sxs-lookup"><span data-stu-id="0ca0b-495">![Configuring destination connection string](aspnet-mvc-4-models-and-data-access/_static/image47.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="0ca0b-496">*Konfigurieren von Ziel-Verbindungszeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="0ca0b-496">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="0ca0b-497">Klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-497">Then click **OK**.</span></span> <span data-ttu-id="0ca0b-498">Bei der Aufforderung zum Erstellen der Datenbank auf **Ja**.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-498">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="0ca0b-499">![Erstellen der Datenbank](aspnet-mvc-4-models-and-data-access/_static/image48.png "erstellen die datenbankzeichenfolge")</span><span class="sxs-lookup"><span data-stu-id="0ca0b-499">![Creating the database](aspnet-mvc-4-models-and-data-access/_static/image48.png "Creating the database string")</span></span>

    <span data-ttu-id="0ca0b-500">*Erstellen der Datenbank*</span><span class="sxs-lookup"><span data-stu-id="0ca0b-500">*Creating the database*</span></span>
7. <span data-ttu-id="0ca0b-501">Die Verbindungszeichenfolge, die Sie für die Verbindung mit SQL-Datenbank in Windows Azure verwendet werden, ist innerhalb der Verbindungstyp Standard Textfeld angezeigt.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-501">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="0ca0b-502">Klicken Sie dann auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-502">Then click **Next**.</span></span>

    <span data-ttu-id="0ca0b-503">![Auf der SQL-Datenbank-Verbindungszeichenfolge](aspnet-mvc-4-models-and-data-access/_static/image49.png "auf SQL-Datenbank-Verbindungszeichenfolge")</span><span class="sxs-lookup"><span data-stu-id="0ca0b-503">![Connection string pointing to SQL Database](aspnet-mvc-4-models-and-data-access/_static/image49.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="0ca0b-504">*Auf der SQL-Datenbank-Verbindungszeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="0ca0b-504">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="0ca0b-505">In der **Vorschau** auf **veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-505">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="0ca0b-506">![Veröffentlichen der Webanwendung](aspnet-mvc-4-models-and-data-access/_static/image50.png "Veröffentlichen der Webanwendung")</span><span class="sxs-lookup"><span data-stu-id="0ca0b-506">![Publishing the web application](aspnet-mvc-4-models-and-data-access/_static/image50.png "Publishing the web application")</span></span>

    <span data-ttu-id="0ca0b-507">*Veröffentlichen der Webanwendung*</span><span class="sxs-lookup"><span data-stu-id="0ca0b-507">*Publishing the web application*</span></span>
9. <span data-ttu-id="0ca0b-508">Sobald der Veröffentlichungsprozess abgeschlossen ist, öffnet Ihr Standardbrowser die veröffentlichte Website.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-508">Once the publishing process finishes, your default browser will open the published web site.</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="0ca0b-509">Anhang C: Verwenden von Codeausschnitten</span><span class="sxs-lookup"><span data-stu-id="0ca0b-509">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="0ca0b-510">Mit Codeausschnitten müssen Sie den Code zur Hand benötigten.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-510">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="0ca0b-511">Das Lab-Dokument informiert Sie genau wann sie verwendet werden kann wie in der folgenden Abbildung dargestellt.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-511">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="0ca0b-512">![Verwenden von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihrem Projekt](aspnet-mvc-4-models-and-data-access/_static/image51.png "mithilfe von Visual Studio-Codeausschnitten, die Code in das Projekt einfügen.")</span><span class="sxs-lookup"><span data-stu-id="0ca0b-512">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-models-and-data-access/_static/image51.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="0ca0b-513">*Verwenden von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihrem Projekt*</span><span class="sxs-lookup"><span data-stu-id="0ca0b-513">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="0ca0b-514">***Hinzufügen ein Codeausschnitts, die über die Tastatur (nur c#)***</span><span class="sxs-lookup"><span data-stu-id="0ca0b-514">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="0ca0b-515">Platzieren Sie den Cursor, wo Sie möchten den Code einfügen.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-515">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="0ca0b-516">Starten Sie den codeausschnittsnamen (ohne Leerzeichen oder Bindestriche) eingeben.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-516">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="0ca0b-517">Beobachten Sie, wie IntelliSense zeigt übereinstimmende Codeausschnitte Namen.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-517">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="0ca0b-518">Wählen Sie den richtigen Codeausschnitt (oder halten Sie eingeben, bis die gesamte codeausschnittsnamen ausgewählt ist).</span><span class="sxs-lookup"><span data-stu-id="0ca0b-518">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="0ca0b-519">Drücken Sie die Tab-Taste zweimal auf Einfügen des Codeausschnitts an der Cursorposition ein.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-519">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="0ca0b-520">![Geben Sie den Namen des Ausschnitts](aspnet-mvc-4-models-and-data-access/_static/image52.png "Geben Sie den Namen des Ausschnitts")</span><span class="sxs-lookup"><span data-stu-id="0ca0b-520">![Start typing the snippet name](aspnet-mvc-4-models-and-data-access/_static/image52.png "Start typing the snippet name")</span></span>

<span data-ttu-id="0ca0b-521">*Geben Sie den Namen des Ausschnitts*</span><span class="sxs-lookup"><span data-stu-id="0ca0b-521">*Start typing the snippet name*</span></span>

<span data-ttu-id="0ca0b-522">![Tabstopp drücken, um den hervorgehobenen Codeausschnitt auswählen](aspnet-mvc-4-models-and-data-access/_static/image53.png "Tab drücken, um den hervorgehobenen Codeausschnitt auswählen")</span><span class="sxs-lookup"><span data-stu-id="0ca0b-522">![Press Tab to select the highlighted snippet](aspnet-mvc-4-models-and-data-access/_static/image53.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="0ca0b-523">*Drücken Sie Tabstopp, um den hervorgehobenen Codeausschnitt auswählen*</span><span class="sxs-lookup"><span data-stu-id="0ca0b-523">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="0ca0b-524">![Drücken Sie die Tabulatortaste erneut, und der Ausschnitt erweitert](aspnet-mvc-4-models-and-data-access/_static/image54.png "drücken Sie die Tabulatortaste erneut, und der Ausschnitt werden erweitert.")</span><span class="sxs-lookup"><span data-stu-id="0ca0b-524">![Press Tab again and the snippet will expand](aspnet-mvc-4-models-and-data-access/_static/image54.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="0ca0b-525">*Drücken Sie die Tabulatortaste erneut, und der Ausschnitt werden erweitert.*</span><span class="sxs-lookup"><span data-stu-id="0ca0b-525">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="0ca0b-526">***Hinzufügen ein Codeausschnitts, die mit der Maus (c#, Visual Basic und XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-526">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="0ca0b-527">Mit der rechten Maustaste, in dem den Codeausschnitt eingefügt werden soll.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-527">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="0ca0b-528">Wählen Sie **Ausschnitt einfügen** gefolgt von **Meine Codeausschnitte**.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-528">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="0ca0b-529">Wählen Sie die relevante Codeausschnitte in der Liste, indem Sie darauf klicken.</span><span class="sxs-lookup"><span data-stu-id="0ca0b-529">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="0ca0b-530">![Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten](aspnet-mvc-4-models-and-data-access/_static/image55.png "mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten,")</span><span class="sxs-lookup"><span data-stu-id="0ca0b-530">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-models-and-data-access/_static/image55.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="0ca0b-531">*Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten*</span><span class="sxs-lookup"><span data-stu-id="0ca0b-531">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="0ca0b-532">![Wählen Sie die relevante Codeausschnitte in der Liste, indem Sie darauf klicken](aspnet-mvc-4-models-and-data-access/_static/image56.png "die relevante Codeausschnitte in der Liste auswählen, indem Sie darauf klicken")</span><span class="sxs-lookup"><span data-stu-id="0ca0b-532">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-models-and-data-access/_static/image56.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="0ca0b-533">*Wählen Sie die relevante Codeausschnitte in der Liste, indem Sie darauf klicken*</span><span class="sxs-lookup"><span data-stu-id="0ca0b-533">*Pick the relevant snippet from the list, by clicking on it*</span></span>
