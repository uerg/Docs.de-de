---
uid: web-forms/overview/data-access/introduction/creating-a-business-logic-layer-vb
title: Erstellen einer Geschäftslogikebene (VB) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial wird gezeigt, wie Ihre Geschäftsregeln in eine Geschäftslogikschicht (Business Logic Layer, BLL) zentralisieren, das als Zwischenstufe für den Datenaustausch zwischen t dient...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 142e5181-29ce-4bb9-907b-2a0becf7928b
msc.legacyurl: /web-forms/overview/data-access/introduction/creating-a-business-logic-layer-vb
msc.type: authoredcontent
ms.openlocfilehash: 345f4981ebdd5384068bd42bce0581f94866ad1d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831360"
---
<a name="creating-a-business-logic-layer-vb"></a>Erstellen einer Geschäftslogikebene (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunter](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_2_VB.exe) oder [PDF-Datei herunterladen](creating-a-business-logic-layer-vb/_static/datatutorial02vb1.pdf)

> In diesem Tutorial sehen wir, wie Sie Ihre Geschäftsregeln in eine Geschäftslogikschicht (Business Logic Layer, BLL) zu zentralisieren, das als Zwischenstufe für den Datenaustausch zwischen Präsentationsebene und der DAL dient.


## <a name="introduction"></a>Einführung

(Data Access Layer, DAL) erstellt, der [ersten Tutorial](creating-a-data-access-layer-vb.md) sauber unterteilt die Daten die Darstellungslogik Logik zugreifen. Jedoch während die DAL ordnungsgemäß die Details der Zugriff auf der Darstellungsschicht trennt, ist es Business erzwingen keine Regeln, die angewendet werden kann. Beispielsweise sollten wir für unsere Anwendung verweigert die `CategoryID` oder `SupplierID` Felder der `Products` Tabelle geändert werden, wenn die `Discontinued` Feld auf 1 festgelegt ist, oder wir können Betriebszugehörigkeit Regeln erzwingen Verbot Situationen, in denen ein Mitarbeiter wird von einer Person verwaltet, die Sie eingestellt wurde. Ein weiteres gängiges Szenario ist Autorisierung vielleicht nur Benutzer in einer bestimmten Rolle können Produkte löschen oder Ändern der `UnitPrice` Wert.

In diesem Tutorial sehen wir, wie Sie die folgende Geschäftsregeln in eine Geschäftslogikschicht (Business Logic Layer, BLL) zu zentralisieren, das als Zwischenstufe für den Datenaustausch zwischen Präsentationsebene und der DAL dient. In einer echten Anwendung sollte die BLL als ein separates Klassenbibliotheksprojekt implementiert werden. allerdings für diese Tutorials implementieren wir die BLL als eine Reihe von Klassen in unserer `App_Code` Ordner aus, um die Projektstruktur zu vereinfachen. Abbildung 1 zeigt die Architekturen Beziehungen zwischen der Präsentationsebene BLL- und DAL.


![Die Geschäftslogikschicht trennt die Darstellungsschicht von der Datenzugriffsebene und erzwingt von Geschäftsregeln](creating-a-business-logic-layer-vb/_static/image1.png)

**Abbildung 1**: die BLL trennt die Darstellungsschicht von der Datenzugriffsebene und erzwingt von Geschäftsregeln


