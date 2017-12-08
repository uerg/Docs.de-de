---
uid: mvc/overview/getting-started/introduction/adding-a-controller
title: "Hinzufügen eines Controllers | Microsoft Docs"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: cc764f3b-6921-486a-8f44-c6ccd1249acd
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 878d957344a08450b82b0249d8ca2a205810da4a
ms.sourcegitcommit: 9ecd4e9fb0c40c3693dab079eab1ff94b461c922
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2017
---
<a name="adding-a-controller"></a><span data-ttu-id="48215-102">Hinzufügen eines Controllers</span><span class="sxs-lookup"><span data-stu-id="48215-102">Adding a Controller</span></span>
====================
<span data-ttu-id="48215-103">Durch [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="48215-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE[Tutorial Note](sample/code-location.md)]

<span data-ttu-id="48215-104">MVC steht für *Model-View-Controller*.</span><span class="sxs-lookup"><span data-stu-id="48215-104">MVC stands for *model-view-controller*.</span></span> <span data-ttu-id="48215-105">MVC ist ein Muster für die Entwicklung von Anwendungen, die gut entworfen, einfach zu testender und leicht zu warten sind.</span><span class="sxs-lookup"><span data-stu-id="48215-105">MVC is a pattern for developing applications that are well architected, testable and easy to maintain.</span></span> <span data-ttu-id="48215-106">MVC-basierten Anwendungen enthalten:</span><span class="sxs-lookup"><span data-stu-id="48215-106">MVC-based applications contain:</span></span>

- <span data-ttu-id="48215-107">**M** Odels: Klassen, die die Daten der Anwendung darstellen, und, die Validierungslogik zum Erzwingen von Geschäftsregeln für diese Daten verwenden.</span><span class="sxs-lookup"><span data-stu-id="48215-107">**M** odels: Classes that represent the data of the application and that use validation logic to enforce business rules for that data.</span></span>
- <span data-ttu-id="48215-108">**V** Iews: Vorlagendateien von der Anwendung zum dynamischen Generieren von HTML-Antworten verwendeten.</span><span class="sxs-lookup"><span data-stu-id="48215-108">**V** iews: Template files that your application uses to dynamically generate HTML responses.</span></span>
- <span data-ttu-id="48215-109">**C** Ontrollers: Klassen, die Behandlung eingehender Browseranforderungen, Abrufen von Daten des Modells, und geben Sie Vorlagen anzeigen, die eine Antwort an den Browser zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="48215-109">**C** ontrollers: Classes that handle incoming browser requests, retrieve model data, and then specify view templates that return a response to the browser.</span></span>

<span data-ttu-id="48215-110">Wir werden all diese Konzepte in diesem Lernprogramm Reihe abdecken und erläutert deren Verwendung zum Erstellen einer Anwendung.</span><span class="sxs-lookup"><span data-stu-id="48215-110">We'll be covering all these concepts in this tutorial series and show you how to use them to build an application.</span></span>

> [!NOTE]
> <span data-ttu-id="48215-111">Im vorherigen Schritt die Standard-MVC wurde die Vorlage ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="48215-111">In the previous step the Default MVC template was selected.</span></span> <span data-ttu-id="48215-112">Dies erstellt Home, Konto und Controller standardmäßig verwalten.</span><span class="sxs-lookup"><span data-stu-id="48215-112">This creates Home, Account and Manage Controllers by default.</span></span>

<span data-ttu-id="48215-113">Beginnen wir mit eine Controllerklasse erstellen.</span><span class="sxs-lookup"><span data-stu-id="48215-113">Let's begin by creating a controller class.</span></span> <span data-ttu-id="48215-114">In **Projektmappen-Explorer**, mit der rechten Maustaste die *Controller* Ordner, und klicken Sie dann auf **hinzufügen**, klicken Sie dann **Controller**.</span><span class="sxs-lookup"><span data-stu-id="48215-114">In **Solution Explorer**, right-click the *Controllers* folder and then click **Add**, then **Controller**.</span></span>


![](adding-a-controller/_static/image1.png)

<span data-ttu-id="48215-115">In der **Gerüst hinzufügen** (Dialogfeld), klicken Sie auf **MVC 5-Controller - leere**, und klicken Sie dann auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="48215-115">In the **Add Scaffold** dialog box, click **MVC 5 Controller - Empty**, and then click **Add**.</span></span>

![](adding-a-controller/_static/image2.png)  
 

<span data-ttu-id="48215-116">Nennen Sie Ihre neue Controller "HelloWorldController", und klicken Sie auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="48215-116">Name your new controller "HelloWorldController" and click **Add**.</span></span>

![Hinzufügen eines Controllers](adding-a-controller/_static/image3.png)

<span data-ttu-id="48215-118">Beachten Sie im **Projektmappen-Explorer** , dass eine neue Datei benannte erstellt wurden *HelloWorldController.cs* und einen neuen Ordner *Views\HelloWorld*.</span><span class="sxs-lookup"><span data-stu-id="48215-118">Notice in **Solution Explorer** that a new file has been created named *HelloWorldController.cs* and a new folder *Views\HelloWorld*.</span></span> <span data-ttu-id="48215-119">Der Controller ist in der IDE geöffnet.</span><span class="sxs-lookup"><span data-stu-id="48215-119">The controller is open in the IDE.</span></span>

![](adding-a-controller/_static/image4.png)

<span data-ttu-id="48215-120">Ersetzen Sie den Inhalt der Datei durch den folgenden Code ein.</span><span class="sxs-lookup"><span data-stu-id="48215-120">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

<span data-ttu-id="48215-121">Die Controllermethoden werden als Beispiel eine HTML-Zeichenfolge zurück.</span><span class="sxs-lookup"><span data-stu-id="48215-121">The controller methods will return a string of HTML as an example.</span></span> <span data-ttu-id="48215-122">Der Domänencontroller heißt `HelloWorldController` und die erste Methode ist mit dem Namen `Index`.</span><span class="sxs-lookup"><span data-stu-id="48215-122">The controller is named `HelloWorldController` and the first method is named `Index`.</span></span> <span data-ttu-id="48215-123">Nehmen wir in einem Browser aufrufen.</span><span class="sxs-lookup"><span data-stu-id="48215-123">Let's invoke it from a browser.</span></span> <span data-ttu-id="48215-124">Führen Sie die Anwendung (drücken Sie F5 oder STRG + F5).</span><span class="sxs-lookup"><span data-stu-id="48215-124">Run the application (press F5 or Ctrl+F5).</span></span> <span data-ttu-id="48215-125">Fügen Sie im Browser &quot;HelloWorld&quot; auf den Pfad in der Adressleiste.</span><span class="sxs-lookup"><span data-stu-id="48215-125">In the browser, append &quot;HelloWorld&quot; to the path in the address bar.</span></span> <span data-ttu-id="48215-126">(Z. B. in der Abbildung unten, dessen `http://localhost:1234/HelloWorld.`) die Seite im Browser wird das folgende Bildschirmfoto aussehen.</span><span class="sxs-lookup"><span data-stu-id="48215-126">(For example, in the illustration below, it's `http://localhost:1234/HelloWorld.`) The page in the browser will look like the following screenshot.</span></span> <span data-ttu-id="48215-127">In der oben genannten Methode hat der Code direkt eine Zeichenfolge zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="48215-127">In the method above, the code returned a string directly.</span></span> <span data-ttu-id="48215-128">Sie mitgeteilt, dass das System auf HTML-Code zurückgeben, und es wurde!</span><span class="sxs-lookup"><span data-stu-id="48215-128">You told the system to just return some HTML, and it did!</span></span>

