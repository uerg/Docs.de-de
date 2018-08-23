---
uid: web-api/overview/advanced/http-message-handlers
title: HTTP-Meldungshandler in der ASP.NET Web-API | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: riande
ms.date: 02/13/2012
ms.assetid: 9002018b-3aa3-4358-bb1c-fbb5bc751d01
msc.legacyurl: /web-api/overview/advanced/http-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 0b0d7b4c543dc4e597c6c472083898f3a8095a83
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835842"
---
<a name="http-message-handlers-in-aspnet-web-api"></a><span data-ttu-id="96c6d-102">HTTP-Meldungshandler in der ASP.NET Web-API</span><span class="sxs-lookup"><span data-stu-id="96c6d-102">HTTP Message Handlers in ASP.NET Web API</span></span>
====================
<span data-ttu-id="96c6d-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="96c6d-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="96c6d-104">Ein *Meldungshandler* ist eine Klasse, die eine HTTP-Anforderung empfängt, und gibt eine HTTP-Antwort zurück.</span><span class="sxs-lookup"><span data-stu-id="96c6d-104">A *message handler* is a class that receives an HTTP request and returns an HTTP response.</span></span> <span data-ttu-id="96c6d-105">Meldungshandler abgeleitet werden, von der abstrakten **HttpMessageHandler** Klasse.</span><span class="sxs-lookup"><span data-stu-id="96c6d-105">Message handlers derive from the abstract **HttpMessageHandler** class.</span></span>

<span data-ttu-id="96c6d-106">In der Regel eine Reihe von Meldungshandler verkettet werden.</span><span class="sxs-lookup"><span data-stu-id="96c6d-106">Typically, a series of message handlers are chained together.</span></span> <span data-ttu-id="96c6d-107">Der erste Handler eine HTTP-Anforderung empfängt, erfolgt die Verarbeitung und gibt die Anforderung an dem nächsten Handler.</span><span class="sxs-lookup"><span data-stu-id="96c6d-107">The first handler receives an HTTP request, does some processing, and gives the request to the next handler.</span></span> <span data-ttu-id="96c6d-108">An einem bestimmten Punkt wird die Antwort wird erstellt und wird wieder die Kette.</span><span class="sxs-lookup"><span data-stu-id="96c6d-108">At some point, the response is created and goes back up the chain.</span></span> <span data-ttu-id="96c6d-109">Dieses Muster wird aufgerufen, eine *Delegieren* Handler.</span><span class="sxs-lookup"><span data-stu-id="96c6d-109">This pattern is called a *delegating* handler.</span></span>

![](http-message-handlers/_static/image1.png)

## <a name="server-side-message-handlers"></a><span data-ttu-id="96c6d-110">Serverseitige-Meldungshandler</span><span class="sxs-lookup"><span data-stu-id="96c6d-110">Server-Side Message Handlers</span></span>

<span data-ttu-id="96c6d-111">Auf der Serverseite verwendet die Web-API-Pipeline einige integrierte-Meldungshandler:</span><span class="sxs-lookup"><span data-stu-id="96c6d-111">On the server side, the Web API pipeline uses some built-in message handlers:</span></span>

- <span data-ttu-id="96c6d-112">**HttpServer** vom Host der Anforderung ab.</span><span class="sxs-lookup"><span data-stu-id="96c6d-112">**HttpServer** gets the request from the host.</span></span>
- <span data-ttu-id="96c6d-113">**HttpRoutingDispatcher** sendet die Anforderung auf Grundlage der Route.</span><span class="sxs-lookup"><span data-stu-id="96c6d-113">**HttpRoutingDispatcher** dispatches the request based on the route.</span></span>
- <span data-ttu-id="96c6d-114">**HttpControllerDispatcher** sendet die Anforderung an einen Web-API-Controller.</span><span class="sxs-lookup"><span data-stu-id="96c6d-114">**HttpControllerDispatcher** sends the request to a Web API controller.</span></span>

