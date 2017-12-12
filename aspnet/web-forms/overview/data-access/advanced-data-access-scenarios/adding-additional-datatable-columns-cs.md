---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/adding-additional-datatable-columns-cs
title: "Hinzufügen von zusätzlichen DataTable-Spalten (c#) | Microsoft Docs"
author: rick-anderson
description: "Wenn Sie den TableAdapter-Assistenten zum Erstellen eines typisierten Datasets verwenden, enthält entsprechende DataTable die Spalten, die von der Abfrage Hauptdatenbank zurückgegeben. Aber es..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: 615f3361-f21f-4338-8bc1-fce8ae071de9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/adding-additional-datatable-columns-cs
msc.type: authoredcontent
ms.openlocfilehash: 0b1fe8d2e376065aed8d94b1267910bd1f7e5bd0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="adding-additional-datatable-columns-c"></a>Hinzufügen von zusätzlichen DataTable-Spalten (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Herunterladen von Code](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_70_CS.zip) oder [PDF herunterladen](adding-additional-datatable-columns-cs/_static/datatutorial70cs1.pdf)

> Wenn Sie den TableAdapter-Assistenten zum Erstellen eines typisierten Datasets verwenden, enthält entsprechende DataTable die Spalten, die von der Abfrage Hauptdatenbank zurückgegeben. Aber es gibt Situationen, wenn der DataTable einbinden zusätzliche Spalten muss. In diesem Lernprogramm lernen wir an, warum die gespeicherte Prozeduren empfohlen werden, wenn wir zusätzliche DataTable-Spalten benötigen.


## <a name="introduction"></a>Einführung

Wenn einen TableAdapter eine typisierte DataSet hinzufügen, wird das entsprechende DataTable-s-Schema durch der Hauptabfrage des TableAdapter s bestimmt. Wenn die Hauptabfrage Datenfelder gibt z. B. *ein*, *B*, und *C*, DataTable müssen drei entsprechende Spalten, die mit dem Namen *ein*, *B*, und *C*. Neben der Hauptabfrage kann ein TableAdapter zusätzliche Abfragen enthalten, die möglicherweise auch noch eine Teilmenge der Daten basierend auf einige Parameter zurückgibt. Beispielsweise zusätzlich zu den `ProductsTableAdapter` s Hauptabfrage, die Informationen zu allen Produkten zurückgibt, es enthält auch Methoden wie z. B. `GetProductsByCategoryID(categoryID)` und `GetProductByProductID(productID)`, die bestimmten Produktinformationen auf Grundlage eines angegebenen Parameters zurück.

Das Modell, dass das DataTable-s-Schema die Hauptabfrage des TableAdapter s widerspiegeln funktioniert gut, wenn alle der TableAdapter-s-Methoden die gleiche oder weniger Datenfelder als die in der Hauptabfrage zurückgeben. Wenn eine Methode des TableAdapter zusätzliche Datenfelder zurückgeben muss, sollten wir das DataTable-s-Schema entsprechend erweitern. In der [Master/Detail, verwenden eine Aufzählung der Master-Datensätze mit Details DataList](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) Lernprogramm, die wir, eine Methode, um hinzugefügt die `CategoriesTableAdapter` , die zurückgegeben der `CategoryID`, `CategoryName`, und `Description` Datenfelder, die in definierten der Hauptabfrage plus `NumberOfProducts`, eine zusätzliche Datenfelder, die die Anzahl der Produkte jeder Kategorie zugeordnet gemeldet. Wir manuell eine neue Spalte hinzugefügt der `CategoriesDataTable` zum Erfassen der `NumberOfProducts` Datenfeld der Spalte Wert von dieser neuen Methode.

