---
uid: web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
title: Aufrufen von Web-API aus einer Windows Phone 8-Anwendung (c#) | Microsoft-Dokumentation
author: rmcmurray
description: Erstellen Sie eine vollständige End-to-End-Szenario bestehend aus einer ASP.NET Web-API-Anwendung, die einen Katalog mit Büchern, die eine Windows Phone 8-Anwendung bereitstellt.
ms.author: riande
ms.date: 10/09/2013
ms.assetid: b9775f41-352a-4f82-baa6-23e95b342e20
msc.legacyurl: /web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
msc.type: authoredcontent
ms.openlocfilehash: ca2b5f41f6c3bd38faacd1e15c4dee6f6210aff7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832293"
---
<a name="calling-web-api-from-a-windows-phone-8-application-c"></a><span data-ttu-id="685c8-103">Aufrufen von Web-API aus einer Windows Phone 8-Anwendung (c#)</span><span class="sxs-lookup"><span data-stu-id="685c8-103">Calling Web API from a Windows Phone 8 Application (C#)</span></span>
====================
<span data-ttu-id="685c8-104">durch [Robert McMurray](https://github.com/rmcmurray)</span><span class="sxs-lookup"><span data-stu-id="685c8-104">by [Robert McMurray](https://github.com/rmcmurray)</span></span>

<span data-ttu-id="685c8-105">In diesem Tutorial erfahren Sie, wie erstellen Sie eine vollständige End-to-End-Szenario bestehend aus einer ASP.NET Web-API-Anwendung, die einen Katalog mit Büchern, die eine Windows Phone 8-Anwendung bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="685c8-105">In this tutorial, you will learn how to create a complete end-to-end scenario consisting of an ASP.NET Web API application that provides a catalog of books to a Windows Phone 8 application.</span></span>

### <a name="overview"></a><span data-ttu-id="685c8-106">Übersicht</span><span class="sxs-lookup"><span data-stu-id="685c8-106">Overview</span></span>

<span data-ttu-id="685c8-107">RESTful-Dienste wie ASP.NET-Web-API, vereinfachen die Erstellung von HTTP-basierte Anwendungen für Entwickler durch das abstrahieren der Architektur für die serverseitige und clientseitige Anwendungen.</span><span class="sxs-lookup"><span data-stu-id="685c8-107">RESTful services like ASP.NET Web API simplify the creation of HTTP-based applications for developers by abstracting the architecture for server-side and client-side applications.</span></span> <span data-ttu-id="685c8-108">Statt ein proprietäres Protokoll der Socket-basierte für die Kommunikation, Web-API-Entwickler müssen einfach die erforderlichen HTTP-Methoden für die entsprechende Anwendung zu veröffentlichen (zum Beispiel: GET, POST, PUT, DELETE), und Entwicklern von Clientanwendungen ausschließlich verarbeiten müssen die HTTP-Methoden, die für ihre Anwendung erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="685c8-108">Instead of creating a proprietary socket-based protocol for communication, Web API developers simply need to publish the requisite HTTP methods for their application, (for example: GET, POST, PUT, DELETE), and client application developers only need to consume the HTTP methods that are necessary for their application.</span></span>

<span data-ttu-id="685c8-109">In diesem End-to-End-Tutorial lernen Sie, wie Sie Web-API verwenden, um die folgenden Projekte zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="685c8-109">In this end-to-end tutorial, you will learn how to use Web API to create the following projects:</span></span>

- <span data-ttu-id="685c8-110">In der [ersten Teil dieses Lernprogramms](#STEP1), erstellen Sie eine ASP.NET Web-API-Anwendung, alle Vorgänge zum Verwalten einer Bücherkatalog Create, Read, Update und Delete (CRUD) unterstützt.</span><span class="sxs-lookup"><span data-stu-id="685c8-110">In the [first part of this tutorial](#STEP1), you will create an ASP.NET Web API application that supports all of the Create, Read, Update, and Delete (CRUD) operations to manage a book catalog.</span></span> <span data-ttu-id="685c8-111">Diese Anwendung verwendet die [Beispiel-XML-Datei (books.xml)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) auf MSDN.</span><span class="sxs-lookup"><span data-stu-id="685c8-111">This application will use the [Sample XML File (books.xml)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) from MSDN.</span></span>
- <span data-ttu-id="685c8-112">In der [zweiter Teil dieses Lernprogramms](#STEP2), erstellen Sie eine interaktive Windows Phone 8-Anwendung, die Daten aus Ihrer Web-API-Anwendung abruft.</span><span class="sxs-lookup"><span data-stu-id="685c8-112">In the [second part of this tutorial](#STEP2), you will create an interactive Windows Phone 8 application that retrieves the data from your Web API application.</span></span>

#### <a name="prerequisites"></a><span data-ttu-id="685c8-113">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="685c8-113">Prerequisites</span></span>

- <span data-ttu-id="685c8-114">Visual Studio 2013 mit dem Windows Phone 8 SDK</span><span class="sxs-lookup"><span data-stu-id="685c8-114">Visual Studio 2013 with the Windows Phone 8 SDK installed</span></span>
- <span data-ttu-id="685c8-115">Windows 8 oder höher auf einem 64-Bit-System mit Hyper-V installiert</span><span class="sxs-lookup"><span data-stu-id="685c8-115">Windows 8 or later on a 64-bit system with Hyper-V installed</span></span>
- <span data-ttu-id="685c8-116">Eine Liste der zusätzlichen Anforderungen, finden Sie unter den *Systemanforderungen* im Abschnitt der [Windows Phone SDK 8.0](https://www.microsoft.com/download/details.aspx?id=35471) Downloadseite.</span><span class="sxs-lookup"><span data-stu-id="685c8-116">For a list of additional requirements, see the *System Requirements* section on the [Windows Phone SDK 8.0](https://www.microsoft.com/download/details.aspx?id=35471) download page.</span></span>

> [!NOTE]
> <span data-ttu-id="685c8-117">Wenn Sie beabsichtigen, um die Konnektivität zwischen Web-API und Windows Phone 8-Projekte auf dem lokalen System zu testen, müssen Sie die Anweisungen in der *[Web-API-Anwendungen auf einem lokalen mit der Windows Phone 8-Emulator Computer](https://go.microsoft.com/fwlink/?LinkId=324014)* Artikel zum Einrichten Ihrer testumgebung.</span><span class="sxs-lookup"><span data-stu-id="685c8-117">If you are going to test the connectivity between Web API and Windows Phone 8 projects on your local system, you will need to follow the instructions in the *[Connecting the Windows Phone 8 Emulator to Web API Applications on a Local Computer](https://go.microsoft.com/fwlink/?LinkId=324014)* article to set up your testing environment.</span></span>


<a id="STEP1"></a>
### <a name="step-1-creating-the-web-api-bookstore-project"></a><span data-ttu-id="685c8-118">Schritt 1: Erstellen der Web-API angegeben, dass-Projekt</span><span class="sxs-lookup"><span data-stu-id="685c8-118">Step 1: Creating the Web API Bookstore Project</span></span>

<span data-ttu-id="685c8-119">Der erste Schritt in diesem End-to-End-Tutorial ist ein Web-API-Projekt zu erstellen, die alle CRUD-Vorgänge unterstützt. Beachten Sie, dass Sie diese Lösung in das Windows Phone-Anwendungsprojekt hinzufügen [Schritt2](#STEP2) in diesem Tutorial.</span><span class="sxs-lookup"><span data-stu-id="685c8-119">The first step of this end-to-end tutorial is to create a Web API project that supports all of the CRUD operations; note that you will add the Windows Phone application project to this solution in [Step 2](#STEP2) of this tutorial.</span></span>

1. <span data-ttu-id="685c8-120">Open **Visual Studio 2013**.</span><span class="sxs-lookup"><span data-stu-id="685c8-120">Open **Visual Studio 2013**.</span></span>
2. <span data-ttu-id="685c8-121">Klicken Sie auf **Datei**, klicken Sie dann **neue**, und klicken Sie dann **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="685c8-121">Click **File**, then **New**, and then **Project**.</span></span>
3. <span data-ttu-id="685c8-122">Wenn die **neues Projekt** Dialogfeld wird angezeigt, erweitern Sie **installiert**, klicken Sie dann **Vorlagen**, klicken Sie dann **Visual C#-**, und klicken Sie dann **Web**.</span><span class="sxs-lookup"><span data-stu-id="685c8-122">When the **New Project** dialog box is displayed, expand **Installed**, then **Templates**, then **Visual C#**, and then **Web**.</span></span>


   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image2.png)](calling-web-api-from-a-windows-phone-8-application/_static/image1.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                <span data-ttu-id="685c8-123">Klicken Sie auf Bild zu erweitern</span><span class="sxs-lookup"><span data-stu-id="685c8-123">Click image to expand</span></span>                                                                |


4. <span data-ttu-id="685c8-124">Markieren Sie **ASP.NET-Webanwendung**, geben Sie **BookStore** für den Projektnamen ein, und klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="685c8-124">Highlight **ASP.NET Web Application**, enter **BookStore** for the project name, and then click **OK**.</span></span>
5. <span data-ttu-id="685c8-125">Wenn die **neues ASP.NET-Projekt** Dialogfeld angezeigt wird, wählen die **Web-API-** Vorlage, und klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="685c8-125">When the **New ASP.NET Project** dialog box is displayed, select the **Web API** template, and then click **OK**.</span></span>


   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image4.png)](calling-web-api-from-a-windows-phone-8-application/_static/image3.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                <span data-ttu-id="685c8-126">Klicken Sie auf Bild zu erweitern</span><span class="sxs-lookup"><span data-stu-id="685c8-126">Click image to expand</span></span>                                                                |


6. <span data-ttu-id="685c8-127">Wenn die Web-API-Projekt geöffnet wird, wird entfernen Sie den Beispielcontroller aus dem Projekt:</span><span class="sxs-lookup"><span data-stu-id="685c8-127">When the Web API project opens, remove the sample controller from the project:</span></span>

    1. <span data-ttu-id="685c8-128">Erweitern Sie die **Controller** Ordner im Projektmappen-Explorer.</span><span class="sxs-lookup"><span data-stu-id="685c8-128">Expand the **Controllers** folder in the solution explorer.</span></span>
    2. <span data-ttu-id="685c8-129">Mit der rechten Maustaste die **ValuesController.cs** Datei, und klicken Sie dann auf **löschen**.</span><span class="sxs-lookup"><span data-stu-id="685c8-129">Right-click the **ValuesController.cs** file, and then click **Delete**.</span></span>
    3. <span data-ttu-id="685c8-130">Klicken Sie auf **OK** Wenn aufgefordert, den Löschvorgang zu bestätigen.</span><span class="sxs-lookup"><span data-stu-id="685c8-130">Click **OK** when prompted to confirm the deletion.</span></span>
7. <span data-ttu-id="685c8-131">Hinzufügen einer XML-Datendatei für das Web-API-Projekt; Diese Datei enthält den Inhalt des Katalogs Bookstore:</span><span class="sxs-lookup"><span data-stu-id="685c8-131">Add an XML data file to the Web API project; this file contains the contents of the bookstore catalog:</span></span>

   1. <span data-ttu-id="685c8-132">Mit der rechten Maustaste die **App\_Daten** Ordner im Projektmappen-Explorer, klicken Sie dann auf **hinzufügen**, und klicken Sie dann auf **neues Element**.</span><span class="sxs-lookup"><span data-stu-id="685c8-132">Right-click the **App\_Data** folder in the solution explorer, then click **Add**, and then click **New Item**.</span></span>
   2. <span data-ttu-id="685c8-133">Wenn die **neues Element hinzufügen** Dialogfeld wird angezeigt, markieren Sie die **XML-Datei** Vorlage.</span><span class="sxs-lookup"><span data-stu-id="685c8-133">When the **Add New Item** dialog box is displayed, highlight the **XML File** template.</span></span>
   3. <span data-ttu-id="685c8-134">Nennen Sie die Datei **Books.xml**, und klicken Sie dann auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="685c8-134">Name the file **Books.xml**, and then click **Add**.</span></span>
   4. <span data-ttu-id="685c8-135">Wenn die **Books.xml** Datei geöffnet wird, ersetzen Sie den Code in der Datei mit den XML-Code aus dem Beispiel **books.xml** -Datei auf der MSDN:</span><span class="sxs-lookup"><span data-stu-id="685c8-135">When the **Books.xml** file is opened, replace the code in the file with the XML from the sample **books.xml** file on MSDN:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample1.xml)]
   5. <span data-ttu-id="685c8-136">Speichern Sie und schließen Sie die XML-Datei.</span><span class="sxs-lookup"><span data-stu-id="685c8-136">Save and close the XML file.</span></span>

8. <span data-ttu-id="685c8-137">Der Bookstore-Modell für das Web-API-Projekt hinzufügen; Dieses Modell enthält die Logik für Create, Read, Update und Delete (CRUD) für die Anwendung angegeben, dass:</span><span class="sxs-lookup"><span data-stu-id="685c8-137">Add the bookstore model to the Web API project; this model contains the Create, Read, Update, and Delete (CRUD) logic for the bookstore application:</span></span>

   1. <span data-ttu-id="685c8-138">Mit der rechten Maustaste die **Modelle** Ordner im Projektmappen-Explorer, klicken Sie dann auf **hinzufügen**, und klicken Sie dann auf **Klasse**.</span><span class="sxs-lookup"><span data-stu-id="685c8-138">Right-click the **Models** folder in the solution explorer, then click **Add**, and then click **Class**.</span></span>
   2. <span data-ttu-id="685c8-139">Wenn die **neues Element hinzufügen** Dialogfeld wird angezeigt, benennen Sie die Klassendatei **BookDetails.cs**, und klicken Sie dann auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="685c8-139">When the **Add New Item** dialog box is displayed, name the class file **BookDetails.cs**, and then click **Add**.</span></span>
   3. <span data-ttu-id="685c8-140">Wenn die **BookDetails.cs** Datei geöffnet wird, ersetzen Sie den Code in der Datei durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="685c8-140">When the **BookDetails.cs** file is opened, replace the code in the file with the following:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample2.cs)]
   4. <span data-ttu-id="685c8-141">Speichern und schließen Sie die **BookDetails.cs** Datei.</span><span class="sxs-lookup"><span data-stu-id="685c8-141">Save and close the **BookDetails.cs** file.</span></span>

9. <span data-ttu-id="685c8-142">Fügen Sie der Bookstore-Controller für das Web-API-Projekt hinzu:</span><span class="sxs-lookup"><span data-stu-id="685c8-142">Add the bookstore controller to the Web API project:</span></span>

   1. <span data-ttu-id="685c8-143">Mit der rechten Maustaste die **Controller** Ordner im Projektmappen-Explorer, klicken Sie dann auf **hinzufügen**, und klicken Sie dann auf **Controller**.</span><span class="sxs-lookup"><span data-stu-id="685c8-143">Right-click the **Controllers** folder in the solution explorer, then click **Add**, and then click **Controller**.</span></span>
   2. <span data-ttu-id="685c8-144">Wenn die **Gerüst hinzufügen** Dialogfeld wird angezeigt, markieren Sie **Web API 2-Controller – leer**, und klicken Sie dann auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="685c8-144">When the **Add Scaffold** dialog box is displayed, highlight **Web API 2 Controller - Empty**, and then click **Add**.</span></span>
   3. <span data-ttu-id="685c8-145">Wenn die **Controller hinzufügen** Dialogfeld wird angezeigt, nennen Sie den Controller **BooksController**, und klicken Sie dann auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="685c8-145">When the **Add Controller** dialog box is displayed, name the controller **BooksController**, and then click **Add**.</span></span>
   4. <span data-ttu-id="685c8-146">Wenn die **BooksController.cs** Datei geöffnet wird, ersetzen Sie den Code in der Datei durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="685c8-146">When the **BooksController.cs** file is opened, replace the code in the file with the following:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample3.cs)]
   5. <span data-ttu-id="685c8-147">Speichern und schließen Sie die **BooksController.cs** Datei.</span><span class="sxs-lookup"><span data-stu-id="685c8-147">Save and close the **BooksController.cs** file.</span></span>