<span data-ttu-id="96c6d-115">Sie können benutzerdefinierte Handler zur Pipeline hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="96c6d-115">You can add custom handlers to the pipeline.</span></span> <span data-ttu-id="96c6d-116">Nachrichtenhandler sind gut für die übergreifende Probleme, die auf der Ebene der HTTP-Nachrichten (statt über Controlleraktionen) ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="96c6d-116">Message handlers are good for cross-cutting concerns that operate at the level of HTTP messages (rather than controller actions).</span></span> <span data-ttu-id="96c6d-117">Beispielsweise können ein Nachrichtenhandler:</span><span class="sxs-lookup"><span data-stu-id="96c6d-117">For example, a message handler might:</span></span>

- <span data-ttu-id="96c6d-118">Lesen oder Ändern von Anforderungsheadern.</span><span class="sxs-lookup"><span data-stu-id="96c6d-118">Read or modify request headers.</span></span>
- <span data-ttu-id="96c6d-119">Hinzufügen eines Antwortheaders zu antworten.</span><span class="sxs-lookup"><span data-stu-id="96c6d-119">Add a response header to responses.</span></span>
- <span data-ttu-id="96c6d-120">Überprüfen Sie Anforderungen, bevor sie den Controller erreichen.</span><span class="sxs-lookup"><span data-stu-id="96c6d-120">Validate requests before they reach the controller.</span></span>

<span data-ttu-id="96c6d-121">Dieses Diagramm zeigt zwei benutzerdefinierte Handler, die in der Pipeline eingefügt:</span><span class="sxs-lookup"><span data-stu-id="96c6d-121">This diagram shows two custom handlers inserted into the pipeline:</span></span>

![](http-message-handlers/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="96c6d-122">Klicken Sie auf der Clientseite verwendet "HttpClient" auch Meldungshandler.</span><span class="sxs-lookup"><span data-stu-id="96c6d-122">On the client side, HttpClient also uses message handlers.</span></span> <span data-ttu-id="96c6d-123">Weitere Informationen finden Sie unter [HttpClient-Meldungshandler](httpclient-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="96c6d-123">For more information, see [HttpClient Message Handlers](httpclient-message-handlers.md).</span></span>


## <a name="custom-message-handlers"></a><span data-ttu-id="96c6d-124">Benutzerdefinierte Meldungshandler</span><span class="sxs-lookup"><span data-stu-id="96c6d-124">Custom Message Handlers</span></span>

<span data-ttu-id="96c6d-125">Zum Schreiben von eines benutzerdefinierten Nachrichtenhandler abgeleitet **System.Net.Http.DelegatingHandler** und überschreiben die **SendAsync** Methode.</span><span class="sxs-lookup"><span data-stu-id="96c6d-125">To write a custom message handler, derive from **System.Net.Http.DelegatingHandler** and override the **SendAsync** method.</span></span> <span data-ttu-id="96c6d-126">Diese Methode hat die folgende Signatur:</span><span class="sxs-lookup"><span data-stu-id="96c6d-126">This method has the following signature:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample1.cs)]

<span data-ttu-id="96c6d-127">Die Methode akzeptiert eine **HttpRequestMessage** als Eingabe und asynchron gibt ein **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="96c6d-127">The method takes an **HttpRequestMessage** as input and asynchronously returns an **HttpResponseMessage**.</span></span> <span data-ttu-id="96c6d-128">Eine typische Implementierung führt Folgendes aus:</span><span class="sxs-lookup"><span data-stu-id="96c6d-128">A typical implementation does the following:</span></span>

1. <span data-ttu-id="96c6d-129">Verarbeitet die Anforderungsnachricht an.</span><span class="sxs-lookup"><span data-stu-id="96c6d-129">Process the request message.</span></span>
2. <span data-ttu-id="96c6d-130">Rufen Sie `base.SendAsync` zum Senden der Anforderung an den inneren Handler.</span><span class="sxs-lookup"><span data-stu-id="96c6d-130">Call `base.SendAsync` to send the request to the inner handler.</span></span>
3. <span data-ttu-id="96c6d-131">Der innere Handler gibt eine Antwortnachricht zurück.</span><span class="sxs-lookup"><span data-stu-id="96c6d-131">The inner handler returns a response message.</span></span> <span data-ttu-id="96c6d-132">(Dieser Schritt ist asynchron.)</span><span class="sxs-lookup"><span data-stu-id="96c6d-132">(This step is asynchronous.)</span></span>
4. <span data-ttu-id="96c6d-133">Die Antwort zu verarbeiten und an den Aufrufer zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="96c6d-133">Process the response and return it to the caller.</span></span>

