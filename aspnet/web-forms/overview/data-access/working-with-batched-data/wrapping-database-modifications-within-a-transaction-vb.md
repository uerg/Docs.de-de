---
uid: web-forms/overview/data-access/working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb
title: Umschließen von Datenbankänderungen innerhalb einer Transaktion (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Dieses Tutorial ist der erste von vier, die untersucht aktualisieren, löschen und Einfügen von Batches von Daten. In diesem Tutorial wird erläutert, wie Transaktionen zulassen...
ms.author: riande
ms.date: 06/26/2007
ms.assetid: 7d821db5-6cbb-4b38-af14-198f9155fc82
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb
msc.type: authoredcontent
ms.openlocfilehash: 986baf521bf68b60e9c868f070f31a3aee21db8d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837771"
---
<a name="wrapping-database-modifications-within-a-transaction-vb"></a>Umschließen von Datenbankänderungen innerhalb einer Transaktion (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_63_VB.zip) oder [PDF-Datei herunterladen](wrapping-database-modifications-within-a-transaction-vb/_static/datatutorial63vb1.pdf)

> Dieses Tutorial ist der erste von vier, die untersucht aktualisieren, löschen und Einfügen von Batches von Daten. In diesem Tutorial wird erläutert, wie können Datenbanktransaktionen Änderungen von Batch als atomischen Vorgang, festgelegt Dadurch wird sichergestellt, dass alle Schritte erfolgreich sind oder alle Schritte fehlschlagen.


## <a name="introduction"></a>Einführung

Wie wir gesehen haben, beginnend mit der [eine Übersicht der einfügen, aktualisieren und Löschen von Daten](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) Tutorial GridView bietet integrierte Unterstützung für auf Zeilenebene zu bearbeiten und löschen. Mit wenigen Mausklicks ist es möglich, eine umfassende Änderung-Schnittstelle zu erstellen, ohne eine Codezeile schreiben zu müssen, solange Sie Inhalt mit bearbeiten und Löschen von auf einer Basis pro Zeile sind. Allerdings in bestimmten Szenarien ist dies nicht ausreichend, und wir bieten Benutzern die Möglichkeit zum Bearbeiten oder Löschen von Datensätzen müssen.

Beispielsweise verwenden die meisten webbasierten e-Mail-Clients ein Raster zum Auflisten der jede Nachricht, wobei jede Zeile ein Kontrollkästchen zusammen mit den e-Mail-s-Informationen (Betreff, Absender usw.) enthält. Diese Schnittstelle ermöglicht den Benutzer, mehrere Nachrichten zu löschen, und klicken Sie dann auf eine ausgewählte Nachrichten löschen-Schaltfläche. Eine Schnittstelle für die Batchverarbeitung eignet sich insbesondere in Situationen, in denen Benutzer häufig viele unterschiedliche Datensätze bearbeiten. Statt den Benutzer auf erzwungen bearbeiten, nehmen ihre Änderung vor, und klicken Sie dann die Updates für die einzelnen Datensätze, die geändert werden muss, eine Schnittstelle für die Batchverarbeitung rendert, jede Zeile mit der Bearbeitung-Schnittstelle. Der Benutzer kann den Satz von Zeilen, die geändert werden müssen, schnell zu ändern, und klicken Sie dann diese Änderungen speichern, indem Sie auf eine Schaltfläche "Alle aktualisieren". In diesen Tutorials betrachten wir, wie Sie Schnittstellen für das Einfügen, bearbeiten und Löschen von Batches von Daten zu erstellen.

Beim Ausführen von Batchvorgängen es wichtig, zu bestimmen, ob es möglich sein soll für einige der Vorgänge im Batch erfolgreich ausgeführt werden kann andere while s fehl. Betrachten Sie einen Batch Schnittstelle – was geschehen soll, wenn es sich bei der erste ausgewählte Datensatz wurde erfolgreich gelöscht wird, aber das zweite Argument ein Fehler auftritt, z. B. aufgrund einer Verletzung der foreign Key-Einschränkung löschen? Sollte der erste Datensatz s Löschvorgang rückgängig werden gemacht oder ist es für den ersten Datensatz gelöscht bleibt akzeptabel?

Wenn Sie möchten, dass den Batchvorgang behandelt werden soll, als ein [atomischer Vorgang](http://en.wikipedia.org/wiki/Atomic_operation), einer, in denen entweder alle Schritte erfolgreich ausgeführt werden oder alle Schritte fehlschlagen, dann muss der Datenzugriffsebene erweitert werden, um Unterstützung für die [Datenbank Transaktionen](http://en.wikipedia.org/wiki/Database_transaction). Datenbanktransaktionen garantieren Unteilbarkeit für den Satz von `INSERT`, `UPDATE`, und `DELETE` Anweisungen des wirkungsfelds der Transaktion ausgeführt und sind eine Funktion, die von den meisten Systemen für alle modernen Datenbank unterstützt werden.

In diesem Tutorial betrachten wir die DAL zur Verwendung von Transaktionen zu erweitern. Nachfolgenden Tutorials werden für Batch einfügen, aktualisieren und Löschen von Schnittstellen implementieren Webseiten untersuchen. Lassen Sie s beginnen!

> [!NOTE]
> Beim Ändern von Daten in einer Batchtransaktion ist die Unteilbarkeit nicht immer erforderlich. In einigen Szenarien ist es möglicherweise akzeptabel, Sie haben einige datenänderungen, die erfolgreich ausgeführt werden und andere Benutzer im selben Batch ein Fehler auftritt, z. B. wenn eine Gruppe von e-Mails aus einem webbasierten e-Mail-Client gelöscht. Wenn es s nicht genau eine Datenbank-Fehler durch das Löschen verarbeiten, es s ist möglicherweise akzeptabel, dass dieser Datensätze ohne Fehler verarbeitet gelöschte bleiben. In solchen Fällen muss die DAL nicht geändert werden, um die Datenbanktransaktionen unterstützt. Es gibt andere Batch Vorgang Szenarien, jedoch, in denen Unteilbarkeit unerlässlich ist. Wenn ein Kunde ihr Guthaben von einem Bankkonto auf einen anderen bewegt wird, müssen zwei Vorgänge ausgeführt werden: der Betrag tatsächlich aus dem das erste Konto abgezogen und klicken Sie dann die zweite hinzugefügt werden müssen. Während die Bank kann nicht von Bedeutung der erste Schritt erfolgreich ausgeführt werden müssen, aber der zweite Schritt fehlschlägt, werden seine Kunden verständlicherweise verärgert sein. Sollten Sie dieses Tutorial durcharbeiten und die Verbesserungen an die DAL Datenbanktransaktionen unterstützen, auch wenn Sie nicht beabsichtigen, für deren Verwendung in den Batch einfügen, aktualisieren und Löschen von Schnittstellen, die wir in den folgenden drei Tutorials erstellen, zu implementieren.


## <a name="an-overview-of-transactions"></a>Eine Übersicht über Transaktionen

Die meisten Datenbanken umfassen die Unterstützung für *Transaktionen*, die es ermöglichen, mehrere Datenbankbefehle in eine einzelne logische Arbeitseinheit gruppiert werden. Die Datenbankbefehle, die eine Transaktion bilden werden garantiert atomarisch, d. h., dass alle Befehle fehl schlagen, oder alle erfolgreich.

Im Allgemeinen werden die Transaktionen über SQL-Anweisungen, die mithilfe des folgenden Musters implementiert:

1. Geben Sie an den Anfang einer Transaktion.
2. Führen Sie die SQL-Anweisungen, aus die die Transaktion besteht.
3. Wenn ein Fehler in einer der Anweisungen in Schritt2, Rollback der Transaktion vorhanden ist.
4. Wenn alle Anweisungen aus Schritt2 ohne Fehler abgeschlossen, committen Sie die Transaktion aus.

Die SQL-Anweisungen, die zum Erstellen, einen Commit auszuführen, und ein Rollback die Transaktion kann manuell eingegeben werden, wenn SQL-Skripts schreiben, oder erstellen Sie Prozeduren gespeicherte oder über programmgesteuerte bedeutet, dass mithilfe von ADO.NET oder die Klassen in der [ `System.Transactions` Namespace](https://msdn.microsoft.com/library/system.transactions.aspx). Verwalten von Transaktionen, die mithilfe von ADO.NET, in diesem Lernprogramm nur untersucht werden. In einem späteren Tutorial betrachten wir die Verwendung von gespeicherten Prozeduren in der Datenzugriffsebene, woraufhin wir die SQL-Anweisungen zum Erstellen, Rollback und Ausführen eines Commits für Transaktionen untersucht. In der Zwischenzeit finden Sie in [Verwalten von Transaktionen in gespeicherten Prozeduren von SQL Server](http://www.4guysfromrolla.com/webtech/080305-1.shtml) für Weitere Informationen.

> [!NOTE]
> Die [ `TransactionScope` Klasse](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) in die `System.Transactions` -Namespace ermöglicht es Entwicklern, programmgesteuert eine Reihe von Anweisungen innerhalb des Bereichs einer Transaktion umschließen und bietet Unterstützung für komplexe Transaktionen, bei denen mehrere Quellen, z. B. zwei verschiedenen Datenbanken oder sogar heterogene Arten von Datenspeichern, z.B. Microsoft SQL Server-Datenbank, eine Oracle-Datenbank und einen Webdienst. Ich haben sich entschieden, zur Verwendung von ADO.NET-Transaktionen für dieses Lernprogramm anstelle von der `TransactionScope` Klasse da ADO.NET spezifischere für Datenbanktransaktionen und in vielen Fällen handelt es sich viel weniger ressourcenintensiv ist. Darüber hinaus werden in bestimmten Szenarien der `TransactionScope` Klasse verwendet den Microsoft Distributed Transaction Coordinator (MSDTC). Die Konfiguration, Implementierung und Leistung Probleme umgebenden MSDTC ist es ein Thema stattdessen spezielle und erweiterte und würde den Rahmen dieser Tutorials.


Bei der Arbeit mit dem SqlClient-Anbieter in ADO.NET werden Transaktionen durch einen Aufruf initiiert die [ `SqlConnection` Klasse](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) s [ `BeginTransaction` Methode](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.begintransaction.aspx), gibt eine [ `SqlTransaction` Objekt](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.aspx). Die datenänderungsanweisungen, die Zusammensetzung der Transaktion befinden sich in einem `try...catch` Block. Bei einem in einer Anweisung in Fehler der `try` blockieren, Ausführung Datenübertragungen an den die `catch` Block, in dem Rollback der Transaktion kann werden über, die `SqlTransaction` s-Objekt [ `Rollback` Methode](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.rollback.aspx). Wenn alle Anweisungen, einen Aufruf erfolgreich die `SqlTransaction` s-Objekt [ `Commit` Methode](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.commit.aspx) am Ende der `try` Block ein Commit die Transaktion. Der folgende Codeausschnitt veranschaulicht dieses Muster. Finden Sie unter [Datenbankkonsistenz mit Transaktionen warten](http://aspnet.4guysfromrolla.com/articles/072705-1.aspx) für zusätzliche Syntax und Beispiele für die Verwendung von Transaktionen in ADO.NET.


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample1.vb)]

Standardmäßig verwenden die TableAdapters in einem typisierten DataSet Transaktionen nicht. Um Transaktionen zu unterstützen müssen wir erweitern die TableAdapter-Klassen, um zusätzliche Methoden enthalten, die die oben genannten Muster verwenden, um eine Reihe von Anweisungen zur Datenänderung innerhalb des Bereichs einer Transaktion ausführen. In Schritt2 sehen wir, wie Sie partielle Klassen verwenden, um diese Methoden hinzufügen.

## <a name="step-1-creating-the-working-with-batched-data-web-pages"></a>Schritt 1: Erstellen über das Arbeiten mit Batchdaten Web Pages

Bevor wir beginnen, Gewusst wie: Erweitern Sie die DAL zur Unterstützung von Transaktionen untersuchen, können Sie zuerst einen kurzen Augenblick zum Erstellen von ASP.NET Web Pages, die wir für dieses Tutorial benötigen und die drei, die Folgen s ein. Starten, indem Sie einen neuen Ordner namens hinzufügen `BatchData` und fügen Sie dann die folgenden ASP.NET-Seiten, verknüpfen jede Seite mit den `Site.master` Masterseite.

- `Default.aspx`
- `Transactions.aspx`
- `BatchUpdate.aspx`
- `BatchDelete.aspx`
- `BatchInsert.aspx`


![Fügen Sie die ASP.NET-Seiten für die Lernprogramme SqlDataSource-bezogene hinzu.](wrapping-database-modifications-within-a-transaction-vb/_static/image1.gif)

**Abbildung 1**: die ASP.NET-Seiten für die Lernprogramme SqlDataSource-bezogene hinzufügen


Wie bei den anderen Ordnern `Default.aspx` verwendet die `SectionLevelTutorialListing.ascx` Benutzersteuerelement in den Tutorials in einen Abschnitt aufgelistet. Aus diesem Grund fügen dieses Benutzersteuerelement zu `Default.aspx` durch Ziehen aus dem Projektmappen-Explorer auf die Seite s Entwurfsansicht.


[![Fügen Sie das SectionLevelTutorialListing.ascx-Benutzersteuerelement an "default.aspx"](wrapping-database-modifications-within-a-transaction-vb/_static/image2.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image1.png)

**Abbildung 2**: Hinzufügen der `SectionLevelTutorialListing.ascx` Benutzersteuerelement `Default.aspx` ([klicken Sie, um das Bild in voller Größe anzeigen](wrapping-database-modifications-within-a-transaction-vb/_static/image2.png))


Abschließend fügen Sie diese vier Seiten als Einträge der `Web.sitemap` Datei. Fügen Sie das folgende Markup insbesondere nach dem Anpassen der Sitemap `<siteMapNode>`:


[!code-xml[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample2.xml)]

Nach der Aktualisierung `Web.sitemap`, können Sie die Lernprogramme-Website über einen Browser anzeigen. Klicken Sie im Menü auf der linken Seite enthält jetzt Elemente für das Arbeiten mit batchdaten Tutorials.


![Die Sitemap enthält jetzt die Einträge für das Arbeiten mit Batchdaten-Tutorials](wrapping-database-modifications-within-a-transaction-vb/_static/image3.gif)

**Abbildung 3**: die Sitemap enthält nun Einträge für das Arbeiten mit Batchdaten-Lernprogramme


## <a name="step-2-updating-the-data-access-layer-to-support-database-transactions"></a>Schritt 2: Aktualisieren der Datenzugriffsebene, um Transaktionen zu unterstützen.

Wie im ersten Tutorial erläutert [Erstellen einer Datenzugriffsschicht](../introduction/creating-a-data-access-layer-vb.md), die typisierte DataSet im DAL besteht aus Datentabellen und TableAdapters. Die Datentabellen enthalten Daten, während die TableAdapters, die Funktionalität zum Lesen von Daten aus der Datenbank in der DataTable zum Aktualisieren der Datenbank mit Änderungen an der DataTables und So weiter bereitstellen. Denken Sie daran, dass die TableAdapters bieten zwei Muster zum Aktualisieren von Daten, die ich als Batchaktualisierung und DB-Direct bezeichnet. Mit dem BatchUpdate-Muster wird der TableAdapter ein DataSet, eine DataTable oder eine Auflistung von DataRows übergeben. Diese Daten aufgelistet und für die einzelnen eingefügt, geändert oder gelöschte Zeile, die `InsertCommand`, `UpdateCommand`, oder `DeleteCommand` ausgeführt wird. Mit dem Datenbank-Direct-Muster wird der TableAdapter stattdessen die Werte der Spalten für das Einfügen, aktualisieren oder Löschen eines einzelnen Datensatzes übergeben. DB direkte Pattern-Methode verwendet dann diese übergebenen Werte die entsprechenden auszuführenden `InsertCommand`, `UpdateCommand`, oder `DeleteCommand` Anweisung.

Unabhängig von der updatemuster verwendet verwenden Sie die automatisch generierten TableAdapter-Methoden nicht Transaktionen. Standardmäßig wird jede INSERT-, Update- oder Delete, die von der TableAdapter ausgeführt als diskrete einzelner Vorgang behandelt. Stellen Sie sich beispielsweise vor, dass das DB-Direct-Muster von Code in die Geschäftslogikschicht, zum Einfügen von zehn Datensätze in der Datenbank verwendet wird. Dieser Code würde die TableAdapter aufgerufen `Insert` zehnmal-Methode. Wenn die ersten fünf einfügungen erfolgreich, aber einen sechsten Auftrag, die zu einer Ausnahme geführt haben, würden die ersten fünf eingefügten Datensätze in der Datenbank verbleiben. Wenn das BatchUpdate-Muster verwendet wird, zur Durchführung von einfügungen, Updates und löschungen an der eingefügten, geändert, und die gelöschten Zeilen in einer "DataTable", wenn die erste mehrere Änderungen war erfolgreich, aber eine höhere Version hat einen Fehler, die diese früheren Änderungen festgestellt, abgeschlossen in der Datenbank bleibt.

In bestimmten Szenarien möchten wir über eine Reihe von Änderungen Unteilbarkeit sicherzustellen. Um dies zu erreichen, wir TableAdapter manuell erweitern müssen, neue Methoden, die ausgeführt werden, die `InsertCommand`, `UpdateCommand`, und `DeleteCommand` s, unter dem Dach einer Transaktion. In [Erstellen einer Datenzugriffsschicht](../introduction/creating-a-data-access-layer-vb.md) erläutert, mit [Teilklassen](http://en.wikipedia.org/wiki/Partial_type) zum Erweitern der Funktionalität von vorhandenen DataTables innerhalb des typisierten Datasets. Diese Technik kann auch mit TableAdapters verwendet werden.

Das typisierte DataSet `Northwind.xsd` befindet sich in der `App_Code` Ordner s `DAL` Unterordner. Erstellen Sie einen Unterordner in der `DAL` Ordner mit dem Namen `TransactionSupport` und fügen Sie eine neue Klassendatei mit dem Namen `ProductsTableAdapter.TransactionSupport.vb` (siehe Abbildung 4). Diese Datei enthält die partielle Implementierung der `ProductsTableAdapter` , die Funktionen zur Durchführung von datenänderungen, die mithilfe einer Transaktion enthält.


![Fügen Sie einen Ordner namens TransactionSupport und eine Klassendatei mit dem Namen ProductsTableAdapter.TransactionSupport.vb hinzu](wrapping-database-modifications-within-a-transaction-vb/_static/image4.gif)

**Abbildung 4**: Fügen Sie einen Ordner mit dem Namen `TransactionSupport` und eine Klassendatei mit dem Namen `ProductsTableAdapter.TransactionSupport.vb`


Geben Sie den folgenden Code in die `ProductsTableAdapter.TransactionSupport.vb` Datei:


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample3.vb)]

Die `Partial` -Schlüsselwort in der Klassendeklaration hier gibt an, an den Compiler an, die die Elemente hinzugefügt werden, hinzugefügt werden die `ProductsTableAdapter` -Klasse in der `NorthwindTableAdapters` Namespace. Beachten Sie die `Imports System.Data.SqlClient` -Anweisung am Anfang der Datei. Da der TableAdapter für die Verwendung des SqlClient-Anbieters konfiguriert wurde, intern verwendet eine [ `SqlDataAdapter` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqldataadapter.aspx) Objekt, das die Befehle in der Datenbank. Daher müssen wir verwenden die `SqlTransaction` Klasse, um die Transaktion zu beginnen, und führen Sie einen commit oder Rollback für ihn dann. Wenn Sie einen anderen Datenspeicher als Microsoft SQL Server verwenden, müssen Sie den entsprechenden Anbieter verwenden.

Diese Methoden geben Sie die Bausteine, die erforderlich sind, Rollback- und commit einer Transaktion. Markiert sind `Public`, aktivieren sie aus verwendet werden die `ProductsTableAdapter`, von einer anderen Klasse in der DAL oder aus einer anderen Ebene in der Architektur, wie z. B. die BLL. `BeginTransaction` Öffnet die internen TableAdapter `SqlConnection` (falls erforderlich), startet die Transaktion, und weist sie der `Transaction` -Eigenschaft und fügt die Transaktion an den internen `SqlDataAdapter` s `SqlCommand` Objekte. `CommitTransaction` und `RollbackTransaction` rufen Sie die `Transaction` s-Objekt `Commit` und `Rollback` Methoden bzw. vor dem Schließen der internen `Connection` Objekt.

## <a name="step-3-adding-methods-to-update-and-delete-data-under-the-umbrella-of-a-transaction"></a>Schritt 3: Hinzufügen von Methoden zum Aktualisieren und Löschen von Daten unter dem Dach einer Transaktion

Mit diesen Methoden abgeschlossen ist, können wir re-Methoden hinzufügen `ProductsDataTable` oder die BLL, die eine Reihe von Befehlen unter dem Dach einer Transaktion ausführen. Die folgende Methode verwendet das Muster im Batch zu aktualisieren, aktualisieren Sie eine `ProductsDataTable` -Instanz mithilfe einer Transaktion. Es startet eine Transaktion, durch den Aufruf der `BeginTransaction` -Methode, und dann verwendet, eine `Try...Catch` Block, um die Anweisungen zur Datenänderung ausgegeben. Wenn der Aufruf der `Adapter` s-Objekt `Update` Methode führt zu einer Ausnahme, die Ausführung wird zum Übertragen der `catch` Block, in dem die Transaktion wird ein Rollback, und die Ausnahme erneut ausgelöst. Bedenken Sie, dass die `Update` Methode implementiert das Muster Batchaktualisierung durch Auflisten der Zeilen der angegebenen `ProductsDataTable` und ausführen, die die erforderlichen `InsertCommand`, `UpdateCommand`, und `DeleteCommand` s. Wenn eine der folgenden Befehle führt zu einem Fehler, wird die Transaktion zurückgesetzt rückgängig machen die vorherigen Änderungen, die während der Lebensdauer von Transaktionen s vorgenommen wurden. Sollte die `Update` Anweisung ohne Fehler abgeschlossen ist, wird die Transaktion ein Commit in seiner Gesamtheit ausgeführt.


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample4.vb)]