10. <span data-ttu-id="685c8-148">Erstellen Sie die Web-API-Anwendung auf Fehler überprüfen.</span><span class="sxs-lookup"><span data-stu-id="685c8-148">Build the Web API application to check for errors.</span></span>

<a id="STEP2"></a>
### <a name="step-2-adding-the-windows-phone-8-bookstore-catalog-project"></a><span data-ttu-id="685c8-149">Schritt 2: Hinzufügen des Windows Phone 8 Bookstore-Katalog-Projekts</span><span class="sxs-lookup"><span data-stu-id="685c8-149">Step 2: Adding the Windows Phone 8 Bookstore Catalog Project</span></span>

<span data-ttu-id="685c8-150">Der nächste Schritt dieses End-to-End-Szenario ist die Erstellung die kataloganwendung für Windows Phone 8.</span><span class="sxs-lookup"><span data-stu-id="685c8-150">The next step of this end-to-end scenario is to create the catalog application for Windows Phone 8.</span></span> <span data-ttu-id="685c8-151">Diese Anwendung verwendet die *Windows Phone Databound App* Vorlage für die Standardbenutzeroberfläche und verwendet die Web-API-Anwendung, die Sie in erstellt [Schritt 1](#STEP1) in diesem Tutorial als Datenquelle.</span><span class="sxs-lookup"><span data-stu-id="685c8-151">This application will use the *Windows Phone Databound App* template for the default user interface, and it will use the Web API application that you created in [Step 1](#STEP1) of this tutorial as the data source.</span></span>

1. <span data-ttu-id="685c8-152">Mit der rechten Maustaste die **BookStore** -Lösung in die im Projektmappen-Explorer, klicken Sie dann auf **hinzufügen**, und klicken Sie dann **neues Projekt**.</span><span class="sxs-lookup"><span data-stu-id="685c8-152">Right-click the **BookStore** solution in the in the solution explorer, then click **Add**, and then **New Project**.</span></span>
2. <span data-ttu-id="685c8-153">Wenn die **neues Projekt** Dialogfeld wird angezeigt, erweitern Sie **installiert**, klicken Sie dann **Visual C#-**, und klicken Sie dann **Windows Phone**.</span><span class="sxs-lookup"><span data-stu-id="685c8-153">When the **New Project** dialog box is displayed, expand **Installed**, then **Visual C#**, and then **Windows Phone**.</span></span>
3. <span data-ttu-id="685c8-154">Markieren Sie **Windows Phone Databound App**, geben Sie **BookCatalog** für den Namen ein, und klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="685c8-154">Highlight **Windows Phone Databound App**, enter **BookCatalog** for the name, and then click **OK**.</span></span>
4. <span data-ttu-id="685c8-155">Das Json.NET NuGet-Paket zum Hinzufügen der **BookCatalog** Projekt:</span><span class="sxs-lookup"><span data-stu-id="685c8-155">Add the Json.NET NuGet package to the **BookCatalog** project:</span></span>

    1. <span data-ttu-id="685c8-156">Mit der rechten Maustaste **Verweise** für die **BookCatalog** Projekt im Projektmappen-Explorer, und klicken Sie dann auf **NuGet-Pakete verwalten**.</span><span class="sxs-lookup"><span data-stu-id="685c8-156">Right-click **References** for the **BookCatalog** project in the solution explorer, and then click **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="685c8-157">Wenn die **NuGet-Pakete verwalten** Dialogfeld wird angezeigt, erweitern Sie die **Online** aus, und markieren Sie **nuget.org**.</span><span class="sxs-lookup"><span data-stu-id="685c8-157">When the **Manage NuGet Packages** dialog box is displayed, expand the **Online** section, and highlight **nuget.org**.</span></span>
    3. <span data-ttu-id="685c8-158">Geben Sie **Json.NET** in die Suche ein, und klicken Sie auf das Symbol "Suche".</span><span class="sxs-lookup"><span data-stu-id="685c8-158">Enter **Json.NET** in the search field and click the search icon.</span></span>
    4. <span data-ttu-id="685c8-159">Markieren Sie **Json.NET** in den Suchergebnissen, und klicken Sie dann auf **installieren**.</span><span class="sxs-lookup"><span data-stu-id="685c8-159">Highlight **Json.NET** in the search results, and then click **Install**.</span></span>
    5. <span data-ttu-id="685c8-160">Wenn die Installation abgeschlossen ist, klicken Sie auf **schließen**.</span><span class="sxs-lookup"><span data-stu-id="685c8-160">When the installation has completed, click **Close**.</span></span>
5. <span data-ttu-id="685c8-161">Hinzufügen der **BookDetails** Modell der **BookCatalog** Projekteigenschaften; enthält ein generisches Modell der Bookstore-Klasse:</span><span class="sxs-lookup"><span data-stu-id="685c8-161">Add the **BookDetails** model to the **BookCatalog** project; this contains a generic model of the bookstore class:</span></span>

   1. <span data-ttu-id="685c8-162">Mit der rechten Maustaste die **BookCatalog** Projekt im Projektmappen-Explorer, und klicken Sie dann **hinzufügen**, und klicken Sie dann auf **neuer Ordner**.</span><span class="sxs-lookup"><span data-stu-id="685c8-162">Right-click the **BookCatalog** project in the solution explorer, then click **Add**, and then click **New Folder**.</span></span>
   2. <span data-ttu-id="685c8-163">Nennen Sie diesen Ordner **Modelle**.</span><span class="sxs-lookup"><span data-stu-id="685c8-163">Name the new folder **Models**.</span></span>
   3. <span data-ttu-id="685c8-164">Mit der rechten Maustaste die **Modelle** Ordner im Projektmappen-Explorer, klicken Sie dann auf **hinzufügen**, und klicken Sie dann auf **Klasse**.</span><span class="sxs-lookup"><span data-stu-id="685c8-164">Right-click the **Models** folder in the solution explorer, then click **Add**, and then click **Class**.</span></span>
   4. <span data-ttu-id="685c8-165">Wenn die **neues Element hinzufügen** Dialogfeld wird angezeigt, benennen Sie die Klassendatei **BookDetails.cs**, und klicken Sie dann auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="685c8-165">When the **Add New Item** dialog box is displayed, name the class file **BookDetails.cs**, and then click **Add**.</span></span>
   5. <span data-ttu-id="685c8-166">Wenn die **BookDetails.cs** Datei geöffnet wird, ersetzen Sie den Code in der Datei durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="685c8-166">When the **BookDetails.cs** file is opened, replace the code in the file with the following:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample4.cs)]
   6. <span data-ttu-id="685c8-167">Speichern und schließen Sie die **BookDetails.cs** Datei.</span><span class="sxs-lookup"><span data-stu-id="685c8-167">Save and close the **BookDetails.cs** file.</span></span>

