---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs
title: Verwenden die vorhandene gespeicherte Prozeduren für das typisierte DataSet TableAdapters (c#) | Microsoft Docs
author: rick-anderson
description: Im vorherigen Lernprogramm haben wir gelernt, den TableAdapter-Assistenten verwenden, um neue gespeicherte Prozeduren zu generieren. In diesem Lernprogramm erfahren Sie, wie die gleichen TableAdapter...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: 440bef2a-1641-4238-99e3-8e2d44e7d94c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs
msc.type: authoredcontent
ms.openlocfilehash: df8a714325ce99db615eddc3d457da5c926919ba
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877072"
---
<a name="using-existing-stored-procedures-for-the-typed-datasets-tableadapters-c"></a>Verwenden die vorhandene gespeicherte Prozeduren für das typisierte DataSet TableAdapters (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Herunterladen von Code](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_68_CS.zip) oder [PDF herunterladen](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/datatutorial68cs1.pdf)

> Im vorherigen Lernprogramm haben wir gelernt, den TableAdapter-Assistenten verwenden, um neue gespeicherte Prozeduren zu generieren. In diesem Lernprogramm lernen wir an, wie die gleichen TableAdapter-Assistenten mit bereits vorhandener gespeicherter Prozeduren arbeiten können. Wir erfahren Sie, wie die Datenbank neue gespeicherte Prozeduren manuell hinzugefügt werden.


## <a name="introduction"></a>Einführung

In der [vorherigen Lernprogramm](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) wurde erläutert, wie das typisierte DataSet s TableAdapters konfiguriert werden konnte, um gespeicherte Prozeduren für den Zugriff Daten anstelle von Ad-hoc-SQL-Anweisungen verwenden. Insbesondere untersucht es wie den TableAdapter-Assistenten automatisch diese gespeicherten Prozeduren erstellt haben. Beim Portieren einer älteren Anwendung in ASP.NET 2.0 oder eine ASP.NET 2.0-Website, um ein vorhandenes Data Modell erstellen, sind wahrscheinlich, dass die Datenbank enthält bereits die gespeicherten Prozeduren, die Sie benötigen. Alternativ empfiehlt es sich um die gespeicherten Prozeduren zu erstellen, manuell oder über ein Tool als dem TableAdapter-Assistenten, der die gespeicherten Prozeduren automatisch generiert.

In diesem Lernprogramm werden wir konfigurieren den TableAdapter um vorhandene gespeicherte Prozeduren betrachten. Da die Northwind-Datenbank nur einen kleinen Satz von integrierten gespeicherten Prozeduren aufweist, werden wir auch die erforderlichen Schritte zum neue gespeicherte Prozeduren über Visual Studio-Umgebung auf die Datenbank manuell hinzufügen betrachten. Lassen Sie s beginnen!

> [!NOTE]
> In der [Umbruch Datenbankänderungen innerhalb einer Transaktion](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs.md) Lernprogramm hinzugefügt Methoden des TableAdapter zum Unterstützen von Transaktionen (`BeginTransaction`, `CommitTransaction`usw.). Alternativ können Transaktionen vollständig innerhalb einer gespeicherten Prozedur verwaltet werden, die keine Änderungen an der Datenzugriffsebene Code erforderlich ist. In diesem Lernprogramm lernen wir die T-SQL-Befehle zum Ausführen einer gespeicherten Prozedur s-Anweisungen innerhalb des Bereichs einer Transaktion kennen.


## <a name="step-1-adding-stored-procedures-to-the-northwind-database"></a>Schritt 1: Hinzufügen von gespeicherten Prozeduren mit der Datenbank Northwind

Visual Studio erleichtert das Hinzufügen einer Datenbank neuer gespeicherter Prozeduren. Let s hinzufügen eine neue gespeicherte Prozedur zur Northwind-Datenbank, die alle Spalten gibt die `Products` für diejenigen, die über eine bestimmte Tabelle `CategoryID` Wert. Erweitern Sie in Server Explorer-Fenster der Northwind-Datenbank, so, dass die Ordner - Datenbank-Diagrammen, Tabellen, Sichten usw. -angezeigt werden. Wie im vorherigen Lernprogramm wurde gezeigt, enthält der Ordner gespeicherte Prozeduren der Datenbank s bereits vorhandener gespeicherter Prozeduren. Zum Hinzufügen einer neuen gespeicherten Prozedur mit der rechten Maustaste in des Ordners gespeicherte Prozeduren und wählen Sie im Kontextmenü die Option neue gespeicherte Prozedur hinzufügen.


[![Mit der rechten Maustaste in des Ordners gespeicherte Prozeduren und Hinzufügen einer neuen gespeicherten Prozedur](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image2.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image1.png)

**Abbildung 1**: mit der rechten Maustaste in den Ordner für die gespeicherten Prozeduren und Hinzufügen einer neuen gespeicherten Prozedur ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image3.png))


