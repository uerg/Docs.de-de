---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb
title: Implementieren von Optimistischer Parallelität (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Für eine Webanwendung, die mehreren Benutzern, Daten bearbeiten kann, besteht das Risiko, dass zwei Benutzer die gleichen Daten zur gleichen Zeit bearbeitet zur Verfügung. In diesem Tutori...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: 2646968c-2826-4418-b1d0-62610ed177e3
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb
msc.type: authoredcontent
ms.openlocfilehash: e33e4b401d957f4aa5560193dd8af0e53ca3b631
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837740"
---
<a name="implementing-optimistic-concurrency-vb"></a>Implementieren von Optimistischer Parallelität (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunter](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_21_VB.exe) oder [PDF-Datei herunterladen](implementing-optimistic-concurrency-vb/_static/datatutorial21vb1.pdf)

> Für eine Webanwendung, die mehreren Benutzern, Daten bearbeiten kann, besteht das Risiko, dass zwei Benutzer die gleichen Daten zur gleichen Zeit bearbeitet zur Verfügung. In diesem Tutorial implementieren wir Steuerung durch vollständige Parallelität, um dieses Risiko zu behandeln.


## <a name="introduction"></a>Einführung

Für Webanwendungen, die nur Benutzer Daten anzeigen können, oder für Benutzer, die nur einen einzigen Benutzer enthalten, der Daten ändern können, besteht keine Bedrohung durch zwei gleichzeitige Benutzer versehentlich die Änderungen überschrieben. Für Webanwendungen, mit denen mehrere Benutzer zu aktualisieren oder Löschen von Daten, allerdings besteht das Risiko eines Benutzers Änderungen mit einem anderen gleichzeitigen Benutzer miteinander in Konflikt geraten. Ohne die Richtlinie Parallelität vorhanden überschreibt der Benutzer, die ihre Änderungen ein Commit ausgeführt, wenn zwei Benutzer gleichzeitig einen einzelnen Datensatz bearbeiten die Änderungen, die im ersten zuletzt.

Angenommen Sie, dass zwei Benutzer, Jisun und Sam, sowohl eine Seite in unserer Anwendung, für die Besucher besuchen wurden, aktualisieren und löschen die Produkte über ein GridView-Steuerelement zulässig. Beide klicken Sie auf die Schaltfläche "Bearbeiten" in der GridView ungefähr zur selben Zeit aus. Jisun ändert sich der Name des Produkts zu "Chai Tee" und klickt auf die Schaltfläche "Aktualisieren". Das Ergebnis ist ein `UPDATE` -Anweisung, die für die Datenbank gesendet wird, die festlegt *alle* des Produkts aktualisierbaren Felder (obwohl Jisun nur für ein Feld aktualisiert `ProductName`). Zu diesem Zeitpunkt besitzt die Datenbank die Werte "Chai Tee," die Kategorie Getränke, Lieferanten außergewöhnlichen fluessiger Form, und so weiter für dieses bestimmte Produkt. Allerdings zeigt GridView auf Sams-Bildschirm als "Chai" immer noch den Namen des Produkts in der bearbeitbaren GridView-Zeile. Einige Sekunden nach Jisuns Änderungen übernommen wurden Sam-die Kategorie "Gewürze" updates und Update klickt. Dies führt zu einer `UPDATE` Anweisung gesendet, um die Datenbank, die Namen des Produkts zu "Chai," festlegt der `CategoryID` zu den entsprechenden Getränke Kategorie-ID, und So weiter. Die Jisun-Änderungen an den Namen des Produkts wurde überschrieben. Abbildung 1 zeigt grafisch dieser Serie von Ereignissen.


[![Wenn zwei Benutzer gleichzeitig Datensatz gibt es s Möglichkeit, dass ein Benutzer Änderungen der anderen Person überschrieben aktualisieren](implementing-optimistic-concurrency-vb/_static/image2.png)](implementing-optimistic-concurrency-vb/_static/image1.png)

**Abbildung 1**: Wenn zwei Benutzer gleichzeitig aktualisieren einen Datensatz vorhanden s Potenzial für Änderungen eines Benutzers zum Überschreiben der anderen Person ([klicken Sie, um das Bild in voller Größe anzeigen](implementing-optimistic-concurrency-vb/_static/image3.png))


Wenn zwei Benutzer eine Seite besuchen, kann ebenso ein Benutzer sein, sich mitten in einem Datensatz aktualisieren, wenn er von einem anderen Benutzer gelöscht wird. Alternativ dazu können Sie zwischen, wenn ein Benutzer eine Seite geladen wird, und wenn sie auf die Schaltfläche "löschen" klicken, kann auf einem anderen Benutzer den Inhalt dieses Datensatzes geändert haben.

Es gibt drei [parallelitätssteuerung](http://en.wikipedia.org/wiki/Concurrency_control) Strategien zur Verfügung:

- **Nichts Unternehmen** – Wenn gleichzeitige Benutzer den gleichen Datensatz ändern, können Sie den letzten Commit (das Standardverhalten) zu gewinnen
- [**Vollständige Parallelität** ](http://en.wikipedia.org/wiki/Optimistic_concurrency_control) -davon aus, die besteht möglicherweise Parallelität steht in Konflikt hin und wieder, den Großteil der Zeit, die solche Konflikte auftreten, wird nicht; daher, wenn ein Konflikt auftreten,, einfach die Benutzer darüber informieren, die ihre Änderungen kann nicht gespeichert werden, weil ein anderer Benutzer die gleichen Daten geändert hat
- **Pessimistische Nebenläufigkeit** -wird davon ausgegangen, dass Parallelitätskonflikte üblich, dass der Benutzer wird nicht toleriert wird nicht angewiesen, ihre Änderungen wurden nicht aufgrund gleichzeitiger Aktivität eines anderen Benutzers gespeichert; daher Sperre zu Beginn einer Benutzers Aktualisieren eines Datensatzes , wodurch verhindern, dass alle anderen Benutzer bearbeiten oder löschen diesen Datensatz aus, bis der Benutzer die Änderungen ein Commit ausgeführt

Alle unsere Tutorials verwendet bisher haben die standardmäßige Parallelität-Auflösungsstrategie – nämlich haben wir den letzten Schreibvorgang gewinnen können. In diesem Tutorial betrachten wir, wie Sie die Steuerung für optimistische Parallelität implementieren.

> [!NOTE]
> Beispiele für die eingeschränkte Parallelität in dieser tutorialreihe wird nicht erläutert. Eingeschränkte Parallelität wird nur selten verwendet werden, weil z. B. sperrt, wenn nicht ordnungsgemäß Cacheseiten, kann verhindern, dass andere Benutzer aktualisieren von Daten. Z. B. wenn ein Benutzer einen Datensatz für die Bearbeitung gesperrt, und dann verlassen, für den Tag hat, bevor es entsperrt, werden kein anderer Benutzer diesen Datensatz aktualisieren, bis der ursprüngliche Benutzer zurückgibt und seine standortupdates. Aus diesem Grund in Situationen, in denen die eingeschränkte Parallelität verwendet wird, besteht in der Regel ein Timeout, das, wenn erreicht, bricht die Sperre ab. Ticket sales Websites, bei denen Sperren einen bestimmten Arbeitsplätze-Speicherort für kurze Zeit, während der Benutzer mit der Order-Prozess abgeschlossen ist, ist ein Beispiel für die Steuerung durch eingeschränkte Parallelität.


## <a name="step-1-looking-at-how-optimistic-concurrency-is-implemented"></a>Schritt 1: Betrachten wie die vollständige Parallelität wird implementiert.

Steuerung für optimistische Parallelität funktioniert, indem Sie sicherstellen, dass der Datensatz aktualisieren oder löschen die gleichen Werte verfügt, wie zuvor beim Aktualisieren oder Löschen von Prozess starten. Z. B. beim Klicken auf die Schaltfläche "Bearbeiten" in einem bearbeitbaren GridView-Ansicht, die Werte des Datensatzes aus der Datenbank gelesen und in die Textfelder und anderen Websteuerelementen angezeigt. Diese ursprünglichen Werte werden durch die GridView gespeichert. Später, nachdem der Benutzer nimmt ihre Änderungen vor, und klickt auf die Schaltfläche "Aktualisieren", werden die ursprünglichen Werte sowie die neuen Werte in der Geschäftslogikebene und dann auf der Datenzugriffsebene gesendet. Die Datenzugriffsebene muss es sich um eine SQL-Anweisung ausgeben, die nur den Datensatz aktualisiert wird, wenn die ursprünglichen Werte, die der Benutzer bearbeiten gestartet, die Werte noch in der Datenbank identisch sind. Abbildung 2 zeigt diese Abfolge von Ereignissen.


[![Für die Update- oder Delete erfolgreich ausgeführt werden soll müssen die ursprünglichen Werte der aktuellen Datenbankwerte gleich sein.](implementing-optimistic-concurrency-vb/_static/image5.png)](implementing-optimistic-concurrency-vb/_static/image4.png)

**Abbildung 2**: für die Update- oder Delete, hergestellt wird, die ursprünglichen Werte müssen werden gleich die aktuellen Datenbankwerte ([klicken Sie, um das Bild in voller Größe anzeigen](implementing-optimistic-concurrency-vb/_static/image6.png))


Es gibt verschiedene Ansätze zum Implementieren von optimistischer Parallelität (finden Sie unter [Peter A. Bromberg](http://peterbromberg.net/)des [Optmistic Parallelität aktualisieren Logik](http://www.eggheadcafe.com/articles/20050719.asp) für einen kurzen Blick auf eine Reihe von Optionen). Die ADO.NET typisierte DataSet enthält eine Implementierung, die nur die Teilstriche eines Kontrollkästchens konfiguriert werden kann. Aktivieren der optimistischen Parallelität für ein TableAdapter im typisierten DataSet der TableAdapters erweitert `UPDATE` und `DELETE` -Anweisungen enthalten einen Vergleich aller von den ursprünglichen Werten in der `WHERE` Klausel. Die folgenden `UPDATE` -Anweisung aktualisiert z. B. den Namen und den Preis eines Produkts nur dann, wenn die aktuellen Datenbankwerte die Werte gleich sind, die ursprünglich, beim Aktualisieren des Datensatzes in den GridView-Ansicht abgerufen wurden. Die `@ProductName` und `@UnitPrice` Parameter enthalten die neuen Werten, die vom Benutzer eingegeben haben, während `@original_ProductName` und `@original_UnitPrice` enthalten die Werte, die ursprünglich in der GridView geladen wurden, als auf die Schaltfläche "Bearbeiten" geklickt wurde:


[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample1.sql)]

> [!NOTE]
> Dies `UPDATE` Anweisung wurde zur besseren Lesbarkeit vereinfacht. In der Praxis die `UnitPrice` Einchecken der `WHERE` Klausel wäre etwas komplexer, da `UnitPrice` darf `NULL` s und überprüfen, wenn `NULL = NULL` gibt immer "false" zurück (Sie müssen stattdessen `IS NULL`).


Zusätzlich zur Verwendung einer anderen zugrunde liegenden `UPDATE` -Anweisung, konfigurieren einen TableAdapter verwenden der optimistischen Parallelität ändert außerdem die Signatur der DB direkte Methoden. Erinnern Sie sich an unser Tutorial erste [ *Erstellen einer Datenzugriffsschicht*](../introduction/creating-a-data-access-layer-cs.md), dass DB direkte Methoden sind diejenigen, die akzeptiert eine Liste von skalaren Werte als Eingabeparameter (statt als stark typisierte DataRow oder DataTable-Instanz). Bei Verwendung von optimistischer Parallelität der Datenbank, die direkte `Update()` und `Delete()` Methoden umfassen die Eingabeparameter für die ursprünglichen Werte ebenfalls. Darüber hinaus das Aktualisieren des Codes in die BLL für die Verwendung von Batch Muster (die `Update()` methodenüberladungen, DataRows und DataTables und nicht als skalare Werte akzeptieren) muss ebenfalls geändert werden.

Stattdessen als Erweitern unsere vorhandenen TableAdapters von der DAL verwenden Sie optimistischen Parallelität (die derart ändern die BLL zu ermöglichen), lassen Sie uns stattdessen Erstellen eines neuen typisierten Datasets mit dem Namen `NorthwindOptimisticConcurrency`, auf die wir Hinzufügen einer `Products` TableAdapter, vollständigen Parallelität wird verwendet. Danach erstellen wir eine `ProductsOptimisticConcurrencyBLL` Business Logic Layer-Klasse, die die entsprechenden Änderungen zur Unterstützung der optimistischen Parallelität DAL verfügt. Auf dieser Grundlage festgelegt ist, werden feststellen, dass wir bereit, die ASP.NET-Seite zu erstellen.

## <a name="step-2-creating-a-data-access-layer-that-supports-optimistic-concurrency"></a>Schritt 2: Erstellen einer Datenzugriffsschicht mit unterstützt optimistische Parallelität

Um einen neuen typisierte DataSet zu erstellen, mit der Maustaste auf die `DAL` Ordner innerhalb der `App_Code` Ordner, und fügen Sie ein neues DataSet mit dem Namen `NorthwindOptimisticConcurrency`. Wie wir im ersten Tutorial gesehen haben, wird dadurch das typisierte DataSet, starten den TableAdapter-Konfigurations-Assistenten automatisch so einen neuen TableAdapter hinzugefügt. Im ersten Bildschirm werden wir eine aufgefordert, geben Sie die Datenbank zum Herstellen einer Verbindung mit – Verbindung mit der gleichen Datenbank Northwind mithilfe der `NORTHWNDConnectionString` aus `Web.config`.


[![Verbinden Sie mit der gleichen Northwind-Datenbank](implementing-optimistic-concurrency-vb/_static/image8.png)](implementing-optimistic-concurrency-vb/_static/image7.png)

**Abbildung 3**: Verbinden mit der gleichen Northwind-Datenbank ([klicken Sie, um das Bild in voller Größe anzeigen](implementing-optimistic-concurrency-vb/_static/image9.png))


Wir werden dann aufgefordert, wie die Daten abzufragen: über eine Ad-hoc-SQL-Anweisung eine neue gespeicherte Prozedur oder eine vorhandene gespeicherte Prozedur. Da wir in unserem ursprünglichen DAL Ad-hoc-SQL-Abfragen verwendet, verwenden Sie diese Option hier ebenfalls.


[![Geben Sie die Daten abgerufen, mit Ad-hoc-SQL-Anweisungen](implementing-optimistic-concurrency-vb/_static/image11.png)](implementing-optimistic-concurrency-vb/_static/image10.png)

**Abbildung 4**: Geben Sie die Daten zum Abrufen, die mit Ad-hoc-SQL-Anweisungen ([klicken Sie, um das Bild in voller Größe anzeigen](implementing-optimistic-concurrency-vb/_static/image12.png))


Geben Sie auf dem folgenden Bildschirm die SQL-Abfrage zu verwenden, um die Produktinformationen abzurufen. Verwenden wir die genaue gleiche SQL-Abfrage, die zum die `Products` TableAdapter unter Verwendung des ursprünglichen DAL, wodurch alle die `Product` Spalten zusammen mit den produktanforderungen Supplier "und" Kategorie Namen:


[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample2.sql)]


[![Verwenden Sie die gleiche SQL-Abfrage aus der ProductsTableAdapter in der ursprünglichen DAL](implementing-optimistic-concurrency-vb/_static/image14.png)](implementing-optimistic-concurrency-vb/_static/image13.png)

**Abbildung 5**: Verwenden Sie die gleiche SQL-Abfrage aus der `Products` TableAdapter in der ursprünglichen DAL ([klicken Sie, um das Bild in voller Größe anzeigen](implementing-optimistic-concurrency-vb/_static/image15.png))


Klicken Sie bevor Sie fortfahren, auf dem nächsten Bildschirm auf die Schaltfläche "Erweiterte Optionen". Damit diese TableAdapter einsetzen-Steuerung für optimistische Parallelität, einfach das Kontrollkästchen Sie "Verwenden von optimistischer Parallelität".


[![Aktivieren der Steuerung für optimistische Parallelität durch Überprüfen der &quot;Verwenden von optimistischer Parallelität&quot; Kontrollkästchen](implementing-optimistic-concurrency-vb/_static/image17.png)](implementing-optimistic-concurrency-vb/_static/image16.png)

**Abbildung 6**: Aktivieren Sie Steuerung für optimistische Parallelität durch Aktivieren des Kontrollkästchens "Verwenden von optimistischer Parallelität" ([klicken Sie, um das Bild in voller Größe anzeigen](implementing-optimistic-concurrency-vb/_static/image18.png))


Geben Sie schließlich, dass der TableAdapter-Datenzugriffsmuster verwenden soll, die DataTable füllen und DataTable zurückgeben; Geben Sie außerdem, dass die DB direkten Methoden erstellt werden soll. Ändern Sie dem Methodennamen für die Rückgabe einer DataTable-Muster von GetData in GetProducts, um spiegeln die Benennungskonventionen, die wir in unserem ursprünglichen DAL verwendet.


[![Haben Sie den TableAdapter verwenden alle Datenzugriffsmuster](implementing-optimistic-concurrency-vb/_static/image20.png)](implementing-optimistic-concurrency-vb/_static/image19.png)

**Abbildung 7**: haben Sie die TableAdapter verwenden alle Datenzugriffsmuster ([klicken Sie, um das Bild in voller Größe anzeigen](implementing-optimistic-concurrency-vb/_static/image21.png))


Nach Abschluss des Assistenten, der DataSet-Designer umfasst einen stark typisierten `Products` DataTable und TableAdapter-Klasse. Benennen Sie die DataTable aus in Ruhe `Products` zu `ProductsOptimisticConcurrency`, dies können Sie mit der rechten Maustaste in der DataTable Titelleiste des Fensters, und benennen Sie im Kontextmenü auswählen.


[![Eine DataTable und TableAdapter wurden mit dem typisierten DataSet hinzugefügt](implementing-optimistic-concurrency-vb/_static/image23.png)](implementing-optimistic-concurrency-vb/_static/image22.png)

**Abbildung 8**: eine DataTable und TableAdapter hinzugefügt wurden, die typisierte DataSet ([klicken Sie, um das Bild in voller Größe anzeigen](implementing-optimistic-concurrency-vb/_static/image24.png))


Die Unterschiede zwischen der `UPDATE` und `DELETE` Abfragen zwischen der `ProductsOptimisticConcurrency` TableAdapter (der vollständigen Parallelität verwendet) und die ProductsTableAdapter (was nicht), klicken Sie auf den TableAdapter, und wechseln Sie zu dem Fenster "Eigenschaften". In der `DeleteCommand` und `UpdateCommand` Eigenschaften `CommandText` Untereigenschaften sehen Sie die eigentliche SQL-Syntax, die an die Datenbank gesendet wird, wenn die DAL Update oder Delete-verwandten Methoden aufgerufen werden. Für die `ProductsOptimisticConcurrency` TableAdapter die `DELETE` -Anweisung verwendet wird:


[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample3.sql)]

Während der `DELETE` -Anweisung für den TableAdapter im ursprünglichen DAL-Produkt ist viel einfacher:


[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample4.sql)]

Wie Sie sehen können, die `WHERE` -Klausel in der `DELETE` -Anweisung für den TableAdapter, der wiederum optimistische Parallelität nutzt enthält einen Vergleich zwischen den einzelnen der `Product` Tabelle vorhandenen Spaltenwerte und die ursprünglichen Werte zum Zeitpunkt der GridView (oder DetailsView oder FormView) wurde zuletzt aufgefüllt. Da alle Felder außer `ProductID`, `ProductName`, und `Discontinued` können `NULL` Werte, die zusätzlichen Parametern und Überprüfungen werden eingeschlossen, um ordnungsgemäß verglichen `NULL` Werte in der `WHERE` Klausel.

Wir wird keine zusätzliche DataTables auf das vollständige nebenläufigkeitsfähigen DataSet für dieses Tutorial hinzufügen wie unsere ASP.NET-Seite nur erhalten, aktualisieren und Löschen von Produktinformationen. Wir weiterhin müssen allerdings Hinzufügen der `GetProductByProductID(productID)` Methode, um die `ProductsOptimisticConcurrency` TableAdapter.

Zu diesem Zweck mit der Maustaste, auf dem TableAdapter-Titelleiste (Bereich rechts oben die `Fill` und `GetProducts` Methodennamen), und wählen Sie im Kontextmenü der Abfrage hinzufügen. Hierdurch wird die Konfigurations-Assistent die TableAdapter-Abfragen. Wie entscheiden Sie sich mit der Erstkonfiguration des TableAdapter zum Erstellen der `GetProductByProductID(productID)` Methode, die mit Ad-hoc-SQL-Anweisungen (siehe Abbildung 4). Da die `GetProductByProductID(productID)` Methode gibt Informationen zu einem bestimmten Produkt zurück, um anzugeben, dass diese Abfrage ist eine `SELECT` Abfragetyp, die Zeilen zurückgibt.


[![Markieren Sie den Typ der Abfrage als ein &quot;wählen Sie die Zeilen zurückgibt&quot;](implementing-optimistic-concurrency-vb/_static/image26.png)](implementing-optimistic-concurrency-vb/_static/image25.png)

**Abbildung 9**: Markieren Sie den Typ der Abfrage als ein "`SELECT` Zeilen zurückgegeben" ([klicken Sie, um das Bild in voller Größe anzeigen](implementing-optimistic-concurrency-vb/_static/image27.png))


Auf dem nächsten Bildschirm werden wir für die SQL-Abfrage verwenden, mit dem TableAdapter-Standardabfrage vorab geladenen aufgefordert. Erweitern Sie die vorhandene Abfrage aus, um die Klausel erweitert `WHERE ProductID = @ProductID`, wie in Abbildung 10 dargestellt.


[![Hinzufügen eine WHERE-Klausel, um die vorab geladenen Abfrage einen bestimmtes Produktdatensatz zurückgeben](implementing-optimistic-concurrency-vb/_static/image29.png)](implementing-optimistic-concurrency-vb/_static/image28.png)

**Abbildung 10**: Hinzufügen einer `WHERE` -Klausel, um die Pre-Loaded Abfrage einem bestimmten Produktdatensatz zurückgeben ([klicken Sie, um das Bild in voller Größe anzeigen](implementing-optimistic-concurrency-vb/_static/image30.png))


Abschließend ändern Sie die generierte Methode Objektnamen `FillByProductID` und `GetProductByProductID`.


[![Benennen Sie die Methoden FillByProductID und GetProductByProductID](implementing-optimistic-concurrency-vb/_static/image32.png)](implementing-optimistic-concurrency-vb/_static/image31.png)

**Abbildung 11**: Benennen Sie die Methoden zum `FillByProductID` und `GetProductByProductID` ([klicken Sie, um das Bild in voller Größe anzeigen](implementing-optimistic-concurrency-vb/_static/image33.png))


Mit dieser Assistent abgeschlossen wurde, enthält der TableAdapter nun zwei Methoden zum Abrufen von Daten: `GetProducts()`, gibt *alle* Products und `GetProductByProductID(productID)`, die das angegebene Produkt zurückgibt.

## <a name="step-3-creating-a-business-logic-layer-for-the-optimistic-concurrency-enabled-dal"></a>Schritt 3: Erstellen einer Geschäftslogikebene für optimistische Parallelität-fähigen DAL

Unsere vorhandenen `ProductsBLL` -Klasse verfügt über die Beispiele für die Verwendung sowohl die direkten DB-Muster die Batchaktualisierung. Die `AddProduct` Methode und `UpdateProduct` beide Überladungen verwenden Batch updatemusters, übergibt ein `ProductRow` Instanz TableAdapters-Update-Methode. Die `DeleteProduct` Methode, die auf der anderen Seite verwendet das DB-direct-Muster, der TableAdapters aufrufen `Delete(productID)` Methode.

Mit dem neuen `ProductsOptimisticConcurrency` TableAdapter, die Datenbank direkte Methoden erfordern jetzt, dass auch die ursprünglichen Werte übergeben werden. Z. B. die `Delete` Methode jetzt erwartet, dass zehn Eingabeparameter: die ursprüngliche `ProductID`, `ProductName`, `SupplierID`, `CategoryID`, `QuantityPerUnit`, `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, `ReorderLevel`, und `Discontinued`. Er verwendet diese zusätzliche Eingabeparameter Werte in `WHERE` -Klausel der `DELETE` Anweisung, die für die Datenbank gesendet werden, nur den angegebenen Datensatz zu löschen, wenn die aktuellen Werte von der Datenbank bis zu die ursprünglichen zu ordnen.

Während Sie die Signatur der Methode für der TableAdapters `Update` in Batch updatemusters verwendete Methode hat sich nicht geändert, die der erforderlichen Code zum Zeichnen die ursprünglichen und neuen Werte hat. Aus diesem Grund und nicht als zu versuchen, verwenden Sie die optimistische Parallelität-fähigen DAL mit unserer vorhandenen `ProductsBLL` Klasse, die wir erstellen eine neue Business Logic Layer-Klasse für die Arbeit mit unseren neuen DAL.

Fügen Sie eine Klasse, die mit dem Namen `ProductsOptimisticConcurrencyBLL` auf die `BLL` Ordner innerhalb der `App_Code` Ordner.


![Fügen Sie der ProductsOptimisticConcurrencyBLL-Klasse für den BLL-Ordner](implementing-optimistic-concurrency-vb/_static/image34.png)

**Abbildung 12**: Hinzufügen der `ProductsOptimisticConcurrencyBLL` Klasse, um den BLL-Ordner


Als Nächstes fügen Sie den folgenden Code der `ProductsOptimisticConcurrencyBLL` Klasse:


[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample5.vb)]

Beachten Sie, die mit `NorthwindOptimisticConcurrencyTableAdapters` Anweisung über den Start der Klassendeklaration. Die `NorthwindOptimisticConcurrencyTableAdapters` -Namespace enthält die `ProductsOptimisticConcurrencyTableAdapter` -Klasse, die die DAL Methoden bereitstellt. Auch vor der Klassendeklaration finden Sie die `System.ComponentModel.DataObject` -Attribut, das Visual Studio, um diese Klasse enthalten, in der Dropdownliste für das ObjectDataSource-Steuerelement-Assistent weist.

Die `ProductsOptimisticConcurrencyBLL`des `Adapter` Eigenschaft ermöglicht den schnellen Zugriff auf eine Instanz von der `ProductsOptimisticConcurrencyTableAdapter` Klasse, und folgt dem Muster, die in unserem ursprünglichen BLL-Klassen verwendet (`ProductsBLL`, `CategoriesBLL`und so weiter). Schließlich die `GetProducts()` Methode ruft einfach nach unten in der DAL `GetProducts()` -Methode und gibt eine `ProductsOptimisticConcurrencyDataTable` -Objekt aufgefüllt, mit einer `ProductsOptimisticConcurrencyRow` -Instanz für jede Produktdatensatz in der Datenbank.

## <a name="deleting-a-product-using-the-db-direct-pattern-with-optimistic-concurrency"></a>Löschen eines Produkts, die vollständige Parallelität mit dem direkten DB-Muster

Verwendung des direkten DB-Musters mit einer DAL, die vollständigen Parallelität verwendet, müssen die Methoden der neuen und ursprünglichen Werte übergeben werden. Für das Löschen, es sind keine neuen Werte, daher müssen nur die ursprünglichen Werte übergeben werden. In unserem BLL klicken Sie dann akzeptieren wir alle ursprünglichen Parameter als Eingabeparameter. Lassen Sie uns die `DeleteProduct` -Methode in der die `ProductsOptimisticConcurrencyBLL` Klasse verwenden, die direkte DB-Methode. Dies bedeutet, dass diese Methode muss in allen Feldern der zehn Product-Daten als Eingabeparameter nutzen, und übergeben diese an die DAL aus, wie im folgenden Code gezeigt:


[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample6.vb)]

Wenn die ursprünglichen Werte – diese Werte, die zuletzt in der GridView (oder DetailsView oder FormView-Steuerelement) geladen wurden – unterscheiden sich von den Werten in der Datenbank, klickt der Benutzer die Schaltfläche zum Löschen der `WHERE` Klausel wird nicht mit jeder Datenbank-Datensatz und keine Datensätze übereinstimmen sind betroffen. Daher der TableAdapters `Delete` Methode zurück `0` und den BLL `DeleteProduct` Methode zurück `false`.

## <a name="updating-a-product-using-the-batch-update-pattern-with-optimistic-concurrency"></a>Aktualisieren eines Produkts, mit dem Batch-Update-Muster mit vollständiger Parallelität

Wie bereits erwähnt von der TableAdapters `Update` Methode für das Batch-Update-Muster weist die gleiche Methodensignatur, unabhängig davon, ob die vollständige Parallelität verwendet wird. Nämlich die `Update` Methode erwartet, dass eine DataRow, ein Array von DataRows, einer "DataTable" oder eines typisierten Datasets. Es gibt keine zusätzlichen Eingabeparameter für das die ursprünglichen Werte angeben. Dies ist möglich, da die DataTable verfolgt die ursprünglichen und geänderten Werte für die DataRow(s) des. Wenn die DAL gibt seine `UPDATE` -Anweisung, die `@original_ColumnName` Parameter werden mit der ursprünglichen Werte der DataRow aufgefüllt, während die `@ColumnName` Parameter werden durch geänderte Werte der DataRow aufgefüllt.

In der `ProductsBLL` -Klasse (der unsere ursprüngliche, nicht optimistische Parallelität DAL verwendet), wenn das Batch-Update-Muster zu verwenden, um Produktinformationen, die unseren Code führt die folgende Sequenz von Ereignissen zu aktualisieren:

1. Lesen die aktuelle Datenbank Produktinformationen in einen `ProductRow` -Instanz mit der TableAdapters `GetProductByProductID(productID)` Methode
2. Weisen Sie die neuen Werte an die `ProductRow` -Instanz aus Schritt 1
3. Rufen Sie die `Update` Methode und übergeben die `ProductRow` Instanz

Diese Abfolge von Schritten, jedoch wird nicht ordnungsgemäß unterstützen die optimistische Parallelität da die `ProductRow` aufgefüllt, die Schritt 1 wird aufgefüllt, direkt aus der Datenbank, was bedeutet, dass die ursprünglichen Werte, die von der DataRow verwendet, die derzeit im vorhanden sind die die Datenbank und nicht die, die an die GridView zu Beginn der Bearbeitung gebunden wurden. Stattdessen bei mithilfe einer optimistischen Parallelität-DAL aktiviert, wir müssen zum Ändern der `UpdateProduct` methodenüberladungen, um die folgenden Schritte ausführen:

1. Lesen die aktuelle Datenbank Produktinformationen in einen `ProductsOptimisticConcurrencyRow` -Instanz mit der TableAdapters `GetProductByProductID(productID)` Methode
2. Weisen Sie die *ursprünglichen* -Werte in der `ProductsOptimisticConcurrencyRow` -Instanz aus Schritt 1
3. Rufen Sie die `ProductsOptimisticConcurrencyRow` Instanz `AcceptChanges()` -Methode, die der DataRow weist an, dass die aktuellen Werte der "original" sind
4. Weisen Sie die *neue* -Werte in der `ProductsOptimisticConcurrencyRow` Instanz
5. Rufen Sie die `Update` Methode und übergeben die `ProductsOptimisticConcurrencyRow` Instanz

Schritt 1-Lesevorgänge in allen von der aktuellen Datenbankwerte für den angegebenen Datensatz. Dieser Schritt ist überflüssig, in der `UpdateProduct` Überladung, die aktualisiert *alle* von der Produkt-Spalten (wie diese Werte werden überschrieben, in Schritt2), jedoch ist sehr wichtig für diese Überladungen, in denen nur eine Teilmenge der Werte in den Spalten werden als übergeben, Eingabeparameter. Nachdem die ursprünglichen Werte zugewiesen wurden die `ProductsOptimisticConcurrencyRow` -Instanz die `AcceptChanges()` Methode wird aufgerufen, die die aktuellen Werte der DataRow markiert, als die ursprünglichen Werte in verwendet werden die `@original_ColumnName` Parameter in der `UPDATE` Anweisung. Als Nächstes werden die neuen Werte der Parameter zugewiesen, auf die `ProductsOptimisticConcurrencyRow` und schließlich die `Update` Methode wird aufgerufen, in der DataRow übergeben.

Der folgende code zeigt die `UpdateProduct` Überladung, die alle Produktdaten akzeptiert Felder als Eingabeparameter. Obwohl hier nicht angezeigt, die `ProductsOptimisticConcurrencyBLL` Klasse, die im Download enthalten, für dieses Tutorial auch enthält eine `UpdateProduct` Überladung, die nur des produktanforderungen Name und Preis als Eingabeparameter akzeptiert.


[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample7.vb)]

## <a name="step-4-passing-the-original-and-new-values-from-the-aspnet-page-to-the-bll-methods"></a>Schritt 4: Übergeben die ursprünglichen und neuen Werte von der ASP.NET-Seite an die BLL-Methoden

Der DAL und vollständige BLL übrig bleibt eine ASP.NET-Seite erstellen, die die an das System integrierte Logik für vollständige Parallelität nutzen können. Insbesondere müssen die Daten-Websteuerelement (die GridView, DetailsView oder FormView) daran denken, dass die ursprünglichen Werte und dem ObjectDataSource-Steuerelement beide Sätze von Werten an den Geschäftslogikebene übergeben werden müssen. Darüber hinaus muss die ASP.NET-Seite konfiguriert werden, um problemlos parallelitätsverletzungen behandeln.

Öffnen Sie zunächst die `OptimisticConcurrency.aspx` auf der Seite die `EditInsertDelete` Ordner und Hinzufügen einer GridView-Ansicht im Designer festlegen seiner `ID` Eigenschaft `ProductsGrid`. Aus den GridView Smarttag, deaktivieren Sie zum Erstellen einer neuen, mit dem Namen "ObjectDataSource" `ProductsOptimisticConcurrencyDataSource`. Da diese "ObjectDataSource" die DAL verwenden, die vollständige Parallelität unterstützt werden soll, konfigurieren Sie ihn zur Verwendung der `ProductsOptimisticConcurrencyBLL` Objekt.


[![Haben Sie die Verwendung von "ObjectDataSource" das ProductsOptimisticConcurrencyBLL-Objekt](implementing-optimistic-concurrency-vb/_static/image36.png)](implementing-optimistic-concurrency-vb/_static/image35.png)

**Abbildung 13**: haben Sie die Verwendung von "ObjectDataSource" die `ProductsOptimisticConcurrencyBLL` Objekt ([klicken Sie, um das Bild in voller Größe anzeigen](implementing-optimistic-concurrency-vb/_static/image37.png))


Wählen Sie die `GetProducts`, `UpdateProduct`, und `DeleteProduct` Methoden aus den Dropdownlisten im Assistenten. Verwenden Sie die Überladung, die alle Datenfelder des Produkts akzeptiert, bei der UpdateProduct-Methode.

## <a name="configuring-the-objectdatasource-controls-properties"></a>Konfigurieren von Eigenschaften für das ObjectDataSource-Steuerelement

Nach Abschluss des Assistenten sollte dem ObjectDataSource-Steuerelement deklarative Markup wie folgt aussehen:


[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample8.aspx)]

Wie Sie sehen können, die `DeleteParameters` Auflistung enthält ein `Parameter` Instanz für jedes der zehn Eingabeparameter in der `ProductsOptimisticConcurrencyBLL` Klasse `DeleteProduct` Methode. Ebenso die `UpdateParameters` Auflistung enthält ein `Parameter` Instanz für jedes der Eingabeparameter im `UpdateProduct`.

Für diese vorherigen Tutorials, die die Datenänderung beteiligt, würden wir dem ObjectDataSource-Steuerelement entfernen `OldValuesParameterFormatString` Eigenschaft an diesem Punkt ist, da diese Eigenschaft gibt an, dass die BLL-Methode die alten (oder ursprüngliche) Werte übergeben werden muss, sowie die neuen Werte erwartet. Darüber hinaus gibt Wert dieser Eigenschaft die Namen der Eingabeparameter für die ursprünglichen Werte an. Da wir die ursprünglichen Werte in die BLL übergeben werden, sind *nicht* entfernen Sie diese Eigenschaft.

> [!NOTE]
> Der Wert des der `OldValuesParameterFormatString` Eigenschaft muss die Eingabeparameter-Namen in die BLL, bei denen die ursprünglichen Werte zugeordnet. Da wir diese Parameter mit dem Namen `original_productName`, `original_supplierID`usw., lassen Sie die `OldValuesParameterFormatString` Eigenschaftswert als `original_{0}`. Hätte, aber der BLL Methoden Parameter Namen wie `old_productName`, `old_supplierID`usw., müssen Sie aktualisieren die `OldValuesParameterFormatString` Eigenschaft `old_{0}`.


Es gibt eine letzte Eigenschaft-Einstellung, die in der Reihenfolge für das ObjectDataSource-Steuerelement auf die ursprünglichen Werte ordnungsgemäß an die BLL-Methoden übergeben, erstellt werden muss. Dem ObjectDataSource-Steuerelement verfügt über eine [ConflictDetection Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.conflictdetection.aspx) , die zugewiesen werden [einen von zwei Werten](https://msdn.microsoft.com/library/system.web.ui.conflictoptions.aspx):

- `OverwriteChanges` – Der Standardwert; die ursprünglichen Werte werden der BLL Methoden ursprünglichen Parameter nicht gesendet werden.
- `CompareAllValues` : sendet die ursprünglichen Werte, die BLL-Methoden Wählen Sie diese Option aus, wenn Sie vollständigen Parallelität verwenden

Legen Sie in Ruhe die `ConflictDetection` Eigenschaft `CompareAllValues`.

## <a name="configuring-the-gridviews-properties-and-fields"></a>Konfigurieren von GridView Eigenschaften und Felder

Mit dem ObjectDataSource-Steuerelement-Eigenschaften ordnungsgemäß konfiguriert konzentrieren wir uns für das Einrichten der GridView. Da wir die GridView zu unterstützen, bearbeiten und löschen möchten, klicken Sie zunächst auf die Kontrollkästchen aktivieren Sie bearbeiten und löschen aktivieren von GridView Smarttag. Dadurch wird eine CommandField hinzugefügt, deren `ShowEditButton` und `ShowDeleteButton` auf festlegen `true`.

Bei der Bindung an die `ProductsOptimisticConcurrencyDataSource` ObjectDataSource GridView enthält ein Feld für die einzelnen Datenfelder des Produkts. Während solche einer GridView-Ansicht bearbeitet werden kann, ist die Benutzeroberfläche alles, was jedoch akzeptabel. Die `CategoryID` und `SupplierID` BoundFields rendert als Textfelder ein, dass der Benutzer die entsprechende Kategorie und der Lieferant als ID-Nummern eingeben. Es werden keine Formatierung für numerische Felder und keine Validierungssteuerelemente, um sicherzustellen, dass den Namen des Produkts angegeben wurde und der Preis, im Lager vorrätigen Stückzahl, Einheiten auf Reihenfolge und Werte für die neuanordnung beide entsprechenden numerischen Werte sind und größer als oder gleich 0 (null).

Wie wir unter den *Hinzufügen von Steuerelementen zur gültigkeitsprüfung zum Bearbeiten und Einfügen von Schnittstellen* und *Anpassen der Benutzeroberfläche für die Änderung der Daten* Tutorials, die Benutzeroberfläche kann angepasst werden, indem Ersetzen die BoundFields durch von TemplateFields an. Ich habe diese GridView und die Bearbeitung Schnittstelle folgendermaßen geändert:

- Entfernt die `ProductID`, `SupplierName`, und `CategoryName` BoundFields
- Konvertiert die `ProductName` BoundField in ein TemplateField und ein Steuerelement RequiredFieldValidation hinzugefügt.
- Konvertiert die `CategoryID` und `SupplierID` BoundFields, von TemplateFields, und die Bearbeitungsschnittstelle DropDownList-Steuerelementen statt Textfelder Verwendung angepasst. In diesen von TemplateFields `ItemTemplates`, `CategoryName` und `SupplierName` Datenfelder angezeigt.
- Konvertiert die `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, und `ReorderLevel` BoundFields von TemplateFields und hinzugefügten CompareValidator-Steuerelement.

Da wir bereits untersucht, wie diese Aufgaben in vorherigen Tutorials haben, ich einfach hier die letzte deklarative Syntax auflisten und lassen Sie die Implementierung als Methode.


[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample9.aspx)]

