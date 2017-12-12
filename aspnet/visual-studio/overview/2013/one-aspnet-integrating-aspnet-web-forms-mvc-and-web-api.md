---
uid: visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
title: "Praktische Übungseinheiten: One ASP.NET\": Integrieren von ASP.NET Web Forms, MVC und Web-API | Microsoft Docs"
author: rick-anderson
description: ASP.NET ist ein Framework zum Erstellen von Websites, apps und Dienste, die spezielle Technologien wie MVC, Web-API und andere. Mit der Erweiterung ASP.NET h...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: 4fe2558d-67cc-4d12-a5c1-6fb9f6f16137
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
msc.type: authoredcontent
ms.openlocfilehash: 55109723e566a9f7c66c1a59414377b05dbec760
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="hands-on-lab-one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api"></a><span data-ttu-id="be7ee-104">Praktische Übungseinheiten: One ASP.NET": Integrieren von ASP.NET Web Forms, MVC und Web-API</span><span class="sxs-lookup"><span data-stu-id="be7ee-104">Hands On Lab: One ASP.NET: Integrating ASP.NET Web Forms, MVC and Web API</span></span>
====================
<span data-ttu-id="be7ee-105">durch [Web Lager Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="be7ee-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="be7ee-106">Herunterladen von Web-Lager Training Kit</span><span class="sxs-lookup"><span data-stu-id="be7ee-106">Download Web Camps Training Kit</span></span>](http://aka.ms/webcamps-training-kit)

> <span data-ttu-id="be7ee-107">ASP.NET ist ein Framework zum Erstellen von Websites, apps und Dienste, die spezielle Technologien wie MVC, Web-API und andere.</span><span class="sxs-lookup"><span data-stu-id="be7ee-107">ASP.NET is a framework for building Web sites, apps and services using specialized technologies such as MVC, Web API and others.</span></span> <span data-ttu-id="be7ee-108">Mit der Erweiterung aufgetreten ASP.NET seit seiner Erstellung und die ausdrückliche müssen diese Technologien integriert haben, stattgefunden haben aktuelle Aufwand arbeiten für **One ASP.NET"**.</span><span class="sxs-lookup"><span data-stu-id="be7ee-108">With the expansion ASP.NET has seen since its creation and the expressed need to have these technologies integrated, there have been recent efforts in working towards **One ASP.NET**.</span></span>
> 
> <span data-ttu-id="be7ee-109">Visual Studio 2013 führt ein neues einheitliche Projektsystem können Sie eine Anwendung erstellen und verwenden die ASP.NET-Technologien in einem Projekt ein.</span><span class="sxs-lookup"><span data-stu-id="be7ee-109">Visual Studio 2013 introduces a new unified project system which lets you build an application and use all the ASP.NET technologies in one project.</span></span> <span data-ttu-id="be7ee-110">Dieses Feature entfällt die Notwendigkeit, eine Technologie zu Beginn eines Projekts und mit dem sie auswählen, und stattdessen empfiehlt die Verwendung mehrerer ASP.NET Frameworks in einem Projekt.</span><span class="sxs-lookup"><span data-stu-id="be7ee-110">This feature eliminates the need to pick one technology at the start of a project and stick with it, and instead encourages the use of multiple ASP.NET frameworks within one project.</span></span>
> 
> <span data-ttu-id="be7ee-111">Alle Beispielcode und Codeausschnitte sind im Web Lager Training Kit unter enthalten [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="be7ee-111">All sample code and snippets are included in the Web Camps Training Kit, available at [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).</span></span>


<a id="Overview"></a>
## <a name="overview"></a><span data-ttu-id="be7ee-112">Übersicht</span><span class="sxs-lookup"><span data-stu-id="be7ee-112">Overview</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="be7ee-113">Ziele</span><span class="sxs-lookup"><span data-stu-id="be7ee-113">Objectives</span></span>

<span data-ttu-id="be7ee-114">In dieser praktischen Übungseinheit erfahren Sie, wie Sie:</span><span class="sxs-lookup"><span data-stu-id="be7ee-114">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="be7ee-115">Erstellen einer Website auf der Grundlage der **One ASP.NET"** Projekttyp</span><span class="sxs-lookup"><span data-stu-id="be7ee-115">Create a Web site based on the **One ASP.NET** project type</span></span>
- <span data-ttu-id="be7ee-116">Verwenden von unterschiedlichen **ASP.NET** Frameworks wie **MVC** und **Web-API** im selben Projekt</span><span class="sxs-lookup"><span data-stu-id="be7ee-116">Use different **ASP.NET** frameworks like **MVC** and **Web API** in the same project</span></span>
- <span data-ttu-id="be7ee-117">Identifizieren Sie die Hauptkomponenten einer **ASP.NET** Anwendung</span><span class="sxs-lookup"><span data-stu-id="be7ee-117">Identify the main components of an **ASP.NET** application</span></span>
- <span data-ttu-id="be7ee-118">Nutzen Sie die **ASP.NET Gerüstbau** Framework zum automatischen Erstellen von Controller und Ansichten zum Ausführen von CRUD-Vorgängen basierend auf Ihren Modellklassen</span><span class="sxs-lookup"><span data-stu-id="be7ee-118">Take advantage of the **ASP.NET Scaffolding** framework to automatically create Controllers and Views to perform CRUD operations based on your model classes</span></span>
- <span data-ttu-id="be7ee-119">Verfügbar machen Sie den gleichen Satz von Informationen in den Computer und lesbare Formate, die mit dem richtigen Tool für jeden Auftrag</span><span class="sxs-lookup"><span data-stu-id="be7ee-119">Expose the same set of information in machine- and human-readable formats using the right tool for each job</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="be7ee-120">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="be7ee-120">Prerequisites</span></span>

<span data-ttu-id="be7ee-121">Folgendes ist erforderlich, um diese praktische Übungseinheit:</span><span class="sxs-lookup"><span data-stu-id="be7ee-121">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="be7ee-122">[Visual Studio Express 2013 für Web](https://www.microsoft.com/visualstudio/) oder höher</span><span class="sxs-lookup"><span data-stu-id="be7ee-122">[Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/) or greater</span></span>
- [<span data-ttu-id="be7ee-123">Visual Studio 2013 Update 1</span><span class="sxs-lookup"><span data-stu-id="be7ee-123">Visual Studio 2013 Update 1</span></span>](https://go.microsoft.com/fwlink/?LinkId=301714)

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="be7ee-124">Setup</span><span class="sxs-lookup"><span data-stu-id="be7ee-124">Setup</span></span>

<span data-ttu-id="be7ee-125">Um die Übungen in dieser praktischen Übungseinheit auszuführen, müssen Sie zunächst die Umgebung einrichten.</span><span class="sxs-lookup"><span data-stu-id="be7ee-125">In order to run the exercises in this hands-on lab, you will need to set up your environment first.</span></span>

1. <span data-ttu-id="be7ee-126">Öffnen Sie Windows-Explorer und Navigieren in des Labors **Quelle** Ordner.</span><span class="sxs-lookup"><span data-stu-id="be7ee-126">Open Windows Explorer and browse to the lab's **Source** folder.</span></span>
2. <span data-ttu-id="be7ee-127">Mit der rechten Maustaste auf **"Setup.cmd"** , und wählen Sie **als Administrator ausführen** um den Setupvorgang zu starten, die Ihrer Umgebung konfigurieren und installieren die Visual Studio-Codeausschnitte für diese Übungseinheit.</span><span class="sxs-lookup"><span data-stu-id="be7ee-127">Right-click on **Setup.cmd** and select **Run as administrator** to launch the setup process that will configure your environment and install the Visual Studio code snippets for this lab.</span></span>
3. <span data-ttu-id="be7ee-128">Wenn das Dialogfeld Benutzerkontensteuerung angezeigt wird, bestätigen Sie die Aktion aus, um den Vorgang fortzusetzen.</span><span class="sxs-lookup"><span data-stu-id="be7ee-128">If the User Account Control dialog box is shown, confirm the action to proceed.</span></span>

> [!NOTE]
> <span data-ttu-id="be7ee-129">Stellen Sie sicher, dass Sie alle Abhängigkeiten für diese Umgebung aktiviert haben, bevor Sie das Setup ausführen.</span><span class="sxs-lookup"><span data-stu-id="be7ee-129">Make sure you have checked all the dependencies for this lab before running the setup.</span></span>


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a><span data-ttu-id="be7ee-130">Verwenden die Codeausschnitte</span><span class="sxs-lookup"><span data-stu-id="be7ee-130">Using the Code Snippets</span></span>

<span data-ttu-id="be7ee-131">In der Lab-Dokument werden Sie aufgefordert, Codeblöcke einfügen.</span><span class="sxs-lookup"><span data-stu-id="be7ee-131">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="be7ee-132">Der Einfachheit halber die meisten dieser Code dient als Visual Studio-Codeausschnitte, die Sie in Visual Studio 2013 zu vermeiden, dass sie manuell hinzufügen aus zugreifen können.</span><span class="sxs-lookup"><span data-stu-id="be7ee-132">For your convenience, most of this code is provided as Visual Studio Code Snippets, which you can access from within Visual Studio 2013 to avoid having to add it manually.</span></span>

> [!NOTE]
> <span data-ttu-id="be7ee-133">Jede Übung wird eine beginnend Projektmappe befindet sich im beiliegen der **beginnen** Ordner der Übung, die Ihnen ermöglicht, jede Übung unabhängig von den anderen folgen.</span><span class="sxs-lookup"><span data-stu-id="be7ee-133">Each exercise is accompanied by a starting solution located in the **Begin** folder of the exercise that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="be7ee-134">Denken Sie daran, dass die Codeausschnitte, die während einer Übung hinzugefügt werden in diese Lösungen starten fehlen, werden und funktionieren möglicherweise nicht, bis die Übung abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="be7ee-134">Please be aware that the code snippets that are added during an exercise are missing from these starting solutions and may not work until you have completed the exercise.</span></span> <span data-ttu-id="be7ee-135">In den Quellcode für eine Übung, finden Sie auch eine **End** Ordner, der durch den Code, der aus den Schritten in der entsprechenden Übung führt Visual Studio-Projektmappe enthält.</span><span class="sxs-lookup"><span data-stu-id="be7ee-135">Inside the source code for an exercise, you will also find an **End** folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="be7ee-136">Sie können diese Lösungen als Leitfaden verwenden, wenn Sie weitere Hilfe benötigen, wie Sie mithilfe dieser praktischen Übungseinheit arbeiten.</span><span class="sxs-lookup"><span data-stu-id="be7ee-136">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>


* * *

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="be7ee-137">Übungen</span><span class="sxs-lookup"><span data-stu-id="be7ee-137">Exercises</span></span>

<span data-ttu-id="be7ee-138">Diese praktische Übungseinheit enthält die folgenden Übungen durcharbeiten:</span><span class="sxs-lookup"><span data-stu-id="be7ee-138">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="be7ee-139">Erstellen eines neuen Web Forms-Anwendungsprojekts</span><span class="sxs-lookup"><span data-stu-id="be7ee-139">Creating a New Web Forms Project</span></span>](#Exercise1)
2. [<span data-ttu-id="be7ee-140">Erstellen einen MVC-Controller, die mithilfe von Gerüstbau</span><span class="sxs-lookup"><span data-stu-id="be7ee-140">Creating an MVC Controller Using Scaffolding</span></span>](#Exercise2)
3. [<span data-ttu-id="be7ee-141">Erstellen eine Web-API-Controller, die mithilfe von Gerüstbau</span><span class="sxs-lookup"><span data-stu-id="be7ee-141">Creating a Web API Controller Using Scaffolding</span></span>](#Exercise3)

<span data-ttu-id="be7ee-142">Geschätzte Zeit zum Abschließen dieser testumgebung: **60 Minuten**</span><span class="sxs-lookup"><span data-stu-id="be7ee-142">Estimated time to complete this lab: **60 minutes**</span></span>

> [!NOTE]
> <span data-ttu-id="be7ee-143">Wenn Sie Visual Studio zum ersten Mal starten, müssen Sie eine der vordefinierten Einstellungen Sammlungen auswählen.</span><span class="sxs-lookup"><span data-stu-id="be7ee-143">When you first start Visual Studio, you must select one of the predefined settings collections.</span></span> <span data-ttu-id="be7ee-144">Jede vordefinierte Sammlung dient zur einer bestimmten Entwicklungsstil und Fensterlayouts, Editor-Verhalten, IntelliSense-Codeausschnitte und Optionen des Dialogfelds bestimmt.</span><span class="sxs-lookup"><span data-stu-id="be7ee-144">Each predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options.</span></span> <span data-ttu-id="be7ee-145">In dieser Übung wird beschrieben, die Aktionen erforderlich, um eine bestimmte Aufgabe in Visual Studio auszuführen, bei Verwendung der **allgemeine Entwicklungseinstellungen** Auflistung.</span><span class="sxs-lookup"><span data-stu-id="be7ee-145">The procedures in this lab describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection.</span></span> <span data-ttu-id="be7ee-146">Wählen Sie eine Auflistung von unterschiedlichen Einstellungen für Ihre Entwicklungsumgebung möglicherweise Unterschiede in den Schritten, die Sie berücksichtigen sollten.</span><span class="sxs-lookup"><span data-stu-id="be7ee-146">If you choose a different settings collection for your development environment, there may be differences in the steps that you should take into account.</span></span>


<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-new-web-forms-project"></a><span data-ttu-id="be7ee-147">Übung 1: Erstellen eines neuen Web Forms-Anwendungsprojekts</span><span class="sxs-lookup"><span data-stu-id="be7ee-147">Exercise 1: Creating a New Web Forms Project</span></span>

<span data-ttu-id="be7ee-148">In dieser Übung erstellen Sie eine neue Web Forms-Website in Visual Studio 2013 verwenden die **One ASP.NET"** einheitliche Projekt, dem Sie Web Forms, MVC und Web-API-Komponenten in der gleichen Anwendung leicht zu integrieren können.</span><span class="sxs-lookup"><span data-stu-id="be7ee-148">In this exercise you will create a new Web Forms site in Visual Studio 2013 using the **One ASP.NET** unified project experience, which will allow you to easily integrate Web Forms, MVC and Web API components in the same application.</span></span> <span data-ttu-id="be7ee-149">Anschließend untersuchen Sie die generierte Lösung und die Teile zu identifizieren, und abschließend sehen Sie die Website in Aktion.</span><span class="sxs-lookup"><span data-stu-id="be7ee-149">You will then explore the generated solution and identify its parts, and finally you will see the Web site in action.</span></span>

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-a-new-site-using-the-one-aspnet-experience"></a><span data-ttu-id="be7ee-150">Aufgabe 1 – erstellen einen neuen Standort mithilfe einer ASP.NET-Erfahrung</span><span class="sxs-lookup"><span data-stu-id="be7ee-150">Task 1 – Creating a New Site Using the One ASP.NET Experience</span></span>

<span data-ttu-id="be7ee-151">In dieser Aufgabe Sie starten eine neue Website erstellen, in Visual Studio auf der Grundlage der **One ASP.NET"** Projekttyp.</span><span class="sxs-lookup"><span data-stu-id="be7ee-151">In this task you will start creating a new Web site in Visual Studio based on the **One ASP.NET** project type.</span></span> <span data-ttu-id="be7ee-152">**Eine ASP.NET** vereinheitlicht alle ASP.NET-Technologien und bietet Ihnen die Möglichkeit, mischen und zuordnen diese wie gewünscht.</span><span class="sxs-lookup"><span data-stu-id="be7ee-152">**One ASP.NET** unifies all ASP.NET technologies and gives you the option to mix and match them as desired.</span></span> <span data-ttu-id="be7ee-153">Klicken Sie dann, erkennen Sie die verschiedenen Komponenten von Web Forms, MVC und Web-API, die live paralleler Ausführung innerhalb der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="be7ee-153">You will then recognize the different components of Web Forms, MVC and Web API that live side by side within your application.</span></span>

1. <span data-ttu-id="be7ee-154">Open **Visual Studio Express 2013 für Web** , und wählen Sie **Datei | Neues Projekt...**  um eine neue Projektmappe zu starten.</span><span class="sxs-lookup"><span data-stu-id="be7ee-154">Open **Visual Studio Express 2013 for Web** and select **File | New Project...** to start a new solution.</span></span>

    ![Erstellen eines neuen Projekts](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image1.png)

    <span data-ttu-id="be7ee-156">*Erstellen eines neuen Projekts*</span><span class="sxs-lookup"><span data-stu-id="be7ee-156">*Creating a New Project*</span></span>
2. <span data-ttu-id="be7ee-157">In der **neues Projekt** wählen Sie im Dialogfeld **ASP.NET-Webanwendung** unter der **Visual C# | Web** Registerkarte, und stellen Sie sicher, dass **.NET Framework 4.5** ausgewählt ist.</span><span class="sxs-lookup"><span data-stu-id="be7ee-157">In the **New Project** dialog box, select **ASP.NET Web Application** under the **Visual C# | Web** tab, and make sure **.NET Framework 4.5** is selected.</span></span> <span data-ttu-id="be7ee-158">Nennen Sie das Projekt *MyHybridSite*, wählen Sie eine **Speicherort** , und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="be7ee-158">Name the project *MyHybridSite*, choose a **Location** and click **OK**.</span></span>

    ![Neues ASP.NET Web Application-Projekt](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image2.png)

    <span data-ttu-id="be7ee-160">*Erstellen eines neuen Projekts für die ASP.NET-Webanwendung*</span><span class="sxs-lookup"><span data-stu-id="be7ee-160">*Creating a new ASP.NET Web Application project*</span></span>
3. <span data-ttu-id="be7ee-161">In der **neues ASP.NET-Projekt** wählen Sie im Dialogfeld die **Web Forms** Vorlage, und wählen die **MVC** und **Web-API** Optionen.</span><span class="sxs-lookup"><span data-stu-id="be7ee-161">In the **New ASP.NET Project** dialog box, select the **Web Forms** template and select the **MVC** and **Web API** options.</span></span> <span data-ttu-id="be7ee-162">Stellen Sie außerdem sicher, dass die **Authentifizierung** Option wird festgelegt, um **einzelne Benutzerkonten**.</span><span class="sxs-lookup"><span data-stu-id="be7ee-162">Also, make sure that the **Authentication** option is set to **Individual User Accounts**.</span></span> <span data-ttu-id="be7ee-163">Klicken Sie auf **OK** um den Vorgang fortzusetzen.</span><span class="sxs-lookup"><span data-stu-id="be7ee-163">Click **OK** to continue.</span></span>

    ![Erstellen ein neues Projekt mit der Web Forms-Projektvorlage, einschließlich von Web-API und MVC-Komponenten](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image3.png)

    <span data-ttu-id="be7ee-165">*Erstellen ein neues Projekt mit der Web Forms-Projektvorlage, einschließlich von Web-API und MVC-Komponenten*</span><span class="sxs-lookup"><span data-stu-id="be7ee-165">*Creating a new project with the Web Forms template, including Web API and MVC components*</span></span>
4. <span data-ttu-id="be7ee-166">Sie können jetzt die Struktur der generierten Lösung durchsuchen.</span><span class="sxs-lookup"><span data-stu-id="be7ee-166">You can now explore the structure of the generated solution.</span></span>

    ![Untersuchen die generierte Lösung](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image4.png)

    <span data-ttu-id="be7ee-168">*Untersuchen die generierte Lösung*</span><span class="sxs-lookup"><span data-stu-id="be7ee-168">*Exploring the generated solution*</span></span>

    1. <span data-ttu-id="be7ee-169">**Konto:** dieser Ordner enthält das Web Form-Seiten zum Registrieren, melden Sie sich an und Verwalten von Benutzerkonten für die Anwendung.</span><span class="sxs-lookup"><span data-stu-id="be7ee-169">**Account:** This folder contains the Web Form pages to register, log in to and manage the application's user accounts.</span></span> <span data-ttu-id="be7ee-170">In diesem Ordner hinzugefügt, wenn die **einzelne Benutzerkonten** Authentifizierungsoption während der Konfiguration der Web Forms-Projektvorlage ausgewählt ist.</span><span class="sxs-lookup"><span data-stu-id="be7ee-170">This folder is added when the **Individual User Accounts** authentication option is selected during the configuration of the Web Forms project template.</span></span>
    2. <span data-ttu-id="be7ee-171">**Modelle:** dieser Ordner enthält die Klassen, die Ihre Anwendungsdaten darstellen.</span><span class="sxs-lookup"><span data-stu-id="be7ee-171">**Models:** This folder will contain the classes that represent your application data.</span></span>
    3. <span data-ttu-id="be7ee-172">**Controller** und **Ansichten**: Diese Ordner sind erforderlich, damit die **ASP.NET-MVC** und **ASP.NET Web API** Komponenten.</span><span class="sxs-lookup"><span data-stu-id="be7ee-172">**Controllers** and **Views**: These folders are required for the **ASP.NET MVC** and **ASP.NET Web API** components.</span></span> <span data-ttu-id="be7ee-173">Untersuchen Sie die Technologien MVC und Web-API in der nächsten Übung.</span><span class="sxs-lookup"><span data-stu-id="be7ee-173">You will explore the MVC and Web API technologies in the next exercises.</span></span>
    4. <span data-ttu-id="be7ee-174">Die **"default.aspx"**, **Contact.aspx** und **About.aspx** Dateien sind vordefinierte Web Form-Seiten, die Sie als Ausgangspunkt verwenden können, erstellen Sie die Seiten, die spezifisch für Ihre die Anwendung.</span><span class="sxs-lookup"><span data-stu-id="be7ee-174">The **Default.aspx**, **Contact.aspx** and **About.aspx** files are pre-defined Web Form pages that you can use as starting points to build the pages specific to your application.</span></span> <span data-ttu-id="be7ee-175">Die Programmierlogik der Dateien befindet sich in einer separaten Datei genannt der &quot;Code-Behind-&quot; gemäß der ein &quot;. aspx.cs&quot; oder &quot;. aspx.vb&quot; Erweiterung (je nach den verwendete Sprache).</span><span class="sxs-lookup"><span data-stu-id="be7ee-175">The programming logic of those files resides in a separate file referred to as the &quot;code-behind&quot; file, which has an &quot;.aspx.vb&quot; or &quot;.aspx.cs&quot; extension (depending on the language used).</span></span> <span data-ttu-id="be7ee-176">Die Code-Behind-Logik auf dem Server ausgeführt wird und die HTML-Ausgabe für die Seite dynamisch erzeugt.</span><span class="sxs-lookup"><span data-stu-id="be7ee-176">The code-behind logic runs on the server and dynamically produces the HTML output for your page.</span></span>
    5. <span data-ttu-id="be7ee-177">Die **Site.Master** und **Site.Mobile.Master** Seiten definieren das Aussehen und Verhalten und das Standardverhalten aller Seiten in der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="be7ee-177">The **Site.Master** and **Site.Mobile.Master** pages define the look and feel and the standard behavior of all the pages in the application.</span></span>
5. <span data-ttu-id="be7ee-178">Doppelklicken Sie auf die **"default.aspx"** Datei, um den Inhalt der Seite zu untersuchen.</span><span class="sxs-lookup"><span data-stu-id="be7ee-178">Double-click the **Default.aspx** file to explore the content of the page.</span></span>

    ![Untersuchen die Seite "default.aspx"](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image5.png)

    <span data-ttu-id="be7ee-180">*Untersuchen die Seite "default.aspx"*</span><span class="sxs-lookup"><span data-stu-id="be7ee-180">*Exploring the Default.aspx page*</span></span>

    > [!NOTE]
    > <span data-ttu-id="be7ee-181">Die **Seite** Direktive am Anfang der Datei definiert die Attribute eines Web Forms-Seite.</span><span class="sxs-lookup"><span data-stu-id="be7ee-181">The **Page** directive at the top of the file defines the attributes of the Web Forms page.</span></span> <span data-ttu-id="be7ee-182">Z. B. die **MasterPageFile** -Attribut gibt den Pfad zum Master Seite – in diesem Fall die *Site.Master* Seiten- und **Inherits** Attribut definiert die Code-Behind-Klasse für die Seite zu erben.</span><span class="sxs-lookup"><span data-stu-id="be7ee-182">For example, the **MasterPageFile** attribute specifies the path to the master page -in this case, the *Site.Master* page- and the **Inherits** attribute defines the code-behind class for the page to inherit.</span></span> <span data-ttu-id="be7ee-183">Diese Klasse befindet sich in der Datei, die bestimmt, indem die **CodeBehind** Attribut.</span><span class="sxs-lookup"><span data-stu-id="be7ee-183">This class is located in the file determined by the **CodeBehind** attribute.</span></span>
    > 
    > <span data-ttu-id="be7ee-184">Die **Asp: Inhalt** Steuerelement enthält den tatsächlichen Inhalt der Seite (Text, Markup und Steuerelementen) und zugeordnet wird eine **Asp: ContentPlaceHolder** Steuerelement auf der Masterseite.</span><span class="sxs-lookup"><span data-stu-id="be7ee-184">The **asp:Content** control holds the actual content of the page (text, markup and controls) and is mapped to a **asp:ContentPlaceHolder** control on the master page.</span></span> <span data-ttu-id="be7ee-185">In diesem Fall wird der Inhalt der Seite gerendert werden, in der *MainContent* Steuerelement definiert, der *Site.Master* Seite.</span><span class="sxs-lookup"><span data-stu-id="be7ee-185">In this case, the page content will be rendered inside the *MainContent* control defined in the *Site.Master* page.</span></span>
6. <span data-ttu-id="be7ee-186">Erweitern Sie die **App\_starten** Ordner, und beachten Sie, dass die **WebApiConfig.cs** Datei.</span><span class="sxs-lookup"><span data-stu-id="be7ee-186">Expand the **App\_Start** folder and notice the **WebApiConfig.cs** file.</span></span> <span data-ttu-id="be7ee-187">Visual Studio enthalten diese Datei in der generierten Lösung, da Sie beim Konfigurieren von Ihrem Projekts mit der Vorlage eine ASP.NET Web-API enthalten.</span><span class="sxs-lookup"><span data-stu-id="be7ee-187">Visual Studio included that file in the generated solution because you included Web API when configuring your project with the One ASP.NET template.</span></span>
7. <span data-ttu-id="be7ee-188">Öffnen der **WebApiConfig.cs** Datei.</span><span class="sxs-lookup"><span data-stu-id="be7ee-188">Open the **WebApiConfig.cs** file.</span></span> <span data-ttu-id="be7ee-189">In der *"webapiconfig"* Klasse finden Sie die Konfiguration zugeordnet Web-API, die HTTP zugeordnet wird leitet **Web-API-Controller**.</span><span class="sxs-lookup"><span data-stu-id="be7ee-189">In the *WebApiConfig* class you will find the configuration associated with Web API, which maps HTTP routes to **Web API controllers**.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample1.cs)]
8. <span data-ttu-id="be7ee-190">Öffnen der **RouteConfig.cs** Datei.</span><span class="sxs-lookup"><span data-stu-id="be7ee-190">Open the **RouteConfig.cs** file.</span></span> <span data-ttu-id="be7ee-191">Innerhalb der *RegisterRoutes* Methode finden Sie die Konfiguration zugeordnet MVC, das die HTTP-Routen an ordnet **MVC-Controller**.</span><span class="sxs-lookup"><span data-stu-id="be7ee-191">Inside the *RegisterRoutes* method you will find the configuration associated with MVC, which maps HTTP routes to **MVC controllers**.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample2.cs)]

<a id="Ex1Task2"></a>
#### <a name="task-2--running-the-solution"></a><span data-ttu-id="be7ee-192">Aufgabe 2: Ausführen der Projektmappe</span><span class="sxs-lookup"><span data-stu-id="be7ee-192">Task 2 – Running the Solution</span></span>

<span data-ttu-id="be7ee-193">Sie wird in dieser Aufgabe führen Sie die Projektmappe generierte, untersuchen Sie die app und einige Funktionen, wie URLs und die integrierte Authentifizierung.</span><span class="sxs-lookup"><span data-stu-id="be7ee-193">In this task you will run the generated solution, explore the app and some of its features, like URL rewriting and built-in authentication.</span></span>

1. <span data-ttu-id="be7ee-194">Um die Projektmappe auszuführen, drücken Sie die **F5** oder klicken Sie auf die **starten** Schaltfläche auf der Symbolleiste befinden.</span><span class="sxs-lookup"><span data-stu-id="be7ee-194">To run the solution, press **F5** or click the **Start** button located on the toolbar.</span></span> <span data-ttu-id="be7ee-195">Die Startseite der Anwendung sollte im Browser geöffnet werden.</span><span class="sxs-lookup"><span data-stu-id="be7ee-195">The application home page should open in the browser.</span></span>

    ![Ausführen der Projektmappe](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image6.png)
2. <span data-ttu-id="be7ee-197">Stellen Sie sicher, dass die Web Forms-Seiten aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="be7ee-197">Verify that the Web Forms pages are being invoked.</span></span> <span data-ttu-id="be7ee-198">Zu diesem Zweck fügen **/contact.aspx** an die URL in die Adressleiste ein und drücken Sie **EINGABETASTE**.</span><span class="sxs-lookup"><span data-stu-id="be7ee-198">To do this, append **/contact.aspx** to the URL in the address bar and press **Enter**.</span></span>

    ![Friendly URLs](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image7.png)

    <span data-ttu-id="be7ee-200">*Friendly URLs*</span><span class="sxs-lookup"><span data-stu-id="be7ee-200">*Friendly URLs*</span></span>

    > [!NOTE]
    > <span data-ttu-id="be7ee-201">Wie Sie sehen können, ändert sich die URL in **/wenden Sie sich an**.</span><span class="sxs-lookup"><span data-stu-id="be7ee-201">As you can see, the URL changes to **/contact**.</span></span> <span data-ttu-id="be7ee-202">Beginnend mit **ASP.NET 4**, URL-routing-Funktionen wurden hinzugefügt, um Web Forms, damit Sie schreiben können, wie URLs  *[http://www.mysite.com/products/software](http://www.mysite.com/products/software)*  statt  *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)*.</span><span class="sxs-lookup"><span data-stu-id="be7ee-202">Starting from **ASP.NET 4**, URL routing capabilities were added to Web Forms, so you can write URLs like *[http://www.mysite.com/products/software](http://www.mysite.com/products/software)* instead of *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)*.</span></span> <span data-ttu-id="be7ee-203">Weitere Informationen finden Sie unter [URL-Routing](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md).</span><span class="sxs-lookup"><span data-stu-id="be7ee-203">For more information refer to [URL Routing](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md).</span></span>
3. <span data-ttu-id="be7ee-204">Untersuchen Sie nun die Authentifizierungsablauf in die Anwendung integriert.</span><span class="sxs-lookup"><span data-stu-id="be7ee-204">You will now explore the authentication flow integrated into the application.</span></span> <span data-ttu-id="be7ee-205">Klicken Sie hierzu auf **registrieren** in der oberen rechten Ecke der Seite.</span><span class="sxs-lookup"><span data-stu-id="be7ee-205">To do this, click **Register** in the upper-right corner of the page.</span></span>

    ![Registrieren eines neuen Benutzers](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image8.png)

    <span data-ttu-id="be7ee-207">*Registrieren eines neuen Benutzers*</span><span class="sxs-lookup"><span data-stu-id="be7ee-207">*Registering a new user*</span></span>
4. <span data-ttu-id="be7ee-208">In der **registrieren** geben eine **Benutzername** und **Kennwort**, und klicken Sie dann auf **registrieren**.</span><span class="sxs-lookup"><span data-stu-id="be7ee-208">In the **Register** page, enter a **User name** and **Password**, and then click **Register**.</span></span>

    ![Seite "registrieren"](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image9.png)

    <span data-ttu-id="be7ee-210">*Seite "registrieren"*</span><span class="sxs-lookup"><span data-stu-id="be7ee-210">*Register page*</span></span>
5. <span data-ttu-id="be7ee-211">Die Anwendung wird das neue Konto registriert, und der Benutzer authentifiziert ist.</span><span class="sxs-lookup"><span data-stu-id="be7ee-211">The application registers the new account, and the user is authenticated.</span></span>

    ![Benutzerauthentifizierung](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image10.png)

    <span data-ttu-id="be7ee-213">*Benutzerauthentifizierung*</span><span class="sxs-lookup"><span data-stu-id="be7ee-213">*User authenticated*</span></span>
6. <span data-ttu-id="be7ee-214">Wechseln Sie zurück zu Visual Studio, und drücken Sie **UMSCHALT + F5** debugging beenden.</span><span class="sxs-lookup"><span data-stu-id="be7ee-214">Go back to Visual Studio and press **SHIFT + F5** to stop debugging.</span></span>

<a id="Exercise2"></a>
### <a name="exercise-2-creating-an-mvc-controller-using-scaffolding"></a><span data-ttu-id="be7ee-215">Übung 2: Erstellen eines MVC-Controllers mithilfe von Gerüstbau</span><span class="sxs-lookup"><span data-stu-id="be7ee-215">Exercise 2: Creating an MVC Controller Using Scaffolding</span></span>

<span data-ttu-id="be7ee-216">In dieser Übung gelangen Sie sicherungsframework von ASP.NET Gerüstbau bereitgestellt, die von Visual Studio und erstellen einen ASP.NET MVC 5-Controller mit Aktionen und Razor-Ansichten zum CRUD-Vorgänge ausgeführt werden, ohne eine einzige Codezeile schreiben zu müssen.</span><span class="sxs-lookup"><span data-stu-id="be7ee-216">In this exercise you will take advantage of the ASP.NET Scaffolding framework provided by Visual Studio to create an ASP.NET MVC 5 controller with actions and Razor views to perform CRUD operations, without writing a single line of code.</span></span> <span data-ttu-id="be7ee-217">Der Prozess des Gerüstbaus wird Entity Framework Code First verwenden, um den Datenkontext und das Datenbankschema in der SQL-Datenbank zu generieren.</span><span class="sxs-lookup"><span data-stu-id="be7ee-217">The scaffolding process will use Entity Framework Code First to generate the data context and the database schema in the SQL database.</span></span>

<span data-ttu-id="be7ee-218">**Über Entity Framework Code First**</span><span class="sxs-lookup"><span data-stu-id="be7ee-218">**About Entity Framework Code First**</span></span>

<span data-ttu-id="be7ee-219">Entity Framework (EF) ist eine objektrelationale Mapper (ORM), die Ihnen zum Erstellen von datenzugriffsanwendungen mittels Programmierung mit einem konzeptionellen Anwendungsmodell anstelle des Programmierens direkt mit einem Speicherschema ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="be7ee-219">Entity Framework (EF) is an object-relational mapper (ORM) that enables you to create data access applications by programming with a conceptual application model instead of programming directly using a relational storage schema.</span></span>

<span data-ttu-id="be7ee-220">Der Entity Framework Code First Modellierung-Workflow ermöglicht es Ihnen, Ihren eigenen Domänenklassen zu verwenden, um das Modell darzustellen, dem EF beim Ausführen von Abfragen nutzt, Nachverfolgen von Änderungen und Update-Funktionen.</span><span class="sxs-lookup"><span data-stu-id="be7ee-220">The Entity Framework Code First modeling workflow allows you to use your own domain classes to represent the model that EF relies on when performing querying, change-tracking and updating functions.</span></span> <span data-ttu-id="be7ee-221">Mithilfe des Code First-Entwicklung-Workflows, müssen nicht auf Ihre Anwendung zu beginnen, indem Sie das Erstellen einer Datenbank oder das Angeben eines Schemas.</span><span class="sxs-lookup"><span data-stu-id="be7ee-221">Using the Code First development workflow, you do not need to begin your application by creating a database or specifying a schema.</span></span> <span data-ttu-id="be7ee-222">Stattdessen können Sie .NET Standardklassen, die definieren, die am besten geeigneten domänenmodellobjekten für Ihre Anwendung schreiben, und Entity Framework wird die Datenbank für Sie erstellt.</span><span class="sxs-lookup"><span data-stu-id="be7ee-222">Instead, you can write standard .NET classes that define the most appropriate domain model objects for your application, and Entity Framework will create the database for you.</span></span>

> [!NOTE]
> <span data-ttu-id="be7ee-223">Weitere Informationen finden Sie Informationen zu Entity Framework [hier](../../../entity-framework.md).</span><span class="sxs-lookup"><span data-stu-id="be7ee-223">You can learn more about Entity Framework [here](../../../entity-framework.md).</span></span>


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-new-model"></a><span data-ttu-id="be7ee-224">Aufgabe 1 – erstellen ein neues Modell</span><span class="sxs-lookup"><span data-stu-id="be7ee-224">Task 1 – Creating a New Model</span></span>

<span data-ttu-id="be7ee-225">Definieren Sie jetzt eine **Person** -Klasse, die das Modell durch den Prozess des Gerüstbaus zum Erstellen von MVC-Controller und Ansichten verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="be7ee-225">You will now define a **Person** class, which will be the model used by the scaffolding process to create the MVC controller and the views.</span></span> <span data-ttu-id="be7ee-226">Beginnen Sie mit dem Erstellen einer **Person** Modellklasse, und die CRUD-Vorgänge im Controller automatisch erstellt werden mithilfe von Gerüstbau Funktionen.</span><span class="sxs-lookup"><span data-stu-id="be7ee-226">You will start by creating a **Person** model class, and the CRUD operations in the controller will be automatically created using scaffolding features.</span></span>

1. <span data-ttu-id="be7ee-227">Open **Visual Studio Express 2013 für Web** und die **MyHybridSite.sln** Projektmappe befindet sich in der **Quell-/Ex2-MvcScaffolding/Anfang** Ordner.</span><span class="sxs-lookup"><span data-stu-id="be7ee-227">Open **Visual Studio Express 2013 for Web** and the **MyHybridSite.sln** solution located in the **Source/Ex2-MvcScaffolding/Begin** folder.</span></span> <span data-ttu-id="be7ee-228">Alternativ können Sie die Projektmappe fortsetzen, dass Sie im vorherigen Schritt abgerufen haben.</span><span class="sxs-lookup"><span data-stu-id="be7ee-228">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>
2. <span data-ttu-id="be7ee-229">In **Projektmappen-Explorer**, mit der rechten Maustaste die **Modelle** Ordner, der die **MyHybridSite** Projekt, und wählen Sie **hinzufügen | Klasse...** .</span><span class="sxs-lookup"><span data-stu-id="be7ee-229">In **Solution Explorer**, right-click the **Models** folder of the **MyHybridSite** project and select **Add | Class...**.</span></span>

    ![Modellklasse von Person hinzufügen](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image11.png)

    <span data-ttu-id="be7ee-231">*Modellklasse von Person hinzufügen*</span><span class="sxs-lookup"><span data-stu-id="be7ee-231">*Adding the Person model class*</span></span>
3. <span data-ttu-id="be7ee-232">In der **neues Element hinzufügen** (Dialogfeld), benennen Sie die Datei *Person.cs* , und klicken Sie auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="be7ee-232">In the **Add New Item** dialog box, name the file *Person.cs* and click **Add**.</span></span>

    ![Erstellen der Person-Modell-Klasse](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image12.png)

    <span data-ttu-id="be7ee-234">*Erstellen der Person-Modell-Klasse*</span><span class="sxs-lookup"><span data-stu-id="be7ee-234">*Creating the Person model class*</span></span>
4. <span data-ttu-id="be7ee-235">Ersetzen Sie den Inhalt der **Person.cs** Datei durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="be7ee-235">Replace the content of the **Person.cs** file with the following code.</span></span> <span data-ttu-id="be7ee-236">Drücken Sie **STRG + S** um die Änderungen zu speichern.</span><span class="sxs-lookup"><span data-stu-id="be7ee-236">Press **CTRL + S** to save the changes.</span></span>

    <span data-ttu-id="be7ee-237">(Codeausschnitt - *BringingTogetherOneAspNet - Ex2 - PersonClass*)</span><span class="sxs-lookup"><span data-stu-id="be7ee-237">(Code Snippet - *BringingTogetherOneAspNet - Ex2 - PersonClass*)</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample3.cs)]
5. <span data-ttu-id="be7ee-238">In **Projektmappen-Explorer**, mit der rechten Maustaste die **MyHybridSite** Projekt, und wählen Sie **erstellen**, oder drücken Sie **STRG + UMSCHALT + B** zum Erstellen des Projekts.</span><span class="sxs-lookup"><span data-stu-id="be7ee-238">In **Solution Explorer**, right-click the **MyHybridSite** project and select **Build**, or press **CTRL + SHIFT + B** to build the project.</span></span>

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-an-mvc-controller"></a><span data-ttu-id="be7ee-239">Aufgabe 2: Erstellen eines MVC-Controllers</span><span class="sxs-lookup"><span data-stu-id="be7ee-239">Task 2 – Creating an MVC Controller</span></span>

<span data-ttu-id="be7ee-240">Nachdem die **Person** Modell erstellt wurde, verwenden Sie ASP.NET-MVC-Gerüstbau mit Entity Framework zum Erstellen der CRUD-Controlleraktionen und Ansichten für **Person**.</span><span class="sxs-lookup"><span data-stu-id="be7ee-240">Now that the **Person** model is created, you will use ASP.NET MVC scaffolding with Entity Framework to create the CRUD controller actions and views for **Person**.</span></span>

1. <span data-ttu-id="be7ee-241">In **Projektmappen-Explorer**, mit der rechten Maustaste die **Controller** Ordner, der die **MyHybridSite** Projekt, und wählen Sie **hinzufügen | Neues Gerüst...** .</span><span class="sxs-lookup"><span data-stu-id="be7ee-241">In **Solution Explorer**, right-click the **Controllers** folder of the **MyHybridSite** project and select **Add | New Scaffolded Item...**.</span></span>

    ![Erstellen eines neuen scaffolded Domänencontrollers](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image13.png)

    <span data-ttu-id="be7ee-243">*Erstellen eines neuen Migrationsgerüst-Domänencontrollers*</span><span class="sxs-lookup"><span data-stu-id="be7ee-243">*Creating a new Scaffolded Controller*</span></span>
2. <span data-ttu-id="be7ee-244">In der **Gerüst hinzufügen** wählen Sie im Dialogfeld **MVC 5-Controller mit Ansichten unter Verwendung von Entity Framework** , und klicken Sie dann auf **hinzufügen.**</span><span class="sxs-lookup"><span data-stu-id="be7ee-244">In the **Add Scaffold** dialog box, select **MVC 5 Controller with views, using Entity Framework** and then click **Add.**</span></span>

    ![MVC 5-Controller mit Ansichten und Entity Framework auswählen](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image14.png)

    <span data-ttu-id="be7ee-246">*MVC 5-Controller mit Ansichten und Entity Framework auswählen*</span><span class="sxs-lookup"><span data-stu-id="be7ee-246">*Selecting MVC 5 Controller with views and Entity Framework*</span></span>
3. <span data-ttu-id="be7ee-247">Legen Sie *MvcPersonController* als der **Controllernamen**, wählen die **asynchrone Controlleraktionen verwenden** aus, und wählen Sie **Person (MyHybridSite.Models)**  als die **Modellschemas**.</span><span class="sxs-lookup"><span data-stu-id="be7ee-247">Set *MvcPersonController* as the **Controller name**, select the **Use async controller actions** option and select **Person (MyHybridSite.Models)** as the **Model class**.</span></span>

    ![Einen MVC-Controller mit Gerüst hinzufügen](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image15.png)

    <span data-ttu-id="be7ee-249">*Einen MVC-Controller mit Gerüst hinzufügen*</span><span class="sxs-lookup"><span data-stu-id="be7ee-249">*Adding an MVC controller with scaffolding*</span></span>
4. <span data-ttu-id="be7ee-250">Klicken Sie unter **Datenkontextklasse**, klicken Sie auf **neuen Datenkontext...** .</span><span class="sxs-lookup"><span data-stu-id="be7ee-250">Under **Data context class**, click **New data context...**.</span></span>

    ![Erstellen einen neuen Datenkontext](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image16.png)

    <span data-ttu-id="be7ee-252">*Erstellen einen neuen Datenkontext*</span><span class="sxs-lookup"><span data-stu-id="be7ee-252">*Creating a new data context*</span></span>
5. <span data-ttu-id="be7ee-253">In der **neuen Datenkontext** Dialogfeld Feld "Name" den neuen Datenkontext *PersonContext* , und klicken Sie auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="be7ee-253">In the **New Data Context** dialog box, name the new data context *PersonContext* and click **Add**.</span></span>

    ![Erstellen neue PersonContext](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image17.png)

    <span data-ttu-id="be7ee-255">*Erstellen den neuen PersonContext-Typ*</span><span class="sxs-lookup"><span data-stu-id="be7ee-255">*Creating the new PersonContext type*</span></span>
6. <span data-ttu-id="be7ee-256">Klicken Sie auf **hinzufügen** zum Erstellen der neuen Controllers für **Person** mit Gerüstbau.</span><span class="sxs-lookup"><span data-stu-id="be7ee-256">Click **Add** to create the new controller for **Person** with scaffolding.</span></span> <span data-ttu-id="be7ee-257">Visual Studio generiert die Controlleraktionen, den Datenkontext Person und Razor-Ansichten.</span><span class="sxs-lookup"><span data-stu-id="be7ee-257">Visual Studio will then generate the controller actions, the Person data context and the Razor views.</span></span>

    ![Nach dem Erstellen der MVC-Controller mit Gerüstbau](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image18.png)

    <span data-ttu-id="be7ee-259">*Nach dem Erstellen der MVC-Controller mit Gerüstbau*</span><span class="sxs-lookup"><span data-stu-id="be7ee-259">*After creating the MVC controller with scaffolding*</span></span>
7. <span data-ttu-id="be7ee-260">Öffnen der **MvcPersonController.cs** in der Datei die **Controller** Ordner.</span><span class="sxs-lookup"><span data-stu-id="be7ee-260">Open the **MvcPersonController.cs** file in the **Controllers** folder.</span></span> <span data-ttu-id="be7ee-261">Beachten Sie, dass die CRUD-Aktionsmethoden automatisch generiert wurden.</span><span class="sxs-lookup"><span data-stu-id="be7ee-261">Notice that the CRUD action methods have been generated automatically.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample4.cs)]

    > [!NOTE]
    > <span data-ttu-id="be7ee-262">Durch Auswahl der **asynchrone Controlleraktionen verwenden** Kontrollkästchen aus das Gerüst Optionen in den vorherigen Schritten, generiert Visual Studio asynchrone Aktionsmethoden für alle Aktionen, die Zugriff auf den Datenkontext Person einschließen.</span><span class="sxs-lookup"><span data-stu-id="be7ee-262">By selecting the **Use async controller actions** check box from the scaffolding options in the previous steps, Visual Studio generates asynchronous action methods for all actions that involve access to the Person data context.</span></span> <span data-ttu-id="be7ee-263">Es wird empfohlen, die Verwendung asynchroner Aktionsmethoden für lang andauernde, nicht CPU-gebundene Anforderungen, um zu vermeiden, dass der Webserver aus arbeiten ausführen, während die Anforderung verarbeitet wird.</span><span class="sxs-lookup"><span data-stu-id="be7ee-263">It is recommended that you use asynchronous action methods for long-running, non-CPU bound requests to avoid blocking the Web server from performing work while the request is being processed.</span></span>

