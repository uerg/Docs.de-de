---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/showing-multiple-records-per-row-with-the-datalist-control-cs
title: Mit mehreren Datensätzen pro Zeile mit dem DataList-Steuerelement (c#) | Microsoft Docs
author: rick-anderson
description: In diesem kurzen Tutorial lernen wir zum Anpassen der DataList-Layout über seine Eigenschaften RepeatColumns und RepeatDirection kennen.
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: cf5acaf5-d4f6-4957-badc-b89956b285f3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/showing-multiple-records-per-row-with-the-datalist-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 308427836c1fef05e66d1f5348c6bd9e80290f9b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30891216"
---
<a name="showing-multiple-records-per-row-with-the-datalist-control-c"></a>Anzeigen von mehreren Datensätzen pro Zeile mit dem DataList-Steuerelement (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunterladen](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_31_CS.exe) oder [PDF herunterladen](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/datatutorial31cs1.pdf)

> In diesem kurzen Tutorial lernen wir zum Anpassen der DataList-Layout über seine Eigenschaften RepeatColumns und RepeatDirection kennen.


## <a name="introduction"></a>Einführung

In den Beispielen DataList wir gibt es in den letzten zwei Lernprogrammen gesehen haben jeder Datensatz aus der Datenquelle als eine Zeile in eine einspaltige HTML gerendert `<table>`. Obgleich dies das DataList-Standardverhalten ist, ist es sehr einfach, die DataList-Anzeige anzupassen, dass eine Tabelle mit mehreren Spalten, die mehrzeilige der Datenquellenelemente verteilt sind. Darüber hinaus führt es möglich, dass alle Daten s Datenquellenelemente, die in einer einzigen Zeile und mehrspaltigen DataList angezeigt.

Wir können über das DataList-s-Layout Anpassen der `RepeatColumns` und `RepeatDirection` Eigenschaften, die anzugeben, wie viele Spalten gerendert werden, und gibt an, ob diese Elemente vertikal oder horizontal angeordnet sind. Abbildung 1 zeigt z. B. eine DataList, in dem Produktinformationen in einer Tabelle mit drei Spalten angezeigt.


[![DataList zeigt drei Produkte pro Zeile](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image2.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image1.png)

**Abbildung 1**: das DataList zeigt drei Produkte pro Zeile ([klicken Sie hier, um das Bild in voller Größe angezeigt](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image3.png))


Bericht mehrere Datenquellenelemente pro Zeile enthält, kann das DataList effektiver horizontale Bildschirmbereich nutzen. In diesem kurzen Tutorial lernen wir diese zwei DataList-Eigenschaften kennen.

## <a name="step-1-displaying-product-information-in-a-datalist"></a>Schritt 1: Anzeigen von Produktinformationen in einem DataList

Bevor wir untersuchen das `RepeatColumns` und `RepeatDirection` Eigenschaften, Let s erstellen Sie zunächst DataList auf unserer Seite, die Produktinformationen, die mit dem Layout einspaltiges, mehrzeilige Standardtabelle auflistet. In diesem Beispiel können Sie s s Produktname, Kategorie und verwenden das folgende Markup Preis angezeigt:


[!code-html[Main](showing-multiple-records-per-row-with-the-datalist-control-cs/samples/sample1.html)]

Wir haben gesehen, wie Daten an DataList in vorherigen Beispielen binden, damit ich durch diese Schritte schnell verschieben müssen. Öffnen Sie zunächst die `RepeatColumnAndDirection.aspx` auf der Seite der `DataListRepeaterBasics` Ordner, und ziehen Sie eine DataList aus der Toolbox in den Designer. Abonnieren von aus dem Smarttag DataList s, um eine neue ObjectDataSource erstellen und konfigurieren, dass die Daten beziehen der `ProductsBLL` Klasse s `GetProducts` Methode auswählen (keine) aus dem Assistenten s INSERT-, Update-, aus, und Löschen von Registerkarten.

Nach dem Erstellen und binden die neue ObjectDataSource an DataList, Visual Studio erstellt automatisch eine `ItemTemplate` , die den Namen und Wert für die einzelnen Datenfelder Produkt anzeigt. Anpassen der `ItemTemplate` entweder direkt über das deklarative Markup oder aus den Vorlagen bearbeiten option in das DataList-s-Smarttag, sodass verwendet das Markup oben, und Ersetzen Sie dabei die *Produktname*, *Kategorienamen* , und *Preis* Text mit Bezeichnungsfeld-Steuerelementen, die die entsprechenden Databinding-Syntax verwenden, um Werte zugewiesen ihre `Text` Eigenschaften. Nach der Aktualisierung der `ItemTemplate`, Ihrer deklarativen Markup der Seite "s" sollte etwa wie folgt aussehen:


[!code-aspx[Main](showing-multiple-records-per-row-with-the-datalist-control-cs/samples/sample2.aspx)]

Beachten Sie, dass Ve enthalten einen Formatbezeichner in der `Eval` Databinding-Syntax für die `UnitPrice`, den zurückgegebenen Wert als Währung - Formatierung `Eval("UnitPrice", "{0:C}").`

Nehmen Sie einen Moment Zeit, um die Seite in einem Browser besuchen. Wie in Abbildung 2 gezeigt, rendert die DataList als einspaltiges, mehrzeilige Tabelle Produkte.


[![Standardmäßig wird das DataList-rendert als einspaltiges, mehrzeilige Tabelle](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image5.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image4.png)

**Abbildung 2**: standardmäßig, das DataList rendert als einzelne-Spalte, Tabelle mit mehreren Zeilen ([klicken Sie hier, um das Bild in voller Größe angezeigt](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image6.png))


## <a name="step-2-changing-the-datalist-s-layout-direction"></a>Schritt 2: Ändern der DataList-s-Layoutausrichtung

Während das Standardverhalten für die DataList besteht darin, dessen Elemente in einer Tabelle eine Spalte, die mehrzeilige vertikal anordnen dieses Verhalten kann leicht geändert werden über das DataList s [ `RepeatDirection` Eigenschaft](https://msdn.microsoft.com/system.web.ui.webcontrols.datalist.repeatdirection.aspx). Die `RepeatDirection` Eigenschaft kann einen von zwei möglichen Werten annehmen: `Horizontal` oder `Vertical` (Standard).

Durch Ändern der `RepeatDirection` Eigenschaft von `Vertical` zu `Horizontal`, DataList rendert der Datensätze in einer einzelnen Zeile eine Spalte pro Datenquellenelement erstellen. Um diesen Effekt zu veranschaulichen, klicken Sie auf das DataList im Designer, und ändern Sie dann im Fenster Eigenschaften die `RepeatDirection` Eigenschaft von `Vertical` auf `Horiztonal`. Sofort auf diese Weise Designer passt das Layout DataList s Erstellen einer einzigen Zeile und mehrspaltigen-Schnittstelle (siehe Abbildung 3).


[![Die RepeatDirection Eigenschaft bestimmt, wie die Richtung der DataList-s-Elemente werden angeordnet](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image8.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image7.png)

**Abbildung 3**: die `RepeatDirection` Eigenschaft bestimmt, wie die Richtung der DataList-s-Elemente angeordnet, sind ([klicken Sie hier, um das Bild in voller Größe angezeigt](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image9.png))


Kleine Mengen von Daten, die eine einzelne Zeile und Anzeigen von möglicherweise mit mehreren Spalten Tabelle eine ideale Möglichkeit, um die Bildschirmfläche zu maximieren. Für größere Datenmengen wird eine einzelne Zeile erfordern jedoch zahlreiche Spalten, welche Pushvorgänge diese Elemente, kann nicht auf dem Bildschirm ausschalten nach rechts passen. Abbildung 4 zeigt die Produkte aus, wenn in einer einzelnen Zeile DataList gerendert. Da viele Produkte (mehr als 80) vorhanden sind, müssen die Benutzer führen Sie einen Bildlauf ganz nach rechts, um Informationen zu Produkten anzuzeigen.


[![Für Datenquellen stets groß genug sein erfordern eine einzelne Spalte DataList horizontalen Bildlauf](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image11.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image10.png)

**Abbildung 4**: für ausreichend große Datenquellen, die eine einzelne Spalte DataList wird erfordern horizontalen Bildlauf ([klicken Sie hier, um das Bild in voller Größe angezeigt](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image12.png))


## <a name="step-3-displaying-data-in-a-multi-column-multi-row-table"></a>Schritt 3: Anzeigen von Daten in einer Tabelle mit mehreren Spalten, die mehrzeilige

Um einen mehrspaltigen, mehrzeilige DataList zu erstellen, müssen wir Festlegen der [ `RepeatColumns` Eigenschaft](https://msdn.microsoft.com/system.web.ui.webcontrols.datalist.repeatcolumns.aspx) auf die Anzahl der anzuzeigenden Spalten. Wird standardmäßig die `RepeatColumns` Eigenschaft ist auf 0 festgelegt, wodurch das DataList zum Anzeigen aller Elemente in einer einzelnen Zeile oder Spalte (abhängig vom Wert der `RepeatDirection` Eigenschaft).

In unserem Beispiel können Sie s, die drei Produkte pro Tabellenzeile angezeigt. Legen Sie deshalb die `RepeatColumns` -Eigenschaft auf 3. Nehmen Sie einen Moment Zeit, um die Ergebnisse in einem Browser anzuzeigen, nach dieser Änderung. Wie in Abbildung 5 gezeigt, werden die Produkte in einer Tabelle drei Spalten, die mehrzeilige jetzt aufgeführt.


[![Es werden drei Produkte pro Zeile angezeigt.](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image14.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image13.png)

**Abbildung 5**: drei Produkte pro Zeile angezeigt werden ([klicken Sie hier, um das Bild in voller Größe angezeigt](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image15.png))


Die `RepeatDirection` -Eigenschaft beeinflusst, wie die Elemente in der DataList angelegt werden. Abbildung 5 zeigt die Ergebnisse mit den `RepeatDirection` -Eigenschaftensatz auf `Horizontal`. Beachten Sie, dass die ersten drei Produkte Chai, ändern und Anissirup von links nach rechts und oben nach unten angeordnet sind. Die nächsten drei Produkte (beginnend mit Chef Anton s Cajun Seasoning) werden in einer Zeile unterhalb der ersten drei angezeigt. Ändern der `RepeatDirection` -Eigenschaft zurück auf `Vertical`jedoch diese Produkte von oben nach unten angeordnet, von links nach rechts, wie die Abbildung 6 veranschaulicht.


[![Hier wird werden vertikal angeordnet die Produkte](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image17.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image16.png)

**Abbildung 6**: Hier werden die Produkte vertikal angeordnet ([klicken Sie hier, um das Bild in voller Größe angezeigt](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image18.png))


Die Anzahl der Zeilen in der resultierenden Tabelle angezeigten hängt die Anzahl der Datensätze insgesamt an das DataList gebunden. Gesagt, es s, die die Obergrenze für die gesamte Anzahl der Datenquellenelemente durch dividiert die `RepeatColumns` Eigenschaftswert. Da die `Products` Tabelle hat derzeit 84 Produkte, das durch 3 teilbar ist, 28 Zeilen vorhanden sind. Wenn die Anzahl der Elemente in der Datenquelle und die `RepeatColumns` Eigenschaftswert sind nicht teilbar, und klicken Sie dann die letzte Zeile oder Spalte weist leere Zellen. Wenn die `RepeatDirection` auf festgelegt ist `Vertical`, dann die letzte Spalte leere Zellen; Wenn `RepeatDirection` ist `Horizontal`, und klicken Sie dann die letzte Zeile die leeren Zellen besitzt.

## <a name="summary"></a>Zusammenfassung

Das DataList Listen werden standardmäßig, dessen Elemente in einer Tabelle einspaltiges, mehrzeilige, die das Layout des GridView mit einer einzelnen TemplateField imitiert. Während dieser Standardlayout zulässig ist, können wir Bildschirmfläche maximieren, indem mehrere Datenquellenelemente pro Zeile angezeigt. Dies ist lediglich das DataList s festlegen `RepeatColumns` Eigenschaft, um die Anzahl der Spalten, die pro Zeile angezeigt. Darüber hinaus DataList s `RepeatDirection` Eigenschaft kann verwendet werden, um anzugeben, ob der Inhalt der Tabelle mit mehreren Spalten, die mehrzeilige horizontal von links nach rechts, oben nach unten oder vertikal von oben nach unten, links nach rechts angeordnet werden.

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurde John Suru. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](formatting-the-datalist-and-repeater-based-upon-data-cs.md)
> [Weiter](nested-data-web-controls-cs.md)
