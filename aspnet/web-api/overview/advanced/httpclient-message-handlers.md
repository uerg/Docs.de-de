---
uid: web-api/overview/advanced/httpclient-message-handlers
title: Meldungshandler für "HttpClient" in ASP.NET Web-API | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2012
ms.topic: article
ms.assetid: 5a4b6c80-b2e9-4710-8969-d5076f7f82b8
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/httpclient-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 805741b0ac682b7479ce82127df48b1b9a49a427
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
ms.locfileid: "26506789"
---
<a name="httpclient-message-handlers-in-aspnet-web-api"></a><span data-ttu-id="b0986-102">Meldungshandler für "HttpClient" in ASP.NET Web-API</span><span class="sxs-lookup"><span data-stu-id="b0986-102">HttpClient Message Handlers in ASP.NET Web API</span></span>
====================
<span data-ttu-id="b0986-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="b0986-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="b0986-104">Ein *Nachrichtenhandler* ist eine Klasse, empfängt eine HTTP-Anforderung und gibt eine HTTP-Antwort zurück.</span><span class="sxs-lookup"><span data-stu-id="b0986-104">A *message handler* is a class that receives an HTTP request and returns an HTTP response.</span></span>

<span data-ttu-id="b0986-105">In der Regel eine Reihe von Meldungshandler verkettet werden.</span><span class="sxs-lookup"><span data-stu-id="b0986-105">Typically, a series of message handlers are chained together.</span></span> <span data-ttu-id="b0986-106">Der erste Handler empfängt eine HTTP-Anforderung, Verarbeitung und die Anforderung an dem nächsten Handler ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="b0986-106">The first handler receives an HTTP request, does some processing, and gives the request to the next handler.</span></span> <span data-ttu-id="b0986-107">Die Antwort wird erstellt und herabgesetzt aufwärts in der Kette, zu einem späteren Zeitpunkt.</span><span class="sxs-lookup"><span data-stu-id="b0986-107">At some point, the response is created and goes back up the chain.</span></span> <span data-ttu-id="b0986-108">Dieses Muster wird aufgerufen, eine *Delegieren* Handler.</span><span class="sxs-lookup"><span data-stu-id="b0986-108">This pattern is called a *delegating* handler.</span></span>

![](httpclient-message-handlers/_static/image1.png)

<span data-ttu-id="b0986-109">Auf der Clientseite der **"HttpClient"** Klasse einen Meldungshandler verwendet, um Anforderungen zu verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="b0986-109">On the client side, the **HttpClient** class uses a message handler to process requests.</span></span> <span data-ttu-id="b0986-110">Der Standardhandler ist **HttpClientHandler**, die die Anforderung über das Netzwerk sendet und ruft die Antwort vom Server ab.</span><span class="sxs-lookup"><span data-stu-id="b0986-110">The default handler is **HttpClientHandler**, which sends the request over the network and gets the response from the server.</span></span> <span data-ttu-id="b0986-111">Sie können benutzerdefinierte Meldungshandler in die Client-Pipeline einfügen:</span><span class="sxs-lookup"><span data-stu-id="b0986-111">You can insert custom message handlers into the client pipeline:</span></span>

![](httpclient-message-handlers/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="b0986-112">ASP.NET Web-API verwendet Meldungshandler auch auf der Serverseite.</span><span class="sxs-lookup"><span data-stu-id="b0986-112">ASP.NET Web API also uses message handlers on the server side.</span></span> <span data-ttu-id="b0986-113">Weitere Informationen finden Sie unter [HTTP-Meldungshandler](http-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="b0986-113">For more information, see [HTTP Message Handlers](http-message-handlers.md).</span></span>


## <a name="custom-message-handlers"></a><span data-ttu-id="b0986-114">Benutzerdefinierte Meldungshandler</span><span class="sxs-lookup"><span data-stu-id="b0986-114">Custom Message Handlers</span></span>

<span data-ttu-id="b0986-115">Leiten Sie zum Schreiben eines benutzerdefinierten Message-Handlers aus **System.Net.Http.DelegatingHandler** und überschreiben die **"SendAsync"** Methode.</span><span class="sxs-lookup"><span data-stu-id="b0986-115">To write a custom message handler, derive from **System.Net.Http.DelegatingHandler** and override the **SendAsync** method.</span></span> <span data-ttu-id="b0986-116">Hier wird die Signatur der Methode:</span><span class="sxs-lookup"><span data-stu-id="b0986-116">Here is the method signature:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample1.cs)]