Wie in beschrieben die [Hochladen von Dateien](../working-with-binary-files/uploading-files-cs.md) Lernprogramm, großartige muss darauf geachtet werden mit TableAdapters, die Ad-hoc-SQL-Anweisungen verwenden und verfügen über Methoden, deren Datenfelder stimmen die Hauptabfrage nicht genau überein. Wenn der TableAdapter-Konfigurations-Assistent erneut ausgeführt wird, aktualisiert es alle Methoden s TableAdapter, damit ihre Daten Feldliste aus die Hauptabfrage übereinstimmt. Daher werden keine Methoden mit benutzerdefinierten Spaltenlisten kehren Sie zu der Liste der Hauptabfrage s-Spalte und nicht die erwarteten Daten zurück. Dieses Problem nicht, da bei der Verwendung gespeicherter Prozeduren.

In diesem Lernprogramm untersuchen wir an, wie ein DataTable-s-Schema, um weitere Spalten einschließen, zu erweitern. Aufgrund der unzulänglichen des TableAdapter bei Verwendung von Ad-hoc-SQL-Anweisungen werden wir in diesem Lernprogramm gespeicherte Prozeduren verwenden. Finden Sie in der [Erstellen neuer gespeicherter Prozeduren für die typisierte DataSet s TableAdapters](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) und [vorhandene gespeicherte Prozeduren verwenden, für die typisierte DataSet s TableAdapters](https://data-access/tutorials/using-existing-stored-procedures-for-the-typed-dataset-amp-rsquo-s-tableadapters-cs) Lernprogramme für Weitere Informationen zu Konfigurieren einen TableAdapter um gespeicherte Prozeduren verwenden.

## <a name="step-1-adding-apricequartilecolumn-to-theproductsdatatable"></a>Schritt 1: Hinzufügen einer`PriceQuartile`Spalte, um die`ProductsDataTable`

In der *Erstellen neuer gespeicherter Prozeduren für die typisierte DataSet s TableAdapters* Lernprogramm typisiertes DataSet namens erstellten `NorthwindWithSprocs`. Dieses DataSet enthält derzeit zwei DataTables: `ProductsDataTable` und `EmployeesDataTable`. Die `ProductsTableAdapter` hat die folgenden drei Methoden:

- `GetProducts`-der Hauptabfrage, die zurückgibt, alle Datensätze aus der `Products` Tabelle
- `GetProductsByCategoryID(categoryID)`-Gibt alle Produkte mit dem angegebenen *CategoryID*.
- `GetProductByProductID(productID)`-Gibt das bestimmte Produkt mit dem angegebenen *"ProductID"*.

Der Hauptabfrage und zwei zusätzlichen Methoden zurückgeben den gleichen Satz von Datenfeldern, d. h. alle Spalten aus der `Products` Tabelle. Es gibt keine korrelierten Unterabfragen oder `JOIN` s herausziehen aufeinander bezogene Daten in der `Categories` oder `Suppliers` Tabellen. Aus diesem Grund die `ProductsDataTable` verfügt über eine entsprechende Spalte für jedes Feld in der `Products` Tabelle.

Für dieses Lernprogramm können s eine Methode zum Hinzufügen der `ProductsTableAdapter` mit dem Namen `GetProductsWithPriceQuartile` aller Produkte zurückgibt. Zusätzlich zu den Datenfelder die standardmäßige Product `GetProductsWithPriceQuartile` gehören auch eine `PriceQuartile` Feld "Daten", der angibt, unter welcher Quartil der Produktpreis s fällt. Beispielsweise müssen die Produkte, deren Preise, in dem die teuersten 25 gelten % eine `PriceQuartile` Wert 1, während die, deren Preis in der unteren 25 fallen % den Wert 4 hat. Vor Gedanken machen, wir Erstellen der gespeicherten Prozedur zum Zurückgeben dieser Informationen, wir zuerst müssen jedoch Aktualisieren der `ProductsDataTable` enthalten eine Spalte enthalten die `PriceQuartile` Ergebnisse bei der `GetProductsWithPriceQuartile` Methode wird verwendet.

Öffnen der `NorthwindWithSprocs` DataSet mit der rechten Maustaste auf die `ProductsDataTable`. Wählen Sie hinzufügen aus dem Kontextmenü, und wählen Sie dann die Spalte.


[![Fügen Sie eine neue Spalte hinzu, um die ProductsDataTable](adding-additional-datatable-columns-cs/_static/image2.png)](adding-additional-datatable-columns-cs/_static/image1.png)

**Abbildung 1**: Hinzufügen einer neuen Spalte, die `ProductsDataTable` ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-additional-datatable-columns-cs/_static/image3.png))


