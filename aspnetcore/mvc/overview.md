---
title: "Übersicht über ASP.NET Core MVC"
author: ardalis
description: Erfahren Sie, wie ASP.NET Core MVC ist ein funktionsreiches Framework zum Erstellen von Web-apps und APIs mit Model-View-Controller entwerfen Muster.
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 89af38d1-52e0-4db7-b791-dbce909b0714
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/overview
ms.openlocfilehash: 2492b6aa4602dbbf3b9cd3dca00d40690c640cab
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
# <a name="overview-of-aspnet-core-mvc"></a>Übersicht über ASP.NET Core MVC

Durch [Steve Smith](https://ardalis.com/)

ASP.NET Core MVC ist ein funktionsreiches Framework zum Erstellen von Web-apps und APIs mit Model-View-Controller entwerfen Muster.

## <a name="what-is-the-mvc-pattern"></a>Was ist das MVC-Schema?

Das Architekturschema Model-View-Controller (MVC) trennt eine Anwendung in drei Hauptgruppen von Komponenten: Modelle, Ansichten und Controllern. Das Muster erleichtert erzielen [Trennung von Anliegen](http://deviq.com/separation-of-concerns/). Mit diesem Muster werden benutzeranforderungen an einen Controller weitergeleitet, die für die Arbeit mit dem Modell auf Benutzeraktionen ausführen und/oder das Abrufen von Ergebnissen von Abfragen. Der Controller wählt die Sicht, die dem Benutzer angezeigt, und alle dafür erforderlichen Modelldaten erhalten.

Das folgende Diagramm zeigt die drei wichtigsten Komponenten und welche verweisen auf die anderen:

![MVC-Muster](overview/_static/mvc.png)

Diese Abgrenzung Aufgaben können Sie die Anwendung im Hinblick auf Komplexität skaliert, da es einfacher ist, code, Debuggen und Testen etwas (Modell, Ansicht und Controller), das einen einzelnen Auftrag hat (und folgt dem [Prinzip einzigen Verantwortung ](http://deviq.com/single-responsibility-principle/)). Es ist schwieriger zu aktualisieren, Test- und Debugcode, der über mindestens zwei dieser drei Bereiche verteilt Abhängigkeiten aufweist. Beispielsweise ist Benutzeroberflächenlogik häufiger als Geschäftslogik zu ändern. Wenn Präsentation Code und Geschäftslogik in einem einzigen Objekt kombiniert werden, müssen Sie zum Ändern eines Objekts, die Geschäftslogik enthält jedes Mal, wenn Sie die Benutzeroberfläche ändern. Dies ist wahrscheinlich, Fehler verursachen und erfordern, die ein erneuter Test alle Geschäftslogik nach jeder minimalen Benutzeroberfläche ändern.

> [!NOTE]
> Die Ansicht und Controller richten sich nach dem Modell. Das Modell hängt jedoch weder für die Sicht als auch für den Controller. Dies ist einer der Hauptvorteile der Trennung. Aufgrund dieser Trennung kann das Modell erstellt und getestet werden unabhängig von der die visuelle Darstellung.

### <a name="model-responsibilities"></a>Model Verantwortlichkeiten

Das Modell in einer MVC-Anwendung stellt den Zustand der Anwendung und alle Geschäftslogiken oder Vorgänge, die von ihm ausgeführt werden soll. Im Modell zusammen mit jeder Implementierungslogik für das Beibehalten des Zustands der Anwendung sollte Geschäftslogik gekapselt werden. Stark typisierte Ansichten mithilfe für diese Sicht zeigt in der Regel von ViewModel-Typen, die speziell für die Daten enthalten; der Controller wird erstellen und füllen diese ViewModel-Instanzen aus dem Modell.

> [!NOTE]
> Es gibt viele Möglichkeiten, das Modell in einer app zu organisieren, die das MVC-Architekturschema verwendet. Erfahren Sie mehr über einige [verschiedene Arten von Modelltypen](http://deviq.com/kinds-of-models/).

### <a name="view-responsibilities"></a>Zuständigkeitsbereiche anzeigen

Ansichten sind für die Darstellung von Inhalten über die Benutzeroberfläche zuständig. Verwenden sie die [Razor-Ansichtsmodul](#razor-view-engine) .NET Code in HTML-Markup eingebettet werden sollen. Minimale Logik in Ansichten muss, und eine Logik in ihnen sollte beziehen Inhalt darstellen. Wenn Sie feststellen, nötig, viel Logik in der Ansicht auszuführen, Dateien, um Daten aus einem komplexen Modell anzuzeigen, können Sie verwenden eine [Ansichtskomponente](views/view-components.md), ViewModel, oder der Vorlage anzeigen, um die Sicht zu vereinfachen.

### <a name="controller-responsibilities"></a>Controller Zuständigkeiten

Controller sind die Komponenten behandeln Benutzerinteraktionen, mit dem Modell arbeiten und letztlich wählen Sie eine Ansicht zu rendern. In einer MVC-Anwendung zeigt die Ansicht nur Informationen; der Controller behandelt und antwortet auf Benutzereingaben und Interaktion. In der MVC-Muster, der Controller ist der anfängliche Einstiegspunkt und ist verantwortlich für die Auswahl, welches Modell zum Arbeiten mit Typen und die zu rendernde Ansicht (daher Sie seinen Namen - Steuerelemente wie die app auf eine bestimmte Anforderung reagiert).

> [!NOTE]
> Domänencontroller sollten durch zu viele Aufgaben nicht übermäßig kompliziert sein. Damit Controllerlogik nicht übermäßig kompliziert, verwenden die [Prinzip einzigen Verantwortung](http://deviq.com/single-responsibility-principle/) Push Geschäftslogik aus dem Controller und in das Domänenmodell.

>[!TIP]
> Wenn Sie feststellen, dass Ihre Controlleraktionen häufig die gleichen Arten von Aktionen ausführen, führen Sie Sie der [nicht selbst Prinzip wiederholen](http://deviq.com/don-t-repeat-yourself/) umgezogen diese häufig verwendete Aktionen in [Filter](#filters).

## <a name="what-is-aspnet-core-mvc"></a>Was ist der Kern von ASP.NET-MVC

Das ASP.NET-MVC-Framework Core ist eine einfache open Source mitgliedschaftsbasierte PresentationFramework für die Verwendung mit ASP.NET Core optimiert.

ASP.NET Core MVC Weise Mustern basierende dynamische Websites erstellen, die eine klare Trennung von Anliegen ermöglicht. Er bietet Ihnen die vollständige Kontrolle über Markup, TDD-freundliche Entwicklung und Verwendungen unterstützt die aktuellsten Webstandards.

## <a name="features"></a>Features

ASP.NET Core MVC umfasst Folgendes:

* [Routing](#routing)
* [Wurden die modellbindung](#model-binding)
* [Modellvalidierung](#model-validation)
* [Abhängigkeitsinjektion](../fundamentals/dependency-injection.md)
* [Filter](#filters)
* [Bereiche](#areas)
* [Web-APIs](#web-apis)
* [Prüfbarkeit](#testability)
* [Razor-Ansichtsmodul](#razor-view-engine)
* [Stark typisierte Ansichten](#strongly-typed-views)
* [Taghilfsprogramme](#tag-helpers)
* [Anzeigen von Komponenten](#view-components)

### <a name="routing"></a>Routing

ASP.NET Core MVC basiert auf der Basis von [ASP.NET Core routing](../fundamentals/routing.md), eine leistungsstarke URL-Zuordnung-Komponente, mit dem Sie Anwendungen mit verständlichen und durchsuchbaren URLs zu erstellen. Dadurch können Sie die URL-Benennungsschemas für Ihre Anwendung zu definieren, die funktionieren gut für Search Engine Optimization (SEO) und für die linkgenerierung ohne Berücksichtigung wie die Dateien auf dem Webserver organisiert sind. Sie können definieren, die Routen, die mit einer geeigneten Route Vorlage, die routeneinschränkungen-Wert, Standardwerte und optionale Werte unterstützt.

*Routing konventionsbasierte* akzeptiert, ermöglicht Sie die URL global definieren Formaten, bei denen die Anwendung, und wie diese jeweils über Formaten ordnet eine bestimmte Aktionsmethode auf angegebene Controller. Wenn eine eingehende Anforderung empfangen wird, wird das Routingmodul analysiert die URL und liegt eine Übereinstimmung mit einer der definierten URL-Formate und ruft dann die zugehörigen Controlleraktionsmethode.

```csharp
routes.MapRoute(name: "Default", template: "{controller=Home}/{action=Index}/{id?}");
```

*-Attribut routing* ermöglicht es Ihnen, die Routinginformationen angeben, durch das ergänzen der Controller und Aktionen mit Attributen, die Routen für die Anwendung zu definieren. Dies bedeutet, dass die Routendefinitionen, neben dem Controller und Aktion platziert werden, mit denen sie verknüpft sind.

```csharp
[Route("api/[controller]")]
public class ProductsController : Controller
{
  [HttpGet("{id}")]
  public IActionResult GetProduct(int id)
  {
    ...
  }
}
```

### <a name="model-binding"></a>Wurden die modellbindung

ASP.NET Core MVC [modellbindung](models/model-binding.md) konvertiert Client-Anforderungsdaten (Formularwerte, Routendaten, Abfragezeichenfolgen-Parameter, HTTP-Header) in Objekte, die der Controller verarbeiten kann. Daher muss nicht mehr der Controllerlogik zu ermitteln, die eingehende Anforderungsdaten auszuführen; Sie enthalten die Daten einfach, als Parameter für die Aktionsmethoden.

```csharp
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null) { ... }
   ```

### <a name="model-validation"></a>modellvalidierung

ASP.NET Core MVC unterstützt [Überprüfung](models/validation.md) durch das ergänzen der Model-Objekts mit Daten Anmerkung Validierungsattribute. Die überprüfungsattribute werden auf dem Client überprüft, bevor Werte an den Server zurückgesendet werden, als auch auf dem Server vor der Controlleraktion aufgerufen wird.

```csharp
using System.ComponentModel.DataAnnotations;
public class LoginViewModel
{
    [Required]
    [EmailAddress]
    public string Email { get; set; }

    [Required]
    [DataType(DataType.Password)]
    public string Password { get; set; }

    [Display(Name = "Remember me?")]
    public bool RememberMe { get; set; }
}
```

Eine Controlleraktion:

```csharp
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null)
{
    if (ModelState.IsValid)
    {
      // work with the model
    }
    // If we got this far, something failed, redisplay form
    return View(model);
}
```

Das Framework verarbeitet Anforderungsdaten sowohl auf dem Client als auch auf dem Server zu überprüfen. Validierungslogik auf Modelltypen angegeben wird, die den gerenderten Ansichten als unaufdringlichen Anmerkungen hinzugefügt und wird im Browser mit erzwungen [jQuery-Validierung](https://jqueryvalidation.org/).

### <a name="dependency-injection"></a>Abhängigkeitsinjektion

ASP.NET Core verfügt über integrierte Unterstützung für [abhängigkeiteneinschleusung (DI)](../fundamentals/dependency-injection.md). In ASP.NET-MVC Core [Controller](controllers/dependency-injection.md) können Anforderung erforderlichen Dienste über ihre Konstruktoren, die ihnen ermöglicht, befolgen die [expliziten Abhängigkeiten Prinzip](http://deviq.com/explicit-dependencies-principle/).

Ihrer app können Sie auch [Abhängigkeitsinjektion in der Sicht Dateien](views/dependency-injection.md)unter Verwendung der `@inject` Richtlinie:

```cshtml
@inject SomeService ServiceName
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ServiceName.GetTitle</title>
</head>
<body>
    <h1>@ServiceName.GetTitle</h1>
</body>
</html>
```

### <a name="filters"></a>Filter

[Filter](controllers/filters.md) ermöglicht es Entwicklern, querschnittliche bedenken, wie Ausnahmebehandlung oder Autorisierung zu kapseln. Filter Ausführen von benutzerdefinierten vorab und nachträglich verarbeiteten Logik für Aktionsmethoden zu aktivieren und zu bestimmten Zeitpunkten in der Ausführungspipeline für eine bestimmte Anforderung Ausführung konfiguriert werden können. Filter können auf Domänencontrollern oder Aktionen, die als Attribute angewendet werden (oder Global ausgeführt werden). Mehrere Filter (z. B. `Authorize`) im Framework enthalten sind.


```csharp
[Authorize]
   public class AccountController : Controller
   {
```

### <a name="areas"></a>Bereiche

[Bereiche](controllers/areas.md) bieten eine Möglichkeit, eine große ASP.NET Core MVC-Web-app in kleinere funktionale Einheiten zu partitionieren. Ein Bereich ist letztendlich eine MVC-Struktur in einer Anwendung. Klicken Sie in einer MVC-Projekt logische Komponenten wie das Modell, Controller und Ansicht in unterschiedlichen Ordnern beibehalten werden, und MVC verwendet Konventionen zur Namensgebung, um die Beziehung zwischen diesen Komponenten zu erstellen. Für eine große app kann es vorteilhaft sein, die app in separate hohe Ebene Bereiche der Funktionalität partitioniert sein. Z. B. eine e-Commerce-app mit mehreren Geschäftsbereichen, z. B. Auschecken, Abrechnung und Suche usw. Jede dieser Einheiten haben ihre eigenen logischen Komponentenansichten, Controller und Modelle.

### <a name="web-apis"></a>Web-APIs

Abgesehen davon, dass eine hervorragende Plattform zum Erstellen von Websites, weist ASP.NET Core MVC bietet umfassende Unterstützung für das Erstellen von Web-APIs. Sie können Dienste erstellen, die eine Breite Palette von Clients einschließlich Browsern und mobilen Geräten erreichen können.

Das Framework bietet Unterstützung für HTTP-Inhalt-Aushandlung mit integrierter Unterstützung für [Formatieren von Daten](models/formatting.md) als JSON oder XML. Schreiben von [benutzerdefinierten Formatierer](advanced/custom-formatters.md) zum Hinzufügen der Unterstützung für Ihre eigenen Formate.

Verwenden Sie zum Aktivieren der Unterstützung für Hypermedia linkgenerierung. Aktivieren der Unterstützung für einfache [Cross-Origin Resource sharing (CORS)](http://www.w3.org/TR/cors/) , damit Ihre Web-APIs für mehrere Webanwendungen freigegeben sein können.

### <a name="testability"></a>Prüfbarkeit

Das Framework Schnittstellen und Abhängigkeitsinjektion nutzen Es eignet sich besonders gut für Komponententests und das Framework enthält Barrierefreiheitsfunktionen (z. B. ein TestHost und InMemory-Anbieter für Entity Framework) [Integrationstests](../testing/integration-testing.md) schnelle und einfache als auch. Erfahren Sie mehr über [testen Controllerlogik](controllers/testing.md).

### <a name="razor-view-engine"></a>Razor-Ansichtsmodul

[ASP.NET Core MVC-Ansichten](views/overview.md) verwenden die [Razor-Ansichtsmodul](views/razor.md) Ansichten gerendert werden. Razor ist eine kompakte, ausdrucksbasierte und flüssigen Vorlage Markupsprache zum Definieren von Ansichten mit eingebetteten C#-Code. Razor wird verwendet, um Webinhalte auf dem Server dynamisch zu generieren. Sie können den Servercode ordnungsgemäß mit Client-Side-Inhalte und Code kombinieren.

```text
<ul>
  @for (int i = 0; i < 5; i++) {
    <li>List item @i</li>
  }
</ul>
```

Mithilfe der Razor-Ansichtsmodul können [Layouts](views/layout.md), [Teilansichten](views/partial.md) und ersetzbaren Abschnitte.

### <a name="strongly-typed-views"></a>Stark typisierte Ansichten

In MVC Razor-Ansichten können stark basierend auf Ihrem Modell eingegeben werden. Domänencontroller können mit Ansichten, die Ihre Ansichten, typüberprüfung und IntelliSense-Unterstützung aktivieren ein stark typisiertes Modell übergeben.

Die folgende Sicht definiert z. B. ein Modell vom Typ `IEnumerable<Product>`:

```cshtml
@model IEnumerable<Product>
<ul>
    @foreach (Product p in Model)
    {
        <li>@p.Name</li>
    }
</ul>
```

### <a name="tag-helpers"></a>Tag-Hilfsprogramme

[Kennzeichnen von Hilfsprogrammen](views/tag-helpers/intro.md) Aktivieren der serverseitigen Code zu erstellen und das rendering von HTML-Elementen in Razor-Dateien teilnehmen. Können Sie benutzerdefinierte Tags definieren Tag Hilfsprogramme (z. B. `<environment>`) oder um das Verhalten der vorhandene Tags ändern (z. B. `<label>`). Tag-Hilfsprogramme, die auf bestimmte Elemente, die basierend auf der Elementname und dessen Attribute gebunden werden. Sie bieten die Vorteile des serverseitigen Renderings gelernter Bearbeitungsart HTML.

Es gibt viele integrierte Tag Hilfen für allgemeine Aufgaben – z. B. das Erstellen von Formularen, Links, Laden von Ressourcen und weitere- und sogar noch stärker verfügbar in öffentlichen GitHub-Repositorys und als NuGet-Pakete. Tag-Hilfsprogramme in c# erstellt wurden, und sie Zielen auf HTML-Elemente, die basierend auf Elementnamen, Attributnamen oder übergeordneten Tags. Z. B. die integrierten LinkTagHelper kann verwendet werden, um einen Link zum Erstellen der `Login` Aktion von der `AccountsController`:

```cshtml
<p>
    Thank you for confirming your email.
    Please <a asp-controller="Account" asp-action="Login">Click here to Log in</a>.
</p>
```

Die `EnvironmentTagHelper` können verwendet werden, um Ihre Ansichten (z. B. unformatierte oder verkleinerte) basierend auf der Common Language Runtime-Umgebung, z. B. Entwicklungs-, Staging- oder Produktionsumgebung anderen Skripts einschließt:

```cshtml
<environment names="Development">
    <script src="~/lib/jquery/dist/jquery.js"></script>
</environment>
<environment names="Staging,Production">
    <script src="https://ajax.aspnetcdn.com/ajax/jquery/jquery-2.1.4.min.js"
            asp-fallback-src="~/lib/jquery/dist/jquery.min.js"
            asp-fallback-test="window.jQuery">
    </script>
</environment>
```

Tag Hilfsvorlagen können eine HTML-freundliche entwicklungserfahrung und eine umfassende IntelliSense-Umgebung zum Erstellen von HTML und Razor-Markup. Die meisten integrierten Tag Hilfsprogramme vorhandenen HTML-Elemente als Ziel und geben Sie serverseitige Attribute für das Element.

### <a name="view-components"></a>Anzeigen von Komponenten

[Anzeigen von Komponenten](views/view-components.md) ermöglichen es Ihnen, Renderinglogik Verpacken und diesen in der gesamten Anwendung wiederverwenden. Ähnlich [Teilansichten](views/partial.md), aber mit zugeordneten Logik.
