---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
title: 'Teil 4: Modelle und Datenzugriff | Microsoft Docs'
author: jongalloway
description: Diese Reihe von Lernprogrammen details aller die Schritte zum Erstellen von ASP.NET MVC-Music Store-beispielanwendung. Teil 4 werden die Modelle und Datenzugriff behandelt.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: ab55ca81-ab9b-44a0-8700-dc6da2599335
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
msc.type: authoredcontent
ms.openlocfilehash: 76671bbc7050d111b4d156c45584ba5aa4f1ea8f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879477"
---
<a name="part-4-models-and-data-access"></a><span data-ttu-id="03722-104">Teil 4: Modelle und Datenzugriff</span><span class="sxs-lookup"><span data-stu-id="03722-104">Part 4: Models and Data Access</span></span>
====================
<span data-ttu-id="03722-105">durch [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="03722-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="03722-106">Das MVC-Music Store ist eine lernprogrammanwendung, die führt und erklärt schrittweise, wie mithilfe von ASP.NET MVC und Visual Studio für die Webentwicklung.</span><span class="sxs-lookup"><span data-stu-id="03722-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="03722-107">Das MVC-Music Store handelt es sich um einfache Beispiel Store Implementierung, die online Musikalben verkauft und implementiert grundlegende standortverwaltung Benutzer anmelden und shopping Cart-Funktionalität.</span><span class="sxs-lookup"><span data-stu-id="03722-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>
> 
> <span data-ttu-id="03722-108">Diese Reihe von Lernprogrammen details aller die Schritte zum Erstellen von ASP.NET MVC-Music Store-beispielanwendung.</span><span class="sxs-lookup"><span data-stu-id="03722-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="03722-109">Teil 4 werden die Modelle und Datenzugriff behandelt.</span><span class="sxs-lookup"><span data-stu-id="03722-109">Part 4 covers Models and Data Access.</span></span>


<span data-ttu-id="03722-110">Bisher haben wir nur "dummy-Daten" durch unsere Controller an unsere Ansichtsvorlagen übergeben wurde.</span><span class="sxs-lookup"><span data-stu-id="03722-110">So far, we've just been passing "dummy data" from our Controllers to our View templates.</span></span> <span data-ttu-id="03722-111">Nun können wir eine echte Datenbank verknüpft.</span><span class="sxs-lookup"><span data-stu-id="03722-111">Now we're ready to hook up a real database.</span></span> <span data-ttu-id="03722-112">In diesem Lernprogramm fügen wir zum Verwenden von SQL Server Compact Edition (häufig als SQL CE bezeichnet) als unsere Datenbankmodul abdecken.</span><span class="sxs-lookup"><span data-stu-id="03722-112">In this tutorial we'll be covering how to use SQL Server Compact Edition (often called SQL CE) as our database engine.</span></span> <span data-ttu-id="03722-113">SQL CE ist eine kostenlose, eingebettete, dateibasierte Datenbank, die keine erfordert Installation oder Konfiguration, wodurch das tatsächlich für lokale Entwicklung praktisch.</span><span class="sxs-lookup"><span data-stu-id="03722-113">SQL CE is a free, embedded, file based database that doesn't require any installation or configuration, which makes it really convenient for local development.</span></span>

## <a name="database-access-with-entity-framework-code-first"></a><span data-ttu-id="03722-114">Datenbankzugriff mit Entity Framework Code First</span><span class="sxs-lookup"><span data-stu-id="03722-114">Database access with Entity Framework Code-First</span></span>

<span data-ttu-id="03722-115">Wir verwenden die Entity Framework (EF)-Unterstützung, die in ASP.NET MVC 3-Projekte, Abfragen und Aktualisieren der Datenbank enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="03722-115">We'll use the Entity Framework (EF) support that is included in ASP.NET MVC 3 projects to query and update the database.</span></span> <span data-ttu-id="03722-116">EF ist ein flexibles Objekt relationalen zuordnen (ORM) Daten-API, die Entwicklern, Abfragen und Aktualisieren von Daten in einer Datenbank auf objektorientierte Weise gespeichert ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="03722-116">EF is a flexible object relational mapping (ORM) data API that enables developers to query and update data stored in a database in an object-oriented way.</span></span>

<span data-ttu-id="03722-117">Entity Framework, Version 4 unterstützt ein Code First aufgerufen Entwicklung-Paradigma.</span><span class="sxs-lookup"><span data-stu-id="03722-117">Entity Framework version 4 supports a development paradigm called code-first.</span></span> <span data-ttu-id="03722-118">Code First können Sie Modellobjekt zu erstellen, indem Sie das Schreiben von einfachen Klassen (auch bekannt als POCO aus CLR-Objekte "Plain Old") und kann auch die Datenbank im Handumdrehen von Klassen erstellen.</span><span class="sxs-lookup"><span data-stu-id="03722-118">Code-first allows you to create model object by writing simple classes (also known as POCO from "plain-old" CLR objects), and can even create the database on the fly from your classes.</span></span>

### <a name="changes-to-our-model-classes"></a><span data-ttu-id="03722-119">Änderungen an unseren Modellklassen</span><span class="sxs-lookup"><span data-stu-id="03722-119">Changes to our Model Classes</span></span>

<span data-ttu-id="03722-120">Es wird die Erstellung Datenbankfunktion in Entity Framework in diesem Lernprogramm genutzt werden.</span><span class="sxs-lookup"><span data-stu-id="03722-120">We will be leveraging the database creation feature in Entity Framework in this tutorial.</span></span> <span data-ttu-id="03722-121">Bevor wir dies tun, aber wir einige geringfügige Änderungen daran vornehmen zu unserem Modellklassen hinzufügen in einige Dinge, die wir später verwenden.</span><span class="sxs-lookup"><span data-stu-id="03722-121">Before we do that, though, let's make a few minor changes to our model classes to add in some things we'll be using later on.</span></span>

#### <a name="adding-the-artist-model-classes"></a><span data-ttu-id="03722-122">Die Interpret-Modellklassen hinzufügen</span><span class="sxs-lookup"><span data-stu-id="03722-122">Adding the Artist Model Classes</span></span>

<span data-ttu-id="03722-123">Unsere Alben werden Künstler, zugeordnet werden, damit wir eine einfache Modellklasse, um ein Künstler beschreiben hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="03722-123">Our Albums will be associated with Artists, so we'll add a simple model class to describe an Artist.</span></span> <span data-ttu-id="03722-124">Fügen Sie eine neue Klasse mit dem Namen mit dem unten gezeigten Code Artist.cs Ordner Models.</span><span class="sxs-lookup"><span data-stu-id="03722-124">Add a new class to the Models folder named Artist.cs using the code shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample1.cs)]

#### <a name="updating-our-model-classes"></a><span data-ttu-id="03722-125">Unsere Modellklassen aktualisieren</span><span class="sxs-lookup"><span data-stu-id="03722-125">Updating our Model Classes</span></span>

<span data-ttu-id="03722-126">Aktualisieren Sie die Album-Klasse, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="03722-126">Update the Album class as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample2.cs)]