Wir sind sehr nahe, dass ein voll funktionsfähiges Beispiel. Es gibt jedoch einige Besonderheiten, die sich einschleichen und uns Probleme verursachen. Darüber hinaus benötigen wir eine Schnittstelle, mit der den Benutzer benachrichtigt, wenn eine parallelitätsverletzung aufgetreten ist.

> [!NOTE]
> Damit eine Daten-Websteuerelement, übergeben die ursprünglichen Werte ordnungsgemäß auf dem ObjectDataSource-Steuerelement (die anschließend an die BLL übergeben werden), es ist wichtig, die des GridView `EnableViewState` -Eigenschaftensatz auf `true` (Standard). Wenn Sie den Ansichtszustand deaktivieren, werden die ursprünglichen Werte beim Postback verloren.


## <a name="passing-the-correct-original-values-to-the-objectdatasource"></a>Übergeben die richtigen Werte für die ursprünglichen, an dem ObjectDataSource-Steuerelement

Es gibt einige Probleme mit der Möglichkeit, die die GridView konfiguriert wurde. Wenn dem ObjectDataSource-Steuerelement `ConflictDetection` -Eigenschaftensatz auf `CompareAllValues` (als ist unsere), wenn dem ObjectDataSource-Steuerelement `Update()` oder `Delete()` Methoden werden aufgerufen, durch die GridView (DetailsView oder FormView-Steuerelement), wird dem ObjectDataSource-Steuerelement versucht wird, kopiert der GridView ursprünglichen Werte in der entsprechenden `Parameter` Instanzen. Siehe Abbildung 2 für eine grafische Darstellung dieses Prozesses.