Wie in Abbildung 1 gezeigt, die Option Neue gespeicherte Prozedur hinzufügen wird ein Skriptfenster geöffnet in Visual Studio mit der Gliederung des SQL-Skripts zum Erstellen der gespeicherten Prozedur erforderlich sind. Es ist unsere Aufgabe rolloutphasen dieses Skript aus, und führen Sie es, die an diesem Punkt der Datenbank die gespeicherte Prozedur hinzugefügt werden.

Geben Sie das folgende Skript aus:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample1.sql)]

Mit diesem Skript, das bei der Ausführung wird eine neue gespeicherte Prozedur hinzufügen, mit der Northwind-Datenbank mit dem Namen `Products_SelectByCategoryID`. Diese gespeicherte Prozedur nimmt einen einzelnen Eingabeparameter (`@CategoryID`, des Typs `int`) und alle Felder für diese Produkte mit einer passenden Längenwert `CategoryID` Wert.

Zum Ausführen dieser `CREATE PROCEDURE` Skript und fügen Sie die gespeicherte Prozedur in der Datenbank, klicken Sie auf das Symbol "Speichern" in der Symbolleiste oder STRG + S drücken. Zeigt die neu erstellte nach dies der Fall, Aktualisierungen Ordner "Stored Procedures" gespeicherte Prozedur aus. Außerdem ändert das Skript im Fenster Besonderheit aus `CREATE PROCEDURE dbo.Products_SelectProductByCategoryID` auf `ALTER PROCEDURE` `dbo.Products_SelectProductByCategoryID`. `CREATE PROCEDURE` Fügt eine neue gespeicherte Prozedur mit der Datenbank, während `ALTER PROCEDURE` aktualisiert eine vorhandene. Seit dem Start des Skripts geändert hat `ALTER PROCEDURE`, ändern die gespeicherten Prozeduren geben Sie Parameter oder SQL-Anweisungen, und klicken auf das Symbol "Speichern", wird die gespeicherte Prozedur mit diesen Änderungen aktualisiert.

Abbildung 2 zeigt Visual Studio nach dem `Products_SelectByCategoryID` gespeicherte Prozedur wurde gespeichert.


[![Die gespeicherte Prozedur Products_SelectByCategoryID wurde hinzugefügt, mit der Datenbank](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image5.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image4.png)

**Abbildung 2**: die gespeicherte Prozedur `Products_SelectByCategoryID` wurde in der Datenbank hinzugefügt wurde ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image6.png))


## <a name="step-2-configuring-the-tableadapter-to-use-an-existing-stored-procedure"></a>Schritt 2: Konfigurieren des TableAdapters zum Verwenden einer vorhandenen gespeicherten Prozedur

Nachdem die `Products_SelectByCategoryID` gespeicherte Prozedur wurde mit der Datenbank können wir Concigure unserer Data Access Layer, um diese gespeicherte Prozedur zu verwenden, wenn einer der Methoden aufgerufen wird. Insbesondere fügen wir eine `GetProducstByCategoryID(categoryID)` Methode, um die `ProductsTableAdapter` in der `NorthwindWithSprocs` typisierte DataSet, das Aufrufe der `Products_SelectByCategoryID` gespeicherte Prozedur, die soeben erstellt wurde.

