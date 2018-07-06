---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-vb
title: Formatieren des DataList- und Wiederholungssteuerelements auf Datenbasis (VB) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial werden wir schrittweise Beispiele wie wir die Darstellung der DataList- oder Repeater-Steuerelemente, entweder mithilfe von Formatierungsfunktionen mit formatieren...
ms.author: aspnetcontent
ms.date: 09/13/2006
ms.assetid: e2f401ae-37bb-4b19-aa97-d6b385d40f88
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 428438b2bae062c09d13c002f4729c3c394975a1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807709"
---
<a name="formatting-the-datalist-and-repeater-based-upon-data-vb"></a>Formatieren des DataList- und Wiederholungssteuerelements auf Datenbasis (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunter](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_30_VB.exe) oder [PDF-Datei herunterladen](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/datatutorial30vb1.pdf)

> In diesem Tutorial werden wir schrittweise Beispiele wie wir die Darstellung der DataList- oder Repeater-Steuerelemente, entweder mithilfe von Formatierungsfunktionen in Vorlagen oder durch Behandlung des Ereignisses für die Datenbindung zu formatieren.


## <a name="introduction"></a>Einführung

Wie wir im vorherigen Tutorial gesehen haben, bietet DataList-Steuerelement eine Reihe von Formatvorlagen bezogenen Eigenschaften, die die Darstellung zu beeinflussen. Insbesondere wurde erläutert, wie standardmäßige CSS-Klassen an die Datenliste s weisen `HeaderStyle`, `ItemStyle`, `AlternatingItemStyle`, und `SelectedItemStyle` Eigenschaften. Zusätzlich zu diesen vier Eigenschaften, umfasst DataList-Steuerelement eine Reihe von anderen auf Formatvorlagen bezogenen Steuerelementeigenschaften, wie z. B. `Font`, `ForeColor`, `BackColor`, und `BorderWidth`, um nur einige zu nennen. Das Repeater-Steuerelement enthält keine Formatvorlagen bezogenen Eigenschaften. Alle diese formateinstellungen müssen direkt in das Markup in den Repeater-s-Vorlagen vorgenommen werden.

Oft, hängt wie die Daten formatiert werden soll jedoch von den Daten selbst. Z. B. beim Auflisten von Produkten wir möchten die Produktinformationen in einen hellen grauen Schrift angezeigt, wenn es nicht mehr unterstützt wird, oder wir hervorheben möchten die `UnitsInStock` -Wert, wenn es sich um 0 (null) ist. Wie wir in vorherigen Tutorials gesehen haben, bieten die GridView, DetailsView und FormView-Steuerelement zwei unterschiedliche Möglichkeiten, um ihre Darstellung auf der Grundlage ihrer Daten zu formatieren:

- **Die `DataBound` Ereignis** erstellen Sie einen Ereignishandler für das entsprechende `DataBound` Ereignis, das ausgelöst wird, nachdem die Daten zu jedem Element gebunden wurde (für die GridView war die `RowDataBound` Ereignis DataList- oder Wiederholungssteuerelement ist die `ItemDataBound`Ereignis). In diesem Fall Handler auf, die gebundenen Daten nur untersucht werden kann und formatierungsentscheidungen vorgenommen. Untersuchten wir diese Vorgehensweise in der [benutzerdefinierte Formatierung basierend auf Daten](../custom-formatting/custom-formatting-based-upon-data-vb.md) Tutorial.
- **Formatieren von Funktionen in Vorlagen** bei Verwendung von TemplateFields in DetailsView oder GridView-Steuerelemente oder einer Vorlage in das FormView-Steuerelement können wir eine Formatierung Funktion der ASP.NET Page s-Code-Behind-Klasse, die Geschäftslogikschicht oder eines hinzufügen andere Klassenbibliothek, die von der Webanwendung zugegriffen werden kann. Diese Formatierung Funktion lässt eine beliebige Anzahl von Eingabeparametern, jedoch muss den HTML-Code zum Rendern in der Vorlage zurück. Formatierungsoptionen wurden zuerst in untersucht die [Verwenden von TemplateFields, im GridView-Steuerelement](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) Tutorial.