GridView ursprünglichen Werte werden insbesondere die Werte in den Anweisungen für die bidirektionale Datenbindung zugewiesen, jedes Mal die Daten an die GridView gebunden werden. Aus diesem Grund ist es wichtig, dass die erforderlichen ursprünglichen Werte, die alle über bidirektionale Datenbindung erfasst werden und werden, dass sie in einem Format konvertiert werden kann.

Um festzustellen, warum dies wichtig ist, nehmen Sie einen Moment Zeit, besuchen unsere Seite in einem Browser. Erwartungsgemäß funktioniert, sind die GridView jedes Produkt mit einer Schaltfläche Bearbeiten und Löschen in der linken Spalte aufgeführt.


[![Die Produkte sind in einer GridView-Ansicht aufgeführt.](implementing-optimistic-concurrency-vb/_static/image39.png)](implementing-optimistic-concurrency-vb/_static/image38.png)

**Abbildung 14**: der Produkte finden Sie in einer GridView-Ansicht ([klicken Sie, um das Bild in voller Größe anzeigen](implementing-optimistic-concurrency-vb/_static/image40.png))


Wenn Sie die Schaltfläche "löschen" für jedes Produkt, klicken Sie auf eine `FormatException` ausgelöst.


[![Versucht, alle Produktergebnisse in FormatException zu löschen.](implementing-optimistic-concurrency-vb/_static/image42.png)](implementing-optimistic-concurrency-vb/_static/image41.png)

