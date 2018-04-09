---
uid: web-forms/overview/data-access/working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs
title: Umbruch Datenbankänderungen innerhalb einer Transaktion (c#) | Microsoft Docs
author: rick-anderson
description: Dieses Lernprogramm ist die erste von vier, die prüft, aktualisieren, löschen und Einfügen von Batches von Daten. In diesem Lernprogramm lernen wir an, wie Datenbanktransaktionen zulassen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: b45fede3-c53a-4ea1-824b-20200808dbae
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs
msc.type: authoredcontent
ms.openlocfilehash: a3f8ec2de7b9259e4bb83f4346bde8abfd643fb4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="wrapping-database-modifications-within-a-transaction-c"></a>Umbruch Datenbankänderungen innerhalb einer Transaktion (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Herunterladen von Code](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_63_CS.zip) oder [PDF herunterladen](wrapping-database-modifications-within-a-transaction-cs/_static/datatutorial63cs1.pdf)

> Dieses Lernprogramm ist die erste von vier, die prüft, aktualisieren, löschen und Einfügen von Batches von Daten. In diesem Lernprogramm lernen wir an, wie Datenbanktransaktionen ermöglichen es Batch Änderungen einer atomaren Operation durchgeführt wird, der sicherstellt, dass entweder alle Schritte erfolgreich sind oder alle Schritte fehlschlagen.


## <a name="introduction"></a>Einführung

Wie wir gesehen haben, beginnend mit der [eine Übersicht der einfügen, aktualisieren und Löschen von Daten](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) Lernprogramm GridView bietet integrierte Unterstützung für auf Zeilenebene zu bearbeiten und löschen. Mit wenigen Mausklicks ist es möglich, eine große Datenmenge Änderung Schnittstelle zu erstellen, ohne eine Codezeile schreiben zu müssen, so lange Sie Inhalt mit bearbeiten und Löschen von regelmäßig pro Zeile sind. Allerdings in bestimmten Szenarien ist dies nicht ausreichend, und wir Benutzern die Berechtigung zum Bearbeiten oder Löschen eines Batches von Datensätzen bereitstellen müssen.

Beispielsweise verwenden die meisten webbasierte e-Mail-Clients ein Raster jede Nachricht aufgelistet, wobei jede Zeile ein Kontrollkästchen zusammen mit der e-Mail-s-Informationen (Betreff, Sender usw.) enthält. Diese Schnittstelle ermöglicht den Benutzer mehrere Nachrichten löschen, indem sie einchecken und dann auf eine Schaltfläche ausgewählten Nachrichten löschen. Eine Schnittstelle für die Batchverarbeitung eignet sich in Situationen, in denen Benutzer häufig viele unterschiedliche Datensätze bearbeiten. Anstatt auf den Benutzer dazu zwingen bearbeiten, deren Änderung vorzunehmen, und klicken Sie dann die Updates für jeden Datensatz, der geändert werden muss, eine Schnittstelle für die Batchverarbeitung rendert jeder Zeile mit dessen Bearbeitung-Oberfläche. Der Benutzer kann den Satz von Zeilen, die geändert werden müssen, schnell zu ändern und speichern Sie diese Änderungen durch Klicken auf eine Schaltfläche Alle aktualisieren. In diesen Lernprogrammen untersuchen wir zum Erstellen von Schnittstellen für das Einfügen, bearbeiten und Löschen von Batches von Daten.

Beim Ausführen von Batchvorgängen es wichtig zu bestimmen, ob es möglich sein soll für einige der Vorgänge im Batch erfolgreich andere dagegen s fehl. Betrachten Sie einen Batch, die Schnittstelle – was geschehen soll, wenn es sich bei der erste ausgewählte Datensatz erfolgreich gelöscht wurde, aber das zweite Argument ein Fehler auftritt, z. B. aufgrund einer Verletzung der foreign Key-Einschränkung löschen? Sollte der erste Datensatz s Löschvorgang rückgängig werden gemacht oder ist es für den ersten Datensatz gelöschten bleiben akzeptabel?

Gegebenenfalls den Batchvorgang behandelt werden, als ein [atomaren Vorgang](http://en.wikipedia.org/wiki/Atomic_operation), einer, in denen entweder alle Schritte erfolgreich ausgeführt werden oder alle Schritte fehlschlagen, dann muss der Datenzugriffsebene erweitert werden, dass die Unterstützung für [Datenbank Transaktionen](http://en.wikipedia.org/wiki/Database_transaction). Datenbanktransaktionen garantieren Unteilbarkeit von Transaktionen für den Satz von `INSERT`, `UPDATE`, und `DELETE` Anweisungen des wirkungsfelds der Transaktion ausgeführt und sind eine Funktion, die von den meisten alle modernen Datenbanksystemen unterstützt.

In diesem Lernprogramm sehen wir uns die DAL Verwenden von Datenbanktransaktionen zu erweitern. Nachfolgende Lernprogrammen untersucht implementierende Webseiten für Batch einfügen, aktualisieren und Löschen von Schnittstellen. Lassen Sie s beginnen!

> [!NOTE]
> Beim Ändern von Daten in einer Batchtransaktion ist Unteilbarkeit nicht immer erforderlich. In einigen Szenarien ist es möglicherweise zulässig, einige datenänderungen erfolgreich ausgeführt werden und andere Benutzer im selben Batch ein Fehler auftritt, z. B. wenn eine Reihe von e-Mail-Nachrichten über ein webbasiertes e-Mail-Client löschen. Wenn es s nicht genau eine Datenbank-Fehler durch das Löschen verarbeiten, es s wahrscheinlich akzeptabel, dass diese ohne Fehler verarbeiteten Datensätze gelöschten bleiben. In solchen Fällen muss die DAL nicht geändert werden, um Datenbanktransaktionen unterstützen. Es gibt andere Batch Vorgang Szenarien, jedoch, in denen Unteilbarkeit unabdingbar ist. Wenn ein Kunde ihr Guthaben von einem Bankkonto auf einen anderen verschoben wird, müssen zwei Vorgänge ausgeführt werden: die Überweisung müssen das erste Konto abgezogen und anschließend das zweite hinzugefügt. Während die Bank nichts ausmacht kann, dass der erste Schritt erfolgreich ausgeführt werden, aber der zweite Schritt fehl, wäre die Kunden verständlicherweise verärgert. Ich empfehlen Ihnen, dieses Lernprogramm zu absolvieren, und implementieren die Erweiterungen für die DAL Datenbanktransaktionen unterstützen, auch wenn Sie nicht beabsichtigen, auf die Verwendung dieser Batch einfügen, aktualisieren und Löschen von Schnittstellen, die in den folgenden drei Lernprogrammen erstellen müssen.


## <a name="an-overview-of-transactions"></a>Eine Übersicht über Transaktionen

Die meisten Datenbanken enthalten die Unterstützung für *Transaktionen*, ermöglichen die mehrerer Datenbankbefehle in eine einzelne logische Arbeitseinheit gruppiert werden. Die Datenbankbefehle, die eine Transaktion bilden sind unbedingt atomarisch, d. h., dass entweder alle Befehle fehl schlagen, oder alle funktionieren.

Im Allgemeinen werden Transaktionen über SQL-Anweisungen, die mithilfe des folgenden Musters implementiert:

1. Geben Sie an den Anfang einer Transaktion.
2. Führen Sie die SQL-Anweisungen, die die Transaktion zu bilden.
3. Wenn ein Fehler in einer der Anweisungen aus Schritt2, Rollback der Transaktion vorhanden ist.
4. Wenn alle Anweisungen aus Schritt2 ohne Fehler abgeschlossen werden, ein commit der Transaktions.

Die SQL-Anweisungen, die zum Erstellen, einen Commit auszuführen, und Rollback die Transaktion kann manuell eingegeben werden, wenn SQL-Skripts schreiben, oder Erstellen von Prozeduren gespeicherten oder über programmgesteuerte bedeutet, dass mithilfe von ADO.NET oder die Klassen in der [ `System.Transactions` Namespace](https://msdn.microsoft.com/library/system.transactions.aspx). Verwalten von Transaktionen, die mithilfe von ADO.NET, in diesem Lernprogramm nur untersucht werden. In einem späteren Lernprogramm betrachten wir Verwendung von gespeicherten Prozeduren in der Datenzugriffsebene zu diesem Zeitpunkt die SQL-Anweisungen zum Erstellen, Rollback und Commit für Transaktionen untersucht werden. Wenden Sie sich in der Zwischenzeit an [Verwalten von Transaktionen in gespeicherten Prozeduren von SQL Server](http://www.4guysfromrolla.com/webtech/080305-1.shtml) für Weitere Informationen.

> [!NOTE]
> Die [ `TransactionScope` Klasse](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) in die `System.Transactions` Namespace ermöglicht es Entwicklern, programmgesteuert eine Reihe von Anweisungen innerhalb des Bereichs einer Transaktion umschließen und bietet Unterstützung für komplexe Transaktionen, die mehrere einschließen Quellen, z. B. zwei verschiedenen Datenbanken oder sogar heterogene Arten von Datenspeichern, wie z. B. Microsoft SQL Server-Datenbank, einer Oracle-Datenbank und einen Webdienst. Ich haben sich entschieden, zur Verwendung von ADO.NET Transaktionen für dieses Lernprogramm anstelle von der `TransactionScope` Klasse da ADO.NET finden Sie spezifischere für Datenbanktransaktionen und in vielen Fällen ist viel weniger ressourcenintensiv ist. Darüber hinaus werden in bestimmten Szenarien die `TransactionScope` Klasse verwendet den Microsoft Distributed Transaction Coordinator (MSDTC). Die Konfiguration, Implementierung und Leistung Probleme umgebenden MSDTC erleichtert ein Thema vielmehr spezialisierten und erweiterte und Gegenstand mit diesen Lernprogrammen.


Bei der Arbeit mit dem SqlClient-Anbieter in ADO.NET werden Transaktionen gestartet, durch einen Aufruf der [ `SqlConnection` Klasse](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) s [ `BeginTransaction` Methode](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.begintransaction.aspx), welche gibt eine [ `SqlTransaction` Objekt](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.aspx). Die datenänderungsanweisungen, die Zusammensetzung der Transaktion befinden sich innerhalb einer `try...catch` Block. In einer Anweisung im Falle ein Fehlers die `try` blockieren, Ausführung übertragungsflusses, um die `catch` Block, in dem Rollback der Transaktion kann werden über, die `SqlTransaction` s-Objekt [ `Rollback` Methode](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.rollback.aspx). Wenn alle Anweisungen erfolgreich einen Aufruf abgeschlossen der `SqlTransaction` s-Objekt [ `Commit` Methode](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.commit.aspx) am Ende der `try` Block ein Commit die Transaktion. Der folgende Codeausschnitt veranschaulicht dieses Muster. Finden Sie unter [Datenbankkonsistenz mit Transaktionen warten](http://aspnet.4guysfromrolla.com/articles/072705-1.aspx) für zusätzliche Syntax und Beispiele für die Verwendung von Transaktionen in ADO.NET.


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample1.cs)]

Standardmäßig verwenden die TableAdapters in einem typisierten DataSet Transaktionen nicht. Zur Unterstützung für Transaktionen erweitern, die TableAdapter-Klassen, um zusätzliche Methoden enthalten, die die oben genannten Muster zu verwenden, um eine Reihe von Anweisungen zur Datenänderung innerhalb des Bereichs einer Transaktion ausführen müssen. In Schritt2 sehen wir, wie partielle Klassen zu verwenden, um diese Methoden hinzufügen.

## <a name="step-1-creating-the-working-with-batched-data-web-pages"></a>Schritt 1: Erstellen der Arbeit mit Webseiten als Batch erstellten Daten

Bevor wir beginnen, wie erweitern, die DAL zur Unterstützung von Datenbanktransaktionen untersuchen, können Sie s, schalten Sie zuerst einen Moment Zeit, um ASP.NET Web Pages, die wir für dieses Lernprogramm benötigen und die drei, die folgen zu erstellen. Starten, indem Sie einen neuen Ordner namens `BatchData` und fügen Sie dann die folgenden ASP.NET-Seiten, verknüpfen jede Seite mit den `Site.master` Masterseite.

- `Default.aspx`
- `Transactions.aspx`
- `BatchUpdate.aspx`
- `BatchDelete.aspx`
- `BatchInsert.aspx`


![Fügen Sie die ASP.NET-Seiten für die Lernprogramme SqlDataSource-bezogene hinzu.](wrapping-database-modifications-within-a-transaction-cs/_static/image1.gif)

**Abbildung 1**: Fügen Sie die ASP.NET-Seiten für die Lernprogramme SqlDataSource-bezogene hinzu


Wie bei den anderen Ordnern `Default.aspx` verwendet die `SectionLevelTutorialListing.ascx` Benutzersteuerelement, um die Lernprogramme in einem Abschnitt aufzulisten. Aus diesem Grund Hinzufügen dieses Benutzersteuerelement zu `Default.aspx` durch Ziehen aus dem Projektmappen-Explorer auf die Seite s Entwurfsansicht.


[![Das Benutzersteuerelement SectionLevelTutorialListing.ascx "default.aspx" hinzufügen](wrapping-database-modifications-within-a-transaction-cs/_static/image2.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image1.png)

**Abbildung 2**: Hinzufügen der `SectionLevelTutorialListing.ascx` Benutzersteuerelement `Default.aspx` ([klicken Sie hier, um das Bild in voller Größe angezeigt](wrapping-database-modifications-within-a-transaction-cs/_static/image2.png))


Abschließend fügen Sie diese vier Seiten als Einträge an die `Web.sitemap` Datei. Fügen Sie das folgende Markup insbesondere nach dem Anpassen der Siteübersicht `<siteMapNode>`:


[!code-xml[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample2.xml)]

Nach der Aktualisierung `Web.sitemap`, nehmen einen Moment Zeit, um die Lernprogramme-Website über einen Browser anzuzeigen. Klicken Sie im Menü auf der linken Seite enthält nun Elemente für das Arbeiten mit Daten als Batch erstellten Lernprogramme.


![Die Siteübersicht enthält nun die Einträge für die Arbeit mit Daten als Batch erstellten Lernprogramme](wrapping-database-modifications-within-a-transaction-cs/_static/image3.gif)

**Abbildung 3**: die Siteübersicht enthält jetzt die Einträge für die Arbeit mit Daten als Batch erstellten Lernprogramme


## <a name="step-2-updating-the-data-access-layer-to-support-database-transactions"></a>Schritt 2: Aktualisieren der Datenzugriffsebene zur Unterstützung von Datenbanktransaktionen

Wie im ersten Lernprogramm erläutert [erstellen eine Datenzugriffsschicht](../introduction/creating-a-data-access-layer-cs.md), die typisierte DataSet in unserer DAL besteht DataTables und TableAdapters. Die Datentabellen enthalten Daten, während die TableAdapters, die Funktionalität zum Lesen von Daten aus der Datenbank in die DataTables, zum Aktualisieren der Datenbank mit Änderungen an der DataTables usw. bereitstellen. Beachten Sie, dass die TableAdapters bieten zwei Muster zum Aktualisieren von Daten, die ich als BatchUpdate "und" DB-Direct "bezeichnet. Mit dem BatchUpdate-Muster wird der TableAdapter Datasets, einer Datentabelle oder einer Auflistung von DataRows übergeben. Diese Daten aufgelistet und für jeden eingefügt, geändert oder gelöschte Zeile, die `InsertCommand`, `UpdateCommand`, oder `DeleteCommand` ausgeführt wird. Mit dem DB-Direct-Muster wird der TableAdapter stattdessen die Werte der Spalten für das Einfügen, aktualisieren oder Löschen eines einzelnen Datensatzes erforderlich übergeben. Die DB-direkten Muster-Methode verwendet dann diese übergebene Werte zum Ausführen der entsprechenden `InsertCommand`, `UpdateCommand`, oder `DeleteCommand` Anweisung.

Unabhängig von der updatemuster verwendet verwenden Sie die TableAdapters automatisch generierte Methoden nicht Transaktionen. Standardmäßig wird jede INSERT-, Update- oder Delete des TableAdapter ausgeführt als einzelner Vorgang diskret behandelt. Stellen Sie sich z. B. vor, dass die DB-Direct-Muster von Code in die BLL, zum Einfügen von zehn Datensätze in der Datenbank verwendet wird. Dieser Code würde die TableAdapter aufrufen `Insert` zehnmal-Methode. Wenn die ersten fünf einfügungen erfolgreich, aber das sechste Element zu einer Ausnahme geführt hat, bleibt die ersten fünf eingefügte Datensätze in der Datenbank. Auf ähnliche Weise, wenn das BatchUpdate Muster auf Einfügevorgänge verwendet wird, Updates und löschungen an der eingefügten, geändert und Zeilen in einer "DataTable" gelöscht, wenn die erste einige Änderungen erfolgreich war, aber eine höhere Version ist ein Fehler, die diese früheren Änderungen aufgetreten, abgeschlossen, die in der Datenbank bleiben würden.

In bestimmten Szenarien möchten wir Unteilbarkeit über eine Reihe von Änderungen sicherzustellen. Wir die TableAdapter manuell erweitern müssen, indem Hinzufügen von neuen Methoden, die ausgeführt werden hierzu die `InsertCommand`, `UpdateCommand`, und `DeleteCommand` s des wirkungsfelds einer Transaktion. In [erstellen eine Datenzugriffsschicht](../introduction/creating-a-data-access-layer-cs.md) erläutert, mit [Teilklassen](http://en.wikipedia.org/wiki/Partial_type) zum Erweitern der Funktionalität der DataTables innerhalb des typisierten Datasets. Diese Technik kann auch mit TableAdapters verwendet werden.

Das typisierte DataSet `Northwind.xsd` befindet sich der `App_Code` Ordner s `DAL` Unterordner. Erstellen Sie einen Unterordner in der `DAL` Ordner mit dem Namen `TransactionSupport` und fügen Sie eine neue Klassendatei mit dem Namen `ProductsTableAdapter.TransactionSupport.cs` (siehe Abbildung 4). Diese Datei wird die partielle Implementierung des halten die `ProductsTableAdapter` , die Methoden zum Ausführen von datenänderungen, die unter Verwendung einer Transaktion enthält.


![Fügen Sie einen Ordner namens TransactionSupport und eine Klassendatei mit dem Namen ProductsTableAdapter.TransactionSupport.cs hinzu](wrapping-database-modifications-within-a-transaction-cs/_static/image4.gif)

**Abbildung 4**: Fügen Sie einen Ordner mit dem Namen `TransactionSupport` und eine Klassendatei mit dem Namen `ProductsTableAdapter.TransactionSupport.cs`


Geben Sie den folgenden Code in die `ProductsTableAdapter.TransactionSupport.cs` Datei:


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample3.cs)]

Die `partial` -Schlüsselwort in der Klassendeklaration hier gibt an, an den Compiler, die innerhalb von hinzugefügten Member hinzugefügt werden, die `ProductsTableAdapter` -Klasse in der `NorthwindTableAdapters` Namespace. Beachten Sie die `using System.Data.SqlClient` -Anweisung am Anfang der Datei. Seit der TableAdapter für die Verwendung des SqlClient-Anbieters konfiguriert wurde, er verwendet intern eine [ `SqlDataAdapter` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqldataadapter.aspx) -Objekt, dessen Befehle an die Datenbank ausgeben. Folglich müssen wir verwenden die `SqlTransaction` Klasse, um die Transaktion zu beginnen und dann einen commit oder Rollback für sie. Wenn Sie einen anderen Datenspeicher als Microsoft SQL Server verwenden, müssen Sie den entsprechenden Anbieter verwenden.

Diese Methoden geben Sie die Bausteine, die erforderlich sind, Rollback- und commit einer Transaktion. Markiert sind `public`, aktivieren sie innerhalb von verwendet werden die `ProductsTableAdapter`, von einer anderen Klasse in der DAL oder aus einer anderen Ebene in der Architektur, z. B. die BLL. `BeginTransaction` Öffnet die internen TableAdapter `SqlConnection` (falls erforderlich), startet die Transaktion und weist sie der `Transaction` -Eigenschaft, und fügt die Transaktion an den internen `SqlDataAdapter` s `SqlCommand` Objekte. `CommitTransaction` und `RollbackTransaction` rufen die `Transaction` s-Objekt `Commit` und `Rollback` Methoden, bzw. vor dem Schließen der internen `Connection` Objekt.

## <a name="step-3-adding-methods-to-update-and-delete-data-under-the-umbrella-of-a-transaction"></a>Schritt 3: Hinzufügen von Methoden zum Aktualisieren und Löschen von Daten des Wirkungsfelds einer Transaktion

Mit diesen Methoden abgeschlossen ist, wir re Methoden hinzufügen Installationsbereit `ProductsDataTable` oder die BLL, die eine Reihe von Befehlen des wirkungsfelds einer Transaktion ausführen. Die folgende Methode mithilfe des Musters Batchaktualisierung Aktualisieren einer `ProductsDataTable` -Instanz unter Verwendung einer Transaktion. Es startet eine Transaktion durch Aufrufen der `BeginTransaction` -Methode und dann verwendet, eine `try...catch` Block, um die Anweisungen zur Datenänderung ausgeben. Wenn der Aufruf der `Adapter` s-Objekt `Update` Methode zu einer Ausnahme, die Ausführung überträgt an die `catch` Block, in dem die Transaktion wird ein Rollback, und die Ausnahme erneut ausgelöst. Bedenken Sie, dass die `Update` Methode implementiert das Muster BatchUpdate durch das Auflisten der Zeilen des angegebenen `ProductsDataTable` und Ausführen der erforderlichen `InsertCommand`, `UpdateCommand`, und `DeleteCommand` s. Wenn eines dieser Befehle führt zu einem Fehler, wird die Transaktion zurückgesetzt der vorherigen während der Lebensdauer von Transaktionen s vorgenommenen Änderungen rückgängig zu machen. Sollte die `Update` Anweisung ohne Fehler abgeschlossen ist, wird die Transaktion ein Commit in seiner Gesamtheit ausgeführt.


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample4.cs)]

