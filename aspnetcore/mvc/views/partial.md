---
title: Verwenden von Teilansichten in ASP.NET Core
author: ardalis
description: Erfahren Sie, wie eine Teilansicht zu einer Ansicht werden kann, die innerhalb einer anderen Ansicht gerendert wird, und wann diese in ASP.NET Core-Apps verwendet werden sollten.
ms.author: riande
ms.custom: mvc
ms.date: 07/06/2018
uid: mvc/views/partial
ms.openlocfilehash: 983f3caae34b21b46d8f556e70673cf3c97abbd3
ms.sourcegitcommit: 661d30492d5ef7bbca4f7e709f40d8f3309d2dac
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "37938458"
---
# <a name="partial-views-in-aspnet-core"></a>Verwenden von Teilansichten in ASP.NET Core

Von [Steve Smith](https://ardalis.com/), [Maher JENDOUBI](https://twitter.com/maherjend), [Rick Anderson](https://twitter.com/RickAndMSFT) und [Scott Sauber](https://twitter.com/scottsauber)

ASP.NET Core MVC unterstützt Teilansichten, die nützlich zum gemeinsamen Verwenden wiederverwendbarer Teile von Webseiten für unterschiedliche Ansichten sind.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/partial/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-are-partial-views"></a>Informationen zu Teilansichten

Bei einer Teilansicht handelt es sich um eine Ansicht die innerhalb einer anderen Ansicht gerendert wird. Die HTML-Ausgabe, die durch das Ausführen der Teilansicht generiert wird, wird in die aufrufende bzw. die übergeordnete Ansicht gerendert. Wie Ansichten verwenden auch Teilansichten die Erweiterung *.cshtml*.

Die Projektvorlage **Webanwendung** von ASP.NET Core 2.1 enthält z.B. die Teilansicht *_CookieConsentPartial.cshtml*. Die Teilansicht wird innerhalb von *_Layout.cshtml* geladen:

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Shared/_Layout.cshtml?name=snippet_CookieConsentPartial)]

## <a name="when-to-use-partial-views"></a>Wann werden Teilansichten verwendet?

Teilansichten stellen eine effektive Möglichkeit dar, große Ansichten in kleinere Komponenten zu unterteilen. Dadurch können Duplizierungen von Ansichtsinhalt reduziert und Ansichtselemente wiederverwendet werden. Häufig verwendete Layoutelemente müssen in [_Layout.cshtml](xref:mvc/views/layout) angegeben werden. Wiederverwendbarer Inhalt, der nicht zum Layout gehört, kann in Teilansichten gekapselt werden.

Auf einer komplexen Seite, die aus mehreren logischen Bestandteilen besteht, ist es hilfreich, mit jedem Bestandteil als einzelne Teilansicht zu arbeiten. Jeder Bestandteil der Seite kann isoliert vom Rest der Seite angezeigt werden. Die Ansicht der Seite selbst wird einfacher, da sie nur aus der allgemeinen Seitenstruktur sowie Aufrufen zum Rendern der Teilansichten besteht.

> [!TIP]
> Halten Sie sich in Ihren Ansichten an das [Don’t Repeat Yourself-Prinzip](https://deviq.com/don-t-repeat-yourself/).

## <a name="declare-partial-views"></a>Deklarieren von Teilansichten

Teilansichten werden auf dieselbe Weise wie normale Ansichten erstellt, und zwar indem eine *CSHTML*-Datei im Ordner *Ansichten* erstellt wird. Es gibt keinen Unterschied in der Semantik zwischen Teilansichten und normalen Ansichten. Sie werden jedoch unterschiedlich gerendert. Sie können über eine Ansicht verfügen, die direkt über das [ViewResult](/dotnet/api/microsoft.aspnetcore.mvc.viewresult) eines Controllers zurückgegeben wird. Dieselbe Ansicht kann dann auch als Teilansicht verwendet werden. Der Hauptunterschied beim Rendern von Teilansichten und Ansichten liegt darin, dass Teilansichten nicht die Datei *_ViewStart.cshtml* ausführen. Normale Ansichten führen *_ViewStart.cshtml* jedoch aus. Weiter Informationen zu *_ViewStart.cshtml* finden Sie unter [Layout](xref:mvc/views/layout).

Dateinamen von Teilansichten beginnen per Konvention oft mit `_`. Diese Namenskonvention ist keine Voraussetzung, sie hilft jedoch dabei, Teilansichten von normalen Ansichten visuell zu unterscheiden.

## <a name="reference-a-partial-view"></a>Verweisen auf eine Teilansicht

Innerhalb einer Ansichtsseite gibt es mehrere Möglichkeiten, eine Teilansicht zu rendern. Die bewährte Methode ist die Verwendung des asynchronen Renderings.

::: moniker range=">= aspnetcore-2.1"

### <a name="partial-tag-helper"></a>Hilfsprogramm für Teiltags

Das Hilfsprogramm für Teiltags erfordert ASP.NET Core 2.1 oder höher. Es rendert asynchron und verwendet eine HTML-ähnliche Syntax:

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Home/Discovery.cshtml?name=snippet_PartialTagHelper)]

