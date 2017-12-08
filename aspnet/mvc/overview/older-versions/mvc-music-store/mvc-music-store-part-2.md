---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
title: 'Teil 2: Controller | Microsoft Docs'
author: jongalloway
description: Diese Reihe von Lernprogrammen details aller die Schritte zum Erstellen von ASP.NET MVC-Music Store-beispielanwendung. Teil 2 werden Controller behandelt.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 998ce4e1-9d72-435b-8f1c-399a10ae4360
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
msc.type: authoredcontent
ms.openlocfilehash: bdafd751e996e759d516d0fa25b09eff21241ed7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="part-2-controllers"></a><span data-ttu-id="88af6-104">Teil 2: Controller</span><span class="sxs-lookup"><span data-stu-id="88af6-104">Part 2: Controllers</span></span>
====================
<span data-ttu-id="88af6-105">durch [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="88af6-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="88af6-106">Das MVC-Music Store ist eine lernprogrammanwendung, die führt und erklärt schrittweise, wie mithilfe von ASP.NET MVC und Visual Studio für die Webentwicklung.</span><span class="sxs-lookup"><span data-stu-id="88af6-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="88af6-107">Das MVC-Music Store handelt es sich um einfache Beispiel Store Implementierung, die online Musikalben verkauft und implementiert grundlegende standortverwaltung Benutzer anmelden und shopping Cart-Funktionalität.</span><span class="sxs-lookup"><span data-stu-id="88af6-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="88af6-108">Diese Reihe von Lernprogrammen details aller die Schritte zum Erstellen von ASP.NET MVC-Music Store-beispielanwendung.</span><span class="sxs-lookup"><span data-stu-id="88af6-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="88af6-109">Teil 2 werden Controller behandelt.</span><span class="sxs-lookup"><span data-stu-id="88af6-109">Part 2 covers Controllers.</span></span>


<span data-ttu-id="88af6-110">Mit herkömmlichen webframeworks werden eingehende URLs in der Regel Dateien auf dem Datenträger zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="88af6-110">With traditional web frameworks, incoming URLs are typically mapped to files on disk.</span></span> <span data-ttu-id="88af6-111">Zum Beispiel: eine Anforderung für eine URL wie "/ Products.aspx" oder "/ Products.php" möglicherweise durch eine Datei "Products.aspx" oder "Products.php" verarbeitet werden.</span><span class="sxs-lookup"><span data-stu-id="88af6-111">For example: a request for a URL like "/Products.aspx" or "/Products.php" might be processed by a "Products.aspx" or "Products.php" file.</span></span>

<span data-ttu-id="88af6-112">MVC-Frameworks webbasierte ordnen URLs Servercode etwas anders.</span><span class="sxs-lookup"><span data-stu-id="88af6-112">Web-based MVC frameworks map URLs to server code in a slightly different way.</span></span> <span data-ttu-id="88af6-113">Statt eingehende URLs Dateien, ordnen sie stattdessen URLs an Methoden für Klassen.</span><span class="sxs-lookup"><span data-stu-id="88af6-113">Instead of mapping incoming URLs to files, they instead map URLs to methods on classes.</span></span> <span data-ttu-id="88af6-114">Diese Klassen werden als "Controller" bezeichnet und sie sind verantwortlich für die Verarbeitung von eingehenden HTTP-Anforderungen, Behandeln von Benutzereingaben, abrufen und Speichern von Daten und bestimmen die zu sendende Antwort zurück an den Client (HTML-Seite anzeigen, eine Datei herunterladen, zu einer anderen umleiten URL, usw.).</span><span class="sxs-lookup"><span data-stu-id="88af6-114">These classes are called "Controllers" and they are responsible for processing incoming HTTP requests, handling user input, retrieving and saving data, and determining the response to send back to the client (display HTML, download a file, redirect to a different URL, etc.).</span></span>

## <a name="adding-a-homecontroller"></a><span data-ttu-id="88af6-115">Hinzufügen einer HomeController</span><span class="sxs-lookup"><span data-stu-id="88af6-115">Adding a HomeController</span></span>

<span data-ttu-id="88af6-116">Wir beginnen unsere MVC Music Store-Anwendung durch Hinzufügen einer Controllerklasse, die URLs auf der Startseite unserer Website behandelt.</span><span class="sxs-lookup"><span data-stu-id="88af6-116">We'll begin our MVC Music Store application by adding a Controller class that will handle URLs to the Home page of our site.</span></span> <span data-ttu-id="88af6-117">Wir führen Sie die Standard-Benennungskonventionen von ASP.NET MVC und ihn HomeController aufrufen.</span><span class="sxs-lookup"><span data-stu-id="88af6-117">We'll follow the default naming conventions of ASP.NET MVC and call it HomeController.</span></span>

<span data-ttu-id="88af6-118">Mit der rechten Maustaste im Projektmappen-Explorer des Ordners "Controller" und wählen Sie "Hinzufügen" und dann auf "Controller..."</span><span class="sxs-lookup"><span data-stu-id="88af6-118">Right-click the "Controllers" folder within the Solution Explorer and select "Add", and then the "Controller…"</span></span> <span data-ttu-id="88af6-119">Befehl:</span><span class="sxs-lookup"><span data-stu-id="88af6-119">command:</span></span>

![](mvc-music-store-part-2/_static/image1.jpg)

<span data-ttu-id="88af6-120">Hierdurch wird das Dialogfeld "Controller hinzufügen" angezeigt.</span><span class="sxs-lookup"><span data-stu-id="88af6-120">This will bring up the "Add Controller" dialog.</span></span> <span data-ttu-id="88af6-121">Nennen Sie den Controller "HomeController.cs", und drücken Sie die Schaltfläche "hinzufügen".</span><span class="sxs-lookup"><span data-stu-id="88af6-121">Name the controller "HomeController" and press the Add button.</span></span>

![](mvc-music-store-part-2/_static/image1.png)

<span data-ttu-id="88af6-122">Dies erstellt eine neue Datei HomeController.cs, durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="88af6-122">This will create a new file, HomeController.cs, with the following code:</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample1.cs)]

