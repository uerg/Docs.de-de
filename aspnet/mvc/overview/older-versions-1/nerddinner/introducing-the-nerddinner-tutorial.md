---
uid: mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
title: Einführung zum NerdDinner-Tutorial | Microsoft-Dokumentation
author: shanselman
description: Die beste Möglichkeit, erfahren, ein neues Framework ist etwas damit zu erstellen. In diesem Tutorial erläutert, wie Sie eine kleine, aber abgeschlossen ist,-Anwendung, die mithilfe von P.NET-Konfiguration erstellen...
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: 397522d5-0402-4b94-b810-a2fb564f869d
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
msc.type: authoredcontent
ms.openlocfilehash: 9188df1eca7f488502640bc17d5034f93f4ac82c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805109"
---
<a name="introducing-the-nerddinner-tutorial"></a><span data-ttu-id="7fef3-104">Einführung zum NerdDinner-Tutorial</span><span class="sxs-lookup"><span data-stu-id="7fef3-104">Introducing the NerdDinner Tutorial</span></span>
====================
<span data-ttu-id="7fef3-105">durch [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="7fef3-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

[<span data-ttu-id="7fef3-106">PDF herunterladen</span><span class="sxs-lookup"><span data-stu-id="7fef3-106">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="7fef3-107">Die beste Möglichkeit, erfahren, ein neues Framework ist etwas damit zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="7fef3-107">The best way to learn a new framework is to build something with it.</span></span> <span data-ttu-id="7fef3-108">Dieses Tutorial führt Sie durch die Schritte zum Erstellen einer kleines, aber abgeschlossen haben, Anwendung, die mithilfe von ASP.NET MVC-1, und eine Einführung in die wichtigsten Konzepte.</span><span class="sxs-lookup"><span data-stu-id="7fef3-108">This tutorial walks through how to build a small, but complete, application using ASP.NET MVC 1, and introduces some of the core concepts behind it.</span></span>
> 
> <span data-ttu-id="7fef3-109">Wenn Sie ASP.NET MVC 3 verwenden, sollten Sie Sie folgen den [erste Schritte mit MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) oder [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) Tutorials.</span><span class="sxs-lookup"><span data-stu-id="7fef3-109">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-tutorial"></a><span data-ttu-id="7fef3-110">NerdDinner-Tutorial</span><span class="sxs-lookup"><span data-stu-id="7fef3-110">NerdDinner Tutorial</span></span>

<span data-ttu-id="7fef3-111">Die beste Möglichkeit, erfahren, ein neues Framework ist etwas damit zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="7fef3-111">The best way to learn a new framework is to build something with it.</span></span> <span data-ttu-id="7fef3-112">Dieses Tutorial führt Sie durch die Schritte zum Erstellen einer kleines, aber abgeschlossen haben, Anwendung, die mit ASP.NET MVC, und eine Einführung in die wichtigsten Konzepte.</span><span class="sxs-lookup"><span data-stu-id="7fef3-112">This tutorial walks through how to build a small, but complete, application using ASP.NET MVC, and introduces some of the core concepts behind it.</span></span>

<span data-ttu-id="7fef3-113">Die Anwendung zum Erstellen der hier verwendeten heißt "NerdDinner".</span><span class="sxs-lookup"><span data-stu-id="7fef3-113">The application we are going to build is called "NerdDinner".</span></span> <span data-ttu-id="7fef3-114">NerdDinner bietet eine einfache Möglichkeit für Benutzer zum Suchen und Organisieren von Dinner online:</span><span class="sxs-lookup"><span data-stu-id="7fef3-114">NerdDinner provides an easy way for people to find and organize dinners online:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image1.png)

<span data-ttu-id="7fef3-115">NerdDinner kann registrierte Benutzer erstellen, bearbeiten und Löschen von Dinner.</span><span class="sxs-lookup"><span data-stu-id="7fef3-115">NerdDinner enables registered users to create, edit and delete dinners.</span></span> <span data-ttu-id="7fef3-116">Einen konsistenten Satz von Überprüfung und Geschäftsregeln erzwungen für die Anwendung:</span><span class="sxs-lookup"><span data-stu-id="7fef3-116">It enforces a consistent set of validation and business rules across the application:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image2.png)

<span data-ttu-id="7fef3-117">Besucher können eine AJAX-basierten Zuordnung verwenden, um nach bevorstehenden Dinner verwendet wird, treten in der Nähe von ihnen zu suchen:</span><span class="sxs-lookup"><span data-stu-id="7fef3-117">Visitors can use an AJAX-based map to search for upcoming dinners being held near them:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image3.png)