6. <span data-ttu-id="685c8-168">Update der **"MainViewModel.cs"** Klasse einbeziehen, die Funktionalität für die Kommunikation mit der BookStore-Web-API-Anwendung:</span><span class="sxs-lookup"><span data-stu-id="685c8-168">Update the **MainViewModel.cs** class to include the functionality to communicate with the BookStore Web API application:</span></span>

   1. <span data-ttu-id="685c8-169">Erweitern Sie die **ViewModels** Ordner im Projektmappen-Explorer, und doppelklicken Sie dann auf die **"MainViewModel.cs"** Datei.</span><span class="sxs-lookup"><span data-stu-id="685c8-169">Expand the **ViewModels** folder in the solution explorer, and then double-click the **MainViewModel.cs** file.</span></span>
   2. <span data-ttu-id="685c8-170">Wenn die **"MainViewModel.cs"** Datei wird geöffnet, ersetzen Sie den Code in der Datei mit den folgenden; Beachten Sie, dass Sie für das update benötigen die `apiUrl` Konstante mit der richtigen URL Ihrer Web-API:</span><span class="sxs-lookup"><span data-stu-id="685c8-170">When the **MainViewModel.cs** file is opened, replace the code in the file with the following; note that you will need to update the value of the `apiUrl` constant with the actual URL of your Web API:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample5.cs)]
   3. <span data-ttu-id="685c8-171">Speichern und schließen Sie die **"MainViewModel.cs"** Datei.</span><span class="sxs-lookup"><span data-stu-id="685c8-171">Save and close the **MainViewModel.cs** file.</span></span>