Hinzufügen der `UpdateWithTransaction` Methode, um die `ProductsTableAdapter` Klasse über die partielle Klasse in `ProductsTableAdapter.TransactionSupport.vb`. Alternativ konnte diese Methode hinzugefügt werden, der Business Logic Layer s `ProductsBLL` Klasse mit ein paar kleinen syntaktische Änderungen. Nämlich das Schlüsselwort `Me` in `Me.BeginTransaction()`, `Me.CommitTransaction()`, und `Me.RollbackTransaction()` durch ersetzt werden müsste `Adapter` (zur Erinnerung: `Adapter` ist der Name einer Eigenschaft in `ProductsBLL` des Typs `ProductsTableAdapter`).

Die `UpdateWithTransaction` -Methode verwendet das BatchUpdate-Muster, jedoch eine Reihe von DB-Direct-Aufrufe kann innerhalb des Bereichs einer Transaktion, wie die folgende Methode zeigt auch verwendet werden. Die `DeleteProductsWithTransaction` -Methode akzeptiert als Eingabe eine `List(Of T)` des Typs `Integer`, die die `ProductID` s, die gelöscht. Die Methode startet die Transaktion durch einen Aufruf von `BeginTransaction` und dann auf die `Try` blockieren, iteriert durch die angegebene Liste mit dem Aufrufen des DB-Direct-Musters `Delete` Methode für die einzelnen `ProductID` Wert. Wenn die Aufrufe an `Delete` ein Fehler auftritt, wird die Steuerung an die `Catch` Block, in dem die Transaktion ein Rollback, und die Ausnahme erneut ausgelöst. Wenn alle Aufrufe `Delete` erfolgreich ausgeführt werden, und klicken Sie dann die Transaktion ein Commit ausgeführt wird. Fügen Sie diese Methode, um die `ProductsBLL` Klasse.


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample5.vb)]

