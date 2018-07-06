---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/updating-the-tableadapter-to-use-joins-cs
title: Aktualisieren für die Verwendung des TableAdapters Verknüpfungen (c#) | Microsoft-Dokumentation
author: rick-anderson
description: Bei der Arbeit mit einer Datenbank ist es üblich, das Abrufen von Daten, die auf mehrere Tabellen verteilt werden. Zum Abrufen von Daten aus zwei verschiedenen Tabellen können wir eine verwenden...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: 675531a7-cb54-4dd6-89ac-2636e4c285a5
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/updating-the-tableadapter-to-use-joins-cs
msc.type: authoredcontent
ms.openlocfilehash: 5ff63f1dc7c01f2be66dd99e1e00e2c2bb70058d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37391000"
---
<a name="updating-the-tableadapter-to-use-joins-c"></a>Aktualisieren für die Verwendung des TableAdapters Verknüpfungen (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_69_CS.zip) oder [PDF-Datei herunterladen](updating-the-tableadapter-to-use-joins-cs/_static/datatutorial69cs1.pdf)

> Bei der Arbeit mit einer Datenbank ist es üblich, das Abrufen von Daten, die auf mehrere Tabellen verteilt werden. Zum Abrufen von Daten aus zwei verschiedenen Tabellen können wir entweder einer korrelierten Unterabfrage oder ein JOIN-Vorgang verwenden. In diesem Tutorial Vergleichen wir korrelierte Unterabfragen und die JOIN-Syntax, bevor Sie an einen TableAdapter zu erstellen, die einen JOIN in der Hauptabfrage enthält.


## <a name="introduction"></a>Einführung

Mit relationalen Datenbanken werden die Daten, die Arbeiten mit Werten interessiert sind häufig über mehrere Tabellen verteilt. Z. B. bei der Anzeige von Produktinformationen wir wahrscheinlich jedes entsprechenden s Produktkategorie und s Lieferantennamen auflisten möchten. Die `Products` Tabelle `CategoryID` und `SupplierID` Werte, aber die tatsächlichen Namen der Kategorie und Lieferant befinden sich in der `Categories` und `Suppliers` Tabellen.

Zum Abrufen von Informationen aus einer anderen, abhängigen Tabelle können wir entweder *korrelierte Unterabfragen* oder `JOIN` *s*. Eine korrelierte Unterabfrage ist eine geschachtelte `SELECT` Abfrage, die Spalten in der äußeren Abfrage verweist. Z. B. in der [Erstellen einer Datenzugriffsschicht](../introduction/creating-a-data-access-layer-cs.md) Tutorial haben wir zwei abhängige Unterabfragen in verwendet die `ProductsTableAdapter` Hauptabfrage s, den Namen der Kategorie und Lieferanten für jedes Produkt zurückgegeben. Ein `JOIN` ist eine SQL-Konstrukt, das verknüpfte Zeilen aus zwei verschiedenen Tabellen zusammengeführt. Verwendet eine `JOIN` in die [Abfragen von Daten mit dem SqlDataSource-Steuerelement](../accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-cs.md) Lernprogramm aus, um Informationen zu Auftragskategorien zusammen mit der einzelnen Produkte angezeigt.

Der Grund, die wir von der Verwendung verbunden haben `JOIN` mit TableAdapters ist aufgrund von Einschränkungen bezüglich der TableAdapter-s-Assistent automatisch entsprechende generiert `INSERT`, `UPDATE`, und `DELETE` Anweisungen. Genauer gesagt, wenn die Hauptabfrage des TableAdapter s enthält `JOIN` s, der TableAdapter kann nicht automatisch erstellen der Ad-hoc-SQL-Anweisungen oder gespeicherte Prozeduren für die `InsertCommand`, `UpdateCommand`, und `DeleteCommand` Eigenschaften.

In diesem Tutorial, die wir kurz vergleichen und den Kontrast von abhängigen Unterabfragen und `JOIN` s vor, wie Sie einen TableAdapter erstellen, enthält untersuchen `JOIN` s in der Hauptabfrage.

## <a name="comparing-and-contrasting-correlated-subqueries-andjoin-s"></a>Vergleichen und gegenüberstellen von abhängigen Unterabfragen und`JOIN` s

Bedenken Sie, dass die `ProductsTableAdapter` erstellt, die im ersten Lernprogramm, in der `Northwind` DataSet verwendet korrelierte Unterabfragen rüstet s entsprechende Kategorie und Lieferant Produktnamen. Die `ProductsTableAdapter` s Hauptabfrage wird unten angezeigt.


[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample1.sql)]