**Abbildung 15**: Es wird versucht, Any Produktergebnisse löschen, in einem `FormatException` ([klicken Sie, um das Bild in voller Größe anzeigen](implementing-optimistic-concurrency-vb/_static/image43.png))


Die `FormatException` wird ausgelöst, wenn versucht wird, dass dem ObjectDataSource-Steuerelement finden in der ursprünglichen `UnitPrice` Wert. Da die `ItemTemplate` hat die `UnitPrice` als Währung formatiert (`<%# Bind("UnitPrice", "{0:C}") %>`), es enthält kein Währungssymbol wie 19,95 $. Die `FormatException` auftritt, versucht, dass dem ObjectDataSource-Steuerelement konvertieren diese Zeichenfolge in eine `decimal`. Um dieses Problem zu umgehen, haben wir eine Reihe von Optionen an:

- Entfernen Sie die Währung, die Formatierung der `ItemTemplate`. Das heißt, anstelle von `<%# Bind("UnitPrice", "{0:C}") %>`, verwenden Sie einfach `<%# Bind("UnitPrice") %>`. Der Nachteil dieses ist, dass der Preis nicht mehr formatiert ist.
- Anzeigen der `UnitPrice` formatiert als Währung in der `ItemTemplate`, aber die `Eval` Schlüsselwort, um dies zu erreichen. Zur Erinnerung: `Eval` führt unidirektionale Datenbindung. Wir benötigen, geben Sie die `UnitPrice` Wert für die ursprünglichen Werte, damit wir weiterhin, dass eine bidirektionale Datenbindung-Anweisung in benötigen der `ItemTemplate`, aber dies platziert werden in ein Label-Steuerelement, dessen `Visible` -Eigenschaftensatz auf `false`. Wir können das folgende Markup in der ItemTemplate verwenden:


[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample10.aspx)]

- Entfernen Sie die Währung, die Formatierung der `ItemTemplate`mit `<%# Bind("UnitPrice") %>`. In des GridView `RowDataBound` -Ereignishandler, programmgesteuerten Zugriff im Web Bezeichnung steuern, in dem die `UnitPrice` Wert angezeigt wird, und legen Sie dessen `Text` Eigenschaft, um die formatierte Version.
- Lassen Sie die `UnitPrice` als Währung formatiert. In des GridView `RowDeleting` -Ereignishandler ersetzen die vorhandene ursprüngliche `UnitPrice` Wert (19,95$) mit einem tatsächlichen Dezimalwert `Decimal.Parse`. Erläutert, wie etwa in der `RowUpdating` -Ereignishandler in der [ *behandeln BLL- und DAL-Ebene von Ausnahmen in einer ASP.NET-Seite* ](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) Tutorial.

Für mein Beispiel habe ich entschieden, die im Zusammenhang mit der zweiten Methode ein ausgeblendetes Bezeichnung Web hinzufügen-Steuerelement, dessen `Text` -Eigenschaft ist bidirektional, an die unformatierte datengebunden `UnitPrice` Wert.

Versuchen Sie nach der Lösung dieses Problems, klicken Sie erneut auf die Schaltfläche "löschen" für jedes Produkt aus. Dieses Mal Sie erhalten eine `InvalidOperationException` Wenn dem ObjectDataSource-Steuerelement versucht, rufen Sie der BLL des `UpdateProduct` Methode.