<a id="Ex2Task3"></a>
#### <a name="task-3--running-the-solution"></a><span data-ttu-id="be7ee-264">Aufgabe 3: Ausführen der Projektmappe</span><span class="sxs-lookup"><span data-stu-id="be7ee-264">Task 3 – Running the Solution</span></span>

<span data-ttu-id="be7ee-265">In dieser Aufgabe führen Sie der Projektmappe erneut aus, um zu überprüfen, ob die Ansichten für **Person** sind wie erwartet funktioniert.</span><span class="sxs-lookup"><span data-stu-id="be7ee-265">In this task, you will run the solution again to verify that the views for **Person** are working as expected.</span></span> <span data-ttu-id="be7ee-266">Fügen Sie eine neue Person ein, um sicherzustellen, dass es erfolgreich in der Datenbank gespeichert ist.</span><span class="sxs-lookup"><span data-stu-id="be7ee-266">You will add a new person to verify that it is successfully saved to the database.</span></span>

1. <span data-ttu-id="be7ee-267">Drücken Sie **F5** um die Projektmappe auszuführen.</span><span class="sxs-lookup"><span data-stu-id="be7ee-267">Press **F5** to run the solution.</span></span>
2. <span data-ttu-id="be7ee-268">Navigieren Sie zu **/MvcPerson**.</span><span class="sxs-lookup"><span data-stu-id="be7ee-268">Navigate to **/MvcPerson**.</span></span> <span data-ttu-id="be7ee-269">Die scaffolded an, die die Liste der Personen zeigt, sollte angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="be7ee-269">The scaffolded view that shows the list of people should appear.</span></span>
3. <span data-ttu-id="be7ee-270">Klicken Sie auf **neu erstellen** eine neue Person hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="be7ee-270">Click **Create New** to add a new person.</span></span>

    ![Navigieren zu der scaffolded MVC-Ansichten](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image19.png)

    <span data-ttu-id="be7ee-272">*Navigieren zu der scaffolded MVC-Ansichten*</span><span class="sxs-lookup"><span data-stu-id="be7ee-272">*Navigating to the scaffolded MVC views*</span></span>
