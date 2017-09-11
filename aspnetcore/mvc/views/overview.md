---
title: "Sichten (Übersicht)"
author: ardalis
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 668c320d-c050-45e3-8161-2f460dc93b2f
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/overview
ms.openlocfilehash: 318d8832dadadd6946c7ffe58f9d89aaf68f54fc
ms.sourcegitcommit: 4693cb02d845adf2efa00e07ad432c81867bfa12
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/08/2017
---
# <a name="rendering-html-with-views-in-aspnet-core-mvc"></a>Rendern von HTML mit ASP.NET Core MVC-Ansichten

Durch [Steve Smith](http://ardalis.com)

ASP.NET Core MVC-Controller können Ergebnisse formatierte mit *Ansichten*.

## <a name="what-are-views"></a>Was sind Ansichten?

In der Model-View-Controller (MVC)-Muster die *Ansicht* kapselt die Präsentation Details der Interaktion mit der app des Benutzers. Ansichten sind HTML-Vorlagen mit eingebetteten Code, die Inhalt an den Client senden zu generieren. Verwendet [Razor-Syntax](razor.md), sodass Code für die Interaktion mit HTML mit minimalem Codeeinsatz oder Zeremonie.

ASP.NET Core MVC-Ansichten werden *cshtml* Dateien standardmäßig in einem *Ansichten* Ordner innerhalb der Anwendung. In der Regel wird jedem Controller einen eigenen Ordner haben, in der Ansichten für bestimmte Controlleraktionen sind.

![Ansichtenordner im Projektmappen-Explorer](overview/_static/views_solution_explorer.png)

Zusätzlich zu den Ansichten aktionsspezifische [Teilansichten](partial.md), [Layouts und andere Dateien besonderen Ansicht](layout.md) können zur Wiederholung reduzieren und ermöglichen die Wiederverwendung in der app-Ansichten verwendet werden.

## <a name="benefits-of-using-views"></a>Vorteile der Verwendung von Sichten

Bieten die Ansichten [Trennung von Anliegen](http://deviq.com/separation-of-concerns/) innerhalb einer MVC-app, die Benutzer Schnittstelle Ebene Markup getrennt von der Geschäftslogik zu kapseln. ASP.NET MVC-Ansichten verwenden [Razor-Syntax](razor.md) Wechsel zwischen den HTML-Markup und Server Side Logik Kinderspiel vornehmen. Allgemeinen, wiederkehrende Aspekte der Benutzeroberfläche der app einfach wiederverwendet werden zwischen Ansichten unter Verwendung von [Layout und die freigegebenen Direktiven](layout.md) oder [Teilansichten](partial.md).

## <a name="creating-a-view"></a>Erstellen einer Ansicht

Sichten, die für einen Controller spezifisch sind werden erstellt, der *Ansichten / [ControllerName]* Ordner. Sichten, die für Domänencontroller, freigegeben werden befinden sich der */Ansichten/freigegeben* Ordner. Benennen Sie die Ansichtsdatei identisch mit der Aktion zugeordneten Controller und Hinzufügen der *cshtml* Dateierweiterung. Z. B. zum Erstellen einer Ansicht für die *zu* Aktion auf die *Home* Controller, erstellen Sie die *About.cshtml* in der Datei die  * /Ansichten/Start*Ordner.

Eine Beispieldatei für die Sicht (*About.cshtml*):

[!code-html[Main](../../common/samples/WebApplication1/Views/Home/About.cshtml)]

*Razor* Code ist gekennzeichnet durch die `@` Symbol. C#-Anweisungen innerhalb von geschweiften Klammern Codeblöcke abgegrenzt Razor ausgeführt werden (`{` `}`), z. B. die Zuweisung von "About" auf die `ViewData["Title"]` oben angezeigten Element. Razor kann verwendet werden, um Werte in HTML anzeigen, indem Sie einfach verweisen auf den Wert mit der `@` Sonderzeichen, wie gezeigt in der `<h2>` und `<h3>` oben aufgeführten Elemente.

In dieser Ansicht konzentriert sich auf den Teil der Ausgabe für die er zuständig ist. Der Rest der Seite Layout und andere allgemeine Aspekte der Ansicht, werden an anderer Stelle angegeben. Erfahren Sie mehr über [Layout und die freigegebenen ansichtslogik](layout.md).

## <a name="how-do-controllers-specify-views"></a>Wie Controller Ansichten angeben?

Ansichten werden in der Regel zurückgegeben, von Aktionen als ein `ViewResult`. Ihrer Aktionsmethode erstellen und zurückgeben kann eine `ViewResult` , häufiger aber direkt, wenn Ihr Controller erbt `Controller`, verwenden Sie einfach die `View` Hilfsmethode, wie dieses Beispiel veranschaulicht:

*HomeController.cs*

[!code-csharp[Main](../../common/samples/WebApplication1/Controllers/HomeController.cs?highlight=5&range=16-21)]

Die `View` -Hilfsmethode verfügt über mehrere Überladungen auf, um die Rückgabe Ansichten für app-Entwickler zu vereinfachen. Sie können optional eine Sicht zurückgegeben sowie ein Modellobjekt Übergabe an die Ansicht angeben.

Wenn diese Aktion zurückgegeben wird, die *About.cshtml* oben gezeigten Ansicht gerendert wird:

![Zu den Seiten](overview/_static/about-page.png)

### <a name="view-discovery"></a>View-Ermittlung

Ein Prozess wird aufgerufen, wenn eine Aktion eine Sicht zurückgegeben wird, *Ansicht Ermittlung* erfolgt. Dieser Prozess wird bestimmt, welche Datei anzeigen verwendet werden. Wenn eine bestimmte Datei angegeben wird, sucht die Laufzeit für eine Controller-spezifische Ansicht zuerst, dann entsprechende Ansichtsname in sucht nach der *Shared* Ordner.

Wenn eine Aktion gibt die `View` -Methode, wie folgt `return View();`, der Aktionsname wird verwendet, wie der Ansichtsname. Angenommen, wenn dies von einer Aktionsmethode namens "Index" aufgerufen wurden, wäre entspricht dem Übergeben einer Ansichtsname "Index" es. Ein Sichtname explizit an die Methode übergeben werden kann (`return View("SomeView");`). Zeigen Sie in beiden Fällen Ermittlung sucht eine übereinstimmende-Datei in ein:

   1. Ansichten /\<ControllerName > /\<ViewName > cshtml

   2. Ansichten/freigegeben/\<ViewName > cshtml

>[!TIP]
> Es wird empfohlen, gemäß der Konvention von zurückzugeben `View()` aus Aktionen, wenn möglich, da dabei mehr Flexibilität bietet, einfacher Umgestalten von Code.

Anstatt ein Ansichtsname kann ein Dateipfad für die Sicht angegeben werden. Wenn einen absoluten Pfad des Anwendungsstamms ab (optional beginnend mit "/" oder "~ /"), wird die *cshtml* Erweiterung muss als Teil der Dateipfad angegeben werden. Beispiel: `return View("Views/Home/About.cshtml");`. Alternativ können Sie einen relativen Pfad aus dem Verzeichnis Controller-spezifische innerhalb der *Ansichten* Directory Ansichten in verschiedenen Verzeichnissen an. Zum Beispiel: `return View("../Manage/Index");` innerhalb der *Home* Controller. Auf ähnliche Weise können Sie das aktuelle Controller-spezifische Verzeichnis durchlaufen: `return View("./About");`. Beachten Sie, dass relative Pfade verwenden, nicht die *cshtml* Erweiterung. Wie bereits erwähnt wurde führen Sie die bewährte Methode zum Organisieren von der Dateistruktur für Ansichten, die Beziehungen zwischen Domänencontrollern, Aktionen und Ansichten für die Verwaltbarkeit und Klarheit entsprechend aus.

> [!NOTE]
> [Teilansichten](partial.md) und [anzeigen Komponenten](view-components.md) ähnliche (aber nicht identisch) Ermittlungsmechanismus verwenden.

> [!NOTE]
> Sie können anpassen, dass die Standardkonvention bezüglich unter dem Ansichten innerhalb der app gespeichert werden, mithilfe ein benutzerdefinierten `IViewLocationExpander`.

>[!TIP]
> Sichtnamen möglicherweise Groß-/Kleinschreibung beachtet, abhängig von dem zugrunde liegenden Dateisystem. Aus Kompatibilitätsgründen Betriebssystemen müssen Sie immer übereinstimmen Sie Fall zwischen Controller und Aktionsnamen und zugeordnete Ansicht-Ordner und Dateinamen.

## <a name="passing-data-to-views"></a>Übergeben von Daten mit Ansichten

Sie können Daten mit Ansichten mithilfe mehrerer Mechanismen übergeben. Die zuverlässigste Methode ist die Angabe ein *Modell* Typ in der Ansicht (gemeinhin als eine *Viewmodel*, um die Unterscheidung von Business-Domäne Modelltypen), und übergeben Sie eine Instanz dieses Typs der Ansicht von der Aktion. Es wird empfohlen, dass Sie ein Modell oder Sicht verwenden, um Daten an eine Ansicht zu übergeben. Dadurch wird die Ansicht zu starken typüberprüfung nutzen. Sie können angeben, dass ein Modell für eine Sicht mit der `@model` Richtlinie:

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [1]}} -->

