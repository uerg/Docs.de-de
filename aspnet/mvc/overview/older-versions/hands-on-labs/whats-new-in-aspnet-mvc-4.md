---
uid: mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
title: Neues in ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: ASP.NET MVC 4 ist ein Framework zum Erstellen von skalierbaren, standardbasierte Windows-Webanwendungen, die unter Verwendung von bewährte Entwurfsmuster geeinigt und die Leistung von ASP.NET und...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 48f7feb3-872f-485d-b96f-e30011ff8c4a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 977a6b5a84825ebd087752dcc2ebc0c5410e1657
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="whats-new-in-aspnet-mvc-4"></a><span data-ttu-id="d4c65-103">Was ist neu in ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="d4c65-103">What's New in ASP.NET MVC 4</span></span>

<span data-ttu-id="d4c65-104">Durch [Web Lager Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="d4c65-104">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="d4c65-105">Herunterladen von Web-Lager Training Kit</span><span class="sxs-lookup"><span data-stu-id="d4c65-105">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="d4c65-106">ASP.NET MVC 4 ist ein Framework zum Erstellen von skalierbaren, standardbasierte Windows-Webanwendungen, die unter Verwendung von bewährte Entwurfsmuster geeinigt und die Leistung von ASP.NET und .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="d4c65-106">ASP.NET MVC 4 is a framework for building scalable, standards-based web applications using well-established design patterns and the power of the ASP.NET and the .NET framework.</span></span> <span data-ttu-id="d4c65-107">Diese neue, vierte Version des Frameworks konzentriert sich auf die mobile Anwendung Webentwicklung einfacher zu machen.</span><span class="sxs-lookup"><span data-stu-id="d4c65-107">This new, fourth version of the framework focuses on making mobile web application development easier.</span></span>

<span data-ttu-id="d4c65-108">Zunächst bei der Erstellung eines neuen ASP.NET MVC 4-Projekts besteht jetzt eine mobile Anwendung-Projektvorlage, die Sie verwenden können, um eine eigenständige app speziell für mobile Geräte zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="d4c65-108">To begin with, when you create a new ASP.NET MVC 4 project there is now a mobile application project template you can use to build a standalone app specifically for mobile devices.</span></span> <span data-ttu-id="d4c65-109">Darüber hinaus wird die ASP.NET MVC 4 mit jQuery Mobile über jQuery.Mobile.MVC NuGet-Paket integriert.</span><span class="sxs-lookup"><span data-stu-id="d4c65-109">Additionally, ASP.NET MVC 4 integrates with jQuery Mobile through a jQuery.Mobile.MVC NuGet package.</span></span> <span data-ttu-id="d4c65-110">jQuery Mobile ist ein HTML5-basierte Framework für die Entwicklung von webapps, die mit allen gängigen mobilen Gerät-Plattformen, einschließlich Windows Phone, iPhone, Android usw. kompatibel sind.</span><span class="sxs-lookup"><span data-stu-id="d4c65-110">jQuery Mobile is an HTML5-based framework for developing web apps that are compatible with all popular mobile device platforms, including Windows Phone, iPhone, Android and so on.</span></span> <span data-ttu-id="d4c65-111">Wenn die gewünschte Spezialisierung ermöglicht Sie verschiedene Ansichten für verschiedene Geräte dienen und gerätespezifische Optimierungen allerdings auch ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="d4c65-111">However, if you need specialization, ASP.NET MVC 4 also enables you to serve different views for different devices and provide device-specific optimizations.</span></span>

<span data-ttu-id="d4c65-112">In dieser praktischen Übungseinheit, beginnen Sie mit der ASP.NET MVC 4 &quot;Internetanwendung&quot; Projektvorlage für eine Fotogalerie-Anwendung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="d4c65-112">In this hands-on lab, you will start with the ASP.NET MVC 4 &quot;Internet Application&quot; project template to create a Photo Gallery application.</span></span> <span data-ttu-id="d4c65-113">Sie werden schrittweise erhöhen, die app mithilfe von jQuery Mobile und ASP.NET MVC 4 neuen Funktionen, die um sie mit anderen mobilen Geräten und desktop Webbrowsern kompatibel zu machen.</span><span class="sxs-lookup"><span data-stu-id="d4c65-113">You will progressively enhance the app using jQuery Mobile and ASP.NET MVC 4's new features to make it compatible with different mobile devices and desktop web browsers.</span></span> <span data-ttu-id="d4c65-114">Außerdem lernen Sie neuen Code Know-how für die codegenerierung und wie ASP.NET MVC 4 asynchrone Aktionsmethoden schreiben, durch die Unterstützung einer Aufgabe erleichtert&lt;ActionResult&gt; Rückgabetypen.</span><span class="sxs-lookup"><span data-stu-id="d4c65-114">You will also learn about new code recipes for code generation and how ASP.NET MVC 4 makes it easier for you to write asynchronous action methods by supporting Task&lt;ActionResult&gt; return types.</span></span>

> [!NOTE]
> <span data-ttu-id="d4c65-115">Alle Beispielcode und Codeausschnitte sind im Web Lager Training Kit unter enthalten [Versionen von Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="d4c65-115">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="d4c65-116">Das Projekt, das speziell für diese Übung finden Sie unter [Neuigkeiten in Web Forms in ASP.NET 4.5](https://github.com/Microsoft-Web/HOL-ASPNETWebForms).</span><span class="sxs-lookup"><span data-stu-id="d4c65-116">The project specific to this lab is available at [What's New in Web Forms in ASP.NET 4.5](https://github.com/Microsoft-Web/HOL-ASPNETWebForms).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="d4c65-117">Ziele</span><span class="sxs-lookup"><span data-stu-id="d4c65-117">Objectives</span></span>

<span data-ttu-id="d4c65-118">In dieser praktischen Übungseinheit erfahren Sie, wie Sie:</span><span class="sxs-lookup"><span data-stu-id="d4c65-118">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="d4c65-119">Nutzen Sie die Erweiterungen der für die ASP.NET MVC-Projekt Vorlagen, einschließlich der neue Projektvorlage für die mobile Anwendung</span><span class="sxs-lookup"><span data-stu-id="d4c65-119">Take advantage of the enhancements to the ASP.NET MVC project templates-including the new mobile application project template</span></span>
- <span data-ttu-id="d4c65-120">Verwenden Sie das HTML5 Viewport-Attribut und der CSS-Medienabfragen, um die Anzeige auf mobilen Geräten zu verbessern</span><span class="sxs-lookup"><span data-stu-id="d4c65-120">Use the HTML5 viewport attribute and CSS media queries to improve the display on mobile devices</span></span>
- <span data-ttu-id="d4c65-121">Verwenden Sie für progressiven Verbesserungen und zum Erstellen von Web-Touch-optimierte Benutzeroberfläche jQuery Mobile</span><span class="sxs-lookup"><span data-stu-id="d4c65-121">Use jQuery Mobile for progressive enhancements and for building touch-optimized web UI</span></span>
- <span data-ttu-id="d4c65-122">Mobile-spezifischen Ansichten erstellen</span><span class="sxs-lookup"><span data-stu-id="d4c65-122">Create mobile-specific views</span></span>
- <span data-ttu-id="d4c65-123">Umschalten zwischen mobilen und desktop-Ansichten in der Anwendung mithilfe der ansichtumschaltung Komponente</span><span class="sxs-lookup"><span data-stu-id="d4c65-123">Use the view-switcher component to toggle between mobile and desktop views in the application</span></span>
- <span data-ttu-id="d4c65-124">Erstellen von asynchronen Controller Dank Unterstützung der Aufgabe</span><span class="sxs-lookup"><span data-stu-id="d4c65-124">Create asynchronous controllers using task support</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="d4c65-125">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="d4c65-125">Prerequisites</span></span>

<span data-ttu-id="d4c65-126">Sie benötigen zum Abschließen dieser testumgebung die folgenden Elemente:</span><span class="sxs-lookup"><span data-stu-id="d4c65-126">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="d4c65-127">[Microsoft Visual Studio Express 2012 für das Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) oder sogar eine höhere (Lesen [Anhang B](#AppendixB) Anleitungen zur Installation).</span><span class="sxs-lookup"><span data-stu-id="d4c65-127">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix B](#AppendixB) for instructions on how to install it).</span></span>
- <span data-ttu-id="d4c65-128">[ASP.NET MVC 4](../../../mvc4.md) (enthalten in der Microsoft Visual Studio 2012-Installation)</span><span class="sxs-lookup"><span data-stu-id="d4c65-128">[ASP.NET MVC 4](../../../mvc4.md) (included in the Microsoft Visual Studio 2012 installation)</span></span>
- <span data-ttu-id="d4c65-129">Windows Phone-Emulator (enthalten der [Windows Phone 7.1.1 SDK](https://www.microsoft.com/download/details.aspx?id=29233))</span><span class="sxs-lookup"><span data-stu-id="d4c65-129">Windows Phone Emulator (included in the [Windows Phone 7.1.1 SDK](https://www.microsoft.com/download/details.aspx?id=29233))</span></span>
- <span data-ttu-id="d4c65-130">Optional – [WebMatrix 2](https://www.microsoft.com/web/webmatrix/) mit **Electric Plum iPhone-Simulator** -Erweiterung (nur für Übung 3 verwendet, um die Webanwendung mit einem iPhone-Simulator durchsuchen)</span><span class="sxs-lookup"><span data-stu-id="d4c65-130">Optional - [WebMatrix 2](https://www.microsoft.com/web/webmatrix/) with **Electric Plum iPhone Simulator** extension (only for Exercise 3 used to browse the web application with an iPhone simulator)</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="d4c65-131">Setup</span><span class="sxs-lookup"><span data-stu-id="d4c65-131">Setup</span></span>

<span data-ttu-id="d4c65-132">In der Lab-Dokument werden Sie aufgefordert, Codeblöcke einfügen.</span><span class="sxs-lookup"><span data-stu-id="d4c65-132">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="d4c65-133">Der Einfachheit halber die meisten dieser Code dient als Visual Studio-Codeausschnitte, die Sie von innerhalb von Visual Studio verwenden können, um zu vermeiden, müssen sie manuell hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="d4c65-133">For your convenience, most of that code is provided as Visual Studio Code Snippets, which you can use from within Visual Studio to avoid having to add it manually.</span></span>

<span data-ttu-id="d4c65-134">So installieren Sie die Codeausschnitte</span><span class="sxs-lookup"><span data-stu-id="d4c65-134">To install the code snippets:</span></span>

1. <span data-ttu-id="d4c65-135">Öffnen Sie ein Windows-Explorer-Fenster, und navigieren Sie in des Labors **Source\Setup** Ordner.</span><span class="sxs-lookup"><span data-stu-id="d4c65-135">Open a Windows Explorer window and browse to the lab's **Source\Setup** folder.</span></span>
2. <span data-ttu-id="d4c65-136">Doppelklicken Sie auf die **"Setup.cmd"** Datei in diesem Ordner zum Installieren von Visual Studio-Codeausschnitte.</span><span class="sxs-lookup"><span data-stu-id="d4c65-136">Double-click the **Setup.cmd** file in this folder to install the Visual Studio code snippets.</span></span>

<span data-ttu-id="d4c65-137">Wenn Sie nicht mit den Visual Studio-Codeausschnitte und zu erfahren, wie deren Verwendung vertraut sind, Sie können finden Sie im Anhang in diesem Dokument &quot; [Anhang A: Verwenden von Code Snippets](#AppendixA)&quot;.</span><span class="sxs-lookup"><span data-stu-id="d4c65-137">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix A: Using Code Snippets](#AppendixA)&quot;.</span></span>

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="d4c65-138">Übungen</span><span class="sxs-lookup"><span data-stu-id="d4c65-138">Exercises</span></span>

<span data-ttu-id="d4c65-139">Diese praktische Übungseinheit enthält die folgenden Übungen durcharbeiten:</span><span class="sxs-lookup"><span data-stu-id="d4c65-139">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="d4c65-140">Neue ASP.NET MVC 4-Projektvorlagen</span><span class="sxs-lookup"><span data-stu-id="d4c65-140">New ASP.NET MVC 4 Project Templates</span></span>](#Exercise1)
2. [<span data-ttu-id="d4c65-141">Erstellen der Fotogalerie Web-Anwendung</span><span class="sxs-lookup"><span data-stu-id="d4c65-141">Creating the Photo Gallery Web Application</span></span>](#Exercise2)
3. [<span data-ttu-id="d4c65-142">Hinzufügen von Unterstützung für Mobile Geräte</span><span class="sxs-lookup"><span data-stu-id="d4c65-142">Adding Support for Mobile Devices</span></span>](#Exercise3)
4. [<span data-ttu-id="d4c65-143">Verwenden asynchrone Controller</span><span class="sxs-lookup"><span data-stu-id="d4c65-143">Using Asynchronous Controllers</span></span>](#Exercise4)

> [!NOTE]
> <span data-ttu-id="d4c65-144">Jede Übung beiliegen ein **End** Ordner, der sich so ergebende Lösung, die Sie nach Abschluss der Übung abrufen soll.</span><span class="sxs-lookup"><span data-stu-id="d4c65-144">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="d4c65-145">Sie können diese Lösung als Leitfaden verwenden, ggf. zusätzliche Hilfe bei der Übungen durcharbeiten.</span><span class="sxs-lookup"><span data-stu-id="d4c65-145">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="d4c65-146">Geschätzte Zeit zum Abschließen dieser testumgebung: **60 Minuten**.</span><span class="sxs-lookup"><span data-stu-id="d4c65-146">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_New_ASPNET_MVC_4_Project_Templates"></a>
### <a name="exercise-1-new-aspnet-mvc-4-project-templates"></a><span data-ttu-id="d4c65-147">Übung 1: Neues ASP.NET MVC 4-Projektvorlagen</span><span class="sxs-lookup"><span data-stu-id="d4c65-147">Exercise 1: New ASP.NET MVC 4 Project Templates</span></span>

<span data-ttu-id="d4c65-148">In dieser Übung untersuchen Sie die Erweiterungen in ASP.NET MVC 4-Projektvorlagen.</span><span class="sxs-lookup"><span data-stu-id="d4c65-148">In this exercise, you will explore the enhancements in the ASP.NET MVC 4 Project templates.</span></span> <span data-ttu-id="d4c65-149">Zusätzlich zu den Internet-Anwendungsvorlage enthält in MVC 3 bereits vorhanden., diese Version nun eine separate Vorlage für Mobile Anwendungen.</span><span class="sxs-lookup"><span data-stu-id="d4c65-149">In addition to the Internet Application template, already present in MVC 3, this version now includes a separate template for Mobile applications.</span></span> <span data-ttu-id="d4c65-150">Zuerst sehen Sie sich einige relevanten Funktionen der einzelnen Vorlagen.</span><span class="sxs-lookup"><span data-stu-id="d4c65-150">First, you will look at some relevant features of each of the templates.</span></span> <span data-ttu-id="d4c65-151">Klicken Sie dann, arbeiten Sie auf das Rendern der Seite ordnungsgemäß auf die verschiedenen Plattformen mithilfe des richtigen Ansatzes.</span><span class="sxs-lookup"><span data-stu-id="d4c65-151">Then, you will work on rendering your page properly on the different platforms by using the right approach.</span></span>

<a id="Task_1_-_Exploring_the_Internet_Application_Template"></a>
#### <a name="task-1---exploring-the-internet-application-template"></a><span data-ttu-id="d4c65-152">Aufgabe 1: Untersuchen der Internet Application-Vorlage</span><span class="sxs-lookup"><span data-stu-id="d4c65-152">Task 1 - Exploring the Internet Application Template</span></span>

1. <span data-ttu-id="d4c65-153">Open **Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="d4c65-153">Open **Visual Studio**.</span></span>
2. <span data-ttu-id="d4c65-154">Wählen Sie die **Datei | Neue | Projekt** Menübefehl.</span><span class="sxs-lookup"><span data-stu-id="d4c65-154">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="d4c65-155">In der **neues Projekt** wählen Sie im Dialogfeld die **Visual C# | Web** Vorlage auf der linken Struktur, und wählen Sie **ASP.NET MVC 4-Webanwendung.**</span><span class="sxs-lookup"><span data-stu-id="d4c65-155">In the **New Project** dialog, select the **Visual C# | Web** template on the left pane tree, and choose **ASP.NET MVC 4 Web Application.**</span></span> <span data-ttu-id="d4c65-156">Nennen Sie das Projekt **PhotoGallery**, wählen Sie einen Speicherort (oder übernehmen Sie den Standardwert), und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="d4c65-156">Name the project **PhotoGallery**, select a location (or leave the default) and click **OK**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d4c65-157">Später passen Sie PhotoGallery ASP.NET MVC 4-Lösung, die Sie erstellen.</span><span class="sxs-lookup"><span data-stu-id="d4c65-157">You will later customize the PhotoGallery ASP.NET MVC 4 solution you are now creating.</span></span>

    <span data-ttu-id="d4c65-158">![Erstellen eines neuen Projekts](whats-new-in-aspnet-mvc-4/_static/image1.png "Erstellen eines neuen Projekts")</span><span class="sxs-lookup"><span data-stu-id="d4c65-158">![Creating a new project](whats-new-in-aspnet-mvc-4/_static/image1.png "Creating a new project")</span></span>

    <span data-ttu-id="d4c65-159">*Erstellen eines neuen Projekts*</span><span class="sxs-lookup"><span data-stu-id="d4c65-159">*Creating a new project*</span></span>
3. <span data-ttu-id="d4c65-160">In der **neues ASP.NET MVC 4-Projekt** wählen Sie im Dialogfeld die **Internetanwendung** -Projektvorlage, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="d4c65-160">In the **New ASP.NET MVC 4 Project** dialog, select the **Internet Application** project template and click **OK**.</span></span> <span data-ttu-id="d4c65-161">Stellen Sie sicher, dass Sie Razor als Ansichtsmodul ausgewählt haben.</span><span class="sxs-lookup"><span data-stu-id="d4c65-161">Make sure you have selected Razor as the view engine.</span></span>

    <span data-ttu-id="d4c65-162">![Erstellen einer neuen ASP.NET MVC 4 Internetanwendung](whats-new-in-aspnet-mvc-4/_static/image2.png "Erstellen einer neuen ASP.NET MVC 4 Internet-Anwendung")</span><span class="sxs-lookup"><span data-stu-id="d4c65-162">![Creating a new ASP.NET MVC 4 Internet Application](whats-new-in-aspnet-mvc-4/_static/image2.png "Creating a new ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="d4c65-163">*Erstellen einer neuen ASP.NET MVC 4 Internet-Anwendung*</span><span class="sxs-lookup"><span data-stu-id="d4c65-163">*Creating a new ASP.NET MVC 4 Internet Application*</span></span>

    > [!NOTE]
    > <span data-ttu-id="d4c65-164">Razor-Syntax wurde in ASP.NET MVC 3 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="d4c65-164">Razor syntax has been introduced in ASP.NET MVC 3.</span></span> <span data-ttu-id="d4c65-165">Das Ziel besteht darin, die Anzahl von Zeichen und Tastatureingaben in einer Datei erforderlich, eine schnelle und Fluid Codierung Workflow aktivieren zu minimieren.</span><span class="sxs-lookup"><span data-stu-id="d4c65-165">Its goal is to minimize the number of characters and keystrokes required in a file, enabling a fast and fluid coding workflow.</span></span> <span data-ttu-id="d4c65-166">Razor nutzt vorhandenen c# / VB (oder anderen) Sprachkenntnisse und übermittelt Sie eine Vorlage Markup-Syntax, die einen awesome HTML-Konstruktion-Workflow ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="d4c65-166">Razor leverages existing C# / VB (or other) language skills and delivers a template markup syntax that enables an awesome HTML construction workflow.</span></span>
4. <span data-ttu-id="d4c65-167">Drücken Sie **F5** um die erneuerten Vorlagen anzuzeigen und die Projektmappe auszuführen.</span><span class="sxs-lookup"><span data-stu-id="d4c65-167">Press **F5** to run the solution and see the renewed templates.</span></span> <span data-ttu-id="d4c65-168">Sie können die folgenden Funktionen Auschecken:</span><span class="sxs-lookup"><span data-stu-id="d4c65-168">You can check out the following features:</span></span>

    <span data-ttu-id="d4c65-169">**Modernes Vorlagen**</span><span class="sxs-lookup"><span data-stu-id="d4c65-169">**Modern-style templates**</span></span>

    <span data-ttu-id="d4c65-170">Die Vorlagen haben erneuert wurde, mehr Modern aussehende Arten bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="d4c65-170">The templates have been renewed, providing more modern-looking styles.</span></span>

    <span data-ttu-id="d4c65-171">![Vorlagen für ASP.NET MVC 4 restyled](whats-new-in-aspnet-mvc-4/_static/image3.png "MVC 4 restyled Vorlagen")</span><span class="sxs-lookup"><span data-stu-id="d4c65-171">![ASP.NET MVC 4 restyled templates](whats-new-in-aspnet-mvc-4/_static/image3.png "MVC 4 restyled templates")</span></span>

    <span data-ttu-id="d4c65-172">*ASP.NET MVC 4 restyled Vorlagen*</span><span class="sxs-lookup"><span data-stu-id="d4c65-172">*ASP.NET MVC 4 restyled templates*</span></span>

    <span data-ttu-id="d4c65-173">![Seite "Neues wenden Sie sich an"](whats-new-in-aspnet-mvc-4/_static/image4.png "Seite Neuer Kontakt")</span><span class="sxs-lookup"><span data-stu-id="d4c65-173">![New Contact page](whats-new-in-aspnet-mvc-4/_static/image4.png "New Contact page")</span></span>

    <span data-ttu-id="d4c65-174">*Neuer Kontakt (Seite)*</span><span class="sxs-lookup"><span data-stu-id="d4c65-174">*New Contact page*</span></span>

    <span data-ttu-id="d4c65-175">**Adaptives Rendering**</span><span class="sxs-lookup"><span data-stu-id="d4c65-175">**Adaptive Rendering**</span></span>

    <span data-ttu-id="d4c65-176">Sehen Sie sich die Größe des Browserfensters, und beachten Sie, wie das Seitenlayout dynamisch an die Größe des neuen Fensters anpasst.</span><span class="sxs-lookup"><span data-stu-id="d4c65-176">Check out resizing the browser window and notice how the page layout dynamically adapts to the new window size.</span></span> <span data-ttu-id="d4c65-177">Diese Vorlagen mithilfe der adaptiven Renderingtechnik um ordnungsgemäß auf Desktop- und mobile Plattformen ohne Anpassung zu rendern.</span><span class="sxs-lookup"><span data-stu-id="d4c65-177">These templates use the adaptive rendering technique to render properly in both desktop and mobile platforms without any customization.</span></span>

    <span data-ttu-id="d4c65-178">![ASP.NET MVC 4-Projektvorlage in anderen Browser Größen](whats-new-in-aspnet-mvc-4/_static/image5.png "ASP.NET MVC 4-Projektvorlage in anderen Browser Größen")</span><span class="sxs-lookup"><span data-stu-id="d4c65-178">![ASP.NET MVC 4 project template in different browser sizes](whats-new-in-aspnet-mvc-4/_static/image5.png "ASP.NET MVC 4 project template in different browser sizes")</span></span>

    <span data-ttu-id="d4c65-179">*ASP.NET MVC 4-Projektvorlage in anderen Browser Größen*</span><span class="sxs-lookup"><span data-stu-id="d4c65-179">*ASP.NET MVC 4 project template in different browser sizes*</span></span>

    <span data-ttu-id="d4c65-180">**Umfangreichere-Benutzeroberfläche in JavaScript**</span><span class="sxs-lookup"><span data-stu-id="d4c65-180">**Richer UI with JavaScript**</span></span>

    <span data-ttu-id="d4c65-181">Eine andere Erweiterung aus Standardprojektvorlagen ist die Verwendung von JavaScript, überwiegend interaktive JavaScript bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="d4c65-181">Another enhancement to default project templates is the use of JavaScript to provide a more interactive JavaScript.</span></span> <span data-ttu-id="d4c65-182">Die Links anmelden und registrieren, die in der Vorlage benutzten vereinigen, wie mithilfe der jQuery-Überprüfungen Eingabefelder aus einer clientseitigen überprüfen.</span><span class="sxs-lookup"><span data-stu-id="d4c65-182">The Login and Register links used in the template exemplify how to use the jQuery Validations to validate the input fields from client-side.</span></span>

    ![jQuery-Validierung](whats-new-in-aspnet-mvc-4/_static/image6.png)

    <span data-ttu-id="d4c65-184">*jQuery-Validierung*</span><span class="sxs-lookup"><span data-stu-id="d4c65-184">*jQuery Validation*</span></span>

    > [!NOTE]
    > <span data-ttu-id="d4c65-185">Beachten Sie, dass die beiden Abschnitte, die im ersten Abschnitt Sie melden Sie sich in kann melden Sie sich mit einem Konto registriert, von der Website und im zweiten Abschnitt, den Sie Altenativelly melden Sie sich mit einem anderen Authentifizierungsdienst wie Google (standardmäßig deaktiviert können).</span><span class="sxs-lookup"><span data-stu-id="d4c65-185">Notice the two log in sections, in the first section you can log in using a registerd account from the site and in the second section you can altenativelly log in using another authentication service like google (disabled by default).</span></span>
5. <span data-ttu-id="d4c65-186">Schließen Sie den Browser, um den Debugger beenden und zurück zu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d4c65-186">Close the browser to stop the debugger and return to Visual Studio.</span></span>
6. <span data-ttu-id="d4c65-187">Öffnen Sie die Datei **AuthConfig.cs** unterhalb der **App\_starten** Ordner.</span><span class="sxs-lookup"><span data-stu-id="d4c65-187">Open the file **AuthConfig.cs** located under the **App\_Start** folder.</span></span>
7. <span data-ttu-id="d4c65-188">Entfernen Sie die Kommentarzeichen aus der letzten Zeile zum Registrieren von Google-Client für *OAuth* Authentifizierung.</span><span class="sxs-lookup"><span data-stu-id="d4c65-188">Remove the comment from the last line to register Google client for *OAuth* authentication.</span></span>


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample1.cs)]

> [!NOTE]
> Notice you can easily enable authentication using any OpenID or OAuth service like Facebook, Twitter, Microsoft, etc.
~~~
8. <span data-ttu-id="d4c65-189">Drücken Sie **F5** führen Sie die Projektmappe, und navigieren zur Anmeldeseite.</span><span class="sxs-lookup"><span data-stu-id="d4c65-189">Press **F5** to run the solution and navigate to the login page.</span></span>
9. <span data-ttu-id="d4c65-190">Wählen Sie **Google** Dienst zum Anmelden.</span><span class="sxs-lookup"><span data-stu-id="d4c65-190">Select **Google** service to log in.</span></span>

    ![Das Protokoll auswählen im Dienst](whats-new-in-aspnet-mvc-4/_static/image7.png)

    <span data-ttu-id="d4c65-192">*Das Protokoll auswählen im Dienst*</span><span class="sxs-lookup"><span data-stu-id="d4c65-192">*Selecting the log in service*</span></span>
10. <span data-ttu-id="d4c65-193">Melden Sie sich mit Ihrem Google-Konto.</span><span class="sxs-lookup"><span data-stu-id="d4c65-193">Log in using your Google account.</span></span>
11. <span data-ttu-id="d4c65-194">Zulassen Sie den Standort ("localhost") zum Abrufen von Informationen von Google-Konto</span><span class="sxs-lookup"><span data-stu-id="d4c65-194">Allow the site (localhost) to retrieve information from Google account.</span></span>
12. <span data-ttu-id="d4c65-195">Schließlich müssen Sie am Standort der Google-Konto zuordnen zu registrieren.</span><span class="sxs-lookup"><span data-stu-id="d4c65-195">Finally, you will have to register in the site to associate the Google account.</span></span>

   ![Ordnen Sie Ihre Google-Konto](whats-new-in-aspnet-mvc-4/_static/image8.png)

   <span data-ttu-id="d4c65-197">*Zuordnen von Google-Konto*</span><span class="sxs-lookup"><span data-stu-id="d4c65-197">*Associating your Google account*</span></span>
13. <span data-ttu-id="d4c65-198">Schließen Sie den Browser, um den Debugger beenden und zurück zu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d4c65-198">Close the browser to stop the debugger and return to Visual Studio.</span></span>
14. <span data-ttu-id="d4c65-199">Untersuchen Sie nun die Projektmappe Auschecken einige andere neuen Funktionen, die in der Projektvorlage von ASP.NET MVC 4 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="d4c65-199">Now explore the solution to check out some other new features introduced by ASP.NET MVC 4 in the project template.</span></span>

   <span data-ttu-id="d4c65-200">![Die Projektvorlage für ASP.NET MVC 4 Internet Anwendung](whats-new-in-aspnet-mvc-4/_static/image9.png "der Projektvorlage Internet ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="d4c65-200">![The ASP.NET MVC 4 Internet Application Project Template](whats-new-in-aspnet-mvc-4/_static/image9.png "The ASP.NET MVC 4 Internet Application Project Template")</span></span>

   <span data-ttu-id="d4c65-201">*Die ASP.NET MVC 4 Internet Application-Projektvorlage*</span><span class="sxs-lookup"><span data-stu-id="d4c65-201">*The ASP.NET MVC 4 Internet Application Project Template*</span></span>

   - <span data-ttu-id="d4c65-202">**HTML 5 Markup**</span><span class="sxs-lookup"><span data-stu-id="d4c65-202">**HTML 5 Markup**</span></span>

       <span data-ttu-id="d4c65-203">Durchsuchen Sie die Ansichten der Vorlagen so ermitteln Sie das neue Design Markup.</span><span class="sxs-lookup"><span data-stu-id="d4c65-203">Browse template views to find out the new theme markup.</span></span>

       <span data-ttu-id="d4c65-204">![Neue Vorlage für die Verwendung von Razor und HTML5-Markups About.cshtml. ] (whats-new-in-aspnet-mvc-4/_static/image10.png "Neue Vorlage für die Verwendung von Razor und HTML5-Markups About.cshtml.")</span><span class="sxs-lookup"><span data-stu-id="d4c65-204">![New template, using Razor and HTML5 markup About.cshtml.](whats-new-in-aspnet-mvc-4/_static/image10.png "New template, using Razor and HTML5 markup About.cshtml.")</span></span>

       <span data-ttu-id="d4c65-205">*Neue Vorlage für die Verwendung von Razor und HTML5-Markups (About.cshtml).*</span><span class="sxs-lookup"><span data-stu-id="d4c65-205">*New template, using Razor and HTML5 markup (About.cshtml).*</span></span>
   - <span data-ttu-id="d4c65-206">**Aktualisierte JavaScript-Bibliotheken**</span><span class="sxs-lookup"><span data-stu-id="d4c65-206">**Updated JavaScript libraries**</span></span>

       <span data-ttu-id="d4c65-207">Die ASP.NET MVC 4-Standardvorlage enthält nun KnockoutJS, ein MVVM JavaScript-Framework, das rich erstellen kann und extrem reaktionsschnellen Webanwendungen, die mit JavaScript und HTML.</span><span class="sxs-lookup"><span data-stu-id="d4c65-207">The ASP.NET MVC 4 default template now includes KnockoutJS, a JavaScript MVVM framework that lets you create rich and highly responsive web applications using JavaScript and HTML.</span></span> <span data-ttu-id="d4c65-208">Wie werden in MVC3, jQuery und jQuery UI-Bibliotheken ebenfalls in ASP.NET MVC 4 enthalten.</span><span class="sxs-lookup"><span data-stu-id="d4c65-208">Like in MVC3, jQuery and jQuery UI libraries are also included in ASP.NET MVC 4.</span></span>

     > [!NOTE]
     > <span data-ttu-id="d4c65-209">Sie erhalten weitere Informationen zur KnockOutJS Library unter diesem Link: [ [ http://learn.knockoutjs.com/ ](http://learn.knockoutjs.com/) ](http://learn.knockoutjs.com/).</span><span class="sxs-lookup"><span data-stu-id="d4c65-209">You can get more information about KnockOutJS library in this link: [[http://learn.knockoutjs.com/](http://learn.knockoutjs.com/)](http://learn.knockoutjs.com/).</span></span> <span data-ttu-id="d4c65-210">Darüber hinaus erfahren Sie über jQuery und jQuery UI in [ [ http://docs.jquery.com/ ](http://docs.jquery.com/) ](http://docs.jquery.com/).</span><span class="sxs-lookup"><span data-stu-id="d4c65-210">Additionally, you can learn about jQuery and jQuery UI in [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span></span>

<a id="Task_2_-_Exploring_the_Mobile_Application_Template"></a>
#### <a name="task-2---exploring-the-mobile-application-template"></a><span data-ttu-id="d4c65-211">Aufgabe 2: untersuchen die Vorlage für die Mobile Anwendung</span><span class="sxs-lookup"><span data-stu-id="d4c65-211">Task 2 - Exploring the Mobile Application Template</span></span>

<span data-ttu-id="d4c65-212">ASP.NET MVC 4 erleichtert die Entwicklung von Websites für mobile Geräte und Tablet-Browser.</span><span class="sxs-lookup"><span data-stu-id="d4c65-212">ASP.NET MVC 4 facilitates the development of websites for mobile and tablet browsers.</span></span> <span data-ttu-id="d4c65-213">Diese Vorlage hat die gleiche Anwendungsstruktur wie die Internet Application-Vorlage (Beachten Sie, dass der controllercode praktisch identisch ist), aber der Stil wurde geändert, um auf mobilen Geräten Touch-basierte ordnungsgemäß gerendert.</span><span class="sxs-lookup"><span data-stu-id="d4c65-213">This template has the same application structure as the Internet Application template (notice that the controller code is practically identical), but its style was modified to render properly in touch-based mobile devices.</span></span>

1. <span data-ttu-id="d4c65-214">Wählen Sie die **Datei | Neue | Projekt** Menübefehl.</span><span class="sxs-lookup"><span data-stu-id="d4c65-214">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="d4c65-215">In der **neues Projekt** wählen Sie im Dialogfeld die **Visual C# | Web** Vorlage auf der linken Struktur, und wählen Sie die **ASP.NET MVC 4-Webanwendung.**</span><span class="sxs-lookup"><span data-stu-id="d4c65-215">In the **New Project** dialog, select the **Visual C# | Web** template on the left pane tree, and choose the **ASP.NET MVC 4 Web Application.**</span></span> <span data-ttu-id="d4c65-216">Nennen Sie das Projekt **PhotoGallery.Mobile**, wählen Sie einen Speicherort (oder übernehmen Sie den Standardwert), wählen Sie &quot;zu Projektmappe hinzufügen&quot; , und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="d4c65-216">Name the project **PhotoGallery.Mobile**, select a location (or leave the default), select &quot;Add to solution&quot; and click **OK**.</span></span>
2. <span data-ttu-id="d4c65-217">In der **neues ASP.NET MVC 4-Projekt** wählen Sie im Dialogfeld die **Mobile-Anwendung** -Projektvorlage, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="d4c65-217">In the **New ASP.NET MVC 4 Project** dialog, select the **Mobile Application** project template and click **OK**.</span></span> <span data-ttu-id="d4c65-218">Stellen Sie sicher, dass Sie Razor als Ansichtsmodul ausgewählt haben.</span><span class="sxs-lookup"><span data-stu-id="d4c65-218">Make sure you have selected Razor as the view engine.</span></span>

    <span data-ttu-id="d4c65-219">![Erstellen einer neuen ASP.NET MVC 4 Mobile Anwendung](whats-new-in-aspnet-mvc-4/_static/image11.png "Erstellen einer neuen ASP.NET MVC 4 Mobile Anwendung")</span><span class="sxs-lookup"><span data-stu-id="d4c65-219">![Creating a new ASP.NET MVC 4 Mobile Application](whats-new-in-aspnet-mvc-4/_static/image11.png "Creating a new ASP.NET MVC 4 Mobile Application")</span></span>

    <span data-ttu-id="d4c65-220">*Erstellen einer neuen ASP.NET MVC 4 Mobile Anwendung*</span><span class="sxs-lookup"><span data-stu-id="d4c65-220">*Creating a new ASP.NET MVC 4 Mobile Application*</span></span>
3. <span data-ttu-id="d4c65-221">Jetzt sind Sie in der Lage, untersuchen die Projektmappe, und sehen Sie sich einige der neuen Funktionen von der ASP.NET MVC 4-Projektmappenvorlage für mobile Geräte:</span><span class="sxs-lookup"><span data-stu-id="d4c65-221">Now you are able to explore the solution and check out some of the new features introduced by the ASP.NET MVC 4 solution template for mobile:</span></span>

    - <span data-ttu-id="d4c65-222">**jQuery Mobile-Bibliothek**</span><span class="sxs-lookup"><span data-stu-id="d4c65-222">**jQuery Mobile Library**</span></span>

        <span data-ttu-id="d4c65-223">Die Mobile Anwendung-Projektvorlage enthält die mobilen jQuery-Bibliothek, also eine open Source-Bibliothek für mobile Browserkompatibilität.</span><span class="sxs-lookup"><span data-stu-id="d4c65-223">The Mobile Application project template includes the jQuery Mobile library, which is an open source library for mobile browser compatibility.</span></span> <span data-ttu-id="d4c65-224">jQuery Mobile gilt schrittweise zunehmenden Verbesserungen für mobile Browser, CSS und JavaScript zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="d4c65-224">jQuery Mobile applies progressive enhancement to mobile browsers that support CSS and JavaScript.</span></span> <span data-ttu-id="d4c65-225">Schrittweise zunehmenden Verbesserungen ermöglicht allen Browsern, die grundlegenden Inhalte einer Webseite anzuzeigen, während er nur die leistungsstärksten Browser zum Anzeigen von umfangreichen Inhalten ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="d4c65-225">Progressive enhancement enables all browsers to display the basic content of a web page, while it only enables the most powerful browsers to display the rich content.</span></span> <span data-ttu-id="d4c65-226">Die JavaScript und CSS-Dateien, enthalten, in dem jQuery Mobile Stil Hilfe mobile Browsern an den Inhalt in den Bildschirm passen, ohne jede Änderung im Markup Seite.</span><span class="sxs-lookup"><span data-stu-id="d4c65-226">The JavaScript and CSS files, included in the jQuery Mobile style, help mobile browsers to fit the content in the screen without making any change in the page markup.</span></span>

        ![jQuery-mobile-library-included-in-the-template](whats-new-in-aspnet-mvc-4/_static/image12.png)

        <span data-ttu-id="d4c65-228">*in der Vorlage enthaltenen jQuery mobile-Bibliothek*</span><span class="sxs-lookup"><span data-stu-id="d4c65-228">*jQuery mobile library included in the template*</span></span>
    - <span data-ttu-id="d4c65-229">**HTML5-basierte markup**</span><span class="sxs-lookup"><span data-stu-id="d4c65-229">**HTML5 based markup**</span></span>

        ![Mobile-application-template-using-HTML5-markup](whats-new-in-aspnet-mvc-4/_static/image13.png)

        <span data-ttu-id="d4c65-231">*Mobile Anwendung-Vorlage, die mit HTML5 Markup (Login.cshtml und index.cshtml)*</span><span class="sxs-lookup"><span data-stu-id="d4c65-231">*Mobile application template using HTML5 markup, (Login.cshtml and index.cshtml)*</span></span>
4. <span data-ttu-id="d4c65-232">Drücken Sie **F5** um die Projektmappe auszuführen.</span><span class="sxs-lookup"><span data-stu-id="d4c65-232">Press **F5** to run the solution.</span></span>
5. <span data-ttu-id="d4c65-233">Öffnen der **Windows Phone 7 Emulator**.</span><span class="sxs-lookup"><span data-stu-id="d4c65-233">Open the **Windows Phone 7 Emulator**.</span></span>
6. <span data-ttu-id="d4c65-234">Öffnen Sie Internet Explorer, in der Phone-Startbildschirm.</span><span class="sxs-lookup"><span data-stu-id="d4c65-234">In the phone start screen, open Internet Explorer.</span></span> <span data-ttu-id="d4c65-235">Sehen Sie sich die URL, wobei der desktop-Anwendung gestartet, und navigieren Sie zu der URL, die vom Telefon (z. B. `http://localhost:[PortNumber]/`).</span><span class="sxs-lookup"><span data-stu-id="d4c65-235">Check out the URL where the desktop application started and browse to that URL from the phone (e.g. `http://localhost:[PortNumber]/`).</span></span>
7. <span data-ttu-id="d4c65-236">Jetzt können Sie geben die Seite "Login", oder sehen Sie sich die zu den Seiten.</span><span class="sxs-lookup"><span data-stu-id="d4c65-236">Now you are able to enter the login page or check out the about page.</span></span> <span data-ttu-id="d4c65-237">Beachten Sie, dass das Format der Website auf die neue Windows Store-app für mobile Geräte.</span><span class="sxs-lookup"><span data-stu-id="d4c65-237">Notice that the style of the website is based on the new Metro app for mobile.</span></span> <span data-ttu-id="d4c65-238">Die ASP.NET MVC 4-Projektvorlage wird richtig angezeigt, auf mobilen Geräten, und stellen Sie sicher, dass alle Elemente der Seite sichtbar und aktiviert sind.</span><span class="sxs-lookup"><span data-stu-id="d4c65-238">The ASP.NET MVC 4 project template is correctly displayed on mobile devices, making sure all the elements of the page are visible and enabled.</span></span> <span data-ttu-id="d4c65-239">Beachten Sie, dass die Links in der Kopfzeile groß genug, um geklickt oder abgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="d4c65-239">Notice that the links on the header are big enough to be clicked or tapped.</span></span>

    <span data-ttu-id="d4c65-240">![Projekt-Seiten in einem mobilen Gerät](whats-new-in-aspnet-mvc-4/_static/image14.png "Projekt Vorlagenseiten für ein mobiles Gerät")</span><span class="sxs-lookup"><span data-stu-id="d4c65-240">![Project template pages in a mobile device](whats-new-in-aspnet-mvc-4/_static/image14.png "Project template pages in a mobile device")</span></span>

    <span data-ttu-id="d4c65-241">*Vorlagenseiten für das Projekt in einem mobilen Gerät*</span><span class="sxs-lookup"><span data-stu-id="d4c65-241">*Project template pages in a mobile device*</span></span>
8. <span data-ttu-id="d4c65-242">Die neue Vorlage verwendet auch die **Viewport Meta-Tag**.</span><span class="sxs-lookup"><span data-stu-id="d4c65-242">The new template also uses the **Viewport meta tag**.</span></span> <span data-ttu-id="d4c65-243">Die meisten mobile Browsern definieren eine Breite für ein virtuelles Browserfenster oder &quot;Viewport&quot;, ist größer als die tatsächliche Breite des mobilen Geräts.</span><span class="sxs-lookup"><span data-stu-id="d4c65-243">Most mobile browsers define a width for a virtual browser window or &quot;viewport&quot;, which is larger than the actual width of the mobile device.</span></span> <span data-ttu-id="d4c65-244">Dadurch können mobile Browser, um die gesamte Webseite innerhalb der virtuellen Anzeige anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="d4c65-244">This enables mobile browsers to display the entire web page inside the virtual display.</span></span> <span data-ttu-id="d4c65-245">Die **Viewport Meta-Tag** können Webentwickler, legen Sie die Breite, Höhe und Skalierung Browserbereich auf mobilen Geräten **.**</span><span class="sxs-lookup"><span data-stu-id="d4c65-245">The **Viewport meta tag** allows web developers to set the width, height and scale of the browser area on mobile devices **.**</span></span> <span data-ttu-id="d4c65-246">Die ASP.NET MVC 4-Vorlage für Mobile Anwendungen wird des Viewports auf die Breite des Geräts (&quot;Breite = Gerät Breite&quot;) in die Layoutvorlage (*Views\Shared\_Layout.cshtml*), sodass alle der Seiten werden ihre Viewport auf das Gerät Bildschirmbreite festgelegt haben.</span><span class="sxs-lookup"><span data-stu-id="d4c65-246">The ASP.NET MVC 4 template for Mobile Applications sets the viewport to the device width (&quot;width=device-width&quot;) in the layout template (*Views\Shared\_Layout.cshtml*), so that all the pages will have their viewport set to the device screen width.</span></span> <span data-ttu-id="d4c65-247">Beachten Sie, dass das Viewport-Meta-Tag die Standardansicht für den Browser nicht geändert werden.</span><span class="sxs-lookup"><span data-stu-id="d4c65-247">Notice that the Viewport meta tag will not change the default browser view.</span></span>
9. <span data-ttu-id="d4c65-248">Open  **\_Layout.cshtml**befindet sich im die **Ansichten | Freigegebene** Ordner und einen Kommentar das Viewport-Meta-Tag.</span><span class="sxs-lookup"><span data-stu-id="d4c65-248">Open **\_Layout.cshtml**, located in the **Views | Shared** folder, and comment the Viewport meta tag.</span></span> <span data-ttu-id="d4c65-249">Führen Sie die Anwendung, wenn nicht bereits geöffnet, und sehen Sie sich die Unterschiede.</span><span class="sxs-lookup"><span data-stu-id="d4c65-249">Run the application, if not already opened, and check out the differences.</span></span>


~~~
[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample2.cshtml)]

![The site after commenting the viewport meta tag](whats-new-in-aspnet-mvc-4/_static/image15.png "The site after commenting the viewport meta tag")

*The site after commenting the viewport meta tag*
~~~
10. <span data-ttu-id="d4c65-250">Drücken Sie in Visual Studio **UMSCHALT** + **F5** zum Debuggen der Anwendung zu beenden.</span><span class="sxs-lookup"><span data-stu-id="d4c65-250">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>
11. <span data-ttu-id="d4c65-251">Kommentieren Sie den Viewport-Meta-Tag.</span><span class="sxs-lookup"><span data-stu-id="d4c65-251">Uncomment the viewport meta tag.</span></span>


~~~
[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample3.cshtml)]
~~~

<a id="Task_3_-_Using_Adaptive_Rendering"></a>
#### <a name="task-3---using-adaptive-rendering"></a><span data-ttu-id="d4c65-252">Aufgabe 3 - mit adaptiver Rendering</span><span class="sxs-lookup"><span data-stu-id="d4c65-252">Task 3 - Using Adaptive Rendering</span></span>

<span data-ttu-id="d4c65-253">In dieser Aufgabe erfahren Sie, eine andere Methode zum Rendern einer Webseite ordnungsgemäß auf mobilen Geräten und Webbrowsern zur gleichen Zeit ohne jede Anpassung.</span><span class="sxs-lookup"><span data-stu-id="d4c65-253">In this task, you will learn another method to render a Web page correctly on mobile devices and Web browsers at the same time without any customization.</span></span> <span data-ttu-id="d4c65-254">Sie haben bereits Viewport Meta-Tag mit einem ähnlichen Zweck verwendet.</span><span class="sxs-lookup"><span data-stu-id="d4c65-254">You have already used Viewport meta tag with a similar purpose.</span></span> <span data-ttu-id="d4c65-255">Nachdem Sie eine weitere leistungsstarke Methode erfüllt: *adaptive Rendering*.</span><span class="sxs-lookup"><span data-stu-id="d4c65-255">Now you will meet another powerful method: *adaptive rendering*.</span></span>

<span data-ttu-id="d4c65-256">Adaptives Rendering ist eine Technik, die verwendet **CSS3-Medienabfragen** angewendet auf eine Seite anpassen.</span><span class="sxs-lookup"><span data-stu-id="d4c65-256">Adaptive rendering is a technique that uses **CSS3 media queries** to customize the style applied to a page.</span></span> <span data-ttu-id="d4c65-257">Medienabfragen definieren die Bedingungen in einem Stylesheet, gruppieren CSS-Formatvorlagen unter einer bestimmten Bedingung.</span><span class="sxs-lookup"><span data-stu-id="d4c65-257">Media queries define conditions inside a style sheet, grouping CSS styles under a certain condition.</span></span> <span data-ttu-id="d4c65-258">Nur, wenn die Bedingung "true" ist, wird das Format für die deklarierte Objekte angewendet.</span><span class="sxs-lookup"><span data-stu-id="d4c65-258">Only when the condition is true, the style is applied to the declared objects.</span></span>

<span data-ttu-id="d4c65-259">Die adaptive Renderingtechnik Flexibilität ermöglicht eine Anpassung für den Standort auf verschiedenen Geräten anzeigen.</span><span class="sxs-lookup"><span data-stu-id="d4c65-259">The flexibility provided by the adaptive rendering technique enables any customization for displaying the site on different devices.</span></span> <span data-ttu-id="d4c65-260">Sie können beliebig viele Formatvorlagen auf ein einzelnes Stylesheet ohne das Schreiben von Code für Geschäftslogik, sollen den Stil wählen definieren.</span><span class="sxs-lookup"><span data-stu-id="d4c65-260">You can define as many styles as you want on a single style sheet without writing logic code to choose the style.</span></span> <span data-ttu-id="d4c65-261">Daher ist es sehr praktisch Möglichkeit zur Anpassung der Seite Formatvorlagen, wie er verringert jedoch die Menge der doppelten Code und die Logik auch für das Rendern von Zwecken.</span><span class="sxs-lookup"><span data-stu-id="d4c65-261">Therefore, it is a very neat way of adapting page styles, as it reduces the amount of duplicated code and logic for rendering purposes.</span></span> <span data-ttu-id="d4c65-262">Andererseits, würde Auslastung der Netzwerkbandbreite erhöhen, wie die Größe der CSS-Dateien sind unwesentlich zunehmen kann.</span><span class="sxs-lookup"><span data-stu-id="d4c65-262">On the other hand, bandwidth consumption would increase, as the size of your CSS files could grow marginally.</span></span>

<span data-ttu-id="d4c65-263">Mithilfe der adaptiven Renderingtechnik Ihrer Website werden **ordnungsgemäß, unabhängig vom Browser angezeigt.**</span><span class="sxs-lookup"><span data-stu-id="d4c65-263">By using the adaptive rendering technique, your site will be **displayed properly, regardless of the browser.**</span></span> <span data-ttu-id="d4c65-264">Sie sollten jedoch berücksichtigen, wenn die zusätzliche Bandbreite Laden ist relevant.</span><span class="sxs-lookup"><span data-stu-id="d4c65-264">However, you should consider if the bandwidth extra load is a concern.</span></span>

> [!NOTE]
> <span data-ttu-id="d4c65-265">Das Grundformat einer Medienabfrage ist: @media \[Bereich: alle | handheld | drucken | Projektion | Bildschirm\] ([Eigenschaft: Wert] und... [Eigenschaft: Wert])</span><span class="sxs-lookup"><span data-stu-id="d4c65-265">The basic format of a media query is: @media \[Scope: all | handheld | print | projection | screen\] ([property:value] and ... [property:value])</span></span>


<span data-ttu-id="d4c65-266">Beispiele für Medienabfragen: &gt;  <strong>@media alle und (max. Breite von NULL: 1000px) und (min Breite: 700px) "{}":</strong> für alle Lösungen zwischen 700px und 1000px.</span><span class="sxs-lookup"><span data-stu-id="d4c65-266">Examples of media queries: &gt;<strong>@media all and (max-width: 1000px) and (min-width: 700px) {}:</strong> For all the resolutions between 700px and 1000px.</span></span>

> <span data-ttu-id="d4c65-267"><strong>@media Bildschirm und (min Breite: 400 px) und (max. Breite von NULL: 700px) {...}:</strong> nur für Bildschirme.</span><span class="sxs-lookup"><span data-stu-id="d4c65-267"><strong>@media screen and (min-width: 400px) and (max-width: 700px) { ... }:</strong> Only for screens.</span></span> <span data-ttu-id="d4c65-268">Die Lösung muss zwischen 400 und 700px sein.</span><span class="sxs-lookup"><span data-stu-id="d4c65-268">The resolution must be between 400 and 700px.</span></span>
> 
> <span data-ttu-id="d4c65-269"><strong>@media Handheld und (min Breite: 20em), Bildschirm und (min Breite: 20em) {...}:</strong> für Handheld-Geräte (Mobile und Geräte) und Bildschirme.</span><span class="sxs-lookup"><span data-stu-id="d4c65-269"><strong>@media handheld and (min-width: 20em), screen and (min-width: 20em) { ... }:</strong> For handhelds(mobile and devices) and screens.</span></span> <span data-ttu-id="d4c65-270">Die minimale Breite muss größer als 20em sein.</span><span class="sxs-lookup"><span data-stu-id="d4c65-270">The minimum width must be greater than 20em.</span></span>
> 
> <span data-ttu-id="d4c65-271">Weitere Informationen hierzu finden Sie auf der [W3C-Website](http://www.w3.org/TR/css3-mediaqueries/).</span><span class="sxs-lookup"><span data-stu-id="d4c65-271">You can find more information about this on the [W3C site](http://www.w3.org/TR/css3-mediaqueries/).</span></span>


<span data-ttu-id="d4c65-272">Sie werden jetzt untersuchen, wie die adaptive Wiedergabe funktioniert, verbessern die Lesbarkeit des ASP.NET-MVC 4-Standard-Website-Vorlage.</span><span class="sxs-lookup"><span data-stu-id="d4c65-272">You will now explore how the adaptive rendering works, improving the readability of the ASP.NET MVC 4 default website template.</span></span>

1. <span data-ttu-id="d4c65-273">Öffnen der **PhotoGallery.sln** Lösung, die Sie in Aufgabe 1 erstellt haben, und wählen Sie die **PhotoGallery** Projekt.</span><span class="sxs-lookup"><span data-stu-id="d4c65-273">Open the **PhotoGallery.sln** solution you have created at Task 1 and select the **PhotoGallery** project.</span></span> <span data-ttu-id="d4c65-274">Drücken Sie **F5** um die Projektmappe auszuführen.</span><span class="sxs-lookup"><span data-stu-id="d4c65-274">Press **F5** to run the solution.</span></span>
2. <span data-ttu-id="d4c65-275">Ändern Sie die Größe des Browsers Breite, die Windows festlegen, um die Hälfte oder weniger als ein Viertel ihrer ursprünglichen Größe beträgt.</span><span class="sxs-lookup"><span data-stu-id="d4c65-275">Resize the browser's width, setting the windows to half or to less than a quarter of its original size.</span></span> <span data-ttu-id="d4c65-276">Beachten Sie, was geschieht, mit den Elementen in der Kopfzeile: Einige Elemente werden nicht in den sichtbaren Bereich des Headers angezeigt.</span><span class="sxs-lookup"><span data-stu-id="d4c65-276">Notice what happens with the items in the header: Some elements will not appear in the visible area of the header.</span></span>
3. <span data-ttu-id="d4c65-277">Open <strong>"Site.CSS" ändern</strong> -Datei aus Visual Studio-Projektmappen-Explorer befindet sich im <strong>Content</strong> Projektordner.</span><span class="sxs-lookup"><span data-stu-id="d4c65-277">Open <strong>Site.css</strong> file from the Visual Studio Solution explorer, located in <strong>Content</strong> project folder.</span></span> <span data-ttu-id="d4c65-278">Drücken Sie <strong>STRG + F</strong> integrierte Suche in Visual Studio öffnen und das Schreiben <strong>@media</strong> zum Suchen der <strong>CSS Medienabfrage</strong>.</span><span class="sxs-lookup"><span data-stu-id="d4c65-278">Press <strong>CTRL + F</strong> to open Visual Studio integrated search, and write <strong>@media</strong> to locate the <strong>CSS media query</strong>.</span></span>

    <span data-ttu-id="d4c65-279">Die Media-Abfrage-Bedingung in dieser Vorlage definierten funktioniert auf diese Weise: Wenn der Browser Fenstergröße unterschreitet **850 px**, die CSS-Regeln angewendet sind die Personen, die innerhalb dieses Blocks Medien definiert.</span><span class="sxs-lookup"><span data-stu-id="d4c65-279">The media query condition defined in this template works in this way: When the browser's window size is below **850 px**, the CSS rules applied are the ones defined inside this media block.</span></span>

    <span data-ttu-id="d4c65-280">![Suchen die Medienabfrage](whats-new-in-aspnet-mvc-4/_static/image16.png "die Medienabfrage suchen")</span><span class="sxs-lookup"><span data-stu-id="d4c65-280">![Locating the media query](whats-new-in-aspnet-mvc-4/_static/image16.png "Locating the media query")</span></span>

    <span data-ttu-id="d4c65-281">*Die Medienabfrage suchen*</span><span class="sxs-lookup"><span data-stu-id="d4c65-281">*Locating the media query*</span></span>
4. <span data-ttu-id="d4c65-282">Ersetzen Sie den Max-Width-Attribut-Wert, der in 850 festgelegt px mit **10px**, um das adaptive Rendering zu deaktivieren, und drücken Sie **STRG + S** um die Änderungen zu speichern.</span><span class="sxs-lookup"><span data-stu-id="d4c65-282">Replace the max-width attribute value set in 850 px with **10px**, in order to disable the adaptive rendering, and press **CTRL + S** to save the changes.</span></span> <span data-ttu-id="d4c65-283">Zurück zu den Browser, und drücken Sie **STRG + F5** zur Aktualisierung von der Seite mit den Änderungen vorgenommen wurden.</span><span class="sxs-lookup"><span data-stu-id="d4c65-283">Return to the browser and press **CTRL + F5** to refresh the page with the changes you have made.</span></span> <span data-ttu-id="d4c65-284">Beachten Sie die Unterschiede in den beiden Seiten, wenn die Breite des Fensters anpassen.</span><span class="sxs-lookup"><span data-stu-id="d4c65-284">Notice the differences in both pages when adjusting the width of the window.</span></span>

    <span data-ttu-id="d4c65-285">![In der linken Seite anwendet der @media Format, in der rechten Seite den Stil ausgelassen wird](whats-new-in-aspnet-mvc-4/_static/image17.png "In der linken Seite wendet die @media Format, in der rechten Seite den Stil ausgelassen wird")</span><span class="sxs-lookup"><span data-stu-id="d4c65-285">![In the left, the page is applying the @media style, in the right, the style is omitted](whats-new-in-aspnet-mvc-4/_static/image17.png "In the left, the page is applying the @media style, in the right, the style is omitted")</span></span>

    <span data-ttu-id="d4c65-286"><em>In der linken Seite wendet die @media Formatvorlage in das Recht, das Format wird weggelassen.</em></span><span class="sxs-lookup"><span data-stu-id="d4c65-286"><em>In the left, the page is applying the @media style, in the right, the style is omitted</em></span></span>

    <span data-ttu-id="d4c65-287">Jetzt prüfen wir heraus, was auf mobilen Geräten:</span><span class="sxs-lookup"><span data-stu-id="d4c65-287">Now, let's check out what happens on mobile devices:</span></span>

    <span data-ttu-id="d4c65-288">![In der linken Seite anwendet der @media Format, in der rechten Seite den Stil ausgelassen wird](whats-new-in-aspnet-mvc-4/_static/image18.png "In der linken Seite wendet die @media Format, in der rechten Seite den Stil ausgelassen wird")</span><span class="sxs-lookup"><span data-stu-id="d4c65-288">![In the left, the page is applying the @media style, in the right, the style is omitted](whats-new-in-aspnet-mvc-4/_static/image18.png "In the left, the page is applying the @media style, in the right, the style is omitted")</span></span>

    <span data-ttu-id="d4c65-289"><em>In der linken Seite wendet die @media Formatvorlage in das Recht, das Format wird weggelassen.</em></span><span class="sxs-lookup"><span data-stu-id="d4c65-289"><em>In the left, the page is applying the @media style, in the right, the style is omitted</em></span></span>

    <span data-ttu-id="d4c65-290">Obwohl Sie bemerken, dass die Änderungen beim Rendern der Seite in einem Webbrowser nicht sehr wichtig ist, sind, wenn ein mobiles Gerät verwenden werden die Unterschiede leichter ersichtlich.</span><span class="sxs-lookup"><span data-stu-id="d4c65-290">Although you will notice that the changes when the page is rendered in a Web browser are not very significant, when using a mobile device the differences become more obvious.</span></span> <span data-ttu-id="d4c65-291">Auf der linken Seite des Bilds sehen wir, dass das benutzerdefinierte Format die Lesbarkeit verbessert.</span><span class="sxs-lookup"><span data-stu-id="d4c65-291">On the left side of the image, we can see that the custom style improved the readability.</span></span>

    <span data-ttu-id="d4c65-292">Adaptive rendern kann in viele weitere Szenarien, erleichtert es, gelten bedingte Formatvorlagen auf eine Website und Lösung von allgemeinen formatvorlagenprobleme praktisch Ansatz verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="d4c65-292">Adaptive rendering can be used in many more scenarios, making it easier to apply conditional styling to a Web site and solving common style issues with a neat approach.</span></span>

    <span data-ttu-id="d4c65-293">Die Viewport-Meta-Tag und der CSS-Medienabfragen sind nicht spezifisch für ASP.NET MVC 4, damit Sie diese Funktionen in einer beliebigen Webanwendung nutzen können.</span><span class="sxs-lookup"><span data-stu-id="d4c65-293">The Viewport meta tag and CSS media queries are not specific to ASP.NET MVC 4, so you can take advantage of these features in any web application.</span></span>
5. <span data-ttu-id="d4c65-294">Drücken Sie in Visual Studio **UMSCHALT** + **F5** zum Debuggen der Anwendung zu beenden.</span><span class="sxs-lookup"><span data-stu-id="d4c65-294">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_the_Photo_Gallery_Web_Application"></a>
### <a name="exercise-2-creating-the-photo-gallery-web-application"></a><span data-ttu-id="d4c65-295">Übung 2: Erstellen der Fotogalerie Web-Anwendung</span><span class="sxs-lookup"><span data-stu-id="d4c65-295">Exercise 2: Creating the Photo Gallery Web Application</span></span>

<span data-ttu-id="d4c65-296">In dieser Übung arbeiten Sie für eine Fotogalerie-Anwendung auf die Fotos anzeigen.</span><span class="sxs-lookup"><span data-stu-id="d4c65-296">In this exercise, you will work on a Photo Gallery application to display photos.</span></span> <span data-ttu-id="d4c65-297">Beginnen Sie mit der ASP.NET MVC 4-Projektvorlage, und klicken Sie dann fügen Sie eine Funktion zum Abrufen von Fotos von einem Dienst, und sie auf der Startseite anzeigen.</span><span class="sxs-lookup"><span data-stu-id="d4c65-297">You will start with the ASP.NET MVC 4 project template, and then you will add a feature to retrieve photos from a service and display them in the home page.</span></span>

<span data-ttu-id="d4c65-298">In der folgenden Übung aktualisieren Sie diese Lösung, um die Möglichkeit zu verbessern, die er auf mobilen Geräten angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="d4c65-298">In the following exercise, you will update this solution to enhance the way it is displayed on mobile devices.</span></span>

<a id="Task_1_-_Creating_a_Mock_Photo_Service"></a>
#### <a name="task-1---creating-a-mock-photo-service"></a><span data-ttu-id="d4c65-299">Aufgabe 1: Erstellen eines simulierten Foto-Diensts</span><span class="sxs-lookup"><span data-stu-id="d4c65-299">Task 1 - Creating a Mock Photo Service</span></span>

<span data-ttu-id="d4c65-300">In dieser Aufgabe erstellen Sie ein Mock des Foto-Diensts zum Abrufen des Inhalts, der im Katalog angezeigt werden soll.</span><span class="sxs-lookup"><span data-stu-id="d4c65-300">In this task, you will create a mock of the photo service to retrieve the content that will be displayed in the gallery.</span></span> <span data-ttu-id="d4c65-301">Zu diesem Zweck fügen Sie einen neuen Domänencontroller, der einfach eine JSON-Datei mit den Daten von jedem Foto zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="d4c65-301">To do this, you will add a new controller that will simply return a JSON file with the data of each photo.</span></span>

1. <span data-ttu-id="d4c65-302">Open **Visual Studio** Wenn nicht bereits geöffnet.</span><span class="sxs-lookup"><span data-stu-id="d4c65-302">Open **Visual Studio** if not already opened.</span></span>
2. <span data-ttu-id="d4c65-303">Wählen Sie die **Datei | Neue | Projekt** Menübefehl.</span><span class="sxs-lookup"><span data-stu-id="d4c65-303">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="d4c65-304">In der **neues Projekt** wählen Sie im Dialogfeld die **Visual C# | Web** Vorlage auf der linken Struktur, und wählen Sie **ASP.NET MVC 4-Webanwendung.**</span><span class="sxs-lookup"><span data-stu-id="d4c65-304">In the **New Project** dialog, select the **Visual C# | Web** template on the left pane tree, and choose **ASP.NET MVC 4 Web Application.**</span></span> <span data-ttu-id="d4c65-305">Nennen Sie das Projekt **PhotoGallery**, wählen Sie einen Speicherort (oder übernehmen Sie den Standardwert), und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="d4c65-305">Name the project **PhotoGallery**, select a location (or leave the default) and click **OK**.</span></span> <span data-ttu-id="d4c65-306">Alternativ können Sie Arbeit fortsetzen, aus der vorhandenen ASP.NET MVC 4 **Internetanwendung** -Lösung von **Übung 1** und den nächsten Schritt überspringen.</span><span class="sxs-lookup"><span data-stu-id="d4c65-306">Alternatively, you can continue working from your existing ASP.NET MVC 4 **Internet Application** solution from **Exercise 1** and skip the next step.</span></span>
3. <span data-ttu-id="d4c65-307">In der **neues ASP.NET MVC 4-Projekt** wählen Sie im Dialogfeld die **Internetanwendung** -Projektvorlage, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="d4c65-307">In the **New ASP.NET MVC 4 Project** dialog box, select the **Internet Application** project template and click **OK**.</span></span> <span data-ttu-id="d4c65-308">Stellen Sie sicher, dass Sie Razor als Ansichtsmodul ausgewählt haben.</span><span class="sxs-lookup"><span data-stu-id="d4c65-308">Make sure you have Razor selected as the View Engine.</span></span>
4. <span data-ttu-id="d4c65-309">In der **Projektmappen-Explorer**, mit der rechten Maustaste die **App\_Daten** Ordner des Projekts, und wählen Sie **hinzufügen | Vorhandenes Element**.</span><span class="sxs-lookup"><span data-stu-id="d4c65-309">In the **Solution Explorer**, right-click the **App\_Data** folder of your project, and select **Add | Existing Item**.</span></span> <span data-ttu-id="d4c65-310">Navigieren Sie zu der **Source\Assets\App\_Daten** Ordner dieser Anleitung und Hinzufügen der **Photos.json** Datei.</span><span class="sxs-lookup"><span data-stu-id="d4c65-310">Browse to the **Source\Assets\App\_Data** folder of this lab and add the **Photos.json** file.</span></span>
5. <span data-ttu-id="d4c65-311">Erstellen Sie einen neuen Domänencontroller mit dem Namen **PhotoController**.</span><span class="sxs-lookup"><span data-stu-id="d4c65-311">Create a new controller with the name **PhotoController**.</span></span> <span data-ttu-id="d4c65-312">Zu diesem Zweck mit der Maustaste auf die **Controller** wechseln Sie zum Ordner **hinzufügen** , und wählen Sie **Controller.**</span><span class="sxs-lookup"><span data-stu-id="d4c65-312">To do this, right-click on the **Controllers** folder, go to **Add** and select **Controller.**</span></span> <span data-ttu-id="d4c65-313">Führen Sie den Namen des Controllers, lassen Sie die **leerer MVC-Controller** Vorlage, und klicken Sie auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="d4c65-313">Complete the controller name, leave the **Empty MVC controller** template and click **Add**.</span></span>

    <span data-ttu-id="d4c65-314">![Hinzufügen der PhotoController](whats-new-in-aspnet-mvc-4/_static/image19.png "der PhotoController hinzufügen")</span><span class="sxs-lookup"><span data-stu-id="d4c65-314">![Adding the PhotoController](whats-new-in-aspnet-mvc-4/_static/image19.png "Adding the PhotoController")</span></span>

    <span data-ttu-id="d4c65-315">*Hinzufügen der PhotoController*</span><span class="sxs-lookup"><span data-stu-id="d4c65-315">*Adding the PhotoController*</span></span>
6. <span data-ttu-id="d4c65-316">Ersetzen Sie die **Index** -Methode durch Folgendes **Katalog** Aktion, und der Rückgabewert des Inhalts aus der JSON-Datei, die Sie vor kurzem dem Projekt hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="d4c65-316">Replace the **Index** method with the following **Gallery** action, and return the content from the JSON file you have recently added to the project.</span></span>

    <span data-ttu-id="d4c65-317">(Codeausschnitt - *ASP.NET MVC 4 Lab - Ex02 - Katalog Aktion*)</span><span class="sxs-lookup"><span data-stu-id="d4c65-317">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Gallery Action*)</span></span>


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample4.cs)]
~~~
7. <span data-ttu-id="d4c65-318">Drücken Sie **F5** , führen Sie die Projektmappe, und navigieren Sie zu folgender URL, um den Mocks erstellt Foto-Dienst zu testen: `http://localhost:[port]/photo/gallery` (der Wert [Port] hängt von den aktuellen Port, an die Anwendung gestartet wurde).</span><span class="sxs-lookup"><span data-stu-id="d4c65-318">Press **F5** to run the solution, and then browse to the following URL in order to test the mocked photo service: `http://localhost:[port]/photo/gallery` (the [port] value depends on the current port where the application was launched).</span></span> <span data-ttu-id="d4c65-319">Die Anforderung an diese URL abrufen soll den Inhalt der **Photos.json** Datei.</span><span class="sxs-lookup"><span data-stu-id="d4c65-319">The request to this URL should retrieve the content of the **Photos.json** file.</span></span>

    <span data-ttu-id="d4c65-320">![Testen des Diensts Mocks erstellt Foto](whats-new-in-aspnet-mvc-4/_static/image20.png "Testen des Diensts Foto Mocks erstellt")</span><span class="sxs-lookup"><span data-stu-id="d4c65-320">![Testing the mocked photo service](whats-new-in-aspnet-mvc-4/_static/image20.png "Testing the mocked photo service")</span></span>

    <span data-ttu-id="d4c65-321">*Testen des Diensts Foto Mocks erstellt*</span><span class="sxs-lookup"><span data-stu-id="d4c65-321">*Testing the mocked photo service*</span></span>

<span data-ttu-id="d4c65-322">In einer echten Implementierung können Sie [ASP.NET Web API](../../../../web-api/index.md) Implementieren des Diensts Fotogalerie.</span><span class="sxs-lookup"><span data-stu-id="d4c65-322">In a real implementation you could use [ASP.NET Web API](../../../../web-api/index.md) to implement the Photo Gallery service.</span></span> <span data-ttu-id="d4c65-323">ASP.NET Web-API ist ein Framework, das erleichtert das Erstellen von HTTP-Diensten, die eine Breite Palette von Clients, einschließlich Browsern und mobilen Geräten erreichen.</span><span class="sxs-lookup"><span data-stu-id="d4c65-323">ASP.NET Web API is a framework that makes it easy to build HTTP services that reach a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="d4c65-324">Die ASP.NET-Web-API ist eine ideale Plattform zum Erstellen von RESTful-Anwendungen in .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="d4c65-324">ASP.NET Web API is an ideal platform for building RESTful applications on the .NET Framework.</span></span>

<a id="Task_2_-_Displaying_the_Photo_Gallery"></a>
#### <a name="task-2---displaying-the-photo-gallery"></a><span data-ttu-id="d4c65-325">Aufgabe 2: Anzeigen der Fotogalerie</span><span class="sxs-lookup"><span data-stu-id="d4c65-325">Task 2 - Displaying the Photo Gallery</span></span>

<span data-ttu-id="d4c65-326">In dieser Aufgabe aktualisieren Sie die Homepage, um die Fotogalerie mithilfe des mocked-Diensts in die erste Aufgabe in dieser Übung erstellten anzeigen.</span><span class="sxs-lookup"><span data-stu-id="d4c65-326">In this task, you will update the Home page to show the photo gallery by using the mocked service you created in the first task of this exercise.</span></span> <span data-ttu-id="d4c65-327">Fügen die Modelldateien und die Katalogsichten zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="d4c65-327">You will add model files and update the gallery views.</span></span>

1. <span data-ttu-id="d4c65-328">Drücken Sie in Visual Studio **UMSCHALT** + **F5** zum Debuggen der Anwendung zu beenden.</span><span class="sxs-lookup"><span data-stu-id="d4c65-328">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>
2. <span data-ttu-id="d4c65-329">Erstellen der **Foto** -Klasse in der **Modelle** Ordner.</span><span class="sxs-lookup"><span data-stu-id="d4c65-329">Create the **Photo** class in the **Models** folder.</span></span> <span data-ttu-id="d4c65-330">Zu diesem Zweck mit der Maustaste auf die **Modelle** Ordner wählen **hinzufügen** , und klicken Sie auf **Klasse**.</span><span class="sxs-lookup"><span data-stu-id="d4c65-330">To do this, right-click on the **Models** folder, select **Add** and click **Class**.</span></span> <span data-ttu-id="d4c65-331">Legen Sie dann den Namen **Photo.cs** , und klicken Sie auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="d4c65-331">Then, set the name to **Photo.cs** and click **Add**.</span></span>
3. <span data-ttu-id="d4c65-332">Fügen Sie die folgenden Elemente auf der **Foto** Klasse.</span><span class="sxs-lookup"><span data-stu-id="d4c65-332">Add the following members to the **Photo** class.</span></span>

    <span data-ttu-id="d4c65-333">(Codeausschnitt - *ASP.NET MVC 4-Lab - Ex02 - Foto Modell*)</span><span class="sxs-lookup"><span data-stu-id="d4c65-333">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Photo model*)</span></span>


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample5.cs)]
~~~
4. <span data-ttu-id="d4c65-334">Öffnen Sie im Ordner **Controller** die Datei **HomeController.cs**.</span><span class="sxs-lookup"><span data-stu-id="d4c65-334">Open the **HomeController.cs** file from the **Controllers** folder.</span></span>
5. <span data-ttu-id="d4c65-335">Fügen Sie die folgenden using-Anweisungen hinzu.</span><span class="sxs-lookup"><span data-stu-id="d4c65-335">Add the following using statements.</span></span>

    <span data-ttu-id="d4c65-336">(Codeausschnitt - *ASP.NET MVC 4 Lab - Ex02 - HomeController Using-Direktiven*)</span><span class="sxs-lookup"><span data-stu-id="d4c65-336">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - HomeController Usings*)</span></span>


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample6.cs)]
~~~
6. <span data-ttu-id="d4c65-337">Update der **Index** zu verwendende Aktion **"HttpClient"** abgerufen Katalogdaten und verwenden Sie dann die **JavaScriptSerializer** Deserialisierung wird das Modell anzeigen.</span><span class="sxs-lookup"><span data-stu-id="d4c65-337">Update the **Index** action to use **HttpClient** to retrieve the gallery data, and then use the **JavaScriptSerializer** to deserialize it to the view model.</span></span>

    <span data-ttu-id="d4c65-338">(Codeausschnitt - *ASP.NET MVC 4-Lab - Ex02 - Index-Aktion*)</span><span class="sxs-lookup"><span data-stu-id="d4c65-338">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Index Action*)</span></span>


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample7.cs)]
~~~
7. <span data-ttu-id="d4c65-339">Öffnen der **Index.cshtml** -Datei unter der **Views\Home den** Ordner und Ersetzen Sie alle Inhalte durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="d4c65-339">Open the **Index.cshtml** file located under the **Views\Home** folder and replace all the content with the following code.</span></span>

    <span data-ttu-id="d4c65-340">Dieser Code durchläuft die Fotos, die vom Dienst abgerufen und in eine ungeordnete Liste angezeigt.</span><span class="sxs-lookup"><span data-stu-id="d4c65-340">This code loops through all the photos retrieved from the service and displays them into an unordered list.</span></span>

    <span data-ttu-id="d4c65-341">(Codeausschnitt - *ASP.NET MVC 4 Lab - Ex02 - Fotoliste*)</span><span class="sxs-lookup"><span data-stu-id="d4c65-341">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Photo List*)</span></span>