Öffnen Sie zunächst die `NorthwindWithSprocs` DataSet. Mit der rechten Maustaste auf die `ProductsTableAdapter` , und wählen Sie die Abfrage hinzufügen, um den TableAdapter-Abfrage-Konfigurations-Assistenten zu starten. In der [vorherigen Lernprogramm](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) wir entschied sich für die TableAdapter eine neue gespeicherte Prozedur für uns erstellt haben. Dieses Lernprogramm jedoch möchten wir die neue Methode des TableAdapter zur vorhandenen Netzwerkdaten `Products_SelectByCategoryID` gespeicherte Prozedur. Daher wählen Sie die Option zum Verwenden einer vorhandene gespeicherten Prozedur aus dem ersten Schritt des Assistenten-s, und klicken Sie dann auf Weiter.


[![Wählen Sie die vorhandene gespeicherte Prozedur Option Verwendung](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image8.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image7.png)

**Abbildung 3**: Wählen Sie die vorhandene verwenden gespeicherte Prozedur Option ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image9.png))


Der folgende Bildschirm bietet eine Dropdownliste mit s-Datenbank gespeicherte Prozeduren aufgefüllt. Auswählen einer gespeicherten Prozedur listet seine Eingabeparameter auf der linken Seite und die Datenfelder zurückgegeben (sofern vorhanden) auf der rechten Seite. Wählen Sie die `Products_SelectByCategoryID` gespeicherte Prozedur aus der Liste aus, und klicken Sie auf Weiter.


[![Wählen Sie die Products_SelectByCategoryID gespeicherte Prozedur](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image11.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image10.png)

**Abbildung 4**: Wählen Sie die `Products_SelectByCategoryID` gespeicherte Prozedur ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image12.png))


Im nächste Bildschirm werden wir gefragt, welche Art von Daten von der gespeicherten Prozedur zurückgegeben wird, und hier unsere Antwort bestimmt den Typ, der von der TableAdapter-s-Methode zurückgegeben. Beispielsweise, wenn Sie auf tabellarische Daten zurückgegeben werden, die Methode zurück eine `ProductsDataTable` Instanz, die mit der von der gespeicherten Prozedur zurückgegebenen Datensätze aufgefüllt. Im Gegensatz dazu bei wir, dass diese gespeicherte Prozedur einen einzelnen Wert zurückgibt, die auf der TableAdapter zurück ein `object` , den Wert in der ersten Spalte des ersten Datensatzes, der von der gespeicherten Prozedur zurückgegebenen zugewiesen ist.

Da die `Products_SelectByCategoryID` gespeicherte Prozedur gibt alle Produkte, die zu einer bestimmten Kategorie gehören, wählen Sie die erste Antwort - Tabellendaten - aus, und klicken Sie auf Weiter.


[![Um anzugeben Sie, dass die gespeicherte Prozedur Tabellendaten zurückgibt](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image14.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image13.png)

**Abbildung 5**: anzugeben, dass die gespeicherte Prozedur Tabellendaten zurückgibt ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image15.png))


Übrig bleibt, um anzugeben, welche Methode Muster verwenden die Namen für diese Methoden folgen. Lassen Sie beide Füllung eine DataTable und der Rückgabewert eine DataTable-Optionen überprüft, aber die Methoden zum Benennen `FillByCategoryID` und `GetProductsByCategoryID`. Klicken Sie dann auf Weiter, um eine Zusammenfassung der Aufgaben zu überprüfen, die der Assistent ausführt. Wenn alles richtig aussieht, klicken Sie auf "Fertig stellen".


[![Name der Methoden FillByCategoryID und GetProductsByCategoryID](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image17.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image16.png)

**Abbildung 6**: Benennen Sie die Methoden `FillByCategoryID` und `GetProductsByCategoryID` ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image18.png))


> [!NOTE]
> Die TableAdapter-Methoden, die soeben erstellt wurde, `FillByCategoryID` und `GetProductsByCategoryID`, erwarten, dass einen Eingabeparameter vom Typ `int`. Dieser Eingabeparameterwert wird übergeben, in der gespeicherten Prozedur über seine `@CategoryID` Parameter. Wenn Sie ändern die `Products_SelectByCategory` Parameter der gespeicherten Prozeduren s, Sie müssen auch die Parameter für diese TableAdapter-Methoden zu aktualisieren. Entsprechend der Anleitung unter dem [vorherigen Lernprogramm](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md), dies kann auf zwei Arten ausgeführt werden: durch manuell hinzufügen oder Entfernen von Parametern aus der Parameterauflistung oder durch den TableAdapter-Assistenten erneut.


