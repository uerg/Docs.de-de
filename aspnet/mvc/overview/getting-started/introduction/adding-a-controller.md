---
uid: mvc/overview/getting-started/introduction/adding-a-controller
title: Hinzufügen eines Controllers | Microsoft-Dokumentation
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: cc764f3b-6921-486a-8f44-c6ccd1249acd
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: d5cc9db7b1eec139a37afb6427fd761342fcc1f5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827369"
---
<a name="adding-a-controller"></a><span data-ttu-id="d23d3-102">Hinzufügen eines Controllers</span><span class="sxs-lookup"><span data-stu-id="d23d3-102">Adding a Controller</span></span>
====================
<span data-ttu-id="d23d3-103">durch [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="d23d3-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

<span data-ttu-id="d23d3-104">MVC steht für *Model-View-Controller*.</span><span class="sxs-lookup"><span data-stu-id="d23d3-104">MVC stands for *model-view-controller*.</span></span> <span data-ttu-id="d23d3-105">MVC ist ein Muster für die Entwicklung von Anwendungen, die gut strukturiert, getestet und leicht zu warten sind.</span><span class="sxs-lookup"><span data-stu-id="d23d3-105">MVC is a pattern for developing applications that are well architected, testable and easy to maintain.</span></span> <span data-ttu-id="d23d3-106">MVC-basierten Anwendungen enthalten:</span><span class="sxs-lookup"><span data-stu-id="d23d3-106">MVC-based applications contain:</span></span>

- <span data-ttu-id="d23d3-107">**M** odelle: Klassen, die die Daten der Anwendung darstellen, und, die Validierungslogik zum Erzwingen von Geschäftsregeln für diese Daten verwenden.</span><span class="sxs-lookup"><span data-stu-id="d23d3-107">**M** odels: Classes that represent the data of the application and that use validation logic to enforce business rules for that data.</span></span>
- <span data-ttu-id="d23d3-108">**V** Iews: Vorlagendateien, die Ihre Anwendung verwendet, um HTML-Antworten dynamisch zu generieren.</span><span class="sxs-lookup"><span data-stu-id="d23d3-108">**V** iews: Template files that your application uses to dynamically generate HTML responses.</span></span>
- <span data-ttu-id="d23d3-109">**C** ontroller: Klassen, die eingehenden Browseranforderungen verarbeiten rufen Modelldaten und geben Sie dann die Vorlagen anzeigen, die eine Antwort an den Browser zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="d23d3-109">**C** ontrollers: Classes that handle incoming browser requests, retrieve model data, and then specify view templates that return a response to the browser.</span></span>

<span data-ttu-id="d23d3-110">Wir werden all diese Konzepte in dieser tutorialreihe abdecken und gezeigt, wie Sie diese verwenden, um eine Anwendung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="d23d3-110">We'll be covering all these concepts in this tutorial series and show you how to use them to build an application.</span></span>

> [!NOTE]
> <span data-ttu-id="d23d3-111">Im vorherigen Schritt des Standard-MVC wurde die Vorlage ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="d23d3-111">In the previous step the Default MVC template was selected.</span></span> <span data-ttu-id="d23d3-112">Dies erstellt Home-Konto und die Controller verwalten, standardmäßig.</span><span class="sxs-lookup"><span data-stu-id="d23d3-112">This creates Home, Account and Manage Controllers by default.</span></span>

<span data-ttu-id="d23d3-113">Wir erstellen zunächst eine Controller-Klasse.</span><span class="sxs-lookup"><span data-stu-id="d23d3-113">Let's begin by creating a controller class.</span></span> <span data-ttu-id="d23d3-114">In **Projektmappen-Explorer**, mit der rechten Maustaste die *Controller* Ordner, und klicken Sie dann auf **hinzufügen**, klicken Sie dann **Controller**.</span><span class="sxs-lookup"><span data-stu-id="d23d3-114">In **Solution Explorer**, right-click the *Controllers* folder and then click **Add**, then **Controller**.</span></span>


![](adding-a-controller/_static/image1.png)

<span data-ttu-id="d23d3-115">In der **Gerüst hinzufügen** Dialogfeld klicken Sie auf **MVC 5 Controller - leer**, und klicken Sie dann auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="d23d3-115">In the **Add Scaffold** dialog box, click **MVC 5 Controller - Empty**, and then click **Add**.</span></span>

![](adding-a-controller/_static/image2.png)  
 

<span data-ttu-id="d23d3-116">Geben Sie Ihre neuen Controller den Namen "HelloWorldController", und klicken Sie auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="d23d3-116">Name your new controller "HelloWorldController" and click **Add**.</span></span>

![Controller hinzufügen](adding-a-controller/_static/image3.png)

<span data-ttu-id="d23d3-118">Beachten Sie, dass **Projektmappen-Explorer** , dass eine neue Datei benannte erstellt wurde *HelloWorldController.cs* und einen neuen Ordner *Views\HelloWorld*.</span><span class="sxs-lookup"><span data-stu-id="d23d3-118">Notice in **Solution Explorer** that a new file has been created named *HelloWorldController.cs* and a new folder *Views\HelloWorld*.</span></span> <span data-ttu-id="d23d3-119">Der Controller ist in der IDE geöffnet.</span><span class="sxs-lookup"><span data-stu-id="d23d3-119">The controller is open in the IDE.</span></span>

![](adding-a-controller/_static/image4.png)

<span data-ttu-id="d23d3-120">Ersetzen Sie den Inhalt der Datei mit dem folgenden Code ein.</span><span class="sxs-lookup"><span data-stu-id="d23d3-120">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

<span data-ttu-id="d23d3-121">Die Kontroller-Methoden gibt eine Zeichenfolge mit HTML als Beispiel zurück.</span><span class="sxs-lookup"><span data-stu-id="d23d3-121">The controller methods will return a string of HTML as an example.</span></span> <span data-ttu-id="d23d3-122">Der Controller wird mit dem Namen `HelloWorldController` und die erste Methode heißt `Index`.</span><span class="sxs-lookup"><span data-stu-id="d23d3-122">The controller is named `HelloWorldController` and the first method is named `Index`.</span></span> <span data-ttu-id="d23d3-123">Lassen Sie uns in einem Browser aufrufen.</span><span class="sxs-lookup"><span data-stu-id="d23d3-123">Let's invoke it from a browser.</span></span> <span data-ttu-id="d23d3-124">Führen Sie die Anwendung (drücken Sie F5 oder STRG + F5).</span><span class="sxs-lookup"><span data-stu-id="d23d3-124">Run the application (press F5 or Ctrl+F5).</span></span> <span data-ttu-id="d23d3-125">Fügen Sie im Browser &quot;HelloWorld&quot; auf den Pfad in der Adressleiste.</span><span class="sxs-lookup"><span data-stu-id="d23d3-125">In the browser, append &quot;HelloWorld&quot; to the path in the address bar.</span></span> <span data-ttu-id="d23d3-126">(Z. B. in der Abbildung unten, dessen `http://localhost:1234/HelloWorld.`) die Seite im Browser wird wie im folgenden Screenshot aussehen.</span><span class="sxs-lookup"><span data-stu-id="d23d3-126">(For example, in the illustration below, it's `http://localhost:1234/HelloWorld.`) The page in the browser will look like the following screenshot.</span></span> <span data-ttu-id="d23d3-127">In der oben genannten Methode hat der Code direkt eine Zeichenfolge zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="d23d3-127">In the method above, the code returned a string directly.</span></span> <span data-ttu-id="d23d3-128">Sie sagte, dass das System nur HTML-Code zurückgeben, und dies der Fall war.</span><span class="sxs-lookup"><span data-stu-id="d23d3-128">You told the system to just return some HTML, and it did!</span></span>