Beide Techniken Formatierung sind verfügbar, mit dem DataList- und Wiederholungssteuerelement-Steuerelementen. In diesem Tutorial werden wir Beispiele für die Verwendung der beiden Techniken für beide Steuerelemente durchlaufen.

## <a name="using-theitemdataboundevent-handler"></a>Mithilfe der`ItemDataBound`-Ereignishandler

Wenn Daten gebunden ist, einem DataList-Steuerelement, ein Datenquellen-Steuerelement oder über die Daten programmgesteuert zuweisen, auf das Steuerelement s `DataSource` -Eigenschaft ab, und rufen die `DataBind()` -Methode, DataList-Steuerelement s `DataBinding` Ereignis ausgelöst wird, die Datenquelle aufgelistet, und jeder Datensatz an die Datenliste gebunden ist. Für jeden Datensatz in der Datenquelle, DataList-Steuerelement erstellt eine [ `DataListItem` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistitem.aspx) -Standardobjekt ab, und klicken Sie dann auf den aktuellen Datensatz gebunden. Während dieses Vorgangs löst DataList-Steuerelement zwei Ereignisse:

- **`ItemCreated`** wird ausgelöst, nachdem die `DataListItem` erstellt wurde
- **`ItemDataBound`** wird ausgelöst, nachdem der aktuelle Datensatz an gebunden wurde die `DataListItem`

Die folgenden Schritte beschreiben den Prozess der Datenbindung für das DataList-Steuerelement.

