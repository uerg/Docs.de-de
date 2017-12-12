---
uid: web-api/overview/advanced/http-message-handlers
title: HTTP-Meldungshandler in ASP.NET Web-API | Microsoft Docs
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/13/2012
ms.topic: article
ms.assetid: 9002018b-3aa3-4358-bb1c-fbb5bc751d01
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/http-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 931ad3330d5e4dc2424ab37b8a6e2a3d123a8684
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="http-message-handlers-in-aspnet-web-api"></a><span data-ttu-id="700c5-102">HTTP-Meldungshandler in ASP.NET Web-API</span><span class="sxs-lookup"><span data-stu-id="700c5-102">HTTP Message Handlers in ASP.NET Web API</span></span>
====================
<span data-ttu-id="700c5-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="700c5-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="700c5-104">Ein *Nachrichtenhandler* ist eine Klasse, empfängt eine HTTP-Anforderung und gibt eine HTTP-Antwort zurück.</span><span class="sxs-lookup"><span data-stu-id="700c5-104">A *message handler* is a class that receives an HTTP request and returns an HTTP response.</span></span> <span data-ttu-id="700c5-105">Meldungshandler Ableiten von der abstrakten **HttpMessageHandler** Klasse.</span><span class="sxs-lookup"><span data-stu-id="700c5-105">Message handlers derive from the abstract **HttpMessageHandler** class.</span></span>

<span data-ttu-id="700c5-106">In der Regel eine Reihe von Meldungshandler verkettet werden.</span><span class="sxs-lookup"><span data-stu-id="700c5-106">Typically, a series of message handlers are chained together.</span></span> <span data-ttu-id="700c5-107">Der erste Handler empfängt eine HTTP-Anforderung, Verarbeitung und die Anforderung an dem nächsten Handler ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="700c5-107">The first handler receives an HTTP request, does some processing, and gives the request to the next handler.</span></span> <span data-ttu-id="700c5-108">Die Antwort wird erstellt und herabgesetzt aufwärts in der Kette, zu einem späteren Zeitpunkt.</span><span class="sxs-lookup"><span data-stu-id="700c5-108">At some point, the response is created and goes back up the chain.</span></span> <span data-ttu-id="700c5-109">Dieses Muster wird aufgerufen, eine *Delegieren* Handler.</span><span class="sxs-lookup"><span data-stu-id="700c5-109">This pattern is called a *delegating* handler.</span></span>

![](http-message-handlers/_static/image1.png)

## <a name="server-side-message-handlers"></a><span data-ttu-id="700c5-110">Serverseitige Meldungshandler</span><span class="sxs-lookup"><span data-stu-id="700c5-110">Server-Side Message Handlers</span></span>

<span data-ttu-id="700c5-111">Auf der Serverseite verwendet die Web-API-Pipeline einige integrierte-Meldungshandler:</span><span class="sxs-lookup"><span data-stu-id="700c5-111">On the server side, the Web API pipeline uses some built-in message handlers:</span></span>

- <span data-ttu-id="700c5-112">**HttpServer** die Anforderung vom Host ab.</span><span class="sxs-lookup"><span data-stu-id="700c5-112">**HttpServer** gets the request from the host.</span></span>
- <span data-ttu-id="700c5-113">**HttpRoutingDispatcher** leitet die Anforderung auf Grundlage der Route.</span><span class="sxs-lookup"><span data-stu-id="700c5-113">**HttpRoutingDispatcher** dispatches the request based on the route.</span></span>
- <span data-ttu-id="700c5-114">**HttpControllerDispatcher** sendet die Anforderung an einen Web-API-Controller.</span><span class="sxs-lookup"><span data-stu-id="700c5-114">**HttpControllerDispatcher** sends the request to a Web API controller.</span></span>

<span data-ttu-id="700c5-115">Sie können benutzerdefinierte Handler für die Pipeline hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="700c5-115">You can add custom handlers to the pipeline.</span></span> <span data-ttu-id="700c5-116">Meldungshandler eignen sich für querschnittliche Sicherheitsrisiken, die auf der Ebene der HTTP-Nachrichten (statt Controlleraktionen) ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="700c5-116">Message handlers are good for cross-cutting concerns that operate at the level of HTTP messages (rather than controller actions).</span></span> <span data-ttu-id="700c5-117">Beispielsweise können ein Meldungshandler:</span><span class="sxs-lookup"><span data-stu-id="700c5-117">For example, a message handler might:</span></span>