![](adding-a-controller/_static/image5.png)

<span data-ttu-id="d23d3-129">ASP.NET MVC ruft verschiedene Controllerklassen (und darin enthaltene unterschiedliche Aktionsmethoden) abhängig von der eingehenden URL an.</span><span class="sxs-lookup"><span data-stu-id="d23d3-129">ASP.NET MVC invokes different controller classes (and different action methods within them) depending on the incoming URL.</span></span> <span data-ttu-id="d23d3-130">Die URL-Routinglogik auf Standard, die ASP.NET MVC verwendet ein Format wie dieses, um zu bestimmen, welcher Code zum Aufrufen:</span><span class="sxs-lookup"><span data-stu-id="d23d3-130">The default URL routing logic used by ASP.NET MVC uses a format like this to determine what code to invoke:</span></span>

`/[Controller]/[ActionName]/[Parameters]`

<span data-ttu-id="d23d3-131">Sie legen Sie das Format für das routing in der *App\_Start/RouteConfig.cs* Datei.</span><span class="sxs-lookup"><span data-stu-id="d23d3-131">You set the format for routing in the *App\_Start/RouteConfig.cs* file.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample2.cs?highlight=7-8)]

<span data-ttu-id="d23d3-132">Wenn Sie die Anwendung auszuführen, und Sie keine URL-Segmente geben, wird standardmäßig mit dem Controller "Home", und die Aktionsmethode "Index" angegeben wird, in dem Abschnitt "Defaults" des obigen Codes.</span><span class="sxs-lookup"><span data-stu-id="d23d3-132">When you run the application and don't supply any URL segments, it defaults to the "Home" controller and the "Index" action method specified in the defaults section of the code above.</span></span>

