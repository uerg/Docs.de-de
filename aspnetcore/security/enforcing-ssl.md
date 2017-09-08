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
ms.openlocfilehash: 35554939bd574b2826053004ed437bee46154c2b
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2017
---
# <a name="enforcing-ssl-in-an-aspnet-core-app"></a><span data-ttu-id="94d62-104">Erzwingen von SSL in einer ASP.NET Core-app</span><span class="sxs-lookup"><span data-stu-id="94d62-104">Enforcing SSL in an ASP.NET Core app</span></span>

<span data-ttu-id="94d62-105">Durch [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="94d62-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="94d62-106">Dieses Dokument zeigt, wie Sie:</span><span class="sxs-lookup"><span data-stu-id="94d62-106">This document shows how to:</span></span>

- <span data-ttu-id="94d62-107">Erfordern Sie SSL für alle Anforderungen (gilt nur für HTTPS-Anforderungen).</span><span class="sxs-lookup"><span data-stu-id="94d62-107">Require SSL for all requests (HTTPS requests only).</span></span>
- <span data-ttu-id="94d62-108">Umleiten Sie alle HTTP-Anforderungen an HTTPS.</span><span class="sxs-lookup"><span data-stu-id="94d62-108">Redirect all HTTP requests to HTTPS.</span></span>

## <a name="require-ssl"></a><span data-ttu-id="94d62-109">SSL erforderlich</span><span class="sxs-lookup"><span data-stu-id="94d62-109">Require SSL</span></span>

<span data-ttu-id="94d62-110">Die [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) wird verwendet, um SSL erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="94d62-110">The [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require SSL.</span></span> <span data-ttu-id="94d62-111">Controllern oder Methoden mit diesem Attribut ergänzen kann, oder Sie können es Global wie folgt anwenden:</span><span class="sxs-lookup"><span data-stu-id="94d62-111">You can decorate controllers or methods with this attribute or you can apply it globally as shown below:</span></span>

<span data-ttu-id="94d62-112">Fügen Sie folgenden Code zum `ConfigureServices` in `Startup`:</span><span class="sxs-lookup"><span data-stu-id="94d62-112">Add the following code to `ConfigureServices` in `Startup`:</span></span>

<span data-ttu-id="94d62-113">[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-)]</span><span class="sxs-lookup"><span data-stu-id="94d62-113">[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-)]</span></span>

<span data-ttu-id="94d62-114">Der hervorgehobene Code oben ist erforderlich, alle Anforderungen verwenden `HTTPS`, daher HTTP-Anforderungen werden ignoriert.</span><span class="sxs-lookup"><span data-stu-id="94d62-114">The highlighted code above requires all requests use `HTTPS`, therefore HTTP requests are ignored.</span></span> <span data-ttu-id="94d62-115">Die folgende hervorgehobene Code leitet alle HTTP-Anforderungen an HTTPS:</span><span class="sxs-lookup"><span data-stu-id="94d62-115">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

<span data-ttu-id="94d62-116">[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-)]</span><span class="sxs-lookup"><span data-stu-id="94d62-116">[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-)]</span></span>

<span data-ttu-id="94d62-117">Finden Sie unter [URL umschreiben Middleware](xref:fundamentals/url-rewriting) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="94d62-117">See [URL Rewriting Middleware](xref:fundamentals/url-rewriting) for more information.</span></span>

<span data-ttu-id="94d62-118">Verwendung von HTTPS Global (`options.Filters.Add(new RequireHttpsAttribute());`) ist eine bewährte Sicherheitsmethode.</span><span class="sxs-lookup"><span data-stu-id="94d62-118">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="94d62-119">Anwenden der `[RequireHttps]` Attribut, um alle Controller ist nicht so sicher wie die Verwendung von HTTPS Global berücksichtigt.</span><span class="sxs-lookup"><span data-stu-id="94d62-119">Applying the `[RequireHttps]` attribute to all controller is not considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="94d62-120">Sie können nicht gewährleisten neue Domänencontroller hinzugefügt, um Ihre app werden Denken Sie daran, gelten die `[RequireHttps]` Attribut.</span><span class="sxs-lookup"><span data-stu-id="94d62-120">You can't guarantee new controllers added to your app will remember to apply the `[RequireHttps]` attribute.</span></span>

## <a name="set-up-iis-express-for-sslhttps"></a><span data-ttu-id="94d62-121">Einrichten von IIS Express für SSL/HTTPS</span><span class="sxs-lookup"><span data-stu-id="94d62-121">Set up IIS Express for SSL/HTTPS</span></span>

<span data-ttu-id="94d62-122">Finden Sie unter [Einrichten der HTTPS-Entwicklung in ASP.NET Core](xref:security/https#iisxpress).</span><span class="sxs-lookup"><span data-stu-id="94d62-122">See [Setting up HTTPS for development in ASP.NET Core](xref:security/https#iisxpress).</span></span>
