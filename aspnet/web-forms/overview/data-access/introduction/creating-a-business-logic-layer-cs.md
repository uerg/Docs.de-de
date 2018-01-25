---
uid: web-forms/overview/data-access/introduction/creating-a-business-logic-layer-cs
title: "Erstellen eine Geschäftslogikschicht (c#) | Microsoft Docs"
author: rick-anderson
description: "In diesem Lernprogramm erfahren wir, wie Sie Ihre Geschäftsregeln in einem Business Logic Layer (BLL) zentralisieren, die dient als Mittler für den Datenaustausch zwischen t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 85554606-47cb-4e4f-9848-eed9da579056
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/introduction/creating-a-business-logic-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: 7518ddd11a05a9e3d5df85e3cf6ceffa09a25060
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="creating-a-business-logic-layer-c"></a>Erstellen eine Geschäftslogikschicht (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunterladen](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_2_CS.exe) oder [PDF herunterladen](creating-a-business-logic-layer-cs/_static/datatutorial02cs1.pdf)

> In diesem Lernprogramm erfahren wir, wie Sie Ihre Geschäftsregeln in einem Business Logic Layer (BLL) zentralisieren, die als Vermittler für den Datenaustausch zwischen der Präsentationsschicht und der DAL dient.


## <a name="introduction"></a>Einführung

(Data Access Layer, DAL) erstellt, der [ersten Lernprogramm](creating-a-data-access-layer-cs.md) ordnungsgemäß trennt die Daten die Präsentationslogik Logik zugreifen. Jedoch während die DAL saubersten die Zugriffsdetails Daten aus der Darstellungsschicht trennt, werden sie keine Geschäftsregeln erzwungen, die gelten können. Beispielsweise sollten wir für unsere Anwendung zu unterbinden, die `CategoryID` oder `SupplierID` Felder von der `Products` Tabelle geändert werden, wenn die `Discontinued` Feld auf 1 festgelegt ist, oder wir können Betriebszugehörigkeit-Regeln erzwungen Verbot Situationen, in denen ein Mitarbeiter wird von einer Person verwaltet, nachdem sie eingestellt wurde. Ein weiteres gängiges Szenario ist Autorisierung vielleicht nur Benutzer in einer bestimmten Rolle Löschen von Produkten oder ändern, können die `UnitPrice` Wert.

In diesem Lernprogramm sehen wir, wie diese Geschäftsregeln in einem Business Logic Layer (BLL) zentralisieren, die als Vermittler für den Datenaustausch zwischen der Präsentationsschicht und der DAL dient. In einer realen Anwendung sollte die BLL als ein separates Klassenbibliotheksprojekt implementiert werden. allerdings für diese Lernprogramme implementieren wir die BLL als eine Reihe von Klassen in unserer `App_Code` Ordner, um die Projektstruktur zu vereinfachen. Abbildung 1 zeigt die Architektur Beziehungen zwischen der Präsentationsschicht, BLL und DAL.


![Die BLL trennt die Darstellungsschicht von der Datenzugriffsebene und erzwingt Geschäftsregeln](creating-a-business-logic-layer-cs/_static/image1.png)

**Abbildung 1**: die BLL trennt die Darstellungsschicht von der Datenzugriffsebene und erzwingt Geschäftsregeln


## <a name="step-1-creating-the-bll-classes"></a>Schritt 1: Erstellen der BLL-Klassen

Unsere BLL wird aus vier Klassen: eine für jeden TableAdapter in der DAL bestehen; Jede dieser Klassen BLL müssen Methoden zum Abrufen, einfügen, aktualisieren und Löschen von der jeweiligen TableAdapter in die DAL die entsprechenden Geschäftsregeln anwenden.

Der DAL - und BLL-bezogene Klassen um deutlicher zu trennen, erstellen wir zwei Unterordner in der `App_Code` Ordner `DAL` und `BLL`. Einfach mit der rechten Maustaste auf die `App_Code` Ordner im Projektmappen-Explorer, und wählen Sie die neuen Ordner. Verschieben Sie nach dem erstellen diese beiden Ordner an, die typisierte DataSet erstellt, die im ersten Lernprogramm in der `DAL` Unterordner.

Als Nächstes erstellen Sie die vier BLL-Klasse-Dateien in den `BLL` Unterordner. Um dies zu erreichen, rechtsklicken Sie auf die `BLL` Unterordner, wählen Sie ein neues Element hinzufügen, und wählen Sie die Klassenvorlage. Benennen Sie die vier Klassen `ProductsBLL`, `CategoriesBLL`, `SuppliersBLL`, und `EmployeesBLL`.