Die beiden korrelierter Unterabfragen – `(SELECT CategoryName FROM Categories WHERE Categories.CategoryID = Products.CategoryID)` und `(SELECT CompanyName FROM Suppliers WHERE Suppliers.SupplierID = Products.SupplierID)` -sind `SELECT` Abfragen, die einen einzelnen Wert pro Produkt als eine zusätzliche Spalte in der äußeren zurückgeben `SELECT` Anweisung s-Spaltenliste.

Sie können auch eine `JOIN` können verwendet werden, um jede s Supplier "und" Kategorie Produktname zurückgegeben. Die folgende Abfrage gibt dieselbe Ausgabe wie die oben genannten, aber verwendet `JOIN` s anstelle von Unterabfragen:


[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample2.sql)]

Ein `JOIN` Datensätze aus einer Tabelle mit Datensätzen aus einer anderen Tabelle, die auf der Grundlage einiger Kriterien. In der obigen Abfrage wird z. B. die `LEFT JOIN Categories ON Categories.CategoryID = Products.CategoryID` weist SQL Server an die einzelnen merge Produktdatensatz, der Kategorie aufzeichnen, deren `CategoryID` Wert übereinstimmt, das Produkt s `CategoryID` Wert. Das zusammengeführte Ergebnis kann wir mit der entsprechenden Kategoriefelder für jedes Produkt arbeiten (wie z. B. `CategoryName`).

> [!NOTE]
> `JOIN` s werden häufig verwendet, bei der Abfrage von Daten aus relationalen Datenbanken. Wenn Sie neu ist die `JOIN` Syntax oder zur Syntax ein wenig auffrischen müssen, empfehle ich d der [SQL Join Tutorial](http://www.w3schools.com/sql/sql_join.asp) an [W3 Schulen](http://www.w3schools.com/). Auch interessante sind die [ `JOIN` Grundlagen](https://msdn.microsoft.com/library/ms191517.aspx) und [Unterabfrage Grundlagen](https://msdn.microsoft.com/library/ms189575.aspx) Teile der [SQL-Onlinedokumentation](https://msdn.microsoft.com/library/ms130214.aspx).


Da `JOIN` s und korrelierte Unterabfragen können beide verwendet werden zum Abrufen von verknüpfter Daten aus anderen Tabellen, bleiben viele Entwickler ihre Mal "Kopf" gestreift und fragen sich, welchen Ansatz verwenden. Alle von der SQL-Experten ich ungefähr die gleiche gaben an, dass gesprochen haben, dass die It t spielt leistungsfähig wie SQL Server ungefähr identische Ausführungspläne erstellt wird. Ihren Rat, besteht darin, das Verfahren zu verwenden, dem Sie und Ihr Team mit am besten vertraut sind. Es benötigt, beachten Sie, dass diese Experten nach diesen Rat imparting sofort ihre Einstellung express `JOIN` s über korrelierte Unterabfragen.

Wenn Sie eine Datenzugriffsschicht, die mit typisierten DataSets zu erstellen, funktionieren die Tools besser bei Verwendung von Unterabfragen. Insbesondere der TableAdapter-s-Assistenten nicht generiert automatisch entsprechende `INSERT`, `UPDATE`, und `DELETE` Anweisungen, wenn die Hauptabfrage enthält `JOIN` s, sondern generiert automatisch diese Anweisungen, wenn die Korrelation Unterabfragen werden verwendet.

Um diese Unzulänglichkeit zu untersuchen, erstellen Sie eine temporäre typisierte DataSet in den `~/App_Code/DAL` Ordner. Während der TableAdapter-Konfigurations-Assistenten die Option mithilfe von Ad-hoc-SQL-Anweisungen, und geben den folgenden `SELECT` Abfrage (siehe Abbildung 1):


[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample3.sql)]


[![Geben Sie eine Main-Abfrage, die JOINs enthält.](updating-the-tableadapter-to-use-joins-cs/_static/image2.png)](updating-the-tableadapter-to-use-joins-cs/_static/image1.png)

**Abbildung 1**: Geben Sie eine Main-Abfrage, Contains `JOIN` s ([klicken Sie, um das Bild in voller Größe anzeigen](updating-the-tableadapter-to-use-joins-cs/_static/image3.png))


In der Standardeinstellung des TableAdapter erstellt automatisch `INSERT`, `UPDATE`, und `DELETE` Anweisungen auf Grundlage der Hauptabfrage. Wenn Sie auf die Schaltfläche "Erweiterte" klicken, sehen Sie, dass dieses Feature aktiviert ist. Trotz dieser Einstellung wird der TableAdapter ist nicht möglich, erstellen Sie die `INSERT`, `UPDATE`, und `DELETE` Anweisungen, da die Hauptabfrage enthält eine `JOIN`.


