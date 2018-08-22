---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-with-a-dropdownlist-vb
title: Mit einem DropDownList-Steuerelement (VB) Filtern von Master-/Detailberichten | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial sehen wir, wie die master-Datensätze in einem DropDownList-Steuerelement und die Details der ausgewählten Listenelements in einer GridView-Ansicht angezeigt werden.
ms.author: riande
ms.date: 03/31/2010
ms.assetid: ea44717e-ab2e-46cd-a692-e4a9c0de194c
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-with-a-dropdownlist-vb
msc.type: authoredcontent
ms.openlocfilehash: d9d50da7f11d1494d49fbeaa18a45991e577cdb3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830359"
---
<a name="masterdetail-filtering-with-a-dropdownlist-vb"></a>Master/Detail-Filtern mit einem DropDownList-Steuerelement (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunter](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_7_VB.exe) oder [PDF-Datei herunterladen](master-detail-filtering-with-a-dropdownlist-vb/_static/datatutorial07vb1.pdf)

> In diesem Tutorial sehen wir, wie die master-Datensätze in einem DropDownList-Steuerelement und die Details der ausgewählten Listenelements in einer GridView-Ansicht angezeigt werden.


## <a name="introduction"></a>Einführung

Ist kein gemeinsamer Typ des Berichts die *Master/Detail-Berichts*, in dem der Bericht mit einem Satz von "master" Datensätze beginnt. Der Benutzer kann dann einen der master-Datensätze aufgliedern und Anzeigen des Datensatzes, master "." Master/Detail-Berichte sind eine ideale Option zum Visualisieren von 1: n Beziehungen, z. B. einen Bericht zeigt alle Kategorien an und klicken Sie dann ermöglicht einen Benutzer, wählen Sie eine bestimmte Kategorie und seine zugeordneten Produkte anzeigen. Darüber hinaus eignen sich Master/Detail-Berichte zum Anzeigen ausführlicher Informationen aus besonders "Breite" Tabellen (von denen viele Spalten). Z. B. die "master"-Ebene eines Master-/Detail-Berichts möglicherweise nur den Name und Unit Produktpreis der Produkte in der Datenbank angezeigt und Drilldown für ein bestimmtes Produkt würde Felder anzeigen, die zusätzliche Produkt (Kategorie "," Supplier "," Menge pro Einheit und usw.).

Es gibt viele Möglichkeiten, die mit denen ein Master/Detail-Berichts implementiert werden kann. Über diese und die nächsten drei Tutorials betrachten wir eine Vielzahl von Master/Detail-Berichte. In diesem Tutorial erfahren Sie, wie zum Anzeigen der master-Datensätze in einem [DropDownList-Steuerelement](https://msdn.microsoft.com/library/dtx91y0z.aspx) sowie Details zu dem ausgewählten Element in einer GridView-Ansicht. Insbesondere werden in diesem Lernprogramm die Master/Detail-Berichts, Kategorie und Produktinformationen aufgeführt.

## <a name="step-1-displaying-the-categories-in-a-dropdownlist"></a>Schritt 1: Anzeigen der Kategorien in einem DropDownList-Steuerelement

Unsere Master/Detail-Berichts Listet die Kategorien in einem DropDownList-Steuerelement, mit dem ausgewählten Listenelement Produkte angezeigt weiter unten auf der Seite in einer GridView-Ansicht. Anschließend werden die erste Aufgabe vor uns, haben die Kategorien, die in einem DropDownList-Steuerelement angezeigt. Öffnen der `FilterByDropDownList.aspx` auf der Seite die `Filtering` Ordner auf einem DropDownList-Steuerelement aus der Toolbox auf der Seite Designer ziehen, und legen Sie dessen `ID` Eigenschaft `Categories`. Klicken Sie anschließend auf den Link "Datenquelle auswählen" aus der Dropdownlistes Smarttag. Dadurch wird der Assistent zum Konfigurieren von Datenquellen angezeigt.


[![Geben Sie die DropDownList-Datenquelle](master-detail-filtering-with-a-dropdownlist-vb/_static/image2.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image1.png)

**Abbildung 1**: Geben Sie DropDownLists-Datenquelle ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-with-a-dropdownlist-vb/_static/image3.png))


