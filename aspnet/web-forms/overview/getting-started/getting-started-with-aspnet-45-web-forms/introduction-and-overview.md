---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: Erste Schritte mit ASP.NET 4.5 Web Forms und Visual Studio 2013 | Microsoft-Dokumentation
author: Erikre
description: Diese detaillierte lernprogrammreihe vermittelt Ihnen die Grundlagen zum Erstellen einer ASP.NET Web Forms-Anwendung, die mit ASP.NET 4.5 und Microsoft Visual Studio Express Edition-Datenbank...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: 55e7a5e8c0c7df9597f8597cba49d33da23e9159
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365405"
---
<a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2013"></a><span data-ttu-id="54388-103">Erste Schritte mit ASP.NET 4.5 Web Forms und Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="54388-103">Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013</span></span>
====================
<span data-ttu-id="54388-104">durch [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="54388-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="54388-105">[Herunterladen der Wingtip Toys-Beispielprojekts (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) oder [E-Book (PDF) herunterladen](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="54388-105">[Download Wingtip Toys Sample Project (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

> <span data-ttu-id="54388-106">Diese detaillierte lernprogrammreihe vermittelt Ihnen die Grundlagen zum Erstellen einer ASP.NET Web Forms-Anwendung mithilfe von ASP.NET 4.5 und Microsoft Visual Studio Express 2013 für Web.</span><span class="sxs-lookup"><span data-stu-id="54388-106">This step-by-step tutorial series will teach you the basics of building an ASP.NET Web Forms application using ASP.NET 4.5 and Microsoft Visual Studio Express 2013 for Web.</span></span> [<span data-ttu-id="54388-107">ASP.NET Web Forms-Quiz</span><span class="sxs-lookup"><span data-stu-id="54388-107">ASP.NET Web Forms Quiz</span></span>](http://quizapp.cloudapp.net/?quiz=ASP.NET)  
> <span data-ttu-id="54388-108">Testen Sie Ihr Wissen und verstärken Sie wichtige Konzepte mithilfe der ASP.NET Web Forms-Quiz zu.</span><span class="sxs-lookup"><span data-stu-id="54388-108">Test your knowledge and reinforce key concepts by taking the ASP.NET Web Forms Quiz.</span></span> <span data-ttu-id="54388-109">Dieses Quiz wurde speziell von Inhalt in dieser Reihe von Tutorials entwickelt.</span><span class="sxs-lookup"><span data-stu-id="54388-109">This quiz was specifically designed from content contained in this tutorial series.</span></span> <span data-ttu-id="54388-110">Jede Frage, im Quiz erläutert sowie Links zu weiteren Anleitungen.</span><span class="sxs-lookup"><span data-stu-id="54388-110">Each question in the quiz provides an explanation along with links to additional guidance.</span></span>


## <a name="introduction"></a><span data-ttu-id="54388-111">Einführung</span><span class="sxs-lookup"><span data-stu-id="54388-111">Introduction</span></span>

<span data-ttu-id="54388-112">In dieser tutorialreihe führt Sie durch die erforderlichen Schritte zum Erstellen einer ASP.NET Web Forms-Anwendung mithilfe von Visual Studio Express 2013 für Web und ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="54388-112">This series of tutorials guides you through the steps required to create an ASP.NET Web Forms application using Visual Studio Express 2013 for Web and ASP.NET 4.5.</span></span>

<span data-ttu-id="54388-113">Die Anwendung, Sie erstellen, den Namen **Wingtip Toys**.</span><span class="sxs-lookup"><span data-stu-id="54388-113">The application you'll create is named **Wingtip Toys**.</span></span> <span data-ttu-id="54388-114">Es ist ein vereinfachtes Beispiel einer Speicher-Front-Website, das Elemente online verkauft.</span><span class="sxs-lookup"><span data-stu-id="54388-114">It's a simplified example of a store front web site that sells items online.</span></span> <span data-ttu-id="54388-115">Dieser tutorialreihe werden die neuen Features in ASP.NET 4.5 hervorgehoben.</span><span class="sxs-lookup"><span data-stu-id="54388-115">This tutorial series highlights new features available in ASP.NET 4.5.</span></span>

<span data-ttu-id="54388-116">Kommentare sind Willkommen, und wir erstellen die höchste anstrengungen, um dieser tutorialreihe basierend auf Ihren Vorschlägen zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="54388-116">Comments are welcome, and we'll make every effort to update this tutorial series based on your suggestions.</span></span>

### <a name="download-completed-project"></a><span data-ttu-id="54388-117">Heruntergeladenen Projekt</span><span class="sxs-lookup"><span data-stu-id="54388-117">Download completed project</span></span>

<span data-ttu-id="54388-118">Sie können ein C#-Projekt herunterladen, die das vollständige Lernprogramm enthält.</span><span class="sxs-lookup"><span data-stu-id="54388-118">You can download a C# project that contains the completed tutorial.</span></span>

- <span data-ttu-id="54388-119">[Erste Schritte mit ASP.NET 4.5 Web Forms und Visual Studio 2013 – Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (c#)</span><span class="sxs-lookup"><span data-stu-id="54388-119">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span>

### <a name="review-the-content-by-taking-the-related-aspnet-web-forms-quiz"></a><span data-ttu-id="54388-120">Überprüfen Sie den Inhalt, indem die zugehörigen ASP.NET Web Forms-Quiz</span><span class="sxs-lookup"><span data-stu-id="54388-120">Review the content by taking the related ASP.NET Web Forms quiz</span></span>

<span data-ttu-id="54388-121">Nachdem Sie dieses Tutorial abgeschlossen haben, testen Sie Ihr Wissen und wichtige Konzepte zu verstärken, Dank der [ASP.NET Web Forms-Quiz](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001).</span><span class="sxs-lookup"><span data-stu-id="54388-121">After you complete this tutorial, test your knowledge and reinforce key concepts by taking the [ASP.NET Web Forms Quiz](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001).</span></span> <span data-ttu-id="54388-122">Dieses Quiz wurde speziell von Inhalt in dieser Reihe von Tutorials entwickelt.</span><span class="sxs-lookup"><span data-stu-id="54388-122">This quiz was specifically designed from content contained in this tutorial series.</span></span> <span data-ttu-id="54388-123">Jede Frage, im Quiz erläutert sowie Links zu weiteren Anleitungen.</span><span class="sxs-lookup"><span data-stu-id="54388-123">Each question in the quiz provides an explanation along with links to additional guidance.</span></span>

- [<span data-ttu-id="54388-124">ASP.NET Web Forms-Quiz</span><span class="sxs-lookup"><span data-stu-id="54388-124">ASP.NET Web Forms Quiz</span></span>](http://quizapp.cloudapp.net/?quiz=ASP.NET)

### <a name="audience"></a><span data-ttu-id="54388-125">Zielgruppe</span><span class="sxs-lookup"><span data-stu-id="54388-125">Audience</span></span>

<span data-ttu-id="54388-126">Die Zielgruppe für diese tutorialreihe ist erfahrene Entwickler zur Verfügung, die mit ASP.NET Web Forms vertraut sind.</span><span class="sxs-lookup"><span data-stu-id="54388-126">The intended audience of this tutorial series is experienced developers who are new to ASP.NET Web Forms.</span></span> <span data-ttu-id="54388-127">Ein Entwickler, die in dieser tutorialreihe möchten, müssen die folgenden Fähigkeiten:</span><span class="sxs-lookup"><span data-stu-id="54388-127">A developer interested in this tutorial series should have the following skills:</span></span>

- <span data-ttu-id="54388-128">Vertraute mit einem Objekt dienstorientierten Programmiersprache (OOP)</span><span class="sxs-lookup"><span data-stu-id="54388-128">Familiar with an object oriented programming (OOP) language</span></span>
- <span data-ttu-id="54388-129">Mit den Web Development-Konzepten (HTML, CSS, JavaScript)</span><span class="sxs-lookup"><span data-stu-id="54388-129">Familiar with Web development concepts (HTML, CSS, JavaScript)</span></span>
- <span data-ttu-id="54388-130">Mit relationalen Datenbanken vertraut sind.</span><span class="sxs-lookup"><span data-stu-id="54388-130">Familiar with relational database concepts</span></span>
- <span data-ttu-id="54388-131">Mit den Konzepten der n-schichtige Architektur</span><span class="sxs-lookup"><span data-stu-id="54388-131">Familiar with n-tier architecture concepts</span></span>

<span data-ttu-id="54388-132">Wenn Sie möchten, die oben aufgeführten Bereiche sind, berücksichtigen Sie Folgendes überprüfen:</span><span class="sxs-lookup"><span data-stu-id="54388-132">If you are interested in reviewing the areas listed above, consider reviewing the following content:</span></span>

- [<span data-ttu-id="54388-133">Erste Schritte mit Visual c#</span><span class="sxs-lookup"><span data-stu-id="54388-133">Getting Started with Visual C#</span></span>](https://msdn.microsoft.com/library/a72418yk.aspx)
- <span data-ttu-id="54388-134">[Webentwicklung](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP und JQuery](http://w3schools.com/)</span><span class="sxs-lookup"><span data-stu-id="54388-134">[Web Development](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span></span>
- [<span data-ttu-id="54388-135">Relationale Datenbank</span><span class="sxs-lookup"><span data-stu-id="54388-135">Relational database</span></span>](http://en.wikipedia.org/wiki/Relational_database)
- [<span data-ttu-id="54388-136">Multi-Tier-Architektur</span><span class="sxs-lookup"><span data-stu-id="54388-136">Multitier architecture</span></span>](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a><span data-ttu-id="54388-137">Anwendungsfeatures</span><span class="sxs-lookup"><span data-stu-id="54388-137">Application Features</span></span>

<span data-ttu-id="54388-138">Die ASP.NET Web Form-Features, die in dieser Serie vorgestellten umfassen:</span><span class="sxs-lookup"><span data-stu-id="54388-138">The ASP.NET Web Form features presented in this series include:</span></span>

- <span data-ttu-id="54388-139">Das Webanwendungsprojekt (keine Website-Projekt)</span><span class="sxs-lookup"><span data-stu-id="54388-139">The Web Application Project (not Web Site Project)</span></span>
- <span data-ttu-id="54388-140">Web Forms</span><span class="sxs-lookup"><span data-stu-id="54388-140">Web Forms</span></span>
- <span data-ttu-id="54388-141">Masterseiten, Konfiguration</span><span class="sxs-lookup"><span data-stu-id="54388-141">Master Pages, Configuration</span></span>
- <span data-ttu-id="54388-142">Bootstrap-Stil</span><span class="sxs-lookup"><span data-stu-id="54388-142">Bootstrap</span></span>
- <span data-ttu-id="54388-143">Entitätsframework Code First, LocalDB</span><span class="sxs-lookup"><span data-stu-id="54388-143">Entity Framework Code First, LocalDB</span></span>
- <span data-ttu-id="54388-144">Request-Überprüfung</span><span class="sxs-lookup"><span data-stu-id="54388-144">Request Validation</span></span>
- <span data-ttu-id="54388-145">Modellbindung, Datenanmerkungen, stark typisierte Datensteuerelemente, und der Wert von Anbietern</span><span class="sxs-lookup"><span data-stu-id="54388-145">Strongly Typed Data Controls, Model Binding, Data Annotations, and Value Providers</span></span>
- <span data-ttu-id="54388-146">SSL- und OAuth</span><span class="sxs-lookup"><span data-stu-id="54388-146">SSL and OAuth</span></span>
- <span data-ttu-id="54388-147">ASP.NET Identity, Konfiguration und -Autorisierung</span><span class="sxs-lookup"><span data-stu-id="54388-147">ASP.NET Identity, Configuration, and Authorization</span></span>
- <span data-ttu-id="54388-148">Unaufdringliche Validierung</span><span class="sxs-lookup"><span data-stu-id="54388-148">Unobtrusive Validation</span></span>
- <span data-ttu-id="54388-149">Routing</span><span class="sxs-lookup"><span data-stu-id="54388-149">Routing</span></span>
- <span data-ttu-id="54388-150">ASP.NET – Fehlerbehandlung</span><span class="sxs-lookup"><span data-stu-id="54388-150">ASP.NET Error Handling</span></span>

### <a name="application-scenarios-and-tasks"></a><span data-ttu-id="54388-151">Anwendungsszenarien und Aufgaben</span><span class="sxs-lookup"><span data-stu-id="54388-151">Application Scenarios and Tasks</span></span>

<span data-ttu-id="54388-152">Aufgaben, die in dieser Reihe veranschaulicht:</span><span class="sxs-lookup"><span data-stu-id="54388-152">Tasks demonstrated in this series include:</span></span>

- <span data-ttu-id="54388-153">Erstellen, überprüfen und Ausführen des neuen Projekts</span><span class="sxs-lookup"><span data-stu-id="54388-153">Creating, reviewing and running the new project</span></span>
- <span data-ttu-id="54388-154">Erstellen die Struktur der Datenbank</span><span class="sxs-lookup"><span data-stu-id="54388-154">Creating the database structure</span></span>
- <span data-ttu-id="54388-155">Initialisieren und das seeding der Datenbank</span><span class="sxs-lookup"><span data-stu-id="54388-155">Initializing and seeding the database</span></span>
- <span data-ttu-id="54388-156">Anpassen der Benutzeroberfläche mit Stilen, Grafiken und eine Masterseite</span><span class="sxs-lookup"><span data-stu-id="54388-156">Customizing the UI using styles, graphics and a master page</span></span>
- <span data-ttu-id="54388-157">Hinzufügen von Seiten und navigation</span><span class="sxs-lookup"><span data-stu-id="54388-157">Adding pages and navigation</span></span>
- <span data-ttu-id="54388-158">Anzeigen von Menü-Details und Produktdaten</span><span class="sxs-lookup"><span data-stu-id="54388-158">Displaying menu details and product data</span></span>
- <span data-ttu-id="54388-159">Erstellen einen Einkaufswagen gelegt hat</span><span class="sxs-lookup"><span data-stu-id="54388-159">Creating a shopping cart</span></span>
- <span data-ttu-id="54388-160">Hinzufügen von SSL und OAuth-Unterstützung</span><span class="sxs-lookup"><span data-stu-id="54388-160">Adding SSL and OAuth support</span></span>
- <span data-ttu-id="54388-161">Eine Zahlungsmethode hinzufügen</span><span class="sxs-lookup"><span data-stu-id="54388-161">Adding a payment method</span></span>
- <span data-ttu-id="54388-162">Einschließlich der Rolle Administrator angehören und ein Benutzer der Anwendung</span><span class="sxs-lookup"><span data-stu-id="54388-162">Including an administrator role and a user to the application</span></span>
- <span data-ttu-id="54388-163">Einschränken des Zugriffs auf bestimmte Seiten und Ordner</span><span class="sxs-lookup"><span data-stu-id="54388-163">Restricting access to specific pages and folder</span></span>
- <span data-ttu-id="54388-164">Hochladen einer Datei in der Webanwendung</span><span class="sxs-lookup"><span data-stu-id="54388-164">Uploading a file to the web application</span></span>
- <span data-ttu-id="54388-165">Implementieren der eingabeüberprüfung</span><span class="sxs-lookup"><span data-stu-id="54388-165">Implementing input validation</span></span>
- <span data-ttu-id="54388-166">Registrieren von Routen für die Webanwendung</span><span class="sxs-lookup"><span data-stu-id="54388-166">Registering routes for the web application</span></span>
- <span data-ttu-id="54388-167">Implementieren der Fehlerbehandlung und Protokollierung von Anzeigefehlern</span><span class="sxs-lookup"><span data-stu-id="54388-167">Implementing error handling and error logging</span></span>

## <a name="overview"></a><span data-ttu-id="54388-168">Übersicht</span><span class="sxs-lookup"><span data-stu-id="54388-168">Overview</span></span>

<span data-ttu-id="54388-169">Wenn Sie mit ASP.NET Web Forms vertraut sind, weisen aber Kenntnisse im Umgang mit Programmierkonzepten, Sie haben das richtige Tutorial.</span><span class="sxs-lookup"><span data-stu-id="54388-169">If you are new to ASP.NET Web Forms but have familiarity with programming concepts, you have the right tutorial.</span></span> <span data-ttu-id="54388-170">Wenn Sie bereits mit ASP.NET Web Forms vertraut sind, können Sie aus dieser tutorialreihe durch die neuen Features in ASP.NET 4.5 profitieren.</span><span class="sxs-lookup"><span data-stu-id="54388-170">If you are already familiar with ASP.NET Web Forms, you can benefit from this tutorial series by the new features available in ASP.NET 4.5.</span></span> <span data-ttu-id="54388-171">Wenn Sie sich mit Konzepten und ASP.NET Web Forms-Programmierung auskennen, finden Sie unter der Weitere Lernprogramme zur Verfügung gestellt, in den Webformularen [Einstieg](../../../index.md) Abschnitt auf der ASP.NET-Website.</span><span class="sxs-lookup"><span data-stu-id="54388-171">If you are unfamiliar with programming concepts and ASP.NET Web Forms, see the additional tutorials provided in the Web Forms [Getting Started](../../../index.md) section on the ASP.NET Web site.</span></span>

<span data-ttu-id="54388-172">Die spezifischen **neueste** ASP.NET 4.5 Funktionen bereitgestellte in dieser Web Forms tutorialreihe Folgendes einschließen:</span><span class="sxs-lookup"><span data-stu-id="54388-172">The specific **latest** ASP.NET 4.5 features provided in this Web Forms tutorial series include the following:</span></span>

- <span data-ttu-id="54388-173">Eine einfache Benutzeroberfläche zum Erstellen von Projekten dieses Angebot [Unterstützung für mehrere ASP.NET Frameworks](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC und Web-API).</span><span class="sxs-lookup"><span data-stu-id="54388-173">A simple UI for creating projects that offer [support for multiple ASP.NET frameworks](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC, and Web API).</span></span>
- <span data-ttu-id="54388-174">[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), ein Layout und Design-Framework, reaktionsschnelle Entwurfs- und Designfunktionen Funktionen bietet.</span><span class="sxs-lookup"><span data-stu-id="54388-174">[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), a layout and theming framework that provides responsive design and theming capabilities.</span></span>
- <span data-ttu-id="54388-175">[ASP.NET Identity](../../../../identity/index.md), ein neues ASP.NET-Mitgliedschaftssystem, die in allen ASP.NET-Framework-Versionen und funktioniert mit Web-Software als IIS-hosting funktioniert.</span><span class="sxs-lookup"><span data-stu-id="54388-175">[ASP.NET Identity](../../../../identity/index.md), a new ASP.NET membership system that works the same in all ASP.NET frameworks and works with web hosting software other than IIS.</span></span>
- <span data-ttu-id="54388-176">[Entitätsframework 6](https://msdn.microsoft.com/data/ef.aspx), ein Update für das Entity Framework, das ermöglicht das Abrufen und Bearbeiten von Daten als stark typisierte Objekte, den Zugriff auf Daten asynchron Behandeln von vorübergehenden Verbindungsfehlern, und melden Sie sich SQL-Anweisungen.</span><span class="sxs-lookup"><span data-stu-id="54388-176">[Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx), an update to the Entity Framework which allows you retrieve and manipulate data as strongly typed objects, access data asynchronously, handle transient connection faults, and log SQL statements.</span></span>

<span data-ttu-id="54388-177">Eine vollständige Liste der ASP.NET 4.5-Funktionen, finden Sie unter [ASP.NET and Web Tools für Visual Studio 2013 Release Notes](../../../../visual-studio/overview/2013/release-notes.md).</span><span class="sxs-lookup"><span data-stu-id="54388-177">For a complete list of ASP.NET 4.5 features, see [ASP.NET and Web Tools for Visual Studio 2013 Release Notes](../../../../visual-studio/overview/2013/release-notes.md).</span></span>

### <a name="the-wingtip-toys-sample-application"></a><span data-ttu-id="54388-178">Die Wingtip Toys-Beispielanwendung</span><span class="sxs-lookup"><span data-stu-id="54388-178">The Wingtip Toys Sample Application</span></span>

<span data-ttu-id="54388-179">Die folgenden Screenshots bieten einen schnellen Überblick über die ASP.NET Web Forms-Anwendung, die Sie in diesem Tutorial erstellen.</span><span class="sxs-lookup"><span data-stu-id="54388-179">The following screen shots provide a quick view of the ASP.NET Web forms application that you will create in this tutorial series.</span></span> <span data-ttu-id="54388-180">Wenn Sie die Anwendung aus Visual Studio Express 2013 für Web ausführen, sehen Sie die folgenden Web-Homepage.</span><span class="sxs-lookup"><span data-stu-id="54388-180">When you run the application from Visual Studio Express 2013 for Web, you will see the following web Home page.</span></span>

![Wingtip Toys - Standardseite](introduction-and-overview/_static/image1.png)

<span data-ttu-id="54388-182">Sie können als neuer Benutzer registrieren, oder melden Sie sich als einen vorhandenen Benutzer.</span><span class="sxs-lookup"><span data-stu-id="54388-182">You can register as a new user, or log in as an existing user.</span></span> <span data-ttu-id="54388-183">Navigation wird am oberen für jede Produktkategorie bereitgestellt, indem Sie die verfügbaren Produkte aus der Datenbank abrufen.</span><span class="sxs-lookup"><span data-stu-id="54388-183">Navigation is provided at the top for each product category by retrieving the available products from the database.</span></span>

<span data-ttu-id="54388-184">Wenn Sie den Link Produkte auswählen, werden Sie sehen eine Liste aller verfügbaren Produkte.</span><span class="sxs-lookup"><span data-stu-id="54388-184">By selecting the Products link, you will be able to see a list of all available products.</span></span>

![Wingtip Toys - Produkte](introduction-and-overview/_static/image2.png)

<span data-ttu-id="54388-186">Einzelne Produktdetails sehen auch dazu einen der aufgeführten Produkte.</span><span class="sxs-lookup"><span data-stu-id="54388-186">You can also see individual product details by selecting any of the listed products.</span></span>

![Wingtip Toys - Produktinformationen](introduction-and-overview/_static/image3.png)

<span data-ttu-id="54388-188">Sie können als Benutzer registrieren und melden Sie sich mit die Standardfunktionen der Web Forms-Vorlage.</span><span class="sxs-lookup"><span data-stu-id="54388-188">As a user, you can register and log in using the default functionality of the Web Forms template.</span></span> <span data-ttu-id="54388-189">In diesem Tutorial wird erklärt, wie die Anmeldung mit einem vorhandenen Gmail-Konto.</span><span class="sxs-lookup"><span data-stu-id="54388-189">This tutorial also explains how to login using an existing Gmail account.</span></span> <span data-ttu-id="54388-190">Darüber hinaus können sich anmelden, als Administrator hinzufügen und Entfernen von Produkten aus der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="54388-190">Additionally, you can login as the administrator to add and remove products from the database.</span></span>

![Wingtip Toys - Anmeldung](introduction-and-overview/_static/image4.png)

<span data-ttu-id="54388-192">Nachdem Sie sich als Benutzer angemeldet haben, können Sie Produkte auf den Einkaufswagen und Diskussion zum Bezahlvorgang mit PayPal hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="54388-192">Once you have logged in as a user, you can add products to the shopping cart and checkout with PayPal.</span></span> <span data-ttu-id="54388-193">Beachten Sie, dass diese Anwendung konzipiert ist, mit PayPals-Entwickler-Sandbox.</span><span class="sxs-lookup"><span data-stu-id="54388-193">Note that this sample application is designed to function with PayPal's developer sandbox.</span></span> <span data-ttu-id="54388-194">Keine Transaktion tatsächlich Zahlung erfolgt.</span><span class="sxs-lookup"><span data-stu-id="54388-194">No actual money transaction will take place.</span></span>

![Wingtip Toys - Warenkorb](introduction-and-overview/_static/image5.png)

<span data-ttu-id="54388-196">PayPal wird Ihr Konto, Reihenfolge und Zahlungsinformationen zu bestätigen.</span><span class="sxs-lookup"><span data-stu-id="54388-196">PayPal will confirm your account, order, and payment information.</span></span>

![Wingtip Toys - PayPal](introduction-and-overview/_static/image6.png)

<span data-ttu-id="54388-198">Sie können nach der Rückkehr aus PayPal, überprüfen und Abschließen der Bestellung.</span><span class="sxs-lookup"><span data-stu-id="54388-198">After returning from PayPal, you can review and complete your order.</span></span>

![Wingtip Toys - Order-Überprüfung](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a><span data-ttu-id="54388-200">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="54388-200">Prerequisites</span></span>

<span data-ttu-id="54388-201">Bevor Sie beginnen, stellen Sie sicher, dass Sie die folgende Software auf Ihrem Computer installiert haben:</span><span class="sxs-lookup"><span data-stu-id="54388-201">Before you start, make sure that you have the following software installed on your computer:</span></span>

- <span data-ttu-id="54388-202">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) oder [Microsoft Visual Studio Express 2013 für Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="54388-202">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) or [Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span> <span data-ttu-id="54388-203">.NET Framework wird automatisch installiert.</span><span class="sxs-lookup"><span data-stu-id="54388-203">The .NET Framework is installed automatically.</span></span>

<span data-ttu-id="54388-204">Dieser tutorialreihe verwendet Microsoft Visual Studio Express 2013 für Web.</span><span class="sxs-lookup"><span data-stu-id="54388-204">This tutorial series uses Microsoft Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="54388-205">Sie können entweder von Microsoft Visual Studio Express 2013 für Web oder Microsoft Visual Studio 2013 verwenden, zum Abschließen dieser tutorialreihe.</span><span class="sxs-lookup"><span data-stu-id="54388-205">You can use either Microsoft Visual Studio Express 2013 for Web or Microsoft Visual Studio 2013 to complete this tutorial series.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="54388-206">Microsoft Visual Studio 2013 und Microsoft Visual Studio Express 2013 für Web wird häufig als Visual Studio in dieser tutorialreihe bezeichnet werden.</span><span class="sxs-lookup"><span data-stu-id="54388-206">Microsoft Visual Studio 2013 and Microsoft Visual Studio Express 2013 for Web will often be referred to as Visual Studio throughout this tutorial series.</span></span>


<span data-ttu-id="54388-207">Wenn Sie bereits über eine Visual Studio-Version installiert haben, wird während des Installationsvorgangs neben der vorhandenen Version Visual Studio 2013 oder Microsoft Visual Studio Express 2013 für Web installieren.</span><span class="sxs-lookup"><span data-stu-id="54388-207">If you already have a Visual Studio version installed, the installation process will install Visual Studio 2013 or Microsoft Visual Studio Express 2013 for Web next to the existing version.</span></span> <span data-ttu-id="54388-208">In früheren Versionen erstellten Standorte können in Visual Studio 2013 geöffnet werden und weiterhin in früheren Versionen öffnen.</span><span class="sxs-lookup"><span data-stu-id="54388-208">Sites that you created in earlier versions can be opened in Visual Studio 2013 and continue to open in previous versions.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="54388-209">In dieser exemplarischen Vorgehensweise wird davon ausgegangen, dass die Auswahl der *Webentwicklung* Sammlung von Einstellungen der erstmaligen Start von Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="54388-209">This walkthrough assumes that you selected the *Web Development* collection of settings the first time that you started Visual Studio.</span></span> <span data-ttu-id="54388-210">Weitere Informationen finden Sie unter [Vorgehensweise: Aktivieren Sie Umgebungseinstellungen für die Webentwicklung](https://msdn.microsoft.com/library/ff521558.aspx).</span><span class="sxs-lookup"><span data-stu-id="54388-210">For more information, see [How to: Select Web Development Environment Settings](https://msdn.microsoft.com/library/ff521558.aspx).</span></span>


## <a name="download-the-sample-application"></a><span data-ttu-id="54388-211">Herunterladen der Beispielanwendung</span><span class="sxs-lookup"><span data-stu-id="54388-211">Download the Sample Application</span></span>

<span data-ttu-id="54388-212">Nach der Installation der erforderlichen Komponenten können Sie mit dem Erstellen von neuen Webprojekts, das angezeigt werden in diesem Tutorial beginnen.</span><span class="sxs-lookup"><span data-stu-id="54388-212">After installing the prerequisites, you are ready to begin creating the new Web project that is presented in this tutorial series.</span></span> <span data-ttu-id="54388-213">Wenn Sie möchten **optional** ausführen die beispielanwendung, die dieser tutorialreihe erstellt, Sie können es von der Website für MSDN-Beispiele.</span><span class="sxs-lookup"><span data-stu-id="54388-213">If you would like to **optionally** run the sample application that this tutorial series creates, you can download it from the MSDN Samples site.</span></span> <span data-ttu-id="54388-214">Dieser Download enthält Folgendes:</span><span class="sxs-lookup"><span data-stu-id="54388-214">This download contains the following:</span></span>

- <span data-ttu-id="54388-215">Die beispielanwendung in der *WingtipToys* Ordner.</span><span class="sxs-lookup"><span data-stu-id="54388-215">The sample application in the *WingtipToys* folder.</span></span>
- <span data-ttu-id="54388-216">Die Ressourcen, die zum Erstellen der beispielanwendung in der *WingtipToys-Assets* Ordner in der *WingtipToys* Ordner.</span><span class="sxs-lookup"><span data-stu-id="54388-216">The resources used to create the sample application in the *WingtipToys-Assets* folder in the *WingtipToys* folder.</span></span>

#### <a name="download-the-file-from-msdn-samples-site"></a><span data-ttu-id="54388-217">Die Datei von MSDN-Beispiele-Website herunterladen:</span><span class="sxs-lookup"><span data-stu-id="54388-217">Download the file from MSDN Samples site:</span></span>

<span data-ttu-id="54388-218">[Erste Schritte mit ASP.NET 4.5 Web Forms und Visual Studio 2013 – Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (c#)</span><span class="sxs-lookup"><span data-stu-id="54388-218">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span> 

<span data-ttu-id="54388-219">Der Download ist eine <em>ZIP</em> Datei.</span><span class="sxs-lookup"><span data-stu-id="54388-219">The download is a <em>.zip</em> file.</span></span> <span data-ttu-id="54388-220">Um anzuzeigen, suchen und Auswählen des abgeschlossenen Projekts, das dieser tutorialreihe erstellt die <em>c#</em>Ordner in der <em>ZIP</em> Datei.</span><span class="sxs-lookup"><span data-stu-id="54388-220">To see the completed project that this tutorial series creates, find and select the <em>C#</em>folder in the <em>.zip</em> file.</span></span> <span data-ttu-id="54388-221">Speichern Sie die <em>c#</em> den Ordner aus dem Ordner, die Sie verwenden, um mit Visual Studio 2013-Projekten zu arbeiten.</span><span class="sxs-lookup"><span data-stu-id="54388-221">Save the <em>C#</em> folderto the folder you use to work with Visual Studio 2013 projects.</span></span> <span data-ttu-id="54388-222">Standardmäßig lautet der Ordner von Visual Studio 2013-Projekte wie folgt:</span><span class="sxs-lookup"><span data-stu-id="54388-222">By default, the Visual Studio 2013 projects folder is the following:</span></span>

<span data-ttu-id="54388-223"><strong>C:\Users&#92;</strong><strong><em>&lt;Benutzername&gt;</em></strong><strong>\Documents\Visual Studio 2013\Projects</strong></span><span class="sxs-lookup"><span data-stu-id="54388-223"><strong>C:\Users&#92;</strong><strong><em>&lt;username&gt;</em></strong><strong>\Documents\Visual Studio 2013\Projects</strong></span></span>

<span data-ttu-id="54388-224">Benennen Sie die ***c#*** Ordner ***WingtipToys***.</span><span class="sxs-lookup"><span data-stu-id="54388-224">Rename the ***C#*** folder to ***WingtipToys***.</span></span>

> [!NOTE]
> <span data-ttu-id="54388-225">Wenn Sie bereits einen Ordner namens haben *WingtipToys* im Projektordner, bevor Sie Sie umbenennen, vorhandenen Ordner vorübergehend Umbenennen der *c#* Ordner *WingtipToys*.</span><span class="sxs-lookup"><span data-stu-id="54388-225">If you already have a folder named *WingtipToys* in your Projects folder, temporarily rename that existing folder before renaming the *C#* folder to *WingtipToys*.</span></span>


<span data-ttu-id="54388-226">Öffnen Sie zum Ausführen des abgeschlossenen Projekts der *WingtipToys* Ordner und doppelklicken Sie auf die *WingtipToys.sln* Datei.</span><span class="sxs-lookup"><span data-stu-id="54388-226">To run the completed project, open the *WingtipToys* folder and double-click the *WingtipToys.sln* file.</span></span> <span data-ttu-id="54388-227">Visual Studio 2013 wird das Projekt zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="54388-227">Visual Studio 2013 will open the project.</span></span> <span data-ttu-id="54388-228">Als Nächstes mit der rechten Maustaste die *"default.aspx"* im Projektmappen-Explorer die Datei, und klicken Sie mit der rechten Maustaste auf In Browser anzeigen.</span><span class="sxs-lookup"><span data-stu-id="54388-228">Next, right-click the *Default.aspx* file in the Solution Explorer window and click View In Browser from the right-click menu.</span></span>

### <a name="tutorial-support-and-comments"></a><span data-ttu-id="54388-229">Tutorial Support und Kommentare</span><span class="sxs-lookup"><span data-stu-id="54388-229">Tutorial Support and Comments</span></span>

<span data-ttu-id="54388-230">Verwenden Sie den f und A-Abschnitt, der in enthalten die [erste Schritte mit ASP.NET 4.5 Web Forms und Visual Studio 2013 – Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (c#)-Beispiel für Fragen und Kommentare.</span><span class="sxs-lookup"><span data-stu-id="54388-230">Use the Q AND A section included with the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample for any questions or comments.</span></span>

<span data-ttu-id="54388-231">Kommentare zu dieser tutorialreihe sind Willkommen, und bei der Aktualisierung dieser tutorialreihe wird bemüht versucht, berücksichtigt der Konto-Korrekturen oder Vorschläge für Verbesserungen, die in den Kommentaren zum Tutorial bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="54388-231">Comments on this tutorial series are welcome, and when this tutorial series is updated every effort will be made to take into account corrections or suggestions for improvements that are provided in the tutorial comments.</span></span>

<span data-ttu-id="54388-232">Wenn ein Fehler tritt auf, während der Entwicklung oder die Website nicht ordnungsgemäß ausgeführt wird, wird die Fehlermeldungen können komplexe Hinweise geben, an die Quelle des Problems oder können es nicht erläutert, wie Sie diesen Fehler beheben.</span><span class="sxs-lookup"><span data-stu-id="54388-232">When an error happens during development, or if the Web site does not run correctly, the error messages may give complex clues to the source of the problem or might not explain how to fix it.</span></span> <span data-ttu-id="54388-233">Sie können auch verwenden, um Sie mit einigen gängigen Problemszenarien zu unterstützen, die [ASP.NET-Foren](https://forums.asp.net/) oder im Lieferumfang von f und A-Abschnitt der [erste Schritte mit ASP.NET 4.5 Web Forms und Visual Studio 2013 – Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) () C#) Beispiel.</span><span class="sxs-lookup"><span data-stu-id="54388-233">To help you with some common problem scenarios, you can also use the [ASP.NET forums](https://forums.asp.net/) or the Q AND A section included with the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample.</span></span> <span data-ttu-id="54388-234">Wenn Sie eine Fehlermeldung erhalten, oder etwas funktioniert nicht, wie Sie die Tutorials durchgehen, achten Sie darauf, dass Sie die oben genannten Speicherorten zu suchen.</span><span class="sxs-lookup"><span data-stu-id="54388-234">If you get an error message or something doesn't work as you go through the tutorials, be sure to check the above locations.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="54388-235">Nächste</span><span class="sxs-lookup"><span data-stu-id="54388-235">Next</span></span>](create-the-project.md)