![Fügen Sie vier neue Klassen hinzu, um den Ordner "App_Code"](creating-a-business-logic-layer-cs/_static/image2.png)

**Abbildung 2**: vier neue Klassen hinzufügen der `App_Code` Ordner


Als Nächstes fügen Sie Methoden auf jede der Klassen, die Methoden definiert, die für die TableAdapter aus dem ersten Lernprogramm einfach zu umschließen. Jetzt werden diese Methoden nur direkt in der DAL aufrufen; Wir müssen zu einem späteren Zeitpunkt eine beliebige erforderliche Geschäftslogik hinzufügen, zurück.

> [!NOTE]
> Bei Verwendung von Visual Studio Standard Edition oder höher (d. h., Sie sind *nicht* mit Visual Web Developer), können Sie optional Ihren visuell mit Klassen Entwerfen der [Klassen-Designer](https://msdn.microsoft.com/library/default.asp?url=/library/dv_vstechart/html/clssdsgnr.asp). Finden Sie in der [Klasse Designer Blog](https://blogs.msdn.com/classdesigner/default.aspx) für Weitere Informationen zu dieser neuen Funktion in Visual Studio.


Für die `ProductsBLL` Klasse, die wir insgesamt sieben Methoden hinzufügen müssen:

- `GetProducts()`Gibt alle Produkte zurück
- `GetProductByProductID(productID)`Gibt das Produkt mit der angegebenen Produkt-ID
- `GetProductsByCategoryID(categoryID)`Gibt alle Produkte aus der angegebenen Kategorie zurück.
- `GetProductsBySupplier(supplierID)`Gibt alle Produkte aus den angegebenen Lieferanten
- `AddProduct(productName, supplierID, categoryID, quantityPerUnit, unitPrice, unitsInStock, unitsOnOrder, reorderLevel, discontinued)`Fügt ein neues Produkt in der Datenbank mithilfe der Werte ein übergeben eingehend; Gibt die `ProductID` Wert des neu eingefügten Datensatzes
- `UpdateProduct(productName, supplierID, categoryID, quantityPerUnit, unitPrice, unitsInStock, unitsOnOrder, reorderLevel, discontinued, productID)`aktualisiert ein vorhandenes Produkt mit der übergebenen Werte in der Datenbank an. Gibt `true` wenn genau eine Zeile aktualisiert wurde, `false` andernfalls
- `DeleteProduct(productID)`Löscht das angegebene Produkt aus der Datenbank

ProductsBLL.cs


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample1.cs)]

Die Methoden, die Daten zurückgeben `GetProducts`, `GetProductByProductID`, `GetProductsByCategoryID`, und `GetProductBySuppliersID` sind recht einfach, wie sie in der DAL rufen Sie einfach nach unten. In einigen Szenarien besteht möglicherweise Geschäftsregeln, die implementiert werden, müssen auf dieser Ebene (z. B. die Autorisierungsregeln basierend auf den derzeit angemeldeten Benutzer oder die Rolle, zu der der Benutzer gehört), müssen wir einfach lassen diese Methoden als-ist. Für diese Methode dient dann die BLL lediglich als Proxy über dem Darstellungsschicht von der Datenzugriffsebene die zugrunde liegenden Daten zugreift.