4. <span data-ttu-id="be7ee-273">In der **erstellen** anzuzeigen, geben Sie einen **Namen** und ein **Alter** für die Person, und klicken Sie auf **erstellen**.</span><span class="sxs-lookup"><span data-stu-id="be7ee-273">In the **Create** view, provide a **Name** and an **Age** for the person, and click **Create**.</span></span>

    ![Eine neue Person hinzufügen](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image20.png)

    <span data-ttu-id="be7ee-275">*Eine neue Person hinzufügen*</span><span class="sxs-lookup"><span data-stu-id="be7ee-275">*Adding a new person*</span></span>
5. <span data-ttu-id="be7ee-276">Die neue Person wird zur Liste hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="be7ee-276">The new person is added to the list.</span></span> <span data-ttu-id="be7ee-277">Klicken Sie in der Liste der Elemente auf **Details** der Person-Detailansicht angezeigt.</span><span class="sxs-lookup"><span data-stu-id="be7ee-277">In the element list, click **Details** to display the person's details view.</span></span> <span data-ttu-id="be7ee-278">Klicken Sie auf die **Details** anzuzeigen, klicken Sie auf **zurück zur Listenansicht** um zurück zur Listenansicht zu wechseln.</span><span class="sxs-lookup"><span data-stu-id="be7ee-278">Then, in the **Details** view, click **Back to List** to go back to the list view.</span></span>

    ![Person Details anzuzeigen](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image21.png)

    <span data-ttu-id="be7ee-280">*Person Details anzuzeigen*</span><span class="sxs-lookup"><span data-stu-id="be7ee-280">*Person's details view*</span></span>
