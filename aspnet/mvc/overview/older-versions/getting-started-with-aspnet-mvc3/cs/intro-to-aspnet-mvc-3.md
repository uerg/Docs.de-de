---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
title: "Einführung in ASP.NET MVC 3 (c#) | Microsoft Docs"
author: Rick-Anderson
description: In diesem Lernprogramm erfahren Sie die Grundlagen der Erstellung einer ASP.NET MVC-Webanwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1, also...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 86a80b35-88bd-4b7c-bd58-f6e7997197d4
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: bbeaad9e52db1fd85ef166052a377e8b6732a90a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="intro-to-aspnet-mvc-3-c"></a><span data-ttu-id="7303f-103">Einführung in ASP.NET MVC 3 (c#)</span><span class="sxs-lookup"><span data-stu-id="7303f-103">Intro to ASP.NET MVC 3 (C#)</span></span>
====================
<span data-ttu-id="7303f-104">Durch [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="7303f-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="7303f-105">Eine aktualisierte Version dieses Lernprogramms steht [hier](../../../getting-started/introduction/getting-started.md) , ASP.NET MVC 5 und Visual Studio 2013 verwendet.</span><span class="sxs-lookup"><span data-stu-id="7303f-105">An updated version of this tutorial is available [here](../../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="7303f-106">Es ist sicherer, viel einfacher, führen und weitere Funktionen veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="7303f-106">It's more secure, much simpler to follow and demonstrates more features.</span></span>
> 
> 
> <span data-ttu-id="7303f-107">In diesem Lernprogramm erfahren Sie die Grundlagen der Erstellung einer ASP.NET MVC-Webanwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1, also eine kostenlose Version von Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7303f-107">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="7303f-108">Bevor Sie beginnen, stellen Sie sicher, dass Sie die unten aufgeführten erforderlichen Komponenten installiert haben.</span><span class="sxs-lookup"><span data-stu-id="7303f-108">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="7303f-109">Sie können alle installieren, indem Sie auf den folgenden Link: [Webplattform-Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="7303f-109">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="7303f-110">Alternativ können Sie die erforderlichen Komponenten, die über die folgenden Links einzeln installieren:</span><span class="sxs-lookup"><span data-stu-id="7303f-110">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="7303f-111">Visual Studio Web Developer Express SP1-Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="7303f-111">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="7303f-112">ASP.NET MVC 3 Tools Update</span><span class="sxs-lookup"><span data-stu-id="7303f-112">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="7303f-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Common Language Runtime + Tools unterstützen)</span><span class="sxs-lookup"><span data-stu-id="7303f-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="7303f-114">Wenn Sie Visual Studio 2010 anstelle von Visual Web Developer 2010 verwenden, die Komponenten installieren, indem Sie auf den folgenden Link: [Visual Studio 2010-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="7303f-114">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="7303f-115">Ein Visual Web Developer-Projekt mit C#-Quellcode ist zu diesem Thema steht zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="7303f-115">A Visual Web Developer project with C# source code is available to accompany this topic.</span></span> <span data-ttu-id="7303f-116">[Die C#-Version herunterladen](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="7303f-116">[Download the C# version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="7303f-117">Wenn Sie Visual Basic bevorzugen, wechseln Sie zu der [Visual Basic-Version](../vb/intro-to-aspnet-mvc-3.md) dieses Lernprogramm.</span><span class="sxs-lookup"><span data-stu-id="7303f-117">If you prefer Visual Basic, switch to the [Visual Basic version](../vb/intro-to-aspnet-mvc-3.md) of this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="7303f-118">Was müssen Sie erstellen</span><span class="sxs-lookup"><span data-stu-id="7303f-118">What You'll Build</span></span>

<span data-ttu-id="7303f-119">Implementieren Sie eine einfache Anwendung von Film-Auflistung, die unterstützt werden, erstellen, bearbeiten und Auflisten von Filmen aus einer Datenbank.</span><span class="sxs-lookup"><span data-stu-id="7303f-119">You'll implement a simple movie-listing application that supports creating, editing, and listing movies from a database.</span></span> <span data-ttu-id="7303f-120">Im folgenden sind zwei Screenshots der Anwendung, die Sie erstellen müssen.</span><span class="sxs-lookup"><span data-stu-id="7303f-120">Below are two screenshots of the application you'll build.</span></span> <span data-ttu-id="7303f-121">Es enthält eine Seite, die eine Liste von Filmen aus einer Datenbank anzeigt:</span><span class="sxs-lookup"><span data-stu-id="7303f-121">It includes a page that displays a list of movies from a database:</span></span>

![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image1.png)

<span data-ttu-id="7303f-123">Die Anwendung lässt auch hinzufügen, bearbeiten und Löschen von Filmen sowie ausführliche Informationen finden Sie Details zu einzelnen diejenigen.</span><span class="sxs-lookup"><span data-stu-id="7303f-123">The application also lets you add, edit, and delete movies, as well as see details about individual ones.</span></span> <span data-ttu-id="7303f-124">Alle Dateneingabe Szenarien umfassen die Überprüfung aus, um sicherzustellen, dass die in der Datenbank gespeicherten Daten richtig ist.</span><span class="sxs-lookup"><span data-stu-id="7303f-124">All data-entry scenarios include validation to ensure that the data stored in the database is correct.</span></span>

![](intro-to-aspnet-mvc-3/_static/image2.png)

## <a name="skills-youll-learn"></a><span data-ttu-id="7303f-125">Fähigkeiten, die Sie erfahren</span><span class="sxs-lookup"><span data-stu-id="7303f-125">Skills You'll Learn</span></span>

<span data-ttu-id="7303f-126">Hier ist Sie lernen:</span><span class="sxs-lookup"><span data-stu-id="7303f-126">Here's what you'll learn:</span></span>

- <span data-ttu-id="7303f-127">Vorgehensweise: Erstellen Sie ein neues ASP.NET MVC-Projekt.</span><span class="sxs-lookup"><span data-stu-id="7303f-127">How to create a new ASP.NET MVC project.</span></span>
- <span data-ttu-id="7303f-128">Vorgehensweise: Erstellen von ASP.NET MVC-Controller und Ansichten.</span><span class="sxs-lookup"><span data-stu-id="7303f-128">How to create ASP.NET MVC controllers and views.</span></span>
- <span data-ttu-id="7303f-129">Vorgehensweise: erstellen eine neue Datenbank mit dem Entity Framework Code First-Paradigma.</span><span class="sxs-lookup"><span data-stu-id="7303f-129">How to create a new database using the Entity Framework Code First paradigm.</span></span>
- <span data-ttu-id="7303f-130">Informationen zum Abrufen und Anzeigen von Daten.</span><span class="sxs-lookup"><span data-stu-id="7303f-130">How to retrieve and display data.</span></span>
- <span data-ttu-id="7303f-131">Informationen zum Bearbeiten von Daten und Validierung zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="7303f-131">How to edit data and enable data validation.</span></span>

## <a name="getting-started"></a><span data-ttu-id="7303f-132">Erste Schritte</span><span class="sxs-lookup"><span data-stu-id="7303f-132">Getting Started</span></span>

<span data-ttu-id="7303f-133">Starten Sie durch Ausführen von Visual Web Developer 2010 Express ("Visual Web Developer" für kurze), und wählen Sie **neues Projekt** aus der **starten** Seite.</span><span class="sxs-lookup"><span data-stu-id="7303f-133">Start by running Visual Web Developer 2010 Express ("Visual Web Developer" for short) and select **New Project** from the **Start** page.</span></span>

<span data-ttu-id="7303f-134">Visual Web Developer ist eine IDE oder der integrierten Entwicklungsumgebung.</span><span class="sxs-lookup"><span data-stu-id="7303f-134">Visual Web Developer is an IDE, or integrated development environment.</span></span> <span data-ttu-id="7303f-135">Wie Sie Microsoft Word verwenden, um Dokumente zu schreiben, verwenden Sie eine IDE zum Erstellen von Anwendungen.</span><span class="sxs-lookup"><span data-stu-id="7303f-135">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="7303f-136">In Visual Web Developer ist eine Symbolleiste am oberen Rand mit verschiedenen verfügbaren Optionen für Sie.</span><span class="sxs-lookup"><span data-stu-id="7303f-136">In Visual Web Developer there's a toolbar along the top showing various options available to you.</span></span> <span data-ttu-id="7303f-137">Es gibt auch ein Menü, das eine andere Möglichkeit, Aufgaben in der IDE bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="7303f-137">There's also a menu that provides another way to perform tasks in the IDE.</span></span> <span data-ttu-id="7303f-138">(Z. B. statt der Auswahl **neues Projekt** aus der **starten** Seite können Sie mithilfe des Menüs und auswählen **Datei** &gt; **neues Projekt**.)</span><span class="sxs-lookup"><span data-stu-id="7303f-138">(For example, instead of selecting **New Project** from the **Start** page, you can use the menu and select **File** &gt; **New Project**.)</span></span>

[![](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)

## <a name="creating-your-first-application"></a><span data-ttu-id="7303f-139">Erstellen einer ersten Anwendung</span><span class="sxs-lookup"><span data-stu-id="7303f-139">Creating Your First Application</span></span>

<span data-ttu-id="7303f-140">Sie können Anwendungen mit Visual Basic oder Visual c# als Programmiersprache erstellen.</span><span class="sxs-lookup"><span data-stu-id="7303f-140">You can create applications using either Visual Basic or Visual C# as the programming language.</span></span> <span data-ttu-id="7303f-141">Visual c# auf der linken Seite und wählen Sie dann **ASP.NET MVC 3-Webanwendung**.</span><span class="sxs-lookup"><span data-stu-id="7303f-141">Select Visual C# on the left and then select **ASP.NET MVC 3 Web Application**.</span></span> <span data-ttu-id="7303f-142">Namen für das Projekt "MvcMovie", und klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="7303f-142">Name your project "MvcMovie" and then click **OK**.</span></span> <span data-ttu-id="7303f-143">(Wenn Sie Visual Basic bevorzugen, wechseln Sie zu der [Visual Basic-Version](../vb/intro-to-aspnet-mvc-3.md) dieses Lernprogramms.)</span><span class="sxs-lookup"><span data-stu-id="7303f-143">(If you prefer Visual Basic, switch to the [Visual Basic version](../vb/intro-to-aspnet-mvc-3.md) of this tutorial.)</span></span>

![](intro-to-aspnet-mvc-3/_static/image5.png)

<span data-ttu-id="7303f-144">In der **neues ASP.NET MVC 3-Projekt** wählen Sie im Dialogfeld **Internetanwendung**.</span><span class="sxs-lookup"><span data-stu-id="7303f-144">In the **New ASP.NET MVC 3 Project** dialog box, select **Internet Application**.</span></span> <span data-ttu-id="7303f-145">Überprüfen Sie **verwenden HTML5 Markup** und belassen Sie **Razor** als Standardansichtsmodul.</span><span class="sxs-lookup"><span data-stu-id="7303f-145">Check **Use HTML5 markup** and leave **Razor** as the default view engine.</span></span>

![](intro-to-aspnet-mvc-3/_static/image6.png)

<span data-ttu-id="7303f-146">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="7303f-146">Click **OK**.</span></span> <span data-ttu-id="7303f-147">Visual Web Developer verwendet eine Standardvorlage für das ASP.NET MVC-Projekt, das Sie gerade erstellt haben, das müssen Sie eine funktionierende Anwendung jetzt ohne jegliche!</span><span class="sxs-lookup"><span data-stu-id="7303f-147">Visual Web Developer used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="7303f-148">Dies ist eine einfache "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="7303f-148">This is a simple "Hello World!"</span></span> <span data-ttu-id="7303f-149">der Projekt, und es werden eine gute zum Starten der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="7303f-149">project, and it's a good place to start your application.</span></span>

[![](intro-to-aspnet-mvc-3/_static/image8.png)](intro-to-aspnet-mvc-3/_static/image7.png)

<span data-ttu-id="7303f-150">Wählen Sie im Menü **Debuggen** die Option **Debugging starten**.</span><span class="sxs-lookup"><span data-stu-id="7303f-150">From the **Debug** menu, select **Start Debugging**.</span></span>

![](intro-to-aspnet-mvc-3/_static/image9.png)

<span data-ttu-id="7303f-151">Beachten Sie, dass die Tastenkombination zum Starten des Debuggings F5.</span><span class="sxs-lookup"><span data-stu-id="7303f-151">Notice that the keyboard shortcut to start debugging is F5.</span></span>

<span data-ttu-id="7303f-152">F5 bewirkt, dass Visual Web Developer zum Starten von eines Development Webservers, und führen Sie die Webanwendung.</span><span class="sxs-lookup"><span data-stu-id="7303f-152">F5 causes Visual Web Developer to start a development web server and run your web application.</span></span> <span data-ttu-id="7303f-153">Visual Web Developer startet einen Browser dann und Startseite der Anwendung geöffnet.</span><span class="sxs-lookup"><span data-stu-id="7303f-153">Visual Web Developer then launches a browser and opens the application's home page.</span></span> <span data-ttu-id="7303f-154">Beachten Sie, die besagt, die Adressleiste des Browsers dass `localhost` und nicht etwas wie `example.com`.</span><span class="sxs-lookup"><span data-stu-id="7303f-154">Notice that the address bar of the browser says `localhost` and not something like `example.com`.</span></span> <span data-ttu-id="7303f-155">Grund hierfür ist, `localhost` verweist immer auf Ihrem lokalen Computer, die in diesem Fall die Anwendung ausgeführt wird Sie soeben erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="7303f-155">That's because `localhost` always points to your own local computer, which in this case is running the application you just built.</span></span> <span data-ttu-id="7303f-156">Wenn Visual Web Developer ein Webprojekt ausgeführt wird, wird für den Webserver ein zufälliger Port verwendet.</span><span class="sxs-lookup"><span data-stu-id="7303f-156">When Visual Web Developer runs a web project, a random port is used for the web server.</span></span> <span data-ttu-id="7303f-157">In der folgenden Abbildung ist die Anzahl der zufälligen Port 43246.</span><span class="sxs-lookup"><span data-stu-id="7303f-157">In the image below, the random port number is 43246.</span></span> <span data-ttu-id="7303f-158">Wenn Sie die Anwendung ausführen, sehen Sie möglicherweise eine andere Portnummer an.</span><span class="sxs-lookup"><span data-stu-id="7303f-158">When you run the application, you'll probably see a different port number.</span></span>

![](intro-to-aspnet-mvc-3/_static/image10.png)

<span data-ttu-id="7303f-159">Direkt einsatzbereiten werden Ihnen diese Standardvorlage zwei Seiten besuchen und eine grundlegende Anmeldeseite.</span><span class="sxs-lookup"><span data-stu-id="7303f-159">Right out of the box this default template gives you two pages to visit and a basic login page.</span></span> <span data-ttu-id="7303f-160">Der nächste Schritt besteht ändern, wie diese Anwendung funktioniert, und erfahren etwas über ASP.NET MVC im Prozess.</span><span class="sxs-lookup"><span data-stu-id="7303f-160">The next step is to change how this application works and learn a little bit about ASP.NET MVC in the process.</span></span> <span data-ttu-id="7303f-161">Schließen Sie Ihren Browser, und ändern Sie Code.</span><span class="sxs-lookup"><span data-stu-id="7303f-161">Close your browser and let's change some code.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="7303f-162">Nächste</span><span class="sxs-lookup"><span data-stu-id="7303f-162">Next</span></span>](adding-a-controller.md)