Hinzufügen der `UpdateWithTransaction` Methode, um die `ProductsTableAdapter` Klasse über die partielle Klasse im `ProductsTableAdapter.TransactionSupport.cs`. Alternativ können Sie diese Methode mit dem Business Logic Layer s hinzugefügt werden konnte `ProductsBLL` Klasse mit einigen geringfügigen syntaktische Änderungen. Nämlich das Schlüsselwort hierzu finden Sie unter `this.BeginTransaction()`, `this.CommitTransaction()`, und `this.RollbackTransaction()` durch ersetzt werden müssten `Adapter` (Erinnerung `Adapter` ist der Name einer Eigenschaft in `ProductsBLL` des Typs `ProductsTableAdapter`).

Die `UpdateWithTransaction` Methode verwendet die Batch-updatemuster, aber eine Reihe von DB-Direct-Aufrufe kann auch im Rahmen einer Transaktion, wie die folgende Methode zeigt verwendet werden. Die `DeleteProductsWithTransaction` Methode akzeptiert als Eingabe eine `List<T>` vom Typ `int`, wobei es sich um die `ProductID` s löschen. Die Methode startet die Transaktion über einen Aufruf an `BeginTransaction` und dann auf die `try` blockieren, iteriert durch die bereitgestellte Liste der DB-Direct-Muster aufrufen `Delete` Methode für die einzelnen `ProductID` Wert. Wenn alle Aufrufe des `Delete` ein Fehler auftritt, wird die Steuerung an die `catch` Block, in dem die Transaktion zurückgesetzt wird, und die Ausnahme erneut ausgelöst. Wenn alle Aufrufe `Delete` erfolgreich ausgeführt werden, und klicken Sie dann die Transaktion ein Commit ausgeführt wird. Fügen Sie diese Methode, um die `ProductsBLL` Klasse.


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample5.cs)]