6. <span data-ttu-id="be7ee-281">Klicken Sie auf die **löschen** Link zu der Person zu löschen.</span><span class="sxs-lookup"><span data-stu-id="be7ee-281">Click the **Delete** link to delete the person.</span></span> <span data-ttu-id="be7ee-282">In der **löschen** anzuzeigen, klicken Sie auf **löschen** zum Bestätigen des Vorgangs.</span><span class="sxs-lookup"><span data-stu-id="be7ee-282">In the **Delete** view, click **Delete** to confirm the operation.</span></span>

    ![Durch das Löschen einer person](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image22.png)

    <span data-ttu-id="be7ee-284">*Durch das Löschen einer person*</span><span class="sxs-lookup"><span data-stu-id="be7ee-284">*Deleting a person*</span></span>
7. <span data-ttu-id="be7ee-285">Wechseln Sie zurück zu Visual Studio, und drücken Sie **UMSCHALT + F5** debugging beenden.</span><span class="sxs-lookup"><span data-stu-id="be7ee-285">Go back to Visual Studio and press **SHIFT + F5** to stop debugging.</span></span>

<a id="Exercise3"></a>
### <a name="exercise-3-creating-a-web-api-controller-using-scaffolding"></a><span data-ttu-id="be7ee-286">Übung 3: Erstellen einer Web-API-Controller, die mithilfe von Gerüstbau</span><span class="sxs-lookup"><span data-stu-id="be7ee-286">Exercise 3: Creating a Web API Controller Using Scaffolding</span></span>

