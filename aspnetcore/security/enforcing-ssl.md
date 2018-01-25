---
title: Erzwingen von SSL in einer ASP.NET Core-app
author: rick-anderson
description: Zeigt, wie So fordern Sie SSL in einem Kern der ASP.NET Web-app
ms.author: riande
manager: wpickett
ms.date: 07/19/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/enforcing-ssl
ms.openlocfilehash: f248e9c0463cf4a46a447a9c896b3276a50f5f08
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
# <a name="enforcing-ssl-in-an-aspnet-core-app"></a><span data-ttu-id="a8648-103">Erzwingen von SSL in einer ASP.NET Core-app</span><span class="sxs-lookup"><span data-stu-id="a8648-103">Enforcing SSL in an ASP.NET Core app</span></span>

<span data-ttu-id="a8648-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a8648-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a8648-105">Dieses Dokument zeigt, wie Sie:</span><span class="sxs-lookup"><span data-stu-id="a8648-105">This document shows how to:</span></span>

- <span data-ttu-id="a8648-106">Erfordern Sie SSL für alle Anforderungen (gilt nur für HTTPS-Anforderungen).</span><span class="sxs-lookup"><span data-stu-id="a8648-106">Require SSL for all requests (HTTPS requests only).</span></span>
- <span data-ttu-id="a8648-107">Umleiten Sie alle HTTP-Anforderungen an HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a8648-107">Redirect all HTTP requests to HTTPS.</span></span>

## <a name="require-ssl"></a><span data-ttu-id="a8648-108">Anfordern von SSL</span><span class="sxs-lookup"><span data-stu-id="a8648-108">Require SSL</span></span>

<span data-ttu-id="a8648-109">Die [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) wird verwendet, um SSL erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="a8648-109">The [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require SSL.</span></span> <span data-ttu-id="a8648-110">Controllern oder Methoden mit diesem Attribut ergänzen kann, oder Sie können es Global wie folgt anwenden:</span><span class="sxs-lookup"><span data-stu-id="a8648-110">You can decorate controllers or methods with this attribute or you can apply it globally as shown below:</span></span>

<span data-ttu-id="a8648-111">Fügen Sie folgenden Code zum `ConfigureServices` in `Startup`:</span><span class="sxs-lookup"><span data-stu-id="a8648-111">Add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-)]

<span data-ttu-id="a8648-112">Der hervorgehobene Code oben ist erforderlich, alle Anforderungen verwenden `HTTPS`, daher HTTP-Anforderungen werden ignoriert.</span><span class="sxs-lookup"><span data-stu-id="a8648-112">The highlighted code above requires all requests use `HTTPS`, therefore HTTP requests are ignored.</span></span> <span data-ttu-id="a8648-113">Die folgende hervorgehobene Code leitet alle HTTP-Anforderungen an HTTPS:</span><span class="sxs-lookup"><span data-stu-id="a8648-113">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-)]

<span data-ttu-id="a8648-114">Finden Sie unter [URL umschreiben Middleware](xref:fundamentals/url-rewriting) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="a8648-114">See [URL Rewriting Middleware](xref:fundamentals/url-rewriting) for more information.</span></span>

<span data-ttu-id="a8648-115">Verwendung von HTTPS Global (`options.Filters.Add(new RequireHttpsAttribute());`) ist eine bewährte Sicherheitsmethode.</span><span class="sxs-lookup"><span data-stu-id="a8648-115">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="a8648-116">Anwenden der `[RequireHttps]` Attribut an alle Controller ist nicht so sicher wie die Verwendung von HTTPS global betrachtet.</span><span class="sxs-lookup"><span data-stu-id="a8648-116">Applying the `[RequireHttps]` attribute to all controller isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="a8648-117">Sie können nicht gewährleisten neue Domänencontroller hinzugefügt, um Ihre app werden Denken Sie daran, gelten die `[RequireHttps]` Attribut.</span><span class="sxs-lookup"><span data-stu-id="a8648-117">You can't guarantee new controllers added to your app will remember to apply the `[RequireHttps]` attribute.</span></span>
