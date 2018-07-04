---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
title: 'Teil 4: Modelle und Datenzugriff | Microsoft-Dokumentation'
author: jongalloway
description: Dieser tutorialreihe werden alle Schritte ausgeführt, um die ASP.NET MVC Music Store-beispielanwendung zu erstellen. Teil 4 sind die Modelle und Datenzugriff.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: ab55ca81-ab9b-44a0-8700-dc6da2599335
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
msc.type: authoredcontent
ms.openlocfilehash: ea8fe623a1b59b80fd7f087036b9ed716eafadbe
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402037"
---
<a name="part-4-models-and-data-access"></a><span data-ttu-id="15045-104">Teil 4: Modelle und Datenzugriff</span><span class="sxs-lookup"><span data-stu-id="15045-104">Part 4: Models and Data Access</span></span>
====================
<span data-ttu-id="15045-105">durch [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="15045-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="15045-106">Die MVC Music Store ist ein lernprogrammanwendung, die eingeführt und erläutert Schritt für Schritt, wie ASP.NET MVC und Visual Studio für die Webentwicklung verwenden.</span><span class="sxs-lookup"><span data-stu-id="15045-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="15045-107">Die MVC Music Store ist eine Implementierung eines einfachen Beispiels die Alben online verkauft und implementiert grundlegende Verwaltung, Benutzeranmeldung und shopping Cart-Funktionalität.</span><span class="sxs-lookup"><span data-stu-id="15045-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>
> 
> <span data-ttu-id="15045-108">Dieser tutorialreihe werden alle Schritte ausgeführt, um die ASP.NET MVC Music Store-beispielanwendung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="15045-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="15045-109">Teil 4 sind die Modelle und Datenzugriff.</span><span class="sxs-lookup"><span data-stu-id="15045-109">Part 4 covers Models and Data Access.</span></span>


<span data-ttu-id="15045-110">Bisher haben wir nur "unechten Daten" aus unserer Controllern an unsere Vorlagen anzeigen übergeben wurde.</span><span class="sxs-lookup"><span data-stu-id="15045-110">So far, we've just been passing "dummy data" from our Controllers to our View templates.</span></span> <span data-ttu-id="15045-111">Wir nun können eine echte Datenbank verknüpft.</span><span class="sxs-lookup"><span data-stu-id="15045-111">Now we're ready to hook up a real database.</span></span> <span data-ttu-id="15045-112">In diesem Tutorial werden wir mit SQL Server Compact Edition (häufig SQL CE genannt) als unsere Datenbank-Engine behandelt.</span><span class="sxs-lookup"><span data-stu-id="15045-112">In this tutorial we'll be covering how to use SQL Server Compact Edition (often called SQL CE) as our database engine.</span></span> <span data-ttu-id="15045-113">SQL CE ist eine kostenlose, eingebettete, dateibasierte Datenbank, die keine erfordert Installation oder Konfiguration, dadurch wird es wirklich praktisch, für die lokale Entwicklung.</span><span class="sxs-lookup"><span data-stu-id="15045-113">SQL CE is a free, embedded, file based database that doesn't require any installation or configuration, which makes it really convenient for local development.</span></span>

## <a name="database-access-with-entity-framework-code-first"></a><span data-ttu-id="15045-114">Zugriff auf die Datenbank mit Entity Framework Code First</span><span class="sxs-lookup"><span data-stu-id="15045-114">Database access with Entity Framework Code-First</span></span>

<span data-ttu-id="15045-115">Wir verwenden die Unterstützung von Entity Framework (EF), die in ASP.NET MVC 3-Projekten, zum Abfragen und aktualisieren Sie die Datenbank enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="15045-115">We'll use the Entity Framework (EF) support that is included in ASP.NET MVC 3 projects to query and update the database.</span></span> <span data-ttu-id="15045-116">EF ist ein flexibles Objekt relational mapping (ORM), Daten-API, die Entwicklern, Abfragen und Aktualisieren von Daten in einer Datenbank in eine objektorientierte Weise gespeichert ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="15045-116">EF is a flexible object relational mapping (ORM) data API that enables developers to query and update data stored in a database in an object-oriented way.</span></span>

<span data-ttu-id="15045-117">Entitätsframework, Version 4 unterstützt ein bezeichnet der Code-First-Entwicklung-Paradigma.</span><span class="sxs-lookup"><span data-stu-id="15045-117">Entity Framework version 4 supports a development paradigm called code-first.</span></span> <span data-ttu-id="15045-118">Code First können Sie Model-Objekts zu erstellen, durch das Schreiben von einfachen Klassen (auch bekannt als POCO von "Plain-Old" CLR Objects), und Sie können auch die Datenbank während der Bearbeitung von Klassen erstellen.</span><span class="sxs-lookup"><span data-stu-id="15045-118">Code-first allows you to create model object by writing simple classes (also known as POCO from "plain-old" CLR objects), and can even create the database on the fly from your classes.</span></span>

### <a name="changes-to-our-model-classes"></a><span data-ttu-id="15045-119">Änderungen an unserem ViewModel-Klassen</span><span class="sxs-lookup"><span data-stu-id="15045-119">Changes to our Model Classes</span></span>

<span data-ttu-id="15045-120">Es wird das Datenbank-erstellen-Feature in Entity Framework in diesem Tutorial nutzen.</span><span class="sxs-lookup"><span data-stu-id="15045-120">We will be leveraging the database creation feature in Entity Framework in this tutorial.</span></span> <span data-ttu-id="15045-121">Bevor wir dies tun, stellen jedoch einige kleinere Änderungen lassen Sie uns auf unsere Modellklassen Punkte hinzufügen, die wir später verwenden.</span><span class="sxs-lookup"><span data-stu-id="15045-121">Before we do that, though, let's make a few minor changes to our model classes to add in some things we'll be using later on.</span></span>

#### <a name="adding-the-artist-model-classes"></a><span data-ttu-id="15045-122">Hinzufügen von Modellklassen der Künstler diese geändert hat</span><span class="sxs-lookup"><span data-stu-id="15045-122">Adding the Artist Model Classes</span></span>

<span data-ttu-id="15045-123">Unsere Alben werden Künstler, zugeordnet werden, damit wir eine einfache Modellklasse, die zum Beschreiben eines Interpreten hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="15045-123">Our Albums will be associated with Artists, so we'll add a simple model class to describe an Artist.</span></span> <span data-ttu-id="15045-124">Fügen Sie eine neue Klasse, um den Ordner "Models", die mit dem Namen Artist.cs mit den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="15045-124">Add a new class to the Models folder named Artist.cs using the code shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample1.cs)]

#### <a name="updating-our-model-classes"></a><span data-ttu-id="15045-125">Aktualisieren die Modellklassen</span><span class="sxs-lookup"><span data-stu-id="15045-125">Updating our Model Classes</span></span>

<span data-ttu-id="15045-126">Aktualisieren Sie die Album-Klasse, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="15045-126">Update the Album class as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample2.cs)]

