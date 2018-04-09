---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/updating-the-tableadapter-to-use-joins-vb
title: Aktualisieren des TableAdapters zu verwendenden verknüpft (VB) | Microsoft Docs
author: rick-anderson
description: Bei der Arbeit mit einer Datenbank ist es üblich, Anforderungsdaten verwendet werden, die über mehrere Tabellen verteilt werden. Zum Abrufen von Daten aus zwei verschiedenen Tabellen können wir entweder...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: e624a3e0-061b-4efc-8b0e-5877f9ff6714
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/updating-the-tableadapter-to-use-joins-vb
msc.type: authoredcontent
ms.openlocfilehash: 91d700f3de02dc78692e933644e221e2ac8175a1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="updating-the-tableadapter-to-use-joins-vb"></a>Aktualisieren des TableAdapters zu verwendenden verknüpft (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Herunterladen von Code](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_69_VB.zip) oder [PDF herunterladen](updating-the-tableadapter-to-use-joins-vb/_static/datatutorial69vb1.pdf)

> Bei der Arbeit mit einer Datenbank ist es üblich, Anforderungsdaten verwendet werden, die über mehrere Tabellen verteilt werden. Zum Abrufen von Daten aus zwei verschiedenen Tabellen können wir eine korrelierte Unterabfrage oder ein JOIN-Vorgang. In diesem Lernprogramm Vergleichen wir korrelierte Unterabfragen und die JOIN-Syntax vor dem Blick auf einen TableAdapter erstellen, die einen JOIN in der Hauptabfrage enthält.


## <a name="introduction"></a>Einführung

Bei relationalen Datenbanken werden die Daten, die wir bei der Arbeit mit Interesse sind oft auf mehrere Tabellen verteilt. Z. B. beim Anzeigen von Produktinformationen wir wahrscheinlich so Listen Sie jede entsprechende s Produktkategorie und s Lieferantennamen. Die `Products` Tabelle hat `CategoryID` und `SupplierID` Werte, aber die tatsächlichen Namen der Kategorie und Lieferanten befinden sich in der `Categories` und `Suppliers` Tabellen.

Abrufen von Informationen aus einer anderen, verwandten Tabelle können wir entweder *korrelierte Unterabfragen* oder `JOIN` *s*. Eine korrelierte Unterabfrage ist eine geschachtelte `SELECT` Abfrage, die Spalten in der äußeren Abfrage verweist. Beispielsweise ist in der [erstellen eine Datenzugriffsschicht](../introduction/creating-a-data-access-layer-vb.md) Lernprogramm wir zwei korrelierte Unterabfragen in verwendet der `ProductsTableAdapter` Hauptabfrage s, den Namen der Kategorie und Lieferanten für jedes Produkt zurückgegeben. Ein `JOIN` ist eine SQL-Konstrukt, das verknüpfte Zeilen aus zwei verschiedenen Tabellen zusammengeführt. Es verwendet ein `JOIN` in der [Abfragen von Daten mit SqlDataSource-Steuerelement](../accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-vb.md) Lernprogramm aus, um Informationen zu Auftragskategorien neben jedem Produkt anzuzeigen.

Der Grund, wir haben verbunden, von der Verwendung `JOIN` s mit TableAdapters ist aufgrund von Einschränkungen bezüglich der TableAdapter-s-Assistent automatisch entsprechende generiert `INSERT`, `UPDATE`, und `DELETE` Anweisungen. Genauer gesagt, wenn die Hauptabfrage des TableAdapter s enthält `JOIN` s, TableAdapter kann keine automatischen Erstellen von der Ad-hoc-SQL-Anweisungen oder gespeicherte Prozeduren für die `InsertCommand`, `UpdateCommand`, und `DeleteCommand` Eigenschaften.

In diesem Lernprogramm, wir kurz vergleichen wird und Kontrast korrelierte Unterabfragen und `JOIN` s vor untersuchen wie einen TableAdapter erstellen, enthält `JOIN` s in der Hauptabfrage.

## <a name="comparing-and-contrasting-correlated-subqueries-andjoin-s"></a>Vergleichen und gegenüberstellen korrelierte Unterabfragen und`JOIN` s

Bedenken Sie, dass die `ProductsTableAdapter` in im ersten Lernprogramm erstellt die `Northwind` DataSet korrelierte Unterabfragen s entsprechende Kategorie und Lieferanten Produktnamen wieder zurückbekommen verwendet. Die `ProductsTableAdapter` s Hauptabfrage wird unten gezeigt.


[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample1.sql)]