<span data-ttu-id="03722-127">Stellen Sie anschließend die folgenden Updates auf die Klasse "Genre" ein.</span><span class="sxs-lookup"><span data-stu-id="03722-127">Next, make the following updates to the Genre class.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample3.cs)]

### <a name="adding-the-appdata-folder"></a><span data-ttu-id="03722-128">Hinzufügen der App\_Datenordner</span><span class="sxs-lookup"><span data-stu-id="03722-128">Adding the App\_Data folder</span></span>

<span data-ttu-id="03722-129">Fügen wir eine App\_Datenverzeichnis unsere Projekt, um unsere SQL Server Express-Datenbankdateien zu halten.</span><span class="sxs-lookup"><span data-stu-id="03722-129">We'll add an App\_Data directory to our project to hold our SQL Server Express database files.</span></span> <span data-ttu-id="03722-130">App\_Daten ist ein besonderes Verzeichnis in ASP.NET bereits über die richtigen sicherheitszugriffsberechtigungen für den Datenbankzugriff.</span><span class="sxs-lookup"><span data-stu-id="03722-130">App\_Data is a special directory in ASP.NET which already has the correct security access permissions for database access.</span></span> <span data-ttu-id="03722-131">Wählen Sie im Menü Projekt ASP.NET-Ordner hinzufügen und dann die App\_Daten.</span><span class="sxs-lookup"><span data-stu-id="03722-131">From the Project menu, select Add ASP.NET Folder, then App\_Data.</span></span>

![](mvc-music-store-part-4/_static/image1.png)