7. <span data-ttu-id="685c8-172">Update der **"MainPage.xaml"** Datei den Namen der Anwendung anpassen:</span><span class="sxs-lookup"><span data-stu-id="685c8-172">Update the **MainPage.xaml** file to customize the application name:</span></span>

   1. <span data-ttu-id="685c8-173">Doppelklicken Sie auf die **"MainPage.xaml"** Datei im Projektmappen-Explorer.</span><span class="sxs-lookup"><span data-stu-id="685c8-173">Double-click the **MainPage.xaml** file in the solution explorer.</span></span>
   2. <span data-ttu-id="685c8-174">Wenn die **"MainPage.xaml"** Datei geöffnet wird, suchen Sie nach den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="685c8-174">When the **MainPage.xaml** file is opened, locate the following lines of code:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample6.xml)]
   3. <span data-ttu-id="685c8-175">Ersetzen Sie diese Zeilen durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="685c8-175">Replace those lines with the following:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample7.xml)]
   4. <span data-ttu-id="685c8-176">Speichern und schließen Sie die **"MainPage.xaml"** Datei.</span><span class="sxs-lookup"><span data-stu-id="685c8-176">Save and close the **MainPage.xaml** file.</span></span>

8. <span data-ttu-id="685c8-177">Update der **DetailsPage.xaml** Datei, um die angezeigten Elemente anzupassen:</span><span class="sxs-lookup"><span data-stu-id="685c8-177">Update the **DetailsPage.xaml** file to customize the displayed items:</span></span>

   1. <span data-ttu-id="685c8-178">Doppelklicken Sie auf die **DetailsPage.xaml** Datei im Projektmappen-Explorer.</span><span class="sxs-lookup"><span data-stu-id="685c8-178">Double-click the **DetailsPage.xaml** file in the solution explorer.</span></span>
   2. <span data-ttu-id="685c8-179">Wenn die **DetailsPage.xaml** Datei geöffnet wird, suchen Sie nach den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="685c8-179">When the **DetailsPage.xaml** file is opened, locate the following lines of code:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample8.xml)]
   3. <span data-ttu-id="685c8-180">Ersetzen Sie diese Zeilen durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="685c8-180">Replace those lines with the following:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample9.xml)]
   4. <span data-ttu-id="685c8-181">Speichern und schließen Sie die **DetailsPage.xaml** Datei.</span><span class="sxs-lookup"><span data-stu-id="685c8-181">Save and close the **DetailsPage.xaml** file.</span></span>

