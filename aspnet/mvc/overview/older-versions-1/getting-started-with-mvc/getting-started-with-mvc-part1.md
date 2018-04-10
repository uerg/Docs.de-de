---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
title: Einführung in ASP.NET MVC | Microsoft Docs
author: shanselman
description: Dies ist ein Anfänger-Lernprogramm, das die Grundlagen von ASP.NET MVC. Erstellen Sie eine einfache Web-Anwendung, die Lese- und Schreibvorgänge aus einer Datenbank.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: bf4a1c19-0a94-4208-b268-a96ddcf26946
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
msc.type: authoredcontent
ms.openlocfilehash: 476d832e389b9b5a26fe2d552ca648c79b100056
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/10/2018
---
<a name="intro-to-aspnet-mvc"></a><span data-ttu-id="94c3d-104">Einführung in ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="94c3d-104">Intro to ASP.NET MVC</span></span>
====================
<span data-ttu-id="94c3d-105">durch [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="94c3d-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> > [!NOTE]
> > <span data-ttu-id="94c3d-106">Eine aktualisierte Version ist in diesem Lernprogramm verfügbar [hier](../../getting-started/introduction/getting-started.md) mit [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads).</span><span class="sxs-lookup"><span data-stu-id="94c3d-106">An updated version if this tutorial is available [here](../../getting-started/introduction/getting-started.md) using [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads).</span></span> <span data-ttu-id="94c3d-107">Das neue Lernprogramm verwendet ASP.NET MVC 5, viele Verbesserungen über dieses Lernprogramms bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="94c3d-107">The new tutorial uses ASP.NET MVC 5, which provides many improvements over this tutorial.</span></span>
> 
> 
> <span data-ttu-id="94c3d-108">Dies ist ein Anfänger-Lernprogramm, das die Grundlagen von ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="94c3d-108">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="94c3d-109">Erstellen Sie eine einfache Web-Anwendung, die Lese- und Schreibvorgänge aus einer Datenbank.</span><span class="sxs-lookup"><span data-stu-id="94c3d-109">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="94c3d-110">Besuchen Sie die [ASP.NET MVC-Trainingscenter](../../../index.md) andere ASP.NET-MVC Lernprogramme und Beispiele finden.</span><span class="sxs-lookup"><span data-stu-id="94c3d-110">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="94c3d-111">Wir machen unsere ersten ASP.NET-MVC-Webanwendung mit [Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/).</span><span class="sxs-lookup"><span data-stu-id="94c3d-111">Let's make our first ASP.NET MVC Web Application using [Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/).</span></span> <span data-ttu-id="94c3d-112">Treffen wir eine kleine Film-List-Anwendung, die wir erstellen und Filme auflisten.</span><span class="sxs-lookup"><span data-stu-id="94c3d-112">We'll make a little Movie list application that will let us create and list movies.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="94c3d-113">Was müssen Sie erstellen</span><span class="sxs-lookup"><span data-stu-id="94c3d-113">What You'll Build</span></span>

<span data-ttu-id="94c3d-114">Hier sind zwei Screenshots der Anwendung, die Sie erstellen müssen.</span><span class="sxs-lookup"><span data-stu-id="94c3d-114">Here are two screenshots of the application you'll build.</span></span> <span data-ttu-id="94c3d-115">Sie müssen eine einfache Tabelle von Filmen mit verschiedenen Spalten.</span><span class="sxs-lookup"><span data-stu-id="94c3d-115">You will have a simple table of movies with various columns.</span></span>

<span data-ttu-id="94c3d-116">[![Film List – Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="94c3d-116">[![Movie List - Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)</span></span>

<span data-ttu-id="94c3d-117">Und Sie müssen ein Formular erstellen, damit Filme zur Liste hinzugefügt werden können.</span><span class="sxs-lookup"><span data-stu-id="94c3d-117">And you'll have a Create Form so we can add movies to the list.</span></span>

<span data-ttu-id="94c3d-118">[![Create a Movie - Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="94c3d-118">[![Create a Movie - Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)</span></span>

## <a name="skills-youll-learn"></a><span data-ttu-id="94c3d-119">Fähigkeiten, die Sie erfahren</span><span class="sxs-lookup"><span data-stu-id="94c3d-119">Skills You'll Learn</span></span>

<span data-ttu-id="94c3d-120">In diesem Lernprogramm erfahren Sie die Grundlagen der Erstellung einer ASP.NET MVC-Web-Anwendung mit Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="94c3d-120">This tutorial will teach you the basics of building an ASP.NET MVC Web Application using Visual Studio.</span></span> <span data-ttu-id="94c3d-121">Erfahren Sie:</span><span class="sxs-lookup"><span data-stu-id="94c3d-121">You'll learn:</span></span>

- <span data-ttu-id="94c3d-122">Vorgehensweise: Erstellen Sie ein neues ASP.NET MVC-Projekt</span><span class="sxs-lookup"><span data-stu-id="94c3d-122">How to create a new ASP.NET MVC Project</span></span>
- <span data-ttu-id="94c3d-123">Gewusst wie: Erstellen einer neuen Datenbank mit SQL Server</span><span class="sxs-lookup"><span data-stu-id="94c3d-123">How to create a new Database with SQL Server</span></span>
- <span data-ttu-id="94c3d-124">Vorgehensweise: Erstellen von ASP.NET MVC-Controller und Ansichten</span><span class="sxs-lookup"><span data-stu-id="94c3d-124">How to create ASP.NET MVC Controllers and Views</span></span>
- <span data-ttu-id="94c3d-125">Das Abrufen und Anzeigen von Daten</span><span class="sxs-lookup"><span data-stu-id="94c3d-125">How to retrieve and display data</span></span>
- <span data-ttu-id="94c3d-126">Zum Bearbeiten von Daten und aktivieren Sie die datenüberprüfung</span><span class="sxs-lookup"><span data-stu-id="94c3d-126">How to edit data and enable data validation</span></span>
- <span data-ttu-id="94c3d-127">Vorgehensweise beim Aktualisieren des Datenbankschemas</span><span class="sxs-lookup"><span data-stu-id="94c3d-127">How to update the database schema</span></span>

## <a name="get-started"></a><span data-ttu-id="94c3d-128">Erste Schritte</span><span class="sxs-lookup"><span data-stu-id="94c3d-128">Get Started</span></span>

<span data-ttu-id="94c3d-129">Starten Sie durch Ausführen von Visual Web Developer 2010 Express (Ich werde sie von nun an "VWD" aufrufen), und wählen Sie Neues Projekt aus den Startbildschirm.</span><span class="sxs-lookup"><span data-stu-id="94c3d-129">Start by running Visual Web Developer 2010 Express (I'll call it "VWD" from now on) and select New Project from the Start Screen.</span></span>

<span data-ttu-id="94c3d-130">Visual Web Developer ist eine IDE oder Integrated Developer Environment an.</span><span class="sxs-lookup"><span data-stu-id="94c3d-130">Visual Web Developer is an IDE, or Integrated Developer Environment.</span></span> <span data-ttu-id="94c3d-131">Wie Sie Microsoft Word verwenden, um Dokumente zu schreiben, verwenden Sie eine IDE zum Erstellen von Anwendungen.</span><span class="sxs-lookup"><span data-stu-id="94c3d-131">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="94c3d-132">Es wird eine Symbolleiste am oberen Rand mit verschiedenen verfügbaren Optionen, die Sie als auch im Menü, das Sie hätte auch auf die Datei auswählen | Neues Projekt.</span><span class="sxs-lookup"><span data-stu-id="94c3d-132">There's a toolbar along the top showing various options available to you, as well as the menu you could also have used to Select File | New project.</span></span>

<span data-ttu-id="94c3d-133">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="94c3d-133">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)</span></span>

## <a name="creating-your-first-application"></a><span data-ttu-id="94c3d-134">Erstellen einer ersten Anwendung</span><span class="sxs-lookup"><span data-stu-id="94c3d-134">Creating Your First Application</span></span>

<span data-ttu-id="94c3d-135">Sie können Anwendungen mit Visual Basic oder Visual c# erstellen.</span><span class="sxs-lookup"><span data-stu-id="94c3d-135">You can create applications using Visual Basic or Visual C#.</span></span> <span data-ttu-id="94c3d-136">Jetzt, wählen Sie Visual c# auf der linken Seite Wählen Sie "ASP.NET MVC 2-Webanwendung".</span><span class="sxs-lookup"><span data-stu-id="94c3d-136">For now, Select Visual C# on the left, then pick "ASP.NET MVC 2 Web Application."</span></span> <span data-ttu-id="94c3d-137">Namen für das Projekt "Videos", und klicken Sie auf OK.</span><span class="sxs-lookup"><span data-stu-id="94c3d-137">Name your project "Movies" and click OK.</span></span>

<span data-ttu-id="94c3d-138">[![Neues Projekt](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="94c3d-138">[![New Project](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)</span></span>

<span data-ttu-id="94c3d-139">Auf die rechte Seite ist im Projektmappen-Explorer die Dateien und Ordner in der Anwendung angezeigt.</span><span class="sxs-lookup"><span data-stu-id="94c3d-139">On the right-hand side is the Solution Explorer showing all the files and folders in your application.</span></span> <span data-ttu-id="94c3d-140">Das große Fenster in der Mitte ist, in dem Code bearbeiten und die meiste Ihrer Zeit in Anspruch.</span><span class="sxs-lookup"><span data-stu-id="94c3d-140">The big window in the middle is where you edit your code and spend most of your time.</span></span> <span data-ttu-id="94c3d-141">Visual Studio verwendet eine Standardvorlage für das ASP.NET MVC-Projekt, das Sie gerade erstellt haben, das müssen Sie eine funktionierende Anwendung jetzt ohne jegliche!</span><span class="sxs-lookup"><span data-stu-id="94c3d-141">Visual Studio used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="94c3d-142">Dies ist eine einfache "Hello World!</span><span class="sxs-lookup"><span data-stu-id="94c3d-142">This is a simple "Hello World!</span></span> <span data-ttu-id="94c3d-143">Projekt, und es ist eine gute, für die Anwendung zu starten.</span><span class="sxs-lookup"><span data-stu-id="94c3d-143">project, and it is a good place to start for our application.</span></span>

<span data-ttu-id="94c3d-144">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="94c3d-144">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)</span></span>

<span data-ttu-id="94c3d-145">Wählen Sie die Schaltfläche "Wiedergabe" auf der Symbolleiste.</span><span class="sxs-lookup"><span data-stu-id="94c3d-145">Select the "play" button to the toolbar.</span></span>

![Debugging starten](getting-started-with-mvc-part1/_static/image11.png)

<span data-ttu-id="94c3d-147">Es ist ein grüner Pfeil nach rechts, die das Programm zu kompilieren und starten Sie die Anwendung in einem Webbrowser.</span><span class="sxs-lookup"><span data-stu-id="94c3d-147">It's a green arrow pointing to the right that will compile your program and start your application in a web browser.</span></span>

<span data-ttu-id="94c3d-148">*Hinweis: Sie können stattdessen drücken Sie F5, oder wählen Sie Debug -&gt;Starten des Debuggens aus dem Menü "Debuggen".*</span><span class="sxs-lookup"><span data-stu-id="94c3d-148">*NOTE: You can instead press F5 on your keyboard, or select Debug-&gt;Start Debugging from the "Debug" menu.*</span></span>

<span data-ttu-id="94c3d-149">Dadurch wird Visual Web Developer zum Starten eines entwicklungswebservers, und führen unsere Webanwendung (es gibt keine Konfiguration oder manuelle Schritte erforderlich, um dies zu ermöglichen).</span><span class="sxs-lookup"><span data-stu-id="94c3d-149">This will cause Visual Web Developer to start a development web-server and run our web application (there are no configuration or manual steps required to enable this).</span></span> <span data-ttu-id="94c3d-150">Anschließend wird einen Browser starten und konfigurieren, dass die Anwendung-Startseite zu navigieren.</span><span class="sxs-lookup"><span data-stu-id="94c3d-150">It will then launch a browser and configure it to browse the application's home-page.</span></span> <span data-ttu-id="94c3d-151">Unten können sehen Sie, dass die Adressleiste des Browsers "Localhost" und nicht etwa ' example.com ' lautet. Das liegt daran "localhost" stets auf Ihren eigenen lokalen Computer - Zeigt die in diesem Fall die Anwendung ausgeführt wird, die wir gerade erstellt.</span><span class="sxs-lookup"><span data-stu-id="94c3d-151">Notice below that the address bar of the browser says "localhost", and not something like example.com. That's because localhost always points to your own local computer - which in this case is running the application we just built.</span></span>

<span data-ttu-id="94c3d-152">[![Startseite](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="94c3d-152">[![Home Page](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)</span></span>

<span data-ttu-id="94c3d-153">Direktes bietet diese Standardvorlage Sie zwei Seiten besuchen und eine grundlegende Anmeldeseite.</span><span class="sxs-lookup"><span data-stu-id="94c3d-153">Out of the box this default template gives you two pages to visit and a basic login page.</span></span> <span data-ttu-id="94c3d-154">Wir ändern, wie diese Anwendung funktioniert, und erfahren Sie etwas ASP.NET-MVC im Prozess.</span><span class="sxs-lookup"><span data-stu-id="94c3d-154">Let's change how this application works and learn a little bit about ASP.NET MVC in the process.</span></span> <span data-ttu-id="94c3d-155">Schließen Sie Ihren Browser, und ermöglicht es, Code zu ändern.</span><span class="sxs-lookup"><span data-stu-id="94c3d-155">Close your browser and lets change some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="94c3d-156">Nächste</span><span class="sxs-lookup"><span data-stu-id="94c3d-156">Next</span></span>](getting-started-with-mvc-part2.md)