Dadurch wird eine neue Spalte hinzugefügt, zu der mit dem Namen Column1 vom Typ "DataTable" `System.String`. Wir müssen dieser Spaltenname s PriceQuartile und dessen Typ zu aktualisieren `System.Int32` , da er verwendet wird, eine Zahl zwischen 1 und 4 enthalten. Wählen Sie die neu hinzugefügte Spalte der `ProductsDataTable` , und legen Sie im Fenster Eigenschaften die `Name` PriceQuartile Eigenschaft und die `DataType` Eigenschaft `System.Int32`.


[![Festlegen Sie Neuer Spaltenname s und Eigenschaften für Datentypen](adding-additional-datatable-columns-cs/_static/image5.png)](adding-additional-datatable-columns-cs/_static/image4.png)

**Abbildung 2**: Legen Sie die neue Spalte s `Name` und `DataType` Eigenschaften ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-additional-datatable-columns-cs/_static/image6.png))


Wie in Abbildung 2 dargestellt, stehen zusätzliche Eigenschaften, die festgelegt werden können, z. B., ob die Werte in der Spalte sein müssen eindeutig sein, wenn die Spalte eine Spalte automatisch inkrementiert wird, ob Datenbank- `NULL` Werte zulässig sind, und so weiter. Lassen Sie diese Werte auf ihre Standardwerte festgelegt.

## <a name="step-2-creating-thegetproductswithpricequartilemethod"></a>Schritt 2: Erstellen der`GetProductsWithPriceQuartile`Methode

Nun, dass die `ProductsDataTable` wurde aktualisiert und enthalten die `PriceQuartile` Spalte wir sind bereit für die Erstellung der `GetProductsWithPriceQuartile` Methode. Starten Sie, indem Sie mit der rechten Maustaste auf den TableAdapter und Abfrage hinzufügen aus dem Kontextmenü. Daraufhin wird der TableAdapter-Abfrage-Konfigurations-Assistenten, in der werden uns zuerst gefragt, ob Ad-hoc-SQL-Anweisungen oder einer neuen oder vorhandenen gespeicherten Prozedur verwendet werden soll. Da wir t Verschlüsselungskennwort noch haben eine gespeicherte Prozedur, die den Preis Quartil Daten zurückgibt, können Sie s des TableAdapter zum Erstellen dieser gespeicherten Prozedur für uns zu ermöglichen. Wählen Sie die Option erstellen neue gespeicherte Prozedur, und klicken Sie auf Weiter.


[![Weisen Sie dem TableAdapter-Assistenten zum Erstellen der gespeicherten Prozedur für uns](adding-additional-datatable-columns-cs/_static/image8.png)](adding-additional-datatable-columns-cs/_static/image7.png)

**Abbildung 3**: Weisen Sie dem TableAdapter-Assistenten zum Erstellen der gespeicherten Prozedur für uns ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-additional-datatable-columns-cs/_static/image9.png))


Im nachfolgenden Bildschirm in Abbildung 4 gezeigt vom Assistenten gefragt uns welche Art von Abfrage hinzufügen. Da die `GetProductsWithPriceQuartile` Methodenrückgabewert werden alle Spalten und Datensätze aus der `Products` Tabelle, die wählen Sie die Zeilen aus, und klicken Sie auf Weiter zurückgibt.