[![Dem ObjectDataSource-Steuerelement kann eine Methode mit den Eingabeparametern finden, die sie senden möchte](implementing-optimistic-concurrency-vb/_static/image45.png)](implementing-optimistic-concurrency-vb/_static/image44.png)

**Abbildung 16**: dem ObjectDataSource-Steuerelement wurde eine Methode mit der Eingabeparameter, die sie senden möchte nicht gefunden ([klicken Sie, um das Bild in voller Größe anzeigen](implementing-optimistic-concurrency-vb/_static/image46.png))


Betrachten die Meldung der Ausnahme aus, es ist klar, dass dem ObjectDataSource-Steuerelement, eine Geschäftslogikschicht aufrufen möchte `DeleteProduct` Methode, die enthält `original_CategoryName` und `original_SupplierName` Eingabeparameter. Grund hierfür ist die `ItemTemplate` s für die `CategoryID` und `SupplierID` von TemplateFields enthalten derzeit die bidirektionale Bindung-Anweisungen mit der `CategoryName` und `SupplierName` Datenfelder. Stattdessen müssen wir gehören `Bind` -Anweisungen mit der `CategoryID` und `SupplierID` Datenfelder. Um dies zu erreichen, ersetzen Sie die vorhandenen Bindung-Anweisungen mit `Eval` -Anweisungen, und fügen Sie dann auf ausgeblendete Label-Steuerelement, dessen `Text` Eigenschaften gebunden sind, um die `CategoryID` und `SupplierID` Datenfelder, die bidirektionale Datenbindung, Verwendung, wie unten:


[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample11.aspx)]

Mit diesen Änderungen können wir jetzt erfolgreich löschen und Bearbeiten von Produktinformationen. In Schritt 5 betrachten wir, wie Sie überprüfen, ob nebenläufigkeitsverstöße erkannt wird, werden wird. Aber nehmen Sie jetzt ein paar Minuten für Versuch aktualisieren und löschen einige Datensätze aus, um sicherzustellen, dass die Aktualisierung und Löschen für ein einzelner Benutzer funktioniert wie erwartet.

## <a name="step-5-testing-the-optimistic-concurrency-support"></a>Schritt 5: Testen der vollständigen Parallelität-Unterstützung

Um sicherzustellen, dass parallelitätsverletzungen erkannte (statt sich ergebenden Daten Blind überschrieben) werden, müssen wir zwei Browserfenstern zu dieser Seite zu öffnen. Klicken Sie in beiden Browserinstanzen auf die Schaltfläche "Bearbeiten" aus, um Chai entspricht. Klicken Sie dann in nur einem der Browser, ändern Sie den Namen "Chai Tee", und klicken Sie auf aktualisieren. Das Update soll erfolgreich ausgeführt werden und die GridView in den Zustand vor der Bearbeitung mit "Chai Tee" als der Name des neuen Produkts.

