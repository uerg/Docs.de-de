---
title: Client-IP-Listen sicherer Adressen für ASP.NET Core
author: damienbod
description: Erfahren Sie, wie Middleware oder Aktion Schreibfilter zum remote-IP-Adressen mit einer Liste der zulässigen IP-Adressen zu überprüfen.
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/31/2018
uid: security/ip-safelist
ms.openlocfilehash: 40fe7b67359efd1692490099c3fb529ba4a6148f
ms.sourcegitcommit: 08bf41d4b3e696ab512b044970e8304816f8cc56
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2018
ms.locfileid: "44040109"
---
# <a name="client-ip-safelist-for-aspnet-core"></a><span data-ttu-id="450d4-103">Client-IP-Listen sicherer Adressen für ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="450d4-103">Client IP safelist for ASP.NET Core</span></span>

<span data-ttu-id="450d4-104">Durch [Damien Bowden](https://twitter.com/damien_bod) und [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="450d4-104">By [Damien Bowden](https://twitter.com/damien_bod) and [Tom Dykstra](https://github.com/tdykstra)</span></span>
 
<span data-ttu-id="450d4-105">Dieser Artikel zeigt zwei Möglichkeiten, eine IP-Listen sicherer Adressen (auch bekannt als eine Whitelist) zu implementieren:</span><span class="sxs-lookup"><span data-stu-id="450d4-105">This article shows two ways to implement an IP safelist (also known as a whitelist):</span></span>

* <span data-ttu-id="450d4-106">Mithilfe von ASP.NET Core-Middleware die remote IP-Adresse jeder Anforderung überprüfen.</span><span class="sxs-lookup"><span data-stu-id="450d4-106">By using ASP.NET Core middleware to check the remote IP address of every request.</span></span>
* <span data-ttu-id="450d4-107">Mithilfe von ASP.NET Core-Aktionsfilter um die Anforderungen für bestimmte Aktionsmethoden remote IP-Adresse zu überprüfen.</span><span class="sxs-lookup"><span data-stu-id="450d4-107">By using ASP.NET Core action filters to check the remote IP address of requests for specific action methods.</span></span>

<span data-ttu-id="450d4-108">Die Beispiel-app zeigt beide Ansätze.</span><span class="sxs-lookup"><span data-stu-id="450d4-108">The sample app illustrates both approaches.</span></span> <span data-ttu-id="450d4-109">In jedem Fall wird eine Zeichenfolge, die genehmigte Client-IP-Adressen in einer app-Einstellung gespeichert.</span><span class="sxs-lookup"><span data-stu-id="450d4-109">In each case, a string containing approved client IP addresses is stored in an app setting.</span></span> <span data-ttu-id="450d4-110">Die Middleware oder Filter analysiert die Zeichenfolge in eine Liste, und überprüft, ob die remote-IP in der Liste ist.</span><span class="sxs-lookup"><span data-stu-id="450d4-110">The middleware or filter parses the string into a list and  checks if the remote IP is in the list.</span></span> <span data-ttu-id="450d4-111">Wenn dies nicht der Fall ist, ein HTTP 403 Verboten-Statuscode zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="450d4-111">If not, an HTTP 403 Forbidden status code is returned.</span></span>

<span data-ttu-id="450d4-112">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/ip-safelist/samples/2.x/ClientIpAspNetCore) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="450d4-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/ip-safelist/samples/2.x/ClientIpAspNetCore) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="the-safelist"></a><span data-ttu-id="450d4-113">Die Listen sicherer Adressen</span><span class="sxs-lookup"><span data-stu-id="450d4-113">The safelist</span></span>

<span data-ttu-id="450d4-114">Die Liste ist so konfiguriert, der *"appSettings.JSON"* Datei.</span><span class="sxs-lookup"><span data-stu-id="450d4-114">The list is configured in the *appsettings.json* file.</span></span> <span data-ttu-id="450d4-115">Dabei handelt es sich eine durch Semikolons getrennte Liste kann IPv4 und IPv6-Adressen enthalten.</span><span class="sxs-lookup"><span data-stu-id="450d4-115">It's a semicolon-delimited list and can contain IPv4 and IPv6 addresses.</span></span>

[!code-json[](ip-safelist/samples/2.x/ClientIpAspNetCore/appsettings.json?highlight=2)]

## <a name="middleware"></a><span data-ttu-id="450d4-116">Middleware</span><span class="sxs-lookup"><span data-stu-id="450d4-116">Middleware</span></span>

<span data-ttu-id="450d4-117">Die `Configure` Methode fügt die Middleware und die Zeichenfolge von Listen sicherer Adressen in einem Konstruktorparameter an diese übergibt.</span><span class="sxs-lookup"><span data-stu-id="450d4-117">The `Configure` method adds the middleware and passes the safelist string to it in a constructor parameter.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_Configure&highlight=7)]