Die `AddProduct` und `UpdateProduct` Methoden sowohl sind als Parameter die Werte für die verschiedenen Produkt Felder und Hinzufügen eines neuen Produkts oder einer vorhandenen bzw. aktualisieren. Da viele der der `Product` Tabellenspalten lässt `NULL` Werte (`CategoryID`, `SupplierID`, und `UnitPrice`, um nur einige zu nennen), die Eingabeparameter für `AddProduct` und `UpdateProduct` , die solche Spalten Verwendung zugeordnet[auf NULL festlegbare Typen](https://msdn.microsoft.com/library/1t3y8s4s(v=vs.80).aspx). Auf NULL festlegbare Typen sind neu in .NET 2.0 und eine Technik bereitzustellen, für sein, der angibt, ob ein Werttyp stattdessen sollten `null`. In c# können Sie einen Werttyp als dem nullable-Typ kennzeichnen, indem hinzufügen `?` nach dem Typ (z. B. `int? x;`). Finden Sie in der [auf NULL festlegbare Typen](https://msdn.microsoft.com/library/1t3y8s4s.aspx) im Abschnitt der [C#-Programmierhandbuch](https://msdn.microsoft.com/library/67ef8sbd%28VS.80%29.aspx) für Weitere Informationen.

Alle drei Methoden geben einen booleschen Wert, der angibt, ob eine Zeile eingefügt, aktualisiert oder gelöscht werden, da der Vorgang nicht in einer betroffenen Zeile möglicherweise wurde zurück. Beispielsweise ruft der Entwickler der Seite `DeleteProduct` übergeben eine `ProductID` für ein Produkt nicht existierende der `DELETE` Anweisung ausgegeben wird, auf die Datenbank hat keine Auswirkung und somit die `DeleteProduct` Methode zurück `false`.

Beachten Sie, dass, wenn Sie ein neues Produkt hinzufügen oder Aktualisieren einer vorhandenen wir nehmen in das neue oder geänderte Produkt Feldwerte als eine Liste von skalaren im Gegensatz zu akzeptieren einen `ProductsRow` Instanz. Dieser Ansatz wurde gewählt, weil die `ProductsRow` -Klasse ableitet, die ADO.NET `DataRow` -Klasse, die nicht über einen parameterlosen Standardkonstruktor verfügt. Zum Erstellen eines neuen `ProductsRow` Instanz wir müssen zunächst erstellen eine `ProductsDataTable` -Instanz und rufen Sie dann die `NewProductRow()` Methode (was der Fall ist `AddProduct`). Dieser Schwierigkeit rears den Kopf, wenn wir zum Einfügen und Aktualisieren von Produkten mithilfe der ObjectDataSource aktivieren. Kurz gesagt, versucht das ObjectDataSource zum Erstellen einer Instanz der Eingabeparameter. Wenn die Methode BLL erwartet eine `ProductsRow` Instanz, versucht das ObjectDataSource, erstellen Sie einen, jedoch zu einem Fehler aufgrund der Mangel an einen parameterlosen Standardkonstruktor. Weitere Informationen zu diesem Problem finden Sie in den folgenden zwei ASP.NET Foren Beiträgen: [ObjectDataSources mit Strongly-Typed DataSets aktualisieren](https://forums.asp.net/1098630/ShowPost.aspx), und [Problem mit ObjectDataSource und Strongly-Typed DataSet](https://forums.asp.net/1048212/ShowPost.aspx).

Als Nächstes wird in beiden `AddProduct` und `UpdateProduct`, erstellt der Code eine `ProductsRow` Instanz, und füllt sie mit den Werten, die nur übergeben. Verschiedene Überprüfungen auf Feldebene können auftreten, wenn DataColumns einer DataRow Werte zuweisen. Aus diesem Grund kann die übergebenen Werte manuell wieder in einer DataRow einfügen sichergestellt werden die Gültigkeit der Daten an die BLL-Methode übergeben wird. Die stark typisierte DataRow-Klassen von Visual Studio generiert verwenden leider nicht auf NULL festlegbare Typen. Vielmehr, um anzugeben, dass eine bestimmte DataColumn in einer DataRow entsprechen sollten eine `NULL` Datenbank Wert wir verwenden die `SetColumnNameNull()` Methode.

In `UpdateProduct` laden wir zuerst in das Produkt, das update mit `GetProductByProductID(productID)`. Dies mag wie eine unnötige Roundtrips zur Datenbank, werden diese zusätzlichen Reise lohnende in zukünftigen Lernprogrammen zu prüfen, die vollständigen Parallelität zu untersuchen. Vollständige Parallelität ist eine Technik, um sicherzustellen, dass zwei Benutzer gleichzeitig auf denselben Daten arbeiten jeweiligen Änderungen nicht versehentlich überschreiben. Den gesamten Datensatz Grabbing erleichtert auch zum Erstellen von Aktualisierungsmethoden in die BLL, die nur eine Teilmenge der Spalten, die DataRow ändern. Wenn wir untersuchen das `SuppliersBLL` sehen wir ein Beispiel für eine solche Klasse.

Schließlich, beachten Sie, dass die `ProductsBLL` -Klasse verfügt über die [DataObject Attribut](https://msdn.microsoft.com/library/system.componentmodel.dataobjectattribute.aspx) angewendet wird (der `[System.ComponentModel.DataObject]` Syntax direkt vor dem Class-Anweisung im oberen Bereich der Datei) und die Methoden verfügen über [ DataObjectMethodAttribute Attribute](https://msdn.microsoft.com/library/system.componentmodel.dataobjectmethodattribute.aspx). Die `DataObject` Attribut markiert die Klasse als ein Objekt, das für die Bindung an eine [ObjectDataSource-Steuerelement](https://msdn.microsoft.com/library/9a4kyhcx.aspx), während die `DataObjectMethodAttribute` den Zweck der Methode angibt. Wird in zukünftigen Lernprogrammen sehen, vereinfacht die Verwendung des ASP.NET 2.0 ObjectDataSource deklarativ Daten aus einer Klasse zuzugreifen. Um die Liste der möglichen Klassen zum Binden an das ObjectDataSource-Assistenten Filtern zu unterstützen, werden standardmäßig nur die Klassen, die als gekennzeichnete `DataObjects` werden in den Assistenten Dropdown-Liste angezeigt. Die `ProductsBLL` Klasse funktioniert genauso gut ohne diese Attribute, aber diese hinzufügen erleichtert das Arbeiten mit im Assistenten für das ObjectDataSource.

## <a name="adding-the-other-classes"></a>Hinzufügen von anderen Klassen

Mit der `ProductsBLL` Klasse abgeschlossen, wir noch die Klassen für das Arbeiten mit Kategorien, Lieferanten und Mitarbeiter hinzufügen müssen. Nehmen Sie einen Moment Zeit, um die folgenden Klassen und Methoden, die mit den Konzepten von den oben genannten Beispiel zu erstellen:

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

Die eine Methode zu beachten ist die `SuppliersBLL` Klasse `UpdateSupplierAddress` Methode. Diese Methode stellt eine Schnittstelle für die Aktualisierung nur die Adressinformationen des Lieferanten. Intern wird diese Methode liest der `SupplierDataRow` für das angegebene Objekt `supplierID` (mit `GetSupplierBySupplierID`), legt deren Eigenschaften adressbezogenen fest und ruft dann nach unten in der `SupplierDataTable`des `Update` Methode. Die `UpdateSupplierAddress` Methode folgt:


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample2.cs)]

Finden Sie in diesem Artikel Download für meine vollständige Implementierung der BLL Klassen.

## <a name="step-2-accessing-the-typed-datasets-through-the-bll-classes"></a>Schritt 2: Den Zugriff auf typisierte DataSets durch die BLL-Klassen

Im ersten Lernprogramm Filtermenü Beispiele für das direkt mit dem typisierten DataSet programmgesteuert arbeiten, aber durch das Hinzufügen von unseren BLL Klassen ist die Präsentationsebene Formulargröße gegen die BLL stattdessen. In der `AllProducts.aspx` Beispiel aus dem ersten Lernprogramm, das `ProductsTableAdapter` wurde um die Liste der Produkte an eine GridView zu binden verwendet, wie im folgenden Code gezeigt:


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample3.cs)]