![](adding-a-controller/_static/image5.png)

<span data-ttu-id="48215-129">ASP.NET MVC ruft verschiedene Controllerklassen (und andere Aktionsmethoden darin enthaltene) abhängig von der eingehenden URL an.</span><span class="sxs-lookup"><span data-stu-id="48215-129">ASP.NET MVC invokes different controller classes (and different action methods within them) depending on the incoming URL.</span></span> <span data-ttu-id="48215-130">Die Standard-URL Routinglogik von ASP.NET MVC verwendet ein Format wie folgt, um zu bestimmen, welcher Code zum Aufrufen:</span><span class="sxs-lookup"><span data-stu-id="48215-130">The default URL routing logic used by ASP.NET MVC uses a format like this to determine what code to invoke:</span></span>

`/[Controller]/[ActionName]/[Parameters]`

<span data-ttu-id="48215-131">Legen Sie das Format für das routing in der *App\_Start/RouteConfig.cs* Datei.</span><span class="sxs-lookup"><span data-stu-id="48215-131">You set the format for routing in the *App\_Start/RouteConfig.cs* file.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample2.cs?highlight=7-8)]

<span data-ttu-id="48215-132">Wenn Sie die Anwendung auszuführen, und Sie keine URL-Segmente geben, wird standardmäßig die "Home" Controller und die Aktionsmethode "Index" im Abschnitt "Standardwerte" des obigen Codes angegeben.</span><span class="sxs-lookup"><span data-stu-id="48215-132">When you run the application and don't supply any URL segments, it defaults to the "Home" controller and the "Index" action method specified in the defaults section of the code above.</span></span>

