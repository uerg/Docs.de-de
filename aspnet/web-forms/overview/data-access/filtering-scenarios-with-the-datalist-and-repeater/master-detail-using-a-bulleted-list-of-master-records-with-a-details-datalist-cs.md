---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs
title: Master/Detail-Details DataList (c#) eine Aufzählung der Master-Datensätze mit | Microsoft Docs
author: rick-anderson
description: In diesem Lernprogramm fügen wir den zweiseitige Master/Detail-Bericht, der im vorherigen Lernprogramm in einer einzelnen Seite komprimieren zeigt eine Aufzählung der Kategorienamen bei t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2006
ms.topic: article
ms.assetid: c727bb73-7b59-41a1-8dc3-623c6d69e7c2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: c041c352c379dc1d3c0f13013e7e323faa500912
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30888967"
---
<a name="masterdetail-using-a-bulleted-list-of-master-records-with-a-details-datalist-c"></a>Master/Detail-verwenden eine Aufzählung der Master-Datensätze mit einem DataList Details (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunterladen](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_35_CS.exe) oder [PDF herunterladen](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/datatutorial35cs1.pdf)

> In diesem Lernprogramm werden wir den zweiseitige Master/Detail-Bericht, der im vorherigen Lernprogramm in einer einzelnen Seite komprimieren, der auf der linken Seite des Bildschirms und der ausgewählten Kategorie Produkte auf der rechten Seite des Bildschirms eine Aufzählung der Kategorienamen anzeigt.


## <a name="introduction"></a>Einführung

In der [vorherigen Lernprogramm](master-detail-filtering-acess-two-pages-datalist-cs.md) erläutert, wie einen Master/Detail-Bericht über zwei Seiten zu trennen. Auf der Masterseite verwendet wurde eine Wiederholungsmodul-Steuerelement, um eine Aufzählung der Kategorien zu rendern. Jeder Kategoriename wurde, einen Link, der beim Klicken auf würde Take der Benutzer auf der Detailseite eine zweispaltige DataList, in dem dieser Produkte ergab gehört der ausgewählten Kategorie.

In diesem Lernprogramm werden wir das Lernprogramm zweiseitige in einer einzelnen Seite komprimieren eine Aufzählung der Kategorienamen auf der linken Seite des Bildschirms anzeigen, mit jeder Kategoriename als LinkButton gerendert. Klicken auf eine neben dem Kategorienamen LinkButtons einen Postback etwas, und bindet Produkte der ausgewählten Kategorie an eine zweispaltige DataList auf der rechten Seite des Bildschirms. Zusätzlich zur Anzeige der einzelnen Kategorienamen s, Repeater auf der linken Seite zeigt, wie viele insgesamt Produkte nach einer bestimmten Kategorie werden (siehe Abbildung 1).


[![Die Kategorie s Name und die gesamte Anzahl der Produkte werden auf der linken Seite angezeigt.](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image2.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image1.png)

**Abbildung 1**: die Kategorie s Name "und" Total Products Anzahl werden angezeigt, auf der linken Seite ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image3.png))


## <a name="step-1-displaying-a-repeater-in-the-left-portion-of-the-screen"></a>Schritt 1: Einen Repeater im linken Bereich des Bildschirms anzeigen

Für dieses Lernprogramm benötigen wir die Aufzählung der Kategorien auf der linken Seite der ausgewählten Kategorie s Produkte angezeigt werden. Inhalt innerhalb einer Webseite kann positioniert werden, mithilfe der standardmäßigen HTML-Elementen Absatztags geschütztes Leerzeichen, `<table>` s usw. oder durch cascading Stylesheet (CSS)-Techniken. Bisher haben alle unseren Tutorials, CSS-Techniken zum Positionieren verwendet. Wenn erstellt die Benutzeroberfläche für die Navigation in unserer Masterseite in der [Masterseiten und Websitenavigation](../introduction/master-pages-and-site-navigation-cs.md) Lernprogramm wird *absolute Positionierung*, der angibt, das genaue Pixel-Offset für die Navigation Liste und der Hauptinhalt. Alternativ kann CSS verwendet werden, um ein Element nach rechts oder links von einem anderen über einzufügen *unverankerte*. Wir können die Aufzählung der Kategorien auf der linken Seite der ausgewählten Kategorie s Produkte angezeigt werden, von Gleitkomma Repeater auf der linken Seite des DataList haben.

Öffnen der `CategoriesAndProducts.aspx` Seite aus der `DataListRepeaterFiltering` Ordner und fügen Sie der Seite ein Repeater und DataList hinzu. Die Repeater s festgelegt `ID` auf `Categories` und DataList s zum `CategoryProducts`. Wechseln Sie zur Quellansicht, und fügen Sie die Repeater und DataList-Steuerelemente innerhalb ihrer eigenen `<div>` Elemente. Einschließen, also die Repeater innerhalb einer `<div>` Element erste, und klicken Sie dann das DataList in eine eigene `<div>` -Element direkt nach Wiederholungsmoduls. Das Markup sollte an diesem Punkt folgt oder ähnlich aussehen:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample1.aspx)]