<span data-ttu-id="88af6-123">Um so weit wie möglich zu starten, ersetzen Sie wir die Index-Methode, mit der eine einfache Methode, die nur eine Zeichenfolge zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="88af6-123">To start as simply as possible, let's replace the Index method with a simple method that just returns a string.</span></span> <span data-ttu-id="88af6-124">Wir stellen zwei Änderungen:</span><span class="sxs-lookup"><span data-stu-id="88af6-124">We'll make two changes:</span></span>

- <span data-ttu-id="88af6-125">Ändern Sie die Methode zum Zurückgeben einer Zeichenfolge statt ein Aktionsergebnis dar</span><span class="sxs-lookup"><span data-stu-id="88af6-125">Change the method to return a string instead of an ActionResult</span></span>
- <span data-ttu-id="88af6-126">Ändern Sie die return-Anweisung um "Hello aus" zurückzukehren.</span><span class="sxs-lookup"><span data-stu-id="88af6-126">Change the return statement to return "Hello from Home"</span></span>

<span data-ttu-id="88af6-127">Die Methode sollte jetzt wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="88af6-127">The method should now look like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample2.cs)]

## <a name="running-the-application"></a><span data-ttu-id="88af6-128">Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="88af6-128">Running the Application</span></span>

<span data-ttu-id="88af6-129">Jetzt führen Sie zunächst den Standort.</span><span class="sxs-lookup"><span data-stu-id="88af6-129">Now let's run the site.</span></span> <span data-ttu-id="88af6-130">Wir können unsere Webserver starten und probieren Sie die Website mit einer der folgenden::</span><span class="sxs-lookup"><span data-stu-id="88af6-130">We can start our web-server and try out the site using any of the following::</span></span>

- <span data-ttu-id="88af6-131">Wählen Sie das Debuggen starten Menüelement für Debug ⇨</span><span class="sxs-lookup"><span data-stu-id="88af6-131">Choose the Debug ⇨ Start Debugging menu item</span></span>
- <span data-ttu-id="88af6-132">Klicken Sie auf der grünen Pfeilschaltfläche auf der Symbolleiste![](mvc-music-store-part-2/_static/image2.jpg)</span><span class="sxs-lookup"><span data-stu-id="88af6-132">Click the Green arrow button in the toolbar ![](mvc-music-store-part-2/_static/image2.jpg)</span></span>
- <span data-ttu-id="88af6-133">Verwenden Sie die Tastenkombination F5.</span><span class="sxs-lookup"><span data-stu-id="88af6-133">Use the keyboard shortcut, F5.</span></span>