<span data-ttu-id="d23d3-133">Der erste Teil der URL bestimmt die auszuführende Controllerklasse.</span><span class="sxs-lookup"><span data-stu-id="d23d3-133">The first part of the URL determines the controller class to execute.</span></span> <span data-ttu-id="d23d3-134">Also */HelloWorld* ordnet die `HelloWorldController` Klasse.</span><span class="sxs-lookup"><span data-stu-id="d23d3-134">So */HelloWorld* maps to the `HelloWorldController` class.</span></span> <span data-ttu-id="d23d3-135">Der zweite Teil der URL bestimmt die Aktionsmethode in der Klasse ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="d23d3-135">The second part of the URL determines the action method on the class to execute.</span></span> <span data-ttu-id="d23d3-136">Also */HelloWorld/Index* würde dazu führen, dass die `Index` Methode der `HelloWorldController` Klasse ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="d23d3-136">So */HelloWorld/Index* would cause the `Index` method of the `HelloWorldController` class to execute.</span></span> <span data-ttu-id="d23d3-137">Beachten Sie, die wir mussten nur durchsuchen */HelloWorld* und `Index` Methode wurde in der Standardeinstellung verwendet.</span><span class="sxs-lookup"><span data-stu-id="d23d3-137">Notice that we only had to browse to */HelloWorld* and the `Index` method was used by default.</span></span> <span data-ttu-id="d23d3-138">Dies ist, da eine Methode mit dem Namen `Index` ist die Standardmethode, die für einen Controller aufgerufen wird, wenn eine nicht explizit angegeben ist.</span><span class="sxs-lookup"><span data-stu-id="d23d3-138">This is because a method named `Index` is the default method that will be called on a controller if one is not explicitly specified.</span></span> <span data-ttu-id="d23d3-139">Der dritte Teil des URL-Segments (`Parameters`) ist für Routendaten.</span><span class="sxs-lookup"><span data-stu-id="d23d3-139">The third part of the URL segment ( `Parameters`) is for route data.</span></span> <span data-ttu-id="d23d3-140">Weiterleiten von Daten dass später in diesem Tutorial sehen.</span><span class="sxs-lookup"><span data-stu-id="d23d3-140">We'll see route data later on in this tutorial.</span></span>

<span data-ttu-id="d23d3-141">Wechseln Sie zu `http://localhost:xxxx/HelloWorld/Welcome`.</span><span class="sxs-lookup"><span data-stu-id="d23d3-141">Browse to `http://localhost:xxxx/HelloWorld/Welcome`.</span></span> <span data-ttu-id="d23d3-142">Die `Welcome` wird ausgeführt und gibt die Zeichenfolge &quot;Dies ist die Welcome Action-Methode... &quot;.</span><span class="sxs-lookup"><span data-stu-id="d23d3-142">The `Welcome` method runs and returns the string &quot;This is the Welcome action method...&quot;.</span></span> <span data-ttu-id="d23d3-143">Die standardzuordnung für MVC ist `/[Controller]/[ActionName]/[Parameters]`.</span><span class="sxs-lookup"><span data-stu-id="d23d3-143">The default MVC mapping is `/[Controller]/[ActionName]/[Parameters]`.</span></span> <span data-ttu-id="d23d3-144">Bei dieser URL ist `HelloWorld` der Controller und `Welcome` die Aktionsmethode.</span><span class="sxs-lookup"><span data-stu-id="d23d3-144">For this URL, the controller is `HelloWorld` and `Welcome` is the action method.</span></span> <span data-ttu-id="d23d3-145">Sie haben den Teil `[Parameters]` der URL noch nicht verwendet.</span><span class="sxs-lookup"><span data-stu-id="d23d3-145">You haven't used the `[Parameters]` part of the URL yet.</span></span>

![](adding-a-controller/_static/image6.png)