## <a name="applying-transactions-across-multiple-tableadapters"></a>Anwenden von Transaktionen über mehrere TableAdapters

Die transaktionsbezogenen Code untersucht, die in diesem Lernprogramm kann für mehrere Anweisungen für die `ProductsTableAdapter` als atomarer Vorgang behandelt werden soll. Aber was geschieht, wenn mehrere Änderungen an anderen Datenbanktabellen atomar ausgeführt werden müssen? Beispielsweise kann beim Löschen einer Kategorie wir zunächst seine aktuelle Produkte einige andere Kategorie neu zuweisen möchten. Diese beiden Schritte aus, die Produkte Neuzuweisen und löschen die Kategorie sollte als atomarer Vorgang ausgeführt werden. Aber die `ProductsTableAdapter` enthält nur die Methoden zum Ändern von der `Products` Tabelle und die `CategoriesTableAdapter` enthält nur die Methoden zum Ändern der `Categories` Tabelle. Umfassen eine Transaktion kann wie beide TableAdapters?

Eine Möglichkeit besteht darin, eine Methode zum Hinzufügen der `CategoriesTableAdapter` mit dem Namen `DeleteCategoryAndReassignProducts(categoryIDtoDelete, reassignToCategoryID)` und diese Methode eine gespeicherte Prozedur aufrufen, die sowohl die Produkte weist und die Kategorie innerhalb des Bereichs einer Transaktion, die in der gespeicherten Prozedur definiert löscht haben. Betrachten wir zum begin, commit und Rollback-Transaktionen in gespeicherten Prozeduren in einem späteren Lernprogramm.

