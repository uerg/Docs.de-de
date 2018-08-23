---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb
title: Master-/Detailbericht mit einer Aufzählung der Masterdatensätze und einem Details DataList-Steuerelement (VB) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial werden wir die zwei Seiten Master/Detail-Berichts des vorherigen Tutorials in einer einzelnen Seite Komprimieren einer Aufzählung der Namen der Kategorien auf t mit...
ms.author: riande
ms.date: 10/17/2006
ms.assetid: ee20742f-6fb7-49a0-a009-058fe363aacb
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb
msc.type: authoredcontent
ms.openlocfilehash: 3a10d6e5f60efad1f88c5acc8371a24dbf8d2cb7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825931"
---
<a name="masterdetail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb"></a>Master-/Detailbericht mit einer Aufzählung der Masterdatensätze und einem Details DataList-Steuerelement (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunter](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_35_VB.exe) oder [PDF-Datei herunterladen](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/datatutorial35vb1.pdf)

> In diesem Tutorial werden wir die zwei Seiten Master/Detail-Berichts des vorherigen Tutorials in einer einzelnen Seite komprimieren eine Aufzählung der Namen der Kategorien auf der linken Seite des Bildschirms und der ausgewählten Kategorie Produkte auf der rechten Seite des Bildschirms angezeigt.


## <a name="introduction"></a>Einführung

In der [vorherigen Lernprogramm](master-detail-filtering-acess-two-pages-datalist-vb.md) erläutert, wie Sie über zwei Seiten ein Master-/Detail-Berichts zu trennen. Auf der Masterseite verwendet haben wir ein Repeater-Steuerelement, um eine Aufzählung von Kategorien zu rendern. Jeder Kategoriename wurde ein Link, der beim Klicken auf würde Take der Benutzer auf die Seite "Details", in einem DataList-Steuerelement zwei Spalten, in denen dieser Produkte gezeigt gehört der ausgewählten Kategorie.

In diesem Tutorial werden wir das Tutorial zwei Seiten in einer einzelnen Seite komprimieren eine Aufzählung der Namen der Kategorien auf der linken Seite des Bildschirms angezeigt, mit jeder Kategorienamen, die als ein LinkButton gerendert. Klicken Sie auf eines der Kategoriename LinkButtons einen Postback verursacht, und bindet die Produkte der ausgewählten Kategorie s auf einem zweispalten-DataList-Steuerelement auf der rechten Seite des Bildschirms. Zusätzlich zur Anzeige der einzelnen Kategorienamen s, des Repeaters auf der linken Seite zeigt, wie viele insgesamt Produkte für eine angegebene Kategorie werden (siehe Abbildung 1).


[![Die Kategorie s Name und die gesamte Anzahl der Produkte werden auf der linken Seite angezeigt.](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image2.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image1.png)

**Abbildung 1**: die Kategorie-s-Name "und" Total Products-Anzahl werden angezeigt, auf der linken Seite ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image3.png))


## <a name="step-1-displaying-a-repeater-in-the-left-portion-of-the-screen"></a>Schritt 1: Anzeigen von einem Wiederholungssteuerelement im linken Bereich des Bildschirms

Für dieses Tutorial benötigen wir die Aufzählung der Kategorien auf der linken Seite der ausgewählten Kategorie s Produkte angezeigt werden. Inhalt auf einer Webseite kann positioniert werden, mithilfe von HTML-Elemente Absatz Standardtags, Leerzeichen, `<table>` s, usw. oder über Techniken für cascading Stylesheet (CSS). Alle unsere Tutorials haben bisher CSS-Techniken für die Positionierung verwendet. Wenn wir die Benutzeroberfläche für die Navigation erstellt, in die master-Seite in der [Masterseiten und Sitenavigation](../introduction/master-pages-and-site-navigation-vb.md) Tutorial verwendet *absolute Positionierung*, das genaue Pixel-Offset für die Navigation angeben Liste und den Hauptinhalt. Alternativ können CSS verwendet werden, um ein Element nach rechts oder links von einem anderen über positionieren *unverankerte*. Wir haben die Aufzählung der Kategorien auf der linken Seite der ausgewählten Kategorie s Produkte angezeigt werden, von floating Repeater auf der linken Seite des DataList-Steuerelement

Öffnen der `CategoriesAndProducts.aspx` Seite die `DataListRepeaterFiltering` Ordner und fügen Sie auf der Seite ein Repeater und einem DataList-Steuerelement hinzu. Festlegen des Repeaters s `ID` zu `Categories` und DataList-Steuerelement s zu `CategoryProducts`. Wechseln Sie zur Quellansicht, und fügen Sie die Steuerelemente Repeater- und das DataList-ASP.NET-Serversteuerelement innerhalb ihrer eigenen `<div>` Elemente. Einschließen, also den Repeater innerhalb einer `<div>` Element erste, und klicken Sie dann das DataList-Steuerelement in einem eigenen `<div>` Elements direkt hinter dem Repeater. Das Markup sollte jetzt etwa wie folgt aussehen:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample1.aspx)]