<span data-ttu-id="d23d3-146">Lassen Sie uns das Beispiel etwas so ändern, dass Parameterinformationen aus der URL an den Controller übergeben werden können (z. B. */HelloWorld/Welcome? Name = Scott&amp;Numtimes = 4*).</span><span class="sxs-lookup"><span data-stu-id="d23d3-146">Let's modify the example slightly so that you can pass some parameter information from the URL to the controller (for example, */HelloWorld/Welcome?name=Scott&amp;numtimes=4*).</span></span> <span data-ttu-id="d23d3-147">Ändern Ihrer `Welcome` Methode, um zwei Parameter enthalten, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="d23d3-147">Change your `Welcome` method to include two parameters as shown below.</span></span> <span data-ttu-id="d23d3-148">Beachten Sie, dass der Code die c#-Funktion optional-Parameter verwendet, um anzugeben, dass die `numTimes` Parameter sollte der Standardwert 1, wenn kein Wert für diesen Parameter übergeben wird.</span><span class="sxs-lookup"><span data-stu-id="d23d3-148">Note that the code uses the C# optional-parameter feature to indicate that the `numTimes` parameter should default to 1 if no value is passed for that parameter.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample3.cs)]

> [!NOTE]
> <span data-ttu-id="d23d3-149">Sicherheitshinweis: Der Code oben verwendet [HttpUtility.HtmlEncode durchführen](https://msdn.microsoft.com/library/ee360286(v=vs.110).aspx) zum Schützen der Anwendung vor schädlicher Eingaben (über JavaScript).</span><span class="sxs-lookup"><span data-stu-id="d23d3-149">Security Note: The code above uses [HttpUtility.HtmlEncode](https://msdn.microsoft.com/library/ee360286(v=vs.110).aspx) to protect the application from malicious input (namely JavaScript).</span></span> <span data-ttu-id="d23d3-150">Weitere Informationen finden Sie unter [How to: Protect Against Script Exploits in einer Webanwendung durch Anwenden von HTML-Codierung in Zeichenfolgen](https://msdn.microsoft.com/library/a2a4yykt(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="d23d3-150">For more information see [How to: Protect Against Script Exploits in a Web Application by Applying HTML Encoding to Strings](https://msdn.microsoft.com/library/a2a4yykt(v=vs.100).aspx).</span></span>


 <span data-ttu-id="d23d3-151">Führen Sie die Anwendung, und navigieren Sie zu der Beispiel-URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`).</span><span class="sxs-lookup"><span data-stu-id="d23d3-151">Run your application and browse to the example URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`).</span></span> <span data-ttu-id="d23d3-152">Sie können versuchen, verschiedene Werte für `name` und `numtimes` in der URL.</span><span class="sxs-lookup"><span data-stu-id="d23d3-152">You can try different values for `name` and `numtimes` in the URL.</span></span> <span data-ttu-id="d23d3-153">Die [ASP.NET MVC-modellbindungssystem](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) ordnet automatisch die benannten Parameter aus der Abfragezeichenfolge in der Adressleiste den Parametern der Methode.</span><span class="sxs-lookup"><span data-stu-id="d23d3-153">The [ASP.NET MVC model binding system](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automatically maps the named parameters from the query string in the address bar to parameters in your method.</span></span>

![](adding-a-controller/_static/image7.png)

<span data-ttu-id="d23d3-154">Im Beispiel oben das URL-Segment ( `Parameters`) nicht verwendet wird, die `name` und `numTimes` als Parameter übergebene [Abfragezeichenfolgen](http://en.wikipedia.org/wiki/Query_string).</span><span class="sxs-lookup"><span data-stu-id="d23d3-154">In the sample above, the URL segment ( `Parameters`) is not used, the `name` and `numTimes` parameters are passed as [query strings](http://en.wikipedia.org/wiki/Query_string).</span></span> <span data-ttu-id="d23d3-155">Das Platzhalterzeichen ?</span><span class="sxs-lookup"><span data-stu-id="d23d3-155">The ?</span></span> <span data-ttu-id="d23d3-156">(Fragezeichen) in der obigen URL ist ein Trennzeichen aus, und das die Abfragezeichenfolgen folgen.</span><span class="sxs-lookup"><span data-stu-id="d23d3-156">(question mark) in the above URL is a separator, and the query strings follow.</span></span> <span data-ttu-id="d23d3-157">Das Zeichen &amp; trennt Abfragezeichenfolgen.</span><span class="sxs-lookup"><span data-stu-id="d23d3-157">The &amp; character separates query strings.</span></span>

<span data-ttu-id="d23d3-158">Ersetzen Sie die willkommene-Methode, mit dem folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="d23d3-158">Replace the Welcome method with the following code:</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample4.cs)]