Eine weitere Option ist eine Hilfsklasse in der DAL erstellen, enthält die `DeleteCategoryAndReassignProducts(categoryIDtoDelete, reassignToCategoryID)` Methode. Diese Methode würde erstellen Sie eine Instanz von der `CategoriesTableAdapter` und `ProductsTableAdapter` und legen Sie diese in zwei TableAdapters `Connection` Eigenschaften auf den gleichen `SqlConnection` Instanz. AT starten diesen Punkt, entweder eine der beiden TableAdapters würden die Transaktion mit einem Aufruf von `BeginTransaction`. Die TableAdapter-Methoden zum Neuzuweisen die Produkte und löschen die Kategorie würde aufgerufen, einem `try...catch` Block mit der Transaktion einen Commit oder Rollback Bedarf zurück.

## <a name="step-4-adding-theupdatewithtransactionmethod-to-the-business-logic-layer"></a>Schritt 4: Hinzufügen der`UpdateWithTransaction`Methode, um die Geschäftslogikschicht

In Schritt 3 hinzugefügt wir ein `UpdateWithTransaction` Methode, um die `ProductsTableAdapter` in der DAL. Es sollte eine entsprechende Methode der BLL hinzugefügt werden. Während die Darstellungsschicht direkt auf die DAL aufzurufenden aufrufen können die `UpdateWithTransaction` -Methode, wird eine mehrstufige Architektur zu definieren, die die DAL aus der Darstellungsschicht isoliert haben diese Lernprogramme strived. Aus diesem Grund behooves uns dieser Ansatz zu fortfahren.