Anstatt das Erstellen von separaten Klassen implementiert unsere [Geschäftslogik](http://en.wikipedia.org/wiki/Business_logic), wir konnten diese Logik auch direkt in das typisierte DataSet mit partiellen Klassen platzieren. Ein Beispiel zum Erstellen und Erweitern eines typisierten Datasets finden Sie im ersten Tutorial zurück.

## <a name="step-1-creating-the-bll-classes"></a>Schritt 1: Erstellen der BLL-Klassen

Unsere BLL werden vier Klassen, die eine für jeden TableAdapter in die DAL erstellt werden; Jede dieser Klassen BLL müssen die Methoden zum Abrufen, einfügen, aktualisieren und löschen aus der jeweiligen TableAdapter in die DAL aus, die entsprechenden Geschäftsregeln anwenden.

Um mehr sauber trennen Sie die DAL - und BLL-bezogenen Klassen, erstellen wir zwei Unterordner, in der `App_Code` Ordner `DAL` und `BLL`. Einfach mit der rechten Maustaste auf die `App_Code` Ordner im Projektmappen-Explorer, und wählen Sie die neuen Ordner. Verschieben Sie nach dem erstellen diese beiden Ordner an, die typisierte DataSet erstellt, die im ersten Tutorial in der `DAL` Unterordner.

Erstellen Sie als Nächstes die vier Dateien der BLL-Klasse in der `BLL` Unterordner. Zu diesem Zweck mit der Maustaste auf die `BLL` Unterordner, wählen Sie ein neues Element hinzufügen, und wählen Sie die Klassenvorlage. Benennen Sie die vier Klassen `ProductsBLL`, `CategoriesBLL`, `SuppliersBLL`, und `EmployeesBLL`.


![Vier neue Klassen in den Ordner "App_Code" hinzufügen](creating-a-business-logic-layer-vb/_static/image2.png)

**Abbildung 2**: Fügen Sie vier neue Klassen in der die `App_Code` Ordner


Als Nächstes fügen Sie Methoden auf jede der Klassen, die für die TableAdapter-Steuerelemente aus dem ersten Lernprogramm definierten Methoden einfach zu umschließen. Jetzt werden diese Methoden nur direkt in die DAL aufrufen. höher, um alle erforderlichen Geschäftslogik hinzufügen, zurückgegeben.

> [!NOTE]
> Bei Verwendung von Visual Studio Standard Edition oder höher (d. h., Sie sind *nicht* mit Visual Web Developer), können Sie optional Ihre Klassen, die mithilfe von visuell entwerfen der [Klassen-Designer](https://msdn.microsoft.com/library/default.asp?url=/library/dv_vstechart/html/clssdsgnr.asp). Finden Sie in der [Klasse Designer Blog](https://blogs.msdn.com/classdesigner/default.aspx) für Weitere Informationen zu diesem neuen Feature in Visual Studio.


Für die `ProductsBLL` Klasse wir insgesamt sieben Methoden hinzufügen müssen:

- `GetProducts()` Alle Produkte zurückgegeben
- `GetProductByProductID(productID)` Gibt das Produkt mit der angegebenen Produkt-ID
- `GetProductsByCategoryID(categoryID)` Gibt alle Produkte aus der angegebenen Kategorie zurück.
- `GetProductsBySupplier(supplierID)` Gibt alle Produkte vom angegebenen Anbieter zurück.
- `AddProduct(productName, supplierID, categoryID, quantityPerUnit, unitPrice, unitsInStock, unitsOnOrder, reorderLevel, discontinued)` Fügt ein neues Produkt in die Datenbank, die mit den Werten übergegebenen; Gibt die `ProductID` Wert des neu eingefügten Datensatzes
- `UpdateProduct(productName, supplierID, categoryID, quantityPerUnit, unitPrice, unitsInStock, unitsOnOrder, reorderLevel, discontinued, productID)` aktualisiert ein vorhandenes Produkt mit die übergebenen Werte in der Datenbank; Gibt `True` wenn genau eine Zeile aktualisiert wurde, `False` andernfalls
- `DeleteProduct(productID)` Löscht das angegebene Produkt aus der Datenbank

ProductsBLL.vb


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample1.vb)]

Die Methoden, die Daten zurückgeben `GetProducts`, `GetProductByProductID`, `GetProductsByCategoryID`, und `GetProductBySuppliersID` sind eigentlich ziemlich einfach, wie sie in der DAL einfach nach unten rufen. In einigen Szenarien besteht möglicherweise Geschäftsregeln, die implementiert werden müssen auf dieser Ebene (z. B. die Autorisierungsregeln basierend auf den derzeit angemeldeten Benutzer oder die Rolle, zu denen der Benutzer gehört), lassen wir einfach diese Methoden als-ist. Für diese Methode dient dann die BLL nur als ein Proxy, der durch den die Darstellungsschicht die zugrunde liegenden Daten aus der Datenzugriffsebene zugreift.