## <a name="step-3-adding-agetproductsbycategoryidcategoryidmethod-to-the-bll"></a>Schritt 3: Hinzufügen einer`GetProductsByCategoryID(categoryID)`Methode, um die BLL

Mit der `GetProductsByCategoryID` DAL-Methode abgeschlossen, der nächste Schritt ist für den Zugriff auf diese Methode in der Geschäftslogikebene. Öffnen der `ProductsBLLWithSprocs` Klassendatei, und fügen Sie die folgende Methode hinzu:


[!code-csharp[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample2.cs)]

Diese Methode BLL gibt einfach auftragsantwortnachrichten zurück die `ProductsDataTable` Merry der `ProductsTableAdapter` s `GetProductsByCategoryID` Methode. Die `DataObjectMethodAttribute` Attribut enthält Metadaten, die vom ObjectDataSource s Konfigurieren von Datenquellen-Assistenten verwendet. Diese Methode wird vor allem in der Registerkarte "SELECT" s Dropdown-Liste angezeigt.

## <a name="step-4-displaying-products-by-category"></a>Schritt 4: Anzeigen von Produkten nach Kategorie

So testen Sie die neu hinzugefügte `Products_SelectByCategoryID` gespeicherte Prozedur und die entsprechenden DAL und BLL Methoden können s eine ASP.NET-Seite zu erstellen, die einer DropDownList und eine GridView enthält. Der DropDownList werden alle Kategorien in der Datenbank aufgelistet, während die GridView Produkte zur ausgewählten Kategorie angezeigt werden.

> [!NOTE]
> Wir Ve erstellt Master/Detail-Schnittstellen mit DropDownLists im vorherigen Lernprogrammen. Eine eingehendere Betrachtung ein solchen Master/Detail-Berichts implementieren, finden Sie in der [Master/Detail-Filtern mit einer DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) Lernprogramm.


Öffnen der `ExistingSprocs.aspx` auf der Seite der `AdvancedDAL` Ordner, und ziehen Sie eine DropDownList aus der Toolbox in den Designer. Festlegen der DropDownList-s `ID` Eigenschaft `Categories` und seine `AutoPostBack` Eigenschaft `true`. Als Nächstes aus seinem Smarttag an eine neue ObjectDataSource mit dem Namen der DropDownList binden `CategoriesDataSource`. Das ObjectDataSource so konfigurieren, dass abgerufen, deren Daten aus der `CategoriesBLL` Klasse s `GetCategories` Methode. Legen Sie die Dropdownlisten in der Update-, INSERT-, und Löschen von Registerkarten (keine).


[![Abrufen von Daten aus der CategoriesBLL Klasse s GetCategories-Methode](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image20.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image19.png)

**Abbildung 7**: Abrufen von Daten aus der `CategoriesBLL` Klasse s `GetCategories` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image21.png))


[![Legen Sie die Dropdownlisten in der Update-, INSERT- und Löschen von Registerkarten auf (keine)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image23.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image22.png)

**Abbildung 8**: Legen Sie das Dropdown-Listen in aktualisieren, einfügen und Löschen von Registerkarten auf (keine) ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image24.png))


Konfigurieren Sie nach Abschluss des Assistenten ObjectDataSource der DropDownList zum Anzeigen der `CategoryName` Feld "Daten" und für die Verwendung der `CategoryID` -Felds ist, als die `Value` für jede `ListItem`.

An diesem Punkt sollte DropDownList und ObjectDataSource s deklarative Markup etwa wie folgt:


[!code-aspx[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample3.aspx)]

Ziehen Sie anschließend eine GridView in den Designer, und platzieren es unterhalb der Dropdownliste aus. Legen Sie die GridView s `ID` auf `ProductsByCategory` und aus seinem Smarttag binden Sie es an eine neue ObjectDataSource mit dem Namen `ProductsByCategoryDataSource`. Konfigurieren der `ProductsByCategoryDataSource` ObjectDataSource verwenden die `ProductsBLLWithSprocs` -Klasse, wenn sich beide Komponenten rufen Sie die Daten mithilfe der `GetProductsByCategoryID(categoryID)` Methode. Seit dieser GridView nur zum Anzeigen von Daten verwendet werden wird, legen Sie die Dropdownlisten in der Update-, INSERT- und Löschen von Registerkarten (keine), und klicken Sie auf Weiter.