<span data-ttu-id="7fef3-118">Durch Klicken auf einem Dinner gelangen sie auf eine Detailseite, erhalten sie weitere Informationen:</span><span class="sxs-lookup"><span data-stu-id="7fef3-118">Clicking a dinner will take them to a details page where they can learn more about it:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image4.png)

<span data-ttu-id="7fef3-119">Wenn sie Ihre Teilnahme an der Dinner können auf der Website registrieren oder anmelden:</span><span class="sxs-lookup"><span data-stu-id="7fef3-119">If they are interested in attending the dinner they can login or register on the site:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image5.png)

<span data-ttu-id="7fef3-120">Sie können eine AJAX-basierte RSVP-Link, um das Ereignis teilnehmen klicken Sie dann auf:</span><span class="sxs-lookup"><span data-stu-id="7fef3-120">They can then click an AJAX-based RSVP link to attend the event:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image6.png)

![](introducing-the-nerddinner-tutorial/_static/image7.png)

### <a name="implementing-nerddinner"></a><span data-ttu-id="7fef3-121">NerdDinner implementieren</span><span class="sxs-lookup"><span data-stu-id="7fef3-121">Implementing NerdDinner</span></span>

<span data-ttu-id="7fef3-122">Wir werden unsere NerdDinner-Anwendung beginnen Sie mit der Datei -&gt;Befehl für neues Projekt in Visual Studio zum Erstellen eines neuen ASP.NET MVC-Projekts.</span><span class="sxs-lookup"><span data-stu-id="7fef3-122">We are going to begin our NerdDinner application by using the File-&gt;New Project command within Visual Studio to create a brand new ASP.NET MVC project.</span></span> <span data-ttu-id="7fef3-123">Es wird schrittweise Funktionen und Features hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="7fef3-123">We will then incrementally add functionality and features.</span></span> <span data-ttu-id="7fef3-124">Dabei werden folgende Themen behandelt:</span><span class="sxs-lookup"><span data-stu-id="7fef3-124">Along the way we'll cover:</span></span>