Die `AddProduct` und `UpdateProduct` Methoden sowohl sind als Parameter die Werte für die verschiedenen Product-Felder und ein neues Produkt hinzufügen oder Aktualisieren eines bereits vorhandenen, bzw. Da viele der `Product` Tabellenspalten lässt `NULL` Werte (`CategoryID`, `SupplierID`, und `UnitPrice`, um nur einige zu nennen), die Eingabeparameter für `AddProduct` und `UpdateProduct` , die solche Nutzung dann Spalten zugeordnet[auf NULL festlegbare Typen](https://msdn.microsoft.com/library/1t3y8s4s(v=vs.80).aspx). Auf NULL festlegbare Typen sind neu in .NET 2.0 und eine Technik bereitzustellen, für sein, der angibt, ob ein Werttyp, stattdessen sollten `Nothing`. Finden Sie in der [Paul Vick](http://www.panopticoncentral.net/)Blogeintrag [die Wahrheit über Nullable-Typen und VB](http://www.panopticoncentral.net/archive/2004/06/04/1180.aspx) und die technische Dokumentation für die [Nullable](https://msdn.microsoft.com/library/b3h38hb0%28VS.80%29.aspx) -Struktur für Weitere Informationen.

Alle drei Methoden geben einen booleschen Wert, der angibt, ob eine Zeile eingefügt, aktualisiert oder gelöscht, da der Vorgang nicht in einer betroffenen Zeile entstehen wurde zurück. Wenn der Seitenentwickler ruft z. B. `DeleteProduct` übergeben eine `ProductID` für ein Produkt nicht existierende der `DELETE` -Anweisung mit der Datenbank hat keine Auswirkungen und somit auch die `DeleteProduct` Methode zurück `False`.

Beachten Sie, dass, wenn Sie ein neues Produkt hinzufügen oder aktualisieren eine vorhandene wir nehmen bei Feldwerten für das neue oder geänderte Produkt als eine Liste von skalaren im Gegensatz zu akzeptieren einen `ProductsRow` Instanz. Dieser Ansatz wurde gewählt, weil die `ProductsRow` Klasse leitet sich von den ADO.NET `DataRow` -Klasse, die nicht über einen parameterlosen Standardkonstruktor verfügt. Zum Erstellen eines neuen `ProductsRow` -Instanz muss zunächst erstellen wir eine `ProductsDataTable` -Instanz, und rufen Sie dann die `NewProductRow()` Methode (was der Fall ist im `AddProduct`). Diese Unzulänglichkeit rears den Kopf, wenn wir aktivieren, um das Einfügen und Aktualisieren von Produkten, die mit dem ObjectDataSource-Steuerelement. Kurz gesagt, versucht das ObjectDataSource-Steuerelement zum Erstellen einer Instanz der Eingabeparameter. Wenn die BLL-Methode erwartet, dass eine `ProductsRow` -Instanz versucht dem ObjectDataSource-Steuerelement erstellen, aber aufgrund des Mangels an einen Standardkonstruktor ohne Parameter fehl. Weitere Informationen zu diesem Problem finden Sie in den folgenden zwei ASP.NET-Foren-Beiträgen: [ObjectDataSources Strongly-Typed Datasets aktualisieren](https://forums.asp.net/1098630/ShowPost.aspx), und [Problem mit "ObjectDataSource" und Strongly-Typed DataSet](https://forums.asp.net/1048212/ShowPost.aspx).

Als Nächstes wird in beiden `AddProduct` und `UpdateProduct`, erstellt der Code eine `ProductsRow` -Instanz, und füllt sie mit der gerade übergebenen Werte. Beim Zuweisen von Werten zu DataColumns einer DataRow möglich, verschiedene Überprüfungen auf Feldebene. Aus diesem Grund trägt der übergebenen Werte manuell wieder in einer DataRow platzieren die Gültigkeit der Daten, die an die BLL-Methode übergeben wird. Leider verwenden die stark typisierte DataRow-Klassen generiert, die von Visual Studio nicht auf NULL festlegbare Typen. Um anzugeben, dass eine bestimmte DataColumn in einer DataRow entsprechen muss eine `NULL` Datenbank Wert muss die `SetColumnNameNull()` Methode.

In `UpdateProduct` zuerst laden wir das Produkt, das update mit `GetProductByProductID(productID)`. Dies mag wie eine unnötige Roundtrips zur Datenbank, werden diese zusätzlichen Reise lohnt sich in zukünftigen Lernprogrammen zu prüfen, die vollständigen Parallelität zu untersuchen. Vollständige Parallelität ist ein Verfahren, um sicherzustellen, dass zwei Benutzer, die gleichzeitig auf denselben Daten arbeiten nicht versehentlich die Änderungen überschrieben. Abrufen des gesamten Datensatzes erleichtert auch zum Erstellen von Update-Methoden in der BLL, die nur eine Teilmenge der DataRow-Spalten zu ändern. Wenn wir untersuchen die `SuppliersBLL` Klasse, die wir sehen uns ein Beispiel dafür.

Beachten Sie schließlich, dass die `ProductsBLL` -Klasse verfügt über die [DataObject-Attributs](https://msdn.microsoft.com/library/system.componentmodel.dataobjectattribute.aspx) angewendet wird (die `[System.ComponentModel.DataObject]` Syntax direkt vor der Class-Anweisung am Anfang der Datei) und die Methoden verfügen über [ DataObjectMethodAttribute Attribute](https://msdn.microsoft.com/library/system.componentmodel.dataobjectmethodattribute.aspx). Die `DataObject` -Attribut markiert die Klasse als ein Objekt, das Binden an eine [ObjectDataSource-Steuerelement](https://msdn.microsoft.com/library/9a4kyhcx.aspx), während die `DataObjectMethodAttribute` gibt den Zweck der Methode. In zukünftigen Lernprogrammen sehen, erleichtert ASP.NET 2.0 "ObjectDataSource" deklarativ Zugriff auf Daten aus einer Klasse. Damit können Filtern der Liste der möglichen Klassen im Assistenten für das ObjectDataSource-Steuerelement zu binden, werden standardmäßig nur die Klassen, die als `DataObjects` werden in der Dropdownliste des Assistenten angezeigt. Die `ProductsBLL` Klasse funktioniert genauso gut ohne diese Attribute, aber Hinzufügens erleichtert es, mit dem ObjectDataSource-Steuerelement-Assistenten zu arbeiten.

## <a name="adding-the-other-classes"></a>Hinzufügen von anderen Klassen

Mit der `ProductsBLL` Klasse nun vollständig, noch müssen Sie die Klassen für das Arbeiten mit Kategorien, Lieferanten und Mitarbeiter hinzufügen. Nehmen Sie einen Moment Zeit, um die folgenden Klassen und Methoden, die mit den Konzepten aus dem obigen Beispiel zu erstellen:

- **CategoriesBLL.cs**

    - `GetCategories()`
    - `GetCategoryByCategoryID(categoryID)`
- **SuppliersBLL.cs**

    - `GetSuppliers()`
    - `GetSupplierBySupplierID(supplierID)`
    - `GetSuppliersByCountry(country)`
    - `UpdateSupplierAddress(supplierID, address, city, country)`
- **EmployeesBLL.cs**

    - `GetEmployees()`
    - `GetEmployeeByEmployeeID(employeeID)`
    - `GetEmployeesByManager(managerID)`

Beachten Sie die Methode ist die `SuppliersBLL` Klasse `UpdateSupplierAddress` Methode. Diese Methode stellt eine Schnittstelle für die Aktualisierung nur Informationen zur Adresse des Lieferanten. Intern wird diese Methode liest, der `SupplierDataRow` -Objekt für die angegebene `supplierID` (mit `GetSupplierBySupplierID`), dessen Eigenschaften, und klicken Sie dann nach unten in der `SupplierDataTable`des `Update` Methode. Die `UpdateSupplierAddress` folgt:


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample2.vb)]

Finden Sie in der Download dieses Artikels, für diese vollständige Implementierung der BLL-Klassen.

## <a name="step-2-accessing-the-typed-datasets-through-the-bll-classes"></a>Schritt 2: Zugreifen auf die typisierten DataSets über die BLL-Klassen

Im ersten Tutorial erläutert, Beispiele für das arbeiten direkt mit dem typisierten DataSet programmgesteuert sollten, durch das Hinzufügen von die BLL-Klassen, die Präsentationsebene jedoch funktionieren für die BLL stattdessen. In der `AllProducts.aspx` Beispiel aus dem ersten Lernprogramm, das `ProductsTableAdapter` so binden die Liste der Produkte an einer GridView-Ansicht verwendet wurde, wie im folgenden Code gezeigt:


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample3.vb)]