[![Unsere Abfrage wird eine SELECT-Anweisung, gibt mehrere Zeilen sein.](adding-additional-datatable-columns-cs/_static/image11.png)](adding-additional-datatable-columns-cs/_static/image10.png)

**Abbildung 4**: unsere Abfrage eine `SELECT` Anweisung, gibt mehrere Zeilen ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-additional-datatable-columns-cs/_static/image12.png))


Wir werden als Nächstes aufgefordert, für die `SELECT` Abfrage. Geben Sie die folgende Abfrage in den Assistenten:


[!code-sql[Main](adding-additional-datatable-columns-cs/samples/sample1.sql)]

Die oben dargestellte Abfrage verwendet die SQL Server 2005-s neue [ `NTILE` Funktion](https://msdn.microsoft.com/en-us/library/ms175126.aspx) , die Ergebnisse in vier Gruppen unterteilen, bei dem Gruppen von bestimmt sind, die `UnitPrice` Werte, die in absteigender Reihenfolge sortiert.

Abfrage-Generator weiß leider nicht wie beim Analysieren der `OVER` Schlüsselwort und zeigt einen Fehler beim Analysieren der obigen Abfrage. Daher geben Sie die oben dargestellte Abfrage direkt in das Textfeld im Assistenten ohne mit dem Abfrage-Generator ein.

> [!NOTE]
> Weitere Informationen zu NTILE und SQL Server 2005 s andere Rangfolgefunktionen finden Sie unter [aufgelisteten Ergebnisse zurückgeben, mit Microsoft SQL Server 2005](http://www.4guysfromrolla.com/webtech/010406-1.shtml) und die [Rangfolge Funktionen Abschnitt](https://msdn.microsoft.com/en-us/library/ms189798.aspx) aus der [SQL Server 2005-Onlinedokumentation](https://msdn.microsoft.com/en-us/library/ms189798.aspx).


Nach der Eingabe der `SELECT` Abfrage und auf Weiter klicken, wird der Assistent stellt uns, geben Sie einen Namen für die gespeicherte Prozedur wird erstellt. Benennen Sie die neue gespeicherte Prozedur `Products_SelectWithPriceQuartile` , und klicken Sie auf Weiter.


[![Name der gespeicherten Prozedur Products_SelectWithPriceQuartile](adding-additional-datatable-columns-cs/_static/image14.png)](adding-additional-datatable-columns-cs/_static/image13.png)

**Abbildung 5**: Benennen Sie die gespeicherte Prozedur `Products_SelectWithPriceQuartile` ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-additional-datatable-columns-cs/_static/image15.png))


Schließlich werden wir nennen Sie die TableAdapter-Methoden aufgefordert. Lassen Sie beide Füllung eine "DataTable", und geben Sie eine DataTable Kontrollkästchen aktiviert und den Namen der Methoden `FillWithPriceQuartile` und `GetProductsWithPriceQuartile`.


[![Namen die TableAdapter-Methoden, und klicken Sie auf Fertig stellen](adding-additional-datatable-columns-cs/_static/image17.png)](adding-additional-datatable-columns-cs/_static/image16.png)

**Abbildung 6**: Benennen Sie die s-Methoden des TableAdapter, und klicken Sie auf Fertig stellen ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-additional-datatable-columns-cs/_static/image18.png))


Mit der `SELECT` Abfrage angegeben, und die gespeicherte Prozedur und die TableAdapter-Methoden, die mit dem Namen, klicken Sie auf ' Fertig stellen ', um den Assistenten abzuschließen. An diesem Punkt erhalten Sie möglicherweise eine Warnung oder zwei vom Assistenten darauf hingewiesen werden, die die `OVER` SQL-Konstrukt oder die Anweisung wird nicht unterstützt. Diese Warnung können ignoriert werden.