[![Konfigurieren der ObjectDataSource zur Verwendung der ProductsBLLWithSprocs-Klasse](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image26.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image25.png)

**Abbildung 9**: Konfigurieren der ObjectDataSource verwenden die `ProductsBLLWithSprocs` Klasse ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image27.png))


[![Abrufen von Daten aus der GetProductsByCategoryID(categoryID)-Methode](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image29.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image28.png)

**Abbildung 10**: Abrufen von Daten aus der `GetProductsByCategoryID(categoryID)` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image30.png))


Die Methode, die in der Registerkarte "SELECT" ausgewählt wurde erwartet einen Parameter aus, damit der letzte Schritt des Assistenten für die Quelle des Parameters s uns aufgefordert werden. Festlegen der Parameter-Quellliste Dropdown-Steuerelement, und wählen Sie die `Categories` -Steuerelement aus der Dropdownliste ControlID. Klicken Sie auf ' Fertig stellen ', um den Assistenten abzuschließen.


[![Verwenden der DropDownList Kategorien als Quelle für die CategoryID-Parameter](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image32.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image31.png)

**Abbildung 11**: Verwenden der `Categories` DropDownList als Quelle für die `categoryID` Parameter ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image33.png))


Nach Abschluss des Assistenten ObjectDataSource wird Visual Studio BoundFields und eine CheckBoxField für jedes der Felder Product Daten hinzufügen. Wahlweise können Sie diese Felder anpassen, nach Bedarf.

Besuchen Sie die Seite über einen Browser. Beim Zugriff auf die Seite die Kategorie "Getränke" ausgewählt ist und die entsprechenden Produkte im Raster aufgeführt. Ändern die Dropdown-Liste in einer alternativen Kategorie als Abbildung 12 zeigt, bewirkt ein Postback und lädt das Raster mit den Produkten der neu ausgewählte Kategorie.


[![Die Produkte in der Kategorie erzeugen werden angezeigt.](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image35.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image34.png)

**Abbildung 12**: der Produkte in der Kategorie zu erzeugen, werden angezeigt ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image36.png))


## <a name="step-5-wrapping-a-stored-procedure-s-statements-within-the-scope-of-a-transaction"></a>Schritt 5: Wrapping s-Anweisungen eine gespeicherte Prozedur innerhalb des Bereichs einer Transaktion

In der [Umbruch Datenbankänderungen innerhalb einer Transaktion](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs.md) Lernprogramm erörterten Techniken zum Ausführen einer Reihe von Datenbank-änderungsanweisungen innerhalb des Bereichs einer Transaktion. Wie bereits erwähnt, die die Änderungen des wirkungsfelds einer Transaktion ausgeführt wurde, entweder alle erfolgreich ausgeführt werden, oder alle Fehler, die Unteilbarkeit gewährleistet. Methoden für die Verwendung von Transaktionen umfassen Folgendes:

- Verwenden der Klassen in der `System.Transactions` -Namespace
- Die Datenzugriffsebene nutzen ADO.NET-Klassen wie `SqlTransaction`, und
- Hinzufügen der T-SQL-Transaktion-Befehle direkt in der gespeicherten Prozedur

Die *Umbruch Datenbankänderungen innerhalb einer Transaktion* Lernprogramm die ADO.NET-Klassen in der DAL verwendet. Im weiteren Verlauf dieses Lernprogramms untersucht, wie eine Transaktion mit T-SQL-Befehlen innerhalb einer gespeicherten Prozedur zu verwalten.

Die drei wichtigsten SQL-Befehle für manuell starten, Commit und Rollback einer Transaktion sind `BEGIN TRANSACTION`, `COMMIT TRANSACTION`, und `ROLLBACK TRANSACTION`zugeordnet. Wie mit den ADO.NET-Ansatz, wenn Transaktionen in einer gespeicherten Prozedur verwenden, wir das folgende Muster angewendet müssen:

1. Geben Sie an den Anfang einer Transaktion.
2. Führen Sie die SQL-Anweisungen, die die Transaktion zu bilden.
3. Wenn ein Fehler in einer der Anweisungen aus Schritt2, Rollback der Transaktion vorhanden ist
4. Wenn alle Anweisungen aus Schritt2 ohne Fehler abgeschlossen werden, ein commit der Transaktions.

Dieses Muster kann in T-SQL-Syntax, die mithilfe der folgenden Vorlage implementiert werden:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample4.sql)]

Die Vorlage beginnt mit dem Definieren einer `TRY...CATCH` ein Konstrukt, das noch nicht mit SQL Server 2005 zu blockieren. LIKE mit `try...catch` Blöcke in C#-, SQL `TRY...CATCH` Block wird ausgeführt, die Anweisungen in der `TRY` Block. Wenn eine Anweisung einen Fehler auslöst, wird die Steuerung sofort an die `CATCH` Block.

Wenn keine Fehler Ausführen der SQL-Anweisungen, Zusammensetzung der Transaktion die `COMMIT TRANSACTION` -Anweisung führt einen Commit der Änderungen und schließt die Transaktion. Wenn jedoch einer der Anweisungen zu einem Fehler führt der `ROLLBACK TRANSACTION` in die `CATCH` Block wird die Datenbank in den Zustand vor dem Start der Transaktion. Die gespeicherte Prozedur auch löst einen Fehler mit der [RAISERROR-Befehl](https://msdn.microsoft.com/library/ms178592.aspx), wodurch eine `SqlException` , die in der Anwendung ausgelöst werden.

> [!NOTE]
> Da die `TRY...CATCH` Block ist neu in SQL Server 2005, die oben stehende Vorlage kann nicht ausgeführt werden, wenn Sie ältere Versionen von Microsoft SQL Server verwenden. Wenn Sie SQL Server 2005 nicht verwenden, wenden Sie sich an [Verwalten von Transaktionen in gespeicherten Prozeduren von SQL Server](http://www.4guysfromrolla.com/webtech/080305-1.shtml) für eine Vorlage, die mit anderen Versionen von SQL Server verwendet werden kann.


Lassen Sie s eine konkrete Beispiel betrachten. Eine foreign Key-Einschränkung besteht zwischen der `Categories` und `Products` Tabellen, was bedeutet, dass jedes `CategoryID` -Feld in der `Products` Tabelle zuordnen muss eine `CategoryID` Wert in der `Categories` Tabelle. Alle Aktionen, die verletzen würde diese Einschränkung, z. B. versuchen, eine Kategorie zu löschen, die Produkte verknüpft ist, führt zu einer Verletzung der foreign Key-Einschränkung. Um dies zu überprüfen, rufen Sie das Beispiel aktualisieren und Löschen von vorhandenen Binärdaten in das Arbeiten mit Binärdaten Abschnitt (`~/BinaryData/UpdatingAndDeleting.aspx`). Auf dieser Seite sind die einzelnen Kategorien im System zusammen mit den Schaltflächen "Bearbeiten" und "Delete" (siehe Abbildung 13), aber wenn Sie versuchen, eine Kategorie löschen, Produkte,-z. B. Getränke - zugeordnet sind, die Delete schlägt fehl, aufgrund einer Verletzung der foreign Key-Einschränkung (siehe Abbildung 14).


[![Jede Kategorie wird in einem GridView mit bearbeiten und Löschen von Schaltflächen angezeigt.](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image38.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image37.png)

**Abbildung 13**: jede Kategorie wird angezeigt, in einem GridView mit den Schaltflächen löschen und bearbeiten ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image39.png))


[![Sie können eine Kategorie löschen, vorhandene Produkte hat](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image41.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image40.png)

**Abbildung 14**: eine Kategorie, die vorhandene Produkte kann nicht gelöscht werden ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image42.png))