## <a name="applying-transactions-across-multiple-tableadapters"></a>Anwenden von Transaktionen über mehrere TableAdapters

Der Code im transaktionsbezogenen in diesem Tutorial kann für mehrere Anweisungen für die `ProductsTableAdapter` , als atomischen Vorgang behandelt werden soll. Doch was geschieht, wenn mehrere Änderungen an anderen Datenbanktabellen unteilbar durchgeführt werden müssen? Beispielsweise kann beim Löschen einer Kategorie wir zuerst seine aktuelle Produkte einige andere Kategorie neu zuweisen möchten. Diese beiden Schritte Neuzuweisen die Produkte aus, und löschen die Kategorie sollte als atomarer Vorgang ausgeführt werden. Aber die `ProductsTableAdapter` enthält nur die Methoden zum Ändern der `Products` Tabelle und die `CategoriesTableAdapter` enthält nur die Methoden zum Ändern der `Categories` Tabelle. Wie kann eine Transaktion sowohl TableAdapters umfassen?

Eine Möglichkeit besteht darin, eine Methode zum Hinzufügen der `CategoriesTableAdapter` mit dem Namen `DeleteCategoryAndReassignProducts(categoryIDtoDelete, reassignToCategoryID)` und diese Methode, die eine gespeicherte Prozedur aufrufen, die die Produkte weist und löscht die Kategorie innerhalb des Bereichs einer Transaktion, die in der gespeicherten Prozedur definiert haben. Betrachten wir wie begin, commit und von rollbacktransaktionen in gespeicherten Prozeduren in einem späteren Tutorial.