```html
@model WebApplication1.ViewModels.Address
   <h2>Contact</h2>
   <address>
       @Model.Street<br />
       @Model.City, @Model.State @Model.PostalCode<br />
       <abbr title="Phone">P:</abbr>
       425.555.0100
   </address>
   ```

Nachdem ein Modell für eine Sicht angegeben wurde, kann die Instanz gesendet, um die Sicht zugegriffen werden, in einer stark typisierten mit `@Model` wie oben gezeigt. Um eine Instanz des Modelltyps auf die Ansicht zu gewährleisten, werden der Controller es als Parameter übergeben:

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "csharp", "highlight_args": {"hl_lines": [13]}} -->

```csharp
public IActionResult Contact()
   {
       ViewData["Message"] = "Your contact page.";

       var viewModel = new Address()
       {
           Name = "Microsoft",
           Street = "One Microsoft Way",
           City = "Redmond",
           State = "WA",
           PostalCode = "98052-6399"
       };
       return View(viewModel);
   }
   ```

Es gibt keinerlei Beschränkungen, die Typen, die für eine Sicht als ein Modell bereitgestellt werden können. Es wird empfohlen Ansichtsmodelle mit wenigen oder gar keinen Verhalten einfaches altes CLR Object (POCO) übergeben, damit die Geschäftslogik an anderer Stelle in der app gekapselt werden. Ein Beispiel dieses Ansatzes ist die *Adresse* Viewmodel im obigen Beispiel verwendet:

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "csharp", "highlight_args": {"hl_lines": [13]}} -->

