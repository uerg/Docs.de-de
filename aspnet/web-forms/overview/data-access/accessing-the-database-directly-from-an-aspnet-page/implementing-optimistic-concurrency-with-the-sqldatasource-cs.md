---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/implementing-optimistic-concurrency-with-the-sqldatasource-cs
title: "Implementieren der vollständigen Parallelität mit SqlDataSource (c#) | Microsoft Docs"
author: rick-anderson
description: "In diesem Lernprogramm werden die Grundlagen der Steuerung durch vollständige Parallelität überprüfen und sehen Sie sich dann wie sie mithilfe der SqlDataSource-Steuerelement implementiert."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2007
ms.topic: article
ms.assetid: df999966-ac48-460e-b82b-4877a57d6ab9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/implementing-optimistic-concurrency-with-the-sqldatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: b089a0b25aa5a520f3e20af8ec5212072ad7c7bf
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="implementing-optimistic-concurrency-with-the-sqldatasource-c"></a>Implementieren der vollständigen Parallelität mit SqlDataSource (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunterladen](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_50_CS.exe) oder [PDF herunterladen](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/datatutorial50cs1.pdf)

> In diesem Lernprogramm werden die Grundlagen der Steuerung durch vollständige Parallelität überprüfen und sehen Sie sich dann wie sie mithilfe der SqlDataSource-Steuerelement implementiert.


## <a name="introduction"></a>Einführung

In dem vorherigen Lernprogramm untersucht wir zum Einfügen, aktualisieren und Löschen von Funktionen, um die SqlDataSource-Steuerelement hinzufügen. Kurz gesagt, die diese Features bieten wir zum erforderlich sind. Geben Sie den entsprechenden `INSERT`, `UPDATE`, oder `DELETE` SQL-Anweisung im Steuerelement s `InsertCommand`, `UpdateCommand`, oder `DeleteCommand` Eigenschaften werden zusammen mit den entsprechenden Parameter in der `InsertParameters`, `UpdateParameters`, und `DeleteParameters` Sammlungen. Während diese Eigenschaften und Auflistungen manuell angegeben werden können, bietet das Konfigurieren von Datenquellen-Assistenten s Schaltfläche "Erweitert" eine generieren `INSERT`, `UPDATE`, und `DELETE` Anweisungen Kontrollkästchen, das automatisch-erstellen von diesen Anweisungen wird basierend auf der `SELECT` Anweisung.

Zusammen mit der Generierung `INSERT`, `UPDATE`, und `DELETE` Anweisungen Kontrollkästchen, das Dialogfeld Erweiterte SQL-Generierungsoptionen umfasst eine Option zum Verwenden einer Verletzung der vollständigen Parallelität (siehe Abbildung 1). Wenn dieses Kontrollkästchen aktiviert, die `WHERE` Klauseln in der automatisch generierten `UPDATE` und `DELETE` -Anweisungen werden geändert, damit das Update nur ausgeführt, oder löschen, wenn die zugrunde liegenden Datenbank Daten erkannt t geändert wurde, seit der Benutzer zuletzt geladen die Daten in das Raster.


![Vollständige Parallelität Unterstützung können Sie hinzufügen, über den erweiterten SQL-Generierungsoptionen (Dialogfeld)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image1.gif)

**Abbildung 1**: vollständige Parallelität Unterstützung können Sie hinzufügen, über den erweiterten SQL-Generierungsoptionen (Dialogfeld)


In der [optimistische Parallelität implementieren](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md) Lernprogramm untersucht die Grundlagen der Steuerung durch vollständige Parallelität und wie sie das ObjectDataSource hinzugefügt. In diesem Lernprogramm wir auf die Grundlagen der Steuerung durch vollständige Parallelität Retuschieren und sehen Sie sich dann wie sie mithilfe der SqlDataSource implementiert.

## <a name="a-recap-of-optimistic-concurrency"></a>Eine Zusammenfassung der vollständigen Parallelität

Für Webanwendungen, die ermöglichen, mehrere, gleichzeitige Benutzer zum Bearbeiten oder löschen die gleichen Daten vorhanden sind, die Möglichkeit, dass ein Benutzer kann einen anderen s Änderungen versehentlich überschreiben. In der [optimistische Parallelität implementieren](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md) Lernprogramm ich im folgende Beispiel bereitgestellt:

Stellen Sie sich vor, dass Benutzern Jisun und Sam, sowohl eine Seite in einer Anwendung, für die zulässige Besucher besuchen wurden zu aktualisieren und Löschen von Produkten über ein GridView-Steuerelement. Beide klicken Sie auf die Schaltfläche "Bearbeiten" für Chai ungefähr zur selben Zeit. Jisun ändert sich der Name des Produkts in Chai Tee und klickt auf die Schaltfläche "Aktualisieren". Das Ergebnis ist ein `UPDATE` -Anweisung, die an die Datenbank gesendet wird, die festlegt *alle* der aktualisierbaren Felder Product s (obwohl Jisun nur für ein Feld aktualisiert `ProductName`). Zu diesem Zeitpunkt benötigt die Datenbank die Werte Chai Tee, der Kategorie Getränke, die Lieferanten Exotic Liquids für dieses bestimmte Produkt und so weiter. Allerdings zeigt die GridView Sam-s-Bildschirm als Chai weiterhin den Produktnamen in der bearbeitbaren GridView-Zeile. Einige Sekunden nach Jisun s Änderungen ein Commit ausgeführt wurde, wurden Sam aktualisiert die Kategorie "Gewürze" und klickt auf Update. Dies führt zu einer `UPDATE` Anweisung gesendet, um die Datenbank, die den Namen des Produkts auf Chai, setzt die `CategoryID` auf die entsprechende Kategorie-ID "Gewürze" usw. Jisun s Änderungen an den Namen des Produkts wurde überschrieben.

Abbildung 2 zeigt diese Interaktion.


