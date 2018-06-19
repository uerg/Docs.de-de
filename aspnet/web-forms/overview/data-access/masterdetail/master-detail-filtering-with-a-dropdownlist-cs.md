---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-with-a-dropdownlist-cs
title: Master/Detail-Filtern mit einer DropDownList (c#) | Microsoft Docs
author: rick-anderson
description: In diesem Lernprogramm sehen wir, wie die Masterdatensätze in die Details des ausgewählten Listenelements in einem GridView und eines DropDownList-Steuerelements angezeigt werden.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 53e659cc-eefb-40c1-a1dc-559481c99443
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-with-a-dropdownlist-cs
msc.type: authoredcontent
ms.openlocfilehash: 42a6a76b0b05045bed1ada227b7c32a51600b760
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30880634"
---
<a name="masterdetail-filtering-with-a-dropdownlist-c"></a>Master/Detail-Filtern mit einer DropDownList (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunterladen](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_7_CS.exe) oder [PDF herunterladen](master-detail-filtering-with-a-dropdownlist-cs/_static/datatutorial07cs1.pdf)

> In diesem Lernprogramm sehen wir, wie die Masterdatensätze in die Details des ausgewählten Listenelements in einem GridView und eines DropDownList-Steuerelements angezeigt werden.


## <a name="introduction"></a>Einführung

Kein gemeinsamer Typ des Berichts ist die *Master/Detail-Bericht*, in dem der Bericht Bericht enthält einen Satz mit "Masterdatensätze" beginnt. Der Benutzer kann dann einen der master Datensätze Drilldown in und Anzeigen von diesen master Datensatz "Details." Master/Detail-Berichte sind eine ideale Option zum Visualisieren von 1: n-Beziehungen, z. B. einen Bericht alle Kategorien anzeigen und klicken Sie dann ermöglichen einen Benutzer wählen Sie eine bestimmte Kategorie und seine zugehörigen Produkte angezeigt. Darüber hinaus eignen sich Master/Detail-Berichten zum Anzeigen ausführlicher Informationen aus besonders "Breite" Tabellen (Argumente, die viele Spalten haben). Beispielsweise der "master" Maß an einer Master/Detail-Bericht möglicherweise nur den Namen und die Einheit Produktpreis der Produkte in der Datenbank anzeigen und Drilldowns in ein bestimmtes Produkt würde Felder anzeigen, die zusätzliche Produkt (Kategorie "," Supplier "," Menge pro Einheit, und usw.).

Es gibt viele Möglichkeiten, die mit denen ein Master/Detail-Bericht implementiert werden kann. Über diese und die nächsten drei Lernprogramme sehen wir uns eine Vielzahl von Master/Detail-Berichten. In diesem Lernprogramm sehen wir, Vorgehensweise beim Anzeigen von Masterdatensätze in einem [DropDownList-Steuerelement](https://msdn.microsoft.com/library/dtx91y0z.aspx) und die Details des ausgewählten Listenelements in einem GridView. Insbesondere werden in diesem Lernprogramm Master/Detail-Bericht, Kategorie und Produktinformationen aufgelistet.

## <a name="step-1-displaying-the-categories-in-a-dropdownlist"></a>Schritt 1: Anzeigen der Kategorien in einer DropDownList

Unsere Master/Detail-Bericht listet die Kategorien in einer DropDownList mit dem ausgewählten Listenelement Produkte angezeigt weiter unten auf der Seite in einem GridView. Anschließend werden die erste Aufgabe vor uns, haben die Kategorien, die in einer Dropdownliste angezeigt. Öffnen der `FilterByDropDownList.aspx` auf der Seite der `Filtering` Ordner, ziehen Sie DropDownList aus der Toolbox auf die Seite-Designer, und legen Sie dessen `ID` Eigenschaft `Categories`. Klicken Sie dann auf den Link Datenquelle auswählen, aus der DropDownList smart Tag auf. Dadurch wird das Konfigurieren von Datenquellen-Assistenten angezeigt.


[![Angeben der DropDownList-Datenquelle](master-detail-filtering-with-a-dropdownlist-cs/_static/image2.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image1.png)

**Abbildung 1**: Angeben der DropDownList-Datenquelle ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-with-a-dropdownlist-cs/_static/image3.png))


Wählen Sie zum Hinzufügen einer neuen ObjectDataSource mit dem Namen `CategoriesDataSource` aufruft, die die `CategoriesBLL` Klasse `GetCategories()` Methode.


[![Fügen Sie eine neue, mit dem Namen CategoriesDataSource ObjectDataSource hinzu](master-detail-filtering-with-a-dropdownlist-cs/_static/image5.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image4.png)

**Abbildung 2**: Hinzufügen einer neuen ObjectDataSource namens `CategoriesDataSource` ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-with-a-dropdownlist-cs/_static/image6.png))