Um die Repeater auf der linken Seite des DataList-Steuerelement "float", müssen wir verwenden die `float` CSS-Style-Attribut, wie folgt:


[!code-html[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample2.html)]

Die `float: left;` : Verschiebt das erste `<div>` Element auf der linken Seite des zweiten. Die `width` und `padding-right` Einstellungen weisen darauf hin, die erste `<div>` s `width` und wie viel Auffüllung hinzugefügt wird, zwischen der `<div>` Elementinhalt s und seinem rechten Rand. Weitere Informationen zu unverankerte Elemente in CSS finden Sie in der [Floatutorial](http://css.maxdesign.com.au/floatutorial/).

Anstatt die Style-Einstellung direkt über das erste anzugeben `<p>` Element s `style` Attribut, können Sie stattdessen erstellen Sie eine neue CSS-Klasse im s `Styles.css` mit dem Namen `FloatLeft`:


[!code-css[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample3.css)]

Anschließend ersetzen wir können die `<div>` mit `<div class="FloatLeft">`.

Nach dem Hinzufügen von CSS-Klasse, und konfigurieren das Markup in der `CategoriesAndProducts.aspx` Seite, wechseln Sie in den Designer. Den Repeater Gleitkommatyp in der linken Seite des DataList-Steuerelement, (obwohl rechts jetzt sowohl einfach angezeigt werden wie Felder seit wir noch in der konfigurieren Sie ihre Datenquellen oder Vorlagen speichern gray) sollte angezeigt werden.


[![Auf der linken Seite des DataList-Steuerelement ist der Repeater abgedockt.](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image5.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image4.png)

**Abbildung 2**: der linken Seite des DataList-Steuerelement ist das-Repeater Betragssumme ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image6.png))


## <a name="step-2-determining-the-number-of-products-for-each-category"></a>Schritt 2: Bestimmen der Anzahl der Produkte für die einzelnen Kategorien

Steuern mit dem umgebenden Markup vollständige Repeater- und das DataList-ASP.NET-Serversteuerelement s wir erneut bereit, um die Kategoriedaten an das Repeater zu binden. Wie in der Aufzählung der Kategorien in Abbildung 1 gezeigt, müssen jedoch zusätzlich zu jeder Kategorie-s-Name wir auch zum Anzeigen der Anzahl der Produkte, die der Kategorie zugeordnet. Für den Zugriff auf diese Informationen können wir entweder:

- **Bestimmen Sie diese Informationen aus der ASP.NET Page-s-Code-Behind-Klasse.** Einer bestimmten *`categoryID`* können wir die Anzahl der zugeordneten Produkte zu ermitteln, durch den Aufruf der `ProductsBLL` Klasse s `GetProductsByCategoryID(categoryID)` Methode. Diese Methode gibt eine `ProductsDataTable` Objekt, dessen `Count` Eigenschaft gibt an, wie viele `ProductsRow` s vorhanden ist, d.h. die Anzahl der Produkte für die angegebene *`categoryID`*. Erstellen wir eine `ItemDataBound` -Ereignishandler für das Repeater, die für jede Kategorie an der Repeater gebunden aufruft der `ProductsBLL` Klasse s `GetProductsByCategoryID(categoryID)` Methode und die Anzahl der in der Ausgabe enthält.
- **Update der `CategoriesDataTable` im typisierten DataSet sollen eine `NumberOfProducts` Spalte.** Wir können dann aktualisieren, die `GetCategories()` -Methode in der die `CategoriesDataTable` enthalten diese Informationen, oder Sie lassen `GetCategories()` als-ist, und erstellen Sie ein neues `CategoriesDataTable` aufgerufene Methode `GetCategoriesAndNumberOfProducts()`.

Lassen Sie s, die beide Verfahren zu untersuchen. Der erste Ansatz ist einfacher zu implementieren, da wir Raten von t müssen der Datenzugriffsebene aktualisiert. Es erfordert jedoch weitere Kommunikation mit der Datenbank. Der Aufruf der `ProductsBLL` s-Klasse `GetProductsByCategoryID(categoryID)` -Methode in der die `ItemDataBound` Ereignishandler fügt einen zusätzlichen Datenbank-Aufruf für jede Kategorie im Wiederholungsmodul angezeigt. Mit dieser Technik stehen *N* + 1-Datenbank-Aufrufe, in denen *N* ist die Anzahl der Kategorien im Wiederholungsmodul angezeigt. Mit dem zweiten Ansatz wird die produktanzahl zurückgegeben, mit Informationen zu jeder Kategorie aus der `CategoriesBLL` s-Klasse `GetCategories()` (oder `GetCategoriesAndNumberOfProducts()`) Methode, die somit nur eine Reise in die Datenbank.

