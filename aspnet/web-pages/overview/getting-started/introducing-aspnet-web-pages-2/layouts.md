---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
title: Einführung in ASP.NET Web Pages - Erstellen eines konsistenten Layouts | Microsoft-Dokumentation
author: tfitzmac
description: In diesem Tutorial erfahren Sie, wie Sie die Layouts zu verwenden, um ein einheitliches Aussehen für die Seiten auf einer Website zu erstellen, die ASP.NET Web Pages verwendet. Es wird vorausgesetzt, Sie haben die...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: c85ec591-f8d7-4882-b763-de6ab9f3df7a
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
msc.type: authoredcontent
ms.openlocfilehash: 3368a3e3c9dc56b98ca0adddf4ffb181c7b34863
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37379446"
---
<a name="introducing-aspnet-web-pages---creating-a-consistent-layout"></a>Einführung in ASP.NET Web Pages - Erstellen eines konsistenten Layouts
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> In diesem Tutorial erfahren Sie, wie Sie mit *Layouts* ein einheitliches Aussehen für die Seiten auf einer Website zu erstellen, die ASP.NET Web Pages verwendet. Es wird vorausgesetzt, Sie haben die Reihe über [Löschen von Daten in ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251584).
> 
> Sie lernen Folgendes:
> 
> - Welche eine Layoutseite ist.
> - Wie Sie Layoutseiten mit dynamischen Inhalten zu kombinieren.
> - Informationen zum Übergeben von Werten an eine Layoutseite.


## <a name="about-layouts"></a>Informationen zu Layouts

Die Seiten, die Sie bisher erstellt haben alle wurden abgeschlossen ist, eigenständige Seiten. Alle am selben Standort gehören, jedoch sind keine allgemeine Elemente oder ein standard überprüfen.

