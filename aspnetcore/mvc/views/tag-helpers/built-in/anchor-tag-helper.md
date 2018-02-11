---
title: Anchor-Tag-Hilfsprogramm in ASP.NET Core
author: pkellner
description: Zeigt die Arbeit mit dem Anchor-Taghilfsprogramm
manager: wpickett
ms.author: riande
ms.date: 12/20/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: 404fc7bc3b35114066f035e1ac28d10a8279ccbc
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
---
# <a name="anchor-tag-helper"></a>Anchor-Taghilfsprogramm

Von [Peter Kellner](http://peterkellner.net) 

Das Anchor-Taghilfsprogramm verbessert das HTML-Anchor-Tag (`<a ... ></a>`), indem neue Attribute hinzugefügt werden. Der generierte Link (auf dem `href`-Tag) wird mit den neuen Attributen erstellt. Diese URL kann ein optionales Protokoll wie Https enthalten.

Der nachfolgende SpeakerController wird in Beispielen in diesem Dokument verwendet.

**SpeakerController.cs** 

[!code-csharp[SpeakerController](sample/TagHelpersBuiltInAspNetCore/src/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs)]


## <a name="anchor-tag-helper-attributes"></a>Attribute von Anchor-Taghilfsprogrammen

### <a name="asp-controller"></a>asp-controller

`asp-controller` dient dazu, die Controller zuzuordnen, die zum Generieren der URL verwendet werden. Die angegebenen Controller müssen im aktuellen Projekt vorhanden sein. Der folgende Code listet alle Lautsprecher auf: 

```cshtml
<a asp-controller="Speaker" asp-action="Index">All Speakers</a>
```

Das generierte Markup sieht wie folgt aus:

```html
<a href="/Speaker">All Speakers</a>
```

Wenn `asp-controller` angegeben und `asp-action` nicht angegeben ist, wird die Standardcontrollermethode der aktuell ausgeführten Ansicht als Standardeinstellung `asp-action` verwendet. Wird im obigen Beispiel `asp-action` ausgelassen und das Anchor-Taghilfsprogramm aus der `Index`-Ansicht des *HomeController* (**/Home**) generiert, so sieht das generierte Markup wie folgt aus:

```html
<a href="/Home">All Speakers</a>
```

### <a name="asp-action"></a>asp-action

`asp-action` ist der Name der Aktionsmethode im Controller, die im generierten `href`-Attribut enthalten ist. Der folgende Code legt das generierte `href`-Attribut beispielsweise so fest, dass es auf die Detailseite des Lautsprechers verweist:

```html
<a asp-controller="Speaker" asp-action="Detail">Speaker Detail</a>
```

Das generierte Markup sieht wie folgt aus:

```html
<a href="/Speaker/Detail">Speaker Detail</a>
```

Wenn kein `asp-controller`-Attribut angegeben ist, wird der Standardcontroller verwendet, der die Ansicht aufruft, die die aktuelle Ansicht ausführt.  
 
Wenn das `asp-action`-Attribut `Index` ist, so wird keine Aktion an die URL angefügt, wodurch die `Index`-Standardmethode aufgerufen wird. Die angegebene (oder standardmäßige) Aktion muss im Controller vorhanden sein, auf den in `asp-controller` verwiesen wird.

### <a name="asp-page"></a>asp-page

Verwenden Sie das `asp-page`-Attribut in einem Anchor-Tag, damit die URL auf eine bestimmten Seite verweist. Wenn Sie dem Seitennamen einen Schrägstrich „/“ voranstellen, wird die URL erstellt. Die URL im folgenden Beispiel verweist auf die Seite „Lautsprecher“ im aktuellen Verzeichnis.

```cshtml
<a asp-page="/Speakers">All Speakers</a>
```

Das `asp-page`-Attribut im vorherigen Codebeispiel rendert die HTML-Ausgabe in der Ansicht, die dem folgenden Codeausschnitt ähnelt:

```html
<a href="/items?page=%2FSpeakers">Speakers</a>
```

Das `asp-page`-Attribut und die `asp-route`-, `asp-controller`- und `asp-action`-Attribute schließen sich gegenseitig aus. Allerdings kann `asp-page` mit `asp-route-id` genutzt werden, um das Routing zu steuern, wie im folgenden Codebeispiel veranschaulicht:

```cshtml
<a asp-page="/Speaker" asp-route-id="@speaker.Id">View Speaker</a>
```

`asp-route-id` erzeugt die folgende Ausgabe:

```html
https://localhost:44399/Speakers/Index/2?page=%2FSpeaker
```

> [!NOTE]
> Damit Sie das `asp-page`-Attribut in Razor-Seiten verwenden können, müssen die URLs relative Pfade sein, z.B. `"./Speaker"`. Relative Pfade im `asp-page`-Attribut sind nicht in MVC-Ansichten verfügbar. Verwenden Sie stattdessen die Syntax „/“ für MVC-Ansichten.

### <a name="asp-route-value"></a>asp-route-{value}

`asp-route-` ist ein Platzhalterroutenpräfix. Alle Werte, die Sie nach dem nachfolgenden Gedankenstrich einfügen, werden als potenzielle Routenparameter interpretiert. Wenn keine Standardroute gefunden wird, wird dieses Routenpräfix an das generierte Href-Attribut als Anforderungsparameter und -wert angefügt. Andernfalls wird es in der Routenvorlage ersetzt.

Nehmen wir an, Sie verfügen über eine Controllermethode, die wie folgt definiert ist:

```csharp
public IActionResult AnchorTagHelper(string id)
{
    var speaker = new SpeakerData()
    {
        SpeakerId = 12
    };
    return View(viewName, speaker);
}
```

Und Sie haben die Standardvorlage für die Route in *Startup.cs* wie folgt definiert:

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
});

