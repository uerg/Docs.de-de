---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/adding-additional-datatable-columns-vb
title: Hinzufügen von zusätzlichen DataTable-Spalten (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Wenn Sie den TableAdapter-Assistenten zum Erstellen eines typisierten Datasets verwenden, enthält die entsprechende DataTable Spalten, die von der Abfrage für die Hauptdatenbank zurückgegeben. Aber es...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: 1e8e65f9-fe3e-4250-810b-c90227786bed
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/adding-additional-datatable-columns-vb
msc.type: authoredcontent
ms.openlocfilehash: 84cb5f267a79fe7787ea23f1dc924e10e4d6fe13
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402784"
---
<a name="adding-additional-datatable-columns-vb"></a>Hinzufügen von zusätzlichen DataTable-Spalten (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_70_VB.zip) oder [PDF-Datei herunterladen](adding-additional-datatable-columns-vb/_static/datatutorial70vb1.pdf)

> Wenn Sie den TableAdapter-Assistenten zum Erstellen eines typisierten Datasets verwenden, enthält die entsprechende DataTable Spalten, die von der Abfrage für die Hauptdatenbank zurückgegeben. Aber es gibt Situationen, in denen Wenn DataTable zusätzliche Spalten enthalten muss. In diesem Tutorial wird erläutert, warum die gespeicherte Prozeduren empfohlen werden, wenn wir die zusätzliche DataTable-Spalten benötigen.


## <a name="introduction"></a>Einführung

Wenn Sie einen TableAdapter mit einem typisierten DataSet hinzufügen, wird das entsprechende DataTable-s-Schema durch die Hauptabfrage des TableAdapter s bestimmt. Wenn die Hauptabfrage Datenfelder gibt z. B. *ein*, *B*, und *C*, DataTable müssen drei entsprechende Spalten, die mit dem Namen *ein*, *B*, und *C*. Zusätzlich zu der Hauptabfrage kann ein TableAdapter zusätzliche Abfragen enthalten, die möglicherweise eine Teilmenge der Daten basierend auf einige Parameter zurückgeben. Z. B. zusätzlich zu den `ProductsTableAdapter` s Hauptabfrage, die Informationen über alle Produkte zurückgibt, sie enthält außerdem Methoden wie `GetProductsByCategoryID(categoryID)` und `GetProductByProductID(productID)`, die bestimmte Produktinformationen auf Grundlage eines angegebenen Parameters zurück.

Das Modell der Dank der DataTable-s-Schema die Hauptabfrage des TableAdapter s entsprechend funktioniert gut, wenn alle den TableAdapter-s-Methoden die gleiche oder weniger Datenfelder als die in der Hauptabfrage zurück. Wenn eine Methode des TableAdapter zusätzliche Datenfelder zurückgeben muss, sollten wir entsprechend das DataTable-s-Schema erweitern. In der [Master/Detail, verwenden eine Liste mit Aufzählungszeichen Liste der Masterdatensätze mit einem DataList-Steuerelement Details](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md) Tutorial, die wir hinzugefügt, dass eine Methode, um der `CategoriesTableAdapter` zurückgegebenen der `CategoryID`, `CategoryName`, und `Description` Datenfelder, die in definierten der Hauptabfrage plus `NumberOfProducts`, ein Feld zusätzliche Daten, die die Anzahl der Produkte verknüpft ist, wobei jede Kategorie gemeldet. Wir manuell eine neue Spalte hinzugefügt der `CategoriesDataTable` zum Erfassen der `NumberOfProducts` Datenfeldwert von dieser neuen Methode.

Siehe die [Hochladen von Dateien](../working-with-binary-files/uploading-files-vb.md) Lernprogramm, große Vorsicht mit TableAdapters, die mithilfe von Ad-hoc-SQL-Anweisungen und verfügen über Methoden, deren Datenfelder nicht genau die Hauptabfrage entsprechen. Wenn der TableAdapter-Konfigurations-Assistent erneut ausgeführt wird, aktualisiert es alle Methoden s TableAdapter, damit ihre Daten Feldliste aus die Hauptabfrage übereinstimmt. Daher werden keine Methoden mit benutzerdefinierten Spaltenlisten Wiederherstellen der Hauptabfrage s Spaltenliste und nicht die erwarteten Daten zurückgeben. Dieses Problem nicht beim Verwenden gespeicherter Prozeduren.

In diesem Tutorial werden wir ein DataTable-s-Schema um zusätzliche Spalten aufzunehmen erweitern betrachten. Aufgrund der Fehleranfälligkeit des TableAdapter bei Verwendung von Ad-hoc-SQL-Anweisungen in diesem Tutorial werden wir gespeicherte Prozeduren verwenden. Finden Sie in der [Erstellen neuer gespeicherter Prozeduren für die typisierte DataSet-s TableAdapters](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) und [vorhandene gespeicherte Prozeduren verwenden, für die typisierte DataSet-s TableAdapters](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) Lernprogramme für Weitere Informationen zu Konfigurieren einen TableAdapter um gespeicherte Prozeduren verwenden.

## <a name="step-1-adding-apricequartilecolumn-to-theproductsdatatable"></a>Schritt 1: Hinzufügen einer`PriceQuartile`Spalte in der`ProductsDataTable`

In der *Erstellen neuer gespeicherter Prozeduren für die typisierte DataSet-s TableAdapters* Tutorial, die wir erstellt, eine typisierte DataSet mit dem Namen haben `NorthwindWithSprocs`. Dieses DataSet enthält derzeit zwei DataTables: `ProductsDataTable` und `EmployeesDataTable`. Die `ProductsTableAdapter` weist die folgenden drei Methoden:

- `GetProducts` -die Hauptabfrage, wodurch alle Datensätze aus der `Products` Tabelle
- `GetProductsByCategoryID(categoryID)` -Gibt alle Produkte mit dem angegebenen *CategoryID*.
- `GetProductByProductID(productID)` -Gibt das bestimmte Produkt mit dem angegebenen *"ProductID"*.

Der Hauptabfrage und die zwei zusätzlichen Methoden zurückgeben den gleichen Satz von Datenfeldern, d. h. alle Spalten aus der `Products` Tabelle. Es gibt keine korrelierten Unterabfragen oder `JOIN` s abrufen von verknüpften Daten aus der `Categories` oder `Suppliers` Tabellen. Aus diesem Grund die `ProductsDataTable` verfügt über eine entsprechende Spalte für jedes Feld in der `Products` Tabelle.

In diesem Tutorial können s eine Methode zum Hinzufügen der `ProductsTableAdapter` mit dem Namen `GetProductsWithPriceQuartile` , die alle Produkte zurückgibt. Zusätzlich zu den standard-Product-Daten-Feldern `GetProductsWithPriceQuartile` umfasst auch eine `PriceQuartile` Feld "Daten", der angibt, welche Quartil der Produktpreis s zuzuordnen. Diese Produkte, deren Preise in dem die teuersten 25 werden % müssen z. B. eine `PriceQuartile` Wert von 1, sind diejenigen, deren Preise in der unteren 25 werden % den Wert 4. Bevor wir das Erstellen der gespeicherten Prozedur zum Zurückgeben dieser Informationen kümmern, allerdings müssen wir zuerst zum Aktualisieren der `ProductsDataTable` auf eine Spalte zum Speichern enthalten die `PriceQuartile` Ergebnisse bei der `GetProductsWithPriceQuartile` Methode wird verwendet.

Öffnen der `NorthwindWithSprocs` DataSet mit der rechten Maustaste auf die `ProductsDataTable`. Wählen Sie aus dem Kontextmenü hinzufügen, und wählen Sie dann die Spalte.


[![Fügen Sie eine neue Spalte hinzu, um die ProductsDataTable](adding-additional-datatable-columns-vb/_static/image2.png)](adding-additional-datatable-columns-vb/_static/image1.png)

**Abbildung 1**: Hinzufügen einer neuen Spalte, die `ProductsDataTable` ([klicken Sie, um das Bild in voller Größe anzeigen](adding-additional-datatable-columns-vb/_static/image3.png))


Dadurch wird eine neue Spalte hinzugefügt, zu der mit dem Namen Column1 vom Typ "DataTable" `System.String`. Wir müssen dieser Spaltenname s PriceQuartile und dessen Typ zu aktualisieren `System.Int32` , da er verwendet wird, die eine Zahl zwischen 1 und 4 enthalten soll. Wählen Sie die neu hinzugefügte Spalte in der `ProductsDataTable` , und legen Sie im Eigenschaftenfenster die `Name` PriceQuartile Eigenschaft und die `DataType` Eigenschaft `System.Int32`.


[![Legen Sie die s-Name der neuen Spalte und die Eigenschaften für Datentypen](adding-additional-datatable-columns-vb/_static/image5.png)](adding-additional-datatable-columns-vb/_static/image4.png)

**Abbildung 2**: Legen Sie die neue Spalte s `Name` und `DataType` Eigenschaften ([klicken Sie, um das Bild in voller Größe anzeigen](adding-additional-datatable-columns-vb/_static/image6.png))


Wie in Abbildung 2 gezeigt, gibt es zusätzliche Eigenschaften, die festgelegt werden können, z. B. gibt an, ob die Werte in der Spalte eindeutig sein müssen, wenn die Spalte eine Spalte automatisch inkrementiert wird, ob Datenbank `NULL` Werte sind zulässig und so weiter. Lassen Sie diese Werte, die auf ihre Standardwerte festgelegt.

## <a name="step-2-creating-thegetproductswithpricequartilemethod"></a>Schritt 2: Erstellen der`GetProductsWithPriceQuartile`Methode

Nun, dass die `ProductsDataTable` wurde aktualisiert und enthalten die `PriceQuartile` Spalte sind wir bereit, erstellen die `GetProductsWithPriceQuartile` Methode. Zunächst mit der rechten Maustaste auf den TableAdapter und Abfrage hinzufügen im Kontextmenü auswählen. Dadurch wird der TableAdapter-Abfrage-Konfigurations-Assistenten, in der werden uns zuerst gefragt, ob Ad-hoc-SQL-Anweisungen oder einer neuen oder vorhandenen gespeicherten Prozedur verwendet werden soll. Da wir Einbau t zusätzlichen noch einer gespeicherten Prozedur, der Preis Quartil Daten zurückgibt, können Sie die s den TableAdapter zum Erstellen von dieser gespeicherten Prozedur für uns zu ermöglichen. Wählen Sie die Option neue gespeicherte Prozedur erstellen, und klicken Sie auf Weiter.


[![Weisen Sie die TableAdapter-Assistenten, um die gespeicherte Prozedur für uns erstellen](adding-additional-datatable-columns-vb/_static/image8.png)](adding-additional-datatable-columns-vb/_static/image7.png)

**Abbildung 3**: Weisen Sie den TableAdapter-Assistenten, um die gespeicherte Prozedur für uns erstellen ([klicken Sie, um das Bild in voller Größe anzeigen](adding-additional-datatable-columns-vb/_static/image9.png))


Im folgenden Bildschirm dargestellt in Abbildung 4 des Assistenten werden wir gefragt, welche Art von Abfrage hinzufügen. Da die `GetProductsWithPriceQuartile` Methode gibt alle Spalten und Datensätzen aus der `Products` Tabelle, wählen Sie die Auswahl, welche die gibt Zeilen aus, und klicken Sie auf Weiter.


[![Die Abfrage wird eine SELECT-Anweisung, gibt mehrere Zeilen werden.](adding-additional-datatable-columns-vb/_static/image11.png)](adding-additional-datatable-columns-vb/_static/image10.png)

**Abbildung 4**: unsere Abfrage wird eine `SELECT` Anweisung, gibt mehrere Zeilen ([klicken Sie, um das Bild in voller Größe anzeigen](adding-additional-datatable-columns-vb/_static/image12.png))


Als Nächstes werden wir dazu aufgefordert, für die `SELECT` Abfrage. Geben Sie die folgende Abfrage in den Assistenten aus:


[!code-sql[Main](adding-additional-datatable-columns-vb/samples/sample1.sql)]

Die obige Abfrage verwendet SQL Server 2005-s neue [ `NTILE` Funktion](https://msdn.microsoft.com/library/ms175126.aspx) auf die Ergebnisse in vier Gruppen zu unterteilen, die Gruppen, in denen hängen von, der `UnitPrice` Werte in absteigender Reihenfolge sortiert.

Leider den Abfrage-Generator ist nicht bekannt, wie beim Analysieren der `OVER` Schlüsselwort und wird eine Fehlermeldung angezeigt, wenn die obige Abfrage zu analysieren. Aus diesem Grund geben Sie die obige Abfrage direkt in das Textfeld des Assistenten ohne Verwendung des Abfrage-Generators ein.

> [!NOTE]
> Weitere Informationen zu NTILE und SQL Server 2005-s anderen Rangfolgefunktionen zur finden Sie unter [aufgelisteten Ergebnisse zurückgeben, mit Microsoft SQL Server 2005](http://www.4guysfromrolla.com/webtech/010406-1.shtml) und [Rangfolgefunktionen Abschnitt](https://msdn.microsoft.com/library/ms189798.aspx) aus der [SQL Server 2005-Onlinedokumentation](https://msdn.microsoft.com/library/ms189798.aspx).


Nach dem Eingeben der `SELECT` Abfrage ein, und klicken Sie auf Weiter, fragt der Assistent auf der einen Namen für die gespeicherte Prozedur angegeben wird. Benennen Sie die neue gespeicherte Prozedur `Products_SelectWithPriceQuartile` , und klicken Sie auf Weiter.


[![Name der gespeicherten Prozedur Products_SelectWithPriceQuartile](adding-additional-datatable-columns-vb/_static/image14.png)](adding-additional-datatable-columns-vb/_static/image13.png)

**Abbildung 5**: Benennen Sie die gespeicherte Prozedur `Products_SelectWithPriceQuartile` ([klicken Sie, um das Bild in voller Größe anzeigen](adding-additional-datatable-columns-vb/_static/image15.png))


Schließlich sind wir aufgefordert, benennen Sie die TableAdapter-Methoden. Lassen Sie beide der Füllung eine "DataTable" und einem DataTable-Kontrollkästchen aktiviert und die Namen die Methoden zurückzugeben `FillWithPriceQuartile` und `GetProductsWithPriceQuartile`.


[![Methoden für die TableAdapter-Namen, und klicken Sie auf Fertig stellen](adding-additional-datatable-columns-vb/_static/image17.png)](adding-additional-datatable-columns-vb/_static/image16.png)

**Abbildung 6**: Benennen Sie das s-Methoden des TableAdapter, und klicken Sie auf Fertig stellen ([klicken Sie, um das Bild in voller Größe anzeigen](adding-additional-datatable-columns-vb/_static/image18.png))


Mit der `SELECT` Abfrage angegeben, und die gespeicherte Prozedur und TableAdapter-Methoden, die mit dem Namen, klicken Sie auf "Fertig stellen", um den Assistenten abzuschließen. An diesem Punkt erhalten Sie möglicherweise eine Warnung oder zwei über den Assistenten, dass die `OVER` SQL-Konstrukt oder die Anweisung wird nicht unterstützt. Diese Warnungen können ignoriert werden.

Nach Abschluss des Assistenten sollte der TableAdapter enthalten die `FillWithPriceQuartile` und `GetProductsWithPriceQuartile` Methoden und die Datenbank eine gespeicherte Prozedur namens aufzunehmen `Products_SelectWithPriceQuartile`. Nehmen Sie einen Moment Zeit, um zu überprüfen, dass der TableAdapter tatsächlich diese neue Methode enthält, die gespeicherte Prozedur auf die Datenbank ordnungsgemäß hinzugefügt wurde. Wenn die Datenbank wird überprüft, ob die gespeicherte Prozedur versuchen Sie es mit der rechten Maustaste auf den Ordner "gespeicherte Prozeduren" und "Aktualisieren" nicht angezeigt wird.


![Stellen Sie sicher, dass eine neue Methode des TableAdapter hinzugefügt wurde](adding-additional-datatable-columns-vb/_static/image19.png)

**Abbildung 7**: Stellen Sie sicher, dass eine neue Methode des TableAdapter hinzugefügt wurde


[![Stellen Sie sicher, dass die Datenbank die Products_SelectWithPriceQuartile enthält gespeicherte Prozedur](adding-additional-datatable-columns-vb/_static/image21.png)](adding-additional-datatable-columns-vb/_static/image20.png)

**Abbildung 8**: sicherstellen, dass die Datenbank enthält die `Products_SelectWithPriceQuartile` Stored Procedure ([klicken Sie, um das Bild in voller Größe anzeigen](adding-additional-datatable-columns-vb/_static/image22.png))


> [!NOTE]
> Einer der Vorteile der Verwendung gespeicherter Prozeduren anstelle von Ad-hoc-SQL-Anweisungen ist, dass es sich bei den TableAdapter-Konfigurations-Assistenten erneut ausführen der gespeicherten Prozeduren Spaltenlisten nicht geändert werden. Überprüfen Sie dies können Sie, indem Sie mit der rechten Maustaste auf den TableAdapter, mit der Option konfigurieren aus dem Kontextmenü, um den Assistenten starten, und klicken Sie dann auf "Fertig stellen", um es abzuschließen. Navigieren Sie anschließend auf die Datenbank und die Ansicht der `Products_SelectWithPriceQuartile` gespeicherte Prozedur. Beachten Sie, dass die Liste der Spalten nicht geändert wurde. Hatten wir genutzt haben-Ad-hoc-SQL-Anweisungen den TableAdapter-Konfigurations-Assistenten erneut ausführen würden wurden rückgängig diese Abfrage s Spaltenliste der Hauptabfrage Spaltenliste entfernen und die NTILE-Anweisung aus der Abfrage ein, die entsprechend der `GetProductsWithPriceQuartile` Methode.


Bei der Datenzugriffsebene s `GetProductsWithPriceQuartile` Methode aufgerufen wird, führt des TableAdapters die `Products_SelectWithPriceQuartile` von gespeicherten Prozeduren und fügt eine Zeile in der `ProductsDataTable` für jeden Datensatz zurückgegebenen. Die Datenfelder, die von der gespeicherten Prozedur zurückgegebenen zugeordnet sind die `ProductsDataTable` s-Spalten. Da gibt es eine `PriceQuartile` Datenfeld zurückgegeben wird, von der gespeicherten Prozedur, dessen Wert zugewiesen wird die `ProductsDataTable` s `PriceQuartile` Spalte.

Für die TableAdapter-Methoden, deren Abfragen geben keine zurück, eine `PriceQuartile` Datenfeld, das `PriceQuartile` Spaltenwert s ist der Wert von dessen `DefaultValue` Eigenschaft. Wie in Abbildung 2 gezeigt, wird dieser Wert festgelegt, um `DBNull`, der Standardwert. Wenn Sie einen anderen Standardwert möchten, legen Sie einfach die `DefaultValue` Eigenschaft entsprechend. Achten Sie darauf, die die `DefaultValue` Wert ist gültig. die Spalte s `DataType` (d. h. `System.Int32` für die `PriceQuartile` Spalte).

An diesem Punkt haben wir die erforderlichen Schritte ausgeführt, für das Hinzufügen einer zusätzlichen Spalte zu einer DataTable. Um sicherzustellen, dass diese zusätzliche Spalte wie erwartet funktioniert, können Sie s eine ASP.NET-Seite erstellen, in dem jede s Produktname, Preis und Preis Quartil angezeigt. Bevor wir dies tun, jedoch müssen wir zuerst aktualisieren Sie die Business Logic Layer, um eine Methode enthalten, die die DAL-s-nach unten Aufrufe `GetProductsWithPriceQuartile` Methode. Wir als Nächstes aktualisieren Sie die BLL in Schritt 3, und klicken Sie dann die ASP.NET-Seite in Schritt 4 erstellen.

## <a name="step-3-augmenting-the-business-logic-layer"></a>Schritt 3: Erweitern den Geschäftslogikebene

Bevor wir die Verwendung der neuen `GetProductsWithPriceQuartile` Methode auf der Darstellungsschicht, sollten wir zuerst eine entsprechende Methode an die BLL hinzufügen. Öffnen der `ProductsBLLWithSprocs` Klassendatei, und fügen Sie den folgenden Code hinzu:


[!code-vb[Main](adding-additional-datatable-columns-vb/samples/sample2.vb)]

Wie andere Datenabrufmethoden in `ProductsBLLWithSprocs`, `GetProductsWithPriceQuartile` Methode ruft einfach die DAL s entspricht `GetProductsWithPriceQuartile` Methode und die Ergebnisse werden zurückgegeben.

## <a name="step-4-displaying-the-price-quartile-information-in-an-aspnet-web-page"></a>Schritt 4: Anzeigen der Quartil Preisinformationen in einer ASP.NET-Webseite

Führen wir außerdem mit der BLL erneut bereit, um eine ASP.NET-Seite zu erstellen, die Quartile Preis für jedes Produkt anzeigt. Öffnen der `AddingColumns.aspx` auf der Seite die `AdvancedDAL` Ordner, und ziehen Sie einer GridView-Ansicht aus der Toolbox auf den Designer, Festlegen der `ID` Eigenschaft `Products`. Von GridView s Smarttags, binden Sie es an eine neue, mit dem Namen "ObjectDataSource" `ProductsDataSource`. Konfigurieren Sie mit dem ObjectDataSource-Steuerelement die `ProductsBLLWithSprocs` Klasse s `GetProductsWithPriceQuartile` Methode. Da dies ein nur-Lese Raster sein wird, legen Sie die Dropdownlisten in der Update-, INSERT-, und Löschen von Registerkarten (keine) aus.


[![Konfigurieren von dem ObjectDataSource-Steuerelement zur Verwendung der ProductsBLLWithSprocs-Klasse](adding-additional-datatable-columns-vb/_static/image24.png)](adding-additional-datatable-columns-vb/_static/image23.png)

**Abbildung 9**: Konfigurieren von dem ObjectDataSource-Steuerelement verwenden die `ProductsBLLWithSprocs` Klasse ([klicken Sie, um das Bild in voller Größe anzeigen](adding-additional-datatable-columns-vb/_static/image25.png))


[![Abrufen von Produktinformationen aus der GetProductsWithPriceQuartile-Methode](adding-additional-datatable-columns-vb/_static/image27.png)](adding-additional-datatable-columns-vb/_static/image26.png)

**Abbildung 10**: Abrufen von Produktinformationen aus der `GetProductsWithPriceQuartile` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](adding-additional-datatable-columns-vb/_static/image28.png))


Nach Abschluss der Konfigurieren von Datenquellen-Assistenten wird Visual Studio automatisch eine BoundField- oder CheckBoxField hinzufügen an die GridView für die einzelnen Datenfelder, die von der Methode zurückgegebenen. Ist eines dieser Datenfelder `PriceQuartile`, d.h., dass die Spalte, die wir hinzugefügt, die `ProductsDataTable` in Schritt 1.

Bearbeiten Sie die Felder GridView s, entfernen alle außer den `ProductName`, `UnitPrice`, und `PriceQuartile` BoundFields. Konfigurieren der `UnitPrice` BoundField, dessen Wert als Währung zu formatieren und die `UnitPrice` und `PriceQuartile` BoundFields rechts - und zentriert bzw. Zum Schluss aktualisieren Sie die verbleibenden BoundFields `HeaderText` Eigenschaften für das Produkt, Preis und Preis Quartil, bzw. Darüber hinaus das Kontrollkästchen Sie sortieren aktivieren aus dem GridView-s-Smarttag.

Nachdem diese Änderungen sollten GridView und "ObjectDataSource" s deklarative Markup wie folgt aussehen:


[!code-aspx[Main](adding-additional-datatable-columns-vb/samples/sample3.aspx)]

Abbildung 11 zeigt diese Seite, wenn über einen Browser aufgerufen. Beachten Sie, dass zunächst die Produkte nach Preis in absteigender Reihenfolge mit den einzelnen Produkten, die eine entsprechende zugewiesen sortiert werden `PriceQuartile` Wert. Natürlich können diese Daten nach anderen Kriterien sortiert werden, mit dem Preis Quartil-Spaltenwert weiterhin reflektieren die Rangfolge Produkt s in Bezug auf den Preis (siehe Abbildung 12).


[![Die Produkte werden nach deren Preise sortiert.](adding-additional-datatable-columns-vb/_static/image30.png)](adding-additional-datatable-columns-vb/_static/image29.png)

**Abbildung 11**: die Produkte werden nach deren Preise sortiert ([klicken Sie, um das Bild in voller Größe anzeigen](adding-additional-datatable-columns-vb/_static/image31.png))


[![Die Produkte werden nach Namen sortiert.](adding-additional-datatable-columns-vb/_static/image33.png)](adding-additional-datatable-columns-vb/_static/image32.png)

**Abbildung 12**: die Produkte werden nach Namen sortiert ([klicken Sie, um das Bild in voller Größe anzeigen](adding-additional-datatable-columns-vb/_static/image34.png))


> [!NOTE]
> Mit wenigen Codezeilen können wir GridView, damit sie die Produktzeilen, die basierend auf gefärbt erweitern ihre `PriceQuartile` Wert. Wir können diese Produkte in das erste Quartil hellgrün, die in der zweiten Quartil eine Hosttags in Gelb, Farbe und so weiter. Sollten Sie diese Funktionalität hinzufügen werden. Wenn Sie Ihre Kenntnisse über die Formatierung einer GridView-Ansicht benötigen, wenden Sie sich an den [benutzerdefinierte Formatierung basierend auf Daten](../custom-formatting/custom-formatting-based-upon-data-vb.md) Tutorial.


## <a name="an-alternative-approach---creating-another-tableadapter"></a>Eine Alternative Methode: erstellen eine andere TableAdapter

Wie in diesem Tutorial, beim Hinzufügen einer Methode zu einem TableAdapter, die die Datenfelder als zurückgibt durch die Hauptabfrage ausgeschrieben erläutert, können wir die entsprechende Spalten der Datentabelle hinzufügen. Ein solcher Ansatz, allerdings funktioniert gut, nur, wenn es gibt eine kleine Anzahl von Methoden in den TableAdapter, die verschiedenen Datenfeldern zurückgeben und diese alternativen Datenfelder nicht aus der Hauptabfrage zu stark variieren.

Anstelle der Datentabelle Spalten hinzugefügt haben, können Sie stattdessen eine andere TableAdapter zum DataSet hinzufügen, die die Methoden aus dem ersten TableAdapter enthält, die verschiedenen Datenfeldern zurückgeben. Für dieses Tutorial anstatt hinzuzufügen der `PriceQuartile` Spalte die `ProductsDataTable` (, in dem er dient lediglich durch die `GetProductsWithPriceQuartile` Methode), wir hätten einen zusätzlichen TableAdapter auf das DataSet mit dem Namen `ProductsWithPriceQuartileTableAdapter` , verwendet der `Products_SelectWithPriceQuartile` gespeichert die Prozedur als der Hauptabfrage. ASP.NET-Seiten, die zum Abrufen von Produktinformationen, mit dem Preis Quartil erforderlich ist, verwenden die `ProductsWithPriceQuartileTableAdapter`, während solche, die nicht weiterhin verwenden, können die `ProductsTableAdapter`.

Durch das Hinzufügen eines neuen TableAdapter, die DataTables untarnished bleiben, und ihre Spalten spiegeln genau die Datenfelder, die von ihren TableAdapter-s-Methoden zurückgegeben werden. Allerdings können zusätzliche TableAdapters sich wiederholende Aufgaben und Funktionen einführen. Z. B., wenn diese ASP.NET Seiten angezeigten der `PriceQuartile` Spalte auch erforderlich zu Einfüge-, aktualisieren und Löschen von Support, die `ProductsWithPriceQuartileTableAdapter` benötigen die `InsertCommand`, `UpdateCommand`, und `DeleteCommand` Eigenschaften ordnungsgemäß konfiguriert. Während diese Eigenschaften zu spiegeln, würde die `ProductsTableAdapter` s, diese Konfiguration stellt einen zusätzlichen Schritt. Außerdem sind nun zwei Möglichkeiten, zu aktualisieren, löschen oder Hinzufügen eines Produkts mit der Datenbank - über die `ProductsTableAdapter` und `ProductsWithPriceQuartileTableAdapter` Klassen.

Der Download für dieses Tutorial enthält eine `ProductsWithPriceQuartileTableAdapter` -Klasse in der `NorthwindWithSprocs` DataSet, das diese alternative Methode veranschaulicht.

## <a name="summary"></a>Zusammenfassung

In den meisten Fällen den gleichen Satz von Datenfeldern alle Methoden in einem TableAdapter zurück, aber es gibt Situationen, wenn eine bestimmte Methode oder zwei möglicherweise ein zusätzliches Feld zurück. Z. B. in der [Master/Detail, verwenden eine Liste mit Aufzählungszeichen Liste der Masterdatensätze mit einem DataList-Steuerelement Details](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md) Tutorial, die wir hinzugefügt, dass eine Methode, um die `CategoriesTableAdapter` , zusätzlich zu der Hauptabfrage s Datenfelder, die zurückgegeben eine `NumberOfProducts` Datenfelds, die Anzahl der Produkte jeder Kategorie zugeordnet wird gemeldet. In diesem Tutorial erläutert, Hinzufügen einer Methode in der `ProductsTableAdapter` zurückgegebenen eine `PriceQuartile` Feld zusätzlich zu den Feldern der Hauptabfrage s-Daten. Zum Erfassen von zusätzlicher Daten werden Felder durch den TableAdapter-s-Methoden zurückgegeben, dass die entsprechende Spalten der Datentabelle hinzufügen möchten.

Wenn Sie manuell Spalten der Datentabelle hinzufügen möchten, empfiehlt es sich, dass der TableAdapter gespeicherte Prozeduren verwenden. Wenn Ad-hoc-SQL-Anweisungen in der TableAdapter verwendet wird, wird jedes Mal der TableAdapter-Konfigurations-Assistenten alle Methoden ausgeführt die Datenfelder, die von der main-Abfrage zurückgegebenen Daten Feldlisten wiederherstellen. Dieses Problem erstreckt sich nicht gespeicherte Prozeduren verwendet, ist diese werden empfohlen und in diesem Tutorial verwendet wurden.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial wurden Randy Schmidt, Jacky Goor, Bernadette Leigh und Hilton Giesenow. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](updating-the-tableadapter-to-use-joins-vb.md)
> [Weiter](working-with-computed-columns-vb.md)