Öffnen der `ProductsBLL` Klassendatei, und fügen Sie eine Methode mit dem Namen `UpdateWithTransaction` , einfach Aufrufe an die entsprechende DAL-Methode. Es sollte jetzt zwei neue Methoden in sein `ProductsBLL`: `UpdateWithTransaction`, die Sie gerade hinzugefügt haben, und `DeleteProductsWithTransaction`, der in Schritt 3 hinzugefügt wurde.


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample6.cs)]

> [!NOTE]
> Diese Methoden schließen Sie nicht die `DataObjectMethodAttribute` Attribut zugewiesen werden, um die meisten Methoden in der `ProductsBLL` Klasse, da wir diese Methoden direkt von den Code-Behind-Klassen von ASP.NET Seiten aufgerufen werden müssen. Bedenken Sie, dass `DataObjectMethodAttribute` wird verwendet, um zu kennzeichnen, welche Methoden in der ObjectDataSource s Konfigurieren von Datenquellen, Assistenten und auf welche Registerkarte (SELECT, UPDATE, INSERT oder DELETE) angezeigt werden soll. Da die GridView verfügt nicht über integrierte Unterstützung für den Batch bearbeiten oder löschen, müssen wir diese Methoden programmgesteuert aufgerufen, sondern verwenden Sie deklarativen Code frei Ansatz.