1. DataList-Steuerelement s [ `DataBinding` Ereignis](https://msdn.microsoft.com/library/system.web.ui.control.databinding.aspx) ausgelöst wird
2. Die Daten werden an die Datenliste gebunden.  
  
   Für jeden Datensatz in der Datenquelle 

    1. Erstellen Sie eine `DataListItem` Objekt
    2. Auslösen der [ `ItemCreated` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.itemcreated.aspx)
    3. Binden Sie den Datensatz auf der `DataListItem`
    4. Auslösen der [ `ItemDataBound` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.itemdatabound.aspx)
    5. Hinzufügen der `DataListItem` auf die `Items` Auflistung

Beim Binden von Daten an das Repeater-Steuerelement, durchläuft sie genau dieselbe Sequenz von Schritten. Der einzige Unterschied ist, dass statt des `DataListItem` Instanzen, die erstellt wird, der Repeater verwendet [ `RepeaterItem` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeateritem(VS.80).aspx)s.

> [!NOTE]
> Der aufmerksame Leser eine leichte Anomalie zwischen die Abfolge der Schritte haben möglicherweise bemerkt, das eintritt, wenn dem DataList- und Wiederholungssteuerelement an Daten im Vergleich zu gebunden sind, wenn die GridView an Daten gebunden ist. Am Ende des Protokollfragments der dem Prozess der Datenbindung, GridView löst die `DataBound` Ereignis jedoch weder die Repeater das DataList-Steuerelement einem solchen haben. Dies ist, weil die DataList- oder Repeater-Steuerelemente wieder in den ASP.NET 1.x-Zeitrahmen erstellt wurden, bevor das vorab und nachträglich auf Ereignis-Muster "Meldungshandler" häufiger eingesetzt werden mussten.


Mit GridView ist eine Option für die Formatierung basierend auf den Daten, erstellen Sie einen Ereignishandler für die `ItemDataBound` Ereignis. Dieser Ereignishandler würde überprüfen Sie die Daten, die nur auf gebunden wurde, hatte der `DataListItem` oder `RepeaterItem` und Auswirkungen auf die Formatierung des Steuerelements nach Bedarf.

Formatieren von Änderungen für das DataList-Steuerelement, das gesamte Element implementiert werden kann, mit der `DataListItem` s Formatvorlagen bezogenen Eigenschaften, die den Standard enthalten `Font`, `ForeColor`, `BackColor`, `CssClass`und so weiter. Um die Formatierung von bestimmten Websteuerelementen innerhalb der DataList-s-Vorlage zu beeinflussen, müssen wir programmgesteuerten Zugriff auf und Ändern des Stils von dieser Web-Steuerelemente. Erläutert, wie diese zurück in die *benutzerdefinierte Formatierung basierend auf Daten* Tutorial. Repeater-Steuerelement, wie die `RepeaterItem` -Klasse verfügt über keine Formatvorlagen bezogenen Eigenschaften; aus diesem Grund alle Formatvorlagen bezogenen Änderungen an einer `RepeaterItem` in die `ItemDataBound` Ereignishandler muss erfolgen, indem Sie programmgesteuert zugreifen und diese aktualisieren Websteuerelemente in die Vorlage.

Da die `ItemDataBound` Formatierung von Verfahren für die dem DataList- und Wiederholungssteuerelement nahezu identisch sind, in unserem Beispiel konzentriert sich auf die mit DataList-Steuerelement.

## <a name="step-1-displaying-product-information-in-the-datalist"></a>Schritt 1: Anzeigen von Produktinformationen im DataList-Steuerelement

Bevor wir die Formatierung kümmern, Let s zunächst erstellen eine Seite, die einem DataList-Steuerelement verwendet wird, um Produktinformationen anzuzeigen. In der [vorherigen Tutorial](displaying-data-with-the-datalist-and-repeater-controls-vb.md) wir einem DataList-Steuerelement erstellt, deren `ItemTemplate` angezeigt wird, jede s, Produktname, die Kategorie, die Lieferanten, die Menge pro Einheit und Preis. Lassen Sie s, die diese Funktionalität wird hier in diesem Tutorial zu wiederholen. Zu diesem Zweck erstellen Sie entweder DataList-Steuerelement und seine "ObjectDataSource" von Grund auf neu, oder Sie können über die Steuerelemente auf der Seite, die im vorherigen Tutorial erstellte kopieren (`Basics.aspx`) und fügen Sie sie auf der Seite für dieses Tutorial (`Formatting.aspx`).

Nachdem Sie die Funktionalität DataList-Steuerelement und "ObjectDataSource" repliziert haben `Basics.aspx` in `Formatting.aspx`, DataList-Steuerelement s ändern in Ruhe `ID` Eigenschaft aus `DataList1` um einen aussagekräftigeren `ItemDataBoundFormattingExample`. Als Nächstes werden DataList-Steuerelement in einem Browser anzeigen. Wie in Abbildung 1 gezeigt, ist der einzige Formatierung Unterschied zwischen jedes Produkt, die Farbe des Hintergrunds wechselt.


[![Die Produkte sind in dem DataList-Steuerelement aufgeführt.](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image2.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image1.png)

**Abbildung 1**: der Produkte finden Sie in das DataList-Steuerelement ([klicken Sie, um das Bild in voller Größe anzeigen](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image3.png))


In diesem Tutorial können Sie s DataList-Steuerelement formatieren, sodass alle Produkte mit einem Preis kleiner-als-20,00 $pro sowohl der Name müssen und Unit price gelb hervorgehoben.

## <a name="step-2-programmatically-determining-the-value-of-the-data-in-the-itemdatabound-event-handler"></a>Schritt 2: Programmgesteuertes bestimmen den Wert der Daten in den ItemDataBound-Ereignishandler

Da nur die Produkte mit einem Preis unter 20,00 $pro wird die benutzerdefinierte Formatierung angewendet haben, müssen wir jede s Produktpreis bestimmen können. Beim Binden von Daten an einem DataList-Steuerelement, DataList-Steuerelement zählt die Datensätze in der Datenquelle und für die einzelnen Datensätze erstellt eine `DataListItem` Instanz, die den Datensatz der Datenquelle zum Binden der `DataListItem`. Nach den entsprechenden Datensatz s Daten gebunden wurde mit dem aktuellen `DataListItem` -Objekt, das DataList s `ItemDataBound` Ereignis wird ausgelöst. Erstellen wir einen Ereignishandler für dieses Ereignis, um die Datenwerte für die aktuelle überprüfen `DataListItem` und, basierend auf diesen Werten, Änderungen vornehmen, Formatierungen erforderlich.

Erstellen Sie eine `ItemDataBound` -Ereignis für DataList-Steuerelement und fügen Sie den folgenden Code hinzu:


[!code-vb[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample1.vb)]

Während das Konzept und die Semantik hinter dem DataList-Steuerelement s `ItemDataBound` Ereignishandler sind identisch mit denen ein, die das GridView-s `RowDataBound` -Ereignishandler in der *benutzerdefinierte Formatierung basierend auf Daten* Tutorial, um die Syntax unterscheidet etwas. Wenn die `ItemDataBound` Ereignis wird ausgelöst, die `DataListItem` nur an Daten gebunden wird an die entsprechenden Ereignishandler über übergeben `e.Item` (anstelle von `e.Row`entspricht der Vorgehensweise bei der GridView-s `RowDataBound` -Ereignishandler). DataList-Steuerelement s `ItemDataBound` Ereignishandler ausgelöst wird, für die *jedes* Zeile DataList-Steuerelement, einschließlich Kopfzeilen, Fußzeilen und Trennzeichen für Zeilen hinzugefügt. Die Produktinformationen ist jedoch nur auf die Datenzeilen gebunden. Aus diesem Grund bei Verwendung der `ItemDataBound` Ereignis, um die Daten zu überprüfen, die an DataList-Steuerelement gebunden werden, müssen wir zuerst sicherstellen, dass es neu mit einem Datenelement arbeiten. Dies kann erreicht werden, indem Sie überprüfen die `DataListItem` s [ `ItemType` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistitem.itemtype.aspx), die einen der enthalten kann [die folgenden acht Werte](https://msdn.microsoft.com/library/system.web.ui.webcontrols.listitemtype.aspx):

- `AlternatingItem`
- `EditItem`
- `Footer`
- `Header`
- `Item`
- `Pager`
- `SelectedItem`
- `Separator`

Beide `Item` und `AlternatingItem``DataListItem` s Zusammensetzung der DataList-s-Datenelemente. Wenn wir erneut die Arbeit mit einer `Item` oder `AlternatingItem`, wir Zugriff auf die tatsächliche `ProductsRow` -Instanz, die mit dem aktuellen gebunden war `DataListItem`. Die `DataListItem` s [ `DataItem` Eigenschaft](https://msdn.microsoft.com/system.web.ui.webcontrols.datalistitem.dataitem.aspx) enthält einen Verweis auf die `DataRowView` Objekt, dessen `Row` Eigenschaft stellt einen Verweis auf die tatsächliche `ProductsRow` Objekt.

Als Nächstes überprüfen wir die `ProductsRow` s-Instanz `UnitPrice` Eigenschaft. Seit der Tabelle "Products" s `UnitPrice` Feld ermöglicht `NULL` Werte, bevor Sie versuchen, den Zugriff auf die `UnitPrice` Eigenschaft wir sollten zunächst überprüfen, festzustellen, ob sie verfügt über eine `NULL` -Wert mithilfe der `IsUnitPriceNull()` Methode. Wenn die `UnitPrice` Wert ist kein `NULL`, wir dann prüfen um festzustellen, ob es kleiner als 20,00 $pro. Wenn es tatsächlich unter 20,00 $ ist, müssen wir die benutzerdefinierte Formatierung anzuwenden.

## <a name="step-3-highlighting-the-product-s-name-and-price"></a>Schritt 3: Markieren, die Produkt-s-Namen und Preise

Sobald wir wissen, dass ein Produktpreis s weniger als 20,00 $ ist, übrig bleibt, markieren Sie den Namen und den Preis. Um dies zu erreichen, wir müssen zunächst programmgesteuert verweisen die Label-Steuerelemente in der `ItemTemplate` , von denen Produktname s und Preis angezeigt. Als Nächstes benötigen wir diese einen gelben Hintergrund angezeigt. Diese Formatierungsinformationen kann angewendet werden, durch direktes Bearbeiten der Bezeichnungen `BackColor` Eigenschaften (`LabelID.BackColor = Color.Yellow`), im Idealfall jedoch alle Angelegenheiten im Zusammenhang mit Anzeige durch cascading Stylesheets ausgedrückt werden. Tatsächlich haben wir bereits ein Stylesheet, die bereitstellt, die gewünschte Formatierung in definierten `Styles.css`  -  `AffordablePriceEmphasis`, die erstellt wurde, und erläutert, die der *benutzerdefinierte Formatierung basierend auf Daten* Tutorial.

Wenn die Formatierung angewendet werden soll, legen Sie einfach die zwei Label-Websteuerelemente `CssClass` Eigenschaften `AffordablePriceEmphasis`, wie im folgenden Code gezeigt:


[!code-vb[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample2.vb)]

Mit der `ItemDataBound` -Ereignishandler ausgeführt, rufen Sie erneut die `Formatting.aspx` Seite in einem Browser. Wie Abbildung 2 veranschaulicht, müssen diese Produkte mit einem Preis unter 20,00 $pro, ihren Namen und den Preis hervorgehoben.


[![Diese Produkte kleiner als 20,00 $pro hervorgehoben sind](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image5.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image4.png)

**Abbildung 2**: Diese Produkte kleiner als 20,00 $pro werden hervorgehoben ([klicken Sie, um das Bild in voller Größe anzeigen](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image6.png))


> [!NOTE]
> Da DataList-Steuerelement als HTML gerendert wird `<table>`, dessen `DataListItem` Instanzen verfügen über Formatvorlagen bezogenen Eigenschaften, die festgelegt werden, um ein bestimmtes Format für das gesamte Element gelten. Angenommen, wir wollten, markieren Sie die *gesamte* Element Gelb, wenn der Preis von weniger als 20,00 $ war, wir könnten wurden ersetzt den Code, der die Bezeichnungen auf die verwiesen wird, und legen ihre `CssClass` Eigenschaften mit der folgenden Zeile des Codes: `e.Item.CssClass = "AffordablePriceEmphasis"` (siehe Abbildung 3).


Die `RepeaterItem` s, aus denen das Repeater-Steuerelement, allerdings Don t bieten solche Eigenschaften Format auf. Daher erfordert die benutzerdefinierte Formatierung auf das Repeater das Anwenden von Stileigenschaften auf die Web-Steuerelemente in den Vorlagen Repeater s nur wie in Abbildung 2.


[![Das gesamte Produkt-Element ist für Produkte unter 20,00 $pro hervorgehoben.](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image8.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image7.png)

**Abbildung 3**: das gesamte Produkt-Element wird für die Produkte unter 20,00 $pro hervorgehoben ([klicken Sie, um das Bild in voller Größe anzeigen](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image9.png))


## <a name="using-formatting-functions-from-within-the-template"></a>Verwenden die Formatierungsfunktionen von innerhalb der Vorlage

In der *Verwenden von TemplateFields, im GridView-Steuerelement* Tutorial erläutert, wie eine Formatierung Funktion innerhalb einer GridView TemplateField verwenden, wenden Sie benutzerdefinierte Formatierung auf Grundlage der Daten, die an die GridView-s-Zeilen gebunden. Eine Formatierung-Funktion ist eine Methode, die aus einer Vorlage kann aufgerufen werden, und gibt den HTML-Code, an seiner Stelle ausgegeben werden soll. Formatierungsoptionen können in der CodeBehind-Klasse in ASP.NET Seite s gespeichert werden oder zentralisiert werden kann, in Klassendateien in die `App_Code` Ordner oder in einem separaten Klassenbibliotheksprojekt. Verschieben die Formatierungsfunktion aus der ASP.NET Page-s-Code-Behind-Klasse ist ideal, wenn Sie die gleiche Funktion für die Formatierung verwenden, auf mehreren Seiten von ASP.NET oder in anderen ASP.NET-Webanwendungen möchten.

Um die Formatierungsoptionen zu demonstrieren, haben können s die Produktinformationen, die den Text [DISCONTINUED] neben dem Produktnamen s enthalten, wenn es s nicht mehr unterstützt. Darüber hinaus können s hervorgehobenen gelben Preis If haben es kleiner als 20,00 $pro (wie der `ItemDataBound` Beispiel für einen Ereignishandler); Wenn der Preis 20,00 $pro ist oder höher, Let s nicht den tatsächlichen Preis angezeigt, aber stattdessen rufen Sie der Text ein, geben Sie für eine Preisinformationen. Abbildung 4 zeigt einen Screenshot, der die Produkte, die Liste, die mit diesen Regeln für die Formatierung angewendet.


[![Für die teuersten Produkte wird der Preis mit dem Text, rufen Sie für eine Preisinformationen ersetzt.](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image11.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image10.png)

**Abbildung 4**: der Preis wird für teuersten Produkte, mit dem Text, rufen Sie für eine Preisinformationen ersetzt ([klicken Sie, um das Bild in voller Größe anzeigen](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image12.png))


## <a name="step-1-create-the-formatting-functions"></a>Schritt 1: Erstellen Sie die Formatierungsfunktionen

In diesem Beispiel wir benötigen zwei Formatierungsfunktionen, eine, die den Namen des Produkts zusammen mit dem Text [DISCONTINUED], zeigt an, bei Bedarf und eine, die entweder einen hervorgehobenen Preis angezeigt, wenn es kleiner als 20,00 $pro oder den Text ein, rufen Sie für eine andernfalls Preisinformationen. Lassen Sie s, die diese Funktionen in der ASP.NET Seite s-Code-Behind-Klasse erstellen, und nennen Sie diese `DisplayProductNameAndDiscontinuedStatus` und `DisplayPrice`. Beide Methoden müssen als Zeichenfolge Rendern den HTML-Code zurückgeben, und beide müssen markiert werden `Protected` (oder `Public`) um ab der ASP.NET Seite s deklarative Syntax aufgerufen werden. Der Code für diese beiden Methoden folgt:


[!code-vb[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample3.vb)]

Beachten Sie, dass die `DisplayProductNameAndDiscontinuedStatus` Methode akzeptiert die Werte von der `productName` und `discontinued` Daten von Feldern nach Skalarwerte, während die `DisplayPrice` -Methode akzeptiert eine `ProductsRow` Instanz (anstelle eines `unitPrice` Skalarwert). Beide Ansätze funktioniert. allerdings, wenn die Formatierungsfunktion arbeitet mit skalaren Werten, die Datenbank enthalten kann `NULL` Werte (z. B. `UnitPrice`; weder `ProductName` noch `Discontinued` ermöglichen `NULL` Werte), besondere Vorsicht bei der Verarbeitung dieser Skalare Eingaben.

Insbesondere muss der Eingabeparameter des Typs `Object` da der eingehende Wert möglicherweise einen `DBNull` Instanz statt mit den erwarteten Datentyp. Darüber hinaus eine Überprüfung muss erfolgen, zu bestimmen, ob der eingehende Wert einer Datenbank ist `NULL` Wert. D. h. wenn wir wollten die `DisplayPrice` Methode, um den Preis als einen skalaren Wert an, wir d akzeptiert haben, verwenden Sie den folgenden Code:


[!code-vb[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample4.vb)]

Beachten Sie, dass die `unitPrice` Eingabeparameter ist vom Typ `Object` und, die die Auswertung der Anweisung wurde geändert, um Wenn ermitteln `unitPrice` ist `DBNull` oder nicht. Darüber hinaus, da die `unitPrice` Eingabeparameter wird als übergeben eine `Object`, es muss ein decimal-Wert umgewandelt werden.

## <a name="step-2-calling-the-formatting-function-from-the-datalist-s-itemtemplate"></a>Schritt 2: Aufrufen der Formatierungsfunktion aus ItemTemplate DataList s

Mit die Formatierungen Funktionen unserer ASP.NET Seite s Code-Behind-Klasse hinzugefügt die, übrig bleibt, rufen diese Funktionen aus DataList-Steuerelement s Formatierung `ItemTemplate`. Um eine Formatierung Funktion aus einer Vorlage zu aufzurufen, platzieren Sie den Aufruf der Funktion in die Databinding-Syntax:


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample5.aspx)]

Im DataList-Steuerelement s `ItemTemplate` der `ProductNameLabel` Bezeichnung Websteuerelement derzeit zeigt den Namen des Produkts s durch Zuweisen der `Text` Eigenschaft das Ergebnis der `<%# Eval("ProductName") %>`. Damit diese den Namen und den Text [DISCONTINUED], anzeigen, wenn benötigt, aktualisieren Sie die deklarative Syntax, damit er stattdessen weist der `Text` Eigenschaft ist der Wert von der `DisplayProductNameAndDiscontinuedStatus` Methode. Dabei müssen wir übergeben, in der Produktname s und nicht mehr unterstützte Werte, die mit der `Eval("columnName")` Syntax. `Eval` Gibt einen Wert vom Typ `Object`, aber die `DisplayProductNameAndDiscontinuedStatus` Methode erwartet Parameter vom Typ `String` und `Boolean`; aus diesem Grund müssen wir die Rückgabewerte Umwandeln der `Eval` Methode, um die Typen Erwarteter Eingabeparameter wie folgt:


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample6.aspx)]

Wir können einfach festlegen, um den Preis anzuzeigen, die `UnitPriceLabel` Bezeichnung s `Text` Eigenschaft, um den Rückgabewert von der `DisplayPrice` -Methode, wie wir für die Anzeige der Name des Produkts s haben und [] Text eingestellt. Allerdings nicht in der `UnitPrice` als skalare Eingabeparameter, wir stattdessen übergeben Sie in der gesamten `ProductsRow` Instanz:


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample7.aspx)]

Können Sie mit den Aufrufen für die Formatierungen Funktionen vorhanden unseren Fortschritt in einem Browser anzeigen. Ihr Bildschirm sollte ähnlich wie in Abbildung 5 aussehen, die nicht mehr unterstützte Produkte, die auch den Text [DISCONTINUED] und dieser Produkte, die mehr als 20,00 $pro müssen ihren Preis Kosten durch den Text ersetzt Aufruf für eine Preisinformationen.


[![Für die teuersten Produkte wird der Preis mit dem Text, rufen Sie für eine Preisinformationen ersetzt.](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image14.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image13.png)

**Abbildung 5**: der Preis wird für teuersten Produkte, mit dem Text, rufen Sie für eine Preisinformationen ersetzt ([klicken Sie, um das Bild in voller Größe anzeigen](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image15.png))


## <a name="summary"></a>Zusammenfassung

Formatieren den Inhalt eines DataList- oder Repeater-Steuerelements, die auf Grundlage der Daten kann mithilfe von zwei Techniken erreicht werden. Die erste Technik ist, erstellen Sie einen Ereignishandler für die `ItemDataBound` Ereignis, das ausgelöst wird, wie jeder Datensatz in der Datenquelle, um ein neues gebunden ist `DataListItem` oder `RepeaterItem`. In der `ItemDataBound` -Ereignishandler die aktuellen Artikel s Daten untersucht werden können, und klicken Sie dann das Formatieren angewendet werden auf den Inhalt der Vorlage oder für `DataListItem` s, auf das gesamte Element selbst.

Alternativ kann benutzerdefinierte Formatierung realisiert werden, durch das Formatieren von Funktionen. Eine Formatierung Funktion ist eine Methode, die über DataList-Steuerelement aufgerufen werden kann oder Repeater-s-Vorlagen, die den HTML-Code ausgeben an seiner Stelle zurückgibt. Der HTML-Code von einer Formatierung Funktion zurückgegebenen wird häufig durch die Werte gebunden wird, auf das aktuelle Element bestimmt. Diese Werte können in der Formatierungsfunktion übergeben werden, entweder als skalare Werte oder durch das gesamte Objekt gebunden wird, auf das Element übergeben (wie z. B. die `ProductsRow` Instanz).

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial wurden Yaakov Ellis Randy Schmidt und Liz Shulok. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](displaying-data-with-the-datalist-and-repeater-controls-vb.md)
> [Weiter](showing-multiple-records-per-row-with-the-datalist-control-vb.md)