<span data-ttu-id="be7ee-287">Die Web-API-Framework ist Teil des Stapels ASP.NET und entwickelt, um die implementierende HTTP-Dienste, in der Regel senden und Empfangen von Daten über eine REST-API JSON oder XML-Format zu vereinfachen.</span><span class="sxs-lookup"><span data-stu-id="be7ee-287">The Web API framework is part of the ASP.NET Stack and designed to make implementing HTTP services easier, generally sending and receiving JSON- or XML-formatted data through a RESTful API.</span></span>

<span data-ttu-id="be7ee-288">In dieser Übung werden Sie ASP.NET Gerüstbau erneut verwenden, um eine Web-API-Controller zu generieren.</span><span class="sxs-lookup"><span data-stu-id="be7ee-288">In this exercise, you will use ASP.NET Scaffolding again to generate a Web API controller.</span></span> <span data-ttu-id="be7ee-289">Verwenden Sie die gleiche **Person** und **PersonContext** Klassen aus der vorherigen Übung, um die gleiche Person Daten im JSON-Format bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="be7ee-289">You will use the same **Person** and **PersonContext** classes from the previous exercise to provide the same person data in JSON format.</span></span> <span data-ttu-id="be7ee-290">Es wird angezeigt, wie Sie die gleichen Ressourcen auf unterschiedliche Weise in der gleichen ASP.NET-Anwendung verfügbar gemacht werden können.</span><span class="sxs-lookup"><span data-stu-id="be7ee-290">You will see how you can expose the same resources in different ways within the same ASP.NET application.</span></span>