[![Wählen Sie die Klasse CategoriesBLL verwendet](master-detail-filtering-with-a-dropdownlist-cs/_static/image8.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image7.png)

**Abbildung 3**: Wählen Sie zum Verwenden der `CategoriesBLL` Klasse ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-with-a-dropdownlist-cs/_static/image9.png))


[![Konfigurieren der ObjectDataSource zur Verwendung der GetCategories()-Methode](master-detail-filtering-with-a-dropdownlist-cs/_static/image11.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image10.png)

**Abbildung 4**: Konfigurieren der ObjectDataSource verwenden die `GetCategories()` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-with-a-dropdownlist-cs/_static/image12.png))


Nach dem Konfigurieren der ObjectDataSource müssen wir noch angeben, welche Datenquellenfeld in DropDownList angezeigt werden soll und welche sollte einen als der Wert für das Listenelement zugeordnet ist. Haben die `CategoryName` -Felds ist, als die Anzeige und `CategoryID` als Wert für jedes Listenelement.


[![Weisen Sie die CategoryName-Feld und CategoryID verwenden der DropDownList-Anzeige auf, als Wert](master-detail-filtering-with-a-dropdownlist-cs/_static/image14.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image13.png)

**Abbildung 5**: haben Sie die Anzeige der DropDownList der `CategoryName` Feld und Verwendung `CategoryID` als Wert ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-with-a-dropdownlist-cs/_static/image15.png))


An diesem Punkt haben wir ein DropDownList-Steuerelement, das mit den Datensätzen aufgefüllt wird die `Categories` Tabelle (alle in ungefähr sechs Sekunden erreicht). Abbildung 6 zeigt unseren Fortschritt bisher ein, wenn Sie über einen Browser angezeigt.


[![Eine Dropdownliste werden die aktuellen Kategorien aufgelistet.](master-detail-filtering-with-a-dropdownlist-cs/_static/image17.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image16.png)

**Abbildung 6**: ein Dropdown-Listet die aktuellen Kategorien ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-with-a-dropdownlist-cs/_static/image18.png))


## <a name="step-2-adding-the-products-gridview"></a>Schritt 2: Hinzufügen von Produkten GridView

Der letzte Schritt in unserem Master/Detail-Bericht wird der Produkte in Zusammenhang mit der ausgewählten Kategorie aufgeführt. Klicken Sie zu diesem Zweck die Seite eine GridView hinzu, und erstellen Sie eine neue, mit dem Namen ObjectDataSource `productsDataSource`. Haben die `productsDataSource` Steuerelement Reduzieren der zugehörigen Daten aus der `ProductsBLL` Klasse `GetProductsByCategoryID(categoryID)` Methode.


[![Wählen Sie die GetProductsByCategoryID(categoryID)-Methode](master-detail-filtering-with-a-dropdownlist-cs/_static/image20.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image19.png)

**Abbildung 7**: Wählen Sie die `GetProductsByCategoryID(categoryID)` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-with-a-dropdownlist-cs/_static/image21.png))


Nachdem Sie diese Methode auswählen, der ObjectDataSource Assistent fordert uns für den Wert für der Methode *`categoryID`* Parameter. Verwenden Sie den Wert des ausgewählten `categories` DropDownList Element legen Sie als Quelle für die Parameter zum Steuern und die ControlID auf `Categories`.


[![Legen Sie die CategoryID-Parameter auf den Wert der DropDownList Kategorien](master-detail-filtering-with-a-dropdownlist-cs/_static/image23.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image22.png)

**Abbildung 8**: Festlegen der *`categoryID`* auf den Wert von der `Categories` DropDownList ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-with-a-dropdownlist-cs/_static/image24.png))