<span data-ttu-id="d23d3-159">Führen Sie die Anwendung aus, und geben Sie die folgende URL: `http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`</span><span class="sxs-lookup"><span data-stu-id="d23d3-159">Run the application and enter the following URL: `http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`</span></span>

![](adding-a-controller/_static/image8.png)

<span data-ttu-id="d23d3-160">Dieses Mal das dritte URL-Segment abgeglichen routenparameters `ID.` der `Welcome` Aktionsmethode enthält einen Parameter (`ID`), die mit der URL-Spezifikation in der `RegisterRoutes` Methode.</span><span class="sxs-lookup"><span data-stu-id="d23d3-160">This time the third URL segment matched the route parameter `ID.` The `Welcome` action method contains a parameter (`ID`) that matched the URL specification in the `RegisterRoutes` method.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample5.cs?highlight=7)]

<span data-ttu-id="d23d3-161">In ASP.NET MVC-Anwendungen ist es üblicher, übergeben Sie Parameter als Routendaten (wie mit der ID oben) als Übergabe als Abfragezeichenfolgen.</span><span class="sxs-lookup"><span data-stu-id="d23d3-161">In ASP.NET MVC applications, it's more typical to pass in parameters as route data (like we did with ID above) than passing them as query strings.</span></span> <span data-ttu-id="d23d3-162">Sie könnten auch hinzufügen, eine Route, um beide übergeben die `name` und `numtimes` in Parametern wie in der URL-Routendaten.</span><span class="sxs-lookup"><span data-stu-id="d23d3-162">You could also add a route to pass both the `name` and `numtimes` in parameters as route data in the URL.</span></span> <span data-ttu-id="d23d3-163">In der *App\_start\routeconfig* Datei, die "Hello"-Route hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="d23d3-163">In the *App\_Start\RouteConfig.cs* file, add the "Hello" route:</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample6.cs?highlight=13-16)]

<span data-ttu-id="d23d3-164">Führen Sie die Anwendung, und navigieren Sie zu `/localhost:XXX/HelloWorld/Welcome/Scott/3`.</span><span class="sxs-lookup"><span data-stu-id="d23d3-164">Run the application and browse to `/localhost:XXX/HelloWorld/Welcome/Scott/3`.</span></span>

![](adding-a-controller/_static/image9.png)

<span data-ttu-id="d23d3-165">Viele MVC-Anwendungen auf funktioniert die Standardroute.</span><span class="sxs-lookup"><span data-stu-id="d23d3-165">For many MVC applications, the default route works fine.</span></span> <span data-ttu-id="d23d3-166">Erfahren Sie später in diesem Lernprogramm aus, um Daten mithilfe der modellbindung zu übergeben, und ändern Sie die Standardroute für, die keine.</span><span class="sxs-lookup"><span data-stu-id="d23d3-166">You'll learn later in this tutorial to pass data using the model binder, and you won't have to modify the default route for that.</span></span>

<span data-ttu-id="d23d3-167">In diesen Beispielen wurde der Controller Ausführen der &quot;VC&quot; -Teil von MVC – d. h. die Ansicht und Controller Arbeit.</span><span class="sxs-lookup"><span data-stu-id="d23d3-167">In these examples the controller has been doing the &quot;VC&quot; portion of MVC — that is, the view and controller work.</span></span> <span data-ttu-id="d23d3-168">Der Controller gibt HTML direkt zurück.</span><span class="sxs-lookup"><span data-stu-id="d23d3-168">The controller is returning HTML directly.</span></span> <span data-ttu-id="d23d3-169">Normalerweise sollen nicht Controller HTML direkt zurückgeben, da dies sehr mühsam Code wird.</span><span class="sxs-lookup"><span data-stu-id="d23d3-169">Ordinarily you don't want controllers returning HTML directly, since that becomes very cumbersome to code.</span></span> <span data-ttu-id="d23d3-170">Stattdessen wird in der Regel eine separate ansichtsvorlagendatei verwendet, um Hilfe, die HTML-Antwort zu generieren.</span><span class="sxs-lookup"><span data-stu-id="d23d3-170">Instead we'll typically use a separate view template file to help generate the HTML response.</span></span> <span data-ttu-id="d23d3-171">Sehen wir uns weiter an, wie wir dies erreichen können.</span><span class="sxs-lookup"><span data-stu-id="d23d3-171">Let's look next at how we can do this.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d23d3-172">[Zurück](getting-started.md)
> [Weiter](adding-a-view.md)</span><span class="sxs-lookup"><span data-stu-id="d23d3-172">[Previous](getting-started.md)
[Next](adding-a-view.md)</span></span>