<span data-ttu-id="88af6-134">Mit einer der oben genannten Schritte werden unsere Projekt kompilieren und dann führen den ASP.NET Development Server, die erstellt in Visual Web Developer gestartet wird.</span><span class="sxs-lookup"><span data-stu-id="88af6-134">Using any of the above steps will compile our project, and then cause the ASP.NET Development Server that is built-into Visual Web Developer to start.</span></span> <span data-ttu-id="88af6-135">Eine Benachrichtigung erscheint in der unteren Ecke des Bildschirms, um anzugeben, dass ASP.NET Development Server gestartet wurde, und zeigt der Portnummer an, dass er unter ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="88af6-135">A notification will appear in the bottom corner of the screen to indicate that the ASP.NET Development Server has started up, and will show the port number that it is running under.</span></span>

![](mvc-music-store-part-2/_static/image2.png)

<span data-ttu-id="88af6-136">Visual Web Developer wird dann automatisch geöffnet ein Browserfenster mit unserem Webserver, dessen URL verweist.</span><span class="sxs-lookup"><span data-stu-id="88af6-136">Visual Web Developer will then automatically open a browser window whose URL points to our web-server.</span></span> <span data-ttu-id="88af6-137">Dadurch können wir schnell die Webanwendung zu testen:</span><span class="sxs-lookup"><span data-stu-id="88af6-137">This will allow us to quickly try out our web application:</span></span>

![](mvc-music-store-part-2/_static/image3.png)

<span data-ttu-id="88af6-138">Okay, das war ziemlich schnell – wir eine neue Website erstellt, hinzugefügt, eine drei-Zeile-Funktion, und wir haben Text in einem Browser.</span><span class="sxs-lookup"><span data-stu-id="88af6-138">Okay, that was pretty quick – we created a new website, added a three line function, and we've got text in a browser.</span></span> <span data-ttu-id="88af6-139">Nicht rocket Science allerdings handelt es sich um einen Einstieg.</span><span class="sxs-lookup"><span data-stu-id="88af6-139">Not rocket science, but it's a start.</span></span>

<span data-ttu-id="88af6-140">*Hinweis: Visual Web Developer enthält den ASP.NET Development Server, die Ihre Website für eine Anzahl von zufälligen freien "Port" ausgeführt wird. In der oben dargestellten Screenshot die Website ausgeführt wird, am `http://localhost:26641/`, sodass Port 26641 verwendet wird. Die Portnummer wird unterschiedlich sein. Wenn wir in diesem Lernprogramm URLs like /Store/Browse erörtern, geht, die nach der Portnummer. Wenn 26641 die Portnummer an, Navigieren zum/Store/durchsuchen bedeutet Navigieren zum `http://localhost:26641/Store/Browse`.*</span><span class="sxs-lookup"><span data-stu-id="88af6-140">*Note: Visual Web Developer includes the ASP.NET Development Server, which will run your website on a random free "port" number. In the screenshot above, the site is running at `http://localhost:26641/`, so it's using port 26641. Your port number will be different. When we talk about URL's like /Store/Browse in this tutorial, that will go after the port number. Assuming a port number of 26641, browsing to /Store/Browse will mean browsing to `http://localhost:26641/Store/Browse`.*</span></span>

## <a name="adding-a-storecontroller"></a><span data-ttu-id="88af6-141">Hinzufügen einer StoreController</span><span class="sxs-lookup"><span data-stu-id="88af6-141">Adding a StoreController</span></span>

<span data-ttu-id="88af6-142">Eine einfache HomeController, die die Homepage von unserer Website implementiert wurde hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="88af6-142">We added a simple HomeController that implements the Home Page of our site.</span></span> <span data-ttu-id="88af6-143">Nun fügen wir einen anderen Controller, mit dem wir die Suche nach Funktionalität unserer-Music Store implementieren.</span><span class="sxs-lookup"><span data-stu-id="88af6-143">Let's now add another controller that we'll use to implement the browsing functionality of our music store.</span></span> <span data-ttu-id="88af6-144">Unser Store-Controller unterstützt drei Szenarien:</span><span class="sxs-lookup"><span data-stu-id="88af6-144">Our store controller will support three scenarios:</span></span>

