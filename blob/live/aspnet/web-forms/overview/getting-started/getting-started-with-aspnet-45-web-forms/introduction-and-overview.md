---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: Erste Schritte mit ASP.NET 4.5 WebForms und Visual Studio 2013 | Microsoft Docs
author: Erikre
description: Diese schrittweise Reihe von Lernprogrammen erfahren Sie die Grundlagen der Erstellung einer ASP.NET Web Forms-Anwendung mithilfe von ASP.NET 4.5 und Microsoft Visual Studio Express ein...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: 6ee3e244c4ed29384d11c7acc1440692d3f9b23e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2013"></a><span data-ttu-id="c4fe3-103">Erste Schritte mit ASP.NET 4.5 WebForms und Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="c4fe3-103">Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013</span></span>
====================
<span data-ttu-id="c4fe3-104">Durch [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="c4fe3-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="c4fe3-105">[Herunterladen des Wingtip Toys-Beispielprojekt (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) oder [(PDF) E-Book herunterladen](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="c4fe3-105">[Download Wingtip Toys Sample Project (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

> <span data-ttu-id="c4fe3-106">Diese schrittweise Reihe von Lernprogrammen erfahren Sie die Grundlagen der Erstellung einer ASP.NET Web Forms-Anwendung mithilfe von ASP.NET 4.5 und Microsoft Visual Studio Express 2013 für das Web.</span><span class="sxs-lookup"><span data-stu-id="c4fe3-106">This step-by-step tutorial series will teach you the basics of building an ASP.NET Web Forms application using ASP.NET 4.5 and Microsoft Visual Studio Express 2013 for Web.</span></span> [<span data-ttu-id="c4fe3-107">ASP.NET Web Forms-Quiz</span><span class="sxs-lookup"><span data-stu-id="c4fe3-107">ASP.NET Web Forms Quiz</span></span>](http://quizapp.cloudapp.net/?quiz=ASP.NET)  
> <span data-ttu-id="c4fe3-108">Testen Sie Ihr Wissen und zu verstärken Sie Schlüsselkonzepte ergreifen Sie hierzu das Quiz zu ASP.NET Web Forms.</span><span class="sxs-lookup"><span data-stu-id="c4fe3-108">Test your knowledge and reinforce key concepts by taking the ASP.NET Web Forms Quiz.</span></span> <span data-ttu-id="c4fe3-109">Dieses Quiz wurde speziell von Inhalt in diesem Lernprogramm Reihe.</span><span class="sxs-lookup"><span data-stu-id="c4fe3-109">This quiz was specifically designed from content contained in this tutorial series.</span></span> <span data-ttu-id="c4fe3-110">Jede Frage im Quiz erläutert sowie Links zu weiteren Anleitungen.</span><span class="sxs-lookup"><span data-stu-id="c4fe3-110">Each question in the quiz provides an explanation along with links to additional guidance.</span></span>


## <a name="introduction"></a><span data-ttu-id="c4fe3-111">Einführung</span><span class="sxs-lookup"><span data-stu-id="c4fe3-111">Introduction</span></span>

<span data-ttu-id="c4fe3-112">Diese Reihe von Lernprogrammen führt Sie durch die Schritte zum Erstellen einer ASP.NET Web Forms-Anwendung mithilfe von Visual Studio Express 2013 für Web und ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="c4fe3-112">This series of tutorials guides you through the steps required to create an ASP.NET Web Forms application using Visual Studio Express 2013 for Web and ASP.NET 4.5.</span></span>

<span data-ttu-id="c4fe3-113">Die Anwendung, die Sie erstellen, wird mit dem Namen **Wingtip Toys**.</span><span class="sxs-lookup"><span data-stu-id="c4fe3-113">The application you'll create is named **Wingtip Toys**.</span></span> <span data-ttu-id="c4fe3-114">Es ist ein vereinfachtes Beispiel einer Front-Store-Website, die Elemente online verkauft.</span><span class="sxs-lookup"><span data-stu-id="c4fe3-114">It's a simplified example of a store front web site that sells items online.</span></span> <span data-ttu-id="c4fe3-115">Diese Reihe von Lernprogrammen werden die neue Funktionen von ASP.NET 4.5 hervorgehoben.</span><span class="sxs-lookup"><span data-stu-id="c4fe3-115">This tutorial series highlights new features available in ASP.NET 4.5.</span></span>

<span data-ttu-id="c4fe3-116">Kommentare sind Willkommen, und stellen wir bestrebt, die diese Reihe von Lernprogrammen basierend auf Ihren Vorschlägen zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="c4fe3-116">Comments are welcome, and we'll make every effort to update this tutorial series based on your suggestions.</span></span>

### <a name="download-completed-project"></a><span data-ttu-id="c4fe3-117">Download abgeschlossen-Projekt</span><span class="sxs-lookup"><span data-stu-id="c4fe3-117">Download completed project</span></span>

<span data-ttu-id="c4fe3-118">Sie können ein C#-Projekt herunterladen, die das vollständige Lernprogramm enthält.</span><span class="sxs-lookup"><span data-stu-id="c4fe3-118">You can download a C# project that contains the completed tutorial.</span></span>

- <span data-ttu-id="c4fe3-119">[Erste Schritte mit ASP.NET 4.5 WebForms und Visual Studio 2013 – Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (c#)</span><span class="sxs-lookup"><span data-stu-id="c4fe3-119">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span>

### <a name="review-the-content-by-taking-the-related-aspnet-web-forms-quiz"></a><span data-ttu-id="c4fe3-120">Überprüfen Sie den Inhalt, indem die zugehörige ASP.NET Web Forms-Quiz</span><span class="sxs-lookup"><span data-stu-id="c4fe3-120">Review the content by taking the related ASP.NET Web Forms quiz</span></span>

<span data-ttu-id="c4fe3-121">Nachdem Sie dieses Lernprogramm abgeschlossen haben, testen Sie Ihre Kenntnisse und Schlüsselkonzepte vertiefen, ergreifen Sie hierzu die [ASP.NET Web Forms-Quiz](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001).</span><span class="sxs-lookup"><span data-stu-id="c4fe3-121">After you complete this tutorial, test your knowledge and reinforce key concepts by taking the [ASP.NET Web Forms Quiz](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001).</span></span> <span data-ttu-id="c4fe3-122">Dieses Quiz wurde speziell von Inhalt in diesem Lernprogramm Reihe.</span><span class="sxs-lookup"><span data-stu-id="c4fe3-122">This quiz was specifically designed from content contained in this tutorial series.</span></span> <span data-ttu-id="c4fe3-123">Jede Frage im Quiz erläutert sowie Links zu weiteren Anleitungen.</span><span class="sxs-lookup"><span data-stu-id="c4fe3-123">Each question in the quiz provides an explanation along with links to additional guidance.</span></span>

- [<span data-ttu-id="c4fe3-124">ASP.NET Web Forms-Quiz</span><span class="sxs-lookup"><span data-stu-id="c4fe3-124">ASP.NET Web Forms Quiz</span></span>](http://quizapp.cloudapp.net/?quiz=ASP.NET)

### <a name="audience"></a><span data-ttu-id="c4fe3-125">Zielgruppe</span><span class="sxs-lookup"><span data-stu-id="c4fe3-125">Audience</span></span>

<span data-ttu-id="c4fe3-126">Die Zielgruppe dieses Reihe von Lernprogrammen wird erfahrenen Entwickler, die ASP.NET Web Forms nicht vertraut sind.</span><span class="sxs-lookup"><span data-stu-id="c4fe3-126">The intended audience of this tutorial series is experienced developers who are new to ASP.NET Web Forms.</span></span> <span data-ttu-id="c4fe3-127">Ein Entwickler, die diese Reihe von Lernprogrammen interessiert, sollten die folgenden Kenntnisse verfügen:</span><span class="sxs-lookup"><span data-stu-id="c4fe3-127">A developer interested in this tutorial series should have the following skills:</span></span>

- <span data-ttu-id="c4fe3-128">Vertraute mit einem Objekt dienstorientierten Programmiersprache (OOP)</span><span class="sxs-lookup"><span data-stu-id="c4fe3-128">Familiar with an object oriented programming (OOP) language</span></span>
- <span data-ttu-id="c4fe3-129">Mit Web Development-Konzepten (HTML, CSS, JavaScript)</span><span class="sxs-lookup"><span data-stu-id="c4fe3-129">Familiar with Web development concepts (HTML, CSS, JavaScript)</span></span>
- <span data-ttu-id="c4fe3-130">Mit relationalen Datenbanken vertraut sind.</span><span class="sxs-lookup"><span data-stu-id="c4fe3-130">Familiar with relational database concepts</span></span>
- <span data-ttu-id="c4fe3-131">Mit den Konzepten der n-Tier-Architektur</span><span class="sxs-lookup"><span data-stu-id="c4fe3-131">Familiar with n-tier architecture concepts</span></span>

<span data-ttu-id="c4fe3-132">Wenn Sie interessiert, überprüfen die oben aufgeführten Bereiche sind, erwägen Sie, überprüfen den folgenden Inhalt:</span><span class="sxs-lookup"><span data-stu-id="c4fe3-132">If you are interested in reviewing the areas listed above, consider reviewing the following content:</span></span>

- [<span data-ttu-id="c4fe3-133">Erste Schritte mit Visual c#</span><span class="sxs-lookup"><span data-stu-id="c4fe3-133">Getting Started with Visual C#</span></span>](https://msdn.microsoft.com/library/a72418yk.aspx)
- <span data-ttu-id="c4fe3-134">[Webentwicklung](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP und JQuery](http://w3schools.com/)</span><span class="sxs-lookup"><span data-stu-id="c4fe3-134">[Web Development](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span></span>
- [<span data-ttu-id="c4fe3-135">Relationale Datenbank</span><span class="sxs-lookup"><span data-stu-id="c4fe3-135">Relational database</span></span>](http://en.wikipedia.org/wiki/Relational_database)
- [<span data-ttu-id="c4fe3-136">Multi-Tier-Architektur</span><span class="sxs-lookup"><span data-stu-id="c4fe3-136">Multitier architecture</span></span>](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a><span data-ttu-id="c4fe3-137">Anwendungsfunktionen</span><span class="sxs-lookup"><span data-stu-id="c4fe3-137">Application Features</span></span>

<span data-ttu-id="c4fe3-138">Der ASP.NET Web Form-Funktionen, die in diese Reihe dargestellt:</span><span class="sxs-lookup"><span data-stu-id="c4fe3-138">The ASP.NET Web Form features presented in this series include:</span></span>

- <span data-ttu-id="c4fe3-139">Das Webanwendungsprojekt (keine Website-Projekt)</span><span class="sxs-lookup"><span data-stu-id="c4fe3-139">The Web Application Project (not Web Site Project)</span></span>
- <span data-ttu-id="c4fe3-140">Web Forms</span><span class="sxs-lookup"><span data-stu-id="c4fe3-140">Web Forms</span></span>
- <span data-ttu-id="c4fe3-141">Masterseiten, Konfiguration</span><span class="sxs-lookup"><span data-stu-id="c4fe3-141">Master Pages, Configuration</span></span>
- <span data-ttu-id="c4fe3-142">Bootstrap</span><span class="sxs-lookup"><span data-stu-id="c4fe3-142">Bootstrap</span></span>
- <span data-ttu-id="c4fe3-143">Entity Framework Code zuerst LocalDB</span><span class="sxs-lookup"><span data-stu-id="c4fe3-143">Entity Framework Code First, LocalDB</span></span>
- <span data-ttu-id="c4fe3-144">Anforderungsüberprüfung</span><span class="sxs-lookup"><span data-stu-id="c4fe3-144">Request Validation</span></span>
- <span data-ttu-id="c4fe3-145">Datensteuerelementen, stark typisierten Modell binden, Datenanmerkungen, und der Wert von Anbietern</span><span class="sxs-lookup"><span data-stu-id="c4fe3-145">Strongly Typed Data Controls, Model Binding, Data Annotations, and Value Providers</span></span>
- <span data-ttu-id="c4fe3-146">SSL- und OAuth</span><span class="sxs-lookup"><span data-stu-id="c4fe3-146">SSL and OAuth</span></span>
- <span data-ttu-id="c4fe3-147">ASP.NET Identity, Konfiguration und Autorisierung</span><span class="sxs-lookup"><span data-stu-id="c4fe3-147">ASP.NET Identity, Configuration, and Authorization</span></span>
- <span data-ttu-id="c4fe3-148">Unaufdringlichen Überprüfung</span><span class="sxs-lookup"><span data-stu-id="c4fe3-148">Unobtrusive Validation</span></span>
- <span data-ttu-id="c4fe3-149">Routing</span><span class="sxs-lookup"><span data-stu-id="c4fe3-149">Routing</span></span>
- <span data-ttu-id="c4fe3-150">ASP.NET-Fehlerbehandlung</span><span class="sxs-lookup"><span data-stu-id="c4fe3-150">ASP.NET Error Handling</span></span>

### <a name="application-scenarios-and-tasks"></a><span data-ttu-id="c4fe3-151">Anwendungsszenarien und Aufgaben</span><span class="sxs-lookup"><span data-stu-id="c4fe3-151">Application Scenarios and Tasks</span></span>

<span data-ttu-id="c4fe3-152">U. a. folgende Aufgaben in dieser Reihe veranschaulicht:</span><span class="sxs-lookup"><span data-stu-id="c4fe3-152">Tasks demonstrated in this series include:</span></span>

- <span data-ttu-id="c4fe3-153">Erstellen, überprüfen und Ausführen des neuen Projekts</span><span class="sxs-lookup"><span data-stu-id="c4fe3-153">Creating, reviewing and running the new project</span></span>
- <span data-ttu-id="c4fe3-154">Erstellen die Struktur der Datenbank</span><span class="sxs-lookup"><span data-stu-id="c4fe3-154">Creating the database structure</span></span>
- <span data-ttu-id="c4fe3-155">Initialisieren und das seeding der Datenbank</span><span class="sxs-lookup"><span data-stu-id="c4fe3-155">Initializing and seeding the database</span></span>
- <span data-ttu-id="c4fe3-156">Anpassen der Benutzeroberfläche, die mit Formatvorlagen, Grafiken und einer Masterseite</span><span class="sxs-lookup"><span data-stu-id="c4fe3-156">Customizing the UI using styles, graphics and a master page</span></span>
- <span data-ttu-id="c4fe3-157">Hinzufügen von Seiten und navigation</span><span class="sxs-lookup"><span data-stu-id="c4fe3-157">Adding pages and navigation</span></span>
- <span data-ttu-id="c4fe3-158">Zum Anzeigen von Details im Menü und Produktdaten</span><span class="sxs-lookup"><span data-stu-id="c4fe3-158">Displaying menu details and product data</span></span>
- <span data-ttu-id="c4fe3-159">Erstellen den Einkaufswagen legen</span><span class="sxs-lookup"><span data-stu-id="c4fe3-159">Creating a shopping cart</span></span>
- <span data-ttu-id="c4fe3-160">Hinzufügen von SSL und OAuth-Unterstützung</span><span class="sxs-lookup"><span data-stu-id="c4fe3-160">Adding SSL and OAuth support</span></span>
- <span data-ttu-id="c4fe3-161">Eine Zahlungsmethode hinzufügen</span><span class="sxs-lookup"><span data-stu-id="c4fe3-161">Adding a payment method</span></span>
- <span data-ttu-id="c4fe3-162">Z. B. eine Rolle "Administrator" und ein Benutzer auf die Anwendung</span><span class="sxs-lookup"><span data-stu-id="c4fe3-162">Including an administrator role and a user to the application</span></span>
- <span data-ttu-id="c4fe3-163">Einschränken des Zugriffs auf bestimmte Seiten und Ordner</span><span class="sxs-lookup"><span data-stu-id="c4fe3-163">Restricting access to specific pages and folder</span></span>
- <span data-ttu-id="c4fe3-164">Hochladen einer Datei an die Web-Anwendung</span><span class="sxs-lookup"><span data-stu-id="c4fe3-164">Uploading a file to the web application</span></span>
- <span data-ttu-id="c4fe3-165">Implementieren die Validierung von Benutzereingaben</span><span class="sxs-lookup"><span data-stu-id="c4fe3-165">Implementing input validation</span></span>
- <span data-ttu-id="c4fe3-166">Registrieren von Routen für die Webanwendung</span><span class="sxs-lookup"><span data-stu-id="c4fe3-166">Registering routes for the web application</span></span>
- <span data-ttu-id="c4fe3-167">Implementieren von Fehlerbehandlungs- und Protokollierung von Anzeigefehlern</span><span class="sxs-lookup"><span data-stu-id="c4fe3-167">Implementing error handling and error logging</span></span>

## <a name="overview"></a><span data-ttu-id="c4fe3-168">Übersicht</span><span class="sxs-lookup"><span data-stu-id="c4fe3-168">Overview</span></span>

<span data-ttu-id="c4fe3-169">Wenn Sie mit ASP.NET Web Forms vertraut sind, weisen aber Kenntnisse Programmierungskonzepte, müssen Sie das richtige Lernprogramm.</span><span class="sxs-lookup"><span data-stu-id="c4fe3-169">If you are new to ASP.NET Web Forms but have familiarity with programming concepts, you have the right tutorial.</span></span> <span data-ttu-id="c4fe3-170">Wenn Sie bereits mit ASP.NET Web Forms vertraut sind, profitieren Sie von dieser Reihe von Lernprogrammen durch die neuen Funktionen in ASP.NET 4.5 verfügbar.</span><span class="sxs-lookup"><span data-stu-id="c4fe3-170">If you are already familiar with ASP.NET Web Forms, you can benefit from this tutorial series by the new features available in ASP.NET 4.5.</span></span> <span data-ttu-id="c4fe3-171">Wenn Sie mit Konzepten und ASP.NET Web Forms-Programmierung noch nicht vertraut sind, finden Sie unter der Weitere Lernprogramme zur Verfügung gestellt, in der Web Forms [Einstieg](../../../index.md) Abschnitt auf der ASP.NET-Website.</span><span class="sxs-lookup"><span data-stu-id="c4fe3-171">If you are unfamiliar with programming concepts and ASP.NET Web Forms, see the additional tutorials provided in the Web Forms [Getting Started](../../../index.md) section on the ASP.NET Web site.</span></span>

<span data-ttu-id="c4fe3-172">Die spezifische **neueste** ASP.NET 4.5-Funktionen bereitgestellt in dieser Web Forms Reihe von Lernprogrammen Folgendes:</span><span class="sxs-lookup"><span data-stu-id="c4fe3-172">The specific **latest** ASP.NET 4.5 features provided in this Web Forms tutorial series include the following:</span></span>

- <span data-ttu-id="c4fe3-173">Eine einfache Benutzeroberfläche zum Erstellen von Projekten des betreffenden Angebots [für mehrere Frameworks, die ASP.NET unterstützen](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC und Web-API).</span><span class="sxs-lookup"><span data-stu-id="c4fe3-173">A simple UI for creating projects that offer [support for multiple ASP.NET frameworks](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC, and Web API).</span></span>
- <span data-ttu-id="c4fe3-174">[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), ein Layout und Designumgebung Framework, das reaktionsfähige Design und Designumgebung Funktionen bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="c4fe3-174">[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), a layout and theming framework that provides responsive design and theming capabilities.</span></span>
- <span data-ttu-id="c4fe3-175">[ASP.NET Identity](../../../../identity/index.md), eine neue ASP.NET-Mitgliedschaftssystem, die in allen ASP.NET Frameworks und funktioniert mit Webhosting-Software als IIS arbeitet.</span><span class="sxs-lookup"><span data-stu-id="c4fe3-175">[ASP.NET Identity](../../../../identity/index.md), a new ASP.NET membership system that works the same in all ASP.NET frameworks and works with web hosting software other than IIS.</span></span>
- <span data-ttu-id="c4fe3-176">[Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx), ein Update für das Entity Framework dadurch abrufen und Bearbeiten von Daten als stark typisierte Objekte, den Zugriff auf Daten asynchron verarbeiten vorübergehende Verbindungsfehler auslösen, und SQL-Anweisungen zu protokollieren.</span><span class="sxs-lookup"><span data-stu-id="c4fe3-176">[Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx), an update to the Entity Framework which allows you retrieve and manipulate data as strongly typed objects, access data asynchronously, handle transient connection faults, and log SQL statements.</span></span>

<span data-ttu-id="c4fe3-177">Eine vollständige Liste der Features von ASP.NET 4.5 finden Sie unter [ASP.NET und Webtools für Visual Studio 2013-Versionshinweise](../../../../visual-studio/overview/2013/release-notes.md).</span><span class="sxs-lookup"><span data-stu-id="c4fe3-177">For a complete list of ASP.NET 4.5 features, see [ASP.NET and Web Tools for Visual Studio 2013 Release Notes](../../../../visual-studio/overview/2013/release-notes.md).</span></span>

### <a name="the-wingtip-toys-sample-application"></a><span data-ttu-id="c4fe3-178">Die Beispielanwendung des Wingtip Toys</span><span class="sxs-lookup"><span data-stu-id="c4fe3-178">The Wingtip Toys Sample Application</span></span>

<span data-ttu-id="c4fe3-179">Die folgenden Screenshots bieten einen schnellen Überblick über die ASP.NET Web Forms-Anwendung, die Sie in diesem Lernprogramm Reihe erstellen.</span><span class="sxs-lookup"><span data-stu-id="c4fe3-179">The following screen shots provide a quick view of the ASP.NET Web forms application that you will create in this tutorial series.</span></span> <span data-ttu-id="c4fe3-180">Wenn Sie die Anwendung aus Visual Studio Express 2013 für Web ausführen, sehen Sie die folgenden Web-Startseite.</span><span class="sxs-lookup"><span data-stu-id="c4fe3-180">When you run the application from Visual Studio Express 2013 for Web, you will see the following web Home page.</span></span>

![Wingtip Toys - Standardseite](introduction-and-overview/_static/image1.png)

<span data-ttu-id="c4fe3-182">Sie können als neuer Benutzer registrieren, oder melden Sie sich als einen vorhandenen Benutzer.</span><span class="sxs-lookup"><span data-stu-id="c4fe3-182">You can register as a new user, or log in as an existing user.</span></span> <span data-ttu-id="c4fe3-183">Navigation wird am oberen für jede Produktkategorie durch Abrufen der verfügbaren Produkte aus der Datenbank bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="c4fe3-183">Navigation is provided at the top for each product category by retrieving the available products from the database.</span></span>

<span data-ttu-id="c4fe3-184">Wenn Sie den Link Produkte auswählen, werden Sie eine Liste aller verfügbaren Produkte angezeigt.</span><span class="sxs-lookup"><span data-stu-id="c4fe3-184">By selecting the Products link, you will be able to see a list of all available products.</span></span>

![Wingtip Toys - Produkte](introduction-and-overview/_static/image2.png)

<span data-ttu-id="c4fe3-186">Einzelne Produktdetails sehen dazu einen der aufgeführten Produkte.</span><span class="sxs-lookup"><span data-stu-id="c4fe3-186">You can also see individual product details by selecting any of the listed products.</span></span>

![Wingtip Toys - Produktdetails](introduction-and-overview/_static/image3.png)

<span data-ttu-id="c4fe3-188">Als ein Benutzer können Sie registrieren und melden Sie sich mit den Standardfunktionen der Web Forms-Vorlage.</span><span class="sxs-lookup"><span data-stu-id="c4fe3-188">As a user, you can register and log in using the default functionality of the Web Forms template.</span></span> <span data-ttu-id="c4fe3-189">In diesem Lernprogramm wird beschrieben, wie mit einem vorhandenen Gmail-Konto anmelden.</span><span class="sxs-lookup"><span data-stu-id="c4fe3-189">This tutorial also explains how to login using an existing Gmail account.</span></span> <span data-ttu-id="c4fe3-190">Darüber hinaus können Sie sich als Administrator hinzufügen und Entfernen von Produkten aus der Datenbank anmelden.</span><span class="sxs-lookup"><span data-stu-id="c4fe3-190">Additionally, you can login as the administrator to add and remove products from the database.</span></span>

![Wingtip Toys - Anmeldung](introduction-and-overview/_static/image4.png)

<span data-ttu-id="c4fe3-192">Nachdem Sie sich als Benutzer angemeldet haben, können Sie mit dem Einkaufswagen und Auschecken von PayPal Produkte hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="c4fe3-192">Once you have logged in as a user, you can add products to the shopping cart and checkout with PayPal.</span></span> <span data-ttu-id="c4fe3-193">Beachten Sie, dass diese beispielanwendung mit PayPal Entwickler Sandkasten geeignet ist.</span><span class="sxs-lookup"><span data-stu-id="c4fe3-193">Note that this sample application is designed to function with PayPal's developer sandbox.</span></span> <span data-ttu-id="c4fe3-194">Keine tatsächlichen Money-Transaktion wird ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="c4fe3-194">No actual money transaction will take place.</span></span>

![Wingtip Toys - Einkaufswagen](introduction-and-overview/_static/image5.png)

<span data-ttu-id="c4fe3-196">PayPal wird Ihr Konto verhindern, Reihenfolge und Zahlungsinformationen bestätigt.</span><span class="sxs-lookup"><span data-stu-id="c4fe3-196">PayPal will confirm your account, order, and payment information.</span></span>

![Wingtip Toys - PayPal](introduction-and-overview/_static/image6.png)

<span data-ttu-id="c4fe3-198">Nach der Rückkehr von PayPal, können Sie überprüfen und Ihre Bestellung abzuschließen.</span><span class="sxs-lookup"><span data-stu-id="c4fe3-198">After returning from PayPal, you can review and complete your order.</span></span>

![Wingtip Toys - Reihenfolge überprüfen](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a><span data-ttu-id="c4fe3-200">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="c4fe3-200">Prerequisites</span></span>

<span data-ttu-id="c4fe3-201">Bevor Sie beginnen, stellen Sie sicher, dass Sie die folgende Software auf Ihrem Computer installiert haben:</span><span class="sxs-lookup"><span data-stu-id="c4fe3-201">Before you start, make sure that you have the following software installed on your computer:</span></span>

- <span data-ttu-id="c4fe3-202">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/en-us/downloads#vs) oder [Microsoft Visual Studio Express 2013 für Web](https://www.microsoft.com/visualstudio/11/en-us/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="c4fe3-202">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/en-us/downloads#vs) or [Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/en-us/downloads#express-web).</span></span> <span data-ttu-id="c4fe3-203">.NET Framework wird automatisch installiert.</span><span class="sxs-lookup"><span data-stu-id="c4fe3-203">The .NET Framework is installed automatically.</span></span>

<span data-ttu-id="c4fe3-204">Diese Reihe von Lernprogrammen verwendet Microsoft Visual Studio Express 2013 für Web an.</span><span class="sxs-lookup"><span data-stu-id="c4fe3-204">This tutorial series uses Microsoft Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="c4fe3-205">Sie können entweder Microsoft Visual Studio Express 2013 für Web oder Microsoft Visual Studio 2013 verwenden, zum Abschließen dieser Reihe von Lernprogrammen.</span><span class="sxs-lookup"><span data-stu-id="c4fe3-205">You can use either Microsoft Visual Studio Express 2013 for Web or Microsoft Visual Studio 2013 to complete this tutorial series.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="c4fe3-206">Microsoft Visual Studio 2013 und Microsoft Visual Studio Express 2013 für das Web wird häufig als Visual Studio in der gesamten dieser Reihe von Lernprogrammen bezeichnet werden.</span><span class="sxs-lookup"><span data-stu-id="c4fe3-206">Microsoft Visual Studio 2013 and Microsoft Visual Studio Express 2013 for Web will often be referred to as Visual Studio throughout this tutorial series.</span></span>


<span data-ttu-id="c4fe3-207">Wenn Sie bereits eine Version von Visual Studio installiert haben, wird während des Installationsvorgangs neben der vorhandenen Version Visual Studio 2013 oder Microsoft Visual Studio Express 2013 für Web installieren.</span><span class="sxs-lookup"><span data-stu-id="c4fe3-207">If you already have a Visual Studio version installed, the installation process will install Visual Studio 2013 or Microsoft Visual Studio Express 2013 for Web next to the existing version.</span></span> <span data-ttu-id="c4fe3-208">In früheren Versionen erstellten Standorte können in Visual Studio 2013 geöffnet werden und weiterhin in früheren Versionen öffnen.</span><span class="sxs-lookup"><span data-stu-id="c4fe3-208">Sites that you created in earlier versions can be opened in Visual Studio 2013 and continue to open in previous versions.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="c4fe3-209">In dieser exemplarischen Vorgehensweise wird davon ausgegangen, dass die Auswahl der *Webentwicklung* Auflistung von Einstellungen zum ersten Mal starten von Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c4fe3-209">This walkthrough assumes that you selected the *Web Development* collection of settings the first time that you started Visual Studio.</span></span> <span data-ttu-id="c4fe3-210">Weitere Informationen finden Sie unter [wie: Wählen Sie Web Development Umgebungseinstellungen](https://msdn.microsoft.com/en-us/library/ff521558.aspx).</span><span class="sxs-lookup"><span data-stu-id="c4fe3-210">For more information, see [How to: Select Web Development Environment Settings](https://msdn.microsoft.com/en-us/library/ff521558.aspx).</span></span>


## <a name="download-the-sample-application"></a><span data-ttu-id="c4fe3-211">Laden Sie die Beispielanwendung</span><span class="sxs-lookup"><span data-stu-id="c4fe3-211">Download the Sample Application</span></span>

<span data-ttu-id="c4fe3-212">Nach der Installation der erforderlichen Komponenten können Sie beginnen, erstellen das neue Web-Projekt, das bereitgestellt wird, in diesem Lernprogramm Reihe.</span><span class="sxs-lookup"><span data-stu-id="c4fe3-212">After installing the prerequisites, you are ready to begin creating the new Web project that is presented in this tutorial series.</span></span> <span data-ttu-id="c4fe3-213">Wenn Sie eine möchten **optional** Ausführen der beispielanwendung, die diese Reihe von Lernprogrammen erstellt, Sie können es herunterladen von der Website für MSDN-Beispiele.</span><span class="sxs-lookup"><span data-stu-id="c4fe3-213">If you would like to **optionally** run the sample application that this tutorial series creates, you can download it from the MSDN Samples site.</span></span> <span data-ttu-id="c4fe3-214">Dieser Download enthält Folgendes:</span><span class="sxs-lookup"><span data-stu-id="c4fe3-214">This download contains the following:</span></span>

- <span data-ttu-id="c4fe3-215">Die beispielanwendung in der *WingtipToys* Ordner.</span><span class="sxs-lookup"><span data-stu-id="c4fe3-215">The sample application in the *WingtipToys* folder.</span></span>
- <span data-ttu-id="c4fe3-216">Ressourcen zum Erstellen der beispielanwendung in der *WingtipToys Bestand* Ordner in der *WingtipToys* Ordner.</span><span class="sxs-lookup"><span data-stu-id="c4fe3-216">The resources used to create the sample application in the *WingtipToys-Assets* folder in the *WingtipToys* folder.</span></span>

#### <a name="download-the-file-from-msdn-samples-site"></a><span data-ttu-id="c4fe3-217">Die Datei von MSDN-Beispiele-Website herunterladen:</span><span class="sxs-lookup"><span data-stu-id="c4fe3-217">Download the file from MSDN Samples site:</span></span>

<span data-ttu-id="c4fe3-218">[Erste Schritte mit ASP.NET 4.5 WebForms und Visual Studio 2013 – Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (c#)</span><span class="sxs-lookup"><span data-stu-id="c4fe3-218">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span> 

<span data-ttu-id="c4fe3-219">Der Download ist eine *ZIP* Datei.</span><span class="sxs-lookup"><span data-stu-id="c4fe3-219">The download is a *.zip* file.</span></span> <span data-ttu-id="c4fe3-220">Anzeigen des abgeschlossenen Projekts, die diese Reihe von Lernprogrammen erstellt, suchen und wählen Sie die *c#*Ordner in der *ZIP* Datei.</span><span class="sxs-lookup"><span data-stu-id="c4fe3-220">To see the completed project that this tutorial series creates, find and select the *C#*folder in the *.zip* file.</span></span> <span data-ttu-id="c4fe3-221">Speichern Sie die *c#* den Ordner aus dem Ordner, die Sie zum Arbeiten mit Visual Studio 2013-Projekte verwenden.</span><span class="sxs-lookup"><span data-stu-id="c4fe3-221">Save the *C#* folderto the folder you use to work with Visual Studio 2013 projects.</span></span> <span data-ttu-id="c4fe3-222">Standardmäßig lautet der Ordner von Visual Studio 2013-Projekte wie folgt:</span><span class="sxs-lookup"><span data-stu-id="c4fe3-222">By default, the Visual Studio 2013 projects folder is the following:</span></span>

<span data-ttu-id="c4fe3-223">**C:\Users\*****&lt;Benutzername&gt;*** \Documents\Visual Studio 2013\Projects**</span><span class="sxs-lookup"><span data-stu-id="c4fe3-223">**C:\Users\*****&lt;username&gt;*****\Documents\Visual Studio 2013\Projects**</span></span>

<span data-ttu-id="c4fe3-224">Benennen Sie die ***c#*** Ordner ***WingtipToys***.</span><span class="sxs-lookup"><span data-stu-id="c4fe3-224">Rename the ***C#*** folder to ***WingtipToys***.</span></span>

> [!NOTE]
> <span data-ttu-id="c4fe3-225">Wenn bereits einen Ordner namens *WingtipToys* im Projektordner, benennen Sie vorübergehend vorhandenen Ordner vor dem Umbenennen der *c#* Ordner *WingtipToys*.</span><span class="sxs-lookup"><span data-stu-id="c4fe3-225">If you already have a folder named *WingtipToys* in your Projects folder, temporarily rename that existing folder before renaming the *C#* folder to *WingtipToys*.</span></span>


<span data-ttu-id="c4fe3-226">Öffnen Sie zum Ausführen des abgeschlossenen Projekts, das *WingtipToys* Ordner und doppelklicken Sie auf die *WingtipToys.sln* Datei.</span><span class="sxs-lookup"><span data-stu-id="c4fe3-226">To run the completed project, open the *WingtipToys* folder and double-click the *WingtipToys.sln* file.</span></span> <span data-ttu-id="c4fe3-227">Visual Studio 2013 wird das Projekt zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="c4fe3-227">Visual Studio 2013 will open the project.</span></span> <span data-ttu-id="c4fe3-228">Als Nächstes mit der rechten Maustaste die *"default.aspx"* im Fenster Projektmappen-Explorer die Datei, und klicken Sie mit der rechten Maustaste In Browser anzeigen.</span><span class="sxs-lookup"><span data-stu-id="c4fe3-228">Next, right-click the *Default.aspx* file in the Solution Explorer window and click View In Browser from the right-click menu.</span></span>

### <a name="tutorial-support-and-comments"></a><span data-ttu-id="c4fe3-229">Lernprogramm Support und Kommentare</span><span class="sxs-lookup"><span data-stu-id="c4fe3-229">Tutorial Support and Comments</span></span>

<span data-ttu-id="c4fe3-230">Verwenden Sie den f und A-Abschnitt enthalten die [erste Schritte mit ASP.NET 4.5 Web Forms und Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (c#)-Beispiel für Fragen und Kommentare.</span><span class="sxs-lookup"><span data-stu-id="c4fe3-230">Use the Q AND A section included with the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample for any questions or comments.</span></span>

<span data-ttu-id="c4fe3-231">Kommentare für diese Reihe von Lernprogrammen Willkommen sind, und bei der Aktualisierung dieser Reihe von Lernprogrammen bemüht vorgenommen werden berücksichtigt Konto Korrekturen oder Vorschläge für Verbesserungen, die in den Kommentaren Tutorial bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="c4fe3-231">Comments on this tutorial series are welcome, and when this tutorial series is updated every effort will be made to take into account corrections or suggestions for improvements that are provided in the tutorial comments.</span></span>

<span data-ttu-id="c4fe3-232">Wenn ein Fehler tritt auf, während der Entwicklung, oder die Website nicht ordnungsgemäß ausgeführt werden kann, wird die Fehlermeldungen können komplexe Informationen zu den Ursachen geben, an die Quelle des Problems oder möglicherweise nicht erläutert, wie sie diesen Fehler beheben.</span><span class="sxs-lookup"><span data-stu-id="c4fe3-232">When an error happens during development, or if the Web site does not run correctly, the error messages may give complex clues to the source of the problem or might not explain how to fix it.</span></span> <span data-ttu-id="c4fe3-233">Sie können auch verwenden, um Sie für einige allgemeine Problemszenarien zu finden, die [ASP.NET Foren](https://forums.asp.net/) oder im Lieferumfang von f und A-Abschnitt der [erste Schritte mit ASP.NET 4.5 Web Forms und Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ( C#) Beispiel.</span><span class="sxs-lookup"><span data-stu-id="c4fe3-233">To help you with some common problem scenarios, you can also use the [ASP.NET forums](https://forums.asp.net/) or the Q AND A section included with the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample.</span></span> <span data-ttu-id="c4fe3-234">Wenn Sie eine Fehlermeldung erhalten, oder etwas funktioniert nicht, wenn Sie die Lernprogramme absolvieren, achten Sie darauf, dass Sie die oben genannten Speicherorte zu überprüfen.</span><span class="sxs-lookup"><span data-stu-id="c4fe3-234">If you get an error message or something doesn't work as you go through the tutorials, be sure to check the above locations.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="c4fe3-235">Nächste</span><span class="sxs-lookup"><span data-stu-id="c4fe3-235">Next</span></span>](create-the-project.md)
