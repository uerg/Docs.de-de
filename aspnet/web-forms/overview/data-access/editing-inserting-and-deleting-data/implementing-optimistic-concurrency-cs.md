---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs
title: "Implementieren der vollständigen Parallelität (c#) | Microsoft Docs"
author: rick-anderson
description: "Für eine Webanwendung, die mehrere Benutzer Daten bearbeiten kann, besteht das Risiko, dass zwei Benutzer gleichzeitig dieselben Daten bearbeiten werden möglicherweise zur Verfügung. In diesem Tutori..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 56e15b33-93b8-43ad-8e19-44c6647ea05c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs
msc.type: authoredcontent
ms.openlocfilehash: a19e6c320838849e10d2aa397a23a0ee906bac22
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="implementing-optimistic-concurrency-c"></a>Implementieren der vollständigen Parallelität (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunterladen](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_21_CS.exe) oder [PDF herunterladen](implementing-optimistic-concurrency-cs/_static/datatutorial21cs1.pdf)

> Für eine Webanwendung, die mehrere Benutzer Daten bearbeiten kann, besteht das Risiko, dass zwei Benutzer gleichzeitig dieselben Daten bearbeiten werden möglicherweise zur Verfügung. In diesem Lernprogramm implementieren wir Steuerung durch vollständige Parallelität, um dieses Risiko zu behandeln.


## <a name="introduction"></a>Einführung

Für Webanwendungen, die nur Benutzer Daten anzeigen können, oder für Benutzer, die nur einen einzigen Benutzer enthalten, wer Daten ändern kann, besteht keine Gefahr von zwei gleichzeitigen Benutzern versehentliches Überschreiben von einer anderen Änderungen. Für Webanwendungen, die mehrere Benutzer zu aktualisieren oder Löschen von Daten ermöglichen, besteht jedoch das Potenzial für einen Benutzer Änderungen mit einem anderen gleichzeitigen Benutzer miteinander in Konflikt geraten. Ohne jede Parallelität Richtlinie vorhanden Wenn zwei Benutzer gleichzeitig einen einzelnen Datensatz bearbeiten werden der Benutzer, die Änderungen ein Commit ausgeführt, wird zuletzt die vom ersten vorgenommenen Änderungen überschrieben.

Angenommen Sie, dass zwei Benutzer, Jisun und Sam, sowohl eine Seite in der vorliegenden Anwendung anzeigten, die zulässig Besucher aktualisieren und löschen die Produkte über ein GridView-Steuerelement. Beide klicken Sie auf die Schaltfläche "Bearbeiten", in der GridView ungefähr zur selben Zeit. Jisun ändert sich der Name des Produkts in "Chai Tee" und klickt auf die Schaltfläche "Aktualisieren". Das Ergebnis ist ein `UPDATE` -Anweisung, die an die Datenbank gesendet wird, die festlegt *alle* das Produkt aktualisierbaren Felder (obwohl Jisun nur für ein Feld aktualisiert `ProductName`). Zu diesem Zeitpunkt weist die Datenbank die Werte "Chai Tee," die Kategorie Getränke Lieferanten Exotic Liquids für dieses bestimmte Produkt und so weiter. Allerdings zeigt GridView Sam Bildschirm als "Chai" weiterhin den Produktnamen in der bearbeitbaren GridView-Zeile. Einige Sekunden nach Jisuns Änderungen ein Commit ausgeführt wurde, wurden Sam aktualisiert die Kategorie "Gewürze" und klickt auf Update. Dies führt zu einer `UPDATE` Anweisung gesendet, um die Datenbank, die den Produktnamen, "Chai" legt die `CategoryID` zu den entsprechenden Getränke Kategorie-ID und So weiter. Der Jisun Änderungen an den Produktnamen wurde überschrieben. Abbildung 1 zeigt grafisch dieser Serie von Ereignissen.


[![Wenn zwei Benutzer gleichzeitig einen Datensatz zu aktualisieren ändert es s Potenzial für einen Benutzer zum Überschreiben von anderen](implementing-optimistic-concurrency-cs/_static/image2.png)](implementing-optimistic-concurrency-cs/_static/image1.png)

**Abbildung 1**: Wenn zwei Benutzer gleichzeitig Update ein Datensatz vorhanden s Potenzial für einen Benutzer geändert wird, überschreiben die anderen s ([klicken Sie hier, um das Bild in voller Größe angezeigt](implementing-optimistic-concurrency-cs/_static/image3.png))


Auf ähnliche Weise, wenn zwei Benutzer eine Seite besuchen, ein Benutzer möglicherweise in einem Datensatz aktualisieren, wenn sie von einem anderen Benutzer gelöscht wird. Alternativ dazu können Sie zwischen, wenn ein Benutzer eine Seite geladen wird, und wenn sie auf die Schaltfläche "löschen" klicken, ein anderer Benutzer den Inhalt für diesen Datensatz geändert worden sein.