9. <span data-ttu-id="685c8-182">Erstellen Sie die Windows Phone-Anwendung auf Fehler überprüfen.</span><span class="sxs-lookup"><span data-stu-id="685c8-182">Build the Windows Phone application to check for errors.</span></span>

### <a name="step-3-testing-the-end-to-end-solution"></a><span data-ttu-id="685c8-183">Schritt 3: Testen der End-to-End-Lösung</span><span class="sxs-lookup"><span data-stu-id="685c8-183">Step 3: Testing the End-to-End Solution</span></span>

<span data-ttu-id="685c8-184">Siehe die *Voraussetzungen* Abschnitt dieses Tutorials, die beim Testen der Konnektivität zwischen Web-API und Windows Phone 8-Projekte auf dem lokalen System, müssen Sie die Anleitung in der *[ Web-API-Anwendungen auf einem lokalen Computer mit Windows Phone 8 Emulator](https://go.microsoft.com/fwlink/?LinkId=324014)* Artikel zum Einrichten Ihrer testumgebung.</span><span class="sxs-lookup"><span data-stu-id="685c8-184">As mentioned in the *Prerequisites* section of this tutorial, when you are testing the connectivity between Web API and Windows Phone 8 projects on your local system, you will need to follow the instructions in the *[Connecting the Windows Phone 8 Emulator to Web API Applications on a Local Computer](https://go.microsoft.com/fwlink/?LinkId=324014)* article to set up your testing environment.</span></span>

<span data-ttu-id="685c8-185">Nachdem Sie die testumgebung konfiguriert haben, müssen Sie die Windows Phone-Anwendung als Startprojekt festlegen.</span><span class="sxs-lookup"><span data-stu-id="685c8-185">Once you have the testing environment configured, you will need to set the Windows Phone application as the startup project.</span></span> <span data-ttu-id="685c8-186">Markieren Sie hierzu die **BookCatalog** im Projektmappen-Explorer, und klicken Sie dann auf Anwendung **als Startprojekt festlegen**:</span><span class="sxs-lookup"><span data-stu-id="685c8-186">To do so, highlight the **BookCatalog** application in the solution explorer, and then click **Set as StartUp Project**:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image6.png)](calling-web-api-from-a-windows-phone-8-application/_static/image5.png) |
| --- |
| <span data-ttu-id="685c8-187">Klicken Sie auf Bild zu erweitern</span><span class="sxs-lookup"><span data-stu-id="685c8-187">Click image to expand</span></span> |