Angenommen Sie, jedoch, dass wir erlauben Kategorien gelöscht werden soll, unabhängig davon, ob sie Produkte verknüpft haben möchten. Eine Kategorie mit Produkte gelöscht werden soll, angenommen, wir auch löschen, deren vorhandene Produkte möchten (obwohl eine andere Möglichkeit wäre, seine Produkte einfach festzulegende `CategoryID` Werte `NULL`). Diese Funktionalität kann durch die Cascade-Regeln der foreign Key-Einschränkung implementiert werden. Es konnte auch eine gespeicherte Prozedur, die akzeptiert erstellt eine `@CategoryID` Eingabeparameter und beim Aufrufen, löscht explizit alle die zugehörigen Produkte, und klicken Sie dann die angegebene Kategorie.

Unsere erste Versuch zum solche gespeicherte Prozedur kann wie folgt aussehen:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample5.sql)]

Während dies die zugehörige Produkte und die Kategorie definitiv gelöscht werden, ist dies nicht des wirkungsfelds einer Transaktion erfolgen. Stellen Sie sich vor, dass einige andere foreign Key-Einschränkung auf `Categories` würde, die das Löschen eines bestimmten verbieten `@CategoryID` Wert. Das Problem ist, dass in einem solchen Fall alle Produkte gelöscht werden, bevor wir versuchen, das die Kategorie zu löschen. Das Ergebnis darin, dass für eine Kategorie, der diese gespeicherte Prozedur alle seine Produkte entfernen würden während die Kategorie verblieben sind, da er weiterhin Datensätze in einer anderen Tabelle verknüpft wurde.

Wenn die gespeicherte Prozedur innerhalb des Bereichs einer Transaktion, jedoch die Löschvorgänge zu umschlossen wurden die `Products` Tabelle würde auf ein Rollback bei einem Fehler beim Löschen `Categories`. Das folgende Skript für die gespeicherte Prozedur eine Transaktion verwendet, um die Unteilbarkeit zwischen den beiden gewährleisten `DELETE` Anweisungen:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample6.sql)]

Hinzuzufügende erkundet die `Categories_Delete` gespeicherte Prozedur mit der Datenbank Northwind. Verweisen Sie zurück zu Schritt 1 Anleitungen zum Hinzufügen von gespeicherten Prozeduren in einer Datenbank.

## <a name="step-6-updating-thecategoriestableadapter"></a>Schritt 6: Aktualisieren der`CategoriesTableAdapter`

Bei der es hinzugefügt haben die `Categories_Delete` gespeicherte Prozedur mit der Datenbank, die DAL ist derzeit so konfiguriert, dass um Ad-hoc-SQL-Anweisungen zu verwenden, um den Löschvorgang auszuführen. Wir aktualisieren müssen die `CategoriesTableAdapter` und anweisen, um die Verwendung der `Categories_Delete` stattdessen gespeicherte Prozedur.

> [!NOTE]
> Weiter oben in diesem Lernprogramm wurden wir arbeiten mit den `NorthwindWithSprocs` DataSet. Aber, dass DataSet nur eine einzelne Entität `ProductsDataTable`, und wir arbeiten mit Kategorien müssen. Aus diesem Grund für den Rest dieses Lernprogramms, wenn ich das Data Access Layer I-m verweisen auf Erörtern der `Northwind` DataSet, das Projekt, das wir zuerst im erstellt die [erstellen eine Datenzugriffsschicht](../introduction/creating-a-data-access-layer-cs.md) Lernprogramm.


Öffnen des Datasets Northwind wählen die `CategoriesTableAdapter`, und fahren Sie mit dem Eigenschaftenfenster. Die Eigenschaften im Fensterlisten der `InsertCommand`, `UpdateCommand`, `DeleteCommand`, und `SelectCommand` TableAdapter als auch die Informationen und die Verbindungszeichenfolge verwendet. Erweitern Sie die `DeleteCommand` Eigenschaft zum Anzeigen von Details. Wie in Abbildung 15 gezeigt, die `DeleteCommand` s `ComamndType` Eigenschaftensatz auf Text, den angewiesen wird, senden Sie den Text in der `CommandText` Eigenschaft als Ad-hoc-SQL-Abfrage.


![Wählen Sie die CategoriesTableAdapter im Designer, um seine Eigenschaften im Eigenschaftenfenster anzuzeigen.](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image43.png)