```csharp
namespace WebApplication1.ViewModels
   {
       public class Address
       {
           public string Name { get; set; }
           public string Street { get; set; }
           public string City { get; set; }
           public string State { get; set; }
           public string PostalCode { get; set; }
       }
   }
   ```

> [!NOTE]
> Nichts daran hindert Sie die gleichen Klassen als Ihr Modell Geschäftstypen und Ihre Anzeige Modelltypen verwenden. Allerdings werden und voneinander getrennt können Ihre Ansichten unabhängig von Ihrer Domäne oder der Persistenz-Modell abweichen, bietet einige Sicherheitsvorteile bei als auch (für Modelle, die Benutzer an der Anwendung mit sendet [modellbindung](../models/model-binding.md)).

### <a name="loosely-typed-data"></a>Lose typisierte Daten

Zusätzlich zu den stark typisierten Ansichten haben alle Ansichten Zugriff auf eine schwach typisierte Auflistung von Daten. Diese derselben Sammlung verwiesen werden kann, entweder über die `ViewData` oder `ViewBag` Eigenschaften für Controller und Ansichten. Die `ViewBag` Eigenschaft ist ein Wrapper um `ViewData` enthält, die eine dynamische Ansicht über diese Sammlung. Es ist keine separate Auflistung.

`ViewData`erfolgt ein Dictionary-Objekt über `string` Schlüssel. Können Sie speichern und Abrufen von Objekten, und Sie müssen diese für einen bestimmten Typ umwandeln, wenn Sie zu extrahieren. Sie können `ViewData` Daten von einem Controller mit Ansichten, sowie Ansichten (, Teilansichten und Layouts) übergeben. Zeichenfolgendaten können gespeichert und direkt, ohne die Notwendigkeit einer Typumwandlung verwendet werden.