- <span data-ttu-id="700c5-118">Lesen oder Ändern von Anforderungsheadern.</span><span class="sxs-lookup"><span data-stu-id="700c5-118">Read or modify request headers.</span></span>
- <span data-ttu-id="700c5-119">Hinzufügen eines Antwortheaders zu antworten.</span><span class="sxs-lookup"><span data-stu-id="700c5-119">Add a response header to responses.</span></span>
- <span data-ttu-id="700c5-120">Überprüfen Sie Anforderungen, bevor sie den Controller erreichen.</span><span class="sxs-lookup"><span data-stu-id="700c5-120">Validate requests before they reach the controller.</span></span>

<span data-ttu-id="700c5-121">Dieses Diagramm zeigt zwei benutzerdefinierte Handler in der Pipeline eingefügt:</span><span class="sxs-lookup"><span data-stu-id="700c5-121">This diagram shows two custom handlers inserted into the pipeline:</span></span>

![](http-message-handlers/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="700c5-122">Auf der Clientseite verwendet "HttpClient" auch Meldungshandler.</span><span class="sxs-lookup"><span data-stu-id="700c5-122">On the client side, HttpClient also uses message handlers.</span></span> <span data-ttu-id="700c5-123">Weitere Informationen finden Sie unter [Meldungshandler für "HttpClient"](httpclient-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="700c5-123">For more information, see [HttpClient Message Handlers](httpclient-message-handlers.md).</span></span>


## <a name="custom-message-handlers"></a><span data-ttu-id="700c5-124">Benutzerdefinierte Meldungshandler</span><span class="sxs-lookup"><span data-stu-id="700c5-124">Custom Message Handlers</span></span>

<span data-ttu-id="700c5-125">Leiten Sie zum Schreiben eines benutzerdefinierten Message-Handlers aus **System.Net.Http.DelegatingHandler** und überschreiben die **"SendAsync"** Methode.</span><span class="sxs-lookup"><span data-stu-id="700c5-125">To write a custom message handler, derive from **System.Net.Http.DelegatingHandler** and override the **SendAsync** method.</span></span> <span data-ttu-id="700c5-126">Diese Methode hat die folgende Signatur:</span><span class="sxs-lookup"><span data-stu-id="700c5-126">This method has the following signature:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample1.cs)]

<span data-ttu-id="700c5-127">Die Methode nimmt ein **HttpRequestMessage** als Eingabe und asynchron gibt eine **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="700c5-127">The method takes an **HttpRequestMessage** as input and asynchronously returns an **HttpResponseMessage**.</span></span> <span data-ttu-id="700c5-128">Eine typische Implementierung führt Folgendes aus:</span><span class="sxs-lookup"><span data-stu-id="700c5-128">A typical implementation does the following:</span></span>

1. <span data-ttu-id="700c5-129">Verarbeitet die Anforderungsnachricht an.</span><span class="sxs-lookup"><span data-stu-id="700c5-129">Process the request message.</span></span>
2. <span data-ttu-id="700c5-130">Rufen Sie `base.SendAsync` zum Senden der Anforderung an den internen Handler.</span><span class="sxs-lookup"><span data-stu-id="700c5-130">Call `base.SendAsync` to send the request to the inner handler.</span></span>
3. <span data-ttu-id="700c5-131">Der innere Handler gibt eine Antwortnachricht zurück.</span><span class="sxs-lookup"><span data-stu-id="700c5-131">The inner handler returns a response message.</span></span> <span data-ttu-id="700c5-132">(Dieser Schritt ist asynchron.)</span><span class="sxs-lookup"><span data-stu-id="700c5-132">(This step is asynchronous.)</span></span>
4. <span data-ttu-id="700c5-133">Verarbeiten der Antwort, und geben sie an den Aufrufer zurück.</span><span class="sxs-lookup"><span data-stu-id="700c5-133">Process the response and return it to the caller.</span></span>

