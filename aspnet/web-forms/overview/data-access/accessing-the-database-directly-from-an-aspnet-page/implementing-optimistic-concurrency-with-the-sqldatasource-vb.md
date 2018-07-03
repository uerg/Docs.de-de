---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/implementing-optimistic-concurrency-with-the-sqldatasource-vb
title: Implementieren von Optimistischer Parallelität mit dem SqlDataSource-Steuerelement (VB) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial haben wir überprüfen Sie die Grundlagen der Steuerung durch vollständige Parallelität und untersuchen Sie anschließend, wie Sie mit dem SqlDataSource-Steuerelement zu implementieren.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2007
ms.topic: article
ms.assetid: a8fa72ee-8328-4854-a419-c1b271772303
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/implementing-optimistic-concurrency-with-the-sqldatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: a71e5ee06e4a970e7e6d6c3b5b0351ad218e0253
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37391046"
---
<a name="implementing-optimistic-concurrency-with-the-sqldatasource-vb"></a>Implementieren von Optimistischer Parallelität mit dem SqlDataSource-Steuerelement (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunter](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_50_VB.exe) oder [PDF-Datei herunterladen](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/datatutorial50vb1.pdf)

> In diesem Tutorial haben wir überprüfen Sie die Grundlagen der Steuerung durch vollständige Parallelität und untersuchen Sie anschließend, wie Sie mit dem SqlDataSource-Steuerelement zu implementieren.


## <a name="introduction"></a>Einführung

Im vorherigen Tutorial untersucht wir hinzufügen, einfügen, aktualisieren und Löschen von Funktionen, um dem SqlDataSource-Steuerelement. Kurz gesagt, diese Features bieten wir zum erforderlich sind. Geben Sie den entsprechenden `INSERT`, `UPDATE`, oder `DELETE` SQL-Anweisung in das Steuerelement s `InsertCommand`, `UpdateCommand`, oder `DeleteCommand` Eigenschaften werden zusammen mit den entsprechenden Parameter in der `InsertParameters`, `UpdateParameters`, und `DeleteParameters` Sammlungen. Während diese Eigenschaften und Auflistungen manuell angegeben werden können, bietet das Konfigurieren von Datenquellen-Assistenten s Schaltfläche "Erweitert" einer generieren `INSERT`, `UPDATE`, und `DELETE` Anweisungen-Kontrollkästchen, das automatisch diese Anweisungen erstellen wird basierend auf der `SELECT` Anweisung.

Zusammen mit der Generierung `INSERT`, `UPDATE`, und `DELETE` Anweisungen deaktivieren, aktivieren Sie das Dialogfeld Erweiterte SQL-Generierungsoptionen enthält eine Option für die Verwendung optimistischer Parallelität (siehe Abbildung 1). Wenn dieses Kontrollkästchen aktiviert, die `WHERE` Klauseln in den automatisch generierten `UPDATE` und `DELETE` -Anweisungen werden geändert, um nur das Update auszuführen oder zu löschen, wenn das zugrunde liegenden Datenbank Daten t geändert wurde, seit der Benutzer letzten Laden der das in das Raster.


![Sie können die vollständige Parallelität-Unterstützung hinzufügen, von den erweiterten SQL-Generierung Dialogfeld "Optionen"](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image1.gif)

**Abbildung 1**: Sie können die vollständige Parallelität-Unterstützung hinzufügen, von den erweiterten SQL-Generierung Dialogfeld "Optionen"


In der [optimistische Parallelität implementieren](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md) Tutorial untersuchten wir die Grundlagen der Steuerung durch vollständige Parallelität und wie Sie es dem ObjectDataSource-Steuerelement hinzuzufügen. In diesem Tutorial wir auf die Grundlagen der Steuerung durch vollständige Parallelität Retuschieren und untersuchen Sie anschließend, wie Sie mit dem SqlDataSource-Steuerelement zu implementieren.

## <a name="a-recap-of-optimistic-concurrency"></a>Eine Zusammenfassung der vollständigen Parallelität

Für Webanwendungen, die ermöglichen, mehrere Benutzer gleichzeitig zum Bearbeiten oder löschen die gleichen Daten, besteht eine Möglichkeit, dass ein Benutzer kann einem anderen s ändert versehentlich überschreiben. In der [optimistische Parallelität implementieren](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md) Tutorial, die ich im folgenden Beispiel wird bereitgestellt:

Stellen Sie sich, dass zwei Benutzer, Jisun und Sam, sowohl eine Seite in einer Anwendung, für die Besucher besuchen wurden, aktualisieren und Löschen von Produkten über ein GridView-Steuerelement zulässig. Beide klicken Sie auf die Schaltfläche "Bearbeiten" für Chai entspricht ungefähr zur selben Zeit. Jisun ändert sich der Name des Produkts in Chai Tee und klickt auf die Schaltfläche "Aktualisieren". Das Ergebnis ist ein `UPDATE` -Anweisung, die für die Datenbank gesendet wird, die festlegt *alle* Produkt s aktualisierbaren Felder (obwohl Jisun nur für ein Feld aktualisiert `ProductName`). Zu diesem Zeitpunkt besitzt die Datenbank die Werte der Lieferant außergewöhnlichen fluessiger Form, und so weiter für dieses bestimmte Produkt Chai Tee, der Kategorie "Getränke,". Allerdings zeigt GridView Sam-s-Bildschirm als Chai immer noch den Namen des Produkts in der bearbeitbaren GridView-Zeile. Wenige Sekunden nach dem Jisun s Änderungen übernommen wurden Sam-die Kategorie "Gewürze" updates und Update klickt. Dies führt zu einer `UPDATE` Anweisung gesendet, um die Datenbank, die den Namen des Produkts, das Chai entspricht, legt diese fest der `CategoryID` zu den entsprechenden "Gewürze" Kategorie-ID, und So weiter. Jisun-s-Änderungen an den Namen des Produkts wurde überschrieben.

Abbildung 2 zeigt dies.