Die meisten Sites haben ein einheitliches Aussehen und Layout. Angenommen, Sie zum Wechseln der [Microsoft.com/Web](https://www.microsoft.com/web/) Standort- und zu suchen, sehen Sie, dass alle Seiten, um ein Layout für die allgemeine und eine visual Studio-Designs entsprechen:

![Microsoft.com/Web-Webseite, die mit dem Layout des der Header, Navigation, Inhaltsbereich und Fußzeile](layouts/_static/image1.png)

Ein *ineffizient* Möglichkeit zum Erstellen dieses Layout wäre ein Header, Navigationsleiste und Fußzeile separat für jede definieren. Werden, würden Sie das gleiche Markup duplizieren. Wenn Sie Änderungen vornehmen möchten (z. B. aktualisieren die Fußzeile), müssen Sie jede Seite separat zu ändern.

D. h. wo *Layoutseiten* eingehen. In ASP.NET Web Pages, können Sie eine Layoutseite definieren, die einen allgemeinen Container für Seiten auf Ihrer Website bereitstellt. Auf der Layoutseite kann z. B. die Header, Navigation und Fußzeile enthalten. Auf der Layoutseite enthält einen Platzhalter Speicherorts für der Hauptinhalt.

Sie können dann die einzelnen Inhaltsseiten definieren, die das Markup und Code nur auf dieser Seite enthalten. Vollständige HTML-Seiten werden müssen Inhaltsseiten keine; Sie müssen auch keine haben eine `<body>` Element. Sie haben außerdem eine einzige Zeile Code, die ASP.NET mitteilt, welche Layoutseite den Inhalt angezeigt werden soll. Hier ist ein Bild, das ungefähr veranschaulicht die Funktionsweise von dieser Beziehung:

![Konzeptionelle Diagramm zeigt zwei Inhaltsseiten und einer Layoutseite aus, in der sie passen](layouts/_static/image2.png)

Diese Interaktion ist einfach zu verstehen, wenn Sie es in Aktion zu sehen. In diesem Tutorial ändern Sie Filme Seiten, um ein Layout verwenden.

## <a name="adding-a-layout-page"></a>Hinzufügen einer Layoutseite

Sie beginnen, erstellen Sie eine Layoutseite, die ein typisches Seitenlayout mit einem Header, Footer und einen Bereich für den Hauptinhalt definiert. Fügen Sie am Standort WebPagesMovies eine CSHTML-Seite namens  *\_Layout.cshtml*.

Der führende Unterstrich ( `_` ) Zeichen ist von Bedeutung. Wenn eine Seite-Name mit einem Unterstrich beginnt, wird nicht ASP.NET diese Seite direkt an den Browser senden. Diese Konvention können Sie die Seiten zu definieren, die für Ihre Website erforderlich sind, aber Benutzer direkt anfordern können sollte nicht.

Ersetzen Sie den Inhalt auf der Seite Folgendes ein:

[!code-html[Main](layouts/samples/sample1.html)]

Wie Sie sehen können, handelt es sich bei diesem Markup ist ausschließlich aus HTML bestehen, die verwendet `<div>` Elemente zum Definieren von drei Abschnitte in der Seite plus 1 mehr `<div>` Element, das drei Abschnitte enthalten soll. Die Fußzeile enthält Razor-Code ein wenig: `@DateTime.Now.Year`, rendert die an dieser Stelle auf der Seite des aktuellen Jahrs.

Beachten Sie, dass es ein Link zu einem Stylesheet, mit dem Namen *Movies.css*. Das Stylesheet ist, in dem die Details des physischen Layouts der Elemente definiert werden. Sie erstellen, die in Kürze.

Die einzigen ungewöhnlichen-Funktion in diesem  *\_Layout.cshtml* Seite ist die `@Render.Body()` Zeile. Dies ist der Platzhalter, wo der Inhalt für den dieses Layout mit einer anderen Seite zusammengeführt wird.

## <a name="adding-a-css-file"></a>Hinzufügen einer CSS-Datei

Die bevorzugte Methode zum Definieren der tatsächlichen Anordnung (d. h. Darstellung) von Elementen auf der Seite ist die Verwendung von cascading Stylesheet (CSS)-Regeln. So Sie erstellen eine *CSS* -Datei mit den Regeln für das neue Layout.

Wählen Sie den Stamm Ihrer Website in WebMatrix. Klicken Sie dann in der **Dateien** Registerkarte des Menübands, klicken Sie auf den Pfeil unter der **neu** Schaltfläche, und klicken Sie dann auf **neuer Ordner**.

![Die Option "Neuer Ordner" unter neu auf dem Menüband.](layouts/_static/image3.png)

Nennen Sie diesen Ordner *Stile*.

![Benennen den neuen Ordner "Formatvorlagen"](layouts/_static/image4.png)

In der neuen *Stile* Ordner eine Datei namens *Movies.css*.

![Erstellen einer neuen Movies.css-Datei](layouts/_static/image5.png)

Ersetzen Sie den Inhalt der neuen *CSS* Datei durch Folgendes:

[!code-css[Main](layouts/samples/sample2.css)]

Es wird nicht viel über diese CSS-Regeln, mit Ausnahme von zwei Dinge beachten angenommen. Ist, dass zusätzlich zum Festlegen von Schriftarten und Größen können die Regeln absolute Positionierung, verwenden um den Speicherort der Kopfzeile, Fußzeile und Hauptinhaltsbereich einzurichten. Wenn Sie neu positionieren in CSS ist, können Sie lesen die [CSS-Positionierung](http://www.w3schools.com/css/css_positioning.asp) am Standort W3Schools Tutorial.

Das andere, was zu beachten ist, dass Sie unten auf die wir Stilregeln kopiert haben, die ursprünglich definiert einzeln in die *Movies.cshtml* Datei. Diese Regeln wurden in verwendet die [Einführung in die Anzeige von Daten durch Verwenden von ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251580) beschrieben wird, stellen die `WebGrid` Helper Rendern von Markup, das der Tabelle verteilt hinzugefügt. (Wenn Sie vorhaben, verwenden Sie eine *CSS* Datei Style-Definitionen, können Sie auch die Stilregeln für die gesamte Website darin einfügen.)

## <a name="updating-the-movies-file-to-use-the-layout"></a>Aktualisieren der Filme-Datei, um das Layout zu verwenden.

Jetzt können Sie die vorhandenen Dateien an Ihrem Standort verwenden Sie das neue Layout aktualisieren. Öffnen der *Movies.cshtml* Datei. Im oberen Bereich als erste Zeile des Codes, fügen Sie Folgendes hinzu:

[!code-csharp[Main](layouts/samples/sample3.cs)]

Die Seite beginnt jetzt auf diese Weise:

[!code-cshtml[Main](layouts/samples/sample4.cshtml?highlight=2)]

Diese eine Codezeile weist ASP.NET, die bei der *Filme* Seite ausgeführt wird, die sie die zusammengeführt werden sollen, mit der  *\_Layout.cshtml* Datei.

Da die *Movies.cshtml* -Datei verwendet jetzt eine Layoutseite verwenden, können Sie das Markup aus Entfernen der *Movies.cshtml* Seite, die vom übernommen hat die  *\_Layout.cshtml*Datei. Nehmen Sie sich die `<!DOCTYPE>`, `<html>`, und `<body>` Start- und Endtags. Nutzen Sie die gesamte `<head>` -Element und dessen Inhalt, enthält die Stilregeln für das Raster, da Sie jetzt diese Regeln haben einem *CSS* Datei. Während Sie schon dabei sind, ändern Sie die vorhandene `<h1>` Element eine `<h2>` Element; Sie haben eine `<h1>` Element bereits der Layoutseite. Ändern der `<h2>` Text, der "Liste Filme".

Sie müssen normalerweise wäre nicht diese Arten von Änderungen in einer Inhaltsseite zu machen. Wenn Sie Ihre Website mit einer Layoutseite beginnen, erstellen Sie zunächst Inhaltsseiten ohne all diese Elemente. In diesem Fall sind jedoch eine eigenständigen Seite in einem konvertiert, die ein Layout verwendet wird, gibt es also praktisch zur Bereinigung.

Wenn Sie fertig sind, die *Movies.cshtml* Seite wird wie folgt aussehen:

[!code-cshtml[Main](layouts/samples/sample5.cshtml)]

### <a name="testing-the-layout"></a>Testen das Layout

Jetzt können Sie sehen, wie das Layout aussieht. WebMatrix, mit der Maustaste der *Movies.cshtml* Seite und wählen Sie **in Browser starten**. Wenn der Browser die Seite angezeigt wird, sieht es auf dieser Seite:

![Seite "Movies" mit einem Layout gerendert](layouts/_static/image6.png)

ASP.NET hat den Inhalt der Seite Movies.cshtml in zusammengeführt der  *\_Layout.cshtml* Seite rechts, wobei die `RenderBody` Methode ist. Und natürlich die  *\_Layout.cshtml* Verweise Seite eine *CSS* Datei, die die Darstellung der Seite definiert.

## <a name="updating-the-addmovie-page-to-use-the-layout"></a>Zum Aktualisieren der AddMovie-Seite, um das Layout zu verwenden.

Die eigentlichen Vorteile der Layouts ist, dass Sie sie für alle Seiten auf Ihrer Website verwenden können. Öffnen der *AddMovie.cshtml* Seite.

Erinnern Sie sich, die die *AddMovie.cshtml* Seite hatten ursprünglich eine CSS-Regeln, um das Aussehen von Überprüfungsfehlermeldungen definieren. Da Sie haben eine *CSS* -Datei für Ihre Website jetzt, Sie können diese Regeln zum Verschieben der *CSS* Datei. Entfernen Sie sie aus der *AddMovie.cshtml* Datei, und fügen sie am unteren Rand der *Movies.css* Datei. Verschieben Sie die folgenden Regeln:

[!code-css[Main](layouts/samples/sample6.css)]

Stellen Sie jetzt die gleichen Arten von Änderungen in *AddMovie.cshtml* , die Sie für *Movies.cshtml* – hinzufügen `Layout="~/_Layout.cshtml;` und entfernen Sie das HTML-Markup, das nun überflüssige ist. Ändern Sie das `<h1>`-Element in `<h2>`. Wenn Sie fertig sind, wird die Seite wie im folgenden Beispiel aussehen:

[!code-cshtml[Main](layouts/samples/sample7.cshtml)]

Führen Sie die Seite. Sie sieht jetzt wie in dieser Abbildung aus:

!["Add Filme" Seiten mit einem layout](layouts/_static/image7.png)

Sie ähnliche Änderungen an den Seiten der Website vornehmen möchten – *EditMovie.cshtml* und *DeleteMovie.cshtml*. Allerdings können Sie vor dem Ausführen des Layouts einer anderen Änderung vornehmen, die eine größere Flexibilität ermöglicht.

## <a name="passing-title-information-to-the-layout-page"></a>Informationen zu Softwaretiteln übergeben die Seite ' Layout '

Die  *\_Layout.cshtml* Seite, die Sie erstellt hat eine `<title>` -Element, das auf "Meine Movie-Website" festgelegt ist. Die meisten Browser anzeigen den Inhalt dieses Elements als Text auf einer Registerkarte:

![Die Seite &lt;Titel&gt; in einer Browserregisterkarte angezeigten Elemente](layouts/_static/image8.png)

Diese Informationen zu Softwaretiteln ist generisch. Nehmen wir an, dass das Titeltext sollen genauer auf die aktuelle Seite. (Der Titeltext wird auch von Suchmaschinen verwendet um zu bestimmen, was zu Ihrer Seite ist.) Sie können Informationen übergeben, von einer Inhaltsseite wie *Movies.cshtml* oder *AddMovie.cshtml* auf der Seite "Layout" und dann diese Informationen an die Layoutseite anpassen darstellt.

Öffnen der *Movies.cshtml* Seite erneut. Fügen Sie im Code oben wird die folgende Zeile hinzu:

[!code-csharp[Main](layouts/samples/sample8.cs)]

Die `Page` Objekt steht für alle *.cshtml* Seiten und ist für diesen Zweck, d. h. zum Freigeben von Informationen zwischen einer Seite und des Layouts.

Öffnen der<em>\_Layout.cshtml</em> Seite. Ändern der `<title>` Element so, dass die It wie dieses Markup hinzu:

[!code-html[Main](layouts/samples/sample9.html)]

Dieser Code rendert den Inhalt der `Page.Title` Eigenschaft direkt an dieser Stelle auf der Seite.

Führen Sie die *Movies.cshtml* Seite. Dieses Mal die Registerkarte "Browser" zeigt, was Sie als Wert des übergebenen `Page.Title`:

![Eine Browserregisterkarte mit dem Titel, dynamisch erstellt](layouts/_static/image9.png)

Sie können den Quellcode der Seite im Browser anzuzeigen. Sie sehen, dass die `<title>` als Element gerendert wird `<title>List Movies</title>`.

> [!TIP] 
> 
> **Das Page-Objekt**
> 
> Ein nützliches Feature von `Page` besteht darin, dass es sich um ein dynamisches Objekt ist – die `Title` Eigenschaft ist keine feste oder reservierte Namen. Können Sie *alle* für den Wert der `Page` Objekt. Beispielsweise Sie können so einfach haben übergeben den Titel mithilfe einer Eigenschaft mit dem Namen `Page.CurrentName` oder `Page.MyPage`. Die einzige Einschränkung besteht darin, dass der Name muss den normalen Regeln für welche Eigenschaften genannt werden können. (Z. B. kann nicht der Name enthalten ein Leerzeichen.)
> 
> Sie können eine beliebige Anzahl von Werten übergeben, mit der `Page` Objekt. Falls gewünscht, können Sie die Informationen auf der Seite "Layout" übergeben, können Sie Werte übergeben, indem Sie etwa `Page.MovieTitle` und `Page.Genre` und `Page.MovieYear`. (Oder jeder anderen Namen, die Sie entwickelt, um die Informationen zu speichern.) Die einzige Anforderung besteht, dies ist wahrscheinlich offensichtlich – besteht darin, dass Sie die gleichen Namen in der Seite Inhalt und die Seite "Layout" verwenden.
> 
> Die Informationen, die Sie, indem übergeben die `Page` Objekt ist nicht beschränkt auf nur Text, der auf der Layoutseite angezeigt. Sie können einen Wert übergeben, um die Seite "Layout", und klicken Sie dann Code auf der Layoutseite kann den Wert entscheiden, ob Sie einen Abschnitt der Seite angezeigt wie *CSS* -Datei, um verwenden und so weiter. Die Werte, die Sie übergeben die `Page` Objekt werden wie alle anderen Standardwerten Verwendung im Code. Es ist einfach, dass die Werte aus der Seite Inhalt stammen und die Seite ' Layout ' übergeben werden.


Öffnen der *AddMovie.cshtml* Seite, und fügen Sie eine Zeile am Anfang der Code, der einen Titel für die *AddMovie.cshtml* Seite:

[!code-csharp[Main](layouts/samples/sample10.cs)]

Führen Sie die *AddMovie.cshtml* Seite. Sie sehen den neuen Titel vorhanden:

![Eine Browserregisterkarte mit der 'Filme hinzufügen' Titel dynamisch erstellt](layouts/_static/image10.png)

## <a name="updating-the-remaining-pages-to-use-the-layout"></a>Aktualisieren die verbleibenden Seiten, um das Layout zu verwenden.

Jetzt können Sie die verbleibenden Seiten auf Ihrer Website abschließen, sodass sie das neue Layout verwenden. Open *EditMovie.cshtml* und *DeleteMovie.cshtml* wiederum, und stellen Sie die gleichen Änderungen in den einzelnen.

Fügen Sie die Zeile des Codes, der mit der Layoutseite verknüpft:

[!code-csharp[Main](layouts/samples/sample11.cs)]

Hinzufügen einer Zeile, um den Titel der Seite festzulegen:

[!code-csharp[Main](layouts/samples/sample12.cs)]

oder:

[!code-csharp[Main](layouts/samples/sample13.cs)]

Entfernen Sie das überflüssige HTML-Markup, lassen Sie nur die Bits, die in sind im Grunde die `<body>` -Element (plus den Codeblock am oberen Rand).

Ändern der `<h1>` Elemente werden eine `<h2>` Element.

Wenn Sie diese Änderungen vorgenommen haben, testen Sie jede, und stellen Sie sicher, dass es ordnungsgemäß angezeigt wird und dass der Titel richtig ist.

## <a name="parting-thoughts-about-layout-pages"></a>Trennflächen Ideen Layoutseiten

In diesem Tutorial, in dem Sie erstellt haben eine  *\_Layout.cshtml* Seite und verwendet die `RenderBody` Methode, um Inhalte von einer anderen Seite zusammengeführt. Dies ist das grundlegende Muster für die Verwendung von Layouts in Webseiten.

Layoutseiten haben zusätzliche Funktionen, die hier nicht behandelt. Sie können z. B. Layoutseiten schachteln – eine Layoutseite kann wiederum eine andere verweisen. Geschachtelte Layouts ist hilfreich, wenn Sie mit einer Website in den Unterabschnitten arbeiten, die unterschiedliche Layouts erfordern. Sie können auch zusätzliche Methoden verwenden (z. B. `RenderSection`) einrichten, mit dem Namen Abschnitte der Layoutseite.

Die Kombination der Layoutseiten, und *CSS* Dateien ist leistungsstark. Wie Sie in den nächsten Lernprogramm sehen werden, in WebMatrix Sie erstellen eine Website auf Grundlage einer *Vorlage*, bietet Ihnen einen Standort vorgefertigter Funktionalität darin. Die Vorlagen, Layoutseiten und CSS zum Erstellen von Websites, die toll Aussehen und deren Features wie Menüs, nutzen. Hier ist ein Screenshot der Startseite von einer Website auf Grundlage einer Vorlage, die mit Funktionen, die Layoutseiten und CSS verwenden:

![Layout von WebMatrix-Websitevorlage, zeigt der Header, im Navigationsbereich, Inhaltsbereich, Optionaler Abschnitt und Anmeldung Links erstellt](layouts/_static/image11.png)

## <a name="complete-listing-for-movie-page-updated-to-use-a-layout-page"></a>Vollständige Liste für Movie-Seite (wird aktualisiert, um eine Layoutseite verwenden)

[!code-cshtml[Main](layouts/samples/sample14.cshtml)]

## <a name="complete-page-listing-for-add-movie-page-updated-for-layout"></a>Auf der Seite abgeschlossen für hinzufügen Movie-Seite (für das Layout aktualisiert)

[!code-cshtml[Main](layouts/samples/sample15.cshtml)]

## <a name="complete-page-listing-for-delete-movie-page-updated-for-layout"></a>Auf der Seite abgeschlossen-Eintrag für den Film "löschen" (für das Layout aktualisiert)

[!code-cshtml[Main](layouts/samples/sample16.cshtml)]

## <a name="complete-page-listing-for-edit-movie-page-updated-for-layout"></a>Führen Sie die Liste der Seite für Seite-Film "Edit" (für das Layout aktualisiert)

[!code-cshtml[Main](layouts/samples/sample17.cshtml)]

## <a name="coming-up-next"></a>Als Nächstes kommen

Im nächsten Tutorial erfahren Sie, wie Sie Ihre Website mit dem Internet zu veröffentlichen, sodass jeder sie sehen kann.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Erstellen einer konsistenten aussehen](https://go.microsoft.com/fwlink/?LinkID=202891) – ein Artikel, die weitere Details zum Arbeiten mit Layouts bereitstellt. Es wird beschrieben, wie einen Wert an einer Layoutseite zu übergeben, die zeigt, oder Blendet einen Teil des Inhalts.
- [Geschachtelte Layoutseiten mit Razor](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) – Mike Brind Blogs verdeutlicht, wie Sie Layoutseiten schachteln. (Umfasst ein Download der Seiten.)

> [!div class="step-by-step"]
> [Zurück](deleting-data.md)
> [Weiter](publishing.md)