Eine weitere Möglichkeit ist eine Hilfsklasse in der DAL zu erstellen, enthält die `DeleteCategoryAndReassignProducts(categoryIDtoDelete, reassignToCategoryID)` Methode. Diese Methode wird eine Instanz erstellen die `CategoriesTableAdapter` und `ProductsTableAdapter` und legen Sie diese beiden TableAdapters `Connection` Eigenschaften auf die gleiche `SqlConnection` Instanz. An diesen Punkt, eines der zwei TableAdapters würde die Transaktion mit einem Aufruf von initiieren `BeginTransaction`. Die TableAdapter-Methoden zum Neuzuweisen die Produkte aus, und löschen die Kategorie würde aufgerufen werden, einem `Try...Catch` Block mit der Transaktion einen Commit oder Rollback zurück, je nach Bedarf.

## <a name="step-4-adding-theupdatewithtransactionmethod-to-the-business-logic-layer"></a>Schritt 4: Hinzufügen der`UpdateWithTransaction`Methode, um die Geschäftslogikebene

In Schritt 3, die wir hinzugefügt ein `UpdateWithTransaction` Methode, um die `ProductsTableAdapter` in der DAL. Es sollte eine entsprechende Methode der BLL hinzugefügt werden. Während die Darstellungsschicht direkt auf die DAL aufzurufende aufrufen könnten die `UpdateWithTransaction` -Methode in diesen Tutorials haben seit langem angestrebt, eine mehrschichtigen Architektur zu definieren, die die DAL auf der Darstellungsschicht isoliert. Aus diesem Grund modelliere es uns, um diesen Ansatz, den Vorgang fortzusetzen.