## <a name="determining-the-number-of-products-in-the-itemdatabound-event-handler"></a>Bestimmen der Anzahl der Produkte im ItemDataBound-Ereignishandler

Bestimmen der Anzahl der Produkte in jeder Kategorie im Wiederholungsmodul s `ItemDataBound` Ereignishandler erfordert keine Änderungen an unserer vorhandenen Datenzugriffsebene. Alle können Änderungen direkt in die `CategoriesAndProducts.aspx` Seite. Starten Sie durch das Hinzufügen einer neuen, mit dem Namen "ObjectDataSource" `CategoriesDataSource` über das Repeater-s-Smarttag. Als Nächstes konfigurieren Sie die `CategoriesDataSource` "ObjectDataSource" so, dass die It Ruft ab, die Daten aus der die `CategoriesBLL` Klasse s `GetCategories()` Methode.


[![Konfigurieren von dem ObjectDataSource-Steuerelement zur Verwendung der Klasse CategoriesBLL s GetCategories()-Methode](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image8.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image7.png)

**Abbildung 3**: Konfigurieren von dem ObjectDataSource-Steuerelement verwenden die `CategoriesBLL` s-Klasse `GetCategories()` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image9.png))


Jedes Element in der `Categories` Repeater muss geklickt werden kann und dazu führen, dass beim Klicken auf die `CategoryProducts` DataList-Steuerelement zum Anzeigen dieser Produkte für die ausgewählte Kategorie. Dies kann erreicht werden, indem Sie machen jede Kategorie einen Hyperlink, der an diese gleichen Seite verknüpfen (`CategoriesAndProducts.aspx`), sondern übergibt die `CategoryID` über die Abfragezeichenfolge, ähnlich wie im vorherigen Tutorial erläutert. Der Vorteil dieses Ansatzes ist, dass eine Seite anzeigen einer bestimmten Kategorie s-Produkten mit einem Lesezeichen versehen und von Suchmaschinen indiziert werden kann.

Alternativ können wir jede Kategorie erstellen LinkButton, ist der Ansatz, den wir für dieses Tutorial verwenden werden. LinkButton in den Browser des Benutzers s, als Link gerendert werden, jedoch führt durch Klicken auf einen Postback; Beim Postback muss die s DataList-Steuerelement "ObjectDataSource" aktualisiert werden, um diese Produkte der ausgewählten Kategorie angezeigt werden. In diesem Tutorial sinnvoller, einen Link zu verwenden als die Verwendung von LinkButton; Allerdings gibt es möglicherweise andere Szenarien, in denen Verwendung von LinkButton noch besseren ist. Während der Hyperlink-Ansatz wäre ideal für dieses Beispiel ist, können Sie stattdessen die Untersuchung mit LinkButton s ein. Wir sehen, führt das mit LinkButton einige Herausforderungen, die andernfalls nicht mit einem Link auftreten würde. Aus diesem Grund wird mit LinkButton in diesem Tutorial markieren Sie diese Herausforderungen und Lösungen für diese Szenarien bieten, in dem wir eine LinkButton anstelle eines Links verwenden möchten.

> [!NOTE]
> Es wird empfohlen, die in diesem Tutorial verwenden ein Linksteuerelement wiederholen oder `<a>` Element anstelle von LinkButton.


Das folgende Markup zeigt die deklarative Syntax des Repeaters und dem ObjectDataSource-Steuerelement. Beachten Sie, dass die Repeater-s-Vorlagen eine Liste mit Aufzählungszeichen mit dem jedes Element als ein LinkButton gerendert:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample4.aspx)]

> [!NOTE]
> Für dieses Tutorial benötigen Repeater Ansichtszustand aktiviert (Beachten Sie die Auslassung der `EnableViewState="False"` aus die deklarative Syntax des Repeater-s). In Schritt 3 wir erstellen einen Ereignishandler für das Repeater s `ItemCommand` Ereignis in dem aktualisiert, um den DataList-Steuerelement "ObjectDataSource"-s-s `SelectParameters` Auflistung. Der Repeater s `ItemCommand`jedoch gewonnen t ausgelöst, wenn der Ansichtszustand deaktiviert ist. Finden Sie unter [ein Stumper eine Frage ASP.NET](http://scottonwriting.net/sowblog/posts/1263.aspx) und [seine Lösung](http://scottonwriting.net/sowBlog/posts/1268.aspx) für Weitere Informationen darüber, warum Ansichtszustand für ein Repeater s aktiviert werden `ItemCommand` Ereignis ausgelöst.


ImageButton mit dem die `ID` Eigenschaftswert `ViewCategory` verfügt nicht über die `Text` Eigenschaftensatz. Wenn wir nur den Namen der Kategorie angezeigt möchten haben, würden festgelegt haben die Texteigenschaft deklarativ über Databinding-Syntax, wie folgt:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample5.aspx)]