### <a name="creating-a-connection-string-in-the-webconfig-file"></a><span data-ttu-id="03722-132">Erstellen einer Verbindungszeichenfolge in der Datei "Web.config"</span><span class="sxs-lookup"><span data-stu-id="03722-132">Creating a Connection String in the web.config file</span></span>

<span data-ttu-id="03722-133">Wir werden einige Zeilen Konfigurationsdatei der Website hinzufügen, damit Entity Framework Herstellen einer Verbindung mit der Datenbank bekannt ist.</span><span class="sxs-lookup"><span data-stu-id="03722-133">We will add a few lines to the website's configuration file so that Entity Framework knows how to connect to our database.</span></span> <span data-ttu-id="03722-134">Doppelklicken Sie auf die Datei "Web.config" im Stammverzeichnis des Projekts.</span><span class="sxs-lookup"><span data-stu-id="03722-134">Double-click on the Web.config file located in the root of the project.</span></span>

![](mvc-music-store-part-4/_static/image2.png)

<span data-ttu-id="03722-135">Führen Sie einen Bildlauf zum unteren Rand dieser Datei, und fügen eine &lt;ConnectionStrings&gt; direkt über die letzte Zeile im Abschnitt, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="03722-135">Scroll to the bottom of this file and add a &lt;connectionStrings&gt; section directly above the last line, as shown below.</span></span>

[!code-xml[Main](mvc-music-store-part-4/samples/sample4.xml)]

### <a name="adding-a-context-class"></a><span data-ttu-id="03722-136">Hinzufügen von Context-Klasse</span><span class="sxs-lookup"><span data-stu-id="03722-136">Adding a Context Class</span></span>

<span data-ttu-id="03722-137">Mit der rechten Maustaste in den Ordner Models, und fügen Sie eine neue Klasse namens MusicStoreEntities.cs hinzu.</span><span class="sxs-lookup"><span data-stu-id="03722-137">Right-click the Models folder and add a new class named MusicStoreEntities.cs.</span></span>

![](mvc-music-store-part-4/_static/image3.png)

<span data-ttu-id="03722-138">Diese Klasse wird den Datenbankkontext Entity Framework darstellen wird behandeln erstellen, lesen, aktualisieren und delete-Operationen für uns.</span><span class="sxs-lookup"><span data-stu-id="03722-138">This class will represent the Entity Framework database context, and will handle our create, read, update, and delete operations for us.</span></span> <span data-ttu-id="03722-139">Der Code für diese Klasse wird unten gezeigt.</span><span class="sxs-lookup"><span data-stu-id="03722-139">The code for this class is shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample5.cs)]

<span data-ttu-id="03722-140">Das war schon alles – es ist keine weitere Konfiguration, spezielle Schnittstellen usw. Durch die Erweiterung der Basisklasse ' DbContext ', ist unsere MusicStoreEntities-Klasse können unsere Datenbankvorgänge für uns zu behandeln.</span><span class="sxs-lookup"><span data-stu-id="03722-140">That's it - there's no other configuration, special interfaces, etc. By extending the DbContext base class, our MusicStoreEntities class is able to handle our database operations for us.</span></span> <span data-ttu-id="03722-141">Nun, da wir, die eingebunden haben, fügen Sie einigen weitere Eigenschaften zu unserem Modellklassen einige zusätzliche Informationen in die Datenbank nutzen.</span><span class="sxs-lookup"><span data-stu-id="03722-141">Now that we've got that hooked up, let's add a few more properties to our model classes to take advantage of some of the additional information in our database.</span></span>

### <a name="adding-our-store-catalog-data"></a><span data-ttu-id="03722-142">Unsere Katalogdaten Speicher hinzufügen</span><span class="sxs-lookup"><span data-stu-id="03722-142">Adding our store catalog data</span></span>