<span data-ttu-id="15045-127">Stellen Sie als Nächstes die folgenden Updates auf die Klasse "Genre" ein.</span><span class="sxs-lookup"><span data-stu-id="15045-127">Next, make the following updates to the Genre class.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample3.cs)]

### <a name="adding-the-appdata-folder"></a><span data-ttu-id="15045-128">Hinzufügen der App\_Datenordner</span><span class="sxs-lookup"><span data-stu-id="15045-128">Adding the App\_Data folder</span></span>

<span data-ttu-id="15045-129">Fügen wir eine App\_Datenverzeichnis auf das Projekt aus, um unsere SQL Server Express-Datenbankdateien speichern.</span><span class="sxs-lookup"><span data-stu-id="15045-129">We'll add an App\_Data directory to our project to hold our SQL Server Express database files.</span></span> <span data-ttu-id="15045-130">App\_Daten ist ein besonderes Verzeichnis in ASP.NET die bereits die richtige Sicherheits-Zugriffsberechtigungen für den Zugriff auf die Datenbank hat.</span><span class="sxs-lookup"><span data-stu-id="15045-130">App\_Data is a special directory in ASP.NET which already has the correct security access permissions for database access.</span></span> <span data-ttu-id="15045-131">Wählen Sie im Menü Projekt ASP.NET-Ordner hinzufügen und dann die App\_Daten.</span><span class="sxs-lookup"><span data-stu-id="15045-131">From the Project menu, select Add ASP.NET Folder, then App\_Data.</span></span>

![](mvc-music-store-part-4/_static/image1.png)