Nehmen Sie einen Moment Zeit, um unseren Fortschritt in einem Browser zu überprüfen. Wenn die Seite zuerst besuchen zu können, gehören dieser Produkte der ausgewählten Kategorie (Getränke) angezeigt werden, (wie in Abbildung 9 gezeigt), aber der DropDownList ändern nicht die Daten aktualisiert. Dies ist, da ein Postback, damit die GridView auftreten muss zu aktualisieren. Zu diesem Zweck haben wir zwei Möglichkeiten, die (nicht erforderlich, Code schreiben zu müssen):

- **Legen Sie der Kategorien DropDownList**[AutoPostBack-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.listcontrol.autopostback%28VS.80%29.aspx)**auf "true".** (Sie können dies durch Aktivieren der Option "AutoPostBack aktivieren" in der DropDownList-Smarttag.) Dadurch werden einen Postback ausgelöst, wenn der DropDownList ausgewählt Element vom Benutzer geändert wird. Daher, wenn der Benutzer eine neue Kategorie aus der Dropdownliste auswählt ein Postback wird Stellen Sie sicher, und GridView wird mit der Produkte für die neu ausgewählte Kategorie aktualisiert. (Dies ist der Ansatz, den ich in diesem Lernprogramm verwendet haben.)
- **Fügen Sie ein Websteuerelement auf Schaltfläche neben der DropDownList hinzu.** Legen Sie dessen `Text` Eigenschaft zu aktualisieren oder etwas Ähnliches. Bei diesem Ansatz muss der Benutzer wählen Sie eine neue Kategorie, und klicken Sie dann auf die Schaltfläche "". Auf die Schaltfläche wird dazu führen, dass einen Postback und aktualisieren die GridView, um die Produkte der ausgewählten Kategorie aufzulisten.

Abbildung 9 und 10 veranschaulichen den Master-/Detail-Bericht in Aktion.


[![Bei der ersten Seite besuchen, werden die Produkte Getränke angezeigt.](master-detail-filtering-with-a-dropdownlist-cs/_static/image26.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image25.png)

**Abbildung 9**: bei der ersten Seite besuchen, werden die Produkte Getränke angezeigt ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-with-a-dropdownlist-cs/_static/image27.png))


[![Durch Auswahl eines neuen Produkts (erzeugen) automatisch bewirkt, dass ein Postback handeln, die GridView aktualisieren](master-detail-filtering-with-a-dropdownlist-cs/_static/image29.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image28.png)

**Abbildung 10**: durch Auswahl eines neuen Produkts (erzeugen) automatisch bewirkt, dass ein Postback handeln, aktualisieren die GridView ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-with-a-dropdownlist-cs/_static/image30.png))


## <a name="adding-a----choose-a-category----list-item"></a>Hinzufügen eines Listenelements "– Wählen Sie eine Kategorie –"

Beim ersten Zugriff auf die `FilterByDropDownList.aspx` Seite die Kategorien DropDownLists erste Listenelement (Getränke) wird standardmäßig angezeigt, die mit den Produkten Getränke GridView ausgewählt ist. Statt mit Produkte für die erste Kategorie, wir stattdessen ein DropDownList-Element haben möchten, ausgewählt, sagt etwas wie "– Wählen Sie eine Kategorie--".

Um ein neues Listenelement DropDownList hinzuzufügen, wechseln Sie zu dem Eigenschaftenfenster, und klicken Sie auf die Auslassungszeichen in der `Items` Eigenschaft. Hinzufügen ein neuen Listenelements mit dem `Text` "– Wählen Sie eine Kategorie –" und die `Value` `-1`.


[![Hinzufügen einer – wählen Sie eine Kategorie – Listenelement](master-detail-filtering-with-a-dropdownlist-cs/_static/image32.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image31.png)

**Abbildung 11**: Hinzufügen einer – wählen Sie eine Kategorie – Listenelement ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-with-a-dropdownlist-cs/_static/image33.png))


Alternativ können Sie das Listenelement hinzufügen, indem Sie das folgende Markup DropDownList hinzufügen:

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-cs/samples/sample1.aspx)]