<span data-ttu-id="700c5-134">Hier ist ein einfaches Beispiel:</span><span class="sxs-lookup"><span data-stu-id="700c5-134">Here is a trivial example:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="700c5-135">Der Aufruf von `base.SendAsync` asynchron ist.</span><span class="sxs-lookup"><span data-stu-id="700c5-135">The call to `base.SendAsync` is asynchronous.</span></span> <span data-ttu-id="700c5-136">Wenn der Handler nach diesem Aufruf Arbeit ausführt, verwenden Sie die **"await"** -Schlüsselwort, wie dargestellt.</span><span class="sxs-lookup"><span data-stu-id="700c5-136">If the handler does any work after this call, use the **await** keyword, as shown.</span></span>


<span data-ttu-id="700c5-137">Ein delegierenden Handler inneren Handler auch überspringen und direkt die Antwort zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="700c5-137">A delegating handler can also skip the inner handler and directly create the response:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample3.cs)]

<span data-ttu-id="700c5-138">Wenn eine delegieren Ereignishandler erstellt die Antwort ohne Aufruf `base.SendAsync`, die Anforderung überspringt den Rest der Pipeline.</span><span class="sxs-lookup"><span data-stu-id="700c5-138">If a delegating handler creates the response without calling `base.SendAsync`, the request skips the rest of the pipeline.</span></span> <span data-ttu-id="700c5-139">Dies kann nach einem Ausnahmehandler nützlich sein, die die Anforderung (Erstellen einer Fehlerantwort) überprüft.</span><span class="sxs-lookup"><span data-stu-id="700c5-139">This can be useful for a handler that validates the request (creating an error response).</span></span>

![](http-message-handlers/_static/image3.png)

## <a name="adding-a-handler-to-the-pipeline"></a><span data-ttu-id="700c5-140">Hinzufügen eines Ereignishandlers auf die Pipeline</span><span class="sxs-lookup"><span data-stu-id="700c5-140">Adding a Handler to the Pipeline</span></span>

<span data-ttu-id="700c5-141">Fügen Sie zum Hinzufügen eines meldungshandlers auf der Serverseite den Handler, der die **HttpConfiguration.MessageHandlers** Auflistung.</span><span class="sxs-lookup"><span data-stu-id="700c5-141">To add a message handler on the server side, add the handler to the **HttpConfiguration.MessageHandlers** collection.</span></span> <span data-ttu-id="700c5-142">Wenn Sie die Vorlage "ASP.NET MVC 4-Webanwendung" verwendet, um das Projekt zu erstellen, erreichen Sie dies in der **"webapiconfig"** Klasse:</span><span class="sxs-lookup"><span data-stu-id="700c5-142">If you used the "ASP.NET MVC 4 Web Application" template to create the project, you can do this inside the **WebApiConfig** class:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample4.cs)]

<span data-ttu-id="700c5-143">Message-Ereignishandler werden aufgerufen, in der gleichen Reihenfolge, die sie im angezeigten **MessageHandlers** Auflistung.</span><span class="sxs-lookup"><span data-stu-id="700c5-143">Message handlers are called in the same order that they appear in **MessageHandlers** collection.</span></span> <span data-ttu-id="700c5-144">Da sie geschachtelt sind, durchläuft die Antwortnachricht in die andere Richtung.</span><span class="sxs-lookup"><span data-stu-id="700c5-144">Because they are nested, the response message travels in the other direction.</span></span> <span data-ttu-id="700c5-145">Der letzte Handler ist, also zuerst die Antwortnachricht.</span><span class="sxs-lookup"><span data-stu-id="700c5-145">That is, the last handler is the first to get the response message.</span></span>

<span data-ttu-id="700c5-146">Beachten Sie, dass Sie nicht die inneren Handler festlegen müssen. die Web-API-Framework verbindet sich automatisch die Meldungshandler.</span><span class="sxs-lookup"><span data-stu-id="700c5-146">Notice that you don't need to set the inner handlers; the Web API framework automatically connects the message handlers.</span></span>