<span data-ttu-id="48215-133">Der erste Teil der URL bestimmt die Controllerklasse ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="48215-133">The first part of the URL determines the controller class to execute.</span></span> <span data-ttu-id="48215-134">Daher */HelloWorld* ordnet die `HelloWorldController` Klasse.</span><span class="sxs-lookup"><span data-stu-id="48215-134">So */HelloWorld* maps to the `HelloWorldController` class.</span></span> <span data-ttu-id="48215-135">Der zweite Teil der URL bestimmt die Aktionsmethode in der Klasse ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="48215-135">The second part of the URL determines the action method on the class to execute.</span></span> <span data-ttu-id="48215-136">Daher */HelloWorld/Index* würde dazu führen, dass die `Index` Methode der `HelloWorldController` Klasse ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="48215-136">So */HelloWorld/Index* would cause the `Index` method of the `HelloWorldController` class to execute.</span></span> <span data-ttu-id="48215-137">Beachten Sie, das wir durchsuchen musste */HelloWorld* und die `Index` Methode wurde in der Standardeinstellung verwendet.</span><span class="sxs-lookup"><span data-stu-id="48215-137">Notice that we only had to browse to */HelloWorld* and the `Index` method was used by default.</span></span> <span data-ttu-id="48215-138">Dies ist, da eine Methode mit dem Namen `Index` ist die Standardmethode, die auf einem Domänencontroller aufgerufen wird, wenn nicht explizit angegeben ist.</span><span class="sxs-lookup"><span data-stu-id="48215-138">This is because a method named `Index` is the default method that will be called on a controller if one is not explicitly specified.</span></span> <span data-ttu-id="48215-139">Der dritte Teil des URL-Segments (`Parameters`) ist für Routendaten.</span><span class="sxs-lookup"><span data-stu-id="48215-139">The third part of the URL segment ( `Parameters`) is for route data.</span></span> <span data-ttu-id="48215-140">Wir sehen Routendaten weiter unten in diesem Lernprogramm.</span><span class="sxs-lookup"><span data-stu-id="48215-140">We'll see route data later on in this tutorial.</span></span>

<span data-ttu-id="48215-141">Wechseln Sie zu `http://localhost:xxxx/HelloWorld/Welcome`.</span><span class="sxs-lookup"><span data-stu-id="48215-141">Browse to `http://localhost:xxxx/HelloWorld/Welcome`.</span></span> <span data-ttu-id="48215-142">Die `Welcome` wird ausgeführt und gibt die Zeichenfolge &quot;Dies ist die Willkommensseite Aktionsmethode... &quot;.</span><span class="sxs-lookup"><span data-stu-id="48215-142">The `Welcome` method runs and returns the string &quot;This is the Welcome action method...&quot;.</span></span> <span data-ttu-id="48215-143">Die standardzuordnung für MVC ist `/[Controller]/[ActionName]/[Parameters]`.</span><span class="sxs-lookup"><span data-stu-id="48215-143">The default MVC mapping is `/[Controller]/[ActionName]/[Parameters]`.</span></span> <span data-ttu-id="48215-144">Bei dieser URL ist `HelloWorld` der Controller und `Welcome` die Aktionsmethode.</span><span class="sxs-lookup"><span data-stu-id="48215-144">For this URL, the controller is `HelloWorld` and `Welcome` is the action method.</span></span> <span data-ttu-id="48215-145">Sie haben den Teil `[Parameters]` der URL noch nicht verwendet.</span><span class="sxs-lookup"><span data-stu-id="48215-145">You haven't used the `[Parameters]` part of the URL yet.</span></span>

![](adding-a-controller/_static/image6.png)