Um die Repeater auf der linken Seite des DataList float, müssen wir verwenden die `float` CSS-Stil-Attribut, wie folgt:


[!code-html[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample2.html)]

Die `float: left;` : Verschiebt die erste `<div>` Element links neben dem zweiten Ausdruck. Die `width` und `padding-right` Einstellungen weisen darauf hin, das erste `<div>` s `width` und wie viel Abstand zwischen dem wird die `<div>` Elementinhalt s und seinem rechten Rand. Weitere Informationen zu Elementen in CSS Gleitkomma sehen Sie sich die [Floatutorial](http://css.maxdesign.com.au/floatutorial/).

Anstatt die stileinstellung direkt über das erste anzugeben `<p>` Element s `style` -Attribut angegeben wird, können Sie stattdessen erstellen Sie eine neue CSS-Klasse in s `Styles.css` mit dem Namen `FloatLeft`:


[!code-css[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample3.css)]

Dann, wir ersetzen können die `<div>` mit `<div class="FloatLeft">`.

Nach dem Hinzufügen von CSS-Klasse, und konfigurieren das Markup in der `CategoriesAndProducts.aspx` Seite, wechseln Sie in den Designer. Daraufhin sollte die Repeater, Gleitkommatyp in der linken Seite des DataList, (obwohl rechts jetzt beide einfach angezeigt werden als Felder grau, seit es Ve noch auf ihre Datenquellen oder Vorlagen konfigurieren).


[![Wiederholungsmoduls wird auf der linken Seite des DataList umfließt.](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image5.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image4.png)

**Abbildung 2**: die Repeater wird auf der linken Seite des DataList umfließt ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image6.png))


## <a name="step-2-determining-the-number-of-products-for-each-category"></a>Schritt 2: Bestimmen der Anzahl der Produkte für die einzelnen Kategorien

Steuern mit vollständige Markup umgibt Repeater und DataList s wir re kann jetzt die Kategoriedaten an den Repeater binden. Wie in der Aufzählung von Kategorien in Abbildung 1 gezeigt, müssen jedoch zusätzlich zu den einzelnen s Kategorienamen wir auch zum Anzeigen der Anzahl der Produkte, die der Kategorie zugeordnet. Für den Zugriff auf diese Informationen können wir entweder:

- **Ermitteln Sie diese Informationen aus der ASP.NET Seite s-Code-Behind-Klasse.** Einer bestimmten *`categoryID`* können wir die Anzahl der zugeordneten Produkte zu ermitteln, durch Aufrufen der `ProductsBLL` Klasse s `GetProductsByCategoryID(categoryID)` Methode. Diese Methode gibt ein `ProductsDataTable` Objekt, dessen `Count` Eigenschaft gibt an, wie viele `ProductsRow` s vorhanden ist, ist die Anzahl der Produkte für die angegebene *`categoryID`*. Erstellen wir ein `ItemDataBound` -Ereignishandler für die Repeater, die für jede Kategorie gebunden an die Repeater aufruft der `ProductsBLL` Klasse s `GetProductsByCategoryID(categoryID)` Methode und enthält die Anzahl der in der Ausgabe.
- **Update der `CategoriesDataTable` in das typisierte DataSet enthalten eine `NumberOfProducts` Spalte.** Wir können dann aktualisieren, die `GetCategories()` Methode in der `CategoriesDataTable` umfassen diese Informationen, oder lassen Sie alternativ `GetCategories()` als-ist, und erstellen Sie ein neues `CategoriesDataTable` Methode mit dem Namen `GetCategoriesAndNumberOfProducts()`.

Lassen Sie s beiden Techniken zu untersuchen. Der erste Ansatz ist einfacher zu implementieren, da wir t müssen der Datenzugriffsebene aktualisiert Verschlüsselungskennwort; Dies erfordert allerdings weitere Kommunikation mit der Datenbank. Der Aufruf der `ProductsBLL` Klasse s `GetProductsByCategoryID(categoryID)` Methode in der `ItemDataBound` -Ereignishandler fügt einen zusätzliche Datenbankaufruf für die einzelnen Kategorien im Wiederholungsmodul angezeigt. Mit dieser Technik stehen *N* + 1 Datenbankaufrufe, in denen *N* ist die Anzahl der Kategorien im Wiederholungsmodul angezeigt. Mit den zweiten Ansatz, die produktanzahl zurückgegeben, mit Informationen zu jeder Kategorie aus der `CategoriesBLL` Klasse s `GetCategories()` (oder `GetCategoriesAndNumberOfProducts()`) Methode, wodurch nur einen Trip zur Datenbank.

