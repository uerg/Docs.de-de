---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-vb
title: Formatieren der DataList und Repeater basierend auf Daten (VB) | Microsoft Docs
author: rick-anderson
description: "In diesem Lernprogramm fügen wir durchlaufen Beispiele, wie wir die Darstellung der DataList und Repeater-Steuerelemente, entweder mithilfe von Formatierungsfunktionen mit formatieren..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: e2f401ae-37bb-4b19-aa97-d6b385d40f88
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 48e0f2bad8c048e943ec2a3ce72cc0f7ca4d34d9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="formatting-the-datalist-and-repeater-based-upon-data-vb"></a>Formatieren der DataList und Repeater basierend auf Daten (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunterladen](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_30_VB.exe) oder [PDF herunterladen](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/datatutorial30vb1.pdf)

> In diesem Lernprogramm fügen wir schrittweise Beispiele, wie wir die Darstellung von Steuerelementen DataList und Repeater Formatierungsfunktionen in Vorlagen oder durch die Behandlung des Ereignisses datengebundenen formatieren.


## <a name="introduction"></a>Einführung

Wie wir im vorherigen Lernprogramm gesehen haben, bietet das DataList eine Reihe von Formatvorlagen bezogenen Eigenschaften, die seine Darstellung beeinflussen. Insbesondere wurde erläutert, wie standardmäßige DataList s CSS-Klassen zuordnen `HeaderStyle`, `ItemStyle`, `AlternatingItemStyle`, und `SelectedItemStyle` Eigenschaften. Zusätzlich zu diesen vier Eigenschaften umfasst DataList eine Reihe von anderen Formatvorlagen bezogenen Steuerelementeigenschaften, z. B. `Font`, `ForeColor`, `BackColor`, und `BorderWidth`, um nur einige zu nennen. Wiederholungsmodul-Steuerelement enthält keine Formatvorlagen bezogenen Eigenschaften. Alle solchen formateinstellungen müssen direkt in das Markup in den Repeater s Vorlagen vorgenommen werden.

Hängt oft von, wie die Daten formatiert werden soll, die Daten selbst. Z. B. beim Auflisten von Produkten wir möchten die Produktinformationen in hell grauer Schrift angezeigt, wenn es nicht mehr unterstützt wird, oder wir hervorheben möchten die `UnitsInStock` Wert, wenn sie 0 (null) ist. Wie wir im vorherigen Lernprogrammen gesehen haben, bieten die GridView, DetailsView und FormView zwei unterschiedliche Möglichkeiten, ihre Darstellung auf Basis ihrer Daten formatieren:

- **Die `DataBound` Ereignis** erstellen Sie einen Ereignishandler für das entsprechende `DataBound` Ereignis, das ausgelöst wird, nachdem die Daten mit jedem Element gebunden wurde (für die GridView war die `RowDataBound` Ereignis; für das DataList und Repeater ist die `ItemDataBound`Ereignis). In diesem Ereignis Handler, der gebundenen Daten nur untersucht werden kann, und formatieren Entscheidungen vorgenommen. Diese Technik in untersucht die [benutzerdefinierte Formatierung basierend auf Daten](../custom-formatting/custom-formatting-based-upon-data-vb.md) Lernprogramm.
- **Formatieren von Funktionen in den Vorlagen** bei Verwendung von TemplateFields in die DetailsView oder GridView-Steuerelemente oder einer Vorlage in der FormView-Steuerelement kann ein Formatierungsfunktion zur ASP.NET Seite "s" Code-Behind-Klasse, die Business Logic Layer oder mit einer hinzufügen andere Klassenbibliothek, die aus der Webanwendung zugänglich ist. Diese Formatierungsfunktion kann eine beliebige Anzahl von Eingabeparametern übernehmen, aber es muss den HTML-Code zum Rendern in der Vorlage zurückgeben. Formatierung Funktionen wurden zunächst geprüft, der [mithilfe von TemplateFields, im des GridView-Steuerelements](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) Lernprogramm.