Es gibt drei [parallelitätssteuerung](http://en.wikipedia.org/wiki/Concurrency_control) Strategien zur Verfügung:

- **Nichts Unternehmen** -ist gleichzeitiger Benutzer denselben Datensatz ändern, können Sie die letzten Commits win (das Standardverhalten)
- [**Vollständige Parallelität** ](http://en.wikipedia.org/wiki/Optimistic_concurrency_control) -gehen davon aus, die besteht möglicherweise Parallelität Konflikt hin und wieder, den Großteil der Zeit, die solche Konflikte auftreten, wird nicht; daher, wenn ein Konflikt einfach informieren Sie den Benutzer, die ihre Änderungen kann nicht gespeichert werden, weil ein anderer Benutzer die gleichen Daten geändert hat
- **Die eingeschränkte Parallelität** -Parallelitätskonflikte sind inzwischen und, dass der Benutzer wird nicht toleriert wird nicht angewiesen, ihre Änderungen wurden nicht gespeichert, aufgrund gleichzeitiger Aktivität eines anderen Benutzers angenommen; daher, wenn ein Benutzer Aktualisieren eines Datensatzes startet, Sie sperren , um anderen Benutzern bearbeiten oder löschen diesen Datensatz aus, bis der Benutzer die Änderungen ein Commit zu verhindern

Alle unsere Lernprogramme verwendet bisher haben Parallelität Auflösung Standardstrategie - nämlich haben wir den letzten Schreibvorgang gewinnen können. In diesem Lernprogramm untersuchen wir zum Implementieren der Steuerung durch vollständige Parallelität.

> [!NOTE]
> Es wird nicht durch eingeschränkte Parallelität Beispiele in diesem Lernprogramm Reihe betrachten. Die eingeschränkte Parallelität wird selten verwendet werden, weil z. B. gesperrt wurde, nicht ordnungsgemäß aufgegeben, kann verhindern, dass andere Benutzer aktualisieren von Daten. Z. B. wenn ein Benutzer einen Datensatz für die Bearbeitung gesperrt, und klicken Sie dann für den Tag vor dem entsperren verlässt, wird kein anderer Benutzer werden diesen Datensatz zu aktualisieren, bis der ursprünglichen Benutzer zurückgibt und seine standortupdates kann. Aus diesem Grund in Situationen, in denen die eingeschränkte Parallelität verwendet wird, besteht in der Regel ein Timeout, die erreicht wird, bricht die Sperre ab. Sales Ticket-Websites, die Sperren einen bestimmten sitzen-Speicherort für kurze Zeit, während der Benutzer mit der Order-ausgeführt wird, ist ein Beispiel der Steuerung durch eingeschränkte Parallelität.


## <a name="step-1-looking-at-how-optimistic-concurrency-is-implemented"></a>Schritt 1: Ansehen wie vollständige Parallelität wird implementiert.

Steuerung durch vollständige Parallelität funktioniert, indem Sie sicherstellen, dass der Datensatz aktualisiert oder gelöscht werden die gleichen Werte wie beim Aktualisieren oder Löschen von Prozess gestartet. Beim Klicken auf die Schaltfläche "Bearbeiten", in einem bearbeitbaren GridView "" sind die Werte des Datensatzes beispielsweise aus der Datenbank gelesen und in die Textfelder und anderen Websteuerelementen angezeigt. Diese ursprünglichen Werte werden durch die GridView gespeichert. Später, nachdem der Benutzer mit der ihre Änderungen und klickt auf die Schaltfläche "Aktualisieren", werden die ursprünglichen Werte sowie die neuen Werte der Geschäftslogikschicht und klicken Sie dann auf der Datenzugriffsebene gesendet. Die Datenzugriffsebene muss es sich um eine SQL-Anweisung ausgeben, die nur den Datensatz aktualisiert wird, wenn die ursprünglichen Werte, die der Benutzer bearbeiten gestartet, die Werte noch in der Datenbank identisch sind. Abbildung 2 zeigt diese Abfolge von Ereignissen.


[![Für die Update- oder Delete erfolgreich ausgeführt werden soll müssen die ursprünglichen Werte der aktuellen Datenbankwerte gleich sein.](implementing-optimistic-concurrency-cs/_static/image5.png)](implementing-optimistic-concurrency-cs/_static/image4.png)

**Abbildung 2**: für die Update- oder Delete SUCCEED, die ursprünglichen Werte müssen werden gleich der aktuellen Datenbankwerte ([klicken Sie hier, um das Bild in voller Größe angezeigt](implementing-optimistic-concurrency-cs/_static/image6.png))


Es gibt verschiedene Ansätze zum Implementieren der vollständigen Parallelität (finden Sie unter [Peter A. Bromberg](http://peterbromberg.net/)des [Optmistic Parallelität aktualisieren Logik](http://www.eggheadcafe.com/articles/20050719.asp) für einen kurzen Blick auf eine Reihe von Optionen). ADO.NET typisierte DataSet stellt eine Implementierung, die mit nur der Takt des ein Kontrollkästchen konfiguriert werden kann. Aktivieren vollständigen Parallelität für ein TableAdapter in das typisierte DataSet TableAdapters ergänzt `UPDATE` und `DELETE` Anweisungen, die einen Vergleich aller von den ursprünglichen Werten in der `WHERE` Klausel. Die folgenden `UPDATE` -Anweisung aktualisiert z. B. den Namen und den Preis eines Produkts nur, wenn die aktuellen Datenbankwerte die Werte gleich, die ursprünglich abgerufen wurden sind, wenn den Datensatz in die GridView zu aktualisieren. Die `@ProductName` und `@UnitPrice` Parameter enthalten die neuen Werten, die durch den Benutzer eingegeben werden, während `@original_ProductName` und `@original_UnitPrice` enthalten die Werte, die ursprünglich in die GridView geladen wurden, wenn auf die Schaltfläche "Bearbeiten" geklickt wurde:


[!code-sql[Main](implementing-optimistic-concurrency-cs/samples/sample1.sql)]

> [!NOTE]
> Dies `UPDATE` Anweisung wurde zur besseren Lesbarkeit vereinfacht. In der Praxis die `UnitPrice` Einchecken der `WHERE` Klausel wäre komplizierter seit `UnitPrice` darf `NULL` s und es wird überprüft, wenn `NULL = NULL` gibt immer "false" zurück (Sie müssen stattdessen `IS NULL`).


Zusätzlich zur Verwendung einer anderen zugrunde liegenden `UPDATE` -Anweisung, konfigurieren einen TableAdapter verwenden optimistische Parallelität ändert auch die Signatur von seiner DB direkte Methoden. Aus unserem ersten Lernprogramm, wissen Sie bereits [ *erstellen eine Datenzugriffsschicht*](../introduction/creating-a-data-access-layer-cs.md), die direkte DB-Methoden sind solche, die eine Liste von skalaren akzeptiert Werte als Eingabeparameter (statt einer stark typisierten DataRow oder DataTable-Instanz). Bei Verwendung von Parallelität, die direkte DB `Update()` und `Delete()` erfolgen Eingabeparameter für die ursprünglichen Werte ebenfalls. Darüber hinaus aktualisieren Sie den Code in die BLL für die Verwendung des Batchs Muster (die `Update()` methodenüberladungen, die Skalarwerte, anstatt DataRows und DataTables akzeptieren) muss ebenfalls geändert werden.

Stattdessen als Erweitern unsere vorhandenen DAL TableAdapters auf vollständige Parallelität (das Ändern der BLL aufnehmen einer würde), wir verwenden stattdessen Erstellen eines neuen typisierten Datasets mit dem Namen `NorthwindOptimisticConcurrency`, an denen wir Hinzufügen einer `Products` TableAdapter, vollständigen Parallelität wird verwendet. Nach, die wir erstellen eine `ProductsOptimisticConcurrencyBLL` Business Logic Layer-Klasse, die die entsprechenden Änderungen an der vollständige Parallelität DAL unterstützen verfügt. Nachdem diese-standortserverinstanz angeordnet wurden, müssen wir die ASP.NET-Seite zu erstellen sind.

## <a name="step-2-creating-a-data-access-layer-that-supports-optimistic-concurrency"></a>Schritt 2: Erstellen eine Datenzugriffsschicht, unterstützt optimistische Parallelität

Um einen neuen typisierte DataSet zu erstellen, mit der Maustaste auf die `DAL` Ordner innerhalb der `App_Code` Ordner, und fügen Sie ein neues DataSet mit dem Namen `NorthwindOptimisticConcurrency`. Wie wir im ersten Lernprogramm gesehen haben, wird dadurch das typisierte DataSet, wird der TableAdapter-Konfigurations-Assistent automatisch gestartet also einen neuen TableAdapter hinzugefügt. Im ersten Bildschirm wir aufgefordert, geben Sie die Datenbank, um eine Verbindung mit herstellen: Herstellen einer Verbindung mit der gleichen Datenbank Northwind mithilfe der `NORTHWNDConnectionString` Festlegen von `Web.config`.


[![Herstellen einer Verbindung mit der gleichen Northwind-Datenbank](implementing-optimistic-concurrency-cs/_static/image8.png)](implementing-optimistic-concurrency-cs/_static/image7.png)

**Abbildung 3**: Herstellen einer Verbindung mit der gleichen Northwind-Datenbank ([klicken Sie hier, um das Bild in voller Größe angezeigt](implementing-optimistic-concurrency-cs/_static/image9.png))


Wir werden als Nächstes aufgefordert, wie sich die Daten abzufragen: über eine Ad-hoc-SQL-Anweisung eine neue gespeicherte Prozedur oder eine vorhandene gespeicherte Prozedur. Da wir in unserer ursprünglichen DAL Ad-hoc-SQL-Abfragen verwendet, verwenden Sie diese Option hier ebenfalls.


[![Geben Sie die Daten zum Abrufen mithilfe einer Ad-hoc-SQL-Anweisung](implementing-optimistic-concurrency-cs/_static/image11.png)](implementing-optimistic-concurrency-cs/_static/image10.png)

**Abbildung 4**: Geben Sie die Daten abgerufen, die mit Ad-hoc-SQL-Anweisungen ([klicken Sie hier, um das Bild in voller Größe angezeigt](implementing-optimistic-concurrency-cs/_static/image12.png))


Geben Sie die SQL-Abfrage zu verwenden, um Produktinformationen abzurufen, auf dem folgenden Bildschirm. Verwenden wir die genaue dieselbe SQL-Abfrage, die verwendet werden, für die `Products` TableAdapter aus unserem ursprünglichen DAL, die alle zurückgibt der `Product` Spalten zusammen mit den Namen für das Produkt Supplier "und" Kategorie:


[!code-sql[Main](implementing-optimistic-concurrency-cs/samples/sample2.sql)]


[![Verwenden Sie die gleiche SQL-Abfrage aus dem TableAdapter Produkte in der ursprünglichen DAL](implementing-optimistic-concurrency-cs/_static/image14.png)](implementing-optimistic-concurrency-cs/_static/image13.png)

**Abbildung 5**: Verwenden Sie die gleiche SQL-Abfrage aus der `Products` TableAdapter im ursprünglichen DAL ([klicken Sie hier, um das Bild in voller Größe angezeigt](implementing-optimistic-concurrency-cs/_static/image15.png))


Vor dem Verschieben auf dem nächsten Bildschirm, klicken Sie auf die Schaltfläche "Erweiterte Optionen". Damit diese TableAdapter beschäftigen-Steuerung durch vollständige Parallelität, einfach das Kontrollkästchen Sie "Vollständige Parallelität verwenden".


[![Aktivieren der Steuerung durch vollständige Parallelität durch Überprüfen der &quot;vollständige Parallelität verwenden&quot; Kontrollkästchen](implementing-optimistic-concurrency-cs/_static/image17.png)](implementing-optimistic-concurrency-cs/_static/image16.png)

**Abbildung 6**: Aktivieren Sie Steuerung durch vollständige Parallelität durch Aktivieren des Kontrollkästchens "Verwenden der vollständigen Parallelität" ([klicken Sie hier, um das Bild in voller Größe angezeigt](implementing-optimistic-concurrency-cs/_static/image18.png))


Schließlich anzugeben Sie, dass der TableAdapter den Datenzugriffsmuster verwenden soll, die sowohl DataTable füllen und DataTable zurückgeben; auch Geben Sie an, dass die direkte DB-Methoden erstellt werden soll. Ändern Sie dem Methodennamen für die Rückgabe einer DataTable-Muster von GetData auf GetProducts, um den Benennungskonventionen widerspiegeln, die wir in unserer ursprünglichen DAL verwendet.


[![Nutzen alle Datenzugriffsmuster TableAdapter haben](implementing-optimistic-concurrency-cs/_static/image20.png)](implementing-optimistic-concurrency-cs/_static/image19.png)

**Abbildung 7**: haben Sie die TableAdapter nutzen alle Datenzugriffsmuster ([klicken Sie hier, um das Bild in voller Größe angezeigt](implementing-optimistic-concurrency-cs/_static/image21.png))


Nach Abschluss des Assistenten, der DataSet-Designer umfasst einen stark typisierten `Products` DataTable und TableAdapter. Erkundet DataTable aus umbenennen `Products` zu `ProductsOptimisticConcurrency`, wozu Sie verwenden können, indem Sie mit der rechten Maustaste auf die Titelleiste der Datentabelle und Umbenennen aus dem Kontextmenü auswählen.


[![Eine DataTable und TableAdapter wurden mit dem typisierten DataSet hinzugefügt](implementing-optimistic-concurrency-cs/_static/image23.png)](implementing-optimistic-concurrency-cs/_static/image22.png)

**Abbildung 8**: ein DataTable und TableAdapter hinzugefügt wurden, die typisierte DataSet ([klicken Sie hier, um das Bild in voller Größe angezeigt](implementing-optimistic-concurrency-cs/_static/image24.png))


Um die Unterschiede zwischen finden Sie unter der `UPDATE` und `DELETE` Abfragen zwischen den `ProductsOptimisticConcurrency` TableAdapter (der vollständigen Parallelität verwendet) und die Produkte TableAdapter (die keine), klicken Sie auf den TableAdapter, und wechseln Sie zu dem Eigenschaftenfenster. In der `DeleteCommand` und `UpdateCommand` Eigenschaften `CommandText` Untereigenschaften sehen Sie die aktuelle SQL-Syntax, die an die Datenbank gesendet wird, wenn der DAL Update- oder Delete-bezogenen Methoden aufgerufen werden. Für die `ProductsOptimisticConcurrency` TableAdapter die `DELETE` -Anweisung verwendet wird:


[!code-sql[Main](implementing-optimistic-concurrency-cs/samples/sample3.sql)]

Während der `DELETE` -Anweisung für den TableAdapter Produkt in unserem ursprünglichen DAL ist viel einfacher:


[!code-sql[Main](implementing-optimistic-concurrency-cs/samples/sample4.sql)]

Wie Sie sehen können, die `WHERE` -Klausel in der `DELETE` -Anweisung für den TableAdapter, die vollständigen Parallelität verwendet, enthält einen Vergleich zwischen den einzelnen der `Product` Tabelle vorhandenen Spaltenwerte und die ursprünglichen Werte zum Zeitpunkt der GridView (oder DetailsView oder FormView) wurde zuletzt aufgefüllt. Seit alle Felder außer `ProductID`, `ProductName`, und `Discontinued` können `NULL` -Werte, zusätzliche Parameter und Überprüfungen werden eingeschlossen, um ordnungsgemäß zu vergleichen `NULL` Werte in der `WHERE` Klausel.

Wir wird keine zusätzlichen DataTables auf das vollständige nebenläufigkeitsfähigen DataSet für dieses Lernprogramm hinzufügen werden wie unsere ASP.NET-Seite nur erhalten, aktualisieren und Löschen von Produktinformationen. Wir noch müssen jedoch zum Hinzufügen der `GetProductByProductID(productID)` Methode, um die `ProductsOptimisticConcurrency` TableAdapter.

Um dies zu erreichen, Maustaste auf die Titelleiste des TableAdapter (Bereich rechts oben die `Fill` und `GetProducts` Methodennamen), und wählen Sie im Kontextmenü der Abfrage hinzufügen. TableAdapter-Abfragekonfigurations-Assistent wird gestartet. Wie entscheiden Sie sich mit der Erstkonfiguration des TableAdapter zum Erstellen der `GetProductByProductID(productID)` Methode, die mit Ad-hoc-SQL-Anweisungen (siehe Abbildung 4). Da die `GetProductByProductID(productID)` Methode gibt Informationen über ein bestimmtes Produkt zurück, um anzugeben, dass diese Abfrage ist ein `SELECT` Abfragetyp, die Zeilen zurückgibt.


[![Markieren Sie den Abfragetyp als eine &quot;auswählen, welche die Zeilen zurückgibt&quot;](implementing-optimistic-concurrency-cs/_static/image26.png)](implementing-optimistic-concurrency-cs/_static/image25.png)

**Abbildung 9**: Markieren Sie den Abfragetyp als eine "`SELECT` die Zeilen zurückgibt" ([klicken Sie hier, um das Bild in voller Größe angezeigt](implementing-optimistic-concurrency-cs/_static/image27.png))


Auf dem nächsten Bildschirm sind wir für die SQL-Abfrage verwenden, mit dem TableAdapter-Standardabfrage vorab geladenen aufgefordert. Erweitern Sie die vorhandene Abfrage aus, um die-Klausel `WHERE ProductID = @ProductID`, wie in Abbildung 10.


[![Hinzufügen eine WHERE-Klausel, um die vorab geladenen Abfrage einen bestimmtes Produktdatensatz zurückgeben](implementing-optimistic-concurrency-cs/_static/image29.png)](implementing-optimistic-concurrency-cs/_static/image28.png)

**Abbildung 10**: Hinzufügen einer `WHERE` -Klausel, um die Pre-Loaded Abfrage einem bestimmten Produktdatensatz zurückgeben ([klicken Sie hier, um das Bild in voller Größe angezeigt](implementing-optimistic-concurrency-cs/_static/image30.png))


Abschließend ändern Sie die generierte Methode Objektnamen `FillByProductID` und `GetProductByProductID`.


[![Benennen Sie die Methoden FillByProductID und GetProductByProductID](implementing-optimistic-concurrency-cs/_static/image32.png)](implementing-optimistic-concurrency-cs/_static/image31.png)

**Abbildung 11**: Benennen Sie die Methoden zum `FillByProductID` und `GetProductByProductID` ([klicken Sie hier, um das Bild in voller Größe angezeigt](implementing-optimistic-concurrency-cs/_static/image33.png))


Mit dieser Assistent abgeschlossen wurde, enthält die TableAdapter jetzt zwei Methoden zum Abrufen von Daten: `GetProducts()`, welche gibt *alle* Produkte; und `GetProductByProductID(productID)`, wobei das angegebene Produkt zurückgegeben.

## <a name="step-3-creating-a-business-logic-layer-for-the-optimistic-concurrency-enabled-dal"></a>Schritt 3: Erstellen eine Geschäftslogikschicht für optimistische Parallelität aktiviert DAL

Unsere vorhandene `ProductsBLL` -Klasse verfügt über die Beispiele für die Verwendung der BatchUpdate und die direkte DB-Muster. Die `AddProduct` Methode und `UpdateProduct` beide Überladungen verwenden updatemusters Batch übergeben einer `ProductRow` Instanz mit dem TableAdapter-Aktualisierungsmethode. Die `DeleteProduct` -Methode, verwendet das DB-direct-Muster, des TableAdapter aufrufen, andererseits, `Delete(productID)` Methode.

Mit dem neuen `ProductsOptimisticConcurrency` TableAdapter, die DB direkte Methoden erfordern jetzt auch die ursprünglichen Werte übergeben werden. Z. B. die `Delete` Methode jetzt erwartet zehn Eingabeparameter entgegen: die ursprüngliche `ProductID`, `ProductName`, `SupplierID`, `CategoryID`, `QuantityPerUnit`, `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, `ReorderLevel`, und `Discontinued`. Er verwendet diese zusätzliche Eingabeparameter-Werte in `WHERE` -Klausel der `DELETE` Anweisung an die Datenbank gesendet werden, nur den angegebenen Datensatz löschen, wenn die aktuellen Werte für die Datenbank bis zu der ursprünglichen ereignislebensdauern zuordnen.

Während die Methodensignatur für die TableAdapter `Update` in updatemusters Batch verwendete Methode nicht geändert wurde, hat der Codes erforderlich, um die ursprünglichen und neuen Werte zu zeichnen. Aus diesem Grund statt versuchen, verwenden die optimistische Parallelität aktiviert DAL mit unserem vorhandenen `ProductsBLL` Klasse, erstellen wir eine neue Business Logic Layer-Klasse zum Arbeiten mit unserem neuen DAL.

Fügen Sie eine Klasse mit dem Namen `ProductsOptimisticConcurrencyBLL` auf die `BLL` Ordner innerhalb der `App_Code` Ordner.


![Fügen Sie in den Ordner BLL die ProductsOptimisticConcurrencyBLL-Klasse](implementing-optimistic-concurrency-cs/_static/image34.png)

**Abbildung 12**: Hinzufügen der `ProductsOptimisticConcurrencyBLL` Klasse, um den Ordner BLL


Fügen Sie den folgenden Code in die `ProductsOptimisticConcurrencyBLL` Klasse:


[!code-csharp[Main](implementing-optimistic-concurrency-cs/samples/sample5.cs)]

Beachten Sie die Verwendung `NorthwindOptimisticConcurrencyTableAdapters` Anweisung über den Beginn der Klassendeklaration. Die `NorthwindOptimisticConcurrencyTableAdapters` -Namespace enthält die `ProductsOptimisticConcurrencyTableAdapter` Klasse, die die DAL Methoden bereitstellt. Auch vor der Klassendeklaration finden Sie die `System.ComponentModel.DataObject` -Attribut, das weist Visual Studio für diese Klasse in der Dropdownliste das ObjectDataSource-Assistenten zu schließen.

Die `ProductsOptimisticConcurrencyBLL`des `Adapter` Eigenschaft ermöglicht den schnellen Zugriff auf eine Instanz der `ProductsOptimisticConcurrencyTableAdapter` Klasse, und folgt dem Muster in den ursprünglichen BLL-Klassen verwendet (`ProductsBLL`, `CategoriesBLL`usw.). Schließlich die `GetProducts()` Methode ruft einfach nach unten in der DAL `GetProducts()` -Methode und gibt eine `ProductsOptimisticConcurrencyDataTable` -Objekt aufgefüllt wird, mit einer `ProductsOptimisticConcurrencyRow` -Instanz für jede Produktdatensatz in der Datenbank.

## <a name="deleting-a-product-using-the-db-direct-pattern-with-optimistic-concurrency"></a>Löschen eines Produkts unter Verwendung des DB-Direct-Musters mit vollständiger Parallelität

Wenn das direkte DB-Muster für eine DAL verwenden, die vollständigen Parallelität verwendet, müssen die Methoden die neuen und ursprünglichen Werte übergeben werden. Zum Löschen, es sind keine neuen Werte, daher müssen nur die ursprünglichen Werte übergeben werden. In unserem BLL müssen, werden alle von der ursprünglichen Parameter als Eingabeparameter akzeptiert. Wir haben die `DeleteProduct` Methode in der `ProductsOptimisticConcurrencyBLL` Klasse verwenden, die direkte DB-Methode. Dies bedeutet, dass diese Methode muss alle zehn Produkt Datenfelder als Eingabeparameter, und diese an die DAL übergeben, wie im folgenden Code gezeigt:


[!code-csharp[Main](implementing-optimistic-concurrency-cs/samples/sample6.cs)]

Wenn die ursprünglichen Werte – diese Werte, die zuletzt in der GridView (oder DetailsView oder FormView) geladen wurden - unterscheiden sich von den Werten in der Datenbank, klickt der Benutzer die Schaltfläche "löschen" die `WHERE` -Klausel wird nicht mit Datenbank-Datensatz und keine Datensätze überein sind betroffen. Daher TableAdapters `Delete` Methode zurück `0` und der BLL `DeleteProduct` Methode zurück `false`.

## <a name="updating-a-product-using-the-batch-update-pattern-with-optimistic-concurrency"></a>Aktualisieren eines Produkts unter Verwendung des Batch-Update-Musters mit vollständiger Parallelität

Wie oben erwähnt von des TableAdapters `Update` Methode für die Batch-updatemuster hat die gleiche Methodensignatur unabhängig davon, ob die vollständige Parallelität verwendet wird. Nämlich die `Update` Methode erwartet DataRow, ein Array von DataRows, einer "DataTable" oder ein typisiertes DataSet. Es sind keine zusätzlichen Eingabeparameter für die ursprünglichen Werte angeben. Dies ist möglich, da die DataTable die ursprünglichen und geänderten Werte für die DataRow(s) der nachverfolgt. Wenn die DAL ausgibt seine `UPDATE` -Anweisung, die `@original_ColumnName` Parameter werden mit der ursprünglichen Werte der DataRow aufgefüllt, während die `@ColumnName` Parameter sind mit den DataRow geänderten Werten aufgefüllt.

In der `ProductsBLL` -Klasse (die unsere ursprüngliche, nicht durch vollständige Parallelität DAL), Verwendung der Batch-updatemuster um Produktinformationen unsere Code führt die folgende Abfolge von Ereignissen zu aktualisieren:

1. Lesen die aktuelle Datenbank Produktinformationen in einen `ProductRow` -Instanz mit des TableAdapters `GetProductByProductID(productID)` Methode
2. Weisen Sie die neuen Werten zu den `ProductRow` -Instanz aus Schritt 1
3. Rufen Sie die `Update` -Methode auf und übergibt die `ProductRow` Instanz

Diese Abfolge von Schritten, jedoch wird nicht ordnungsgemäß durch vollständige Parallelität unterstützen, da die `ProductRow` aufgefüllt, die Schritt 1 wird aufgefüllt, direkt aus der Datenbank, was bedeutet, dass die ursprünglichen Werte von DataRow verwendet, die derzeit im vorhanden sind die Datenbank und nicht die, die an die GridView zu Beginn der Bearbeitung verbunden waren. Stattdessen verwenden eine optimistische Parallelität-DAL aktiviert, es müssen zum Ändern der `UpdateProduct` -methodenüberladungen verwenden Sie die folgenden Schritte aus:

1. Lesen die aktuelle Datenbank Produktinformationen in einen `ProductsOptimisticConcurrencyRow` -Instanz mit des TableAdapters `GetProductByProductID(productID)` Methode
2. Weisen Sie die *ursprünglichen* -Werte in der `ProductsOptimisticConcurrencyRow` -Instanz aus Schritt 1
3. Rufen Sie die `ProductsOptimisticConcurrencyRow` Instanz `AcceptChanges()` -Methode, die DataRow wird angewiesen, dass die aktuellen Werte der "ursprünglichen" ereignislebensdauern sind
4. Weisen Sie die *neue* -Werte in der `ProductsOptimisticConcurrencyRow` Instanz
5. Rufen Sie die `Update` -Methode auf und übergibt die `ProductsOptimisticConcurrencyRow` Instanz

Schritt 1-Lesevorgänge in allen von der aktuellen Datenbankwerte für das angegebene Produkt-Datensatz. Dieser Schritt ist überflüssig, in der `UpdateProduct` Überladung, die aktualisiert *alle* der Product-Spalten (als diese Werte werden überschrieben in Schritt2), aber ist wichtig für eine dieser Überladungen, in denen nur eine Teilmenge der Spaltenwerte als übergeben, Eingabeparameter. Nachdem die ursprünglichen Werte zugewiesen wurden die `ProductsOptimisticConcurrencyRow` -Instanz, die `AcceptChanges()` Methode wird aufgerufen, die die aktuellen DataRow-Werte als ursprüngliche Werte zu verwendende markiert die `@original_ColumnName` Parameter in der `UPDATE` Anweisung. Als Nächstes werden die neuen Parameterwerte zugewiesen, um die `ProductsOptimisticConcurrencyRow` und schließlich die `Update` Methode aufgerufen wird, in der DataRow übergeben.

Der folgende code zeigt die `UpdateProduct` Überladung, die alle Produktdaten akzeptiert Felder als Eingabeparameter. Während nicht hier gezeigt die `ProductsOptimisticConcurrencyBLL` Klasse, die im Download enthalten, für dieses Lernprogramm auch enthält eine `UpdateProduct` Überladung, die nur der Product Name und der Preis als Eingabeparameter akzeptiert.


[!code-csharp[Main](implementing-optimistic-concurrency-cs/samples/sample7.cs)]

## <a name="step-4-passing-the-original-and-new-values-from-the-aspnet-page-to-the-bll-methods"></a>Schritt 4: Übergeben die ursprünglichen und neuen Werte von der ASP.NET-Seite an die BLL-Methode

Die DAL und BLL abgeschlossen übrig bleibt eine ASP.NET-Seite zu erstellen, die die vollständige Parallelität-Logik, die in das System integriert genutzt werden können. Insbesondere müssen die Daten (GridView, DetailsView oder FormView)-Websteuerelement daran denken, dass die ursprünglichen Werte und die ObjectDataSource beide Sätze von Werten an die Geschäftslogikschicht übergeben werden müssen. Darüber hinaus muss die ASP.NET-Seite konfiguriert werden, um parallelitätsverletzungen Datenbindungsvorgängen erfolgreich behandelt.

Öffnen Sie zunächst die `OptimisticConcurrency.aspx` auf der Seite der `EditInsertDelete` Ordner und das Hinzufügen einer GridView in den Designer, Festlegen seiner `ID` Eigenschaft `ProductsGrid`. Erstellen Sie eine neue ObjectDataSource mit dem Namen aus der GridView Smarttag, wahlweise `ProductsOptimisticConcurrencyDataSource`. Da soll diese ObjectDataSource die DAL verwenden, die vollständige Parallelität unterstützt, konfigurieren Sie ihn, verwenden Sie die `ProductsOptimisticConcurrencyBLL` Objekt.


[![Haben Sie die Verwendung ObjectDataSource ProductsOptimisticConcurrencyBLL-Objekt](implementing-optimistic-concurrency-cs/_static/image36.png)](implementing-optimistic-concurrency-cs/_static/image35.png)

**Abbildung 13**: haben Sie die Verwendung ObjectDataSource der `ProductsOptimisticConcurrencyBLL` Objekt ([klicken Sie hier, um das Bild in voller Größe angezeigt](implementing-optimistic-concurrency-cs/_static/image37.png))


Wählen Sie die `GetProducts`, `UpdateProduct`, und `DeleteProduct` Methoden aus den Dropdownlisten im Assistenten. Verwenden Sie die Überladung, die das Produkt Datenfelder akzeptiert, bei der UpdateProduct-Methode.

## <a name="configuring-the-objectdatasource-controls-properties"></a>Konfigurieren von Eigenschaften für das ObjectDataSource-Steuerelement

Nach Abschluss des Assistenten, sollte das ObjectDataSource deklarative Markup wie folgt aussehen:


[!code-aspx[Main](implementing-optimistic-concurrency-cs/samples/sample8.aspx)]

Wie Sie sehen können, die `DeleteParameters` Auflistung enthält ein `Parameter` Instanz für jeden der zehn Eingabeparameter in der `ProductsOptimisticConcurrencyBLL` Klasse `DeleteProduct` Methode. Entsprechend der `UpdateParameters` Auflistung enthält ein `Parameter` Instanz für jede der Eingabeparameter im `UpdateProduct`.

Für die vorherigen Lernprogrammen, die Änderung von Daten beteiligt, entfernen Sie das ObjectDataSource `OldValuesParameterFormatString` Eigenschaft an diesem Punkt, da diese Eigenschaft gibt an, dass die BLL-Methode die alten (oder ursprüngliche) Werte übergeben werden muss, sowie die neuen Werte erwartet. Darüber hinaus gibt Wert dieser Eigenschaft die Namen der Eingabeparameter für die ursprünglichen Werte an. Da wir in den ursprünglichen Werten in der BLL übergeben, *nicht* dieser Eigenschaft entfernen.

> [!NOTE]
> Der Wert, der die `OldValuesParameterFormatString` Eigenschaft muss den input Parameternamen in der BLL, bei denen die ursprünglichen Werte zuordnen. Da wir diese Parameter mit dem Namen `original_productName`, `original_supplierID`usw., lassen Sie die `OldValuesParameterFormatString` Eigenschaftswert als `original_{0}`. Wäre, allerdings BLL Methoden Eingabeparameter Namen wie `old_productName`, `old_supplierID`usw., müssen Sie beim Aktualisieren der `OldValuesParameterFormatString` Eigenschaft `old_{0}`.


Es ist eine abschließende Eigenschaft-Einstellung, die in der Reihenfolge für das ObjectDataSource auf die ursprünglichen Werte ordnungsgemäß an die BLL-Methoden übergeben vorgenommen werden muss. Das ObjectDataSource hat eine [ConflictDetection-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.conflictdetection.aspx) , können zugewiesen [einen von zwei Werten](https://msdn.microsoft.com/library/system.web.ui.conflictoptions.aspx):

- `OverwriteChanges`-Der Standardwert; die ursprünglichen Werte werden keine an der ursprünglichen Eingabeparameter BLL Methoden gesendet werden.
- `CompareAllValues`-die ursprünglichen Werte, die Methoden für BLL sendet. Wählen Sie diese Option aus, wenn Sie vollständigen Parallelität verwenden

Erkundet festzulegende der `ConflictDetection` Eigenschaft `CompareAllValues`.

## <a name="configuring-the-gridviews-properties-and-fields"></a>Konfigurieren von Eigenschaften und Felder der GridView

Mit der ObjectDataSource Eigenschaften ordnungsgemäß konfiguriert werfen wir die GridView einrichten. Da wir die GridView zu unterstützen, bearbeiten und löschen möchten, klicken Sie zunächst auf die Kontrollkästchen aktivieren Sie bearbeiten und löschen aktivieren aus der GridView Smarttag. Dadurch wird eine CommandField hinzugefügt, deren `ShowEditButton` und `ShowDeleteButton` festgelegt `true`.

Bei der Bindung an die `ProductsOptimisticConcurrencyDataSource` ObjectDataSource, GridView enthält ein Feld für die einzelnen Datenfelder des Produkts. Während eine solchen GridView bearbeitet werden kann, sieht die benutzerumgebung gar nichts jedoch akzeptabel. Die `CategoryID` und `SupplierID` BoundFields rendert als Textfelder ein, da der Benutzer die entsprechende Kategorie und der Lieferant als ID-Nummern eingeben. Es werden keine Formatierung für numerische Felder und keine Validierungssteuerelemente, stellen Sie sicher, dass das Produkt Anwendungsname angegeben wurde und Einzelpreis Einheiten im Lager, Einheiten Order-und neu anordnen Ebenenwerte beide entsprechenden numerischen Werte sind und größer als oder gleich 0 (null).

Wie in erläutert die *Validierungssteuerelemente hinzufügen, bearbeiten und Einfügen von Schnittstellen* und *Anpassen der Benutzeroberfläche für die Änderung der Daten* Lernprogramme, die Benutzeroberfläche kann angepasst werden durch Ersetzen die BoundFields mit von TemplateFields. Ich wurde diese GridView und die Bearbeitung-Schnittstelle auf folgende Weise verändert:

- Entfernt die `ProductID`, `SupplierName`, und `CategoryName` BoundFields
- Konvertiert die `ProductName` BoundField in ein TemplateField und ein RequiredFieldValidation-Steuerelement hinzugefügt.
- Konvertiert die `CategoryID` und `SupplierID` BoundFields auf von TemplateFields, und die Bearbeitung Schnittstelle für die Textfelder ein, anstatt DropDownLists verwenden angepasst. In diesen von TemplateFields `ItemTemplates`, `CategoryName` und `SupplierName` Datenfelder angezeigt werden.
- Konvertiert die `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, und `ReorderLevel` BoundFields von TemplateFields und hinzugefügten CompareValidator-Steuerelemente.

Da wir wie diese Aufgaben im vorherigen Lernprogrammen bereits überprüft haben, kann ich Sende nur hier die endgültige deklarative Syntax auflisten und lassen Sie die Implementierung als Methode.


[!code-aspx[Main](implementing-optimistic-concurrency-cs/samples/sample9.aspx)]

Wir sind sehr nahe, dass ein voll funktionsfähiges Beispiel. Es gibt jedoch einige Besonderheiten, die sich verzeichnen und uns Probleme verursachen. Darüber hinaus brauchen wir noch eine Schnittstelle, die den Benutzer benachrichtigt werden, wenn eine parallelitätsverletzung aufgetreten ist.

> [!NOTE]
> Damit ein Daten-Websteuerelement, übergeben Sie die ursprünglichen Werte ordnungsgemäß an das ObjectDataSource (die dann an die BLL übergeben werden), ist es von entscheidender Bedeutung, die der GridView `EnableViewState` -Eigenschaftensatz auf `true` (Standard). Wenn Sie den Ansichtszustand deaktivieren, werden die ursprünglichen Werte auf Postback verloren gegangen.


## <a name="passing-the-correct-original-values-to-the-objectdatasource"></a>Übergeben die richtigen ursprüngliche Werte an das ObjectDataSource

Es gibt eine Reihe von Problemen bei der Art die GridView konfiguriert wurde. Wenn das ObjectDataSource `ConflictDetection` -Eigenschaftensatz auf `CompareAllValues` (wie besehen unsere), wenn das ObjectDataSource `Update()` oder `Delete()` Methoden werden aufgerufen, indem die GridView (oder DetailsView oder FormView), wird das ObjectDataSource versucht, so kopieren Sie die GridView ursprünglichen Werte in die entsprechenden `Parameter` Instanzen. Verweisen Sie zurück in Abbildung 2 für eine grafische Darstellung dieses Prozesses.

Insbesondere werden die GridView ursprünglichen Werte der Werte in den Anweisungen für die bidirektionale Datenbindung zugewiesen, jedes Mal die Daten an die GridView gebunden werden. Daher ist es wichtig, dass die erforderlichen ursprünglichen Werte, die alle über die bidirektionale Datenbindung erfasst werden und dass sie in einem konvertierbar Format bereitgestellt werden.

Nehmen Sie einen Moment Zeit, besuchen unsere-Seite in einem Browser, um anzuzeigen, warum dies wichtig ist. Wie erwartet, werden die GridView jedes Produkt mit einer Schaltfläche "Bearbeiten und löschen" in der äußersten linken Spalte aufgeführt.


[![Produkte werden in einem GridView aufgelistet.](implementing-optimistic-concurrency-cs/_static/image39.png)](implementing-optimistic-concurrency-cs/_static/image38.png)

**Abbildung 14**: die Produkte werden in einem GridView aufgelistet ([klicken Sie hier, um das Bild in voller Größe angezeigt](implementing-optimistic-concurrency-cs/_static/image40.png))


Wenn Sie die Schaltfläche "löschen" für jedes Produkt auf eine `FormatException` ausgelöst wird.


[![Bei dem Versuch, Produkt Ergebnisse in eine FormatException löschen](implementing-optimistic-concurrency-cs/_static/image42.png)](implementing-optimistic-concurrency-cs/_static/image41.png)

**Abbildung 15**: Versuch Löschen beliebiger Produkt Ergebnisse in einem `FormatException` ([klicken Sie hier, um das Bild in voller Größe angezeigt](implementing-optimistic-concurrency-cs/_static/image43.png))


Die `FormatException` wird ausgelöst, wenn das ObjectDataSource versucht wird, lesen Sie in der ursprünglichen `UnitPrice` Wert. Da die `ItemTemplate` hat die `UnitPrice` als Währung formatiert (`<%# Bind("UnitPrice", "{0:C}") %>`), er enthält ein Währungssymbol, z. B. $19,95. Die `FormatException` auftritt, die das ObjectDataSource versucht wird, konvertiert diese Zeichenfolge in eine `decimal`. Um dieses Problem zu umgehen, haben wir eine Reihe von Optionen an:

- Entfernen Sie das Währungsformat aus der `ItemTemplate`. D. h. anstatt `<%# Bind("UnitPrice", "{0:C}") %>`, verwenden Sie einfach `<%# Bind("UnitPrice") %>`. Der Nachteil dieses ist, dass der Preis nicht mehr formatiert ist.
- Anzeigen der `UnitPrice` formatiert als Währung in der `ItemTemplate`, verwenden jedoch den `Eval` Schlüsselwort, um dies zu erreichen. Bedenken Sie, dass `Eval` unidirektionale Databinding ausführt. Wir müssen angeben der `UnitPrice` Wert für die ursprünglichen Werte, damit wir noch, dass eine bidirektionale Databinding-Anweisung in benötigen der `ItemTemplate`, aber dies platziert werden kann, in einem Label-Steuerelement, dessen `Visible` -Eigenschaftensatz auf `false`. Wir konnten das folgende Markup in ItemTemplate verwenden:


[!code-aspx[Main](implementing-optimistic-concurrency-cs/samples/sample10.aspx)]

- Entfernen Sie das Währungsformat aus der `ItemTemplate`mit `<%# Bind("UnitPrice") %>`. In der GridView `RowDataBound` -Ereignishandler, programmgesteuert auf den Zugriff steuern die Bezeichnung Web, anhand derer die `UnitPrice` Wert angezeigt wird, und legen Sie dessen `Text` Eigenschaft, um die formatierte Version.
- Lassen Sie die `UnitPrice` als Währung formatiert. In der GridView `RowDeleting` Ereignishandler, ersetzen Sie die vorhandene ursprüngliche `UnitPrice` Wert ($19,95) mit einem tatsächlichen Dezimalwert `Decimal.Parse`. Wurde erläutert, wie in etwas Ähnliches zu erreichen der `RowUpdating` -Ereignishandler in der [ *Behandlung von BLL- und DAL-Ebene Ausnahmen auf einer ASP.NET-Seite* ](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) Lernprogramm.

Meine ich ausgewählt haben, fahren Sie mit den zweiten Ansatz, beispielsweise Hinzufügen einer ausgeblendeten Bezeichnung Web-Steuerelement, dessen `Text` Eigenschaft ist für bidirektionale gebundenen die unformatierten Daten `UnitPrice` Wert.

Versuchen Sie nach der Lösung dieses Problems erneut auf die Schaltfläche "löschen" für jedes Produkt klicken. Sie erhalten diesmal ein `InvalidOperationException` wann das ObjectDataSource zum Aufrufen der BLL `UpdateProduct` Methode.


[![Das ObjectDataSource kann eine Methode mit dem Eingabeparameter finden, die sie senden möchte](implementing-optimistic-concurrency-cs/_static/image45.png)](implementing-optimistic-concurrency-cs/_static/image44.png)

**Abbildung 16**: das ObjectDataSource wurde eine Methode mit der Input-Parameter, die sie senden möchte nicht gefunden ([klicken Sie hier, um das Bild in voller Größe angezeigt](implementing-optimistic-concurrency-cs/_static/image46.png))


Betrachten die Meldung der Ausnahme aus, es ist klar, dass das ObjectDataSource eine BLL aufrufen möchte `DeleteProduct` Methode, die enthält `original_CategoryName` und `original_SupplierName` Eingabeparameter. Grund hierfür ist die `ItemTemplate` s für die `CategoryID` und `SupplierID` von TemplateFields derzeit Anweisungen enthalten, die bidirektionale Bindung mit der `CategoryName` und `SupplierName` Datenfelder. Stattdessen müssen wir enthalten `Bind` -Anweisungen mit der `CategoryID` und `SupplierID` Datenfelder. Um dies zu erreichen, ersetzen Sie die vorhandenen Bind-Anweisungen mit `Eval` -Anweisungen, und fügen Sie dann auf ausgeblendete Label-Steuerelemente, deren `Text` Eigenschaften gebunden sind, um die `CategoryID` und `SupplierID` Datenfelder, die wie gezeigt, über die bidirektionale Datenbindung, Im folgenden:


[!code-aspx[Main](implementing-optimistic-concurrency-cs/samples/sample11.aspx)]

Mit diesen Änderungen werden wir jetzt erfolgreich löschen und Bearbeiten von Produktinformationen können! In Schritt 5 betrachten wir, wie stellen Sie sicher, dass nebenläufigkeitsverstöße erkannt wird, werden wird. Aber jetzt zu aktualisieren versuchen Sie es einige Minuten dauern, bis, und löschen einige Datensätze, um sicherzustellen, dass die Aktualisierung und Löschen für einen einzelnen Benutzer erwartungsgemäß funktioniert.

## <a name="step-5-testing-the-optimistic-concurrency-support"></a>Schritt 5: Testen der vollständigen Parallelität-Unterstützung

Um sicherzustellen, dass parallelitätsverletzungen erkannten (statt resultierenden Daten Blind überschrieben) werden, müssen wir zwei Browserfenstern auf dieser Seite zu öffnen. Klicken Sie in beiden Browserinstanzen für Chai auf die Schaltfläche "Bearbeiten". Klicken Sie dann nur einen Browser, ändern Sie den Namen in "Chai Tee", und klicken Sie auf aktualisieren. Das Update sollte erfolgreich sind und die GridView Datenbankzustands vorab Bearbeitung mit "Chai Tee" als neues Produktname.

In den anderen Fenster Browserinstanz zeigt jedoch der Produktnamen Textfeld immer noch "Chai". In diesem zweiten Browserfenster aktualisieren, die `UnitPrice` auf `25.00`. Keine Unterstützung für vollständige Parallelität würde Update in der zweiten Browserinstanz auf den Produktnamen an "Chai", wodurch die von der ersten Browserinstanz vorgenommenen Änderungen überschrieben ändern. Mit vollständiger Parallelität verwendet, jedoch auf die Schaltfläche "Aktualisieren" in der zweiten Browserinstanz führt zu einem [DBConcurrencyException](https://msdn.microsoft.com/library/system.data.dbconcurrencyexception.aspx).


[![Wenn eine Parallelitätsverletzung erkannt wird, wird eine DBConcurrencyException ausgelöst.](implementing-optimistic-concurrency-cs/_static/image48.png)](implementing-optimistic-concurrency-cs/_static/image47.png)

**Abbildung 17**: Wenn eine Parallelitätsverletzung erkannt wird, eine `DBConcurrencyException` ausgelöst ([klicken Sie hier, um das Bild in voller Größe angezeigt](implementing-optimistic-concurrency-cs/_static/image49.png))


Die `DBConcurrencyException` wird nur ausgelöst, wenn der DAL Batch updatemuster genutzt wird. Das direkte DB-Muster löst keine Ausnahme aus, lediglich bedeutet dies, dass keine Zeilen betroffen sind. Um dies zu veranschaulichen, werden beide Browserinstanzen GridView an ihren vorab Bearbeitungszustand zurück. Als Nächstes in der ersten Browserinstanz, klicken Sie auf die Schaltfläche "Bearbeiten" und ändern Sie den Produktnamen aus "Chai Tee" an "Chai" und klicken Sie auf aktualisieren. Klicken Sie in der zweiten Browserfenster für Chai auf die Schaltfläche "löschen".

Beim Klicken auf "löschen" die Seite sendet zurück, die GridView Ruft das ObjectDataSource `Delete()` -Methode und die ObjectDataSource Aufrufe nach unten der `ProductsOptimisticConcurrencyBLL` Klasse `DeleteProduct` Methode weiter und übergibt dabei die ursprünglichen Werte. Die ursprüngliche `ProductName` Wert, der zweite Browserinstanz "Chai Tee", die mit dem aktuellen übereinstimmt `ProductName` Wert in der Datenbank. Aus diesem Grund die `DELETE` Anweisung mit der Datenbank ausgegeben wirkt sich auf 0 (null) Zeilen, da es kein Datensatz in der Datenbank ist, die die `WHERE` Klausel erfüllen. Die `DeleteProduct` -Methode zurückkehrt `false` und das ObjectDataSource-Daten an die GridView gebunden ist.

Aus der Perspektive des Endbenutzers klicken auf die Schaltfläche "löschen" für Chai Tee im zweiten Browserfenster verursacht den Bildschirm, flash und, nach zurückzukommen, das Produkt wird immer noch vorhanden ist, obwohl jetzt als "Chai" (die Änderung des Produktnamens vorgenommen, die vom ersten Browser danach die Instanz). Wenn der Benutzer erneut auf die Schaltfläche "löschen" klickt, der Löschvorgang zwar ausgeführt werden, wie die GridView ursprünglichen's `ProductName` Wert ("Chai") sich nun mit dem Wert in der Datenbank übereinstimmt.

In beiden Fällen wird die benutzerfreundlichkeit zurückgelegt Ideal ist. Wir deutlich möchte nicht dem Benutzer anzuzeigen, die wesentlichen Details der `DBConcurrencyException` -Ausnahme aus, wenn der Batch-updatemuster mit. Und das Verhalten, wenn mit dem direkten DB-Muster ist etwas verwirrend, da der Benutzer ist ein Fehler aufgetreten, aber es keine genaue Angabe der verdeutlichen, warum wurde.

Um diese beiden Probleme zu beheben, können wir Bezeichnung Websteuerelemente auf der Seite, die eine Erläuterung, warum ein Update- oder Delete-Fehler beim Bereitstellen erstellen. Für das Batch-Update-Muster können bestimmen, und zwar unabhängig davon, ob eine `DBConcurrencyException` Ausnahme bei einem nach der Ebene der GridView-Ereignishandler, d. h. der Warnhinweis anzeigen, je nach Bedarf. Untersuchen wir für die direkte DB-Methode ist den Rückgabewert der Methode BLL (also `true` , wenn eine Zeile betroffen war, `false` andernfalls) und eine informationsmeldung Bedarf anzuzeigen.

## <a name="step-6-adding-informational-messages-and-displaying-them-in-the-face-of-a-concurrency-violation"></a>Schritt 6: Hinzufügen von Informationsmeldungen und deren Ausführung eine Parallelitätsverletzung Anzeige

Tritt eine parallelitätsverletzung vom Verhalten hängt davon ab, ob der DAL BatchUpdate oder DB-direct-Muster verwendet wurde. Unser Tutorial aus, verwendet beide Muster, mit dem Muster für den Batch-Update aktualisieren als auch die direkte DB-Muster, die zum Löschen von verwendet wird. Um zu beginnen, fügen Sie zwei Label-Websteuerelemente auf unserer Seite, die erklären, dass eine parallelitätsverletzung aufgetreten beim Versuch, löschen oder Aktualisieren von Daten. Legen Sie des Label-Steuerelements `Visible` und `EnableViewState` Eigenschaften `false`; Dies bewirkt, dass sie auf jeder Seite Besuch ausgeblendet werden, außer für diese bestimmte Seite Where besucht ihre `Visible` -Eigenschaftensatz wird programmgesteuert auf `true`.


[!code-aspx[Main](implementing-optimistic-concurrency-cs/samples/sample12.aspx)]

Zusätzlich zu ihren `Visible`, `EnabledViewState`, und `Text` ich haben ebenfalls Festlegen von Eigenschaften, die `CssClass` Eigenschaft, um `Warning`, dies bedeutet, dass die Bezeichnung des in einer Schriftart große, Rot, kursiv, fett angezeigt werden. Dieses CSS `Warning` Klasse definiert und Styles.css hinzugefügt wurde wieder in die *untersuchen die Ereignisse zugeordneten einfügen, aktualisieren und Löschen von* Lernprogramm.

Nachdem diese Bezeichnungen hinzugefügt wurde, sollte der Designer in Visual Studio Abbildung 18 ähneln.


[![Zwei Label-Steuerelemente wurden der Seite "hinzugefügt](implementing-optimistic-concurrency-cs/_static/image51.png)](implementing-optimistic-concurrency-cs/_static/image50.png)

**Abbildung 18**: zwei Label-Steuerelemente hinzugefügt wurden die Seite ([klicken Sie hier, um das Bild in voller Größe angezeigt](implementing-optimistic-concurrency-cs/_static/image52.png))


Mit dieser Bezeichnung Web Kontrollen, wir jetzt können Sie bestimmen, wenn eine parallelitätsverletzung aufgetreten ist, zeigen Sie an dem der entsprechenden Bezeichnung untersuchen `Visible` Eigenschaft kann festgelegt werden, um `true`, die Meldung zu Informationszwecken angezeigt.

## <a name="handling-concurrency-violations-when-updating"></a>Behandeln von Parallelität, bei der Aktualisierung

Sehen wir uns zunächst an, wie parallelitätsverletzungen behandelt wird, wenn der Batch-updatemuster mit. Da solche Verletzungen mit dem Batch Muster Ursache Aktualisieren einer `DBConcurrencyException` Ausnahme ausgelöst wird, müssen wir unsere ASP.NET-Seite, um zu bestimmen, Code hinzufügen, ob ein `DBConcurrencyException` während des Updates ist ein Ausnahme aufgetreten. Wenn also wir eine Nachricht an die Benutzer erklären angezeigt werden soll, die ihre Änderungen nicht gespeichert wurden, weil ein anderer Benutzer die gleichen Daten zwischen geändert wurde, beim Bearbeiten des Datensatzes starten und wenn sie die Schaltfläche "Aktualisieren" geklickt wurde.

Wie wir gesehen, in haben der *Behandlung von BLL- und DAL-Ebene Ausnahmen auf einer ASP.NET-Seite* Tutorial, solche Ausnahmen erkannt und in das Websteuerelement Daten nach der Ebene Ereignishandler unterdrückt werden können. Aus diesem Grund müssen so erstellen Sie einen Ereignishandler für der GridView `RowUpdated` Ereignis, das überprüft, wenn eine `DBConcurrencyException` Ausnahme wurde ausgelöst. Dieser Ereignishandler ist ein Verweis auf jede Ausnahme, die während des Aktualisierungsvorgangs ausgelöst wurde übergeben, wie im Ereignisprotokoll Ereignishandler folgenden code gezeigt:


[!code-csharp[Main](implementing-optimistic-concurrency-cs/samples/sample13.cs)]

Im face von einem `DBConcurrencyException` Ausnahme dieser Ereignishandler zeigt die `UpdateConflictMessage` Label-Steuerelement und gibt an, dass die Ausnahme verarbeitet wurde. Mit diesem Code werden tritt eine parallelitätsverletzung beim Aktualisieren eines Datensatzes sind des Benutzers Änderungen verloren, da sie ein anderer Benutzer Änderungen gleichzeitig überschrieben haben, würden. Insbesondere ist die GridView Datenbankzustands vorab Bearbeitung zurückgegeben und an Daten in der aktuellen Datenbank gebunden. Hiermit wird die GridView-Zeile mit der anderen Benutzer geändert wird, aktualisiert die zuvor nicht sichtbar waren. Darüber hinaus die `UpdateConflictMessage` Label-Steuerelement wird nur Ursache für den Benutzer erläutert. Diese Abfolge von Ereignissen wird in Abbildung 19 beschrieben.


[![Ein Benutzer s werden in die Vorderseite eines eine Parallelitätsverletzung Updates verloren.](implementing-optimistic-concurrency-cs/_static/image54.png)](implementing-optimistic-concurrency-cs/_static/image53.png)

**Abbildung 19**: ein Benutzer Updates sind verlorene Bestätigungen in die Vorderseite eines eine Parallelitätsverletzung ([klicken Sie hier, um das Bild in voller Größe angezeigt](implementing-optimistic-concurrency-cs/_static/image55.png))


> [!NOTE]
> Alternativ anstatt GridView an den Bearbeitungszustand vorab zurückgegeben, wir werden u. u. GridView im Bearbeitung Zustand durch Festlegen der `KeepInEditMode` Eigenschaft übergebenen `GridViewUpdatedEventArgs` Objekt auf "true". Wenn Sie diesen Ansatz verwenden, allerdings werden sicher, dass die Daten an die GridView zu binden (durch Aufrufen seiner `DataBind()` Methode), damit die Bearbeitungsoberfläche des anderen Benutzers Werte geladen werden. Der Code zum Download für dieses Lernprogramm wurde diese beiden Zeilen des Codes in der `RowUpdated` Ereignishandler auskommentiert; einfach kommentieren diese Codezeilen GridView haben bleiben im Bearbeitungsmodus nach eine parallelitätsverletzung liegt vor.


## <a name="responding-to-concurrency-violations-when-deleting"></a>Reagieren auf Parallelitätsverletzungen, beim Löschen von

Mit dem direkten DB-Muster es ist keine Ausnahme ausgelöst wird, bei dem eine parallelitätsverletzung liegt vor. Stattdessen wirkt sich auf die Database-Anweisung einfach keine Datensätze, wie die WHERE-Klausel nicht mit jedem Datensatz übereinstimmt. Alle Methoden Datenänderung in die BLL erstellt wurden entwickelt, dass sie einen booleschen Wert fest, und zwar unabhängig davon, ob sie genau einen Datensatz beeinflusst zurückgeben. Untersuchen wir daher, um festzustellen, ob eine parallelitätsverletzung aufgetreten ist, wenn Sie einen Datensatz löschen, den Rückgabewert der BLL `DeleteProduct` Methode.

Der Rückgabewert für eine Methode BLL untersucht werden kann, in der ObjectDataSource nach der Servicelevel-Ereignishandler durch den `ReturnValue` Eigenschaft von der `ObjectDataSourceStatusEventArgs` Objekt an den Ereignishandler übergeben. Da wir bei der Bestimmung des Rückgabewerts von Interesse sind, die die `DeleteProduct` -Methode, müssen wir erstellen einen Ereignishandler für das ObjectDataSource `Deleted` Ereignis. Die `ReturnValue` -Eigenschaft ist vom Typ `object` möglich `null` Wenn eine Ausnahme ausgelöst wurde, und die Methode wurde unterbrochen, bevor sie einen Wert zurückgeben kann. Daher wir sollten zuerst sicherstellen, dass die `ReturnValue` Eigenschaft ist nicht `null` und ist ein boolescher Wert. Diese Überprüfung erfolgreich war, veranschaulichen wir, vorausgesetzt die `DeleteConflictMessage` Steuerelement beschriftet, wenn die `ReturnValue` ist `false`. Dies kann erfolgen, mit dem folgenden Code:


[!code-csharp[Main](implementing-optimistic-concurrency-cs/samples/sample14.cs)]

Bei einer parallelitätsverletzung Delete-Anforderung des Benutzers abgebrochen. Die GridView wird aktualisiert und angezeigt, dass die Änderungen, die für den entsprechenden Datensatz zwischen dem Zeitpunkt den Benutzer aufgetreten geladen, auf der Seite, und wenn er auf die Schaltfläche "löschen" geklickt. Wenn solche eine Verletzung herausstellt, die `DeleteConflictMessage` Bezeichnung angezeigt wird, erläutern, was gerade geschehen ist (siehe Abbildung 20).


[![Ein Benutzer s löschen ist bei einer Parallelitätsverletzung abgebrochen.](implementing-optimistic-concurrency-cs/_static/image57.png)](implementing-optimistic-concurrency-cs/_static/image56.png)

**Abbildung 20**: ein Benutzer s löschen, bietet Stabilität gegenüber eine Parallelitätsverletzung abgebrochen wird ([klicken Sie hier, um das Bild in voller Größe angezeigt](implementing-optimistic-concurrency-cs/_static/image58.png))


## <a name="summary"></a>Zusammenfassung

Verkaufschancen parallelitätsverletzungen vorhanden sind, in jeder Anwendung, die mehrere ermöglicht gleichzeitigen Benutzern, Daten aktualisieren oder löschen. Wenn solche Verletzungen für, wenn zwei Benutzer gleichzeitig dieselben Daten wem auch immer in den letzten Schreibvorgang "Wins" erhält aktualisieren, des anderen Benutzers überschreiben ändert wird nicht berücksichtigt werden. Alternativ können Entwickler entweder Steuerung durch vollständige oder eingeschränkte Parallelität implementieren. Steuerung durch vollständige Parallelität wird davon ausgegangen, dass parallelitätsverletzungen wird selten und einfach lässt nicht zu einem Update oder delete-Befehl, die eine parallelitätsverletzung bilden, würden. Steuerung durch eingeschränkte Parallelität wird davon ausgegangen, dass die Parallelität Verstöße werden häufig und update oder delete-Befehl einfach Ablehnung eines bestimmten Benutzers nicht zulässig ist. Mit der Steuerung durch eingeschränkte Parallelität umfasst das Aktualisieren eines Datensatzes, sperren, dadurch, dass alle anderen Benutzer geändert oder der Datensatz gelöscht, während es gesperrt ist.

Das typisierte DataSet im .NET bietet Funktionen für die Unterstützung der Steuerung durch vollständige Parallelität. Insbesondere die `UPDATE` und `DELETE` Anweisungen, die mit der Datenbank ausgegeben werden alle Spalten der Tabelle enthalten, dadurch wird sichergestellt, dass die Update- oder Delete nur auftreten, wenn der aktuellen Datensatzdaten, mit der zu sichernden Daten den Benutzer übereinstimmt hatte, wenn Ausführen von ihren Update- oder Delete. Sobald die DAL zur Unterstützung der vollständigen Parallelität konfiguriert wurde, müssen die BLL Methoden aktualisiert werden. Darüber hinaus muss die ASP.NET-Seite, die die BLL, nach unten aufruft werden so konfiguriert, dass das ObjectDataSource ruft die ursprünglichen Werte aus dem Web-Steuerelement ab und sie in der BLL nach unten übergibt.

Wie wir in diesem Lernprogramm gesehen haben, beinhaltet die Implementierung der Steuerung durch vollständige Parallelität in eine ASP.NET-Webanwendung Aktualisieren der DAL und BLL und Hinzufügen der Unterstützung der ASP.NET-Seite. Fest, ob dieser zusätzliche Aufwand eine hintergrundprüfung Investition Ihrer Zeit und Aufwand ist, hängt von der Anwendung ab. Wenn Sie selten gleichzeitigen Benutzern, die Daten aktualisieren, oder die Daten, die sie aktualisieren unterscheidet sich von den anderen, ist parallelitätssteuerung kein wichtiger Aspekt. Wenn allerdings routinemäßig mehrere Benutzer auf Ihrer Website mit denselben Daten arbeiten müssen, können die parallelitätssteuerung verhindern, dass ein Benutzer Update- oder Löschvorgänge unwissentlich Überschreiben einer anderen Person.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

>[!div class="step-by-step"]
[Zurück](customizing-the-data-modification-interface-cs.md)
[Weiter](adding-client-side-confirmation-when-deleting-cs.md)