<a id="Ex3Task1"></a>
#### <a name="task-1--creating-a-web-api-controller"></a><span data-ttu-id="be7ee-291">Aufgabe 1 – erstellen eine Web-API-Controller</span><span class="sxs-lookup"><span data-stu-id="be7ee-291">Task 1 – Creating a Web API Controller</span></span>

<span data-ttu-id="be7ee-292">In dieser Aufgabe erstellen Sie ein neues **Web-API-Controller** , wird die Person-Daten in einem Computer die Verwendbarkeit Format wie JSON offenlegen.</span><span class="sxs-lookup"><span data-stu-id="be7ee-292">In this task you will create a new **Web API Controller** that will expose the person data in a machine-consumable format like JSON.</span></span>

1. <span data-ttu-id="be7ee-293">Wenn nicht bereits geöffnet haben, öffnen Sie **Visual Studio Express 2013 für Web** , und öffnen Sie die **MyHybridSite.sln** Projektmappe befindet sich in der **Quell-/Ex3-WebAPI/Anfang** Ordner.</span><span class="sxs-lookup"><span data-stu-id="be7ee-293">If not already opened, open **Visual Studio Express 2013 for Web** and open the **MyHybridSite.sln** solution located in the **Source/Ex3-WebAPI/Begin** folder.</span></span> <span data-ttu-id="be7ee-294">Alternativ können Sie die Projektmappe fortsetzen, dass Sie im vorherigen Schritt abgerufen haben.</span><span class="sxs-lookup"><span data-stu-id="be7ee-294">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>

    > [!NOTE]
    > <span data-ttu-id="be7ee-295">Wenn Sie mit der Begin-Lösung von Übung 3 starten, drücken Sie **STRG + UMSCHALT + B** auf die Projektmappe zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="be7ee-295">If you start with the Begin solution from Exercise 3, press **CTRL + SHIFT + B** to build the solution.</span></span>
2. <span data-ttu-id="be7ee-296">In **Projektmappen-Explorer**, mit der rechten Maustaste die **Controller** Ordner, der die **MyHybridSite** Projekt, und wählen Sie **hinzufügen | Neues Gerüst...** .</span><span class="sxs-lookup"><span data-stu-id="be7ee-296">In **Solution Explorer**, right-click the **Controllers** folder of the **MyHybridSite** project and select **Add | New Scaffolded Item...**.</span></span>

    ![Erstellen eines neuen scaffolded Domänencontrollers](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image23.png)

    <span data-ttu-id="be7ee-298">*Erstellen eines neuen scaffolded Domänencontrollers*</span><span class="sxs-lookup"><span data-stu-id="be7ee-298">*Creating a new scaffolded Controller*</span></span>
3. <span data-ttu-id="be7ee-299">In der **Gerüst hinzufügen** wählen Sie im Dialogfeld **Web-API** im linken Bereich, klicken Sie dann **Web API 2-Controller mit Aktionen unter Verwendung von Entity Framework** im mittleren Bereich, und klicken Sie dann auf  **Fügen Sie hinzu.**</span><span class="sxs-lookup"><span data-stu-id="be7ee-299">In the **Add Scaffold** dialog box, select **Web API** in the left pane, then **Web API 2 Controller with actions, using Entity Framework** in the middle pane and then click **Add.**</span></span>

    <span data-ttu-id="be7ee-300">![Auswählen von Web-API 2-Controller mit Aktionen und Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "auswählen von Web-API 2-Controller mit Aktionen und Entity Framework")</span><span class="sxs-lookup"><span data-stu-id="be7ee-300">![Selecting Web API 2 Controller with actions and Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "Selecting Web API 2 Controller with actions and Entity Framework")</span></span>

    <span data-ttu-id="be7ee-301">*Auswählen von Web-API 2-Controller mit Aktionen und Entity Framework*</span><span class="sxs-lookup"><span data-stu-id="be7ee-301">*Selecting Web API 2 Controller with actions and Entity Framework*</span></span>
4. <span data-ttu-id="be7ee-302">Legen Sie *ApiPersonController* als der **Controllernamen**, wählen die **asynchrone Controlleraktionen verwenden** aus, und wählen Sie **Person (MyHybridSite.Models)**  und **PersonContext (MyHybridSite.Models)** als die **Modell** und **Datenkontext** bzw. Klassen.</span><span class="sxs-lookup"><span data-stu-id="be7ee-302">Set *ApiPersonController* as the **Controller name**, select the **Use async controller actions** option and select **Person (MyHybridSite.Models)** and **PersonContext (MyHybridSite.Models)** as the **Model** and **Data context** classes respectively.</span></span> <span data-ttu-id="be7ee-303">Klicken Sie anschließend auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="be7ee-303">Then click **Add**.</span></span>

    <span data-ttu-id="be7ee-304">![Ein Web-API-Controller mit Gerüst hinzufügen](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "einen Web-API-Controller mit Gerüst hinzufügen")</span><span class="sxs-lookup"><span data-stu-id="be7ee-304">![Adding a Web API Controller with scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "Adding a Web API controller with scaffolding")</span></span>

    <span data-ttu-id="be7ee-305">*Einen Web-API-Controller mit Gerüst hinzufügen*</span><span class="sxs-lookup"><span data-stu-id="be7ee-305">*Adding a Web API controller with scaffolding*</span></span>
5. <span data-ttu-id="be7ee-306">Visual Studio generiert die **ApiPersonController** Klasse mit vier CRUD-Aktionen, um mit Ihren Daten zu arbeiten.</span><span class="sxs-lookup"><span data-stu-id="be7ee-306">Visual Studio will then generate the **ApiPersonController** class with the four CRUD actions to work with your data.</span></span>

    <span data-ttu-id="be7ee-307">![Nach dem Erstellen des Web-API-Controllers mit Gerüstbau](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "nach dem Erstellen des Web-API-Controllers mit Gerüstbau")</span><span class="sxs-lookup"><span data-stu-id="be7ee-307">![After creating the Web API controller with scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "After creating the Web API controller with scaffolding")</span></span>

    <span data-ttu-id="be7ee-308">*Nach dem Erstellen des Web-API-Controllers mit Gerüstbau*</span><span class="sxs-lookup"><span data-stu-id="be7ee-308">*After creating the Web API controller with scaffolding*</span></span>
