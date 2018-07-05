---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
title: 'Teil 1: Übersicht und Datei -> Neues Projekt | Microsoft-Dokumentation'
author: jongalloway
description: Dieser tutorialreihe werden alle Schritte ausgeführt, um die ASP.NET MVC Music Store-beispielanwendung zu erstellen. Teil 1 Hintergrund Übersicht und Datei -> Neues Projekt.
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: bd356ca3-5bdb-4067-9dac-c9e9923a86e8
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
msc.type: authoredcontent
ms.openlocfilehash: 57856a4a78a650e4abe872004e5be5f8f3b2dbcd
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816338"
---
<a name="part-1-overview-and-file-new-project"></a><span data-ttu-id="19cc3-104">Teil 1: Übersicht und Datei -> Neues Projekt</span><span class="sxs-lookup"><span data-stu-id="19cc3-104">Part 1: Overview and File->New Project</span></span>
====================
<span data-ttu-id="19cc3-105">durch [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="19cc3-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="19cc3-106">Die MVC Music Store ist ein lernprogrammanwendung, die eingeführt und erläutert Schritt für Schritt, wie ASP.NET MVC und Visual Studio für die Webentwicklung verwenden.</span><span class="sxs-lookup"><span data-stu-id="19cc3-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="19cc3-107">Die MVC Music Store ist eine Implementierung eines einfachen Beispiels die Alben online verkauft und implementiert grundlegende Verwaltung, Benutzeranmeldung und shopping Cart-Funktionalität.</span><span class="sxs-lookup"><span data-stu-id="19cc3-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="19cc3-108">Dieser tutorialreihe werden alle Schritte ausgeführt, um die ASP.NET MVC Music Store-beispielanwendung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="19cc3-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="19cc3-109">Teil 1 sind die Übersicht und Datei -&gt;neues Projekt.</span><span class="sxs-lookup"><span data-stu-id="19cc3-109">Part 1 covers Overview and File-&gt;New Project.</span></span>


## <a name="overview"></a><span data-ttu-id="19cc3-110">Übersicht</span><span class="sxs-lookup"><span data-stu-id="19cc3-110">Overview</span></span>

<span data-ttu-id="19cc3-111">Die MVC Music Store ist ein lernprogrammanwendung, die eingeführt und erläutert Schritt für Schritt, wie ASP.NET MVC und Visual Web Developer verwenden, für die Webentwicklung.</span><span class="sxs-lookup"><span data-stu-id="19cc3-111">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Web Developer for web development.</span></span> <span data-ttu-id="19cc3-112">Wir langsam starten, damit für Anfänger auf Web-Entwicklung ist in Ordnung.</span><span class="sxs-lookup"><span data-stu-id="19cc3-112">We'll be starting slowly, so beginner level web development experience is okay.</span></span>

<span data-ttu-id="19cc3-113">Die Anwendung, die wir erstellen, ist eine einfache Musik-Speicher.</span><span class="sxs-lookup"><span data-stu-id="19cc3-113">The application we'll be building is a simple music store.</span></span> <span data-ttu-id="19cc3-114">Es gibt drei Hauptkomponenten der Anwendung: Warenkorb, Auschecken und Verwaltung.</span><span class="sxs-lookup"><span data-stu-id="19cc3-114">There are three main parts to the application: shopping, checkout, and administration.</span></span>

![](mvc-music-store-part-1/_static/image1.jpg)

<span data-ttu-id="19cc3-115">Besucher können Alben nach Genre dorthin navigieren:</span><span class="sxs-lookup"><span data-stu-id="19cc3-115">Visitors can browse Albums by Genre:</span></span>

![](mvc-music-store-part-1/_static/image2.jpg)

<span data-ttu-id="19cc3-116">Sie können ein einzelnes Album anzuzeigen und zu Einkaufswagen hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="19cc3-116">They can view a single album and add it to their cart:</span></span>

![](mvc-music-store-part-1/_static/image3.jpg)

<span data-ttu-id="19cc3-117">Sie können überprüfen, Einkaufswagen, entfernen alle Elemente, die sie nicht mehr benötigen:</span><span class="sxs-lookup"><span data-stu-id="19cc3-117">They can review their cart, removing any items they no longer want:</span></span>

![](mvc-music-store-part-1/_static/image4.jpg)

<span data-ttu-id="19cc3-118">Sie zur Kasse gehen fordert sie zum Anmelden oder registrieren Sie sich für ein Benutzerkonto an.</span><span class="sxs-lookup"><span data-stu-id="19cc3-118">Proceeding to Checkout will prompt them to login or register for a user account.</span></span>

![](mvc-music-store-part-1/_static/image1.png)

![](mvc-music-store-part-1/_static/image2.png)

<span data-ttu-id="19cc3-119">Nach dem Erstellen eines Kontos können sie die Reihenfolge abschließen, indem Sie die Versand- und Zahlung Informationen ausfüllen.</span><span class="sxs-lookup"><span data-stu-id="19cc3-119">After creating an account, they can complete the order by filling out shipping and payment information.</span></span> <span data-ttu-id="19cc3-120">Aus Gründen der Einfachheit, führen wir eine erstaunliche Promotion: alle Komponenten sind kostenlos, wenn sie den Promotioncode "FREE" eingeben.</span><span class="sxs-lookup"><span data-stu-id="19cc3-120">To keep things simple, we're running an amazing promotion: everything's free if they enter promotion code "FREE"!</span></span>

![](mvc-music-store-part-1/_static/image5.jpg)

<span data-ttu-id="19cc3-121">Nach dem Sortieren, sehen sie ein einfaches Bestätigungsbildschirm angezeigt:</span><span class="sxs-lookup"><span data-stu-id="19cc3-121">After ordering, they see a simple confirmation screen:</span></span>

![](mvc-music-store-part-1/_static/image6.jpg)

<span data-ttu-id="19cc3-122">Zusätzlich zu den Seiten der Kunden-Faceing wir außerdem erstellen eine Administrator-Bereich, der eine Liste der Alben aus denen Administratoren erstellen können, bearbeiten, angezeigt und Alben zu löschen:</span><span class="sxs-lookup"><span data-stu-id="19cc3-122">In addition to customer-faceing pages, we'll also build an administrator section that shows a list of albums from which Administrators can Create, Edit, and Delete albums:</span></span>

![](mvc-music-store-part-1/_static/image7.jpg)

## <a name="1-file--gt-new-project"></a><span data-ttu-id="19cc3-123">1. Datei -&gt; neues Projekt</span><span class="sxs-lookup"><span data-stu-id="19cc3-123">1. File -&gt; New Project</span></span>

### <a name="installing-the-software"></a><span data-ttu-id="19cc3-124">Installieren der Clientsoftware</span><span class="sxs-lookup"><span data-stu-id="19cc3-124">Installing the software</span></span>

<span data-ttu-id="19cc3-125">Durch Erstellen eines neuen ASP.NET MVC 3-Projekts verwenden die kostenlose Visual Web Developer 2010 Express (auf dem frei ist) wird mit diesem Tutorial beginnen, und dann werden wir schrittweise Funktionen zum Erstellen einer vollständigen funktionsfähigen Anwendung hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="19cc3-125">This tutorial will begin by creating a new ASP.NET MVC 3 project using the free Visual Web Developer 2010 Express (which is free), and then we'll incrementally add features to create a complete functioning application.</span></span> <span data-ttu-id="19cc3-126">Dabei wird die behandelt mit Masterseiten für konsistente Seitenlayout, mithilfe von AJAX für Seitenupdates und Validierung und Anmeldung des Benutzers Zugriff auf die Datenbank, die Form senden Szenarien, die datenvalidierung.</span><span class="sxs-lookup"><span data-stu-id="19cc3-126">Along the way, we'll cover database access, form posting scenarios, data validation, using master pages for consistent page layout, using AJAX for page updates and validation, user login, and more.</span></span>

<span data-ttu-id="19cc3-127">Sie Schritt für Schritt nachvollziehen können, oder Sie können die fertige Anwendung herunterladen [MVC-Musik-Store](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="19cc3-127">You can follow along step by step, or you can download the completed application from [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>

<span data-ttu-id="19cc3-128">Sie können entweder Visual Studio 2010 SP1 oder Visual Web Developer 2010 Express SP1 (eine kostenlose Version von Visual Studio 2010) verwenden, um die Anwendung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="19cc3-128">You can use either Visual Studio 2010 SP1 or Visual Web Developer 2010 Express SP1 (a free version of Visual Studio 2010) to build the application.</span></span> <span data-ttu-id="19cc3-129">Wir werden die SQL Server Compact-(ebenfalls kostenlos) verwenden, um die Datenbank hostet.</span><span class="sxs-lookup"><span data-stu-id="19cc3-129">We'll be using the SQL Server Compact (also free) to host the database.</span></span> <span data-ttu-id="19cc3-130">Bevor Sie beginnen, stellen Sie sicher, dass Sie die unten aufgeführten erforderlichen Komponenten installiert haben.</span><span class="sxs-lookup"><span data-stu-id="19cc3-130">Before you start, make sure you've installed the prerequisites listed below.</span></span>


- <span data-ttu-id="19cc3-131">[Visual Studio Web Developer Express SP1-Voraussetzungen]</span><span class="sxs-lookup"><span data-stu-id="19cc3-131">[Visual Studio Web Developer Express SP1 prerequisites]</span></span>
- <span data-ttu-id="19cc3-132">[ASP.NET MVC 3 Toolsupdate]</span><span class="sxs-lookup"><span data-stu-id="19cc3-132">[ASP.NET MVC 3 Tools Update]</span></span>
- <span data-ttu-id="19cc3-133">[SQL Server Compact 4.0] – einschließlich Unterstützung für sowohl-Laufzeit und Tools</span><span class="sxs-lookup"><span data-stu-id="19cc3-133">[SQL Server Compact 4.0] - including both runtime and tools support</span></span>


### <a name="creating-a-new-aspnet-mvc-3-project"></a><span data-ttu-id="19cc3-134">Erstellen eines neuen ASP.NET MVC 3-Projekts</span><span class="sxs-lookup"><span data-stu-id="19cc3-134">Creating a new ASP.NET MVC 3 project</span></span>

<span data-ttu-id="19cc3-135">Wir beginnen, indem Sie "Neues Projekt" auswählen, über das Menü "Datei" in Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="19cc3-135">We'll start by selecting "New Project" from the File menu in Visual Web Developer.</span></span> <span data-ttu-id="19cc3-136">Daraufhin wird das Dialogfeld "Neues Projekt".</span><span class="sxs-lookup"><span data-stu-id="19cc3-136">This brings up the New Project dialog.</span></span>

![](mvc-music-store-part-1/_static/image5.png)

<span data-ttu-id="19cc3-137">Ich wähle Visual c# -&gt; Webvorlagen Gruppe auf der linken Seite, und wählen Sie dann die Vorlage "ASP.NET MVC 3-Webanwendung" in der mittleren Spalte.</span><span class="sxs-lookup"><span data-stu-id="19cc3-137">We'll select the Visual C# -&gt; Web Templates group on the left, then choose the "ASP.NET MVC 3 Web Application" template in the center column.</span></span> <span data-ttu-id="19cc3-138">Benennen Sie Ihr Projekt MvcMusicStore aus, und drücken Sie die Schaltfläche "OK".</span><span class="sxs-lookup"><span data-stu-id="19cc3-138">Name your project MvcMusicStore and press the OK button.</span></span>

![](mvc-music-store-part-1/_static/image8.jpg)

<span data-ttu-id="19cc3-139">Dadurch wird ein sekundärer Dialogfeld angezeigt, diese Bezeichnung uns ermöglicht, um eine MVC-spezifischen Einstellungen für das Projekt zu machen.</span><span class="sxs-lookup"><span data-stu-id="19cc3-139">This will display a secondary dialog which allows us to make some MVC specific settings for our project.</span></span> <span data-ttu-id="19cc3-140">Wählen Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="19cc3-140">Select the following:</span></span>

<span data-ttu-id="19cc3-141">Projektvorlage: Wählen Sie die leere</span><span class="sxs-lookup"><span data-stu-id="19cc3-141">Project Template - select Empty</span></span>

<span data-ttu-id="19cc3-142">Engine anzeigen: Wählen Sie Razor</span><span class="sxs-lookup"><span data-stu-id="19cc3-142">View Engine - select Razor</span></span>

<span data-ttu-id="19cc3-143">Verwenden von semantischen HTML5-Markupcode - aktiviert</span><span class="sxs-lookup"><span data-stu-id="19cc3-143">Use HTML5 semantic markup - checked</span></span>

<span data-ttu-id="19cc3-144">Stellen Sie sicher, dass Ihre Einstellungen werden wie unten dargestellt, und drücken Sie dann die Schaltfläche "OK".</span><span class="sxs-lookup"><span data-stu-id="19cc3-144">Verify that your settings are as shown below, then press the OK button.</span></span>

![](mvc-music-store-part-1/_static/image9.jpg)

<span data-ttu-id="19cc3-145">Dadurch wird das Projekt erstellt.</span><span class="sxs-lookup"><span data-stu-id="19cc3-145">This will create our project.</span></span> <span data-ttu-id="19cc3-146">Werfen wir einen Blick auf den Ordner, in denen die Anwendung im Projektmappen-Explorer auf der rechten Seite hinzugefügt wurden.</span><span class="sxs-lookup"><span data-stu-id="19cc3-146">Let's take a look at the folders that have been added to our application in the Solution Explorer on the right side.</span></span>

![](mvc-music-store-part-1/_static/image10.jpg)

<span data-ttu-id="19cc3-147">Die leeren MVC 3-Vorlage nicht vollständig leer ist – eine einfache Ordnerstruktur hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="19cc3-147">The Empty MVC 3 template isn't completely empty – it adds a basic folder structure:</span></span>

![](mvc-music-store-part-1/_static/image6.png)

<span data-ttu-id="19cc3-148">ASP.NET MVC verwendet einige einfachen Benennungskonventionen für Ordnernamen:</span><span class="sxs-lookup"><span data-stu-id="19cc3-148">ASP.NET MVC makes use of some basic naming conventions for folder names:</span></span>

| <span data-ttu-id="19cc3-149">**Ordner**</span><span class="sxs-lookup"><span data-stu-id="19cc3-149">**Folder**</span></span> | <span data-ttu-id="19cc3-150">**Zweck**</span><span class="sxs-lookup"><span data-stu-id="19cc3-150">**Purpose**</span></span> |
| --- | --- |
| <span data-ttu-id="19cc3-151">**/ Controller**</span><span class="sxs-lookup"><span data-stu-id="19cc3-151">**/Controllers**</span></span> | <span data-ttu-id="19cc3-152">Domänencontroller reagieren, geben Sie im Browser entscheiden, was es, und die Antwort an den Benutzer zurück.</span><span class="sxs-lookup"><span data-stu-id="19cc3-152">Controllers respond to input from the browser, decide what to do with it, and return response to the user.</span></span> |
| <span data-ttu-id="19cc3-153">**/ Views**</span><span class="sxs-lookup"><span data-stu-id="19cc3-153">**/Views**</span></span> | <span data-ttu-id="19cc3-154">Sichten enthalten unsere UI-Vorlagen</span><span class="sxs-lookup"><span data-stu-id="19cc3-154">Views hold our UI templates</span></span> |
| <span data-ttu-id="19cc3-155">**/ Modelle**</span><span class="sxs-lookup"><span data-stu-id="19cc3-155">**/Models**</span></span> | <span data-ttu-id="19cc3-156">Modelle enthalten, und Bearbeiten von Daten</span><span class="sxs-lookup"><span data-stu-id="19cc3-156">Models hold and manipulate data</span></span> |
| <span data-ttu-id="19cc3-157">**/ Inhalte**</span><span class="sxs-lookup"><span data-stu-id="19cc3-157">**/Content**</span></span> | <span data-ttu-id="19cc3-158">Dieser Ordner enthält unsere Images, CSS und alle anderen statischen Inhalten</span><span class="sxs-lookup"><span data-stu-id="19cc3-158">This folder holds our images, CSS, and any other static content</span></span> |
| <span data-ttu-id="19cc3-159">**/Scripts**</span><span class="sxs-lookup"><span data-stu-id="19cc3-159">**/Scripts**</span></span> | <span data-ttu-id="19cc3-160">Dieser Ordner enthält unsere JavaScript-Dateien</span><span class="sxs-lookup"><span data-stu-id="19cc3-160">This folder holds our JavaScript files</span></span> |

<span data-ttu-id="19cc3-161">Diese Ordner werden auch in einer leeren ASP.NET MVC-Anwendung enthalten, da ASP.NET MVC-Framework standardmäßig einen Ansatz "Konvention geht vor Konfiguration verwendet", und einige Annahmen standardmäßig basierend auf den Ordner Benennungskonventionen zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="19cc3-161">These folders are included even in an Empty ASP.NET MVC application because the ASP.NET MVC framework by default uses a "convention over configuration" approach and makes some default assumptions based on folder naming conventions.</span></span> <span data-ttu-id="19cc3-162">Suchen z. B. Controller für Ansichten im Ordner "Views" standardmäßig ohne dass Sie dies in Ihrem Code explizit anzugeben.</span><span class="sxs-lookup"><span data-stu-id="19cc3-162">For instance, controllers look for views in the Views folder by default without you having to explicitly specify this in your code.</span></span> <span data-ttu-id="19cc3-163">Mit den Standardkonventionen festhalten reduziert die Menge an Code zu schreiben, Sie müssen und kann auch problemlos für andere Entwickler zu Ihrem Projekt.</span><span class="sxs-lookup"><span data-stu-id="19cc3-163">Sticking with the default conventions reduces the amount of code you need to write, and can also make it easier for other developers to understand your project.</span></span> <span data-ttu-id="19cc3-164">Wir erläutern diesen Konventionen Weitere, wie wir unsere Anwendung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="19cc3-164">We'll explain these conventions more as we build our application.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="19cc3-165">Nächste</span><span class="sxs-lookup"><span data-stu-id="19cc3-165">Next</span></span>](mvc-music-store-part-2.md)