```

Die **Cshtml**-Datei enthält das Anchor-Taghilfsprogramm, das für die Verwendung des Modellparameters **Lautsprecher** notwendig ist. Die Datei wird aus dem Controller an die Ansicht übergeben und sieht wie folgt aus:

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-id=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

Die generierte HTML sieht dann wie folgt aus, da **Id** in der Standardroute gefunden wurde.

```html
<a href='/Speaker/Detail/12'>SpeakerId: 12</a>
```

Wenn das Routenpräfix nicht Teil der gefundenen Routingvorlage ist. Dies ist der Fall für die folgende **Cshtml**-Datei:

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-speakerid=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

Die generierte HTML sieht dann wie folgt aus, da **speakerid** nicht in der zugewiesenen Route gefunden wurde:

```html
<a href='/Speaker/Detail?speakerid=12'>SpeakerId: 12</a>
```

Wenn entweder `asp-controller` oder `asp-action` nicht angegeben werden, erfolgt der gleiche Standardverarbeitungsprozess wie beim `asp-route`-Attribut.

### <a name="asp-route"></a>asp-route

`asp-route` bietet eine Möglichkeit zur Erstellung einer URL, die direkt auf eine benannte Route verlinkt. Mit der Verwendung von Routingattributen kann eine Route wie in `SpeakerController` gezeigt benannt und in der `Evaluations`-Methode verwendet werden.

`Name = "speakerevals"` befiehlt dem Anchor-Taghilfsprogramm das Generieren einer Route direkt zu dieser Controllermethode. Dabei wird die `/Speaker/Evaluations`-URL verwendet. Wenn `asp-controller` oder `asp-action` zusätzlich zu `asp-route` angegeben sind, entspricht die generierte Route möglicherweise nicht Ihren Erwartungen. `asp-route` sollte nicht mit den Attributen `asp-controller` oder `asp-action` verwendet werden, um einen Routenkonflikt zu vermeiden.

### <a name="asp-all-route-data"></a>asp-all-route-data

`asp-all-route-data` ermöglicht das Erstellen eines Wörterbuchs mit Schlüssel-Wert-Paaren, in denen der Schlüssel der Name des Parameters und der Wert der dem Schlüssel zugeordnete Wert ist.

Das Beispiel unten zeigt die Erstellung eines Inline-Wörterbuchs und die Datenübertragung an die Razor-Ansicht. Als Alternative können die Daten auch mit Ihrem Modell übertragen werden.

```cshtml
@{
    var dict =
        new Dictionary<string, string>
        {
            {"speakerId", "11"},
            {"currentYear", "true"}
        };
}
<a asp-route="speakerevalscurrent"
asp-all-route-data="dict">SpeakerEvals</a>
```

Der obenstehende Code generiert die folgende URL: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true

Wenn auf den Link geklickt wird, wird die Controllermethode `EvaluationsCurrent` aufgerufen. Sie wird aufgerufen, da dieser Controller über zwei Zeichenfolgenparameter verfügt, die dem entsprechen, was aus dem `asp-all-route-data`-Wörterbuch erstellt wurde.

Wenn sämtliche Schlüssel im Wörterbuch Routenparametern entsprechen, werden diese Werte in der Route nach Bedarf ersetzt, und die anderen, nicht übereinstimmenden Werte werden als Anforderungsparameter generiert.

### <a name="asp-fragment"></a>asp-fragment

`asp-fragment` definiert ein URL-Fragment, das an die URL angefügt werden soll. Das Anchor-Taghilfsprogramm fügt das Hashzeichen (#) hinzu. Wenn Sie einen Tag erstellen:

```cshtml
<a asp-action="Evaluations" asp-controller="Speaker"  
   asp-fragment="SpeakerEvaluations">About Speaker Evals</a>