Wir möchten jedoch sowohl den Namen der Kategorie s anzeigen *und* die Anzahl der Produkte, die zu dieser Kategorie gehören. Diese Informationen kann aus der Repeater s abgerufen werden `ItemDataBound` -Ereignishandler von einem Aufruf an die `ProductBLL` s-Klasse `GetCategoriesByProductID(categoryID)` -Methode und bestimmen, wie viele Datensätze zurückgegeben werden, in der resultierenden `ProductsDataTable`, wie der folgende Code veranschaulicht:


[!code-vb[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample6.vb)]

Zunächst werden, um sicherzustellen, dass wir erneut die Arbeit mit einem Datenelement (eine, deren `ItemType` ist `Item` oder `AlternatingItem`) und verweisen Sie auf die `CategoriesRow` -Instanz, die nur mit dem aktuellen gebunden wurde `RepeaterItem`. Als Nächstes bestimmt die Anzahl der Produkte für diese Kategorie durch Erstellen einer Instanz von der `ProductsBLL` Klasse Aufrufen der `GetCategoriesByProductID(categoryID)` -Methode, und Bestimmen der Anzahl der Datensätze zurückgegeben, mit der `Count` Eigenschaft. Zum Schluss die `ViewCategory` LinkButton in ItemTemplate ist "References" und die zugehörige `Text` -Eigenschaftensatz auf *"CategoryName"* (*NumberOfProductsInCategory*), wobei  *NumberOfProductsInCategory* als Zahl mit Dezimalstellen formatiert ist.

> [!NOTE]
> Alternativ, wir hätten eine *Formatierung Funktion* der ASP.NET Seite s-Code-Behind-Klasse, die eine Kategorie s akzeptiert `CategoryName` und `CategoryID` -Werte und gibt zurück der `CategoryName` verkettet mit der Anzahl von Produkte in der Kategorie (bestimmt durch Aufrufen der `GetCategoriesByProductID(categoryID)` Methode). Die Ergebnisse einer solchen Formatierung Funktion können deklarativ zugewiesen werden, der LinkButton-s-Text-Eigenschaft, und Ersetzen Sie dabei die Notwendigkeit der `ItemDataBound` -Ereignishandler. Finden Sie in der [Verwenden von TemplateFields, im GridView-Steuerelement](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) oder [Formatieren des DataList- und Wiederholungssteuerelement basierend auf Daten](../displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-vb.md) Lernprogramme für Weitere Informationen zur Verwendung von Formatierungsfunktionen.


Nach dem Hinzufügen dieser Ereignishandler an, nehmen Sie einen Moment Zeit, um die Seite über einen Browser zu testen. Beachten Sie, wie jede Kategorie in einer Aufzählung, die zum Anzeigen von Namen der Kategorie s und die Anzahl der Produkte, die der Kategorie zugeordnet aufgeführt wird (siehe Abbildung 4).


[![Jede Kategorie s Name und die Anzahl der Produkte werden angezeigt.](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image11.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image10.png)

**Abbildung 4**: jede Kategorie-s-Name und die Anzahl der Produkte werden angezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image12.png))


## <a name="updating-thecategoriesdatatableandcategoriestableadapterto-include-the-number-of-products-for-each-category"></a>Aktualisieren der`CategoriesDataTable`und`CategoriesTableAdapter`die Anzahl der Produkte für jede Kategorie enthält

Anstatt Bestimmen der Anzahl der Produkte für die einzelnen Kategorien wie s, die an der Repeater gebunden können wir diesen Prozess optimieren, durch Anpassen der `CategoriesDataTable` und `CategoriesTableAdapter` in der Datenzugriffsebene, um diese Informationen umfassen, die nativ. Um dies zu erreichen, müssen wir eine neue Spalte hinzufügen `CategoriesDataTable` , die die Anzahl der zugeordneten Produkte enthalten soll. Um einer "DataTable" eine neue Spalte hinzuzufügen, öffnen Sie das typisierte DataSet (`App_Code\DAL\Northwind.xsd`) mit der rechten Maustaste auf die DataTable, ändern, und wählen Sie Add / Spalte. Hinzufügen eine neue Spalte, die `CategoriesDataTable` (siehe Abbildung 5).


[![Fügen Sie eine neue Spalte hinzu, um die CategoriesDataSource](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image14.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image13.png)

**Abbildung 5**: Hinzufügen einer neuen Spalte, die `CategoriesDataSource` ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image15.png))


Dadurch wird eine neue Spalte namens hinzugefügt `Column1`, die Sie durch Eingabe in einen anderen Namen ändern können. Benennen Sie diese neue Spalte zu `NumberOfProducts`. Als Nächstes müssen wir diese s-Spalteneigenschaften zu konfigurieren. Klicken Sie auf die neue Spalte, und wechseln Sie zu dem Fenster "Eigenschaften". Ändern Sie die Spalte s `DataType` Eigenschaft `System.String` zu `System.Int32` und legen Sie die `ReadOnly` Eigenschaft `True`, wie in Abbildung 6 dargestellt.