Öffnen der `ProductsBLL` Klassendatei, und fügen Sie eine Methode mit dem Namen `UpdateWithTransaction` , ruft einfach auf die entsprechende DAL-Methode. Es sollten jetzt zwei neue Methoden in sein `ProductsBLL`: `UpdateWithTransaction`, die Sie gerade hinzugefügt haben, und `DeleteProductsWithTransaction`, der in Schritt 3 hinzugefügt wurde.


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample6.vb)]

> [!NOTE]
> Diese Methoden enthalten nicht die `DataObjectMethodAttribute` Attribut zugewiesen werden, um die meisten anderen Methoden in der `ProductsBLL` Klasse, da wir diese Methoden direkt in die CodeBehind-Klassen in ASP.NET Seiten aufgerufen werden. Zur Erinnerung: `DataObjectMethodAttribute` wird verwendet, um zu kennzeichnen, welche Methoden in der "ObjectDataSource"-s Datenquelle konfigurieren, Assistenten und auf welcher Registerkarte (SELECT, UPDATE, INSERT oder DELETE) angezeigt werden soll. Da die GridView verfügt nicht über keine integrierte Unterstützung für Batch bearbeiten oder löschen, müssen wir diese Methoden programmgesteuert aufrufen, anstatt die codefreie deklarativen Ansatz zu verwenden.