6. <span data-ttu-id="be7ee-309">Öffnen der **ApiPersonController.cs** Datei, und überprüfen Sie die *"GetPeople"* Aktionsmethode.</span><span class="sxs-lookup"><span data-stu-id="be7ee-309">Open the **ApiPersonController.cs** file and inspect the *GetPeople* action method.</span></span> <span data-ttu-id="be7ee-310">Diese Methode fragt das DB-Feld der **PersonContext** Typ, um die Personen Daten abzurufen.</span><span class="sxs-lookup"><span data-stu-id="be7ee-310">This method queries the db field of **PersonContext** type in order to get the people data.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample5.cs)]
7. <span data-ttu-id="be7ee-311">Betrachten Sie nun den Kommentar oberhalb der Methodendefinition.</span><span class="sxs-lookup"><span data-stu-id="be7ee-311">Now notice the comment above the method definition.</span></span> <span data-ttu-id="be7ee-312">Er bietet den URI, der diese Aktion verfügbar macht, die Sie in der nächsten Aufgabe verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="be7ee-312">It provides the URI that exposes this action which you will use in the next task.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample6.cs)]

    > [!NOTE]
    > <span data-ttu-id="be7ee-313">Standardmäßig Web-API ist so konfiguriert, dass Abfragen zum Abfangen der */API* Pfad zum Vermeiden von Konflikten mit MVC-Controller.</span><span class="sxs-lookup"><span data-stu-id="be7ee-313">By default, Web API is configured to catch the queries to the */api* path to avoid collisions with MVC controllers.</span></span> <span data-ttu-id="be7ee-314">Wenn Sie diese Einstellung ändern möchten, finden Sie in [Routing in ASP.NET Web API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="be7ee-314">If you need to change this setting, refer to [Routing in ASP.NET Web API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

<a id="Ex3Task2"></a>
#### <a name="task-2--running-the-solution"></a><span data-ttu-id="be7ee-315">Aufgabe 2: Ausführen der Projektmappe</span><span class="sxs-lookup"><span data-stu-id="be7ee-315">Task 2 – Running the Solution</span></span>

<span data-ttu-id="be7ee-316">In dieser Aufgabe verwenden Sie Internet Explorer **F12 Entwicklertools** , die vollständige Antwort aus dem Web-API-Controller zu überprüfen.</span><span class="sxs-lookup"><span data-stu-id="be7ee-316">In this task you will use the Internet Explorer **F12 developer tools** to inspect the full response from the Web API controller.</span></span> <span data-ttu-id="be7ee-317">Es wird angezeigt, wie Sie den Netzwerkverkehr, um weitere Einblicke in Ihre Anwendungsdaten zu erhalten erfassen können.</span><span class="sxs-lookup"><span data-stu-id="be7ee-317">You will see how you can capture network traffic to get more insight into your application data.</span></span>

> [!NOTE]
> <span data-ttu-id="be7ee-318">Stellen Sie sicher, dass **Internet Explorer** ausgewählt ist, der **starten** Schaltfläche befindet sich auf der Symbolleiste von Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="be7ee-318">Make sure that **Internet Explorer** is selected in the **Start** button located on the Visual Studio toolbar.</span></span>
> 
> ![Internet Explorer-option](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image27.png)
> 
> <span data-ttu-id="be7ee-320">Die **F12 Entwicklertools** haben ein umfangreichen Satzes von Funktionen, die in diese praktische Übung-on-Übung nicht behandelt wird.</span><span class="sxs-lookup"><span data-stu-id="be7ee-320">The **F12 developer tools** have a wide set of functionality that is not covered in this hands-on-lab.</span></span> <span data-ttu-id="be7ee-321">Wenn Sie mehr darüber erfahren möchten, lesen Sie [mit den F12 Entwicklertools](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85)).</span><span class="sxs-lookup"><span data-stu-id="be7ee-321">If you want to learn more about it, refer to [Using the F12 developer tools](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85)).</span></span>


1. <span data-ttu-id="be7ee-322">Drücken Sie **F5** um die Projektmappe auszuführen.</span><span class="sxs-lookup"><span data-stu-id="be7ee-322">Press **F5** to run the solution.</span></span>

    > [!NOTE]
    > <span data-ttu-id="be7ee-323">Um diesen Task ordnungsgemäß ausführen zu können, muss Ihre Anwendung Daten verfügen.</span><span class="sxs-lookup"><span data-stu-id="be7ee-323">In order to follow this task correctly, your application needs to have data.</span></span> <span data-ttu-id="be7ee-324">Wenn die Datenbank leer ist, können Sie zurückkehren in Aufgabe 3 in Übung 2 und führen Sie die Schritte zum Erstellen einer neuen Person, die unter Verwendung der MVC-Ansichten.</span><span class="sxs-lookup"><span data-stu-id="be7ee-324">If your database is empty, you can go back to Task 3 in Exercise 2 and follow the steps on how to create a new person using the MVC views.</span></span>
2. <span data-ttu-id="be7ee-325">Drücken Sie im Browser **F12** So öffnen die **Entwicklertools** Bereich.</span><span class="sxs-lookup"><span data-stu-id="be7ee-325">In the browser, press **F12** to open the **Developer Tools** panel.</span></span> <span data-ttu-id="be7ee-326">Drücken Sie **STRG** + **4** oder klicken Sie auf die **Netzwerk** Symbol, und klicken Sie dann auf der grüne Pfeil-Schaltfläche, um das Erfassen des Netzwerkverkehrs zu beginnen.</span><span class="sxs-lookup"><span data-stu-id="be7ee-326">Press **CTRL** + **4** or click the **Network** icon, and then click the green arrow button to begin capturing network traffic.</span></span>

    <span data-ttu-id="be7ee-327">![Initiieren von Web-API-netzwerkerfassung](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "netzwerkerfassung initiieren, Web-API")</span><span class="sxs-lookup"><span data-stu-id="be7ee-327">![Initiating Web API network capture](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "Initiating Web API network capture")</span></span>

    <span data-ttu-id="be7ee-328">*Web-API-netzwerkerfassung initiieren*</span><span class="sxs-lookup"><span data-stu-id="be7ee-328">*Initiating Web API network capture*</span></span>
3. <span data-ttu-id="be7ee-329">Append **-api/ApiPerson** an die URL in die Adressleiste des Browsers angezeigt.</span><span class="sxs-lookup"><span data-stu-id="be7ee-329">Append **api/ApiPerson** to the URL in the browser's address bar.</span></span> <span data-ttu-id="be7ee-330">Sie werden jetzt die Details der Antwort vom Überprüfen der **ApiPersonController**.</span><span class="sxs-lookup"><span data-stu-id="be7ee-330">You will now inspect the details of the response from the **ApiPersonController**.</span></span>

    <span data-ttu-id="be7ee-331">![Abrufen von Daten der Person über Web-API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "Abrufen von Daten der Person über Web-API")</span><span class="sxs-lookup"><span data-stu-id="be7ee-331">![Retrieving person data through Web API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "Retrieving person data through Web API")</span></span>

    <span data-ttu-id="be7ee-332">*Abrufen von Daten der Person über Web-API*</span><span class="sxs-lookup"><span data-stu-id="be7ee-332">*Retrieving person data through Web API*</span></span>

    > [!NOTE]
    > <span data-ttu-id="be7ee-333">Sobald der Download abgeschlossen ist, werden Sie aufgefordert, eine Aktion mit der heruntergeladenen Datei herzustellen.</span><span class="sxs-lookup"><span data-stu-id="be7ee-333">Once the download finishes, you will be prompted to make an action with the downloaded file.</span></span> <span data-ttu-id="be7ee-334">Lassen Sie das Dialogfeld geöffnet, um des Inhalts der Antwort durch den Entwickler Toolfenster beobachten können.</span><span class="sxs-lookup"><span data-stu-id="be7ee-334">Leave the dialog box open in order to be able to watch the response content through the Developers Tool window.</span></span>
4. <span data-ttu-id="be7ee-335">Sie werden nun den Text der Antwort überprüfen.</span><span class="sxs-lookup"><span data-stu-id="be7ee-335">Now you will inspect the body of the response.</span></span> <span data-ttu-id="be7ee-336">Klicken Sie hierzu auf die **Details** Registerkarte, und klicken Sie dann auf **Antworttext**.</span><span class="sxs-lookup"><span data-stu-id="be7ee-336">To do this, click the **Details** tab and then click **Response body**.</span></span> <span data-ttu-id="be7ee-337">Sehen Sie sich, dass die heruntergeladenen Daten eine Liste von Objekten mit den Eigenschaften **Id**, **Namen** und **Alter** entsprechen den **Person** -Klasse.</span><span class="sxs-lookup"><span data-stu-id="be7ee-337">You can check that the downloaded data is a list of objects with the properties **Id**, **Name** and **Age** that correspond to the **Person** class.</span></span>

    <span data-ttu-id="be7ee-338">![Anzeigen von Web-API-Antworttext](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "Anzeigen von Web-API-Antworttext")</span><span class="sxs-lookup"><span data-stu-id="be7ee-338">![Viewing Web API Response Body](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "Viewing Web API Response Body")</span></span>

    <span data-ttu-id="be7ee-339">*Anzeigen von Web-API-Antworttext*</span><span class="sxs-lookup"><span data-stu-id="be7ee-339">*Viewing Web API Response Body*</span></span>

<a id="Ex3Task3"></a>
#### <a name="task-3--adding-web-api-help-pages"></a><span data-ttu-id="be7ee-340">Aufgabe 3: Hinzufügen von Web-API-Hilfeseiten</span><span class="sxs-lookup"><span data-stu-id="be7ee-340">Task 3 – Adding Web API Help Pages</span></span>

<span data-ttu-id="be7ee-341">Wenn Sie eine Web-API erstellen, ist es hilfreich, eine Hilfeseite erstellen, damit andere Entwickler wissen, wie Ihre API aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="be7ee-341">When you create a Web API, it is useful to create a help page so that other developers will know how to call your API.</span></span> <span data-ttu-id="be7ee-342">Sie erstellen und die Dokumentationsseiten manuell aktualisieren, aber es ist besser, automatisch, um zu vermeiden, dass Wartungsarbeiten Vorgehensweise generiert.</span><span class="sxs-lookup"><span data-stu-id="be7ee-342">You could create and update the documentation pages manually, but it is better to auto-generate them to avoid having to do maintenance work.</span></span> <span data-ttu-id="be7ee-343">In dieser Aufgabe verwenden Sie ein NuGet-Paket zum automatischen Generieren von Web-API-Hilfeseiten der Projektmappe.</span><span class="sxs-lookup"><span data-stu-id="be7ee-343">In this task you will use a Nuget package to automatically generate Web API help pages to the solution.</span></span>

1. <span data-ttu-id="be7ee-344">Aus der **Tools** in Visual Studio, wählen Sie im Menü **Bibliothekspaket-Manager**, und klicken Sie dann auf **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="be7ee-344">From the **Tools** menu in Visual Studio, select **Library Package Manager**, and then click **Package Manager Console**.</span></span>
2. <span data-ttu-id="be7ee-345">In der **Package Manager Console** Fenster, führen Sie den folgenden Befehl:</span><span class="sxs-lookup"><span data-stu-id="be7ee-345">In the **Package Manager Console** window, execute the following command:</span></span>

    [!code-powershell[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample7.ps1)]

    > [!NOTE]
    > <span data-ttu-id="be7ee-346">Die **Microsoft.AspNet.WebApi.HelpPage** Paket installiert die erforderlichen Assemblys und fügt der MVC-Ansichten für die Hilfeseiten unter der **Bereiche/HelpPage** Ordner.</span><span class="sxs-lookup"><span data-stu-id="be7ee-346">The **Microsoft.AspNet.WebApi.HelpPage** package installs the necessary assemblies and adds MVC Views for the help pages under the **Areas/HelpPage** folder.</span></span>

    <span data-ttu-id="be7ee-347">![HelpPage Bereich](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "HelpPage-Bereich")</span><span class="sxs-lookup"><span data-stu-id="be7ee-347">![HelpPage Area](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "HelpPage Area")</span></span>

    <span data-ttu-id="be7ee-348">*HelpPage Bereich*</span><span class="sxs-lookup"><span data-stu-id="be7ee-348">*HelpPage Area*</span></span>
