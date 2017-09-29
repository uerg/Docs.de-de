---
title: Erzwingen von SSL in einer ASP.NET Core-app
author: rick-anderson
description: Zeigt, wie So fordern Sie SSL in einem Kern der ASP.NET Web-app
keywords: ASP.NET Core "," SSL "," HTTPS "," RequireHttpsAttribute "," IIS Express
ms.author: riande
manager: wpickett
ms.date: 07/19/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/enforcing-ssl
ms.openlocfilehash: 6f2755a606000717ca8a57f045b1ef613c7f14f6
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/28/2017
---
# <a name="enforcing-ssl-in-an-aspnet-core-app"></a><span data-ttu-id="6d800-104">Erzwingen von SSL in einer ASP.NET Core-app</span><span class="sxs-lookup"><span data-stu-id="6d800-104">Enforcing SSL in an ASP.NET Core app</span></span>

<span data-ttu-id="6d800-105">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="6d800-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="6d800-106">Dieses Dokument zeigt, wie Sie:</span><span class="sxs-lookup"><span data-stu-id="6d800-106">This document shows how to:</span></span>

- <span data-ttu-id="6d800-107">Erfordern Sie SSL für alle Anforderungen (gilt nur für HTTPS-Anforderungen).</span><span class="sxs-lookup"><span data-stu-id="6d800-107">Require SSL for all requests (HTTPS requests only).</span></span>
- <span data-ttu-id="6d800-108">Umleiten Sie alle HTTP-Anforderungen an HTTPS.</span><span class="sxs-lookup"><span data-stu-id="6d800-108">Redirect all HTTP requests to HTTPS.</span></span>

## <a name="require-ssl"></a><span data-ttu-id="6d800-109">Anfordern von SSL</span><span class="sxs-lookup"><span data-stu-id="6d800-109">Require SSL</span></span>

<span data-ttu-id="6d800-110">Die [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) wird verwendet, um SSL erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="6d800-110">The [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require SSL.</span></span> <span data-ttu-id="6d800-111">Controllern oder Methoden mit diesem Attribut ergänzen kann, oder Sie können es Global wie folgt anwenden:</span><span class="sxs-lookup"><span data-stu-id="6d800-111">You can decorate controllers or methods with this attribute or you can apply it globally as shown below:</span></span>

<span data-ttu-id="6d800-112">Fügen Sie folgenden Code zum `ConfigureServices` in `Startup`:</span><span class="sxs-lookup"><span data-stu-id="6d800-112">Add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-)]

<span data-ttu-id="6d800-113">Der hervorgehobene Code oben ist erforderlich, alle Anforderungen verwenden `HTTPS`, daher HTTP-Anforderungen werden ignoriert.</span><span class="sxs-lookup"><span data-stu-id="6d800-113">The highlighted code above requires all requests use `HTTPS`, therefore HTTP requests are ignored.</span></span> <span data-ttu-id="6d800-114">Die folgende hervorgehobene Code leitet alle HTTP-Anforderungen an HTTPS:</span><span class="sxs-lookup"><span data-stu-id="6d800-114">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-)]

<span data-ttu-id="6d800-115">Finden Sie unter [URL umschreiben Middleware](xref:fundamentals/url-rewriting) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="6d800-115">See [URL Rewriting Middleware](xref:fundamentals/url-rewriting) for more information.</span></span>

<span data-ttu-id="6d800-116">Verwendung von HTTPS Global (`options.Filters.Add(new RequireHttpsAttribute());`) ist eine bewährte Sicherheitsmethode.</span><span class="sxs-lookup"><span data-stu-id="6d800-116">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="6d800-117">Anwenden der `[RequireHttps]` Attribut, um alle Controller ist nicht so sicher wie die Verwendung von HTTPS Global berücksichtigt.</span><span class="sxs-lookup"><span data-stu-id="6d800-117">Applying the `[RequireHttps]` attribute to all controller is not considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="6d800-118">Sie können nicht gewährleisten neue Domänencontroller hinzugefügt, um Ihre app werden Denken Sie daran, gelten die `[RequireHttps]` Attribut.</span><span class="sxs-lookup"><span data-stu-id="6d800-118">You can't guarantee new controllers added to your app will remember to apply the `[RequireHttps]` attribute.</span></span>