![Legen Sie den Datentyp und die ReadOnly-Eigenschaften der neuen Spalte.](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image16.png)

**Abbildung 6**: Legen Sie die `DataType` und `ReadOnly` Eigenschaften der neuen Spalte.


Während der `CategoriesDataTable` verfügt jetzt über eine `NumberOfProducts` Spalte der Wert wird durch die entsprechende Abfragen des TableAdapter-s nicht festgelegt. Aktualisieren wir die `GetCategories()` Methode, um diese Informationen zurückzugeben, wenn wir solche Informationen sollen zurückgegeben wird, jedes Mal, wenn Sie Informationen zu Auftragskategorien abgerufen wird. Wenn Sie allerdings müssen wir nur die Anzahl der zugeordneten Produkte für die Kategorien in seltenen Fällen (z. B. speziell für dieses Tutorial) zu erfassen, lassen wir `GetCategories()` als-ist, und erstellen Sie eine neue Methode, die diese Informationen zurückgibt. Let-s verwenden dieses letztgenannte Ansatz erstellen eine neue Methode namens `GetCategoriesAndNumberOfProducts()`.

Hinzufügen neuer `GetCategoriesAndNumberOfProducts()` -Methode, mit der rechten Maustaste auf die `CategoriesTableAdapter` , und wählen Sie die neue Abfrage. Dadurch wird auf der TableAdapter-Abfragen Konfigurations-Assistent, die wir mehrere Male in vorherigen Tutorials verwendet haben. Starten Sie den Assistenten für diese Methode wird angegeben, dass die Abfrage eine Ad-hoc-SQL-Anweisung verwendet, die Zeilen zurückgibt.


[![Erstellen Sie die Methode, die mit Ad-hoc-SQL-Anweisungen](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image18.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image17.png)

**Abbildung 7**: Exemplarische Vorgehensweise: Erstellen von der Methode mit einer Ad-hoc-SQL-Anweisung ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image19.png))


[![Die SQL-Anweisung gibt Zeilen zurück.](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image21.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image20.png)

**Abbildung 8**: der SQL-Anweisung gibt Zeilen ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image22.png))


Im nächsten Assistentenbildschirm verlangt, für die Abfrage zu verwenden. Jede Kategorie s zurückzugebenden `CategoryID`, `CategoryName`, und `Description` Felder sowie die Anzahl der Produkte mit der Kategorie, verwenden Sie die folgenden `SELECT` Anweisung:


[!code-sql[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample7.sql)]


[![Geben Sie die Abfrage zu verwendenden](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image24.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image23.png)

**Abbildung 9**: Geben Sie die Abfrage zu verwendenden ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image25.png))


Beachten Sie, dass die Unterabfrage, die berechnet die Anzahl der Produkte, die der Kategorie zugeordneten Alias `NumberOfProducts`. Diese Benennungskonvention Übereinstimmung führt dazu, dass den Rückgabewert von diesem Unterabfrage zugeordnet werden die `CategoriesDataTable` s `NumberOfProducts` Spalte.

Im letzte Schritt werden, geben Sie diese Abfrage aus, wählen Sie den Namen für die neue Methode. Verwendung `FillWithNumberOfProducts` und `GetCategoriesAndNumberOfProducts` für die Füllung einer DataTable und Rückgabe eine "DataTable"-Mustern bzw.


[![Name der neuen TableAdapter s Methoden FillWithNumberOfProducts und GetCategoriesAndNumberOfProducts](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image27.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image26.png)

**Abbildung 10**: Benennen Sie die neuen TableAdapter-Methoden `FillWithNumberOfProducts` und `GetCategoriesAndNumberOfProducts` ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image28.png))


An diesem Punkt wurde der Datenzugriffsebene erweitert, um die Anzahl der Produkte nach Kategorie gehören. Da alle unsere Darstellungsschicht alle Aufrufe an die DAL durch eine separate Geschäftslogikebene leitet müssen wir eine entsprechende hinzufügen `GetCategoriesAndNumberOfProducts` Methode, um die `CategoriesBLL` Klasse:


[!code-vb[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample8.vb)]

Der DAL und der BLL abgeschlossen, es erneut bereit zum Binden von dieser Daten die `Categories` Repeater in `CategoriesAndProducts.aspx`! Wenn Sie haben bereits ein ObjectDataSource-Steuerelement für Repeater aus der bestimmen erstellt die Anzahl der Produkte in der `ItemDataBound` Ereignishandler Löschen dieser "ObjectDataSource" und Entfernen des Repeaters s `DataSourceID` Eigenschaft festlegen; auch ohne Kabel der Repeater s `ItemDataBound` Ereignis des ereignishandlers durch das Entfernen der `Handles Categories.OnItemDataBound` Syntax in der ASP.NET Code-Behind-Klasse.