![Geben Sie eine Main-Abfrage, die JOINs enthält.](updating-the-tableadapter-to-use-joins-cs/_static/image4.png)

**Abbildung 2**: Geben Sie eine Main-Abfrage, die enthält `JOIN` s


Klicken Sie auf "Fertig stellen", um den Assistenten abzuschließen. An diesem Punkt die DataSet-s-Designer enthält einen einzelnen TableAdapter mit einer "DataTable" mit Spalten für jede der die zurückgegebenen Felder in der `SELECT` Abfrage s Spaltenliste. Dies schließt die `CategoryName` und `SupplierName`, wie in Abbildung 3 dargestellt.


![Die Datentabelle enthält eine Spalte für jedes Feld in der Liste der Spalten zurückgegeben](updating-the-tableadapter-to-use-joins-cs/_static/image5.png)

**Abbildung 3**: DataTable enthält eine Spalte für jedes Feld in der Liste der Spalten zurückgegeben.


Während die DataTable, die entsprechenden Spalten verfügt, wird der TableAdapter verfügt nicht über Werte für die `InsertCommand`, `UpdateCommand`, und `DeleteCommand` Eigenschaften. Um dies zu bestätigen, klicken Sie auf den TableAdapter im Designer, und fahren Sie mit dem Fenster "Eigenschaften". Es wird angezeigt, die die `InsertCommand`, `UpdateCommand`, und `DeleteCommand` Eigenschaften auf (keine) festgelegt werden.


[![Die InsertCommand UpdateCommand und DeleteCommand-Eigenschaften werden auf (keine) festgelegt.](updating-the-tableadapter-to-use-joins-cs/_static/image7.png)](updating-the-tableadapter-to-use-joins-cs/_static/image6.png)

**Abbildung 4**: die `InsertCommand`, `UpdateCommand`, und `DeleteCommand` Eigenschaften auf (keine) festgelegt werden ([klicken Sie, um das Bild in voller Größe anzeigen](updating-the-tableadapter-to-use-joins-cs/_static/image8.png))


Zur Umgehung dieser Schwierigkeit können wir manuell die SQL-Anweisungen und Parameter zum Angeben der `InsertCommand`, `UpdateCommand`, und `DeleteCommand` Eigenschaften über das Fenster "Eigenschaften". Alternativ kann zunächst durch Konfigurieren der TableAdapter der Hauptabfrage s, um *nicht* enthalten `JOIN` s. Dadurch wird die `INSERT`, `UPDATE`, und `DELETE` Anweisungen, die automatisch für uns generiert werden. Nach Abschluss des Assistenten an, wir könnten aktualisieren Sie anschließend manuell die TableAdapter `SelectCommand` im Eigenschaftenfenster, sodass die It enthält die `JOIN` Syntax.

Führendes Prüfer für dieses Tutorial wurden Hilton Geisenow, David Suru und Teresa Murphy. Das bedeutet, dass alle wir später vorgenommenen Anpassungen verloren gehen würden, wenn wir mit der rechten auf den TableAdapter Maustaste, wählen im Kontextmenü der konfigurieren und den Assistenten erneut durchgeführt.

Die Fehleranfälligkeit die automatisch generierten TableAdapter `INSERT`, `UPDATE`, und `DELETE` Anweisungen lautet Glücklicherweise auf Ad-hoc-SQL-Anweisungen beschränkt. Wenn gespeicherte Prozeduren in der TableAdapter verwendet wird, können Sie Anpassen der `SelectCommand`, `InsertCommand`, `UpdateCommand`, oder `DeleteCommand` gespeicherte Prozeduren, und führen Sie den TableAdapter-Konfigurations-Assistenten erneut aus, ohne zu befürchten, dass die gespeicherten Prozeduren werden müssen geändert.

In den nächsten Schritten erstellen wir einen TableAdapter, die Anfangs eine hauptspeicherpool für Abfragen, die alle lässt verwendet `JOIN` s, damit die entsprechenden einfügen, aktualisieren und Löschen gespeicherter Prozeduren werden automatisch generiert werden. Aktualisieren wir dann die `SelectCommand` deshalb verwendet, die eine `JOIN` , zusätzliche Spalten aus verknüpften Tabellen zurückgibt. Zum Schluss wir erstellen eine entsprechende Business Logic Layer-Klasse und veranschaulicht die Verwendung des TableAdapters auf einer ASP.NET-Webseite.

## <a name="step-1-creating-the-tableadapter-using-a-simplified-main-query"></a>Schritt 1: Erstellen den TableAdapter mithilfe einer vereinfachten Hauptspeicherpool für Abfragen

