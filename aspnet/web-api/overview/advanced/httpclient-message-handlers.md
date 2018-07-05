---
uid: web-api/overview/advanced/httpclient-message-handlers
title: HttpClient-Meldungshandler in der ASP.NET Web-API | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 10/01/2012
ms.assetid: 5a4b6c80-b2e9-4710-8969-d5076f7f82b8
msc.legacyurl: /web-api/overview/advanced/httpclient-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: db9edcf4fb31e967c3d4e7635f96c68829aec97d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37831371"
---
<a name="httpclient-message-handlers-in-aspnet-web-api"></a><span data-ttu-id="cce90-102">HttpClient-Meldungshandler in der ASP.NET Web-API</span><span class="sxs-lookup"><span data-stu-id="cce90-102">HttpClient Message Handlers in ASP.NET Web API</span></span>
====================
<span data-ttu-id="cce90-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="cce90-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="cce90-104">Ein *Meldungshandler* ist eine Klasse, die eine HTTP-Anforderung empfängt, und gibt eine HTTP-Antwort zurück.</span><span class="sxs-lookup"><span data-stu-id="cce90-104">A *message handler* is a class that receives an HTTP request and returns an HTTP response.</span></span>

<span data-ttu-id="cce90-105">In der Regel eine Reihe von Meldungshandler verkettet werden.</span><span class="sxs-lookup"><span data-stu-id="cce90-105">Typically, a series of message handlers are chained together.</span></span> <span data-ttu-id="cce90-106">Der erste Handler eine HTTP-Anforderung empfängt, erfolgt die Verarbeitung und gibt die Anforderung an dem nächsten Handler.</span><span class="sxs-lookup"><span data-stu-id="cce90-106">The first handler receives an HTTP request, does some processing, and gives the request to the next handler.</span></span> <span data-ttu-id="cce90-107">An einem bestimmten Punkt wird die Antwort wird erstellt und wird wieder die Kette.</span><span class="sxs-lookup"><span data-stu-id="cce90-107">At some point, the response is created and goes back up the chain.</span></span> <span data-ttu-id="cce90-108">Dieses Muster wird aufgerufen, eine *Delegieren* Handler.</span><span class="sxs-lookup"><span data-stu-id="cce90-108">This pattern is called a *delegating* handler.</span></span>

![](httpclient-message-handlers/_static/image1.png)

<span data-ttu-id="cce90-109">Klicken Sie auf der Clientseite der **"HttpClient"** Klasse einen Meldungshandler verwendet, um Anforderungen zu verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="cce90-109">On the client side, the **HttpClient** class uses a message handler to process requests.</span></span> <span data-ttu-id="cce90-110">Der Standardhandler **HttpClientHandler**, die die Anforderung sendet, über das Netzwerk und die Antwort vom Server abgerufen.</span><span class="sxs-lookup"><span data-stu-id="cce90-110">The default handler is **HttpClientHandler**, which sends the request over the network and gets the response from the server.</span></span> <span data-ttu-id="cce90-111">Sie können benutzerdefinierte Meldungshandler in der Client-Pipeline einfügen:</span><span class="sxs-lookup"><span data-stu-id="cce90-111">You can insert custom message handlers into the client pipeline:</span></span>

![](httpclient-message-handlers/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="cce90-112">ASP.NET Web-API verwendet Meldungshandler auch auf dem Server.</span><span class="sxs-lookup"><span data-stu-id="cce90-112">ASP.NET Web API also uses message handlers on the server side.</span></span> <span data-ttu-id="cce90-113">Weitere Informationen finden Sie unter [HTTP-Meldungshandler](http-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="cce90-113">For more information, see [HTTP Message Handlers](http-message-handlers.md).</span></span>


## <a name="custom-message-handlers"></a><span data-ttu-id="cce90-114">Benutzerdefinierte Meldungshandler</span><span class="sxs-lookup"><span data-stu-id="cce90-114">Custom Message Handlers</span></span>

<span data-ttu-id="cce90-115">Zum Schreiben von eines benutzerdefinierten Nachrichtenhandler abgeleitet **System.Net.Http.DelegatingHandler** und überschreiben die **SendAsync** Methode.</span><span class="sxs-lookup"><span data-stu-id="cce90-115">To write a custom message handler, derive from **System.Net.Http.DelegatingHandler** and override the **SendAsync** method.</span></span> <span data-ttu-id="cce90-116">So sieht die Signatur der Methode aus:</span><span class="sxs-lookup"><span data-stu-id="cce90-116">Here is the method signature:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample1.cs)]