Beide dieser Techniken Formatierung sind mit den verschiedenen Steuerelementen verfügbar. In diesem Lernprogramm fügen wir Beispiele zur Verwendung der beiden Techniken für beide Steuerelemente durchlaufen.

## <a name="using-theitemdataboundevent-handler"></a>Mithilfe der`ItemDataBound`-Ereignishandler

Wenn Daten gebunden ist, eine DataList ein Datenquellen-Steuerelement oder über programmgesteuert Zuweisen von Daten an das Steuerelement s `DataSource` -Eigenschaft und der Aufruf seiner `DataBind()` -Methode, die DataList s `DataBinding` Ereignis ausgelöst wird, die Datenquelle aufgelistet, und jeder Datensatz an DataList gebunden ist. Für jeden Datensatz in der Datenquelle, die DataList erstellt eine [ `DataListItem` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.datalistitem.aspx) -Objekt ab, und klicken Sie dann auf den aktuellen Datensatz gebunden. Während dieses Vorgangs werden die DataList zwei Ereignisse ausgelöst:

- **`ItemCreated`**wird ausgelöst, nachdem der `DataListItem` erstellt wurde
- **`ItemDataBound`**wird ausgelöst, nachdem der aktuelle Datensatz an gebunden wurde die`DataListItem`

Die folgenden Schritte beschreiben die Bindung von Daten für das DataList-Steuerelement.