Die beiden korrelierte Unterabfragen - `(SELECT CategoryName FROM Categories WHERE Categories.CategoryID = Products.CategoryID)` und `(SELECT CompanyName FROM Suppliers WHERE Suppliers.SupplierID = Products.SupplierID)` -sind `SELECT` Abfragen, die einen einzelnen Wert pro Produkt als eine zusätzliche Spalte in der äußeren zurückgeben `SELECT` Anweisung s Spaltenliste.

Alternativ können Sie eine `JOIN` können verwendet werden, um jede s Supplier "und" Kategorie Produktname zurückgegeben. Die folgende Abfrage gibt dieselbe Ausgabe wie oben, verwendet jedoch `JOIN` s anstelle von Unterabfragen:


[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample2.sql)]

Ein `JOIN` Datensätze aus einer Tabelle mit Datensätzen aus einer anderen Tabelle basierend auf bestimmte Kriterien. In der obigen Abfrage, z. B. die `LEFT JOIN Categories ON Categories.CategoryID = Products.CategoryID` weist SQL Server jedes Zusammenführen Produktdatensatz, der Kategorie aufzeichnen, deren `CategoryID` Wert entspricht das Produkt s `CategoryID` Wert. Zusammengeführte Ergebnis erlaubt es uns, mit der entsprechenden Kategoriefelder für jedes Produkt arbeiten (z. B. `CategoryName`).

> [!NOTE]
> `JOIN` s werden häufig verwendet, bei der Abfrage von Daten aus relationalen Datenbanken. Wenn Sie neu der `JOIN` Syntax oder müssen auf ihre Nutzung etwas auffrischen d sollten die [SQL Join Lernprogramm](http://www.w3schools.com/sql/sql_join.asp) am [W3 Schulen](http://www.w3schools.com/). Auch Folgendes zu lesen sind die [ `JOIN` Grundlagen](https://msdn.microsoft.com/library/ms191517.aspx) und [Unterabfrage Grundlagen](https://msdn.microsoft.com/library/ms189575.aspx) Abschnitte der [SQL-Onlinedokumentation](https://msdn.microsoft.com/library/ms130214.aspx).


Da `JOIN` s und korrelierte Unterabfragen können beide werden verwendet, um verwandte Daten aus anderen Tabellen abzurufen, für die meisten Entwickler bleiben ihre Köpfe beträchtliche und Fragen Sie sich, welchen Ansatz verwenden. Alle SQL-Experten ich Ve gesprochen um ungefähr die gleiche Aufgabe gesagt haben, dass die It t verfügt über eine Rolle spielen performance-wise wie SQL Server ungefähr identische Ausführungspläne erstellt werden. Ihre Ratschläge ist dann das Verfahren verwenden, dem Sie und Ihr Team am häufigsten mit vertraut sind. Sie verdient Beachten Sie, dass diese Experten nach diesen Rat imparting sofort ihre Einstellung express `JOIN` s über korrelierte Unterabfragen.

Beim Erstellen eine Datenzugriffsschicht typisierter DataSets mithilfe der Tools besser geeignet verwenden Unterabfragen. Insbesondere die TableAdapter-s-Assistent nicht automatisch generiert entsprechende `INSERT`, `UPDATE`, und `DELETE` Anweisungen, wenn die Hauptabfrage enthält `JOIN` s, aber generiert automatisch diese Anweisungen beim korrelierte Unterabfragen werden verwendet.

Um diese Schwierigkeit zu untersuchen, erstellen Sie einen temporären typisierte DataSet in den `~/App_Code/DAL` Ordner. Bei den TableAdapter-Konfigurations-Assistenten wählen, Ad-hoc-SQL-Anweisungen verwenden, und geben den folgenden `SELECT` Abfrage (siehe Abbildung 1):


[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample3.sql)]


[![Geben Sie eine Hauptspeicherpool für Abfragen, die JOINs enthält.](updating-the-tableadapter-to-use-joins-vb/_static/image2.png)](updating-the-tableadapter-to-use-joins-vb/_static/image1.png)

**Abbildung 1**: Geben Sie eine Main-Abfrage, der enthält `JOIN` s ([klicken Sie hier, um das Bild in voller Größe angezeigt](updating-the-tableadapter-to-use-joins-vb/_static/image3.png))


Standardmäßig Erstellen der TableAdapter automatisch `INSERT`, `UPDATE`, und `DELETE` Anweisungen anhand der Hauptabfrage. Wenn Sie auf die Schaltfläche "Erweitert" klicken, sehen Sie, dass dieses Feature aktiviert ist. Trotz dieser Einstellung wird der TableAdapter nicht möglich, erstellen die `INSERT`, `UPDATE`, und `DELETE` Anweisungen, da die Hauptabfrage enthält eine `JOIN`.


