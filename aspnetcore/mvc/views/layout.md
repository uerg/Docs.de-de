---
title: Layout
author: ardalis
description: 
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/layout
ms.openlocfilehash: 3e9e5949d8940a33508e24f0da015b49b7ba468c
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
---
# <a name="layout"></a>Layout

Von [Steve Smith](https://ardalis.com/)

Ansichten haben oft visuelle und programmatische Elemente gemeinsam. In diesem Artikel lernen Sie, wie Sie gemeinsam verwendet Layouts und Anweisungen verwenden und gemeinsam verwendeten Code ausführen, bevor Sie Ansichten in Ihrer ASP.NET-App rendern.

## <a name="what-is-a-layout"></a>Was ist ein Layout?

Die meisten Web-Apps haben ein einheitliches Layout, sodass der Benutzer beim Navigieren zwischen den Seiten immer die gleichen Elemente wiedererkennt. Das Layout enthält normalerweise gängige Benutzeroberflächenelemente wie einen App-Header, Navigations- oder Menüelemente und Fußzeilen.

![Beispiel für ein Seitenlayout](layout/_static/page-layout.png)

Gängige HTML-Strukturen wie Skripts und Stylesheets werden häufig von vielen Seiten in einer App verwendet. Alle diese gemeinsamen Elemente werden in einer *Layoutdatei* definiert, auf die dann von jeder Ansicht einer App verwiesen werden kann. Layouts minimieren Codeduplikate in Ansichten, sodass das [DRY-Prinzip (Don‘t Repeat Yourself)](http://deviq.com/don-t-repeat-yourself/) eingehalten wird.

Per Konvention heißt das Standardlayout einer ASP.NET-App `_Layout.cshtml`. In der Projektvorlage von Visual Studio ASP.NET Core MVC befindet sich diese Layoutdatei im `Views/Shared`-Ordner:

![Ordner „Views“ (Ansichten) im Projektmappen-Explorer](layout/_static/web-project-views.png)

Dieses Layout definiert eine allgemeine Vorlage für Ansichten in der App. Apps erfordern kein Layout, und sie können mehr als ein Layout definieren, wobei unterschiedliche Ansichten unterschiedliche Layouts festlegen.

Ein Beispiel-`_Layout.cshtml`:

[!code-html[Main](../../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=42,66)]

## <a name="specifying-a-layout"></a>Festlegen eines Layouts

Razor-Ansichten verfügen über die Eigenschaft `Layout`. Mit dieser Eigenschaft können einzelne Ansichten Layouts festlegen:

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]

Das angegebene Layout kann einen vollständigen Pfad (z.B. `/Views/Shared/_Layout.cshtml`) oder einen Teilnamen (z.B. `_Layout`) verwenden. Wenn ein Teilname angegeben wird, sucht die Razor-Ansichtsengine anhand ihres normalen Erkennungsprozesses nach der Layoutdatei. Der Ordner, der mit einem Controller verknüpft ist, wird als Erstes durchsucht. Anschließend wird der Ordner `Shared` durchsucht. Dieser Erkennungsprozess stimmt mit dem zum Erkennen von [Teilansichten](partial.md) überein.

Standardmäßig muss jedes Layout `RenderBody` aufrufen. An der Stelle des Aufrufs von `RenderBody` wird der Inhalt der Ansicht gerendert.

<a name="layout-sections-label"></a>

### <a name="sections"></a>Abschnitte

Optional kann ein Layout auf mindestens einen *Abschnitt* verweisen, indem es `RenderSection` aufruft. Abschnitte geben an, wo bestimmte Seitenelemente platziert werden sollen. Jeder Aufruf von `RenderSection` kann angeben, ob dieser Abschnitt erforderlich ist. Wenn ein erforderlicher Bereich nicht gefunden werden kann, wird eine Ausnahme ausgelöst. Einzelne Ansichten verwenden die Razor-Syntax `@section`, um den in einem Abschnitt zu rendernden Inhalt anzugeben. Wenn eine Ansicht einen Abschnitt definiert, muss dieser auch gerendert werden. Andernfalls wird eine Ausnahme ausgelöst.

Eine Beispieldefinition von `@section` in einer Ansicht:

```html
@section Scripts {
     <script type="text/javascript" src="/scripts/main.js"></script>
   }
   ```

Im oben stehenden Code werden Validierungsskripts zum Abschnitt `scripts` in einer Ansicht mit einem Formular hinzugefügt. Es ist möglich, dass andere Ansichten in dieser Anwendung keine zusätzlichen Skripts erfordern, sodass auch kein Skriptbereich definiert werden muss.

Die Abschnitte, die in einer Ansicht definiert wurden, stehen nur auf deren Layoutseite zur Verfügung. Teilansichten, Ansichtskomponenten und andere Teile eines Ansichtssystems können nicht auf sie verweisen.

### <a name="ignoring-sections"></a>Ignorieren von Abschnitten

Standardmäßig müssen der Text und die Abschnitte einer Inhaltsseite alle von der Layoutseite gerendert werden. Die Razor-Ansichtsengine erzwingt dies, indem sie erfasst, ob der Text und jeder Abschnitt gerendert wurde.

Rufen Sie die Methoden `IgnoreBody` und `IgnoreSection` auf, um die Ansichtsengine anzuweisen, den Text oder die Abschnitte zu ignorieren.

Der Text und jeder Abschnitt einer Razor-Seite müssen entweder gerendert oder ignoriert werden.

<a name="viewimports"></a>

## <a name="importing-shared-directives"></a>Importieren gemeinsam verwendeter Anweisungen

Ansichten können Razor-Anweisungen verwenden, um verschiedene Aktionen durchzuführen, wie z.B. das Importieren von Namespaces oder das durchführen von [Dependency Injection](dependency-injection.md). Anweisungen, die von mehreren Ansichten gemeinsam verwendet werden, können in einer gemeinsam verwendeten `_ViewImports.cshtml`-Datei angegeben werden. Die `_ViewImports`-Datei unterstützt die folgenden Anweisungen:

* `@addTagHelper`

* `@removeTagHelper`

* `@tagHelperPrefix`

* `@using`

* `@model`

* `@inherits`

* `@inject`

Die Datei unterstützt keine anderen Razor-Features wie Funktionen und Abschnittdefinitionen.

Eine `_ViewImports.cshtml`-Beispieldatei:

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]