## <a name="determining-the-number-of-products-in-the-itemdatabound-event-handler"></a>Bestimmen der Anzahl der Produkte in der ItemDataBound-Ereignishandler

Bestimmen der Anzahl der Produkte für die einzelnen Kategorien im Wiederholungsmodul s `ItemDataBound` Ereignishandler erfordert keine Änderungen an unseren vorhandenen Datenzugriffsebene. Alle können Änderungen direkt in die `CategoriesAndProducts.aspx` Seite. Starten Sie durch Hinzufügen einer neuen ObjectDataSource mit dem Namen `CategoriesDataSource` über das Smarttag Repeater s. Anschließend konfigurieren Sie die `CategoriesDataSource` ObjectDataSource so, dass die It ruft seine Daten aus ab der `CategoriesBLL` Klasse s `GetCategories()` Methode.


[![Konfigurieren der ObjectDataSource zur Verwendung der Klasse CategoriesBLL s GetCategories()-Methode](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image8.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image7.png)

**Abbildung 3**: Konfigurieren der ObjectDataSource verwenden die `CategoriesBLL` Klasse s `GetCategories()` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image9.png))


Jedes Element in der `Categories` Repeater muss werden durch Klicken aktivierbaren und dazu führen, dass beim Klicken auf die `CategoryProducts` DataList dieser Produkte für die ausgewählte Kategorie angezeigt. Dies geschieht durch Erstellen von jeder Kategorie einen Link, der an der gleichen Seite verknüpfen (`CategoriesAndProducts.aspx`), und übergeben Sie jedoch die `CategoryID` über die Abfragezeichenfolge, ähnlich wie im vorherigen Lernprogramm wurde erläutert. Der Vorteil dieses Ansatzes ist, dass eine Seite mit einer bestimmten Kategorie s Produkte mit einem Lesezeichen versehene und von einer Suchmaschine indiziert werden kann.

Alternativ können wir jede Kategorie, LinkButton, dies ist der Ansatz, den wir für dieses Lernprogramm verwendet werden. LinkButton rendert im Browser als Link Benutzer s jedoch, führt durch Klicken auf einen Postback; Beim Postback muss das DataList-s ObjectDataSource aktualisiert werden, um die Produkte, die zur ausgewählten Kategorie angezeigt werden. Für dieses Lernprogramm stellt einen Link mit sinnvoller sein als die Verwendung von LinkButton; Allerdings gibt es möglicherweise andere Szenarien, in denen Verwendung von LinkButton mehr vorteilhaft ist. Während der Hyperlink Ansatz ideal für dieses Beispiel wäre, können Sie stattdessen untersuchen LinkButton mit s an. Wir sehen, stellt das mit LinkButton eine Herausforderung dar, die nicht mit einem Link andernfalls auftreten würden. Aus diesem Grund wird mit LinkButton in diesem Lernprogramm markieren Sie diese Probleme und Lösungen für diese Szenarien ermöglichen, in denen wir LinkButton anstelle eines Links zu verwenden möchten.

> [!NOTE]
> Es wird empfohlen, dieses Lernprogramm verwenden ein Linksteuerelement wiederholen oder `<a>` Element anstelle von LinkButton.


Das folgende Markup veranschaulicht die deklarative Syntax für Wiederholungsmoduls und das ObjectDataSource. Beachten Sie, dass die Repeater s Vorlagen eine Aufzählung mit jedem Element als LinkButton gerendert:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample4.aspx)]

> [!NOTE]
> Für dieses Lernprogramm benötigen Repeater seinen Ansichtszustand aktiviert (Dabei wird der von der `EnableViewState="False"` vom Repeater s deklarative Syntax). In Schritt 3 wir erstellen einen Ereignishandler für das Repeater s `ItemCommand` Ereignis in dem wir das DataList s ObjectDataSource s aktualisiert werden `SelectParameters` Auflistung. Die Repeater s `ItemCommand`jedoch/gewonnen t auslösen, wenn der Ansichtszustand deaktiviert ist. Finden Sie unter [Stumper ein, der einer Frage ASP.NET](http://scottonwriting.net/sowblog/posts/1263.aspx) und [seine Lösung](http://scottonwriting.net/sowBlog/posts/1268.aspx) für Weitere Informationen dazu, warum Ansichtszustand für ein Repeater s muss aktiviert sein `ItemCommand` Ereignis ausgelöst.


LinkButton mit der `ID` Eigenschaftswert `ViewCategory` verfügt nicht über die `Text` Eigenschaftensatz. Wenn wir nur den Namen der Kategorie angezeigt möchten hatte, wir würden festgelegt haben die Texteigenschaft deklarativ durch Databinding-Syntax wie folgt:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample5.aspx)]

