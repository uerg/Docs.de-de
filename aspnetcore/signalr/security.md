---
title: Überlegungen zur Sicherheit in ASP.NET Core SignalR
author: tdykstra
description: Erfahren Sie, wie Sie Authentifizierung und Autorisierung in ASP.NET Core SignalR verwenden.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 06/29/2018
uid: signalr/security
ms.openlocfilehash: b66c7fbfbaee4c70a68f3132875fbc81018c3e20
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095131"
---
# <a name="security-considerations-in-aspnet-core-signalr"></a><span data-ttu-id="bd41a-103">Überlegungen zur Sicherheit in ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="bd41a-103">Security considerations in ASP.NET Core SignalR</span></span>

<span data-ttu-id="bd41a-104">Durch [Andrew Stanton-Nurse](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="bd41a-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

## <a name="overview"></a><span data-ttu-id="bd41a-105">Übersicht</span><span class="sxs-lookup"><span data-stu-id="bd41a-105">Overview</span></span>

<span data-ttu-id="bd41a-106">SignalR stellt eine Anzahl von Sicherheitsmaßnahmen standardmäßig bereit.</span><span class="sxs-lookup"><span data-stu-id="bd41a-106">SignalR provides a number of security protections by default.</span></span> <span data-ttu-id="bd41a-107">Es ist wichtig zu verstehen, wie diese Schutzmaßnahmen zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="bd41a-107">It's important to understand how to configure these protections.</span></span>

### <a name="cross-origin-resource-sharing"></a><span data-ttu-id="bd41a-108">Cross-Origin Resource sharing</span><span class="sxs-lookup"><span data-stu-id="bd41a-108">Cross-origin resource sharing</span></span>

<span data-ttu-id="bd41a-109">[Cross-Origin Resource sharing (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) können verwendet werden, um die SignalR-Verbindungen zwischen verschiedenen Ursprüngen im Browser zulassen.</span><span class="sxs-lookup"><span data-stu-id="bd41a-109">[Cross-origin resource sharing (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) can be used to allow cross-origin SignalR connections in the browser.</span></span> <span data-ttu-id="bd41a-110">Wenn Ihr JavaScript-Code auf einem anderen Domänennamen aus der SignalR-app gehostet wird, müssen Sie aktivieren die [ASP.NET Core-CORS-Middleware](xref:security/cors) um die Verbindung zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="bd41a-110">If your JavaScript code is hosted on a different domain name from your SignalR app, you have to enable the [ASP.NET Core CORS middleware](xref:security/cors) in order to allow the connection.</span></span> <span data-ttu-id="bd41a-111">Im Allgemeinen können Sie Cross-Origin-Anforderungen nur von Domänen, die Sie steuern.</span><span class="sxs-lookup"><span data-stu-id="bd41a-111">In general, allow cross-origin requests only from domains you control.</span></span> <span data-ttu-id="bd41a-112">Wenn auf Ihrer Website gehostet wird z. B. `http://www.example.com` und Ihre SignalR-app gehostet wird `http://signalr.example.com`, sollten Sie CORS konfigurieren, in der SignalR-app, um nur den Ursprung zuzulassen `www.example.com`.</span><span class="sxs-lookup"><span data-stu-id="bd41a-112">For example, if your site is hosted on `http://www.example.com` and your SignalR app is hosted on `http://signalr.example.com`, you should configure CORS in your SignalR app to only allow the origin `www.example.com`.</span></span>

<span data-ttu-id="bd41a-113">Weitere Informationen zum Konfigurieren von CORS finden Sie unter [der Dokumentation zu ASP.NET Core CORS](xref:security/cors).</span><span class="sxs-lookup"><span data-stu-id="bd41a-113">For more information on configuring CORS, see [the documentation on ASP.NET Core CORS](xref:security/cors).</span></span> <span data-ttu-id="bd41a-114">SignalR erforderlich, dass die folgenden CORS-Richtlinien ordnungsgemäß ausgeführt wird:</span><span class="sxs-lookup"><span data-stu-id="bd41a-114">SignalR requires the following CORS policies in order to operate correctly:</span></span>

* <span data-ttu-id="bd41a-115">Die Richtlinie muss die bestimmten Ursprünge zulassen, die Sie erwarten, die oder Sie können einen beliebigen Ursprung (nicht empfohlen).</span><span class="sxs-lookup"><span data-stu-id="bd41a-115">The policy must allow the specific origins you expect, or allow any origin (not recommended).</span></span>
* <span data-ttu-id="bd41a-116">HTTP-Methoden `GET` und `POST` müssen zulässig sein.</span><span class="sxs-lookup"><span data-stu-id="bd41a-116">HTTP methods `GET` and `POST` must be allowed.</span></span>
* <span data-ttu-id="bd41a-117">Anmeldeinformationen müssen aktiviert sein, selbst wenn Sie die Authentifizierung nicht verwenden.</span><span class="sxs-lookup"><span data-stu-id="bd41a-117">Credentials must be enabled, even when you aren't using authentication.</span></span>

<span data-ttu-id="bd41a-118">Beispielsweise kann die folgende CORS-Richtlinie einer SignalR-Browser-Client auf gehosteten `http://example.com` auf Ihre SignalR-app zugreifen:</span><span class="sxs-lookup"><span data-stu-id="bd41a-118">For example, the following CORS policy allows a SignalR browser client hosted on `http://example.com` to access your SignalR app:</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    // ... other middleware ...

    // Make sure the CORS middleware is ahead of SignalR.
    app.UseCors(builder => {
        builder.WithOrigins("http://example.com")
            .AllowAnyHeader()
            .WithMethods("GET", "POST")
            .AllowCredentials();
    });

    // ... other middleware ...

    app.UseSignalR();

    // ... other middleware ...
}
```

> [!NOTE]
> <span data-ttu-id="bd41a-119">SignalR ist nicht kompatibel mit dem integrierten CORS-Feature in Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="bd41a-119">SignalR is not compatible with the built-in CORS feature in Azure App Service.</span></span>

### <a name="access-token-logging"></a><span data-ttu-id="bd41a-120">Access-token-Protokollierung</span><span class="sxs-lookup"><span data-stu-id="bd41a-120">Access token logging</span></span>

<span data-ttu-id="bd41a-121">Bei der Verwendung von WebSockets oder Server-Sent Ereignisse sendet der Browserclient das Zugriffstoken in der Abfragezeichenfolge an.</span><span class="sxs-lookup"><span data-stu-id="bd41a-121">When using WebSockets or Server-Sent Events, the browser client sends the access token in the query string.</span></span> <span data-ttu-id="bd41a-122">Dies ist im Allgemeinen so sicher wie die Verwendung des Standards `Authorization` -Header, aber viele Webserver die URL für jede Anforderung protokollieren, einschließlich der Abfragezeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="bd41a-122">This is generally as secure as using the standard `Authorization` header, however many web servers log the URL for each request, including the query string.</span></span> <span data-ttu-id="bd41a-123">Dies bedeutet, dass das Zugriffstoken in Protokollen enthalten sein kann.</span><span class="sxs-lookup"><span data-stu-id="bd41a-123">This means the access token may be included in logs.</span></span> <span data-ttu-id="bd41a-124">Sollten Sie die protokollierungseinstellungen des Webservers, um zu vermeiden, diese Informationen protokolliert.</span><span class="sxs-lookup"><span data-stu-id="bd41a-124">Consider reviewing the web server's logging settings to avoid logging this information.</span></span>

### <a name="exceptions"></a><span data-ttu-id="bd41a-125">Ausnahmen</span><span class="sxs-lookup"><span data-stu-id="bd41a-125">Exceptions</span></span>

<span data-ttu-id="bd41a-126">Ausnahmemeldungen gelten im Allgemeinen sensible Daten, die für einen Client offen gelegt werden sollte nicht.</span><span class="sxs-lookup"><span data-stu-id="bd41a-126">Exception messages are generally considered sensitive data that shouldn't be revealed to a client.</span></span> <span data-ttu-id="bd41a-127">In der Standardeinstellung senden nicht SignalR die Details der Ausnahme, die von einer hubmethode ausgelöst wird, an dem Client.</span><span class="sxs-lookup"><span data-stu-id="bd41a-127">By default, SignalR doesn't send the details of an exception thrown by a hub method to the client.</span></span> <span data-ttu-id="bd41a-128">Stattdessen erhält der Client eine generische Meldung angezeigt, dass ein Fehler aufgetreten ist.</span><span class="sxs-lookup"><span data-stu-id="bd41a-128">Instead, the client receives a generic message indicating an error occurred.</span></span> <span data-ttu-id="bd41a-129">Sie können dieses Verhalten überschreiben, indem Sie die Einstellung der [ `EnableDetailedErrors` ](xref:signalr/configuration#configure-server-options) festlegen.</span><span class="sxs-lookup"><span data-stu-id="bd41a-129">You can override this behavior by setting the [`EnableDetailedErrors`](xref:signalr/configuration#configure-server-options) setting.</span></span>

### <a name="buffer-management"></a><span data-ttu-id="bd41a-130">Pufferverwaltung</span><span class="sxs-lookup"><span data-stu-id="bd41a-130">Buffer management</span></span>

<span data-ttu-id="bd41a-131">SignalR verwendet pro Verbindung Puffer, um eingehende und ausgehende Nachrichten zu verwalten.</span><span class="sxs-lookup"><span data-stu-id="bd41a-131">SignalR uses per-connection buffers in order to manage incoming and outgoing messages.</span></span> <span data-ttu-id="bd41a-132">Standardmäßig beschränkt SignalR diese Puffer auf 32KB.</span><span class="sxs-lookup"><span data-stu-id="bd41a-132">By default, SignalR limits these buffers to 32KB.</span></span> <span data-ttu-id="bd41a-133">Dies bedeutet, dass die größte mögliche Nachricht, die einem Client oder Server senden kann 32 KB ist.</span><span class="sxs-lookup"><span data-stu-id="bd41a-133">This means the largest possible message a client or server can send is 32KB.</span></span> <span data-ttu-id="bd41a-134">Dies bedeutet auch, dass die maximale Arbeitsspeichermenge, die von einer Verbindung für Nachrichten verwendet 32KB ist.</span><span class="sxs-lookup"><span data-stu-id="bd41a-134">This also means the maximum amount of memory consumed by a connection for messages is 32KB.</span></span> <span data-ttu-id="bd41a-135">Wenn Sie, dass die Nachrichten immer kleiner als dieser Grenzwert sind wissen, können Sie diese Größe aus, um zu verhindern, dass einen Client die Möglichkeit zum Senden einer Nachricht größer und zwingt den Server, Arbeitsspeicher zuordnen, um es annehmen, reduzieren.</span><span class="sxs-lookup"><span data-stu-id="bd41a-135">If you know your messages are always smaller than this limit, you can reduce this size to prevent a client from being able to send a larger message and force the server to allocate memory to accept it.</span></span> <span data-ttu-id="bd41a-136">Auf ähnliche Weise, wenn Sie, dass Ihre Nachrichten größer als dieser Grenzwert sind wissen, können Sie ihn erhöhen.</span><span class="sxs-lookup"><span data-stu-id="bd41a-136">Similarly, if you know your messages are larger than this limit, you can increase it.</span></span> <span data-ttu-id="bd41a-137">Bedenken Sie jedoch, dass mit diesen Grenzwert zu erhöhen, dass der Client dazu führen, dass den Server zusätzlichen Arbeitsspeicher reserviert werden kann und möglicherweise Reduzieren der Anzahl von gleichzeitigen Verbindungen, die Ihre app behandeln kann.</span><span class="sxs-lookup"><span data-stu-id="bd41a-137">However, be aware that increasing this limit means that the client is able to cause the server to allocate additional memory and may reduce the number of concurrent connections your app can handle.</span></span>

<span data-ttu-id="bd41a-138">Es gibt separate Grenzwerte für eingehende und ausgehende Nachrichten, sowohl die konfiguriert werden können, auf die [ `HttpConnectionDispatcherOptions` ](xref:signalr/configuration#configure-server-options) Objekt im konfigurierten `MapHub`:</span><span class="sxs-lookup"><span data-stu-id="bd41a-138">There are separate limits for incoming and outgoing messages, both can be configured on the [`HttpConnectionDispatcherOptions`](xref:signalr/configuration#configure-server-options) object configured in `MapHub`:</span></span>

* <span data-ttu-id="bd41a-139">`ApplicationMaxBufferSize` die maximale Anzahl von Bytes aus dem Client darstellt, die die Server-Puffer.</span><span class="sxs-lookup"><span data-stu-id="bd41a-139">`ApplicationMaxBufferSize` represents the maximum number of bytes from the client that the server buffers.</span></span> <span data-ttu-id="bd41a-140">Wenn der Client versucht, eine Nachricht zu senden, die diesen Wert überschreitet, kann die Verbindung geschlossen.</span><span class="sxs-lookup"><span data-stu-id="bd41a-140">If the client attempts to send a message larger than this limit, the connection may be closed.</span></span>
* <span data-ttu-id="bd41a-141">`TransportMaxBufferSize` Stellt die maximale Anzahl von Bytes, die der Server senden kann.</span><span class="sxs-lookup"><span data-stu-id="bd41a-141">`TransportMaxBufferSize` represents the maximum number of bytes the server can send.</span></span> <span data-ttu-id="bd41a-142">Wenn der Server versucht, eine Nachricht zu senden (einschließlich der Rückgabewerte von Hub-Methoden) größer als diese Einschränkung wird eine Ausnahme ausgelöst werden.</span><span class="sxs-lookup"><span data-stu-id="bd41a-142">If the server attempts to send a message (includes return values from hub methods) larger than this limit, an exception will be thrown.</span></span>

<span data-ttu-id="bd41a-143">Festlegen der Beschränkung auf `0` die Begrenzung ganz deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="bd41a-143">Setting the limit to `0` disables the limit entirely.</span></span> <span data-ttu-id="bd41a-144">Dies sollte jedoch mit äußerster Sorgfalt erfolgen.</span><span class="sxs-lookup"><span data-stu-id="bd41a-144">However, this should be done with extreme caution.</span></span> <span data-ttu-id="bd41a-145">Entfernen das Limit kann ein Client zum Senden einer Nachricht beliebiger Größe.</span><span class="sxs-lookup"><span data-stu-id="bd41a-145">Removing the limit allows a client to send a message of any size.</span></span> <span data-ttu-id="bd41a-146">Dies konnte von überschüssiger Arbeitsspeicher zugeordnet werden, dazu führen, dass ein böswilliger Client verwendet werden, erheblich die Anzahl von gleichzeitigen Verbindungen verringert werden kann, die die app zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="bd41a-146">This could be used by a malicious client to cause excess memory to be allocated, which could dramatically reduce the number of concurrent connections your app can support.</span></span>