Wählen Sie zum Hinzufügen einer neuen, mit dem Namen "ObjectDataSource" `CategoriesDataSource` aufruft, die die `CategoriesBLL` -Klasse `GetCategories()` Methode.


[![Fügen Sie eine neue, mit dem Namen CategoriesDataSource "ObjectDataSource"](master-detail-filtering-with-a-dropdownlist-vb/_static/image5.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image4.png)

**Abbildung 2**: Hinzufügen einer neuen "ObjectDataSource" mit dem Namen `CategoriesDataSource` ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-with-a-dropdownlist-vb/_static/image6.png))


[![Wählen Sie die Klasse CategoriesBLL verwendet](master-detail-filtering-with-a-dropdownlist-vb/_static/image8.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image7.png)

**Abbildung 3**: Wählen Sie zum Verwenden der `CategoriesBLL` Klasse ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-with-a-dropdownlist-vb/_static/image9.png))


[![Konfigurieren von dem ObjectDataSource-Steuerelement zur Verwendung der GetCategories()-Methode](master-detail-filtering-with-a-dropdownlist-vb/_static/image11.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image10.png)

**Abbildung 4**: Konfigurieren von dem ObjectDataSource-Steuerelement verwenden die `GetCategories()` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-with-a-dropdownlist-vb/_static/image12.png))


Nach dem Konfigurieren der müssen noch angeben, welche Datenquellenfeld in DropDownList angezeigt werden soll und welche "ObjectDataSource" sollte eine zugeordnet ist, als der Wert für das Element der Liste sein. Haben die `CategoryName` -Feld als die Anzeige und `CategoryID` als Wert für jedes Listenelement.


[![Haben Sie die DropDownList-Anzeige "CategoryName" Feld aus, und verwenden CategoryID als Wert](master-detail-filtering-with-a-dropdownlist-vb/_static/image14.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image13.png)

**Abbildung 5**: haben Sie die DropDownList-Anzeige der `CategoryName` Feld und die Verwendung `CategoryID` als Wert ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-with-a-dropdownlist-vb/_static/image15.png))


An diesem Punkt haben wir ein DropDownList-Steuerelement, das die Datensätze aus enthält die `Categories` Tabelle (alle erreicht, etwa sechs Sekunden). Abbildung 6 zeigt unseren Fortschritt bisher ein, wenn Sie über einen Browser angezeigt.


[![Eine Dropdown-Liste sind die aktuellen Kategorien aufgeführt.](master-detail-filtering-with-a-dropdownlist-vb/_static/image17.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image16.png)

**Abbildung 6**: ein Dropdownlisten der aktuellen Kategorien ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-with-a-dropdownlist-vb/_static/image18.png))


## <a name="step-2-adding-the-products-gridview"></a>Schritt 2: Hinzufügen der Produkte GridView

Der letzte Schritt in unserer Master/Detail-Berichts wird die ausgewählte Kategorie zugeordneten Produkte aufgeführt. Um dies zu erreichen, Hinzufügen einer GridView-Ansicht auf der Seite, und erstellen Sie eine neue, mit dem Namen "ObjectDataSource" `productsDataSource`. Haben die `productsDataSource` Steuerelement reduzieren, die Daten aus der die `ProductsBLL` Klasse `GetProductsByCategoryID(categoryID)` Methode.


[![Wählen Sie die GetProductsByCategoryID(categoryID)-Methode](master-detail-filtering-with-a-dropdownlist-vb/_static/image20.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image19.png)

**Abbildung 7**: Wählen Sie die `GetProductsByCategoryID(categoryID)` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-with-a-dropdownlist-vb/_static/image21.png))


Nach dem Auswählen dieser Methode an, der ObjectDataSource-Steuerelement-Assistent verlangt den Wert für die Methode *`categoryID`* Parameter. Der Wert des ausgewählten `categories` DropDownList-Element legen Sie die Quelle für die Parameter zum Steuern und die ControlID zu `Categories`.


[![Legen Sie die CategoryID-Parameter auf den Wert von DropDownList Kategorien](master-detail-filtering-with-a-dropdownlist-vb/_static/image23.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image22.png)

**Abbildung 8**: Legen Sie die *`categoryID`* Parameter, um den Wert des der `Categories` DropDownList ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-with-a-dropdownlist-vb/_static/image24.png))