## <a name="step-5-atomically-updating-database-data-from-the-presentation-layer"></a>Schritt 5: Aktualisieren von Datenbankdaten von der Darstellungsschicht atomar

Um die Auswirkungen zu veranschaulichen, die die Transaktion wurde beim Aktualisieren von Datensätzen, Let s erstellen Sie eine Benutzeroberfläche, die Listet alle Produkte in einer GridView-Ansicht enthält eine Schaltfläche "Web steuern, die beim Klicken auf die Produkte weist `CategoryID` Werte. Insbesondere die Kategorie-neuzuweisung fortgesetzt wird, damit die erste mehrere Produkte eine gültige zugewiesen sind `CategoryID` Wert, während andere bewusst sind zugewiesen, eine nicht vorhandene `CategoryID` Wert. Wenn wir versuchen, die Datenbank mit einem Produkt zu aktualisieren, deren `CategoryID` entspricht keine vorhandene Kategorie s `CategoryID`, eine Verletzung der foreign Key-Einschränkung erfolgt, und eine Ausnahme ausgelöst. In diesem Beispiel sehen werden, wenn Sie mithilfe einer Transaktion aus der Verletzungen von foreign Key-Einschränkung wird die Ausnahme dem vorherigen führt gültige `CategoryID` Änderungen ein Rollback ausgeführt werden. Wenn eine Transaktion nicht verwendet werden soll, bleibt jedoch die Änderungen an den ersten Kategorien.

Öffnen Sie zunächst die `Transactions.aspx` auf der Seite die `BatchData` Ordner, und ziehen Sie einer GridView-Ansicht aus der Toolbox in den Designer. Legen Sie dessen `ID` zu `Products` und von sein Smarttag, binden Sie es an eine neue, mit dem Namen "ObjectDataSource" `ProductsDataSource`. Konfigurieren Sie zum Abrufen der Daten aus dem ObjectDataSource-Steuerelement die `ProductsBLL` Klasse s `GetProducts` Methode. Einer GridView-Ansicht schreibgeschützt sein wird, also legen Sie die Dropdownlisten in der Update-, INSERT- und Registerkarten (keine) löschen und klicken Sie auf "Fertig stellen".