[![Wenn zwei Benutzer gleichzeitig einen Datensatz zu aktualisieren ändert es s Potenzial für einen Benutzer s zum Überschreiben der anderen s](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image2.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image1.png)

**Abbildung 2**: Wenn zwei Benutzer gleichzeitig das Update ein Datensatz vorhanden s Potenzial für einen Benutzer s ändert, überschreiben Sie die anderen s ([klicken Sie, um das Bild in voller Größe anzeigen](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image2.png))


Um zu verhindern, dass dieses Szenario Herausklappen, eine Form der [parallelitätssteuerung](http://en.wikipedia.org/wiki/Concurrency_control) implementiert werden muss. [Vollständige Parallelität](http://en.wikipedia.org/wiki/Optimistic_concurrency_control) der Schwerpunkt in diesem Tutorial funktioniert auf der Annahme, dass zwar möglicherweise Parallelitätskonflikte hin und wieder, der Großteil der Zeit, die solche Konflikte t gewonnen auftreten. Aus diesem Grund, wenn ein Konflikt auftreten,, informiert Steuerung für optimistische Parallelität einfach den Benutzer, dass ihre Änderungen kann nicht gespeichert werden, da ein anderer Benutzer die gleichen Daten geändert hat.

> [!NOTE]
> Für Anwendungen, in denen davon ausgegangen wird, ist, dass es viele Parallelitätskonflikte oder solche Konflikte nicht tolerierbar sind, kann dann Steuerung durch eingeschränkte Parallelität stattdessen verwendet werden. Siehe die [optimistische Parallelität implementieren](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md) Tutorial für eine ausführlichere Erläuterung der Steuerung durch eingeschränkte Parallelität.


Steuerung für optimistische Parallelität funktioniert, indem Sie sicherstellen, dass der Datensatz aktualisieren oder löschen die gleichen Werte verfügt, wie zuvor beim Aktualisieren oder Löschen von Prozess starten. Z. B. beim Klicken auf die Schaltfläche "Bearbeiten" in einem bearbeitbaren GridView-Ansicht, die-s-Datensatzwerte aus der Datenbank gelesen und in die Textfelder und anderen Websteuerelementen angezeigt. Diese ursprünglichen Werte werden durch die GridView gespeichert. Später, nachdem der Benutzer nimmt ihre Änderungen vor, und klickt auf die Schaltfläche "Aktualisieren", die `UPDATE` -Anweisung verwendet muss berücksichtigen Sie die ursprünglichen Werte sowie die neuen Werte und den zugrunde liegenden Datenbankdatensatz nur aktualisieren, wenn die ursprünglichen Werte, dass der Benutzer bearbeiten sind identisch mit den Werten noch in der Datenbank. Abbildung 3 zeigt diese Abfolge von Ereignissen.


[![Für die Update- oder Delete erfolgreich ausgeführt werden soll müssen die ursprünglichen Werte der aktuellen Datenbankwerte gleich sein.](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image3.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image3.png)

**Abbildung 3**: für die Update- oder Delete, hergestellt wird, die ursprünglichen Werte müssen werden gleich die aktuellen Datenbankwerte ([klicken Sie, um das Bild in voller Größe anzeigen](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image4.png))


Es gibt verschiedene Ansätze zum Implementieren von optimistischer Parallelität (finden Sie unter [Peter A. Bromberg](http://peterbromberg.net/) s [Optmistic Parallelität aktualisieren Logik](http://www.eggheadcafe.com/articles/20050719.asp) für einen kurzen Blick auf eine Reihe von Optionen). Die Technik, die verwendet werden, von dem SqlDataSource-Steuerelement (oder per ADO.NET typisierten DataSets, die in unserer Data Access Layer verwendet) ergänzt das `WHERE` -Klausel, um einen Vergleich aller die ursprünglichen Werte enthalten. Die folgenden `UPDATE` -Anweisung aktualisiert z. B. den Namen und den Preis eines Produkts nur dann, wenn die aktuellen Datenbankwerte die Werte gleich sind, die ursprünglich, beim Aktualisieren des Datensatzes in den GridView-Ansicht abgerufen wurden. Die `@ProductName` und `@UnitPrice` Parameter enthalten die neuen Werten, die vom Benutzer eingegeben haben, während `@original_ProductName` und `@original_UnitPrice` enthalten die Werte, die ursprünglich in der GridView geladen wurden, als auf die Schaltfläche "Bearbeiten" geklickt wurde:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample1.sql)]

In diesem Tutorial sehen, ist das Aktivieren der Steuerung durch vollständige Parallelität mit dem SqlDataSource-Steuerelement so einfach wie das Aktivieren eines Kontrollkästchens.

## <a name="step-1-creating-a-sqldatasource-that-supports-optimistic-concurrency"></a>Schritt 1: Erstellen einer SqlDataSource-Steuerelement, unterstützt optimistische Parallelität

Öffnen Sie zunächst die `OptimisticConcurrency.aspx` Seite die `SqlDataSource` Ordner. Ziehen Sie ein SqlDataSource-Steuerelement aus der Toolbox auf den Designer, Einstellungen der `ID` Eigenschaft `ProductsDataSourceWithOptimisticConcurrency`. Klicken Sie anschließend auf den Link "Datenquelle konfigurieren" aus dem Steuerelement-s-Smarttag. Wählen Sie aus dem ersten Bildschirm des Assistenten, arbeiten Sie mit der `NORTHWINDConnectionString` , und klicken Sie auf Weiter.


[![Wählen Sie für die Arbeit mit der NORTHWINDConnectionString](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image4.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image5.png)

**Abbildung 4**: Wählen Sie für die Arbeit mit der `NORTHWINDConnectionString` ([klicken Sie, um das Bild in voller Größe anzeigen](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image6.png))


In diesem Beispiel, das wir Hinzufügen einer GridView-Ansicht, die es ermöglicht Benutzern das Bearbeiten der `Products` Tabelle. Wählen Sie daher aus dem Bildschirm für die Select-Anweisung konfigurieren, die `Products` Tabelle aus der Dropdown-Liste, und wählen Sie die `ProductID`, `ProductName`, `UnitPrice`, und `Discontinued` Spalten, wie in Abbildung 5 dargestellt.


[![Aus der Produkttabelle zurück, die ProductID, ProductName, UnitPrice und nicht mehr unterstützte Spalten](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image5.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image7.png)

**Abbildung 5**: aus der `Products` Tabelle, zum Zurückgeben der `ProductID`, `ProductName`, `UnitPrice`, und `Discontinued` Spalten ([klicken Sie, um das Bild in voller Größe anzeigen](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image8.png))


Wählen Sie die Spalten, klicken Sie auf die Schaltfläche "Erweitert", um das Dialogfeld "Erweiterte SQL-Generierungsoptionen" zu öffnen. Überprüfen Sie die generieren `INSERT`, `UPDATE`, und `DELETE` Anweisungen und verwenden Sie optimistische Parallelität Kontrollkästchen, und klicken Sie auf OK (siehe Abbildung 1 für einen Screenshot). Schließen Sie den Assistenten, indem Sie auf Weiter, und schließen.

Nach Abschluss der Konfigurieren von Datenquellen-Assistenten können Sie überprüfen die resultierende `DeleteCommand` und `UpdateCommand` Eigenschaften und die `DeleteParameters` und `UpdateParameters` Sammlungen. Die einfachste Möglichkeit hierzu ist, klicken auf der Registerkarte "Datenquelle" in der unteren linken Ecke auf der Seite s deklarativen Syntax finden Sie unter. Dort sehen Sie ein `UpdateCommand` Wert:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample2.sql)]

Mit sieben Parameter in der `UpdateParameters` Auflistung:


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample3.aspx)]

Auf ähnliche Weise die `DeleteCommand` Eigenschaft und `DeleteParameters` Auflistung sollte wie folgt aussehen:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample4.sql)]


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample5.aspx)]