Nehmen Sie einen Moment Zeit, unseren Fortschritt in einem Browser ausprobieren. Wenn die Seite zuerst besuchen zu können, werden diese Produkte der ausgewählten Kategorie gehören (Getränke) werden angezeigt (siehe Abbildung 9), aber der Dropdownliste ändern nicht die Daten aktualisiert. Dies ist, da ein Postback, damit die GridView auftreten muss zu aktualisieren. Zu diesem Zweck gibt es zwei Optionen zur Verfügung (die nicht erforderlich, Code schreiben zu müssen):

- **Legen Sie der Kategorien DropDownList**[AutoPostBack-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.listcontrol.autopostback%28VS.80%29.aspx)**auf "true".** (Sie erreichen dies durch Aktivieren der Option "AutoPostBack aktivieren" in der Dropdownlistes Smarttag.) Dadurch wird einen Postback ausgelöst, wenn DropDownLists ausgewählt Element vom Benutzer geändert wird. Aus diesem Grund, wenn der Benutzer eine neue Kategorie aus der Dropdownliste auswählt, wird ein Postback zur Folge haben und GridView mit den Produkten für die neu ausgewählte Kategorie aktualisiert werden. (Dies ist der Ansatz, den ich in diesem Tutorial verwendet haben.)
- **Fügen Sie ein Steuerelement Schaltfläche neben der Dropdownliste hinzu.** Legen Sie dessen `Text` -Eigenschaft, eine Aktualisierung oder etwas Ähnliches. Bei diesem Ansatz muss der Benutzer auf eine neue Kategorie auswählen, und klicken Sie dann auf die Schaltfläche. Klicken auf die Schaltfläche wird dazu führen, dass einen Postback, und aktualisieren die GridView, um die Produkte der ausgewählten Kategorie aufzulisten.

Abbildung 9 und 10 veranschaulichen die Master/Detail-Berichts in Aktion.


[![Wenn die Seite zuerst besuchen zu können, werden die trinken Produkte angezeigt.](master-detail-filtering-with-a-dropdownlist-vb/_static/image26.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image25.png)

**Abbildung 9**: die trinken-Produkte werden angezeigt, wenn die Seite zuerst besuchen zu können, ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-with-a-dropdownlist-vb/_static/image27.png))


[![Beim Auswählen eines neuen Produkts (erstellen) automatisch bewirkt, dass ein PostBack, und Aktualisieren der GridView](master-detail-filtering-with-a-dropdownlist-vb/_static/image29.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image28.png)

**Abbildung 10**: Auswählen eines neuen Produkts (generieren) werden auch automatisch bewirkt, dass ein PostBack, aktualisieren die GridView ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-with-a-dropdownlist-vb/_static/image30.png))


## <a name="adding-a----choose-a-category----list-item"></a>Hinzufügen eines Listenelements "– eine Kategorie auswählen:"

Beim ersten Zugriff auf die `FilterByDropDownList.aspx` Seite die Kategorien DropDownLists ersten Listenelements (Getränke) ist standardmäßig aktiviert und die trinken Produkte in den GridView-Ansicht angezeigt. Anstatt mit Produkten für die erste Kategorie, wir möchten möglicherweise stattdessen ein DropDownList-Element, das ausgewählt so etwas wie ": eine Kategorie auswählen:".

Um ein neues Listenelement DropDownList hinzuzufügen, wechseln Sie zu dem Fenster "Eigenschaften", und klicken Sie auf die Auslassungspunkte in der `Items` Eigenschaft. Hinzufügen ein neuen Listenelements mit dem `Text` "– eine Kategorie auswählen:" und die `Value` `-1`.


[![Hinzufügen einer – wählen Sie eine Kategorie – Listenelement](master-detail-filtering-with-a-dropdownlist-vb/_static/image32.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image31.png)

**Abbildung 11**: Hinzufügen einer – wählen Sie eine Kategorie – Listenelement ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-with-a-dropdownlist-vb/_static/image33.png))


Alternativ können Sie das Listenelement hinzufügen, indem Sie das folgende Markup DropDownList hinzufügen:


[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-vb/samples/sample1.aspx)]

