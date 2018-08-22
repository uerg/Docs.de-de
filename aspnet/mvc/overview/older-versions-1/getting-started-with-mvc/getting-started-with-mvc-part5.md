---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
title: Zugreifen auf Modelldaten anhand eines Controllers | Microsoft-Dokumentation
author: shanselman
description: Dies ist ein Tutorial für Anfänger, die die Grundlagen von ASP.NET MVC eingeführt werden. Erstellen Sie eine einfache Webanwendung, die aus einer Datenbank liest und schreibt.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: 004703cd-e0e9-4ba7-9974-1b0475c71222
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
msc.type: authoredcontent
ms.openlocfilehash: 76dc324134dc93c9552741fea9f1136abdc9184a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827361"
---
<a name="accessing-your-models-data-from-a-controller"></a><span data-ttu-id="3cec4-104">Zugreifen auf Modelldaten anhand eines Controllers</span><span class="sxs-lookup"><span data-stu-id="3cec4-104">Accessing your Model's Data from a Controller</span></span>
====================
<span data-ttu-id="3cec4-105">durch [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="3cec4-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="3cec4-106">Dies ist ein Tutorial für Anfänger, die die Grundlagen von ASP.NET MVC eingeführt werden.</span><span class="sxs-lookup"><span data-stu-id="3cec4-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="3cec4-107">Sie erstellen eine einfache Webanwendung, die aus einer Datenbank liest und schreibt.</span><span class="sxs-lookup"><span data-stu-id="3cec4-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="3cec4-108">Besuchen Sie die [ASP.NET MVC-Informationscenter](../../../index.md) anderen ASP.NET MVC anhand von Tutorials und Beispiele finden.</span><span class="sxs-lookup"><span data-stu-id="3cec4-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="3cec4-109">In diesem Abschnitt werden wir erstellen eine neue MoviesController-Klasse, und Schreiben von Code, der unsere Daten abgerufen und wieder an den Browser, die mithilfe einer ansichtsvorlage anzeigt.</span><span class="sxs-lookup"><span data-stu-id="3cec4-109">In this section we are going to create a new MoviesController class, and write some code that retrieves our Movie data and displays it back to the browser using a View template.</span></span>

<span data-ttu-id="3cec4-110">Klicken Sie mit der rechten Maustaste auf den Ordner "Controllers", und stellen Sie eine neue MoviesController.</span><span class="sxs-lookup"><span data-stu-id="3cec4-110">Right click on the Controllers folder and make a new MoviesController.</span></span>

<span data-ttu-id="3cec4-111">[![Controller hinzufügen](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="3cec4-111">[![Add Controller](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)</span></span>

<span data-ttu-id="3cec4-112">Dadurch wird eine neue "MoviesController.cs"-Datei unterhalb unserer \Controllers-Ordner, in unserem Projekt erstellt.</span><span class="sxs-lookup"><span data-stu-id="3cec4-112">This will create a new "MoviesController.cs" file underneath our \Controllers folder within our project.</span></span> <span data-ttu-id="3cec4-113">Aktualisieren wir die MovieController Abrufen einer Liste von Filmen aus unserer Datenbank neu aufgefüllt.</span><span class="sxs-lookup"><span data-stu-id="3cec4-113">Let's update the MovieController to retrieve the list of movies from our newly populated database.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part5/samples/sample1.cs)]

<span data-ttu-id="3cec4-114">Wir werden eine LINQ-Abfrage ausführen, so, dass wir nur freigegeben, nachdem der Sommer 1984 Filme abgerufen.</span><span class="sxs-lookup"><span data-stu-id="3cec4-114">We are performing a LINQ query so that we only retrieve movies released after the summer of 1984.</span></span> <span data-ttu-id="3cec4-115">Wir benötigen eine ansichtsvorlage zum Rendern dieser Liste von Filmen zurück, also mit der rechten Maustaste in der Methode und wählen "Ansicht hinzufügen", um es zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="3cec4-115">We'll need a View template to render this list of movies back, so right-click in the method and select Add View to create it.</span></span>

<span data-ttu-id="3cec4-116">Wir müssen das Dialogfeld "Ansicht hinzufügen" um anzugeben, dass wir eine Liste übergeben&lt;Movies.Models.Movie&gt; , unsere Vorlage anzeigen.</span><span class="sxs-lookup"><span data-stu-id="3cec4-116">Within the Add View dialog we'll indicate that we are passing a List&lt;Movies.Models.Movie&gt; to our View template.</span></span> <span data-ttu-id="3cec4-117">Im Gegensatz zu den älteren Zeiten, die wir verwendet das Dialogfeld "Ansicht hinzufügen", und wählen Sie eine "Leere" Vorlage erstellen, möchten wir werden anzugeben, dass diesmal wir Visual Studio automatisch "eine ansichtsvorlage für uns mit einigen Standardinhalt Gerüst für".</span><span class="sxs-lookup"><span data-stu-id="3cec4-117">Unlike the previous times we used the Add View dialog and chose to create an "Empty" template, this time we'll indicate that we want Visual Studio to automatically "scaffold" a view template for us with some default content.</span></span> <span data-ttu-id="3cec4-118">Wir werden dies durch Auswählen des Elements "List" in "Content-Dropdownliste im Menü Ansicht.</span><span class="sxs-lookup"><span data-stu-id="3cec4-118">We'll do this by selecting the "List" item within the "View content dropdown menu.</span></span>

<span data-ttu-id="3cec4-119">Denken Sie daran: Wenn Sie ein erstelltes haben eine neue Klasse Sie Kompilieren Ihrer Anwendung für die es in das Dialogfeld "Ansicht-" angezeigt wird müssen.</span><span class="sxs-lookup"><span data-stu-id="3cec4-119">Remember, when you have a created a new class you'll need to compile your application for it to show up in the Add View Dialog.</span></span>

![Ansicht hinzufügen](getting-started-with-mvc-part5/_static/image3.png)

<span data-ttu-id="3cec4-121">Klicken Sie auf Hinzufügen und das System generiert automatisch Code für eine Ansicht für uns, in dem unsere Liste von Filmen angezeigt.</span><span class="sxs-lookup"><span data-stu-id="3cec4-121">Click add and the system will automatically generate the code for a View for us that displays our list of movies.</span></span> <span data-ttu-id="3cec4-122">Dies ist ein guter Zeitpunkt zum Ändern der &lt;h2&gt; Überschrift z. B. "Meine Movie List", wie zuvor mit der "Hello World" angezeigt.</span><span class="sxs-lookup"><span data-stu-id="3cec4-122">This is a good time to change the &lt;h2&gt; heading to something like "My Movie List" like we did earlier with the Hello World view.</span></span>

<span data-ttu-id="3cec4-123">[![Filme – Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="3cec4-123">[![Movies - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)</span></span>

<span data-ttu-id="3cec4-124">Führen Sie die Anwendung, und finden Sie in der Adressleiste auf /Movies.</span><span class="sxs-lookup"><span data-stu-id="3cec4-124">Run your application and visit /Movies in the address bar.</span></span> <span data-ttu-id="3cec4-125">Jetzt haben wir die Daten mit der eine einfache Abfrage in den Controller aus der Datenbank abgerufen und zurückgegeben, die Daten an eine Ansicht, die über Filme bekannt sind.</span><span class="sxs-lookup"><span data-stu-id="3cec4-125">Now we've retrieved data from the database using a basic query inside the Controller and returned the data to a View that knows about Movies.</span></span> <span data-ttu-id="3cec4-126">Diese Ansicht, und klicken Sie dann führt Spin-Vorgänge über die Liste der Filme und wird eine Tabelle mit Daten für uns erstellt.</span><span class="sxs-lookup"><span data-stu-id="3cec4-126">That View then spins through the list of Movies and creates a table of data for us.</span></span>

<span data-ttu-id="3cec4-127">[![Filmliste – Windows InternetExplorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="3cec4-127">[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)</span></span>

<span data-ttu-id="3cec4-128">Es wird nicht implementiert bearbeiten "," Details "und" Delete-Funktionen mit dieser Anwendung -, sodass wir nicht die Standardlinks benötigen, die die Vorlage Gerüst für uns erstellt.</span><span class="sxs-lookup"><span data-stu-id="3cec4-128">We won't be implementing Edit, Details and Delete functionality with this application - so we don't need the default links that the scaffold template created for us.</span></span> <span data-ttu-id="3cec4-129">Öffnen Sie die /Movies/Index.aspx-Datei, und entfernen Sie sie.</span><span class="sxs-lookup"><span data-stu-id="3cec4-129">Open up the /Movies/Index.aspx file and remove them.</span></span>

<span data-ttu-id="3cec4-130">Hier ist der Quellcode für die wie unsere aktuellen ansichtsvorlage aussehen soll, sobald wir diese Änderungen vornehmen:</span><span class="sxs-lookup"><span data-stu-id="3cec4-130">Here is the source code for what our updated View template should look like once we make these changes:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part5/samples/sample2.aspx)]

<span data-ttu-id="3cec4-131">Es ist die Links, die wir nicht brauchen, Erstellung, daher wird sie für dieses Beispiel entfernt werden.</span><span class="sxs-lookup"><span data-stu-id="3cec4-131">It's creating links that we won't need, so we'll delete them for this example.</span></span> <span data-ttu-id="3cec4-132">Wir bleiben unseren neu erstellen Link jedoch, wie das nächste ist!</span><span class="sxs-lookup"><span data-stu-id="3cec4-132">We will keep our Create New link though, as that's next!</span></span> <span data-ttu-id="3cec4-133">So sieht unsere app wie mit dieser Spalte entfernt.</span><span class="sxs-lookup"><span data-stu-id="3cec4-133">Here's what our app looks like with that column removed.</span></span>

<span data-ttu-id="3cec4-134">[![Filmliste – Windows InternetExplorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="3cec4-134">[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)</span></span>

<span data-ttu-id="3cec4-135">Wir haben jetzt eine einfache Liste von unserem Movie-Daten.</span><span class="sxs-lookup"><span data-stu-id="3cec4-135">We now have a simple listing of our movie data.</span></span> <span data-ttu-id="3cec4-136">Jedoch, wenn wir auf den Link "Neu erstellen" klicken, wir erhalten eine Fehlermeldung, da es nicht verknüpft ist!</span><span class="sxs-lookup"><span data-stu-id="3cec4-136">However, if we click the "Create New" link, we'll get an error as it's not hooked up!</span></span> <span data-ttu-id="3cec4-137">Lassen Sie uns eine Create Action-Methode implementieren, und ermöglichen einem Benutzer zur Eingabe neuer Filme in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="3cec4-137">Let's implement a Create Action method and enable a user to enter new movies in our database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3cec4-138">[Zurück](getting-started-with-mvc-part4.md)
> [Weiter](getting-started-with-mvc-part6.md)</span><span class="sxs-lookup"><span data-stu-id="3cec4-138">[Previous](getting-started-with-mvc-part4.md)
[Next](getting-started-with-mvc-part6.md)</span></span>