[![Wenn zwei Benutzer gleichzeitig einen Datensatz zu aktualisieren ändert es s Potenzial für einen Benutzer zum Überschreiben von anderen](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image2.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image1.png)

**Abbildung 2**: Wenn zwei Benutzer gleichzeitig Update ein Datensatz vorhanden s Potenzial für einen Benutzer geändert wird, überschreiben die anderen s ([klicken Sie hier, um das Bild in voller Größe angezeigt](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image2.png))


Um zu verhindern, dass dieses Szenario Herausklappen, eine Form der [parallelitätssteuerung](http://en.wikipedia.org/wiki/Concurrency_control) muss implementiert werden. [Vollständige Parallelität](http://en.wikipedia.org/wiki/Optimistic_concurrency_control) der Schwerpunkt dieses Lernprogramm funktioniert unter der Annahme, dass es möglicherweise Parallelitätskonflikte und dann vorhanden sein, der Großteil der Zeit, die solche Konflikte/t gewonnen auftreten. Daher, wenn ein Konflikt informiert Steuerung durch vollständige Parallelität einfach dem Benutzer, dass ihre Änderungen kann nicht gespeichert werden, weil ein anderer Benutzer die gleichen Daten geändert hat.

> [!NOTE]
> Für Anwendungen, in denen davon ausgegangen wird, dass sich viele Parallelitätskonflikte oder Konflikte dieser Art nicht tolerierbar sind, kann dann Steuerung durch eingeschränkte Parallelität stattdessen verwendet werden. Verweisen auf die [optimistische Parallelität implementieren](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md) Lernprogramm eine ausführlichere Diskussion zur Steuerung durch eingeschränkte Parallelität.


Steuerung durch vollständige Parallelität funktioniert, indem Sie sicherstellen, dass der Datensatz aktualisiert oder gelöscht werden die gleichen Werte wie beim Aktualisieren oder Löschen von Prozess gestartet. Z. B. beim Klicken auf die Schaltfläche "Bearbeiten", in einem bearbeitbaren GridView "" Datensatzwerte s aus der Datenbank gelesen und in die Textfelder und anderen Websteuerelementen angezeigt. Diese ursprünglichen Werte werden durch die GridView gespeichert. Wenn der Benutzer mit der ihre Änderungen und klickt auf die Schaltfläche "Aktualisieren" später die `UPDATE` verwendete Anweisung muss berücksichtigen Sie die ursprünglichen Werte sowie die neuen Werte und nur den zugrunde liegenden Datenbank-Datensatz aktualisieren, wenn die ursprünglichen Werte, dass der Benutzer bearbeiten gestartet sind identisch mit den Werten noch in der Datenbank. Abbildung 3 zeigt diese Abfolge von Ereignissen.


[![Für die Update- oder Delete erfolgreich ausgeführt werden soll müssen die ursprünglichen Werte der aktuellen Datenbankwerte gleich sein.](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image3.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image3.png)

**Abbildung 3**: für die Update- oder Delete SUCCEED, die ursprünglichen Werte müssen werden gleich der aktuellen Datenbankwerte ([klicken Sie hier, um das Bild in voller Größe angezeigt](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image4.png))


Es gibt verschiedene Ansätze zum Implementieren der vollständigen Parallelität (finden Sie unter [Peter A. Bromberg](http://www.eggheadcafe.com/articles/pbrombergresume.asp) s [Optmistic Parallelität aktualisieren Logik](http://www.eggheadcafe.com/articles/20050719.asp) für einen kurzen Blick auf eine Reihe von Optionen). Durch die SqlDataSource (und durch das ADO.NET typisierter DataSets verwendet, in unserem Datenzugriffsebene) verwendete Technik ergänzt die `WHERE` -Klausel, um einen Vergleich aller von den ursprünglichen Werten enthalten. Die folgenden `UPDATE` -Anweisung aktualisiert z. B. den Namen und den Preis eines Produkts nur, wenn die aktuellen Datenbankwerte die Werte gleich, die ursprünglich abgerufen wurden sind, wenn den Datensatz in die GridView zu aktualisieren. Die `@ProductName` und `@UnitPrice` Parameter enthalten die neuen Werten, die durch den Benutzer eingegeben werden, während `@original_ProductName` und `@original_UnitPrice` enthalten die Werte, die ursprünglich in die GridView geladen wurden, wenn auf die Schaltfläche "Bearbeiten" geklickt wurde:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample1.sql)]

Wie wir in diesem Lernprogramm sehen werden, ist das Aktivieren der Steuerung durch vollständige Parallelität mit der SqlDataSource so einfach wie ein Kontrollkästchen aktivieren.

## <a name="step-1-creating-a-sqldatasource-that-supports-optimistic-concurrency"></a>Schritt 1: Erstellen einer SqlDataSource, unterstützt optimistische Parallelität

Öffnen Sie zunächst die `OptimisticConcurrency.aspx` Seite aus der `SqlDataSource` Ordner. Ziehen Sie ein SqlDataSource-Steuerelement aus der Toolbox in den Designer, Einstellungen der `ID` Eigenschaft `ProductsDataSourceWithOptimisticConcurrency`. Klicken Sie dann auf den Link "Datenquelle konfigurieren" aus dem Steuerelement s smart Tag auf. Wählen Sie aus dem ersten Bildschirm des Assistenten, mit dem `NORTHWINDConnectionString` , und klicken Sie auf Weiter.


[![Wählen Sie für die Arbeit mit der NORTHWINDConnectionString](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image4.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image5.png)

**Abbildung 4**: Wählen Sie für die Arbeit mit der `NORTHWINDConnectionString` ([klicken Sie hier, um das Bild in voller Größe angezeigt](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image6.png))


In diesem Beispiel wir werden hinzufügen eine GridView, die es ermöglicht Benutzern das Bearbeiten der `Products` Tabelle. Aus diesem Grund, aus dem Bildschirm Select-Anweisung konfigurieren, wählen Sie die `Products` Tabelle aus der Dropdown-Liste, und wählen Sie die `ProductID`, `ProductName`, `UnitPrice`, und `Discontinued` Spalten, wie in Abbildung 5 dargestellt.


[![Zurückzugeben Sie aus der Tabelle Products, die ProductID, ProductName, UnitPrice und nicht mehr unterstützte Spalten](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image5.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image7.png)

**Abbildung 5**: aus der `Products` Tabelle, zum Zurückgeben der `ProductID`, `ProductName`, `UnitPrice`, und `Discontinued` Spalten ([klicken Sie hier, um das Bild in voller Größe angezeigt](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image8.png))


Klicken Sie nach dem Auswählen der Spalten, auf die Schaltfläche "Erweitert", um das Dialogfeld Erweiterte SQL-Generierungsoptionen anzuzeigen. Überprüfen Sie die generieren `INSERT`, `UPDATE`, und `DELETE` Anweisungen mithilfe der vollständigen Parallelität Kontrollkästchen, und klicken Sie auf OK (siehe Abbildung 1 für einen Screenshot). Schließen Sie den Assistenten, indem Sie auf Weiter, dann auf Fertig stellen.

Nehmen Sie nach Abschluss des Assistenten für die Datenquelle konfigurieren einen Moment Zeit, untersuchen Sie das resultierende `DeleteCommand` und `UpdateCommand` Eigenschaften und die `DeleteParameters` und `UpdateParameters` Sammlungen. Die einfachste Möglichkeit hierzu ist, klicken Sie auf der Registerkarte "Datenquelle" in der unteren linken Ecke auf der Seite s deklarative Syntax finden Sie unter auf. Es wird Ihnen eine `UpdateCommand` Wert:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample2.sql)]

Mit sieben Parameter in der `UpdateParameters` Auflistung:


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample3.aspx)]

Auf ähnliche Weise die `DeleteCommand` Eigenschaft und `DeleteParameters` Auflistung sollte wie folgt aussehen:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample4.sql)]


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample5.aspx)]