Darüber hinaus müssen wir die DropDownList-Steuerelement festgelegt `AppendDataBoundItems` auf "true", da die Kategorien aus dem ObjectDataSource-Steuerelement zu DropDownList gebunden, gibt sie alle manuell hinzugefügten Listenelemente Wenn überschreiben müssen `AppendDataBoundItems` ist nicht "true".


![Legen Sie die AppendDataBoundItems-Eigenschaft auf "true"](master-detail-filtering-with-a-dropdownlist-vb/_static/image34.png)

**Abbildung 12**: Legen Sie die `AppendDataBoundItems` Eigenschaft auf "true"


Nach diesen Änderungen, wenn zunächst auf der Seite die Option "--eine Kategorie auswählen:" ausgewählt ist, und keine Produkte angezeigt werden.


[![Klicken Sie auf dem ersten Laden einer Seite werden keine Produkte angezeigt.](master-detail-filtering-with-a-dropdownlist-vb/_static/image36.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image35.png)

**Abbildung 13**: auf der ersten Seite laden keine Produkte angezeigt werden ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-with-a-dropdownlist-vb/_static/image37.png))


Der Grund, die keine Produkte angezeigt werden, da das Listenelement "– eine Kategorie auswählen:" ausgewählt ist, ist der Wert ist `-1` und sind keine Produkte in der Datenbank mit einem `CategoryID` von `-1`. Wenn dies das Verhalten ist, möchten Sie an diesem Punkt sind Sie bereits fertig! Wenn Sie jedoch, den Sie anzeigen möchten *alle* der Kategorien, wenn das Listenelement "– eine Kategorie auswählen:" ausgewählt ist, zurück zu den `ProductsBLL` Klasse und Anpassen der `GetProductsByCategoryID(categoryID)` Methode, sodass die It Ruft die `GetProducts()` Methode Wenn der übergebene in *`categoryID`* -Parameter ist kleiner als 0 (null):


[!code-vb[Main](master-detail-filtering-with-a-dropdownlist-vb/samples/sample2.vb)]

Die hier verwendete Technik ist ähnlich wie bei der es verwendet, um alle Lieferanten anzuzeigen zurück in die [deklarative Parameter](../basic-reporting/declarative-parameters-cs.md) Tutorial, obwohl in diesem Beispiel wir den Wert verwenden, `-1` , um anzugeben, dass alle Datensätze werden soll Im Gegensatz zu abgerufen `Nothing`. Grund hierfür ist die *`categoryID`* Parameter, der die `GetProductsByCategoryID(categoryID)` Methode erwartet, dass als Integer-Wert übergeben, während in diesem Tutorial deklarative Parameter in einen Zeichenfolgen-Eingabeparameter übergeben wurden.

Abbildung 14 zeigt einen Screenshot des `FilterByDropDownList.aspx` Wenn die Option "--eine Kategorie auswählen:" ausgewählt ist. Hier werden alle Produkte standardmäßig angezeigt, und die Benutzer kann die Anzeige eingrenzen, indem Sie die Auswahl einer bestimmten Kategorie.


[![Alle Produkte sind jetzt aufgeführt werden standardmäßig](master-detail-filtering-with-a-dropdownlist-vb/_static/image39.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image38.png)

**Abbildung 14**: alle Produkte sind jetzt aufgeführt werden standardmäßig ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-with-a-dropdownlist-vb/_static/image40.png))


## <a name="summary"></a>Zusammenfassung

Zum Anzeigen von hierarchisch-bezogenen Daten leichter häufig zur Darstellung der Daten mithilfe von Master/Detail-Berichte, aus denen kann der Benutzer starten darauf verwenden, die Daten aus der oben in der Hierarchie und einen Drilldown in die Details. In diesem Tutorial untersucht wir das Erstellen eines einfachen Master/Detail-Berichts, die Produkte einer ausgewählten Kategorie angezeigt. Dies wurde mithilfe einer DropDownList für die Liste der Kategorien und einer GridView für die Produkte, die der ausgewählten Kategorie gehören.

In der [nächsten Tutorial](master-detail-filtering-with-two-dropdownlists-vb.md) werfen wir einen Schritt der DropDownList-Schnittstelle darüber hinaus mit zwei DropDownList-Steuerelementen.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Zurück](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)
> [Weiter](master-detail-filtering-with-two-dropdownlists-vb.md)