~~~
[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample8.cshtml)]
~~~
8. <span data-ttu-id="d4c65-342">In der **Projektmappen-Explorer**, mit der rechten Maustaste die **Content** Ordner des Projekts, und wählen Sie **hinzufügen | Vorhandenes Element**.</span><span class="sxs-lookup"><span data-stu-id="d4c65-342">In the **Solution Explorer**, right-click the **Content** folder of your project, and select **Add | Existing Item**.</span></span> <span data-ttu-id="d4c65-343">Navigieren Sie zu der **Source\Assets\Content** Ordner dieser Anleitung und Hinzufügen der **"Site.CSS" ändern** Datei.</span><span class="sxs-lookup"><span data-stu-id="d4c65-343">Browse to the **Source\Assets\Content** folder of this lab and add the **Site.css** file.</span></span> <span data-ttu-id="d4c65-344">Sie müssen den Ersatz zu bestätigen.</span><span class="sxs-lookup"><span data-stu-id="d4c65-344">You will have to confirm its replacement.</span></span> <span data-ttu-id="d4c65-345">Wenn Sie haben die **"Site.CSS" ändern** Datei öffnen, müssen Sie bestätigen, um die Datei auch erneut laden.</span><span class="sxs-lookup"><span data-stu-id="d4c65-345">If you have the **Site.css** file open, you will have to confirm to reload the file also.</span></span>
9. <span data-ttu-id="d4c65-346">Öffnen Sie den Datei-Explorer, und kopieren Sie die gesamte **Fotos** Ordner befindet sich unter dem **Source\Assets** Ordner dieser Anleitung in den Stammordner des Projekts im Projektmappen-Explorer.</span><span class="sxs-lookup"><span data-stu-id="d4c65-346">Open File Explorer and copy the entire **Photos** folder located under the **Source\Assets** folder of this lab to the root folder of your project in Solution Explorer.</span></span>
10. <span data-ttu-id="d4c65-347">Führen Sie die Anwendung aus.</span><span class="sxs-lookup"><span data-stu-id="d4c65-347">Run the application.</span></span> <span data-ttu-id="d4c65-348">Jetzt sollte der Startseite anzeigen von Fotos im Katalog angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="d4c65-348">You should now see the home page displaying the photos in the gallery.</span></span>

    <span data-ttu-id="d4c65-349">![Fotogalerie](whats-new-in-aspnet-mvc-4/_static/image21.png "Fotogalerie")</span><span class="sxs-lookup"><span data-stu-id="d4c65-349">![Photo Gallery](whats-new-in-aspnet-mvc-4/_static/image21.png "Photo Gallery")</span></span>

    <span data-ttu-id="d4c65-350">*Fotogalerie*</span><span class="sxs-lookup"><span data-stu-id="d4c65-350">*Photo Gallery*</span></span>