### <a name="creating-a-connection-string-in-the-webconfig-file"></a><span data-ttu-id="15045-132">Erstellen einer Verbindungszeichenfolge in der Datei "Web.config"</span><span class="sxs-lookup"><span data-stu-id="15045-132">Creating a Connection String in the web.config file</span></span>

<span data-ttu-id="15045-133">Wir werden einige Zeilen der Website-Konfigurationsdatei hinzufügen, damit Entity Framework erkennt, Herstellen einer Verbindung mit der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="15045-133">We will add a few lines to the website's configuration file so that Entity Framework knows how to connect to our database.</span></span> <span data-ttu-id="15045-134">Doppelklicken Sie auf die Datei "Web.config" im Stammverzeichnis des Projekts.</span><span class="sxs-lookup"><span data-stu-id="15045-134">Double-click on the Web.config file located in the root of the project.</span></span>

![](mvc-music-store-part-4/_static/image2.png)

<span data-ttu-id="15045-135">Führen Sie einen Bildlauf zum unteren Rand dieser Datei, und fügen eine &lt;ConnectionStrings&gt; direkt oberhalb der letzten Zeile im Abschnitt, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="15045-135">Scroll to the bottom of this file and add a &lt;connectionStrings&gt; section directly above the last line, as shown below.</span></span>

[!code-xml[Main](mvc-music-store-part-4/samples/sample4.xml)]

### <a name="adding-a-context-class"></a><span data-ttu-id="15045-136">Hinzufügen einer Context-Klasse</span><span class="sxs-lookup"><span data-stu-id="15045-136">Adding a Context Class</span></span>

<span data-ttu-id="15045-137">Mit der rechten Maustaste in den Ordner "Models", und fügen Sie eine neue Klasse namens MusicStoreEntities.cs hinzu.</span><span class="sxs-lookup"><span data-stu-id="15045-137">Right-click the Models folder and add a new class named MusicStoreEntities.cs.</span></span>

![](mvc-music-store-part-4/_static/image3.png)

<span data-ttu-id="15045-138">Diese Klasse wird den Entity Framework-Datenbankkontext darstellen und wird verarbeiten unserer erstellen, lesen, aktualisieren und delete-Operationen für uns.</span><span class="sxs-lookup"><span data-stu-id="15045-138">This class will represent the Entity Framework database context, and will handle our create, read, update, and delete operations for us.</span></span> <span data-ttu-id="15045-139">Der Code für diese Klasse ist unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="15045-139">The code for this class is shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample5.cs)]

<span data-ttu-id="15045-140">Das ist schon alles – es ist keine weitere Konfiguration, spezielle Schnittstellen usw. Unsere MusicStoreEntities-Klasse kann durch die Erweiterung der DbContext-Basisklasse, unsere Datenbankvorgänge für uns zu behandeln.</span><span class="sxs-lookup"><span data-stu-id="15045-140">That's it - there's no other configuration, special interfaces, etc. By extending the DbContext base class, our MusicStoreEntities class is able to handle our database operations for us.</span></span> <span data-ttu-id="15045-141">Nun, da wir, die eingebunden haben, fügen Sie einigen weitere Eigenschaften auf unserem Modellklassen einige zusätzliche Informationen in unserer Datenbank nutzen.</span><span class="sxs-lookup"><span data-stu-id="15045-141">Now that we've got that hooked up, let's add a few more properties to our model classes to take advantage of some of the additional information in our database.</span></span>

### <a name="adding-our-store-catalog-data"></a><span data-ttu-id="15045-142">Unsere Daten der Store-Katalog hinzufügen</span><span class="sxs-lookup"><span data-stu-id="15045-142">Adding our store catalog data</span></span>

<span data-ttu-id="15045-143">Es wird ein Feature in Entity Framework nutzen "Seed"-Daten in eine neu erstellte Datenbank hinzufügt.</span><span class="sxs-lookup"><span data-stu-id="15045-143">We will take advantage of a feature in Entity Framework which adds "seed" data to a newly created database.</span></span> <span data-ttu-id="15045-144">Unsere Store-Katalog mit einer Liste von Genres und Interpreten, Alben ausgefüllt.</span><span class="sxs-lookup"><span data-stu-id="15045-144">This will pre-populate our store catalog with a list of Genres, Artists, and Albums.</span></span> <span data-ttu-id="15045-145">Der MvcMusicStore-Assets.zip Download – was unsere Website Design-Dateien, die weiter oben in diesem Tutorial verwendete enthalten – verfügt über eine Klassendatei mit Seed-Daten, befindet sich in einem Ordner namens Code.</span><span class="sxs-lookup"><span data-stu-id="15045-145">The MvcMusicStore-Assets.zip download - which included our site design files used earlier in this tutorial - has a class file with this seed data, located in a folder named Code.</span></span>

