---
title: Cache-Tag-Hilfsprogramm im Kern der ASP.NET MVC
author: pkellner
description: Zeigt die zum Arbeiten mit Cache-Tag-Hilfsprogramm
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: 10aa1b493dbd0672cac789f6e48ddf2f14ba35dc
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a><span data-ttu-id="4f283-103">Cache-Tag-Hilfsprogramm im Kern der ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="4f283-103">Cache Tag Helper in ASP.NET Core MVC</span></span>

<span data-ttu-id="4f283-104">Von [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="4f283-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="4f283-105">Die Cache-Tag-Hilfsprogramm bietet die Möglichkeit, die Leistung Ihrer App ASP.NET Core erheblich zu verbessern, indem Sie dessen Inhalt auf den internen Cache-Anbieter von ASP.NET Core Zwischenspeichern.</span><span class="sxs-lookup"><span data-stu-id="4f283-105">The Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to the internal ASP.NET Core cache provider.</span></span>

<span data-ttu-id="4f283-106">Das Razor-Ansichtsmodul legt den Standardwert `expires-after` auf 20 Minuten.</span><span class="sxs-lookup"><span data-stu-id="4f283-106">The Razor View Engine sets the default `expires-after` to twenty minutes.</span></span>

<span data-ttu-id="4f283-107">Das folgende Markup für den Razor werden zwischengespeichert, Datum/Uhrzeit:</span><span class="sxs-lookup"><span data-stu-id="4f283-107">The following Razor markup caches the date/time:</span></span>

```cshtml
<cache>@DateTime.Now</cache>
```

<span data-ttu-id="4f283-108">Die erste Anforderung an die Seite mit `CacheTagHelper` wird das aktuelle Datum/Uhrzeit angezeigt.</span><span class="sxs-lookup"><span data-stu-id="4f283-108">The first request to the page that contains `CacheTagHelper` will display the current date/time.</span></span> <span data-ttu-id="4f283-109">Zusätzliche Anforderungen werden den zwischengespeicherten Wert angezeigt, bis der Cache abläuft (standardmäßig 20 Minuten) oder durch ungenügenden Arbeitsspeicher bedingt entfernt wird.</span><span class="sxs-lookup"><span data-stu-id="4f283-109">Additional requests will show the cached value until the cache expires (default 20 minutes) or is evicted by memory pressure.</span></span>

<span data-ttu-id="4f283-110">Sie können die Cachedauer mit der folgenden Attribute festlegen:</span><span class="sxs-lookup"><span data-stu-id="4f283-110">You can set the cache duration with the following attributes:</span></span>

## <a name="cache-tag-helper-attributes"></a><span data-ttu-id="4f283-111">Zwischenspeichern von Tagattribute-Hilfsprogramm</span><span class="sxs-lookup"><span data-stu-id="4f283-111">Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled"></a><span data-ttu-id="4f283-112">enabled</span><span class="sxs-lookup"><span data-stu-id="4f283-112">enabled</span></span>    


| <span data-ttu-id="4f283-113">Attributtyp</span><span class="sxs-lookup"><span data-stu-id="4f283-113">Attribute Type</span></span>    | <span data-ttu-id="4f283-114">Gültige Werte</span><span class="sxs-lookup"><span data-stu-id="4f283-114">Valid Values</span></span>      |
|----------------   |----------------   |
| <span data-ttu-id="4f283-115">boolean</span><span class="sxs-lookup"><span data-stu-id="4f283-115">boolean</span></span>           | <span data-ttu-id="4f283-116">"True" (Standard)</span><span class="sxs-lookup"><span data-stu-id="4f283-116">"true" (default)</span></span>  |
|                   | <span data-ttu-id="4f283-117">"false"</span><span class="sxs-lookup"><span data-stu-id="4f283-117">"false"</span></span>   |


<span data-ttu-id="4f283-118">Bestimmt, ob der Inhalt durch die Cache-Tag-Hilfsprogramm eingeschlossen zwischengespeichert wird.</span><span class="sxs-lookup"><span data-stu-id="4f283-118">Determines whether the content enclosed by the Cache Tag Helper is cached.</span></span> <span data-ttu-id="4f283-119">Die Standardeinstellung ist `true`.</span><span class="sxs-lookup"><span data-stu-id="4f283-119">The default is `true`.</span></span>  <span data-ttu-id="4f283-120">Wenn auf festgelegt `false` dieser Cache Tag Helper unbeachtet Zwischenspeichern auf der gerenderten Ausgabe.</span><span class="sxs-lookup"><span data-stu-id="4f283-120">If set to `false` this Cache Tag Helper will have no caching effect on the rendered output.</span></span>

<span data-ttu-id="4f283-121">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="4f283-121">Example:</span></span>

```cshtml
<cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-on"></a><span data-ttu-id="4f283-122">expires-on</span><span class="sxs-lookup"><span data-stu-id="4f283-122">expires-on</span></span> 

| <span data-ttu-id="4f283-123">Attributtyp</span><span class="sxs-lookup"><span data-stu-id="4f283-123">Attribute Type</span></span>    | <span data-ttu-id="4f283-124">Beispielwert</span><span class="sxs-lookup"><span data-stu-id="4f283-124">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="4f283-125">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="4f283-125">DateTimeOffset</span></span>    | <span data-ttu-id="4f283-126">"@new DateTime(2025,1,29,17,02,0)"</span><span class="sxs-lookup"><span data-stu-id="4f283-126">"@new DateTime(2025,1,29,17,02,0)"</span></span>    |


<span data-ttu-id="4f283-127">Legt ein absolutes Ablaufdatum fest.</span><span class="sxs-lookup"><span data-stu-id="4f283-127">Sets an absolute expiration date.</span></span> <span data-ttu-id="4f283-128">Im folgende Beispiel werden der Inhalt des Cache Tag bis 17:02 Uhr auf 29 Januar 2025 zwischengespeichert.</span><span class="sxs-lookup"><span data-stu-id="4f283-128">The following example will cache the contents of the Cache Tag Helper until 5:02 PM on January 29, 2025.</span></span>

<span data-ttu-id="4f283-129">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="4f283-129">Example:</span></span>

```cshtml
<cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-after"></a><span data-ttu-id="4f283-130">läuft ab nach</span><span class="sxs-lookup"><span data-stu-id="4f283-130">expires-after</span></span>

| <span data-ttu-id="4f283-131">Attributtyp</span><span class="sxs-lookup"><span data-stu-id="4f283-131">Attribute Type</span></span>    | <span data-ttu-id="4f283-132">Beispielwert</span><span class="sxs-lookup"><span data-stu-id="4f283-132">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="4f283-133">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="4f283-133">TimeSpan</span></span>    | <span data-ttu-id="4f283-134">"@TimeSpan.FromSeconds(120)"</span><span class="sxs-lookup"><span data-stu-id="4f283-134">"@TimeSpan.FromSeconds(120)"</span></span>    |


<span data-ttu-id="4f283-135">Legt die Zeitspanne seit der ersten Anforderung, zum Zwischenspeichern des Inhalts.</span><span class="sxs-lookup"><span data-stu-id="4f283-135">Sets the length of time from the first request time to cache the contents.</span></span> 

<span data-ttu-id="4f283-136">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="4f283-136">Example:</span></span>

```cshtml
<cache expires-after="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-sliding"></a><span data-ttu-id="4f283-137">Ablauf-gleitende</span><span class="sxs-lookup"><span data-stu-id="4f283-137">expires-sliding</span></span>

| <span data-ttu-id="4f283-138">Attributtyp</span><span class="sxs-lookup"><span data-stu-id="4f283-138">Attribute Type</span></span>    | <span data-ttu-id="4f283-139">Beispielwert</span><span class="sxs-lookup"><span data-stu-id="4f283-139">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="4f283-140">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="4f283-140">TimeSpan</span></span>    | <span data-ttu-id="4f283-141">"@TimeSpan.FromSeconds(60)"</span><span class="sxs-lookup"><span data-stu-id="4f283-141">"@TimeSpan.FromSeconds(60)"</span></span>     |


<span data-ttu-id="4f283-142">Legt die Zeit, die ein Eintrag im Cache entfernt werden soll, wenn er nicht zugegriffen wurde.</span><span class="sxs-lookup"><span data-stu-id="4f283-142">Sets the time that a cache entry should be evicted if it has not been accessed.</span></span>

<span data-ttu-id="4f283-143">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="4f283-143">Example:</span></span>

```cshtml
<cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-header"></a><span data-ttu-id="4f283-144">variieren-by-header</span><span class="sxs-lookup"><span data-stu-id="4f283-144">vary-by-header</span></span>

| <span data-ttu-id="4f283-145">Attributtyp</span><span class="sxs-lookup"><span data-stu-id="4f283-145">Attribute Type</span></span>    | <span data-ttu-id="4f283-146">Beispielwerte</span><span class="sxs-lookup"><span data-stu-id="4f283-146">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="4f283-147">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="4f283-147">String</span></span>            | <span data-ttu-id="4f283-148">"User-Agent"</span><span class="sxs-lookup"><span data-stu-id="4f283-148">"User-Agent"</span></span>                  |
|                   | <span data-ttu-id="4f283-149">"Benutzer-Agent, Content-encoding"</span><span class="sxs-lookup"><span data-stu-id="4f283-149">"User-Agent,content-encoding"</span></span> |

<span data-ttu-id="4f283-150">Nimmt einen einzelnen Header-Wert oder eine durch Trennzeichen getrennte Liste von Headerwerten, die eine Aktualisierung des Cache auslösen, wenn sich diese ändern.</span><span class="sxs-lookup"><span data-stu-id="4f283-150">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when they change.</span></span> <span data-ttu-id="4f283-151">Im folgende Beispiel überwacht den Headerwert `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="4f283-151">The following example monitors the header value `User-Agent`.</span></span> <span data-ttu-id="4f283-152">Im Beispiel speichert den Inhalt für alle anderen `User-Agent` dargestellt, mit dem Webserver.</span><span class="sxs-lookup"><span data-stu-id="4f283-152">The example will cache the content for every different `User-Agent` presented to the web server.</span></span>

<span data-ttu-id="4f283-153">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="4f283-153">Example:</span></span>

```cshtml
<cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-query"></a><span data-ttu-id="4f283-154">durch eine Abfrage variieren</span><span class="sxs-lookup"><span data-stu-id="4f283-154">vary-by-query</span></span>

| <span data-ttu-id="4f283-155">Attributtyp</span><span class="sxs-lookup"><span data-stu-id="4f283-155">Attribute Type</span></span>    | <span data-ttu-id="4f283-156">Beispielwerte</span><span class="sxs-lookup"><span data-stu-id="4f283-156">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="4f283-157">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="4f283-157">String</span></span>            | <span data-ttu-id="4f283-158">"Erstellen"</span><span class="sxs-lookup"><span data-stu-id="4f283-158">"Make"</span></span>                |
|                   | <span data-ttu-id="4f283-159">"Marke, Modell"</span><span class="sxs-lookup"><span data-stu-id="4f283-159">"Make,Model"</span></span> |

<span data-ttu-id="4f283-160">Nimmt einen einzelnen Header-Wert oder eine durch Trennzeichen getrennte Liste von Headerwerten, die eine Aktualisierung des Cache auslösen, wenn der Headerwert geändert wird.</span><span class="sxs-lookup"><span data-stu-id="4f283-160">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header value changes.</span></span> <span data-ttu-id="4f283-161">Im folgende Beispiel prüft die Werte der `Make` und `Model`.</span><span class="sxs-lookup"><span data-stu-id="4f283-161">The following example looks at the values of `Make` and `Model`.</span></span>

<span data-ttu-id="4f283-162">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="4f283-162">Example:</span></span>

```cshtml
<cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-route"></a><span data-ttu-id="4f283-163">Verändern von route</span><span class="sxs-lookup"><span data-stu-id="4f283-163">vary-by-route</span></span>

| <span data-ttu-id="4f283-164">Attributtyp</span><span class="sxs-lookup"><span data-stu-id="4f283-164">Attribute Type</span></span>    | <span data-ttu-id="4f283-165">Beispielwerte</span><span class="sxs-lookup"><span data-stu-id="4f283-165">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="4f283-166">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="4f283-166">String</span></span>            | <span data-ttu-id="4f283-167">"Erstellen"</span><span class="sxs-lookup"><span data-stu-id="4f283-167">"Make"</span></span>                |
|                   | <span data-ttu-id="4f283-168">"Marke, Modell"</span><span class="sxs-lookup"><span data-stu-id="4f283-168">"Make,Model"</span></span> |

<span data-ttu-id="4f283-169">Nimmt einen einzelnen Header-Wert oder eine durch Trennzeichen getrennte Liste von Headerwerten, die eine Aktualisierung des Cache auslösen, wenn Daten routenparameters Änderung Wert(e) an.</span><span class="sxs-lookup"><span data-stu-id="4f283-169">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the route data parameter value(s) change.</span></span> <span data-ttu-id="4f283-170">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="4f283-170">Example:</span></span>

<span data-ttu-id="4f283-171">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="4f283-171">*Startup.cs*</span></span> 

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```
  
<span data-ttu-id="4f283-172">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="4f283-172">*Index.cshtml*</span></span>

```cshtml
<cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-cookie"></a><span data-ttu-id="4f283-173">Verändern von Cookies</span><span class="sxs-lookup"><span data-stu-id="4f283-173">vary-by-cookie</span></span>

| <span data-ttu-id="4f283-174">Attributtyp</span><span class="sxs-lookup"><span data-stu-id="4f283-174">Attribute Type</span></span>    | <span data-ttu-id="4f283-175">Beispielwerte</span><span class="sxs-lookup"><span data-stu-id="4f283-175">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="4f283-176">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="4f283-176">String</span></span>            | <span data-ttu-id="4f283-177">".AspNetCore.Identity.Application"</span><span class="sxs-lookup"><span data-stu-id="4f283-177">".AspNetCore.Identity.Application"</span></span>                |
|                   | <span data-ttu-id="4f283-178">".AspNetCore.Identity.Application,HairColor"</span><span class="sxs-lookup"><span data-stu-id="4f283-178">".AspNetCore.Identity.Application,HairColor"</span></span> |

<span data-ttu-id="4f283-179">Nimmt einen einzelnen Header-Wert oder eine durch Trennzeichen getrennte Liste von Headerwerten, die eine Aktualisierung des Cache auslösen, wenn die Headerwerte (s) ändern.</span><span class="sxs-lookup"><span data-stu-id="4f283-179">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header values(s) change.</span></span> <span data-ttu-id="4f283-180">Im folgende Beispiel prüft die ASP.NET Identity zugeordneten Cookies ab.</span><span class="sxs-lookup"><span data-stu-id="4f283-180">The following example looks at the cookie associated with ASP.NET Identity.</span></span> <span data-ttu-id="4f283-181">Wenn ein Benutzer authentifiziert wird der Anforderungscookies festgelegt werden, die eine Aktualisierung des Cache auslöst.</span><span class="sxs-lookup"><span data-stu-id="4f283-181">When a user is authenticated the request cookie to be set which triggers a cache refresh.</span></span>

<span data-ttu-id="4f283-182">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="4f283-182">Example:</span></span>

```cshtml
<cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-user"></a><span data-ttu-id="4f283-183">variieren von Benutzer</span><span class="sxs-lookup"><span data-stu-id="4f283-183">vary-by-user</span></span>

| <span data-ttu-id="4f283-184">Attributtyp</span><span class="sxs-lookup"><span data-stu-id="4f283-184">Attribute Type</span></span>    | <span data-ttu-id="4f283-185">Beispielwerte</span><span class="sxs-lookup"><span data-stu-id="4f283-185">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="4f283-186">Boolesch</span><span class="sxs-lookup"><span data-stu-id="4f283-186">Boolean</span></span>             | <span data-ttu-id="4f283-187">"true"</span><span class="sxs-lookup"><span data-stu-id="4f283-187">"true"</span></span>                  |
|                     | <span data-ttu-id="4f283-188">"False" (Standard)</span><span class="sxs-lookup"><span data-stu-id="4f283-188">"false" (default)</span></span> |

<span data-ttu-id="4f283-189">Gibt an, und zwar unabhängig davon, ob der Cache zurückgesetzt werden soll, wenn der angemeldete Benutzer (oder Kontext Prinzipal) ändert.</span><span class="sxs-lookup"><span data-stu-id="4f283-189">Specifies whether or not the cache should reset when the logged-in user (or Context Principal) changes.</span></span> <span data-ttu-id="4f283-190">Der aktuelle Benutzer ist auch bekannt als der Prinzipal für die Anforderung Kontext und können in einer Razor-Ansicht angezeigt werden, durch Verweisen auf `@User.Identity.Name`.</span><span class="sxs-lookup"><span data-stu-id="4f283-190">The current user is also known as the Request Context Principal and can be viewed in a Razor view by referencing `@User.Identity.Name`.</span></span>

<span data-ttu-id="4f283-191">Im folgende Beispiel prüft den aktuell angemeldeten Benutzers ab.</span><span class="sxs-lookup"><span data-stu-id="4f283-191">The following example looks at the current logged in user.</span></span>  

<span data-ttu-id="4f283-192">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="4f283-192">Example:</span></span>

```cshtml
<cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="4f283-193">Mit diesem Attribut behält den Inhalt im Cache für eine Anmeldung und Abmeldung Aktualisierungszyklus durchläuft.</span><span class="sxs-lookup"><span data-stu-id="4f283-193">Using this attribute maintains the contents in cache through a log-in and log-out cycle.</span></span>  <span data-ttu-id="4f283-194">Bei Verwendung `vary-by-user="true"`, eine Anmeldung und Abmeldung Aktion erklärt den Cache für den authentifizierten Benutzer.</span><span class="sxs-lookup"><span data-stu-id="4f283-194">When using `vary-by-user="true"`, a log-in and log-out action invalidates the cache for the authenticated user.</span></span>  <span data-ttu-id="4f283-195">Der Cache wird ungültig, da ein neue eindeutige Cookiewert bei der Anmeldung generiert wurde.</span><span class="sxs-lookup"><span data-stu-id="4f283-195">The cache is invalidated because a new unique cookie value is generated on login.</span></span> <span data-ttu-id="4f283-196">Cache wird für den anonymen Zustand beibehalten, wenn kein Cookie vorhanden ist, oder ist abgelaufen.</span><span class="sxs-lookup"><span data-stu-id="4f283-196">Cache is maintained for the anonymous state when no cookie is present or has expired.</span></span> <span data-ttu-id="4f283-197">Dies bedeutet, wenn kein Benutzer angemeldet ist, wird der Cache beibehalten werden.</span><span class="sxs-lookup"><span data-stu-id="4f283-197">This means if no user is logged in, the cache will be maintained.</span></span>

- - -

### <a name="vary-by"></a><span data-ttu-id="4f283-198">vary-by</span><span class="sxs-lookup"><span data-stu-id="4f283-198">vary-by</span></span>

| <span data-ttu-id="4f283-199">Attributtyp</span><span class="sxs-lookup"><span data-stu-id="4f283-199">Attribute Type</span></span>    | <span data-ttu-id="4f283-200">Beispielwerte</span><span class="sxs-lookup"><span data-stu-id="4f283-200">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="4f283-201">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="4f283-201">String</span></span>             | <span data-ttu-id="4f283-202">"@Model"</span><span class="sxs-lookup"><span data-stu-id="4f283-202">"@Model"</span></span>                 |


<span data-ttu-id="4f283-203">Ermöglicht die Anpassung der Daten zwischengespeichert, ruft.</span><span class="sxs-lookup"><span data-stu-id="4f283-203">Allows for customization of what data gets cached.</span></span> <span data-ttu-id="4f283-204">Wenn das Objekt verweist auf das Attribut Zeichenfolge geändert wird, den Inhalt des Hilfsprogramms Tag Cache aktualisiert wurde.</span><span class="sxs-lookup"><span data-stu-id="4f283-204">When the object referenced by the attribute's string value changes, the content of the Cache Tag Helper is updated.</span></span> <span data-ttu-id="4f283-205">Eine zeichenfolgenverkettung aus Modellwerte werden häufig mit diesem Attribut zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="4f283-205">Often a string-concatenation of model values are assigned to this attribute.</span></span>  <span data-ttu-id="4f283-206">Praktisch bedeutet dies, dass ein Update für die verketteten Werte des Caches erklärt wird.</span><span class="sxs-lookup"><span data-stu-id="4f283-206">Effectively, that means an update to any of the concatenated values invalidates the cache.</span></span>

<span data-ttu-id="4f283-207">Im folgende Beispiel wird davon ausgegangen, das Rendern der Ansicht Sums des ganzzahlige Wert, der die zwei Routenparameter Controllermethode `myParam1` und `myParam2`, und gibt zurück, die als einzelnes Modell-Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="4f283-207">The following example assumes the controller method rendering the view sums the integer value of the two route parameters, `myParam1` and `myParam2`, and returns that as the single model property.</span></span> <span data-ttu-id="4f283-208">Bei dieser Summe geändert wird, wird der Inhalt des Cache-Tag-Hilfsprogramms gerendert und erneut zwischengespeichert.</span><span class="sxs-lookup"><span data-stu-id="4f283-208">When this sum changes, the content of the Cache Tag Helper is rendered and cached again.</span></span>  

<span data-ttu-id="4f283-209">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="4f283-209">Example:</span></span>

<span data-ttu-id="4f283-210">Aktion:</span><span class="sxs-lookup"><span data-stu-id="4f283-210">Action:</span></span>

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

<span data-ttu-id="4f283-211">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="4f283-211">*Index.cshtml*</span></span>

```cshtml
<cache vary-by="@Model"">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="priority"></a><span data-ttu-id="4f283-212">priority</span><span class="sxs-lookup"><span data-stu-id="4f283-212">priority</span></span>

| <span data-ttu-id="4f283-213">Attributtyp</span><span class="sxs-lookup"><span data-stu-id="4f283-213">Attribute Type</span></span>    | <span data-ttu-id="4f283-214">Beispielwerte</span><span class="sxs-lookup"><span data-stu-id="4f283-214">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="4f283-215">CacheItemPriority</span><span class="sxs-lookup"><span data-stu-id="4f283-215">CacheItemPriority</span></span>  | <span data-ttu-id="4f283-216">"High"</span><span class="sxs-lookup"><span data-stu-id="4f283-216">"High"</span></span>                   |
|                    | <span data-ttu-id="4f283-217">"Low"</span><span class="sxs-lookup"><span data-stu-id="4f283-217">"Low"</span></span> |
|                    | <span data-ttu-id="4f283-218">"NeverRemove"</span><span class="sxs-lookup"><span data-stu-id="4f283-218">"NeverRemove"</span></span> |
|                    | <span data-ttu-id="4f283-219">"Normal"</span><span class="sxs-lookup"><span data-stu-id="4f283-219">"Normal"</span></span> |

<span data-ttu-id="4f283-220">Stellt die integrierten Cacheanbieter Cache Entfernung Anleitungen bereit.</span><span class="sxs-lookup"><span data-stu-id="4f283-220">Provides cache eviction guidance to the built-in cache provider.</span></span> <span data-ttu-id="4f283-221">Der Webserver entfernen `Low` Einträge zuerst zwischengespeichert, wenn es nicht genügend Arbeitsspeicher vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="4f283-221">The web server will evict `Low` cache entries first when it's under memory pressure.</span></span>

<span data-ttu-id="4f283-222">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="4f283-222">Example:</span></span>

```cshtml
<cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="4f283-223">Die `priority` Attribut eine bestimmte Ebene des Cache Aufbewahrung garantiert nicht.</span><span class="sxs-lookup"><span data-stu-id="4f283-223">The `priority` attribute doesn't guarantee a specific level of cache retention.</span></span> <span data-ttu-id="4f283-224">`CacheItemPriority`ist nur ein Vorschlag.</span><span class="sxs-lookup"><span data-stu-id="4f283-224">`CacheItemPriority` is only a suggestion.</span></span> <span data-ttu-id="4f283-225">Wenn dieses Attribut auf `NeverRemove` garantiert nicht, dass der Cache immer beibehalten werden.</span><span class="sxs-lookup"><span data-stu-id="4f283-225">Setting this attribute to `NeverRemove` doesn't guarantee that the cache will always be retained.</span></span> <span data-ttu-id="4f283-226">Finden Sie unter [zusätzliche Ressourcen](#additional-resources) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="4f283-226">See [Additional Resources](#additional-resources) for more information.</span></span>

<span data-ttu-id="4f283-227">Das Cache-Tag-Hilfsprogramm ist abhängig von der [Memory-Cachedienst](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="4f283-227">The Cache Tag Helper is dependent on the [memory cache service](xref:performance/caching/memory).</span></span> <span data-ttu-id="4f283-228">Die Cache-Tag-Hilfsprogramm wird der Dienst auf, wenn er nicht hinzugefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="4f283-228">The Cache Tag Helper adds the service if it has not been added.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4f283-229">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="4f283-229">Additional resources</span></span>

* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