<span data-ttu-id="700c5-147">Domänenmodus [Selbsthosting](../older-versions/self-host-a-web-api.md), erstellen Sie eine Instanz von der **HttpSelfHostConfiguration** -Klasse und das Hinzufügen der Handler die **MessageHandlers** Auflistung.</span><span class="sxs-lookup"><span data-stu-id="700c5-147">If you are [self-hosting](../older-versions/self-host-a-web-api.md), create an instance of the **HttpSelfHostConfiguration** class and add the handlers to the **MessageHandlers** collection.</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample5.cs)]

<span data-ttu-id="700c5-148">Jetzt sehen wir uns einige Beispiele für benutzerdefinierte Meldungshandler.</span><span class="sxs-lookup"><span data-stu-id="700c5-148">Now let's look at some examples of custom message handlers.</span></span>

## <a name="example-x-http-method-override"></a><span data-ttu-id="700c5-149">Beispiel: X-HTTP-Method-Override</span><span class="sxs-lookup"><span data-stu-id="700c5-149">Example: X-HTTP-Method-Override</span></span>

<span data-ttu-id="700c5-150">X-HTTP-Method-Override ist eine nicht standardmäßige HTTP-Header.</span><span class="sxs-lookup"><span data-stu-id="700c5-150">X-HTTP-Method-Override is a non-standard HTTP header.</span></span> <span data-ttu-id="700c5-151">Es dient für Clients, die bestimmte HTTP-Anforderungstypen wie PUT oder DELETE senden können.</span><span class="sxs-lookup"><span data-stu-id="700c5-151">It is designed for clients that cannot send certain HTTP request types, such as PUT or DELETE.</span></span> <span data-ttu-id="700c5-152">Stattdessen wird der Client sendet eine POST-Anforderung, und legt den X-HTTP-Method-Override-Header an die gewünschte Methode fest.</span><span class="sxs-lookup"><span data-stu-id="700c5-152">Instead, the client sends a POST request and sets the X-HTTP-Method-Override header to the desired method.</span></span> <span data-ttu-id="700c5-153">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="700c5-153">For example:</span></span>

[!code-console[Main](http-message-handlers/samples/sample6.cmd)]

<span data-ttu-id="700c5-154">Hier ist ein Message-Handler, der die Unterstützung für X-HTTP-Method-Override hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="700c5-154">Here is a message handler that adds support for X-HTTP-Method-Override:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample7.cs)]

<span data-ttu-id="700c5-155">In der **"SendAsync"** -Methode, der Handler überprüft, ob die Anforderungsnachricht eine POST-Anforderung ist, und gibt an, ob den Header X-HTTP-Method-Override enthält.</span><span class="sxs-lookup"><span data-stu-id="700c5-155">In the **SendAsync** method, the handler checks whether the request message is a POST request, and whether it contains the X-HTTP-Method-Override header.</span></span> <span data-ttu-id="700c5-156">Wenn dies der Fall ist, überprüft den Headerwert und ändert anschließend die Anforderungsmethode.</span><span class="sxs-lookup"><span data-stu-id="700c5-156">If so, it validates the header value, and then modifies the request method.</span></span> <span data-ttu-id="700c5-157">Abschließend ruft der Handler `base.SendAsync` die Nachricht an den nächsten Handler zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="700c5-157">Finally, the handler calls `base.SendAsync` to pass the message to the next handler.</span></span>

<span data-ttu-id="700c5-158">Wenn die Anforderung erreicht die **HttpControllerDispatcher** -Klasse, **HttpControllerDispatcher** leitet die Anforderung basierend auf der aktualisierten Anforderungsmethode.</span><span class="sxs-lookup"><span data-stu-id="700c5-158">When the request reaches the **HttpControllerDispatcher** class, **HttpControllerDispatcher** will route the request based on the updated request method.</span></span>

## <a name="example-adding-a-custom-response-header"></a><span data-ttu-id="700c5-159">Beispiel: Hinzufügen eines benutzerdefinierten Antwortheaders</span><span class="sxs-lookup"><span data-stu-id="700c5-159">Example: Adding a Custom Response Header</span></span>