<span data-ttu-id="15045-146">Innerhalb des Codes / Ordner "Models", suchen Sie die SampleData.cs-Datei, und legen Sie sie in den Ordner "Models" in Ihrem Projekt, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="15045-146">Within the Code / Models folder, locate the SampleData.cs file and drop it into the Models folder in our project, as shown below.</span></span>

![](mvc-music-store-part-4/_static/image4.png)

<span data-ttu-id="15045-147">Nun müssen wir eine Codezeile erforderlich, um mitzuteilen, Entity Framework über die SampleData-Klasse hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="15045-147">Now we need to add one line of code to tell Entity Framework about that SampleData class.</span></span> <span data-ttu-id="15045-148">Doppelklicken Sie auf die Datei "Global.asax" im Stammverzeichnis des Projekts, öffnen es, und fügen die folgende Zeile am Anfang die Anwendung\_Start-Methode.</span><span class="sxs-lookup"><span data-stu-id="15045-148">Double-click on the Global.asax file in the root of the project to open it and add the following line to the top the Application\_Start method.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample6.cs)]

<span data-ttu-id="15045-149">An diesem Punkt haben wir die erforderlichen Schritte zum Konfigurieren von Entity Framework für unser Projekt abgeschlossen.</span><span class="sxs-lookup"><span data-stu-id="15045-149">At this point, we've completed the work necessary to configure Entity Framework for our project.</span></span>

## <a name="querying-the-database"></a><span data-ttu-id="15045-150">Abfragen der Datenbank</span><span class="sxs-lookup"><span data-stu-id="15045-150">Querying the Database</span></span>

<span data-ttu-id="15045-151">Jetzt aktualisieren wir unsere StoreController, damit anstelle von "dummy-Daten" stattdessen unsere Datenbank alle zugehörigen Informationen Abfragen aufrufen.</span><span class="sxs-lookup"><span data-stu-id="15045-151">Now let's update our StoreController so that instead of using "dummy data" it instead calls into our database to query all of its information.</span></span> <span data-ttu-id="15045-152">Wir beginnen mit Deklarieren eines Feldes in der **StoreController** , die eine Instanz der MusicStoreEntities-Klasse, mit dem Namen StoreDB enthalten soll:</span><span class="sxs-lookup"><span data-stu-id="15045-152">We'll start by declaring a field on the **StoreController** to hold an instance of the MusicStoreEntities class, named storeDB:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample7.cs)]

### <a name="updating-the-store-index-to-query-the-database"></a><span data-ttu-id="15045-153">Aktualisieren des Store-Index zum Abfragen der Datenbank</span><span class="sxs-lookup"><span data-stu-id="15045-153">Updating the Store Index to query the database</span></span>

<span data-ttu-id="15045-154">Die MusicStoreEntities-Klasse wird vom Entity Framework verwaltet und stellt eine Auflistungseigenschaft für jede Tabelle in der Datenbank zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="15045-154">The MusicStoreEntities class is maintained by the Entity Framework and exposes a collection property for each table in our database.</span></span> <span data-ttu-id="15045-155">Wir aktualisieren unsere StoreControllers Index-Aktion zum Abrufen von allen Genres in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="15045-155">Let's update our StoreController's Index action to retrieve all Genres in our database.</span></span> <span data-ttu-id="15045-156">Zuvor haben wir dies von Zeichenfolgendaten fest zu programmieren.</span><span class="sxs-lookup"><span data-stu-id="15045-156">Previously we did this by hard-coding string data.</span></span> <span data-ttu-id="15045-157">Jetzt können wir die Entity Framework-Datenkontext Generes Sammlung stattdessen nur verwenden:</span><span class="sxs-lookup"><span data-stu-id="15045-157">Now we can instead just use the Entity Framework context Generes collection:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample8.cs)]