![Geben Sie eine Hauptspeicherpool für Abfragen, die JOINs enthält.](updating-the-tableadapter-to-use-joins-vb/_static/image4.png)

**Abbildung 2**: Geben Sie eine Hauptspeicherpool für Abfragen, die enthält `JOIN` s


Klicken Sie auf ' Fertig stellen ', um den Assistenten abzuschließen. An diesem Punkt wird die DataSet-s-Designer einen einzelnen TableAdapter mit einer DataTable mit Spalten enthalten, für jede die zurückgegebenen Felder in der `SELECT` Abfrage s Spaltenliste. Dies schließt die `CategoryName` und `SupplierName`, wie in Abbildung 3 dargestellt.


![DataTable enthält eine Spalte für jedes Feld in der Spaltenliste zurückgegeben](updating-the-tableadapter-to-use-joins-vb/_static/image5.png)

**Abbildung 3**: DataTable enthält eine Spalte für jedes Feld in der Spaltenliste zurückgegeben


Während der DataTable die entsprechenden Spalten verfügt, wird der TableAdapter verfügt nicht über Werte für die `InsertCommand`, `UpdateCommand`, und `DeleteCommand` Eigenschaften. Um dies zu bestätigen, klicken Sie auf den TableAdapter im Designer, und wechseln Sie dann auf Eigenschaftenfenster. Es wird angezeigt, die die `InsertCommand`, `UpdateCommand`, und `DeleteCommand` Eigenschaften werden festgelegt (keine).


[![Die InsertCommand, UpdateCommand und DeleteCommand Eigenschaften werden auf (keine) festgelegt.](updating-the-tableadapter-to-use-joins-vb/_static/image7.png)](updating-the-tableadapter-to-use-joins-vb/_static/image6.png)

**Abbildung 4**: die `InsertCommand`, `UpdateCommand`, und `DeleteCommand` Eigenschaften werden festgelegt (None) ([klicken Sie hier, um das Bild in voller Größe angezeigt](updating-the-tableadapter-to-use-joins-vb/_static/image8.png))


Zur Umgehung dieser Schwierigkeit, wir können manuell Geben Sie den SQL-Anweisungen und Parameter für die `InsertCommand`, `UpdateCommand`, und `DeleteCommand` Eigenschaften über das Eigenschaftenfenster. Es können auch beginnen, durch Konfigurieren der TableAdapter Hauptabfrage s zum *nicht* enthalten `JOIN` s. Dadurch kann die `INSERT`, `UPDATE`, und `DELETE` Anweisungen, die automatisch für uns generiert werden. Nach Abschluss des Assistenten an, wir konnten dann manuell aktualisieren die TableAdapter `SelectCommand` über das Eigenschaftenfenster so, dass die It umfasst die `JOIN` Syntax.

Dieser Ansatz funktioniert, zwar es ist sehr jedoch fehleranfällig, wenn Ad-hoc-SQL-Abfragen verwenden, da jedes Mal, wenn die Hauptabfrage des TableAdapter s neu konfiguriert, über den Assistenten, die automatisch generierte `INSERT`, `UPDATE`, und `DELETE` Anweisungen neu erstellt werden. Das bedeutet, dass alle wir später vorgenommenen Anpassungen verloren gehen würden wir mit der rechten Maustaste auf den TableAdapter konfigurieren aus dem Kontextmenü auswählen und den Assistenten erneut abgeschlossen.

Die unzulänglichen der automatisch generierten TableAdapter-Zuordnungsvorgänge `INSERT`, `UPDATE`, und `DELETE` Anweisungen ist, Glücklicherweise auf Ad-hoc-SQL-Anweisungen beschränkt. Wenn der TableAdapter gespeicherte Prozeduren verwendet wird, können Sie Anpassen der `SelectCommand`, `InsertCommand`, `UpdateCommand`, oder `DeleteCommand` gespeicherte Prozeduren und führen Sie den TableAdapter-Konfigurations-Assistenten erneut aus, ohne jedoch, dass die gespeicherten Prozeduren werden geändert.

Über den nächsten verwendet mehrere Schritte, die wir erstellt einen TableAdapter, die zu Beginn einer hauptspeicherpool für Abfragen, die alle lässt `JOIN` s, damit die entsprechenden einfügen, aktualisieren und Löschen gespeicherter Prozeduren automatisch generiert werden. Wir aktualisieren Sie dann die `SelectCommand` so verwendet, die eine `JOIN` , zusätzliche Spalten aus verknüpften Tabellen zurückgibt. Schließlich wir erstellen Sie eine entsprechende Business Logic Layer-Klasse und veranschaulicht die Verwendung des TableAdapters in eine ASP.NET-Webseite.

