---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
title: Verwenden von vorhandenen gespeicherten Prozeduren für TableAdapter-Steuerelemente des typisierten DataSet (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Im vorherigen Tutorial haben wir wie den TableAdapter-Assistenten verwenden, um neue gespeicherte Prozeduren zu generieren. In diesem Tutorial erfahren Sie, wie demselben TableAdapter...
ms.author: riande
ms.date: 07/18/2007
ms.assetid: 2da25f6a-757e-4e7b-a812-1575288d8f7a
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
msc.type: authoredcontent
ms.openlocfilehash: 8860f0ac9c3026fcf83a3eb7e6baecf2163964d1
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825848"
---
<a name="using-existing-stored-procedures-for-the-typed-datasets-tableadapters-vb"></a>Verwenden von vorhandenen gespeicherten Prozeduren für TableAdapter-Steuerelemente des typisierten DataSet (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_68_VB.zip) oder [PDF-Datei herunterladen](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/datatutorial68vb1.pdf)

> Im vorherigen Tutorial haben wir wie den TableAdapter-Assistenten verwenden, um neue gespeicherte Prozeduren zu generieren. In diesem Lernprogramm erfahren wir, wie die denselben Assistenten für TableAdapter mit gespeicherten Prozeduren arbeiten können. Wir erfahren Sie, wie unsere Datenbank neue gespeicherte Prozeduren manuell hinzugefügt werden kann.


## <a name="introduction"></a>Einführung

In der [vorherigen Lernprogramm](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) erläutert, wie das typisierte DataSet-s TableAdapters konfiguriert werden konnte, um gespeicherte Prozeduren für den Zugriff Daten anstelle von Ad-hoc-SQL-Anweisungen verwenden. Insbesondere untersucht wir, wie Sie den TableAdapter-Assistenten, die diese gespeicherten Prozeduren automatisch zu erstellen. Bei der Portierung von einer älteren Anwendung zu ASP.NET 2.0 oder eine ASP.NET 2.0-Website, um ein vorhandenes Datenmodell zu erstellen, ist es wahrscheinlich, dass die Datenbank enthält bereits die gespeicherten Prozeduren, die wir benötigen. Sie können auch empfiehlt es sich um die gespeicherten Prozeduren zu erstellen, manuell oder über ein Tool als dem TableAdapter-Assistenten, der die gespeicherten Prozeduren automatisch generiert.

In diesem Tutorial werden wir konfigurieren den TableAdapter um vorhandene gespeicherte Prozeduren verwenden betrachten. Da die Northwind-Datenbank nur eine kleine Gruppe von integrierten gespeicherten Prozeduren verfügt, werden wir auch die erforderlichen Schritte zum neue gespeicherte Prozeduren der Datenbank über Visual Studio-Umgebung manuell hinzufügen ansehen. Lassen Sie s beginnen!

> [!NOTE]
> In der [Umschließen von Datenbankänderungen innerhalb einer Transaktion](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) Tutorial wir Methoden hinzugefügt, dem TableAdapter zum Unterstützen von Transaktionen (`BeginTransaction`, `CommitTransaction`und so weiter). Alternativ können Transaktionen vollständig innerhalb einer gespeicherten Prozedur erfordert keine Änderungen am Code der Datenzugriffsebene verwaltet werden. In diesem Lernprogramm lernen wir die T-SQL-Befehle verwendet, um eine gespeicherte Prozedur s-Anweisungen innerhalb des Bereichs einer Transaktion auszuführen.


## <a name="step-1-adding-stored-procedures-to-the-northwind-database"></a>Schritt 1: Hinzufügen von gespeicherten Prozeduren mit der Datenbank Northwind

Visual Studio erleichtert das Hinzufügen von neuen gespeicherten Prozeduren in einer Datenbank. Let s hinzufügen eine neue gespeicherte Prozedur zur Northwind-Datenbank, die alle Spalten zurückgibt. die `Products` mit einer bestimmten Tabelle `CategoryID` Wert. Erweitern Sie im Server-Explorer-Fenster die Northwind-Datenbank ein, sodass die Ordner - Datenbankdiagramme, Tabellen, Ansichten und So weiter – angezeigt werden. Wie wir im vorherigen Tutorial gesehen haben, enthält der Ordner für gespeicherte Prozeduren der Datenbank s vorhandene gespeicherte Prozeduren. Um eine neue gespeicherte Prozedur hinzuzufügen, mit der rechten Maustaste in des Ordners gespeicherte Prozeduren und wählen Sie im Kontextmenü die Option neue gespeicherte Prozedur hinzufügen.


[![Mit der rechten Maustaste in des Ordners gespeicherte Prozeduren, und fügen Sie eine neue gespeicherte Prozedur hinzu.](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image2.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image1.png)

**Abbildung 1**: mit der rechten Maustaste in den Ordner für gespeicherte Prozeduren und Hinzufügen einer neuen gespeicherten Prozedur ([klicken Sie, um das Bild in voller Größe anzeigen](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image3.png))


Wie in Abbildung 1 gezeigt, die neue gespeicherte Prozedur hinzufügen-Option wird ein Skriptfenster geöffnet in Visual Studio mit der Gliederung des SQL-Skripts zum Erstellen der gespeicherten Prozedur erforderlich sind. Es ist unsere Aufgabe Ausarbeitung dieses Skript aus, und führen Sie es, die an diesem Punkt der Datenbank die gespeicherte Prozedur hinzugefügt werden.

Geben Sie das folgende Skript aus:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample1.sql)]