Zusätzlich zum Erweitern der `WHERE` -Klauseln der `UpdateCommand` und `DeleteCommand` Eigenschaften (und die zusätzlichen Parameter für die jeweiligen Parameter Sammlungen hinzufügen), die vollständige Parallelitätsoption passt zwei andere Verwendung auswählen Eigenschaften:

- Ändert die [ `ConflictDetection` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.conflictdetection.aspx) aus `OverwriteChanges` (Standard),`CompareAllValues`
- Ändert die [ `OldValuesParameterFormatString` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.oldvaluesparameterformatstring.aspx) von {0} (Standard) zum ursprünglichen\_{0}.

Wenn die Daten Websteuerelement SqlDataSource s aufruft `Update()` oder `Delete()` -Methode, die ursprünglichen Werte übergibt. Wenn die SqlDataSource s `ConflictDetection` -Eigenschaftensatz auf `CompareAllValues`, diese ursprünglichen Werte der Befehl hinzugefügt werden. Die `OldValuesParameterFormatString` Eigenschaft ermöglicht dem Benennungsschema für diese Werteparameter der ursprüngliche verwendet. Das Konfigurieren von Datenquellen-Assistent verwendet ursprünglichen\_{0} und benennt Sie jeden ursprünglichen Parameter in der `UpdateCommand` und `DeleteCommand` Eigenschaften und `UpdateParameters` und `DeleteParameters` Sammlungen entsprechend.

> [!NOTE]
> Da wir re nicht mithilfe der SqlDataSource-Steuerelement s einfügen-Funktionen, die gerne Entfernen der `InsertCommand` Eigenschaft und die zugehörige `InsertParameters` Auflistung.


## <a name="correctly-handlingnullvalues"></a>Ordnungsgemäßen Behandlung von`NULL`Werte

Leider die augmented `UPDATE` und `DELETE` Anweisungen automatisch vom Assistenten zum Konfigurieren von Datenquellen bei Verwendung der vollständigen Parallelität führen *nicht* funktionieren mit Datensätzen mit `NULL` Werte. Um festzustellen, warum, sollten Sie unsere SqlDataSource s `UpdateCommand`:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample6.sql)]

Die `UnitPrice` Spalte in der `Products` Tabelle möglich `NULL` Werte. Wenn ein bestimmter Datensatz weist ein `NULL` Wert für `UnitPrice`, die `WHERE` Klausel Teil `[UnitPrice] = @original_UnitPrice` wird *immer* auf "false" ausgewertet werden, da `NULL = NULL` gibt immer "false" zurück. Aus diesem Grund Datensätze enthalten, die `NULL` Werte können nicht bearbeitet oder gelöscht wird, als der `UPDATE` und `DELETE` Anweisungen `WHERE` Klauseln/t Return gewonnen Zeilen zum Aktualisieren oder löschen.