Legen Sie einige Werte für `ViewData` in einer Aktion:

```csharp
public IActionResult SomeAction()
   {
       ViewData["Greeting"] = "Hello";
       ViewData["Address"]  = new Address()
       {
           Name = "Steve",
           Street = "123 Main St",
           City = "Hudson",
           State = "OH",
           PostalCode = "44236"
       };

       return View();
   }
   ```

Arbeiten Sie mit den Daten in einer Ansicht:

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [3, 6]}} -->

```html
@{
       // Requires cast
       var address = ViewData["Address"] as Address;
   }

   @ViewData["Greeting"] World!

   <address>
       @address.Name<br />
       @address.Street<br />
       @address.City, @address.State @address.PostalCode
   </address>
   ```

Die `ViewBag` Objekte bietet dynamische Zugriff auf die Objekte, die in gespeicherten `ViewData`. Dies kann einfacher zu handhaben, da es keine Typumwandlung erforderlich sein. Das gleiche Beispiel wie oben mit `ViewBag` anstelle eines stark typisierten `address` Instanz in der Sicht:

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [1, 4, 5, 6]}} -->

```html
@ViewBag.Greeting World!

   <address>
       @ViewBag.Address.Name<br />
       @ViewBag.Address.Street<br />
       @ViewBag.Address.City, @ViewBag.Address.State @ViewBag.Address.PostalCode
   </address>
   ```

> [!NOTE]
> Da beide auf das gleiche zugrunde liegende verweisen `ViewData` Auflistung Sie mischen und Zuordnen zwischen `ViewData` und `ViewBag` beim Lesen und Schreiben der Werte, wenn einfache.

### <a name="dynamic-views"></a>Dynamische Ansichten

Sichten, die einen Modelltyp nicht deklarieren, aber eine Modellinstanz, die an sie übergeben haben, können dieser Instanz dynamisch verweisen. Angenommen, wenn eine Instanz von `Address` übergeben wird, um eine Sicht, die nicht deklariert eine `@model`, die Sicht wären immer noch in der Lage, dynamisch wie gezeigt auf die Eigenschaften der Instanz zu verweisen:

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [13, 16, 17, 18]}} -->

```html
<address>
       @Model.Street<br />
       @Model.City, @Model.State @Model.PostalCode<br />
       <abbr title="Phone">P:</abbr>
       425.555.0100
   </address>
   ```

Diese Funktion kann einige Flexibilität bieten, aber keinen Schutz Kompilierung kein(e) IntelliSense bietet. Wenn die Eigenschaft nicht vorhanden ist, wird die Seite zur Laufzeit fehlschlagen.

## <a name="more-view-features"></a>Weitere Funktionen

[Kennzeichnen von Hilfsprogrammen](tag-helpers/intro.md) erleichtern das Hinzufügen von serverseitigen Verhalten zu vorhandenen HTML-Tags, vermeiden von benutzerdefiniertem Code oder Hilfsprogramme in Ansichten verwendet werden müssen. Tag-Hilfsprogrammen gelten als Attribute HTML-Elementen, die ignoriert werden, von Editoren, die nicht vertraut, sodass ansichtsmarkup bearbeitet und in einer Vielzahl von Tools gerendert werden. Tag-Hilfsmethoden sind vielseitig verwendbar, und insbesondere kann [arbeiten mit Formularen](working-with-forms.md) sehr viel einfacher.

Generieren von benutzerdefinierten HTML-Markup kann mit vielen integrierten HTML-Hilfsmethoden erreicht werden, und eine komplexer Benutzeroberflächen-Logik (u. u. mit einem eigenen datenanforderungen) kann gekapselt werden [Ansichtskomponenten](view-components.md). Anzeigen von Komponenten bieten die gleiche Trennung von Anliegen, die den Controller und Ansichten bieten und können entfällt der Bedarf für Aktionen und Ansichten für den Umgang mit Daten, die durch allgemeine Elemente der Benutzeroberfläche verwendet.

Wie viele andere Aspekte von ASP.NET Core Ansichten unterstützen [Abhängigkeitsinjektion](../../fundamentals/dependency-injection.md), sodass Dienste sind [in Sichten eingefügt](dependency-injection.md).