<span data-ttu-id="96c6d-134">Hier ist ein sehr einfaches Beispiel:</span><span class="sxs-lookup"><span data-stu-id="96c6d-134">Here is a trivial example:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="96c6d-135">Der Aufruf von `base.SendAsync` ist asynchron.</span><span class="sxs-lookup"><span data-stu-id="96c6d-135">The call to `base.SendAsync` is asynchronous.</span></span> <span data-ttu-id="96c6d-136">Wenn der Handler für alle Vorgänge nach dem Aufruf ausführt, verwenden Sie die **"await"** -Schlüsselwort, wie gezeigt.</span><span class="sxs-lookup"><span data-stu-id="96c6d-136">If the handler does any work after this call, use the **await** keyword, as shown.</span></span>


<span data-ttu-id="96c6d-137">Ein delegierender Handler kann den inneren Handler auch überspringen und direkt die Antwort zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="96c6d-137">A delegating handler can also skip the inner handler and directly create the response:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample3.cs)]

<span data-ttu-id="96c6d-138">Wenn eine Delegierung Ereignishandler erstellt die Antwort ohne `base.SendAsync`, die Anforderung wird übersprungen, den Rest der Pipeline.</span><span class="sxs-lookup"><span data-stu-id="96c6d-138">If a delegating handler creates the response without calling `base.SendAsync`, the request skips the rest of the pipeline.</span></span> <span data-ttu-id="96c6d-139">Dies kann nach einem Handler nützlich sein, der die Anforderung (erstellen eine Fehlerantwort) überprüft.</span><span class="sxs-lookup"><span data-stu-id="96c6d-139">This can be useful for a handler that validates the request (creating an error response).</span></span>

![](http-message-handlers/_static/image3.png)

## <a name="adding-a-handler-to-the-pipeline"></a><span data-ttu-id="96c6d-140">Hinzufügen eines Handlers für die Pipeline</span><span class="sxs-lookup"><span data-stu-id="96c6d-140">Adding a Handler to the Pipeline</span></span>

<span data-ttu-id="96c6d-141">Um einen Meldungshandler auf dem Server hinzuzufügen, fügen Sie den Handler, der die **HttpConfiguration.MessageHandlers** Auflistung.</span><span class="sxs-lookup"><span data-stu-id="96c6d-141">To add a message handler on the server side, add the handler to the **HttpConfiguration.MessageHandlers** collection.</span></span> <span data-ttu-id="96c6d-142">Wenn Sie die Vorlage "ASP.NET MVC 4-Webanwendung" verwendet, um das Projekt zu erstellen, erreichen Sie dies in der **WebApiConfig** Klasse:</span><span class="sxs-lookup"><span data-stu-id="96c6d-142">If you used the "ASP.NET MVC 4 Web Application" template to create the project, you can do this inside the **WebApiConfig** class:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample4.cs)]

<span data-ttu-id="96c6d-143">Message-Ereignishandler werden aufgerufen, in der gleichen Reihenfolge, die sie in angezeigt werden **MessageHandlers** Auflistung.</span><span class="sxs-lookup"><span data-stu-id="96c6d-143">Message handlers are called in the same order that they appear in **MessageHandlers** collection.</span></span> <span data-ttu-id="96c6d-144">Da sie geschachtelt sind, wird die Response-Nachricht in die andere Richtung übertragen.</span><span class="sxs-lookup"><span data-stu-id="96c6d-144">Because they are nested, the response message travels in the other direction.</span></span> <span data-ttu-id="96c6d-145">Der letzte Handler ist, also den ersten, die Response-Nachricht.</span><span class="sxs-lookup"><span data-stu-id="96c6d-145">That is, the last handler is the first to get the response message.</span></span>

<span data-ttu-id="96c6d-146">Beachten Sie, dass Sie nicht der internen Handler festlegen müssen. die Web-API-Framework wird automatisch der Meldungshandler hergestellt.</span><span class="sxs-lookup"><span data-stu-id="96c6d-146">Notice that you don't need to set the inner handlers; the Web API framework automatically connects the message handlers.</span></span>