> [!NOTE]
> Dieser Fehler wurde im Juni von / / 2004 in zuerst an Microsoft gemeldet [SqlDataSource generiert falsche SQL-Anweisungen](https://connect.microsoft.com/VisualStudio/feedback/ViewFeedback.aspx?FeedbackID=93937) und wird daher voraussichtlich in der nächsten Version von ASP.NET behoben werden.


Um dieses Problem zu beheben, müssen Sie manuell aktualisieren, die `WHERE` in beiden Klauseln der `UpdateCommand` und `DeleteCommand` Eigenschaften für **alle** Spalten mit können `NULL` Werte. Im Allgemeinen ändern `[ColumnName] = @original_ColumnName` an:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample7.sql)]

Diese Änderung können direkt über die deklaratives Markup, über die Optionen UpdateQuery oder DeleteQuery über das Eigenschaftenfenster oder über das UPDATE vorgenommen werden, und löschen Registerkarten angeben, eine benutzerdefinierte SQL-Anweisung oder gespeicherte Prozedur-Option in den Daten konfigurieren Datenquellen-Assistenten. Erneut, diese Änderung muss erfolgen, für *jeder* Spalte in der `UpdateCommand` und `DeleteCommand` s `WHERE` -Klausel, die enthalten kann `NULL` Werte.

Die folgende geänderte führt dies zu unserem Beispiel: Anwenden von `UpdateCommand` und `DeleteCommand` Werte:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample8.sql)]

## <a name="step-2-adding-a-gridview-with-edit-and-delete-options"></a>Schritt 2: Hinzufügen einer GridView mit bearbeiten und Löschoptionen

Mit dem SqlDataSource so konfiguriert, dass die Unterstützung der vollständigen Parallelität übrig bleibt zum Hinzufügen von ein Datentyps-Websteuerelement auf der Seite ", der diese parallelitätssteuerung verwendet. Können Sie für dieses Lernprogramm s hinzufügen eine GridView, die beide bearbeiten und Löschen von Funktionen. Um dies zu erreichen, ziehen Sie eine GridView aus der Toolbox auf die Designer, und legen seine `ID` auf `Products`. Aus den GridView s smart Tag, binden Sie es an die `ProductsDataSourceWithOptimisticConcurrency` SqlDataSource-Steuerelement, die in Schritt 1 hinzugefügt. Überprüfen Sie schließlich die Optionen für die Bearbeitung aktivieren und löschen aktivieren aus den smart Tag.


[![GridView an die SqlDataSource binden und bearbeiten und Löschen von aktivieren](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image6.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image9.png)

**Abbildung 6**: GridView zu binden, SqlDataSource und Bearbeiten aktivieren und löschen ([klicken Sie hier, um das Bild in voller Größe angezeigt](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image10.png))


Konfigurieren Sie nach dem Hinzufügen der GridView, seine Darstellung durch das Entfernen der `ProductID` BoundField, Ändern der `ProductName` BoundField-s `HeaderText` Eigenschaft Produkt, und Aktualisieren der `UnitPrice` BoundField, damit seine `HeaderText` Eigenschaft ist einfach Preis. Im Idealfall wir d verbessern, die Bearbeitungsoberfläche Einbeziehung einen RequiredFieldValidator für die `ProductName` Wert und eine CompareValidator für die `UnitPrice` Wert (s eine ordnungsgemäß formatierte numerische Wert sicherzustellen, dass es). Finden Sie in der [Anpassen der Benutzeroberfläche für die Änderung der Daten](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) Lernprogramm für eine eingehendere Betrachtung der Schnittstelle bearbeiten GridView s anpassen.

> [!NOTE]
> Die GridView, die Ansichtszustand s aktiviert werden muss, da die ursprünglichen Werte, die von der GridView an die SqlDataSource übergeben werden im Ansichtszustand gespeichert.


Nachdem Sie diese Änderungen an die GridView haben, sollte die GridView und SqlDataSource deklarative Markup etwa wie folgt aussehen:


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample9.aspx)]

Um die Steuerung durch vollständige Parallelität in Aktion zu sehen, zwei Browserfenster zu öffnen, und laden die `OptimisticConcurrency.aspx` Seite in beide. Klicken Sie auf die Schaltflächen "Bearbeiten" für das erste Produkt in beiden Browsern. In einem Browser eingeben ändern Sie den Namen des Produkts, und klicken Sie auf aktualisieren. Der Browser wird postback und GridView wird mit dem neuen Produktnamen für den Datensatz, der gerade bearbeitet in seiner vorab Bearbeitungsmodus zurückgesetzt.

