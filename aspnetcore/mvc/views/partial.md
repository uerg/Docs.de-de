---
title: Teilansichten
author: ardalis
description: Verwenden von Teilansichten in ASP.NET Core MVC
ms.author: riande
manager: wpickett
ms.date: 03/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/partial
ms.openlocfilehash: e5c8aac855a1f4ec4c6f08087dbe77f6820cc506
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
---
# <a name="partial-views"></a>Teilansichten

Durch [Steve Smith](https://ardalis.com/), [Maher JENDOUBI](https://twitter.com/maherjend), und [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core MVC unterstützt Teilansichten sind hilfreich, wenn Sie wieder verwendbare Teile der Webseiten, die Sie zwischen verschiedenen Ansichten freigeben möchten.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/partial/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-are-partial-views"></a>Was sind Teilansichten?

Eine partielle Ansicht ist eine Sicht, die innerhalb einer anderen Ansicht gerendert wird. Die HTML-Ausgabe generiert, die durch Ausführen der Teilansicht wird in der Ansicht aufrufen (oder übergeordneten) gerendert. Ebenso wie die Ansichten, Teilansichten verwenden die *cshtml* Dateierweiterung.

## <a name="when-should-i-use-partial-views"></a>Wann sollte ich Teilansichten verwenden?

Teilansichten sind eine effektive Möglichkeit zum Aufteilen von großen Ansichten in kleinere Komponenten. Sie können die Duplizierung Inhalt anzeigen verringert und Zulassen von Ansichtselementen wiederverwendet wird. Allgemeine Layoutelemente sollte angegeben werden, [_Layout.cshtml](layout.md). Nicht-Layout wiederverwendbaren Inhalt kann in Teilansichten gekapselt werden.

Wenn Sie eine komplexe Seite setzt sich aus mehreren logischen Teilen verfügen, kann es hilfreich sein, die mit jedem Datenelement als eigene partielle Ansicht arbeiten sein. Jeder Teil der Seite kann isoliert vom Rest der Seite angezeigt werden, und die Ansicht für die Seite selbst wird viel einfacher, da sie nur die allgemeine Seitenstruktur und der Aufrufe an die Teilansichten zu rendern enthält.

Tipp: Führen Sie die [Don't wiederholen selbst Prinzip](http://deviq.com/don-t-repeat-yourself/) in Ihren Ansichten.

## <a name="declaring-partial-views"></a>Deklarieren von Teilansichten

Teilansichten werden wie jede andere Sicht erstellt: Sie erstellen eine *cshtml* innerhalb der *Ansichten* Ordner. Es gibt keine semantische Unterschied zwischen einer Teilansicht und eine reguläre Ansicht - sie werden lediglich anders gerendert. Sie können eine Sicht, die direkt von eines Controllers zurückgegeben wird `ViewResult`, und die gleiche Sicht kann verwendet werden, wie eine Teilansicht. Der Hauptunterschied zwischen wie eine Sicht und eine partielle Ansicht gerendert werden besteht darin, Teilansichten nicht ausgeführt werden *Ansichten "_viewstart.cshtml"* (während Ansichten erfahren Sie mehr über - *Ansichten "_viewstart.cshtml"* in [Layout ](layout.md)).

## <a name="referencing-a-partial-view"></a>Verweisen auf eine Teilansicht

Von innerhalb einer Seite anzeigen und gibt es mehrere Möglichkeiten einer Teilansicht rendern. Die am einfachsten ist die Verwendung `Html.Partial`, welche gibt eine `IHtmlString` kann verwiesen werden, durch den Aufruf mit voranstellen `@`:

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Home/About.cshtml?range=9)]

Die `PartialAsync` Methode ist verfügbar, für teilweise mit asynchronen Code anzeigt (obwohl Code in den Ansichten im Allgemeinen nicht empfehlenswert ist):

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Home/About.cshtml?range=8)]

Sie können eine Teilansicht mit Rendern `RenderPartial`. Diese Methode wird nicht ein Ergebnis zurückgegeben. Er überträgt die gerenderte Ausgabe direkt in die Antwort. Da es ein Ergebnis zurückgibt, muss er innerhalb eines Blocks der Razor-Code aufgerufen werden (Sie können auch aufrufen `RenderPartialAsync` bei Bedarf):

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Home/About.cshtml?range=10-12)]

Da es direkt das Ergebnis streamt `RenderPartial` und `RenderPartialAsync` möglicherweise eine bessere Leistung in einigen Szenarien. Verwenden Sie in den meisten Fällen es, Sie empfohlen ist jedoch `Partial` und `PartialAsync`.