```

Die generierte URL sieht wie folgt aus: http://localhost/Speaker/Evaluations#SpeakerEvaluations

Hashtags sind beim Erstellen von clientseitigen Anwendungen nützlich. Sie können beispielsweise zum einfachen Markieren und Suchen in JavaScript verwendet werden.

### <a name="asp-area"></a>asp-area

`asp-area` legt den Bereichsnamen fest, den ASP.NET Core verwendet, um die entsprechende Route festzulegen. Die folgenden Beispiele zeigen, wie das Bereichsattribut eine Neuzuordnung von Routen bewirken kann. Das Festlegen von `asp-area` auf Blogs stellt das Verzeichnis `Areas/Blogs` den Routen der zugeordneten Controller und Ansichten für diesen Anchor-Tag voran.

* Projektname
  * wwwroot
  * Bereiche
    * Blogs
      * Controller
        * HomeController.cs
      * Ansichten
        * Startseite
          * Index.cshtml
          * AboutBlog.cshtml
  * Controller

Das Angeben eines gültigen Bereichstags, z.B. ```area="Blogs"```, beim Verweisen auf die ```AboutBlog.cshtml```-Datei unter Verwendung des Anchor-Taghilfsprogramms sieht wie folgt aus.

```cshtml
<a asp-action="AboutBlog" asp-controller="Home" asp-area="Blogs">Blogs About</a>
```

Die generierte HTML enthält das Bereichssegment und sieht wie folgt aus:

```html
<a href="/Blogs/Home/AboutBlog">Blogs About</a>
```

> [!TIP]
> Die Routenvorlage muss einen Verweis auf den Bereich enthalten (falls vorhanden), damit MVC-Bereiche in einer Webanwendung funktionieren. Diese Vorlage, die der zweite Parameter des `routes.MapRoute`-Methodenaufrufs ist, wird wie folgt angezeigt: `template: '"{area:exists}/{controller=Home}/{action=Index}"'`

### <a name="asp-protocol"></a>asp-protocol

Das `asp-protocol`-Attribut gibt ein Protokoll in Ihrer URL an (z.B. `https`). Ein Beispiel für ein Anchor-Taghilfsprogramm, das das Protokoll enthält, sieht wie folgt aus:

```<a asp-protocol="https" asp-action="About" asp-controller="Home">About</a>```

und generiert die HTML wie folgt:

```<a href="https://localhost/Home/About">About</a>```

Die Domäne im Beispiel ist „localhost“, aber das Anchor-Taghilfsprogramm verwendet die öffentliche Domäne der Website beim Generieren der URL.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Bereiche](xref:mvc/controllers/areas)