Fügen Sie mit der Repeater im ursprünglichen Zustand zurück, eine neue, mit dem Namen "ObjectDataSource" `CategoriesDataSource` über das Repeater-s-Smarttag. Konfigurieren Sie mit dem ObjectDataSource-Steuerelement die `CategoriesBLL` -Klasse, aber statt verwendet, sollte die `GetCategories()` -Methode, verwenden sie `GetCategoriesAndNumberOfProducts()` stattdessen (siehe Abbildung 11).


[![Konfigurieren von dem ObjectDataSource-Steuerelement zur Verwendung der GetCategoriesAndNumberOfProducts-Methode](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image30.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image29.png)

**Abbildung 11**: Konfigurieren von dem ObjectDataSource-Steuerelement verwenden die `GetCategoriesAndNumberOfProducts` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image31.png))


Aktualisieren Sie als Nächstes die `ItemTemplate` , damit die LinkButton s `Text` Eigenschaft zugewiesen wird deklarativ mit Datenbindungssyntax und enthält die beiden die `CategoryName` und `NumberOfProducts` Datenfelder. Das vollständige deklarativen Markup für das Repeater und `CategoriesDataSource` "ObjectDataSource" Gehen Sie folgendermaßen vor:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample9.aspx)]

Die Ausgabe durch Aktualisieren der DAL sollen gerendert eine `NumberOfProducts` Spalte entspricht der Verwendung der `ItemDataBound` Event Handler Ansatz (siehe Abbildung 4 auf einem Bildschirm finden Sie unter Aufnahme des Wiederholungsmoduls mit den Kategorienamen und die Anzahl der Produkte).

## <a name="step-3-displaying-the-selected-category-s-products"></a>Schritt 3: Die ausgewählte Kategorie-s-Produkte anzeigen

An diesem Punkt haben wir die `Categories` Repeater, die die Liste der Kategorien sowie die Anzahl der Produkte in jeder Kategorie angezeigt. Der Repeater verwendet eine LinkButton für jede Kategorie an, dass beim Klicken auf weisen bewirkt, dass ein Postback, an dem wir müssen für diese Produkte für die ausgewählte Kategorie in der `CategoryProducts` DataList-Steuerelement.

Eine Herausforderung, die für uns ist wie DataList-Steuerelement nur die Produkte für die ausgewählte Kategorie angezeigt. In der [Master/Detail mithilfe eines auswählbaren GridView-Mastersteuerelements mit einem DetailsView Details](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md) Tutorial wurde erläutert, wie einer GridView-Ansicht zu erstellen, deren Zeilen ausgewählt werden kann, mit der ausgewählten Zeile s details in einem DetailsView auf derselben Seite angezeigt wird. Das GridView-s "ObjectDataSource" zurückgegebenen Informationen zu allen Produkten, die mit der `ProductsBLL` s `GetProducts()` das DetailsView-s "ObjectDataSource"-Methode abgerufenen Informationen über das ausgewählte Produkt mithilfe der `GetProductsByProductID(productID)` Methode. Die *`productID`* Parameterwert wurde deklarativ angegeben, indem Sie den Wert der GridView Zuordnungsvorgänge zuordnen `SelectedValue` Eigenschaft. Leider Repeater verfügt nicht über eine `SelectedValue` Eigenschaft und kann nicht als Parameterquelle fungieren.

> [!NOTE]
> Dies ist eine dieser Herausforderungen, die bei der Verwendung von LinkButton in einem Wiederholungssteuerelement angezeigt. Hatten wir einen Link für die Übergabe in verwendet die `CategoryID` über die Abfragezeichenfolge wir könnten verwenden Sie stattdessen das QueryString-Feld als Quelle für die s-Parameterwert.


Bevor wir den Mangel an Gedanken einer `SelectedValue` let-Eigenschaft für das Repeater jedoch s zuerst DataList-Steuerelement an ein ObjectDataSource-Steuerelement zu binden, und geben Sie die `ItemTemplate`.

Deaktivieren Sie aus dem Smarttag DataList s, zum Hinzufügen einer neuen, mit dem Namen "ObjectDataSource" `CategoryProductsDataSource` und konfigurieren Sie ihn zur Verwendung der `ProductsBLL` s-Klasse `GetProductsByCategoryID(categoryID)` Methode. Da DataList-Steuerelement in diesem Tutorial eine nur-Lese Schnittstelle bietet, können Sie die Dropdown-Listen in der INSERT-, Update-, und Löschen von Registerkarten (keine).


[![Konfigurieren von dem ObjectDataSource-Steuerelement zur Verwendung ProductsBLL Klasse s GetProductsByCategoryID(categoryID)-Methode](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image33.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image32.png)

**Abbildung 12**: Konfigurieren von dem ObjectDataSource-Steuerelement zu verwendenden `ProductsBLL` s-Klasse `GetProductsByCategoryID(categoryID)` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image34.png))


