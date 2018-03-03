---
title: Erzwingen von HTTPS in einer ASP.NET Core-app
author: rick-anderson
description: Veranschaulicht, wie HTTPS/TLS in einer ASP.NET Core erfordern Web-app.
manager: wpickett
ms.author: riande
ms.date: 2/9/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: dc320faf0048200412f131ea816f33f29ac023e1
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2018
---
# <a name="enforcing-https-in-an-aspnet-core-app"></a><span data-ttu-id="f1407-103">Erzwingen von HTTPS in einer ASP.NET Core-app</span><span class="sxs-lookup"><span data-stu-id="f1407-103">Enforcing HTTPS in an ASP.NET Core app</span></span>

<span data-ttu-id="f1407-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f1407-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f1407-105">Dieses Dokument zeigt, wie Sie:</span><span class="sxs-lookup"><span data-stu-id="f1407-105">This document shows how to:</span></span>

- <span data-ttu-id="f1407-106">HTTPS für alle Anforderungen erforderlich.</span><span class="sxs-lookup"><span data-stu-id="f1407-106">Require HTTPS for all requests.</span></span>
- <span data-ttu-id="f1407-107">Umleiten Sie alle HTTP-Anforderungen an HTTPS.</span><span class="sxs-lookup"><span data-stu-id="f1407-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="f1407-108">Führen Sie **nicht** verwenden `RequireHttpsAttribute` auf Web-APIs, die vertraulichen Informationen zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="f1407-108">Do **not** use `RequireHttpsAttribute` on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="f1407-109">`RequireHttpsAttribute` verwendet HTTP-Statuscodes Browsern von HTTP an HTTPS umgeleitet.</span><span class="sxs-lookup"><span data-stu-id="f1407-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="f1407-110">API-Clients möglicherweise nicht verstehen oder Umleitung von HTTP in HTTPS unterliegen.</span><span class="sxs-lookup"><span data-stu-id="f1407-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="f1407-111">Diese Clients möglicherweise Informationen über HTTP gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="f1407-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="f1407-112">Web-APIs sollten entweder:</span><span class="sxs-lookup"><span data-stu-id="f1407-112">Web APIs should either:</span></span>
>
>* <span data-ttu-id="f1407-113">Nicht auf HTTP überwacht.</span><span class="sxs-lookup"><span data-stu-id="f1407-113">Not listen on HTTP.</span></span>
>* <span data-ttu-id="f1407-114">Schließen Sie die Verbindung mit dem Statuscode 400 (Ungültige Anforderung), und verarbeiten Sie die Anforderung nicht.</span><span class="sxs-lookup"><span data-stu-id="f1407-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

## <a name="require-https"></a><span data-ttu-id="f1407-115">HTTPS erforderlich</span><span class="sxs-lookup"><span data-stu-id="f1407-115">Require HTTPS</span></span>

<span data-ttu-id="f1407-116">Die [RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) wird verwendet, um HTTPS erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="f1407-116">The [RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) is used to require HTTPS.</span></span> <span data-ttu-id="f1407-117">`[RequireHttpsAttribute]` ergänzen können, Controllern oder Methoden oder global angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="f1407-117">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="f1407-118">Um das Attribut global anzuwenden, fügen Sie den folgenden Code zum `ConfigureServices` in `Startup`:</span><span class="sxs-lookup"><span data-stu-id="f1407-118">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="f1407-119">Die vorherige hervorgehobene Code erfordert, verwenden alle Anforderungen `HTTPS`daher HTTP-Anforderungen werden ignoriert.</span><span class="sxs-lookup"><span data-stu-id="f1407-119">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="f1407-120">Die folgende hervorgehobene Code leitet alle HTTP-Anforderungen an HTTPS:</span><span class="sxs-lookup"><span data-stu-id="f1407-120">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="f1407-121">Weitere Informationen finden Sie unter [URL umschreiben Middleware](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="f1407-121">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>

<span data-ttu-id="f1407-122">Verwendung von HTTPS Global (`options.Filters.Add(new RequireHttpsAttribute());`) ist eine bewährte Sicherheitsmethode.</span><span class="sxs-lookup"><span data-stu-id="f1407-122">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="f1407-123">Anwenden der `[RequireHttps]` Attribut für alle Controller/Razor-Seiten wird nicht so sicher wie die Verwendung von HTTPS global betrachtet.</span><span class="sxs-lookup"><span data-stu-id="f1407-123">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="f1407-124">Sie können nicht gewährleisten die `[RequireHttps]` -Attribut angewendet wird, wenn neue Domänencontroller und Razor-Seiten hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="f1407-124">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>