Nach Abschluss des Assistenten für TableAdapter enthalten sollte die `FillWithPriceQuartile` und `GetProductsWithPriceQuartile` Methoden und die Datenbank eine gespeicherte Prozedur namens aufzunehmen `Products_SelectWithPriceQuartile`. Nehmen Sie einen Moment Zeit, um zu überprüfen, dass die TableAdapter tatsächlich dieser neuen Methode enthält, die gespeicherte Prozedur mit der Datenbank ordnungsgemäß hinzugefügt wurde. Wenn die Datenbank wird überprüft, ob die gespeicherte Prozedur versuchen Sie es mit der rechten Maustaste auf den Ordner gespeicherte Prozeduren und "Aktualisieren" nicht angezeigt wird.


![Stellen Sie sicher, dass eine neue Methode des TableAdapter hinzugefügt wurde](adding-additional-datatable-columns-cs/_static/image19.png)

**Abbildung 7**: Stellen Sie sicher, dass eine neue Methode des TableAdapter hinzugefügt wurde


[![Stellen Sie sicher, dass die Datenbank die Products_SelectWithPriceQuartile enthält gespeicherte Prozedur](adding-additional-datatable-columns-cs/_static/image21.png)](adding-additional-datatable-columns-cs/_static/image20.png)

**Abbildung 8**: sicherstellen, dass die Datenbank enthält die `Products_SelectWithPriceQuartile` gespeicherte Prozedur ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-additional-datatable-columns-cs/_static/image22.png))


> [!NOTE]
> Einer der Vorteile der Verwendung gespeicherter Prozeduren anstelle von Ad-hoc-SQL-Anweisungen ist, dass die gespeicherten Prozeduren Spaltenlisten den TableAdapter-Konfigurations-Assistenten erneut ausführen nicht geändert werden. Überprüfen Sie dies können Sie, indem Sie mit der rechten Maustaste auf den TableAdapter, mit der Option konfigurieren aus dem Kontextmenü, um den Assistenten zu starten, und klicken Sie dann auf ' Fertig stellen ', um sie abzuschließen. Als Nächstes wechseln Sie zu der Datenbank und die Ansicht der `Products_SelectWithPriceQuartile` gespeicherten Prozedur. Beachten Sie, dass die Liste der Spalten nicht geändert wurde. Hatten wir wurde mithilfe von Ad-hoc-SQL-Anweisungen, die TableAdapter-Konfigurations-Assistenten erneut ausführen würden wurden rückgängig dieser Abfrage s Spaltenliste entsprechend der Hauptabfrage Spaltenliste, wodurch die NTILE-Anweisung aus der Abfrage verwendet werden, indem Sie die `GetProductsWithPriceQuartile` Methode.


Wenn die Datenzugriffsebene s `GetProductsWithPriceQuartile` Methode aufgerufen wird, führt der TableAdapter die `Products_SelectWithPriceQuartile` von gespeicherten Prozeduren und fügt eine Zeile hinzu der `ProductsDataTable` für jeden Datensatz zurückgegebenen. Die Datenfelder, die von der gespeicherten Prozedur zurückgegebenen zugeordnet werden die `ProductsDataTable` s-Spalten. Da es ist ein `PriceQuartile` Feld "Daten" aus der gespeicherten Prozedur zurückgegeben deren Wert zugewiesen ist die `ProductsDataTable` s `PriceQuartile` Spalte.

Für die TableAdapter-Methoden, in deren Abfragen keine zurück geben. ein `PriceQuartile` Feld "Daten" der `PriceQuartile` Spaltenwert s ist der Wert von dessen `DefaultValue` Eigenschaft. Wie in Abbildung 2 gezeigt, wird dieser Wert festgelegt, um `DBNull`, die Standardeinstellung. Wenn Sie einen anderen Standardwert lieber möchten, legen Sie einfach die `DefaultValue` Eigenschaft entsprechend. Achten Sie darauf, die die `DefaultValue` Wert ist gültig anhand der s `DataType` (d. h. `System.Int32` für die `PriceQuartile` Spalte).

