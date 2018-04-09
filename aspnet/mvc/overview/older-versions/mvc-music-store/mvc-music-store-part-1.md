---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
title: 'Teil 1: Übersicht und Datei -> Neues Projekt | Microsoft Docs'
author: jongalloway
description: Diese Reihe von Lernprogrammen details aller die Schritte zum Erstellen von ASP.NET MVC-Music Store-beispielanwendung. Teil 1 erläutert Übersicht und Datei -> Neues Projekt.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: bd356ca3-5bdb-4067-9dac-c9e9923a86e8
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
msc.type: authoredcontent
ms.openlocfilehash: 2082927d18c95563893da199d60347fa15952446
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="part-1-overview-and-file-new-project"></a><span data-ttu-id="612c1-104">Teil 1: Übersicht und Datei -> Neues Projekt</span><span class="sxs-lookup"><span data-stu-id="612c1-104">Part 1: Overview and File->New Project</span></span>
====================
<span data-ttu-id="612c1-105">durch [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="612c1-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="612c1-106">Das MVC-Music Store ist eine lernprogrammanwendung, die führt und erklärt schrittweise, wie mithilfe von ASP.NET MVC und Visual Studio für die Webentwicklung.</span><span class="sxs-lookup"><span data-stu-id="612c1-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="612c1-107">Das MVC-Music Store handelt es sich um einfache Beispiel Store Implementierung, die online Musikalben verkauft und implementiert grundlegende standortverwaltung Benutzer anmelden und shopping Cart-Funktionalität.</span><span class="sxs-lookup"><span data-stu-id="612c1-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="612c1-108">Diese Reihe von Lernprogrammen details aller die Schritte zum Erstellen von ASP.NET MVC-Music Store-beispielanwendung.</span><span class="sxs-lookup"><span data-stu-id="612c1-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="612c1-109">Teil 1 behandelt (Übersicht) "und" Datei -&gt;neues Projekt.</span><span class="sxs-lookup"><span data-stu-id="612c1-109">Part 1 covers Overview and File-&gt;New Project.</span></span>


## <a name="overview"></a><span data-ttu-id="612c1-110">Übersicht</span><span class="sxs-lookup"><span data-stu-id="612c1-110">Overview</span></span>

<span data-ttu-id="612c1-111">Das MVC-Music Store ist eine lernprogrammanwendung, die werden vorgestellt und erläutert schrittweise, wie ASP.NET MVC und Visual Web Developer zur Webentwicklung verwendet.</span><span class="sxs-lookup"><span data-stu-id="612c1-111">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Web Developer for web development.</span></span> <span data-ttu-id="612c1-112">Wir werden langsam gestartet werden, damit Anfänger Ebene Webentwicklung auftreten ist in Ordnung.</span><span class="sxs-lookup"><span data-stu-id="612c1-112">We'll be starting slowly, so beginner level web development experience is okay.</span></span>

<span data-ttu-id="612c1-113">Die Anwendung, die wir erstellen werden müssen ist eine einfache-Music Store.</span><span class="sxs-lookup"><span data-stu-id="612c1-113">The application we'll be building is a simple music store.</span></span> <span data-ttu-id="612c1-114">Es gibt drei Hauptteilen zusammen, um die Anwendung: Warenkorb, Auschecken und Verwaltung.</span><span class="sxs-lookup"><span data-stu-id="612c1-114">There are three main parts to the application: shopping, checkout, and administration.</span></span>

![](mvc-music-store-part-1/_static/image1.jpg)

<span data-ttu-id="612c1-115">Besucher können Alben "Genre" suchen:</span><span class="sxs-lookup"><span data-stu-id="612c1-115">Visitors can browse Albums by Genre:</span></span>

![](mvc-music-store-part-1/_static/image2.jpg)

<span data-ttu-id="612c1-116">Sie können einen einzelnen Album anzeigen und Einkaufswagen hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="612c1-116">They can view a single album and add it to their cart:</span></span>

![](mvc-music-store-part-1/_static/image3.jpg)

<span data-ttu-id="612c1-117">Sie können überprüfen, dass Einkaufswagen entfernt alle Elemente, die sie ohne mehr benötigen:</span><span class="sxs-lookup"><span data-stu-id="612c1-117">They can review their cart, removing any items they no longer want:</span></span>

![](mvc-music-store-part-1/_static/image4.jpg)

<span data-ttu-id="612c1-118">Sie zur Kasse gehen werden aufgefordert, diese anmelden oder Registrieren für ein Benutzerkonto an.</span><span class="sxs-lookup"><span data-stu-id="612c1-118">Proceeding to Checkout will prompt them to login or register for a user account.</span></span>

![](mvc-music-store-part-1/_static/image1.png)

![](mvc-music-store-part-1/_static/image2.png)

<span data-ttu-id="612c1-119">Nach dem Erstellen eines Kontos, können sie die Reihenfolge abschließen, indem Shipping "und" Payment Informationen ausfüllen.</span><span class="sxs-lookup"><span data-stu-id="612c1-119">After creating an account, they can complete the order by filling out shipping and payment information.</span></span> <span data-ttu-id="612c1-120">Zur Vereinfachung der Dinge wir eine erstaunliche Promotion ausführen: alles ist kostenlos, wenn sie den Promotioncode "FREE" eingeben.</span><span class="sxs-lookup"><span data-stu-id="612c1-120">To keep things simple, we're running an amazing promotion: everything's free if they enter promotion code "FREE"!</span></span>

![](mvc-music-store-part-1/_static/image5.jpg)

<span data-ttu-id="612c1-121">Nach dem Sortieren, eine einfache Bestätigungsbildschirm angezeigt:</span><span class="sxs-lookup"><span data-stu-id="612c1-121">After ordering, they see a simple confirmation screen:</span></span>

![](mvc-music-store-part-1/_static/image6.jpg)

<span data-ttu-id="612c1-122">Zusätzlich zu den Seiten der Kunden Faceing müssen wir auch erstellen einen Administrator-Abschnitt, der eine Liste von Alben aus denen Administratoren erstellen können, bearbeiten, angezeigt wird und Alben zu löschen:</span><span class="sxs-lookup"><span data-stu-id="612c1-122">In addition to customer-faceing pages, we'll also build an administrator section that shows a list of albums from which Administrators can Create, Edit, and Delete albums:</span></span>

![](mvc-music-store-part-1/_static/image7.jpg)

## <a name="1-file--gt-new-project"></a><span data-ttu-id="612c1-123">1. Datei -&gt; neues Projekt</span><span class="sxs-lookup"><span data-stu-id="612c1-123">1. File -&gt; New Project</span></span>

### <a name="installing-the-software"></a><span data-ttu-id="612c1-124">Installieren der Clientsoftware</span><span class="sxs-lookup"><span data-stu-id="612c1-124">Installing the software</span></span>

<span data-ttu-id="612c1-125">In diesem Lernprogramm beginnt mit dem Erstellen eines neuen ASP.NET MVC 3-Projekts verwenden die kostenlose Visual Web Developer 2010 Express (sodass freigegeben ist), und klicken Sie dann wir fügen inkrementell Funktionen, um eine vollständig funktionierende Anwendung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="612c1-125">This tutorial will begin by creating a new ASP.NET MVC 3 project using the free Visual Web Developer 2010 Express (which is free), and then we'll incrementally add features to create a complete functioning application.</span></span> <span data-ttu-id="612c1-126">Nebenbei müssen eingegangen mit Masterseiten für konsistente Seitenlayout, mithilfe von AJAX für Seite-Aktualisierungen und Validierung, Benutzername und vieles mehr Datenbankzugriff, Formular Buchung Szenarien, die datenvalidierung.</span><span class="sxs-lookup"><span data-stu-id="612c1-126">Along the way, we'll cover database access, form posting scenarios, data validation, using master pages for consistent page layout, using AJAX for page updates and validation, user login, and more.</span></span>

<span data-ttu-id="612c1-127">Sie Schritt für Schritt nachvollziehen können, oder Sie können aus die fertigen Anwendung herunterladen [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="612c1-127">You can follow along step by step, or you can download the completed application from [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>

<span data-ttu-id="612c1-128">Sie können Visual Studio 2010 SP1 oder Visual Web Developer 2010 Express SP1 (eine kostenlose Version von Visual Studio 2010) verwenden, zum Erstellen der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="612c1-128">You can use either Visual Studio 2010 SP1 or Visual Web Developer 2010 Express SP1 (a free version of Visual Studio 2010) to build the application.</span></span> <span data-ttu-id="612c1-129">Wir müssen die SQL Server Compact-(auch kostenlos) verwenden, zum Hosten der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="612c1-129">We'll be using the SQL Server Compact (also free) to host the database.</span></span> <span data-ttu-id="612c1-130">Bevor Sie beginnen, stellen Sie sicher, dass Sie die unten aufgeführten erforderlichen Komponenten installiert haben.</span><span class="sxs-lookup"><span data-stu-id="612c1-130">Before you start, make sure you've installed the prerequisites listed below.</span></span>


- <span data-ttu-id="612c1-131">[Visual Studio Web Developer Express SP1-Voraussetzungen]</span><span class="sxs-lookup"><span data-stu-id="612c1-131">[Visual Studio Web Developer Express SP1 prerequisites]</span></span>
- <span data-ttu-id="612c1-132">[ASP.NET MVC 3 Toolsupdate]</span><span class="sxs-lookup"><span data-stu-id="612c1-132">[ASP.NET MVC 3 Tools Update]</span></span>
- <span data-ttu-id="612c1-133">[SQL Server Compact 4.0] – einschließlich der Unterstützung für Common Language Runtime und Tools</span><span class="sxs-lookup"><span data-stu-id="612c1-133">[SQL Server Compact 4.0] - including both runtime and tools support</span></span>


### <a name="creating-a-new-aspnet-mvc-3-project"></a><span data-ttu-id="612c1-134">Erstellen eines neuen ASP.NET MVC 3-Projekts</span><span class="sxs-lookup"><span data-stu-id="612c1-134">Creating a new ASP.NET MVC 3 project</span></span>

<span data-ttu-id="612c1-135">Wir beginnen, indem Sie in Visual Web Developer im Menü Datei "Neues Projekt" auswählen.</span><span class="sxs-lookup"><span data-stu-id="612c1-135">We'll start by selecting "New Project" from the File menu in Visual Web Developer.</span></span> <span data-ttu-id="612c1-136">Daraufhin wird das Dialogfeld "Neues Projekt".</span><span class="sxs-lookup"><span data-stu-id="612c1-136">This brings up the New Project dialog.</span></span>

![](mvc-music-store-part-1/_static/image5.png)

<span data-ttu-id="612c1-137">Wir wählen Visual c# -&gt; Webvorlagen Gruppe auf der linken Seite, und wählen Sie dann die Vorlage "ASP.NET MVC 3-Webanwendung" in der mittleren Spalte.</span><span class="sxs-lookup"><span data-stu-id="612c1-137">We'll select the Visual C# -&gt; Web Templates group on the left, then choose the "ASP.NET MVC 3 Web Application" template in the center column.</span></span> <span data-ttu-id="612c1-138">Namen für das Projekt MvcMusicStore, und drücken Sie die Schaltfläche "OK".</span><span class="sxs-lookup"><span data-stu-id="612c1-138">Name your project MvcMusicStore and press the OK button.</span></span>

![](mvc-music-store-part-1/_static/image8.jpg)

<span data-ttu-id="612c1-139">Dies wird ein sekundärer Dialogfeld angezeigt, die uns einige MVC-spezifischen Einstellungen für unsere Projekt vornehmen können.</span><span class="sxs-lookup"><span data-stu-id="612c1-139">This will display a secondary dialog which allows us to make some MVC specific settings for our project.</span></span> <span data-ttu-id="612c1-140">Wählen Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="612c1-140">Select the following:</span></span>

<span data-ttu-id="612c1-141">Projektvorlage: Wählen Sie leer</span><span class="sxs-lookup"><span data-stu-id="612c1-141">Project Template - select Empty</span></span>

<span data-ttu-id="612c1-142">Anzeigen von Datenbankmodul – wählen Sie Razor</span><span class="sxs-lookup"><span data-stu-id="612c1-142">View Engine - select Razor</span></span>

<span data-ttu-id="612c1-143">Verwenden Sie HTML5 semantic Markup - überprüft</span><span class="sxs-lookup"><span data-stu-id="612c1-143">Use HTML5 semantic markup - checked</span></span>

<span data-ttu-id="612c1-144">Stellen Sie sicher, dass Ihre Einstellungen wie unten dargestellt sind, und drücken Sie dann die Schaltfläche "OK".</span><span class="sxs-lookup"><span data-stu-id="612c1-144">Verify that your settings are as shown below, then press the OK button.</span></span>

![](mvc-music-store-part-1/_static/image9.jpg)

<span data-ttu-id="612c1-145">Dadurch wird unsere Projekt erstellt.</span><span class="sxs-lookup"><span data-stu-id="612c1-145">This will create our project.</span></span> <span data-ttu-id="612c1-146">Werfen wir einen Blick auf die Ordner, die die Anwendung im Projektmappen-Explorer auf der rechten Seite hinzugefügt wurden.</span><span class="sxs-lookup"><span data-stu-id="612c1-146">Let's take a look at the folders that have been added to our application in the Solution Explorer on the right side.</span></span>

![](mvc-music-store-part-1/_static/image10.jpg)

<span data-ttu-id="612c1-147">Die leere MVC 3-Vorlage nicht vollständig leer ist – er fügt eine grundlegende Ordnerstruktur:</span><span class="sxs-lookup"><span data-stu-id="612c1-147">The Empty MVC 3 template isn't completely empty – it adds a basic folder structure:</span></span>

![](mvc-music-store-part-1/_static/image6.png)

<span data-ttu-id="612c1-148">ASP.NET MVC binärencoder einige einfachen Benennungskonventionen für Ordnernamen:</span><span class="sxs-lookup"><span data-stu-id="612c1-148">ASP.NET MVC makes use of some basic naming conventions for folder names:</span></span>

| <span data-ttu-id="612c1-149">**Ordner**</span><span class="sxs-lookup"><span data-stu-id="612c1-149">**Folder**</span></span> | <span data-ttu-id="612c1-150">**Purpose**</span><span class="sxs-lookup"><span data-stu-id="612c1-150">**Purpose**</span></span> |
| --- | --- |
| <span data-ttu-id="612c1-151">**/Controllers**</span><span class="sxs-lookup"><span data-stu-id="612c1-151">**/Controllers**</span></span> | <span data-ttu-id="612c1-152">Domänencontroller für die vom Browser Eingabe müssen entscheiden, was Sie darin tun, und Antwort an den Benutzer reagieren.</span><span class="sxs-lookup"><span data-stu-id="612c1-152">Controllers respond to input from the browser, decide what to do with it, and return response to the user.</span></span> |
| <span data-ttu-id="612c1-153">**/Views**</span><span class="sxs-lookup"><span data-stu-id="612c1-153">**/Views**</span></span> | <span data-ttu-id="612c1-154">Ansichten halten unsere UI-Vorlagen</span><span class="sxs-lookup"><span data-stu-id="612c1-154">Views hold our UI templates</span></span> |
| <span data-ttu-id="612c1-155">**/Models**</span><span class="sxs-lookup"><span data-stu-id="612c1-155">**/Models**</span></span> | <span data-ttu-id="612c1-156">Modelle enthalten, und Bearbeiten von Daten</span><span class="sxs-lookup"><span data-stu-id="612c1-156">Models hold and manipulate data</span></span> |
| <span data-ttu-id="612c1-157">**/Content**</span><span class="sxs-lookup"><span data-stu-id="612c1-157">**/Content**</span></span> | <span data-ttu-id="612c1-158">Dieser Ordner enthält, unsere Bilder, CSS und alle anderen statischen Inhalten</span><span class="sxs-lookup"><span data-stu-id="612c1-158">This folder holds our images, CSS, and any other static content</span></span> |
| <span data-ttu-id="612c1-159">**/Scripts**</span><span class="sxs-lookup"><span data-stu-id="612c1-159">**/Scripts**</span></span> | <span data-ttu-id="612c1-160">Dieser Ordner enthält unsere JavaScript-Dateien</span><span class="sxs-lookup"><span data-stu-id="612c1-160">This folder holds our JavaScript files</span></span> |

<span data-ttu-id="612c1-161">Diese Ordner sind auch in einer leeren ASP.NET MVC-Anwendung enthalten, da das ASP.NET MVC-Framework standardmäßig einen Ansatz "Konvention über Konfiguration verwendet", und einige Annahmen Standardwert basierend auf den Ordnerbenennungskonventionen.</span><span class="sxs-lookup"><span data-stu-id="612c1-161">These folders are included even in an Empty ASP.NET MVC application because the ASP.NET MVC framework by default uses a "convention over configuration" approach and makes some default assumptions based on folder naming conventions.</span></span> <span data-ttu-id="612c1-162">Suchen z. B. Domänencontrollern für Ansichten im Ordner "Ansichten" standardmäßig ohne dass Sie dies explizit im Code anzugeben.</span><span class="sxs-lookup"><span data-stu-id="612c1-162">For instance, controllers look for views in the Views folder by default without you having to explicitly specify this in your code.</span></span> <span data-ttu-id="612c1-163">Verwenden von den Standardkonventionen reduziert die Menge an Code, die Sie schreiben, müssen und Sie können auch einfacher von anderen Entwicklern Projekts verstanden.</span><span class="sxs-lookup"><span data-stu-id="612c1-163">Sticking with the default conventions reduces the amount of code you need to write, and can also make it easier for other developers to understand your project.</span></span> <span data-ttu-id="612c1-164">Wir erläutern die folgenden Konventionen für die weitere, wenn wir unsere Anwendung erstellen.</span><span class="sxs-lookup"><span data-stu-id="612c1-164">We'll explain these conventions more as we build our application.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="612c1-165">Nächste</span><span class="sxs-lookup"><span data-stu-id="612c1-165">Next</span></span>](mvc-music-store-part-2.md)