## <a name="step-1-creating-the-tableadapter-using-a-simplified-main-query"></a>Schritt 1: Erstellen der TableAdapter eine vereinfachte Hauptspeicherpool für Abfragen mit

Für dieses Lernprogramm fügen wir ein TableAdapter und stark typisierte DataTable für die `Employees` -Tabelle in der `NorthwindWithSprocs` DataSet. Die `Employees` Tabelle enthält eine `ReportsTo` angegebenen Feld der `EmployeeID` des Mitarbeiters s-Managers. Beispielsweise Mitarbeiter Anne Dodsworth hat eine `ReportTo` Abstand von 5, also die `EmployeeID` von Steven Buchanan. Folglich berichtet Anne Stefan, ihrem Manager. Zusammen mit jeder Mitarbeiter s reporting `ReportsTo` Wert, es kann auch der Name des Vorgesetzten abrufen möchten. Dies geschieht mithilfe einer `JOIN`. Während mit einer `JOIN` beim anfänglich Erstellung des TableAdapter, der Assistent generiert automatisch die entsprechenden Insert schließt, Update- und delete-Funktionen. Daher beginnen wir mit dem Erstellen eines TableAdapters, deren Hauptabfrage keine enthält `JOIN` s. Klicken Sie dann in Schritt2 haben wir aktualisieren die Hauptabfrage gespeicherte Prozedur zum Abrufen der Managername s über eine `JOIN`.

Öffnen Sie zunächst die `NorthwindWithSprocs` DataSet in den `~/App_Code/DAL` Ordner. Mit der rechten Maustaste auf den Designer, wählen Sie im Kontextmenü die Option "hinzufügen", und wählen Sie das Menüelement TableAdapter. Hierdurch wird der TableAdapter-Konfigurations-Assistent gestartet. Wie Abbildung 5 zeigt, müssen Sie den Assistenten neue gespeicherte Prozeduren erstellen, und klicken Sie auf Weiter. Für aufzufrischen zum Erstellen neuer Prozeduren aus dem TableAdapter-s-Assistenten gespeichert wird, wenden Sie sich an den [Erstellen neuer gespeicherter Prozeduren für die typisierte DataSet s TableAdapters](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) Lernprogramm.


[![Wählen Sie die neuen gespeicherten Prozeduren Option für das Erstellen](updating-the-tableadapter-to-use-joins-vb/_static/image10.png)](updating-the-tableadapter-to-use-joins-vb/_static/image9.png)

**Abbildung 5**: Wählen Sie das Erstellen neuer Procedures (Option) gespeichert ([klicken Sie hier, um das Bild in voller Größe angezeigt](updating-the-tableadapter-to-use-joins-vb/_static/image11.png))


Verwenden Sie die folgende `SELECT` -Anweisung für die Hauptabfrage des TableAdapter s:


[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample4.sql)]

Da diese Abfrage nicht einschließt `JOIN` s, der TableAdapter-Assistenten erstellt automatisch gespeicherte Prozeduren mit den entsprechenden `INSERT`, `UPDATE`, und `DELETE` Anweisungen als auch eine gespeicherte Prozedur für den Hauptknoten ausführen Abfrage.

Der folgende Schritt kann Wir nennen Sie die TableAdapter s gespeicherten Prozeduren. Verwenden Sie die Namen `Employees_Select`, `Employees_Insert`, `Employees_Update`, und `Employees_Delete`, wie in Abbildung 6 veranschaulicht.


[![Name der TableAdapter s gespeicherten Prozeduren](updating-the-tableadapter-to-use-joins-vb/_static/image13.png)](updating-the-tableadapter-to-use-joins-vb/_static/image12.png)

**Abbildung 6**: Benennen der TableAdapter s Stored Procedures ([klicken Sie hier, um das Bild in voller Größe angezeigt](updating-the-tableadapter-to-use-joins-vb/_static/image14.png))


Der letzte Schritt werden Sie aufgefordert, benennen Sie die TableAdapter-s-Methoden. Verwendung `Fill` und `GetEmployees` als den Methodennamen. Achten Sie auch darauf, um die Methoden erstellen, um Updates direkt an die Datenbank (GenerateDBDirectMethods) das Kontrollkästchen aktiviert senden zu lassen.


[![Name der TableAdapter s Methoden ausfüllen und GetEmployees](updating-the-tableadapter-to-use-joins-vb/_static/image16.png)](updating-the-tableadapter-to-use-joins-vb/_static/image15.png)

**Abbildung 7**: Benennen Sie die TableAdapter-Methoden `Fill` und `GetEmployees` ([klicken Sie hier, um das Bild in voller Größe angezeigt](updating-the-tableadapter-to-use-joins-vb/_static/image17.png))