In diesem Tutorial fügen wir ein TableAdapter und stark typisierte DataTable für die `Employees` -Tabelle in der `NorthwindWithSprocs` DataSet. Die `Employees` Tabelle enthält eine `ReportsTo` angegebenen Feld der `EmployeeID` des Mitarbeiters s-Managers. Beispielsweise Mitarbeiter Anne Dodsworth verfügt über eine `ReportTo` Wert von 5, der die `EmployeeID` von Steven Buchanan. Daher meldet Anne Steven, ihr Manager. Zusammen mit jeder Mitarbeiter s reporting `ReportsTo` Wert, wir sollten auch den Namen der Manager abzurufen. Dies geschieht mithilfe einer `JOIN`. Während mit einer `JOIN` beim anfänglichen Erstellen des TableAdapters den Assistenten wird automatisch generiert. das entsprechende einfügen hingewiesen, aktualisieren und Löschen von Funktionen. Aus diesem Grund werden zunächst durch Erstellen eines TableAdapter, deren Hauptabfrage keine enthält `JOIN` s. Klicken Sie dann in Schritt2 haben wir aktualisieren die Hauptabfrage gespeicherte Prozedur zum Abrufen der Managername s über eine `JOIN`.

Öffnen Sie zunächst die `NorthwindWithSprocs` DataSet in den `~/App_Code/DAL` Ordner. Mit der rechten Maustaste auf den Designer, wählen Sie im Kontextmenü die Option hinzufügen, und wählen Sie das Menüelement TableAdapter. Hierdurch wird der TableAdapter-Konfigurations-Assistenten. Wie Abbildung 5 zeigt, müssen Sie den Assistenten neue gespeicherte Prozeduren erstellen, und klicken Sie auf Weiter. Für eine Auffrischung zum Erstellen neuer Prozeduren über den TableAdapter-s-Assistenten gespeicherter finden Sie in der [Erstellen neuer gespeicherter Prozeduren für die typisierte DataSet-s TableAdapters](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) Tutorial.


[![Wählen Sie die neuen gespeicherten Prozeduren Option für das Erstellen](updating-the-tableadapter-to-use-joins-cs/_static/image10.png)](updating-the-tableadapter-to-use-joins-cs/_static/image9.png)

**Abbildung 5**: Wählen Sie erstellen neue gespeicherte Procedures (Option) ([klicken Sie, um das Bild in voller Größe anzeigen](updating-the-tableadapter-to-use-joins-cs/_static/image11.png))


Verwenden Sie die folgenden `SELECT` -Anweisung für die Hauptabfrage des TableAdapter s:


[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample4.sql)]

Da diese Abfrage nicht enthält, führt `JOIN` s, der TableAdapter-Assistenten erstellt automatisch gespeicherte Prozeduren mit den entsprechenden `INSERT`, `UPDATE`, und `DELETE` Anweisungen als auch eine gespeicherte Prozedur für die Ausführung der Hauptseite Abfrage.

Die folgende Schritte kann wir die TableAdapter s, die gespeicherten Prozeduren nennen. Verwenden Sie die Namen `Employees_Select`, `Employees_Insert`, `Employees_Update`, und `Employees_Delete`, wie in Abbildung 6 dargestellt.


[![Name der TableAdapter s gespeicherten Prozeduren](updating-the-tableadapter-to-use-joins-cs/_static/image13.png)](updating-the-tableadapter-to-use-joins-cs/_static/image12.png)

**Abbildung 6**: Benennen der TableAdapter s gespeicherte Prozeduren ([klicken Sie, um das Bild in voller Größe anzeigen](updating-the-tableadapter-to-use-joins-cs/_static/image14.png))


Der letzte Schritt verlangt, um den TableAdapter-s-Methoden zu nennen. Verwendung `Fill` und `GetEmployees` den Namen der Methode. Außerdem werden Sie sicher, dass die Create-Methoden, um Updates direkt an das Kontrollkästchen Datenbank (GenerateDBDirectMethods) zu senden.


[![Name der TableAdapter-s-Methoden-Füllung und GetEmployees](updating-the-tableadapter-to-use-joins-cs/_static/image16.png)](updating-the-tableadapter-to-use-joins-cs/_static/image15.png)

**Abbildung 7**: Benennen Sie die TableAdapter-Methoden `Fill` und `GetEmployees` ([klicken Sie, um das Bild in voller Größe anzeigen](updating-the-tableadapter-to-use-joins-cs/_static/image17.png))