Zu diesem Zeitpunkt haben wir die notwendigen Schritte ausgeführt, für das Hinzufügen einer zusätzlichen Spalte zu einer "DataTable". Um sicherzustellen, dass diese zusätzliche Spalte erwartungsgemäß funktioniert, können Sie s eine ASP.NET-Seite zu erstellen, die jede s Produktname, Preis und Preis Quartil angezeigt. Bevor wir dies tun, allerdings müssen Sie zuerst die Business Logic Layer, um eine Methode enthalten, die die DAL-s-nach unten Aufrufe zu aktualisieren `GetProductsWithPriceQuartile` Methode. Wir als Nächstes aktualisieren Sie die BLL in Schritt 3, und erstellen Sie die ASP.NET-Seite in Schritt 4.

## <a name="step-3-augmenting-the-business-logic-layer"></a>Schritt 3: Erweitern der Geschäftslogikebene

Bevor wir die neue verwenden `GetProductsWithPriceQuartile` Methode aus der Präsentationsschicht, sollten wir zuerst eine entsprechende Methode der BLL hinzufügen. Öffnen der `ProductsBLLWithSprocs` Klassendatei, und fügen Sie den folgenden Code hinzu:


[!code-csharp[Main](adding-additional-datatable-columns-cs/samples/sample2.cs)]

Wie die andere Datenabrufmethoden in `ProductsBLLWithSprocs`, `GetProductsWithPriceQuartile` Methode ruft einfach die DAL s entspricht `GetProductsWithPriceQuartile` Methode und die Ergebnisse zurückgegeben werden.

## <a name="step-4-displaying-the-price-quartile-information-in-an-aspnet-web-page"></a>Schritt 4: Anzeigen der Quartil Preisinformationen in einer ASP.NET-Webseite

Führen Sie durch das Hinzufügen von BLL wir re kann jetzt eine ASP.NET-Seite zu erstellen, die das Quartile Preis für jedes Produkt angezeigt. Öffnen der `AddingColumns.aspx` auf der Seite der `AdvancedDAL` Ordner, und ziehen Sie eine GridView aus der Toolbox in den Designer, Festlegen seiner `ID` Eigenschaft `Products`. Aus den GridView s smart Tag, binden Sie es auf eine neue ObjectDataSource mit dem Namen `ProductsDataSource`. Konfigurieren der ObjectDataSource verwenden die `ProductsBLLWithSprocs` Klasse s `GetProductsWithPriceQuartile` Methode. Da dies ein nur-Lese Raster sein wird, legen Sie die Dropdownlisten in der Update-, INSERT-, und Löschen von Registerkarten (keine).


[![Konfigurieren der ObjectDataSource zur Verwendung der ProductsBLLWithSprocs-Klasse](adding-additional-datatable-columns-cs/_static/image24.png)](adding-additional-datatable-columns-cs/_static/image23.png)

**Abbildung 9**: Konfigurieren der ObjectDataSource verwenden die `ProductsBLLWithSprocs` Klasse ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-additional-datatable-columns-cs/_static/image25.png))


[![Abrufen von Produktinformationen aus der GetProductsWithPriceQuartile-Methode](adding-additional-datatable-columns-cs/_static/image27.png)](adding-additional-datatable-columns-cs/_static/image26.png)

**Abbildung 10**: Abrufen von Produktinformationen aus der `GetProductsWithPriceQuartile` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-additional-datatable-columns-cs/_static/image28.png))


Nach dem Konfigurieren von Datenquellen-Assistenten abschließen, wird Visual Studio automatisch eine BoundField- oder CheckBoxField hinzufügen an die GridView für die einzelnen Datenfelder, die von der Methode zurückgegebenen. Eines dieser Datenfelder ist `PriceQuartile`, also in der Spalte, die wir, um hinzugefügt die `ProductsDataTable` in Schritt 1.