<span data-ttu-id="15045-158">Es müssen keine Änderungen, unsere Vorlage für die Ansicht ausgeführt werden, da wir weiterhin die gleichen StoreIndexViewModel zurückgeben, bevor: wir nur Livedaten aus unserer Datenbank jetzt zurückgeben, zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="15045-158">No changes need to happen to our View template since we're still returning the same StoreIndexViewModel we returned before - we're just returning live data from our database now.</span></span>

<span data-ttu-id="15045-159">Wenn wir führen das Projekt erneut aus, und rufen Sie die URL "/ Store", sehen wir jetzt eine Liste mit allen Genres in unserer Datenbank:</span><span class="sxs-lookup"><span data-stu-id="15045-159">When we run the project again and visit the "/Store" URL, we'll now see a list of all Genres in our database:</span></span>

![](mvc-music-store-part-4/_static/image1.jpg)

### <a name="updating-store-browse-and-details-to-use-live-data"></a><span data-ttu-id="15045-160">Aktualisieren von Store durchsuchen und Details zur Verwendung von live-Daten</span><span class="sxs-lookup"><span data-stu-id="15045-160">Updating Store Browse and Details to use live data</span></span>

<span data-ttu-id="15045-161">Durch den/Store/durchsuchen? Genre =*[einige Genre]* Aktionsmethode suchen wir nach einer "Genre" anhand des Namens.</span><span class="sxs-lookup"><span data-stu-id="15045-161">With the /Store/Browse?genre=*[some-genre]* action method, we're searching for a Genre by name.</span></span> <span data-ttu-id="15045-162">Wir nur ein Ergebnis, erwarten, da wir immer zwei Einträge für denselben Namen "Genre" haben darf nicht, weshalb wir verwenden können, die. Single() Erweiterung in LINQ-Abfrage für das entsprechende Objekt für "Genre" wie folgt (nicht-Typ dies noch):</span><span class="sxs-lookup"><span data-stu-id="15045-162">We only expect one result, since we shouldn't ever have two entries for the same Genre name, and so we can use the .Single() extension in LINQ to query for the appropriate Genre object like this (don't type this yet):</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample9.cs)]

<span data-ttu-id="15045-163">Die einzelne Methode nimmt einen Lambdaausdruck als Parameter, der angibt, dass ein einzelnes "Genre"-Objekt, dessen Name mit dem Wert übereinstimmt, die, den wir definiert haben, soll.</span><span class="sxs-lookup"><span data-stu-id="15045-163">The Single method takes a Lambda expression as a parameter, which specifies that we want a single Genre object such that its name matches the value we've defined.</span></span> <span data-ttu-id="15045-164">Im obigen Fall werden wir ein einzelnes "Genre"-Objekt mit dem Namenswert, der übereinstimmende Disco geladen.</span><span class="sxs-lookup"><span data-stu-id="15045-164">In the case above, we are loading a single Genre object with a Name value matching Disco.</span></span>

<span data-ttu-id="15045-165">Werfen wir einen Entity Framework-Feature nutzen, mit der wir an, dass andere verbundene Entitäten, die ebenfalls geladen werden soll, wenn sich das Genre-Objekt abgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="15045-165">We'll take advantage of an Entity Framework feature that allows us to indicate other related entities we want loaded as well when the Genre object is retrieved.</span></span> <span data-ttu-id="15045-166">Dieses Feature heißt die Abfrage Ergebnis strukturieren und ermöglicht es uns, um die Anzahl der zu verringern, müssen wir Zugriff auf die Datenbank, um alle Informationen abrufen, die wir benötigen.</span><span class="sxs-lookup"><span data-stu-id="15045-166">This feature is called Query Result Shaping, and enables us to reduce the number of times we need to access the database to retrieve all of the information we need.</span></span> <span data-ttu-id="15045-167">Wir möchten die Alben für "Genre" Wir abzurufen, vorab abgerufen werden, damit wir unsere Abfrage durch Hinzufügen von Genres.Include("Albums") aktualisieren, um anzugeben, dass auch die zugehörigen Alben werden soll.</span><span class="sxs-lookup"><span data-stu-id="15045-167">We want to pre-fetch the Albums for Genre we retrieve, so we'll update our query to include from Genres.Include("Albums") to indicate that we want related albums as well.</span></span> <span data-ttu-id="15045-168">Dies ist effizienter, da es sowohl unser "Genre" als auch das Album Daten in einer einzelnen Datenbank-Anforderung abgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="15045-168">This is more efficient, since it will retrieve both our Genre and Album data in a single database request.</span></span>

<span data-ttu-id="15045-169">Mit den Erklärungen nicht im Weg sieht wie unsere aktualisierte durchsuchen-Controlleraktion:</span><span class="sxs-lookup"><span data-stu-id="15045-169">With the explanations out of the way, here's how our updated Browse controller action looks:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample10.cs)]

<span data-ttu-id="15045-170">Wir können jetzt aktualisieren der Store durchsuchen-Ansicht zum Anzeigen von Alben, die in jeder "Genre" verfügbar sind.</span><span class="sxs-lookup"><span data-stu-id="15045-170">We can now update the Store Browse View to display the albums which are available in each Genre.</span></span> <span data-ttu-id="15045-171">Öffnen Sie die ansichtsvorlage (gefunden in /Views/Store/Browse.cshtml), und fügen Sie eine Aufzählung von Alben, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="15045-171">Open the view template (found in /Views/Store/Browse.cshtml) and add a bulleted list of Albums as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample11.cshtml)]