Wir möchten jedoch sowohl der Kategoriename s anzeigen *und* die Anzahl der Produkte, die zu dieser Kategorie gehören. Abruf dieser Informationen aus den Repeater s `ItemDataBound` -Ereignishandler von einem Aufruf an die `ProductBLL` Klasse s `GetCategoriesByProductID(categoryID)` -Methode und bestimmen, wie viele Datensätze zurückgegeben werden, in der resultierenden `ProductsDataTable`, wie der folgende Code veranschaulicht:


[!code-csharp[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample6.cs)]

Wir zunächst sicherstellen, dass wir arbeiten mit einem Datenelement (eine, deren `ItemType` ist `Item` oder `AlternatingItem`) und verweisen Sie auf die `CategoriesRow` -Instanz, die nur mit dem aktuellen gebunden wurde `RepeaterItem`. Als Nächstes bestimmen die Anzahl der Produkte für diese Kategorie durch Erstellen einer Instanz von der `ProductsBLL` -Klasse aufrufen seiner `GetCategoriesByProductID(categoryID)` -Methode und Bestimmen der Anzahl von Datensätzen zurückgegeben, mit der `Count` Eigenschaft. Schließlich die `ViewCategory` LinkButton in ItemTemplate ist "References" und die zugehörige `Text` -Eigenschaftensatz auf *CategoryName* (*NumberOfProductsInCategory*), wobei  *NumberOfProductsInCategory* als Zahl mit Dezimalstellen formatiert ist.

> [!NOTE]
> Alternativ wir hätten ein *Formatierung Funktion* der ASP.NET Seite "s" Code-Behind-Klasse, die eine Kategorie s akzeptiert `CategoryName` und `CategoryID` Werte und gibt die `CategoryName` verkettet mit der Anzahl von Produkte in der Kategorie (durch Aufrufen der `GetCategoriesByProductID(categoryID)` Methode). Die Ergebnisse eines solchen Formatierungsfunktion konnte deklarativ zugewiesen werden, mit dem s LinkButton Text-Eigenschaft, und Ersetzen Sie dabei die Notwendigkeit der `ItemDataBound` -Ereignishandler. Finden Sie in der [mithilfe von TemplateFields, im des GridView-Steuerelements](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) oder [Formatierung des DataList und Repeater basierend auf Daten](../displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-cs.md) Lernprogramme für Weitere Informationen zur Verwendung von Formatierungsfunktionen.


Nehmen Sie einen Moment Zeit, testen Sie die Seite über einen Browser, nach dem Hinzufügen dieser Ereignishandler können. Beachten Sie, wie jede Kategorie in einer Aufzählung, die zum Anzeigen von den Kategorienamen s und die Anzahl der Produkte, die der Kategorie zugeordnet aufgeführt ist (siehe Abbildung 4).


[![Jede Kategorie s Name und die Anzahl der Produkte werden angezeigt.](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image11.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image10.png)

**Abbildung 4**: jede Kategorie s Name und die Anzahl der Produkte angezeigt werden ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image12.png))


## <a name="updating-thecategoriesdatatableandcategoriestableadapterto-include-the-number-of-products-for-each-category"></a>Aktualisieren der`CategoriesDataTable`und`CategoriesTableAdapter`enthalten die Anzahl der Produkte für die einzelnen Kategorien

Anstatt Bestimmen der Anzahl der Produkte in jeder Kategorie an, wie er s gebunden, an die Repeater wir können diesen Prozess optimieren, durch Anpassen der `CategoriesDataTable` und `CategoriesTableAdapter` in der Datenzugriffsebene, um diese Informationen umfassen, systemintern. Um dies zu erreichen, müssen wir eine neue Spalte hinzufügen `CategoriesDataTable` zum Aufnehmen der Anzahl der zugeordneten Produkte. Um eine neue Spalte zu einer "DataTable" hinzuzufügen, öffnen Sie das typisierte DataSet (`App_Code\DAL\Northwind.xsd`) mit der rechten Maustaste auf die DataTable zu ändern, und wählen Sie hinzufügen / Spalte. Hinzufügen eine neue Spalte, die `CategoriesDataTable` (siehe Abbildung 5).


[![Fügen Sie eine neue Spalte hinzu, um die CategoriesDataSource](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image14.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image13.png)

**Abbildung 5**: Hinzufügen einer neuen Spalte, die `CategoriesDataSource` ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image15.png))


Dadurch wird eine neue Spalte mit dem Namen hinzugefügt `Column1`, die Sie ändern können, indem Sie einfach in einen anderen Namen eingeben. Benennen Sie diese neue Spalte in `NumberOfProducts`. Als Nächstes müssen wir diese s-Spalteneigenschaften konfigurieren. Klicken Sie auf die neue Spalte, und wechseln Sie zum Fenster Eigenschaften. Ändern Sie die Spalte s `DataType` Eigenschaft aus `System.String` auf `System.Int32` und legen Sie die `ReadOnly` Eigenschaft `True`, wie in Abbildung 6 veranschaulicht.