Da die `GetProductsByCategoryID(categoryID)` Methode erwartet einen Eingabeparameter (*`categoryID`*), mit dem Konfigurieren von Datenquellen-Assistenten können wir die Parameter-s-Quelle angegeben. Mussten die Kategorien wurden aufgelistet in einer GridView-Ansicht oder einem DataList-Steuerelement, d legen wir der Parameterliste der Quelle Dropdown-Steuerelement und die ControlID auf die `ID` der Daten-Websteuerelement. Da der Repeater fehlt jedoch eine `SelectedValue` Eigenschaft, die nicht als Parameterquelle verwendet werden. Wenn Sie überprüfen, werden Sie feststellen, dass die ControlID-Dropdown-Liste nur ein Steuerelement enthält `ID``CategoryProducts`, `ID` der DataList-Steuerelement.

Jetzt wird der Quellliste des Dropdown-Parameter auf "None" fest. Wir zum Schluss werden programmgesteuert zuweisen Wert dieses Parameters, wenn eine Kategorie, die im Wiederholungsmodul auf LinkButton geklickt wird.


[![Führen Sie als Parameter-Quelle für die CategoryID-Parameter nicht angeben](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image36.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image35.png)

**Abbildung 13**: Geben Sie keine Quelle-Parameter für die *`categoryID`* Parameter ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image37.png))


Nach dem Fertigstellen des Assistenten für die Datenquelle konfigurieren Visual Studio generiert automatisch DataList-Steuerelement s `ItemTemplate`. Ersetzen Sie diese Standardeinstellung `ItemTemplate` wir mit der Vorlage, die in den vorherigen Tutorials verwendet; darüber hinaus legen Sie die DataList s `RepeatColumns` Eigenschaft auf 2. Nach diesen Änderungen sollte das deklarative Markup für Ihre DataList-Steuerelement und seine zugehörigen "ObjectDataSource" wie folgt aussehen:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample10.aspx)]

Derzeit den `CategoryProductsDataSource` "ObjectDataSource" s *`categoryID`* Parameter nicht festgelegt ist, damit keine Produkte angezeigt werden, wenn Sie die Seite anzeigen. Was wir tun müssen, dass der Wert dieses Parameters legen Sie basierend auf den `CategoryID` der Kategorie im Wiederholungsmodul geklickt wurde. Dies führt zu zwei Herausforderungen: zuerst, wie wir feststellen, wenn ein LinkButton im Wiederholungsmodul s `ItemTemplate` wurde geklickt wurde, und das zweite wie können wir ermitteln die `CategoryID` der entsprechenden Kategorie, deren LinkButton geklickt wurde?