Verwenden Sie die neue BLL Klassen, das geändert werden muss, ist einfach die erste Zeile des Codes ersetzen die `ProductsTableAdapter` Objekt mit einem `ProductBLL` Objekt:


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample4.vb)]

Die BLL-Klassen können auch mit dem ObjectDataSource-Steuerelement deklarativ (da die typisierte DataSet) zugegriffen werden. Wir werden ausführlicher zu "ObjectDataSource" in den folgenden Tutorials erläutern.


[![Die Liste der Produkte wird in einer GridView-Ansicht angezeigt.](creating-a-business-logic-layer-vb/_static/image4.png)](creating-a-business-logic-layer-vb/_static/image3.png)

**Abbildung 3**: die Liste der Produkte in einer GridView-Ansicht angezeigt wird ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-business-logic-layer-vb/_static/image5.png))


## <a name="step-3-adding-field-level-validation-to-the-datarow-classes"></a>Schritt 3: Hinzufügen von Feldebenenvalidierung für die DataRow-Klassen

Feldebenenvalidierung sind Überprüfungen, die die Eigenschaftswerte die Geschäftsobjekte beim Einfügen oder Aktualisieren von betrifft. Einige Regeln feldebenenvalidierung für Produkte umfassen:

- Die `ProductName` Feld muss 40 Zeichen oder weniger lang sein.
- Die `QuantityPerUnit` Feld muss 20 Zeichen oder weniger lang sein.
- Die `ProductID`, `ProductName`, und `Discontinued` Felder sind erforderlich, aber alle anderen Felder sind optional
- Die `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, und `ReorderLevel` Felder müssen größer als oder gleich 0 (null) sein.

Diese Regeln können und auf Datenbankebene ausgedrückt werden. Die Zeichenlänge für den `ProductName` und `QuantityPerUnit` Felder werden erfasst, von den Datentypen dieser Spalten in der `Products` Tabelle (`nvarchar(40)` und `nvarchar(20)`bzw.). Gibt an, ob Felder erforderlichen und optionalen werden durch ausgedrückt werden, wenn die Datenbanktabellen-Spalte erlaubt `NULL` s. Vier [check-Einschränkungen](https://msdn.microsoft.com/library/ms188258.aspx) vorhanden sind nur Werte größer als oder gleich 0 (null) in herstellen können, sicherzustellen, dass die `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, oder `ReorderLevel` Spalten.