- <span data-ttu-id="88af6-145">Eine Auflistung-Seite mit der Musik Genres in unserer-Music store</span><span class="sxs-lookup"><span data-stu-id="88af6-145">A listing page of the music genres in our music store</span></span>
- <span data-ttu-id="88af6-146">Seite "Durchsuchen", die alle von der Musikalben in einer bestimmten "Genre" aufgelistet sind</span><span class="sxs-lookup"><span data-stu-id="88af6-146">A browse page that lists all of the music albums in a particular genre</span></span>
- <span data-ttu-id="88af6-147">Seite "Details", das Informationen zu einem bestimmten Musikalbum anzeigt</span><span class="sxs-lookup"><span data-stu-id="88af6-147">A details page that shows information about a specific music album</span></span>

<span data-ttu-id="88af6-148">Wir beginnen, indem Sie eine neue StoreController-Klasse hinzufügen...</span><span class="sxs-lookup"><span data-stu-id="88af6-148">We'll start by adding a new StoreController class..</span></span> <span data-ttu-id="88af6-149">Wenn Sie nicht bereits geschehen, beenden Sie die Ausführung der Anwendung durch Schließen des Browsers oder das Beenden des Debuggens Menüelement für Debug ⇨ auswählen.</span><span class="sxs-lookup"><span data-stu-id="88af6-149">If you haven't already, stop running the application either by closing the browser or selecting the Debug ⇨ Stop Debugging menu item.</span></span>

<span data-ttu-id="88af6-150">Fügen Sie jetzt eine neue StoreController hinzu.</span><span class="sxs-lookup"><span data-stu-id="88af6-150">Now add a new StoreController.</span></span> <span data-ttu-id="88af6-151">Wie wir mit HomeController haben, müssen wir dies ausführen, indem Sie mit der rechten Maustaste auf den Ordner "Controller" im Projektmappen-Explorer und Add -&gt;Controller Menüelement</span><span class="sxs-lookup"><span data-stu-id="88af6-151">Just like we did with HomeController, we'll do this by right-clicking on the "Controllers" folder within the Solution Explorer and choosing the Add-&gt;Controller menu item</span></span>

![](mvc-music-store-part-2/_static/image4.png)

<span data-ttu-id="88af6-152">Unsere neue StoreController verfügt bereits über eine "Index"-Methode.</span><span class="sxs-lookup"><span data-stu-id="88af6-152">Our new StoreController already has an "Index" method.</span></span> <span data-ttu-id="88af6-153">Wir verwenden diese Methode "Index" unsere Angebotsseite implementieren, die alle Genres in unserer-Music Store aufgelistet.</span><span class="sxs-lookup"><span data-stu-id="88af6-153">We'll use this "Index" method to implement our listing page that lists all genres in our music store.</span></span> <span data-ttu-id="88af6-154">Fügen wir auch zwei weitere Methoden, um die beiden anderen Szenarien implementieren wir unsere StoreController behandeln soll: Durchsuchen und Details.</span><span class="sxs-lookup"><span data-stu-id="88af6-154">We'll also add two additional methods to implement the two other scenarios we want our StoreController to handle: Browse and Details.</span></span>

<span data-ttu-id="88af6-155">Diese Methoden ("Index", "Durchsuchen" und "Details") in unserer Controller "Controlleraktionen" aufgerufen werden, und wie Sie bereits mit der HomeController.Index ()-Aktionsmethode gesehen haben, ist ihre Arbeit auf URL-Anforderungen zu reagieren und welche Inhalte (im Allgemeinen) ermitteln sollte gesendet werden, zurück an den Browser oder Benutzer, der die URL aufgerufen hat.</span><span class="sxs-lookup"><span data-stu-id="88af6-155">These methods (Index, Browse and Details) within our Controller are called "Controller Actions", and as you've already seen with the HomeController.Index()action method, their job is to respond to URL requests and (generally speaking) determine what content should be sent back to the browser or user that invoked the URL.</span></span>