<span data-ttu-id="15045-172">Ausführung unserer Anwendung, und navigieren zu/Store/durchsuchen? Genre = Jazz zeigt an, die aus der Datenbank, Anzeigen von alle Alben in unserer ausgewählte Genre jetzt unsere Ergebnisse abgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="15045-172">Running our application and browsing to /Store/Browse?genre=Jazz shows that our results are now being pulled from the database, displaying all albums in our selected Genre.</span></span>

![](mvc-music-store-part-4/_static/image2.jpg)

<span data-ttu-id="15045-173">Wir stellen die gleiche ändern Sie in unserem/Store/Details / [Id] URL, und Ersetzen Sie unsere unechten Daten durch die Abfrage einer Datenbank, die ein Album lädt, deren ID mit dem Wert des Parameters übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="15045-173">We'll make the same change to our /Store/Details/[id] URL, and replace our dummy data with a database query which loads an Album whose ID matches the parameter value.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample12.cs)]

<span data-ttu-id="15045-174">Unsere Anwendung ausführen, und Navigieren zum /Store/Details/1 zeigt, dass unsere Ergebnisse jetzt aus der Datenbank abgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="15045-174">Running our application and browsing to /Store/Details/1 shows that our results are now being pulled from the database.</span></span>

![](mvc-music-store-part-4/_static/image5.png)

<span data-ttu-id="15045-175">Nun, da unsere Seite "Store-Details" zum Anzeigen eines Albums durch die ID des Albums eingerichtet ist, aktualisieren wir die **Durchsuchen** anzeigen, um mit der Detailansicht zu verknüpfen.</span><span class="sxs-lookup"><span data-stu-id="15045-175">Now that our Store Details page is set up to display an album by the Album ID, let's update the **Browse** view to link to the Details view.</span></span> <span data-ttu-id="15045-176">Wir verwenden Html.ActionLink, genau wie aus dem Store Index Store durchsuchen am Ende des vorherigen Abschnitts verknüpfen.</span><span class="sxs-lookup"><span data-stu-id="15045-176">We will use Html.ActionLink, exactly as we did to link from Store Index to Store Browse at the end of the previous section.</span></span> <span data-ttu-id="15045-177">Der vollständige Quellcode für die Ansicht durchsuchen wird unten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="15045-177">The complete source for the Browse view appears below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample13.cshtml)]

<span data-ttu-id="15045-178">Jetzt können wir aus unserer Seite "Store" auf die Seite "Genre" suchen, die die Listen der verfügbaren Alben und durch Klicken auf ein Album wir können Details anzeigen, für die jeweiligen Album zu sehen.</span><span class="sxs-lookup"><span data-stu-id="15045-178">We're now able to browse from our Store page to a Genre page, which lists the available albums, and by clicking on an album we can view details for that album.</span></span>

![](mvc-music-store-part-4/_static/image6.png)

> [!div class="step-by-step"]
> <span data-ttu-id="15045-179">[Zurück](mvc-music-store-part-3.md)
> [Weiter](mvc-music-store-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="15045-179">[Previous](mvc-music-store-part-3.md)
[Next](mvc-music-store-part-5.md)</span></span>