<span data-ttu-id="96c6d-147">Möchten [Selbsthosting](../older-versions/self-host-a-web-api.md), erstellen Sie eine Instanz von der **HttpSelfHostConfiguration** -Klasse und die Handler zum Hinzufügen der **MessageHandlers** Auflistung.</span><span class="sxs-lookup"><span data-stu-id="96c6d-147">If you are [self-hosting](../older-versions/self-host-a-web-api.md), create an instance of the **HttpSelfHostConfiguration** class and add the handlers to the **MessageHandlers** collection.</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample5.cs)]

<span data-ttu-id="96c6d-148">Jetzt sehen wir uns einige Beispiele für benutzerdefinierte Meldungshandler.</span><span class="sxs-lookup"><span data-stu-id="96c6d-148">Now let's look at some examples of custom message handlers.</span></span>

## <a name="example-x-http-method-override"></a><span data-ttu-id="96c6d-149">Beispiel: X-HTTP-Method-Override-</span><span class="sxs-lookup"><span data-stu-id="96c6d-149">Example: X-HTTP-Method-Override</span></span>

<span data-ttu-id="96c6d-150">X-HTTP-Method-Override ist einem nicht standardmäßigen HTTP-Header.</span><span class="sxs-lookup"><span data-stu-id="96c6d-150">X-HTTP-Method-Override is a non-standard HTTP header.</span></span> <span data-ttu-id="96c6d-151">Es dient für Clients, die bestimmte HTTP-Anforderungstypen, z. B. PUT oder DELETE senden können.</span><span class="sxs-lookup"><span data-stu-id="96c6d-151">It is designed for clients that cannot send certain HTTP request types, such as PUT or DELETE.</span></span> <span data-ttu-id="96c6d-152">Stattdessen wird der Client sendet eine POST-Anforderung, und legt den X-HTTP-Method-Override-Header auf die gewünschte Methode.</span><span class="sxs-lookup"><span data-stu-id="96c6d-152">Instead, the client sends a POST request and sets the X-HTTP-Method-Override header to the desired method.</span></span> <span data-ttu-id="96c6d-153">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="96c6d-153">For example:</span></span>

[!code-console[Main](http-message-handlers/samples/sample6.cmd)]

<span data-ttu-id="96c6d-154">Hier ist ein Meldungshandler, das Unterstützung für X-HTTP-Method-Override aus:</span><span class="sxs-lookup"><span data-stu-id="96c6d-154">Here is a message handler that adds support for X-HTTP-Method-Override:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample7.cs)]

<span data-ttu-id="96c6d-155">In der **SendAsync** -Methode, der Handler überprüft wird, gibt an, ob die Anforderungsnachricht für eine POST-Anforderung ist, und ob es sich um den X-HTTP-Method-Override-Header enthält.</span><span class="sxs-lookup"><span data-stu-id="96c6d-155">In the **SendAsync** method, the handler checks whether the request message is a POST request, and whether it contains the X-HTTP-Method-Override header.</span></span> <span data-ttu-id="96c6d-156">Wenn dies der Fall ist, überprüft den Wert von Header und ändert anschließend die Anforderungsmethode.</span><span class="sxs-lookup"><span data-stu-id="96c6d-156">If so, it validates the header value, and then modifies the request method.</span></span> <span data-ttu-id="96c6d-157">Der Handler zum Schluss ruft `base.SendAsync` die Nachricht an den nächsten Handler übergeben.</span><span class="sxs-lookup"><span data-stu-id="96c6d-157">Finally, the handler calls `base.SendAsync` to pass the message to the next handler.</span></span>

<span data-ttu-id="96c6d-158">Wenn die Anforderung erreicht den **HttpControllerDispatcher** -Klasse, **HttpControllerDispatcher** leitet die Anforderung, die basierend auf dem die aktualisierte Anforderungsmethode.</span><span class="sxs-lookup"><span data-stu-id="96c6d-158">When the request reaches the **HttpControllerDispatcher** class, **HttpControllerDispatcher** will route the request based on the updated request method.</span></span>

## <a name="example-adding-a-custom-response-header"></a><span data-ttu-id="96c6d-159">Beispiel: Hinzufügen eines benutzerdefinierten Antwortheaders</span><span class="sxs-lookup"><span data-stu-id="96c6d-159">Example: Adding a Custom Response Header</span></span>