<span data-ttu-id="685c8-188">Wenn Sie F5 drücken, startet Visual Studio sowohl die Windows Phone-Emulator, der angezeigt wird eine &quot;warten&quot; Nachricht, während die Anwendungsdaten aus Ihrer Web-API abgerufen werden:</span><span class="sxs-lookup"><span data-stu-id="685c8-188">When you press F5, Visual Studio will start both the Windows Phone Emulator, which will display a &quot;Please Wait&quot; message while the application data is retrieved from your Web API:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image8.png)](calling-web-api-from-a-windows-phone-8-application/_static/image7.png) |
| --- |
| <span data-ttu-id="685c8-189">Klicken Sie auf Bild zu erweitern</span><span class="sxs-lookup"><span data-stu-id="685c8-189">Click image to expand</span></span> |

<span data-ttu-id="685c8-190">Wenn alles erfolgreich ist, sehen Sie den Katalog angezeigt:</span><span class="sxs-lookup"><span data-stu-id="685c8-190">If everything is successful, you should see the catalog displayed:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image10.png)](calling-web-api-from-a-windows-phone-8-application/_static/image9.png) |
| --- |
| <span data-ttu-id="685c8-191">Klicken Sie auf Bild zu erweitern</span><span class="sxs-lookup"><span data-stu-id="685c8-191">Click image to expand</span></span> |