<span data-ttu-id="88af6-156">Beginnen wir unsere StoreController Implementierung durch Ändern der theIndex()-Methode, um die Zeichenfolge "Hello aus Store.Index()" zurück, und wir werden ähnliche Methoden für Browse() und Details() hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="88af6-156">We'll start our StoreController implementation by changing theIndex() method to return the string "Hello from Store.Index()" and we'll add similar methods for Browse() and Details():</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample3.cs)]

<span data-ttu-id="88af6-157">Führen Sie das Projekt erneut aus, und Durchsuchen Sie die folgenden URLs:</span><span class="sxs-lookup"><span data-stu-id="88af6-157">Run the project again and browse the following URLs:</span></span>

- <span data-ttu-id="88af6-158">/ Store</span><span class="sxs-lookup"><span data-stu-id="88af6-158">/Store</span></span>
- <span data-ttu-id="88af6-159">/ Store/durchsuchen</span><span class="sxs-lookup"><span data-stu-id="88af6-159">/Store/Browse</span></span>
- <span data-ttu-id="88af6-160">/ Store/Details</span><span class="sxs-lookup"><span data-stu-id="88af6-160">/Store/Details</span></span>

<span data-ttu-id="88af6-161">Zugriff auf diese URLs wird Aufrufen der Aktionsmethoden in unserer Controller und Antworten Zeichenfolge zurückzugeben:</span><span class="sxs-lookup"><span data-stu-id="88af6-161">Accessing these URLs will invoke the action methods within our Controller and return string responses:</span></span>

![](mvc-music-store-part-2/_static/image5.png)

<span data-ttu-id="88af6-162">Das ist hervorragend, aber dies nur Konstanten Zeichenfolgen sind.</span><span class="sxs-lookup"><span data-stu-id="88af6-162">That's great, but these are just constant strings.</span></span> <span data-ttu-id="88af6-163">Stellen sie dynamisch, damit sie Informationen aus der URL und sie auf der Seite ausgegeben zeigt.</span><span class="sxs-lookup"><span data-stu-id="88af6-163">Let's make them dynamic, so they take information from the URL and display it in the page output.</span></span>

<span data-ttu-id="88af6-164">Wechseln Sie zunächst die Aktionsmethode durchsuchen, um eine Querystring-Wert aus der URL abzurufen.</span><span class="sxs-lookup"><span data-stu-id="88af6-164">First we'll change the Browse action method to retrieve a querystring value from the URL.</span></span> <span data-ttu-id="88af6-165">Dazu können wir unsere Aktionsmethode einen Parameter "Genre" hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="88af6-165">We can do this by adding a "genre" parameter to our action method.</span></span> <span data-ttu-id="88af6-166">Wenn wir dies tun übergibt ASP.NET-MVC automatisch alle Abfragezeichenfolge und Formularbereitstellungsparameter, mit dem Namen "Genre" in unseren Aktionsmethode, wenn er aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="88af6-166">When we do this ASP.NET MVC will automatically pass any querystring or form post parameters named "genre" to our action method when it is invoked.</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample4.cs)]

<span data-ttu-id="88af6-167">*Hinweis: Wir haben HttpUtility.HtmlEncode durchführen Utility-Methode verwenden, um die Benutzereingabe zu bereinigen. Dadurch wird verhindert, dass Benutzer Räumen Javascript in unserer Sicht mit einem Link wie /Store/Browse? "Genre" =&lt;Skript&gt;window.location= "http://hackersite.com"&lt;/script&gt;.*</span><span class="sxs-lookup"><span data-stu-id="88af6-167">*Note: We're using the HttpUtility.HtmlEncode utility method to sanitize the user input. This prevents users from injecting Javascript into our View with a link like /Store/Browse?Genre=&lt;script&gt;window.location='http://hackersite.com'&lt;/script&gt;.*</span></span>

<span data-ttu-id="88af6-168">Jetzt sehen wir navigieren Sie zu/Store/durchsuchen? "Genre" Disco =</span><span class="sxs-lookup"><span data-stu-id="88af6-168">Now let's browse to /Store/Browse?Genre=Disco</span></span>

![](mvc-music-store-part-2/_static/image6.png)