<span data-ttu-id="03722-143">Es wird eine Funktion in Entity Framework nutzen, "Seed" Daten in eine neu erstellte Datenbank hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="03722-143">We will take advantage of a feature in Entity Framework which adds "seed" data to a newly created database.</span></span> <span data-ttu-id="03722-144">Dadurch wird unsere Store-Katalog mit einer Liste von Genres Künstler und Alben vorab aufgefüllt.</span><span class="sxs-lookup"><span data-stu-id="03722-144">This will pre-populate our store catalog with a list of Genres, Artists, and Albums.</span></span> <span data-ttu-id="03722-145">MvcMusicStore Assets.zip Download des - unserer Website Design-Dateien, die weiter oben in diesem Lernprogramm verwendet enthalten – verfügt über eine Klassendatei mit dieser Ausgangswerte befindet sich in einem Ordner namens Code.</span><span class="sxs-lookup"><span data-stu-id="03722-145">The MvcMusicStore-Assets.zip download - which included our site design files used earlier in this tutorial - has a class file with this seed data, located in a folder named Code.</span></span>

<span data-ttu-id="03722-146">Innerhalb des Codes / Ordner Models, suchen Sie die Datei SampleData.cs, und legen Sie sie in der Ordner Models in unserem Projekt aus, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="03722-146">Within the Code / Models folder, locate the SampleData.cs file and drop it into the Models folder in our project, as shown below.</span></span>

![](mvc-music-store-part-4/_static/image4.png)

<span data-ttu-id="03722-147">Nun müssen wir eine Codezeile anzuweisen, Entity Framework zu SampleData Klasse hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="03722-147">Now we need to add one line of code to tell Entity Framework about that SampleData class.</span></span> <span data-ttu-id="03722-148">Doppelklicken Sie auf die Datei "Global.asax" im Stammverzeichnis des Projekts geöffnet, und fügen die folgende Zeile am Anfang die Anwendung\_Start-Methode.</span><span class="sxs-lookup"><span data-stu-id="03722-148">Double-click on the Global.asax file in the root of the project to open it and add the following line to the top the Application\_Start method.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample6.cs)]

<span data-ttu-id="03722-149">An diesem Punkt haben wir die erforderlichen Schritte zum Konfigurieren von Entity Framework für unsere Projekt abgeschlossen.</span><span class="sxs-lookup"><span data-stu-id="03722-149">At this point, we've completed the work necessary to configure Entity Framework for our project.</span></span>

## <a name="querying-the-database"></a><span data-ttu-id="03722-150">Abfragen der Datenbank</span><span class="sxs-lookup"><span data-stu-id="03722-150">Querying the Database</span></span>

<span data-ttu-id="03722-151">Jetzt aktualisieren wir unsere StoreController, sodass anstelle von "dummy Daten" stattdessen in die Datenbank alle seine Informationen abgefragt wird.</span><span class="sxs-lookup"><span data-stu-id="03722-151">Now let's update our StoreController so that instead of using "dummy data" it instead calls into our database to query all of its information.</span></span> <span data-ttu-id="03722-152">Wir beginnen mit deklarieren ein Feld für die **StoreController** um eine Instanz der MusicStoreEntities-Klasse, die mit dem Namen StoreDB aufzunehmen:</span><span class="sxs-lookup"><span data-stu-id="03722-152">We'll start by declaring a field on the **StoreController** to hold an instance of the MusicStoreEntities class, named storeDB:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample7.cs)]

### <a name="updating-the-store-index-to-query-the-database"></a><span data-ttu-id="03722-153">Aktualisieren den Columnstore-Index zum Abfragen der Datenbank</span><span class="sxs-lookup"><span data-stu-id="03722-153">Updating the Store Index to query the database</span></span>

<span data-ttu-id="03722-154">Die MusicStoreEntities-Klasse wird vom Entity Framework verwaltet und stellt die Auflistungseigenschaft für jede Tabelle in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="03722-154">The MusicStoreEntities class is maintained by the Entity Framework and exposes a collection property for each table in our database.</span></span> <span data-ttu-id="03722-155">Wir aktualisieren unsere StoreController Index-Aktion, um alle Genres in der Datenbank abrufen.</span><span class="sxs-lookup"><span data-stu-id="03722-155">Let's update our StoreController's Index action to retrieve all Genres in our database.</span></span> <span data-ttu-id="03722-156">Zuvor haben wir dies durch eine feste Programmierung Zeichenfolgendaten.</span><span class="sxs-lookup"><span data-stu-id="03722-156">Previously we did this by hard-coding string data.</span></span> <span data-ttu-id="03722-157">Nun können wir die Entity Framework-Datenkontext Generes Auflistung stattdessen nur verwenden:</span><span class="sxs-lookup"><span data-stu-id="03722-157">Now we can instead just use the Entity Framework context Generes collection:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample8.cs)]