3. <span data-ttu-id="be7ee-349">Standardmäßig wird die Hilfe über Seiten verfügen Platzhalter-Zeichenfolgen für die Dokumentation.</span><span class="sxs-lookup"><span data-stu-id="be7ee-349">By default, the help pages have placeholder strings for documentation.</span></span> <span data-ttu-id="be7ee-350">XML-Dokumentationskommentare können Sie um die Dokumentation zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="be7ee-350">You can use XML documentation comments to create the documentation.</span></span> <span data-ttu-id="be7ee-351">Um dieses Feature zu aktivieren, öffnen die **HelpPageConfig.cs** -Datei die **Bereiche/HelpPage/App\_starten** Ordner und die auskommentierung der folgenden Zeile:</span><span class="sxs-lookup"><span data-stu-id="be7ee-351">To enable this feature, open the **HelpPageConfig.cs** file located in the **Areas/HelpPage/App\_Start** folder and uncomment the following line:</span></span>

    [!code-javascript[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample8.js)]
4. <span data-ttu-id="be7ee-352">In **Projektmappen-Explorer**, mit der rechten Maustaste des Projekts **MyHybridSite**Option **Eigenschaften** , und klicken Sie auf die **erstellen** Registerkarte.</span><span class="sxs-lookup"><span data-stu-id="be7ee-352">In **Solution Explorer**, right-click the project **MyHybridSite**, select **Properties** and click the **Build** tab.</span></span>

    <span data-ttu-id="be7ee-353">![Registerkarte "erstellen"](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "Abschnitt erstellen")</span><span class="sxs-lookup"><span data-stu-id="be7ee-353">![Build tab](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "Build section")</span></span>

    <span data-ttu-id="be7ee-354">*Registerkarte "erstellen"*</span><span class="sxs-lookup"><span data-stu-id="be7ee-354">*Build tab*</span></span>
5. <span data-ttu-id="be7ee-355">Klicken Sie unter **Ausgabe**Option **XML-Dokumentationsdatei**.</span><span class="sxs-lookup"><span data-stu-id="be7ee-355">Under **Output**, select **XML documentation file**.</span></span> <span data-ttu-id="be7ee-356">Geben Sie im Bearbeitungsfeld **App\_Data/XmlDocument.xml**.</span><span class="sxs-lookup"><span data-stu-id="be7ee-356">In the edit box, type **App\_Data/XmlDocument.xml**.</span></span>

    <span data-ttu-id="be7ee-357">![Ausgabe in der Registerkarte "Build" im Abschnitt](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "Ausgabe Abschnitt auf der Registerkarte "Build"")</span><span class="sxs-lookup"><span data-stu-id="be7ee-357">![Output section in Build tab](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "Output section in build tab")</span></span>

    <span data-ttu-id="be7ee-358">*Ausgabeabschnitt auf der Registerkarte "Build"*</span><span class="sxs-lookup"><span data-stu-id="be7ee-358">*Output section in Build tab*</span></span>
6. <span data-ttu-id="be7ee-359">Drücken Sie **STRG** + **S** um die Änderungen zu speichern.</span><span class="sxs-lookup"><span data-stu-id="be7ee-359">Press **CTRL** + **S** to save the changes.</span></span>
7. <span data-ttu-id="be7ee-360">Öffnen der **ApiPersonController.cs** -Datei von der **Controller** Ordner.</span><span class="sxs-lookup"><span data-stu-id="be7ee-360">Open the **ApiPersonController.cs** file from the **Controllers** folder.</span></span>
8. <span data-ttu-id="be7ee-361">Geben Sie eine neue Zeile zwischen den *"GetPeople"* Methodensignatur und die */ / api/ApiPerson abrufen* kommentieren, und geben Sie dann drei Schrägstriche.</span><span class="sxs-lookup"><span data-stu-id="be7ee-361">Enter a new line between the *GetPeople* method signature and the *// GET api/ApiPerson* comment, and then type three forward slashes.</span></span>

    > [!NOTE]
    > <span data-ttu-id="be7ee-362">Visual Studio fügt automatisch die XML-Elemente, die die Dokumentation der Methode zu definieren.</span><span class="sxs-lookup"><span data-stu-id="be7ee-362">Visual Studio automatically inserts the XML elements which define the method documentation.</span></span>
9. <span data-ttu-id="be7ee-363">Hinzufügen einer summary-Text und der Rückgabewert für die *"GetPeople"* Methode.</span><span class="sxs-lookup"><span data-stu-id="be7ee-363">Add a summary text and the return value for the *GetPeople* method.</span></span> <span data-ttu-id="be7ee-364">Es sollte wie folgt aussehen.</span><span class="sxs-lookup"><span data-stu-id="be7ee-364">It should look like the following.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample9.cs)]
10. <span data-ttu-id="be7ee-365">Drücken Sie **F5** um die Projektmappe auszuführen.</span><span class="sxs-lookup"><span data-stu-id="be7ee-365">Press **F5** to run the solution.</span></span>
11. <span data-ttu-id="be7ee-366">Append **/help** an die URL in der Adressleiste, um die Hilfeseite zu suchen.</span><span class="sxs-lookup"><span data-stu-id="be7ee-366">Append **/help** to the URL in the address bar to browse to the help page.</span></span>

    <span data-ttu-id="be7ee-367">![ASP.NET Web-API-Hilfeseite](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "ASP.NET Web-API-Hilfeseite")</span><span class="sxs-lookup"><span data-stu-id="be7ee-367">![ASP.NET Web API Help Page](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "ASP.NET Web API Help Page")</span></span>

    <span data-ttu-id="be7ee-368">*ASP.NET Web API Help Page*</span><span class="sxs-lookup"><span data-stu-id="be7ee-368">*ASP.NET Web API Help Page*</span></span>

    > [!NOTE]
    > <span data-ttu-id="be7ee-369">Der Hauptinhalt der Seite ist eine Tabelle von APIs, die vom Netzwerkcontroller gruppiert.</span><span class="sxs-lookup"><span data-stu-id="be7ee-369">The main content of the page is a table of APIs, grouped by controller.</span></span> <span data-ttu-id="be7ee-370">Die Tabelleneinträge dynamisch generiert werden, mithilfe der **IApiExplorer** Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="be7ee-370">The table entries are generated dynamically, using the **IApiExplorer** interface.</span></span> <span data-ttu-id="be7ee-371">Wenn Sie hinzufügen oder Aktualisieren von ein API-Controller, wird die Tabelle automatisch das nächste Mal aktualisiert werden, die die Anwendung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="be7ee-371">If you add or update an API controller, the table will be automatically updated the next time you build the application.</span></span>
    > 
    > <span data-ttu-id="be7ee-372">Die **API** Spalte listet die HTTP-Methode und des relativen URI.</span><span class="sxs-lookup"><span data-stu-id="be7ee-372">The **API** column lists the HTTP method and relative URI.</span></span> <span data-ttu-id="be7ee-373">Die **Beschreibung** Spalte enthält Informationen, die aus der Dokumentation der Methode extrahiert wurde.</span><span class="sxs-lookup"><span data-stu-id="be7ee-373">The **Description** column contains information that has been extracted from the method's documentation.</span></span>
12. <span data-ttu-id="be7ee-374">Beachten Sie, dass die Beschreibung an, die Sie hinzugefügt haben, über die Methodendefinition in der Description-Spalte angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="be7ee-374">Note that the description you added above the method definition is displayed in the description column.</span></span>

    <span data-ttu-id="be7ee-375">![API-methodenbeschreibung](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "Beschreibung der API-Methode")</span><span class="sxs-lookup"><span data-stu-id="be7ee-375">![API method description](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "API method description")</span></span>

    <span data-ttu-id="be7ee-376">*Beschreibung der API-Methode*</span><span class="sxs-lookup"><span data-stu-id="be7ee-376">*API method description*</span></span>
13. <span data-ttu-id="be7ee-377">Klicken Sie auf die API-Methoden zum Navigieren zu einer Seite mit ausführlicheren Informationen, einschließlich beispielantworttexte.</span><span class="sxs-lookup"><span data-stu-id="be7ee-377">Click one of the API methods to navigate to a page with more detailed information, including sample response bodies.</span></span>

    <span data-ttu-id="be7ee-378">![Detail-Informationsseite](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "Detailinformationen zur Seite "")</span><span class="sxs-lookup"><span data-stu-id="be7ee-378">![Detail Information page](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "Detail Information page")</span></span>

    <span data-ttu-id="be7ee-379">*Seite "Informationen"*</span><span class="sxs-lookup"><span data-stu-id="be7ee-379">*Detailed information page*</span></span>

* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="be7ee-380">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="be7ee-380">Summary</span></span>

<span data-ttu-id="be7ee-381">Dieser praktischen Übungseinheit abschließen, indem Sie haben gelernt, wie Sie:</span><span class="sxs-lookup"><span data-stu-id="be7ee-381">By completing this hands-on lab you have learned how to:</span></span>

- <span data-ttu-id="be7ee-382">Erstellen einer neuen Webanwendung verwenden eine ASP.NET-Umgebung in Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="be7ee-382">Create a new Web application using the One ASP.NET Experience in Visual Studio 2013</span></span>
- <span data-ttu-id="be7ee-383">Mehrere Technologien von ASP.NET in ein einzelnes Projekt integrieren</span><span class="sxs-lookup"><span data-stu-id="be7ee-383">Integrate multiple ASP.NET technologies into one single project</span></span>
- <span data-ttu-id="be7ee-384">Generieren Sie aus Ihren Modellklassen mithilfe von Gerüstbau für ASP.NET MVC-Controller und Ansichten</span><span class="sxs-lookup"><span data-stu-id="be7ee-384">Generate MVC controllers and views from your model classes using ASP.NET Scaffolding</span></span>
- <span data-ttu-id="be7ee-385">Generieren von Web-API-Controller, die Features wie z. B. für die asynchrone Programmierung und Datenzugriff über Entity Framework verwenden</span><span class="sxs-lookup"><span data-stu-id="be7ee-385">Generate Web API controllers, which use features such as Async Programming and data access through Entity Framework</span></span>
- <span data-ttu-id="be7ee-386">Web-API-Hilfeseiten automatisch zu generieren, für die Controller</span><span class="sxs-lookup"><span data-stu-id="be7ee-386">Automatically generate Web API Help Pages for your controllers</span></span>