Verwenden Sie die neue BLL Klassen, die geändert werden muss ist ersetzen Sie die erste Zeile des Codes die `ProductsTableAdapter` -Objekt mit einer `ProductBLL` Objekt:


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample4.cs)]

Die BLL Klassen können auch deklarativ (wie das typisierte DataSet) mithilfe der ObjectDataSource zugegriffen werden. Wir müssen die ObjectDataSource ausführlicher in den folgenden Lernprogrammen erörtert.


[![Die Liste der Produkte wird in einem GridView angezeigt.](creating-a-business-logic-layer-cs/_static/image4.png)](creating-a-business-logic-layer-cs/_static/image3.png)

**Abbildung 3**: die Liste der Produkte in einer GridView angezeigt wird ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-business-logic-layer-cs/_static/image5.png))


## <a name="step-3-adding-field-level-validation-to-the-datarow-classes"></a>Schritt 3: Hinzufügen einer Validierung der Feldebene auf die DataRow-Klassen

Überprüfungsfehler auf werden überprüft, die auf die Eigenschaftswerte von Geschäftsobjekten, beim Einfügen oder Aktualisieren von beziehen. Einige Feldebene Überprüfungsregeln für Produkte umfassen:

- Die `ProductName` Feld muss 40 Zeichen oder weniger umfasst
- Die `QuantityPerUnit` Feld muss 20 Zeichen oder weniger umfasst
- Die `ProductID`, `ProductName`, und `Discontinued` Felder sind erforderlich, aber alle anderen Felder sind optional
- Die `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, und `ReorderLevel` Felder müssen größer als oder gleich 0 (null) sein.

Diese Regeln können und auf Datenbankebene ausgedrückt werden soll. Die Zeichenlänge für den `ProductName` und `QuantityPerUnit` Felder werden erfasst, die Datentypen der Spalten in der `Products` Tabelle (`nvarchar(40)` und `nvarchar(20)`bzw.). Gibt an, ob Felder erforderlichen und optionalen werden durch ausgedrückt werden, wenn die Datenbanktabellen-Spalte erlaubt `NULL` s. Vier [check-Einschränkungen](https://msdn.microsoft.com/library/ms188258.aspx) vorhanden, die stellen sicher, dass nur Werte größer als oder gleich 0 (null) in können die `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, oder `ReorderLevel` Spalten.