<span data-ttu-id="48215-146">Nehmen wir das Beispiel etwas so ändern, dass einige Parameterinformationen aus der URL an den Controller übergeben werden können (z. B. */HelloWorld/Willkommen? Name = Scott&amp;Numtimes = 4*).</span><span class="sxs-lookup"><span data-stu-id="48215-146">Let's modify the example slightly so that you can pass some parameter information from the URL to the controller (for example, */HelloWorld/Welcome?name=Scott&amp;numtimes=4*).</span></span> <span data-ttu-id="48215-147">Ändern Ihrer `Welcome` Methode, um zwei Parameter enthalten, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="48215-147">Change your `Welcome` method to include two parameters as shown below.</span></span> <span data-ttu-id="48215-148">Beachten Sie, dass der Code die C#-optionalen Parameter-Funktion verwendet, um anzugeben, dass die `numTimes` Parameter sollte auf 1 Standard, falls kein Wert für diesen Parameter übergeben wird.</span><span class="sxs-lookup"><span data-stu-id="48215-148">Note that the code uses the C# optional-parameter feature to indicate that the `numTimes` parameter should default to 1 if no value is passed for that parameter.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample3.cs)]

> [!NOTE]
> <span data-ttu-id="48215-149">Sicherheitshinweis: Der Code oben verwendet [HttpUtility.HtmlEncode durchführen](https://msdn.microsoft.com/en-us/library/ee360286(v=vs.110).aspx) zum Schützen der Anwendung von böswillige Eingaben (d. h. "JavaScript").</span><span class="sxs-lookup"><span data-stu-id="48215-149">Security Note: The code above uses [HttpUtility.HtmlEncode](https://msdn.microsoft.com/en-us/library/ee360286(v=vs.110).aspx) to protect the application from malicious input (namely JavaScript).</span></span> <span data-ttu-id="48215-150">Weitere Informationen finden Sie unter [How to: Protect Against Script Exploits in einer Web-Anwendung durch Anwenden von HTML-Codierung in Zeichenfolgen](https://msdn.microsoft.com/en-us/library/a2a4yykt(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="48215-150">For more information see [How to: Protect Against Script Exploits in a Web Application by Applying HTML Encoding to Strings](https://msdn.microsoft.com/en-us/library/a2a4yykt(v=vs.100).aspx).</span></span>


 <span data-ttu-id="48215-151">Führen Sie die Anwendung, und navigieren Sie zu der Beispiel-URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`).</span><span class="sxs-lookup"><span data-stu-id="48215-151">Run your application and browse to the example URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`).</span></span> <span data-ttu-id="48215-152">Sie können versuchen, verschiedene Werte für `name` und `numtimes` in der URL.</span><span class="sxs-lookup"><span data-stu-id="48215-152">You can try different values for `name` and `numtimes` in the URL.</span></span> <span data-ttu-id="48215-153">Die [Bindungssystem für ASP.NET MVC-Modell](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) ordnet automatisch die benannten Parameter aus der Abfragezeichenfolge in der Adressleiste den Parametern der Methode.</span><span class="sxs-lookup"><span data-stu-id="48215-153">The [ASP.NET MVC model binding system](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automatically maps the named parameters from the query string in the address bar to parameters in your method.</span></span>

![](adding-a-controller/_static/image7.png)

<span data-ttu-id="48215-154">Im Beispiel oben das URL-Segment ( `Parameters`) nicht verwendet wird, die `name` und `numTimes` Parameter werden als übergeben [Abfragezeichenfolgen](http://en.wikipedia.org/wiki/Query_string).</span><span class="sxs-lookup"><span data-stu-id="48215-154">In the sample above, the URL segment ( `Parameters`) is not used, the `name` and `numTimes` parameters are passed as [query strings](http://en.wikipedia.org/wiki/Query_string).</span></span> <span data-ttu-id="48215-155">Das Platzhalterzeichen ?</span><span class="sxs-lookup"><span data-stu-id="48215-155">The ?</span></span> <span data-ttu-id="48215-156">(Fragezeichen) in der obigen URL ein Trennzeichen ist, und befolgen Sie die Abfragezeichenfolgen an Sie.</span><span class="sxs-lookup"><span data-stu-id="48215-156">(question mark) in the above URL is a separator, and the query strings follow.</span></span> <span data-ttu-id="48215-157">Das Zeichen &amp; trennt Abfragezeichenfolgen.</span><span class="sxs-lookup"><span data-stu-id="48215-157">The &amp; character separates query strings.</span></span>

<span data-ttu-id="48215-158">Ersetzen Sie die Willkommensseite Methode durch den folgenden Code ein:</span><span class="sxs-lookup"><span data-stu-id="48215-158">Replace the Welcome method with the following code:</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample4.cs)]