Zusätzlich zum Erweitern der `WHERE` Klauseln der `UpdateCommand` und `DeleteCommand` Eigenschaften (und die zusätzlichen Parameter für die jeweiligen Parameter Sammlungen hinzufügen), wählen die Verwendung von optimistischer Parallelitätsoption passt an zwei andere Eigenschaften:

- Änderungen der [ `ConflictDetection` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.conflictdetection.aspx) aus `OverwriteChanges` (Standard), `CompareAllValues`
- Änderungen der [ `OldValuesParameterFormatString` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.oldvaluesparameterformatstring.aspx) aus {0} (Standard) zum Originalbild\_ {0} .

Wenn die Daten-Websteuerelement s SqlDataSource-Steuerelement aufruft `Update()` oder `Delete()` -Methode, die ursprünglichen Werte übergibt. Wenn die SqlDataSource s `ConflictDetection` -Eigenschaftensatz auf `CompareAllValues`, diese ursprünglichen Werte werden an den Befehl hinzugefügt. Die `OldValuesParameterFormatString` Eigenschaft stellt das Muster für diese Wertparameter der ursprüngliche verwendet. Der Konfigurieren von Datenquellen-Assistent verwendet die ursprüngliche\_ {0} und nennt alle ursprünglichen Parameter in der `UpdateCommand` und `DeleteCommand` Eigenschaften und `UpdateParameters` und `DeleteParameters` Sammlungen entsprechend.

> [!NOTE]
> Da wir erneut die nicht mit dem SqlDataSource-Steuerelement STRG + s, Einfügen von Funktionen, können Sie entfernen die `InsertCommand` Eigenschaft und die zugehörige `InsertParameters` Auflistung.


## <a name="correctly-handlingnullvalues"></a>Ordnungsgemäße Handhabung`NULL`Werte

Leider ist das ergänzte `UPDATE` und `DELETE` Anweisungen automatisch generierte vom Konfigurieren von Datenquellen-Assistenten bei Verwendung der vollständigen Parallelität werden *nicht* arbeiten mit Datensätzen, die enthalten `NULL` Werte. Berücksichtigen Sie deshalb erhalten unsere SqlDataSource s `UpdateCommand`:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample6.sql)]

Die `UnitPrice` -Spalte in der `Products` Tabelle haben `NULL` Werte. Wenn ein bestimmter Datensatz enthält eine `NULL` Wert für `UnitPrice`, `WHERE` Klausel Teil `[UnitPrice] = @original_UnitPrice` wird *immer* auf "false" ausgewertet werden, da `NULL = NULL` gibt immer "false" zurück. Aus diesem Grund, die Datensätze enthalten `NULL` Werte können nicht bearbeitet oder gelöscht wird, als die `UPDATE` und `DELETE` Anweisungen `WHERE` Klauseln gewonnen t zurück zum Aktualisieren oder Löschen von Zeilen.