In der anderen Fenster Browserinstanz zeigt jedoch der Name des Produkts Textfeld immer noch "Chai". In diesem zweiten Browserfenster aktualisieren die `UnitPrice` zu `25.00`. Ohne Unterstützung von optimistischer Parallelität würden Update in der zweiten Browserinstanz auf den Namen des Produkts zurück in "Chai", und die Änderungen durch die erste Instanz eines Webbrowsers überschrieb ändern. Mit vollständiger Parallelität, die verwendet werden, klicken Sie auf die Schaltfläche "Aktualisieren" in der zweiten Browserinstanz führt jedoch zu einem [DBConcurrencyException](https://msdn.microsoft.com/library/system.data.dbconcurrencyexception.aspx).


[![Wenn eine Parallelitätsverletzung erkannt wird, wird eine DBConcurrencyException ausgelöst.](implementing-optimistic-concurrency-vb/_static/image48.png)](implementing-optimistic-concurrency-vb/_static/image47.png)

**Abbildung 17**: Wenn eine Parallelitätsverletzung erkannt wird, eine `DBConcurrencyException` ausgelöst ([klicken Sie, um das Bild in voller Größe anzeigen](implementing-optimistic-concurrency-vb/_static/image49.png))


Die `DBConcurrencyException` wird nur ausgelöst werden, wenn die DAL Batch-updatemuster genutzt wird. Das direkte DB-Muster wird keine Ausnahme ausgelöst werden, lediglich bedeutet dies, dass keine Zeilen betroffen sind. Um dies zu veranschaulichen, werden beide Browserinstanzen GridView an ihren vorab Bearbeitungszustand zurück. Als Nächstes in der ersten Browserinstanz, klicken Sie auf die Schaltfläche "Bearbeiten" und ändern Sie den Produktnamen aus "Chai Tee" an "Chai" und klicken Sie auf aktualisieren. Klicken Sie im zweiten Browserfenster auf die Schaltfläche "löschen" für Chai entspricht.

Beim Klicken auf "löschen" die Seite Daten zurückgegeben, GridView aufruft, das "ObjectDataSource" `Delete()` -Methode und dem ObjectDataSource-Steuerelement ruft nach unten in der `ProductsOptimisticConcurrencyBLL` Klasse `DeleteProduct` -Methode, die ursprünglichen Werte übergeben. Die ursprüngliche `ProductName` Wert für die zweite Browserinstanz ist "Chai Tee", die mit dem aktuellen entspricht `ProductName` Wert in der Datenbank. Aus diesem Grund die `DELETE` -Anweisung mit der Datenbank wirkt sich auf 0 (null) Zeilen aus, da kein Datensatz in der Datenbank vorhanden ist, die die `WHERE` Klausel erfüllen. Die `DeleteProduct` Methodenrückgabe `false` und Daten mit dem ObjectDataSource-Steuerelement werden erneut an die GridView gebunden.

Aus Sicht des Endbenutzers blinkt den Bildschirm, auf die Schaltfläche "löschen" für Tee Chai entspricht, in der zweiten Browserfenster verursacht werden und nach wieder, das Produkt ist noch vorhanden, obwohl es als "Chai" (die Änderung des Produktnamens, die von der ersten Browser jetzt aufgeführt ist die Instanz). Wenn der Benutzer erneut die Schaltfläche "löschen" klickt, der Löschvorgang wird ausgeführt werden, wie GridView ursprünglichen ist `ProductName` Wert ("Chai") Sie jetzt mit dem Wert in der Datenbank entspricht.

In beiden Fällen ist die Benutzeroberfläche alles andere als Ideal. Wir natürlich nicht dem Benutzer die Einzelheiten anzeigen möchten die `DBConcurrencyException` Ausnahme aus, wenn der Batch-Update-Muster verwenden. Und das Verhalten bei Verwendung des direkten DB-Musters ist etwas verwirrend, wie die Benutzer-Befehl konnte nicht ausgeführt. es wurde jedoch keine genaue Angabe der Grund.

Um diese beiden Probleme zu beheben, können wir die Bezeichnung Websteuerelemente auf der Seite, die eine Erklärung, warum ein Update- oder Delete-Fehler beim Bereitstellen erstellen. Für das Batch-Update-Muster können wir bestimmen, ob eine `DBConcurrencyException` -Ausnahme in den GridView auf beitragsebene-Ereignishandler die Bezeichnung für die Warnung anzeigen, je nach Bedarf. Für die direkte Methode DB untersuchen wir den Rückgabewert der BLL-Methode (d.h. `true` Wenn eine Zeile geändert wurde, `false` andernfalls) und eine informationsmeldung angezeigt, je nach Bedarf.

## <a name="step-6-adding-informational-messages-and-displaying-them-in-the-face-of-a-concurrency-violation"></a>Schritt 6: Hinzufügen von Informationsmeldungen, und sie bei der eine Parallelitätsverletzung anzuzeigen

Tritt eine parallelitätsverletzung, hängt vom Verhalten ab, ob die DAL Batchaktualisierung oder DB-direct-Muster verwendet wurde. Unser Tutorial verwendet beide Muster, mit dem Batch-Update-Muster verwendet wird, für das Aktualisieren und das direkte DB-Muster dient zum Löschen. Zunächst fügen Sie zwei Label-Websteuerelemente auf unserer Seite, die erklären, dass eine parallelitätsverletzung Fehler beim Löschen oder Aktualisieren von Daten. Legen Sie des Label-Steuerelements `Visible` und `EnableViewState` Eigenschaften `false`; Dies bewirkt, dass sie auf jedem Besuch der Seite ausgeblendet werden soll, mit der Ausnahme für diese bestimmte Seite Where besucht die `Visible` -Eigenschaftensatz programmgesteuert auf `true`.


[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample12.aspx)]

Zusätzlich zum Festlegen ihrer `Visible`, `EnabledViewState`, und `Text` Eigenschaften, habe ich auch festgelegt die `CssClass` Eigenschaft `Warning`, der bewirkt, dass der Bezeichnungsfelds des in einer Schriftart für große, Rot, kursiv, fett angezeigt werden. Dieses CSS `Warning` Klasse definiert und Styles.css hinzugefügt wurde in der *Untersuchen der Ereignisse zugeordnet einfügen, aktualisieren und löschen* Tutorial.

Nach dem Hinzufügen dieser Bezeichnungen, sollte der Designer in Visual Studio Abbildung 18 ähneln.


[![Zwei Label-Steuerelemente wurden auf der Seite hinzugefügt](implementing-optimistic-concurrency-vb/_static/image51.png)](implementing-optimistic-concurrency-vb/_static/image50.png)

**Abbildung 18**: zwei Label-Steuerelemente hinzugefügt wurden die Seite ([klicken Sie, um das Bild in voller Größe anzeigen](implementing-optimistic-concurrency-vb/_static/image52.png))


Mit diesen Label-Web-Steuerelementen vorhanden, wir möchten Sie erfahren Sie, wie ein, um zu bestimmen, wenn eine parallelitätsverletzung aufgetreten ist, zeigen Sie an dem der entsprechenden Bezeichnung `Visible` Eigenschaft kann festgelegt werden, um `true`, die Meldung dient zu Informationszwecken angezeigt.

## <a name="handling-concurrency-violations-when-updating"></a>Behandeln von Parallelität, bei der Aktualisierung

Sehen wir uns zunächst an, wie Parallelität behandelt wird, wenn der Batch-Update-Muster verwenden. Da solche Verstöße zu erkennen mit dem Muster Ursache aktualisieren eine `DBConcurrencyException` Ausnahme ausgelöst wird, müssen wir unsere ASP.NET-Seite, um zu bestimmen, Code hinzufügen, ob eine `DBConcurrencyException` Ausnahme, die während des Updates aufgetreten ist. Wenn also, wir eine Nachricht an den Benutzer erläutert angezeigt werden sollen, die ihre Änderungen nicht gespeichert wurden, weil ein anderer Benutzer die gleichen Daten zwischen geändert haben, als sie begannen, den Datensatz zu bearbeiten, und wenn sie die Schaltfläche "Aktualisieren" geklickt.

Wie wir, in gesehen der *behandeln BLL- und DAL-Ebene von Ausnahmen in einer ASP.NET-Seite* Tutorial, solche Ausnahmen erkannt und in der Web-Steuerelements auf beitragsebene Ereignishandler unterdrückt werden können. Aus diesem Grund müssen wir erstellen einen Ereignishandler für der GridView `RowUpdated` -Ereignis, das überprüft, wenn eine `DBConcurrencyException` Ausnahme wurde ausgelöst. Dieser Ereignishandler wird einen Verweis auf jede Ausnahme, die während des Aktualisierungsvorgangs, ausgelöst wurde übergeben, wie dargestellt, im Ereignisprotokoll Ereignishandler folgenden code:


[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample13.vb)]