Können Sie nach Abschluss des Assistenten, überprüfen Sie die gespeicherten Prozeduren in der Datenbank. Daraufhin sollte die vier neue: `Employees_Select`, `Employees_Insert`, `Employees_Update`, und `Employees_Delete`. Überprüfen Sie anschließend die `EmployeesDataTable` und `EmployeesTableAdapter` gerade erstellt haben. Die DataTable enthält eine Spalte für jedes Feld, das von der Hauptabfrage zurückgegeben. Klicken Sie auf den TableAdapter, und fahren Sie mit dem Fenster "Eigenschaften". Es wird angezeigt, die die `InsertCommand`, `UpdateCommand`, und `DeleteCommand` Eigenschaften ordnungsgemäß konfiguriert sind, um die entsprechenden gespeicherten Prozeduren aufrufen.


[![Der TableAdapter enthält INSERT-, Update- und Löschen von Funktionen](updating-the-tableadapter-to-use-joins-cs/_static/image19.png)](updating-the-tableadapter-to-use-joins-cs/_static/image18.png)

**Abbildung 8**: der TableAdapter Insert enthält, Update- und Delete-Funktionen ([klicken Sie, um das Bild in voller Größe anzeigen](updating-the-tableadapter-to-use-joins-cs/_static/image20.png))


Nach dem Einfügen, aktualisieren und Löschen von gespeicherten Prozeduren, die automatisch erstellt und die `InsertCommand`, `UpdateCommand`, und `DeleteCommand` Eigenschaften ordnungsgemäß konfiguriert, wir können zum Anpassen der `SelectCommand` s gespeicherte Prozedur, um zusätzliche zurückzugeben. Informationen zu jedem Mitarbeiter s-Manager. Insbesondere müssen wir aktualisieren die `Employees_Select` gespeicherte Prozedur verwendet eine `JOIN` und zurückgeben den Manager s `FirstName` und `LastName` Werte. Nachdem die gespeicherte Prozedur aktualisiert wurde, müssen wir die DataTable zu aktualisieren, sodass sie diese zusätzlichen Spalten enthält. Wir werden diese beiden Aufgaben in den Schritten 2 und 3 in Angriff nehmen.

## <a name="step-2-customizing-the-stored-procedure-to-include-ajoin"></a>Schritt 2: Anpassen der gespeicherten Prozedur enthalten eine`JOIN`

Navigieren zum Server-Explorer, Drilldown in den Ordner für Northwind s-Datenbank gespeicherte Prozeduren, und öffnen Sie zunächst die `Employees_Select` gespeicherte Prozedur. Wenn Sie diese gespeicherte Prozedur nicht angezeigt werden, wird mit der rechten Maustaste auf den Ordner gespeicherte Prozeduren, und wählen Sie die Aktualisierung. Aktualisieren Sie die gespeicherte Prozedur, sodass er verwendet eine `LEFT JOIN` den s-Manager zuerst zurückgegeben und Nachname:


[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample5.sql)]

Nach der Aktualisierung der `SELECT` -Anweisung, die Änderungen auf das Menü "Datei" und Auswählen von Speichern `Employees_Select`. Sie können Alternativ klicken Sie auf das Symbol "Speichern" in der Symbolleiste oder drücken Sie STRG + S. Nach dem Speichern Ihrer Änderungen an, mit der Maustaste auf die `Employees_Select` gespeicherte Prozedur im Server-Explorer und wählen Sie ausführen. Dies wird die gespeicherte Prozedur auszuführen und zeigen die Ergebnisse im Ausgabefenster angezeigt (siehe Abbildung 9).


[![Die Ergebnisse der gespeicherten Prozeduren werden im Ausgabefenster angezeigt.](updating-the-tableadapter-to-use-joins-cs/_static/image22.png)](updating-the-tableadapter-to-use-joins-cs/_static/image21.png)

**Abbildung 9**: die Ergebnisse für gespeicherte Prozeduren werden im Ausgabefenster angezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](updating-the-tableadapter-to-use-joins-cs/_static/image23.png))


## <a name="step-3-updating-the-datatable-s-columns"></a>Schritt 3: Aktualisieren der DataTable-s-Spalten

An diesem Punkt die `Employees_Select` gespeicherte Prozedur gibt `ManagerFirstName` und `ManagerLastName` Werte, aber die `EmployeesDataTable` fehlt für diese Spalten. Diese fehlenden Spalten können der Datentabelle in einer von zwei Arten hinzugefügt werden:

- **Manuell** – mit der rechten Maustaste auf die DataTable im DataSet-Designer und wählen Sie aus dem Menü "hinzufügen". Sie können dann den Namen der Spalte und seine Eigenschaften entsprechend festlegen.
- **Automatisch** -der TableAdapter-Konfigurations-Assistent aktualisiert die DataTable-s-Spalten entsprechend der von zurückgegebenen Felder der `SelectCommand` gespeicherte Prozedur. Verwendung von Ad-hoc-SQL-Anweisungen des Assistenten entfernt auch die `InsertCommand`, `UpdateCommand`, und `DeleteCommand` Eigenschaften, die seit der `SelectCommand` enthält jetzt eine `JOIN`. Aber wenn gespeicherte Prozeduren zu verwenden, bleiben diese Befehlseigenschaften erhalten.

Wir haben manuelle Hinzufügen von DataTable-Spalten in der vorherigen Lernprogramme, darunter auch untersucht [Master/Detail, verwenden eine Liste mit Aufzählungszeichen Liste der Masterdatensätze mit einem DataList-Steuerelement Details](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) und [Hochladen von Dateien](../working-with-binary-files/uploading-files-cs.md), und wir setzen uns Dieser Prozess erneut im Detail in unserem nächsten Tutorial erläutert. In diesem Tutorial können Sie jedoch s, die die automatische Methode über den TableAdapter-Konfigurations-Assistenten zu verwenden.

Starten, indem Sie mit der rechten Maustaste auf die `EmployeesTableAdapter` , und wählen im Kontextmenü der konfigurieren. Dadurch wird der TableAdapter-Konfigurations-Assistent, der die gespeicherten Prozeduren zum auswählen, einfügen, aktualisieren und löschen, sowie deren Rückgabewerte und Parameter (sofern vorhanden) aufgeführt sind. Abbildung 10 zeigt diesen Assistenten. Hier sehen wir, dass die `Employees_Select` gibt die gespeicherte Prozedur die `ManagerFirstName` und `ManagerLastName` Felder.


[![Der Assistent zeigt der Liste der aktualisierten Spalten für die Employees_Select gespeicherten Prozedur](updating-the-tableadapter-to-use-joins-cs/_static/image25.png)](updating-the-tableadapter-to-use-joins-cs/_static/image24.png)

**Abbildung 10**: der Assistent zeigt die Spaltenliste aktualisiert, für die `Employees_Select` Stored Procedure ([klicken Sie, um das Bild in voller Größe anzeigen](updating-the-tableadapter-to-use-joins-cs/_static/image26.png))


Schließen Sie den Assistenten, indem Sie auf "Fertig stellen". Bei der Rückkehr zum DataSet-Designer die `EmployeesDataTable` umfasst zwei zusätzliche Spalten: `ManagerFirstName` und `ManagerLastName`.


[![Die EmployeesDataTable enthält zwei neue Spalten](updating-the-tableadapter-to-use-joins-cs/_static/image28.png)](updating-the-tableadapter-to-use-joins-cs/_static/image27.png)

**Abbildung 11**: die `EmployeesDataTable` enthält zwei neue Spalten ([klicken Sie, um das Bild in voller Größe anzeigen](updating-the-tableadapter-to-use-joins-cs/_static/image29.png))


Um zu zeigen, dass die aktualisierte `Employees_Select` gespeicherte Prozedur aktiviert ist und einfügen, aktualisieren und löschen die Funktionen des TableAdapter sind zwar noch funktionsfähig, s, die eine Webseite erstellen, mit dem Benutzer anzeigen und Löschen von Mitarbeitern zu ermöglichen. Bevor wir eine solche Seite erstellen, allerdings wir müssen zunächst eine neue Klasse in der Geschäftslogikebene für die Arbeit mit Mitarbeiter erstellen die `NorthwindWithSprocs` DataSet. In Schritt 4 erstellen wir eine `EmployeesBLLWithSprocs` Klasse. In Schritt 5 verwenden wir diese Klasse von einer ASP.NET-Seite.

## <a name="step-4-implementing-the-business-logic-layer"></a>Schritt 4: Implementieren den Geschäftslogikebene

Erstellen Sie eine neue Klassendatei in der `~/App_Code/BLL` Ordner mit dem Namen `EmployeesBLLWithSprocs.cs`. Diese Klasse imitiert die Semantik des vorhandenen `EmployeesBLL` -Klasse, nur diese neue eine weniger Methoden bietet und verwendet die `NorthwindWithSprocs` DataSet (anstelle von der `Northwind` DataSet). Fügen Sie der `EmployeesBLLWithSprocs`-Klasse folgenden Code hinzu.


[!code-csharp[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample6.cs)]