Zusätzlich zum Erzwingen dieser Regeln in der Datenbank sollten sie auch auf Datensatzebene erzwungen werden. In der Tat sind die Feldlänge und gibt an, ob ein Wert erforderlich oder optional ist bereits für jede DataTable Satz von DataColumns erfasst. Um finden Sie in der vorhandenen Feldebene Überprüfung automatisch bereitgestellt, wechseln Sie zu der DataSet-Designer, wählen Sie ein Feld aus einer der Datentabellen, und fahren Sie mit dem Eigenschaftenfenster. Wie in Abbildung 4 gezeigt, die `QuantityPerUnit` DataColumn in der `ProductsDataTable` besitzt eine maximale Länge von 20 Zeichen und lässt `NULL` Werte. Wenn Sie versuchen, legen Sie die `ProductsDataRow`des `QuantityPerUnit` Eigenschaft auf einen Zeichenfolgenwert fest, die mehr als 20 Zeichen ein `ArgumentException` ausgelöst.


[![DataColumn bietet eine grundlegende Feldebene-Validierung](creating-a-business-logic-layer-cs/_static/image7.png)](creating-a-business-logic-layer-cs/_static/image6.png)

**Abbildung 4**: der DataColumn bietet grundlegende Feldebene Validierung ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-business-logic-layer-cs/_static/image8.png))


Leider kann nicht angegeben Grenzen Überprüfungen aus, wie z. B. die `UnitPrice` Wert muss größer als oder gleich 0 (null), über das Fenster "Eigenschaften". Um diese Art der Feldebene Validierung bereitstellen müssen, erstellen Sie einen Ereignishandler für das DataTable [ColumnChanging](https://msdn.microsoft.com/library/system.data.datatable.columnchanging%28VS.80%29.aspx) Ereignis. Siehe die [vorherigen Lernprogramm](creating-a-data-access-layer-cs.md), die Datasets und Datentabellen DataRow-Objekte erstellt, indem das typisierte DataSet durch die Verwendung von partiellen Klassen erweitert werden können. Mit diesem Verfahren wir erstellen eine `ColumnChanging` -Ereignishandler für die `ProductsDataTable` Klasse. Starten Sie durch Erstellen einer Klasse in der `App_Code` Ordner mit dem Namen `ProductsDataTable.ColumnChanging.cs`.


[![Fügen Sie eine neue Klasse, um den Ordner "App_Code"](creating-a-business-logic-layer-cs/_static/image10.png)](creating-a-business-logic-layer-cs/_static/image9.png)

**Abbildung 5**: Hinzufügen einer neuen Klasse in der `App_Code` Ordner ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-business-logic-layer-cs/_static/image11.png))


