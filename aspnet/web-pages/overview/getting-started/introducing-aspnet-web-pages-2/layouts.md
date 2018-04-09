---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
title: Einführung in ASP.NET Web Pages – erstellen ein konsistentes Layout | Microsoft Docs
author: tfitzmac
description: Dieses Lernprogramm veranschaulicht die Layouts verwenden, um ein konsistentes Erscheinungsbild für die Seiten auf einer Website zu erstellen, die ASP.NET Web Pages verwendet. Es wird vorausgesetzt, Sie haben die...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: c85ec591-f8d7-4882-b763-de6ab9f3df7a
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
msc.type: authoredcontent
ms.openlocfilehash: c2d5c4d8ed8a71979c16d484ab90d283a45de537
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="introducing-aspnet-web-pages---creating-a-consistent-layout"></a>Einführung in ASP.NET Web Pages - erstellen ein konsistentes Layout
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> Dieses Lernprogramm veranschaulicht das verwenden *Layouts* ein konsistentes Erscheinungsbild für die Seiten auf einer Website zu erstellen, die ASP.NET Web Pages verwendet. Es wird vorausgesetzt, Sie haben die Reihe über abgeschlossen [Löschen von Datenbankdaten in ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251584).
> 
> Lernen Sie:
> 
> - Welche eine Layoutseite ist.
> - Wie Sie Layoutseiten mit dynamischen Inhalten zu kombinieren.
> - Zum Übergeben von Werten an einer Layoutseite.


## <a name="about-layouts"></a>Informationen zu Layouts

Die Seiten, die Sie bisher erstellt haben alle wurden abgeschlossen ist, eigenständige Seiten. Sie gehören alle für denselben Standort, sie jedoch keine gemeinsame Elemente oder ein standard aussehen.

