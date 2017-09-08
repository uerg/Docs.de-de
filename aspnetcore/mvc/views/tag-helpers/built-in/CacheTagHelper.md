---
title: Cache-Tag-Hilfsprogramm im Kern der ASP.NET MVC
author: pkellner
description: Zeigt die zum Arbeiten mit Cache-Tag-Hilfsprogramm
keywords: ASP.NET Core, Tag-Hilfsprogramm
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: c045d485-d1dc-4cea-a675-46be83b7a012
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/CacheTagHelper
ms.openlocfilehash: 37f93816c75d83211a85c311395e2664d8a649b9
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2017
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a><span data-ttu-id="2e1a5-104">Cache-Tag-Hilfsprogramm im Kern der ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="2e1a5-104">Cache Tag Helper in ASP.NET Core MVC</span></span>

<span data-ttu-id="2e1a5-105">Durch [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="2e1a5-105">By [Peter Kellner](http://peterkellner.net)</span></span> 


<span data-ttu-id="2e1a5-106">Die Cache-Tag-Hilfsprogramm bietet die Möglichkeit, die Leistung Ihrer App ASP.NET Core erheblich zu verbessern, indem Sie dessen Inhalt auf den internen Cache-Anbieter von ASP.NET Core Zwischenspeichern.</span><span class="sxs-lookup"><span data-stu-id="2e1a5-106">The  Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to the internal ASP.NET Core cache provider.</span></span>

<span data-ttu-id="2e1a5-107">Das Razor-Ansichtsmodul legt den Standardwert `expires-after` auf 20 Minuten.</span><span class="sxs-lookup"><span data-stu-id="2e1a5-107">The Razor View Engine sets the default `expires-after` to twenty minutes.</span></span>

<span data-ttu-id="2e1a5-108">Das folgende Markup für den Razor werden zwischengespeichert, Datum/Uhrzeit:</span><span class="sxs-lookup"><span data-stu-id="2e1a5-108">The following Razor markup caches the date/time:</span></span>

```cshtml
<Cache>@DateTime.Now<Cache>
```

<span data-ttu-id="2e1a5-109">Die erste Anforderung an die Seite mit `CacheTagHelper` wird das aktuelle Datum/Uhrzeit angezeigt.</span><span class="sxs-lookup"><span data-stu-id="2e1a5-109">The first request to the page that contains `CacheTagHelper` will display the current date/time.</span></span> <span data-ttu-id="2e1a5-110">Zusätzliche Anforderungen werden den zwischengespeicherten Wert angezeigt, bis der Cache abläuft (standardmäßig 20 Minuten) oder durch ungenügenden Arbeitsspeicher bedingt entfernt wird.</span><span class="sxs-lookup"><span data-stu-id="2e1a5-110">Additional requests will show the cached value until the cache expires (default 20 minutes) or is evicted by memory pressure.</span></span>

<span data-ttu-id="2e1a5-111">Sie können die Cachedauer mit der folgenden Attribute festlegen:</span><span class="sxs-lookup"><span data-stu-id="2e1a5-111">You can set the cache duration with the following attributes:</span></span>

## <a name="cache-tag-helper-attributes"></a><span data-ttu-id="2e1a5-112">Zwischenspeichern von Tagattribute-Hilfsprogramm</span><span class="sxs-lookup"><span data-stu-id="2e1a5-112">Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled"></a><span data-ttu-id="2e1a5-113">enabled</span><span class="sxs-lookup"><span data-stu-id="2e1a5-113">enabled</span></span>    


| <span data-ttu-id="2e1a5-114">Attributtyp</span><span class="sxs-lookup"><span data-stu-id="2e1a5-114">Attribute Type</span></span>    | <span data-ttu-id="2e1a5-115">Gültige Werte</span><span class="sxs-lookup"><span data-stu-id="2e1a5-115">Valid Values</span></span>      |
|----------------   |----------------   |
| <span data-ttu-id="2e1a5-116">boolean</span><span class="sxs-lookup"><span data-stu-id="2e1a5-116">boolean</span></span>           | <span data-ttu-id="2e1a5-117">"True" (Standard)</span><span class="sxs-lookup"><span data-stu-id="2e1a5-117">"true" (default)</span></span>  |
|                   | <span data-ttu-id="2e1a5-118">"false"</span><span class="sxs-lookup"><span data-stu-id="2e1a5-118">"false"</span></span>   |


<span data-ttu-id="2e1a5-119">Bestimmt, ob der Inhalt durch die Cache-Tag-Hilfsprogramm eingeschlossen zwischengespeichert wird.</span><span class="sxs-lookup"><span data-stu-id="2e1a5-119">Determines whether the content enclosed by the Cache Tag Helper is cached.</span></span> <span data-ttu-id="2e1a5-120">Die Standardeinstellung ist `true`.</span><span class="sxs-lookup"><span data-stu-id="2e1a5-120">The default is `true`.</span></span>  <span data-ttu-id="2e1a5-121">Wenn auf festgelegt `false` dieser Cache Tag Helper unbeachtet Zwischenspeichern auf der gerenderten Ausgabe.</span><span class="sxs-lookup"><span data-stu-id="2e1a5-121">If set to `false` this Cache Tag Helper will have no caching effect on the rendered output.</span></span>

<span data-ttu-id="2e1a5-122">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="2e1a5-122">Example:</span></span>

```cshtml
<Cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="expires-on"></a><span data-ttu-id="2e1a5-123">läuft ab am</span><span class="sxs-lookup"><span data-stu-id="2e1a5-123">expires-on</span></span> 

| <span data-ttu-id="2e1a5-124">Attributtyp</span><span class="sxs-lookup"><span data-stu-id="2e1a5-124">Attribute Type</span></span>    | <span data-ttu-id="2e1a5-125">Beispielwert</span><span class="sxs-lookup"><span data-stu-id="2e1a5-125">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="2e1a5-126">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="2e1a5-126">DateTimeOffset</span></span>    | <span data-ttu-id="2e1a5-127">"@new DateTime(2025,1,29,17,02,0)"</span><span class="sxs-lookup"><span data-stu-id="2e1a5-127">"@new DateTime(2025,1,29,17,02,0)"</span></span>    |


<span data-ttu-id="2e1a5-128">Legt ein absolutes Ablaufdatum fest.</span><span class="sxs-lookup"><span data-stu-id="2e1a5-128">Sets an absolute expiration date.</span></span> <span data-ttu-id="2e1a5-129">Im folgende Beispiel werden der Inhalt des Cache Tag bis 17:02 Uhr auf 29 Januar 2025 zwischengespeichert.</span><span class="sxs-lookup"><span data-stu-id="2e1a5-129">The following example will cache the contents of the Cache Tag Helper until 5:02 PM on January 29, 2025.</span></span>

<span data-ttu-id="2e1a5-130">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="2e1a5-130">Example:</span></span>

```cshtml
<Cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="expires-after"></a><span data-ttu-id="2e1a5-131">läuft ab nach</span><span class="sxs-lookup"><span data-stu-id="2e1a5-131">expires-after</span></span>

| <span data-ttu-id="2e1a5-132">Attributtyp</span><span class="sxs-lookup"><span data-stu-id="2e1a5-132">Attribute Type</span></span>    | <span data-ttu-id="2e1a5-133">Beispielwert</span><span class="sxs-lookup"><span data-stu-id="2e1a5-133">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="2e1a5-134">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="2e1a5-134">TimeSpan</span></span>    | <span data-ttu-id="2e1a5-135">"@TimeSpan.FromSeconds(120)"</span><span class="sxs-lookup"><span data-stu-id="2e1a5-135">"@TimeSpan.FromSeconds(120)"</span></span>    |


<span data-ttu-id="2e1a5-136">Legt die Zeitspanne seit der ersten Anforderung, zum Zwischenspeichern des Inhalts.</span><span class="sxs-lookup"><span data-stu-id="2e1a5-136">Sets the length of time from the first request time to cache the contents.</span></span> 

<span data-ttu-id="2e1a5-137">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="2e1a5-137">Example:</span></span>

```cshtml
<Cache expires-on="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="expires-sliding"></a><span data-ttu-id="2e1a5-138">Ablauf-gleitende</span><span class="sxs-lookup"><span data-stu-id="2e1a5-138">expires-sliding</span></span>

| <span data-ttu-id="2e1a5-139">Attributtyp</span><span class="sxs-lookup"><span data-stu-id="2e1a5-139">Attribute Type</span></span>    | <span data-ttu-id="2e1a5-140">Beispielwert</span><span class="sxs-lookup"><span data-stu-id="2e1a5-140">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="2e1a5-141">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="2e1a5-141">TimeSpan</span></span>    | <span data-ttu-id="2e1a5-142">"@TimeSpan.FromSeconds(60)"</span><span class="sxs-lookup"><span data-stu-id="2e1a5-142">"@TimeSpan.FromSeconds(60)"</span></span>     |


<span data-ttu-id="2e1a5-143">Legt die Zeit, die ein Eintrag im Cache entfernt werden soll, wenn er nicht zugegriffen wurde.</span><span class="sxs-lookup"><span data-stu-id="2e1a5-143">Sets the time that a cache entry should be evicted if it has not been accessed.</span></span>

<span data-ttu-id="2e1a5-144">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="2e1a5-144">Example:</span></span>

```cshtml
<Cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-header"></a><span data-ttu-id="2e1a5-145">variieren-by-header</span><span class="sxs-lookup"><span data-stu-id="2e1a5-145">vary-by-header</span></span>

| <span data-ttu-id="2e1a5-146">Attributtyp</span><span class="sxs-lookup"><span data-stu-id="2e1a5-146">Attribute Type</span></span>    | <span data-ttu-id="2e1a5-147">Beispielwerte</span><span class="sxs-lookup"><span data-stu-id="2e1a5-147">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="2e1a5-148">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="2e1a5-148">String</span></span>            | <span data-ttu-id="2e1a5-149">"User-Agent"</span><span class="sxs-lookup"><span data-stu-id="2e1a5-149">"User-Agent"</span></span>                  |
|                   | <span data-ttu-id="2e1a5-150">"Benutzer-Agent, Content-encoding"</span><span class="sxs-lookup"><span data-stu-id="2e1a5-150">"User-Agent,content-encoding"</span></span> |

<span data-ttu-id="2e1a5-151">Nimmt einen einzelnen Header-Wert oder eine durch Trennzeichen getrennte Liste von Headerwerten, die eine Aktualisierung des Cache auslösen, wenn sich diese ändern.</span><span class="sxs-lookup"><span data-stu-id="2e1a5-151">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when they change.</span></span> <span data-ttu-id="2e1a5-152">Im folgende Beispiel überwacht den Headerwert `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="2e1a5-152">The following example monitors the header value `User-Agent`.</span></span> <span data-ttu-id="2e1a5-153">Im Beispiel speichert den Inhalt für alle anderen `User-Agent` dargestellt, mit dem Webserver.</span><span class="sxs-lookup"><span data-stu-id="2e1a5-153">The example will cache the content for every different `User-Agent` presented to the web server.</span></span>

<span data-ttu-id="2e1a5-154">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="2e1a5-154">Example:</span></span>

```cshtml
<Cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-query"></a><span data-ttu-id="2e1a5-155">durch eine Abfrage variieren</span><span class="sxs-lookup"><span data-stu-id="2e1a5-155">vary-by-query</span></span>

| <span data-ttu-id="2e1a5-156">Attributtyp</span><span class="sxs-lookup"><span data-stu-id="2e1a5-156">Attribute Type</span></span>    | <span data-ttu-id="2e1a5-157">Beispielwerte</span><span class="sxs-lookup"><span data-stu-id="2e1a5-157">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="2e1a5-158">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="2e1a5-158">String</span></span>            | <span data-ttu-id="2e1a5-159">"Erstellen"</span><span class="sxs-lookup"><span data-stu-id="2e1a5-159">"Make"</span></span>                |
|                   | <span data-ttu-id="2e1a5-160">"Marke, Modell"</span><span class="sxs-lookup"><span data-stu-id="2e1a5-160">"Make,Model"</span></span> |

<span data-ttu-id="2e1a5-161">Nimmt einen einzelnen Header-Wert oder eine durch Trennzeichen getrennte Liste von Headerwerten, die eine Aktualisierung des Cache auslösen, wenn der Headerwert geändert wird.</span><span class="sxs-lookup"><span data-stu-id="2e1a5-161">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header value changes.</span></span> <span data-ttu-id="2e1a5-162">Im folgende Beispiel prüft die Werte der `Make` und `Model`.</span><span class="sxs-lookup"><span data-stu-id="2e1a5-162">The following example looks at the values of `Make` and `Model`.</span></span>

<span data-ttu-id="2e1a5-163">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="2e1a5-163">Example:</span></span>

```cshtml
<Cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-route"></a><span data-ttu-id="2e1a5-164">Verändern von route</span><span class="sxs-lookup"><span data-stu-id="2e1a5-164">vary-by-route</span></span>

| <span data-ttu-id="2e1a5-165">Attributtyp</span><span class="sxs-lookup"><span data-stu-id="2e1a5-165">Attribute Type</span></span>    | <span data-ttu-id="2e1a5-166">Beispielwerte</span><span class="sxs-lookup"><span data-stu-id="2e1a5-166">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="2e1a5-167">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="2e1a5-167">String</span></span>            | <span data-ttu-id="2e1a5-168">"Erstellen"</span><span class="sxs-lookup"><span data-stu-id="2e1a5-168">"Make"</span></span>                |
|                   | <span data-ttu-id="2e1a5-169">"Marke, Modell"</span><span class="sxs-lookup"><span data-stu-id="2e1a5-169">"Make,Model"</span></span> |

<span data-ttu-id="2e1a5-170">Nimmt einen einzelnen Header-Wert oder eine durch Trennzeichen getrennte Liste von Headerwerten, die eine Aktualisierung des Cache auslösen, wenn Daten routenparameters Änderung Wert(e) an.</span><span class="sxs-lookup"><span data-stu-id="2e1a5-170">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the route data parameter value(s) change.</span></span> <span data-ttu-id="2e1a5-171">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="2e1a5-171">Example:</span></span>

<span data-ttu-id="2e1a5-172">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="2e1a5-172">*Startup.cs*</span></span> 

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```
  
<span data-ttu-id="2e1a5-173">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="2e1a5-173">*Index.cshtml*</span></span>

```cshtml
<Cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-cookie"></a><span data-ttu-id="2e1a5-174">Verändern von Cookies</span><span class="sxs-lookup"><span data-stu-id="2e1a5-174">vary-by-cookie</span></span>

| <span data-ttu-id="2e1a5-175">Attributtyp</span><span class="sxs-lookup"><span data-stu-id="2e1a5-175">Attribute Type</span></span>    | <span data-ttu-id="2e1a5-176">Beispielwerte</span><span class="sxs-lookup"><span data-stu-id="2e1a5-176">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="2e1a5-177">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="2e1a5-177">String</span></span>            | <span data-ttu-id="2e1a5-178">". AspNetCore.Identity.Application"</span><span class="sxs-lookup"><span data-stu-id="2e1a5-178">".AspNetCore.Identity.Application"</span></span>                |
|                   | <span data-ttu-id="2e1a5-179">". AspNetCore.Identity.Application,HairColor"</span><span class="sxs-lookup"><span data-stu-id="2e1a5-179">".AspNetCore.Identity.Application,HairColor"</span></span> |

<span data-ttu-id="2e1a5-180">Nimmt einen einzelnen Header-Wert oder eine durch Trennzeichen getrennte Liste von Headerwerten, die eine Aktualisierung des Cache auslösen, wenn die Headerwerte (s) ändern.</span><span class="sxs-lookup"><span data-stu-id="2e1a5-180">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header values(s) change.</span></span> <span data-ttu-id="2e1a5-181">Im folgende Beispiel prüft die ASP.NET Identity zugeordneten Cookies ab.</span><span class="sxs-lookup"><span data-stu-id="2e1a5-181">The following example looks at the cookie associated with ASP.NET Identity.</span></span> <span data-ttu-id="2e1a5-182">Wenn ein Benutzer authentifiziert wird der Anforderungscookies festgelegt werden, die eine Aktualisierung des Cache auslöst.</span><span class="sxs-lookup"><span data-stu-id="2e1a5-182">When a user is authenticated the request cookie to be set which triggers a cache refresh.</span></span>

<span data-ttu-id="2e1a5-183">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="2e1a5-183">Example:</span></span>

```cshtml
<Cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-user"></a><span data-ttu-id="2e1a5-184">variieren von Benutzer</span><span class="sxs-lookup"><span data-stu-id="2e1a5-184">vary-by-user</span></span>

| <span data-ttu-id="2e1a5-185">Attributtyp</span><span class="sxs-lookup"><span data-stu-id="2e1a5-185">Attribute Type</span></span>    | <span data-ttu-id="2e1a5-186">Beispielwerte</span><span class="sxs-lookup"><span data-stu-id="2e1a5-186">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="2e1a5-187">Boolesch</span><span class="sxs-lookup"><span data-stu-id="2e1a5-187">Boolean</span></span>             | <span data-ttu-id="2e1a5-188">"true"</span><span class="sxs-lookup"><span data-stu-id="2e1a5-188">"true"</span></span>                  |
|                     | <span data-ttu-id="2e1a5-189">"False" (Standard)</span><span class="sxs-lookup"><span data-stu-id="2e1a5-189">"false" (default)</span></span> |

<span data-ttu-id="2e1a5-190">Gibt an, und zwar unabhängig davon, ob der Cache zurückgesetzt werden soll, wenn der angemeldete Benutzer (oder Kontext Prinzipal) ändert.</span><span class="sxs-lookup"><span data-stu-id="2e1a5-190">Specifies whether or not the cache should reset when the logged-in user (or Context Principal) changes.</span></span> <span data-ttu-id="2e1a5-191">Der aktuelle Benutzer ist auch bekannt als der Prinzipal für die Anforderung Kontext und können in einer Razor-Ansicht angezeigt werden, durch Verweisen auf `@User.Identity.Name`.</span><span class="sxs-lookup"><span data-stu-id="2e1a5-191">The current user is also known as the Request Context Principal and can be viewed in a Razor view by referencing `@User.Identity.Name`.</span></span>

<span data-ttu-id="2e1a5-192">Im folgende Beispiel prüft den aktuell angemeldeten Benutzers ab.</span><span class="sxs-lookup"><span data-stu-id="2e1a5-192">The following example looks at the current logged in user.</span></span>  

<span data-ttu-id="2e1a5-193">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="2e1a5-193">Example:</span></span>

```cshtml
<Cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

<span data-ttu-id="2e1a5-194">Mit diesem Attribut behält den Inhalt im Cache für eine Anmeldung und Abmeldung Aktualisierungszyklus durchläuft.</span><span class="sxs-lookup"><span data-stu-id="2e1a5-194">Using this attribute maintains the contents in cache through a log-in and log-out cycle.</span></span>  <span data-ttu-id="2e1a5-195">Bei Verwendung `vary-by-user="true"`, eine Anmeldung und Abmeldung Aktion erklärt den Cache für den authentifizierten Benutzer.</span><span class="sxs-lookup"><span data-stu-id="2e1a5-195">When using `vary-by-user="true"`, a log-in and log-out action invalidates the cache for the authenticated user.</span></span>  <span data-ttu-id="2e1a5-196">Der Cache wird ungültig, da ein neue eindeutige Cookiewert bei der Anmeldung generiert wurde.</span><span class="sxs-lookup"><span data-stu-id="2e1a5-196">The cache is invalidated because a new unique cookie value is generated on login.</span></span> <span data-ttu-id="2e1a5-197">Cache wird für den anonymen Zustand beibehalten, wenn kein Cookie vorhanden ist, oder ist abgelaufen.</span><span class="sxs-lookup"><span data-stu-id="2e1a5-197">Cache is maintained for the anonymous state when no cookie is present or has expired.</span></span> <span data-ttu-id="2e1a5-198">Dies bedeutet, wenn kein Benutzer angemeldet ist, wird der Cache beibehalten werden.</span><span class="sxs-lookup"><span data-stu-id="2e1a5-198">This means if no user is logged in, the cache will be maintained.</span></span>

- - -

### <a name="vary-by"></a><span data-ttu-id="2e1a5-199">Verändern von</span><span class="sxs-lookup"><span data-stu-id="2e1a5-199">vary-by</span></span>

| <span data-ttu-id="2e1a5-200">Attributtyp</span><span class="sxs-lookup"><span data-stu-id="2e1a5-200">Attribute Type</span></span>    | <span data-ttu-id="2e1a5-201">Beispielwerte</span><span class="sxs-lookup"><span data-stu-id="2e1a5-201">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="2e1a5-202">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="2e1a5-202">String</span></span>             | <span data-ttu-id="2e1a5-203">"@Model"</span><span class="sxs-lookup"><span data-stu-id="2e1a5-203">"@Model"</span></span>                 |


<span data-ttu-id="2e1a5-204">Ermöglicht die Anpassung der Daten zwischengespeichert, ruft.</span><span class="sxs-lookup"><span data-stu-id="2e1a5-204">Allows for customization of what data gets cached.</span></span> <span data-ttu-id="2e1a5-205">Wenn das Objekt verweist auf das Attribut Zeichenfolge geändert wird, den Inhalt des Hilfsprogramms Tag Cache aktualisiert wurde.</span><span class="sxs-lookup"><span data-stu-id="2e1a5-205">When the object referenced by the attribute's string value changes, the content of the Cache Tag Helper is updated.</span></span> <span data-ttu-id="2e1a5-206">Eine zeichenfolgenverkettung aus Modellwerte werden häufig mit diesem Attribut zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="2e1a5-206">Often a string-concatenation of model values are assigned to this attribute.</span></span>  <span data-ttu-id="2e1a5-207">Praktisch bedeutet dies, dass ein Update für die verketteten Werte des Caches erklärt wird.</span><span class="sxs-lookup"><span data-stu-id="2e1a5-207">Effectively, that means an update to any of the concatenated values invalidates the cache.</span></span>

<span data-ttu-id="2e1a5-208">Im folgende Beispiel wird davon ausgegangen, das Rendern der Ansicht Sums des ganzzahlige Wert, der die zwei Routenparameter Controllermethode `myParam1` und `myParam2`, und gibt zurück, die als einzelnes Modell-Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="2e1a5-208">The following example assumes the controller method rendering the view sums the integer value of the two route parameters, `myParam1` and `myParam2`, and returns that as the single model property.</span></span> <span data-ttu-id="2e1a5-209">Bei dieser Summe geändert wird, wird der Inhalt des Cache-Tag-Hilfsprogramms gerendert und erneut zwischengespeichert.</span><span class="sxs-lookup"><span data-stu-id="2e1a5-209">When this sum changes, the content of the Cache Tag Helper is rendered and cached again.</span></span>  

<span data-ttu-id="2e1a5-210">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="2e1a5-210">Example:</span></span>

<span data-ttu-id="2e1a5-211">Aktion:</span><span class="sxs-lookup"><span data-stu-id="2e1a5-211">Action:</span></span>

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

<span data-ttu-id="2e1a5-212">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="2e1a5-212">*Index.cshtml*</span></span>

```cshtml
<Cache vary-by="@Model"">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="priority"></a><span data-ttu-id="2e1a5-213">priority</span><span class="sxs-lookup"><span data-stu-id="2e1a5-213">priority</span></span>

| <span data-ttu-id="2e1a5-214">Attributtyp</span><span class="sxs-lookup"><span data-stu-id="2e1a5-214">Attribute Type</span></span>    | <span data-ttu-id="2e1a5-215">Beispielwerte</span><span class="sxs-lookup"><span data-stu-id="2e1a5-215">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="2e1a5-216">CacheItemPriority</span><span class="sxs-lookup"><span data-stu-id="2e1a5-216">CacheItemPriority</span></span>  | <span data-ttu-id="2e1a5-217">"High"</span><span class="sxs-lookup"><span data-stu-id="2e1a5-217">"High"</span></span>                   |
|                    | <span data-ttu-id="2e1a5-218">"Low"</span><span class="sxs-lookup"><span data-stu-id="2e1a5-218">"Low"</span></span> |
|                    | <span data-ttu-id="2e1a5-219">"NeverRemove"</span><span class="sxs-lookup"><span data-stu-id="2e1a5-219">"NeverRemove"</span></span> |
|                    | <span data-ttu-id="2e1a5-220">"Normal"</span><span class="sxs-lookup"><span data-stu-id="2e1a5-220">"Normal"</span></span> |

<span data-ttu-id="2e1a5-221">Stellt die integrierten Cacheanbieter Cache Entfernung Anleitungen bereit.</span><span class="sxs-lookup"><span data-stu-id="2e1a5-221">Provides cache eviction guidance to the built-in cache provider.</span></span> <span data-ttu-id="2e1a5-222">Der Webserver entfernen `Low` Einträge zuerst zwischengespeichert, wenn es nicht genügend Arbeitsspeicher vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="2e1a5-222">The web server will evict `Low` cache entries first when it's under memory pressure.</span></span>

<span data-ttu-id="2e1a5-223">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="2e1a5-223">Example:</span></span>

```cshtml
<Cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

<span data-ttu-id="2e1a5-224">Die `priority` Attribut garantiert keinen bestimmten Grad Cache Beibehaltungsdauer.</span><span class="sxs-lookup"><span data-stu-id="2e1a5-224">The `priority` attribute does not guarantee a specific level of cache retention.</span></span> <span data-ttu-id="2e1a5-225">`CacheItemPriority`ist nur ein Vorschlag.</span><span class="sxs-lookup"><span data-stu-id="2e1a5-225">`CacheItemPriority` is only a suggestion.</span></span> <span data-ttu-id="2e1a5-226">Wenn dieses Attribut auf `NeverRemove` garantiert nicht, dass der Cache immer beibehalten werden.</span><span class="sxs-lookup"><span data-stu-id="2e1a5-226">Setting this attribute to `NeverRemove` does not guarantee that the cache will always be retained.</span></span> <span data-ttu-id="2e1a5-227">Finden Sie unter [zusätzliche Ressourcen](#additional-resources) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="2e1a5-227">See [Additional Resources](#additional-resources) for more information.</span></span>

<span data-ttu-id="2e1a5-228">Das Cache-Tag-Hilfsprogramm ist abhängig von der [Memory-Cachedienst](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="2e1a5-228">The Cache Tag Helper is dependent on the [memory cache service](xref:performance/caching/memory).</span></span> <span data-ttu-id="2e1a5-229">Die Cache-Tag-Hilfsprogramm wird der Dienst auf, wenn er nicht hinzugefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="2e1a5-229">The Cache Tag Helper adds the service if it has not been added.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2e1a5-230">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="2e1a5-230">Additional resources</span></span>

* <xref:performance/caching/memory>
* <xref:security/authentication/identity>