<span data-ttu-id="700c5-160">Hier ist ein Message-Handler, der jede Antwortnachricht einen benutzerdefinierten Header hinzufügt:</span><span class="sxs-lookup"><span data-stu-id="700c5-160">Here is a message handler that adds a custom header to every response message:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample8.cs)]

<span data-ttu-id="700c5-161">Zunächst ruft der Handler `base.SendAsync` , die Anforderung an den inneren Meldungshandler weiterzugeben.</span><span class="sxs-lookup"><span data-stu-id="700c5-161">First, the handler calls `base.SendAsync` to pass the request to the inner message handler.</span></span> <span data-ttu-id="700c5-162">Der innere Handler gibt eine Antwortnachricht zurück, aber erfolgt asynchron über einen **Aufgabe&lt;T&gt;**  Objekt.</span><span class="sxs-lookup"><span data-stu-id="700c5-162">The inner handler returns a response message, but it does so asynchronously using a **Task&lt;T&gt;** object.</span></span> <span data-ttu-id="700c5-163">Die Antwortnachricht ist nicht verfügbar, bis `base.SendAsync` asynchron abgeschlossen.</span><span class="sxs-lookup"><span data-stu-id="700c5-163">The response message is not available until `base.SendAsync` completes asynchronously.</span></span>

<span data-ttu-id="700c5-164">Dieses Beispiel verwendet die **"await"** Schlüsselwort, um die Aufgaben asynchron nach `SendAsync` abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="700c5-164">This example uses the **await** keyword to perform work asynchronously after `SendAsync` completes.</span></span> <span data-ttu-id="700c5-165">Wenn Sie .NET Framework 4.0 abzielen, verwenden die **Aufgabe**&lt;T&gt;**. ContinueWith** Methode:</span><span class="sxs-lookup"><span data-stu-id="700c5-165">If you are targeting .NET Framework 4.0, use the **Task**&lt;T&gt;**.ContinueWith** method:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample9.cs)]

## <a name="example-checking-for-an-api-key"></a><span data-ttu-id="700c5-166">Beispiel: Überprüfung für einen API-Schlüssel</span><span class="sxs-lookup"><span data-stu-id="700c5-166">Example: Checking for an API Key</span></span>

<span data-ttu-id="700c5-167">Einige Webdienste benötigen Clients einen API-Schlüssel in der Anforderung angeben.</span><span class="sxs-lookup"><span data-stu-id="700c5-167">Some web services require clients to include an API key in their request.</span></span> <span data-ttu-id="700c5-168">Das folgende Beispiel zeigt, wie ein Message-Handler Anforderungen für einen gültigen API-Schlüssel überprüfen kann:</span><span class="sxs-lookup"><span data-stu-id="700c5-168">The following example shows how a message handler can check requests for a valid API key:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample10.cs)]