<span data-ttu-id="88af6-169">Nehmen wir als Nächstes die Aktion "Details" zum Lesen und Anzeigen der Eingabeparameter mit dem Namen ID ändern</span><span class="sxs-lookup"><span data-stu-id="88af6-169">Let's next change the Details action to read and display an input parameter named ID.</span></span> <span data-ttu-id="88af6-170">Im Gegensatz zu unserer vorherigen Methode wird nicht wir den ID-Wert als eine Querystring-Parameter einbetten.</span><span class="sxs-lookup"><span data-stu-id="88af6-170">Unlike our previous method, we won't be embedding the ID value as a querystring parameter.</span></span> <span data-ttu-id="88af6-171">Wir müssen es stattdessen direkt in der URL selbst einbetten.</span><span class="sxs-lookup"><span data-stu-id="88af6-171">Instead we'll embed it directly within the URL itself.</span></span> <span data-ttu-id="88af6-172">Zum Beispiel: /Store/Details/5.</span><span class="sxs-lookup"><span data-stu-id="88af6-172">For example: /Store/Details/5.</span></span>

<span data-ttu-id="88af6-173">ASP.NET MVC können uns problemlos erreichen, ohne etwas konfigurieren zu müssen.</span><span class="sxs-lookup"><span data-stu-id="88af6-173">ASP.NET MVC lets us easily do this without having to configure anything.</span></span> <span data-ttu-id="88af6-174">Routing ASP.NETs Standardkonvention ist, das eine URL-Segment nach dem Methodennamen Aktion als einen Parameter namens "ID" zu behandeln.</span><span class="sxs-lookup"><span data-stu-id="88af6-174">ASP.NET MVC's default routing convention is to treat the segment of a URL after the action method name as a parameter named "ID".</span></span> <span data-ttu-id="88af6-175">Wenn die Aktionsmethode einen Parameter namens ID hat wird ASP.NET MVC automatisch das URL-Segment für Sie als Parameter übergeben.</span><span class="sxs-lookup"><span data-stu-id="88af6-175">If your action method has a parameter named ID then ASP.NET MVC will automatically pass the URL segment to you as a parameter.</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample5.cs)]

<span data-ttu-id="88af6-176">Führen Sie die Anwendung, und navigieren Sie zu /Store/Details/5:</span><span class="sxs-lookup"><span data-stu-id="88af6-176">Run the application and browse to /Store/Details/5:</span></span>

![](mvc-music-store-part-2/_static/image7.png)

<span data-ttu-id="88af6-177">Wir kurz zusammengefasst, bisher Fertigstellung:</span><span class="sxs-lookup"><span data-stu-id="88af6-177">Let's recap what we've done so far:</span></span>

- <span data-ttu-id="88af6-178">Wir haben ein neues ASP.NET MVC-Projekt in Visual Web Developer erstellt.</span><span class="sxs-lookup"><span data-stu-id="88af6-178">We've created a new ASP.NET MVC project in Visual Web Developer</span></span>
- <span data-ttu-id="88af6-179">Wir haben die grundlegenden Ordnerstruktur einer ASP.NET MVC-Anwendung erläutert.</span><span class="sxs-lookup"><span data-stu-id="88af6-179">We've discussed the basic folder structure of an ASP.NET MVC application</span></span>
- <span data-ttu-id="88af6-180">Wir haben gelernt, wie Sie unsere Website mithilfe von ASP.NET Development Server ausführen</span><span class="sxs-lookup"><span data-stu-id="88af6-180">We've learned how to run our website using the ASP.NET Development Server</span></span>
- <span data-ttu-id="88af6-181">Wir haben zwei Controllerklassen erstellt: ein HomeController und eine StoreController</span><span class="sxs-lookup"><span data-stu-id="88af6-181">We've created two Controller classes: a HomeController and a StoreController</span></span>
- <span data-ttu-id="88af6-182">Wir haben Aktionsmethoden unserer Domänencontroller hinzugefügt, das auf URL-Anforderungen reagieren und Text an den Browser zurück.</span><span class="sxs-lookup"><span data-stu-id="88af6-182">We've added Action Methods to our controllers which respond to URL requests and return text to the browser</span></span>


>[!div class="step-by-step"]
<span data-ttu-id="88af6-183">[Zurück](mvc-music-store-part-1.md)
[Weiter](mvc-music-store-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="88af6-183">[Previous](mvc-music-store-part-1.md)
[Next](mvc-music-store-part-3.md)</span></span>