<span data-ttu-id="03722-158">Keine Änderungen müssen an unsere Ansichtenvorlage durchgeführt werden soll, da wir noch die gleiche StoreIndexViewModel zurückgeben, wird zurückgegeben, bevor - wir gerade Livedaten aus der Datenbank jetzt zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="03722-158">No changes need to happen to our View template since we're still returning the same StoreIndexViewModel we returned before - we're just returning live data from our database now.</span></span>

<span data-ttu-id="03722-159">Wenn wir führen das Projekt erneut aus, und besuchen Sie "/ Speichern", sehen wir nun eine Liste von Genres alle in der Datenbank:</span><span class="sxs-lookup"><span data-stu-id="03722-159">When we run the project again and visit the "/Store" URL, we'll now see a list of all Genres in our database:</span></span>

![](mvc-music-store-part-4/_static/image1.jpg)

### <a name="updating-store-browse-and-details-to-use-live-data"></a><span data-ttu-id="03722-160">Aktualisieren von Speicher zu durchsuchen und Details, für die Verwendung von Livedaten</span><span class="sxs-lookup"><span data-stu-id="03722-160">Updating Store Browse and Details to use live data</span></span>

<span data-ttu-id="03722-161">Mit der/Store/durchsuchen? "Genre" =*[einige Genre]* Aktionsmethode, wir haben eine "Genre" nach Namen suchen.</span><span class="sxs-lookup"><span data-stu-id="03722-161">With the /Store/Browse?genre=*[some-genre]* action method, we're searching for a Genre by name.</span></span> <span data-ttu-id="03722-162">Wird nur ein Ergebnis erwartet, da wir jemals zwei Einträge für denselben Namen "Genre" verfügen, sollten nicht, weshalb wir verwenden können, die. Single() Erweiterung in LINQ-Abfrage für das entsprechende Objekt für "Genre" wie folgt (nicht vom Typ dies noch):</span><span class="sxs-lookup"><span data-stu-id="03722-162">We only expect one result, since we shouldn't ever have two entries for the same Genre name, and so we can use the .Single() extension in LINQ to query for the appropriate Genre object like this (don't type this yet):</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample9.cs)]

<span data-ttu-id="03722-163">Die einzige Methode nimmt einen Lambda-Ausdruck als Parameter, der angibt, dass ein einzelnes "Genre"-Objekt, dessen Name mit dem Wert übereinstimmt, den es definiert haben soll.</span><span class="sxs-lookup"><span data-stu-id="03722-163">The Single method takes a Lambda expression as a parameter, which specifies that we want a single Genre object such that its name matches the value we've defined.</span></span> <span data-ttu-id="03722-164">Im obigen Fall werden wir ein einzelnes Objekt für "Genre" mit einem Namenswert, der Abgleich Disco geladen.</span><span class="sxs-lookup"><span data-stu-id="03722-164">In the case above, we are loading a single Genre object with a Name value matching Disco.</span></span>

<span data-ttu-id="03722-165">Es wird eine Entity Framework-Funktion nutzen, die uns ermöglicht, die andere verknüpften Entitäten angezeigt werden, die ebenfalls geladen werden soll, wenn das Objekt "Genre" abgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="03722-165">We'll take advantage of an Entity Framework feature that allows us to indicate other related entities we want loaded as well when the Genre object is retrieved.</span></span> <span data-ttu-id="03722-166">Dieses Feature heißt Abfrage Ergebnis strukturiert werden, und ermöglicht es, zu die Anzahl der zu verringern, müssen wir Zugriff auf die Datenbank aus, um alle Informationen abzurufen, die Sie benötigen.</span><span class="sxs-lookup"><span data-stu-id="03722-166">This feature is called Query Result Shaping, and enables us to reduce the number of times we need to access the database to retrieve all of the information we need.</span></span> <span data-ttu-id="03722-167">Wir möchten die Alben für "Genre" Wir abzurufen, vorab abzurufen, damit wir unsere Abfrage von Genres.Include("Albums"), um anzugeben, dass wir auch verwandte Alben möchten aktualisiert werden.</span><span class="sxs-lookup"><span data-stu-id="03722-167">We want to pre-fetch the Albums for Genre we retrieve, so we'll update our query to include from Genres.Include("Albums") to indicate that we want related albums as well.</span></span> <span data-ttu-id="03722-168">Dies ist effizienter, da sie unsere "Genre" und die Album Daten in einer einzelnen Datenbank-Anforderung abgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="03722-168">This is more efficient, since it will retrieve both our Genre and Album data in a single database request.</span></span>

<span data-ttu-id="03722-169">Mit den Erklärungen aus dem Weg sieht wie unsere aktualisierte durchsuchen Controlleraktion aussieht:</span><span class="sxs-lookup"><span data-stu-id="03722-169">With the explanations out of the way, here's how our updated Browse controller action looks:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample10.cs)]

<span data-ttu-id="03722-170">Wir haben jetzt aktualisieren der Store durchsuchen-Ansicht zum Anzeigen von Alben in jeder "Genre" zur Verfügung stehen.</span><span class="sxs-lookup"><span data-stu-id="03722-170">We can now update the Store Browse View to display the albums which are available in each Genre.</span></span> <span data-ttu-id="03722-171">Öffnen Sie die Vorlage anzeigen (im /Views/Store/Browse.cshtml), und fügen Sie eine Aufzählung von Alben hinzu, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="03722-171">Open the view template (found in /Views/Store/Browse.cshtml) and add a bulleted list of Albums as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample11.cshtml)]

