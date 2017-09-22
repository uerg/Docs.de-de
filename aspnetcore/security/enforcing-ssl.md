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
ms.openlocfilehash: e8e7d4a69fd681534fb313ff113805bfd6a6d44e
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2017
---
# <a name="enforcing-ssl-in-an-aspnet-core-app"></a><span data-ttu-id="21e78-104">Erzwingen von SSL in einer ASP.NET Core-app</span><span class="sxs-lookup"><span data-stu-id="21e78-104">Enforcing SSL in an ASP.NET Core app</span></span>

<span data-ttu-id="21e78-105">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="21e78-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="21e78-106">Dieses Dokument zeigt, wie Sie:</span><span class="sxs-lookup"><span data-stu-id="21e78-106">This document shows how to:</span></span>

- <span data-ttu-id="21e78-107">Erfordern Sie SSL für alle Anforderungen (gilt nur für HTTPS-Anforderungen).</span><span class="sxs-lookup"><span data-stu-id="21e78-107">Require SSL for all requests (HTTPS requests only).</span></span>
- <span data-ttu-id="21e78-108">Umleiten Sie alle HTTP-Anforderungen an HTTPS.</span><span class="sxs-lookup"><span data-stu-id="21e78-108">Redirect all HTTP requests to HTTPS.</span></span>

## <a name="require-ssl"></a><span data-ttu-id="21e78-109">Anfordern von SSL</span><span class="sxs-lookup"><span data-stu-id="21e78-109">Require SSL</span></span>

<span data-ttu-id="21e78-110">Die [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) wird verwendet, um SSL erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="21e78-110">The [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require SSL.</span></span> <span data-ttu-id="21e78-111">Controllern oder Methoden mit diesem Attribut ergänzen kann, oder Sie können es Global wie folgt anwenden:</span><span class="sxs-lookup"><span data-stu-id="21e78-111">You can decorate controllers or methods with this attribute or you can apply it globally as shown below:</span></span>

<span data-ttu-id="21e78-112">Fügen Sie folgenden Code zum `ConfigureServices` in `Startup`:</span><span class="sxs-lookup"><span data-stu-id="21e78-112">Add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-)]

<span data-ttu-id="21e78-113">Der hervorgehobene Code oben ist erforderlich, alle Anforderungen verwenden `HTTPS`, daher HTTP-Anforderungen werden ignoriert.</span><span class="sxs-lookup"><span data-stu-id="21e78-113">The highlighted code above requires all requests use `HTTPS`, therefore HTTP requests are ignored.</span></span> <span data-ttu-id="21e78-114">Die folgende hervorgehobene Code leitet alle HTTP-Anforderungen an HTTPS:</span><span class="sxs-lookup"><span data-stu-id="21e78-114">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-)]

<span data-ttu-id="21e78-115">Finden Sie unter [URL umschreiben Middleware](xref:fundamentals/url-rewriting) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="21e78-115">See [URL Rewriting Middleware](xref:fundamentals/url-rewriting) for more information.</span></span>

<span data-ttu-id="21e78-116">Verwendung von HTTPS Global (`options.Filters.Add(new RequireHttpsAttribute());`) ist eine bewährte Sicherheitsmethode.</span><span class="sxs-lookup"><span data-stu-id="21e78-116">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="21e78-117">Anwenden der `[RequireHttps]` Attribut, um alle Controller ist nicht so sicher wie die Verwendung von HTTPS Global berücksichtigt.</span><span class="sxs-lookup"><span data-stu-id="21e78-117">Applying the `[RequireHttps]` attribute to all controller is not considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="21e78-118">Sie können nicht gewährleisten neue Domänencontroller hinzugefügt, um Ihre app werden Denken Sie daran, gelten die `[RequireHttps]` Attribut.</span><span class="sxs-lookup"><span data-stu-id="21e78-118">You can't guarantee new controllers added to your app will remember to apply the `[RequireHttps]` attribute.</span></span>

## <a name="set-up-iis-express-for-sslhttps"></a><span data-ttu-id="21e78-119">Einrichten von IIS Express für SSL/HTTPS</span><span class="sxs-lookup"><span data-stu-id="21e78-119">Set up IIS Express for SSL/HTTPS</span></span>

<span data-ttu-id="21e78-120">Finden Sie unter [Einrichten der HTTPS-Entwicklung in ASP.NET Core](xref:security/https#iisxpress).</span><span class="sxs-lookup"><span data-stu-id="21e78-120">See [Setting up HTTPS for development in ASP.NET Core](xref:security/https#iisxpress).</span></span>