<span data-ttu-id="700c5-169">Dieser Handler sucht nach den API-Schlüssel in der URI-Abfragezeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="700c5-169">This handler looks for the API key in the URI query string.</span></span> <span data-ttu-id="700c5-170">(In diesem Beispiel gehen wir davon aus, dass der Schlüssel eine statische Zeichenfolge ist.</span><span class="sxs-lookup"><span data-stu-id="700c5-170">(For this example, we assume that the key is a static string.</span></span> <span data-ttu-id="700c5-171">"Eine echte Implementierung würde wahrscheinlich eine komplexere Validierung verwendet.) Wenn die Abfragezeichenfolge der Schlüssel enthält, übergibt der Handler die Anforderung an den internen Handler.</span><span class="sxs-lookup"><span data-stu-id="700c5-171">A real implementation would probably use more complex validation.) If the query string contains the key, the handler passes the request to the inner handler.</span></span>

<span data-ttu-id="700c5-172">Wenn die Anforderung nicht über einen gültigen Schlüssel verfügt, erstellt der Handler eine Antwortnachricht mit dem Status 403, verboten.</span><span class="sxs-lookup"><span data-stu-id="700c5-172">If the request does not have a valid key, the handler creates a response message with status 403, Forbidden.</span></span> <span data-ttu-id="700c5-173">In diesem Fall wird der Handler nicht aufgerufen `base.SendAsync`, also innere Handler empfängt die Anforderung, und den Controller ist.</span><span class="sxs-lookup"><span data-stu-id="700c5-173">In this case, the handler does not call `base.SendAsync`, so the inner handler never receives the request, nor does the controller.</span></span> <span data-ttu-id="700c5-174">Der Controller kann daher davon ausgehen, dass alle eingehende Anforderungen einen gültigen API-Schlüssel verfügen.</span><span class="sxs-lookup"><span data-stu-id="700c5-174">Therefore, the controller can assume that all incoming requests have a valid API key.</span></span>

> [!NOTE]
> <span data-ttu-id="700c5-175">Wenn die API-Schlüssel nur für bestimmte Controlleraktionen gilt, erwägen Sie, die einen Aktionsfilter anstelle eines meldungshandlers.</span><span class="sxs-lookup"><span data-stu-id="700c5-175">If the API key applies only to certain controller actions, consider using an action filter instead of a message handler.</span></span> <span data-ttu-id="700c5-176">Aktionsfilter führen Sie nach dem URI routing ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="700c5-176">Action filters run after URI routing is performed.</span></span>


## <a name="per-route-message-handlers"></a><span data-ttu-id="700c5-177">Pro-Route-Meldungshandler</span><span class="sxs-lookup"><span data-stu-id="700c5-177">Per-Route Message Handlers</span></span>

<span data-ttu-id="700c5-178">Handler in der **HttpConfiguration.MessageHandlers** Auflistung global angewendet.</span><span class="sxs-lookup"><span data-stu-id="700c5-178">Handlers in the **HttpConfiguration.MessageHandlers** collection apply globally.</span></span>

<span data-ttu-id="700c5-179">Alternativ können Sie einen Meldungshandler um eine Route hinzufügen, wenn Sie die Route definieren:</span><span class="sxs-lookup"><span data-stu-id="700c5-179">Alternatively, you can add a message handler to a specific route when you define the route:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample11.cs?highlight=16)]

<span data-ttu-id="700c5-180">In diesem Beispiel, wenn der Anforderungs-URI "Route2", entspricht die Anforderung wird an gesendet `MessageHandler2`.</span><span class="sxs-lookup"><span data-stu-id="700c5-180">In this example, if the request URI matches "Route2", the request is dispatched to `MessageHandler2`.</span></span> <span data-ttu-id="700c5-181">Das folgende Diagramm zeigt die Pipeline für diese beiden Routen:</span><span class="sxs-lookup"><span data-stu-id="700c5-181">The following diagram shows the pipeline for these two routes:</span></span>

![](http-message-handlers/_static/image4.png)

<span data-ttu-id="700c5-182">Beachten Sie, dass `MessageHandler2` ersetzt die standardmäßige **HttpControllerDispatcher**.</span><span class="sxs-lookup"><span data-stu-id="700c5-182">Notice that `MessageHandler2` replaces the default **HttpControllerDispatcher**.</span></span> <span data-ttu-id="700c5-183">In diesem Beispiel `MessageHandler2` erstellt die Antwort und passende "Route2" Gehen Sie niemals auf einen Domänencontroller anfordert.</span><span class="sxs-lookup"><span data-stu-id="700c5-183">In this example, `MessageHandler2` creates the response, and requests that match "Route2" never go to a controller.</span></span> <span data-ttu-id="700c5-184">Dadurch können Sie den gesamten Mechanismus der Web-API-Controller mit Ihren eigenen benutzerdefinierten Endpunkt zu ersetzen.</span><span class="sxs-lookup"><span data-stu-id="700c5-184">This lets you replace the entire Web API controller mechanism with your own custom endpoint.</span></span>

<span data-ttu-id="700c5-185">Alternativ können Sie ein Meldungshandler pro Route an delegieren kann **HttpControllerDispatcher**, die dann an einen Controller verteilt.</span><span class="sxs-lookup"><span data-stu-id="700c5-185">Alternatively, a per-route message handler can delegate to **HttpControllerDispatcher**, which then dispatches to a controller.</span></span>

![](http-message-handlers/_static/image5.png)

<span data-ttu-id="700c5-186">Der folgende Code zeigt, wie Sie diese Route konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="700c5-186">The following code shows how to configure this route:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample12.cs)]