> [!NOTE]
> Dieser Fehler wurde zuerst im Juni von 2004 in an Microsoft gemeldet [SqlDataSource-Steuerelement generiert falsche SQL-Anweisungen](https://connect.microsoft.com/VisualStudio/feedback/ViewFeedback.aspx?FeedbackID=93937) und in der nächsten Version von ASP.NET korrigiert werden demnach geplant ist.


Um dieses Problem zu beheben, müssen wir manuell aktualisieren die `WHERE` Klauseln in sowohl die `UpdateCommand` und `DeleteCommand` Eigenschaften für **alle** Spalten können mit `NULL` Werte. Im Allgemeinen ändern `[ColumnName] = @original_ColumnName` auf:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample7.sql)]

Diese Änderung können direkt über die deklaratives Markup, über die Optionen UpdateQuery oder DeleteQuery im Eigenschaftenfenster oder über das UPDATE vorgenommen werden, und Löschen von Registerkarten in der Specify, benutzerdefinierte SQL-Anweisung oder gespeicherte Prozedur-Option in den Daten konfigurieren Datenquellen-Assistenten. In diesem Fall muss diese Änderung vorgenommen werden, für die *jeder* -Spalte in der `UpdateCommand` und `DeleteCommand` s `WHERE` -Klausel, die enthalten, kann `NULL` Werte.

Anwenden dieses zu unserem Beispiel führt die folgende geänderte `UpdateCommand` und `DeleteCommand` Werte:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample8.sql)]

## <a name="step-2-adding-a-gridview-with-edit-and-delete-options"></a>Schritt 2: Hinzufügen einer GridView-Ansicht mit bearbeiten und Löschoptionen

Mit dem SqlDataSource-Steuerelement so konfiguriert, dass das unterstützen der optimistischen Parallelität übrig bleibt ein Daten-Websteuerelement zur Seite hinzufügen, der diese parallelitätssteuerung verwendet. In diesem Tutorial können Sie s Hinzufügen einer GridView-Ansicht, die beide bearbeiten bereitstellt und die Löschfunktionen. Um dies zu erreichen, ziehen Sie in einer GridView-Ansicht aus der Toolbox in den Designer und den Satz der `ID` zu `Products`. Von GridView s Smarttags, binden Sie es an der `ProductsDataSourceWithOptimisticConcurrency` SqlDataSource-Steuerelement, die in Schritt 1 hinzugefügt. Überprüfen Sie abschließend die Optionen für die Bearbeitung aktivieren und löschen aktivieren aus dem Smarttag.


[![Binden Sie GridView zu, auf dem SqlDataSource-Steuerelement, und aktivieren Sie, bearbeiten und löschen](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image6.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image9.png)

**Abbildung 6**: GridView zu binden, SqlDataSource-Steuerelement und Bearbeitung aktivieren und löschen ([klicken Sie, um das Bild in voller Größe anzeigen](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image10.png))


Konfigurieren Sie nach dem Hinzufügen der GridView, seine Darstellung durch das Entfernen der `ProductID` BoundField, Ändern der `ProductName` BoundField-s `HeaderText` Eigenschaft Produkt, und Aktualisieren der `UnitPrice` BoundField, damit die `HeaderText` -Eigenschaft ist einfach Preis. Im Idealfall d optimiert, dass die Bearbeitungsschnittstelle einen RequiredFieldValidator für die Einbeziehung der `ProductName` Wert und einem CompareValidator für die `UnitPrice` Wert (um es sicherzustellen, dass s ein ordnungsgemäß formatierter numerischer Wert). Finden Sie in der [Anpassen der Benutzeroberfläche für die Änderung der Daten](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) Tutorial für eine eingehendere Betrachtung die GridView-s bearbeiten-Schnittstelle anpassen.

> [!NOTE]
> Das GridView, die Ansichtszustand s aktiviert werden muss, da die ursprünglichen Werte, die von der GridView an dem SqlDataSource-Steuerelement übergeben werden im Ansichtszustand gespeichert.


Nach dem vornehmen dieser Änderungen an die GridView, sollte GridView und SqlDataSource deklarative Markup etwa wie folgt aussehen:


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample9.aspx)]