Die `_ViewImports.cshtml`-Datei für eine ASP.NET Core MVC-App befindet sich normalerweise im `Views`-Ordner. Eine `_ViewImports.cshtml`-Datei kann auch in einen anderen Ordner verschoben werden. In diesem Fall wird sie nur auf die Ansichten in diesem Ordner und in dessen Unterordnern angewendet. `_ViewImports`-Dateien werden von der Stammebene aus verarbeitet. Dann wird jeder Ordner verarbeitet, der sich auf dem Weg zur Ansicht befindet. Das bedeutet, dass Einstellungen der Stammebene auf der Ordnerebene außer Kraft gesetzt werden.

Wenn z.B. eine `_ViewImports.cshtml`-Datei auf Stammebene `@model` und `@addTagHelper` angibt, und eine andere `_ViewImports.cshtml`-Datei in einer Ansicht des Ordners, der mit einem Controller verknüpft ist, ein anderes `@model` angibt und einen anderen `@addTagHelper` hinzufügt, hat die Ansicht Zugriff auf beide Taghilfsprogramme und verwendet letzteres `@model`.

Wenn mehrere `_ViewImports.cshtml`-Dateien für eine Ansicht ausgeführt werden, sieht das Verhalten der Anweisungen, die sich in den `ViewImports.cshtml`-Dateien befinden, wie folgt aus:

* `@addTagHelper`, `@removeTagHelper`: werden nach der Reihe ausgeführt

* `@tagHelperPrefix`: dasjenige, das der Ansicht am Nächsten ist, setzt die anderen außer Kraft

* `@model`: dasjenige, das der Ansicht am Nächsten ist, setzt die anderen außer Kraft

* `@inherits`: dasjenige, das der Ansicht am Nächsten ist, setzt die anderen außer Kraft

* `@using`: alle einbezogen; Duplikate werden ignoriert

* `@inject`: dasjenige, das der Ansicht am Nächsten ist, setzt für jede Eigenschaft alle anderen mit dem gleichen Namen außer Kraft

<a name="viewstart"></a>

## <a name="running-code-before-each-view"></a>Ausführen von Code vor jeder Ansicht

Wenn es Code gibt, der vor jeder Ansicht ausgeführt werden muss, sollte dieser in die `_ViewStart.cshtml`-Datei platziert werden. Per Konvention befindet sich die `_ViewStart.cshtml`-Datei im `Views`-Ordner. Die in `_ViewStart.cshtml` aufgelisteten Anweisungen werden vor jeder vollständigen Ansicht (also keine Layouts und keine Teilansichten) ausgeführt. `_ViewStart.cshtml` ist genauso wie [ViewImports.cshtml](xref:mvc/views/layout#viewimports) hierarchisch. Wenn eine `_ViewStart.cshtml`-Datei im Ansichtsordner, der mit einem Controller verknüpft ist, definiert wird, wird sie nach derjenigen ausgeführt, die im Stamm des `Views`-Ordners definiert wurde (falls vorhanden).

Eine `_ViewStart.cshtml`-Beispieldatei:

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]

Die oben stehende Datei gibt an, dass alle Ansichten das `_Layout.cshtml`-Layout verwenden.

> [!NOTE]
> Weder `_ViewStart.cshtml` noch `_ViewImports.cshtml` befinden sich normalerweise im `/Views/Shared`-Ordner. Die Versionen dieser Dateien auf Anwendungsebene sollten direkt in den `/Views`-Ordner platziert werden.