Die `EmployeesBLLWithSprocs` Klasse s `Adapter` Eigenschaft gibt eine Instanz des der `NorthwindWithSprocs` DataSet s `EmployeesTableAdapter`. Hiermit wird von der Klasse s `GetEmployees` und `DeleteEmployee` Methoden. Der `GetEmployees` Methodenaufrufe der `EmployeesTableAdapter` s entspricht `GetEmployees` -Methode, die ruft der `Employees_Select` von gespeicherten Prozeduren und füllt die Ergebnisse in ein `EmployeeDataTable`. Die `DeleteEmployee` Methodenaufrufe auf ähnliche Weise die `EmployeesTableAdapter` s `Delete` -Methode, die Ruft die `Employees_Delete` gespeicherte Prozedur.

## <a name="step-5-working-with-the-data-in-the-presentation-layer"></a>Schritt 5: Arbeiten mit Daten in der Darstellungsschicht

Mit der `EmployeesBLLWithSprocs` Klasse nun vollständig, es erneut bereit, die zum Arbeiten mit Mitarbeiterdaten über eine ASP.NET-Seite. Öffnen der `JOINs.aspx` auf der Seite die `AdvancedDAL` Ordner, und ziehen Sie einer GridView-Ansicht aus der Toolbox auf den Designer, Festlegen der `ID` Eigenschaft `Employees`. Im nächsten Schritt aus GridView s Smarttags, um ein neues ObjectDataSource-Steuerelement, das mit dem Namen im Raster binden `EmployeesDataSource`.

Konfigurieren Sie mit dem ObjectDataSource-Steuerelement die `EmployeesBLLWithSprocs` Klasse und aus den Registerkarten auswählen und löschen, stellen sicher, dass die `GetEmployees` und `DeleteEmployee` Methoden werden aus den Dropdownlisten ausgewählt. Klicken Sie auf "Fertig stellen", um die "ObjectDataSource"-s-Konfiguration abzuschließen.


[![Konfigurieren von dem ObjectDataSource-Steuerelement zur Verwendung der EmployeesBLLWithSprocs-Klasse](updating-the-tableadapter-to-use-joins-cs/_static/image31.png)](updating-the-tableadapter-to-use-joins-cs/_static/image30.png)

**Abbildung 12**: Konfigurieren von dem ObjectDataSource-Steuerelement verwenden die `EmployeesBLLWithSprocs` Klasse ([klicken Sie, um das Bild in voller Größe anzeigen](updating-the-tableadapter-to-use-joins-cs/_static/image32.png))


[![Haben Sie die Verwendung von "ObjectDataSource" aus, die GetEmployees und DeleteEmployee-Methoden](updating-the-tableadapter-to-use-joins-cs/_static/image34.png)](updating-the-tableadapter-to-use-joins-cs/_static/image33.png)

**Abbildung 13**: haben Sie die Verwendung von "ObjectDataSource" die `GetEmployees` und `DeleteEmployee` Methoden ([klicken Sie, um das Bild in voller Größe anzeigen](updating-the-tableadapter-to-use-joins-cs/_static/image35.png))


Visual Studio wird eine BoundField an die GridView hinzufügen, für die einzelnen der `EmployeesDataTable` s-Spalten. Entfernen Sie alle diese BoundFields mit Ausnahme von `Title`, `LastName`, `FirstName`, `ManagerFirstName`, und `ManagerLastName` , und benennen Sie die `HeaderText` Eigenschaften für die letzten vier BoundFields Nachname, Vorname, Manager s Vornamen ein, und Manager s zuletzt bzw. den Namen.

Benutzern gestatten, Mitarbeiter, die auf dieser Seite löschen wir zwei Dinge tun müssen. Weisen Sie zunächst die GridView, um das Löschen von Funktionen bereitzustellen, durch Aktivieren der Option löschen aktivieren, aus der Smarttag. Zweitens: ändern die "ObjectDataSource"-s `OldValuesParameterFormatString` Eigenschaft aus dem Wert festgelegt, indem der ObjectDataSource-Steuerelement-Assistent (`original_{0}`) auf den Standardwert (`{0}`). Nach diesen Änderungen sollte GridView und "ObjectDataSource" s deklarative Markup etwa wie folgt aussehen:


[!code-aspx[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample7.aspx)]

Testen Sie Sie auf der Seite, indem Sie es über einen Browser besuchen. Wie in Abbildung 14 gezeigt, listet die Seite jedes Mitarbeiters und seinen Manager s Namen (sofern vorhanden).


[![Der JOIN in der Employees_Select gespeicherte Prozedur gibt den Namen der Manager-s](updating-the-tableadapter-to-use-joins-cs/_static/image37.png)](updating-the-tableadapter-to-use-joins-cs/_static/image36.png)