Zusätzlich zum Erzwingen dieser Regeln in der Datenbank sollten sie auch auf Datensatzebene erzwungen werden. In der Tat werden die Feldlänge und gibt an, ob ein Wert erforderlich oder optional ist bereits für jede DataTable-Satz von DataColumns erfasst. Um die vorhandenen feldebenenvalidierung automatisch anzuzeigen, wechseln Sie zur DataSet-Designer, wählen Sie ein Feld aus einem der vorhandenen DataTables, und fahren Sie mit dem Fenster "Eigenschaften". Wie in Abbildung 4 gezeigt, die `QuantityPerUnit` DataColumn in die `ProductsDataTable` hat eine maximale Länge von 20 Zeichen und lässt `NULL` Werte. Wenn wir versuchen, legen Sie die `ProductsDataRow`des `QuantityPerUnit` Eigenschaft in einen Zeichenfolgenwert, der mehr als 20 Zeichen ein `ArgumentException` ausgelöst.


[![Die Datenspalte stellt grundlegende Feldebenenvalidierung](creating-a-business-logic-layer-vb/_static/image7.png)](creating-a-business-logic-layer-vb/_static/image6.png)

**Abbildung 4**: der DataColumn bietet grundlegende Feldebenenvalidierung ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-business-logic-layer-vb/_static/image8.png))