Darüber hinaus müssen wir die DropDownList-Steuerelement festgelegt `AppendDataBoundItems` auf "true", da bei die Kategorien von ObjectDataSource DropDownList gebunden sind alle manuell hinzugefügten Listenelemente Wenn überschreiben diese müssen `AppendDataBoundItems` ist nicht "true".


![Legen Sie die AppendDataBoundItems-Eigenschaft auf "true"](master-detail-filtering-with-a-dropdownlist-cs/_static/image34.png)

**Abbildung 12**: Legen Sie die `AppendDataBoundItems` Eigenschaft auf "true"


Nach der Änderung, wenn zuerst auf der Seite die Option "– Wählen Sie eine Kategorie –" ausgewählt ist und keine Produkte angezeigt werden.


[![Auf der ersten Seite sind keine Produkte angezeigt.](master-detail-filtering-with-a-dropdownlist-cs/_static/image36.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image35.png)

**Abbildung 13**: auf der ersten Seite laden Nein Produkte angezeigt werden ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-with-a-dropdownlist-cs/_static/image37.png))


Ist der Grund keine Produkte werden beim angezeigt, da das Listenelement "– Wählen Sie eine Kategorie –" ausgewählt ist, weil ihr Wert ist `-1` und es sind keine Produkte in der Datenbank mit einem `CategoryID` von `-1`. Ist dies das Verhalten werden soll und Sie an diesem Punkt fertig sind. Wenn jedoch angezeigt werden soll *alle* der Kategorien, wenn das Listenelement "– Wählen Sie eine Kategorie –" ausgewählt ist, zurück zu der `ProductsBLL` Klasse und Anpassen der `GetProductsByCategoryID(categoryID)` Methode so, dass die It Ruft die `GetProducts()` Methode Wenn Die übergebene in *`categoryID`* Parameter ist kleiner als 0 (null):

[!code-csharp[Main](master-detail-filtering-with-a-dropdownlist-cs/samples/sample2.cs)]

Die hier verwendete Technik ist vergleichbar mit der Vorgehensweise wird verwendet, um alle Lieferanten anzuzeigen zurück in die [deklarative Parameter](../basic-reporting/declarative-parameters-cs.md) Tutorial, obwohl in diesem Beispiel wir den Wert verwenden, `-1` , um anzugeben, dass alle Datensätze werden soll Im Gegensatz zu abgerufen `null`. Grund hierfür ist die *`categoryID`* Parameter von der `GetProductsByCategoryID(categoryID)` Methode erwartet als Integer-Wert übergeben, während im Lernprogramm deklarative Parameter in einen Zeichenfolgen-Eingabeparameter übergeben wurden.

Abbildung 14 zeigt einen Screenshot der `FilterByDropDownList.aspx` Wenn die Option "– Wählen Sie eine Kategorie –" aktiviert ist. Hier werden sämtliche Produkte in der Standardeinstellung angezeigt, und die Benutzer kann die Anzeige eingrenzen, indem Sie durch Auswahl einer bestimmten Kategorie.


[![Alle Produkte sind jetzt aufgeführten standardmäßig](master-detail-filtering-with-a-dropdownlist-cs/_static/image39.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image38.png)

**Abbildung 14**: alle Produkte sind jetzt aufgeführten standardmäßig ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-with-a-dropdownlist-cs/_static/image40.png))


## <a name="summary"></a>Zusammenfassung

Zum Anzeigen von hierarchisch produktbezogene Daten leichter häufig zum Präsentieren von Daten mithilfe von Master/Detail-Berichte, die der Benutzer kann aus denen die Daten aus der obersten Hierarchieebene beispieldatasets starten und Drilldown auf Details. In diesem Lernprogramm untersucht wir erstellen einen einfache Master/Detail-Bericht, der Produkte einer ausgewählten Kategorie anzeigt. Dies wurde mithilfe einer DropDownList für die Liste der Kategorien und einer GridView für die Produkte, die der ausgewählten Kategorie gehören.

In der [nächsten Lernprogramm](master-detail-filtering-with-two-dropdownlists-cs.md) wir führen die DropDownList-Schnittstelle einen Schritt weiter, mit zwei DropDownLists.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Nächste](master-detail-filtering-with-two-dropdownlists-cs.md)