1. [<span data-ttu-id="7fef3-125">Vorgehensweise: Erstellen Sie ein neues ASP.NET MVC-Anwendungsprojekt</span><span class="sxs-lookup"><span data-stu-id="7fef3-125">How to create a new ASP.NET MVC Project</span></span>](# "Erstellen eines neuen ASP.NET MVC-Projekts")
2. [<span data-ttu-id="7fef3-126">Gewusst wie: Erstellen einer Datenbank</span><span class="sxs-lookup"><span data-stu-id="7fef3-126">How to create a database</span></span>](# "erstellen Sie eine Datenbank")
3. [<span data-ttu-id="7fef3-127">Gewusst wie: Erstellen eines Modells mit geschäftsregelüberprüfungen</span><span class="sxs-lookup"><span data-stu-id="7fef3-127">How to build a model with business rule validations</span></span>](# "Erstellen eines Modells mit Geschäftsregelüberprüfungen")
4. [<span data-ttu-id="7fef3-128">Wie Sie die Controller und Ansichten zu verwenden, um eine Liste/Details-Benutzeroberfläche implementieren</span><span class="sxs-lookup"><span data-stu-id="7fef3-128">How to use controllers and views to implement a listing/details UI</span></span>](# "verwenden Controller und Ansichten zum Implementieren einer Auflistungs-/Detailbenutzeroberfläche")
5. <span data-ttu-id="7fef3-129">[Gewusst wie: Bereitstellen von CRUD (erstellen, lesen, aktualisieren und löschen) Daten bilden Eintrag Unterstützung](# "bieten CRUD (Create, Read, Update, Delete) Daten Formular Eingabe unterstützt")</span><span class="sxs-lookup"><span data-stu-id="7fef3-129">[How to provide CRUD (create, read, update, delete) data form entry support](# "Provide CRUD (Create, Read, Update, Delete) Data Form Entry Support")</span></span>
6. [<span data-ttu-id="7fef3-130">Verwenden von ViewData und Implementieren von ViewModel-Klassen wie</span><span class="sxs-lookup"><span data-stu-id="7fef3-130">How to use ViewData and implement ViewModel classes</span></span>](# "Verwenden von ViewData und Implementieren von ViewModel-Klassen")
7. [<span data-ttu-id="7fef3-131">Gewusst wie: Verwenden Sie die Benutzeroberfläche mit Masterseiten und teilausführungen erneut</span><span class="sxs-lookup"><span data-stu-id="7fef3-131">How to re-use UI using master pages and partials</span></span>](# "Wiederverwenden von UI mithilfe von Masterseiten und Teilausführungen")
8. [<span data-ttu-id="7fef3-132">Gewusst wie: Implementieren von effizienten datenauslagerung</span><span class="sxs-lookup"><span data-stu-id="7fef3-132">How to implement efficient data paging</span></span>](# "implementieren effizient Daten Paging")
9. [<span data-ttu-id="7fef3-133">So sichern Sie Anwendungen mithilfe von Authentifizierung und Autorisierung</span><span class="sxs-lookup"><span data-stu-id="7fef3-133">How to secure applications using authentication and authorization</span></span>](# "sichere Anwendungen mithilfe von Authentifizierung und Autorisierung")
10. [<span data-ttu-id="7fef3-134">Bereitstellen von dynamischen Updates mit AJAX</span><span class="sxs-lookup"><span data-stu-id="7fef3-134">How to use AJAX to deliver dynamic updates</span></span>](# "AJAX zu übermitteln, dynamische Updates verwenden")
11. [<span data-ttu-id="7fef3-135">Wie AJAX verwendet zum Implementieren von zuordnungsszenarien</span><span class="sxs-lookup"><span data-stu-id="7fef3-135">How to use AJAX to implement mapping scenarios</span></span>](# "AJAX verwenden, zum Implementieren von Zuordnungsszenarien")
12. [<span data-ttu-id="7fef3-136">Gewusst wie: Aktivieren Sie automatisierte Komponententests</span><span class="sxs-lookup"><span data-stu-id="7fef3-136">How to enable automated unit testing</span></span>](# "automatisierte Komponententests aktivieren")

<span data-ttu-id="7fef3-137">Sie können Ihr eigenes Exemplar des NerdDinner Erstellen von Grund auf neu, durch die Durchführung der einzelnen führen wir die exemplarische Vorgehensweise in diesem Kapitel.</span><span class="sxs-lookup"><span data-stu-id="7fef3-137">You can build your own copy of NerdDinner from scratch by completing each step we walkthrough in this chapter.</span></span> <span data-ttu-id="7fef3-138">Alternativ können Sie eine abgeschlossene Version des Quellcodes hier herunterladen: [NerdDinner auf GitHub](https://github.com/AspNetMVPSamples/NerdDinner).</span><span class="sxs-lookup"><span data-stu-id="7fef3-138">Alternatively, you can download a completed version of the source code here: [NerdDinner on GitHub](https://github.com/AspNetMVPSamples/NerdDinner).</span></span> <span data-ttu-id="7fef3-139">Sie können optional auch auch [Herunterladen einer kostenlosen PDF-Version dieses Tutorials](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) sollten Sie das Tutorial offline zu lesen.</span><span class="sxs-lookup"><span data-stu-id="7fef3-139">You can also optionally also [download a free PDF version of this tutorial](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) if you want to read the tutorial offline.</span></span>

<span data-ttu-id="7fef3-140">Sie können Visual Studio 2008 oder die kostenlose Visual Web Developer 2008 Express verwenden, um die Anwendung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="7fef3-140">You can use either Visual Studio 2008 or the free Visual Web Developer 2008 Express to build the application.</span></span> <span data-ttu-id="7fef3-141">Sie können entweder SQL Server oder die kostenlose SQL Server Express für die Datenbank verwenden.</span><span class="sxs-lookup"><span data-stu-id="7fef3-141">You can use either SQL Server or the free SQL Server Express for the database.</span></span>

<span data-ttu-id="7fef3-142">Sie können ASP.NET MVC, Visual Web Developer 2008 Express und SQL Server Express (kostenlos) mithilfe von Version 2 der installieren die [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)</span><span class="sxs-lookup"><span data-stu-id="7fef3-142">You can install ASP.NET MVC, Visual Web Developer 2008 Express, and SQL Server Express (all free) using V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)</span></span>

### <a name="now-lets-get-started"></a><span data-ttu-id="7fef3-143">Jetzt lassen Sie uns loslegen...</span><span class="sxs-lookup"><span data-stu-id="7fef3-143">Now let's get started....</span></span>

<span data-ttu-id="7fef3-144">Nun, da wir die neuerungen NerdDinner behandelt habe, wir unsere Ärmel, und Schreiben von Code.</span><span class="sxs-lookup"><span data-stu-id="7fef3-144">Now that we've covered what NerdDinner is, let's roll up our sleeves and write some code.</span></span>

<span data-ttu-id="7fef3-145">Wir beginnen mit Datei -&gt;neues Projekt in Visual Studio die NerdDinner-Anwendung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="7fef3-145">We'll begin by using File-&gt;New Project within Visual Studio to create the NerdDinner application.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="7fef3-146">Nächste</span><span class="sxs-lookup"><span data-stu-id="7fef3-146">Next</span></span>](create-a-new-aspnet-mvc-project.md)