1. Das DataList s [ `DataBinding` Ereignis](https://msdn.microsoft.com/en-us/library/system.web.ui.control.databinding.aspx) ausgelöst wird
2. Die Daten ist an das DataList gebunden.  
  
 Für jeden Datensatz in der Datenquelle 

    1. Erstellen einer `DataListItem` Objekt
    2. Auslösen der [ `ItemCreated` Ereignis](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.datalist.itemcreated.aspx)
    3. Binden Sie den Datensatz der`DataListItem`
    4. Auslösen der [ `ItemDataBound` Ereignis](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.datalist.itemdatabound.aspx)
    5. Hinzufügen der `DataListItem` auf die `Items` Auflistung

Beim Binden von Daten an das Wiederholungsmodul-Steuerelement, wechselt es über die gleichen genaue Reihenfolge von Schritten. Der einzige Unterschied ist, dass statt des `DataListItem` Instanzen erstellt wird, die Repeater verwendet [ `RepeaterItem` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.repeateritem(VS.80).aspx)s.

> [!NOTE]
> Der aufmerksame Leser eine leichte Anomalie zwischen der Abfolge von Schritten aufgefallen, die ablaufen müssen, wenn die DataList und Repeater an Daten im Vergleich zu gebunden sind, wenn die GridView an Daten gebunden ist. Am Ende des Protokollfragments der Bindung von Daten, die GridView löst die `DataBound` Ereignis; jedoch weder die Repeater das DataList-Steuerelement einem solchen haben. Dies ist, da die verschiedenen Steuerelemente wieder in den ASP.NET 1.x Zeitrahmen erstellt wurden, bevor die vor und nach dem Kompatibilitätsgrad Handler-Ereignismuster allgemeine werden mussten.


Wie die GridView eine Option für die Formatierung auf Basis der Daten ist erstellen Sie einen Ereignishandler für das `ItemDataBound` Ereignis. Dieser Ereignishandler würde überprüfen Sie die Daten, die gerade an gebunden wurde, hatte die `DataListItem` oder `RepeaterItem` und wirkt sich auf die Formatierung des Steuerelements nach Bedarf.

Formatieren von Änderungen für das DataList-Steuerelement, das gesamte Element implementiert werden kann, mit der `DataListItem` s Formatvorlagen bezogenen Steuerelementeigenschaften, darunter den Standard `Font`, `ForeColor`, `BackColor`, `CssClass`und so weiter. Um die Formatierung von bestimmten Web-Steuerelementen innerhalb der DataList-s-Vorlage zu beeinflussen, müssen wir programmgesteuerten Zugriff auf und Ändern des Stils von diesen Websteuerelemente. Wurde erläutert, wie diese zurück in die *benutzerdefinierte Formatierung basierend auf Daten* Lernprogramm. Wiederholungsmodul-Steuerelement, wie die `RepeaterItem` -Klasse verfügt über keine Formatvorlagen bezogenen Steuerelementeigenschaften; daher alle Formatvorlagen bezogenen Änderungen an einer `RepeaterItem` in der `ItemDataBound` Ereignishandler muss erfolgen, indem Sie programmgesteuert zugreifen und diese aktualisieren Websteuerelementen in die Vorlage.

Da die `ItemDataBound` Formatierung Technik für das DataList und Repeater nahezu identisch sind, unser Beispiel konzentriert sich auf das DataList mit.

## <a name="step-1-displaying-product-information-in-the-datalist"></a>Schritt 1: Anzeigen von Produktinformationen in den DataList

Vor Gedanken machen, wir die Formatierung, Let s zuerst erstellen Sie eine Seite, die eine DataList verwendet wird, um Produktinformationen anzuzeigen. In der [vorherigen Lernprogramm](displaying-data-with-the-datalist-and-repeater-controls-vb.md) wir DataList erstellt, deren `ItemTemplate` jede s, Produktname, die Kategorie, die Lieferanten, die Menge pro Einheit und Preis angezeigt. Lassen Sie s, die diese Funktionalität wird hier in diesem Lernprogramm zu wiederholen. Um dies zu erreichen, Sie können entweder neu DataList und seine ObjectDataSource von Grund auf neu erstellen oder Sie können diese Steuerelemente auf der Seite, die im vorherigen Lernprogramm erstellt kopieren (`Basics.aspx`) und fügen Sie sie auf der Seite für dieses Lernprogramm (`Formatting.aspx`).

Nachdem Sie die DataList und ObjectDataSource Funktionen aus repliziert wurden `Basics.aspx` in `Formatting.aspx`, erkundet so ändern Sie die DataList s `ID` Eigenschaft aus `DataList1` zu einem aussagekräftigeren `ItemDataBoundFormattingExample`. Als Nächstes das DataList in einem Browser anzuzeigen. Wie in Abbildung 1 gezeigt, ist der einzige Formatierung Unterschied zwischen jedes Produkt, die Farbe des Hintergrunds wechselt.


[![Produkte werden in das DataList-Steuerelement aufgelistet.](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image2.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image1.png)

**Abbildung 1**: die Produkte werden aufgelistet, in das DataList-Steuerelement ([klicken Sie hier, um das Bild in voller Größe angezeigt](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image3.png))


Können Sie für dieses Lernprogramm s DataList formatieren, dass alle Produkte mit einem Preis kleiner 20,00 sowohl der Name müssen und Unit price gelb hervorgehoben.

## <a name="step-2-programmatically-determining-the-value-of-the-data-in-the-itemdatabound-event-handler"></a>Schritt 2: Programmgesteuert den Wert der Daten in der ItemDataBound Ereignishandler zu bestimmen

Da nur solche Produkte mit einem Preis unter 20,00 wird haben, die benutzerdefinierte Formatierung angewendet wird, muss wir jede s Produktpreis bestimmen können. Beim Binden von Daten zu einem DataList DataList zählt die Datensätze in der Datenquelle und, für jeden Datensatz erstellt eine `DataListItem` Instanz Datensatz in der Datenquelle zu binden der `DataListItem`. Nach den entsprechenden Datensatz s Daten gebunden wurde mit dem aktuellen `DataListItem` -Objekt, das DataList s `ItemDataBound` Ereignis ausgelöst wird. Wir können einen Ereignishandler für dieses Ereignis, um die Datenwerte für die aktuelle überprüfen erstellen `DataListItem` und auf Grundlage dieser Werte, Änderungen Formatierung erforderlich.

Erstellen Sie ein `ItemDataBound` -Ereignis für das DataList und fügen Sie den folgenden Code hinzu:


[!code-vb[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample1.vb)]

Während das Konzept und die Semantik hinter der DataList s `ItemDataBound` Ereignishandler sind identisch mit denen durch die GridView s. verwendet `RowDataBound` -Ereignishandler in der *benutzerdefinierte Formatierung basierend auf Daten* Lernprogramm unterscheidet sich die Syntax etwas. Wenn der `ItemDataBound` Ereignis wird ausgelöst, der `DataListItem` nur an Daten gebunden wird übergeben, in die entsprechenden Ereignishandler über `e.Item` (anstelle von `e.Row`bei der GridView s `RowDataBound` Ereignishandler). Das DataList s `ItemDataBound` -Ereignishandler ausgelöst wird, für die *jedes* Zeile DataList, einschließlich der Kopfzeilen, Fußzeilen und Trennzeichen für Zeilen hinzugefügt. Die Produktinformationen ist allerdings nur auf die Datenzeilen gebunden. Aus diesem Grund bei Verwendung der `ItemDataBound` Ereignis, um die Daten zu überprüfen, die an das DataList gebunden werden, müssen wir zuerst sicherstellen, dass wir arbeiten mit einem Datenelement. Dies kann durch Überprüfen der `DataListItem` s [ `ItemType` Eigenschaft](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.datalistitem.itemtype.aspx), wofür eines können [die folgenden acht Werte](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.listitemtype.aspx):

- `AlternatingItem`
- `EditItem`
- `Footer`
- `Header`
- `Item`
- `Pager`
- `SelectedItem`
- `Separator`

Beide `Item` und `AlternatingItem``DataListItem` s Zusammensetzung der DataList-s-Datenelemente. Wir arbeiten vorausgesetzt ein `Item` oder `AlternatingItem`, wir Zugriff auf den tatsächlichen `ProductsRow` -Instanz, die mit dem aktuellen gebunden war `DataListItem`. Die `DataListItem` s [ `DataItem` Eigenschaft](https://msdn.microsoft.com/en-us/system.web.ui.webcontrols.datalistitem.dataitem.aspx) enthält einen Verweis auf die `DataRowView` Objekt, dessen `Row` Eigenschaft stellt einen Verweis auf den tatsächlichen `ProductsRow` Objekt.

Wir überprüfen Sie anschließend die `ProductsRow` Instanz s `UnitPrice` Eigenschaft. Seit der Products-Tabelle s `UnitPrice` Feld ermöglicht es `NULL` Werte, bevor Sie versuchen, den Zugriff auf die `UnitPrice` Eigenschaft wir zuerst prüfen um festzustellen, ob eine `NULL` mithilfe der `IsUnitPriceNull()` Methode. Wenn der `UnitPrice` Wert ist kein `NULL`, wir überprüfen Sie dann, wenn finden Sie unter er kleiner als 20,00. Wenn es tatsächlich unter 20,00 handelt, müssen wir die benutzerdefinierte Formatierung anwenden.

## <a name="step-3-highlighting-the-product-s-name-and-price"></a>Schritt 3: Hervorhebung s Produktname und Preis

Sobald wir wissen, dass ein Produktpreis s weniger als $20,00 ist, alle bleibt besteht darin, markieren Sie den Namen und den Preis. Um dies zu erreichen, es muss zuerst programmgesteuert verweisen, Label-Steuerelemente in der `ItemTemplate` , von denen der Produktname s und Preis angezeigt. Als Nächstes benötigen wir ihnen einen gelben Hintergrund angezeigt. Diese Formatierungsinformationen angewendet werden kann, durch direkte Modifizierung der Bezeichnungen `BackColor` Eigenschaften (`LabelID.BackColor = Color.Yellow`); idealerweise, allerdings sollten alle Angelegenheiten im Zusammenhang mit der Anzeige durch cascading Stylesheets ausgedrückt werden. Tatsächlich haben wir bereits ein Stylesheet, das die gewünschte Formatierung in definierten bietet `Styles.css`  -  `AffordablePriceEmphasis`, die Erstellung und erläutert, die der *benutzerdefinierte Formatierung basierend auf Daten* Lernprogramm.

Um die Formatierung anzuwenden, legen Sie einfach zwei Bezeichnung Websteuerelementen `CssClass` Eigenschaften `AffordablePriceEmphasis`, wie im folgenden Code gezeigt:


[!code-vb[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample2.vb)]

Mit der `ItemDataBound` -Ereignishandler ausgeführt, rufen Sie erneut die `Formatting.aspx` Seite in einem Browser. Wie in Abbildung 2 dargestellt, müssen diese Produkte mit einem Preis unter 20,00 ihren Namen und den Preis hervorgehoben.


[![Diese Produkte weniger als $20,00 hervorgehoben werden](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image5.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image4.png)

**Abbildung 2**: solche Produkte weniger als $20,00 werden hervorgehoben ([klicken Sie hier, um das Bild in voller Größe angezeigt](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image6.png))


> [!NOTE]
> Da das DataList als HTML gerendert wird `<table>`, dessen `DataListItem` Instanzen verfügen über Formatvorlagen bezogenen Eigenschaften, die festgelegt werden können, um ein bestimmtes Format auf das gesamte Element anzuwenden. Angenommen, wir möchten, markieren Sie die *gesamte* Element Gelb, wenn ihren Preis weniger als $20,00 wurde, konnte haben ersetzten wir den Code, der die Bezeichnungen auf die verwiesen wird, und legen ihre `CssClass` Eigenschaften mit der folgenden Codezeile: `e.Item.CssClass = "AffordablePriceEmphasis"` (siehe Abbildung 3).


Die `RepeaterItem` s, aus denen Wiederholungsmodul-Steuerelement, allerdings Stefan t bieten, solche Eigenschaften Format auf. Anwenden einer benutzerdefinierten Formatierung auf Repeater erfordert daher die Anwendung der Formateigenschaften für die Web-Steuerelemente in den Vorlagen Repeater s nur wie in Abbildung 2.


[![Das gesamte Produkt-Element wird für Produkte unter 20,00 hervorgehoben.](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image8.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image7.png)

**Abbildung 3**: die gesamte Produktartikel für Produkte unter 20,00 hervorgehoben ([klicken Sie hier, um das Bild in voller Größe angezeigt](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image9.png))


## <a name="using-formatting-functions-from-within-the-template"></a>Verwenden die Formatierungsfunktionen aus der Vorlage

In der *mithilfe von TemplateFields, im des GridView-Steuerelements* Lernprogramm wurde erläutert, wie eine Formatierung Funktion innerhalb einer GridView TemplateField verwenden, um benutzerdefinierte Formatierung auf Grundlage der Daten an die GridView s Zeilen gebunden. Eine Formatierung Funktion ist eine Methode, die aufgerufen werden kann, aus einer Vorlage und gibt den HTML-Code an seiner Stelle ausgegeben werden. Formatierung Funktionen können sich in der ASP.NET Seite "s" Code-Behind-Klasse befinden oder in Klassendateien in zentralisiert werden die `App_Code` Ordner oder in einem separaten Klassenbibliotheksprojekt. Verschieben die Formatierungsfunktion außerhalb der ASP.NET Seite s-Code-Behind-Klasse ist ideal, wenn Sie verwenden die gleiche Formatierungsfunktion in mehrere ASP.NET-Seiten oder in anderen ASP.NET-Webanwendungen möchten.

Zur Veranschaulichung Formatierungsfunktionen können s haben die Produktinformationen, die den Text [DISCONTINUED] neben den Produktnamen s enthalten, wenn es s nicht mehr unterstützt. Let s müssen außerdem hervorgehobenen gelben Preis If es kleiner als 20,00 $ s (wie der `ItemDataBound` Beispiel für einen Ereignishandler); der Kurs ist 20,00 oder höher, können s nicht den tatsächlichen Preis angezeigt, aber rufen stattdessen der Text ein, wählen Sie für ein Preisangebot. Abbildung 4 zeigt einen Screenshot der Produkte, die mit diesen Regeln für die Formatierung angewendet.


[![Für teure Produkte wird der Preis mit dem Text, rufen Sie einen Preis als Anführungszeichen ersetzt.](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image11.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image10.png)

**Abbildung 4**: für teure Produkte der Preis wird mit dem Text, rufen Sie für ein Preisangebot ersetzt ([klicken Sie hier, um das Bild in voller Größe angezeigt](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image12.png))


## <a name="step-1-create-the-formatting-functions"></a>Schritt 1: Erstellen Sie die Formatierungsfunktionen

In diesem Beispiel zwei Formatierungsfunktionen bereitgestellt: eines, in dem der Name des Produkts zusammen mit dem Text [DISCONTINUED], angezeigt, bei Bedarf, und ein anderes erforderlich, die entweder einen markierten Preis wird angezeigt, wenn sie s kleiner 20,00 oder den Text ein, rufen Sie für eine Kostenvoranschlag andernfalls. Ermöglichen von s, die diese Funktionen in der ASP.NET Seite "s" Code-Behind-Klasse erstellen, und nennen Sie diese `DisplayProductNameAndDiscontinuedStatus` und `DisplayPrice`. Beide Methoden müssen den HTML-Code zum Rendern als Zeichenfolge zurück und müssen beide markiert werden `Protected` (oder `Public`) um ab der ASP.NET Seite s deklarative Syntax aufgerufen werden. Der Code für diese beiden Methoden folgt:


[!code-vb[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample3.vb)]

Beachten Sie, dass die `DisplayProductNameAndDiscontinuedStatus` Methode akzeptiert die Werte von der `productName` und `discontinued` Datenfelder als skalare Werte als auch während der `DisplayPrice` Methode akzeptiert eine `ProductsRow` Instanz (anstelle eines `unitPrice` Skalarwert). Beide Vorgehensweisen funktioniert. jedoch wenn die Formatierungsfunktion mit skalaren Werten, die Datenbank enthalten kann `NULL` Werte (z. B. `UnitPrice`; weder `ProductName` noch `Discontinued` zulassen `NULL` Werte), spezielle muss darauf geachtet werden bei der Verarbeitung von diese Skalare Eingaben.

Insbesondere muss der Eingabeparameter vom Typ `Object` seit der eingehende Wert möglicherweise eine `DBNull` Instanz statt mit den erwarteten Datentyp. Darüber hinaus muss eine Überprüfung vorgenommen werden, um zu bestimmen, und zwar unabhängig davon, ob der eingehende Wert einer Datenbank ist `NULL` Wert. D. h., würden wir den `DisplayPrice` Methode, um den Preis als Skalarwert, wir d akzeptieren müssen den folgenden Code verwenden:


[!code-vb[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample4.vb)]

Beachten Sie, dass die `unitPrice` input-Parameter ist vom Typ `Object` und, die die Auswertung der Anweisung geändert wurde, um zu erfahren, wenn `unitPrice` ist `DBNull` oder nicht. Darüber hinaus seit der `unitPrice` Eingabeparameter wird als übergeben ein `Object`, müssen mit einem decimal-Wert umgewandelt werden.

## <a name="step-2-calling-the-formatting-function-from-the-datalist-s-itemtemplate"></a>Schritt 2: Aufrufen der Formatierungsfunktion aus dem ItemTemplate DataList s

Bei der Formatierung unsere ASP.NET Seite "s" Code-Behind-Klasse hinzugefügt, übrig bleibt, rufen diese Funktionen aus der DataList s Formatierung `ItemTemplate`. Um eine Formatierungsfunktion aus einer Vorlage zu aufzurufen, platzieren Sie den Aufruf der Funktion innerhalb der Databinding-Syntax:


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample5.aspx)]

In DataList s `ItemTemplate` der `ProductNameLabel` Bezeichnung Websteuerelement derzeit zeigt den Produktnamen s durch Zuweisen der `Text` Eigenschaft das Ergebnis von `<%# Eval("ProductName") %>`. Damit haben sie den Namen und den Text [DISCONTINUED], anzeigen, wenn benötigt, aktualisieren Sie die deklarative Syntax, damit er stattdessen weist der `Text` Eigenschaft den Wert von der `DisplayProductNameAndDiscontinuedStatus` Methode. Dabei müssen wir übergeben in s, Produktname und nicht mehr unterstützte Werte mithilfe der `Eval("columnName")` Syntax. `Eval`Gibt einen Wert vom Typ `Object`, aber die `DisplayProductNameAndDiscontinuedStatus` Methode erwartet Eingabeparameter vom Typ `String` und `Boolean`; aus diesem Grund müssen wir die Rückgabewerte Umwandeln der `Eval` Methode, um die erwartete Eingabeparameter Typen wie folgt:


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample6.aspx)]

Wir können ganz einfach festlegen, um den Preis anzuzeigen, die `UnitPriceLabel` Bezeichnung s `Text` Eigenschaft, um den Rückgabewert von der `DisplayPrice` -Methode, wie wir für die Anzeige der Produktname s haben und die [] Text eingestellt. Anstatt jedoch zu übergeben, in der `UnitPrice` als skalare Eingabeparameter, wir stattdessen übergeben Sie in der gesamten `ProductsRow` Instanz:


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample7.aspx)]