Leider kann nicht angegeben Grenzen Überprüfungen, z. B. die `UnitPrice` Wert muss größer als oder gleich 0 (null), über das Eigenschaftenfenster. Um diese Art von feldebenenvalidierung bereitstellen müssen wir einen Ereignishandler für die der DataTable erstellen [ColumnChanging](https://msdn.microsoft.com/library/system.data.datatable.columnchanging%28VS.80%29.aspx) Ereignis. Siehe die [vorherigen Lernprogramm](creating-a-data-access-layer-vb.md), die Datasets, DataTables und DataRow-Objekte, die durch das typisierte DataSet erstellt, die durch die Verwendung von partiellen Klassen erweitert werden können. Mithilfe dieser Technik können wir erstellen eine `ColumnChanging` -Ereignishandler für die `ProductsDataTable` Klasse. Zunächst erstellen Sie eine Klasse in der `App_Code` Ordner mit dem Namen `ProductsDataTable.ColumnChanging.vb`.


[![Fügen Sie eine neue Klasse, zu dem Ordner "App_Code"](creating-a-business-logic-layer-vb/_static/image10.png)](creating-a-business-logic-layer-vb/_static/image9.png)

**Abbildung 5**: Fügen Sie eine neue Klasse, die `App_Code` Ordner ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-business-logic-layer-vb/_static/image11.png))