<span data-ttu-id="450d4-118">Die Middleware analysiert die Zeichenfolge in ein Array und sucht nach der remote-IP-Adresse in das Array.</span><span class="sxs-lookup"><span data-stu-id="450d4-118">The middleware parses the string into an array and looks for the remote IP address in the array.</span></span> <span data-ttu-id="450d4-119">Wenn die remote-IP-Adresse nicht gefunden wird, gibt die Middleware zurück, die HTTP-401 nicht zulässig.</span><span class="sxs-lookup"><span data-stu-id="450d4-119">If the remote IP address is not found, the middleware returns HTTP 401 Forbidden.</span></span> <span data-ttu-id="450d4-120">Dieser Überprüfungsvorgang wird umgangen, um die HTTP Get-Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="450d4-120">This validation process is bypassed for HTTP Get requests.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/AdminSafeListMiddleware.cs?name=snippet_ClassOnly)]

## <a name="action-filter"></a><span data-ttu-id="450d4-121">Action-filter</span><span class="sxs-lookup"><span data-stu-id="450d4-121">Action filter</span></span>

<span data-ttu-id="450d4-122">Wenn Sie eine von Listen sicherer Adressen nur für bestimmte Controller oder Aktionsmethoden anwenden möchten, verwenden Sie einen Aktionsfilter.</span><span class="sxs-lookup"><span data-stu-id="450d4-122">If you want a safelist only for specific controllers or action methods, use an action filter.</span></span> <span data-ttu-id="450d4-123">Im Folgenden ein Beispiel:</span><span class="sxs-lookup"><span data-stu-id="450d4-123">Here's an example:</span></span> 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIdCheckFilter.cs)]

<span data-ttu-id="450d4-124">Der Action-Filter wird zum Dienstcontainer hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="450d4-124">The action filter is added to the services container.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

<span data-ttu-id="450d4-125">Der Filter kann dann für eine Methode Controller bzw. die Aktionsmethode verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="450d4-125">The filter can then be used on a controller or action method.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Controllers/ValuesController.cs?name=snippet_Filter&highlight=1)]

<span data-ttu-id="450d4-126">In der Beispiel-app der Filter angewendet wird, auf die `Get` Methode.</span><span class="sxs-lookup"><span data-stu-id="450d4-126">In the sample app, the filter is applied to the `Get` method.</span></span> <span data-ttu-id="450d4-127">Wenn die app zu, indem Sie senden testen also eine `Get` API anfordern, die das Attribut ist die Client-IP-Adresse überprüfen.</span><span class="sxs-lookup"><span data-stu-id="450d4-127">So when you test the app by sending a `Get` API request, the attribute is validating the client IP address.</span></span> <span data-ttu-id="450d4-128">Wenn Sie testen, indem Sie die API mit anderen HTTP-Methode aufrufen, ist die Middleware die Client-IP-Adresse überprüfen.</span><span class="sxs-lookup"><span data-stu-id="450d4-128">When you test by calling the API with any other HTTP method, the middleware is validating the client IP.</span></span>

## <a name="razor-pages-filter"></a><span data-ttu-id="450d4-129">Razor-Seiten zu filtern.</span><span class="sxs-lookup"><span data-stu-id="450d4-129">Razor Pages filter</span></span> 

<span data-ttu-id="450d4-130">Wenn Sie eine von Listen sicherer Adressen für eine Razor-Seiten-app möchten, verwenden Sie einen Razor-Seiten-Filter.</span><span class="sxs-lookup"><span data-stu-id="450d4-130">If you want a safelist for a Razor Pages app, use a Razor Pages filter.</span></span> <span data-ttu-id="450d4-131">Im Folgenden ein Beispiel:</span><span class="sxs-lookup"><span data-stu-id="450d4-131">Here's an example:</span></span> 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIdCheckPageFilter.cs)]

<span data-ttu-id="450d4-132">Dieser Filter ist aktiviert, indem sie auf die Auflistung der MVC-Filter hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="450d4-132">This filter is enabled by adding it to the MVC Filters collection.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

<span data-ttu-id="450d4-133">Wenn Sie die app ausführen und eine Razor-Seite anfordern, ist der Filter für die Razor-Seiten die Client-IP-Adresse überprüfen.</span><span class="sxs-lookup"><span data-stu-id="450d4-133">When you run the app and request a Razor page, the Razor Pages filter is validating the client IP.</span></span>

## <a name="next-steps"></a><span data-ttu-id="450d4-134">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="450d4-134">Next steps</span></span>

<span data-ttu-id="450d4-135">[Erfahren Sie mehr über ASP.NET Core-Middleware](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="450d4-135">[Learn more about ASP.NET Core Middleware](xref:fundamentals/middleware/index).</span></span>