Im zweiten Browserfenster ändern Sie den Preis (aber lassen Sie den Produktnamen als den ursprünglichen Wert), und klicken Sie auf aktualisieren. Beim Postback im Raster in die vorab Bearbeitungsmodus gibt, aber die Änderung an der Preis wird nicht aufgezeichnet. Der zweite Browser zeigt dem gleichen Wert wie die erste neue Produktnamen mit dem alte Preis. Die in der zweiten Browserfenster vorgenommenen Änderungen sind verloren gegangen. Darüber hinaus sind die Änderungen verloren gegangen vielmehr – wie gab es keine Ausnahme oder an, dass nur eine parallelitätsverletzung aufgetreten ist.


[![Die Änderungen in der zweiten Browserfenster sind im Hintergrund verloren gegangen](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image7.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image11.png)

**Abbildung 7**: die Änderungen in der zweiten Browser Fenster wurden im Hintergrund verloren geht ([klicken Sie hier, um das Bild in voller Größe angezeigt](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image12.png))


Der Grund, warum die zweite Browser s Änderungen nicht ein Commit ausgeführt waren, wurde, da die `UPDATE` Anweisung s `WHERE` -Klausel herausgefiltert, alle Datensätze und daher keine Auswirkungen auf Zeilen. S betrachten können die `UPDATE` -Anweisung erneut aus:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample10.sql)]

Der zweite Browserfenster zur Aktualisierung des Datensatzes der ursprünglichen Produktname angegeben, der `WHERE` -Klausel verfügt über t Übereinstimmung oben mit dem vorhandenen Produktnamen (da es von der ersten Browser geändert wurde). Aus diesem Grund die Anweisung `[ProductName] = @original_ProductName` gibt "false", und die `UPDATE` wirkt sich nicht auf alle Datensätze.

> [!NOTE]
> Löschen Sie auf die gleiche Weise funktioniert. Starten Sie mit zwei Browserfenster öffnen, indem ein bestimmtes Produkt mit einer bearbeiten und anschließend die Änderungen zu speichern. Nach dem Speichern der Änderungen in einem Browser, klicken Sie auf die Schaltfläche "löschen" für das gleiche Produkt in einer anderen. Da die ursprünglichen Werte Stefan t ordnen Sie der `DELETE` Anweisung s `WHERE` -Klausel, der Löschvorgang im Hintergrund ein Fehler auftritt.


Aus der Endbenutzer-s-Perspektive in der zweiten Browserfenster wird nach dem Klicken auf die Schaltfläche "Aktualisieren" im Raster in die vorab Bearbeitungsmodus gibt ihre Änderungen wurden jedoch verloren. Allerdings dort s keine visuelle Bestätigung, die ihre Änderungen t bleiben. Im Idealfall benutzeränderungen s auf eine parallelitätsverletzung verloren, d wir benachrichtigen Sie sie und vielleicht, behalten Sie im Raster im Bearbeitungsmodus befindet. Lassen Sie s betrachten, wie dies zu erreichen.

## <a name="step-3-determining-when-a-concurrency-violation-has-occurred"></a>Schritt 3: Bestimmen, wann eine Parallelität verletzt wurde

Da eine parallelitätsverletzung die Änderungen ablehnt, eine vorgenommen wurde, wäre es gut, um den Benutzer zu warnen, wenn eine parallelitätsverletzung aufgetreten ist. Damit den Benutzer benachrichtigt werden, können s, fügen Sie ein Label-Websteuerelement am oberen Rand der Seite mit dem Namen `ConcurrencyViolationMessage` , deren `Text` Eigenschaft zeigt die folgende Meldung: Sie haben versucht, aktualisieren oder Löschen eines Datensatzes, die gleichzeitig von einem anderen Benutzer aktualisiert wurde. Bitte überprüfen Sie die Änderungen des anderen Benutzers und klicken Sie dann wiederholen Sie die Aktualisierung oder löschen. Legen Sie das Label-Steuerelement s `CssClass` Eigenschaft in "Warnung", also eine CSS-Klasse definiert, `Styles.css` , die Text in Rot, kursiv, fett und große Schriftart anzeigt. Legen Sie schließlich die Bezeichnung s `Visible` und `EnableViewState` Eigenschaften `false`. Dadurch wird die Bezeichnung mit Ausnahme von nur diejenigen, in denen wir explizit festgelegt, Postbacks ausgeblendet seine `Visible` Eigenschaft `true`.


