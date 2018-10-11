---
title: Cache-Taghilfsprogramm im ASP.NET Core MVC
author: pkellner
description: Erfahren Sie, wie das Cache-Taghilfsprogramm verwendet wird.
ms.author: riande
ms.date: 02/14/2017
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: 11754d2858d8f02c7eb9baac8feda9b50ddb3d79
ms.sourcegitcommit: 4d5f8680d68b39c411b46c73f7014f8aa0f12026
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47028154"
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a><span data-ttu-id="ac4a6-103">Cache-Taghilfsprogramm im ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="ac4a6-103">Cache Tag Helper in ASP.NET Core MVC</span></span>

<span data-ttu-id="ac4a6-104">Von [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="ac4a6-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="ac4a6-105">Durch das Cache-Taghilfsprogramm kann die Leistung Ihrer ASP.NET Core-App erheblich verbessert werden, indem deren Inhalte im internen ASP.NET Core-Cacheanbieter zwischengespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="ac4a6-105">The Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to the internal ASP.NET Core cache provider.</span></span>

<span data-ttu-id="ac4a6-106">Die Razor-Ansichtsengine legt für `expires-after` einen Standardwert von 20 Minuten fest.</span><span class="sxs-lookup"><span data-stu-id="ac4a6-106">The Razor View Engine sets the default `expires-after` to twenty minutes.</span></span>

<span data-ttu-id="ac4a6-107">Das folgende Razor-Markup speichert das Datum bzw. die Zeit zwischen:</span><span class="sxs-lookup"><span data-stu-id="ac4a6-107">The following Razor markup caches the date/time:</span></span>

```cshtml
<cache>@DateTime.Now</cache>
```

<span data-ttu-id="ac4a6-108">Über die erste Anforderung an die Seite, die `CacheTagHelper` enthält, wird das aktuelle Datum bzw. die aktuelle Uhrzeit zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="ac4a6-108">The first request to the page that contains `CacheTagHelper` will display the current date/time.</span></span> <span data-ttu-id="ac4a6-109">Über zusätzliche Anforderungen werden die zwischengespeicherten Werte angezeigt, bis der Cache abläuft (standardmäßig 20 Minuten lang) oder aufgrund von Speichermangel entfernt wird.</span><span class="sxs-lookup"><span data-stu-id="ac4a6-109">Additional requests will show the cached value until the cache expires (default 20 minutes) or is evicted by memory pressure.</span></span>

<span data-ttu-id="ac4a6-110">Sie können die Aufbewahrungsdauer im Cache mithilfe der folgenden Attribute festlegen:</span><span class="sxs-lookup"><span data-stu-id="ac4a6-110">You can set the cache duration with the following attributes:</span></span>

## <a name="cache-tag-helper-attributes"></a><span data-ttu-id="ac4a6-111">Attribute von Cache-Taghilfsprogrammen</span><span class="sxs-lookup"><span data-stu-id="ac4a6-111">Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled"></a><span data-ttu-id="ac4a6-112">enabled</span><span class="sxs-lookup"><span data-stu-id="ac4a6-112">enabled</span></span>    


| <span data-ttu-id="ac4a6-113">Attributtyp</span><span class="sxs-lookup"><span data-stu-id="ac4a6-113">Attribute Type</span></span>    | <span data-ttu-id="ac4a6-114">Gültige Werte</span><span class="sxs-lookup"><span data-stu-id="ac4a6-114">Valid Values</span></span>      |
|----------------   |----------------   |
| <span data-ttu-id="ac4a6-115">boolean</span><span class="sxs-lookup"><span data-stu-id="ac4a6-115">boolean</span></span>           | <span data-ttu-id="ac4a6-116">TRUE (Standardwert)</span><span class="sxs-lookup"><span data-stu-id="ac4a6-116">"true" (default)</span></span>  |
|                   | <span data-ttu-id="ac4a6-117">"false"</span><span class="sxs-lookup"><span data-stu-id="ac4a6-117">"false"</span></span>   |


<span data-ttu-id="ac4a6-118">Legt fest, ob der Inhalt zwischengespeichert wird, der vom Cache-Taghilfsprogramm eingeschlossen wird.</span><span class="sxs-lookup"><span data-stu-id="ac4a6-118">Determines whether the content enclosed by the Cache Tag Helper is cached.</span></span> <span data-ttu-id="ac4a6-119">Die Standardeinstellung ist `true`.</span><span class="sxs-lookup"><span data-stu-id="ac4a6-119">The default is `true`.</span></span>  <span data-ttu-id="ac4a6-120">Wenn das Cache-Taghilfsprogramm auf `false` festgelegt ist, wird die gerenderte Ausgabe nicht zwischengespeichert.</span><span class="sxs-lookup"><span data-stu-id="ac4a6-120">If set to `false` this Cache Tag Helper will have no caching effect on the rendered output.</span></span>

<span data-ttu-id="ac4a6-121">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="ac4a6-121">Example:</span></span>

```cshtml
<cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-on"></a><span data-ttu-id="ac4a6-122">expires-on</span><span class="sxs-lookup"><span data-stu-id="ac4a6-122">expires-on</span></span> 

| <span data-ttu-id="ac4a6-123">Attributtyp</span><span class="sxs-lookup"><span data-stu-id="ac4a6-123">Attribute Type</span></span> |           <span data-ttu-id="ac4a6-124">Beispielwert</span><span class="sxs-lookup"><span data-stu-id="ac4a6-124">Example Value</span></span>            |
|----------------|------------------------------------|
| <span data-ttu-id="ac4a6-125">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="ac4a6-125">DateTimeOffset</span></span> | <span data-ttu-id="ac4a6-126">"@new DateTime(2025,1,29,17,02,0)"</span><span class="sxs-lookup"><span data-stu-id="ac4a6-126">"@new DateTime(2025,1,29,17,02,0)"</span></span> |

<span data-ttu-id="ac4a6-127">Legt die absolute Ablaufzeit fest.</span><span class="sxs-lookup"><span data-stu-id="ac4a6-127">Sets an absolute expiration date.</span></span> <span data-ttu-id="ac4a6-128">Im folgenden Beispiel werden die Inhalte des Cache-Taghilfsprogramms bis zum 29. Januar 2025 um 17:02 Uhr zwischengespeichert.</span><span class="sxs-lookup"><span data-stu-id="ac4a6-128">The following example will cache the contents of the Cache Tag Helper until 5:02 PM on January 29, 2025.</span></span>

<span data-ttu-id="ac4a6-129">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="ac4a6-129">Example:</span></span>

```cshtml
<cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-after"></a><span data-ttu-id="ac4a6-130">expires-after</span><span class="sxs-lookup"><span data-stu-id="ac4a6-130">expires-after</span></span>

| <span data-ttu-id="ac4a6-131">Attributtyp</span><span class="sxs-lookup"><span data-stu-id="ac4a6-131">Attribute Type</span></span> |        <span data-ttu-id="ac4a6-132">Beispielwert</span><span class="sxs-lookup"><span data-stu-id="ac4a6-132">Example Value</span></span>         |
|----------------|------------------------------|
|    <span data-ttu-id="ac4a6-133">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="ac4a6-133">TimeSpan</span></span>    | <span data-ttu-id="ac4a6-134">"@TimeSpan.FromSeconds(120)"</span><span class="sxs-lookup"><span data-stu-id="ac4a6-134">"@TimeSpan.FromSeconds(120)"</span></span> |

<span data-ttu-id="ac4a6-135">Legt die Zeitspanne ab der ersten Anforderungszeit fest, um die Inhalte zwischenzuspeichern.</span><span class="sxs-lookup"><span data-stu-id="ac4a6-135">Sets the length of time from the first request time to cache the contents.</span></span> 

<span data-ttu-id="ac4a6-136">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="ac4a6-136">Example:</span></span>

```cshtml
<cache expires-after="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-sliding"></a><span data-ttu-id="ac4a6-137">expires-sliding</span><span class="sxs-lookup"><span data-stu-id="ac4a6-137">expires-sliding</span></span>

| <span data-ttu-id="ac4a6-138">Attributtyp</span><span class="sxs-lookup"><span data-stu-id="ac4a6-138">Attribute Type</span></span> |        <span data-ttu-id="ac4a6-139">Beispielwert</span><span class="sxs-lookup"><span data-stu-id="ac4a6-139">Example Value</span></span>        |
|----------------|-----------------------------|
|    <span data-ttu-id="ac4a6-140">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="ac4a6-140">TimeSpan</span></span>    | <span data-ttu-id="ac4a6-141">"@TimeSpan.FromSeconds(60)"</span><span class="sxs-lookup"><span data-stu-id="ac4a6-141">"@TimeSpan.FromSeconds(60)"</span></span> |

<span data-ttu-id="ac4a6-142">Legt die Zeit fest, nach der ein Cacheeintrag gelöscht werden soll, wenn niemand auf diesen zugegriffen hat.</span><span class="sxs-lookup"><span data-stu-id="ac4a6-142">Sets the time that a cache entry should be evicted if it has not been accessed.</span></span>

<span data-ttu-id="ac4a6-143">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="ac4a6-143">Example:</span></span>

```cshtml
<cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-header"></a><span data-ttu-id="ac4a6-144">vary-by-header</span><span class="sxs-lookup"><span data-stu-id="ac4a6-144">vary-by-header</span></span>

| <span data-ttu-id="ac4a6-145">Attributtyp</span><span class="sxs-lookup"><span data-stu-id="ac4a6-145">Attribute Type</span></span>    | <span data-ttu-id="ac4a6-146">Beispielwerte</span><span class="sxs-lookup"><span data-stu-id="ac4a6-146">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="ac4a6-147">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="ac4a6-147">String</span></span>            | <span data-ttu-id="ac4a6-148">"User-Agent"</span><span class="sxs-lookup"><span data-stu-id="ac4a6-148">"User-Agent"</span></span>                  |
|                   | <span data-ttu-id="ac4a6-149">"User-Agent,content-encoding"</span><span class="sxs-lookup"><span data-stu-id="ac4a6-149">"User-Agent,content-encoding"</span></span> |

<span data-ttu-id="ac4a6-150">Akzeptiert einen einzelnen Headerwert oder eine durch Kommas getrennte Liste von Headerwerten, die eine Cacheaktualisierung auslösen, wenn diese geändert werden.</span><span class="sxs-lookup"><span data-stu-id="ac4a6-150">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when they change.</span></span> <span data-ttu-id="ac4a6-151">Im folgenden Beispiel wird der Headerwert `User-Agent` überwacht.</span><span class="sxs-lookup"><span data-stu-id="ac4a6-151">The following example monitors the header value `User-Agent`.</span></span> <span data-ttu-id="ac4a6-152">Außerdem werden die Inhalte für alle `User-Agent` zwischengespeichert, die dem Webserver präsentiert werden.</span><span class="sxs-lookup"><span data-stu-id="ac4a6-152">The example will cache the content for every different `User-Agent` presented to the web server.</span></span>

<span data-ttu-id="ac4a6-153">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="ac4a6-153">Example:</span></span>

```cshtml
<cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-query"></a><span data-ttu-id="ac4a6-154">vary-by-query</span><span class="sxs-lookup"><span data-stu-id="ac4a6-154">vary-by-query</span></span>

| <span data-ttu-id="ac4a6-155">Attributtyp</span><span class="sxs-lookup"><span data-stu-id="ac4a6-155">Attribute Type</span></span>    | <span data-ttu-id="ac4a6-156">Beispielwerte</span><span class="sxs-lookup"><span data-stu-id="ac4a6-156">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="ac4a6-157">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="ac4a6-157">String</span></span>            | <span data-ttu-id="ac4a6-158">"Make"</span><span class="sxs-lookup"><span data-stu-id="ac4a6-158">"Make"</span></span>                |
|                   | <span data-ttu-id="ac4a6-159">"Make,Model"</span><span class="sxs-lookup"><span data-stu-id="ac4a6-159">"Make,Model"</span></span> |

<span data-ttu-id="ac4a6-160">Akzeptiert einen einzelnen Headerwert oder eine durch Kommas getrennte Liste von Headerwerten, die eine Cacheaktualisierung auslösen, wenn diese geändert werden.</span><span class="sxs-lookup"><span data-stu-id="ac4a6-160">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header value changes.</span></span> <span data-ttu-id="ac4a6-161">Das folgende Beispiel überprüft die Werte von `Make` und `Model`.</span><span class="sxs-lookup"><span data-stu-id="ac4a6-161">The following example looks at the values of `Make` and `Model`.</span></span>

<span data-ttu-id="ac4a6-162">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="ac4a6-162">Example:</span></span>

```cshtml
<cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-route"></a><span data-ttu-id="ac4a6-163">vary-by-route</span><span class="sxs-lookup"><span data-stu-id="ac4a6-163">vary-by-route</span></span>

| <span data-ttu-id="ac4a6-164">Attributtyp</span><span class="sxs-lookup"><span data-stu-id="ac4a6-164">Attribute Type</span></span>    | <span data-ttu-id="ac4a6-165">Beispielwerte</span><span class="sxs-lookup"><span data-stu-id="ac4a6-165">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="ac4a6-166">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="ac4a6-166">String</span></span>            | <span data-ttu-id="ac4a6-167">"Make"</span><span class="sxs-lookup"><span data-stu-id="ac4a6-167">"Make"</span></span>                |
|                   | <span data-ttu-id="ac4a6-168">"Make,Model"</span><span class="sxs-lookup"><span data-stu-id="ac4a6-168">"Make,Model"</span></span> |

<span data-ttu-id="ac4a6-169">Akzeptiert einen einzelnen Headerwert oder eine durch Kommas getrennte Liste von Headerwerten, die eine Cacheaktualisierung auslösen, wenn die Parameterwerte der Routendaten geändert werden.</span><span class="sxs-lookup"><span data-stu-id="ac4a6-169">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the route data parameter value(s) change.</span></span> <span data-ttu-id="ac4a6-170">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="ac4a6-170">Example:</span></span>

<span data-ttu-id="ac4a6-171">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="ac4a6-171">*Startup.cs*</span></span> 

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```

<span data-ttu-id="ac4a6-172">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="ac4a6-172">*Index.cshtml*</span></span>

```cshtml
<cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-cookie"></a><span data-ttu-id="ac4a6-173">vary-by-cookie</span><span class="sxs-lookup"><span data-stu-id="ac4a6-173">vary-by-cookie</span></span>

| <span data-ttu-id="ac4a6-174">Attributtyp</span><span class="sxs-lookup"><span data-stu-id="ac4a6-174">Attribute Type</span></span>    | <span data-ttu-id="ac4a6-175">Beispielwerte</span><span class="sxs-lookup"><span data-stu-id="ac4a6-175">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="ac4a6-176">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="ac4a6-176">String</span></span>            | <span data-ttu-id="ac4a6-177">".AspNetCore.Identity.Application"</span><span class="sxs-lookup"><span data-stu-id="ac4a6-177">".AspNetCore.Identity.Application"</span></span>                |
|                   | <span data-ttu-id="ac4a6-178">".AspNetCore.Identity.Application,HairColor"</span><span class="sxs-lookup"><span data-stu-id="ac4a6-178">".AspNetCore.Identity.Application,HairColor"</span></span> |

<span data-ttu-id="ac4a6-179">Akzeptiert einen einzelnen Headerwert oder eine durch Kommas getrennte Liste von Headerwerten, die eine Cacheaktualisierung auslösen, wenn diese geändert werden.</span><span class="sxs-lookup"><span data-stu-id="ac4a6-179">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header values(s) change.</span></span> <span data-ttu-id="ac4a6-180">Das folgende Beispiel überprüft das Cookie, das der ASP.NET Core Identity zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="ac4a6-180">The following example looks at the cookie associated with ASP.NET Core Identity.</span></span> <span data-ttu-id="ac4a6-181">Wenn ein Benutzer authentifiziert wird, muss der Anforderungscookie festgelegt werden. Dadurch wird eine Cacheaktualisierung ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="ac4a6-181">When a user is authenticated the request cookie to be set which triggers a cache refresh.</span></span>

<span data-ttu-id="ac4a6-182">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="ac4a6-182">Example:</span></span>

```cshtml
<cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-user"></a><span data-ttu-id="ac4a6-183">vary-by-user</span><span class="sxs-lookup"><span data-stu-id="ac4a6-183">vary-by-user</span></span>

| <span data-ttu-id="ac4a6-184">Attributtyp</span><span class="sxs-lookup"><span data-stu-id="ac4a6-184">Attribute Type</span></span>    | <span data-ttu-id="ac4a6-185">Beispielwerte</span><span class="sxs-lookup"><span data-stu-id="ac4a6-185">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="ac4a6-186">Boolesch</span><span class="sxs-lookup"><span data-stu-id="ac4a6-186">Boolean</span></span>             | <span data-ttu-id="ac4a6-187">"true"</span><span class="sxs-lookup"><span data-stu-id="ac4a6-187">"true"</span></span>                  |
|                     | <span data-ttu-id="ac4a6-188">FALSE (Standardwert)</span><span class="sxs-lookup"><span data-stu-id="ac4a6-188">"false" (default)</span></span> |

<span data-ttu-id="ac4a6-189">Gibt an, ob der Cache zurückgesetzt werden soll, wenn sich ein anderer Benutzer anmeldet, also das Kontextprinzipal geändert wird.</span><span class="sxs-lookup"><span data-stu-id="ac4a6-189">Specifies whether or not the cache should reset when the logged-in user (or Context Principal) changes.</span></span> <span data-ttu-id="ac4a6-190">Der aktuelle Benutzer wird auch als Anforderungskontextprinzipal bezeichnet und kann in einer Razor-Ansicht angezeigt werden, indem Sie auf `@User.Identity.Name` verweisen.</span><span class="sxs-lookup"><span data-stu-id="ac4a6-190">The current user is also known as the Request Context Principal and can be viewed in a Razor view by referencing `@User.Identity.Name`.</span></span>

<span data-ttu-id="ac4a6-191">Das folgende Beispiel überprüft den zu diesem Zeitpunkt angemeldeten Benutzer.</span><span class="sxs-lookup"><span data-stu-id="ac4a6-191">The following example looks at the current logged in user.</span></span>  

<span data-ttu-id="ac4a6-192">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="ac4a6-192">Example:</span></span>

```cshtml
<cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="ac4a6-193">Wenn Sie dieses Attribut verwenden, werden die Inhalte im Cache über einen Anmeldungs- und Abmeldungskreislauf verwaltet.</span><span class="sxs-lookup"><span data-stu-id="ac4a6-193">Using this attribute maintains the contents in cache through a log-in and log-out cycle.</span></span>  <span data-ttu-id="ac4a6-194">Wenn Sie das Attribut `vary-by-user="true"` verwenden, wird der Cache über eine Anmeldungs- bzw. Abmeldungsaktion für den authentifizierten Benutzer ungültig.</span><span class="sxs-lookup"><span data-stu-id="ac4a6-194">When using `vary-by-user="true"`, a log-in and log-out action invalidates the cache for the authenticated user.</span></span>  <span data-ttu-id="ac4a6-195">Der Cache wird für ungültig erklärt, da ein neuer eindeutiger Cookiewert bei der Anmeldung generiert wird.</span><span class="sxs-lookup"><span data-stu-id="ac4a6-195">The cache is invalidated because a new unique cookie value is generated on login.</span></span> <span data-ttu-id="ac4a6-196">Der Cache wird für den Status „Anonym“ verwaltet, wenn kein Cookie vorhanden ist oder es abgelaufen ist.</span><span class="sxs-lookup"><span data-stu-id="ac4a6-196">Cache is maintained for the anonymous state when no cookie is present or has expired.</span></span> <span data-ttu-id="ac4a6-197">D.h., der Cache wird verwaltet, wenn kein Benutzer angemeldet ist.</span><span class="sxs-lookup"><span data-stu-id="ac4a6-197">This means if no user is logged in, the cache will be maintained.</span></span>

- - -

### <a name="vary-by"></a><span data-ttu-id="ac4a6-198">vary-by</span><span class="sxs-lookup"><span data-stu-id="ac4a6-198">vary-by</span></span>

| <span data-ttu-id="ac4a6-199">Attributtyp</span><span class="sxs-lookup"><span data-stu-id="ac4a6-199">Attribute Type</span></span> | <span data-ttu-id="ac4a6-200">Beispielwerte</span><span class="sxs-lookup"><span data-stu-id="ac4a6-200">Example Values</span></span> |
|----------------|----------------|
|     <span data-ttu-id="ac4a6-201">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="ac4a6-201">String</span></span>     |    <span data-ttu-id="ac4a6-202">"@Model"</span><span class="sxs-lookup"><span data-stu-id="ac4a6-202">"@Model"</span></span>    |

<span data-ttu-id="ac4a6-203">Über dieses Attribut können Sie festlegen, welche Daten zwischengespeichert werden sollen.</span><span class="sxs-lookup"><span data-stu-id="ac4a6-203">Allows for customization of what data gets cached.</span></span> <span data-ttu-id="ac4a6-204">Wenn das Objekt verändert wird, auf das der Zeichenfolgenwert des Attributs verweist, wird der Inhalt des Cache-Hilfsprogramms aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="ac4a6-204">When the object referenced by the attribute's string value changes, the content of the Cache Tag Helper is updated.</span></span> <span data-ttu-id="ac4a6-205">Häufig wird eine Zeichenfolgenverkettung von Modellwerten diesem Attribut zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="ac4a6-205">Often a string-concatenation of model values are assigned to this attribute.</span></span>  <span data-ttu-id="ac4a6-206">D.h., dass der Cache ungültig wird, wenn ein Update an einem der verketteten Werte vorgenommen wird.</span><span class="sxs-lookup"><span data-stu-id="ac4a6-206">Effectively, that means an update to any of the concatenated values invalidates the cache.</span></span>

<span data-ttu-id="ac4a6-207">Das folgende Beispiel nimmt an, dass die Controllermethode, die die Ansicht rendert, die Integerwerte der beiden Routenparameter `myParam1` und `myParam2` addiert, und diesen als eine Modelleigenschaft zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="ac4a6-207">The following example assumes the controller method rendering the view sums the integer value of the two route parameters, `myParam1` and `myParam2`, and returns that as the single model property.</span></span> <span data-ttu-id="ac4a6-208">Wenn sich diese Summe ändert, wird auch der Inhalt des Cache-Taghilfsprogramms gerendert und erneut zwischengespeichert.</span><span class="sxs-lookup"><span data-stu-id="ac4a6-208">When this sum changes, the content of the Cache Tag Helper is rendered and cached again.</span></span>  

<span data-ttu-id="ac4a6-209">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="ac4a6-209">Example:</span></span>

<span data-ttu-id="ac4a6-210">Aktion:</span><span class="sxs-lookup"><span data-stu-id="ac4a6-210">Action:</span></span>

```csharp
public IActionResult Index(string myParam1,string myParam2,string myParam3)
{
    int num1;
    int num2;
    int.TryParse(myParam1, out num1);
    int.TryParse(myParam2, out num2);
    return View(viewName, num1 + num2);
}
```

<span data-ttu-id="ac4a6-211">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="ac4a6-211">*Index.cshtml*</span></span>

```cshtml
<cache vary-by="@Model"">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="priority"></a><span data-ttu-id="ac4a6-212">priority</span><span class="sxs-lookup"><span data-stu-id="ac4a6-212">priority</span></span>

| <span data-ttu-id="ac4a6-213">Attributtyp</span><span class="sxs-lookup"><span data-stu-id="ac4a6-213">Attribute Type</span></span>    | <span data-ttu-id="ac4a6-214">Beispielwerte</span><span class="sxs-lookup"><span data-stu-id="ac4a6-214">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="ac4a6-215">CacheItemPriority</span><span class="sxs-lookup"><span data-stu-id="ac4a6-215">CacheItemPriority</span></span>  | <span data-ttu-id="ac4a6-216">"High"</span><span class="sxs-lookup"><span data-stu-id="ac4a6-216">"High"</span></span>                   |
|                    | <span data-ttu-id="ac4a6-217">"Low"</span><span class="sxs-lookup"><span data-stu-id="ac4a6-217">"Low"</span></span> |
|                    | <span data-ttu-id="ac4a6-218">"NeverRemove"</span><span class="sxs-lookup"><span data-stu-id="ac4a6-218">"NeverRemove"</span></span> |
|                    | <span data-ttu-id="ac4a6-219">"Normal"</span><span class="sxs-lookup"><span data-stu-id="ac4a6-219">"Normal"</span></span> |

<span data-ttu-id="ac4a6-220">Enthält Anweisungen zum Entfernen des Caches für den integrierten Cacheanbieter.</span><span class="sxs-lookup"><span data-stu-id="ac4a6-220">Provides cache eviction guidance to the built-in cache provider.</span></span> <span data-ttu-id="ac4a6-221">Der Webserver entfernt `Low`-Cacheeinträge erst, wenn der Arbeitsspeicher ausgelastet ist.</span><span class="sxs-lookup"><span data-stu-id="ac4a6-221">The web server will evict `Low` cache entries first when it's under memory pressure.</span></span>

<span data-ttu-id="ac4a6-222">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="ac4a6-222">Example:</span></span>

```cshtml
<cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="ac4a6-223">Das `priority`-Attribut garantiert keine festgelegte Ebene der Cachevermerkdauer.</span><span class="sxs-lookup"><span data-stu-id="ac4a6-223">The `priority` attribute doesn't guarantee a specific level of cache retention.</span></span> <span data-ttu-id="ac4a6-224">Bei `CacheItemPriority` handelt es sich nur um einen Vorschlag.</span><span class="sxs-lookup"><span data-stu-id="ac4a6-224">`CacheItemPriority` is only a suggestion.</span></span> <span data-ttu-id="ac4a6-225">Wenn Sie dieses Attribut auf `NeverRemove` festlegen, ist das noch keine Garantie dafür, dass der Cache für immer gespeichert bleibt.</span><span class="sxs-lookup"><span data-stu-id="ac4a6-225">Setting this attribute to `NeverRemove` doesn't guarantee that the cache will always be retained.</span></span> <span data-ttu-id="ac4a6-226">Weitere Informationen finden Sie unter [Zusätzliche Ressourcen](#additional-resources).</span><span class="sxs-lookup"><span data-stu-id="ac4a6-226">See [Additional Resources](#additional-resources) for more information.</span></span>

<span data-ttu-id="ac4a6-227">Das Cache-Taghilfsprogramm ist vom [Arbeitsspeicher Cache Service](xref:performance/caching/memory) abhängig.</span><span class="sxs-lookup"><span data-stu-id="ac4a6-227">The Cache Tag Helper is dependent on the [memory cache service](xref:performance/caching/memory).</span></span> <span data-ttu-id="ac4a6-228">Das Cache-Taghilfsprogramm fügt den Service wenn nötig hinzu.</span><span class="sxs-lookup"><span data-stu-id="ac4a6-228">The Cache Tag Helper adds the service if it has not been added.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ac4a6-229">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="ac4a6-229">Additional resources</span></span>

* [<span data-ttu-id="ac4a6-230">Zwischenspeichern in Speicher</span><span class="sxs-lookup"><span data-stu-id="ac4a6-230">Cache in-memory</span></span>](xref:performance/caching/memory)
* [<span data-ttu-id="ac4a6-231">Einführung in Identity</span><span class="sxs-lookup"><span data-stu-id="ac4a6-231">Introduction to Identity</span></span>](xref:security/authentication/identity)
