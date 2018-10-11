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
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a>Cache-Taghilfsprogramm im ASP.NET Core MVC

Von [Peter Kellner](http://peterkellner.net) 

Durch das Cache-Taghilfsprogramm kann die Leistung Ihrer ASP.NET Core-App erheblich verbessert werden, indem deren Inhalte im internen ASP.NET Core-Cacheanbieter zwischengespeichert werden.

Die Razor-Ansichtsengine legt für `expires-after` einen Standardwert von 20 Minuten fest.

Das folgende Razor-Markup speichert das Datum bzw. die Zeit zwischen:

```cshtml
<cache>@DateTime.Now</cache>
```

Über die erste Anforderung an die Seite, die `CacheTagHelper` enthält, wird das aktuelle Datum bzw. die aktuelle Uhrzeit zurückgegeben. Über zusätzliche Anforderungen werden die zwischengespeicherten Werte angezeigt, bis der Cache abläuft (standardmäßig 20 Minuten lang) oder aufgrund von Speichermangel entfernt wird.

Sie können die Aufbewahrungsdauer im Cache mithilfe der folgenden Attribute festlegen:

## <a name="cache-tag-helper-attributes"></a>Attribute von Cache-Taghilfsprogrammen

- - -

### <a name="enabled"></a>enabled    


| Attributtyp    | Gültige Werte      |
|----------------   |----------------   |
| boolean           | TRUE (Standardwert)  |
|                   | "false"   |


Legt fest, ob der Inhalt zwischengespeichert wird, der vom Cache-Taghilfsprogramm eingeschlossen wird. Die Standardeinstellung ist `true`.  Wenn das Cache-Taghilfsprogramm auf `false` festgelegt ist, wird die gerenderte Ausgabe nicht zwischengespeichert.

Beispiel:

```cshtml
<cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-on"></a>expires-on 

| Attributtyp |           Beispielwert            |
|----------------|------------------------------------|
| DateTimeOffset | "@new DateTime(2025,1,29,17,02,0)" |

Legt die absolute Ablaufzeit fest. Im folgenden Beispiel werden die Inhalte des Cache-Taghilfsprogramms bis zum 29. Januar 2025 um 17:02 Uhr zwischengespeichert.

Beispiel:

```cshtml
<cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-after"></a>expires-after

| Attributtyp |        Beispielwert         |
|----------------|------------------------------|
|    TimeSpan    | "@TimeSpan.FromSeconds(120)" |

Legt die Zeitspanne ab der ersten Anforderungszeit fest, um die Inhalte zwischenzuspeichern. 

Beispiel:

```cshtml
<cache expires-after="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-sliding"></a>expires-sliding

| Attributtyp |        Beispielwert        |
|----------------|-----------------------------|
|    TimeSpan    | "@TimeSpan.FromSeconds(60)" |

Legt die Zeit fest, nach der ein Cacheeintrag gelöscht werden soll, wenn niemand auf diesen zugegriffen hat.

Beispiel:

```cshtml
<cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-header"></a>vary-by-header

| Attributtyp    | Beispielwerte                |
|----------------   |----------------               |
| Zeichenfolge            | "User-Agent"                  |
|                   | "User-Agent,content-encoding" |

Akzeptiert einen einzelnen Headerwert oder eine durch Kommas getrennte Liste von Headerwerten, die eine Cacheaktualisierung auslösen, wenn diese geändert werden. Im folgenden Beispiel wird der Headerwert `User-Agent` überwacht. Außerdem werden die Inhalte für alle `User-Agent` zwischengespeichert, die dem Webserver präsentiert werden.

Beispiel:

```cshtml
<cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-query"></a>vary-by-query

| Attributtyp    | Beispielwerte                |
|----------------   |----------------               |
| Zeichenfolge            | "Make"                |
|                   | "Make,Model" |

Akzeptiert einen einzelnen Headerwert oder eine durch Kommas getrennte Liste von Headerwerten, die eine Cacheaktualisierung auslösen, wenn diese geändert werden. Das folgende Beispiel überprüft die Werte von `Make` und `Model`.

Beispiel:

```cshtml
<cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-route"></a>vary-by-route

| Attributtyp    | Beispielwerte                |
|----------------   |----------------               |
| Zeichenfolge            | "Make"                |
|                   | "Make,Model" |

Akzeptiert einen einzelnen Headerwert oder eine durch Kommas getrennte Liste von Headerwerten, die eine Cacheaktualisierung auslösen, wenn die Parameterwerte der Routendaten geändert werden. Beispiel:

*Startup.cs* 

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```

*Index.cshtml*

```cshtml
<cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-cookie"></a>vary-by-cookie

| Attributtyp    | Beispielwerte                |
|----------------   |----------------               |
| Zeichenfolge            | ".AspNetCore.Identity.Application"                |
|                   | ".AspNetCore.Identity.Application,HairColor" |

Akzeptiert einen einzelnen Headerwert oder eine durch Kommas getrennte Liste von Headerwerten, die eine Cacheaktualisierung auslösen, wenn diese geändert werden. Das folgende Beispiel überprüft das Cookie, das der ASP.NET Core Identity zugeordnet ist. Wenn ein Benutzer authentifiziert wird, muss der Anforderungscookie festgelegt werden. Dadurch wird eine Cacheaktualisierung ausgelöst.

Beispiel:

```cshtml
<cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-user"></a>vary-by-user

| Attributtyp    | Beispielwerte                |
|----------------   |----------------               |
| Boolesch             | "true"                  |
|                     | FALSE (Standardwert) |

Gibt an, ob der Cache zurückgesetzt werden soll, wenn sich ein anderer Benutzer anmeldet, also das Kontextprinzipal geändert wird. Der aktuelle Benutzer wird auch als Anforderungskontextprinzipal bezeichnet und kann in einer Razor-Ansicht angezeigt werden, indem Sie auf `@User.Identity.Name` verweisen.

Das folgende Beispiel überprüft den zu diesem Zeitpunkt angemeldeten Benutzer.  

Beispiel:

```cshtml
<cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

Wenn Sie dieses Attribut verwenden, werden die Inhalte im Cache über einen Anmeldungs- und Abmeldungskreislauf verwaltet.  Wenn Sie das Attribut `vary-by-user="true"` verwenden, wird der Cache über eine Anmeldungs- bzw. Abmeldungsaktion für den authentifizierten Benutzer ungültig.  Der Cache wird für ungültig erklärt, da ein neuer eindeutiger Cookiewert bei der Anmeldung generiert wird. Der Cache wird für den Status „Anonym“ verwaltet, wenn kein Cookie vorhanden ist oder es abgelaufen ist. D.h., der Cache wird verwaltet, wenn kein Benutzer angemeldet ist.

- - -

### <a name="vary-by"></a>vary-by

| Attributtyp | Beispielwerte |
|----------------|----------------|
|     Zeichenfolge     |    "@Model"    |

Über dieses Attribut können Sie festlegen, welche Daten zwischengespeichert werden sollen. Wenn das Objekt verändert wird, auf das der Zeichenfolgenwert des Attributs verweist, wird der Inhalt des Cache-Hilfsprogramms aktualisiert. Häufig wird eine Zeichenfolgenverkettung von Modellwerten diesem Attribut zugewiesen.  D.h., dass der Cache ungültig wird, wenn ein Update an einem der verketteten Werte vorgenommen wird.

Das folgende Beispiel nimmt an, dass die Controllermethode, die die Ansicht rendert, die Integerwerte der beiden Routenparameter `myParam1` und `myParam2` addiert, und diesen als eine Modelleigenschaft zurückgibt. Wenn sich diese Summe ändert, wird auch der Inhalt des Cache-Taghilfsprogramms gerendert und erneut zwischengespeichert.  

Beispiel:

Aktion:

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

*Index.cshtml*

```cshtml
<cache vary-by="@Model"">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="priority"></a>priority

| Attributtyp    | Beispielwerte                |
|----------------   |----------------               |
| CacheItemPriority  | "High"                   |
|                    | "Low" |
|                    | "NeverRemove" |
|                    | "Normal" |

Enthält Anweisungen zum Entfernen des Caches für den integrierten Cacheanbieter. Der Webserver entfernt `Low`-Cacheeinträge erst, wenn der Arbeitsspeicher ausgelastet ist.

Beispiel:

```cshtml
<cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

Das `priority`-Attribut garantiert keine festgelegte Ebene der Cachevermerkdauer. Bei `CacheItemPriority` handelt es sich nur um einen Vorschlag. Wenn Sie dieses Attribut auf `NeverRemove` festlegen, ist das noch keine Garantie dafür, dass der Cache für immer gespeichert bleibt. Weitere Informationen finden Sie unter [Zusätzliche Ressourcen](#additional-resources).

Das Cache-Taghilfsprogramm ist vom [Arbeitsspeicher Cache Service](xref:performance/caching/memory) abhängig. Das Cache-Taghilfsprogramm fügt den Service wenn nötig hinzu.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Zwischenspeichern in Speicher](xref:performance/caching/memory)
* [Einführung in Identity](xref:security/authentication/identity)