[![Fügen Sie ein Bezeichnungsfeld-Steuerelement zur Seite, um die Warnung anzeigen](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image8.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image13.png)

**Abbildung 8**: Fügen Sie ein Bezeichnungsfeld-Steuerelement zur Seite, um die Warnung angezeigt ([klicken Sie hier, um das Bild in voller Größe angezeigt](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image14.png))


Beim Ausführen eines Updates oder löschen, die GridView s `RowUpdated` und `RowDeleted` Ereignishandler ausgelöst werden, nachdem die Datenquellen-Steuerelements die angeforderte Update- oder Delete ausgeführt hat. Wir können bestimmen, wie viele Zeilen durch den Vorgang aus dieser Ereignishandler betroffen sind. Wenn 0 Zeilen betroffen sind, möchten wir Anzeigen der `ConcurrencyViolationMessage` Bezeichnung.

Erstellen Sie einen Ereignishandler für beide die `RowUpdated` und `RowDeleted` Ereignisse und fügen Sie den folgenden Code hinzu:


[!code-csharp[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample11.cs)]

In beiden Ereignishandler überprüfen wir das `e.AffectedRows` Eigenschaft und, wenn er gleich 0 ist, legen Sie die `ConcurrencyViolationMessage` Bezeichnung s `Visible` Eigenschaft `true`. In der `RowUpdated` Ereignishandler, d. h. es auch festlegen, dass die GridView im Bearbeitungsmodus befinden muss, durch Festlegen der `KeepInEditMode` Eigenschaft auf "true". In diesem Fall müssen wir die Daten in den Entwurfsbereich zu binden, damit die anderen s Benutzerdaten in die Bearbeitungsoberfläche geladen werden. Dies erfolgt durch Aufrufen der GridView s `DataBind()` Methode.

Wie in Abbildung 9 gezeigt, mit diesen beiden Ereignishandler wird eine äußerst bemerkenswerten Meldung angezeigt, wenn eine parallelitätsverletzung liegt vor.


[![Bei einer Parallelitätsverletzung wird eine Meldung angezeigt.](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image9.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image15.png)

**Abbildung 9**: eine Meldung wird angezeigt, bei einer Parallelitätsverletzung ([klicken Sie hier, um das Bild in voller Größe angezeigt](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image16.png))


## <a name="summary"></a>Zusammenfassung

Beim Erstellen einer Webanwendung, in denen mehrere, gleichzeitige Benutzer möglicherweise die gleichen Daten bearbeiten es ist wichtig, die Parallelität Steuerelementoptionen in Betracht ziehen. Standardmäßig die Websteuerelemente ASP.NET-Daten und die Datenquellen-Steuerelementen parallelitätssteuerung nicht berücksichtigt. Wie wir in diesem Lernprogramm gesehen haben, ist die Implementierung die Steuerung durch vollständige Parallelität mit der SqlDataSource relativ schnell und einfach. Die SqlDataSource behandelt Großteil der Arbeit für Ihre hinzufügen, sodass die erweiterte `WHERE` -Klauseln zum automatisch generierten `UPDATE` und `DELETE` Anweisungen, aber es sind einige Besonderheiten zur Behandlung `NULL` Wert Spalten, wie beschrieben in der Ordnungsgemäßen Behandlung von `NULL` Abschnitt-Werte.

In diesem Lernprogramm wird unsere Untersuchung der SqlDataSource abgeschlossen. Unsere verbleibenden Lernprogramme werden zurückgegeben, für die Arbeit mit Daten mit dem ObjectDataSource und Tier-Architektur.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

>[!div class="step-by-step"]
[Zurück](inserting-updating-and-deleting-data-with-the-sqldatasource-cs.md)
[Weiter](querying-data-with-the-sqldatasource-control-vb.md)