11. <span data-ttu-id="d4c65-351">Drücken Sie in Visual Studio **UMSCHALT** + **F5** zum Debuggen der Anwendung zu beenden.</span><span class="sxs-lookup"><span data-stu-id="d4c65-351">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Adding_support_for_mobile_devices"></a>
### <a name="exercise-3-adding-support-for-mobile-devices"></a><span data-ttu-id="d4c65-352">Übung 3: Hinzufügen der Unterstützung für mobile Geräte</span><span class="sxs-lookup"><span data-stu-id="d4c65-352">Exercise 3: Adding support for mobile devices</span></span>

<span data-ttu-id="d4c65-353">Eines der wichtigsten Updates in ASP.NET MVC 4 wird die Unterstützung für mobile-Entwicklung.</span><span class="sxs-lookup"><span data-stu-id="d4c65-353">One of the key updates in ASP.NET MVC 4 is the support for mobile development.</span></span> <span data-ttu-id="d4c65-354">In dieser Übung untersuchen Sie neue ASP.NET MVC 4-Funktionen für mobile Anwendungen durch Erweitern der PhotoGallery-Lösung, die Sie in der vorherigen Übung erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="d4c65-354">In this exercise, you will explore ASP.NET MVC 4 new features for mobile applications by extending the PhotoGallery solution you have created in the previous exercise.</span></span>