Bei den Aufrufen der Formatierung Funktionen vorhanden verwendet werden Sie, können Sie unseren Fortschritt in einem Browser anzuzeigen. Ihren Bildschirm sollte die nicht mehr unterstützte Produkte, einschließlich des Texts [DISCONTINUED] ähnlich wie in Abbildung 5 aussehen und die Produkte, die mehr als $20,00 mit ihren Preis Kostenberechnungen ersetzt mit dem Text bitte Aufruf für ein Preisangebot.


[![Für teure Produkte wird der Preis mit dem Text, rufen Sie einen Preis als Anführungszeichen ersetzt.](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image14.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image13.png)

**Abbildung 5**: für teure Produkte der Preis wird mit dem Text, rufen Sie für ein Preisangebot ersetzt ([klicken Sie hier, um das Bild in voller Größe angezeigt](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image15.png))


## <a name="summary"></a>Zusammenfassung

Formatieren den Inhalt eines Steuerelements DataList oder Repeater basierend auf die Daten kann mithilfe von zwei Techniken erreicht werden. Das erste Verfahren ist, erstellen Sie einen Ereignishandler für das `ItemDataBound` Ereignis, das ausgelöst wird, wie jeder Datensatz in der Datenquelle, um ein neues gebunden ist `DataListItem` oder `RepeaterItem`. In der `ItemDataBound` Ereignishandler, d. h. die aktuelle Elementdaten s untersucht werden können und dann Formatierung kann angewendet werden, auf den Inhalt der Vorlage oder für `DataListItem` s, um das gesamte Element selbst.

Alternativ kann eine benutzerdefinierte Formatierung realisiert werden durch das Formatieren von Funktionen. Eine Formatierungsfunktion ist eine Methode, die von der DataList aufgerufen werden kann oder Repeater s Vorlagen, die den HTML-Code an seiner Stelle auszugebende zurückgibt. Der HTML-Code von einer Formatierung Funktion zurückgegeben wird häufig durch die Werte, die mit dem aktuellen Element gebunden wird bestimmt. Diese Werte entweder als Skalarwerte oder durch Übergeben des gesamten Objekts gebunden wird, auf das Element in die Formatierungsfunktion übergeben werden (z. B. die `ProductsRow` Instanz).

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurden Yaakov Ellis Randy Schmidt und Liz Shulok. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Zurück](displaying-data-with-the-datalist-and-repeater-controls-vb.md)
[Weiter](showing-multiple-records-per-row-with-the-datalist-control-vb.md)