<span data-ttu-id="cce90-117">Die Methode akzeptiert eine **HttpRequestMessage** als Eingabe und asynchron gibt ein **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="cce90-117">The method takes an **HttpRequestMessage** as input and asynchronously returns an **HttpResponseMessage**.</span></span> <span data-ttu-id="cce90-118">Eine typische Implementierung führt Folgendes aus:</span><span class="sxs-lookup"><span data-stu-id="cce90-118">A typical implementation does the following:</span></span>

1. <span data-ttu-id="cce90-119">Verarbeitet die Anforderungsnachricht an.</span><span class="sxs-lookup"><span data-stu-id="cce90-119">Process the request message.</span></span>
2. <span data-ttu-id="cce90-120">Rufen Sie `base.SendAsync` zum Senden der Anforderung an den inneren Handler.</span><span class="sxs-lookup"><span data-stu-id="cce90-120">Call `base.SendAsync` to send the request to the inner handler.</span></span>
3. <span data-ttu-id="cce90-121">Der innere Handler gibt eine Antwortnachricht zurück.</span><span class="sxs-lookup"><span data-stu-id="cce90-121">The inner handler returns a response message.</span></span> <span data-ttu-id="cce90-122">(Dieser Schritt ist asynchron.)</span><span class="sxs-lookup"><span data-stu-id="cce90-122">(This step is asynchronous.)</span></span>
4. <span data-ttu-id="cce90-123">Die Antwort zu verarbeiten und an den Aufrufer zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="cce90-123">Process the response and return it to the caller.</span></span>

<span data-ttu-id="cce90-124">Das folgende Beispiel zeigt einen Meldungshandler, der einen benutzerdefinierten Header zu einer ausgehenden Anforderung hinzufügt:</span><span class="sxs-lookup"><span data-stu-id="cce90-124">The following example shows a message handler that adds a custom header to the outgoing request:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample2.cs)]

<span data-ttu-id="cce90-125">Der Aufruf von `base.SendAsync` ist asynchron.</span><span class="sxs-lookup"><span data-stu-id="cce90-125">The call to `base.SendAsync` is asynchronous.</span></span> <span data-ttu-id="cce90-126">Wenn der Handler für alle Vorgänge nach dem Aufruf ausführt, verwenden Sie die **"await"** Schlüsselwort zum Fortsetzen der Ausführung nach Abschluss der Methode.</span><span class="sxs-lookup"><span data-stu-id="cce90-126">If the handler does any work after this call, use the **await** keyword to resume execution after the method completes.</span></span> <span data-ttu-id="cce90-127">Das folgende Beispiel zeigt einen Handler, der Fehlercode protokolliert.</span><span class="sxs-lookup"><span data-stu-id="cce90-127">The following example shows a handler that logs error codes.</span></span> <span data-ttu-id="cce90-128">Die Protokollierung selbst ist nicht besonders interessant, aber das Beispiel veranschaulicht, an die Antwort innerhalb des Handlers abzurufen.</span><span class="sxs-lookup"><span data-stu-id="cce90-128">The logging itself is not very interesting, but the example shows how to get at the response inside the handler.</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample3.cs?highlight=10,13)]

## <a name="adding-message-handlers-to-the-client-pipeline"></a><span data-ttu-id="cce90-129">Hinzufügen von Meldungshandlern für die Client-Pipeline</span><span class="sxs-lookup"><span data-stu-id="cce90-129">Adding Message Handlers to the Client Pipeline</span></span>

<span data-ttu-id="cce90-130">Hinzufügen von benutzerdefinierten Handler zum **"HttpClient"**, verwenden Sie die **HttpClientFactory.Create** Methode:</span><span class="sxs-lookup"><span data-stu-id="cce90-130">To add custom handlers to **HttpClient**, use the **HttpClientFactory.Create** method:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample4.cs)]

<span data-ttu-id="cce90-131">Message-Ereignishandler werden aufgerufen, in der Reihenfolge, die übergeben werden sie in der **erstellen** Methode.</span><span class="sxs-lookup"><span data-stu-id="cce90-131">Message handlers are called in the order that you pass them into the **Create** method.</span></span> <span data-ttu-id="cce90-132">Da der Handler geschachtelt sind, wird die Response-Nachricht in die andere Richtung übertragen.</span><span class="sxs-lookup"><span data-stu-id="cce90-132">Because handlers are nested, the response message travels in the other direction.</span></span> <span data-ttu-id="cce90-133">Der letzte Handler ist, also den ersten, die Response-Nachricht.</span><span class="sxs-lookup"><span data-stu-id="cce90-133">That is, the last handler is the first to get the response message.</span></span>