Als Nächstes erstellen Sie einen Ereignishandler für das `ColumnChanging` Ereignis, das wird, dass sichergestellt die `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, und `ReorderLevel` Spaltenwerte (wenn nicht `NULL`) sind größer oder gleich 0 (null). Wenn eine solche Spalte außerhalb des gültigen Bereichs ist, lösen eine `ArgumentException`.

ProductsDataTable.ColumnChanging.cs


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample5.cs)]

## <a name="step-4-adding-custom-business-rules-to-the-blls-classes"></a>Schritt 4: Hinzufügen von benutzerdefinierten Geschäftsregeln auf die BLL-Klassen

Zusätzlich zur Validierung der Feldebene möglicherweise auf hoher Ebene benutzerdefinierten Geschäftsregeln, die verschiedene Entitäten oder Konzepte, die der Ebene der einzelnen Spalte nicht ausgedrückt werden kann, wie z. B. umfassen:

- Wenn ein Produkt, werden eingestellt ist seine `UnitPrice` kann nicht aktualisiert werden
- Land des Mitarbeiters muss das Land des Managers identisch sein.
- Ein Produkt kann nicht eingestellt werden, ist dies die einzige Produkt, das vom Lieferanten

BLL Klassen dürfen Überprüfungen aus, um die Einhaltung von Geschäftsregeln für die Anwendung sicherzustellen. Diese Überprüfungen können direkt auf die Methoden hinzugefügt werden, für die sie gelten.

Stellen Sie sich vor, dass unsere Geschäftsregeln vorgibt, dass für ein Produkt nicht nicht mehr unterstützte gekennzeichnet werden, konnte war dies das einzige Produkt von einem bestimmten anderen Lieferanten. D. h. wenn Produkt *X* war das einzige Produkt, das wir von anderen Lieferanten erworben *Y*, markieren wir nicht *X* wie eingestellt; Wenn jedoch Lieferanten *Y*uns mit drei Produkte bereitgestellten *ein*, *B*, und *C*, konnten wir alle markieren, und alle diese als nicht mehr unterstützt. Eine ungerade Geschäftsregel aber Geschäftsregeln und Menschenverstand sind nicht immer ausgerichtet.

Zum Erzwingen dieser Geschäftsregel in die `UpdateProducts` Methode wir überprüfen zunächst würden, ob `Discontinued` wurde `true` und, falls also nennen wir würden `GetProductsBySupplierID` um zu bestimmen, wie viele Produkte wir dieses Produkt Lieferanten erworben wurden. Wenn nur ein Produkt von diesem Lieferanten erworben wird, lösen wir eine `ApplicationException`.


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample6.cs)]

## <a name="responding-to-validation-errors-in-the-presentation-tier"></a>Reagieren auf Validierungsfehler in der Präsentationsebene

Beim Aufrufen der BLL von der Präsentationsebene können wir entscheiden Sie, ob der Versuch, alle Ausnahmen zu behandeln, die möglicherweise ausgelöst, oder informieren Sie ihn an ASP.NET übergeben (die löst die `HttpApplication`des `Error` Ereignis). Um eine Ausnahme behandelt wird, wenn mit der BLL programmgesteuert arbeiten, verwenden wir eine [try…catch](https://msdn.microsoft.com/library/0yd65esw.aspx) Block, wie im folgenden Beispiel gezeigt:


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample7.cs)]

Wie wir in Zukunft Lernprogramme sehen, Behandlung von Ausnahmen, die Bubbling der BLL bei Verwendung eines-Steuerelement zum Einfügen Webserver, aktualisieren oder Löschen von Daten kann direkt in einem Ereignishandler im Gegensatz zur Verwendung in Code umschließen behandelt `try...catch` blockiert.

## <a name="summary"></a>Zusammenfassung

Ein Anwendung mit durchdachter ist in unterschiedliche Ebenen gestalteten von denen jede eine bestimmte Rolle kapselt. Im ersten Lernprogramm dieser Reihe Artikel haben wir eine Datenzugriffsschicht, typisierte DataSets verwenden; In diesem Lernprogramm wird eine Geschäftslogikschicht als erstellt eine Reihe von Klassen in der vorliegenden Anwendungsverzeichnis `App_Code` Ordner, die nach-unten unsere DAL aufrufen. Die BLL implementiert die Logik für die Feldebene und Business-Ebene für die Anwendung. Zusätzlich zur Erstellung einer separaten BLL an, wie in diesem Lernprogramm ist eine weitere Option die TableAdapter-Methoden, durch die Verwendung von partiellen Klassen erweitern. Jedoch mithilfe dieser Technik lässt keine uns vorhandene Methoden überschreiben, noch ist es trennen unsere DAL und unsere BLL ordnungsgemäß als Ansatz, wenn, den wir in diesem Artikel erstellt haben.

Die DAL und BLL abgeschlossen können wir mit auf unserem Darstellungsschicht beginnen. In der [nächsten Lernprogramm](master-pages-and-site-navigation-cs.md) wir nehmen Sie eine kurze Umleitung von Themen zum Zugriff auf der Daten und ein konsistente Seitenlayout für die Verwendung in den Lernprogrammen zu definieren.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurden Liz Shulok, Dennis Patterson Carlos Santos und Hilton Giesenow. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Zurück](creating-a-data-access-layer-cs.md)
[Weiter](master-pages-and-site-navigation-cs.md)