<span data-ttu-id="96c6d-160">Hier ist ein Message-Handler, der einen benutzerdefinierten Header zu jeder Response-Nachricht hinzufügt:</span><span class="sxs-lookup"><span data-stu-id="96c6d-160">Here is a message handler that adds a custom header to every response message:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample8.cs)]

<span data-ttu-id="96c6d-161">Zunächst ruft der Handler `base.SendAsync` die Anforderung an den inneren Meldungshandler übergeben.</span><span class="sxs-lookup"><span data-stu-id="96c6d-161">First, the handler calls `base.SendAsync` to pass the request to the inner message handler.</span></span> <span data-ttu-id="96c6d-162">Der innere Handler gibt eine Antwortnachricht zurück, sondern nur asynchron mit einem **Aufgabe&lt;T&gt;**  Objekt.</span><span class="sxs-lookup"><span data-stu-id="96c6d-162">The inner handler returns a response message, but it does so asynchronously using a **Task&lt;T&gt;** object.</span></span> <span data-ttu-id="96c6d-163">Die Response-Nachricht ist nicht verfügbar, bis `base.SendAsync` asynchron abgeschlossen.</span><span class="sxs-lookup"><span data-stu-id="96c6d-163">The response message is not available until `base.SendAsync` completes asynchronously.</span></span>

<span data-ttu-id="96c6d-164">Dieses Beispiel verwendet die **"await"** Schlüsselwort, um die Aufgaben asynchron nach `SendAsync` abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="96c6d-164">This example uses the **await** keyword to perform work asynchronously after `SendAsync` completes.</span></span> <span data-ttu-id="96c6d-165">Wenn Sie .NET Framework 4.0 ausgerichtet sind, verwenden Sie die **Aufgabe**&lt;T&gt;**. ContinueWith** Methode:</span><span class="sxs-lookup"><span data-stu-id="96c6d-165">If you are targeting .NET Framework 4.0, use the **Task**&lt;T&gt;**.ContinueWith** method:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample9.cs)]

## <a name="example-checking-for-an-api-key"></a><span data-ttu-id="96c6d-166">Beispiel: Überprüfen eines API-Schlüssels</span><span class="sxs-lookup"><span data-stu-id="96c6d-166">Example: Checking for an API Key</span></span>

<span data-ttu-id="96c6d-167">Einige Webdienste werden Clients für einen API-Schlüssel in der Anforderung angeben müssen.</span><span class="sxs-lookup"><span data-stu-id="96c6d-167">Some web services require clients to include an API key in their request.</span></span> <span data-ttu-id="96c6d-168">Das folgende Beispiel zeigt, wie ein Meldungshandler Anforderungen für einen gültigen API-Schlüssel überprüfen kann:</span><span class="sxs-lookup"><span data-stu-id="96c6d-168">The following example shows how a message handler can check requests for a valid API key:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample10.cs)]