<span data-ttu-id="03722-172">Unsere Anwendung ausführen und das Navigieren zu/Store/durchsuchen? "Genre" = Jazz an, dass der aus der Datenbank, Anzeigen von alle Alben in unserer ausgewählten "Genre" jetzt unsere Ergebnisse abgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="03722-172">Running our application and browsing to /Store/Browse?genre=Jazz shows that our results are now being pulled from the database, displaying all albums in our selected Genre.</span></span>

![](mvc-music-store-part-4/_static/image2.jpg)

<span data-ttu-id="03722-173">Wir stellen identisch zu unserem/Store/Informationen / [Id] URL ändern, und Ersetzen Sie unsere Platzhalterdaten mit einer Datenbankabfrage, die ein Album lädt, deren ID der Wert des Parameters übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="03722-173">We'll make the same change to our /Store/Details/[id] URL, and replace our dummy data with a database query which loads an Album whose ID matches the parameter value.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample12.cs)]

<span data-ttu-id="03722-174">Unsere Anwendung ausführen und das Navigieren zu /Store/Details/1 zeigt, dass unsere Ergebnisse jetzt aus der Datenbank abgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="03722-174">Running our application and browsing to /Store/Details/1 shows that our results are now being pulled from the database.</span></span>

![](mvc-music-store-part-4/_static/image5.png)

<span data-ttu-id="03722-175">Nun, dass unsere Store Detailseite so eingerichtet ist, ein Album mit der ID Album anzeigen, aktualisieren wir die **Durchsuchen** Sicht um mit der Ansicht zu verknüpfen.</span><span class="sxs-lookup"><span data-stu-id="03722-175">Now that our Store Details page is set up to display an album by the Album ID, let's update the **Browse** view to link to the Details view.</span></span> <span data-ttu-id="03722-176">Wir verwenden Html.ActionLink, genau so wie wir Verknüpfung im Columnstore-Index Store durchsuchen am Ende des vorherigen Abschnitts.</span><span class="sxs-lookup"><span data-stu-id="03722-176">We will use Html.ActionLink, exactly as we did to link from Store Index to Store Browse at the end of the previous section.</span></span> <span data-ttu-id="03722-177">Die vollständige Quelle für die Ansicht durchsuchen wird unten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="03722-177">The complete source for the Browse view appears below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample13.cshtml)]

<span data-ttu-id="03722-178">Jetzt können wir aus unserer Seite "Store" zu einer Seite "Genre" suchen, die die verfügbaren Alben aufgelistet sind, und durch Klicken auf ein Album können wir Details für diese Album anzeigen.</span><span class="sxs-lookup"><span data-stu-id="03722-178">We're now able to browse from our Store page to a Genre page, which lists the available albums, and by clicking on an album we can view details for that album.</span></span>

![](mvc-music-store-part-4/_static/image6.png)

> [!div class="step-by-step"]
> <span data-ttu-id="03722-179">[Zurück](mvc-music-store-part-3.md)
> [Weiter](mvc-music-store-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="03722-179">[Previous](mvc-music-store-part-3.md)
[Next](mvc-music-store-part-5.md)</span></span>