Dieses Skript, bei der Ausführung wird eine neue gespeicherte Prozedur hinzuzufügen, mit der Northwind-Datenbank, die mit dem Namen `Products_SelectByCategoryID`. Diese gespeicherte Prozedur nimmt einen einzelnen Eingabeparameter (`@CategoryID`, des Typs `int`) und alle Felder für diese Produkte mit einem entsprechenden `CategoryID` Wert.

Zum Ausführen dieser `CREATE PROCEDURE` Skript und fügen Sie die gespeicherte Prozedur in der Datenbank, klicken Sie auf das Symbol "Speichern" in der Symbolleiste oder STRG + S drücken. Zeigt die neu erstellte nach dem dies der Fall, die gespeicherten Prozeduren Ordner aktualisiert, gespeicherte Prozedur aus. Darüber hinaus ändert das Skript im Fenster Besonderheit aus `CREATE PROCEDURE dbo.Products_SelectProductByCategoryID` zu `ALTER PROCEDURE` `dbo.Products_SelectProductByCategoryID`. `CREATE PROCEDURE` Fügt eine neue gespeicherte Prozedur in der Datenbank während `ALTER PROCEDURE` aktualisiert eine bereits vorhandene. Seit dem Start des Skripts geändert hat `ALTER PROCEDURE`, ändern die gespeicherten Prozeduren eingeben, Parameter oder SQL-Anweisungen, und klicken Sie auf das Symbol "Speichern", wird die gespeicherte Prozedur mit diesen Änderungen aktualisiert.

Abbildung 2 zeigt Visual Studio, nachdem die `Products_SelectByCategoryID` gespeicherte Prozedur wurde gespeichert.