Weitere Informationen finden Sie unter <xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper>.

::: moniker-end

### <a name="asynchronous-html-helper"></a>Hilfsprogramm für asynchrone HTML

Wenn Sie ein HTML-Hilfsprogramm verwenden, ist [PartialAsync](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partialasync#Microsoft_AspNetCore_Mvc_Rendering_HtmlHelperPartialExtensions_PartialAsync_Microsoft_AspNetCore_Mvc_Rendering_IHtmlHelper_System_String_) eine bewährte Methode. Es gibt den Typ [IHtmlContent](/dotnet/api/microsoft.aspnetcore.html.ihtmlcontent) zurück, der in einem `Task` umschlossenen ist. Sie können auf die Methode verweisen, indem Sie den Aufruf `@` voranstellen:

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Home/Discovery.cshtml?name=snippet_PartialAsync)]

Alternativ können Sie eine Teilansicht auch mit [RenderPartialAsync](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartialasync) rendern. Diese Methode gibt kein Ergebnis zurück. Sie streamt die gerenderte Ausgabe direkt an die Antwort. Da die Methode kein Ergebnis zurückgibt, muss sie in einem Razor-Codeblock aufgerufen werden:

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Home/Discovery.cshtml?name=snippet_RenderPartialAsync)]

Da sie das Ergebnis direkt streamt, ist die Leistung von `RenderPartialAsync` in einigen Szenarios besser. Es wird jedoch empfohlen, `PartialAsync` zu verwenden.

### <a name="synchronous-html-helper"></a>Hilfsprogramm für synchrone HTML

[Partial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partial) und [RenderPartial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartial) sind die synchronen Entsprechungen von `PartialAsync` und `RenderPartialAsync`. Verwenden Sie diese synchronen Entsprechungen nicht, da sie in manchen Szenarios zu einem Deadlock führen können. Zukünftige Releases enthalten die synchronen Methoden nicht.

> [!IMPORTANT]
> Wenn Ihre Ansichten Code ausführen müssen, verwenden Sie anstelle von Teilansichten [Ansichtskomponenten](xref:mvc/views/view-components).

::: moniker range=">= aspnetcore-2.1"

In ASP.NET Core 2.1 oder höher führt das Aufrufen von `Partial` oder `RenderPartial` zu einer Analysetoolwarnung. Beispielsweise führt die Nutzung von `Partial` zu folgender Warnmeldung:

> Die Verwendung von IHtmlHelper.Partial kann zu Anwendungsdeadlocks führen. Verwenden Sie deshalb das Taghilfsprogramm `<partial>` oder `IHtmlHelper.PartialAsync`.