Um die Steuerung für optimistische Parallelität in Aktion zu sehen, öffnen Sie zwei Browserfenster, und laden die `OptimisticConcurrency.aspx` Seite in beiden. Klicken Sie auf die Schaltflächen Bearbeiten, für das erste Produkt in beiden Browsern. In einem Browser aus ändern Sie den Namen des Produkts, und klicken Sie auf aktualisieren. Der Browser wird postback und GridView wird zurück zu den vorab Bearbeitungsmodus mit dem neuen Produktnamen für den Datensatz, der gerade bearbeitet.

Im zweiten Browserfenster ändern Sie den Preis (aber lassen Sie den Produktnamen als den ursprünglichen Wert), und klicken Sie auf aktualisieren. Beim Postback im Raster auf den vorab Bearbeitungsmodus gibt, aber die Änderung der Preis wird nicht aufgezeichnet. Der zweite Browser zeigt dem gleichen Wert wie die erste Bedingung der neue Produktname mit dem alten Preis. Die Änderungen in der zweiten Browserfenster sind verloren gegangen. Darüber hinaus sind die Änderungen verloren gegangen stattdessen automatisch, wie es war keine Ausnahme oder eine Meldung, dass eine parallelitätsverletzung soeben aufgetreten ist.


[![Die Änderungen in der zweiten Browserfenster sind im Hintergrund verloren gegangen.](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image7.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image11.png)

**Abbildung 7**: die Änderungen in der zweiten Browser-Fenster im Hintergrund gingen ([klicken Sie, um das Bild in voller Größe anzeigen](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image12.png))


Der Grund, warum die zweite browseränderungen s nicht ein Commit ausgeführt waren, wurde, da die `UPDATE` Anweisung s `WHERE` -Klausel gefiltert, um alle Datensätze und daher keine Auswirkungen auf Zeilen. S betrachten können die `UPDATE` Anweisung erneut aus:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample10.sql)]

Wenn das zweite Browserfenster den Datensatz aktualisiert wird, wird der ursprüngliche Produktname angegeben, der `WHERE` Klausel t Übereinstimmung sich mit vorhandenen Namen des Produkts (da es vom ersten Browser geändert wurde). Aus diesem Grund die Anweisung `[ProductName] = @original_ProductName` gibt False zurück, und die `UPDATE` wirkt sich nicht auf alle Einträge.

> [!NOTE]
> Löschen Sie auf die gleiche Weise funktioniert. Mit zwei Browserfenster öffnen zu beginnen, indem Sie ein bestimmtes Produkt mit einem bearbeiten und dann die Änderungen zu speichern. Klicken Sie nach dem Speichern der Änderungen in einem Browser an, auf die Schaltfläche "löschen" für das gleiche Produkt in der anderen. Da die ursprünglichen Werte ich möchte faktenpartition der `DELETE` Anweisung s `WHERE` -Klausel, der Löschvorgang im Hintergrund ein Fehler auftritt.


Aus Sicht der Endbenutzer-s in der zweiten Browserfenster nach dem Klicken auf die Schaltfläche "Aktualisieren" im Raster gibt zurück, in den Bearbeitungsmodus vor, aber die Änderungen sind verloren gegangen. Jedoch dort s keine visuelle Feedback, das Änderungen hat Teamblog zu halten. Im Idealfall eine benutzeränderungen s auf eine parallelitätsverletzung verloren gehen, wir d informiert und vielleicht das Raster im Bearbeitungsmodus belassen. Lassen Sie s, betrachten dazu zu erhalten.

## <a name="step-3-determining-when-a-concurrency-violation-has-occurred"></a>Schritt 3: Bestimmen, wenn eine Parallelitätsverletzung aufgetreten ist

Da eine parallelitätsverletzung die Änderungen zurückgewiesen wird, die eine vorgenommen hat, wäre es schön, die den Benutzer zu warnen, wenn eine parallelitätsverletzung aufgetreten ist. Um den Benutzer darauf aufmerksam, Let s, fügen Sie ein Label-Steuerelement an den Anfang der Seite mit dem Namen `ConcurrencyViolationMessage` , deren `Text` Eigenschaft zeigt die folgende Meldung: Sie haben versucht, aktualisieren oder Löschen eines Datensatzes, die gleichzeitig von einem anderen Benutzer aktualisiert wurde. Bitte überprüfen Sie die Änderungen des anderen Benutzers, und klicken Sie dann wiederholen Sie das Update oder löschen. Legen Sie das Label-Steuerelement s `CssClass` -Eigenschaft in "Warnung", ist eine CSS-Klasse definiert, `Styles.css` , Text in Rot, kursiv, fett und große Schriftart anzeigt. Legen Sie schließlich die Bezeichnung s `Visible` und `EnableViewState` Eigenschaften `False`. Dadurch wird ausgeblendet, die Bezeichnung mit Ausnahme von nur diese Postbacks, in dem wir explizit festlegen, seiner `Visible` Eigenschaft `True`.


