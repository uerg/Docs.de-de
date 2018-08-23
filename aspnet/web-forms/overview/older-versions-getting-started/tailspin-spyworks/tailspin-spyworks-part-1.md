---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
title: 'Teil 1: Datei -> Neues Projekt | Microsoft-Dokumentation'
author: JoeStagner
description: Dieser tutorialreihe werden alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen. Teil 1 enthält die Übersicht über "und" Datei/neu Projekt.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 15d4652b-d5aa-4172-b186-2c7f96ba316d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
msc.type: authoredcontent
ms.openlocfilehash: d2e4b36c9029e86eea9b09974839e96e9aa39ced
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828040"
---
<a name="part-1-file--new-project"></a><span data-ttu-id="1c302-104">Teil 1: Datei -> Neues Projekt</span><span class="sxs-lookup"><span data-stu-id="1c302-104">Part 1: File-> New Project</span></span>
====================
<span data-ttu-id="1c302-105">durch [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="1c302-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="1c302-106">Tailspin Spyworks wird veranschaulicht, wie außerordentlich einfach es ist, erstellen Sie leistungsstarke, skalierbare Anwendungen für die .NET-Plattform.</span><span class="sxs-lookup"><span data-stu-id="1c302-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="1c302-107">Es wird gezeigt, aus wie die hervorragenden neuen Funktionen in ASP.NET 4 zu verwenden, um eine online-Store, einschließlich der Warenkorb, Auschecken und Verwaltung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="1c302-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="1c302-108">Dieser tutorialreihe werden alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen.</span><span class="sxs-lookup"><span data-stu-id="1c302-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="1c302-109">Teil 1 enthält die Übersicht über "und" Datei/neu Projekt.</span><span class="sxs-lookup"><span data-stu-id="1c302-109">Part 1 covers Overview and File/New Project.</span></span>


## <a id="_Toc260221666"></a>  <span data-ttu-id="1c302-110">Übersicht über die</span><span class="sxs-lookup"><span data-stu-id="1c302-110">Overview</span></span>

<span data-ttu-id="1c302-111">Dieses Tutorial bietet eine Einführung in ASP.NET WebForms.</span><span class="sxs-lookup"><span data-stu-id="1c302-111">This tutorial is an introduction to ASP.NET WebForms.</span></span> <span data-ttu-id="1c302-112">Wir langsam starten, damit für Anfänger auf Web-Entwicklung ist in Ordnung.</span><span class="sxs-lookup"><span data-stu-id="1c302-112">We'll be starting slowly, so beginner level web development experience is okay.</span></span>

<span data-ttu-id="1c302-113">Die Anwendung, die wir erstellen, ist es sich um einen einfachen Onlineshop.</span><span class="sxs-lookup"><span data-stu-id="1c302-113">The application we'll be building is a simple on-line store.</span></span>

![](tailspin-spyworks-part-1/_static/image1.jpg)


<span data-ttu-id="1c302-114">Besucher können Produkte nach Kategorie durchsuchen:</span><span class="sxs-lookup"><span data-stu-id="1c302-114">Visitors can browse Products by Category:</span></span>

![](tailspin-spyworks-part-1/_static/image2.jpg)

<span data-ttu-id="1c302-115">Sie können ein einzelnes Produkt anzuzeigen und zu Einkaufswagen hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="1c302-115">They can view a single product and add it to their cart:</span></span>

![](tailspin-spyworks-part-1/_static/image3.jpg)

<span data-ttu-id="1c302-116">Sie können überprüfen, Einkaufswagen, entfernen alle Elemente, die sie nicht mehr benötigen:</span><span class="sxs-lookup"><span data-stu-id="1c302-116">They can review their cart, removing any items they no longer want:</span></span>

![](tailspin-spyworks-part-1/_static/image4.jpg)

<span data-ttu-id="1c302-117">Sie zur Kasse gehen werden sie aufgefordert.</span><span class="sxs-lookup"><span data-stu-id="1c302-117">Proceeding to Checkout will prompt them to</span></span>

![](tailspin-spyworks-part-1/_static/image5.jpg)

![](tailspin-spyworks-part-1/_static/image6.jpg)

<span data-ttu-id="1c302-118">Nach dem Sortieren, sehen sie ein einfaches Bestätigungsbildschirm angezeigt:</span><span class="sxs-lookup"><span data-stu-id="1c302-118">After ordering, they see a simple confirmation screen:</span></span>

![](tailspin-spyworks-part-1/_static/image7.jpg)


<span data-ttu-id="1c302-119">Wir beginnen, indem das Erstellen eines neuen ASP.NET WebForms-Projekts in Visual Studio 2010, und wir werden Funktionen zum Erstellen einer vollständigen funktionsfähigen Anwendung inkrementell hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="1c302-119">We'll begin by creating a new ASP.NET WebForms project in Visual Studio 2010, and we'll incrementally add features to create a complete functioning application.</span></span> <span data-ttu-id="1c302-120">Dabei wird die behandelt mit Masterseiten für konsistente Seitenlayout, AJAX, Validierung, Benutzermitgliedschaft und mehr Zugriff auf die Datenbank, Listen und Raster, Update von Datenseiten, datenüberprüfung.</span><span class="sxs-lookup"><span data-stu-id="1c302-120">Along the way, we'll cover database access, list and grid views, data update pages, data validation, using master pages for consistent page layout, AJAX, validation, user membership, and more.</span></span>

<span data-ttu-id="1c302-121">Sie Schritt für Schritt nachvollziehen können, oder Sie können die fertige Anwendung herunterladen [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)</span><span class="sxs-lookup"><span data-stu-id="1c302-121">You can follow along step by step, or you can download the completed application from [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)</span></span>

<span data-ttu-id="1c302-122">Sie können entweder Visual Studio 2010 oder die kostenlose Visual Web Developer 2010 aus [ https://www.microsoft.com/express/Web/ ](https://www.microsoft.com/express/Web/).</span><span class="sxs-lookup"><span data-stu-id="1c302-122">You can use either Visual Studio 2010 or the free Visual Web Developer 2010 from [https://www.microsoft.com/express/Web/](https://www.microsoft.com/express/Web/).</span></span> <span data-ttu-id="1c302-123">Um die Anwendung zu erstellen, können Sie entweder SQL Server oder die kostenlose SQL Server Express, die die Datenbank hosten.</span><span class="sxs-lookup"><span data-stu-id="1c302-123">To build the application, you can use either SQL Server or the free SQL Server Express to host the database.</span></span>

## <a id="_Toc260221667"></a>  <span data-ttu-id="1c302-124">Datei / neu Projekt</span><span class="sxs-lookup"><span data-stu-id="1c302-124">File / New Project</span></span>

<span data-ttu-id="1c302-125">Wir beginnen, indem Sie das neue Projekt über das Menü "Datei" in Visual Studio auswählen.</span><span class="sxs-lookup"><span data-stu-id="1c302-125">We'll start by selecting the New Project from the File menu in Visual Studio.</span></span> <span data-ttu-id="1c302-126">Daraufhin wird das Dialogfeld "Neues Projekt".</span><span class="sxs-lookup"><span data-stu-id="1c302-126">This brings up the New Project dialog.</span></span>

![](tailspin-spyworks-part-1/_static/image8.jpg)

<span data-ttu-id="1c302-127">Ich wähle Visual c# / Web-Vorlagen auf der linken Seite, und wählen Sie dann die Vorlage "ASP.NET Web Application" in der mittleren Spalte.</span><span class="sxs-lookup"><span data-stu-id="1c302-127">We'll select the Visual C# / Web Templates group on the left, and then choose the "ASP.NET Web Application" template in the center column.</span></span> <span data-ttu-id="1c302-128">Benennen Sie Ihr Projekt TailspinSpyworks aus, und drücken Sie die Schaltfläche "OK".</span><span class="sxs-lookup"><span data-stu-id="1c302-128">Name your project TailspinSpyworks and press the OK button.</span></span>

![](tailspin-spyworks-part-1/_static/image9.jpg)

<span data-ttu-id="1c302-129">Dadurch wird das Projekt erstellt.</span><span class="sxs-lookup"><span data-stu-id="1c302-129">This will create our project.</span></span> <span data-ttu-id="1c302-130">Werfen wir einen Blick auf die Ordner, die in unserer Anwendung im Projektmappen-Explorer auf der rechten Seite enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="1c302-130">Let's take a look at the folders that are included in our application in the Solution Explorer on the right side.</span></span>

![](tailspin-spyworks-part-1/_static/image10.jpg)

<span data-ttu-id="1c302-131">Die leere Projektmappe nicht vollständig leer ist – eine einfache Ordnerstruktur hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="1c302-131">The Empty Solution isn't completely empty – it adds a basic folder structure:</span></span>

![](tailspin-spyworks-part-1/_static/image1.png)

<span data-ttu-id="1c302-132">Beachten Sie die Konventionen, die von der Projektvorlage für ASP.NET 4 standardmäßig implementiert.</span><span class="sxs-lookup"><span data-stu-id="1c302-132">Note the conventions implemented by the ASP.NET 4 default project template.</span></span>

- <span data-ttu-id="1c302-133">Der Ordner "Konto" implementiert eine einfache Benutzeroberfläche für ASP. NET Subsystems für die Mitgliedschaft.</span><span class="sxs-lookup"><span data-stu-id="1c302-133">The "Account" folder implements a basic user interface for ASP.NET's membership subsystem.</span></span>
- <span data-ttu-id="1c302-134">Der Ordner "Scripts" dient als Repository für clientseitige JavaScript-Dateien, und die Core-jQuery-JS-Dateien werden standardmäßig zur Verfügung gestellt.</span><span class="sxs-lookup"><span data-stu-id="1c302-134">The "Scripts" folder serves as the repository for client side JavaScript files and the core jQuery .js files are made available by default.</span></span>
- <span data-ttu-id="1c302-135">Der Ordner "Stile" wird verwendet, um unsere Website Visuals (CSS-Stylesheets) organisieren</span><span class="sxs-lookup"><span data-stu-id="1c302-135">The "Styles" folder is used to organize our web site visuals (CSS Style Sheets)</span></span>

<span data-ttu-id="1c302-136">Wenn wir F5 drücken, um die Anwendung auszuführen und das Rendern der Seite "default.aspx" wird Folgendes angezeigt.</span><span class="sxs-lookup"><span data-stu-id="1c302-136">When we press F5 to run our application and render the default.aspx page we see the following.</span></span>

![](tailspin-spyworks-part-1/_static/image11.jpg)

<span data-ttu-id="1c302-137">Ersetzen Sie die Datei "Style.CSS" aus der Standardvorlage für Web Forms, mit dem CSS-Klassen und das zugehörige Image-Dateien, die das visual Asthetics gerendert werden, die wir für unsere Anwendung Tailspin Spyworks möchten, ist unsere erste Anwendung-Erweiterung werden.</span><span class="sxs-lookup"><span data-stu-id="1c302-137">Our first application enhancement will be to replace the Style.css file from the default WebForms template with the CSS classes and associated image files that will render the visual asthetics that we want for our Tailspin Spyworks application.</span></span>

<span data-ttu-id="1c302-138">Danach rendert unsere Seite "default.aspx" wie folgt aus.</span><span class="sxs-lookup"><span data-stu-id="1c302-138">After doing so our default.aspx page renders like this.</span></span>

![](tailspin-spyworks-part-1/_static/image12.jpg)

<span data-ttu-id="1c302-139">Beachten Sie, dass die Abbildung Links oben rechts auf der Seite und die Menüelemente, die auf die Masterseite hinzugefügt wurden.</span><span class="sxs-lookup"><span data-stu-id="1c302-139">Notice the image links at the top right of the page and the menu items that have been added to the master page.</span></span> <span data-ttu-id="1c302-140">Zeigen Sie nur die Links "Sign In" und "Konto" auf Seiten, die vorhanden sind (generiert durch die Standardvorlage) und den Rest der Seiten, die wir implementiert wird, wenn wir unsere Anwendung erstellen.</span><span class="sxs-lookup"><span data-stu-id="1c302-140">Only the "Sign In" and "Account" links point to pages that exist (generated by the default template) and the rest of the pages we will implement as we build our application.</span></span>

<span data-ttu-id="1c302-141">Wir werden auch die Masterseite in das Verzeichnis Stile zu verschieben.</span><span class="sxs-lookup"><span data-stu-id="1c302-141">We're also going to relocate the Master Page to the Styles directory.</span></span> <span data-ttu-id="1c302-142">Obwohl dies nur eine Einstellung ist kann es etwas einfacher zu machen, wenn wir uns entschieden, unsere Anwendung in der Zukunft "skinable" zu machen.</span><span class="sxs-lookup"><span data-stu-id="1c302-142">Though this is only a preference it may make things a little easier if we decide to make our application "skinable" in the future.</span></span>

<span data-ttu-id="1c302-143">Danach ändern Sie die Masterseite müssen Verweise in allen ASPX-Dateien standardmäßig generiert der ASP.NET Web Forms-Seiten.</span><span class="sxs-lookup"><span data-stu-id="1c302-143">After doing this we'll need to change the master page references in all the .aspx files generated by the default ASP.NET WebForms pages.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="1c302-144">Nächste</span><span class="sxs-lookup"><span data-stu-id="1c302-144">Next</span></span>](tailspin-spyworks-part-2.md)