![Legen Sie den Datentyp und ReadOnly-Eigenschaften der neuen Spalte](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image16.png)

**Abbildung 6**: Legen Sie die `DataType` und `ReadOnly` Eigenschaften der neuen Spalte.


Während der `CategoriesDataTable` verfügt jetzt über eine `NumberOfProducts` Spalte, dessen Wert nicht durch die entsprechenden TableAdapter s Abfragen festgelegt. Wir aktualisieren, können die `GetCategories()` Methode, um diese Informationen zurückzugeben, wenn wir möchten, dass solche Informationen zurückgegeben, jedes Mal, wenn Sie Informationen zu Auftragskategorien abgerufen wird. Wenn allerdings wir brauchen nur die Anzahl der zugeordneten Produkte für die Kategorien in seltenen Fällen (z. B. nur für dieses Lernprogramm) ziehen, dann wir lassen `GetCategories()` als-ist, und erstellen Sie eine neue Methode, die diese Informationen zurückgibt. Let s verwenden Sie diesen zweiten Ansatz, erstellen eine neue Methode mit dem Namen `GetCategoriesAndNumberOfProducts()`.

Um das Hinzufügen neuer `GetCategoriesAndNumberOfProducts()` -Methode, mit der rechten Maustaste auf die `CategoriesTableAdapter` , und wählen Sie die neue Abfrage. Dadurch wird auf der TableAdapter-Abfragen Konfigurations-Assistent, die wir gibt es mehrere Male in vorherigen Lernprogrammen verwendet. Starten Sie den Assistenten für diese Methode an, dass die Abfrage eine Ad-hoc-SQL-Anweisung verwendet, die Zeilen zurückgibt.


[![Erstellen Sie die Methode, die mit Ad-hoc-SQL-Anweisungen](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image18.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image17.png)

**Abbildung 7**: Erstellen Sie die Methode mithilfe einer Ad-hoc-SQL-Anweisung ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image19.png))


[![Die SQL-Anweisung gibt Zeilen zurück.](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image21.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image20.png)

**Abbildung 8**: die SQL-Anweisung gibt Zeilen ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image22.png))


Im nächste Assistentenbildschirm fordert "us" für die Abfrage verwendet. Jede Kategorie s zurückzugebenden `CategoryID`, `CategoryName`, und `Description` Felder sowie die Anzahl der Produkte, die der Kategorie zugeordnet ist, mithilfe der folgenden `SELECT` Anweisung:


[!code-sql[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample7.sql)]


[![Geben Sie die Abfrage zu verwendenden](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image24.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image23.png)

**Abbildung 9**: Geben Sie die Abfrage zu verwendenden ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image25.png))


Beachten Sie, dass die Unterabfrage, die die Anzahl der Produkte, die der Kategorie zugeordnet berechnet Alias `NumberOfProducts`. Diese Benennungskonvention Übereinstimmung führt dazu, dass der Rückgabewert von dieser Unterabfrage verknüpft werden soll die `CategoriesDataTable` s `NumberOfProducts` Spalte.

Im letzte Schritt werden nach der Eingabe dieser Abfrage, die Namen für die neue Methode auswählen. Verwendung `FillWithNumberOfProducts` und `GetCategoriesAndNumberOfProducts` für das Füllen einer DataTable und der Rückgabewert eine "DataTable" patterns, bzw.


[![Name der neuen TableAdapter s Methoden FillWithNumberOfProducts und GetCategoriesAndNumberOfProducts](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image27.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image26.png)

**Abbildung 10**: Benennen Sie die neuen TableAdapter-Methoden `FillWithNumberOfProducts` und `GetCategoriesAndNumberOfProducts` ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image28.png))


An diesem Punkt hat der Datenzugriffsebene erweitert wurde, um die Anzahl der Produkte pro Kategorie einzuschließen. Da unsere Darstellungsschicht leitet alle Aufrufe an die DAL durch eine separate Geschäftslogikschicht weiter müssen wir ein entsprechendes hinzufügen `GetCategoriesAndNumberOfProducts` Methode, um die `CategoriesBLL` Klasse:


[!code-csharp[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample8.cs)]

Der DAL und BLL abgeschlossen, es erneut bereit, diese Daten zu binden der `Categories` Repeater in `CategoriesAndProducts.aspx`! Sie haben bereits ein ObjectDataSource für Repeater aus der bestimmen erstellt die Anzahl der Produkte in der `ItemDataBound` Ereignishandler-Abschnitt, löschen Sie diese ObjectDataSource und Entfernen von Repeater s `DataSourceID` Eigenschaft auch ohne Kabel festlegen; die Repeater s `ItemDataBound` Ereignis des ereignishandlers durch das Entfernen der `Handles Categories.OnItemDataBound` Syntax in der ASP.NET Code-Behind-Klasse.