[![Konfigurieren von dem ObjectDataSource-Steuerelement zur Verwendung der ProductsBLL Klasse s GetProducts-Methode](wrapping-database-modifications-within-a-transaction-vb/_static/image5.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image3.png)

**Abbildung 5**: Konfigurieren von dem ObjectDataSource-Steuerelement verwenden die `ProductsBLL` s-Klasse `GetProducts` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](wrapping-database-modifications-within-a-transaction-vb/_static/image4.png))


[![Legen Sie die Dropdownlisten in der Update-, INSERT- und DELETE werden Registerkarten (keine)](wrapping-database-modifications-within-a-transaction-vb/_static/image6.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image5.png)

**Abbildung 6**: Legen Sie die Dropdownlisten in der Update-, INSERT- und Löschen von Registerkarten auf (keine) ([klicken Sie, um das Bild in voller Größe anzeigen](wrapping-database-modifications-within-a-transaction-vb/_static/image6.png))


Nach Abschluss der Konfigurieren von Datenquellen-Assistenten erstellt Visual Studio BoundFields und eine CheckBoxField für die Product-Datenfelder. Entfernen Sie alle diese Felder mit Ausnahme von `ProductID`, `ProductName`, `CategoryID`, und `CategoryName` , und benennen Sie die `ProductName` und `CategoryName` BoundFields `HeaderText` Eigenschaften zum Produkt und die Kategorie, bzw. Überprüfen Sie aus dem Smarttag die Option Paging aktivieren. Nachdem diese Änderungen vorgenommen wurden, sollte die GridView und "ObjectDataSource" s deklarative Markup wie folgt aussehen:


[!code-aspx[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample7.aspx)]

Als Nächstes fügen Sie drei Schaltfläche Websteuerelemente über GridView hinzu. Legen Sie die Schaltfläche "erste" s-Text-Eigenschaft, auf Raster aktualisieren, die zweite s zu ändern, Kategorien (mit Transaktionen) und die Dritter noch s zu ändern, Kategorien (ohne Transaktion).


[!code-aspx[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample8.aspx)]

An diesem Punkt sollte die Entwurfsansicht in Visual Studio ähnlich wie im Screenshot in Abbildung 7 dargestellt aussehen.


[![Die Seite enthält eine GridView und drei Web-Steuerelemente](wrapping-database-modifications-within-a-transaction-vb/_static/image7.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image7.png)

**Abbildung 7**: die Seite enthält eine GridView und drei Web-Steuerelemente ([klicken Sie, um das Bild in voller Größe anzeigen](wrapping-database-modifications-within-a-transaction-vb/_static/image8.png))


Erstellen von Ereignishandlern für jede Schaltfläche mit den drei s `Click` Ereignisse und verwenden Sie den folgenden Code:


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample9.vb)]

Der Aktualisierung der Schaltfläche "s" `Click` Ereignishandler bindet die Daten an die GridView einfach durch Aufrufen der `Products` GridView s `DataBind` Methode.

Der zweite Ereignishandler weist die Produkte `CategoryID` s und verwendet die neue Transaktion-Methode der BLL, führen Sie die Datenbank aktualisiert wird, unter dem Dach einer Transaktion. Beachten Sie, dass jedes Produkt s `CategoryID` nach dem Zufallsprinzip festgelegt ist, auf den gleichen Wert wie die `ProductID`. Funktioniert dies hervorragend für die erste einige Produkte, da diese Produkte haben `ProductID` Werte, die auftreten, um gültige zuzuordnen `CategoryID` s. Jedoch einmal der `ProductID` s Start zu groß, diese zufällig Überschneidung von `ProductID` s und `CategoryID` s nicht mehr angewendet wird.

Die dritte `Click` -Ereignishandler aktualisiert die Produkte `CategoryID` s auf die gleiche Weise, sendet das Update aber in die Datenbank mithilfe der `ProductsTableAdapter` s-Standard `Update` Methode. Dies `Update` Methode die Reihe von Befehlen in einer Transaktion wird nicht umbrochen, damit diese Änderungen vorgenommen werden, bevor der erste aufgetretene foreign Key-Einschränkung Verstoß gegen Fehler bleiben.

Um dieses Verhalten zu veranschaulichen, finden Sie auf dieser Seite über einen Browser. Zunächst sehen Sie die erste Seite der Daten, wie in Abbildung 8 gezeigt. Klicken Sie anschließend die Schaltfläche ändern Kategorien (mit der Transaktion). Dies dazu führen, dass einen Postback und versucht, alle Produkte aktualisieren `CategoryID` Werte annehmen, aber führt zu einer Verletzung der foreign Key-Einschränkung (siehe Abbildung 9).