Nehmen Sie nach Abschluss des Assistenten einen Moment Zeit, um die gespeicherten Prozeduren in der Datenbank zu überprüfen. Sehen Sie vier neue: `Employees_Select`, `Employees_Insert`, `Employees_Update`, und `Employees_Delete`. Überprüfen Sie als Nächstes die `EmployeesDataTable` und `EmployeesTableAdapter` gerade erstellt haben. DataTable enthält eine Spalte für jedes Feld, das von der Hauptabfrage zurückgegeben. Klicken Sie auf den TableAdapter, und fahren Sie dann auf Eigenschaftenfenster. Es wird angezeigt, die die `InsertCommand`, `UpdateCommand`, und `DeleteCommand` Eigenschaften sind ordnungsgemäß so konfiguriert, dass die entsprechenden gespeicherten Prozeduren aufrufen.


[![TableAdapter enthält INSERT-, Update- und Löschen von Funktionen](updating-the-tableadapter-to-use-joins-vb/_static/image19.png)](updating-the-tableadapter-to-use-joins-vb/_static/image18.png)

**Abbildung 8**: der TableAdapter Insert enthält, aktualisieren und löschen Funktionen ([klicken Sie hier, um das Bild in voller Größe angezeigt](updating-the-tableadapter-to-use-joins-vb/_static/image20.png))


Nach dem Einfügen, aktualisieren und Löschen von gespeicherten Prozeduren, die automatisch erstellt und die `InsertCommand`, `UpdateCommand`, und `DeleteCommand` Eigenschaften ordnungsgemäß konfiguriert, wir können zum Anpassen der `SelectCommand` s gespeicherte Prozedur, um zusätzliche zurückzugeben Informationen zu jedem Mitarbeiter s-Manager. Insbesondere müssen wir aktualisieren die `Employees_Select` gespeicherte Prozedur mit einer `JOIN` und die s-Manager zurückkehren, `FirstName` und `LastName` Werte. Nachdem die gespeicherte Prozedur aktualisiert wurde, müssen wir die DataTable zu aktualisieren, sodass sie diese zusätzlichen Spalten enthält. Wir werden diese beiden Aufgaben in den Schritten 2 und 3 konfigurieren.

## <a name="step-2-customizing-the-stored-procedure-to-include-ajoin"></a>Schritt 2: Anpassen der gespeicherten Prozedur enthalten ein`JOIN`

Starten, indem Sie im Server-Explorer, Drilldowns in den Ordner Northwind s-Datenbank gespeicherte Prozeduren, und Öffnen der `Employees_Select` gespeicherten Prozedur. Wenn Sie diese gespeicherte Prozedur nicht angezeigt werden, mit der rechten Maustaste auf den Ordner gespeicherte Prozeduren, und wählen Sie aktualisieren. Aktualisieren Sie die gespeicherte Prozedur so, dass es verwendet ein `LEFT JOIN` zuerst den Manager s zurückgegeben und Nachnamen:


[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample5.sql)]

Nach der Aktualisierung der `SELECT` -Anweisung, speichern Sie die Änderungen, indem Sie im Menü Datei aufrufen und Auswählen von Speichern `Employees_Select`. Sie können Alternativ klicken Sie auf das Symbol "Speichern" in der Symbolleiste oder drücken Sie STRG + S. Nach dem Speichern der Änderungen an, mit der Maustaste auf die `Employees_Select` gespeicherte Prozedur im Server-Explorer, und wählen Sie Execute. Dies führen Sie die gespeicherte Prozedur und Anzeigen der Ergebnisse im Ausgabefenster angezeigt wird (siehe Abbildung 9).


[![Die Ergebnisse der gespeicherten Prozeduren werden im Ausgabefenster angezeigt.](updating-the-tableadapter-to-use-joins-vb/_static/image22.png)](updating-the-tableadapter-to-use-joins-vb/_static/image21.png)

**Abbildung 9**: die gespeicherten Prozeduren Ergebnisse werden im Ausgabefenster angezeigt ([klicken Sie hier, um das Bild in voller Größe angezeigt](updating-the-tableadapter-to-use-joins-vb/_static/image23.png))


## <a name="step-3-updating-the-datatable-s-columns"></a>Schritt 3: Aktualisieren der DataTable-s-Spalten

An diesem Punkt der `Employees_Select` gespeicherte Prozedur gibt `ManagerFirstName` und `ManagerLastName` Werte, aber die `EmployeesDataTable` fehlt für diese Spalten. Diese fehlenden Spalten können in das DataTable auf zwei Arten hinzugefügt werden:

- **Manuell** – mit der rechten Maustaste auf die DataTable im DataSet-Designer und wählen Sie aus, klicken Sie im Menü hinzufügen. Sie können den Namen der Spalte und Festlegen von Eigenschaften entsprechend.
- **Automatisch** -der TableAdapter-Konfigurations-Assistent aktualisiert die DataTable-s-Spalten entsprechend der von zurückgegebenen Felder die `SelectCommand` gespeicherte Prozedur. Wenn Sie Ad-hoc-SQL-Anweisungen verwenden, wird der Assistent auch Entfernen der `InsertCommand`, `UpdateCommand`, und `DeleteCommand` Eigenschaften seit der `SelectCommand` enthält jetzt eine `JOIN`. Während diese Befehlseigenschaften bei der Verwendung von gespeicherten Prozeduren, jedoch intakt bleiben.

Wir haben manuelle Hinzufügen von DataTable-Spalten in der vorherigen Lernprogramme, darunter auch untersucht [Master/Detail, verwenden eine Aufzählung der Master-Datensätze mit Details DataList](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md) und [Hochladen von Dateien](../working-with-binary-files/uploading-files-vb.md), und wir werden sollen. Betrachten Sie diesen Vorgang erneut im Detail in unserem nächsten Lernprogramm aus. Können Sie für dieses Lernprogramm jedoch, s, die die automatische Methode über den TableAdapter-Konfigurations-Assistenten zu verwenden.

Starten, indem Sie mit der rechten Maustaste auf die `EmployeesTableAdapter` und konfigurieren Sie im Kontextmenü auswählen. Daraufhin wird der TableAdapter-Konfigurations-Assistenten, in der Listet die gespeicherten Prozeduren zum auswählen, einfügen, aktualisieren und löschen, sowie deren Rückgabewerte und Parameter (sofern vorhanden). Abbildung 10 zeigt dieses Assistenten. Hier sehen wir, dass die `Employees_Select` gibt die gespeicherte Prozedur die `ManagerFirstName` und `ManagerLastName` Felder.


[![Der Assistent zeigt der aktualisierten Spaltenliste für den Employees_Select gespeicherte Prozedur](updating-the-tableadapter-to-use-joins-vb/_static/image25.png)](updating-the-tableadapter-to-use-joins-vb/_static/image24.png)

**Abbildung 10**: der Assistent zeigt die Spaltenliste aktualisiert für die `Employees_Select` gespeicherte Prozedur ([klicken Sie hier, um das Bild in voller Größe angezeigt](updating-the-tableadapter-to-use-joins-vb/_static/image26.png))


Schließen Sie den Assistenten, indem Sie auf ' Fertig stellen '. Bei der Rückkehr zu DataSet-Designer die `EmployeesDataTable` umfasst zwei zusätzliche Spalten: `ManagerFirstName` und `ManagerLastName`.


[![Die EmployeesDataTable enthält zwei neue Spalten](updating-the-tableadapter-to-use-joins-vb/_static/image28.png)](updating-the-tableadapter-to-use-joins-vb/_static/image27.png)

**Abbildung 11**: die `EmployeesDataTable` enthält zwei neue Spalten ([klicken Sie hier, um das Bild in voller Größe angezeigt](updating-the-tableadapter-to-use-joins-vb/_static/image29.png))


Um zu veranschaulichen, die die aktualisierte `Employees_Select` gespeicherte Prozedur im Endeffekt dasselbe ist, und dass die INSERT-, Update- und delete-Funktionen des TableAdapter sind zwar noch funktionsfähig, s, die eine Webseite erstellen, die ermöglicht Benutzern das Anzeigen und Löschen von Mitarbeitern ermöglichen. Bevor wir eine solche Seite erstellen, es jedoch zunächst eine neue Klasse in der Business Logic Layer für das Arbeiten mit Mitarbeitern aus erstellen die `NorthwindWithSprocs` DataSet. In Schritt 4, erstellen wir eine `EmployeesBLLWithSprocs` Klasse. In Schritt 5 verwenden wir diese Klasse von einer ASP.NET-Seite.

## <a name="step-4-implementing-the-business-logic-layer"></a>Schritt 4: Implementieren der Geschäftslogikebene

Erstellen Sie eine neue Klassendatei in der `~/App_Code/BLL` Ordner mit dem Namen `EmployeesBLLWithSprocs.vb`. Diese Klasse imitiert die Semantik des vorhandenen `EmployeesBLL` -Klasse, nur diese neue eine bietet weniger Methoden und verwendet die `NorthwindWithSprocs` DataSet (anstelle von der `Northwind` DataSet). Fügen Sie der `EmployeesBLLWithSprocs`-Klasse folgenden Code hinzu.


[!code-vb[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample6.vb)]

