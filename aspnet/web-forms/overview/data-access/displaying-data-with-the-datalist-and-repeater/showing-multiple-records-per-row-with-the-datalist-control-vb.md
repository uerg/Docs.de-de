---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/showing-multiple-records-per-row-with-the-datalist-control-vb
title: Anzeigen von mehreren Datensätzen pro Zeile mit dem DataList-Steuerelement (VB) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Lernprogramm lernen wir des DataList-Steuerelement-Layout über seine RepeatColumns und RepeatDirection Eigenschaften anpassen.
ms.author: aspnetcontent
ms.date: 09/13/2006
ms.assetid: f555c531-bf33-4699-9987-42dbfef23c1f
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/showing-multiple-records-per-row-with-the-datalist-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 55e07159fd9d0f4c750a2522feb0538a1cfb4bea
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37831886"
---
<a name="showing-multiple-records-per-row-with-the-datalist-control-vb"></a>Anzeigen von mehreren Datensätzen pro Zeile mit dem DataList-Steuerelement (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunter](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_31_VB.exe) oder [PDF-Datei herunterladen](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/datatutorial31vb1.pdf)

> In diesem Lernprogramm lernen wir des DataList-Steuerelement-Layout über seine RepeatColumns und RepeatDirection Eigenschaften anpassen.


## <a name="introduction"></a>Einführung

In den Beispielen DataList wir haben gesehen, in den letzten beiden Tutorials haben jeder Datensatz aus der Datenquelle als eine Zeile in eine einspaltige HTML gerendert `<table>`. Obwohl dies das Standardverhalten für das DataList-Steuerelement ist, ist es sehr einfach, die DataList-Anzeige anzupassen, dass der Datenquellenelemente, die auf eine Tabelle mit mehreren Spalten und mehrere Zeilen verteilt sind. Darüber hinaus es möglich, dass alle Daten Quelle in einem einzigen Zeile und mehrspaltige DataList-Steuerelement angezeigten Elemente.

Wir können das Layout DataList s durch Anpassen der `RepeatColumns` und `RepeatDirection` Eigenschaften, die jeweils angeben, wie viele Spalten gerendert werden, und gibt an, ob die Elemente vertikal oder horizontal angeordnet werden. Abbildung 1 zeigt z. B. einem DataList-Steuerelement, in dem Produktinformationen in einer Tabelle mit drei Spalten angezeigt.