[![Die gespeicherte Prozedur Products_SelectByCategoryID wurde in der Datenbank hinzugefügt](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image5.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image4.png)

**Abbildung 2**: die gespeicherte Prozedur `Products_SelectByCategoryID` wurde in der Datenbank hinzugefügt wurden ([klicken Sie, um das Bild in voller Größe anzeigen](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image6.png))


## <a name="step-2-configuring-the-tableadapter-to-use-an-existing-stored-procedure"></a>Schritt 2: Konfigurieren den TableAdapter um eine vorhandene gespeicherte Prozedur zu verwenden.

Nachdem die `Products_SelectByCategoryID` gespeicherte Prozedur wurde in der Datenbank konfigurieren wir unsere Data Access Layer, um diese gespeicherte Prozedur zu verwenden, wenn eine ihrer Methoden aufgerufen wird. Insbesondere fügen wir eine `GetProducstByCategoryID(<_i22_>categoryID)<!--_i22_-->` Methode, um die `ProductsTableAdapter` in die `NorthwindWithSprocs` typisierte DataSet, die aufruft, die `Products_SelectByCategoryID` gespeicherte Prozedur, die wir gerade erstellt haben.

Öffnen Sie zunächst die `NorthwindWithSprocs` DataSet. Mit der rechten Maustaste auf die `ProductsTableAdapter` , und wählen Sie die Abfrage hinzufügen, um den TableAdapter-Abfrage-Konfigurations-Assistenten zu starten. In der [vorherigen Lernprogramm](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) ausgewählt, um den TableAdapter, erstellen Sie eine neue gespeicherte Prozedur für uns zu haben. In diesem Tutorial allerdings möchten wir, die neue Methode des TableAdapter zu verknüpfen, mit dem vorhandenen `Products_SelectByCategoryID` gespeicherte Prozedur. Daher wählen Sie die Option vorhandene gespeicherte Prozedur verwenden, aus dem ersten Schritt des Assistenten-s, und klicken Sie dann auf Weiter.


[![Wählen Sie die vorhandene gespeicherte Prozedur-Option verwenden](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image8.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image7.png)

**Abbildung 3**: Wählen Sie die vorhandene gespeicherte Prozedur Option ([klicken Sie, um das Bild in voller Größe anzeigen](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image9.png))


Der folgende Bildschirm bietet eine Dropdown-Liste mit der s-Datenbank gespeicherte Prozeduren aufgefüllt. Eine gespeicherte Prozedur auswählen, sind seine Eingabeparameter auf der linken Seite und die Datenfelder zurückgegeben (sofern vorhanden) auf der rechten Seite aufgeführt. Wählen Sie die `Products_SelectByCategoryID` gespeicherte Prozedur aus der Liste aus, und klicken Sie auf Weiter.


[![Wählen Sie die Products_SelectByCategoryID gespeicherten Prozedur](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image11.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image10.png)

**Abbildung 4**: Wählen Sie die `Products_SelectByCategoryID` Stored Procedure ([klicken Sie, um das Bild in voller Größe anzeigen](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image12.png))


Im nächste Bildschirm werden wir gefragt, welche Art von Daten von der gespeicherten Prozedur zurückgegeben wird, und hier beantwortet bestimmt den Typ, der von der TableAdapter-s-Methode zurückgegeben. Angenommen, wir angeben, dass es sich bei tabellarischen Daten zurückgegeben werden, die Methode gibt eine `ProductsDataTable` Instanz, die mit den von der gespeicherten Prozedur zurückgegebenen Datensätzen aufgefüllt. Im Gegensatz dazu Wenn, dass diese gespeicherte Prozedur einen einzelnen Wert zurückgibt, geben wir der TableAdapter zurück eine `Object` , die den Wert in der ersten Spalte der ersten von der gespeicherten Prozedur zurückgegebenen Datensatz zugewiesen ist.

Da die `Products_SelectByCategoryID` gespeicherte Prozedur gibt alle Produkte, die einer bestimmten Kategorie gehören, wählen Sie die erste Antwort - Tabellendaten - aus, und klicken Sie auf Weiter.


[![Anzugeben Sie, dass die gespeicherte Prozedur Tabellendaten zurück.](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image14.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image13.png)

**Abbildung 5**: anzugeben, dass die gespeicherte Prozedur Tabellendaten zurückgibt ([klicken Sie, um das Bild in voller Größe anzeigen](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image15.png))


Übrig bleibt, um anzugeben, welche Methode-Muster verwenden die Namen für diese Methoden folgen. Lassen Sie eine DataTable und die Rückgabe, eine DataTable-Optionen aktiviert, aber benennen Sie die Methoden, die, beide der Füllung `FillByCategoryID` und `GetProductsByCategoryID`. Klicken Sie dann auf Weiter, um eine Zusammenfassung der Aufgaben zu überprüfen, die der Assistent ausführt. Wenn alles korrekt aussieht, klicken Sie auf "Fertig stellen".


[![Namen der Methoden FillByCategoryID und GetProductsByCategoryID](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image17.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image16.png)

**Abbildung 6**: Benennen Sie die Methoden `FillByCategoryID` und `GetProductsByCategoryID` ([klicken Sie, um das Bild in voller Größe anzeigen](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image18.png))


> [!NOTE]
> Die TableAdapter-Methoden, die wir gerade erstellt haben, `FillByCategoryID` und `GetProductsByCategoryID`, erwartet einen Eingabeparameter vom Typ `Integer`. Dieser Eingabeparameter-Wert wird übergeben, in der gespeicherten Prozedur über seine `@CategoryID` Parameter. Wenn Sie ändern die `Products_SelectByCategory` Parameter der gespeicherten Prozedur s, müssen Sie auch die Parameter für diese TableAdapter-Methoden zu aktualisieren. Siehe die [vorherigen Tutorial](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md), dies in zwei Arten möglich: manuell hinzufügen oder Entfernen von Parametern aus der Parameterauflistung oder erneut den TableAdapter-Assistenten auszuführen.


## <a name="step-3-adding-agetproductsbycategoryidcategoryidmethod-to-the-bll"></a>Schritt 3: Hinzufügen einer`GetProductsByCategoryID(categoryID)`Methode, um die BLL

Mit der `GetProductsByCategoryID` DAL-Methode abgeschlossen, der nächste Schritt besteht Zugriff auf diese Methode in der Geschäftslogikebene. Öffnen der `ProductsBLLWithSprocs` Klassendatei, und fügen Sie die folgende Methode hinzu:


[!code-vb[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample2.vb)]

Einfach diese Geschäftslogikschicht-Methode gibt zurück, die `ProductsDataTable` zurückgegeben, die von der `ProductsTableAdapter` s `GetProductsByCategoryID` Methode. Die `DataObjectMethodAttribute` Attribut enthält Metadaten, die von der "ObjectDataSource"-s-Konfigurieren von Datenquellen-Assistent verwendet. Diese Methode wird vor allem in der Registerkarte "SELECT" s Dropdown-Liste angezeigt.

## <a name="step-4-displaying-products-by-category"></a>Schritt 4: Produkte nach Kategorie anzeigen

So testen Sie die neu hinzugefügte `Products_SelectByCategoryID` gespeicherte Prozedur und die entsprechenden DAL und BLL-Methoden können s eine ASP.NET-Seite erstellen, die einem DropDownList-Steuerelement und einer GridView-Ansicht enthält. Der Dropdownliste werden alle Kategorien in der Datenbank aufgelistet, während die GridView, die Produkte der ausgewählten Kategorie angezeigt wird.

> [!NOTE]
> Wir haben erstellt Master-/Detailberichten Schnittstellen DropDownList-Steuerelementen in vorherigen Tutorials verwenden. Ausführlichere Informationen zum Implementieren eines solchen Master/Detail-Berichts, finden Sie in der [Master/Detail-Filtern mit einer DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) Tutorial.


Öffnen der `ExistingSprocs.aspx` auf der Seite die `AdvancedDAL` Ordner, und ziehen Sie einem DropDownList-Steuerelement aus der Toolbox in den Designer. Legen Sie die DropDownList-Zuordnungsvorgänge `ID` Eigenschaft `Categories` und die zugehörige `AutoPostBack` Eigenschaft `True`. Binden Sie als Nächstes aus sein Smarttag, DropDownList an eine neue, mit dem Namen "ObjectDataSource" `CategoriesDataSource`. Dem ObjectDataSource-Steuerelement so konfigurieren, dass, die Daten aus der abgerufen die `CategoriesBLL` Klasse s `GetCategories` Methode. Legen Sie die Dropdownlisten in der Update-, INSERT-, und löschen Sie die Registerkarten auf (keine).


[![Abrufen von Daten aus der CategoriesBLL Klasse s GetCategories-Methode](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image20.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image19.png)

**Abbildung 7**: Abrufen von Daten aus der `CategoriesBLL` Klasse s `GetCategories` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image21.png))


[![Legen Sie die Dropdownlisten in der Update-, INSERT- und DELETE werden Registerkarten (keine)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image23.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image22.png)

**Abbildung 8**: Legen Sie die Dropdownlisten in der Update-, INSERT- und Löschen von Registerkarten auf (keine) ([klicken Sie, um das Bild in voller Größe anzeigen](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image24.png))


Konfigurieren Sie nach Abschluss des Assistenten "ObjectDataSource" DropDownList zum Anzeigen der `CategoryName` Feld "Daten" und für die Verwendung der `CategoryID` als Feld der `Value` für jede `ListItem`.

An diesem Punkt sollten die DropDownList und "ObjectDataSource" s deklarative Markup etwa wie folgt:


[!code-aspx[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample3.aspx)]

Ziehen Sie anschließend einer GridView-Ansicht auf den Designer, und platzieren es unterhalb der Dropdownliste aus. Legen Sie die GridView s `ID` zu `ProductsByCategory` und von sein Smarttag, binden Sie es an eine neue, mit dem Namen "ObjectDataSource" `ProductsByCategoryDataSource`. Konfigurieren der `ProductsByCategoryDataSource` "ObjectDataSource" Verwenden der `ProductsBLLWithSprocs` -Klasse, dass rufen Sie die Daten mithilfe der `GetProductsByCategoryID(categoryID)` Methode. Da diese GridView nur zum Anzeigen von Daten verwendet werden wird, legen Sie die Dropdownlisten in der Update-, INSERT-, löschen Sie Registerkarten (keine) und klicken Sie auf Weiter.


[![Konfigurieren von dem ObjectDataSource-Steuerelement zur Verwendung der ProductsBLLWithSprocs-Klasse](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image26.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image25.png)

**Abbildung 9**: Konfigurieren von dem ObjectDataSource-Steuerelement verwenden die `ProductsBLLWithSprocs` Klasse ([klicken Sie, um das Bild in voller Größe anzeigen](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image27.png))


[![Abrufen von Daten aus der GetProductsByCategoryID(categoryID)-Methode](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image29.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image28.png)

**Abbildung 10**: Abrufen von Daten aus der `GetProductsByCategoryID(categoryID)` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image30.png))


Die Methode, die in der Registerkarte "SELECT" ausgewählt erwartet einen Parameter, damit der letzte Schritt des Assistenten uns für die Parameter-s-Quelle fordert. Der Parameter-Quellliste Dropdown-Steuerelement festgelegt, und wählen Sie die `Categories` Steuerelement aus der ControlID Dropdown-Liste. Klicken Sie auf "Fertig stellen", um den Assistenten abzuschließen.


[![Verwenden Sie die Categories DropDownList als Quelle für die CategoryID-Parameter](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image32.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image31.png)

**Abbildung 11**: Verwenden der `Categories` DropDownList als Quelle für die `categoryID` Parameter ([klicken Sie, um das Bild in voller Größe anzeigen](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image33.png))


Nach Abschluss des ObjectDataSource-Steuerelement-Assistenten, wird Visual Studio BoundFields und eine CheckBoxField für jedes Produkt Datenfelder hinzufügen. Können Sie diese Felder anpassen nach Bedarf.

Besuchen Sie die Seite über einen Browser ein. Beim Zugriff auf die Seite, die die Kategorie "Getränke" ausgewählt ist und die entsprechenden Produkte, die im Raster aufgelistet. Ändern die Dropdown-Liste in eine andere Kategorie als Abbildung 12 zeigt ein Postback auslöst und lädt das Raster mit den Produkten, die neu ausgewählte Kategorie.


[![Die Produkte in der Kategorie "erstellen" werden angezeigt.](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image35.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image34.png)

**Abbildung 12**: der Produkte in der Kategorie "erstellen" werden angezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image36.png))


## <a name="step-5-wrapping-a-stored-procedure-s-statements-within-the-scope-of-a-transaction"></a>Schritt 5: Umschließen eine gespeicherte Prozedur-s-Anweisungen innerhalb des Bereichs einer Transaktion

In der [Umschließen von Datenbankänderungen innerhalb einer Transaktion](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb.md) Lernprogramm erläutert Techniken zum Ausführen einer Reihe von Datenbank-änderungsanweisungen innerhalb des Bereichs einer Transaktion. Rückruf, der die Änderungen, die unter dem Dach einer Transaktion ausgeführt wurde, entweder alle erfolgreich ausgeführt werden, oder alle Fehler, die Garantie der Unteilbarkeit. Methoden für die Verwendung von Transaktionen umfassen Folgendes:

- Mithilfe der Klassen in der `System.Transactions` -Namespace
- Verwenden Sie mit der Datenzugriffsebene ADO.NET-Klassen wie `SqlTransaction`, und
- Hinzufügen der T-SQL-Transaktion-Befehle direkt in der gespeicherten Prozedur

Die *Umschließen von Datenbankänderungen innerhalb einer Transaktion* Tutorial die ADO.NET-Klassen in der DAL verwendet. Der Rest dieses Tutorials wird untersucht, wie eine Transaktion, die mithilfe von T-SQL-Befehlen in einer gespeicherten Prozedur zu verwalten.

Die drei wichtigsten SQL-Befehle für manuell starten, Commit und Rollback einer Transaktion sind `BEGIN TRANSACTION`, `COMMIT TRANSACTION`, und `ROLLBACK TRANSACTION`bzw. Wie Sie mit dem ADO.NET-Ansatz, nach der Verwendung von Transaktionen innerhalb einer gespeicherten Prozedur wir das folgende Muster anwenden müssen:

1. Geben Sie an den Anfang einer Transaktion.
2. Führen Sie die SQL-Anweisungen, aus die die Transaktion besteht.
3. Wenn ein Fehler in einer der Anweisungen in Schritt2, Rollback der Transaktion vorhanden ist.
4. Wenn alle Anweisungen aus Schritt2 ohne Fehler abgeschlossen, committen Sie die Transaktion aus.

Dieses Muster kann implementiert werden, in der T-SQL-Syntax, die mithilfe der folgenden Vorlage:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample4.sql)]

Die Vorlage beginnt mit dem Definieren einer `TRY...CATCH` blockieren, ein Konstrukt, das noch nicht mit SQL Server 2005. Wie beispielsweise `Try...Catch` , freigegebene Blöcke in Visual Basic SQL `TRY...CATCH` Block wird ausgeführt, die Anweisungen in der `TRY` Block. Wenn eine Anweisung einen Fehler auslöst, wird die Steuerung sofort an die `CATCH` Block.

Wenn keine Fehler auftreten, Zusammensetzung der Transaktion für die SQL-Anweisungen Ausführen der `COMMIT TRANSACTION` -Anweisung führt einen Commit der Änderungen und schließt die Transaktion. Wenn Sie jedoch eine der Anweisungen führt zu einem Fehler, die `ROLLBACK TRANSACTION` in die `CATCH` Block wird die Datenbank in den Zustand vor dem Start der Transaktion. Die gespeicherte Prozedur löst ebenfalls eine Fehlermeldung mit dem [RAISERROR-Befehl](https://msdn.microsoft.com/library/ms178592.aspx), bewirkt, dass eine `SqlException` , die in der Anwendung ausgelöst werden.

> [!NOTE]
> Da die `TRY...CATCH` Block ist neu in SQL Server 2005, die in der obige Vorlage funktioniert nicht, wenn Sie ältere Versionen von Microsoft SQL Server verwenden. Wenn Sie SQL Server 2005 nicht verwenden, wenden Sie sich an [Verwalten von Transaktionen in gespeicherten Prozeduren von SQL Server](http://www.4guysfromrolla.com/webtech/080305-1.shtml) für eine Vorlage, die mit anderen Versionen von SQL Server verwendet werden kann.


Lassen Sie s ein konkretes Beispiel betrachten. Eine foreign Key-Einschränkung vorhanden ist, zwischen der `Categories` und `Products` Tabellen, was bedeutet, dass jedes `CategoryID` Feld der `Products` Tabelle muss zugeordnet eine `CategoryID` Wert in der `Categories` Tabelle. Alle Aktionen, die gegen verstoßen würde, diese Einschränkung, wie z. B. versuchen, eine Kategorie löschen, die Produkte verknüpft ist, führt zu einer Verletzung der foreign Key-Einschränkung. Um dies zu überprüfen, rufen Sie erneut das Beispiel aktualisieren und Löschen von vorhandenen Binärdaten in das Arbeiten mit Binärdaten Abschnitt (`~/BinaryData/UpdatingAndDeleting.aspx`). Diese Seite listet jede Kategorie im System sowie Schaltflächen zum Bearbeiten und löschen (siehe Abbildung 13), aber wenn Sie versuchen, eine Kategorie löschen, die Produkte – z. B. Getränke - verknüpft ist der Löschvorgang schlägt fehl, aufgrund einer Verletzung der foreign Key-Einschränkung (siehe Abbildung 14).


[![Jede Kategorie wird in einer GridView-Ansicht mit bearbeiten und Löschen von Schaltflächen angezeigt.](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image38.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image37.png)

**Abbildung 13**: jede Kategorie wird in einer GridView-Ansicht mit bearbeiten und Löschen von Schaltflächen angezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image39.png))


[![Sie können keine Kategorie löschen, das vorhandene Produkte](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image41.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image40.png)

**Abbildung 14**: eine Kategorie aus, das vorhandene Produkte kann nicht gelöscht werden ([klicken Sie, um das Bild in voller Größe anzeigen](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image42.png))


Stellen Sie sich vor, jedoch, dass Kategorien gelöscht werden soll, unabhängig davon, ob sie Produkte verknüpft haben zulässig sein sollen. Sollte eine Kategorie mit Produkte gelöscht werden, stellen Sie sich vor, dass auch die vorhandenen Produkte gelöscht werden soll (obwohl eine andere Möglichkeit wäre, legen Sie einfach seine Produkte `CategoryID` Werte `NULL`). Diese Funktion kann über die Cascade-Regeln für foreign Key-Einschränkung implementiert werden. Alternativ können wir erstellen eine gespeicherte Prozedur, die akzeptiert eine `@CategoryID` Eingabeparameter und, wenn aufgerufen, löscht explizit alle die zugehörigen Produkte, und klicken Sie dann die angegebene Kategorie.

Unserem erste Versuch zur solche gespeicherte Prozedur kann wie folgt aussehen:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample5.sql)]

Während dies auf jeden Fall die zugehörige Produkte und Kategorie gelöscht werden, ist dies nicht unter dem Dach einer Transaktion erfolgen. Stellen Sie sich vor, dass es einige andere foreign Key-Einschränkung auf `Categories` würde, die das Löschen eines bestimmten verbieten `@CategoryID` Wert. Das Problem ist, dass in diesem Fall alle Produkte gelöscht werden, bevor wir versuchen, das die Kategorie zu löschen. Das Ergebnis ist, dass für eine Kategorie, diese gespeicherte Prozedur entfernt würde alle seine Produkte während die Kategorie geblieben, da es die Datensätze in einer anderen Tabelle immer noch verbunden ist.

Wenn die gespeicherte Prozedur innerhalb des Bereichs einer Transaktion, jedoch die Löschvorgänge umschlossen wurden die `Products` Tabelle würde auf ein Rollback bei einem Fehler beim Löschen `Categories`. Das folgende Skript für die gespeicherte Prozedur verwendet eine Transaktion, um die Unteilbarkeit zwischen den beiden gewährleisten `DELETE` Anweisungen:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample6.sql)]

Hinzufügen in Ruhe die `Categories_Delete` gespeicherte Prozedur mit der Datenbank Northwind. Verweisen Sie zurück zu Schritt 1 Anleitungen zum Hinzufügen von gespeicherten Prozeduren in einer Datenbank.

## <a name="step-6-updating-thecategoriestableadapter"></a>Schritt 6: Aktualisieren der`CategoriesTableAdapter`

Während wir hinzugefügt haben die `Categories_Delete` gespeicherte Prozedur in der Datenbank, der DAL ist gegenwärtig konfiguriert um Ad-hoc-SQL-Anweisungen zu verwenden, um den Löschvorgang auszuführen. Wir müssen aktualisieren die `CategoriesTableAdapter` und weisen Sie ihn zur Verwendung der `Categories_Delete` stattdessen gespeicherte Prozedur.

> [!NOTE]
> Weiter oben in diesem Tutorial wurden arbeiten wir mit der `NorthwindWithSprocs` DataSet. Aber, dass DataSet nur eine einzelne Entität `ProductsDataTable`, und wir arbeiten mit Kategorien müssen. Aus diesem Grund für den Rest dieses Tutorials aus, wenn ich das Data Access Layer I-m auf sprechen die `Northwind` diejenige, die wir zuerst im erstellt-DataSet der [Erstellen einer Datenzugriffsschicht](../introduction/creating-a-data-access-layer-vb.md) Tutorial.


Öffnen des Datasets Northwind Sie wählen die `CategoriesTableAdapter`, und wechseln Sie zu dem Fenster "Eigenschaften". Die Eigenschaften im Fenster umfassen die `InsertCommand`, `UpdateCommand`, `DeleteCommand`, und `SelectCommand` von TableAdapter als auch die Informationen und die Verbindungszeichenfolge verwendet. Erweitern Sie die `DeleteCommand` Eigenschaft, um dessen Details anzuzeigen. Wie in Abbildung 15 gezeigt, die `DeleteCommand` s `ComamndType` -Eigenschaftensatz auf Text, den angewiesen wird, senden Sie den Text in die `CommandText` Eigenschaft als Ad-hoc-SQL-Abfrage.


![Wählen Sie die CategoriesTableAdapter im Designer, um seine Eigenschaften im Eigenschaftenfenster anzuzeigen.](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image43.png)

**Abbildung 15**: Wählen Sie die `CategoriesTableAdapter` im Designer, um seine Eigenschaften im Eigenschaftenfenster anzuzeigen.


Um diese Einstellungen zu ändern, wählen Sie den Text (DeleteCommand) im Fenster Eigenschaften, und wählen Sie aus der Dropdown-Liste (neu). Hiermit löschen Sie die Einstellungen für die `CommandText`, `CommandType`, und `Parameters` Eigenschaften. Legen Sie als Nächstes die `CommandType` Eigenschaft `StoredProcedure` und geben Sie dann den Namen der gespeicherten Prozedur für die `CommandText` (`dbo.Categories_Delete`). Wenn Sie sicherstellen, um die Eigenschaften in dieser Reihenfolge – zuerst eingeben, die `CommandType` und klicken Sie dann die `CommandText` -Visual Studio füllt automatisch die Parameterauflistung. Wenn Sie diese Eigenschaften nicht in dieser Reihenfolge eingeben, müssen Sie die Parameter durch den Parameter-Auflistungs-Editor manuell hinzufügen. In beiden Fällen sie s rechnen, klicken auf die Auslassungspunkte in der Parameters-Eigenschaft, um den Parameter-Auflistungs-Editor zu öffnen, um sicherzustellen, dass die richtigen Parameter einstellungsänderungen vorgenommen wurden (finden Sie in Abbildung 16). Wenn Sie keine Parameter im Dialogfeld angezeigt werden, fügen Sie der `@CategoryID` Parameter manuell (Sie müssen nicht hinzufügen der `@RETURN_VALUE` Parameter).


![Stellen Sie sicher, dass die Einstellungen für die Parameter richtig sind.](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image44.png)

**Abbildung 16**: Stellen Sie sicher, dass die Einstellungen für die Parameter richtig sind.


Sobald die DAL aktualisiert wurde, löschen eine Kategorie automatisch Löschen aller seiner zugeordneten Produkte und unter dem Dach einer Transaktion tun. Um dies zu überprüfen, auf die Seite aktualisieren und Löschen von vorhandenen Binärdaten zurück, und klicken Sie auf die Schaltfläche "löschen" für eine der Kategorien. Mit nur einem Klick der Maus wird die Kategorie und alle seine zugehörige Produkte gelöscht werden.

> [!NOTE]
> Vor dem Testen der `Categories_Delete` gespeicherte Prozedur, die eine Reihe von Produkten zusammen mit der ausgewählten Kategorie gelöscht werden, es kann ratsam, eine Sicherungskopie der Datenbank sein. Bei Verwendung der `NORTHWND.MDF` Datenbank `App_Data`, einfach schließen Sie Visual Studio, und kopieren Sie die MDF- und LDF-Dateien in `App_Data` auf einem anderen Ordner. Nach dem Testen der Funktionalität können Sie die Datenbank wiederherstellen, indem Sie Visual Studio schließen, und ersetzen die aktuellen MDF und LDF-Dateien im `App_Data` durch die Sicherungskopien.


## <a name="summary"></a>Zusammenfassung

Der TableAdapter-s-Assistent automatisch gespeicherte Prozeduren für uns generiert werden, gibt es Zeiten, wenn wir möglicherweise bereits solchen gespeicherten Prozeduren, die erstellt oder möchten sie stattdessen manuell oder mit anderen Tools erstellen. Um solche Szenarien zu unterstützen, kann der TableAdapter auch konfiguriert werden, um auf eine vorhandene gespeicherte Prozedur verweisen. In diesem Tutorial erläutert, wie gespeicherte Prozeduren manuell über Visual Studio-Umgebung auf eine Datenbank hinzugefügt und die TableAdapter-s-Methoden mit dieser gespeicherten Prozeduren zu verknüpfen. Wir untersucht auch die T-SQL-Befehle und das Skript-Muster, die zum Starten, Commit und Rollback für Transaktionen innerhalb einer gespeicherten Prozedur.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial wurden Hilton Geisenow S Ren Jacob Lauritsen und Teresa Murphy. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
> [Weiter](updating-the-tableadapter-to-use-joins-vb.md)