<span data-ttu-id="48215-159">Führen Sie die Anwendung, und geben Sie die folgende URL:`http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`</span><span class="sxs-lookup"><span data-stu-id="48215-159">Run the application and enter the following URL: `http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`</span></span>

![](adding-a-controller/_static/image8.png)

<span data-ttu-id="48215-160">Dieses Mal das dritte URL-Segment abgeglichen routenparameters `ID.` der `Welcome` Aktionsmethode enthält einen Parameter (`ID`), die mit der URL-Spezifikation in der `RegisterRoutes` Methode.</span><span class="sxs-lookup"><span data-stu-id="48215-160">This time the third URL segment matched the route parameter `ID.` The `Welcome` action method contains a parameter (`ID`) that matched the URL specification in the `RegisterRoutes` method.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample5.cs?highlight=7)]

<span data-ttu-id="48215-161">In ASP.NET MVC-Anwendungen ist es besser in der Regel als Routendaten (z. B. wir mit der ID oben haben) als Übergabe als Abfragezeichenfolgen Parameter zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="48215-161">In ASP.NET MVC applications, it's more typical to pass in parameters as route data (like we did with ID above) than passing them as query strings.</span></span> <span data-ttu-id="48215-162">Sie können auch eine Route zum Übergeben von sowohl Hinzufügen der `name` und `numtimes` in Parametern wie in der URL-Routendaten.</span><span class="sxs-lookup"><span data-stu-id="48215-162">You could also add a route to pass both the `name` and `numtimes` in parameters as route data in the URL.</span></span> <span data-ttu-id="48215-163">In der *App\_Start\RouteConfig.cs* Datei, die "Hello"-Route hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="48215-163">In the *App\_Start\RouteConfig.cs* file, add the "Hello" route:</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample6.cs?highlight=13-16)]

<span data-ttu-id="48215-164">Führen Sie die Anwendung, und navigieren Sie zu `/localhost:XXX/HelloWorld/Welcome/Scott/3`.</span><span class="sxs-lookup"><span data-stu-id="48215-164">Run the application and browse to `/localhost:XXX/HelloWorld/Welcome/Scott/3`.</span></span>

![](adding-a-controller/_static/image9.png)

<span data-ttu-id="48215-165">Viele MVC-Anwendungen auf funktioniert die Standardroute.</span><span class="sxs-lookup"><span data-stu-id="48215-165">For many MVC applications, the default route works fine.</span></span> <span data-ttu-id="48215-166">Erfahren Sie weiter unten in diesem Lernprogramm aus, um Daten mit den Modellbinder übergeben, und müssen nicht die Standardroute für diese zu ändern.</span><span class="sxs-lookup"><span data-stu-id="48215-166">You'll learn later in this tutorial to pass data using the model binder, and you won't have to modify the default route for that.</span></span>

<span data-ttu-id="48215-167">In diesen Beispielen wurde der Controller auf diese Weise wurde das &quot;VC&quot; Teil MVC –, also die Ansicht und Controller-Aufgaben.</span><span class="sxs-lookup"><span data-stu-id="48215-167">In these examples the controller has been doing the &quot;VC&quot; portion of MVC — that is, the view and controller work.</span></span> <span data-ttu-id="48215-168">Der Controller ist HTML direkt zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="48215-168">The controller is returning HTML directly.</span></span> <span data-ttu-id="48215-169">In der Regel soll nicht Controller HTML direkt zurückgeben, da, die sehr aufwendig, Code wird.</span><span class="sxs-lookup"><span data-stu-id="48215-169">Ordinarily you don't want controllers returning HTML directly, since that becomes very cumbersome to code.</span></span> <span data-ttu-id="48215-170">Wir müssen stattdessen in der Regel eine separate Ansicht Vorlagendatei verwenden, können Sie die HTML-Antwort zu generieren.</span><span class="sxs-lookup"><span data-stu-id="48215-170">Instead we'll typically use a separate view template file to help generate the HTML response.</span></span> <span data-ttu-id="48215-171">Sehen wir uns an, wie wir dies weiter.</span><span class="sxs-lookup"><span data-stu-id="48215-171">Let's look next at how we can do this.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="48215-172">[Zurück](getting-started.md)
[Weiter](adding-a-view.md)</span><span class="sxs-lookup"><span data-stu-id="48215-172">[Previous](getting-started.md)
[Next](adding-a-view.md)</span></span>