[![DataList-Steuerelement zeigt die drei Produkte pro Zeile](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image2.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image1.png)

**Abbildung 1**: das DataList-Steuerelement zeigt drei Produkte pro Zeile ([klicken Sie, um das Bild in voller Größe anzeigen](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image3.png))


Durch mehrere Data Source-Elemente pro Zeile angezeigt wird, kann die DataList-Steuerelement effektiver horizontalen Bildschirmplatz nutzen. In diesem Lernprogramm lernen wir diese beiden DataList-Eigenschaften.

## <a name="step-1-displaying-product-information-in-a-datalist"></a>Schritt 1: Anzeigen von Produktinformationen in einem DataList-Steuerelement

Bevor wir untersuchen die `RepeatColumns` und `RepeatDirection` Eigenschaften können s erstellen Sie zunächst einem DataList-Steuerelement auf unserer Seite aus, mit der Standardtabelle für einzelne Spalten, die mehrzeilige Layout Produktinformationen aufgeführt. In diesem Beispiel können Sie s, die die Produkt-s-Name, Kategorie und Preis, verwenden das folgende Markup zeigt:


[!code-html[Main](showing-multiple-records-per-row-with-the-datalist-control-vb/samples/sample1.html)]

Wir haben gesehen, wie binden Sie Daten an einem DataList-Steuerelement in den vorherigen Beispielen, sodass ich diese Schritte schnell durchlaufen werden. Öffnen Sie zunächst die `RepeatColumnAndDirection.aspx` auf der Seite die `DataListRepeaterBasics` Ordner, und ziehen Sie einem DataList-Steuerelement aus der Toolbox in den Designer. Deaktivieren Sie aus dem Smarttag DataList s, um eine neue "ObjectDataSource" erstellen und konfigurieren, dass die Daten aus der pull die `ProductsBLL` Klasse s `GetProducts` -Methode, die (None) auswählen, über den Assistenten s INSERT-, Update-, aus, und Löschen von Registerkarten.

Nach dem Erstellen und dem neuen ObjectDataSource-Steuerelement an die Datenliste binden, Visual Studio erstellt automatisch eine `ItemTemplate` , die den Namen und Wert für jedes Produkt Datenfelder anzeigt. Anpassen der `ItemTemplate` entweder direkt über den deklarativen Markup oder aus den Vorlagen bearbeiten Sie die option im Smarttag DataList s so, dass sie das Markup oben, und Ersetzen Sie dabei verwendet der *Produktname*, *Kategoriename* , und *Preis* Text, Label-Steuerelementen, die die entsprechenden Databinding-Syntax zu verwenden, um Werte zu ihren `Text` Eigenschaften. Nach der Aktualisierung der `ItemTemplate`, im deklarativen Markup Ihrer Seite s sollte etwa wie folgt aussehen:


[!code-aspx[Main](showing-multiple-records-per-row-with-the-datalist-control-vb/samples/sample2.aspx)]

Beachten Sie, dass Ve enthalten einen Formatbezeichner in der `Eval` Databinding-Syntax für die `UnitPrice`, den zurückgegebenen Wert als Währung - Formatierung `Eval("UnitPrice", "{0:C}").`

Nehmen Sie einen Moment Zeit, Ihre Seite in einem Browser besuchen. Wie in Abbildung 2 gezeigt, wird Sie als einspaltiges, mehrzeilige Tabelle Produkte DataList-Steuerelement gerendert.


[![Standardmäßig wird das DataList-Steuerelement gerendert als einspaltiges, mehrzeilige Tabelle](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image5.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image4.png)

**Abbildung 2**: standardmäßig, das DataList-Steuerelement, das als eine einspaltige, Tabelle mit mehreren Zeilen gerendert ([klicken Sie, um das Bild in voller Größe anzeigen](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image6.png))


## <a name="step-2-changing-the-datalist-s-layout-direction"></a>Schritt 2: Ändern die Layoutrichtung der DataList-s

Während das Standardverhalten für DataList-Steuerelement ist, um das Layout der Elemente vertikal in einer Tabelle eine Spalte, die mehrzeilige dieses Verhalten kann leicht geändert werden über DataList s [ `RepeatDirection` Eigenschaft](https://msdn.microsoft.com/system.web.ui.webcontrols.datalist.repeatdirection.aspx). Die `RepeatDirection` Eigenschaft kann einen von zwei möglichen Werten annehmen: `Horizontal` oder `Vertical` (Standard).

Durch Ändern der `RepeatDirection` Eigenschaft `Vertical` zu `Horizontal`, DataList-Steuerelement rendert der Datensätze in einer einzelnen Zeile, erstellen eine Spalte pro Datenquellenelement. Um diesen Effekt zu veranschaulichen, klicken Sie auf das DataList-Steuerelement im Designer, und ändern Sie dann im Eigenschaftenfenster die `RepeatDirection` Eigenschaft `Vertical` zu `Horiztonal`. Sofort auf diese Weise der Designer passt das DataList s Layout, erstellen eine einzelne Zeile, die mit mehreren Spalte Schnittstelle (siehe Abbildung 3).


[![Die RepeatDirection-Eigenschaft bestimmt, wie die Richtung der DataList-s-Elemente werden angeordnet](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image8.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image7.png)

**Abbildung 3**: die `RepeatDirection` Eigenschaft gibt vor, wie die Richtung der DataList-s-Elemente Laid Out werden ([klicken Sie, um das Bild in voller Größe anzeigen](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image9.png))


Kleine Mengen von Daten, die eine einzelne Zeile und Anzeigen von möglicherweise mehrspaltige Tabellen eine ideale Möglichkeit, um die Bildschirmfläche zu maximieren. Für größere Datenmengen wird eine einzelne Zeile erfordern jedoch zahlreiche Spalten, die die Push-Vorgänge diese Elemente, kann nicht auf dem Bildschirm ausschalten nach rechts anpassen. Abbildung 4 zeigt die Produkte aus, wenn in einem einzeiligen DataList-Steuerelement gerendert wird. Da es sich viele Produkte (mehr als 80), verfügt der Benutzer weit nach rechts, um Informationen zu den einzelnen Produkte anzeigen einen Bildlauf durchführen.


[![Bei ausreichend großen Datenquellen wird eine einzelne Spalte DataList erfordert horizontalen Bildlauf](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image11.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image10.png)

**Abbildung 4**: für ausreichend große Datenquellen, die eine einzelne Spalte DataList-Steuerelement wird erfordert horizontalen Bildlauf ([klicken Sie, um das Bild in voller Größe anzeigen](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image12.png))


## <a name="step-3-displaying-data-in-a-multi-column-multi-row-table"></a>Schritt 3: Anzeigen von Daten in eine Tabelle mit mehreren Spalten und mehrere Zeilen

Um einem mehrspaltigen und mehrzeilige DataList-Steuerelement zu erstellen, müssen wir legen die [ `RepeatColumns` Eigenschaft](https://msdn.microsoft.com/system.web.ui.webcontrols.datalist.repeatcolumns.aspx) auf die Anzahl der Spalten angezeigt werden sollen. In der Standardeinstellung die `RepeatColumns` Eigenschaft ist auf 0 festgelegt, dadurch DataList-Steuerelement alle Elemente in eine einzelne Zeile oder Spalte angezeigt wird (abhängig vom Wert der `RepeatDirection` Eigenschaft).

In unserem Beispiel können Sie s, die drei Produkte pro Zeile der Tabelle angezeigt. Legen Sie daher die `RepeatColumns` -Eigenschaft auf 3 fest. Nehmen Sie einen Moment Zeit, um die Ergebnisse in einem Browser anzuzeigen, nach dieser Änderung. Wie in Abbildung 5 gezeigt, werden die Produkte in einer Tabelle drei Spalten und mehreren Zeilen jetzt aufgeführt.


[![Es werden drei Produkte pro Zeile angezeigt.](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image14.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image13.png)

**Abbildung 5**: drei Produkte pro Zeile angezeigt werden ([klicken Sie, um das Bild in voller Größe anzeigen](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image15.png))


Die `RepeatDirection` Eigenschaft wirkt sich auf wie die Elemente im DataList-Steuerelement angeordnet werden. Abbildung 5 zeigt die Ergebnisse mit den `RepeatDirection` -Eigenschaftensatz auf `Horizontal`. Beachten Sie, dass die ersten drei Produkte Chai Chang und Anissirup von links nach rechts und von oben nach unten angeordnet werden. Die nächsten drei Produkte (beginnend mit Chef Anton s Cajun Seasoning) werden in einer Zeile unterhalb der ersten drei angezeigt. Ändern der `RepeatDirection` -Eigenschaft zurück auf `Vertical`, aber diese Produkte von oben nach unten angeordnet, von links nach rechts, wie in Abbildung 6 gezeigt.


[![Hier werden die Produkte vertikal angeordnet](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image17.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image16.png)

**Abbildung 6**: Hier werden die Produkte vertikal angeordnet ([klicken Sie, um das Bild in voller Größe anzeigen](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image18.png))


Die Anzahl der Zeilen, die in der resultierenden Tabelle angezeigten hängt von der Anzahl der Datensätze insgesamt an die Datenliste gebunden. Genauer gesagt wird es s, die die Obergrenze der Gesamtzahl der Datenquellenelemente durch geteilt die `RepeatColumns` -Eigenschaftswert. Da die `Products` Tabelle verfügt derzeit über 84 Produkte, die durch 3 teilbar ist, 28 Zeilen vorhanden sind. Wenn die Anzahl der Elemente in der Datenquelle und die `RepeatColumns` Eigenschaftswert sind nicht gleichmäßig geteilt werden, und klicken Sie dann die letzte Zeile oder Spalte weist leere Zellen. Wenn die `RepeatDirection` nastaven NA hodnotu `Vertical`, und klicken Sie dann die letzte Spalte leere Zellen hat `RepeatDirection` ist `Horizontal`, die letzte Zeile die leeren Zellen müssen.

## <a name="summary"></a>Zusammenfassung

DataList-Steuerelement, listet die Elemente in eine einspaltige, mehrzeilige-Tabelle, die imitiert das Layout einer GridView-Ansicht mit einem einzelnen TemplateField standardmäßig aus. Während dieser Standardlayout akzeptabel ist, können wir Bildschirmfläche maximieren, indem mehrere Datenquellenelemente pro Zeile angezeigt. Dies ist einfach darum, DataList-Steuerelement s `RepeatColumns` Eigenschaft, um die Anzahl der Spalten pro Zeile angezeigt werden sollen. Darüber hinaus DataList-Steuerelement s `RepeatDirection` Eigenschaft kann verwendet werden, um anzugeben, ob der Inhalt der Tabelle mit mehreren Spalten, die mehrzeilige horizontal von links nach rechts und von oben nach unten oder vertikal von oben nach unten, von links nach rechts angeordnet werden soll.

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial ist John Suru. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](formatting-the-datalist-and-repeater-based-upon-data-vb.md)
> [Weiter](nested-data-web-controls-vb.md)