## <a name="step-5-atomically-updating-database-data-from-the-presentation-layer"></a>Schritt 5: Aktualisieren von Datenbankdaten aus der Darstellungsschicht atomar

Um die Auswirkung zu veranschaulichen, die die Transaktion wurde beim Aktualisieren eines Batches von Datensätzen, Let s erstellen eine Benutzeroberfläche, listet alle Produkte in einer GridView und enthält eine Schaltfläche Web, steuern, die beim Klicken auf die Produkte weist `CategoryID` Werte. Insbesondere wird die Kategorie neuzuweisung verstreichen, damit die erste mehrere Produkte eine gültige zugewiesen sind `CategoryID` Wert, während andere absichtlich sind zugewiesen, eine nicht existierende `CategoryID` Wert. Wenn Sie versuchen, die Datenbank mit einem Produkt zu aktualisieren, deren `CategoryID` passt sich nicht auf eine vorhandene Kategorie s `CategoryID`, eine foreign Key-einschränkungsverletzung auftreten wird und eine Ausnahme ausgelöst. Was wir in diesem Beispiel sehen sich ergibt, wenn mithilfe einer Transaktion die Ausnahme, die ausgelöst wird, aus der foreign Key-Einschränkung-Verletzung der vorherigen bewirkt gültige `CategoryID` Änderungen rückgängig gemacht werden. Wenn eine Transaktion nicht verwendet werden soll, bleibt jedoch die Änderungen an der anfänglichen Kategorien.

Öffnen Sie zunächst die `Transactions.aspx` auf der Seite der `BatchData` Ordner, und ziehen Sie eine GridView aus der Toolbox in den Designer. Legen Sie dessen `ID` auf `Products` und aus seinem Smarttag binden Sie es an eine neue ObjectDataSource mit dem Namen `ProductsDataSource`. Konfigurieren der ObjectDataSource zum Abrufen der zugehörigen Daten aus der `ProductsBLL` Klasse s `GetProducts` Methode. Eine GridView schreibgeschützt sein wird, so legen Sie die Dropdownlisten in der Update-, INSERT- und Löschen von Registerkarten (keine) und klicken Sie auf ' Fertig stellen '.