[![Die Produkte werden in einer navigierbaren GridView-Ansicht angezeigt.](wrapping-database-modifications-within-a-transaction-vb/_static/image8.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image9.png)

**Abbildung 8**: der Produkte in einer navigierbaren GridView-Ansicht angezeigt werden ([klicken Sie, um das Bild in voller Größe anzeigen](wrapping-database-modifications-within-a-transaction-vb/_static/image10.png))


[![Erneutes Zuweisen von den Ergebnissen Kategorien zu einer Verletzung der Foreign Key-Einschränkung](wrapping-database-modifications-within-a-transaction-vb/_static/image9.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image11.png)

**Abbildung 9**: Erneutes Zuweisen von den Ergebnissen der Kategorien in der Verletzung einer Foreign Key-Einschränkung ([klicken Sie, um das Bild in voller Größe anzeigen](wrapping-database-modifications-within-a-transaction-vb/_static/image12.png))


Drücken Sie jetzt Ihre Browser-s-zurück-Taste, und klicken Sie dann auf die Schaltfläche "Raster aktualisieren". Nach dem Aktualisieren der das sehen Sie genaue dieselbe Ausgabe wie in Abbildung 8 gezeigt. Also auch wenn einige der Produkte `CategoryID` s wurden geändert in zulässigen Werte und in der Datenbank aktualisiert, sie wurden zurückgesetzt, wenn die foreign Key-Einschränkung verletzt.

Versuchen Sie nun auf die Schaltfläche ändern Kategorien (ohne Transaktion). Dies führt zu dem Fehler aufgrund einer einschränkungsverletzung gleichen foreign Key-Einschränkung (siehe Abbildung 9), aber dieses Mal dieser Produkte, deren `CategoryID` Werte wurden geändert, um die zulässigen Wert wird nicht zurückgesetzt werden. Drücken Sie Ihre Browser-s-zurück-Taste und dann auf die Schaltfläche mit den Raster zu aktualisieren. Wie in Abbildung 10 gezeigt, die `CategoryID` s der ersten acht Produkte haben, wurde neu zugewiesen. In Abbildung 8 mussten Chang z. B. eine `CategoryID` von 1, aber in Abbildung 10 It s, 2 zugewiesen wurde.


[![Einige Produkte CategoryID Werte wurden aktualisiert, während andere wurden nicht](wrapping-database-modifications-within-a-transaction-vb/_static/image10.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image13.png)

**Abbildung 10**: Einige Produkte `CategoryID` Werte wurden aktualisiert, während andere Benutzer wurden keine ([klicken Sie, um das Bild in voller Größe anzeigen](wrapping-database-modifications-within-a-transaction-vb/_static/image14.png))


## <a name="summary"></a>Zusammenfassung

In der Standardeinstellung die TableAdapter-Methoden s den ausgeführten Database-Anweisungen nicht innerhalb des Bereichs einer Transaktion umbrechen, aber mit ein wenig Aufwand können wir die Methoden, die erstellt werden, Commit und Rollback einer Transaktion hinzufügen. In diesem Tutorial erstellten wir drei Methoden in der `ProductsTableAdapter` Klasse: `BeginTransaction`, `CommitTransaction`, und `RollbackTransaction`. Erläutert, wie diese Methoden zusammen mit einem `Try...Catch` Block, um eine Reihe von Anweisungen zur Datenänderung atomar sind. Vor allem auf die wir erstellt haben die `UpdateWithTransaction` -Methode in der die `ProductsTableAdapter`, die mithilfe des Musters Batchaktualisierung führen Sie die notwendigen Änderungen an die Zeilen eines bereitgestellten `ProductsDataTable`. Wir auch hinzugefügt. die `DeleteProductsWithTransaction` Methode, um die `ProductsBLL` Klasse in die BLL, die akzeptiert eine `List` von `ProductID` Werte als Eingabe und ruft die DB-Direct-Pattern-Methode `Delete` für jede `ProductID`. Beide Methoden zu starten, indem eine Transaktion erstellt und dann die Anweisungen zur Datenänderung in einer `Try...Catch` Block. Wenn eine Ausnahme auftritt, wird ein Rollback für die Transaktion, andernfalls ist es ein Commit ausgeführt.

Schritt 5 veranschaulicht die Auswirkungen der transaktionalen Batch-Updates im Vergleich zu BatchUpdates, die nicht zurückgegeben hat, eine Transaktion zu verwenden. In den nächsten drei Tutorials wir bauen auf der Foundation, die in diesem Tutorial das Layout und Erstellen von Benutzeroberflächen für die Durchführung von Batch-Updates, löschungen und einfügungen.

Viel Spaß beim Programmieren!

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Tutorial erläutert finden Sie in den folgenden Ressourcen:

- [Aufrechterhaltung der Datenbankkonsistenz mit Transaktionen](http://aspnet.4guysfromrolla.com/articles/072705-1.aspx)
- [Verwalten von Transaktionen in SQLServer gespeicherte Prozeduren](http://www.4guysfromrolla.com/webtech/080305-1.shtml)
- [Transaktionen, die leicht gemacht: `System.Transactions`](https://blogs.msdn.com/florinlazar/archive/2004/07/23/192239.aspx)
- [TransactionScope und "DataAdapters"](http://andyclymer.blogspot.com/2007/01/transactionscope-and-dataadapters.html)
- [Mithilfe von Oracle-Datenbank-Transaktionen in .NET](http://www.oracle.com/technology/pub/articles/price_dbtrans_dotnet.html)

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial wurden Hilton Giesenow, Dave Gardner und Teresa Murphy. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](batch-inserting-cs.md)
> [Weiter](batch-updating-vb.md)