Die `EmployeesBLLWithSprocs` Klasse s `Adapter` Eigenschaft gibt eine Instanz von der `NorthwindWithSprocs` DataSet s `EmployeesTableAdapter`. Dies wird verwendet, von der Klasse s `GetEmployees` und `DeleteEmployee` Methoden. Die `GetEmployees` Methodenaufrufe der `EmployeesTableAdapter` s entspricht `GetEmployees` -Methode, die aufruft der `Employees_Select` gespeicherte Prozedur und füllt Sie seine Ergebnisse in ein `EmployeeDataTable`. Der `DeleteEmployee` Methodenaufrufe auf ähnliche Weise die `EmployeesTableAdapter` s `Delete` -Methode, die ruft der `Employees_Delete` gespeicherte Prozedur.

## <a name="step-5-working-with-the-data-in-the-presentation-layer"></a>Schritt 5: Arbeiten mit den Daten in der Darstellungsschicht

Mit der `EmployeesBLLWithSprocs` Klasse abgeschlossen ist, wir re kann jetzt zur Bearbeitung der Mitarbeiterdaten über eine ASP.NET-Seite. Öffnen der `JOINs.aspx` auf der Seite der `AdvancedDAL` Ordner, und ziehen Sie eine GridView aus der Toolbox in den Designer, Festlegen seiner `ID` Eigenschaft `Employees`. Binden Sie als Nächstes aus den GridView s smart Tag im Raster an ein neues ObjectDataSource-Steuerelement mit dem Namen `EmployeesDataSource`.

Konfigurieren der ObjectDataSource verwenden die `EmployeesBLLWithSprocs` Klasse und von den Registerkarten auswählen "und" DELETE "sicher, dass die `GetEmployees` und `DeleteEmployee` Methoden aus der Dropdown-Liste ausgewählt werden. Klicken Sie auf ' Fertig stellen ', um die Konfiguration der ObjectDataSource s abzuschließen.


[![Konfigurieren der ObjectDataSource zur Verwendung der EmployeesBLLWithSprocs-Klasse](updating-the-tableadapter-to-use-joins-vb/_static/image31.png)](updating-the-tableadapter-to-use-joins-vb/_static/image30.png)

**Abbildung 12**: Konfigurieren der ObjectDataSource verwenden die `EmployeesBLLWithSprocs` Klasse ([klicken Sie hier, um das Bild in voller Größe angezeigt](updating-the-tableadapter-to-use-joins-vb/_static/image32.png))


[![Haben Sie die Verwendung ObjectDataSource, die GetEmployees und DeleteEmployee-Methoden](updating-the-tableadapter-to-use-joins-vb/_static/image34.png)](updating-the-tableadapter-to-use-joins-vb/_static/image33.png)

**Abbildung 13**: haben Sie die Verwendung ObjectDataSource der `GetEmployees` und `DeleteEmployee` Methoden ([klicken Sie hier, um das Bild in voller Größe angezeigt](updating-the-tableadapter-to-use-joins-vb/_static/image35.png))


Visual Studio wird ein BoundField an die GridView hinzufügen, für jede der `EmployeesDataTable` s-Spalten. Entfernen Sie alle diese BoundFields mit Ausnahme von `Title`, `LastName`, `FirstName`, `ManagerFirstName`, und `ManagerLastName` , und benennen Sie die `HeaderText` Eigenschaften für die letzten vier BoundFields Nachname, Vorname, Manager s First Name, und Manager s Nachname, bzw.

Benutzern gestatten, Mitarbeiter, die auf dieser Seite löschen zwei Dinge erforderlich ist. Weisen Sie zunächst GridView Löschen von Funktionen bereitzustellen, durch Aktivieren der Option löschen aktivieren von smart Tag. Zweitens, ändern Sie das ObjectDataSource-s `OldValuesParameterFormatString` Eigenschaft aus dem Wert festgelegt, indem das ObjectDataSource-Assistenten (`original_{0}`) auf den Standardwert (`{0}`). Nachdem diese Änderungen vorgenommen wurden, sollte GridView und ObjectDataSource s deklarative Markup etwa wie folgt aussehen:


[!code-aspx[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample7.aspx)]

Testen Sie die Seite, indem Sie es über einen Browser besuchen. Wie in Abbildung 14 gezeigt, wird die Seite aufgelistet, jedes Mitarbeiters und seinen Manager s Namen (sofern vorhanden).


[![Der JOIN in der Employees_Select gespeicherte Prozedur gibt den Namen des Managers s](updating-the-tableadapter-to-use-joins-vb/_static/image37.png)](updating-the-tableadapter-to-use-joins-vb/_static/image36.png)