<span data-ttu-id="685c8-192">Wenn Sie sich auf alle Buchtitel tippen, wird die Anwendung die Buch-Beschreibung angezeigt:</span><span class="sxs-lookup"><span data-stu-id="685c8-192">If you tap on any book title, the application will display the book description:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image12.png)](calling-web-api-from-a-windows-phone-8-application/_static/image11.png) |
| --- |
| <span data-ttu-id="685c8-193">Klicken Sie auf Bild zu erweitern</span><span class="sxs-lookup"><span data-stu-id="685c8-193">Click image to expand</span></span> |

<span data-ttu-id="685c8-194">Wenn die Anwendung nicht mit Ihrer Web-API kommunizieren kann, wird eine Fehlermeldung angezeigt:</span><span class="sxs-lookup"><span data-stu-id="685c8-194">If the application cannot communicate with your Web API, an error message will be displayed:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image14.png)](calling-web-api-from-a-windows-phone-8-application/_static/image13.png) |
| --- |
| <span data-ttu-id="685c8-195">Klicken Sie auf Bild zu erweitern</span><span class="sxs-lookup"><span data-stu-id="685c8-195">Click image to expand</span></span> |

<span data-ttu-id="685c8-196">Wenn Sie auf die Fehlermeldung tippen, werden zusätzliche Details zum Fehler angezeigt:</span><span class="sxs-lookup"><span data-stu-id="685c8-196">If you tap on the error message, any additional details about the error will be displayed:</span></span>


| [![](calling-web-api-from-a-windows-phone-8-application/_static/image16.png)](calling-web-api-from-a-windows-phone-8-application/_static/image15.png) |
|-------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                                                 <span data-ttu-id="685c8-197">Klicken Sie auf Bild zu erweitern</span><span class="sxs-lookup"><span data-stu-id="685c8-197">Click image to expand</span></span>                                                                 |

