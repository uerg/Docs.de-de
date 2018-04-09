---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/working-with-computed-columns-cs
title: Arbeiten mit berechneten Spalten (c#) | Microsoft Docs
author: rick-anderson
description: Beim Erstellen einer Datenbanktabelle, Microsoft SQL Server können Sie eine berechnete Spalte zu definieren, dessen Wert aus einem Ausdruck berechnet wird, der normalerweise verw...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: 57459065-ed7c-4dfe-ac9c-54c093abc261
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/working-with-computed-columns-cs
msc.type: authoredcontent
ms.openlocfilehash: 7a67abd2a0c140c0503c07f764549a6d90ef7298
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="working-with-computed-columns-c"></a>Arbeiten mit berechneten Spalten (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Herunterladen von Code](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_71_CS.zip) oder [PDF herunterladen](working-with-computed-columns-cs/_static/datatutorial71cs1.pdf)

> Wenn Sie eine Datenbanktabelle zu erstellen, können mit Microsoft SQL Server eine berechnete Spalte definieren, deren Wert aus einem Ausdruck berechnet wird, die auf andere Werte in der gleichen Datensatz in der Datenbank in der Regel verweist. Solche Werte sind in der Datenbank, wofür Besonderheiten beim Arbeiten mit TableAdapters schreibgeschützt. In diesem Lernprogramm erfahren wir die Bedrohung durch berechnete Spalten Auflagen erfüllt werden können.


## <a name="introduction"></a>Einführung

Microsoft SQL Server ermöglicht  *[berechnete Spalten](https://msdn.microsoft.com/library/ms191250.aspx)*, die Spalten, deren Werte werden von einem Ausdruck, der in der Regel die Werte aus anderen Spalten in derselben Tabelle verweist auf berechnet, werden. Eine Zeit nachverfolgen Datenmodell möglicherweise als Beispiel eine Tabelle namens `ServiceLog` mit Spalten, einschließlich `ServicePerformed`, `EmployeeID`, `Rate`, und `Duration`, o. ä. Während der Gesamtbetrag pro-Dienst-Element (wird die Rate, multipliziert mit der Dauer) konnte über eine Webseite oder andere programmgesteuerte Schnittstelle berechnet werden, ist es möglicherweise hilfreich, eine Spalte in der `ServiceLog` Tabelle mit dem Namen `AmountDue` , die dies gemeldet Informationen. Diese Spalte kann als normale Spalte erstellt werden, aber er dürfte jederzeit aktualisiert werden die `Rate` oder `Duration` Spaltenwerte geändert. Ein besserer Ansatz wäre, stellen die `AmountDue` Spalte eine berechnete Spalte mit dem Ausdruck `Rate * Duration`. Auf diese Weise würde dazu führen, dass SQL Server für die automatische Berechnung der `AmountDue` Spaltenwert immer in einer Abfrage auf sie verwiesen wurde.

Da ein berechnete Spalte s-Wert durch einen Ausdruck bestimmt ist, solche Spalten sind schreibgeschützt und aus diesem Grund können keine Werte zugewiesene auf diese Dateien im `INSERT` oder `UPDATE` Anweisungen. Wenn berechnete Spalten Teil der Hauptabfrage für einen TableAdapter, die Ad-hoc-SQL-Anweisungen verwendet handelt, sie werden jedoch automatisch eingeschlossen in automatisch generierte `INSERT` und `UPDATE` Anweisungen. Folglich die TableAdapter `INSERT` und `UPDATE` Abfragen und `InsertCommand` und `UpdateCommand` Eigenschaften müssen aktualisiert werden, um Verweise auf alle berechneten Spalten zu entfernen.

Eine Herausforderung der Verwendung von berechneten Spalten mit einem TableAdapter, die Ad-hoc-SQL-Anweisungen verwendet wird, die die TableAdapter `INSERT` und `UPDATE` Abfragen automatisch erneut generiert, einem beliebigen Zeitpunkt der TableAdapter-Konfigurations-Assistent abgeschlossen ist. Aus diesem Grund manuell die berechneten Spalten daraus die `INSERT` und `UPDATE` Abfragen werden erneut angezeigt, wenn der Assistent erneut ausgeführt wird. Obwohl TableAdapters, die gespeicherten Prozeduren verwenden t Verschlüsselungskennwort aus dieser unzulänglichen beeinträchtigt, besitzen ihre eigenen Quirksmodus, die wir in Schritt 3 gerecht werden.

In diesem Lernprogramm werden wir eine berechnete Spalte hinzufügen der `Suppliers` -Tabelle in der Northwind-Datenbank und erstellen Sie einen entsprechenden TableAdapter zum Arbeiten mit dieser Tabelle und einer berechneten Spalte. Wir haben unsere TableAdapter gespeicherte Prozeduren anstelle von Ad-hoc-SQL-Anweisungen verwenden, sodass unsere Anpassungen sind t verloren, wenn der TableAdapter-Konfigurations-Assistent verwendet wird.

Lassen Sie s beginnen!

## <a name="step-1-adding-a-computed-column-to-thesupplierstable"></a>Schritt 1: Hinzufügen einer berechneten Spalte, die`Suppliers`Tabelle

Die Northwind-Datenbank besitzt keine berechneten Spalten, sodass wir uns ein hinzuzufügende benötigen. Für dieses Lernprogramm s eine berechnete Spalte hinzufügen kann die `Suppliers` Tabelle mit dem Namen `FullContactName` , die der-s-Kontaktname, Titel und das Unternehmen, die sie für funktionieren zurückgibt, im folgenden Format: `ContactName` (`ContactTitle`, `CompanyName`). Diese berechnete Spalte möglicherweise beim Anzeigen von Informationen zu Lieferanten in Berichten verwendet.

Öffnen Sie zunächst die `Suppliers` Tabellendefinition mit der rechten Maustaste auf die `Suppliers` Tabelle im Server-Explorer und Tabellendefinition öffnen aus dem Kontextmenü auswählen. Dadurch werden die Spalten der Tabelle und ihre Eigenschaften, z. B. ihr Datentyp angezeigt, ob sie zulassen `NULL` s und So weiter. Um eine berechnete Spalte hinzuzufügen, starten Sie dazu den Namen der Spalte in der Tabelle (Definition). Geben Sie anschließend den Ausdruck in das Textfeld (Formel), unter dem Abschnitt Berechnetespaltespezifikation im Fenster Spalteneigenschaften (siehe Abbildung 1). Benennen Sie die berechnete Spalte `FullContactName` und verwenden Sie den folgenden Ausdruck:


[!code-sql[Main](working-with-computed-columns-cs/samples/sample1.sql)]

Beachten Sie, dass Zeichenfolgen können, in SQL verkettet werden mit dem `+` Operator. Die `CASE` -Anweisung kann verwendet werden, wie eine bedingte in einer herkömmlichen Programmiersprache Ihrer Wahl. Im obigen Ausdruck der `CASE` Anweisung als gelesen werden kann: Wenn `ContactTitle` ist nicht `NULL` ausgeben der `ContactTitle` durch ein Komma, andernfalls verketteter Wert ausgeben nichts. Weitere Informationen über die Nützlichkeit der der `CASE` -Anweisung finden Sie unter [der SQL-Power `CASE` Anweisungen](http://www.4guysfromrolla.com/webtech/102704-1.shtml).

> [!NOTE]
> Anstatt eine `CASE` Anweisung hier wir Alternativ hätte `ISNULL(ContactTitle, '')`. [`ISNULL(checkExpression, replacementValue)`](https://msdn.microsoft.com/library/ms184325.aspx) Gibt *CheckExpression* , wenn er nicht NULL ist, anderfalls wird *Ersatzwert*. Entweder während `ISNULL` oder `CASE` funktioniert in dieser Instanz stehen mehr komplizierte Szenarien, in denen die Flexibilität der `CASE` Anweisung kann nicht zugeordnet werden, indem `ISNULL`.


Nach dem Hinzufügen der diese berechnete Spalte sollten Ihren Bildschirm wie im Screenshot in Abbildung 1 aussehen.


[![Fügen Sie eine berechnete Spalte mit dem Namen FullContactName der Lieferanten-Tabelle](working-with-computed-columns-cs/_static/image2.png)](working-with-computed-columns-cs/_static/image1.png)

**Abbildung 1**: Hinzufügen einer berechneten Spalte mit dem Namen `FullContactName` auf die `Suppliers` Tabelle ([klicken Sie hier, um das Bild in voller Größe angezeigt](working-with-computed-columns-cs/_static/image3.png))


Nach dem Benennen der berechneten Spalte, und der Ausdruck eingeben, speichern Sie die Änderungen in der Tabelle durch Klicken auf das Symbol "Speichern" in der Symbolleiste, drücken Sie STRG + S oder indem Sie im Menü Datei aufrufen und Auswählen von Speichern `Suppliers`.

Speichern der Tabelle einschließlich der gerade hinzugefügten Spalte im Server-Explorer aktualisieren sollten die `Suppliers` Spaltenliste s'-Tabelle. Der Ausdruck in das Textfeld (Formel) eingegeben werden darüber hinaus automatisch angepasst, um einen entsprechenden Ausdruck aus, die Spaltennamen in Klammern umgeben unnötige Leerzeichen entfernt (`[]`), sowie Klammern, um mehr explizit anzeigen die Reihenfolge der Vorgänge:


[!code-sql[Main](working-with-computed-columns-cs/samples/sample2.sql)]

Weitere Informationen zu berechneten Spalten in der Microsoft SQL Server, finden Sie in der [technische Dokumentation](https://msdn.microsoft.com/library/ms191250.aspx). Außerdem sehen Sie sich die [How to: Specify Computed Columns](https://msdn.microsoft.com/library/ms188300.aspx) eine schrittweise Anleitung zum Erstellen von berechneten Spalten.

> [!NOTE]
> Standardmäßig werden berechnete Spalten nicht physisch in der Tabelle gespeichert sind jedoch jedes Mal, wenn diese in einer Abfrage verwiesen wird, werden stattdessen neu berechnet. Überprüfen Sie das Kontrollkästchen wird beibehalten, jedoch können Sie SQL Server, um die berechnete Spalte in der Tabelle physisch gespeichert anweisen. Auf diese Weise kann ein Index für die berechnete Spalte erstellt werden, die die Leistung von Abfragen verbessern können, mit denen der Wert der berechneten Spalte in ihren `WHERE` Klauseln. Finden Sie unter [Erstellen von Indizes für berechnete Spalten](https://msdn.microsoft.com/library/ms189292.aspx) für Weitere Informationen.


## <a name="step-2-viewing-the-computed-column-s-values"></a>Schritt 2: Anzeigen der Werte für berechnete Spalten s

Bevor wir die Arbeit auf der Datenzugriffsebene beginnen, Let s nehmen Sie sich an den `FullContactName` Werte. Im Server-Explorer mit der Maustaste auf die `Suppliers` Tabellennamen ein, und wählen Sie die neue Abfrage aus dem Kontextmenü. Hierdurch wird ein Abfragefenster angezeigt, die uns, welche Tabellen in der Abfrage wählen aufgefordert werden. Hinzufügen der `Suppliers` Tabelle, und klicken Sie auf Schließen. Überprüfen Sie anschließend die `CompanyName`, `ContactName`, `ContactTitle`, und `FullContactName` Spalten aus der Tabelle "Suppliers". Klicken Sie abschließend auf das Symbol "rotes Ausrufezeichen" in der Symbolleiste, um die Abfrage auszuführen und die Ergebnisse anzuzeigen.

Wie in Abbildung 2 gezeigt, enthalten die Ergebnisse `FullContactName`, die Listen der `CompanyName`, `ContactName`, und `ContactTitle` Spalten mithilfe der Format-Ldquo;`ContactName` (`ContactTitle`, `CompanyName`) .


[![Die FullContactName verwendet das Format ContactName (ContactTitle, CompanyName)](working-with-computed-columns-cs/_static/image5.png)](working-with-computed-columns-cs/_static/image4.png)

**Abbildung 2**: die `FullContactName` verwendet das Format `ContactName` (`ContactTitle`, `CompanyName`) ([klicken Sie hier, um das Bild in voller Größe angezeigt](working-with-computed-columns-cs/_static/image6.png))


## <a name="step-3-adding-thesupplierstableadapterto-the-data-access-layer"></a>Schritt 3: Hinzufügen der`SuppliersTableAdapter`auf der Datenzugriffsebene

Um mit den Lieferanten in der vorliegenden Anwendung arbeiten muss zunächst ein TableAdapter und einer Datentabelle in unserer DAL erstellt werden. Im Idealfall würde dies erfolgen mithilfe der gleichen einfache Schritte, die in früheren Lernprogramme untersucht. Arbeiten mit berechneten Spalten stellt jedoch einige Falten, die Erläuterung bedürfen.

Wenn Sie einen TableAdapter, die Ad-hoc-SQL-Anweisungen verwendet verwenden, können Sie einfach die berechnete Spalte in der TableAdapter Hauptabfrage s über den TableAdapter-Konfigurations-Assistenten einschließen. Hierzu jedoch automatisch generiert `INSERT` und `UPDATE` Anweisungen, die die berechnete Spalte enthalten. Wenn Sie versuchen, eine dieser Methoden Ausführen einer `SqlException` mit der Meldung der Spalte *ColumnName* kann nicht geändert werden, da sie entweder eine berechnete Spalte ist oder das Ergebnis eines UNION-Operators wird ausgelöst. Während der `INSERT` und `UPDATE` Anweisung angepasst werden, manuell über die TableAdapter `InsertCommand` und `UpdateCommand` Eigenschaften, diese Anpassungen gehen verloren, sobald der TableAdapter-Konfigurations-Assistent erneut ausgeführt wird.

Aufgrund der unzulänglichen TableAdapters, die Ad-hoc-SQL-Anweisungen verwenden, wird empfohlen, dass wir bei der Arbeit mit berechneten Spalten gespeicherte Prozeduren verwenden. Wenn Sie vorhandene gespeicherte Prozeduren verwenden, Konfigurieren des TableAdapter einfach wie beschrieben in der [vorhandene gespeicherte Prozeduren verwenden, für die typisierte DataSet s TableAdapters](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) Lernprogramm. Wenn Sie den TableAdapter-Assistenten, die die gespeicherten Prozeduren erstellt haben, ist es allerdings wichtig, zunächst alle berechneten Spalten aus der Hauptabfrage weglassen. Wenn Sie eine berechnete Spalte in der Hauptabfrage enthalten, informiert der TableAdapter-Konfigurations-Assistent, nach Abschluss des Vorgangs Sie, dass die entsprechenden gespeicherten Prozeduren erstellt werden kann. Kurz gesagt, müssen wir den TableAdapter eine berechnete Spalte frei hauptspeicherpool für Abfragen mit Beginn zu konfigurieren und dann manuell aktualisieren, die entsprechende gespeicherte Prozedur und die TableAdapter `SelectCommand` die berechnete Spalte eingeschlossen werden soll. Dieser Ansatz gleicht die Abfrage verwendet die [Aktualisieren des TableAdapters zu verwendenden](updating-the-tableadapter-to-use-joins-cs.md)`JOIN`*s* Lernprogramm.

Können Sie für dieses Lernprogramm s fügen Sie einen neuen TableAdapter und sorgen sie die gespeicherten Prozeduren automatisch für uns erstellt. Folglich müssen wir zunächst weglassen der `FullContactName` berechnete Spalte aus der Hauptabfrage.

Öffnen Sie zunächst die `NorthwindWithSprocs` DataSet in den `~/App_Code/DAL` Ordner. Mit der rechten Maustaste im Designer, und wählen Sie aus dem Kontextmenü, um einen neuen TableAdapter hinzuzufügen. Hierdurch wird der TableAdapter-Konfigurations-Assistent gestartet. Geben Sie zum Abfragen von Daten aus der Datenbank (`NORTHWNDConnectionString` aus `Web.config`), und klicken Sie auf Weiter. Da alle gespeicherten Prozeduren zum Abfragen oder Ändern von noch nicht erstellt haben die `Suppliers` table, aktivieren Sie das Erstellen neue gespeicherte Prozeduren aus, damit der Assistent für uns zu erstellen, und klicken Sie auf Weiter.


[![Wählen Sie die neuen gespeicherten Prozeduren Option für das Erstellen](working-with-computed-columns-cs/_static/image8.png)](working-with-computed-columns-cs/_static/image7.png)

**Abbildung 3**: Wählen Sie die neuen gespeicherten Prozeduren Option für das Erstellen ([klicken Sie hier, um das Bild in voller Größe angezeigt](working-with-computed-columns-cs/_static/image9.png))


Die nachfolgende Schritt werden uns der Hauptabfrage aufgefordert. Geben Sie die folgende Abfrage gibt die `SupplierID`, `CompanyName`, `ContactName`, und `ContactTitle` Spalten für jeden Lieferanten. Beachten Sie, dass diese Abfrage die berechnete Spalte absichtlich ausgelassen (`FullContactName`); Wir aktualisieren die entsprechende gespeicherte Prozedur, dass diese Spalte in Schritt 4 enthalten.


[!code-sql[Main](working-with-computed-columns-cs/samples/sample3.sql)]

Nach dem Eingeben der Hauptabfrage und wenn Sie auf Weiter, kann der Assistent Wir nennen Sie die vier gespeicherten Prozeduren aus die es generiert werden. Nennen Sie diese gespeicherten Prozeduren `Suppliers_Select`, `Suppliers_Insert`, `Suppliers_Update`, und `Suppliers_Delete`, wie in Abbildung 4 dargestellt.


[![Passen Sie die Namen der automatisch generierten gespeicherten Prozeduren](working-with-computed-columns-cs/_static/image11.png)](working-with-computed-columns-cs/_static/image10.png)

**Abbildung 4**: die Namen von Auto-Generated gespeicherte Prozeduren ([klicken Sie hier, um das Bild in voller Größe angezeigt](working-with-computed-columns-cs/_static/image12.png))


Der nächste Assistentenschritt kann wir ein TableAdapter-s-Methoden, und geben die Muster, die zum Zugreifen auf und Aktualisieren von Daten. Belassen Sie alle drei Kontrollkästchen aktiviert, aber Umbenennen der `GetData` aufzurufende Methode `GetSuppliers`. Klicken Sie auf ' Fertig stellen ', um den Assistenten abzuschließen.


[![Benennen Sie die GetData-Methode in GetSuppliers](working-with-computed-columns-cs/_static/image14.png)](working-with-computed-columns-cs/_static/image13.png)

**Abbildung 5**: Benennen der `GetData` aufzurufende Methode `GetSuppliers` ([klicken Sie hier, um das Bild in voller Größe angezeigt](working-with-computed-columns-cs/_static/image15.png))


Beim Klicken auf "Fertig stellen" wird der Assistent vier gespeicherte Prozeduren erstellen und Hinzufügen des TableAdapter und die entsprechenden DataTable zu das typisierte DataSet.

## <a name="step-4-including-the-computed-column-in-the-tableadapter-s-main-query"></a>Schritt 4: Die berechnete Spalte in der Hauptabfrage des TableAdapter s einschließen

Jetzt müssen wir den TableAdapter aktualisieren und DataTable erstellt in Schritt 3 enthalten die `FullContactName` berechnete Spalte. Dies umfasst zwei Schritte:

1. Aktualisieren der `Suppliers_Select` gespeicherten Prozedur zurückgeben der `FullContactName` berechnete Spalte ist, und
2. Aktualisieren von DataTable, um ein entsprechendes enthalten `FullContactName` Spalte.

Starten Sie, indem Sie auf den Server-Explorer navigieren und Drilldowns in den Ordner "Stored Procedures". Öffnen der `Suppliers_Select` gespeicherte Prozedur und ein Update der `SELECT` Abfrage der `FullContactName` berechnete Spalte:


[!code-sql[Main](working-with-computed-columns-cs/samples/sample4.sql)]

Speichern Sie die Änderungen an die gespeicherte Prozedur, indem Sie auf das Symbol "Speichern" in der Symbolleiste, durch Drücken von STRG + S oder durch Auswählen von Speichern `Suppliers_Select` Option über das Menü Datei.

Als Nächstes auf der DataSet-Designer zurück, mit der rechten Maustaste auf die `SuppliersTableAdapter`, und wählen Sie aus dem Kontextmenü konfigurieren. Beachten Sie, dass die `Suppliers_Select` Spalte enthält jetzt die `FullContactName` Spalte in der Data Columns-Auflistung.


[![Führen Sie den TableAdapter-s-Konfigurations-Assistenten zum Aktualisieren von DataTable-s-Spalten](working-with-computed-columns-cs/_static/image17.png)](working-with-computed-columns-cs/_static/image16.png)

**Abbildung 6**: Führen Sie die TableAdapter-Konfigurations-Assistenten zum Aktualisieren der DataTable-s-Spalten ([klicken Sie hier, um das Bild in voller Größe angezeigt](working-with-computed-columns-cs/_static/image18.png))


Klicken Sie auf ' Fertig stellen ', um den Assistenten abzuschließen. Dadurch wird automatisch hinzugefügt, eine entsprechende Spalte der `SuppliersDataTable`. TableAdapter-Assistenten ist intelligent genug, um zu erkennen, die die `FullContactName` Spalte ist eine berechnete Spalte und daher schreibgeschützt. Folglich wird die Spalte s `ReadOnly` Eigenschaft `true`. Um dies zu überprüfen, wählen Sie die Spalte aus der `SuppliersDataTable` und fahren Sie mit dem Eigenschaftenfenster (siehe Abbildung 7). Beachten Sie, dass die `FullContactName` Spalte s `DataType` und `MaxLength` Eigenschaften werden auch entsprechend festgelegt.


[![Die FullContactName-Spalte ist als schreibgeschützt gekennzeichnet.](working-with-computed-columns-cs/_static/image20.png)](working-with-computed-columns-cs/_static/image19.png)

**Abbildung 7**: die `FullContactName` Spalte ist als schreibgeschützt gekennzeichnet ([klicken Sie hier, um das Bild in voller Größe angezeigt](working-with-computed-columns-cs/_static/image21.png))


## <a name="step-5-adding-agetsupplierbysupplieridmethod-to-the-tableadapter"></a>Schritt 5: Hinzufügen einer`GetSupplierBySupplierID`aufzurufende Methode des TableAdapter

Für dieses Lernprogramm erstellen wir eine ASP.NET-Seite auf, die die Lieferanten in einer aktualisierbaren Tabelle anzeigt. In wurden hinter Lernprogramme wir aktualisiert einen einzelnen Datensatz aus der Business Logic Layer durch Abrufen, DAL zum Weitergeben von Änderungen an bestimmten Datensatz von der DAL als stark typisierte DataTable, aktualisieren die Eigenschaften und anschließendes Senden der aktualisierten DataTable zurück die Datenbank. Im ersten Schritt – Abrufen der den aktualisierten Datensatz gelesen von der DAL - Durchführung wir müssen zunächst eine `GetSupplierBySupplierID(supplierID)` Methode, um der DAL.

Mit der rechten Maustaste auf die `SuppliersTableAdapter` DataSet entwerfen, und wählen Sie aus dem Kontextmenü die Option der Abfrage hinzufügen. Wie in Schritt 3, können Sie den Assistenten eine neue gespeicherte Prozedur für uns zu generieren, indem Sie die Option erstellen neue gespeicherte Prozedur (siehe Abbildung 3 für einen Screenshot der dieser Schritt des Assistenten). Da diese Methode einen Datensatz mit mehreren Spalten zurückgibt, geben Sie an, dass wir verwenden möchten, verwenden Sie eine SQL-Abfrage, die eine SELECT-Anweisung ist die Zeilen zurückgibt, und klicken Sie auf Weiter.


[![Wählen Sie wählen die Option-Zeilen zurückgibt](working-with-computed-columns-cs/_static/image23.png)](working-with-computed-columns-cs/_static/image22.png)

**Abbildung 8**: Wählen Sie die wählen Sie die Zeilen Option zurückgibt ([klicken Sie hier, um das Bild in voller Größe angezeigt](working-with-computed-columns-cs/_static/image24.png))


Die nachfolgende Schritt fordert "us" für die Abfrage für diese Methode verwendet. Geben Sie Folgendes ein, die die gleichen Datenfelder wie der Hauptabfrage jedoch für einen bestimmten Lieferanten zurückgibt.


[!code-sql[Main](working-with-computed-columns-cs/samples/sample5.sql)]

Der nächste Bildschirm fordert uns auf den Namen der gespeicherten Prozedur, die automatisch generiert werden. Nennen Sie diese gespeicherte Prozedur `Suppliers_SelectBySupplierID` , und klicken Sie auf Weiter.


[![Name der gespeicherten Prozedur Suppliers_SelectBySupplierID](working-with-computed-columns-cs/_static/image26.png)](working-with-computed-columns-cs/_static/image25.png)

**Abbildung 9**: Benennen Sie die gespeicherte Prozedur `Suppliers_SelectBySupplierID` ([klicken Sie hier, um das Bild in voller Größe angezeigt](working-with-computed-columns-cs/_static/image27.png))


Schließlich der Assistent fordert "us" für die Daten Muster und Methodennamen, verwenden Sie für den TableAdapter zugreifen. Lassen Sie beide Kontrollkästchen aktiviert, aber Umbenennen der `FillBy` und `GetDataBy` Methoden `FillBySupplierID` und `GetSupplierBySupplierID`bzw.


[![Name der TableAdapter-Methoden FillBySupplierID und GetSupplierBySupplierID](working-with-computed-columns-cs/_static/image29.png)](working-with-computed-columns-cs/_static/image28.png)

**Abbildung 10**: Benennen Sie die TableAdapter-Methoden `FillBySupplierID` und `GetSupplierBySupplierID` ([klicken Sie hier, um das Bild in voller Größe angezeigt](working-with-computed-columns-cs/_static/image30.png))


Klicken Sie auf ' Fertig stellen ', um den Assistenten abzuschließen.

## <a name="step-6-creating-the-business-logic-layer"></a>Schritt 6: Erstellen der Geschäftslogikebene

Bevor wir eine ASP.NET-Seite erstellen, die berechnete Spalte, die in Schritt 1 erstellten verwendet, müssen wir zunächst die entsprechenden Methoden in der BLL hinzufügen. Unsere ASP.NET-Seite, die wir in Schritt 7 erstellt werden, ermöglicht Benutzern das Anzeigen und Bearbeiten von Lieferanten. Deshalb benötigen wir unsere BLL um mindestens eine Methode zum Abrufen aller Lieferanten und ein anderes zum Aktualisieren eines bestimmten Lieferanten bereitzustellen.

Erstellen Sie eine neue Klassendatei mit dem Namen `SuppliersBLLWithSprocs` in den `~/App_Code/BLL` Ordner, und fügen Sie den folgenden Code hinzu:


[!code-csharp[Main](working-with-computed-columns-cs/samples/sample6.cs)]

Wie die anderen Klassen BLL, `SuppliersBLLWithSprocs` verfügt über eine `protected` `Adapter` -Eigenschaft, die eine Instanz zurückgegeben der `SuppliersTableAdapter` Klasse zusammen mit zwei `public` Methoden: `GetSuppliers` und `UpdateSupplier`. Die `GetSuppliers` Methode aufruft, und gibt die `SuppliersDataTable` zurückgegeben, die mit den entsprechenden `GetSupplier` Methode in der Datenzugriffsebene. Die `UpdateSupplier` Methode ruft Informationen zu den bestimmten Lieferanten, die über einen Aufruf an die DAL s aktualisiert `GetSupplierBySupplierID(supplierID)` Methode. Anschließend aktualisiert der `CategoryName`, `ContactName`, und `ContactTitle` Eigenschaften und führt einen Commit dieser Änderungen auf die Datenbank durch Aufrufen der Datenzugriffsebene s `Update` -Methode auf und übergibt den geänderten `SuppliersRow` Objekt.

> [!NOTE]
> Mit Ausnahme von `SupplierID` und `CompanyName`, können Sie alle Spalten in der Tabelle "Suppliers" `NULL` Werte. Aus diesem Grund Wenn übergebenen `contactName` oder `contactTitle` Parameter sind `null` wir müssen die entsprechenden festlegen `ContactName` und `ContactTitle` Eigenschaften einer `NULL` Datenbank Wert mithilfe der `SetContactNameNull` und `SetContactTitleNull`Methoden bzw.


## <a name="step-7-working-with-the-computed-column-from-the-presentation-layer"></a>Schritt 7: Arbeiten mit der berechneten Spalte aus der Darstellungsschicht

Mit der berechneten Spalte hinzugefügt, die `Suppliers` Tabelle, die DAL und BLL entsprechend aktualisiert, wir können zu eine ASP.NET-Seite zu erstellen, die mit der `FullContactName` berechnete Spalte. Öffnen Sie zunächst die `ComputedColumns.aspx` auf der Seite der `AdvancedDAL` Ordner, und ziehen Sie eine GridView aus der Toolbox in den Designer. Legen Sie die GridView s `ID` Eigenschaft `Suppliers` und aus seinem Smarttag binden Sie es an eine neue ObjectDataSource mit dem Namen `SuppliersDataSource`. Konfigurieren der ObjectDataSource verwenden die `SuppliersBLLWithSprocs` Klasse, die wir hinzugefügt, in Schritt 6 sichern, und klicken Sie auf Weiter.


[![Konfigurieren der ObjectDataSource zur Verwendung der SuppliersBLLWithSprocs-Klasse](working-with-computed-columns-cs/_static/image32.png)](working-with-computed-columns-cs/_static/image31.png)

**Abbildung 11**: Konfigurieren der ObjectDataSource verwenden die `SuppliersBLLWithSprocs` Klasse ([klicken Sie hier, um das Bild in voller Größe angezeigt](working-with-computed-columns-cs/_static/image33.png))


Es gibt nur zwei Methoden definiert, der `SuppliersBLLWithSprocs` Klasse: `GetSuppliers` und `UpdateSupplier`. Stellen Sie sicher, dass diese beiden Methoden werden in die SELECT-Anweisung angegeben bzw. Aktualisieren der Registerkarten, und klicken Sie auf "Fertig stellen", um die Konfiguration der ObjectDataSource abzuschließen.

Nach Abschluss des Assistenten zum Konfigurieren von Datenquellen wird Visual Studio ein BoundField für jede der Datenfelder zurückgegeben hinzufügen. Entfernen der `SupplierID` BoundField und ändern Sie die `HeaderText` Eigenschaften der `CompanyName`, `ContactName`, `ContactTitle`, und `FullContactName` BoundFields Unternehmen, Contact Name, Titel und vollständige Contact Name bzw. Aktivieren Sie das Kontrollkästchen Bearbeiten aktivieren, um die GridView s integrierten Bearbeitungsfunktionen zu aktivieren, aus dem Smarttag.

Zusätzlich zum Hinzufügen von BoundFields an die GridView, Abschluss des Datenquellen-Assistenten auch bewirkt, dass Visual Studio, um das ObjectDataSource-s `OldValuesParameterFormatString` Eigenschaft ursprünglichen\_{0}. Diese Einstellung wieder auf den Standardwert {0} zurückgesetzt werden.

Nach dieser Änderungen an die GridView und ObjectDataSource vorgenommen werden, sollte ihre deklarative Markup etwa wie folgt aussehen:


[!code-aspx[Main](working-with-computed-columns-cs/samples/sample7.aspx)]

Als Nächstes finden Sie auf dieser Seite über einen Browser. Wie Abbildung 12 zeigt, wird der einzelnen Lieferanten aufgeführt, in ein Raster mit den enthält die `FullContactName` Spalte, deren Wert einfach die Verkettung von den anderen drei Spalten ist als formatierte `ContactName` (`ContactTitle`, `CompanyName`).


[![Jeder Lieferant wird im Raster aufgeführt.](working-with-computed-columns-cs/_static/image35.png)](working-with-computed-columns-cs/_static/image34.png)

**Abbildung 12**: jede Lieferanten wird im Raster aufgeführt ([klicken Sie hier, um das Bild in voller Größe angezeigt](working-with-computed-columns-cs/_static/image36.png))


Klicken auf die Schaltfläche "Bearbeiten" für ein bestimmter Lieferanten einen Postback verursacht, und diese Zeile im gerenderten dessen Bearbeitung Schnittstelle (siehe Abbildung 13). Die ersten drei Spalten, wie in der Standardeinstellung Schnittstelle Bearbeiten gerendert – ein TextBox-Steuerelement, dessen `Text` Eigenschaft auf den Wert des Feld "Daten" festgelegt ist. Die `FullContactName` Spalte bleibt jedoch als Text. Bei der BoundFields an die GridView nach dem Abschluss des Assistenten Datenquellenkonfiguration hinzugefügt wurden die `FullContactName` BoundField-s `ReadOnly` -Eigenschaft wurde festgelegt, um `true` da das entsprechende `FullContactName` Spalte in der `SuppliersDataTable` hat ihre `ReadOnly` -Eigenschaftensatz auf `true`. Wie in Schritt 4 notiert die `FullContactName` s `ReadOnly` -Eigenschaft wurde festgelegt, um `true` TableAdapter erkannt, dass die Spalte eine berechnete Spalte war.


[![Die FullContactName-Spalte kann nicht bearbeitet werden](working-with-computed-columns-cs/_static/image38.png)](working-with-computed-columns-cs/_static/image37.png)

**Abbildung 13**: die `FullContactName` Spalte kann nicht bearbeitet werden ([klicken Sie hier, um das Bild in voller Größe angezeigt](working-with-computed-columns-cs/_static/image39.png))


Fahren Sie fort, und aktualisieren Sie den Wert aus einem oder mehreren Spalten bearbeitet werden, und klicken Sie auf aktualisieren. Hinweis wie die `FullContactName` s-Wert wird automatisch aktualisiert, um die Änderung widerzuspiegeln.

> [!NOTE]
> Die GridView verwendet derzeit BoundFields für die Elemente bearbeitet, wodurch der Standardbereich Schnittstelle bearbeiten. Da die `CompanyName` Feld ist erforderlich, sie sollten in ein, die einen RequiredFieldValidator enthält TemplateField konvertiert werden. Ich lassen Sie dieses als Übung für die gewünschten Reader. Wenden Sie sich an den [Validierungssteuerelemente hinzufügen, bearbeiten und Einfügen von Schnittstellen](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) Tutorial schrittweise Anleitung zum Hinzufügen von Steuerelementen für die Validierung und ein BoundField in ein TemplateField konvertiert.


## <a name="summary"></a>Zusammenfassung

Wenn Sie das Schema für eine Tabelle definieren, können Microsoft SQL Server die Einbindung von berechneten Spalten. Dies sind die Spalten, deren Werte aus einem Ausdruck berechnet werden, die die Werte aus anderen Spalten in den gleichen Datensatz in der Regel verweist. Da die Werte, für berechnete Spalten in einem Ausdruck basieren sie sind schreibgeschützt und kann einen Wert in zugewiesen werden ein `INSERT` oder `UPDATE` Anweisung. Dies stellt die Herausforderung dar, wenn eine berechnete Spalte in der Hauptabfrage des TableAdapter verwenden, die zum automatischen Generieren von entsprechenden versucht `INSERT`, `UPDATE`, und `DELETE` Anweisungen.

In diesem Lernprogramm erläutert Techniken für die Herausforderungen, die Bedrohung durch berechnete Spalten umgehen. Insbesondere verwendet es gespeicherte Prozeduren in unserer TableAdapter, um die TableAdapters innewohnende unzulänglichen überwunden werden, die Ad-hoc-SQL-Anweisungen verwenden. Wenn Sie Prozeduren mit dem TableAdapter-Assistenten neu erstellen gespeicherte, ist es wichtig, wir haben die Hauptabfrage anfänglich alle berechneten Spalten auslassen, da deren Vorhandensein verhindert, dass die Datenänderung gespeicherte Prozeduren generiert wird. Nachdem der TableAdapter ursprünglich konfiguriert wurde, dessen `SelectCommand` gespeicherte Prozedur retooled werden kann, um berechnete Spalten enthalten.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurden Hilton Geisenow und Teresa Murphy. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](adding-additional-datatable-columns-cs.md)
> [Weiter](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs.md)