Mit den Repeater in seinem ursprünglichen Zustand zurück, Hinzufügen einer neuen ObjectDataSource mit dem Namen `CategoriesDataSource` über das Smarttag Repeater s. Konfigurieren der ObjectDataSource verwenden die `CategoriesBLL` -Klasse, aber anstatt sie verwenden die `GetCategories()` Methode haben sie verwendet `GetCategoriesAndNumberOfProducts()` stattdessen (siehe Abbildung 11).


[![Konfigurieren der ObjectDataSource zur Verwendung der GetCategoriesAndNumberOfProducts-Methode](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image30.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image29.png)

**Abbildung 11**: Konfigurieren der ObjectDataSource verwenden die `GetCategoriesAndNumberOfProducts` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image31.png))


Aktualisieren Sie als Nächstes die `ItemTemplate` , damit die LinkButton s `Text` Eigenschaft zugewiesen wird deklarativ mit Databinding-Syntax und enthält sowohl die `CategoryName` und `NumberOfProducts` Datenfelder. Das vollständige deklaratives Markup für die Repeater und `CategoriesDataSource` ObjectDataSource gehen Sie folgendermaßen vor:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample9.aspx)]

Die Ausgabe, die durch die DAL enthalten aktualisieren gerendert eine `NumberOfProducts` Spalte entspricht der Verwendung der `ItemDataBound` Ereignishandler Ansatz (verweisen zurück auf einen Bildschirm finden Sie unter Abbildung 4 Screenshot des Wiederholungsmoduls Kategorienamen und Anzahl der Produkte angezeigt).

## <a name="step-3-displaying-the-selected-category-s-products"></a>Schritt 3: Anzeigen der ausgewählten Kategorie-s-Produkte

An diesem Punkt haben wir die `Categories` Repeater Anzeigen der Liste der Kategorien zusammen mit der Anzahl der Produkte in jeder Kategorie. Repeater LinkButton verwendet, für die einzelnen Kategorien durch Klicken auf wird eine Postback handeln, bei dem zeigen wir, müssen Sie zum Anzeigen dieser Produkte für die ausgewählte Kategorie in den `CategoryProducts` DataList.

Eine Herausforderung, die für uns ist wie das DataList nur die Produkte für die ausgewählte Kategorie angezeigt. In der [Master/Detail mit auswählbaren Master GridView mit einer Details DetailsView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) Lernprogramm wurde erläutert, wie einer GridView, deren Zeilen erstellen konnten ausgewählt werden, mit der ausgewählten Zeile s details in einem DetailsView auf derselben Seite angezeigt wird. Die GridView s ObjectDataSource zurückgegebenen Informationen zu allen Produkten, die mit der `ProductsBLL` s `GetProducts()` -Methode auf, DetailsView-s ObjectDataSource abgerufenen Informationen zur Verwendung ausgewählten Produkt der `GetProductsByProductID(productID)` Methode. Die *`productID`* Parameterwert deklarativ bereitgestellt wurde, durch den Wert der GridView s zuordnen `SelectedValue` Eigenschaft. Leider Repeater verfügt nicht über eine `SelectedValue` Eigenschaft und kann nicht als Parameterquelle für fungieren.

> [!NOTE]
> Dies ist die Herausforderungen, die bei der Verwendung von LinkButton in ein Repeater angezeigt. Hatten wir einen Link für die Übergabe in verwendet das `CategoryID` über die Abfragezeichenfolge wir konnte verwenden Sie stattdessen das QueryString-Feld als Quelle für den Parameter-s-Wert.


Bevor wir kümmern der Mangel an eine `SelectedValue` -Eigenschaft für die Repeater kann jedoch s zuerst DataList an einem ObjectDataSource binden, und geben Sie ihre `ItemTemplate`.

Abonnieren Sie aus dem Smarttag DataList s, zum Hinzufügen einer neuen ObjectDataSource mit dem Namen `CategoryProductsDataSource` und konfigurieren es für das Verwenden der `ProductsBLL` Klasse s `GetProductsByCategoryID(categoryID)` Methode. Da das DataList in diesem Lernprogramm eine nur-Lese Schnittstelle bietet, können Sie der Dropdownlisten in den INSERT-, Update-, festlegen und Löschen von Registerkarten (keine).


[![Konfigurieren der ObjectDataSource um ProductsBLL s GetProductsByCategoryID(categoryID) Klassenmethode verwenden](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image33.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image32.png)

**Abbildung 12**: Konfigurieren der ObjectDataSource zu verwendenden `ProductsBLL` Klasse s `GetProductsByCategoryID(categoryID)` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image34.png))