Ersetzen Sie Aufrufe von `@Html.Partial` durch `@await Html.PartialAsync` oder durch das Hilfsprogramm für Teiltags. Weiter Informationen zur Migration des Teiltaghilfsprogramms finden Sie unter [Migrate from an HTML Helper (Migration von einem HTML-Hilfsprogramm)](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper#migrate-from-an-html-helper)

::: moniker-end

## <a name="partial-view-discovery"></a>Ermittlung von Teilansichten

Es gibt verschiedene Möglichkeiten, um auf den Speicherort einer Teilansicht zu verweisen. Zum Beispiel:

::: moniker range=">= aspnetcore-2.1"

```cshtml
// Uses a view in current folder with this name.
// If none is found, searches the Shared folder.
<partial name="_ViewName" />

// A view with this name must be in the same folder
<partial name="_ViewName.cshtml" />

// Locate the view based on the app root.
// Paths that start with "/" or "~/" refer to the app root.
<partial name="~/Views/Folder/_ViewName.cshtml" />
<partial name="/Views/Folder/_ViewName.cshtml" />

// Locate the view using a relative path
<partial name="../Account/_LoginPartial.cshtml" />
```

Das vorherige Beispiel verwendet das Teiltaghilfsprogramm, das ASP.NET Core 2.1 oder höher erfordert. Das folgende Beispiel verwendet Hilfsprogramme für asynchrone HTML, um den gleichen Task durchzuführen.

::: moniker-end

```cshtml
// Uses a view in current folder with this name.
// If none is found, searches the Shared folder.
@await Html.PartialAsync("_ViewName")

// A view with this name must be in the same folder
@await Html.PartialAsync("_ViewName.cshtml")

// Locate the view based on the app root.
// Paths that start with "/" or "~/" refer to the app root.
@await Html.PartialAsync("~/Views/Folder/_ViewName.cshtml")
@await Html.PartialAsync("/Views/Folder/_ViewName.cshtml")

// Locate the view using a relative path
@await Html.PartialAsync("../Account/_LoginPartial.cshtml")
```

Sie können verschiedene Teilansichten mit demselben Dateinamen in unterschiedlichen Ordnern verwenden. Wenn Sie auf die Namen der Ansichten (ohne Dateierweiterung) verweisen, verwenden die Ansichten in jedem Ordner die Teilansichten, die sich in diesem Ordner befinden. Sie können auch eine Standardteilansicht festlegen, die verwendet werden soll, und sie im Ordner *Freigegeben* ablegen. Die freigegebene Teilansicht wird in allen Ansichten verwendet, die über keine eigene Version der Teilansicht verfügen. Sie können über eine Standardteilansicht verfügen (im Ordner *Freigegeben*), die von einer Teilansicht überschrieben wird, die denselben Namen wie die übergeordnete Ansicht hat und sich im selben Ordner wie diese befindet.

Teilansichten können *miteinander verkettet* werden – eine Teilansicht kann eine andere Teilansicht aufrufen (allerdings nur, wenn Sie keine Schleife erstellen). In jeder Ansicht oder Teilansicht beziehen sich relative Pfade ausschließlich auf die jeweilige Ansicht und nicht auf die Stammansicht oder die übergeordnete Ansicht.

> [!NOTE]
> Ein [Razor](xref:mvc/views/razor) `section`-Abschnitt, der in einer Teilansicht definiert ist, ist für übergeordnete Ansichten nicht sichtbar. Die `section`-Anweisung ist nur für die Teilansicht, in der sie definiert ist, sichtbar.

## <a name="access-data-from-partial-views"></a>Zugriff auf Daten aus Teilansichten

Wenn eine Teilansicht instanziiert wird, erhält sie eine Kopie des `ViewData`-Wörterbuchs der übergeordneten Ansicht. An den Daten vorgenommene Änderungen in der Teilansicht werden in der übergeordneten Ansicht nicht beibehalten. Ein `ViewData`-Wörterbuch, das in einer Teilansicht verändert wird, geht verloren, wenn die Teilansicht eine Rückgabe ausgibt.

Sie können eine Instanz von [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) an die Teilansicht übergeben:

```cshtml
@await Html.PartialAsync("_PartialName", customViewData)
```

Außerdem können Sie ein Modell an eine Teilansicht übergeben. Dabei kann es sich um das Ansichtsmodell einer Seite oder ein benutzerdefiniertes Objekt handeln. Sie können ein Modell an `PartialAsync` oder `RenderPartialAsync` übergeben:

```cshtml
@await Html.PartialAsync("_PartialName", viewModel)
```

Sie können eine Instanz von `ViewDataDictionary` und ein Ansichtsmodell an eine Teilansicht übergeben:

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/Read.cshtml?name=snippet_PartialAsync)]

Das folgende Markup stellt die *Views/Articles/Read.cshtml*-Ansicht dar, die zwei Teilansichten enthält. Die zweite Teilansicht übergibt ein Modell und `ViewData` an die Teilansicht. Verwenden Sie die Konstruktorüberladung des nachfolgend markierten `ViewDataDictionary`, um ein neues `ViewData`-Wörterbuch zu übergeben und gleichzeitig das vorhandene `ViewData` zu behalten.

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/Read.cshtml?name=snippet_ReadPartialView&highlight=17-20)]

*Views/Shared/_AuthorPartial*:

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Shared/_AuthorPartial.cshtml)]

Die *_ArticleSection*-Teilansicht:

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/_ArticleSection.cshtml)]

Die Teilansichten werden zur Laufzeit in der übergeordneten Ansicht gerendert, die wiederum in *_Layout.cshtml* freigegeben wird.

![Ausgabe der Teilansicht](partial/_static/output.png)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

::: moniker range=">= aspnetcore-2.1"

* <xref:mvc/views/razor>
* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper>
* <xref:mvc/views/view-components>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

* <xref:mvc/views/razor>
* <xref:mvc/views/view-components>

::: moniker-end