[![Abbildung 5: Konfigurieren der ObjectDataSource zur Verwendung der ProductsBLL Klasse s GetProducts-Methode](wrapping-database-modifications-within-a-transaction-cs/_static/image5.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image3.png)

**Abbildung 5**: Abbildung 5: Konfigurieren der ObjectDataSource verwenden die `ProductsBLL` Klasse s `GetProducts` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](wrapping-database-modifications-within-a-transaction-cs/_static/image4.png))


[![Legen Sie die Dropdownlisten in der Update-, INSERT- und Löschen von Registerkarten auf (keine)](wrapping-database-modifications-within-a-transaction-cs/_static/image6.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image5.png)

**Abbildung 6**: Legen Sie das Dropdown-Listen in aktualisieren, einfügen und Löschen von Registerkarten auf (keine) ([klicken Sie hier, um das Bild in voller Größe angezeigt](wrapping-database-modifications-within-a-transaction-cs/_static/image6.png))


Nach Abschluss des Konfigurieren von Datenquellen-Assistenten, erstellt Visual Studio BoundFields und eine CheckBoxField für das Produkt von Datenfeldern. Entfernen Sie alle diese Felder mit Ausnahme von `ProductID`, `ProductName`, `CategoryID`, und `CategoryName` , und benennen Sie die `ProductName` und `CategoryName` BoundFields `HeaderText` Eigenschaften Produkt und Kategorie bzw. Überprüfen Sie aus dem Smarttag die Option Paging aktivieren. Nachdem Sie diese Änderungen vornehmen, sollten die GridView und ObjectDataSource s deklarative Markup wie folgt aussehen:


[!code-aspx[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample7.aspx)]

Als Nächstes fügen Sie oberhalb der GridView drei Schaltfläche Websteuerelemente. Legen Sie die erste Schaltfläche s Texteigenschaft auf Raster aktualisieren, die zweite s Kategorien ändern (mit Transaktion) und die dritte s zu ändern, Kategorien (ohne Transaktion).


[!code-aspx[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample8.aspx)]

An diesem Punkt sollte die Entwurfsansicht in Visual Studio Siehe Abbildung 7 Screenshot ähneln.


[![Diese Seite enthält eine GridView und drei Schaltflächensteuerelemente Web](wrapping-database-modifications-within-a-transaction-cs/_static/image7.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image7.png)

**Abbildung 7**: die Seite enthält eine GridView und drei Schaltfläche Web Controls ([klicken Sie hier, um das Bild in voller Größe angezeigt](wrapping-database-modifications-within-a-transaction-cs/_static/image8.png))


Erstellen von Ereignishandlern für jede der drei Schaltfläche s `Click` Ereignisse und verwenden Sie folgenden Code:


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample9.cs)]

Die Aktualisierung Schaltfläche s `Click` Ereignishandler bindet die Daten an die GridView einfach durch Aufrufen der `Products` GridView s `DataBind` Methode.

Der zweite Ereignishandler weist die Produkte `CategoryID` s und verwendet die neue Transaktion-Methode der BLL zum Ausführen der Datenbank des wirkungsfelds einer Transaktion aktualisiert. Beachten Sie, dass jedes Produkt s `CategoryID` nach dem Zufallsprinzip festgelegt ist, auf den gleichen Wert wie die `ProductID`. Dies funktioniert hervorragend für das erste einige Produkte seit dieser Produkte haben `ProductID` Werte, die durchgeführt werden, um gültige zuzuordnen `CategoryID` s. Doch einmal die `ProductID` s-Start zu groß, diese Zufall Überschneidung von `ProductID` s und `CategoryID` s nicht mehr gültig.

Das dritte `Click` Ereignishandler aktualisiert die Produkte `CategoryID` s auf die gleiche Weise sendet das Update jedoch mit der Datenbank mithilfe der `ProductsTableAdapter` s-Standard `Update` Methode. Dies `Update` Methode die Reihe von Befehlen in einer Transaktion wird nicht umbrochen, bleiben, damit diese Änderungen vor dem ersten Fehler beim foreign Key-Einschränkung Verletzung vorgenommen werden.

Um dieses Verhalten zu demonstrieren, finden Sie auf dieser Seite über einen Browser. Zunächst sollte die erste Seite der Daten angezeigt werden, wie in Abbildung 8 gezeigt. Klicken Sie als Nächstes auf die Schaltfläche mit den Kategorien ändern (mit TRANSACTION). Dadurch wird einen Postback und versucht, alle Produkte aktualisieren `CategoryID` Werte aber führt zu einer Verletzung der foreign Key-Einschränkung (siehe Abbildung 9).