Da die `GetProductsByCategoryID(categoryID)` Methode erwartet einen Eingabeparameter (*`categoryID`*), der Assistent zum Konfigurieren von Datenquellen ermöglicht es, die Parameter-s-Quelle angeben. Hatte die Kategorien wurden in eine GridView oder aufgeführt DataList, d legen wir die Parameterliste der Quelle Dropdown-Steuerelement und dem ControlID auf die `ID` Datenmenge Websteuerelement. Da die Repeater fehlt jedoch eine `SelectedValue` Eigenschaft kann nicht als Parameterquelle für den nicht verwendet werden. Wenn Sie überprüft haben, werden Sie feststellen, dass die ControlID Dropdown-Liste nur ein Steuerelement enthält `ID``CategoryProducts`die `ID` von DataList.

Dropdownliste für die Parameter-Quelle werden jetzt auf None festgelegt. Wir müssen am Ende programmgesteuert Zuweisen dieser Parameterwert, wenn eine Kategorie, die im Wiederholungsmodul LinkButton geklickt wird.


[![Führen Sie als Parameter-Quelle für die CategoryID-Parameter nicht angeben](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image36.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image35.png)

**Abbildung 13**: Geben Sie keine Parameterquelle für die *`categoryID`* Parameter ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image37.png))


Nach Abschluss des Assistenten für die Datenquelle konfigurieren Visual Studio generiert automatisch die DataList s `ItemTemplate`. Ersetzen Sie diese Standardeinstellung `ItemTemplate` wir mit der Vorlage verwendet wird, in dem vorherigen Lernprogramm; legen Sie außerdem die DataList s `RepeatColumns` -Eigenschaft auf 2. Nach diesen Änderungen sollte das deklarative Markup für DataList und seine zugehörigen ObjectDataSource wie folgt aussehen:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample10.aspx)]

Derzeit ist die `CategoryProductsDataSource` ObjectDataSource s *`categoryID`* Parameter nicht festgelegt ist, damit keine Produkte angezeigt werden, wenn die Seite anzeigen. Müssen wir handelt es sich dieser Parameterwert legen Sie auf der Grundlage der `CategoryID` der geklickt wurde Kategorie im wiederholungsmodul ab. Dies führt zu zwei Herausforderungen: zuerst, wie wir feststellen, wenn ein LinkButton im Wiederholungsmodul s `ItemTemplate` wurde geklickt wurde, und zweitens wie können wir ermitteln der `CategoryID` der entsprechenden Kategorie, deren LinkButton geklickt wurde?