Als Nächstes erstellen Sie einen Ereignishandler für die `ColumnChanging` -Ereignis, das wird, dass sichergestellt die `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, und `ReorderLevel` Spaltenwerte (Wenn dies nicht der `NULL`) größer als oder gleich 0 (null). Wenn solche Spalte außerhalb des gültigen Bereichs ist, löst eine `ArgumentException`.

ProductsDataTable.ColumnChanging.vb


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample5.vb)]

## <a name="step-4-adding-custom-business-rules-to-the-blls-classes"></a>Schritt 4: Hinzufügen von benutzerdefinierten Geschäftsregeln für die BLL Klassen

Zusätzlich zu feldebenenvalidierung möglicherweise auf hoher Ebene benutzerdefinierte Geschäftsregeln, die unterschiedliche Entitäten oder Konzepte, die Ebene der einzelnen Spalte nicht ausgedrückt werden kann, wie z. B. umfassen:

- Wenn ein Produkt nicht mehr lieferbar ist, dessen `UnitPrice` kann nicht aktualisiert werden
- Herkunftsland eines Mitarbeiters muss ihren Vorgesetzten Herkunftsland identisch sein.
- Ein Produkt kann nicht eingestellt werden, ist dies das einzige Produkt, die von den Lieferanten bereitgestellte

Die BLL Klassen sollten Überprüfungen zum Sicherstellen der Einhaltung von der Anwendung von Geschäftsregeln enthalten. Diese Überprüfungen können direkt auf die Methoden hinzugefügt werden, für die sie gelten.

Angenommen Sie, die unsere Geschäftsregeln vorgeben, dass ein Produkt nicht nicht mehr unterstützte gekennzeichnet werden konnte, wenn es das einzige Produkt von einem bestimmten Lieferanten war. D. h. wenn Produkt *X* das einzige Produkt, das wir vom Lieferanten erworben wurde *Y*, wir konnte nicht gekennzeichnet werden *X* wie; wenn Sie nicht mehr unterstützt jedoch Lieferanten *Y*uns mit drei Produkte bereitgestellten *ein*, *B*, und *C*konnten wir alle markieren und anschließend alle diese als nicht mehr unterstützt. Eine ungerade Geschäftsregel, aber von Geschäftsregeln und gesundem Menschenverstand zu tun, sind nicht immer ausgerichtet.

Erzwingen Sie diese Geschäftsregel in die `UpdateProducts` Methode, die wir überprüft zunächst, ob `Discontinued` wurde `True` und, wenn wir also aufrief `GetProductsBySupplierID` um zu bestimmen, wie viele Produkte, die wir von anderen Lieferanten des Produkts erworben. Wenn nur ein Produkt aus diesem Lieferanten erworben wird, lösen wir eine `ApplicationException`.


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample6.vb)]

## <a name="responding-to-validation-errors-in-the-presentation-tier"></a>Reagieren auf Validierungsfehler, die in der Präsentationsebene

Beim Aufrufen der BLL von der Präsentationsebene können wir entscheiden, ob versucht, alle Ausnahmen zu behandeln, die möglicherweise ausgelöst werden, oder kann es bis zu ASP.NET zu übergeben (ausgelöst der `HttpApplication`des `Error` Ereignis). Um eine Ausnahme behandeln, wenn Sie mit der BLL programmgesteuert arbeiten, können wir eine [testen... Abfangen](https://msdn.microsoft.com/library/fk6t46tz%28VS.80%29.aspx) blockieren, wie im folgenden Beispiel gezeigt:


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample7.vb)]

Wie wir uns in Zukunft Lernprogramme sehen, Behandlung von Ausnahmen, die Paketfehler der BLL, die bei Verwendung eines-Steuerelement für das Einfügen Webserver, aktualisieren oder Löschen von Daten direkt in einem Ereignishandler statt Code zu umschließen verarbeitet werden kann `Try...Catch` Blöcke.

## <a name="summary"></a>Zusammenfassung

Eine gut entwickelte Anwendung ist in unterschiedliche Ebenen erstellt, von denen jede eine bestimmte Rolle umfassen. Im ersten Tutorial dieser Reihe Artikel haben wir eine Datenzugriffsschicht, die mit typisierten DataSets erstellt; In diesem Tutorial wir eine Geschäftslogikebene als erstellt eine Reihe von Klassen in der Anwendung `App_Code` Ordner, der in der DAL aufrufen. Die Geschäftslogikschicht implementiert die Logik auf und Business-Ebene für die Anwendung. Zusätzlich zur Erstellung einer separaten BLL, wie in diesem Tutorial ist eine weitere Option die TableAdapter Methoden durch die Verwendung von partiellen Klassen zu erweitern. Mithilfe dieser Technik ist gestattet es uns nicht zum Überschreiben der vorhandener Methoden werden jedoch noch ist es trennen DAL und unsere BLL ordnungsgemäß als der Ansatz, den wir in diesem Artikel erstellt haben.

Der DAL und der BLL abgeschlossen können wir auf unsere Darstellungsschicht zu starten. In der [nächsten Tutorial](master-pages-and-site-navigation-vb.md) wir einen kurzen Umweg in den Themen zum Zugriff auf Daten nehmen und ein Seitenlayout einheitliche für die Verwendung in den Tutorials zu definieren.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial wurden Liz Shulok, Dennis Patterson, Carlos Santos und Hilton Giesenow. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](creating-a-data-access-layer-vb.md)
> [Weiter](master-pages-and-site-navigation-vb.md)
