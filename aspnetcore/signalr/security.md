---
title: Überlegungen zur Sicherheit in ASP.NET Core SignalR
author: tdykstra
description: Erfahren Sie, wie Sie Authentifizierung und Autorisierung in ASP.NET Core SignalR verwenden.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 06/29/2018
uid: signalr/security
ms.openlocfilehash: 98b5eb7be87920aacf7a941f76ff652ae7905303
ms.sourcegitcommit: f43f430a166a7ec137fcad12ded0372747227498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2018
ms.locfileid: "49391257"
---
# <a name="security-considerations-in-aspnet-core-signalr"></a><span data-ttu-id="2669c-103">Überlegungen zur Sicherheit in ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="2669c-103">Security considerations in ASP.NET Core SignalR</span></span>

<span data-ttu-id="2669c-104">Durch [Andrew Stanton-Nurse](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="2669c-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

## <a name="overview"></a><span data-ttu-id="2669c-105">Übersicht</span><span class="sxs-lookup"><span data-stu-id="2669c-105">Overview</span></span>

<span data-ttu-id="2669c-106">SignalR stellt eine Anzahl von Sicherheitsmaßnahmen standardmäßig bereit.</span><span class="sxs-lookup"><span data-stu-id="2669c-106">SignalR provides a number of security protections by default.</span></span> <span data-ttu-id="2669c-107">Es ist wichtig zu verstehen, wie diese Schutzmaßnahmen zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="2669c-107">It's important to understand how to configure these protections.</span></span>

### <a name="cross-origin-resource-sharing"></a><span data-ttu-id="2669c-108">Cross-Origin Resource sharing</span><span class="sxs-lookup"><span data-stu-id="2669c-108">Cross-origin resource sharing</span></span>

<span data-ttu-id="2669c-109">[Cross-Origin Resource sharing (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) können verwendet werden, um die SignalR-Verbindungen zwischen verschiedenen Ursprüngen im Browser zulassen.</span><span class="sxs-lookup"><span data-stu-id="2669c-109">[Cross-origin resource sharing (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) can be used to allow cross-origin SignalR connections in the browser.</span></span> <span data-ttu-id="2669c-110">Wenn Ihr JavaScript-Code auf einem anderen Domänennamen aus der SignalR-app gehostet wird, müssen Sie aktivieren die [ASP.NET Core-CORS-Middleware](xref:security/cors) um die Verbindung zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="2669c-110">If your JavaScript code is hosted on a different domain name from your SignalR app, you have to enable the [ASP.NET Core CORS middleware](xref:security/cors) in order to allow the connection.</span></span> <span data-ttu-id="2669c-111">Im Allgemeinen können Sie Cross-Origin-Anforderungen nur von Domänen, die Sie steuern.</span><span class="sxs-lookup"><span data-stu-id="2669c-111">In general, allow cross-origin requests only from domains you control.</span></span> <span data-ttu-id="2669c-112">Wenn auf Ihrer Website gehostet wird z. B. `http://www.example.com` und Ihre SignalR-app gehostet wird `http://signalr.example.com`, sollten Sie CORS konfigurieren, in der SignalR-app, um nur den Ursprung zuzulassen `www.example.com`.</span><span class="sxs-lookup"><span data-stu-id="2669c-112">For example, if your site is hosted on `http://www.example.com` and your SignalR app is hosted on `http://signalr.example.com`, you should configure CORS in your SignalR app to only allow the origin `www.example.com`.</span></span>

<span data-ttu-id="2669c-113">Weitere Informationen zum Konfigurieren von CORS finden Sie unter [der Dokumentation zu ASP.NET Core CORS](xref:security/cors).</span><span class="sxs-lookup"><span data-stu-id="2669c-113">For more information on configuring CORS, see [the documentation on ASP.NET Core CORS](xref:security/cors).</span></span> <span data-ttu-id="2669c-114">SignalR erforderlich, dass die folgenden CORS-Richtlinien ordnungsgemäß ausgeführt wird:</span><span class="sxs-lookup"><span data-stu-id="2669c-114">SignalR requires the following CORS policies in order to operate correctly:</span></span>

* <span data-ttu-id="2669c-115">Die Richtlinie muss die bestimmten Ursprünge zulassen, die Sie erwarten, die oder Sie können einen beliebigen Ursprung (nicht empfohlen).</span><span class="sxs-lookup"><span data-stu-id="2669c-115">The policy must allow the specific origins you expect, or allow any origin (not recommended).</span></span>
* <span data-ttu-id="2669c-116">HTTP-Methoden `GET` und `POST` müssen zulässig sein.</span><span class="sxs-lookup"><span data-stu-id="2669c-116">HTTP methods `GET` and `POST` must be allowed.</span></span>
* <span data-ttu-id="2669c-117">Anmeldeinformationen müssen aktiviert sein, selbst wenn Sie die Authentifizierung nicht verwenden.</span><span class="sxs-lookup"><span data-stu-id="2669c-117">Credentials must be enabled, even when you aren't using authentication.</span></span>

<span data-ttu-id="2669c-118">Beispielsweise kann die folgende CORS-Richtlinie einer SignalR-Browser-Client auf gehosteten `http://example.com` auf Ihre SignalR-app zugreifen:</span><span class="sxs-lookup"><span data-stu-id="2669c-118">For example, the following CORS policy allows a SignalR browser client hosted on `http://example.com` to access your SignalR app:</span></span>

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
> <span data-ttu-id="2669c-119">SignalR ist nicht kompatibel mit dem integrierten CORS-Feature in Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="2669c-119">SignalR is not compatible with the built-in CORS feature in Azure App Service.</span></span>

### <a name="websocket-origin-restriction"></a><span data-ttu-id="2669c-120">Ursprung der WebSocket-Einschränkung</span><span class="sxs-lookup"><span data-stu-id="2669c-120">WebSocket Origin Restriction</span></span>

<span data-ttu-id="2669c-121">Der Schutz erhalten von CORS gelten nicht für WebSockets.</span><span class="sxs-lookup"><span data-stu-id="2669c-121">The protections provided by CORS do not apply to WebSockets.</span></span> <span data-ttu-id="2669c-122">Browser führen keine CORS-Preflight-Prüfliste-Anforderungen, und berücksichtigen sie die Einschränkungen, die im angegebenen `Access-Control` Header bei WebSocket-Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="2669c-122">Browsers do not perform CORS pre-flight requests, nor do they respect the restrictions specified in `Access-Control` headers when making WebSocket requests.</span></span> <span data-ttu-id="2669c-123">Allerdings Browser Sende die `Origin` Header, wenn die WebSocket-Anforderungen ausgeben.</span><span class="sxs-lookup"><span data-stu-id="2669c-123">However, browsers do send the `Origin` header when issuing WebSocket requests.</span></span> <span data-ttu-id="2669c-124">Sie sollten Ihre Anwendung diese Header zu überprüfen, um sicherzustellen, dass nur WebSockets, stammen die Ursprünge an, die Sie erwarten dürfen konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="2669c-124">You should configure your application to validate these headers in order to ensure that only WebSockets coming from the origins you expect are allowed.</span></span>

<span data-ttu-id="2669c-125">In ASP.NET Core 2.1 dafür eine benutzerdefinierte Middleware kann man mit **oben `UseSignalR`, und jeder authentifizierungsmiddleware** in Ihre `Configure` Methode:</span><span class="sxs-lookup"><span data-stu-id="2669c-125">In ASP.NET Core 2.1, this can be achieved using a custom middleware you can place **above `UseSignalR`, and any authentication middleware** in your `Configure` method:</span></span>

```csharp
// In your Startup class, add a static field listing the allowed Origin values:
private static readonly HashSet<string> _allowedOrigins = new HashSet<string>()
{
    // Add allowed origins here. For example:
    "http://www.mysite.com",
    "http://mysite.com",
};

// In your Configure method:
public void Configure(IApplicationBuilder app)
{
    // ... other middleware ...

    // Validate Origin header on WebSocket requests to prevent unexpected cross-site WebSocket requests
    app.Use((context, next) =>
    {
        // Check for a WebSocket request.
        if(string.Equals(context.Request.Headers["Upgrade"], "websocket"))
        {
            var origin = context.Request.Headers["Origin"];

            // If there is no origin header, or if the origin header doesn't match an allowed value:
            if(string.IsNullOrEmpty(origin) && !_allowedOrigins.Contains(origin))
            {
                // The origin is not allowed, reject the request
                context.Response.StatusCode = StatusCodes.Status400BadRequest;
                return Task.CompletedTask;
            }
        }

        // The request is not a WebSocket request or is a valid Origin, so let it continue
        return next();
    });

    // ... other middleware ...

    app.UseSignalR();

    // ... other middleware ...
}
```

> [!NOTE]
> <span data-ttu-id="2669c-126">Die `Origin` Header wird vollständig gesteuert werden, durch den Client und, wie die `Referer` -Header, überlistet werden kann.</span><span class="sxs-lookup"><span data-stu-id="2669c-126">The `Origin` header is completely controlled by the client and, like the `Referer` header, can be faked.</span></span> <span data-ttu-id="2669c-127">Dieser Header sollte nie als Authentifizierungsmechanismus verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="2669c-127">These headers should never be used as an authentication mechanism.</span></span>

### <a name="access-token-logging"></a><span data-ttu-id="2669c-128">Access-token-Protokollierung</span><span class="sxs-lookup"><span data-stu-id="2669c-128">Access token logging</span></span>

<span data-ttu-id="2669c-129">Bei der Verwendung von WebSockets oder Server-Sent Ereignisse sendet der Browserclient das Zugriffstoken in der Abfragezeichenfolge an.</span><span class="sxs-lookup"><span data-stu-id="2669c-129">When using WebSockets or Server-Sent Events, the browser client sends the access token in the query string.</span></span> <span data-ttu-id="2669c-130">Dies ist im Allgemeinen so sicher wie die Verwendung des Standards `Authorization` -Header, aber viele Webserver die URL für jede Anforderung protokollieren, einschließlich der Abfragezeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="2669c-130">This is generally as secure as using the standard `Authorization` header, however many web servers log the URL for each request, including the query string.</span></span> <span data-ttu-id="2669c-131">Dies bedeutet, dass das Zugriffstoken in Protokollen enthalten sein kann.</span><span class="sxs-lookup"><span data-stu-id="2669c-131">This means the access token may be included in logs.</span></span> <span data-ttu-id="2669c-132">Sollten Sie die protokollierungseinstellungen des Webservers, um zu vermeiden, diese Informationen protokolliert.</span><span class="sxs-lookup"><span data-stu-id="2669c-132">Consider reviewing the web server's logging settings to avoid logging this information.</span></span>

### <a name="exceptions"></a><span data-ttu-id="2669c-133">Ausnahmen</span><span class="sxs-lookup"><span data-stu-id="2669c-133">Exceptions</span></span>

<span data-ttu-id="2669c-134">Ausnahmemeldungen gelten im Allgemeinen sensible Daten, die für einen Client offen gelegt werden sollte nicht.</span><span class="sxs-lookup"><span data-stu-id="2669c-134">Exception messages are generally considered sensitive data that shouldn't be revealed to a client.</span></span> <span data-ttu-id="2669c-135">In der Standardeinstellung senden nicht SignalR die Details der Ausnahme, die von einer hubmethode ausgelöst wird, an dem Client.</span><span class="sxs-lookup"><span data-stu-id="2669c-135">By default, SignalR doesn't send the details of an exception thrown by a hub method to the client.</span></span> <span data-ttu-id="2669c-136">Stattdessen erhält der Client eine generische Meldung angezeigt, dass ein Fehler aufgetreten ist.</span><span class="sxs-lookup"><span data-stu-id="2669c-136">Instead, the client receives a generic message indicating an error occurred.</span></span> <span data-ttu-id="2669c-137">Sie können dieses Verhalten überschreiben, indem Sie die Einstellung der [ `EnableDetailedErrors` ](xref:signalr/configuration#configure-server-options) festlegen.</span><span class="sxs-lookup"><span data-stu-id="2669c-137">You can override this behavior by setting the [`EnableDetailedErrors`](xref:signalr/configuration#configure-server-options) setting.</span></span>

### <a name="buffer-management"></a><span data-ttu-id="2669c-138">Pufferverwaltung</span><span class="sxs-lookup"><span data-stu-id="2669c-138">Buffer management</span></span>

<span data-ttu-id="2669c-139">SignalR verwendet pro Verbindung Puffer, um eingehende und ausgehende Nachrichten zu verwalten.</span><span class="sxs-lookup"><span data-stu-id="2669c-139">SignalR uses per-connection buffers in order to manage incoming and outgoing messages.</span></span> <span data-ttu-id="2669c-140">Standardmäßig beschränkt SignalR diese Puffer auf 32KB.</span><span class="sxs-lookup"><span data-stu-id="2669c-140">By default, SignalR limits these buffers to 32KB.</span></span> <span data-ttu-id="2669c-141">Dies bedeutet, dass die größte mögliche Nachricht, die einem Client oder Server senden kann 32 KB ist.</span><span class="sxs-lookup"><span data-stu-id="2669c-141">This means the largest possible message a client or server can send is 32KB.</span></span> <span data-ttu-id="2669c-142">Dies bedeutet auch, dass die maximale Arbeitsspeichermenge, die von einer Verbindung für Nachrichten verwendet 32KB ist.</span><span class="sxs-lookup"><span data-stu-id="2669c-142">This also means the maximum amount of memory consumed by a connection for messages is 32KB.</span></span> <span data-ttu-id="2669c-143">Wenn Sie, dass die Nachrichten immer kleiner als dieser Grenzwert sind wissen, können Sie diese Größe aus, um zu verhindern, dass einen Client die Möglichkeit zum Senden einer Nachricht größer und zwingt den Server, Arbeitsspeicher zuordnen, um es annehmen, reduzieren.</span><span class="sxs-lookup"><span data-stu-id="2669c-143">If you know your messages are always smaller than this limit, you can reduce this size to prevent a client from being able to send a larger message and force the server to allocate memory to accept it.</span></span> <span data-ttu-id="2669c-144">Auf ähnliche Weise, wenn Sie, dass Ihre Nachrichten größer als dieser Grenzwert sind wissen, können Sie ihn erhöhen.</span><span class="sxs-lookup"><span data-stu-id="2669c-144">Similarly, if you know your messages are larger than this limit, you can increase it.</span></span> <span data-ttu-id="2669c-145">Bedenken Sie jedoch, dass mit diesen Grenzwert zu erhöhen, dass der Client dazu führen, dass den Server zusätzlichen Arbeitsspeicher reserviert werden kann und möglicherweise Reduzieren der Anzahl von gleichzeitigen Verbindungen, die Ihre app behandeln kann.</span><span class="sxs-lookup"><span data-stu-id="2669c-145">However, be aware that increasing this limit means that the client is able to cause the server to allocate additional memory and may reduce the number of concurrent connections your app can handle.</span></span>

<span data-ttu-id="2669c-146">Es gibt separate Grenzwerte für eingehende und ausgehende Nachrichten, sowohl die konfiguriert werden können, auf die [ `HttpConnectionDispatcherOptions` ](xref:signalr/configuration#configure-server-options) Objekt im konfigurierten `MapHub`:</span><span class="sxs-lookup"><span data-stu-id="2669c-146">There are separate limits for incoming and outgoing messages, both can be configured on the [`HttpConnectionDispatcherOptions`](xref:signalr/configuration#configure-server-options) object configured in `MapHub`:</span></span>

* <span data-ttu-id="2669c-147">`ApplicationMaxBufferSize` die maximale Anzahl von Bytes aus dem Client darstellt, die die Server-Puffer.</span><span class="sxs-lookup"><span data-stu-id="2669c-147">`ApplicationMaxBufferSize` represents the maximum number of bytes from the client that the server buffers.</span></span> <span data-ttu-id="2669c-148">Wenn der Client versucht, eine Nachricht zu senden, die diesen Wert überschreitet, kann die Verbindung geschlossen.</span><span class="sxs-lookup"><span data-stu-id="2669c-148">If the client attempts to send a message larger than this limit, the connection may be closed.</span></span>
* <span data-ttu-id="2669c-149">`TransportMaxBufferSize` Stellt die maximale Anzahl von Bytes, die der Server senden kann.</span><span class="sxs-lookup"><span data-stu-id="2669c-149">`TransportMaxBufferSize` represents the maximum number of bytes the server can send.</span></span> <span data-ttu-id="2669c-150">Wenn der Server versucht, eine Nachricht zu senden (einschließlich der Rückgabewerte von Hub-Methoden) größer als diese Einschränkung wird eine Ausnahme ausgelöst werden.</span><span class="sxs-lookup"><span data-stu-id="2669c-150">If the server attempts to send a message (includes return values from hub methods) larger than this limit, an exception will be thrown.</span></span>

<span data-ttu-id="2669c-151">Festlegen der Beschränkung auf `0` die Begrenzung ganz deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="2669c-151">Setting the limit to `0` disables the limit entirely.</span></span> <span data-ttu-id="2669c-152">Dies sollte jedoch mit äußerster Sorgfalt erfolgen.</span><span class="sxs-lookup"><span data-stu-id="2669c-152">However, this should be done with extreme caution.</span></span> <span data-ttu-id="2669c-153">Entfernen das Limit kann ein Client zum Senden einer Nachricht beliebiger Größe.</span><span class="sxs-lookup"><span data-stu-id="2669c-153">Removing the limit allows a client to send a message of any size.</span></span> <span data-ttu-id="2669c-154">Dies konnte von überschüssiger Arbeitsspeicher zugeordnet werden, dazu führen, dass ein böswilliger Client verwendet werden, erheblich die Anzahl von gleichzeitigen Verbindungen verringert werden kann, die die app zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="2669c-154">This could be used by a malicious client to cause excess memory to be allocated, which could dramatically reduce the number of concurrent connections your app can support.</span></span>