[![Die Produkte werden in einem auslagerungsfähigen GridView angezeigt.](wrapping-database-modifications-within-a-transaction-cs/_static/image8.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image9.png)

**Abbildung 8**: die Produkte werden in einem auslagerungsfähigen GridView angezeigt ([klicken Sie hier, um das Bild in voller Größe angezeigt](wrapping-database-modifications-within-a-transaction-cs/_static/image10.png))


[![Erneutes Zuweisen von den Ergebnissen Kategorien zu einer Verletzung der Foreign Key-Einschränkung](wrapping-database-modifications-within-a-transaction-cs/_static/image9.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image11.png)

**Abbildung 9**: Erneutes Zuweisen von den Ergebnissen Kategorien zu einer Verletzung der Foreign Key-Einschränkung ([klicken Sie hier, um das Bild in voller Größe angezeigt](wrapping-database-modifications-within-a-transaction-cs/_static/image12.png))


Drücken Sie jetzt Ihr Browser s-Schaltfläche "zurück", und klicken Sie dann auf die Schaltfläche "Raster aktualisieren". Beim Aktualisieren der Daten sehen Sie genaue dieselbe Ausgabe wie in Abbildung 8 gezeigt. D. h. auch jedoch für einige der Produkte `CategoryID` s wurden um rechtliche geänderten Werte und in der Datenbank aktualisiert, sie wurden zurückgesetzt, wenn die foreign Key-Einschränkung verletzt.

Probieren Sie Sie auf die Schaltfläche ändern Kategorien (ohne Transaktion). Dies führt zu dem Fehler aufgrund einer einschränkungsverletzung gleichen foreign Key-Einschränkung (siehe Abbildung 9), aber dieses Mal die Produkte, deren `CategoryID` Werte wurden geändert, um eine gültige Wert wird nicht zurückgesetzt werden. Drücken Sie Ihr Browser s-Schaltfläche "zurück", und klicken Sie dann die Schaltfläche "Raster aktualisieren". Wie in Abbildung 10 gezeigt, die `CategoryID` s der ersten acht Produkte wurden neu zugewiesen wurde. In Abbildung 8 mussten ändern z. B. eine `CategoryID` 1, aber in Abbildung 10 It s 2 zugewiesen wurde.


[![Einige Produkte CategoryID Werte wurden aktualisiert, während andere wurden nicht](wrapping-database-modifications-within-a-transaction-cs/_static/image10.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image13.png)

**Abbildung 10**: Einige Produkte `CategoryID` Werte wurden aktualisiert, während andere wurden nicht ([klicken Sie hier, um das Bild in voller Größe angezeigt](wrapping-database-modifications-within-a-transaction-cs/_static/image14.png))


## <a name="summary"></a>Zusammenfassung

Standardmäßig die TableAdapter-s-Methoden nicht die Datenbank ausgeführten Anweisungen innerhalb des Bereichs einer Transaktion umbrechen, jedoch mit ein wenig Aufwand können wir die Methoden, die erstellt werden, Commit- und Rollback einer Transaktion hinzufügen. In diesem Lernprogramm erstellt es drei Methoden in der `ProductsTableAdapter` Klasse: `BeginTransaction`, `CommitTransaction`, und `RollbackTransaction`. Wurde erläutert, wie diese Methoden zusammen mit einem `try...catch` Block, um eine Reihe von Anweisungen zur Datenänderung atomic. Insbesondere haben wir erstellt die `UpdateWithTransaction` Methode in der `ProductsTableAdapter`, die mithilfe des Musters BatchUpdate führen Sie die notwendigen Änderungen an den Zeilen der einen bereitgestellten `ProductsDataTable`. Wir auch hinzugefügt der `DeleteProductsWithTransaction` Methode, um die `ProductsBLL` Klasse in der BLL akzeptiert eine `List` von `ProductID` -Werte als Eingabe und ruft die Methode der DB-Direct-Muster `Delete` für jede `ProductID`. Beide Methoden zu starten, indem Sie eine Transaktion erstellen und ausführen klicken Sie dann die Anweisungen zur Datenänderung in einer `try...catch` Block. Wenn eine Ausnahme auftritt, wird ein Rollback für die Transaktion, andernfalls ist es ein Commit ausgeführt wurde.

Schritt 5 veranschaulicht den Effekt der transaktionalen BatchUpdates im Vergleich zu BatchUpdates, für die Verwendung eine Transaktion versäumt. In den nächsten drei Lernprogrammen wir bauen auf die in diesem Lernprogramm genannten Foundation und Erstellen von Benutzeroberflächen für BatchUpdates, löschungen und einfügungen ausführen.

Viel Spaß beim Programmieren!

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Lernprogramm erläutert finden Sie in den folgenden Ressourcen:

- [Wahrung der Datenbankkonsistenz mit Transaktionen](http://aspnet.4guysfromrolla.com/articles/072705-1.aspx)
- [Verwalten von Transaktionen in SQLServer gespeicherte Prozeduren](http://www.4guysfromrolla.com/webtech/080305-1.shtml)
- [Transaktionen, die leicht gemacht werden: `System.Transactions`](https://blogs.msdn.com/florinlazar/archive/2004/07/23/192239.aspx)
- ["TransactionScope" und "DataAdapters"](http://andyclymer.blogspot.com/2007/01/transactionscope-and-dataadapters.html)
- [Mithilfe von Oracle Database Transactions in .NET](http://www.oracle.com/technology/pub/articles/price_dbtrans_dotnet.html)

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurden Dave Gardner Hilton Giesenow und Teresa Murphy. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Nächste](batch-updating-cs.md)