Bearbeiten Sie die GridView s-Felder entfernen alle außer den `ProductName`, `UnitPrice`, und `PriceQuartile` BoundFields. Konfigurieren der `UnitPrice` BoundField formatieren dessen Wert als Währung und haben die `UnitPrice` und `PriceQuartile` BoundFields rechts - und zentriert ausgerichtet, bzw. Aktualisieren Sie schließlich die verbleibenden BoundFields `HeaderText` Produkt, Preis und Preis Quartil, Eigenschaften bzw. Prüfen Sie außerdem das Kontrollkästchen Sortieren aktivieren aus dem GridView-s-Smarttag.

Nachdem diese Änderungen sollten GridView und ObjectDataSource s deklarative Markup wie folgt aussehen:


[!code-aspx[Main](adding-additional-datatable-columns-cs/samples/sample3.aspx)]

Abbildung 11 zeigt diese Seite, wenn Sie über einen Browser zugegriffen werden. Beachten Sie, dass zunächst die Produkte anhand deren Preis in absteigender Reihenfolge mit den einzelnen Produkten, die eine entsprechende zugewiesen sortiert werden `PriceQuartile` Wert. Natürlich können diese Daten nach anderen Kriterien sortiert werden, mit der Preis Quartil Spaltenwert weiterhin reflektieren die Rangfolge Produkt s in Bezug auf den Preis (siehe Abbildung 12).


[![Die Produkte werden nach ihrer Preise sortiert.](adding-additional-datatable-columns-cs/_static/image30.png)](adding-additional-datatable-columns-cs/_static/image29.png)

**Abbildung 11**: werden die Produkte nach ihrer Preise sortiert ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-additional-datatable-columns-cs/_static/image31.png))


[![Die Produkte werden durch ihre Namen sortiert.](adding-additional-datatable-columns-cs/_static/image33.png)](adding-additional-datatable-columns-cs/_static/image32.png)

**Abbildung 12**: werden die Produkte nach Namen sortiert ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-additional-datatable-columns-cs/_static/image34.png))


> [!NOTE]
> Mit wenigen Codezeilen konnte wir GridView ergänzen, damit es gefärbt, Produktzeilen auf Grundlage ihrer `PriceQuartile` Wert. Wir können diese Produkte in der Quartil hellgrün, die in der zweiten Quartil einer hellen Gelb Farbe usw. Ich empfehlen Ihnen, einen Moment Zeit, diese Funktionalität hinzufügen. Wenn Sie zum Formatieren einer GridView aufzufrischen benötigen, wenden Sie sich an den [benutzerdefinierte Formatierung basierend auf Daten](../custom-formatting/custom-formatting-based-upon-data-cs.md) Lernprogramm.


## <a name="an-alternative-approach---creating-another-tableadapter"></a>Ein alternativer Ansatz - erstellen eine andere TableAdapter

Wie es beim Hinzufügen einer Methode zu einem TableAdapter, die Datenfelder als die zurückgibt durch die Hauptabfrage wie folgt buchstabiert in diesem Lernprogramm gezeigt, können wir die DataTable entsprechende Spalten hinzufügen. Ein derartiger Ansatz, allerdings gut nur, wenn eine kleine Anzahl von Methoden im TableAdapter, die unterschiedliche Datenfelder zurückgeben und diese alternativen Datenfelder nicht aus der Hauptabfrage zu stark variieren.