**Abbildung 14**: die `JOIN` in die `Employees_Select` gespeicherte Prozedur gibt den Name-s-Manager ([klicken Sie, um das Bild in voller Größe anzeigen](updating-the-tableadapter-to-use-joins-cs/_static/image38.png))


Klicken Sie auf die Schaltfläche "löschen" startet das Löschen von Workflows, der bei der Ausführung endet mit dem `Employees_Delete` gespeicherte Prozedur. Allerdings die versuchte `DELETE` Anweisung in der gespeicherten Prozedur, die erzeugt einen Fehler aufgrund einer Verletzung der foreign Key-Einschränkung (siehe Abbildung 15). Insbesondere verfügt jeder Mitarbeiter einen oder mehrere Datensätze der `Orders` Tabelle, verursacht den Löschvorgang fehl.


[![Löschen eines Mitarbeiters, die entsprechenden Ergebnisse der Aufträge in der Verletzung einer Foreign Key-Einschränkung aufweist](updating-the-tableadapter-to-use-joins-cs/_static/image40.png)](updating-the-tableadapter-to-use-joins-cs/_static/image39.png)

**Abbildung 15**: Löschen eines Mitarbeiters, die entsprechenden Ergebnisse der Aufträge in der Verletzung einer Foreign Key-Einschränkung aufweist ([klicken Sie, um das Bild in voller Größe anzeigen](updating-the-tableadapter-to-use-joins-cs/_static/image41.png))


Zu einem Mitarbeiter werden gelöscht, Sie könnten:

- Update der foreign Key-Einschränkung Kaskadieren von Löschvorgängen
- Löschen Sie manuell die Datensätze aus der `Orders` Tabelle für die Mitarbeiter, die Sie löschen möchten, oder
- Update der `Employees_Delete` gespeicherten Prozedur erst löschen, die verknüpften Datensätze aus der `Orders` Tabelle vor dem Löschen der `Employees` Datensatz. Diese Technik erläutert die [vorhandene gespeicherte Prozeduren verwenden, für die typisierte DataSet-s TableAdapters](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) Tutorial.

Ich lassen Sie dieses als Übung für den Leser.

## <a name="summary"></a>Zusammenfassung

Bei der Arbeit mit relationalen Datenbanken verknüpften häufig für Abfragen zum Abrufen ihrer Daten aus mehreren Tabellen. Korrelierte Unterabfragen und `JOIN` s bieten zwei verschiedene Verfahren für den Zugriff auf Daten aus verknüpften Tabellen in einer Abfrage. In vorherigen Tutorials, die wir, am häufigsten vorgenommen von korrelierten Unterabfragen verwenden, da es sich bei der TableAdapter automatisch generieren kann nicht `INSERT`, `UPDATE`, und `DELETE` -Anweisungen zum Abfragen, die im Zusammenhang mit `JOIN` s. Während diese Werte manuell bereitgestellt werden können, wenn Sie Ad-hoc-SQL-Anweisungen verwenden werden alle Anpassungen nach Abschluss des TableAdapter-Konfigurations-Assistenten wird überschrieben.

Glücklicherweise sind TableAdapters, die erstellt wird, mithilfe von gespeicherten Prozeduren aus der gleichen Fehleranfälligkeit, wie mithilfe von Ad-hoc-SQL-Anweisungen erstellt nicht beeinträchtigt werden. Daher ist es möglich, einen TableAdapter zu erstellen, deren Hauptabfrage verwendet, eine `JOIN` beim Verwenden gespeicherter Prozeduren. In diesem Tutorial wurde erläutert, wie solche einen TableAdapter zu erstellen. Wir haben eine `JOIN`-weniger `SELECT` Abfrage für die Hauptabfrage des TableAdapter s, sodass die entsprechenden INSERT-, Update- und Löschen gespeicherter Prozeduren automatisch erstellt werden würde. Erweitert, mit dem TableAdapter anfängliche Konfiguration abgeschlossen, wir die `SelectCommand` gespeicherte Prozedur verwendet eine `JOIN` und erneuter Ausführung der TableAdapter-Konfigurations-Assistent beim Aktualisieren der `EmployeesDataTable` s-Spalten.

Erneutes Ausführen der TableAdapter-Konfigurations-Assistent automatisch aktualisiert, die `EmployeesDataTable` Spalten entsprechend der Datenfelder, die vom der `Employees_Select` gespeicherte Prozedur. Sie können auch können wir diese Spalten manuell zur DataTable hinzugefügt werden. Manuelles Hinzufügen von Spalten wird im nächsten Tutorial der Datentabelle untersucht.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial wurden Hilton Geisenow, David Suru und Teresa Murphy. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)
> [Weiter](adding-additional-datatable-columns-cs.md)