<span data-ttu-id="b0986-117">Die Methode nimmt ein **HttpRequestMessage** als Eingabe und asynchron gibt eine **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="b0986-117">The method takes an **HttpRequestMessage** as input and asynchronously returns an **HttpResponseMessage**.</span></span> <span data-ttu-id="b0986-118">Eine typische Implementierung führt Folgendes aus:</span><span class="sxs-lookup"><span data-stu-id="b0986-118">A typical implementation does the following:</span></span>

1. <span data-ttu-id="b0986-119">Verarbeitet die Anforderungsnachricht an.</span><span class="sxs-lookup"><span data-stu-id="b0986-119">Process the request message.</span></span>
2. <span data-ttu-id="b0986-120">Rufen Sie `base.SendAsync` zum Senden der Anforderung an den internen Handler.</span><span class="sxs-lookup"><span data-stu-id="b0986-120">Call `base.SendAsync` to send the request to the inner handler.</span></span>
3. <span data-ttu-id="b0986-121">Der innere Handler gibt eine Antwortnachricht zurück.</span><span class="sxs-lookup"><span data-stu-id="b0986-121">The inner handler returns a response message.</span></span> <span data-ttu-id="b0986-122">(Dieser Schritt ist asynchron.)</span><span class="sxs-lookup"><span data-stu-id="b0986-122">(This step is asynchronous.)</span></span>
4. <span data-ttu-id="b0986-123">Verarbeiten der Antwort, und geben sie an den Aufrufer zurück.</span><span class="sxs-lookup"><span data-stu-id="b0986-123">Process the response and return it to the caller.</span></span>

<span data-ttu-id="b0986-124">Das folgende Beispiel zeigt einen Message-Handler, der einen benutzerdefinierten Header der ausgehenden Anforderung hinzufügt:</span><span class="sxs-lookup"><span data-stu-id="b0986-124">The following example shows a message handler that adds a custom header to the outgoing request:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample2.cs)]

<span data-ttu-id="b0986-125">Der Aufruf von `base.SendAsync` asynchron ist.</span><span class="sxs-lookup"><span data-stu-id="b0986-125">The call to `base.SendAsync` is asynchronous.</span></span> <span data-ttu-id="b0986-126">Wenn der Handler nach diesem Aufruf Arbeit ausführt, verwenden Sie die **"await"** Schlüsselwort zum Fortsetzen der Ausführung nach Abschluss der Methode.</span><span class="sxs-lookup"><span data-stu-id="b0986-126">If the handler does any work after this call, use the **await** keyword to resume execution after the method completes.</span></span> <span data-ttu-id="b0986-127">Im folgenden Beispiel wird eines Handlers, der Fehlercodes protokolliert.</span><span class="sxs-lookup"><span data-stu-id="b0986-127">The following example shows a handler that logs error codes.</span></span> <span data-ttu-id="b0986-128">Die Protokollierung selbst ist nicht besonders interessant, aber im Beispiel veranschaulicht, an die Antwort im Handler abzurufen.</span><span class="sxs-lookup"><span data-stu-id="b0986-128">The logging itself is not very interesting, but the example shows how to get at the response inside the handler.</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample3.cs?highlight=10,13)]

## <a name="adding-message-handlers-to-the-client-pipeline"></a><span data-ttu-id="b0986-129">Hinzufügen von Meldungshandlern an die Client-Pipeline</span><span class="sxs-lookup"><span data-stu-id="b0986-129">Adding Message Handlers to the Client Pipeline</span></span>

<span data-ttu-id="b0986-130">Hinzufügen von benutzerdefinierten Handler **"HttpClient"**, verwenden Sie die **HttpClientFactory.Create** Methode:</span><span class="sxs-lookup"><span data-stu-id="b0986-130">To add custom handlers to **HttpClient**, use the **HttpClientFactory.Create** method:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample4.cs)]

<span data-ttu-id="b0986-131">Message-Ereignishandler werden aufgerufen, in der Reihenfolge ab, das Sie Sie sie in übergeben der **erstellen** Methode.</span><span class="sxs-lookup"><span data-stu-id="b0986-131">Message handlers are called in the order that you pass them into the **Create** method.</span></span> <span data-ttu-id="b0986-132">Da der Handler geschachtelt sind, durchläuft die Antwortnachricht in die andere Richtung.</span><span class="sxs-lookup"><span data-stu-id="b0986-132">Because handlers are nested, the response message travels in the other direction.</span></span> <span data-ttu-id="b0986-133">Der letzte Handler ist, also zuerst die Antwortnachricht.</span><span class="sxs-lookup"><span data-stu-id="b0986-133">That is, the last handler is the first to get the response message.</span></span>