**Abbildung 15**: Wählen Sie die `CategoriesTableAdapter` im Designer, um seine Eigenschaften im Eigenschaftenfenster anzuzeigen.


Um diese Einstellungen zu ändern, wählen Sie den Text (DeleteCommand) im Eigenschaftenfenster, und wählen Sie aus (neu) aus der Dropdown-Liste. Hiermit löschen Sie die Einstellungen für die `CommandText`, `CommandType`, und `Parameters` Eigenschaften. Legen Sie anschließend die `CommandType` Eigenschaft `StoredProcedure` , und geben Sie den Namen der gespeicherten Prozedur für die `CommandText` (`dbo.Categories_Delete`). Wenn Sie die Eigenschaften in dieser Reihenfolge - geben zuerst sicherstellen der `CommandType` und dann die `CommandText` -Visual Studio füllt automatisch die Parameters-Auflistung. Wenn Sie diese Eigenschaften nicht in dieser Reihenfolge eingeben, müssen Sie die Parameter durch den Parameter-Auflistungs-Editor manuell hinzufügen. In beiden Fällen wird es s unerlässlich, klicken auf die Auslassungspunkte im Parameters-Eigenschaft, um den Auflistungs-Editor von Parameter zu bringen, um sicherzustellen, dass die richtigen Parameter Einstellungen Änderungen vorgenommen wurden (siehe Abbildung 16). Wenn alle Parameter im Dialogfeld nicht angezeigt wird, fügen Sie der `@CategoryID` Parameter manuell (müssen nicht hinzufügen der `@RETURN_VALUE` Parameter).


![Stellen Sie sicher, dass die Parameter-Einstellungen richtig sind.](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image44.png)

**Abbildung 16**: Stellen Sie sicher, dass die Parameter-Einstellungen richtig sind.


Sobald die DAL aktualisiert wurde, löschen eine Kategorie automatisch Löschen aller seiner zugehörigen Produkte und dazu des wirkungsfelds einer Transaktion. Um dies zu überprüfen, auf die Seite aktualisieren und Löschen von vorhandenen Binärdaten zurückgeben Sie, und klicken Sie auf die Schaltfläche "löschen" für eine der Kategorien. Mit einem einzigen Mausklick der Maus werden der Kategorie und aller seiner zugehörigen Produkte gelöscht.

> [!NOTE]
> Vor dem Testen der `Categories_Delete` gespeicherte Prozedur, die eine Anzahl der Produkte zusammen mit der ausgewählten Kategorie gelöscht werden, ist es möglicherweise empfehlenswert, eine Sicherungskopie der Datenbank erstellen. Bei Verwendung der `NORTHWND.MDF` in Datenbank `App_Data`, einfach schließen Sie Visual Studio, und kopieren Sie die MDF- und LDF-Dateien in `App_Data` zu einem anderen Ordner. Nach dem Testen der Funktionalität können Sie die Datenbank wiederherstellen, indem Sie Visual Studio schließen und ersetzen die aktuellen MDF- und LDF-Dateien im `App_Data` mit der Sicherungskopien.


## <a name="summary"></a>Zusammenfassung

Während der TableAdapter-s-Assistent automatisch für uns gespeicherte Prozeduren generiert, stehen die Zeiten, wenn wir bereits solchen gespeicherten Prozeduren erstellt oder könnten sie stattdessen manuell oder mit anderen Tools erstellt werden soll. Um solche Szenarien zu unterstützen, kann auch die TableAdapter konfiguriert werden, um auf eine vorhandene gespeicherte Prozedur verweisen. In diesem Lernprogramm erläutert wird, wie gespeicherte Prozeduren manuell über die Visual Studio-Umgebung auf eine Datenbank hinzugefügt und die TableAdapter-s-Methoden, diese gespeicherten Prozeduren von Netzwerkdaten. Außerdem wird untersucht die T-SQL-Befehle und das Skript-Muster, die zum Starten und Ausführen eines Commits für Rollbacks von Transaktionen von innerhalb einer gespeicherten Prozedur.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurden Hilton Geisenow S Ren Jacob Lauritsen und Teresa Murphy. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)
> [Weiter](updating-the-tableadapter-to-use-joins-cs.md)
