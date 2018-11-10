---
title: Überlegungen zur Sicherheit in ASP.NET Core SignalR
author: tdykstra
description: Erfahren Sie, wie Sie Authentifizierung und Autorisierung in ASP.NET Core SignalR verwenden.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 10/17/2018
uid: signalr/security
ms.openlocfilehash: be1dd24c40327d9a0d8f91bf75300128d3d52725
ms.sourcegitcommit: fc7eb4243188950ae1f1b52669edc007e9d0798d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51225368"
---
# <a name="security-considerations-in-aspnet-core-signalr"></a><span data-ttu-id="a7076-103">Überlegungen zur Sicherheit in ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="a7076-103">Security considerations in ASP.NET Core SignalR</span></span>

<span data-ttu-id="a7076-104">Durch [Andrew Stanton-Nurse](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="a7076-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

<span data-ttu-id="a7076-105">Dieser Artikel enthält Informationen zum Sichern von SignalR.</span><span class="sxs-lookup"><span data-stu-id="a7076-105">This article provides information on securing SignalR.</span></span>

## <a name="cross-origin-resource-sharing"></a><span data-ttu-id="a7076-106">Cross-Origin Resource sharing</span><span class="sxs-lookup"><span data-stu-id="a7076-106">Cross-origin resource sharing</span></span>

<span data-ttu-id="a7076-107">[Cross-Origin Resource sharing (CORS)](https://www.w3.org/TR/cors/) können verwendet werden, um die SignalR-Verbindungen zwischen verschiedenen Ursprüngen im Browser zulassen.</span><span class="sxs-lookup"><span data-stu-id="a7076-107">[Cross-origin resource sharing (CORS)](https://www.w3.org/TR/cors/) can be used to allow cross-origin SignalR connections in the browser.</span></span> <span data-ttu-id="a7076-108">Wenn JavaScript-Code auf einer anderen Domäne als der SignalR-app gehostet wird [CORS-Middleware](xref:security/cors) muss aktiviert sein, um die JavaScript für die Verbindung zur SignalR-app zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="a7076-108">If JavaScript code is hosted on a different domain from the SignalR app, [CORS middleware](xref:security/cors) must be enabled to allow the JavaScript to connect to the SignalR app.</span></span> <span data-ttu-id="a7076-109">Zulassen, dass Cross-Origin-Anforderungen nur von Domänen, denen, die Sie vertrauen, oder Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="a7076-109">Allow cross-origin requests only from domains you trust or control.</span></span> <span data-ttu-id="a7076-110">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="a7076-110">For example:</span></span>

* <span data-ttu-id="a7076-111">Ihre Website wird auf gehostet. `http://www.example.com`</span><span class="sxs-lookup"><span data-stu-id="a7076-111">Your site is hosted on `http://www.example.com`</span></span>
* <span data-ttu-id="a7076-112">Der SignalR-app wird auf gehostet. `http://signalr.example.com`</span><span class="sxs-lookup"><span data-stu-id="a7076-112">Your SignalR app is hosted on `http://signalr.example.com`</span></span>

<span data-ttu-id="a7076-113">CORS konfiguriert werden sollte, in der SignalR-app, um nur den Ursprung zuzulassen `www.example.com`.</span><span class="sxs-lookup"><span data-stu-id="a7076-113">CORS should be configured in the SignalR app to only allow the origin `www.example.com`.</span></span>

<span data-ttu-id="a7076-114">Weitere Informationen zum Konfigurieren von CORS finden Sie unter [aktivieren Ursprungsübergreifender Anforderungen (CORS)](xref:security/cors).</span><span class="sxs-lookup"><span data-stu-id="a7076-114">For more information on configuring CORS, see [Enable Cross-Origin Requests (CORS)](xref:security/cors).</span></span> <span data-ttu-id="a7076-115">SignalR **erfordert** die folgenden CORS-Richtlinien:</span><span class="sxs-lookup"><span data-stu-id="a7076-115">SignalR **requires** the following CORS policies:</span></span>

* <span data-ttu-id="a7076-116">Ermöglichen Sie die bestimmten erwarteten Ursprünge.</span><span class="sxs-lookup"><span data-stu-id="a7076-116">Allow the specific expected origins.</span></span> <span data-ttu-id="a7076-117">Ermöglicht einem beliebigen Ursprung ist möglich ist aber **nicht** sichere oder empfohlen.</span><span class="sxs-lookup"><span data-stu-id="a7076-117">Allowing any origin is possible but is **not** secure or recommended.</span></span>
* <span data-ttu-id="a7076-118">HTTP-Methoden `GET` und `POST` müssen zulässig sein.</span><span class="sxs-lookup"><span data-stu-id="a7076-118">HTTP methods `GET` and `POST` must be allowed.</span></span>
* <span data-ttu-id="a7076-119">Anmeldeinformationen müssen aktiviert werden, auch wenn keine Authentifizierung verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="a7076-119">Credentials must be enabled, even when authentication is not used.</span></span>

<span data-ttu-id="a7076-120">Beispielsweise kann die folgende CORS-Richtlinie einer SignalR-Browser-Client auf gehosteten `http://example.com` Zugriff auf die SignalR-app, die auf gehosteten `http://signalr.example.com`:</span><span class="sxs-lookup"><span data-stu-id="a7076-120">For example, the following CORS policy allows a SignalR browser client hosted on `http://example.com` to access the SignalR app hosted on `http://signalr.example.com`:</span></span>

[!code-csharp[Main](security/sample/Startup.cs?name=snippet1)]

> [!NOTE]
> <span data-ttu-id="a7076-121">SignalR ist nicht kompatibel mit dem integrierten CORS-Feature in Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="a7076-121">SignalR is not compatible with the built-in CORS feature in Azure App Service.</span></span>

## <a name="websocket-origin-restriction"></a><span data-ttu-id="a7076-122">Ursprung der WebSocket-Einschränkung</span><span class="sxs-lookup"><span data-stu-id="a7076-122">WebSocket Origin Restriction</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="a7076-123">Der Schutz von CORS erhalten, anwenden nicht auf WebSockets.</span><span class="sxs-lookup"><span data-stu-id="a7076-123">The protections provided by CORS don't apply to WebSockets.</span></span> <span data-ttu-id="a7076-124">Origin-Einschränkung für WebSockets, finden Sie in [WebSockets Ursprung Einschränkung](xref:fundamentals/websockets#websocket-origin-restriction).</span><span class="sxs-lookup"><span data-stu-id="a7076-124">For origin restriction on WebSockets, read [WebSockets origin restriction](xref:fundamentals/websockets#websocket-origin-restriction).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="a7076-125">Der Schutz von CORS erhalten, anwenden nicht auf WebSockets.</span><span class="sxs-lookup"><span data-stu-id="a7076-125">The protections provided by CORS don't apply to WebSockets.</span></span> <span data-ttu-id="a7076-126">Führen Sie den Browser **nicht**:</span><span class="sxs-lookup"><span data-stu-id="a7076-126">Browsers do **not**:</span></span>

* <span data-ttu-id="a7076-127">Führen Sie die CORS-Preflight-Prüfliste-Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="a7076-127">Perform CORS pre-flight requests.</span></span>
* <span data-ttu-id="a7076-128">Berücksichtigen Sie die Einschränkungen, die im angegebenen `Access-Control` Header bei WebSocket-Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="a7076-128">Respect the restrictions specified in `Access-Control` headers when making WebSocket requests.</span></span>

<span data-ttu-id="a7076-129">Allerdings Browser Sende die `Origin` Header, wenn die WebSocket-Anforderungen ausgeben.</span><span class="sxs-lookup"><span data-stu-id="a7076-129">However, browsers do send the `Origin` header when issuing WebSocket requests.</span></span> <span data-ttu-id="a7076-130">Anwendungen sollten konfiguriert werden, um zu überprüfen diese Header, um sicherzustellen, dass nur WebSockets, die von der erwarteten Ursprünge zulässig sind.</span><span class="sxs-lookup"><span data-stu-id="a7076-130">Applications should be configured to validate these headers to ensure that only WebSockets coming from the expected origins are allowed.</span></span>

<span data-ttu-id="a7076-131">In ASP.NET Core 2.1 und höher headerüberprüfung erfolgt über eine benutzerdefinierte Middleware platziert **vor `UseSignalR`, und die authentifizierungsmiddleware** in `Configure`:</span><span class="sxs-lookup"><span data-stu-id="a7076-131">In ASP.NET Core 2.1 and later, header validation can be achieved using a custom middleware placed **before `UseSignalR`, and authentication middleware** in `Configure`:</span></span>

[!code-csharp[Main](security/sample/Startup.cs?name=snippet2)]

> [!NOTE]
> <span data-ttu-id="a7076-132">Die `Origin` Header wird gesteuert, durch den Client und, wie die `Referer` -Header, überlistet werden kann.</span><span class="sxs-lookup"><span data-stu-id="a7076-132">The `Origin` header is controlled by the client and, like the `Referer` header, can be faked.</span></span> <span data-ttu-id="a7076-133">Dieser Header sollte **nicht** als Authentifizierungsmechanismus verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="a7076-133">These headers should **not** be used as an authentication mechanism.</span></span>

::: moniker-end

## <a name="access-token-logging"></a><span data-ttu-id="a7076-134">Access-token-Protokollierung</span><span class="sxs-lookup"><span data-stu-id="a7076-134">Access token logging</span></span>

<span data-ttu-id="a7076-135">Bei der Verwendung von WebSockets oder Server-Sent Ereignisse sendet der Browserclient das Zugriffstoken in der Abfragezeichenfolge an.</span><span class="sxs-lookup"><span data-stu-id="a7076-135">When using WebSockets or Server-Sent Events, the browser client sends the access token in the query string.</span></span> <span data-ttu-id="a7076-136">Empfangen des Zugriffstokens über die Abfragezeichenfolge in der Regel so sicher wie die Verwendung des Standards ist `Authorization` Header.</span><span class="sxs-lookup"><span data-stu-id="a7076-136">Receiving the access token via query string is generally as secure as using the standard `Authorization` header.</span></span> <span data-ttu-id="a7076-137">Allerdings melden vielen Webservern an die URL für jede Anforderung, einschließlich der Abfragezeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="a7076-137">However, many web servers log the URL for each request, including the query string.</span></span> <span data-ttu-id="a7076-138">Protokollieren die URLs möglicherweise melden Sie sich das Zugriffstoken.</span><span class="sxs-lookup"><span data-stu-id="a7076-138">Logging the URLs may log the access token.</span></span> <span data-ttu-id="a7076-139">Eine bewährte Methode besteht darin im Web protokolleinstellungen des Servers zu verhindern, dass bei der Protokollierung Zugriffstoken festzulegen.</span><span class="sxs-lookup"><span data-stu-id="a7076-139">A best practice is to set the web server's logging settings to prevent logging access tokens.</span></span>

## <a name="exceptions"></a><span data-ttu-id="a7076-140">Ausnahmen</span><span class="sxs-lookup"><span data-stu-id="a7076-140">Exceptions</span></span>

<span data-ttu-id="a7076-141">Ausnahmemeldungen gelten im Allgemeinen sensible Daten, die für einen Client offen gelegt werden sollte nicht.</span><span class="sxs-lookup"><span data-stu-id="a7076-141">Exception messages are generally considered sensitive data that shouldn't be revealed to a client.</span></span> <span data-ttu-id="a7076-142">In der Standardeinstellung senden nicht SignalR die Details der Ausnahme, die von einer hubmethode ausgelöst wird, an dem Client.</span><span class="sxs-lookup"><span data-stu-id="a7076-142">By default, SignalR doesn't send the details of an exception thrown by a hub method to the client.</span></span> <span data-ttu-id="a7076-143">Stattdessen erhält der Client eine generische Meldung angezeigt, dass ein Fehler aufgetreten ist.</span><span class="sxs-lookup"><span data-stu-id="a7076-143">Instead, the client receives a generic message indicating an error occurred.</span></span> <span data-ttu-id="a7076-144">Mit Ausnahme der Nachrichtenübermittlung an den Client (z. B. in Entwicklungs- oder testumgebung) überschrieben werden kann [ `EnableDetailedErrors` ](xref:signalr/configuration#configure-server-options).</span><span class="sxs-lookup"><span data-stu-id="a7076-144">Exception message delivery to the client can be overridden (for example in development or test) with [`EnableDetailedErrors`](xref:signalr/configuration#configure-server-options).</span></span> <span data-ttu-id="a7076-145">Ausnahmemeldungen sollte nicht an den Client in Produktions-apps verfügbar gemacht werden.</span><span class="sxs-lookup"><span data-stu-id="a7076-145">Exception messages should not be exposed to the client in production apps.</span></span>

## <a name="buffer-management"></a><span data-ttu-id="a7076-146">Pufferverwaltung</span><span class="sxs-lookup"><span data-stu-id="a7076-146">Buffer management</span></span>

<span data-ttu-id="a7076-147">SignalR verwendet pro Verbindung Puffer zum Verwalten von eingehender und ausgehender Nachrichten.</span><span class="sxs-lookup"><span data-stu-id="a7076-147">SignalR uses per-connection buffers to manage incoming and outgoing messages.</span></span> <span data-ttu-id="a7076-148">Standardmäßig beschränkt SignalR diese Puffer auf 32 KB.</span><span class="sxs-lookup"><span data-stu-id="a7076-148">By default, SignalR limits these buffers to 32 KB.</span></span> <span data-ttu-id="a7076-149">Die größte Nachricht, die einem Client oder Server senden kann, beträgt 32 KB.</span><span class="sxs-lookup"><span data-stu-id="a7076-149">The largest message a client or server can send is 32 KB.</span></span> <span data-ttu-id="a7076-150">Der maximale Arbeitsspeicher genutzt werden, indem Sie eine Verbindung für Nachrichten beträgt 32 KB.</span><span class="sxs-lookup"><span data-stu-id="a7076-150">The maximum memory consumed by a connection for messages is 32 KB.</span></span> <span data-ttu-id="a7076-151">Wenn Ihre Nachrichten immer kleiner als 32 KB sind, können Sie die maximale Anzahl reduzieren die:</span><span class="sxs-lookup"><span data-stu-id="a7076-151">If your messages are always smaller than 32 KB, you can reduce the limit, which:</span></span>

* <span data-ttu-id="a7076-152">Verhindert, dass einen Client eine größere Nachricht senden.</span><span class="sxs-lookup"><span data-stu-id="a7076-152">Prevents a client from being able to send a larger message.</span></span>
* <span data-ttu-id="a7076-153">Der Server muss nie zuweisen großer Puffer Nachrichten zu akzeptieren.</span><span class="sxs-lookup"><span data-stu-id="a7076-153">The server will never need to allocate large buffers to accept messages.</span></span>

<span data-ttu-id="a7076-154">Wenn Ihre Nachrichten größer als 32 KB sind, können Sie den Grenzwert erhöhen.</span><span class="sxs-lookup"><span data-stu-id="a7076-154">If your messages are larger than 32 KB, you can increase the limit.</span></span> <span data-ttu-id="a7076-155">Erhöhen diese Beschränkung bedeutet:</span><span class="sxs-lookup"><span data-stu-id="a7076-155">Increasing this limit means:</span></span>

* <span data-ttu-id="a7076-156">Der Client kann den Server an große Speicherpuffer zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="a7076-156">The client can cause the server to allocate large memory buffers.</span></span>
* <span data-ttu-id="a7076-157">Serverzuordnung großen Puffern kann die Anzahl von gleichzeitigen Verbindungen reduzieren.</span><span class="sxs-lookup"><span data-stu-id="a7076-157">Server allocation of large buffers may reduce the number of concurrent connections.</span></span>

<span data-ttu-id="a7076-158">Es sind die Grenzwerte für eingehende und ausgehende Nachrichten, sowohl die konfiguriert werden können, auf die [ `HttpConnectionDispatcherOptions` ](xref:signalr/configuration#configure-server-options) Objekt im konfigurierten `MapHub`:</span><span class="sxs-lookup"><span data-stu-id="a7076-158">There are limits for incoming and outgoing messages, both can be configured on the [`HttpConnectionDispatcherOptions`](xref:signalr/configuration#configure-server-options) object configured in `MapHub`:</span></span>

* <span data-ttu-id="a7076-159">`ApplicationMaxBufferSize` die maximale Anzahl von Bytes aus dem Client darstellt, die die Server-Puffer.</span><span class="sxs-lookup"><span data-stu-id="a7076-159">`ApplicationMaxBufferSize` represents the maximum number of bytes from the client that the server buffers.</span></span> <span data-ttu-id="a7076-160">Wenn der Client versucht, eine Nachricht zu senden, die diesen Wert überschreitet, kann die Verbindung geschlossen.</span><span class="sxs-lookup"><span data-stu-id="a7076-160">If the client attempts to send a message larger than this limit, the connection may be closed.</span></span>
* <span data-ttu-id="a7076-161">`TransportMaxBufferSize` Stellt die maximale Anzahl von Bytes, die der Server senden kann.</span><span class="sxs-lookup"><span data-stu-id="a7076-161">`TransportMaxBufferSize` represents the maximum number of bytes the server can send.</span></span> <span data-ttu-id="a7076-162">Wenn der Server versucht, die eine Nachricht (einschließlich Rückgabewerte von Hub-Methoden) zu senden, die diesen Wert überschreitet, wird eine Ausnahme ausgelöst werden.</span><span class="sxs-lookup"><span data-stu-id="a7076-162">If the server attempts to send a message (including return values from hub methods) larger than this limit, an exception will be thrown.</span></span>

<span data-ttu-id="a7076-163">Festlegen der Beschränkung auf `0` deaktiviert den Grenzwert.</span><span class="sxs-lookup"><span data-stu-id="a7076-163">Setting the limit to `0` disables the limit.</span></span> <span data-ttu-id="a7076-164">Entfernen das Limit kann ein Client zum Senden einer Nachricht beliebiger Größe.</span><span class="sxs-lookup"><span data-stu-id="a7076-164">Removing the limit allows a client to send a message of any size.</span></span> <span data-ttu-id="a7076-165">Böswillige Clients Senden großer Nachrichten können dazu führen, dass überschüssigen Arbeitsspeicher zugeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="a7076-165">Malicious clients sending large messages can cause excess memory to be allocated.</span></span> <span data-ttu-id="a7076-166">Übermäßige speicherauslastung kann die Anzahl von gleichzeitigen Verbindungen erheblich reduzieren.</span><span class="sxs-lookup"><span data-stu-id="a7076-166">Excess memory usage can significantly reduce the number of concurrent connections.</span></span>