<a id="Task_1_-_Installing_jQuery_Mobile_in_an_ASPNET_MVC_4_Application"></a>
#### <a name="task-1---installing-jquery-mobile-in-an-aspnet-mvc-4-application"></a><span data-ttu-id="d4c65-355">Aufgabe 1: Installieren von jQuery Mobile in einer ASP.NET MVC 4-Anwendung</span><span class="sxs-lookup"><span data-stu-id="d4c65-355">Task 1 - Installing jQuery Mobile in an ASP.NET MVC 4 Application</span></span>

1. <span data-ttu-id="d4c65-356">Öffnen der **beginnen** Lösung finden Sie unter **Quell-/Ex3-MobileSupport/Begin/** Ordner.</span><span class="sxs-lookup"><span data-stu-id="d4c65-356">Open the **Begin** solution located at **Source/Ex3-MobileSupport/Begin/** folder.</span></span> <span data-ttu-id="d4c65-357">Andernfalls möglicherweise weiterhin mithilfe der **End** Lösung abgerufen, von der vorherigen Übung abschließen.</span><span class="sxs-lookup"><span data-stu-id="d4c65-357">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="d4c65-358">Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung müssen Sie einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren.</span><span class="sxs-lookup"><span data-stu-id="d4c65-358">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="d4c65-359">Klicken Sie hierzu auf die **Projekt** Menü **NuGet-Pakete verwalten**.</span><span class="sxs-lookup"><span data-stu-id="d4c65-359">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="d4c65-360">In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.</span><span class="sxs-lookup"><span data-stu-id="d4c65-360">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="d4c65-361">Schließlich erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.</span><span class="sxs-lookup"><span data-stu-id="d4c65-361">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="d4c65-362">Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken, die sich in Ihrem Projekt liefern Verringern der Projektgröße.</span><span class="sxs-lookup"><span data-stu-id="d4c65-362">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="d4c65-363">Mit NuGet-Powertools werden durch Angabe der Paketversionen in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, wenn, das Sie das Projekt ausführen, können.</span><span class="sxs-lookup"><span data-stu-id="d4c65-363">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="d4c65-364">Deshalb wird müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit.</span><span class="sxs-lookup"><span data-stu-id="d4c65-364">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="d4c65-365">Öffnen der **Package Manager Console** durch Klicken auf die **Tools** &gt; **Bibliothekspaket-Manager** &gt; **Paket-Manager Konsole** Option des Menüs.</span><span class="sxs-lookup"><span data-stu-id="d4c65-365">Open the **Package Manager Console** by clicking the **Tools** &gt; **Library Package Manager** &gt; **Package Manager Console** menu option.</span></span>

    <span data-ttu-id="d4c65-366">![Öffnen das NuGet-Paket-Manager-Konsole](whats-new-in-aspnet-mvc-4/_static/image22.png "der NuGet-Paket-Manager-Konsole öffnen")</span><span class="sxs-lookup"><span data-stu-id="d4c65-366">![Opening the NuGet Package Manager Console](whats-new-in-aspnet-mvc-4/_static/image22.png "Opening the NuGet Package Manager Console")</span></span>

    <span data-ttu-id="d4c65-367">*Öffnen das NuGet-Paket-Manager-Konsole*</span><span class="sxs-lookup"><span data-stu-id="d4c65-367">*Opening the NuGet Package Manager Console*</span></span>
3. <span data-ttu-id="d4c65-368">Führen Sie in der Paket-Manager-Konsole den folgenden Befehl zum Installieren der **jQuery.Mobile.MVC** Paket.</span><span class="sxs-lookup"><span data-stu-id="d4c65-368">In the Package Manager Console run the following command to install the **jQuery.Mobile.MVC** package.</span></span>

    <span data-ttu-id="d4c65-369">jQuery Mobile ist eine open Source-Bibliothek zum Erstellen von berührungsoptimiertes Web-Benutzeroberfläche.</span><span class="sxs-lookup"><span data-stu-id="d4c65-369">jQuery Mobile is an open source library for building touch-optimized web UI.</span></span> <span data-ttu-id="d4c65-370">Das jQuery.Mobile.MVC NuGet-Paket enthält Hilfsprogramme zum Verwenden von jQuery Mobile für eine ASP.NET MVC 4-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="d4c65-370">The jQuery.Mobile.MVC NuGet package includes helpers to use jQuery Mobile with an ASP.NET MVC 4 application.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d4c65-371">Durch den folgenden Befehl ausführen, werden Sie die Bibliothek jQuery.Mobile.MVC von Nuget herunterladen.</span><span class="sxs-lookup"><span data-stu-id="d4c65-371">By running the following command, you will be downloading the jQuery.Mobile.MVC library from Nuget.</span></span>

    <span data-ttu-id="d4c65-372">PM</span><span class="sxs-lookup"><span data-stu-id="d4c65-372">PM</span></span>

    [!code-powershell[Main](whats-new-in-aspnet-mvc-4/samples/sample9.ps1)]

    <span data-ttu-id="d4c65-373">Mit diesem Befehl werden installiert, jQuery Mobile und einige Helper-Dateien, einschließlich der folgenden:</span><span class="sxs-lookup"><span data-stu-id="d4c65-373">This command installs jQuery Mobile and some helper files, including the following:</span></span>

    - <span data-ttu-id="d4c65-374">**Ansichten/freigegeben/\_Layout.Mobile.cshtml**: ein jQuery Mobile-basierten Layout für kleinere Bildschirme optimiert ist.</span><span class="sxs-lookup"><span data-stu-id="d4c65-374">**Views/Shared/\_Layout.Mobile.cshtml**: is a jQuery Mobile-based layout optimized for a smaller screen.</span></span> <span data-ttu-id="d4c65-375">Wenn die Website aus einem mobilen Browser eine Anforderung empfängt, ersetzen Sie das ursprüngliche Layout (\_Layout.cshtml) durch dieses.</span><span class="sxs-lookup"><span data-stu-id="d4c65-375">When the website receives a request from a mobile browser, it will replace the original layout (\_Layout.cshtml) with this one.</span></span>
    - <span data-ttu-id="d4c65-376">Eine ansichtumschaltung Komponente: besteht aus den **Ansichten/freigegeben/\_ViewSwitcher.cshtml** Teilansicht und **ViewSwitcherController.cs** Controller.</span><span class="sxs-lookup"><span data-stu-id="d4c65-376">A view-switcher component: consists of the **Views/Shared/\_ViewSwitcher.cshtml** partial view and the **ViewSwitcherController.cs** controller.</span></span> <span data-ttu-id="d4c65-377">Diese Komponente zeigt einen Link in mobilen Browsern an Benutzer in der Desktopversion von der Seite wechseln können.</span><span class="sxs-lookup"><span data-stu-id="d4c65-377">This component will show a link on mobile browsers to enable users to switch to the desktop version of the page.</span></span>  
        <span data-ttu-id="d4c65-378">![Fotogalerie-Projekts mit Unterstützung für mobile](whats-new-in-aspnet-mvc-4/_static/image23.png "Fotogalerie-Projekts mit Unterstützung für mobile")</span><span class="sxs-lookup"><span data-stu-id="d4c65-378">![Photo Gallery project with mobile support](whats-new-in-aspnet-mvc-4/_static/image23.png "Photo Gallery project with mobile support")</span></span>

        <span data-ttu-id="d4c65-379">*Fotogalerie-Projekts mit Unterstützung für mobile*</span><span class="sxs-lookup"><span data-stu-id="d4c65-379">*Photo Gallery project with mobile support*</span></span>
4. <span data-ttu-id="d4c65-380">Registrieren Sie die Mobile Bündel.</span><span class="sxs-lookup"><span data-stu-id="d4c65-380">Register the Mobile bundles.</span></span> <span data-ttu-id="d4c65-381">Öffnen Sie hierzu die **Global.asax.cs** Datei, und fügen Sie die folgende Zeile hinzu.</span><span class="sxs-lookup"><span data-stu-id="d4c65-381">To do this, open the **Global.asax.cs** file and add the following line.</span></span>

    <span data-ttu-id="d4c65-382">(Codeausschnitt - *ASP.NET MVC 4 Lab - Ex03 - Register Mobile Bündel*)</span><span class="sxs-lookup"><span data-stu-id="d4c65-382">(Code Snippet - *ASP.NET MVC 4 Lab - Ex03 - Register Mobile Bundles*)</span></span>


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample10.cs)]
~~~
5. <span data-ttu-id="d4c65-383">Führen Sie die Anwendung, die mithilfe eines Webbrowsers desktop.</span><span class="sxs-lookup"><span data-stu-id="d4c65-383">Run the application using a desktop web browser.</span></span>
6. <span data-ttu-id="d4c65-384">Öffnen der **Windows Phone 7 Emulator** befindet sich im **Startmenü | Alle Programme | Windows Phone SDK 7.1 | Windows Phone-Emulator.**</span><span class="sxs-lookup"><span data-stu-id="d4c65-384">Open the **Windows Phone 7 Emulator,** located in **Start Menu | All Programs | Windows Phone SDK 7.1 | Windows Phone Emulator.**</span></span>
7. <span data-ttu-id="d4c65-385">Öffnen Sie Internet Explorer, in der Phone-Startbildschirm.</span><span class="sxs-lookup"><span data-stu-id="d4c65-385">In the phone start screen, open Internet Explorer.</span></span> <span data-ttu-id="d4c65-386">Sehen Sie sich die URL, in dem die Anwendung gestartet, und navigieren Sie zu dieser URL mit den Phone-Browser (z. B. `http://localhost:[PortNumber]/`).</span><span class="sxs-lookup"><span data-stu-id="d4c65-386">Check out the URL where the application started and browse to that URL with the phone browser (e.g. `http://localhost:[PortNumber]/`).</span></span>

    <span data-ttu-id="d4c65-387">Beachten Sie, dass Ihre Anwendung im Windows Phone-Emulator unterschiedliche aussieht wie die jQuery.Mobile.MVC neue Objekte erstellt hat, im Projekt, das Anzeigen von Ansichten für mobile Geräte optimiert.</span><span class="sxs-lookup"><span data-stu-id="d4c65-387">You will notice that your application will look different in the Windows Phone emulator, as the jQuery.Mobile.MVC has created new assets in your project that show views optimized for mobile devices.</span></span>

    <span data-ttu-id="d4c65-388">Beachten Sie die Meldung am oberen Rand der Telefon mit den Link, der in der Desktopansicht wechselt.</span><span class="sxs-lookup"><span data-stu-id="d4c65-388">Notice the message at the top of the phone, showing the link that switches to the Desktop view.</span></span> <span data-ttu-id="d4c65-389">Darüber hinaus die  **\_Layout.Mobile.cshtml** Layouts, das vom Paket erstellt wurde, installiert ist ein anderes Layout einschließlich der in der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="d4c65-389">Additionally, the **\_Layout.Mobile.cshtml** layout that was created by the package you have installed is including a different layout in the application.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d4c65-390">Bisher, besteht keine Verbindung zu mobile Ansicht zurückzukehren.</span><span class="sxs-lookup"><span data-stu-id="d4c65-390">So far, there is no link to get back to mobile view.</span></span> <span data-ttu-id="d4c65-391">Es wird in höheren Versionen enthalten.</span><span class="sxs-lookup"><span data-stu-id="d4c65-391">It will be included in later versions.</span></span>

    <span data-ttu-id="d4c65-392">![Mobile Ansicht der Foto Gallery Startseite](whats-new-in-aspnet-mvc-4/_static/image24.png "Mobile Ansicht der Foto-Katalog-Startseite")</span><span class="sxs-lookup"><span data-stu-id="d4c65-392">![Mobile view of the Photo Gallery Home page](whats-new-in-aspnet-mvc-4/_static/image24.png "Mobile view of the Photo Gallery Home page")</span></span>

    <span data-ttu-id="d4c65-393">*Mobile Ansicht der Foto-Katalog-Startseite*</span><span class="sxs-lookup"><span data-stu-id="d4c65-393">*Mobile view of the Photo Gallery Home page*</span></span>
8. <span data-ttu-id="d4c65-394">Drücken Sie in Visual Studio **UMSCHALT** + **F5** zum Debuggen der Anwendung zu beenden.</span><span class="sxs-lookup"><span data-stu-id="d4c65-394">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>

<a id="Task_2_-_Creating_Mobile_Views"></a>
#### <a name="task-2---creating-mobile-views"></a><span data-ttu-id="d4c65-395">Aufgabe 2: Erstellen von mobilen Ansichten</span><span class="sxs-lookup"><span data-stu-id="d4c65-395">Task 2 - Creating Mobile Views</span></span>

<span data-ttu-id="d4c65-396">In dieser Aufgabe erstellen Sie eine mobile Version die Indexansicht mit Inhalt, die für eine bessere Appareance auf mobilen Geräten angepasst.</span><span class="sxs-lookup"><span data-stu-id="d4c65-396">In this task, you will create a mobile version of the index view with content adapted for better appareance in mobile devices.</span></span>

1. <span data-ttu-id="d4c65-397">Kopieren der **Views\Home\Index.cshtml** anzuzeigen, und fügen Sie ihn auf eine Kopie zu erstellen, benennen Sie die neue Datei um **Index.Mobile.cshtml**.</span><span class="sxs-lookup"><span data-stu-id="d4c65-397">Copy the **Views\Home\Index.cshtml** view and paste it to create a copy, rename the new file to **Index.Mobile.cshtml**.</span></span>
2. <span data-ttu-id="d4c65-398">Öffnen die neu erstellte **Index.Mobile.cshtml** anzuzeigen, und Ersetzen Sie die vorhandene &lt;Ul&gt; Tag mit dem folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="d4c65-398">Open the new created **Index.Mobile.cshtml** view and replace the existing &lt;ul&gt; tag with this code.</span></span> <span data-ttu-id="d4c65-399">Durch dieses Vorgehen Sie aktualisieren die &lt;Ul&gt; Tag mit jQuery Mobile datenanmerkungen von jQuery mobile Designs zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="d4c65-399">By doing this, you will be updating the &lt;ul&gt; tag with jQuery Mobile data annotations to use the mobile themes from jQuery.</span></span>


~~~
[!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample11.html)]

> [!NOTE] 
> 
> Notice that:
> 
> - The **data-role** attribute set to **listview** will render the list using the listview styles.
> 
> - The **data-inset** attribute set to true will show the list with rounded border and margin.
> 
> - The **data-filter** attribute set to **true** will generate a search box.
> 
> You can learn more about jQuery Mobile conventions in the project documentation: [[http://jquerymobile.com/demos/1.1.1/](http://jquerymobile.com/demos/1.1.1/)](http://jquerymobile.com/demos/1.1.1/)
~~~
3. <span data-ttu-id="d4c65-400">Drücken Sie **STRG + S** um die Änderungen zu speichern.</span><span class="sxs-lookup"><span data-stu-id="d4c65-400">Press **CTRL + S** to save the changes.</span></span>
4. <span data-ttu-id="d4c65-401">Wechseln Sie zu der **Windows Phone-Emulator** und den Standort aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="d4c65-401">Switch to the **Windows Phone Emulator** and refresh the site.</span></span> <span data-ttu-id="d4c65-402">Beachten Sie das neue Aussehen und Verhalten der Katalogliste sowie das neue Suchfeld befindet sich oben.</span><span class="sxs-lookup"><span data-stu-id="d4c65-402">Notice the new look and feel of the gallery list, as well as the new search box located on the top.</span></span> <span data-ttu-id="d4c65-403">Geben Sie dann ein Wort in das Suchfeld (z. B. **Tulpen**) So starten Sie eine Suche in der Fotogalerie.</span><span class="sxs-lookup"><span data-stu-id="d4c65-403">Then, type a word in the search box (for instance, **Tulips**) to start a search in the photo gallery.</span></span>

    <span data-ttu-id="d4c65-404">![Katalog mithilfe von Listview-Stil mit Filtern](whats-new-in-aspnet-mvc-4/_static/image25.png "Katalog mithilfe von Listview-Stil mit Filtern")</span><span class="sxs-lookup"><span data-stu-id="d4c65-404">![Gallery using listview style with filtering](whats-new-in-aspnet-mvc-4/_static/image25.png "Gallery using listview style with filtering")</span></span>

    <span data-ttu-id="d4c65-405">*Katalog mithilfe von Listview-Stil mit Filtern*</span><span class="sxs-lookup"><span data-stu-id="d4c65-405">*Gallery using listview style with filtering*</span></span>

    <span data-ttu-id="d4c65-406">Kurz gesagt, haben Sie das Rezept Ansicht Mobilizer verwendet, erstellen Sie eine Kopie der Indexansicht mit der &quot;mobile&quot; Suffix.</span><span class="sxs-lookup"><span data-stu-id="d4c65-406">To summarize, you have used the View Mobilizer recipe to create a copy of the Index view with the &quot;mobile&quot; suffix.</span></span> <span data-ttu-id="d4c65-407">Dieses Suffix zeigt ASP.NET MVC 4, dass jede Anforderung, die von einem mobilen Gerät generiert diese Kopie des Indexes verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="d4c65-407">This suffix indicates to ASP.NET MVC 4 that every request generated from a mobile device will use this copy of the index.</span></span> <span data-ttu-id="d4c65-408">Darüber hinaus haben Sie die mobile Version von jQuery Mobile verwendet für die Website Aussehen und Verhalten auf mobilen Geräten zu verbessern, die Indexansicht aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="d4c65-408">Additionally, you have updated the mobile version of the Index view to use jQuery Mobile for enhancing the site look and feel in mobile devices.</span></span>
5. <span data-ttu-id="d4c65-409">Wechseln Sie zurück zu Visual Studio und öffnen **Site.Mobile.css** unterhalb der **Content** Ordner.</span><span class="sxs-lookup"><span data-stu-id="d4c65-409">Go back to Visual Studio and open **Site.Mobile.css** located under the **Content** folder.</span></span>
6. <span data-ttu-id="d4c65-410">Korrigieren Sie die Positionierung des Titels Foto, zeigen auf der rechten Seite des Bilds zu vereinfachen.</span><span class="sxs-lookup"><span data-stu-id="d4c65-410">Fix the positioning of the photo title to make it show at the right side of the image.</span></span> <span data-ttu-id="d4c65-411">Zu diesem Zweck fügen Sie den folgenden Code aus, um die **Site.Mobile.css** Datei.</span><span class="sxs-lookup"><span data-stu-id="d4c65-411">To do this, add the following code to the **Site.Mobile.css** file.</span></span>

    <span data-ttu-id="d4c65-412">CSS</span><span class="sxs-lookup"><span data-stu-id="d4c65-412">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-mvc-4/samples/sample12.css)]
7. <span data-ttu-id="d4c65-413">Drücken Sie **STRG + S** um die Änderungen zu speichern.</span><span class="sxs-lookup"><span data-stu-id="d4c65-413">Press **CTRL + S** to save the changes.</span></span>
8. <span data-ttu-id="d4c65-414">Wechseln Sie zurück zu den **Windows Phone-Emulator** und den Standort aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="d4c65-414">Switch back to the **Windows Phone Emulator** and refresh the site.</span></span> <span data-ttu-id="d4c65-415">Beachten Sie, dass der Titel Foto wird jetzt ordnungsgemäß positioniert.</span><span class="sxs-lookup"><span data-stu-id="d4c65-415">Notice the photo title is properly positioned now.</span></span>

    <span data-ttu-id="d4c65-416">![Auf der rechten Seite des Bilds positioniert Titel](whats-new-in-aspnet-mvc-4/_static/image26.png "auf der rechten Seite des Bilds positioniert Titel")</span><span class="sxs-lookup"><span data-stu-id="d4c65-416">![Title positioned on the right side of the image](whats-new-in-aspnet-mvc-4/_static/image26.png "Title positioned on the right side of the image")</span></span>

    <span data-ttu-id="d4c65-417">*Auf der rechten Seite des Bilds positioniert Titel*</span><span class="sxs-lookup"><span data-stu-id="d4c65-417">*Title positioned on the right side of the image*</span></span>

<a id="Task_3_-_jQuery_Mobile_Themes"></a>
#### <a name="task-3---jquery-mobile-themes"></a><span data-ttu-id="d4c65-418">Aufgabe 3 - jQuery Mobile-Designs</span><span class="sxs-lookup"><span data-stu-id="d4c65-418">Task 3 - jQuery Mobile Themes</span></span>

<span data-ttu-id="d4c65-419">Jedes Layout und die Widgets im jQuery Mobile ist für ein neues objektorientierte CSS-Framework konzipiert, die mit der eine vollständige einheitliche visuellen Entwurf Design auf Sites und Anwendungen anwenden können.</span><span class="sxs-lookup"><span data-stu-id="d4c65-419">Every layout and widget in jQuery Mobile is designed around a new object-oriented CSS framework that makes it possible to apply a complete unified visual design theme to sites and applications.</span></span>

<span data-ttu-id="d4c65-420">jQuery Mobile Standard Design enthält 5 Muster, die sich Buchstaben (a, b, C, d e) zur schnellen Bezugnahme.</span><span class="sxs-lookup"><span data-stu-id="d4c65-420">jQuery Mobile's default Theme includes 5 swatches that are given letters (a, b, c, d, e) for quick reference.</span></span>

<span data-ttu-id="d4c65-421">In dieser Aufgabe aktualisieren Sie die mobile Layout für ein anderes Design als den Standardwert verwenden.</span><span class="sxs-lookup"><span data-stu-id="d4c65-421">In this task, you will update the mobile layout to use a different theme than the default.</span></span>

1. <span data-ttu-id="d4c65-422">Wechseln Sie zurück zu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d4c65-422">Switch back to Visual Studio.</span></span>
2. <span data-ttu-id="d4c65-423">Öffnen der  **\_Layout.Mobile.cshtml** -Datei im **Views\Shared**.</span><span class="sxs-lookup"><span data-stu-id="d4c65-423">Open the **\_Layout.Mobile.cshtml** file located in **Views\Shared**.</span></span>
3. <span data-ttu-id="d4c65-424">Suchen Sie das Div-Element mit den Data-Role festgelegt &quot;Seite&quot; und Aktualisieren der **Data-Theme** auf &quot; **e**&quot;.</span><span class="sxs-lookup"><span data-stu-id="d4c65-424">Find the div element with the data-role set to &quot;page&quot; and update the **data-theme** to &quot;**e**&quot;.</span></span>


~~~
[!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample13.html)]
~~~
4. <span data-ttu-id="d4c65-425">Drücken Sie **STRG + S** um die Änderungen zu speichern.</span><span class="sxs-lookup"><span data-stu-id="d4c65-425">Press **CTRL + S** to save the changes.</span></span>
5. <span data-ttu-id="d4c65-426">Aktualisieren Sie den Standort in der **Windows Phone-Emulator** , und beachten Sie das neue Farbschema.</span><span class="sxs-lookup"><span data-stu-id="d4c65-426">Refresh the site in the **Windows Phone Emulator** and notice the new colors scheme.</span></span>

    <span data-ttu-id="d4c65-427">![Mobile Layout mit einem anderen Farbschema](whats-new-in-aspnet-mvc-4/_static/image27.png "Mobile Layout mit einem anderen Farbschema")</span><span class="sxs-lookup"><span data-stu-id="d4c65-427">![Mobile layout with a different color scheme](whats-new-in-aspnet-mvc-4/_static/image27.png "Mobile layout with a different color scheme")</span></span>

    <span data-ttu-id="d4c65-428">*Mobile Layout mit einem anderen Farbschema*</span><span class="sxs-lookup"><span data-stu-id="d4c65-428">*Mobile layout with a different color scheme*</span></span>

<a id="Task_4_-_Using_the_View-Switcher_Component_and_the_Browser_Overriding_Features"></a>
#### <a name="task-4---using-the-view-switcher-component-and-the-browser-overriding-features"></a><span data-ttu-id="d4c65-429">Aufgabe 4: Verwenden der Ansichtumschaltung Komponente und den Browser Überschreiben von Funktionen</span><span class="sxs-lookup"><span data-stu-id="d4c65-429">Task 4 - Using the View-Switcher Component and the Browser Overriding Features</span></span>

<span data-ttu-id="d4c65-430">Eine Konvention für Mobilgeräte optimierte Webseiten besteht darin, einen Link hinzufügen, dessen Text ist etwas wie Desktopansicht oder vollständige standortmodus, in dem Benutzer, die in einer desktop-Version von der Seite wechseln kann.</span><span class="sxs-lookup"><span data-stu-id="d4c65-430">A convention for mobile-optimized web pages is to add a link whose text is something like Desktop view or Full site mode that lets users switch to a desktop version of the page.</span></span> <span data-ttu-id="d4c65-431">Das jQuery.Mobile.MVC-Paket enthält ein Beispiel für **ansichtumschaltung** -Komponente für diesen Zweck verwendet werden, der  **\_Layout.Mobile.cshtml** anzeigen.</span><span class="sxs-lookup"><span data-stu-id="d4c65-431">The jQuery.Mobile.MVC package includes a sample **view-switcher** component for this purpose used in the **\_Layout.Mobile.cshtml** view.</span></span>

<span data-ttu-id="d4c65-432">![Link zur Ansicht Desktop wechseln](whats-new-in-aspnet-mvc-4/_static/image28.png "Link, um zur Desktop-Ansicht wechseln")</span><span class="sxs-lookup"><span data-stu-id="d4c65-432">![Link to switch to Desktop View](whats-new-in-aspnet-mvc-4/_static/image28.png "Link to switch to Desktop View")</span></span>

<span data-ttu-id="d4c65-433">*Link zum Desktop-Ansicht wechseln*</span><span class="sxs-lookup"><span data-stu-id="d4c65-433">*Link to switch to Desktop View*</span></span>

<span data-ttu-id="d4c65-434">Ansichtumschaltung verwendet ein neues Feature namens **Browser überschreiben**.</span><span class="sxs-lookup"><span data-stu-id="d4c65-434">The view switcher uses a new feature called **Browser Overriding**.</span></span> <span data-ttu-id="d4c65-435">Mit diesem Feature können Ihre Anwendung Anforderungen zu behandeln, als ob sie von einem anderen Browser (Benutzer-Agent) als dem gestellt würden, die tatsächlich von kommen.</span><span class="sxs-lookup"><span data-stu-id="d4c65-435">This feature lets your application treat requests as if they were coming from a different browser (user agent) than the one they are actually coming from.</span></span>

<span data-ttu-id="d4c65-436">In dieser Aufgabe untersuchen Sie die beispielimplementierung von einem ansichtumschaltung, indem jQuery.Mobile.MVC und dem neuen Browser Überschreiben von Funktionen in ASP.NET MVC 4 hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="d4c65-436">In this task, you will explore the sample implementation of a view-switcher added by jQuery.Mobile.MVC and the new browser overriding features in ASP.NET MVC 4.</span></span>

1. <span data-ttu-id="d4c65-437">Wechseln Sie zurück zu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d4c65-437">Switch back to Visual Studio.</span></span>
2. <span data-ttu-id="d4c65-438">Öffnen der  **\_Layout.Mobile.cshtml** Ansicht befindet sich unter der **Views\Shared** Ordner, und beachten Sie, dass der ansichtumschaltung Komponente als eine Teilansicht verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="d4c65-438">Open the **\_Layout.Mobile.cshtml** view located under the **Views\Shared** folder and notice the view-switcher component being referenced as a partial view.</span></span>

    <span data-ttu-id="d4c65-439">![Mobile Layouts mit den Ansichtumschaltung Komponente](whats-new-in-aspnet-mvc-4/_static/image29.png "Mobile Layouts mit den Ansichtumschaltung Komponente")</span><span class="sxs-lookup"><span data-stu-id="d4c65-439">![Mobile layout using View-Switcher component](whats-new-in-aspnet-mvc-4/_static/image29.png "Mobile layout using View-Switcher component")</span></span>

    <span data-ttu-id="d4c65-440">*Mobile Layouts mit den Ansichtumschaltung Komponente*</span><span class="sxs-lookup"><span data-stu-id="d4c65-440">*Mobile layout using View-Switcher component*</span></span>
3. <span data-ttu-id="d4c65-441">Öffnen der  **\_ViewSwitcher.cshtml** Teilansicht.</span><span class="sxs-lookup"><span data-stu-id="d4c65-441">Open the **\_ViewSwitcher.cshtml** partial view.</span></span>

    <span data-ttu-id="d4c65-442">Die Teilansicht verwendet die neue Methode **ViewContext.HttpContext.GetOverriddenBrowser()** zum Ermitteln des Ursprungs von der webanforderung und zeigen den entsprechenden Link, um entweder mit dem Desktop oder Mobile-Ansichten zu wechseln.</span><span class="sxs-lookup"><span data-stu-id="d4c65-442">The partial view uses the new method **ViewContext.HttpContext.GetOverriddenBrowser()** to determine the origin of the web request and show the corresponding link to switch either to the Desktop or Mobile views.</span></span>

    <span data-ttu-id="d4c65-443">Die **GetOverridenBrowser** Methode gibt ein **HttpBrowserCapabilitiesBase** -Instanz, die der Benutzer-Agent, die derzeit für die Anforderung festgelegt entspricht (tatsächlichen oder überschrieben).</span><span class="sxs-lookup"><span data-stu-id="d4c65-443">The **GetOverridenBrowser** method returns an **HttpBrowserCapabilitiesBase** instance that corresponds to the user agent currently set for the request (actual or overridden).</span></span> <span data-ttu-id="d4c65-444">Sie können diesen Wert verwenden, beim Abrufen von Eigenschaften wie z. B. **IsMobileDevice**.</span><span class="sxs-lookup"><span data-stu-id="d4c65-444">You can use this value to get properties such as **IsMobileDevice**.</span></span>

    <span data-ttu-id="d4c65-445">![Teilansicht ViewSwitcher](whats-new-in-aspnet-mvc-4/_static/image30.png "ViewSwitcher Teilansicht")</span><span class="sxs-lookup"><span data-stu-id="d4c65-445">![ViewSwitcher partial view](whats-new-in-aspnet-mvc-4/_static/image30.png "ViewSwitcher partial view")</span></span>

    <span data-ttu-id="d4c65-446">*Teilansicht ViewSwitcher*</span><span class="sxs-lookup"><span data-stu-id="d4c65-446">*ViewSwitcher partial view*</span></span>
4. <span data-ttu-id="d4c65-447">Öffnen der **ViewSwitcherController.cs** Klasse befindet sich in der **Controller** Ordner.</span><span class="sxs-lookup"><span data-stu-id="d4c65-447">Open the **ViewSwitcherController.cs** class located in the **Controllers** folder.</span></span> <span data-ttu-id="d4c65-448">Sehen Sie sich diese SwitchView Aktion wird aufgerufen, indem Sie den Link in der ViewSwitcher-Komponente, und beachten Sie die neuen HttpContext-Methoden.</span><span class="sxs-lookup"><span data-stu-id="d4c65-448">Check out that SwitchView action is called by the link in the ViewSwitcher component, and notice the new HttpContext methods.</span></span>

    - <span data-ttu-id="d4c65-449">Die **HttpContext.ClearOverridenBrowser()** -Methode entfernt alle außer Kraft gesetzten Benutzer-Agent für die aktuelle Anforderung.</span><span class="sxs-lookup"><span data-stu-id="d4c65-449">The **HttpContext.ClearOverridenBrowser()** method removes any overridden user agent for the current request.</span></span>
    - <span data-ttu-id="d4c65-450">Die **HttpContext.SetOverridenBrowser()** Methode überschreibt die Anforderung tatsächlichen Benutzer-Agent-Wert, der mit dem angegebenen Benutzer-Agent.</span><span class="sxs-lookup"><span data-stu-id="d4c65-450">The **HttpContext.SetOverridenBrowser()** method overrides the request's actual user agent value using the specified user agent.</span></span>  
        <span data-ttu-id="d4c65-451">![ViewSwitcher Controller](whats-new-in-aspnet-mvc-4/_static/image31.png "ViewSwitcher-Controller")</span><span class="sxs-lookup"><span data-stu-id="d4c65-451">![ViewSwitcher Controller](whats-new-in-aspnet-mvc-4/_static/image31.png "ViewSwitcher Controller")</span></span>  
<span data-ttu-id="d4c65-452">*ViewSwitcher-Controller*</span><span class="sxs-lookup"><span data-stu-id="d4c65-452">*ViewSwitcher Controller*</span></span>

        <span data-ttu-id="d4c65-453">Browser überschreiben ist eine grundlegende Funktion von ASP.NET MVC 4, das ist auch verfügbar, auch wenn Sie nicht das jQuery.Mobile.MVC-Paket installieren.</span><span class="sxs-lookup"><span data-stu-id="d4c65-453">Browser Overriding is a core feature of ASP.NET MVC 4, which is also available even if you do not install the jQuery.Mobile.MVC package.</span></span> <span data-ttu-id="d4c65-454">Allerdings diese Funktion wirkt sich auf nur anzeigen, Layout und Speicherortformate der Teilansicht, und er hat keine Auswirkungen auf eine der Funktionen, die vom Request.Browser Objekt abhängig sind.</span><span class="sxs-lookup"><span data-stu-id="d4c65-454">However, this feature affects only view, layout, and partial-view, and it does not affect any of the features that depend on the Request.Browser object.</span></span>

<a id="Task_5_-_Adding_the_View-Switcher_in_the_Desktop_View"></a>
#### <a name="task-5---adding-the-view-switcher-in-the-desktop-view"></a><span data-ttu-id="d4c65-455">Aufgabe 5: Hinzufügen der Ansichtumschaltung in der Desktopansicht</span><span class="sxs-lookup"><span data-stu-id="d4c65-455">Task 5 - Adding the View-Switcher in the Desktop View</span></span>

<span data-ttu-id="d4c65-456">In dieser Aufgabe aktualisieren Sie den desktop Layout zum ansichtumschaltung-enthalten.</span><span class="sxs-lookup"><span data-stu-id="d4c65-456">In this task, you will update the desktop layout to include the view-switcher.</span></span> <span data-ttu-id="d4c65-457">Dadurch können für mobile Benutzer, mobile Ansicht zurückzukehren, beim Durchsuchen von desktop anzeigen.</span><span class="sxs-lookup"><span data-stu-id="d4c65-457">This will allow mobile users to go back to the mobile view when browsing the desktop view.</span></span>

1. <span data-ttu-id="d4c65-458">Aktualisieren Sie den Standort in der **Windows Phone-Emulator**.</span><span class="sxs-lookup"><span data-stu-id="d4c65-458">Refresh the site in the **Windows Phone Emulator**.</span></span>
2. <span data-ttu-id="d4c65-459">Klicken Sie auf die **Desktopansicht** Link am Anfang des Katalogs.</span><span class="sxs-lookup"><span data-stu-id="d4c65-459">Click on the **Desktop view** link at the top of the gallery.</span></span> <span data-ttu-id="d4c65-460">Beachten Sie, dass keine ansichtumschaltung vorliegt, in der desktop können Sie zulassen, dass Sie an die mobile Ansicht zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="d4c65-460">Notice there is no view-switcher in the desktop view to allow you return to the mobile view.</span></span>
3. <span data-ttu-id="d4c65-461">Wechseln Sie zurück zu Visual Studio und Öffnen der  **\_Layout.cshtml** anzeigen.</span><span class="sxs-lookup"><span data-stu-id="d4c65-461">Go back to Visual Studio and open the **\_Layout.cshtml** view.</span></span>
4. <span data-ttu-id="d4c65-462">Suchen Sie den anmeldeabschnitt, und fügen Sie einen Aufruf zum Rendern der  **\_ViewSwitcher** Teilansicht unten die  **\_LogOnPartial** Teilansicht.</span><span class="sxs-lookup"><span data-stu-id="d4c65-462">Find the login section and insert a call to render the **\_ViewSwitcher** partial view below the **\_LogOnPartial** partial view.</span></span> <span data-ttu-id="d4c65-463">Drücken Sie anschließend die **STRG + S** um die Änderungen zu speichern.</span><span class="sxs-lookup"><span data-stu-id="d4c65-463">Then, press **CTRL + S** to save the changes.</span></span>


~~~
[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample14.cshtml)]
~~~
5. <span data-ttu-id="d4c65-464">Drücken Sie **STRG + S** um die Änderungen zu speichern.</span><span class="sxs-lookup"><span data-stu-id="d4c65-464">Press **CTRL + S** to save the changes.</span></span>
6. <span data-ttu-id="d4c65-465">Aktualisieren Sie die Seite in der Windows Phone-Emulator aus, und doppelklicken Sie auf dem Bildschirm, um zu vergrößern.</span><span class="sxs-lookup"><span data-stu-id="d4c65-465">Refresh the page in the Windows Phone Emulator and double-click the screen to zoom in.</span></span> <span data-ttu-id="d4c65-466">Beachten Sie, die auf der Startseite jetzt zeigt die **Mobile Ansicht** Link, der von mobilen in Desktopansicht wechselt.</span><span class="sxs-lookup"><span data-stu-id="d4c65-466">Notice that the home page now shows the **Mobile view** link that switches from mobile to desktop view.</span></span>

    <span data-ttu-id="d4c65-467">![Der Darstellung in der Desktopansicht gerendert](whats-new-in-aspnet-mvc-4/_static/image32.png "Ansichtumschaltung in desktop-Ansicht gerendert")</span><span class="sxs-lookup"><span data-stu-id="d4c65-467">![View Switcher rendered in desktop view](whats-new-in-aspnet-mvc-4/_static/image32.png "View Switcher rendered in desktop view")</span></span>

    <span data-ttu-id="d4c65-468">*In der Desktopansicht gerendert Ansichtumschaltung*</span><span class="sxs-lookup"><span data-stu-id="d4c65-468">*View Switcher rendered in desktop view*</span></span>
7. <span data-ttu-id="d4c65-469">Wechseln Sie zur mobilen Ansicht erneut aus, und navigieren Sie zu <strong>zu</strong> Seite (http://localhost[Port] / Home/zu).</span><span class="sxs-lookup"><span data-stu-id="d4c65-469">Switch to the Mobile view again and browse to <strong>About</strong> page (http://localhost[port]/Home/About).</span></span> <span data-ttu-id="d4c65-470">Beachten Sie, dass, selbst wenn Sie eine Ansicht About.Mobile.cshtml erstellt haben, mit der mobilen Layout der Seite "Info" angezeigt wird (\_Layout.Mobile.cshtml).</span><span class="sxs-lookup"><span data-stu-id="d4c65-470">Notice that, even if you haven't created an About.Mobile.cshtml view, the About page is displayed using the mobile layout (\_Layout.Mobile.cshtml).</span></span>

    <span data-ttu-id="d4c65-471">![Zu den Seiten](whats-new-in-aspnet-mvc-4/_static/image33.png "zu Seiten")</span><span class="sxs-lookup"><span data-stu-id="d4c65-471">![About page](whats-new-in-aspnet-mvc-4/_static/image33.png "About page")</span></span>

    <span data-ttu-id="d4c65-472">*Zu den Seiten*</span><span class="sxs-lookup"><span data-stu-id="d4c65-472">*About page*</span></span>
8. <span data-ttu-id="d4c65-473">Schließlich werden öffnen Sie die Website in einem desktop Webbrowser.</span><span class="sxs-lookup"><span data-stu-id="d4c65-473">Finally, open the site in a desktop Web browser.</span></span> <span data-ttu-id="d4c65-474">Beachten Sie, dass keine der vorherigen Updates der Desktopansicht betroffen ist.</span><span class="sxs-lookup"><span data-stu-id="d4c65-474">Notice that none of the previous updates has affected the desktop view.</span></span>

    <span data-ttu-id="d4c65-475">![PhotoGallery Desktopansicht](whats-new-in-aspnet-mvc-4/_static/image34.png "PhotoGallery Desktopansicht")</span><span class="sxs-lookup"><span data-stu-id="d4c65-475">![PhotoGallery desktop view](whats-new-in-aspnet-mvc-4/_static/image34.png "PhotoGallery desktop view")</span></span>

    <span data-ttu-id="d4c65-476">*PhotoGallery Desktopansicht*</span><span class="sxs-lookup"><span data-stu-id="d4c65-476">*PhotoGallery desktop view*</span></span>

<a id="Task_6_-_Creating_New_Display_Modes"></a>
#### <a name="task-6---creating-new-display-modes"></a><span data-ttu-id="d4c65-477">Aufgabe 6: Erstellen einer neuen Anzeigemodi</span><span class="sxs-lookup"><span data-stu-id="d4c65-477">Task 6 - Creating New Display Modes</span></span>

<span data-ttu-id="d4c65-478">Das neue Anzeige Modi Feature ermöglicht es sich um eine Anwendung, die Sichten je nach Browser auszuwählen, die die Anforderung generiert wird.</span><span class="sxs-lookup"><span data-stu-id="d4c65-478">The new display modes feature lets an application select views depending on the browser that is generating the request.</span></span> <span data-ttu-id="d4c65-479">Z. B. ein Desktopbrowser auf der Startseite "angefordert, die Anwendung zurück die **Views\Home\Index.cshtml** Vorlage.</span><span class="sxs-lookup"><span data-stu-id="d4c65-479">For example, if a desktop browser requests the Home page, the application will return the **Views\Home\Index.cshtml** template.</span></span> <span data-ttu-id="d4c65-480">Klicken Sie dann, wenn ein mobiler Browser auf der Startseite anfordert, die Anwendung zurück die **Views\Home\Index.mobile.cshtml** Vorlage.</span><span class="sxs-lookup"><span data-stu-id="d4c65-480">Then, if a mobile browser requests the Home page, the application will return the **Views\Home\Index.mobile.cshtml** template.</span></span>

<span data-ttu-id="d4c65-481">In dieser Aufgabe erstellen Sie ein benutzerdefiniertes Layout für iPhone-Geräte und müssen Anforderungen aus der iPhone-Geräte zu simulieren.</span><span class="sxs-lookup"><span data-stu-id="d4c65-481">In this task, you will create a customized layout for iPhone devices, and you will have to simulate requests from iPhone devices.</span></span> <span data-ttu-id="d4c65-482">Zu diesem Zweck können Sie entweder einen iPhone Emulator/Simulator (z. B. [Electric Mobile Simulator](http://www.electricplum.com/)) oder einen Browser mit Add-Ons anzuzeigen, die den Benutzer-Agent zu ändern.</span><span class="sxs-lookup"><span data-stu-id="d4c65-482">To do this, you can use either an iPhone emulator/simulator (like [Electric Mobile Simulator](http://www.electricplum.com/)) or a browser with add-ons that modify the user agent.</span></span> <span data-ttu-id="d4c65-483">Anweisungen zum Festlegen der Benutzer-Agent-Zeichenfolge in einem Safaribrowser auf einem iPhone zu emulieren, finden Sie unter [Vorgehensweise können Sie vorgeben, es handelt sich um Internet Explorer Safari](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) im Blog von David Alison des.</span><span class="sxs-lookup"><span data-stu-id="d4c65-483">For instructions on how to set the user agent string in an Safari browser to emulate an iPhone, see [How to let Safari pretend it's IE](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) in David Alison's blog.</span></span>

<span data-ttu-id="d4c65-484">**Beachten Sie, dass diese Aufgabe optional ist, und Sie im Labor fortsetzen können, ohne Sie auszuführen.**</span><span class="sxs-lookup"><span data-stu-id="d4c65-484">**Notice that this task is optional and you can continue throughout the lab without executing it.**</span></span>

1. <span data-ttu-id="d4c65-485">Drücken Sie in Visual Studio **UMSCHALT** + **F5** zum Debuggen der Anwendung zu beenden.</span><span class="sxs-lookup"><span data-stu-id="d4c65-485">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>
2. <span data-ttu-id="d4c65-486">Open **Global.asax.cs** und fügen Sie die folgende Anweisung verwenden.</span><span class="sxs-lookup"><span data-stu-id="d4c65-486">Open **Global.asax.cs** and add the following using statement.</span></span>


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample15.cs)]
~~~
3. <span data-ttu-id="d4c65-487">Fügen Sie den folgenden hervorgehobenen Code in die Anwendung\_Start-Methode.</span><span class="sxs-lookup"><span data-stu-id="d4c65-487">Add the following highlighted code into the Application\_Start method.</span></span>

    <span data-ttu-id="d4c65-488">(Codeausschnitt - *ASP.NET MVC 4-Lab - Ex03 - iPhone "DisplayMode"*)</span><span class="sxs-lookup"><span data-stu-id="d4c65-488">(Code Snippet - *ASP.NET MVC 4 Lab - Ex03 - iPhone DisplayMode*)</span></span>


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample16.cs)]

You have registered a new **DefaultDisplayMode named &quot;iPhone&quot;**, within the static **DisplayModeProvider.Instance.Modes** static list, that will be matched against each incoming request. If the incoming request contains the string &quot;iPhone&quot;, ASP.NET MVC will find the views whose name contain the &quot;iPhone&quot; suffix. The 0 parameter indicates how specific is the new mode; for instance, this view is more specific than the general &quot;.mobile&quot; rule that matches requests from mobile devices.

After this code runs, when an iPhone browser generates a request, your application will use the **Views\Shared\\_Layout.iPhone.cshtml** layout you will create in the next steps.

> [!NOTE]
> This way of testing the request for iPhone has been simplified for demo purposes and might not work as expected for every iPhone user agent string (for example test is case sensitive).
~~~
4. <span data-ttu-id="d4c65-489">Erstellen Sie eine Kopie der  **\_Layout.Mobile.cshtml** in der Datei die **Views\Shared** Ordner und benennen Sie die Kopie um &quot; **\_Layout.iPhone.csthml**&quot;.</span><span class="sxs-lookup"><span data-stu-id="d4c65-489">Create a copy of the **\_Layout.Mobile.cshtml** file in the **Views\Shared** folder and rename the copy to &quot;**\_Layout.iPhone.csthml**&quot;.</span></span>
5. <span data-ttu-id="d4c65-490">Open  **\_Layout.iPhone.csthml** Sie im vorherigen Schritt erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="d4c65-490">Open **\_Layout.iPhone.csthml** you created in the previous step.</span></span>
6. <span data-ttu-id="d4c65-491">Suchen Sie das Div-Element mit dem Data-Role-Attributsatz **Seite** , und ändern Sie die **Data-Theme** -Attribut auf &quot; **eine**&quot;.</span><span class="sxs-lookup"><span data-stu-id="d4c65-491">Find the div element with the data-role attribute set to **page** and change the **data-theme** attribute to &quot;**a**&quot;.</span></span>


~~~
[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample17.cshtml)]

Now you have 3 layouts in your ASP.NET MVC 4 application:

1. **\_Layout.cshtml**: default layout used for desktop browsers.
2. **\_Layout.mobile.cshtml**: default layout used for mobile devices.
3. **\_Layout.iPhone.cshtml**: specific layout for iPhone devices, using a different color scheme to differentiate from \_Layout.mobile.cshtml.
~~~
7. <span data-ttu-id="d4c65-492">Drücken Sie **F5** führen Sie die Anwendung, und suchen den Standort in der **Windows Phone-Emulator**.</span><span class="sxs-lookup"><span data-stu-id="d4c65-492">Press **F5** to run the application and browse the site in the **Windows Phone Emulator**.</span></span>
8. <span data-ttu-id="d4c65-493">Öffnen einer **iPhone-Simulator** (finden Sie unter [Anhang C](#AppendixC) Anweisungen zum Installieren und konfigurieren einen iPhone-Simulator), und wechseln Sie zur Website zu.</span><span class="sxs-lookup"><span data-stu-id="d4c65-493">Open an **iPhone simulator** (see [Appendix C](#AppendixC) for instructions on how to install and configure an iPhone simulator), and browse to the site too.</span></span> <span data-ttu-id="d4c65-494">Beachten Sie, dass die betreffende Vorlage jedes Telefon verwendet.</span><span class="sxs-lookup"><span data-stu-id="d4c65-494">Notice that each phone is using the specific template.</span></span>

    ![Using-different-views-for-each-mobile-device2](whats-new-in-aspnet-mvc-4/_static/image35.png)

    <span data-ttu-id="d4c65-496">*Verwenden verschiedene Ansichten für jedes mobile Gerät*</span><span class="sxs-lookup"><span data-stu-id="d4c65-496">*Using different views for each mobile device*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Using_Asynchronous_Controllers"></a>
### <a name="exercise-4-using-asynchronous-controllers"></a><span data-ttu-id="d4c65-497">Übung 4: Verwenden von asynchronen Controller</span><span class="sxs-lookup"><span data-stu-id="d4c65-497">Exercise 4: Using Asynchronous Controllers</span></span>

<span data-ttu-id="d4c65-498">Microsoft .NET Framework 4.5 führt neue Sprachfeatures in c# und Visual Basic um ein neues Fundament für Asynchronie in der .NET-Programmierung bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="d4c65-498">Microsoft .NET Framework 4.5 introduces new language features in C# and Visual Basic to provide a new foundation for asynchrony in .NET programming.</span></span> <span data-ttu-id="d4c65-499">Diese neue Foundation stellt asynchrone Programmierung, - ähnelt und ungefähr so einfach wie das - synchronen Programmierung.</span><span class="sxs-lookup"><span data-stu-id="d4c65-499">This new foundation makes asynchronous programming similar to - and about as straightforward as - synchronous programming.</span></span> <span data-ttu-id="d4c65-500">Sie können nun schreiben Sie asynchrone Aktionsmethoden in ASP.NET MVC 4 mithilfe der **AsyncController im Vergleich zum** Klasse.</span><span class="sxs-lookup"><span data-stu-id="d4c65-500">You are now able to write asynchronous action methods in ASP.NET MVC 4 by using the **AsyncController** class.</span></span> <span data-ttu-id="d4c65-501">Sie können asynchrone Aktionsmethoden für lang andauernde, nicht CPU-gebundene Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="d4c65-501">You can use asynchronous action methods for long-running, non-CPU bound requests.</span></span> <span data-ttu-id="d4c65-502">Dadurch wird vermieden, dass der Webserver aus arbeiten ausführen, während die Anforderung verarbeitet wird.</span><span class="sxs-lookup"><span data-stu-id="d4c65-502">This avoids blocking the Web server from performing work while the request is being processed.</span></span> <span data-ttu-id="d4c65-503">AsyncController im Vergleich zum Klasse dient normalerweise für lang andauernde Webdienstaufrufe.</span><span class="sxs-lookup"><span data-stu-id="d4c65-503">The AsyncController class is typically used for long-running Web service calls.</span></span>

<span data-ttu-id="d4c65-504">In dieser Übung erläutert die Grundlagen des asynchronen Vorgangs in ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="d4c65-504">This exercise explains the basics of asynchronous operation in ASP.NET MVC 4.</span></span> <span data-ttu-id="d4c65-505">Wenn Sie eine Vertiefung möchten, können Sie die folgenden Artikel auschecken: [[https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)</span><span class="sxs-lookup"><span data-stu-id="d4c65-505">If you want a deeper dive, you can check out the following article: [[https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)</span></span>

<a id="Task_1_-_Implementing_an_Asynchronous_Controller"></a>
#### <a name="task-1---implementing-an-asynchronous-controller"></a><span data-ttu-id="d4c65-506">Aufgabe 1: Implementieren eines asynchronen Controllers</span><span class="sxs-lookup"><span data-stu-id="d4c65-506">Task 1 - Implementing an Asynchronous Controller</span></span>

1. <span data-ttu-id="d4c65-507">Öffnen der **beginnen** Lösung finden Sie unter **Quell-/Ex4-Async/Begin/** Ordner.</span><span class="sxs-lookup"><span data-stu-id="d4c65-507">Open the **Begin** solution located at **Source/Ex4-Async/Begin/** folder.</span></span> <span data-ttu-id="d4c65-508">Andernfalls möglicherweise weiterhin mithilfe der **End** Lösung abgerufen, von der vorherigen Übung abschließen.</span><span class="sxs-lookup"><span data-stu-id="d4c65-508">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="d4c65-509">Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung müssen Sie einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren.</span><span class="sxs-lookup"><span data-stu-id="d4c65-509">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="d4c65-510">Klicken Sie hierzu auf die **Projekt** Menü **NuGet-Pakete verwalten**.</span><span class="sxs-lookup"><span data-stu-id="d4c65-510">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="d4c65-511">In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.</span><span class="sxs-lookup"><span data-stu-id="d4c65-511">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="d4c65-512">Schließlich erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.</span><span class="sxs-lookup"><span data-stu-id="d4c65-512">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="d4c65-513">Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken, die sich in Ihrem Projekt liefern Verringern der Projektgröße.</span><span class="sxs-lookup"><span data-stu-id="d4c65-513">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="d4c65-514">Mit NuGet-Powertools werden durch Angabe der Paketversionen in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, wenn, das Sie das Projekt ausführen, können.</span><span class="sxs-lookup"><span data-stu-id="d4c65-514">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="d4c65-515">Deshalb wird müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit.</span><span class="sxs-lookup"><span data-stu-id="d4c65-515">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="d4c65-516">Öffnen der **HomeController.cs** -Klasse aus den **Controller** Ordner.</span><span class="sxs-lookup"><span data-stu-id="d4c65-516">Open the **HomeController.cs** class from the **Controllers** folder.</span></span>
3. <span data-ttu-id="d4c65-517">Fügen Sie die folgende Anweisung verwenden.</span><span class="sxs-lookup"><span data-stu-id="d4c65-517">Add the following using statement.</span></span>


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample18.cs)]
~~~
4. <span data-ttu-id="d4c65-518">Update der **HomeController** Klasse zu vererben **AsyncController im Vergleich zum**.</span><span class="sxs-lookup"><span data-stu-id="d4c65-518">Update the **HomeController** class to inherit from **AsyncController**.</span></span> <span data-ttu-id="d4c65-519">Domänencontroller, auf denen Ableitung AsyncController im Vergleich zum Aktivieren von ASP.NET zur Verarbeitung von asynchronen Anforderungen, und sie können weiterhin Dienst synchrone Aktionsmethoden.</span><span class="sxs-lookup"><span data-stu-id="d4c65-519">Controllers that derive from AsyncController enable ASP.NET to process asynchronous requests, and they can still service synchronous action methods.</span></span>


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample19.cs)]
~~~
5. <span data-ttu-id="d4c65-520">Hinzufügen der **Async** Schlüsselwort, um die **Index** Methode, und stellen sie den Rückgabetyp **Aufgabe&lt;ActionResult&gt;**.</span><span class="sxs-lookup"><span data-stu-id="d4c65-520">Add the **async** keyword to the **Index** method and make it return the type **Task&lt;ActionResult&gt;**.</span></span>


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample20.cs)]

> [!NOTE]
> The **async** keyword is one of the new keywords the .NET Framework 4.5 provides; it tells the compiler that this method contains asynchronous code. A **Task** object represents an asynchronous operation that may complete at some point in the future.
~~~
6. <span data-ttu-id="d4c65-521">Ersetzen Sie die **Client. GetAsync()** Aufruf mit der vollständigen asynchronen Version "await" Schlüsselwort aus, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="d4c65-521">Replace the **client.GetAsync()** call with the full async version using await keyword as shown below.</span></span>

    <span data-ttu-id="d4c65-522">(Codeausschnitt - *ASP.NET MVC 4 Lab - Ex04 - GetAsync*)</span><span class="sxs-lookup"><span data-stu-id="d4c65-522">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - GetAsync*)</span></span>


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample21.cs)]

> [!NOTE]
> In the previous version, you were using the **Result** property from the **Task** object to block the thread until the result is returned (sync version).
> 
> Adding the **await** keyword tells the compiler to asynchronously wait for the task returned from the method call. This means that the rest of the code will be executed as a callback only after the awaited method completes. Another thing to notice is that you do not need to change your try-catch block in order to make this work: the exceptions that happen in background or in foreground will still be caught without any extra work using a handler provided by the framework.
~~~
7. <span data-ttu-id="d4c65-523">Ändern Sie den Code auf die asynchrone Implementierung fortsetzen, durch die Zeilen mit den neuen Code ersetzen, wie unten dargestellt</span><span class="sxs-lookup"><span data-stu-id="d4c65-523">Change the code to continue with the asynchronous implementation by replacing the lines with the new code as shown below</span></span>

    <span data-ttu-id="d4c65-524">(Codeausschnitt - *ASP.NET MVC 4 Lab - Ex04 - ReadAsStringAsync*)</span><span class="sxs-lookup"><span data-stu-id="d4c65-524">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - ReadAsStringAsync*)</span></span>


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample22.cs)]
~~~
8. <span data-ttu-id="d4c65-525">Führen Sie die Anwendung aus.</span><span class="sxs-lookup"><span data-stu-id="d4c65-525">Run the application.</span></span> <span data-ttu-id="d4c65-526">Sehen Sie keine größeren Änderungen, aber der Code wird nicht blockiert einen Thread aus dem Threadpool, erstellen eine bessere Nutzung von Serverressourcen und Verbessern der Leistung.</span><span class="sxs-lookup"><span data-stu-id="d4c65-526">You will notice no major changes, but your code will not block a thread from the thread pool making a better usage of the server resources and improving performance.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d4c65-527">Erfahren Sie mehr über die neuen Funktionen der asynchronen Programmierung im Labor &quot; **asynchrone Programmierung in .NET 4.5 mit c# und Visual Basic** &quot; in der Visual Studio-Training Kit enthalten.</span><span class="sxs-lookup"><span data-stu-id="d4c65-527">You can learn more about the new asynchronous programming features in the lab &quot;**Asynchronous Programming in .NET 4.5 with C# and Visual Basic**&quot; included in the Visual Studio Training Kit.</span></span>

<a id="Task_2_-_Handling_Time-Outs_with_Cancellation_Tokens"></a>
#### <a name="task-2---handling-time-outs-with-cancellation-tokens"></a><span data-ttu-id="d4c65-528">Aufgabe 2: Behandeln von Timeouts mit Abbruchtoken</span><span class="sxs-lookup"><span data-stu-id="d4c65-528">Task 2 - Handling Time-Outs with Cancellation Tokens</span></span>

<span data-ttu-id="d4c65-529">Asynchrone Aktionsmethoden, die Aufgabeninstanzen zurückgeben können auch Timeouts unterstützen.</span><span class="sxs-lookup"><span data-stu-id="d4c65-529">Asynchronous action methods that return Task instances can also support time-outs.</span></span> <span data-ttu-id="d4c65-530">In dieser Aufgabe aktualisieren Sie die Index-Methodencode eine Timeout-Szenario, das Verwenden eines Abbruchtokens zu behandeln.</span><span class="sxs-lookup"><span data-stu-id="d4c65-530">In this task, you will update the Index method code to handle a time-out scenario using a cancellation token.</span></span>

1. <span data-ttu-id="d4c65-531">Wechseln Sie zurück zu Visual Studio, und drücken Sie **UMSCHALT + F5** debugging beenden.</span><span class="sxs-lookup"><span data-stu-id="d4c65-531">Go back to Visual Studio and press **SHIFT + F5** to stop debugging.</span></span>
2. <span data-ttu-id="d4c65-532">Fügen Sie die folgenden using-Anweisungen die **HomeController.cs** Datei.</span><span class="sxs-lookup"><span data-stu-id="d4c65-532">Add the following using statement to the **HomeController.cs** file.</span></span>


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample23.cs)]
~~~
3. <span data-ttu-id="d4c65-533">Aktualisieren Sie die Index-Aktion empfangen einer **CancellationToken** Argument.</span><span class="sxs-lookup"><span data-stu-id="d4c65-533">Update the Index action to receive a **CancellationToken** argument.</span></span>


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample24.cs)]
~~~
4. <span data-ttu-id="d4c65-534">Update der **GetAsync** Aufruf an das Abbruchtoken übergeben.</span><span class="sxs-lookup"><span data-stu-id="d4c65-534">Update the **GetAsync** call to pass the cancellation token.</span></span>

    <span data-ttu-id="d4c65-535">(Codeausschnitt - *ASP.NET MVC 4-Lab - Ex04 - "SendAsync" mit CancellationToken*)</span><span class="sxs-lookup"><span data-stu-id="d4c65-535">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - SendAsync with CancellationToken*)</span></span>


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample25.cs)]
~~~
5. <span data-ttu-id="d4c65-536">Ergänzen der *Index* Methode mit einer **AsyncTimeout** -Attribut auf 500 Millisekunden festgelegt und ein **HandleError** Attribut zur Verarbeitung konfiguriert  **TaskCanceledException** durch Umleiten an einen **"timedOut"** anzeigen.</span><span class="sxs-lookup"><span data-stu-id="d4c65-536">Decorate the *Index* method with an **AsyncTimeout** attribute set to 500 milliseconds and a **HandleError** attribute configured to handle **TaskCanceledException** by redirecting to a **TimedOut** view.</span></span>

    <span data-ttu-id="d4c65-537">(Codeausschnitt - *ASP.NET MVC 4 Lab - Ex04 - Attribute*)</span><span class="sxs-lookup"><span data-stu-id="d4c65-537">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - Attributes*)</span></span>


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample26.cs)]
~~~
6. <span data-ttu-id="d4c65-538">Öffnen der **PhotoController** Klasse und Aktualisieren der **Katalog** Methode, um die Ausführung von 1000 Millisekunden (1 Sekunde) zum Simulieren einer lang ausgeführten Aufgabe zu verzögern.</span><span class="sxs-lookup"><span data-stu-id="d4c65-538">Open the **PhotoController** class and update the **Gallery** method to delay the execution 1000 miliseconds (1 second) to simulate a long running task.</span></span>


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample27.cs)]
~~~
7. <span data-ttu-id="d4c65-539">Öffnen der **"Web.config"** Datei, und aktivieren Sie benutzerdefinierte Fehler, indem Sie das folgende Element hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="d4c65-539">Open the **Web.config** file and enable custom errors by adding the following element.</span></span>


~~~
[!code-xml[Main](whats-new-in-aspnet-mvc-4/samples/sample28.xml)]
~~~
8. <span data-ttu-id="d4c65-540">Erstellen einer neuen Ansicht in **Views\Shared** mit dem Namen **"timedOut"** und verwenden Sie das Standardlayout.</span><span class="sxs-lookup"><span data-stu-id="d4c65-540">Create a new view in **Views\Shared** named **TimedOut** and use the default layout.</span></span> <span data-ttu-id="d4c65-541">Klicken Sie im Projektmappen-Explorer mit der Maustaste die **Views\Shared** Ordner, und wählen **hinzufügen | Ansicht**.</span><span class="sxs-lookup"><span data-stu-id="d4c65-541">In the Solution Explorer, right-click the **Views\Shared** folder and select **Add | View**.</span></span>

    <span data-ttu-id="d4c65-542">![Verwenden verschiedene Ansichten für jedes mobile Gerät](whats-new-in-aspnet-mvc-4/_static/image36.png "verschiedene Ansichten für jedes mobile Gerät verwenden.")</span><span class="sxs-lookup"><span data-stu-id="d4c65-542">![Using different views for each mobile device](whats-new-in-aspnet-mvc-4/_static/image36.png "Using different views for each mobile device")</span></span>

    <span data-ttu-id="d4c65-543">*Verwenden verschiedene Ansichten für jedes mobile Gerät*</span><span class="sxs-lookup"><span data-stu-id="d4c65-543">*Using different views for each mobile device*</span></span>
9. <span data-ttu-id="d4c65-544">Update der **"timedOut"** Inhalte anzuzeigen, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="d4c65-544">Update the **TimedOut** view content as shown below.</span></span>


~~~
[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample29.cshtml)]
~~~
10. <span data-ttu-id="d4c65-545">Führen Sie die Anwendung, und suchen Sie die Stamm-URL.</span><span class="sxs-lookup"><span data-stu-id="d4c65-545">Run the application and navigate to the root URL.</span></span> <span data-ttu-id="d4c65-546">Wie Sie hinzugefügt haben eine **Thread.Sleep** von 1000 Millisekunden, erhalten Sie ein Timeoutfehler von generiert die **AsyncTimeout** Attribut, und abfängt, indem Sie die **HandleError** Das Attribut.</span><span class="sxs-lookup"><span data-stu-id="d4c65-546">As you have added a **Thread.Sleep** of 1000 milliseconds, you will get a time-out error, generated by the **AsyncTimeout** attribute and catch by the **HandleError** attribute.</span></span>

    <span data-ttu-id="d4c65-547">![Timeout-Ausnahme behandelt](whats-new-in-aspnet-mvc-4/_static/image37.png "Timeoutausnahme behandelt")</span><span class="sxs-lookup"><span data-stu-id="d4c65-547">![Time-out exception handled](whats-new-in-aspnet-mvc-4/_static/image37.png "Time-out exception handled")</span></span>

    <span data-ttu-id="d4c65-548">*Timeoutausnahme behandelt wird*</span><span class="sxs-lookup"><span data-stu-id="d4c65-548">*Time-out exception handled*</span></span>

> [!NOTE]
> <span data-ttu-id="d4c65-549">Darüber hinaus können Sie die Bereitstellung dieser Anwendung, die Windows Azure-Websites folgenden [Anhang D: Veröffentlichen einer ASP.NET MVC 4-Anwendung mithilfe von Web Deploy](#AppendixD).</span><span class="sxs-lookup"><span data-stu-id="d4c65-549">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix D: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixD).</span></span>


<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="d4c65-550">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="d4c65-550">Summary</span></span>

<span data-ttu-id="d4c65-551">In dieser praktische Übung-on-Übungseinheit haben Sie einige der neuen Funktionen in ASP.NET MVC 4 residenten beobachtet.</span><span class="sxs-lookup"><span data-stu-id="d4c65-551">In this hands-on-lab, you've observed some of the new features resident in ASP.NET MVC 4.</span></span> <span data-ttu-id="d4c65-552">Die folgenden Konzepte haben behandelt wurde:</span><span class="sxs-lookup"><span data-stu-id="d4c65-552">The following concepts have been discussed:</span></span>

- <span data-ttu-id="d4c65-553">Nutzen Sie die Erweiterungen der für die ASP.NET MVC-Projekt Vorlagen, einschließlich der neue Projektvorlage für die mobile Anwendung</span><span class="sxs-lookup"><span data-stu-id="d4c65-553">Take advantage of the enhancements to the ASP.NET MVC project templates-including the new mobile application project template</span></span>
- <span data-ttu-id="d4c65-554">Verwenden Sie das HTML5 Viewport-Attribut und der CSS-Medienabfragen, um die Anzeige auf mobilen Geräten zu verbessern</span><span class="sxs-lookup"><span data-stu-id="d4c65-554">Use the HTML5 viewport attribute and CSS media queries to improve the display on mobile devices</span></span>
- <span data-ttu-id="d4c65-555">Verwenden Sie für progressiven Verbesserungen und zum Erstellen von Web-Touch-optimierte Benutzeroberfläche jQuery Mobile</span><span class="sxs-lookup"><span data-stu-id="d4c65-555">Use jQuery Mobile for progressive enhancements and for building touch-optimized web UI</span></span>
- <span data-ttu-id="d4c65-556">Mobile-spezifischen Ansichten erstellen</span><span class="sxs-lookup"><span data-stu-id="d4c65-556">Create mobile-specific views</span></span>
- <span data-ttu-id="d4c65-557">Umschalten zwischen mobilen und desktop-Ansichten in der Anwendung mithilfe der ansichtumschaltung Komponente</span><span class="sxs-lookup"><span data-stu-id="d4c65-557">Use the view-switcher component to toggle between mobile and desktop views in the application</span></span>
- <span data-ttu-id="d4c65-558">Erstellen von asynchronen Controller Dank Unterstützung der Aufgabe</span><span class="sxs-lookup"><span data-stu-id="d4c65-558">Create asynchronous controllers using task support</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a><span data-ttu-id="d4c65-559">Anhang A: Verwenden von Codeausschnitten</span><span class="sxs-lookup"><span data-stu-id="d4c65-559">Appendix A: Using Code Snippets</span></span>

<span data-ttu-id="d4c65-560">Mit Codeausschnitten müssen Sie den Code, den Sie jederzeit griffbereit benötigen.</span><span class="sxs-lookup"><span data-stu-id="d4c65-560">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="d4c65-561">Das Lab-Dokument erfahren Sie, wenn Sie genau, verwenden können wie in der folgenden Abbildung dargestellt.</span><span class="sxs-lookup"><span data-stu-id="d4c65-561">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="d4c65-562">![Verwendung von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihr Projekt](whats-new-in-aspnet-mvc-4/_static/image38.png "Codeausschnitte zum Einfügen von Code in Ihr Projekt mit Visual Studio")</span><span class="sxs-lookup"><span data-stu-id="d4c65-562">![Using Visual Studio code snippets to insert code into your project](whats-new-in-aspnet-mvc-4/_static/image38.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="d4c65-563">*Verwendung von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihr Projekt*</span><span class="sxs-lookup"><span data-stu-id="d4c65-563">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="d4c65-564">***Hinzufügen ein Codeausschnitts, die mithilfe der Tastatur (nur c#)***</span><span class="sxs-lookup"><span data-stu-id="d4c65-564">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="d4c65-565">Platzieren Sie den Cursor, wo möchten Sie den Code einfügen.</span><span class="sxs-lookup"><span data-stu-id="d4c65-565">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="d4c65-566">Starten Sie den Namen des Ausschnitts (ohne Leerzeichen oder Bindestriche) eingeben.</span><span class="sxs-lookup"><span data-stu-id="d4c65-566">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="d4c65-567">Beobachten Sie, wie IntelliSense zeigt übereinstimmenden Namen Ausschnitte.</span><span class="sxs-lookup"><span data-stu-id="d4c65-567">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="d4c65-568">Wählen Sie den richtigen Ausschnitt (oder behalten Sie eingeben, bis der gesamte Ausschnitt Name ausgewählt ist).</span><span class="sxs-lookup"><span data-stu-id="d4c65-568">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="d4c65-569">Drücken Sie die Tab-Taste zweimal, um den Ausschnitt an der Cursorposition eingefügt.</span><span class="sxs-lookup"><span data-stu-id="d4c65-569">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="d4c65-570">![Geben Sie den Namen des Ausschnitts](whats-new-in-aspnet-mvc-4/_static/image39.png "starten Sie den Namen des Ausschnitts eingeben")</span><span class="sxs-lookup"><span data-stu-id="d4c65-570">![Start typing the snippet name](whats-new-in-aspnet-mvc-4/_static/image39.png "Start typing the snippet name")</span></span>

<span data-ttu-id="d4c65-571">*Starten Sie den Namen des Ausschnitts eingeben*</span><span class="sxs-lookup"><span data-stu-id="d4c65-571">*Start typing the snippet name*</span></span>

<span data-ttu-id="d4c65-572">![Drücken Sie Tab, auf den hervorgehobenen Ausschnitt](whats-new-in-aspnet-mvc-4/_static/image40.png "drücken Sie die Tab hervorgehobenen Ausschnitt auswählen")</span><span class="sxs-lookup"><span data-stu-id="d4c65-572">![Press Tab to select the highlighted snippet](whats-new-in-aspnet-mvc-4/_static/image40.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="d4c65-573">*Drücken Sie Tab, um den markierten Ausschnitt auszuwählen*</span><span class="sxs-lookup"><span data-stu-id="d4c65-573">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="d4c65-574">![Drücken Sie erneut die Tab und den Ausschnitt erweitert](whats-new-in-aspnet-mvc-4/_static/image41.png "drücken Sie erneut die Tab und den Ausschnitt erweitert")</span><span class="sxs-lookup"><span data-stu-id="d4c65-574">![Press Tab again and the snippet will expand](whats-new-in-aspnet-mvc-4/_static/image41.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="d4c65-575">*Drücken Sie erneut die Tab und den Ausschnitt erweitert*</span><span class="sxs-lookup"><span data-stu-id="d4c65-575">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="d4c65-576">***Hinzufügen ein Codeausschnitts, die mit der Maus (c#, Visual Basic und XML)***</span><span class="sxs-lookup"><span data-stu-id="d4c65-576">***To add a code snippet using the mouse (C#, Visual Basic and XML)***</span></span>

1. <span data-ttu-id="d4c65-577">Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen möchten.</span><span class="sxs-lookup"><span data-stu-id="d4c65-577">Right-click where you want to insert the code snippet.</span></span>
2. <span data-ttu-id="d4c65-578">Wählen Sie **Ausschnitt einfügen** gefolgt von **eigene Codeausschnitte**.</span><span class="sxs-lookup"><span data-stu-id="d4c65-578">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
3. <span data-ttu-id="d4c65-579">Wählen Sie die relevanten Ausschnitt aus der Liste, indem Sie darauf klicken.</span><span class="sxs-lookup"><span data-stu-id="d4c65-579">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="d4c65-580">![Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten](whats-new-in-aspnet-mvc-4/_static/image42.png "mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten,")</span><span class="sxs-lookup"><span data-stu-id="d4c65-580">![Right-click where you want to insert the code snippet and select Insert Snippet](whats-new-in-aspnet-mvc-4/_static/image42.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="d4c65-581">*Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten*</span><span class="sxs-lookup"><span data-stu-id="d4c65-581">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="d4c65-582">![Wählen Sie den relevanten Ausschnitt aus der Liste, indem Sie darauf klicken](whats-new-in-aspnet-mvc-4/_static/image43.png "den relevanten Ausschnitt aus der Liste auswählen, indem Sie darauf klicken")</span><span class="sxs-lookup"><span data-stu-id="d4c65-582">![Pick the relevant snippet from the list, by clicking on it](whats-new-in-aspnet-mvc-4/_static/image43.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="d4c65-583">*Wählen Sie den relevanten Ausschnitt aus der Liste, indem Sie darauf klicken*</span><span class="sxs-lookup"><span data-stu-id="d4c65-583">*Pick the relevant snippet from the list, by clicking on it*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="d4c65-584">Anhang B: Installieren von Visual Studio Express 2012 für das Web</span><span class="sxs-lookup"><span data-stu-id="d4c65-584">Appendix B: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="d4c65-585">Sie installieren können **Microsoft Visual Studio Express 2012 für das Web** oder ein anderes &quot;Express&quot; Version mithilfe der **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="d4c65-585">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="d4c65-586">Die folgenden Anweisungen führen Sie durch die erforderlichen Schritte zum Installieren *Visual Studio Express 2012 für das Web* mit *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="d4c65-586">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="d4c65-587">Wechseln Sie zu [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="d4c65-587">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="d4c65-588">Auch wenn Sie bereits Webplattform-Installer installiert haben, können Sie öffnen es, und suchen Sie nach dem Produkt &quot; <em>Visual Studio Express 2012 für das Web mit Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="d4c65-588">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="d4c65-589">Klicken Sie auf **jetzt installieren**.</span><span class="sxs-lookup"><span data-stu-id="d4c65-589">Click on **Install Now**.</span></span> <span data-ttu-id="d4c65-590">Wenn Sie keine **Webplattform-Installer** Sie Informationen zum Herunterladen und installieren Sie diese zuerst umgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="d4c65-590">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="d4c65-591">Einmal **Webplattform-Installer** geöffnet ist, klicken Sie auf **installieren** um das Setup zu starten.</span><span class="sxs-lookup"><span data-stu-id="d4c65-591">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="d4c65-592">![Visual Studio Express installieren](whats-new-in-aspnet-mvc-4/_static/image44.png "Visual Studio Express installieren")</span><span class="sxs-lookup"><span data-stu-id="d4c65-592">![Install Visual Studio Express](whats-new-in-aspnet-mvc-4/_static/image44.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="d4c65-593">*Installieren Sie Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="d4c65-593">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="d4c65-594">Lesen Sie die Produkte, Lizenzen und Begriffe, und klicken Sie auf **ich stimme** um den Vorgang fortzusetzen.</span><span class="sxs-lookup"><span data-stu-id="d4c65-594">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Akzeptieren der Lizenzbedingungen](whats-new-in-aspnet-mvc-4/_static/image45.png)

    <span data-ttu-id="d4c65-596">*Akzeptieren der Lizenzbedingungen*</span><span class="sxs-lookup"><span data-stu-id="d4c65-596">*Accepting the license terms*</span></span>
5. <span data-ttu-id="d4c65-597">Warten Sie, bis der Prozess herunterladen und die Installation abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="d4c65-597">Wait until the downloading and installation process completes.</span></span>

    ![Installationsstatus](whats-new-in-aspnet-mvc-4/_static/image46.png)

    <span data-ttu-id="d4c65-599">*Installationsstatus*</span><span class="sxs-lookup"><span data-stu-id="d4c65-599">*Installation progress*</span></span>
6. <span data-ttu-id="d4c65-600">Nach Abschluss der Installation klicken Sie auf **Fertig stellen**.</span><span class="sxs-lookup"><span data-stu-id="d4c65-600">When the installation completes, click **Finish**.</span></span>

    ![Installation wurde abgeschlossen](whats-new-in-aspnet-mvc-4/_static/image47.png)

    <span data-ttu-id="d4c65-602">*Installation wurde abgeschlossen*</span><span class="sxs-lookup"><span data-stu-id="d4c65-602">*Installation completed*</span></span>
7. <span data-ttu-id="d4c65-603">Klicken Sie auf **beenden** Webplattform-Installer zu schließen.</span><span class="sxs-lookup"><span data-stu-id="d4c65-603">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="d4c65-604">Um Visual Studio Express für Web zu öffnen, wechseln Sie zu der **starten** Startseite ein, und starten Sie das Schreiben von &quot; **Visual Studio Express**&quot;, klicken Sie dann auf die **Visual Studio Express für Web** Kachel.</span><span class="sxs-lookup"><span data-stu-id="d4c65-604">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Visual Studio Express für Web-Kachel](whats-new-in-aspnet-mvc-4/_static/image48.png)

    <span data-ttu-id="d4c65-606">*Visual Studio Express für Web-Kachel*</span><span class="sxs-lookup"><span data-stu-id="d4c65-606">*VS Express for Web tile*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Installing_WebMatrix_2_and_iPhone_Simulator"></a>
## <a name="appendix-c-installing-webmatrix-2-and-iphone-simulator"></a><span data-ttu-id="d4c65-607">Anhang C: Installieren von WebMatrix 2 und in der iPhone-Simulator</span><span class="sxs-lookup"><span data-stu-id="d4c65-607">Appendix C: Installing WebMatrix 2 and iPhone Simulator</span></span>

<span data-ttu-id="d4c65-608">Um Ihre Website in einem simulierten iPhone-Gerät ausführen können Sie die WebMatrix-Erweiterung &quot;Electric Mobile-Simulator für das iPhone&quot;.</span><span class="sxs-lookup"><span data-stu-id="d4c65-608">To run your site in a simulated iPhone device you can use the WebMatrix extension &quot;Electric Mobile Simulator for the iPhone&quot;.</span></span> <span data-ttu-id="d4c65-609">Darüber hinaus können Sie die gleiche Erweiterung zum Ausführen des Simulators aus Visual Studio 2012 konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="d4c65-609">Also, you can configure the same extension to run the simulator from Visual Studio 2012.</span></span>

<a id="ApxCTask1"></a>

<a id="Task_1_-_Installing_WebMatrix_2"></a>
#### <a name="task-1---installing-webmatrix-2"></a><span data-ttu-id="d4c65-610">Aufgabe 1: Installieren von WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="d4c65-610">Task 1 - Installing WebMatrix 2</span></span>

1. <span data-ttu-id="d4c65-611">Wechseln Sie zu [ [ https://go.microsoft.com/?linkid=9809776 ](https://go.microsoft.com/?linkid=9809776) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="d4c65-611">Go to [[https://go.microsoft.com/?linkid=9809776](https://go.microsoft.com/?linkid=9809776)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="d4c65-612">Auch wenn Sie bereits Webplattform-Installer installiert haben, können Sie öffnen es, und suchen Sie nach dem Produkt &quot; <em>WebMatrix 2</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="d4c65-612">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>WebMatrix 2</em>&quot;.</span></span>
2. <span data-ttu-id="d4c65-613">Klicken Sie auf **jetzt installieren**.</span><span class="sxs-lookup"><span data-stu-id="d4c65-613">Click on **Install Now**.</span></span> <span data-ttu-id="d4c65-614">Wenn Sie keine **Webplattform-Installer** Sie Informationen zum Herunterladen und installieren Sie diese zuerst umgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="d4c65-614">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="d4c65-615">Einmal **Webplattform-Installer** geöffnet ist, klicken Sie auf **installieren** um das Setup zu starten.</span><span class="sxs-lookup"><span data-stu-id="d4c65-615">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="d4c65-616">![Installieren von WebMatrix 2](whats-new-in-aspnet-mvc-4/_static/image49.png "WebMatrix 2 installieren")</span><span class="sxs-lookup"><span data-stu-id="d4c65-616">![Install WebMatrix 2](whats-new-in-aspnet-mvc-4/_static/image49.png "Install WebMatrix 2")</span></span>

    <span data-ttu-id="d4c65-617">*Installieren von WebMatrix 2*</span><span class="sxs-lookup"><span data-stu-id="d4c65-617">*Install WebMatrix 2*</span></span>
4. <span data-ttu-id="d4c65-618">Lesen Sie die Produkte, Lizenzen und Begriffe, und klicken Sie auf **ich stimme** um den Vorgang fortzusetzen.</span><span class="sxs-lookup"><span data-stu-id="d4c65-618">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    <span data-ttu-id="d4c65-619">![Akzeptieren der Lizenzbedingungen](whats-new-in-aspnet-mvc-4/_static/image50.png "akzeptieren der Lizenzbedingungen")</span><span class="sxs-lookup"><span data-stu-id="d4c65-619">![Accepting the license terms](whats-new-in-aspnet-mvc-4/_static/image50.png "Accepting the license terms")</span></span>

    <span data-ttu-id="d4c65-620">*Akzeptieren der Lizenzbedingungen*</span><span class="sxs-lookup"><span data-stu-id="d4c65-620">*Accepting the license terms*</span></span>
5. <span data-ttu-id="d4c65-621">Warten Sie, bis der Prozess herunterladen und die Installation abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="d4c65-621">Wait until the downloading and installation process completes.</span></span>

    <span data-ttu-id="d4c65-622">![Installationsstatus](whats-new-in-aspnet-mvc-4/_static/image51.png "Installationsstatus")</span><span class="sxs-lookup"><span data-stu-id="d4c65-622">![Installation progress](whats-new-in-aspnet-mvc-4/_static/image51.png "Installation progress")</span></span>

    <span data-ttu-id="d4c65-623">*Installationsstatus*</span><span class="sxs-lookup"><span data-stu-id="d4c65-623">*Installation progress*</span></span>
6. <span data-ttu-id="d4c65-624">Nach Abschluss der Installation klicken Sie auf **Fertig stellen**.</span><span class="sxs-lookup"><span data-stu-id="d4c65-624">When the installation completes, click **Finish**.</span></span>

    <span data-ttu-id="d4c65-625">![Installation wurde abgeschlossen](whats-new-in-aspnet-mvc-4/_static/image52.png "Installation abgeschlossen")</span><span class="sxs-lookup"><span data-stu-id="d4c65-625">![Installation completed](whats-new-in-aspnet-mvc-4/_static/image52.png "Installation completed")</span></span>

    <span data-ttu-id="d4c65-626">*Installation wurde abgeschlossen*</span><span class="sxs-lookup"><span data-stu-id="d4c65-626">*Installation completed*</span></span>
7. <span data-ttu-id="d4c65-627">Klicken Sie auf **beenden** Webplattform-Installer zu schließen.</span><span class="sxs-lookup"><span data-stu-id="d4c65-627">Click **Exit** to close Web Platform Installer.</span></span>

<a id="ApxCTask2"></a>

<a id="Task_2_-_Installing_the_iPhone_Simulator_Extension"></a>
#### <a name="task-2---installing-the-iphone-simulator-extension"></a><span data-ttu-id="d4c65-628">Aufgabe 2: installieren die iPhone-Simulator-Erweiterung</span><span class="sxs-lookup"><span data-stu-id="d4c65-628">Task 2 - Installing the iPhone Simulator Extension</span></span>

1. <span data-ttu-id="d4c65-629">Führen Sie **WebMatrix** und ggf. eine vorhandene Website öffnen oder erstellen Sie ein neues Konto.</span><span class="sxs-lookup"><span data-stu-id="d4c65-629">Run **WebMatrix** and open any existing Web site or create a new one.</span></span>
2. <span data-ttu-id="d4c65-630">Klicken Sie auf die **ausführen** Schaltfläche aus der **Startseite** , und wählen Sie im Menüband **Add new**.</span><span class="sxs-lookup"><span data-stu-id="d4c65-630">Click the **Run** button from the **Home** ribbon and select **Add new**.</span></span>

    <span data-ttu-id="d4c65-631">![Hinzufügen von neuen WebMatrix-Erweiterung](whats-new-in-aspnet-mvc-4/_static/image53.png "neue WebMatrix-Erweiterung hinzufügen")</span><span class="sxs-lookup"><span data-stu-id="d4c65-631">![Adding new WebMatrix extension](whats-new-in-aspnet-mvc-4/_static/image53.png "Adding new WebMatrix extension")</span></span>

    <span data-ttu-id="d4c65-632">*Hinzufügen von neuen WebMatrix-Erweiterung*</span><span class="sxs-lookup"><span data-stu-id="d4c65-632">*Adding new WebMatrix extension*</span></span>
3. <span data-ttu-id="d4c65-633">Wählen Sie **iPhone-Simulator** , und klicken Sie auf **installieren**.</span><span class="sxs-lookup"><span data-stu-id="d4c65-633">Select **iPhone Simulator** and click **Install**.</span></span>

    <span data-ttu-id="d4c65-634">![Durchsuchen von WebMatrix Erweiterungen](whats-new-in-aspnet-mvc-4/_static/image54.png "Durchsuchen von WebMatrix-Erweiterungen")</span><span class="sxs-lookup"><span data-stu-id="d4c65-634">![Browsing WebMatrix extensions](whats-new-in-aspnet-mvc-4/_static/image54.png "Browsing WebMatrix extensions")</span></span>

    <span data-ttu-id="d4c65-635">*Durchsuchen von WebMatrix-Erweiterungen*</span><span class="sxs-lookup"><span data-stu-id="d4c65-635">*Browsing WebMatrix extensions*</span></span>
4. <span data-ttu-id="d4c65-636">Klicken Sie in die Paketdetails auf **installieren** dem Fortsetzen der Installation der Erweiterung.</span><span class="sxs-lookup"><span data-stu-id="d4c65-636">In the package details, click **Install** to continue with the extension installation.</span></span>

    <span data-ttu-id="d4c65-637">![iPhone-Simulator Erweiterung](whats-new-in-aspnet-mvc-4/_static/image55.png "iPhone-Simulator-Erweiterung")</span><span class="sxs-lookup"><span data-stu-id="d4c65-637">![iPhone Simulator extension](whats-new-in-aspnet-mvc-4/_static/image55.png "iPhone Simulator extension")</span></span>

    <span data-ttu-id="d4c65-638">*iPhone-Simulator-Erweiterung*</span><span class="sxs-lookup"><span data-stu-id="d4c65-638">*iPhone Simulator extension*</span></span>
5. <span data-ttu-id="d4c65-639">Lesen Sie und akzeptieren Sie die EULA-Erweiterung.</span><span class="sxs-lookup"><span data-stu-id="d4c65-639">Read and accept the extension EULA.</span></span>

    <span data-ttu-id="d4c65-640">![WebMatrix-Erweiterung EULA](whats-new-in-aspnet-mvc-4/_static/image56.png "WebMatrix-Erweiterung EULA")</span><span class="sxs-lookup"><span data-stu-id="d4c65-640">![WebMatrix extension EULA](whats-new-in-aspnet-mvc-4/_static/image56.png "WebMatrix extension EULA")</span></span>

    <span data-ttu-id="d4c65-641">*WebMatrix extension EULA*</span><span class="sxs-lookup"><span data-stu-id="d4c65-641">*WebMatrix extension EULA*</span></span>
6. <span data-ttu-id="d4c65-642">Jetzt können Sie Ihre Website von WebMatrix ausführen, mit der iPhone-Simulator-Option.</span><span class="sxs-lookup"><span data-stu-id="d4c65-642">Now, you can run your Web site from WebMatrix using the iPhone Simulator option.</span></span>

    <span data-ttu-id="d4c65-643">![Führen Sie mithilfe von iPhone](whats-new-in-aspnet-mvc-4/_static/image57.png "mit iPhone ausgeführt")</span><span class="sxs-lookup"><span data-stu-id="d4c65-643">![Run using iPhone](whats-new-in-aspnet-mvc-4/_static/image57.png "Run using iPhone")</span></span>

    <span data-ttu-id="d4c65-644">*Führen Sie mithilfe von iPhone*</span><span class="sxs-lookup"><span data-stu-id="d4c65-644">*Run using iPhone*</span></span>

<a id="ApxCTask3"></a>

<a id="Task_3_-_Configuring_Visual_Studio_2012_to_run_iPhone_Simulator"></a>
#### <a name="task-3---configuring-visual-studio-2012-to-run-iphone-simulator"></a><span data-ttu-id="d4c65-645">Aufgabe 3: Konfigurieren von Visual Studio 2012 zum iPhone-Simulator ausführen</span><span class="sxs-lookup"><span data-stu-id="d4c65-645">Task 3 - Configuring Visual Studio 2012 to run iPhone Simulator</span></span>

1. <span data-ttu-id="d4c65-646">Öffnen Sie **Visual Studio 2012** und eine beliebige Website öffnen oder erstellen Sie ein neues Projekt.</span><span class="sxs-lookup"><span data-stu-id="d4c65-646">Open **Visual Studio 2012** and open any Web site or create a new project.</span></span>
2. <span data-ttu-id="d4c65-647">Klicken Sie auf den Pfeil nach unten über die Schaltfläche "ausführen", und wählen Sie **Browserauswahl**.</span><span class="sxs-lookup"><span data-stu-id="d4c65-647">Click the down arrow from the Run button and select **Browse with**.</span></span>

    <span data-ttu-id="d4c65-648">![Browserauswahl](whats-new-in-aspnet-mvc-4/_static/image58.png "Browserauswahl")</span><span class="sxs-lookup"><span data-stu-id="d4c65-648">![Browse with](whats-new-in-aspnet-mvc-4/_static/image58.png "Browse with")</span></span>

    <span data-ttu-id="d4c65-649">*Browserauswahl*</span><span class="sxs-lookup"><span data-stu-id="d4c65-649">*Browse with*</span></span>
3. <span data-ttu-id="d4c65-650">In der &quot;Browserauswahl&quot; Dialogfeld klicken Sie auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="d4c65-650">In the &quot;Browse With&quot; dialog, click **Add**.</span></span>
4. <span data-ttu-id="d4c65-651">In der &quot;Programm hinzufügen&quot; Dialogfeld, verwenden Sie die folgenden Werte:</span><span class="sxs-lookup"><span data-stu-id="d4c65-651">In the &quot;Add Program&quot; dialog, use the following values:</span></span>

   - <span data-ttu-id="d4c65-652"><strong>Programm</strong>: C:\Users\*{CurrentUser}<em>\AppData\Local\Microsoft\WebMatrix\Extensions\20\iPhoneSimulator\ElectricMobileSim\ElectricMobileSim.exe * (Aktualisieren Sie den Pfad entsprechend)</em></span><span class="sxs-lookup"><span data-stu-id="d4c65-652"><strong>Program</strong>: C:\Users\*{CurrentUser}<em>\AppData\Local\Microsoft\WebMatrix\Extensions\20\iPhoneSimulator\ElectricMobileSim\ElectricMobileSim.exe *(update the path accordingly)</em></span></span>
   - <span data-ttu-id="d4c65-653">**Argumente**: &quot;1&quot;</span><span class="sxs-lookup"><span data-stu-id="d4c65-653">**Arguments**: &quot;1&quot;</span></span>
   - <span data-ttu-id="d4c65-654">**Anzeigename**: iPhone-Simulator</span><span class="sxs-lookup"><span data-stu-id="d4c65-654">**Friendly name**: iPhone Simulator</span></span>

     <span data-ttu-id="d4c65-655">![Programm hinzufügen](whats-new-in-aspnet-mvc-4/_static/image59.png "Programm hinzufügen")</span><span class="sxs-lookup"><span data-stu-id="d4c65-655">![Add program](whats-new-in-aspnet-mvc-4/_static/image59.png "Add program")</span></span>

     <span data-ttu-id="d4c65-656">*Hinzufügen eines Programms mit Durchsuchen*</span><span class="sxs-lookup"><span data-stu-id="d4c65-656">*Add program to browse with*</span></span>
5. <span data-ttu-id="d4c65-657">Klicken Sie auf **OK** und schließen Sie alle Dialogfelder.</span><span class="sxs-lookup"><span data-stu-id="d4c65-657">Click **OK** and close the dialogs.</span></span>
6. <span data-ttu-id="d4c65-658">Jetzt können Sie Ihre Webanwendungen in der iPhone-Simulator von Visual Studio 2012 ausführen.</span><span class="sxs-lookup"><span data-stu-id="d4c65-658">Now you are able to run your Web applications in the iPhone simulator from Visual Studio 2012.</span></span>

    <span data-ttu-id="d4c65-659">![Navigieren Sie mit der iPhone-Simulator](whats-new-in-aspnet-mvc-4/_static/image60.png "Browserauswahl iPhone-Simulator")</span><span class="sxs-lookup"><span data-stu-id="d4c65-659">![Browse with iPhone Simulator](whats-new-in-aspnet-mvc-4/_static/image60.png "Browse with iPhone Simulator")</span></span>

    <span data-ttu-id="d4c65-660">*Navigieren Sie mit der iPhone-Simulator*</span><span class="sxs-lookup"><span data-stu-id="d4c65-660">*Browse with iPhone Simulator*</span></span>

<a id="AppendixD"></a>

<a id="Appendix_D_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-d-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="d4c65-661">Anhang D: Veröffentlichen einer ASP.NET MVC 4-Anwendung mithilfe von Web Deploy</span><span class="sxs-lookup"><span data-stu-id="d4c65-661">Appendix D: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="d4c65-662">In diesem Anhang wird gezeigt, wie eine neue Website aus dem Windows Azure-Verwaltungsportal erstellen und veröffentlichen Sie die Anwendung, die Sie erhalten haben anhand der testumgebung profitieren von der Web Deploy Veröffentlichungsfunktion von Windows Azure bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="d4c65-662">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxDTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="d4c65-663">Aufgabe 1: Erstellen einer neuen Website aus dem Windows Azure-Portal</span><span class="sxs-lookup"><span data-stu-id="d4c65-663">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="d4c65-664">Wechseln Sie zu der [Windows Azure-Verwaltungsportal](https://manage.windowsazure.com/) und melden Sie sich mit den Microsoft-Anmeldeinformationen, die Ihrem Abonnement zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="d4c65-664">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d4c65-665">Sie können mit Windows Azure hosten 10 ASP.NET-Websites kostenlos und skalieren Sie, wenn Ihr Datenverkehr zunimmt.</span><span class="sxs-lookup"><span data-stu-id="d4c65-665">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="d4c65-666">Sie können registrieren [hier](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="d4c65-666">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="d4c65-667">![Melden Sie sich bei Windows Azure-Portal](whats-new-in-aspnet-mvc-4/_static/image61.png "melden Sie sich bei Windows Azure-Portal")</span><span class="sxs-lookup"><span data-stu-id="d4c65-667">![Log on to Windows Azure portal](whats-new-in-aspnet-mvc-4/_static/image61.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="d4c65-668">*Melden Sie sich bei Windows Azure-Verwaltungsportal*</span><span class="sxs-lookup"><span data-stu-id="d4c65-668">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="d4c65-669">Klicken Sie auf **neu** in der Befehlszeile.</span><span class="sxs-lookup"><span data-stu-id="d4c65-669">Click **New** on the command bar.</span></span>

    <span data-ttu-id="d4c65-670">![Erstellen einer neuen Website](whats-new-in-aspnet-mvc-4/_static/image62.png "Erstellen einer neuen Website")</span><span class="sxs-lookup"><span data-stu-id="d4c65-670">![Creating a new Web Site](whats-new-in-aspnet-mvc-4/_static/image62.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="d4c65-671">*Erstellen einer neuen Website*</span><span class="sxs-lookup"><span data-stu-id="d4c65-671">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="d4c65-672">Klicken Sie auf **berechnen** | **Website**.</span><span class="sxs-lookup"><span data-stu-id="d4c65-672">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="d4c65-673">Wählen Sie dann **Schnellerfassung** Option.</span><span class="sxs-lookup"><span data-stu-id="d4c65-673">Then select **Quick Create** option.</span></span> <span data-ttu-id="d4c65-674">Verfügbare URL für die neue Website bereitstellen, und klicken Sie auf **Website erstellen**.</span><span class="sxs-lookup"><span data-stu-id="d4c65-674">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d4c65-675">Eine Windows Azure-Website ist der Host für eine Webanwendung, die ausgeführt wird, in der Cloud, die Sie steuern und verwalten können.</span><span class="sxs-lookup"><span data-stu-id="d4c65-675">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="d4c65-676">Die Option Schnellerfassung können Sie eine vollständige Webanwendung, die Windows Azure-Website von außerhalb des Portals bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="d4c65-676">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="d4c65-677">Es umfasst nicht die Schritte zum Einrichten einer Datenbank.</span><span class="sxs-lookup"><span data-stu-id="d4c65-677">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="d4c65-678">![Erstellen eine neue Website, die mit der Schnellerfassung](whats-new-in-aspnet-mvc-4/_static/image63.png "erstellen eine neue Website, die mit der Schnellerfassung")</span><span class="sxs-lookup"><span data-stu-id="d4c65-678">![Creating a new Web Site using Quick Create](whats-new-in-aspnet-mvc-4/_static/image63.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="d4c65-679">*Erstellen eine neue Website, die mit der Schnellerfassung*</span><span class="sxs-lookup"><span data-stu-id="d4c65-679">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="d4c65-680">Warten Sie, bis die neue **Website** wird erstellt.</span><span class="sxs-lookup"><span data-stu-id="d4c65-680">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="d4c65-681">Nachdem die Website erstellt wurde, klicken Sie auf den Link unter der **URL** Spalte.</span><span class="sxs-lookup"><span data-stu-id="d4c65-681">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="d4c65-682">Überprüfen Sie, dass die neue Website funktioniert.</span><span class="sxs-lookup"><span data-stu-id="d4c65-682">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="d4c65-683">![Um die neue Website durchsuchen](whats-new-in-aspnet-mvc-4/_static/image64.png "durchsuchen, um die neue Website")</span><span class="sxs-lookup"><span data-stu-id="d4c65-683">![Browsing to the new web site](whats-new-in-aspnet-mvc-4/_static/image64.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="d4c65-684">*Um die neue Website durchsuchen*</span><span class="sxs-lookup"><span data-stu-id="d4c65-684">*Browsing to the new web site*</span></span>

    <span data-ttu-id="d4c65-685">![Website](whats-new-in-aspnet-mvc-4/_static/image65.png "Website ausgeführt wird")</span><span class="sxs-lookup"><span data-stu-id="d4c65-685">![Web site running](whats-new-in-aspnet-mvc-4/_static/image65.png "Web site running")</span></span>

    <span data-ttu-id="d4c65-686">*Die Website ausgeführt wird*</span><span class="sxs-lookup"><span data-stu-id="d4c65-686">*Web site running*</span></span>
6. <span data-ttu-id="d4c65-687">Wechseln Sie zurück zum Portal, und klicken Sie auf den Namen der Website unter der **Namen** Spalte die Verwaltungsseiten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="d4c65-687">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="d4c65-688">![Öffnen die Website-Verwaltungsseiten](whats-new-in-aspnet-mvc-4/_static/image66.png "öffnen die Website-Verwaltungsseiten")</span><span class="sxs-lookup"><span data-stu-id="d4c65-688">![Opening the web site management pages](whats-new-in-aspnet-mvc-4/_static/image66.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="d4c65-689">*Öffnen die Website-Verwaltungsseiten*</span><span class="sxs-lookup"><span data-stu-id="d4c65-689">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="d4c65-690">In der **Dashboard** Seite der **kurzer Blick** auf die **Herunterladen eines Veröffentlichungsprofils** Link.</span><span class="sxs-lookup"><span data-stu-id="d4c65-690">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d4c65-691">Die *Veröffentlichungsprofil* enthält alle Informationen zum Veröffentlichen einer Webanwendung in einer Windows Azure-Website für die einzelnen aktivierten Veröffentlichungsmethoden erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="d4c65-691">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="d4c65-692">Das Veröffentlichungsprofil enthält die URLs, Benutzeranmeldeinformationen und datenbankzeichenfolgen, die erforderlich sind, eine Verbindung herstellen und die Authentifizierung für alle Endpunkte für die eine Veröffentlichungsmethode aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="d4c65-692">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="d4c65-693">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span><span class="sxs-lookup"><span data-stu-id="d4c65-693">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="d4c65-694">![Herunterladen der Website-Veröffentlichungsprofil](whats-new-in-aspnet-mvc-4/_static/image67.png "der Website herunterladen eines Veröffentlichungsprofils")</span><span class="sxs-lookup"><span data-stu-id="d4c65-694">![Downloading the web site publish profile](whats-new-in-aspnet-mvc-4/_static/image67.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="d4c65-695">*Die Website herunterladen eines Veröffentlichungsprofils*</span><span class="sxs-lookup"><span data-stu-id="d4c65-695">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="d4c65-696">Laden Sie das Veröffentlichungsprofil an einem bekannten Speicherort ein.</span><span class="sxs-lookup"><span data-stu-id="d4c65-696">Download the publish profile file to a known location.</span></span> <span data-ttu-id="d4c65-697">In dieser Übung wird weiter Gewusst wie: Verwenden Sie diese Datei zum Veröffentlichen einer Webanwendung auf einem Windows Azure-Websites in Visual Studio angezeigt.</span><span class="sxs-lookup"><span data-stu-id="d4c65-697">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="d4c65-698">![Speichern das Veröffentlichungsprofil](whats-new-in-aspnet-mvc-4/_static/image68.png "speichern das Veröffentlichungsprofil")</span><span class="sxs-lookup"><span data-stu-id="d4c65-698">![Saving the publish profile file](whats-new-in-aspnet-mvc-4/_static/image68.png "Saving the publish profile")</span></span>

    <span data-ttu-id="d4c65-699">*Speichern das Veröffentlichungsprofil*</span><span class="sxs-lookup"><span data-stu-id="d4c65-699">*Saving the publish profile file*</span></span>

<a id="ApxDTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="d4c65-700">Aufgabe 2: konfigurieren den Datenbankserver</span><span class="sxs-lookup"><span data-stu-id="d4c65-700">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="d4c65-701">Wenn die Anwendung durchführt, verwenden Sie SQL Server Datenbanken Sie einen SQL-Datenbankserver erstellen müssen.</span><span class="sxs-lookup"><span data-stu-id="d4c65-701">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="d4c65-702">Wenn eine einfache Anwendung bereitstellen, die keine SQL Server verwendet werden sollen, können Sie diese Aufgabe überspringen.</span><span class="sxs-lookup"><span data-stu-id="d4c65-702">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="d4c65-703">Sie benötigen einen SQL-Datenbankserver zum Speichern der Anwendungsdatenbank.</span><span class="sxs-lookup"><span data-stu-id="d4c65-703">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="d4c65-704">Sehen Sie die SQL-Datenbankservern über Ihr Abonnement in Windows Azure-Verwaltungsportal am **Sql-Datenbanken** | **Server** | **des Servers Dashboard**.</span><span class="sxs-lookup"><span data-stu-id="d4c65-704">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="d4c65-705">Wenn Sie keinen Server erstellt haben, können Sie erstellen, mit der **hinzufügen** auf der Befehlsleiste auf die Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="d4c65-705">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="d4c65-706">Notieren Sie die **Servername und -URL, Administrator-Benutzername und Kennwort**, wie Sie sie in den nächsten Aufgaben verwenden.</span><span class="sxs-lookup"><span data-stu-id="d4c65-706">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="d4c65-707">Erstellen Sie die Datenbank nicht noch, wie er in einer späteren Phase erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="d4c65-707">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="d4c65-708">![SQL-Datenbank-Dashboard](whats-new-in-aspnet-mvc-4/_static/image69.png "SQL-Datenbank-Dashboard")</span><span class="sxs-lookup"><span data-stu-id="d4c65-708">![SQL Database Server Dashboard](whats-new-in-aspnet-mvc-4/_static/image69.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="d4c65-709">*SQL-Datenbank-Dashboard*</span><span class="sxs-lookup"><span data-stu-id="d4c65-709">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="d4c65-710">In der nächsten Aufgabe testen Sie die Verbindung mit der Datenbank, aus Visual Studio aus diesem Grund Sie die lokale IP-Adresse in der Serverliste des müssen **zulässigen IP-Adressen**.</span><span class="sxs-lookup"><span data-stu-id="d4c65-710">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="d4c65-711">Klicken Sie hierzu auf **konfigurieren**, wählen Sie die IP-Adresse von **aktuelle Client-IP-Adresse** und fügen Sie ihn auf die **IP-Startadresse** und **IP-Endadresse** Textfelder, und klicken Sie auf die ![add-client-ip-address-ok-button](whats-new-in-aspnet-mvc-4/_static/image70.png) Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="d4c65-711">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](whats-new-in-aspnet-mvc-4/_static/image70.png) button.</span></span>

    ![Client-IP-Adresse hinzufügen](whats-new-in-aspnet-mvc-4/_static/image71.png)

    <span data-ttu-id="d4c65-713">*Client-IP-Adresse hinzufügen*</span><span class="sxs-lookup"><span data-stu-id="d4c65-713">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="d4c65-714">Einmal die **IP-Clientadresse** wird zu den zulässigen IP-Adressen hinzugefügt Liste, klicken Sie auf **speichern** um die Änderungen zu bestätigen.</span><span class="sxs-lookup"><span data-stu-id="d4c65-714">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Bestätigen von Änderungen](whats-new-in-aspnet-mvc-4/_static/image72.png)

    <span data-ttu-id="d4c65-716">*Bestätigen von Änderungen*</span><span class="sxs-lookup"><span data-stu-id="d4c65-716">*Confirm Changes*</span></span>

<a id="ApxDTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="d4c65-717">Aufgabe 3: Veröffentlichen einer ASP.NET MVC 4-Anwendung mithilfe von Web Deploy</span><span class="sxs-lookup"><span data-stu-id="d4c65-717">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="d4c65-718">Wechseln Sie zurück, die ASP.NET MVC 4-Projektmappe.</span><span class="sxs-lookup"><span data-stu-id="d4c65-718">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="d4c65-719">In der **Projektmappen-Explorer**mit der rechten Maustaste auf das Websiteprojekt, und wählen Sie **veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="d4c65-719">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="d4c65-720">![Veröffentlichen der Anwendung](whats-new-in-aspnet-mvc-4/_static/image73.png "Veröffentlichen der Anwendung")</span><span class="sxs-lookup"><span data-stu-id="d4c65-720">![Publishing the Application](whats-new-in-aspnet-mvc-4/_static/image73.png "Publishing the Application")</span></span>

    <span data-ttu-id="d4c65-721">*Veröffentlichen der Website*</span><span class="sxs-lookup"><span data-stu-id="d4c65-721">*Publishing the web site*</span></span>
2. <span data-ttu-id="d4c65-722">Importieren Sie das Veröffentlichungsprofil, das Sie in der ersten Aufgabe gespeichert.</span><span class="sxs-lookup"><span data-stu-id="d4c65-722">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="d4c65-723">![Importieren des Veröffentlichungsprofils](whats-new-in-aspnet-mvc-4/_static/image74.png "Importieren des Veröffentlichungsprofils")</span><span class="sxs-lookup"><span data-stu-id="d4c65-723">![Importing the publish profile](whats-new-in-aspnet-mvc-4/_static/image74.png "Importing the publish profile")</span></span>

    <span data-ttu-id="d4c65-724">*Importieren Sie das Veröffentlichungsprofil*</span><span class="sxs-lookup"><span data-stu-id="d4c65-724">*Importing publish profile*</span></span>
3. <span data-ttu-id="d4c65-725">Klicken Sie auf **überprüft, ob Verbindung**.</span><span class="sxs-lookup"><span data-stu-id="d4c65-725">Click **Validate Connection**.</span></span> <span data-ttu-id="d4c65-726">Klicken Sie nach Abschluss der Überprüfung auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="d4c65-726">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d4c65-727">Überprüfung ist beendet, sobald Sie sehen, dass ein grünes Häkchen neben der Schaltfläche "Validate Connection" angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="d4c65-727">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="d4c65-728">![Überprüfen der Verbindung](whats-new-in-aspnet-mvc-4/_static/image75.png "Überprüfen der Verbindung")</span><span class="sxs-lookup"><span data-stu-id="d4c65-728">![Validating connection](whats-new-in-aspnet-mvc-4/_static/image75.png "Validating connection")</span></span>

    <span data-ttu-id="d4c65-729">*Überprüfen der Verbindung*</span><span class="sxs-lookup"><span data-stu-id="d4c65-729">*Validating connection*</span></span>
4. <span data-ttu-id="d4c65-730">In der **Einstellungen** Seite der **Datenbanken** auf die Schaltfläche neben dem Textfeld für die datenbankverbindung (d. h. **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="d4c65-730">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="d4c65-731">![Web deploy-Konfiguration](whats-new-in-aspnet-mvc-4/_static/image76.png "Web deploy-Konfiguration")</span><span class="sxs-lookup"><span data-stu-id="d4c65-731">![Web deploy configuration](whats-new-in-aspnet-mvc-4/_static/image76.png "Web deploy configuration")</span></span>

    <span data-ttu-id="d4c65-732">*Web deploy-Konfiguration*</span><span class="sxs-lookup"><span data-stu-id="d4c65-732">*Web deploy configuration*</span></span>
5. <span data-ttu-id="d4c65-733">Konfigurieren Sie die Verbindung mit der Datenbank wie folgt:</span><span class="sxs-lookup"><span data-stu-id="d4c65-733">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="d4c65-734">In der **Servernamen** Geben Sie Ihre SQL-Datenbank Server-URL unter Verwendung der *Tcp:* Präfix.</span><span class="sxs-lookup"><span data-stu-id="d4c65-734">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="d4c65-735">In **Benutzername** Geben Sie Ihre Administrator Serveranmeldenamen an.</span><span class="sxs-lookup"><span data-stu-id="d4c65-735">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="d4c65-736">In **Kennwort** Geben Sie Ihre Server-Administratorkennwort.</span><span class="sxs-lookup"><span data-stu-id="d4c65-736">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="d4c65-737">Geben Sie einen neuen Datenbanknamen ein, z. B.: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="d4c65-737">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="d4c65-738">![Konfigurieren von Zielverbindungszeichenfolge](whats-new-in-aspnet-mvc-4/_static/image77.png "Zielverbindungszeichenfolge konfigurieren")</span><span class="sxs-lookup"><span data-stu-id="d4c65-738">![Configuring destination connection string](whats-new-in-aspnet-mvc-4/_static/image77.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="d4c65-739">*Konfigurieren von Ziel-Verbindungszeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="d4c65-739">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="d4c65-740">Klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="d4c65-740">Then click **OK**.</span></span> <span data-ttu-id="d4c65-741">Bei der Aufforderung zum Erstellen des Datenbank auf **Ja**.</span><span class="sxs-lookup"><span data-stu-id="d4c65-741">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="d4c65-742">![Erstellen der Datenbank](whats-new-in-aspnet-mvc-4/_static/image78.png "erstellen die Datenbank-Zeichenfolge")</span><span class="sxs-lookup"><span data-stu-id="d4c65-742">![Creating the database](whats-new-in-aspnet-mvc-4/_static/image78.png "Creating the database string")</span></span>

    <span data-ttu-id="d4c65-743">*Erstellen der Datenbank*</span><span class="sxs-lookup"><span data-stu-id="d4c65-743">*Creating the database*</span></span>
7. <span data-ttu-id="d4c65-744">Die Verbindungszeichenfolge, die Sie verwenden für die Verbindung mit SQL-Datenbank in Windows Azure ist innerhalb der Verbindung standardmäßig Textfeld angezeigt.</span><span class="sxs-lookup"><span data-stu-id="d4c65-744">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="d4c65-745">Klicken Sie dann auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="d4c65-745">Then click **Next**.</span></span>

    <span data-ttu-id="d4c65-746">![Verbindungszeichenfolge zur SQL-Datenbank zeigt](whats-new-in-aspnet-mvc-4/_static/image79.png "Verbindungszeichenfolge zur SQL-Datenbank zeigt")</span><span class="sxs-lookup"><span data-stu-id="d4c65-746">![Connection string pointing to SQL Database](whats-new-in-aspnet-mvc-4/_static/image79.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="d4c65-747">*Verbindungszeichenfolge, die auf der SQL-Datenbank*</span><span class="sxs-lookup"><span data-stu-id="d4c65-747">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="d4c65-748">In der **Vorschau** auf **veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="d4c65-748">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="d4c65-749">![Veröffentlichen der Webanwendung](whats-new-in-aspnet-mvc-4/_static/image80.png "Veröffentlichen der Webanwendung")</span><span class="sxs-lookup"><span data-stu-id="d4c65-749">![Publishing the web application](whats-new-in-aspnet-mvc-4/_static/image80.png "Publishing the web application")</span></span>

    <span data-ttu-id="d4c65-750">*Veröffentlichen der Webanwendung*</span><span class="sxs-lookup"><span data-stu-id="d4c65-750">*Publishing the web application*</span></span>
9. <span data-ttu-id="d4c65-751">Sobald der Veröffentlichungsprozess abgeschlossen ist, wird der Standardbrowser veröffentlichten Website geöffnet.</span><span class="sxs-lookup"><span data-stu-id="d4c65-751">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="d4c65-752">![Anwendung in Windows Azure veröffentlicht](whats-new-in-aspnet-mvc-4/_static/image81.png "Veröffentlichung der Anwendung in Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="d4c65-752">![Application published to Windows Azure](whats-new-in-aspnet-mvc-4/_static/image81.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="d4c65-753">*Anwendung, die auf Windows Azure veröffentlicht werden soll*</span><span class="sxs-lookup"><span data-stu-id="d4c65-753">*Application published to Windows Azure*</span></span>