Die meisten Standorte haben ein konsistentes Erscheinungsbild und das Layout. Angenommen, Sie zum Wechseln der [Microsoft.com](https://www.microsoft.com/web/) Standort- und sich mit der Registerkarte, sehen Sie, dass alle Seiten eines allgemeinen Layouts und eine visuelle Design entsprechen:

![Zeigt das Layout der Header, Navigationsbereich, im Inhaltsbereich und die Fußzeile Microsoft.com-Website](layouts/_static/image1.png)

Ein *ineffizient* Erstellen dieses Layouts wäre, einen Header, die Navigationsleiste und die Fußzeile auf allen Seiten separat definieren. Werden, würden Sie das gleiche Markup duplizieren. Wenn Sie etwas ändern möchten (z. B. aktualisieren die Fußzeile), müssten Sie jede Seite separat zu ändern.

Wo ist *Layoutseiten* in stammen. In ASP.NET Web Pages können Sie eine Layoutseite definieren, die einen allgemeinen Container für Seiten auf Ihrer Website bereitstellt. Die Seite "Layout" kann z. B. Header, Navigationsbereich und Fußzeile enthalten. Die Seite "Layout" enthält einen Platzhalter, wohin der Hauptinhalt gehen.

Sie können dann einzelne Inhaltsseiten definieren, die das Markup und den Code nur auf dieser Seite enthalten. Inhaltsseiten keine vollständige HTML-Seiten werden; Sie müssen noch nicht einmal haben eine `<body>` Element. Sie haben außerdem eine Codezeile, die ASP.NET mitteilt, welche Layoutseite den Inhalt angezeigt werden soll. Hier ist ein Bild, das etwa zeigt die Funktionsweise dieser Beziehung:

![Konzeptionelles Diagramm zeigt zwei Inhaltsseiten und einer Layoutseite, in der sie passen](layouts/_static/image2.png)

Diese Aktivität ist einfach, zu verstehen, wenn Sie in Aktion zu sehen. In diesem Lernprogramm ändern Sie Ihre Seiten Filme, um ein Layout zu verwenden.

## <a name="adding-a-layout-page"></a>Hinzufügen einer Layoutseite

Beginnen Sie durch das Erstellen einer Layoutseite, die eine typische Seitenlayout mit einem Header, Footer und einen Bereich für den Hauptinhalt definiert. Fügen Sie am Standort WebPagesMovies eine CSHTML-Seite, die mit dem Namen  *\_Layout.cshtml*.

Der führende Unterstrich ( `_` ) Zeichen ist von Bedeutung. Wenn eine Seite-Name mit einem Unterstrich beginnt, wird nicht ASP.NET diese Seite nicht direkt an den Browser gesendet. Diese Konvention können Sie Seiten zu definieren, die für Ihre Website erforderlich sind, aber diese Benutzer direkt anfordern werden sollten.

Der Inhalt auf der Seite durch Folgendes ersetzt:

[!code-html[Main](layouts/samples/sample1.html)]

Wie Sie sehen können, ist dieses Markup nur HTML-Code, der verwendet `<div>` Elemente so definieren Sie drei Abschnitte in der Seite plus eins mehr `<div>` Element, das drei Abschnitte enthalten. Die Fußzeile der enthält einem gewissen Grad Razor-Code: `@DateTime.Now.Year`, dem wird das aktuelle Jahr an dieser Stelle auf der Seite gerendert.

Beachten Sie, dass es ein Link zu einem Stylesheet, mit dem Namen *Movies.css*. Das Stylesheet ist, in dem die Details des physischen Layouts der Elemente definiert werden. Sie erstellen, die in wenigen Augenblicken.

Die einzigen ungewöhnlichen-Funktion in dieser  *\_Layout.cshtml* Seite ist die `@Render.Body()` Zeile. Dies ist der Platzhalter, wo der Inhalt für den dieses Layout mit einer anderen Seite zusammengeführt wird.

## <a name="adding-a-css-file"></a>Hinzufügen einer CSS-Datei

Die bevorzugte Methode zum Definieren der tatsächlichen Anordnung (d. h. Darstellung) von Elementen auf der Seite ist die Verwendung von cascading Stylesheet (CSS)-Regeln. So Sie erstellen eine *CSS* Datei, die die Regeln für das neue Layout verfügt.

Wählen Sie in WebMatrix Stammverzeichnis der Website ein. Klicken Sie dann in der **Dateien** Registerkarte des Menübands, klicken Sie auf den Pfeil unter der **neu** Schaltfläche, und klicken Sie dann auf **neuer Ordner**.

![Die Option "Neuer Ordner" unter neu im Menüband.](layouts/_static/image3.png)

Nennen Sie diesen Ordner *Stile*.

![Benennen den neuen Ordner "Formatvorlagen"](layouts/_static/image4.png)

In der neuen *Stile* Ordner, erstellen Sie eine Datei mit dem Namen *Movies.css*.

![Erstellen einer neuen Movies.css-Datei](layouts/_static/image5.png)

Ersetzen Sie den Inhalt der neuen *CSS* Datei durch Folgendes:

[!code-css[Main](layouts/samples/sample2.css)]

Es wird nicht viel über diese CSS-Regeln ausgenommen, beachten zwei Dinge angenommen. Eine ist, dass zusätzlich zum Festlegen von Schriftarten und Größen, die Regeln absolute Positionierung, verwenden um den Speicherort der Header, Footer und Hauptinhaltsbereich einzurichten. Wenn Sie auf die Positionierung in CSS vertraut sind, erfahren Sie die [CSS-Positionierung](http://www.w3schools.com/css/css_positioning.asp) am Standort W3Schools Tutorial.

Die andere Aufgabe zu beachten ist, dass am unteren finden wir die Stilregeln kopiert haben, die ursprünglich definiert einzeln in die *Movies.cshtml* Datei. Diese Regeln verwendet wurden, der [Einführung zum Anzeigen von Daten durch Verwenden von ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251580) Lernprogramm aus, um stellen die `WebGrid` Helper Rendern von Markup, das der Tabelle verteilt hinzugefügt. (Wenn Sie im Begriff sind, verwenden Sie eine *CSS* Datei für Formaten, können Sie auch Formatierungsregeln für die gesamte Website darin einfügen.)

## <a name="updating-the-movies-file-to-use-the-layout"></a>Aktualisieren die Filme-Datei, um das Layout verwenden

Jetzt können Sie die vorhandenen Dateien an Ihrem Standort verwenden Sie das neue Layout aktualisieren. Öffnen der *Movies.cshtml* Datei. Im oberen Bereich als erste Zeile des Codes, fügen Sie Folgendes hinzu:

[!code-csharp[Main](layouts/samples/sample3.cs)]

Die Seite beginnt jetzt stützen sich auf:

[!code-cshtml[Main](layouts/samples/sample4.cshtml?highlight=2)]

Diese eine Codezeile Weise wird ASP.NET informiert, die bei der *Filme* Seite ausgeführt wird, sollte er mit zusammengeführt werden die  *\_Layout.cshtml* Datei.

Da die *Movies.cshtml* Datei jetzt eine Layoutseite verwendet, können Sie das Markup Entfernen der *Movies.cshtml* Seite, die kümmert hat die  *\_Layout.cshtml*Datei. Entnehmen Sie der `<!DOCTYPE>`, `<html>`, und `<body>` Start- und Endtags. Entnehmen Sie den gesamten `<head>` Element und dessen Inhalt, die Formatierungsregeln für das Raster enthält, da Sie nun diese Regeln haben einer *CSS* Datei. Während Sie sich befinden, ändern Sie den vorhandenen `<h1>` Element ein `<h2>` Element; Sie müssen ein `<h1>` Element im Layoutseite bereits. Ändern der `<h2>` Text "Liste Filme".

Normalerweise würde nicht müssen Sie diese Arten von Änderungen in einer Inhaltsseite zu machen. Wenn Sie Ihre Website mit einer Layoutseite fangen, erstellen Sie zunächst Inhaltsseiten ohne all diese Elemente. In diesem Fall sind jedoch eine Seite "eigenständige" in eine konvertiert, die ein Layout verwendet werden, es gibt also ein Teil der Bereinigung.

Wenn Sie fertig sind, sind die *Movies.cshtml* Seite wird wie folgt aussehen:

[!code-cshtml[Main](layouts/samples/sample5.cshtml)]

### <a name="testing-the-layout"></a>Testen das Layout

Jetzt können Sie sehen, wie das Layout aussieht. WebMatrix, mit der Maustaste die *Movies.cshtml* Seite und wählen Sie **in Browser starten**. Wenn der Browser die Seite angezeigt wird, sieht er auf dieser Seite:

![Filme Seite gerendert wird, verwenden ein layout](layouts/_static/image6.png)

ASP.NET zusammengeführt hat den Inhalt der Seite "Movies.cshtml" in der  *\_Layout.cshtml* Seite rechts, wobei die `RenderBody` Methode ist. Und natürlich die  *\_Layout.cshtml* Seite Verweise ein *CSS* Datei, die das Aussehen der Seite definiert.

## <a name="updating-the-addmovie-page-to-use-the-layout"></a>Aktualisieren die AddMovie-Seite, um das Layout verwenden

Der echte Vorteil des Layouts ist, dass Sie sie für alle Seiten auf Ihrer Website verwenden können. Öffnen der *AddMovie.cshtml* Seite.

Sie möglicherweise berücksichtigen, dass die *AddMovie.cshtml* Seite hatte ursprünglich einige CSS-Regeln, die das Aussehen von Überprüfungsfehlermeldungen definieren. Da Sie haben eine *CSS* -Datei für Ihre Website jetzt, Sie können diese Regeln zum Verschieben der *CSS* Datei. Entfernen Sie sie aus der *AddMovie.cshtml* Datei, und fügen Sie sie am unteren Rand der *Movies.css* Datei. Verschieben Sie die folgenden Regeln:

[!code-css[Main](layouts/samples/sample6.css)]

Jetzt stellen die gleichen Arten von Änderungen in *AddMovie.cshtml* , die Sie für *Movies.cshtml* – hinzufügen `Layout="~/_Layout.cshtml;` , und entfernen Sie das HTML-Markup, das nun irrelevante ist. Ändern Sie das `<h1>`-Element in `<h2>`. Wenn Sie fertig sind, wird die Seite wie im folgenden Beispiel aussehen:

[!code-cshtml[Main](layouts/samples/sample7.cshtml)]

Führen Sie die Seite. Jetzt sieht es wie in dieser Abbildung aus:

!['Add Filme' Seite gerendert wird, verwenden ein layout](layouts/_static/image7.png)

Die Seiten der Website ähnliche Änderungen vornehmen möchten – *EditMovie.cshtml* und *DeleteMovie.cshtml*. Jedoch, bevor Sie dies tun, können Sie eine andere Änderung des Layouts vornehmen, die etwas flexibler vereinfacht.

## <a name="passing-title-information-to-the-layout-page"></a>Übergeben von Informationen zu Softwaretiteln auf der Seite "Layout"

Die  *\_Layout.cshtml* Seite, die Sie erstellt hat eine `<title>` -Element, das auf "Mein Filmwebsite" festgelegt ist. Die meisten Browser zeigt den Inhalt dieses Elements als Text auf einer Registerkarte:

![Der Seite &lt;Titel&gt; auf einer Browserregisterkarte angezeigten Elemente](layouts/_static/image8.png)

Diese Informationen zu Softwaretiteln ist generisch. Nehmen Sie an, dass das Titeltext der aktuellen Seite werden soll. (Der Titeltext wird auch von Suchmaschinen verwendet um zu bestimmen, was über die Seite ist.) Sie können Informationen aus einer Inhaltsseite wie übergeben *Movies.cshtml* oder *AddMovie.cshtml* auf der Seite "Layout" und dann diese Informationen, um anpassen, welche Layoutseite rendert.

Öffnen der *Movies.cshtml* Seite erneut. Fügen Sie im Code oben die folgende Zeile hinzu:

[!code-csharp[Main](layouts/samples/sample8.cs)]

Die `Page` Objekt steht auf allen *cshtml* Seiten und ist für diesen Zweck, d. h. Informationen zwischen einer Seite und das zugehörige Layout gemeinsam verwendet werden.

Öffnen der<em>\_Layout.cshtml</em> Seite. Ändern der `<title>` Element so, dass die It wie dieses Markup hinzu:

[!code-html[Main](layouts/samples/sample9.html)]

Dieser Code rendert den Inhalt der `Page.Title` Eigenschaft direkt an dieser Position auf der Seite.

Führen Sie die *Movies.cshtml* Seite. Dieses Mal die Browserregisterkarte zeigt, was Sie als Wert übergeben `Page.Title`:

![Eine Registerkarte des Webbrowsers, mit dem Titel dynamisch erstellt](layouts/_static/image9.png)

Wenn Sie möchten, zeigen Sie die Seitenquelle im Browser. Sie sehen, dass die `<title>` Element wird als gerendert `<title>List Movies</title>`.

> [!TIP] 
> 
> **Das Page-Objekt**
> 
> Eine hilfreiche Funktion von `Page` ist, dass es ein dynamisches Objekt – die `Title` Eigenschaft ist nicht mit einem festen oder reservierten Namen. Können Sie *alle* Name für den Wert der `Page` Objekt. Angenommen, Sie können genauso einfach haben übergeben den Titel mithilfe einer Eigenschaft mit dem Namen `Page.CurrentName` oder `Page.MyPage`. Die einzige Einschränkung besteht darin, dass der Name muss den normalen Regeln für welche Eigenschaften genannt werden können. (Z. B. kann nicht der Name enthalten ein Leerzeichen.)
> 
> Sie können eine beliebige Anzahl von Werten übergeben, indem die `Page` Objekt. Wenn Sie Informationen auf der Seite "Layout" übergeben, konnte Sie Werte übergeben, indem Sie etwa `Page.MovieTitle` und `Page.Genre` und `Page.MovieYear`. (Oder eine beliebige andere Namen, die Sie entwickelt, um die Informationen zu speichern.) Die einzige Anforderung – Dies ist wahrscheinlich offensichtliche – besteht darin, dass Sie die gleichen Namen in der Seite Inhalt und die Seite "Layout" verwenden.
> 
> Die Informationen, die Sie, indem übergeben die `Page` Objekt ist nicht beschränkt auf nur-Text auf der Seite "Layout" angezeigt werden sollen. Übergeben Sie den Wert auf der Seite "Layout", und klicken Sie dann Code in die Seite "Layout" kann den Wert entscheiden, ob Sie einen Abschnitt der Seite angezeigt wie *CSS* Datei verwenden und so weiter. Die übergebenen Werte in der `Page` Objekt werden wie alle anderen Werte Verwendung im Code. Es ist nur die Werte in der Inhaltsseite stammen und an die Seite "Layout" übergeben werden.


Öffnen der *AddMovie.cshtml* Seite, und fügen Sie eine Zeile an den Anfang der Code, der einen Titel für die *AddMovie.cshtml* Seite:

[!code-csharp[Main](layouts/samples/sample10.cs)]

Führen Sie die *AddMovie.cshtml* Seite. Sie finden Sie in den neuen Titel vorhanden:

![Eine Registerkarte des Webbrowsers, dynamisch erstellten Titel "Filme hinzufügen" angezeigt](layouts/_static/image10.png)

## <a name="updating-the-remaining-pages-to-use-the-layout"></a>Aktualisieren die verbleibenden Seiten, um das Layout zu verwenden.

Jetzt können Sie die verbleibenden Seiten an Ihrem Standort abschließen, sodass sie das neue Layout verwenden. Open *EditMovie.cshtml* und *DeleteMovie.cshtml* wiederum, und nehmen Sie die gleichen Änderungen in den einzelnen.

Fügen Sie die Codezeile, die links auf der Seite "Layout" hinzu:

[!code-csharp[Main](layouts/samples/sample11.cs)]

Hinzufügen einer Zeile, um den Titel der Seite festlegen:

[!code-csharp[Main](layouts/samples/sample12.cs)]

oder:

[!code-csharp[Main](layouts/samples/sample13.cs)]

Entfernen Sie das überflüssige HTML-Markup – im Grunde, lassen Sie nur die Bits, die innerhalb der `<body>` -Element (plus der Codeblock oben).

Ändern der `<h1>` Element ist ein `<h2>` Element.

Wenn Sie diese Änderungen vorgenommen haben, testen Sie, und stellen Sie sicher, dass er ordnungsgemäß angezeigt wird und dass der Titel richtig ist.

## <a name="parting-thoughts-about-layout-pages"></a>Trennflächen Gedanken zu Layoutseiten

In diesem Lernprogramm erstellt Sie eine  *\_Layout.cshtml* Seite und verwendet die `RenderBody` Methode, um Inhalt aus einer anderen Seite zusammenzuführen. Dies ist das grundlegende Muster für die Verwendung von Layouts in Webseiten.

Layoutseiten haben zusätzliche Funktionen, die hier nicht beschrieben. Sie können z. B. Layoutseiten schachteln – eine andere wiederum eine Layoutseite verweisen kann. Geschachtelte Layouts ist hilfreich, wenn Sie Unterabschnitte eines Standorts verwenden, die unterschiedliche Layouts erfordern. Sie können auch zusätzliche Methoden (z. B. `RenderSection`) einrichten, mit dem Namen Abschnitte in der Seite "Layout".

Die Kombination von Layoutseiten und *CSS* Dateien ist leistungsstark. Wie Sie in den nächsten Lernprogramm sehen werden, in WebMatrix können erstellen Sie eine Website auf Grundlage einer *Vorlage*, erhalten Sie eine Site mit vorgefertigten Funktionen darin. Die Vorlagen, Layoutseiten und CSS zum Erstellen von Websites, die großartige und deren Funktionen wie Menüs, verwendet. Hier ist ein Screenshot der Homepage von einer Website auf Basis einer Vorlage, die mit Funktionen, die Layoutseiten und CSS verwenden:

![Layout von WebMatrix-Websitevorlage, die mit den Header, Navigationsbereich, Inhaltsbereichs, optionalen Abschnitt und Anmeldung Links erstellt](layouts/_static/image11.png)

## <a name="complete-listing-for-movie-page-updated-to-use-a-layout-page"></a>Vollständige Auflistung für Filmdetailseite (die Verwendung eine Layoutseite aktualisiert werden)

[!code-cshtml[Main](layouts/samples/sample14.cshtml)]

## <a name="complete-page-listing-for-add-movie-page-updated-for-layout"></a>Auflisten der für die Seite abgeschlossen hinzufügen Filmdetailseite (aktualisiert für Layout)

[!code-cshtml[Main](layouts/samples/sample15.cshtml)]

## <a name="complete-page-listing-for-delete-movie-page-updated-for-layout"></a>Auf der Seite abgeschlossen Listeneintrag löschen Filmdetailseite (aktualisiert für Layout)

[!code-cshtml[Main](layouts/samples/sample16.cshtml)]

## <a name="complete-page-listing-for-edit-movie-page-updated-for-layout"></a>Auf der Seite abgeschlossen Listeneintrag bearbeiten (Seite) Film (aktualisiert für Layout)

[!code-cshtml[Main](layouts/samples/sample17.cshtml)]

## <a name="coming-up-next"></a>Als Nächstes kommen

In den nächsten Lernprogrammen erfahren Sie, wie Sie Ihre Website mit dem Internet zu veröffentlichen, sodass "Jeder", die sie anzeigen kann.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Erstellen ein konsistentes Aussehen](https://go.microsoft.com/fwlink/?LinkID=202891) – ein Artikel, die einige weitere Details zum Arbeiten mit Layouts bereitstellt. Außerdem werden wie einen Wert an einer Layoutseite übergeben, das Anzeigen oder ausblenden Inhalte beschrieben.
- [Geschachtelte Layoutseiten mit Razor](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) – Mike Brind Blogs ein Beispiel zum Layoutseiten schachteln. (Schließt einen Download der Seiten).

> [!div class="step-by-step"]
> [Zurück](deleting-data.md)
> [Weiter](publishing.md)