<span data-ttu-id="96c6d-169">Dieser Handler sucht nach den API-Schlüssel in der URI-Abfragezeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="96c6d-169">This handler looks for the API key in the URI query string.</span></span> <span data-ttu-id="96c6d-170">(In diesem Beispiel wird davon ausgegangen, dass der Schlüssel eine statische Zeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="96c6d-170">(For this example, we assume that the key is a static string.</span></span> <span data-ttu-id="96c6d-171">"Eine realen Implementierung würden wahrscheinlich eine komplexere Validierung verwenden.) Wenn die Abfragezeichenfolge der Schlüssel enthält, übergibt der Handler für die Anforderung an den inneren Handler.</span><span class="sxs-lookup"><span data-stu-id="96c6d-171">A real implementation would probably use more complex validation.) If the query string contains the key, the handler passes the request to the inner handler.</span></span>

<span data-ttu-id="96c6d-172">Wenn die Anforderung nicht über einen gültigen Schlüssel verfügt, erstellt der Handler für eine Antwortnachricht mit dem Status 403, verboten.</span><span class="sxs-lookup"><span data-stu-id="96c6d-172">If the request does not have a valid key, the handler creates a response message with status 403, Forbidden.</span></span> <span data-ttu-id="96c6d-173">In diesem Fall ruft der Handler für nicht auf `base.SendAsync`, also der innere Handler empfängt die Anforderung, noch wird des Controllers.</span><span class="sxs-lookup"><span data-stu-id="96c6d-173">In this case, the handler does not call `base.SendAsync`, so the inner handler never receives the request, nor does the controller.</span></span> <span data-ttu-id="96c6d-174">Der Controller kann daher davon ausgehen, dass alle eingehende Anforderungen über einen gültigen API-Schlüssel verfügen.</span><span class="sxs-lookup"><span data-stu-id="96c6d-174">Therefore, the controller can assume that all incoming requests have a valid API key.</span></span>

> [!NOTE]
> <span data-ttu-id="96c6d-175">Wenn der API-Schlüssel nur für bestimmte Controlleraktionen gilt, erwägen Sie, die einen Aktionsfilter anstelle eines meldungshandlers.</span><span class="sxs-lookup"><span data-stu-id="96c6d-175">If the API key applies only to certain controller actions, consider using an action filter instead of a message handler.</span></span> <span data-ttu-id="96c6d-176">Aktionsfilter werden nach dem URI routing ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="96c6d-176">Action filters run after URI routing is performed.</span></span>


## <a name="per-route-message-handlers"></a><span data-ttu-id="96c6d-177">Pro-Route-Meldungshandler</span><span class="sxs-lookup"><span data-stu-id="96c6d-177">Per-Route Message Handlers</span></span>

<span data-ttu-id="96c6d-178">-Handler in der **HttpConfiguration.MessageHandlers** Auflistung global angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="96c6d-178">Handlers in the **HttpConfiguration.MessageHandlers** collection apply globally.</span></span>

<span data-ttu-id="96c6d-179">Alternativ können Sie einen Meldungshandler für eine bestimmte Route hinzufügen, wenn Sie die Route definieren:</span><span class="sxs-lookup"><span data-stu-id="96c6d-179">Alternatively, you can add a message handler to a specific route when you define the route:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample11.cs?highlight=16)]

<span data-ttu-id="96c6d-180">In diesem Beispiel, wenn der Anforderungs-URI "Route2", entspricht die Anforderung wird gesendet `MessageHandler2`.</span><span class="sxs-lookup"><span data-stu-id="96c6d-180">In this example, if the request URI matches "Route2", the request is dispatched to `MessageHandler2`.</span></span> <span data-ttu-id="96c6d-181">Das folgende Diagramm zeigt die Pipeline für diese zwei Routen:</span><span class="sxs-lookup"><span data-stu-id="96c6d-181">The following diagram shows the pipeline for these two routes:</span></span>

![](http-message-handlers/_static/image4.png)

<span data-ttu-id="96c6d-182">Beachten Sie, dass `MessageHandler2` ersetzt das standardmäßige **HttpControllerDispatcher**.</span><span class="sxs-lookup"><span data-stu-id="96c6d-182">Notice that `MessageHandler2` replaces the default **HttpControllerDispatcher**.</span></span> <span data-ttu-id="96c6d-183">In diesem Beispiel `MessageHandler2` erstellt die Antwort und -Anforderungen, die mit "Route2" Gehen Sie niemals auf einen Controller übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="96c6d-183">In this example, `MessageHandler2` creates the response, and requests that match "Route2" never go to a controller.</span></span> <span data-ttu-id="96c6d-184">Dadurch können Sie den gesamten Web-API-Controller-Mechanismus durch Ihren eigenen benutzerdefinierten Endpunkt zu ersetzen.</span><span class="sxs-lookup"><span data-stu-id="96c6d-184">This lets you replace the entire Web API controller mechanism with your own custom endpoint.</span></span>

<span data-ttu-id="96c6d-185">Alternativ kann ein Message-Handler pro Route an Delegieren **HttpControllerDispatcher**, die dann an einen Controller sendet.</span><span class="sxs-lookup"><span data-stu-id="96c6d-185">Alternatively, a per-route message handler can delegate to **HttpControllerDispatcher**, which then dispatches to a controller.</span></span>

![](http-message-handlers/_static/image5.png)

<span data-ttu-id="96c6d-186">Der folgende Code zeigt, wie Sie diese Route zu konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="96c6d-186">The following code shows how to configure this route:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample12.cs)]
