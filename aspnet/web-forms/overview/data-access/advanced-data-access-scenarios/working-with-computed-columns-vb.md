---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/working-with-computed-columns-vb
title: Arbeiten mit berechneten Spalten (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Wenn Sie eine Datenbanktabelle zu erstellen, Microsoft SQL Server ermöglicht es Ihnen, eine berechnete Spalte zu definieren, dessen Wert aus einem Ausdruck berechnet wird, in der Regel verw...
ms.author: aspnetcontent
ms.date: 08/03/2007
ms.assetid: 5811b8ff-ed56-40fc-9397-6b69ae09a8f6
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/working-with-computed-columns-vb
msc.type: authoredcontent
ms.openlocfilehash: 7e489640c9075b3443725ddd776ca08fd5569232
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830988"
---
<a name="working-with-computed-columns-vb"></a>Arbeiten mit berechneten Spalten (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_71_VB.zip) oder [PDF-Datei herunterladen](working-with-computed-columns-vb/_static/datatutorial71vb1.pdf)

> Wenn Sie eine Datenbanktabelle zu erstellen, können Microsoft SQL Server Sie eine berechnete Spalte definieren, deren Wert aus einem Ausdruck berechnet wird, die in der Regel andere Werte in denselben Datenbank-Datensatz verweist. Solche Werte sind in der Datenbank, die besondere Überlegungen erforderlich ist, bei der Arbeit mit TableAdapters schreibgeschützt. In diesem Tutorial erfahren wir, wie auf die berechneten Spalten darstellen Herausforderungen zu meistern.


## <a name="introduction"></a>Einführung

Microsoft SQL Server ermöglicht  *[berechnete Spalten](https://msdn.microsoft.com/library/ms191250.aspx)*, welche sind die Spalten, die aus einem Ausdruck, der die Werte in der Regel von anderen Spalten in derselben Tabelle verweist, dessen Werte berechnet werden. Beispielsweise gibt es möglicherweise für eine zeitüberwachung Datenmodell eine Tabelle namens `ServiceLog` mit Spalten, einschließlich `ServicePerformed`, `EmployeeID`, `Rate`, und `Duration`, u. a. Während den fällige Betrag pro Dienst Element (wird die Rate, multipliziert mit der Dauer) kann über eine Webseite oder andere programmgesteuerte Schnittstelle berechnet werden, es kann praktisch sein, eine Spalte in enthalten die `ServiceLog` Tabelle mit dem Namen `AmountDue` , die dies gemeldet Informationen. Diese Spalte kann als normale Spalte erstellt werden, aber es jederzeit aktualisiert werden, müssen die `Rate` oder `Duration` Spaltenwerte geändert. Ein besserer Ansatz wäre, stellen die `AmountDue` Spalte eine berechnete Spalte mit dem Ausdruck `Rate * Duration`. Auf diese Weise würde dazu führen, dass SQL Server für die automatische Berechnung der `AmountDue` Spaltenwert immer in einer Abfrage auf sie verwiesen wurde.

Da ein berechnete Spalte s-Wert durch einen Ausdruck bestimmt wird, diese Spalten sind schreibgeschützt und aus diesem Grund können keine Werte zugewiesen haben auf diese Dateien im `INSERT` oder `UPDATE` Anweisungen. Wenn Sie berechnete Spalten Teil der Hauptabfrage für einen TableAdapter handelt, die Ad-hoc-SQL-Anweisungen verwendet, sie sind jedoch automatisch enthalten, in den automatisch generierten `INSERT` und `UPDATE` Anweisungen. Daher die TableAdapter `INSERT` und `UPDATE` Abfragen und `InsertCommand` und `UpdateCommand` Eigenschaften aktualisiert werden müssen, um Verweise auf alle berechneten Spalten entfernen.

Eine Schwierigkeit der Verwendung von berechneten Spalten mit einem TableAdapter, die Ad-hoc-SQL-Anweisungen verwendet wird, die die TableAdapter `INSERT` und `UPDATE` Abfragen werden automatisch neu generiert jedes Mal der TableAdapter-Konfigurations-Assistenten abgeschlossen ist. Aus diesem Grund manuell die berechneten Spalten daraus die `INSERT` und `UPDATE` Abfragen werden erneut angezeigt, wenn der Assistent erneut ausgeführt wird. Obwohl die TableAdapters, die gespeicherte Prozeduren verwenden keine Nachteile für Ihr diese Fehleranfälligkeit leiden, besitzen ihre eigenen Eigenarten, die wir in Schritt 3 gerecht werden.

In diesem Tutorial fügen wir eine berechnete Spalte, die `Suppliers` -Tabelle in der Northwind-Datenbank und erstellen Sie einen entsprechenden TableAdapter zum Arbeiten mit dieser Tabelle und die berechnete Spalte. Wir haben unsere TableAdapter gespeicherte Prozeduren anstelle von Ad-hoc-SQL-Anweisungen zu verwenden, damit unsere Anpassungen sind t verloren, wenn der TableAdapter-Konfigurations-Assistent verwendet wird.

Lassen Sie s beginnen!

## <a name="step-1-adding-a-computed-column-to-thesupplierstable"></a>Schritt 1: Hinzufügen einer berechneten Spalte, die`Suppliers`Tabelle

Die Northwind-Datenbank muss keine berechneten Spalten, daher wir ein selbst hinzufügen müssen. Für dieses Tutorial s, die eine berechnete Spalte hinzufügen lassen die `Suppliers` Tabelle mit dem Namen `FullContactName` , der-s-Kontaktname, Titel und das Unternehmen, die sie für funktionieren zurückgibt, das folgende Format: `ContactName` (`ContactTitle`, `CompanyName`). Diese berechnete Spalte an, die in Berichten verwendet werden kann, bei der Anzeige von Informationen zu Lieferanten.

Öffnen Sie zunächst die `Suppliers` Tabellendefinition mit der rechten Maustaste auf die `Suppliers` Tabelle im Server-Explorer und die Tabellendefinition öffnen aus dem Kontextmenü auswählen. Dadurch werden die Spalten der Tabelle und deren Eigenschaften, z.B. dem Datentyp angezeigt werden, ob sie zulassen `NULL` s und So weiter. Um eine berechnete Spalte hinzuzufügen, starten Sie durch den Namen der Spalte in der Tabelle (Definition) eingeben. Geben Sie anschließend den Ausdruck in das Textfeld (Formel) im Abschnitt "Berechnetespaltespezifikation" im Fenster Eigenschaften (siehe Abbildung 1). Benennen Sie die berechnete Spalte `FullContactName` und verwenden Sie den folgenden Ausdruck:


[!code-sql[Main](working-with-computed-columns-vb/samples/sample1.sql)]

Beachten Sie, dass Zeichenfolgen können, in SQL verkettet werden mit der `+` Operator. Die `CASE` -Anweisung kann verwendet werden, wie eine bedingte in einer traditionellen Programmiersprache. Im obigen Ausdruck die `CASE` Anweisung als gelesen werden kann: Wenn `ContactTitle` ist nicht `NULL` danach die `ContactTitle` emit-Wert verkettet, die durch ein Komma, andernfalls "nothing". Weitere Informationen zu den Nutzen von der `CASE` -Anweisung finden Sie unter [die Leistungsfähigkeit von SQL `CASE` Anweisungen](http://www.4guysfromrolla.com/webtech/102704-1.shtml).

> [!NOTE]
> Anstatt eine `CASE` -Anweisung hier, wir Alternativ hätte `ISNULL(ContactTitle, '')`. [`ISNULL(checkExpression, replacementValue)`](https://msdn.microsoft.com/library/ms184325.aspx) Gibt *CheckExpression* , wenn er nicht NULL ist, andernfalls *"Ersatzwert"*. Entweder während `ISNULL` oder `CASE` funktioniert in diesem Fall stehen die schwierigeren Szenarien, in denen die Flexibilität der `CASE` Anweisung kann nicht zugeordnet werden, indem `ISNULL`.


Nach dem Hinzufügen diese berechnete Spalte sollte Ihr Bildschirm wie im Screenshot in Abbildung 1 aussehen.


[![Hinzufügen einer berechneten Spalteninhalts, die mit dem Namen FullContactName in die Tabelle ' Suppliers '](working-with-computed-columns-vb/_static/image2.png)](working-with-computed-columns-vb/_static/image1.png)

**Abbildung 1**: Hinzufügen einer berechneten Spalte mit dem Namen `FullContactName` auf die `Suppliers` Tabelle ([klicken Sie, um das Bild in voller Größe anzeigen](working-with-computed-columns-vb/_static/image3.png))


Nachdem die berechnete Spalte benennen und Ausdruck eingeben, speichern Sie die Änderungen in der Tabelle durch Klicken auf das Symbol "Speichern" in der Symbolleiste, durch Drücken von STRG + S oder indem Sie auf das Menü "Datei" und Auswählen von Speichern `Suppliers`.

Speichern der Tabelle sollten aktualisieren, einschließlich der gerade hinzugefügten Spalte im Server-Explorer die `Suppliers` Spaltenliste s'-Tabelle. Der Ausdruck in das Textfeld (Formel) eingegeben werden darüber hinaus automatisch angepasst, um einen entsprechenden Ausdruck aus, die Spaltennamen in Klammern umgeben unnötigen Leerraum entfernt (`[]`), sowie Klammern, um genauere anzeigen die Reihenfolge der Vorgänge:


[!code-sql[Main](working-with-computed-columns-vb/samples/sample2.sql)]

Weitere Informationen zu berechneten Spalten in der Microsoft SQL Server, finden Sie in der [technische Dokumentation](https://msdn.microsoft.com/library/ms191250.aspx). Sehen Sie sich auch die [Vorgehensweise: Specify Computed Columns](https://msdn.microsoft.com/library/ms188300.aspx) für schrittweise Anleitungen zum Erstellen von berechneten Spalten.

> [!NOTE]
> Standardmäßig berechnete Spalten nicht physisch in der Tabelle gespeichert sind, aber jedes Mal, wenn sie in einer Abfrage verwiesen werden, werden stattdessen neu berechnet. Allerdings können Sie SQL Server, um die berechnete Spalte physisch in der Tabelle zu speichern, indem Sie das Kontrollkästchen wird beibehalten, anweisen. Auf diese Weise kann ein Index für die berechnete Spalte erstellt werden, die die Leistung von Abfragen verbessern können, mit denen der Wert der berechneten Spalte in ihrer `WHERE` Klauseln. Finden Sie unter [Erstellen von Indizes für berechnete Spalten](https://msdn.microsoft.com/library/ms189292.aspx) für Weitere Informationen.


## <a name="step-2-viewing-the-computed-column-s-values"></a>Schritt 2: Anzeigen der Werte für berechnete Spalten s

Bevor wir die Arbeit auf der Datenzugriffsebene beginnen, können Sie s eine Minute dauern, an die `FullContactName` Werte. Im Server-Explorer mit der Maustaste auf die `Suppliers` Tabellennamen ein, und wählen Sie die neue Abfrage aus dem Kontextmenü. Dadurch wird ein Abfragefenster angezeigt, die fordert, wählen Sie die zu Tabellen in der Abfrage eingeschlossen werden sollen. Hinzufügen der `Suppliers` Tabelle, und klicken Sie auf Schließen. Als Nächstes überprüfen Sie die `CompanyName`, `ContactName`, `ContactTitle`, und `FullContactName` Spalten aus der Tabelle "Suppliers". Klicken Sie abschließend auf das rote Ausrufezeichen-Symbol in der Symbolleiste, um die Abfrage auszuführen und die Ergebnisse anzuzeigen.

Wie in Abbildung 2 gezeigt, enthalten die Ergebnisse `FullContactName`, die Listen der `CompanyName`, `ContactName`, und `ContactTitle` Spalten, die mit dem Format `ContactName` (`ContactTitle`, `CompanyName`).


[![Die FullContactName verwendet Format ContactName (ContactTitle, CompanyName)](working-with-computed-columns-vb/_static/image5.png)](working-with-computed-columns-vb/_static/image4.png)

**Abbildung 2**: die `FullContactName` verwendet das Format `ContactName` (`ContactTitle`, `CompanyName`) ([klicken Sie, um das Bild in voller Größe anzeigen](working-with-computed-columns-vb/_static/image6.png))


## <a name="step-3-adding-thesupplierstableadapterto-the-data-access-layer"></a>Schritt 3: Hinzufügen der`SuppliersTableAdapter`an die Datenzugriffsebene

Für die Arbeit mit den Lieferanteninformationen in unserer Anwendung müssen wir zuerst einen TableAdapter und einer Datentabelle in DAL erstellen. Im Idealfall würde dies erreicht werden mithilfe der gleichen einfache Schritte, die in den vorherigen Tutorials untersucht. Arbeiten mit berechneten Spalten führt jedoch einige Falten, die Erläuterung bedürfen.

Wenn Sie einen TableAdapter, die Ad-hoc-SQL-Anweisungen verwendet verwenden, können Sie einfach die berechnete Spalte in der TableAdapter der Hauptabfrage s, über den TableAdapter-Konfigurations-Assistenten einschließen. Dies allerdings generiert automatisch `INSERT` und `UPDATE` Anweisungen, die die berechnete Spalte enthalten. Wenn Sie versuchen, eine der folgenden Methoden verwenden, führen Sie eine `SqlException` mit der Nachricht aus der Spalte *ColumnName* kann nicht geändert werden, da sie entweder eine berechnete Spalte oder ist das Ergebnis eines UNION-Operators wird ausgelöst. Während der `INSERT` und `UPDATE` Anweisung angepasst werden, manuell über die TableAdapter `InsertCommand` und `UpdateCommand` Eigenschaften diese Anpassungen gehen verloren, wenn der TableAdapter-Konfigurations-Assistent erneut ausgeführt wird.

Aufgrund der Fehleranfälligkeit des TableAdapters, die Ad-hoc-SQL-Anweisungen verwenden, empfiehlt es sich, dass wir die gespeicherte Prozeduren verwenden, bei der Arbeit mit berechneten Spalten. Wenn Sie vorhandene gespeicherte Prozeduren verwenden, konfigurieren den TableAdapter einfach wie unter der [vorhandene gespeicherte Prozeduren verwenden, für die typisierte DataSet-s TableAdapters](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) Tutorial. Wenn Sie den TableAdapter-Assistenten, die die gespeicherten Prozeduren für Sie erstellt haben, ist es jedoch wichtig, zunächst alle berechneten Spalten aus der Hauptabfrage auszulassen. Wenn Sie eine berechnete Spalte in der Hauptabfrage enthalten, informiert der TableAdapter-Konfigurations-Assistent, nach Abschluss des Vorgangs Sie, dass die entsprechenden gespeicherten Prozeduren erstellt werden kann. Kurz gesagt, müssen wir den TableAdapter, die mit einer berechneten Spalte kostenfreie Hauptabfrage Beginn zu konfigurieren und aktualisieren Sie anschließend manuell die entsprechende gespeicherte Prozedur und die TableAdapter `SelectCommand` auf die berechnete Spalte enthalten. Dieser Ansatz ist ähnlich wie mit der [aktualisieren für die Verwendung des TableAdapters](updating-the-tableadapter-to-use-joins-vb.md)`JOIN`*s* Tutorial.

In diesem Tutorial können Sie s fügen Sie einen neuen TableAdapter und automatisch die gespeicherten Prozeduren für uns erstellen. Daher müssen wir zunächst lassen Sie die `FullContactName` berechnete Spalte aus der Hauptabfrage.

Öffnen Sie zunächst die `NorthwindWithSprocs` DataSet in den `~/App_Code/DAL` Ordner. Mit der rechten Maustaste im Designer, und wählen Sie aus dem Kontextmenü, um einen neuen TableAdapter hinzuzufügen. Hierdurch wird der TableAdapter-Konfigurations-Assistenten. Geben Sie die Datenbank zum Abfragen von Daten aus (`NORTHWNDConnectionString` aus `Web.config`), und klicken Sie auf Weiter. Da wir alle gespeicherten Prozeduren für Abfragen oder Ändern der noch nicht erstellt haben die `Suppliers` Tabelle, aktivieren Sie das Erstellen, die neue gespeicherte Prozeduren option, damit der Assistent für uns erstellen und klicken Sie auf Weiter.


[![Wählen Sie die neuen gespeicherten Prozeduren Option für das Erstellen](working-with-computed-columns-vb/_static/image8.png)](working-with-computed-columns-vb/_static/image7.png)

**Abbildung 3**: Wählen Sie die neuen gespeicherten Prozeduren Option für das Erstellen ([klicken Sie, um das Bild in voller Größe anzeigen](working-with-computed-columns-vb/_static/image9.png))


Der nachfolgende Schritt verlangt für die Hauptabfrage. Geben Sie die folgende Abfrage aus, wodurch die `SupplierID`, `CompanyName`, `ContactName`, und `ContactTitle` Spalten für jeden Lieferanten. Beachten Sie, dass diese Abfrage absichtlich die berechnete Spalte lässt (`FullContactName`); aktualisieren wir die entsprechende gespeicherte Prozedur aus, dass diese Spalte in Schritt 4 enthalten.


[!code-sql[Main](working-with-computed-columns-vb/samples/sample3.sql)]

Nach dem Eingeben der Hauptabfrage und anschließend auf klicken, kann der Assistenten wir vier gespeicherten Prozeduren, die er generiert einen Namen. Nennen Sie diese gespeicherten Prozeduren `Suppliers_Select`, `Suppliers_Insert`, `Suppliers_Update`, und `Suppliers_Delete`, wie in Abbildung 4 gezeigt.


[![Die Namen der automatisch generierten gespeicherten Prozeduren](working-with-computed-columns-vb/_static/image11.png)](working-with-computed-columns-vb/_static/image10.png)

**Abbildung 4**: die Namen von Auto-Generated gespeicherte Prozeduren ([klicken Sie, um das Bild in voller Größe anzeigen](working-with-computed-columns-vb/_static/image12.png))


Der nächste Assistentenschritt kann Wir nennen Sie die TableAdapter-s-Methoden, und geben Sie die Muster verwendet, um Daten von und zu aktualisieren. Lassen Sie alle drei Kontrollkästchen aktiviert, aber Umbenennen der `GetData` Methode, um `GetSuppliers`. Klicken Sie auf "Fertig stellen", um den Assistenten abzuschließen.


[![Benennen Sie die GetData-Methode in GetSuppliers](working-with-computed-columns-vb/_static/image14.png)](working-with-computed-columns-vb/_static/image13.png)

**Abbildung 5**: Benennen der `GetData` Methode, um `GetSuppliers` ([klicken Sie, um das Bild in voller Größe anzeigen](working-with-computed-columns-vb/_static/image15.png))


Nach dem Klicken auf "Fertig stellen", wird der Assistent vier gespeicherte Prozeduren erstellen und das typisierte DataSet die TableAdapter und die entsprechende DataTable hinzufügen.

## <a name="step-4-including-the-computed-column-in-the-tableadapter-s-main-query"></a>Schritt 4: Einschließlich der berechneten Spalte in der Hauptabfrage des TableAdapter s

Wir müssen jetzt den TableAdapter zu aktualisieren und DataTable erstellt in Schritt 3 sollen die `FullContactName` berechnete Spalte. Dies umfasst zwei Schritte:

1. Aktualisieren der `Suppliers_Select` gespeicherten Prozedur zurückgeben der `FullContactName` berechnete Spalte, und
2. Aktualisieren die DataTable aus, um die entsprechende include `FullContactName` Spalte.

Starten Sie, indem Sie den Server-Explorer navigieren und einen Drilldown in den Ordner gespeicherte Prozeduren. Öffnen der `Suppliers_Select` gespeicherte Prozedur und ein Update der `SELECT` Abfrage durch Hinzufügen der `FullContactName` berechnete Spalte:


[!code-sql[Main](working-with-computed-columns-vb/samples/sample4.sql)]

Speichern Sie die Änderungen an die gespeicherte Prozedur, indem Sie auf das Symbol "Speichern" in der Symbolleiste, durch Drücken von STRG + S oder durch Auswählen des Speichervorgangs `Suppliers_Select` Option über das Menü Datei.

Als Nächstes kehren Sie zur DataSet-Designer zurück, mit der rechten Maustaste auf die `SuppliersTableAdapter`, und wählen Sie aus dem Kontextmenü konfigurieren. Beachten Sie, dass die `Suppliers_Select` Spalte enthält jetzt die `FullContactName` Spalte in der Data Columns-Auflistung.


[![Führen Sie den TableAdapter-s-Konfigurations-Assistenten zum Aktualisieren der DataTable-s-Spalten](working-with-computed-columns-vb/_static/image17.png)](working-with-computed-columns-vb/_static/image16.png)

**Abbildung 6**: Führen Sie die TableAdapter-Konfigurations-Assistenten zum Aktualisieren der DataTable-s-Spalten ([klicken Sie, um das Bild in voller Größe anzeigen](working-with-computed-columns-vb/_static/image18.png))


Klicken Sie auf "Fertig stellen", um den Assistenten abzuschließen. Hiermit wird automatisch eine entsprechende Spalte der `SuppliersDataTable`. Der TableAdapter-Assistent ist intelligent genug, um zu ermitteln, die `FullContactName` Spalte ist eine berechnete Spalte und daher schreibgeschützt. Daher wird die Spalte s `ReadOnly` Eigenschaft `true`. Um dies zu überprüfen, wählen Sie die Spalte in der `SuppliersDataTable` und fahren Sie mit dem Fenster "Eigenschaften" (siehe Abbildung 7). Beachten Sie, dass die `FullContactName` s'-Spalte `DataType` und `MaxLength` Eigenschaften werden auch entsprechend festgelegt.


[![Die FullContactName-Spalte ist als schreibgeschützt gekennzeichnet.](working-with-computed-columns-vb/_static/image20.png)](working-with-computed-columns-vb/_static/image19.png)

**Abbildung 7**: die `FullContactName` Spalte ist als schreibgeschützt gekennzeichnet ([klicken Sie, um das Bild in voller Größe anzeigen](working-with-computed-columns-vb/_static/image21.png))


## <a name="step-5-adding-agetsupplierbysupplieridmethod-to-the-tableadapter"></a>Schritt 5: Hinzufügen einer`GetSupplierBySupplierID`Methode dem TableAdapter

In diesem Tutorial erstellen wir eine ASP.NET-Seite, die die Lieferanten in einer aktualisierbaren Tabelle anzeigt. In wurden nach Tutorials wir aktualisiert ein einzelnes Datensatzes aus der Geschäftslogikebene von bestimmten Datensatz von der DAL als stark typisierte DataTable, aktualisieren die Eigenschaften und anschließendes Senden der aktualisierten DataTable an die DAL senden, um die Änderungen weiterzugeben sichern wird abgerufen die Datenbank. Zum Durchführen dieses ersten Schritts – Abrufen des Datensatzes aktualisiert wird, von der DAL - müssen wir zuerst eine `GetSupplierBySupplierID(supplierID)` Methode, um die DAL.

Mit der rechten Maustaste auf die `SuppliersTableAdapter` DataSet entwerfen, und wählen Sie aus dem Kontextmenü die Option "hinzufügen". Wie in Schritt 3 können Sie den Assistenten eine neue gespeicherte Prozedur für uns zu generieren, indem die Erstellungsoption neue gespeicherte Prozedur (siehe Abbildung 3 einen Screenshot der dieser Schritt des Assistenten). Da diese Methode einen Datensatz mit mehreren Spalten zurückgibt, anzugeben Sie, dass wir verwenden möchten, verwenden Sie eine SQL-Abfrage, die eine SELECT-Anweisung ist die Zeilen zurückgibt, und klicken Sie auf Weiter.


[![Wählen Sie die wählen die Option-Zeilen zurückgibt](working-with-computed-columns-vb/_static/image23.png)](working-with-computed-columns-vb/_static/image22.png)

**Abbildung 8**: Wählen Sie die Auswahl, welche die Zeilen Option zurückgibt ([klicken Sie, um das Bild in voller Größe anzeigen](working-with-computed-columns-vb/_static/image24.png))


Der nachfolgende Schritt fordert uns für die Abfrage, die für diese Methode verwendet. Geben Sie Folgendes, die die gleichen Datenfelder wie die Hauptabfrage jedoch für einen bestimmten Lieferanten zurückgibt.


[!code-sql[Main](working-with-computed-columns-vb/samples/sample5.sql)]

Im nächste Bildschirm fordert uns, die Namen der gespeicherten Prozedur, die automatisch generiert werden. Nennen Sie diese gespeicherte Prozedur `Suppliers_SelectBySupplierID` , und klicken Sie auf Weiter.


[![Name der gespeicherten Prozedur Suppliers_SelectBySupplierID](working-with-computed-columns-vb/_static/image26.png)](working-with-computed-columns-vb/_static/image25.png)

**Abbildung 9**: Benennen Sie die gespeicherte Prozedur `Suppliers_SelectBySupplierID` ([klicken Sie, um das Bild in voller Größe anzeigen](working-with-computed-columns-vb/_static/image27.png))


Abschließend Anweisungen des Assistenten "us" für die Daten Zugriff auf Muster und Methodennamen, der für den TableAdapter verwendet. Lassen Sie beide Kontrollkästchen aktiviert, aber Umbenennen der `FillBy` und `GetDataBy` Methoden `FillBySupplierID` und `GetSupplierBySupplierID`bzw.


[![Name der TableAdapter-Methoden-FillBySupplierID und GetSupplierBySupplierID](working-with-computed-columns-vb/_static/image29.png)](working-with-computed-columns-vb/_static/image28.png)

**Abbildung 10**: Benennen Sie den TableAdapter-Methoden `FillBySupplierID` und `GetSupplierBySupplierID` ([klicken Sie, um das Bild in voller Größe anzeigen](working-with-computed-columns-vb/_static/image30.png))


Klicken Sie auf "Fertig stellen", um den Assistenten abzuschließen.

## <a name="step-6-creating-the-business-logic-layer"></a>Schritt 6: Erstellen der Geschäftslogikebene

Bevor wir eine ASP.NET-Seite, die die berechnete Spalte, die in Schritt 1 erstellte verwendet erstellen, müssen wir zunächst die entsprechenden Methoden der Geschäftslogikschicht hinzu. Unsere ASP.NET-Seite, dadurch wird es in Schritt 7 erstellt werden, können Benutzer anzeigen und Bearbeiten von Lieferanten. Daher benötigen wir unsere BLL, mindestens eine Methode zum Abrufen aller Lieferanten und eine weitere zum Aktualisieren eines bestimmten Lieferanten zu bieten.

Erstellen Sie eine neue Klassendatei mit dem Namen `SuppliersBLLWithSprocs` in die `~/App_Code/BLL` Ordner, und fügen Sie den folgenden Code hinzu:


[!code-vb[Main](working-with-computed-columns-vb/samples/sample6.vb)]

Wie die anderen Klassen BLL, `SuppliersBLLWithSprocs` verfügt über eine `Protected` `Adapter` -Eigenschaft, die eine Instanz der zurückgibt der `SuppliersTableAdapter` Klasse zusammen mit zwei `Public` Methoden: `GetSuppliers` und `UpdateSupplier`. Die `GetSuppliers` Methode aufruft, und gibt die `SuppliersDataTable` zurückgegeben, die mit den entsprechenden `GetSupplier` -Methode in der Datenzugriffsebene. Die `UpdateSupplier` Methode ruft Informationen zu den bestimmten Lieferanten, die über einen Aufruf an die DAL s aktualisiert `GetSupplierBySupplierID(supplierID)` Methode. Klicken Sie dann aktualisiert die `CategoryName`, `ContactName`, und `ContactTitle` Eigenschaften und übernimmt diese Änderungen in der Datenbank durch Aufrufen der Datenzugriffsebene s `Update` Methode und übergeben die geänderte `SuppliersRow` Objekt.

> [!NOTE]
> Mit Ausnahme von `SupplierID` und `CompanyName`, können Sie alle Spalten in der Tabelle "Suppliers" `NULL` Werte. Aus diesem Grund Wenn der übergegebenen `contactName` oder `contactTitle` Parameter sind `Nothing` müssen wir legen Sie die entsprechende `ContactName` und `ContactTitle` Eigenschaften, die eine `NULL` Datenbank mithilfe der `SetContactNameNull` und `SetContactTitleNull`Methoden bzw.


## <a name="step-7-working-with-the-computed-column-from-the-presentation-layer"></a>Schritt 7: Arbeiten mit der berechneten Spalte auf der Darstellungsschicht

Mit der berechneten Spalte hinzugefügt, die `Suppliers` Tabelle und der DAL und BLL entsprechend aktualisiert, wir können eine ASP.NET-Seite erstellen, die mit der `FullContactName` berechnete Spalte. Öffnen Sie zunächst die `ComputedColumns.aspx` auf der Seite die `AdvancedDAL` Ordner, und ziehen Sie einer GridView-Ansicht aus der Toolbox in den Designer. Legen Sie die GridView s `ID` Eigenschaft `Suppliers` und von sein Smarttag, binden Sie es an eine neue, mit dem Namen "ObjectDataSource" `SuppliersDataSource`. Konfigurieren Sie mit dem ObjectDataSource-Steuerelement die `SuppliersBLLWithSprocs` Klasse, die wir hinzugefügt, in Schritt 6 zurück, und klicken Sie auf Weiter.


[![Konfigurieren von dem ObjectDataSource-Steuerelement zur Verwendung der SuppliersBLLWithSprocs-Klasse](working-with-computed-columns-vb/_static/image32.png)](working-with-computed-columns-vb/_static/image31.png)

**Abbildung 11**: Konfigurieren von dem ObjectDataSource-Steuerelement verwenden die `SuppliersBLLWithSprocs` Klasse ([klicken Sie, um das Bild in voller Größe anzeigen](working-with-computed-columns-vb/_static/image33.png))


Es gibt nur zwei Methoden, die definiert, der `SuppliersBLLWithSprocs` Klasse: `GetSuppliers` und `UpdateSupplier`. Stellen Sie sicher, dass diese beiden Methoden werden in die SELECT-Anweisung angegeben bzw. Aktualisieren von Registerkarten, und klicken Sie auf "Fertig stellen", um die Konfiguration von dem ObjectDataSource-Steuerelement abzuschließen.

Nach Abschluss des Assistenten zum Konfigurieren von Datenquellen wird Visual Studio eine BoundField für jede der die zurückgegebenen Datenfelder hinzugefügt. Entfernen der `SupplierID` BoundField, und ändern Sie die `HeaderText` Eigenschaften der `CompanyName`, `ContactName`, `ContactTitle`, und `FullContactName` BoundFields zu Unternehmen, Contact Name, Titel und vollständiger Name sein wenden Sie sich an, bzw. Überprüfen Sie aus dem Smarttag das Kontrollkästchen Bearbeiten aktivieren, um den integrierten Bearbeitungsfunktionen für s GridView zu aktivieren.

Zusätzlich zum Hinzufügen von BoundFields an die GridView, Abschluss des Datenquellen-Assistenten auch bewirkt, dass Visual Studio, um das "ObjectDataSource"-s festgelegt `OldValuesParameterFormatString` Eigenschaft, um die ursprüngliche\_{0}. Wiederherstellen, diese Einstellung wieder auf den Standardwert {0} .

Nachdem Sie diese Änderungen an die GridView und "ObjectDataSource" vornehmen, sollte ihre deklarative Markup etwa wie folgt aussehen:


[!code-aspx[Main](working-with-computed-columns-vb/samples/sample7.aspx)]

Dann besuchen Sie diese Seite über einen Browser ein. Wie in Abbildung 12 gezeigt, enthält der einzelnen Lieferanten in einem Raster an, die enthält die `FullContactName` Spalte, deren Wert einfach die Verkettung von den anderen drei Spalten ist formatiert als `ContactName` (`ContactTitle`, `CompanyName`).


[![Jeder Anbieter wird im Raster aufgeführt.](working-with-computed-columns-vb/_static/image35.png)](working-with-computed-columns-vb/_static/image34.png)

**Abbildung 12**: jede Lieferanten finden Sie im Raster ([klicken Sie, um das Bild in voller Größe anzeigen](working-with-computed-columns-vb/_static/image36.png))


Auf die Schaltfläche "Bearbeiten", für die ein bestimmter Lieferant ein Postback auslöst und diese Zeile wurde gerendert werden, dessen Bearbeitung Schnittstelle (siehe Abbildung 13). Die ersten drei Spalten in den Standardwert bearbeiten-Schnittstelle zu rendern: ein TextBox-Steuerelement, dessen `Text` -Eigenschaftensatz auf den Wert des Felds. Die `FullContactName` Spalte bleibt jedoch als Text. Bei der BoundFields an die GridView nach dem Abschluss des Assistenten zum Konfigurieren von Datenquellen hinzugefügt wurden die `FullContactName` BoundField-s `ReadOnly` -Eigenschaft wurde festgelegt, um `True` da das entsprechende `FullContactName` -Spalte in der `SuppliersDataTable` verfügt über seine `ReadOnly` -Eigenschaftensatz auf `True`. Wie in Schritt 4 erwähnt die `FullContactName` s `ReadOnly` -Eigenschaft wurde festgelegt, um `True` da TableAdapter erkannt, dass die Spalte eine berechnete Spalte ist.


[![Die FullContactName-Spalte kann nicht bearbeitet werden.](working-with-computed-columns-vb/_static/image38.png)](working-with-computed-columns-vb/_static/image37.png)

**Abbildung 13**: die `FullContactName` Spalte kann nicht bearbeitet werden ([klicken Sie, um das Bild in voller Größe anzeigen](working-with-computed-columns-vb/_static/image39.png))


Fahren Sie fort, und aktualisieren Sie den Wert von einem oder mehreren Spalten bearbeitet werden, und klicken Sie auf aktualisieren. Beachten Sie die `FullContactName` s-Wert wird automatisch aktualisiert, um die Änderung zu übernehmen.

> [!NOTE]
> Das GridView verwendet derzeit BoundFields für die Felder bearbeitet werden, wodurch der Standardbereich bearbeiten-Schnittstelle. Da die `CompanyName` Feld ist erforderlich, es in ein TemplateField, die einen RequiredFieldValidator enthält konvertiert werden sollen. Ich lassen Sie dieses als Übung für den interessierten Leser. Wenden Sie sich an den [Hinzufügen von Steuerelementen zur gültigkeitsprüfung zum Bearbeiten und Einfügen von Schnittstellen](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) Tutorial für-Schritt-Anweisungen eine BoundField in ein TemplateField konvertieren und Hinzufügen von Validierungssteuerelementen.


## <a name="summary"></a>Zusammenfassung

Wenn Sie das Schema für eine Tabelle zu definieren, können Microsoft SQL Server die Einbeziehung von berechneten Spalten. Hierbei handelt es sich um Spalten, deren Werte aus einem Ausdruck berechnet werden, die die Werte in der Regel von anderen Spalten in den gleichen Datensatz verweist. Da die Werte, für berechnete Spalten in einem Ausdruck basieren sie sind schreibgeschützt und kann nicht zugewiesen werden einen Wert in eine `INSERT` oder `UPDATE` Anweisung. Dies führt Herausforderungen, wenn eine berechnete Spalte in der Hauptabfrage des TableAdapter zu verwenden, die versucht, die zum automatischen Generieren von entsprechenden `INSERT`, `UPDATE`, und `DELETE` Anweisungen.

In diesem Tutorial erläutert die Verfahren zum Umgehen der Herausforderungen, berechneten Spalten darstellen. Insbesondere verwendet wir gespeicherte Prozeduren in unserer TableAdapter, um der inhärenten TableAdapters Fehleranfälligkeit zu umgehen, die Ad-hoc-SQL-Anweisungen verwenden. Wenn Sie Prozeduren mit den TableAdapter-Assistenten neu erstellen gespeicherte, ist es wichtig, wir haben die Hauptabfrage anfänglich alle berechneten Spalten auslassen, da ihre Anwesenheit verhindert, dass die gespeicherten Änderungsprozeduren Daten generiert wird. Nachdem der TableAdapter ursprünglich konfiguriert wurde, dessen `SelectCommand` gespeicherte Prozedur retooled werden kann, um eine berechnete Spalte enthalten.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial wurden Hilton Geisenow und Teresa Murphy. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](adding-additional-datatable-columns-vb.md)
> [Weiter](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb.md)