> [!NOTE]
> Wenn Ihre Ansichten zum Ausführen von Code müssen, das empfohlene Muster ist die Verwendung einer [Ansichtskomponente](view-components.md) anstelle einer Teilansicht.

### <a name="partial-view-discovery"></a>Teilansicht-Ermittlung

Wenn eine Teilansicht verwiesen wird, finden Sie in ihrem Speicherort auf verschiedene Weise:

```text
// Uses a view in current folder with this name
// If none is found, searches the Shared folder
@Html.Partial("ViewName")

// A view with this name must be in the same folder
@Html.Partial("ViewName.cshtml")

// Locate the view based on the application root
// Paths that start with "/" or "~/" refer to the application root
@Html.Partial("~/Views/Folder/ViewName.cshtml")
@Html.Partial("/Views/Folder/ViewName.cshtml")

// Locate the view using relative paths
@Html.Partial("../Account/LoginPartial.cshtml")
```

Sie können verschiedene Teilansichten mit dem gleichen Namen in andere Ansichtordner verfügen. Wenn die Ansichten von Namen (ohne Dateierweiterung) verwiesen wird, werden Ansichten im jeweiligen Ordner Teilansicht im gleichen Ordner mit diesen verwenden. Sie können auch angeben eine Standard-Teilansicht zu verwenden, legen Sie das Element in der *Shared* Ordner. Die freigegebene Teilansicht wird von Sichten verwendet werden, die nicht über eine eigene Version der Teilansicht verfügen. Können Sie eine partielle Standardansicht haben (in *Shared*), die durch eine Teilansicht mit dem gleichen Namen im gleichen Ordner wie die übergeordnete Ansicht überschrieben wird.

Teilansichten können werden *verkettet*. Eine Teilansicht kann also eine andere partielle Ansicht aufrufen, (solange Sie keine Schleife erstellen). Sind relative Pfade innerhalb jeder Sicht oder eine Teilansicht immer relativ zu dieser Ansicht anzeigen, nicht der Stammknoten oder übergeordneter aus.

> [!NOTE]
> Wenn Sie deklarieren eine [Razor](razor.md) `section` in eine Teilansicht, es werden für zugehöriger sichtbar; sie werden auf die Teilansicht beschränkt.

## <a name="accessing-data-from-partial-views"></a>Zugriff auf Daten aus Teilansichten

Wenn eine Teilansicht instanziiert wird, ruft Sie eine Kopie der übergeordneten Ansicht `ViewData` Wörterbuch. Updates an, um die Daten in die partielle Ansicht werden nicht in der übergeordneten Ansicht beibehalten. `ViewData`geändert in eine partielle Ansicht geht verloren, wenn die Teilansicht zurückgibt.

Sie können eine Instanz von übergeben `ViewDataDictionary` zur Teilansicht:

```csharp
@Html.Partial("PartialName", customViewData)
   ```

Sie können auch ein Modell in einer Teilansicht übergeben. Dies kann Ansichtsmodell, in der Seite oder einem Teil davon oder ein benutzerdefiniertes Objekt sein. Sie können ein Miningmodell übergeben `Partial`,`PartialAsync`, `RenderPartial`, oder `RenderPartialAsync`:

```csharp
@Html.Partial("PartialName", viewModel)
   ```

Sie können eine Instanz von übergeben `ViewDataDictionary` und einem Ansichtsmodell in eine partielle Ansicht:

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Articles/Read.cshtml?range=15-16)]

Das Markup veranschaulicht die *Views/Articles/Read.cshtml* anzeigen, die zwei Teilansichten enthält. Die zweite Teilansicht übergibt in einem Modell und `ViewData` auf die Teilansicht. Sie können neue übergeben `ViewData` Wörterbuch Beibehaltung der vorhandenen `ViewData` bei Verwendung der Überladung des Konstruktors von der `ViewDataDictionary` unten markiert:

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Articles/Read.cshtml)]

*Views/Shared/AuthorPartial*:

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Shared/AuthorPartial.cshtml)]

Die *ArticleSection* partielle:

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Articles/ArticleSection.cshtml)]

Zur Laufzeit die Replikatsgruppenelemente gerendert werden in der übergeordneten Ansicht, die selbst gerendert wird innerhalb der freigegebenen *_Layout.cshtml*

![Teilansicht-Ausgabe](partial/_static/output.png)