**Abbildung 14**: die `JOIN` in der `Employees_Select` gespeicherte Prozedur gibt die Manager-s-Name ([klicken Sie hier, um das Bild in voller Größe angezeigt](updating-the-tableadapter-to-use-joins-vb/_static/image38.png))


Löschen von Workflows, der bei der Ausführung endet mit Klicken auf die Schaltfläche "löschen" startet die `Employees_Delete` gespeicherte Prozedur. Allerdings die versuchte `DELETE` Anweisung in der gespeicherten Prozedur, die erzeugt einen Fehler aufgrund einer Verletzung der foreign Key-Einschränkung (siehe Abbildung 15). Insbesondere verfügt jeder Mitarbeiter eine oder mehrere Datensätze der `Orders` Tabelle, verursacht den Löschvorgang fehl.


[![Löschen eines Mitarbeiters, in der entsprechenden Aufträge Ergebnisse zu einer Verletzung der Foreign Key-Einschränkung hat](updating-the-tableadapter-to-use-joins-vb/_static/image40.png)](updating-the-tableadapter-to-use-joins-vb/_static/image39.png)

**Abbildung 15**: Löschen von einem Mitarbeiter, die Ergebnisse der entsprechenden Aufträge zu einer Verletzung der Foreign Key-Einschränkung aufweist ([klicken Sie hier, um das Bild in voller Größe angezeigt](updating-the-tableadapter-to-use-joins-vb/_static/image41.png))


Um ein Mitarbeiter zu ermöglichen Sie ein gelöschtes konnte:

- Update der foreign Key-Einschränkung, um Löschvorgänge Kaskadierend weiterzugeben,
- Löschen Sie manuell die Datensätze aus der `Orders` Tabelle für die Mitarbeiter, die Sie löschen möchten, oder
- Update der `Employees_Delete` gespeicherte Prozedur So löschen Sie zuerst die verknüpften Datensätze aus der `Orders` Tabelle vor dem Löschen der `Employees` Datensatz. Wir haben auch diese Technik in der [vorhandene gespeicherte Prozeduren verwenden, für die typisierte DataSet s TableAdapters](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) Lernprogramm.

Ich lassen Sie dieses als Übung für den Leser.

## <a name="summary"></a>Zusammenfassung

Bei der Arbeit mit relationalen Datenbanken wird häufig Abfragen zum Abrufen ihrer Daten aus mehreren verknüpften Tabellen. Korrelierte Unterabfragen und `JOIN` s bieten zwei verschiedene Techniken zum Zugreifen auf Daten aus verknüpften Tabellen in einer Abfrage. In vorherige Lernprogrammen, die wir am häufigsten vorgenommen korrelierte Unterabfragen verwenden, da es sich bei der TableAdapter Automatisches Generieren von kann nicht `INSERT`, `UPDATE`, und `DELETE` -Anweisungen für Abfragen, die im Zusammenhang mit `JOIN` s. Während diese Werte manuell bereitgestellt werden können, wenn mithilfe von Ad-hoc-SQL-Anweisungen werden alle Anpassungen überschrieben, wenn der TableAdapter-Konfigurations-Assistent abgeschlossen ist.

Glücklicherweise sind TableAdapters, die mithilfe von gespeicherten Prozeduren erstellt aus der gleichen unzulänglichen als solche, die mit Ad-hoc-SQL-Anweisungen erstellt nicht wesentlich beeinträchtigt. Daher ist es möglich, einen TableAdapter erstellen, deren Hauptabfrage verwendet, eine `JOIN` bei der Verwendung gespeicherter Prozeduren. In diesem Lernprogramm wurde erläutert, wie solche einen TableAdapter erstellen. Wir beginnen mit einem `JOIN`-weniger `SELECT` Abfragen für die Hauptabfrage des TableAdapter s, damit die entsprechenden Einfüge-, Update- und Delete gespeicherten Prozeduren automatisch erstellt werden würde. Mit dem TableAdapter s anfängliche Konfiguration abgeschlossen, es erweitert die `SelectCommand` gespeicherte Prozedur verwendet eine `JOIN` und der TableAdapter-Konfigurations-Assistenten die Aktualisierung erneut ausgeführt der `EmployeesDataTable` s-Spalten.

Erneutes Ausführen der TableAdapter-Konfigurations-Assistent automatisch aktualisiert, die `EmployeesDataTable` Spalten entsprechend der Datenfelder zurückgegebenes der `Employees_Select` gespeicherte Prozedur. Alternativ können Sie konnten wir diese Spalten manuell DataTable hinzugefügt haben. Wir untersuchen manuelle Hinzufügen von Spalten zu der "DataTable" in den nächsten Lernprogrammen.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurden Hilton Geisenow, David Suru und Teresa Murphy. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
> [Weiter](adding-additional-datatable-columns-vb.md)
