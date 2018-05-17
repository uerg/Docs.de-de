---
title: Verwenden von Teilansichten in ASP.NET Core
author: ardalis
description: Erfahren Sie, wie eine Teilansicht zu einer Ansicht werden kann, die innerhalb einer anderen Ansicht gerendert wird, und wann diese in ASP.NET Core-Apps verwendet werden sollten.
manager: wpickett
ms.author: riande
ms.date: 03/14/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/partial
ms.openlocfilehash: 3deaaeb666e5443d0784f2ac6977e58e1b25d711
ms.sourcegitcommit: 71b93b42cbce8a9b1a12c4d88391e75a4dfb6162
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2018
---
# <a name="partial-views-in-aspnet-core"></a>Verwenden von Teilansichten in ASP.NET Core

Von [Steve Smith](https://ardalis.com/), [Maher JENDOUBI](https://twitter.com/maherjend), [Rick Anderson](https://twitter.com/RickAndMSFT) und [Scott Sauber](https://twitter.com/scottsauber)

ASP.NET Core MVC unterstützt Teilansichten. Diese sind nützlich, wenn Sie über wiederverwendbare Teile von Webseiten verfügen, die Sie für verschiedene Ansichten freigeben möchten.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/partial/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-are-partial-views"></a>Informationen zu Teilansichten

Bei einer Teilansicht handelt es sich um eine Ansicht die innerhalb einer anderen Ansicht gerendert wird. Die HTML-Ausgabe, die durch das Ausführen der Teilansicht generiert wird, wird in die aufrufende bzw. die übergeordnete Ansicht gerendert. Wie Ansichten verwenden auch Teilansichten die Erweiterung *.cshtml*.

## <a name="when-should-i-use-partial-views"></a>Verwendung von Teilansichten

Teilansichten stellen eine effektive Möglichkeit dar, große Ansichten in kleinere Komponenten zu unterteilen. Dadurch können Duplizierungen von Ansichtsinhalt reduziert und Ansichtselemente wiederverwendet werden. Häufig verwendete Layoutelemente müssen in [_Layout.cshtml](layout.md) angegeben werden. Wiederverwendbarer Inhalt, der nicht zum Layout gehört, kann in Teilansichten gekapselt werden.

Wenn Sie über eine komplexe Seite verfügen, die aus mehreren logischen Bestandteilen besteht, ist es hilfreich, wenn Sie mit jedem Bestandteil als einzelne Teilansichten arbeiten. Jeder Bestandteil der Seite kann unabhängig vom Rest der Seite angezeigt werden, wodurch die Ansicht der Seite viel einfacher wird, da sie nur die Gesamtseitenstruktur und Aufrufe enthält, über die die Teilansichten gerendert werden.

Tipp: Halten Sie sich in Ihren Ansichten an das [Don’t Repeat Yourself-Prinzip](http://deviq.com/don-t-repeat-yourself/).

## <a name="declaring-partial-views"></a>Deklarieren von Teilansichten

Teilansichten werden auf dieselbe Weise wie andere Ansichten erstellt: Erstellen Sie eine *CSHTML*-Datei im Ordner *Ansichten*. Es gibt keinen Unterschied in der Semantik zwischen Teilansichten und normalen Ansichten. Sie werden nur unterschiedlich gerendert. Sie können über eine Ansicht verfügen, die direkt über das `ViewResult` eines Controllers zurückgegeben wird. Dieselbe Ansicht kann dann auch als Teilansicht verwendet werden. Der Hauptunterschied beim Rendern von Teilansichten und Ansichten liegt darin, dass Teilansichten nicht die Datei *_ViewStart.cshtml* ausführen, Ansichten hingegen schon (weitere Informationen dazu erhalten Sie unter *_ViewStart.cshtml* unter [Layout](layout.md)).

## <a name="referencing-a-partial-view"></a>Verweisen auf eine Teilansicht

Es gibt mehrere Möglichkeiten, Teilansichten auf Ansichtsseiten zu rendern. Am Besten ist es, `Html.PartialAsync` zu verwenden, wodurch eine `IHtmlString` zurückgegeben wird, auf die verwiesen werden kann, indem dem Aufruf `@` vorangestellt wird:

[!code-cshtml[](partial/sample/src/PartialViewsSample/Views/Home/About.cshtml?range=8)]

Sie können Teilansichten mithilfe von `RenderPartialAsync` rendern. Diese Methode gibt kein Ergebnis zurück, sondern streamt die gerenderte Ausgabe direkt an die Antwort. Da sie kein Ergebnis zurückgibt, muss sie in einem Razor-Codeblock aufgerufen werden:

[!code-cshtml[](partial/sample/src/PartialViewsSample/Views/Home/About.cshtml?range=11-13)]

Da die Methode das Ergebnis direkt streamt, ist die Leistung von `RenderPartialAsync` in einigen Szenarios besser. Es wird jedoch empfohlen, `PartialAsync` zu verwenden.

Obwohl es synchrone Entsprechungen von `Html.PartialAsync` (`Html.Partial`) und `Html.RenderPartialAsync` (`Html.RenderPartial`) gibt, sollten Sie diese synchronen Entsprechungen nicht verwenden, da in manchen Szenarios ein Deadlock auftreten könnte. Synchrone Methoden werden in zukünftigen Versionen nicht mehr verfügbar sein.

> [!NOTE]
> Wenn Ihre Ansichten Code ausführen müssen, wird empfohlen, dass Sie anstelle von Teilansichten [Ansichtskomponenten](view-components.md) verwenden.

### <a name="partial-view-discovery"></a>Ermittlung von Teilansichten

Es gibt verschiedene Möglichkeiten, um auf den Speicherort einer Teilansicht zu verweisen:

```cshtml
// Uses a view in current folder with this name
// If none is found, searches the Shared folder
@await Html.PartialAsync("ViewName")

// A view with this name must be in the same folder
@await Html.PartialAsync("ViewName.cshtml")

// Locate the view based on the application root
// Paths that start with "/" or "~/" refer to the application root
@await Html.PartialAsync("~/Views/Folder/ViewName.cshtml")
@await Html.PartialAsync("/Views/Folder/ViewName.cshtml")

// Locate the view using relative paths
@await Html.PartialAsync("../Account/LoginPartial.cshtml")
```

Sie können verschiedene Teilansichten mit demselben Namen in unterschiedlichen Ordnern verwenden. Wenn Sie auf die Namen der Ansichten (ohne Erweiterung) verweisen, verwenden die Ansichten in jedem Ordner die Teilansichten, die sich im selben Ordner befinden. Sie können auch eine Standardteilansicht festlegen, die verwendet werden soll, und sie im Ordner *Freigegeben* ablegen. Die freigegebene Teilansicht wird dann in allen Ansichten verwendet, die über keine eigene Version der Teilansicht verfügen. Sie können über eine Standardteilansicht verfügen (im Ordner *Freigegeben*), die von einer Teilansicht überschrieben wird, die denselben Namen wie die übergeordnete Ansicht hat und sich im selben Ordner wie diese befindet.

Teilansichten können miteinander *verkettet* werden. D.h., eine Teilansicht kann eine andere Teilansicht aufrufen – allerdings nur, wenn Sie keine Schleife erstellen. In jeder Ansicht oder Teilansicht beziehen sich relative Pfade ausschließlich auf die jeweilige Ansicht und nicht auf die Stammansicht oder die übergeordnete Ansicht.

> [!NOTE]
> Wenn Sie einen [Razor](razor.md)-`section` in einer Teilansicht deklarieren, wird dieser Bereich den übergeordneten Ansichten nicht angezeigt, sondern ist nur auf die Teilansicht beschränkt.

## <a name="accessing-data-from-partial-views"></a>Zugreifen auf Daten aus Teilansichten

Wenn eine Teilansicht instanziiert wird, erhält sie eine Kopie des `ViewData`-Wörterbuchs der übergeordneten Ansicht. An den Daten vorgenommene Änderungen in der Teilansicht werden in der übergeordneten Ansicht nicht beibehalten. Ein `ViewData`-Wörterbuch, das in einer Teilansicht verändert wird, geht verloren, wenn die Teilansicht eine Rückgabe ausgibt.

Sie können eine Instanz von `ViewDataDictionary` an die Teilansicht übergeben:

```cshtml
@await Html.PartialAsync("PartialName", customViewData)
```

Außerdem können Sie ein Modell an eine Teilansicht übergeben. Dabei kann es sich um das Ansichtsmodell einer Seite oder ein benutzerdefiniertes Objekt handeln. Sie können ein Modell an `PartialAsync` oder `RenderPartialAsync` übergeben:

```cshtml
@await Html.PartialAsync("PartialName", viewModel)
```

Sie können eine Instanz von `ViewDataDictionary` und ein Ansichtsmodell an eine Teilansicht übergeben:

[!code-cshtml[](partial/sample/src/PartialViewsSample/Views/Articles/Read.cshtml?range=15-16)]

Das nachfolgende Markup stellt die *Views/Articles/Read.cshtml*-Ansicht dar, die zwei Teilansichten enthält. Die zweite Teilansicht übergibt ein Modell und `ViewData` an die Teilansicht. Sie können ein neues `ViewData`-Wörterbuch übergeben und gleichzeitig das vorhandene `ViewData` behalten, wenn Sie die Konstruktorüberladung des nachfolgend markierten `ViewDataDictionary` verwenden:

[!code-cshtml[](partial/sample/src/PartialViewsSample/Views/Articles/Read.cshtml)]

*Views/Shared/AuthorPartial*:

[!code-cshtml[](partial/sample/src/PartialViewsSample/Views/Shared/AuthorPartial.cshtml)]

Die *ArticleSection*-Teilausführung:

[!code-cshtml[](partial/sample/src/PartialViewsSample/Views/Articles/ArticleSection.cshtml)]

Die Teilausführungen werden zur Laufzeit in der übergeordneten Ansicht gerendert, die wiederum in *_Layout.cshtml* freigegeben wird.

![Ausgabe der Teilansicht](partial/_static/output.png)