Im face von einem `DBConcurrencyException` Ausnahme dieser Ereignishandler zeigt die `UpdateConflictMessage` Beschriftungs-Steuerelement und gibt an, dass die Ausnahme verarbeitet wurde. Mit diesem Code werden tritt eine parallelitätsverletzung beim Aktualisieren eines Datensatzes, sind die Änderungen des Benutzers verloren geht, da sie einen anderen Benutzer Änderungen zur gleichen Zeit überschrieben haben, würden. Insbesondere wird die GridView zurückgegeben, die in den Zustand vor der Bearbeitung und an Daten in der aktuellen Datenbank gebunden. Dadurch wird die GridView-Zeile mit Änderungen des anderen Benutzers aktualisiert, die zuvor nicht sichtbar waren. Darüber hinaus die `UpdateConflictMessage` Label-Steuerelement wird erläutert, für den Benutzer was gerade geschehen ist. Diese Abfolge von Ereignissen wird in Abbildung 19 beschrieben.


[![Ein Benutzer s werden Updates in das Gesicht von eine Parallelitätsverletzung verloren gehen.](implementing-optimistic-concurrency-vb/_static/image54.png)](implementing-optimistic-concurrency-vb/_static/image53.png)

**Abbildung 19**: Benutzer s Updates sind verlorene Bestätigungen in das Gesicht von eine Parallelitätsverletzung ([klicken Sie, um das Bild in voller Größe anzeigen](implementing-optimistic-concurrency-vb/_static/image55.png))


> [!NOTE]
> Alternativ statt die GridView an den vorab Bearbeitungszustand zurückgegeben wird, wir werden u. u. GridView im Bearbeitungsbereich Zustand durch Festlegen der `KeepInEditMode` Eigenschaft des übergebenen `GridViewUpdatedEventArgs` Objekt auf "true". Wenn Sie diesen Ansatz verwenden, allerdings werden sicher, dass die Daten an die GridView zu binden (durch Aufrufen der `DataBind()` Methode), damit die anderen Werte des Benutzers in die Bearbeitungsschnittstelle geladen werden. Der Code zum Herunterladen dieses Lernprogramms zur Verfügung hat, diese beiden Codezeilen in der `RowUpdated` Ereignishandler auskommentiert, einfach kommentieren Sie diese Codezeilen, um die GridView zu erhalten, die nach der eine parallelitätsverletzung im Bearbeitungsmodus belassen.


## <a name="responding-to-concurrency-violations-when-deleting"></a>Reagieren auf Parallelitätsverletzungen, beim Löschen

Durch das direkte DB-Muster müssen Sie keine Ausnahme ausgelöst, bei dem eine parallelitätsverletzung vorhanden ist. Stattdessen wirkt sich auf die Database-Anweisung einfach keine Datensätze, wie die WHERE-Klausel mit jedem Datensatz nicht übereinstimmt. Alle Methoden Änderung Daten in die BLL erstellt wurden konzipiert, dass sie einen booleschen Wert fest, und zwar unabhängig davon, ob wirkt sich auf genau ein Datensatz zurückgeben. Untersuchen wir daher, um zu bestimmen, ob eine parallelitätsverletzung aufgetreten ist, wenn Sie einen Datensatz zu löschen, den Rückgabewert von der BLL des `DeleteProduct` Methode.

Der Rückgabewert für eine Methode für die BLL untersucht werden kann, in dem ObjectDataSource-Steuerelement auf beitragsebene-Ereignishandler durch den `ReturnValue` Eigenschaft der `ObjectDataSourceStatusEventArgs` Objekt in den Ereignishandler übergeben. Da wir den Rückgabewert von ermitteln möchte, sind die `DeleteProduct` -Methode muss zum Erstellen eines ereignishandlers für das "ObjectDataSource" `Deleted` Ereignis. Die `ReturnValue` Eigenschaft ist vom Typ `object` möglich `null` Wenn eine Ausnahme ausgelöst wurde, und die Methode wurde unterbrochen, bevor sie einen Wert zurückgeben kann. Daher wir müssen zunächst sicherstellen, dass die `ReturnValue` Eigenschaft ist nicht `null` und ist ein boolescher Wert. Vorausgesetzt, diese Überprüfung erfolgreich war, wird die `DeleteConflictMessage` Beschriftungs-Steuerelement Wenn die `ReturnValue` ist `false`. Dies kann erreicht werden, mit dem folgenden Code:


[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample14.vb)]

Bei einer Verletzung der Parallelität wird die Delete-Anforderung des Benutzers abgebrochen. Das GridView wird angezeigt, dass die vorgenommenen Änderungen für den entsprechenden Datensatz in der die Zeit zwischen den Benutzer geladen wird, auf der Seite, und wenn er auf die Schaltfläche "löschen" geklickt, aktualisiert. Wenn Sie ein Verstoß gegen herausstellt, die `DeleteConflictMessage` Bezeichnung angezeigt wird, wird erläutert, was gerade geschehen ist (siehe Abbildung 20).


[![Ein Benutzer s löschen, wird bei eine Parallelitätsverletzung abgebrochen.](implementing-optimistic-concurrency-vb/_static/image57.png)](implementing-optimistic-concurrency-vb/_static/image56.png)

**Abbildung 20**: Benutzer s löschen, bei dem eine Parallelitätsverletzung abgebrochen wird ([klicken Sie, um das Bild in voller Größe anzeigen](implementing-optimistic-concurrency-vb/_static/image58.png))


## <a name="summary"></a>Zusammenfassung

Chancen auf parallelitätsverletzungen vorhanden sind, in jeder Anwendung, die mehrere ermöglicht gleichzeitiger Benutzer zu aktualisieren oder Löschen von Daten. Wenn solche Verstöße zu erkennen für, wenn zwei Benutzer gleichzeitig dieselben Daten Personen in den letzten Schreibvorgang "gewinnt" erhält aktualisieren, des anderen Benutzers überschreiben Änderungen Änderungen nicht berücksichtigt werden. Alternativ dazu können Entwickler entweder Steuerelement optimistische oder pessimistische Parallelität implementieren. Steuerung für optimistische Parallelität wird davon ausgegangen, dass parallelitätsverletzungen selten sind und einfach lässt nicht zu einem Update oder delete-Befehl, die eine parallelitätsverletzung darstellen würde. Steuerung durch eingeschränkte Parallelität wird davon ausgegangen, dass die Parallelität Verstöße finden häufig statt und update oder delete-Befehl einfach Ablehnen eines Benutzers nicht zulässig ist. Mit der Steuerung durch eingeschränkte Parallelität umfasst das Aktualisieren eines Datensatzes, Sperren und verhindern, dass alle anderen Benutzer ändern oder löschen den Datensatz, während er gesperrt ist.

Das typisierte DataSet in .NET stellt Funktionen für die Unterstützung der Steuerung durch vollständige Parallelität bereit. Insbesondere die `UPDATE` und `DELETE` -Anweisungen in der Datenbank enthalten alle Spalten der Tabelle, die gewährleisteten, dass die Update- oder Delete nur auftritt, wenn die aktuellen Daten des Datensatzes mit den ursprünglichen Daten entspricht, den Benutzer hatte, wenn Ausführen von ihren Update- oder Delete. Sobald die DAL zum unterstützen der optimistischen Parallelität konfiguriert wurde, müssen die BLL Methoden aktualisiert werden. Darüber hinaus muss die ASP.NET-Seite, die Sie die BLL aufruft werden so konfiguriert, dass die ObjectDataSource ruft die ursprünglichen Werte aus dem Websteuerelement ab und sie entlang in die BLL übergibt.

Wie in diesem Tutorial erläutert, umfasst die Implementierung von Steuerung für optimistische Parallelität in einer ASP.NET-Webanwendung Aktualisieren der DAL und der Geschäftslogikschicht und Hinzufügen von Unterstützung auf der ASP.NET-Seite. Unabhängig davon, ob diese zusätzliche Arbeit eine kluge Investition von Zeit und Aufwand ist, hängt von Ihrer Anwendung ab. Wenn Sie nur selten gleichzeitigen Benutzern haben, Daten aktualisieren oder die Daten, die sie aktualisieren unterscheidet sich von den anderen, ist parallelitätssteuerung kein wichtiger Aspekt. Wenn Sie jedoch, Sie routinemäßig mehrere Benutzer auf Ihrer Website mit denselben Daten arbeiten haben, können parallelitätssteuerung verhindern, dass ein Benutzer die Update- oder Löschvorgänge unwissentlich Überschreiben einer anderen Person.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Zurück](customizing-the-data-modification-interface-vb.md)
> [Weiter](adding-client-side-confirmation-when-deleting-vb.md)