Anstatt DataTable Spalten hinzugefügt haben, können Sie stattdessen eine andere TableAdapter auf das DataSet hinzufügen, die die Methoden aus dem ersten TableAdapter enthält, die unterschiedliche Datenfelder zurückgeben. Für dieses Lernprogramm statt Hinzufügen der `PriceQuartile` Spalte die `ProductsDataTable` (, in dem er dient lediglich durch die `GetProductsWithPriceQuartile` Methode), wir haben einen zusätzlichen TableAdapter konnte auf das DataSet mit dem Namen hinzugefügt `ProductsWithPriceQuartileTableAdapter` , verwendet die `Products_SelectWithPriceQuartile` gespeichert die Prozedur als der Hauptabfrage. ASP.NET-Seiten, die zum Abrufen von Produktinformationen mit dem Preis Quartil erforderlich ist, verwenden die `ProductsWithPriceQuartileTableAdapter`, während solche, die nicht der Fall war konnte verwenden weiterhin die `ProductsTableAdapter`.

Fügen Sie einen neuen TableAdapter hinzu, die DataTables untarnished bleiben und ihre Spalten spiegeln genau die Datenfelder, die von ihren TableAdapter-s-Methoden zurückgegeben. Allerdings können zusätzliche TableAdapters wiederkehrende Aufgaben und Funktionen einführen. Z. B., wenn diese ASP.NET-Seiten angezeigten der `PriceQuartile` Spalte auch benötigt einfügen, aktualisieren und delete-Unterstützung, die `ProductsWithPriceQuartileTableAdapter` müssten haben seine `InsertCommand`, `UpdateCommand`, und `DeleteCommand` Eigenschaften ordnungsgemäß konfiguriert. Während diese Eigenschaften zu spiegeln, würde die `ProductsTableAdapter` s, diese Konfiguration wird einen zusätzlichen Schritt eingeführt. Darüber hinaus gibt es sind nun zwei Möglichkeiten zum Aktualisieren, löschen oder Hinzufügen eines Produkts in der Datenbank - über die `ProductsTableAdapter` und `ProductsWithPriceQuartileTableAdapter` Klassen.

Der Download für dieses Lernprogramm umfasst eine `ProductsWithPriceQuartileTableAdapter` -Klasse in der `NorthwindWithSprocs` DataSet, das diese alternative Methode veranschaulicht.

## <a name="summary"></a>Zusammenfassung

In den meisten Szenarien alle Methoden in einem TableAdapter den gleichen Satz von Datenfelder zurück, aber es vorkommen, dass eine bestimmte Methode oder zwei möglicherweise ein zusätzliches Feld zurückgegeben müssen, werden. Z. B. in der [Master/Detail, verwenden eine Aufzählung der Master-Datensätze mit Details DataList](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) Lernprogramm, die wir, eine Methode, um hinzugefügt die `CategoriesTableAdapter` , dass, sondern die Hauptabfrage s Datenfelder, die zurückgegeben eine `NumberOfProducts` Feld, das berichtet die Anzahl der Produkte jeder Kategorie zugeordnet ist. In diesem Lernprogramm wird erläutert, Hinzufügen einer Methode in der `ProductsTableAdapter` zurückgegebenen ein `PriceQuartile` Feld zusätzlich zu den Feldern der Hauptabfrage s-Daten. Um zusätzliche Daten zu erfassen zurückgegeben Felder von der TableAdapter-s-Methoden, müssen wir die DataTable entsprechende Spalten hinzugefügt.

Wenn Sie zum manuellen Hinzufügen von Spalten zu der "DataTable" Planen, wird empfohlen, dass die TableAdapter gespeicherte Prozeduren verwenden. Wenn TableAdapter Ad-hoc-SQL-Anweisungen verwendet wird, wird jedes Mal der TableAdapter-Konfigurations-Assistenten alle Methoden ausgeführt die Datenfelder, die von der main-Abfrage zurückgegebenen Daten Feldlisten wiederherstellen. Dieses Problem erstreckt sich nicht um gespeicherte Prozeduren, also warum sie empfohlen werden und in diesem Lernprogramm verwendet wurden.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurden Randy Schmidt, Jacky Goor Bernadette Leigh und Hilton Giesenow. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Zurück](updating-the-tableadapter-to-use-joins-cs.md)
[Weiter](working-with-computed-columns-cs.md)