Wie die Steuerelemente für Schaltfläche und ImageButton LinkButton verfügt über eine `Click` Ereignis und eine [ `Command` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.command.aspx). Die `Click` Ereignis dient lediglich Beachten Sie, dass die LinkButton geklickt wurde. In einigen Fällen müssen jedoch zusätzlich zu beachten, dass die LinkButton geklickt wurde es auch einige zusätzliche Informationen an den Ereignishandler übergeben. Wenn dies der Fall, die LinkButton s ist [ `CommandName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.commandname.aspx) und [ `CommandArgument` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.commandargument.aspx) Eigenschaften können diese zusätzlichen Informationen zugewiesen werden. Dann, wenn die LinkButton geklickt wird, dessen `Command` -Ereignis ausgelöst (anstelle von seiner `Click` Ereignis) und der Ereignishandler übergeben die Werte von der `CommandName` und `CommandArgument` Eigenschaften.

Wenn eine `Command` Ereignis wird von innerhalb einer Vorlage im wiederholungsmodul ab, die Repeater s [ `ItemCommand` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeater.itemcommand.aspx) ausgelöst wird, und übergeben der `CommandName` und `CommandArgument` Werte der LinkButton geklickt wurde (oder die Schaltfläche oder ImageButton). Um zu bestimmen, wann eine Kategorie LinkButton im Wiederholungsmodul auf den geklickt wurde, müssen wir daher wie folgt vorgehen:

1. Legen Sie die `CommandName` Eigenschaft LinkButton im Wiederholungsmodul s `ItemTemplate` auf einen beliebigen Wert (ich Ve verwendet ListProducts). Durch Festlegen dieses `CommandName` Wert, der LinkButton s `Command` Ereignis wird ausgelöst, wenn die LinkButton geklickt wird.
2. Legen Sie die s LinkButton `CommandArgument` -Eigenschaft auf den Wert des aktuellen Elements s `CategoryID`.
3. Erstellen Sie einen Ereignishandler für die Repeater s `ItemCommand` Ereignis. Im Ereignishandler, Festlegen der `CategoryProductsDataSource` ObjectDataSource s `CategoryID` auf den Wert des übergebenen `CommandArgument`.

Die folgenden `ItemTemplate` Markup für die Kategorien Repeater implementiert die Schritte 1 und 2. Hinweis wie die `CommandArgument` Wert wird zugewiesen, das Datenelement s `CategoryID` Databinding-Syntax verwenden:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample11.aspx)]

Bei jedem Erstellen einer `ItemCommand` Ereignishandler, es ist unerlässlich, überprüfen Sie immer zuerst die eingehende `CommandName` bewertet werden, da *alle* `Command` Ereignis ausgelöst wird, indem Sie *alle* Button, LinkButton oder ImageButton innerhalb der Repeater führt dazu, dass die `ItemCommand` Ereignis ausgelöst. Während wir derzeit nur eine solche LinkButton jetzt haben, in der Zukunft wir (oder ein anderer Entwickler auf unserem) kann zum Hinzufügen, zusätzliche Web-Schaltflächensteuerelemente Repeater, wenn angeklickt, löst die gleiche `ItemCommand` -Ereignishandler. Daher es s sollten Sie immer sicherstellen, dass Sie überprüfen die `CommandName` Eigenschaft und die programmgesteuerte Logik nur fort, sofern es mit dem erwarteten Wert entspricht, einrichten.

Nachdem sichergestellt wurde, dass das übergebene in `CommandName` Wert gleich ListProducts ist, dann weist des ereignishandlers die `CategoryProductsDataSource` ObjectDataSource s `CategoryID` auf den Wert des übergebenen `CommandArgument`. Diese Änderung mit dem ObjectDataSource s `SelectParameters` automatisch bewirkt, dass das DataList selbst mit der Datenquelle, mit der Produkte für die neu ausgewählte Kategorie erneut binden.


[!code-csharp[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample12.cs)]

Mit dieser Ergänzungen ist unser Tutorial abgeschlossen! Nehmen Sie einen Moment Zeit, um es in einem Browser zu testen. Abbildung 14 zeigt den Bildschirm an, wenn zuerst die Seite besuchen. Da eine Kategorie noch werden ausgewählt muss, werden keine Produkte angezeigt. Durch Klicken auf eine Kategorie, z. B. erzeugen, die Produkte der Produktkategorien in einer Ansicht mit zwei Spalten angezeigt (siehe Abbildung 15).


[![Keine Produkte werden angezeigt, beim ersten Besuch der Seite "](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image39.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image38.png)

**Abbildung 14**: No Produkte sind angezeigt beim ersten Besuch der Seite "([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image40.png))


[![Klicken Sie auf die Kategorie erstellt werden sollen, Listen Sie die entsprechenden Produkte nach rechts](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image42.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image41.png)

**Abbildung 15**: auf die Kategorie erzeugen Listet die Produkte entsprechen, auf der rechten Seite ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image43.png))


## <a name="summary"></a>Zusammenfassung

Wie wir in diesem Lernprogramm und vorangehende gesehen haben, können Master/Detail-Berichte über zwei Seiten verteilt oder auf einem konsolidiert. Einen Master-/Detail-Bericht auf einer einzelnen Seite angezeigt, stellt jedoch eine Herausforderung dar wie am besten zum Layout der Master und Details Datensätze auf der Seite. In der *Master/Detail mit auswählbaren Master GridView mit einer Details DetailsView* Tutorial, mussten wir die Details-Datensätze, die über die master-Datensätze angezeigt werden; in diesem Lernprogramm wir CSS-Techniken verwendet, damit die Masterdatensätze "float", um die Links von den Details.

Zusammen mit der Anzeige von Master-/Detail-Berichte, mussten wir auch die Möglichkeit, untersuchen zum Abrufen der Anzahl der Produkte, die auch jede Kategorie zugeordnet, und wie die serverseitige Logik ausführen, wenn eine LinkButton (Schaltfläche oder ImageButton) geklickt wird, innerhalb einer Repeater.

Dieses Lernprogramm ist unsere Untersuchung von Master/Detail-Berichte mit dem DataList und Repeater abgeschlossen. Unsere nächste Satz von Lernprogrammen wird zum Hinzufügen, bearbeiten und Löschen von Funktionen, um das DataList-Steuerelement veranschaulicht.

Viel Spaß beim Programmieren!

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Lernprogramm erläutert finden Sie in den folgenden Ressourcen:

- [Floatutorial](http://css.maxdesign.com.au/floatutorial/) ein Tutorial zur CSS-Elementen durch CSS Gleitkomma
- [CSS-Positionierung](http://www.brainjar.com/css/positioning/) Weitere Informationen zum Positionieren von Elementen durch CSS
- [Zur Festlegung von Out-Inhalt mit HTML](http://www.w3schools.com/html/html_layout.asp) mit `<table>` s und andere HTML-Elemente für die Positionierung

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurde Zack Jones. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](master-detail-filtering-acess-two-pages-datalist-cs.md)
> [Weiter](master-detail-filtering-with-a-dropdownlist-datalist-vb.md)
