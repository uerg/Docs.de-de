---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
title: 'Teil 1: Neues Projekt für Datei -> | Microsoft Docs'
author: JoeStagner
description: Diese Reihe von Lernprogrammen sind alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen. Teil 1 werden (Übersicht) "und" Datei/neu Projekt behandelt.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 15d4652b-d5aa-4172-b186-2c7f96ba316d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
msc.type: authoredcontent
ms.openlocfilehash: a1b9681516e626b6a0eec420b168a74e05d88afb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="part-1-file--new-project"></a><span data-ttu-id="96db5-104">Teil 1: Datei -> Neues Projekt</span><span class="sxs-lookup"><span data-stu-id="96db5-104">Part 1: File-> New Project</span></span>
====================
<span data-ttu-id="96db5-105">durch [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="96db5-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="96db5-106">Tailspin Spyworks wird veranschaulicht, wie außergewöhnlich einfache ist die leistungsstarke, skalierbare Anwendungen für .NET-Plattform zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="96db5-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="96db5-107">Es wird gezeigt, aus wie die hervorragenden neuen Funktionen in ASP.NET 4 mit um einen Onlineshop, einschließlich Warenkorb, Auschecken und Verwaltung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="96db5-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="96db5-108">Diese Reihe von Lernprogrammen sind alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen.</span><span class="sxs-lookup"><span data-stu-id="96db5-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="96db5-109">Teil 1 werden (Übersicht) "und" Datei/neu Projekt behandelt.</span><span class="sxs-lookup"><span data-stu-id="96db5-109">Part 1 covers Overview and File/New Project.</span></span>


## <a id="_Toc260221666"></a>  <span data-ttu-id="96db5-110">Übersicht über</span><span class="sxs-lookup"><span data-stu-id="96db5-110">Overview</span></span>

<span data-ttu-id="96db5-111">Dieses Lernprogramm ist eine Einführung in ASP.NET Web Forms.</span><span class="sxs-lookup"><span data-stu-id="96db5-111">This tutorial is an introduction to ASP.NET WebForms.</span></span> <span data-ttu-id="96db5-112">Wir werden langsam gestartet werden, damit Anfänger Ebene Webentwicklung auftreten ist in Ordnung.</span><span class="sxs-lookup"><span data-stu-id="96db5-112">We'll be starting slowly, so beginner level web development experience is okay.</span></span>

<span data-ttu-id="96db5-113">Die Anwendung, die wir erstellen werden müssen ist eine einfache Online Store.</span><span class="sxs-lookup"><span data-stu-id="96db5-113">The application we'll be building is a simple on-line store.</span></span>

![](tailspin-spyworks-part-1/_static/image1.jpg)


<span data-ttu-id="96db5-114">Besucher können nach Produkten nach Kategorie zu suchen:</span><span class="sxs-lookup"><span data-stu-id="96db5-114">Visitors can browse Products by Category:</span></span>

![](tailspin-spyworks-part-1/_static/image2.jpg)

<span data-ttu-id="96db5-115">Sie können ein einzelnes Produkt anzuzeigen und Einkaufswagen hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="96db5-115">They can view a single product and add it to their cart:</span></span>

![](tailspin-spyworks-part-1/_static/image3.jpg)

<span data-ttu-id="96db5-116">Sie können überprüfen, dass Einkaufswagen entfernt alle Elemente, die sie ohne mehr benötigen:</span><span class="sxs-lookup"><span data-stu-id="96db5-116">They can review their cart, removing any items they no longer want:</span></span>

![](tailspin-spyworks-part-1/_static/image4.jpg)

<span data-ttu-id="96db5-117">Sie zur Kasse gehen wird zur Eingabe aufgefordert</span><span class="sxs-lookup"><span data-stu-id="96db5-117">Proceeding to Checkout will prompt them to</span></span>

![](tailspin-spyworks-part-1/_static/image5.jpg)

![](tailspin-spyworks-part-1/_static/image6.jpg)

<span data-ttu-id="96db5-118">Nach dem Sortieren, eine einfache Bestätigungsbildschirm angezeigt:</span><span class="sxs-lookup"><span data-stu-id="96db5-118">After ordering, they see a simple confirmation screen:</span></span>

![](tailspin-spyworks-part-1/_static/image7.jpg)


<span data-ttu-id="96db5-119">Wir beginnen, indem Sie ein neues ASP.NET Web Forms-Projekt in Visual Studio 2010 erstellen, und wir werden die Funktionen, um eine vollständig funktionierende Anwendung erstellen inkrementell hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="96db5-119">We'll begin by creating a new ASP.NET WebForms project in Visual Studio 2010, and we'll incrementally add features to create a complete functioning application.</span></span> <span data-ttu-id="96db5-120">Nebenbei müssen eingegangen mit Masterseiten für konsistente Seitenlayout, AJAX, Validierung, die Benutzermitgliedschaft und Datenbankzugriff, Listen und Raster, Update von Datenseiten, Validierung.</span><span class="sxs-lookup"><span data-stu-id="96db5-120">Along the way, we'll cover database access, list and grid views, data update pages, data validation, using master pages for consistent page layout, AJAX, validation, user membership, and more.</span></span>

<span data-ttu-id="96db5-121">Sie Schritt für Schritt nachvollziehen können, oder Sie können aus die fertigen Anwendung herunterladen [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)</span><span class="sxs-lookup"><span data-stu-id="96db5-121">You can follow along step by step, or you can download the completed application from [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)</span></span>

<span data-ttu-id="96db5-122">Sie können Visual Studio 2010 oder das kostenlose Visual Web Developer 2010 aus [ https://www.microsoft.com/express/Web/ ](https://www.microsoft.com/express/Web/).</span><span class="sxs-lookup"><span data-stu-id="96db5-122">You can use either Visual Studio 2010 or the free Visual Web Developer 2010 from [https://www.microsoft.com/express/Web/](https://www.microsoft.com/express/Web/).</span></span> <span data-ttu-id="96db5-123">Um die Anwendung zu erstellen, können Sie entweder SQL Server oder den freien SQL Server Express auf Host der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="96db5-123">To build the application, you can use either SQL Server or the free SQL Server Express to host the database.</span></span>

## <a id="_Toc260221667"></a>  <span data-ttu-id="96db5-124">Datei / neues Projekt</span><span class="sxs-lookup"><span data-stu-id="96db5-124">File / New Project</span></span>

<span data-ttu-id="96db5-125">Wir beginnen, indem Sie das neue Projekt über das Menü Datei in Visual Studio auswählen.</span><span class="sxs-lookup"><span data-stu-id="96db5-125">We'll start by selecting the New Project from the File menu in Visual Studio.</span></span> <span data-ttu-id="96db5-126">Daraufhin wird das Dialogfeld "Neues Projekt".</span><span class="sxs-lookup"><span data-stu-id="96db5-126">This brings up the New Project dialog.</span></span>

![](tailspin-spyworks-part-1/_static/image8.jpg)

<span data-ttu-id="96db5-127">Wir wählen die Visual C#- / Webvorlagen Gruppe auf der linken Seite, und wählen Sie dann die Vorlage "ASP.NET Web-Anwendung" in der mittleren Spalte.</span><span class="sxs-lookup"><span data-stu-id="96db5-127">We'll select the Visual C# / Web Templates group on the left, and then choose the "ASP.NET Web Application" template in the center column.</span></span> <span data-ttu-id="96db5-128">Namen für das Projekt TailspinSpyworks, und drücken Sie die Schaltfläche "OK".</span><span class="sxs-lookup"><span data-stu-id="96db5-128">Name your project TailspinSpyworks and press the OK button.</span></span>

![](tailspin-spyworks-part-1/_static/image9.jpg)

<span data-ttu-id="96db5-129">Dadurch wird unsere Projekt erstellt.</span><span class="sxs-lookup"><span data-stu-id="96db5-129">This will create our project.</span></span> <span data-ttu-id="96db5-130">Werfen wir einen Blick auf die Ordner, die in der vorliegenden Anwendung im Projektmappen-Explorer auf der rechten Seite enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="96db5-130">Let's take a look at the folders that are included in our application in the Solution Explorer on the right side.</span></span>

![](tailspin-spyworks-part-1/_static/image10.jpg)

<span data-ttu-id="96db5-131">Leere Projektmappe nicht vollständig leer ist – er fügt eine grundlegende Ordnerstruktur:</span><span class="sxs-lookup"><span data-stu-id="96db5-131">The Empty Solution isn't completely empty – it adds a basic folder structure:</span></span>

![](tailspin-spyworks-part-1/_static/image1.png)

<span data-ttu-id="96db5-132">Beachten Sie die Konventionen der Projektvorlage für ASP.NET 4-Standard implementiert.</span><span class="sxs-lookup"><span data-stu-id="96db5-132">Note the conventions implemented by the ASP.NET 4 default project template.</span></span>

- <span data-ttu-id="96db5-133">Der Ordner "Konto" implementiert eine einfache Benutzeroberfläche für das ASP. NET Mitgliedschaft Subsystem.</span><span class="sxs-lookup"><span data-stu-id="96db5-133">The "Account" folder implements a basic user interface for ASP.NET's membership subsystem.</span></span>
- <span data-ttu-id="96db5-134">Der Ordner "Skripts" dient als Repository für Client-Side-JavaScript-Dateien und die Core jQuery JS-Dateien werden standardmäßig zur Verfügung gestellt.</span><span class="sxs-lookup"><span data-stu-id="96db5-134">The "Scripts" folder serves as the repository for client side JavaScript files and the core jQuery .js files are made available by default.</span></span>
- <span data-ttu-id="96db5-135">Der Ordner "Formatvorlagen" wird verwendet, um unsere Website Visualisierungen (Cascading Stylesheets) zu organisieren.</span><span class="sxs-lookup"><span data-stu-id="96db5-135">The "Styles" folder is used to organize our web site visuals (CSS Style Sheets)</span></span>

<span data-ttu-id="96db5-136">Wenn wir drücken Sie F5, um die Anwendung ausführen und Rendern der Seite "default.aspx" wird Folgendes angezeigt.</span><span class="sxs-lookup"><span data-stu-id="96db5-136">When we press F5 to run our application and render the default.aspx page we see the following.</span></span>

![](tailspin-spyworks-part-1/_static/image11.jpg)

<span data-ttu-id="96db5-137">Unsere erste Anwendung Erweiterung werden, ersetzen Sie die Datei "Style.css" aus der WebForms-Standardvorlage durch die CSS-Klassen und zugehörige Abbild-Dateien, die visual Asthetics gerendert werden, die wir für unsere Tailspin Spyworks-Anwendung ein.</span><span class="sxs-lookup"><span data-stu-id="96db5-137">Our first application enhancement will be to replace the Style.css file from the default WebForms template with the CSS classes and associated image files that will render the visual asthetics that we want for our Tailspin Spyworks application.</span></span>

<span data-ttu-id="96db5-138">Nach der Anmeldung rendert unsere Seite "default.aspx" wie folgt aus.</span><span class="sxs-lookup"><span data-stu-id="96db5-138">After doing so our default.aspx page renders like this.</span></span>

![](tailspin-spyworks-part-1/_static/image12.jpg)

<span data-ttu-id="96db5-139">Beachten Sie das Bildhyperlinks oben rechts auf der Seite und die Menüelemente, die der Masterseite hinzugefügt wurden.</span><span class="sxs-lookup"><span data-stu-id="96db5-139">Notice the image links at the top right of the page and the menu items that have been added to the master page.</span></span> <span data-ttu-id="96db5-140">Nur die Links "Anmelden" und "Konto" zeigen Sie auf Seiten, die vorhanden sind (von der Standardvorlage generiert) und den Rest der Seiten, die wir implementieren, wenn wir unsere Anwendung erstellen.</span><span class="sxs-lookup"><span data-stu-id="96db5-140">Only the "Sign In" and "Account" links point to pages that exist (generated by the default template) and the rest of the pages we will implement as we build our application.</span></span>

<span data-ttu-id="96db5-141">Wir werden auch die Gestaltungsvorlage in das Verzeichnis Stile zu verschieben.</span><span class="sxs-lookup"><span data-stu-id="96db5-141">We're also going to relocate the Master Page to the Styles directory.</span></span> <span data-ttu-id="96db5-142">Obwohl dies nur eine Einstellung ist kann es etwas Vereinfachung Wenn wir unsere Anwendung in der Zukunft "skinable" machen möchten.</span><span class="sxs-lookup"><span data-stu-id="96db5-142">Though this is only a preference it may make things a little easier if we decide to make our application "skinable" in the future.</span></span>

<span data-ttu-id="96db5-143">Nach der Durchführung dieses wir ändern die Gestaltungsvorlage müssen Verweise in der ASPX-Dateien wird standardmäßig generiert, die ASP.NET Web Forms-Seiten.</span><span class="sxs-lookup"><span data-stu-id="96db5-143">After doing this we'll need to change the master page references in all the .aspx files generated by the default ASP.NET WebForms pages.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="96db5-144">Nächste</span><span class="sxs-lookup"><span data-stu-id="96db5-144">Next</span></span>](tailspin-spyworks-part-2.md)