Wie die Schaltfläche "und" ImageButton LinkButton verfügt über eine `Click` Ereignis und eine [ `Command` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.command.aspx). Die `Click` Ereignis dient lediglich Beachten Sie, dass es sich bei LinkButton geklickt wurde. In einigen Fällen müssen jedoch zusätzlich zu beachten, dass die LinkButton geklickt wurde wir auch einige zusätzliche Informationen an den Ereignishandler übergeben. Wenn dies der Fall, die LinkButton-s ist [ `CommandName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.commandname.aspx) und [ `CommandArgument` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.commandargument.aspx) Eigenschaften können diese zusätzlichen Informationen zugewiesen werden. Dann, wenn auf LinkButton geklickt wird, seine `Command` -Ereignis ausgelöst wird (anstelle von seine `Click` Ereignis) und der Ereignishandler übergeben die Werte der der `CommandName` und `CommandArgument` Eigenschaften.

Wenn eine `Command` -Ereignisses von innerhalb einer Vorlage im Wiederholungsmodul, das Repeater-s [ `ItemCommand` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeater.itemcommand.aspx) ausgelöst wird, und übergeben die `CommandName` und `CommandArgument` Werte von LinkButton geklickt wurde (oder eine Schaltfläche oder ImageButton). Um zu bestimmen, wenn eine Kategorie LinkButton im Wiederholungsmodul geklickt wurde, müssen wir daher die folgenden Schritte ausführen:

1. Legen Sie die `CommandName` Eigenschaft LinkButton im Wiederholungsmodul s `ItemTemplate` mit einem Wert (ich Ve verwendet ListProducts). Durch diese Einstellung `CommandName` Wert, der LinkButton-s `Command` Ereignis wird ausgelöst, wenn auf LinkButton geklickt wird.
2. Legen Sie die s LinkButton `CommandArgument` -Eigenschaft auf den Wert des aktuellen Elements s `CategoryID`.
3. Erstellen Sie einen Ereignishandler für das Repeater s `ItemCommand` Ereignis. Im Ereignishandler, Festlegen der `CategoryProductsDataSource` "ObjectDataSource" s `CategoryID` Parameter, um den Wert des übergebenen `CommandArgument`.

Die folgenden `ItemTemplate` Markup für die Kategorien Repeater implementiert die Schritte 1 und 2. Hinweis wie die `CommandArgument` Wert wird zugewiesen, die dem Datenelement s `CategoryID` Databinding-Syntax verwenden:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample11.aspx)]

Bei jedem Erstellen einer `ItemCommand` -Ereignishandler, es ist ratsam, die immer zuerst überprüfen Sie den eingehenden `CommandName` bewertet werden, da *alle* `Command` Ereignis ausgelöst wird, indem *alle* Schaltfläche "", "LinkButton, oder ImageButton innerhalb des Repeaters führt dazu, dass die `ItemCommand` Ereignis ausgelöst. Während wir derzeit nur eine solche LinkButton jetzt aufweisen, in der Zukunft wir (oder ein anderer Entwickler in unserem Team) kann hinzufügen Weitere Schaltflächensteuerelemente für Web Repeater, die beim Klicken auf, löst die gleiche `ItemCommand` -Ereignishandler. Aus diesem Grund es s, die am besten immer Stellen Sie sicher, dass Sie die `CommandName` Eigenschaft und die programmgesteuerte Logik nur fort, wenn sie mit dem erwarteten Wert übereinstimmt.

Nachdem Sie sichergestellt haben, die der übergegebenen `CommandName` Wert ListProducts, weist Sie dann der Ereignishandler der `CategoryProductsDataSource` "ObjectDataSource" s `CategoryID` Parameter, um den Wert des übergebenen `CommandArgument`. Diese Änderung der "ObjectDataSource"-s `SelectParameters` automatisch verursacht DataList-Steuerelement selbst mit der Datenquelle, die die Produkte für die neu ausgewählte Kategorie erneut binden.


[!code-vb[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample12.vb)]

Mit dieser Ergänzungen wurde unser Tutorial abgeschlossen. Nehmen Sie einen Moment Zeit, um dies zu testen, in einem Browser. Abbildung 14 zeigt den Bildschirm aus, wenn die Seite zuerst besuchen. Da es sich bei eine Kategorie noch muss ausgewählt werden, werden keine Produkte angezeigt. Durch Klicken auf eine Kategorie, z. B. erzeugen, die Produkte in der Kategorie "Product" in einer zweispaltigen Ansicht angezeigt werden (siehe Abbildung 15).


[![Keine Produkte werden angezeigt, beim ersten Zugriff auf der Seite "](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image39.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image38.png)

**Abbildung 14**: keine Produkte werden angezeigt, beim ersten Besuch der Seite ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image40.png))


[![Klicken Sie auf die erzeugen Kategorielisten die entsprechenden Produkte nach rechts](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image42.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image41.png)

**Abbildung 15**: Klicken Sie auf die Kategorie "erstellen" zeigt die übereinstimmenden-Produkte auf der rechten Seite ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image43.png))


## <a name="summary"></a>Zusammenfassung

Wie in diesem Tutorial und im vorherigen Fall beschrieben, können Master/Detail-Berichte über zwei Seiten verteilt, oder auf einem konsolidiert. Anzeigen eines Master-/Detail-Berichts auf einer Seite, führt jedoch einige Herausforderungen wie am besten zum Layout der Master und Details-Datensätze auf der Seite. In der *Master/Detail mithilfe eines auswählbaren GridView-Mastersteuerelements mit einem DetailsView Details* Tutorial, mussten wir die Details-Datensätze, die über die master-Datensätze angezeigt werden; in diesem Tutorial verwendet wir CSS-Techniken, damit der Masterdatensätze "float", um die linke Begrenzung des Details.

Zusammen mit der Anzeige von Master-/Detail-Berichte, wir mussten außerdem die Möglichkeit, Informationen zum Abrufen der Anzahl der Produkte, die auch jede Kategorie zugeordnet wird, wie serverseitige Logik ausführen, wenn ein LinkButton (oder der Schaltfläche oder ImageButton), innerhalb geklickt wird einer "Repeater".

Dieses Tutorial ist unsere Untersuchung von Master/Detail-Berichte mit dem DataList- und Wiederholungssteuerelement abgeschlossen. Unsere nächste Reihe von Lernprogrammen wird hinzufügen, bearbeiten und Löschen von Funktionen, um das DataList-Steuerelement veranschaulicht.

Viel Spaß beim Programmieren!

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Tutorial erläutert finden Sie in den folgenden Ressourcen:

- [Floatutorial](http://css.maxdesign.com.au/floatutorial/) ein Tutorial zur CSS-Elementen mit CSS-Gleitkomma
- [CSS-Positionierung](http://www.brainjar.com/css/positioning/) Weitere Informationen zum Positionieren von Elementen mit CSS
- [Schaffen Sie Inhalt mit HTML](http://www.w3schools.com/html/html_layout.asp) mit `<table>` s und andere HTML-Elemente für die Positionierung

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial wurde Zack Jones. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Vorherige](master-detail-filtering-acess-two-pages-datalist-vb.md)
