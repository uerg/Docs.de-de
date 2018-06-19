---
uid: web-forms/overview/data-access/working-with-batched-data/batch-deleting-cs
title: Batch löschen (c#) | Microsoft Docs
author: rick-anderson
description: Erfahren Sie, wie mehrere Datenbankdatensätze in einem einzigen Vorgang zu löschen. In der Benutzeroberflächenebene bauen es auf eine verbesserte GridView erstellt in einem früheren Zeitpunkt tut...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: ac6916d0-a5ab-4218-9760-7ba9e72d258c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-deleting-cs
msc.type: authoredcontent
ms.openlocfilehash: 59c90dcf373d19aad42250ee6dedba5f09f833b5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30891866"
---
<a name="batch-deleting-c"></a>Batch löschen (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Herunterladen von Code](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_65_CS.zip) oder [PDF herunterladen](batch-deleting-cs/_static/datatutorial65cs1.pdf)

> Erfahren Sie, wie mehrere Datenbankdatensätze in einem einzigen Vorgang zu löschen. In der Benutzeroberflächenebene bauen es auf eine verbesserte GridView, die in einer früheren Lernprogramm erstellt haben. In der Datenzugriffsebene umschließen wir mehrere Delete-Operationen innerhalb einer Transaktion, um sicherzustellen, dass alle Löschvorgänge erfolgreich ausgeführt werden, oder alle Löschvorgänge werden zurückgesetzt.


## <a name="introduction"></a>Einführung

Die [vorherigen Lernprogramm](batch-updating-cs.md) untersucht wie eine Schnittstelle mit vollständig bearbeitbar GridView-Batchverarbeitung erstellt. In Situationen, in denen Benutzer häufig viele Datensätze gleichzeitig bearbeiten, erfordert eine Schnittstelle für die Batchverarbeitung wesentlich weniger Postbacks und Tastatur, Maus Kontext Switches, und somit die Endbenutzer-s-Effizienz zu verbessern. Dieses Verfahren eignet sich entsprechend für Seiten, wo es üblich, dass Benutzer zum Löschen von Datensätzen in einer Aktion ist.

Jeder, der einen online-e-Mail-Client verwendet hat bereits mit einem der am häufigsten verwendeten Batch löschen Schnittstellen vertraut ist: ein Kontrollkästchen in jeder Zeile in einem Raster mit einer entsprechenden alle markierte Elemente löschen Schaltfläche (siehe Abbildung 1). Dieses Lernprogramm ist vielmehr kurz da wir alle die schwierige Aufgabe im vorherigen Lernprogrammen zum Erstellen der Web-basierte Schnittstelle und eine Methode zum Löschen einer Reihe von Datensätzen als einzelner atomarischer Vorgang bereits getan haben. In der [Hinzufügen einer GridView-Spalte der Kontrollkästchen](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs.md) Lernprogramm GridView mit einer Spalte der Kontrollkästchen und in erstellten der [Umbruch Datenbankänderungen innerhalb einer Transaktion](wrapping-database-modifications-within-a-transaction-cs.md) wir eine Methode im erstellt Lernprogramm die BLL, die eine Transaktion, zum Löschen verwenden würden einer `List<T>` von `ProductID` Werte. In diesem Lernprogramm wird aufgebaut und merge unserer vorherigen Möglichkeiten für das Erstellen eines funktionierenden Batches Beispiel löschen.


[![Jede Zeile enthält ein Kontrollkästchen](batch-deleting-cs/_static/image1.gif)](batch-deleting-cs/_static/image1.png)

**Abbildung 1**: jede Zeile enthält ein Kontrollkästchen ([klicken Sie hier, um das Bild in voller Größe angezeigt](batch-deleting-cs/_static/image2.png))


## <a name="step-1-creating-the-batch-deleting-interface"></a>Schritt 1: Erstellen des Batches löschen Schnittstelle

Da wir bereits löschen Schnittstelle im Batch erstellt die [Hinzufügen einer GridView-Spalte der Kontrollkästchen](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs.md) Lernprogramm, wir können einfach kopiert `BatchDelete.aspx` anstelle von Grund auf neu erstellen. Öffnen Sie zunächst die `BatchDelete.aspx` auf der Seite der `BatchData` Ordner und die `CheckBoxField.aspx` auf der Seite der `EnhancedGridView` Ordner. Aus der `CheckBoxField.aspx` Seite, wechseln Sie zur Quellansicht aus, und kopieren Sie das Markup zwischen den `<asp:Content>` tags wie in Abbildung 2 dargestellt.


[![Kopieren Sie deklarative Markup CheckBoxField.aspx in die Zwischenablage](batch-deleting-cs/_static/image2.gif)](batch-deleting-cs/_static/image3.png)

**Abbildung 2**: Kopieren von deklarativen Markup `CheckBoxField.aspx` in die Zwischenablage ([klicken Sie hier, um das Bild in voller Größe angezeigt](batch-deleting-cs/_static/image4.png))


Als Nächstes wechseln Sie zur Quellansicht in `BatchDelete.aspx` und fügen Sie den Inhalt der Zwischenablage in die `<asp:Content>` Tags. Auch kopieren und fügen Sie den Code aus, in der CodeBehind-Klasse in `CheckBoxField.aspx.cs` , innerhalb der Code-Behind-Klasse in `BatchDelete.aspx.cs` (die `DeleteSelectedProducts` Schaltfläche s `Click` Ereignishandler, d. h. die `ToggleCheckState` -Methode, und die `Click` -Ereignishandler für die `CheckAll` und `UncheckAll` Schaltflächen). Nach dem Kopieren über diesen Inhalt der `BatchDelete.aspx` Seite "s" Code-Behind-Klasse sollte den folgenden Code enthalten:


[!code-csharp[Main](batch-deleting-cs/samples/sample1.cs)]

Nehmen Sie nach dem Kopieren über den Quellcode und deklaratives Markup, einen Moment Zeit zum Testen `BatchDelete.aspx` durch ihn über einen Browser anzuzeigen. Daraufhin sollte eine GridView, die die ersten zehn Produkte in einer GridView mit jeder Zeile Auflisten der Produktname s, Kategorie und Preis zusammen mit einem Kontrollkästchen auflisten. Es darf drei Schaltflächen: Überprüfen Sie alle, alle deaktivieren und Löschen ausgewählt Produkte. Klicken auf die Schaltfläche Alle wählt alle Kontrollkästchen, während alle deaktivieren Sie alle Kontrollkästchen deaktiviert. Löschen ausgewählter Produkte auf eine Nachricht mit der Liste zeigt die `ProductID` Werte der ausgewählten Produkte, aber nicht tatsächlich die Produkte gelöscht werden.


[![Die Schnittstelle aus CheckBoxField.aspx wurden an BatchDeleting.aspx verschoben.](batch-deleting-cs/_static/image3.gif)](batch-deleting-cs/_static/image5.png)

**Abbildung 3**: die Schnittstelle aus `CheckBoxField.aspx` ausgelagert wurden `BatchDeleting.aspx` ([klicken Sie hier, um das Bild in voller Größe angezeigt](batch-deleting-cs/_static/image6.png))


## <a name="step-2-deleting-the-checked-products-using-transactions"></a>Schritt 2: Löschen die aktivierte Produkte, die Verwendung von Transaktionen

Mit dem Batch löschen erfolgreich zu kopiert Schnittstelle `BatchDeleting.aspx`, dass alle sich bleibt so aktualisieren den Code, damit die Schaltfläche mit den ausgewählten Produkten löschen die aktivierte Produkte mit löscht die `DeleteProductsWithTransaction` Methode in der `ProductsBLL` Klasse. Dieser Methode hinzugefügt, die der [Umbruch Datenbankänderungen innerhalb einer Transaktion](wrapping-database-modifications-within-a-transaction-cs.md) Lernprogramm akzeptiert als Eingabe eine `List<T>` von `ProductID` Werten und löschungen jeweils entsprechenden `ProductID` innerhalb des Bereichs ein die Transaktion.

Die `DeleteSelectedProducts` Schaltfläche s `Click` -Ereignishandler verwendet derzeit die folgenden `foreach` Schleife zum Durchlaufen der einzelnen GridView-Zeile:


[!code-csharp[Main](batch-deleting-cs/samples/sample2.cs)]

Für jede Zeile der `ProductSelector` CheckBox-Websteuerelement programmgesteuert verwiesen wird. Wenn diese Option aktiviert ist, die Zeile s `ProductID` entnommen wird die `DataKeys` Auflistung und die `DeleteResults` Bezeichnung s `Text` Eigenschaft so aktualisiert wird, eine Meldung gibt an, dass die Zeile zum Löschen ausgewählt wurde.

Der obige Code tatsächlich löscht keine Datensätze wie der Aufruf der `ProductsBLL` Klasse s `Delete` Methode auskommentiert ist. Wurden diese Delete-Logik angewendet werden, würde der Code die Produkte gelöscht, aber nicht in einer atomaren Operation. D. h. wenn die ersten Paar Löschvorgänge in der Sequenz war erfolgreich, aber eine höhere Version ist (möglicherweise wegen eines foreign Key-Einschränkung fehlgeschlagen), würde eine Ausnahme ausgelöst werden dieser Produkte, die bereits gelöscht würden verbleiben jedoch gelöscht.

Um die Unteilbarkeit zu gewährleisten, müssen wir stattdessen die `ProductsBLL` Klasse s `DeleteProductsWithTransaction` Methode. Da diese Methode akzeptiert, die eine Liste der `ProductID` Werte, müssen wir zuerst kompilieren diese Liste, aus dem Raster und klicken Sie dann als Parameter zu übergeben. Wir zuerst erstellen Sie eine Instanz von einem `List<T>` vom Typ `int`. Innerhalb der `foreach` Schleife wir fügen die ausgewählten Produkte müssen `ProductID` Werte dieser `List<T>`. Nach der Schleife dies `List<T>` übergeben werden muss, um die `ProductsBLL` Klasse s `DeleteProductsWithTransaction` Methode. Update der `DeleteSelectedProducts` Schaltfläche s `Click` -Ereignishandler durch den folgenden Code:


[!code-csharp[Main](batch-deleting-cs/samples/sample3.cs)]

Der aktualisierte Code erstellt ein `List<T>` des Typs `int` (`productIDsToDelete`) und füllt sie mit der `ProductID` zu löschende Werte. Nach der `foreach` Schleife, wenn mindestens ein Produkt ausgewählt haben, ist die `ProductsBLL` Klasse s `DeleteProductsWithTransaction` Methode wird aufgerufen, und übergeben Sie dieser Liste. Die `DeleteResults` Bezeichnung wird auch angezeigt, und die Daten erneut an die GridView gebunden wird (sodass die neu gelöschte Datensätze als Zeilen im Raster nicht mehr angezeigt werden).

Abbildung 4 zeigt die GridView, nachdem eine Anzahl von Zeilen zum Löschen ausgewählt wurden. Abbildung 5 zeigt den Bildschirm sofort, nachdem Sie die Schaltfläche "Löschen ausgewählten Produkten" geklickt wurde. Beachten Sie, dass in Abbildung 5 die `ProductID` der gelöschten Datensätze in die Bezeichnung unter GridView angezeigt werden und diese Zeilen werden nicht mehr in der GridView.


[![Die ausgewählten Produkte werden gelöscht.](batch-deleting-cs/_static/image4.gif)](batch-deleting-cs/_static/image7.png)

**Abbildung 4**: die ausgewählten Produkte gelöscht werden ([klicken Sie hier, um das Bild in voller Größe angezeigt](batch-deleting-cs/_static/image8.png))


[![Gelöschte Werte Produkte "ProductID" unterhalb der GridView aufgeführt](batch-deleting-cs/_static/image5.gif)](batch-deleting-cs/_static/image9.png)

**Abbildung 5**: der gelöschte Produkte `ProductID` Werte sind unter der GridView aufgeführt ([klicken Sie hier, um das Bild in voller Größe angezeigt](batch-deleting-cs/_static/image10.png))


> [!NOTE]
> So testen Sie die `DeleteProductsWithTransaction` Methode s Unteilbarkeit, fügen Sie manuell einen Eintrag für ein Produkt in der `Order Details` Tabelle und dann versuchen, das Produkt (zusammen mit anderen) zu löschen. Beim Versuch, das Produkt mit einer zugeordneten Auftrag löschen, aber beachten, wie die Löschung der ausgewählten Produkte anderer Rollback ausgeführt werden, erhalten Sie eine Verletzung der foreign Key-Einschränkung.


## <a name="summary"></a>Zusammenfassung

Erstellen einen Batch löschen Schnittstelle umfasst das Hinzufügen einer GridView mit einer Spalte der Kontrollkästchen, und eine Schaltfläche-Websteuerelement, wenn geklickt haben, werden alle ausgewählten Zeilen als einzelner atomarischer Vorgang gelöscht. In diesem Lernprogramm erstellt es eine solche Schnittstelle Zusammenführung überlappender Arbeit, die in beiden vorherigen Lernprogrammen [Hinzufügen einer GridView-Spalte der Kontrollkästchen](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs.md) und [Umbruch Datenbankänderungen innerhalb einer Transaktion](wrapping-database-modifications-within-a-transaction-cs.md). Im ersten Lernprogramm erstellt es eine GridView mit einer Spalte mit Kontrollkästchen und im letzten Fall wir eine Methode in der BLL implementiert, die beim Übergeben einer `List<T>` von `ProductID` Werte innerhalb des Bereichs einer Transaktion gelöscht.

In den nächsten Lernprogrammen erstellen wir eine Schnittstelle für die Batchverarbeitung von Einfügevorgängen ausführen.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurden Hilton Giesenow und Teresa Murphy. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](batch-updating-cs.md)
> [Weiter](batch-inserting-cs.md)