[![Fügen Sie ein Label-Steuerelement auf der Seite zum Anzeigen der Warnung](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image8.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image13.png)

**Abbildung 8**: Fügen Sie ein Label-Steuerelement auf der Seite zum Anzeigen der Warnung ([klicken Sie, um das Bild in voller Größe anzeigen](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image14.png))


Beim Durchführen eines Updates "oder" Delete "," das GridView-s `RowUpdated` und `RowDeleted` Ereignishandler ausgelöst werden, nachdem die Datenquellen-Steuerelement die angeforderte Update- oder Delete ausgeführt hat. Wir können ermitteln, wie viele Zeilen durch den Vorgang von diesen Ereignishandlern betroffen sind. Wenn keine Zeilen betroffen sind, angezeigt werden sollten die `ConcurrencyViolationMessage` Bezeichnung.

Erstellen Sie einen Ereignishandler für beide die `RowUpdated` und `RowDeleted` Ereignisse, und fügen Sie den folgenden Code hinzu:


[!code-vb[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample11.vb)]

In beiden Ereignishandler überprüfen wir die `e.AffectedRows` Eigenschaft und, wenn er das gleich 0 ist, legen Sie die `ConcurrencyViolationMessage` Bezeichnung s `Visible` Eigenschaft `True`. In der `RowUpdated` -Ereignishandler wir auch anweisen, den die GridView, in den Bearbeitungsmodus zu bleiben, durch Festlegen der `KeepInEditMode` Eigenschaft auf "true". In diesem Fall müssen wir die Daten in das Raster binden, damit die anderen s Benutzerdaten in die Bearbeitungsschnittstelle geladen werden. Dies erfolgt durch Aufrufen der GridView-s `DataBind()` Methode.

Wie in Abbildung 9 gezeigt, bei diesen zwei Ereignishandlern, wird eine äußerst bemerkenswerten Meldung angezeigt, wenn eine parallelitätsverletzung liegt vor.


[![Bei einer Verletzung der Parallelität wird eine Meldung angezeigt.](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image9.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image15.png)

**Abbildung 9**: eine Meldung wird angezeigt, bei dem eine Parallelitätsverletzung ([klicken Sie, um das Bild in voller Größe anzeigen](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image16.png))


## <a name="summary"></a>Zusammenfassung

Beim Erstellen einer Webanwendung, in denen mehrere, gleichzeitige Benutzer möglicherweise die gleichen Daten bearbeiten es ist wichtig, die Verwaltungsoptionen für die Parallelität zu berücksichtigen. Standardmäßig ist die ASP.NET-Daten, die steuert, Web und die Datenquellen-Steuerelemente keine parallelitätssteuerung nutzen. Wie wir in diesem Tutorial gesehen haben, ist die Implementierung der Steuerung durch vollständige Parallelität mit dem SqlDataSource-Steuerelement relativ schnell und einfach. Dem SqlDataSource-Steuerelement behandelt den größten Teil der Arbeit aus, für Ihre hinzufügen Erweitert `WHERE` -Klauseln verwenden, um den automatisch generierten `UPDATE` und `DELETE` Anweisungen, es gibt jedoch sind einige Besonderheiten bei der Verwendung von `NULL` Wert der Spalten, wie unter der Ordnungsgemäße Handhabung `NULL` Abschnitt Werte.

Dieses Lernprogramm schließt unserer Untersuchung von dem SqlDataSource-Steuerelement. Die verbleibenden Lernprogramme werden zurückgegeben, für die Arbeit mit Daten, die mit dem ObjectDataSource-Steuerelement und eine geschichtete Architektur.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Vorherige](inserting-updating-and-deleting-data-with-the-sqldatasource-vb.md)
