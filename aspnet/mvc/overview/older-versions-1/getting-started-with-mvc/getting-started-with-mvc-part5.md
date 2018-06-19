---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
title: Zugriff auf das Modell Daten aus einem Controller | Microsoft Docs
author: shanselman
description: Dies ist ein Anfänger-Lernprogramm, das die Grundlagen von ASP.NET MVC. Erstellen Sie eine einfache Web-Anwendung, die Lese- und Schreibvorgänge aus einer Datenbank.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 004703cd-e0e9-4ba7-9974-1b0475c71222
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
msc.type: authoredcontent
ms.openlocfilehash: 2ba1b73f40a920e27e4a03d9f703e62054d3f25c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/10/2018
ms.locfileid: "30876435"
---
<a name="accessing-your-models-data-from-a-controller"></a><span data-ttu-id="19c7c-104">Zugriff auf das Modell Daten aus einem Controller</span><span class="sxs-lookup"><span data-stu-id="19c7c-104">Accessing your Model's Data from a Controller</span></span>
====================
<span data-ttu-id="19c7c-105">durch [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="19c7c-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="19c7c-106">Dies ist ein Anfänger-Lernprogramm, das die Grundlagen von ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="19c7c-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="19c7c-107">Erstellen Sie eine einfache Web-Anwendung, die Lese- und Schreibvorgänge aus einer Datenbank.</span><span class="sxs-lookup"><span data-stu-id="19c7c-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="19c7c-108">Besuchen Sie die [ASP.NET MVC-Trainingscenter](../../../index.md) andere ASP.NET-MVC Lernprogramme und Beispiele finden.</span><span class="sxs-lookup"><span data-stu-id="19c7c-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="19c7c-109">In diesem Abschnitt werden wir eine neue MoviesController-Klasse erstellen und Code schreiben, der unsere Filmdaten abgerufen und wieder an den Browser, die mithilfe einer Ansichtenvorlage angezeigt.</span><span class="sxs-lookup"><span data-stu-id="19c7c-109">In this section we are going to create a new MoviesController class, and write some code that retrieves our Movie data and displays it back to the browser using a View template.</span></span>

<span data-ttu-id="19c7c-110">Klicken Sie mit der rechten Maustaste auf den Ordner Controller, und stellen Sie eine neue MoviesController.</span><span class="sxs-lookup"><span data-stu-id="19c7c-110">Right click on the Controllers folder and make a new MoviesController.</span></span>

<span data-ttu-id="19c7c-111">[![Hinzufügen eines Controllers](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="19c7c-111">[![Add Controller](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)</span></span>

<span data-ttu-id="19c7c-112">Dadurch wird eine neue "MoviesController.cs" Datei unterhalb unsere \Controllers Ordner im Projekt erstellt.</span><span class="sxs-lookup"><span data-stu-id="19c7c-112">This will create a new "MoviesController.cs" file underneath our \Controllers folder within our project.</span></span> <span data-ttu-id="19c7c-113">Wir aktualisieren die MovieController, um die Liste von Filmen aus unserer neu aufgefüllt Datenbank abzurufen.</span><span class="sxs-lookup"><span data-stu-id="19c7c-113">Let's update the MovieController to retrieve the list of movies from our newly populated database.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part5/samples/sample1.cs)]

<span data-ttu-id="19c7c-114">Wir Ausführen eine LINQ-Abfrage, damit wir nur freigegeben, nachdem die Sommer 1984 Filme abgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="19c7c-114">We are performing a LINQ query so that we only retrieve movies released after the summer of 1984.</span></span> <span data-ttu-id="19c7c-115">Benötigen wir eine Vorlage anzeigen, diese Liste von Filmen wieder zu rendern, also mit der rechten Maustaste in der Methode und Auswählen von "Ansicht hinzufügen", um ihn zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="19c7c-115">We'll need a View template to render this list of movies back, so right-click in the method and select Add View to create it.</span></span>

<span data-ttu-id="19c7c-116">In das Dialogfeld "Ansicht hinzufügen" fügen wir anzugeben, dass wir eine Liste übergeben&lt;Movies.Models.Movie&gt; zu unserem Vorlage anzeigen.</span><span class="sxs-lookup"><span data-stu-id="19c7c-116">Within the Add View dialog we'll indicate that we are passing a List&lt;Movies.Models.Movie&gt; to our View template.</span></span> <span data-ttu-id="19c7c-117">Im Gegensatz zum älteren Zeiten, die wir das Dialogfeld "Ansicht hinzufügen" verwendet und eine "Empty" Vorlage erstellen möchten, möchten wir anzugeben, dass fügen diesmal wir Visual Studio automatisch "eine Ansichtenvorlage für uns mit einigen Standardinhalt Gerüst".</span><span class="sxs-lookup"><span data-stu-id="19c7c-117">Unlike the previous times we used the Add View dialog and chose to create an "Empty" template, this time we'll indicate that we want Visual Studio to automatically "scaffold" a view template for us with some default content.</span></span> <span data-ttu-id="19c7c-118">Wir müssen dazu "Listenelement" in "Content-Dropdownliste im Menü Ansicht.</span><span class="sxs-lookup"><span data-stu-id="19c7c-118">We'll do this by selecting the "List" item within the "View content dropdown menu.</span></span>

<span data-ttu-id="19c7c-119">Beachten Sie, dass, wenn Sie ein erstelltes haben eine neue Klasse, Sie für die Anwendungskompilierung dafür in der Ansicht-Dialogfeld angezeigt müssen.</span><span class="sxs-lookup"><span data-stu-id="19c7c-119">Remember, when you have a created a new class you'll need to compile your application for it to show up in the Add View Dialog.</span></span>

![Fügen Sie die Ansicht](getting-started-with-mvc-part5/_static/image3.png)

<span data-ttu-id="19c7c-121">Klicken Sie auf Hinzufügen, und das System generiert automatisch den Code für eine Ansicht für uns, die die Liste von Filmen anzeigt.</span><span class="sxs-lookup"><span data-stu-id="19c7c-121">Click add and the system will automatically generate the code for a View for us that displays our list of movies.</span></span> <span data-ttu-id="19c7c-122">Dies ist ein guter Zeitpunkt zum Ändern der &lt;h2&gt; Überschrift z. B. "Meine Film-Liste" wie zuvor mit der "Hello World" angezeigt.</span><span class="sxs-lookup"><span data-stu-id="19c7c-122">This is a good time to change the &lt;h2&gt; heading to something like "My Movie List" like we did earlier with the Hello World view.</span></span>

<span data-ttu-id="19c7c-123">[![Filme – Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="19c7c-123">[![Movies - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)</span></span>

<span data-ttu-id="19c7c-124">Führen Sie die Anwendung, und besuchen Sie /Movies in der Adressleiste.</span><span class="sxs-lookup"><span data-stu-id="19c7c-124">Run your application and visit /Movies in the address bar.</span></span> <span data-ttu-id="19c7c-125">Jetzt haben wir werden Daten aus der Datenbank mithilfe einer einfachen Abfrage innerhalb der Controller abgefragt und die Daten an eine Ansicht, die über Filme weiß zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="19c7c-125">Now we've retrieved data from the database using a basic query inside the Controller and returned the data to a View that knows about Movies.</span></span> <span data-ttu-id="19c7c-126">Diese Sicht wird dann durch die Liste von Filmen stößt und eine Tabelle mit Daten für uns erstellt.</span><span class="sxs-lookup"><span data-stu-id="19c7c-126">That View then spins through the list of Movies and creates a table of data for us.</span></span>

<span data-ttu-id="19c7c-127">[![Film List – Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="19c7c-127">[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)</span></span>

<span data-ttu-id="19c7c-128">Es wird nicht implementieren bearbeiten "," Details "und" Delete-Funktionalität mit dieser Anwendung - daher die Standardlinks brauchen wir, die die Vorlage Gerüst für uns erstellt.</span><span class="sxs-lookup"><span data-stu-id="19c7c-128">We won't be implementing Edit, Details and Delete functionality with this application - so we don't need the default links that the scaffold template created for us.</span></span> <span data-ttu-id="19c7c-129">Öffnen Sie die /Movies/Index.aspx-Datei, und entfernen Sie sie.</span><span class="sxs-lookup"><span data-stu-id="19c7c-129">Open up the /Movies/Index.aspx file and remove them.</span></span>

<span data-ttu-id="19c7c-130">Hier ist der Quellcode für wie unsere aktualisierte Ansichtenvorlage aussehen soll, sobald wir diese Änderungen vorzunehmen:</span><span class="sxs-lookup"><span data-stu-id="19c7c-130">Here is the source code for what our updated View template should look like once we make these changes:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part5/samples/sample2.aspx)]

<span data-ttu-id="19c7c-131">Es wird Links, die wir nicht brauchen, erstellen, sodass wir in diesem Beispiel gelöscht werden.</span><span class="sxs-lookup"><span data-stu-id="19c7c-131">It's creating links that we won't need, so we'll delete them for this example.</span></span> <span data-ttu-id="19c7c-132">Wir behält unsere Link neu erstellen, weiter darin!</span><span class="sxs-lookup"><span data-stu-id="19c7c-132">We will keep our Create New link though, as that's next!</span></span> <span data-ttu-id="19c7c-133">Hier sieht unsere app mit dieser Spalte entfernt.</span><span class="sxs-lookup"><span data-stu-id="19c7c-133">Here's what our app looks like with that column removed.</span></span>

<span data-ttu-id="19c7c-134">[![Film List – Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="19c7c-134">[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)</span></span>

<span data-ttu-id="19c7c-135">Wir haben jetzt eine einfache Liste von unseren Filmdaten.</span><span class="sxs-lookup"><span data-stu-id="19c7c-135">We now have a simple listing of our movie data.</span></span> <span data-ttu-id="19c7c-136">Jedoch wenn wir den Link "Neu erstellen" klicken, wird einen Fehler erhalten, da er nicht verknüpft ist!</span><span class="sxs-lookup"><span data-stu-id="19c7c-136">However, if we click the "Create New" link, we'll get an error as it's not hooked up!</span></span> <span data-ttu-id="19c7c-137">Wir implementieren Sie eine Create Action-Methode, und ermöglichen einem Benutzer, geben neue Kinofilmen in die Datenbank.</span><span class="sxs-lookup"><span data-stu-id="19c7c-137">Let's implement a Create Action method and enable a user to enter new movies in our database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="19c7c-138">[Zurück](getting-started-with-mvc-part4.md)
> [Weiter](getting-started-with-mvc-part6.md)</span><span class="sxs-lookup"><span data-stu-id="19c7c-138">[Previous](getting-started-with-mvc-part4.md)
[Next](getting-started-with-mvc-part6.md)</span></span>
