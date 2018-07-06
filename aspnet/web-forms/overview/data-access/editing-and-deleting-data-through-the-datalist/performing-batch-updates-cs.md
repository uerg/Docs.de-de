---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/performing-batch-updates-cs
title: Durchführen von BatchUpdates (c#) | Microsoft-Dokumentation
author: rick-anderson
description: Erfahren Sie, wie erstellen Sie eine vollständig bearbeitbare DataList-Steuerelement, in dem alle Elemente sind im, Bearbeitungsmodus wird, deren Werte gespeichert werden können, indem Sie auf eine Schaltfläche "Alle aktualisieren" auf die...
ms.author: aspnetcontent
ms.date: 10/30/2006
ms.assetid: 57743ca7-5695-4e07-aed1-44b297f245a9
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/performing-batch-updates-cs
msc.type: authoredcontent
ms.openlocfilehash: 3528444269a3595681696251d3906a204090410c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807804"
---
<a name="performing-batch-updates-c"></a>Durchführen von BatchUpdates (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunter](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_37_CS.exe) oder [PDF-Datei herunterladen](performing-batch-updates-cs/_static/datatutorial37cs1.pdf)

> Erfahren Sie, wie erstellen Sie eine vollständig bearbeitbare DataList-Steuerelement, in dem alle Elemente befinden sich in, zu bearbeiten, Modus und die Schaltfläche "Alle aktualisieren" auf der Seite, deren Werte gespeichert werden können.


## <a name="introduction"></a>Einführung

In der [vorherigen Lernprogramm](an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md) wurde beschrieben, wie ein DataList auf Elementebene zu erstellen. Wie standard bearbeitbaren GridView jedes Element im DataList-Steuerelement enthalten eine Bearbeitung-Schaltfläche geklickt wird, würde das Element bearbeitet werden machen. Während dies auf Elementebene Bearbeitung eignet sich gut für Daten, die nur gelegentlich aktualisiert werden, erfordern bestimmte Szenarien zu Anwendungsfällen die Benutzer viele Datensätze zu bearbeiten. Wenn ein Benutzer so bearbeiten Sie Dutzende von Datensätzen muss und gezwungen, klicken Sie auf Bearbeiten, stellen ihre Änderungen und Updates für jeden einzelnen auf, kann die Menge des auf ihre Produktivität behindern. In solchen Situationen eine bessere Option darstellt, zu einem vollständig bearbeitbare DataList-Steuerelement, eine Where *alle* Elemente werden im Bearbeitungsmodus befindet und deren Werte bearbeitet werden können, indem Sie auf eine Schaltfläche "Alle aktualisieren", auf der Seite (siehe Abbildung 1).


[![Jedes Element in einem vollständig bearbeitbare DataList-Steuerelement kann geändert werden](performing-batch-updates-cs/_static/image2.png)](performing-batch-updates-cs/_static/image1.png)

**Abbildung 1**: jedes Element in einem vollständig bearbeitbare DataList-Steuerelement kann geändert werden ([klicken Sie, um das Bild in voller Größe anzeigen](performing-batch-updates-cs/_static/image3.png))


In diesem Tutorial betrachten wir, wie Benutzer zum Aktualisieren der Lieferanten-Adressinformationen, die mit einem DataList-Steuerelement vollständig bearbeitet werden können.

## <a name="step-1-create-the-editable-user-interface-in-the-datalist-s-itemtemplate"></a>Schritt 1: Erstellen der bearbeitbaren-Benutzeroberfläche in der ItemTemplate s DataList-Steuerelement

Im vorherigen Tutorial, in denen wir zu einem standard, der auf Elementebene bearbeitbaren DataList-Steuerelement erstellen, wir zwei Vorlagen verwendet:

- `ItemTemplate` enthalten, die nur-Lese Benutzeroberfläche (die Bezeichnung-Web-Steuerelemente zum Anzeigen von jeder s Produktnamen und Preis).
- `EditItemTemplate` enthalten die bearbeiten-Modus-Benutzeroberfläche (die beiden TextBox-Web-Steuerelemente).

DataList-Steuerelement s `EditItemIndex` Eigenschaft gibt vor, was `DataListItem` (sofern vorhanden) gerendert wird, mit der `EditItemTemplate`. Insbesondere die `DataListItem` , deren `ItemIndex` Wert entspricht die DataList s `EditItemIndex` Eigenschaft gerendert wird, mit der `EditItemTemplate`. Dieses Modell funktioniert gut, wenn Sie eine Zeit, aber voneinander fällt nur ein Element bearbeitet werden kann, bei einem vollständig bearbeitbare DataList-Steuerelement zu erstellen.

Für eine vollständig bearbeitbare DataList, möchten wir *alle* von der `DataListItem` s zum Rendern mithilfe der Benutzeroberfläche bearbeitet werden. Die einfachste Möglichkeit hierzu ist, definiert die bearbeitbaren-Schnittstelle in der `ItemTemplate`. Zum Ändern der Lieferanten-Adressinformationen, enthält die bearbeitbare Schnittstelle die Lieferantenname als Text und anschließend die Textfelder für die Adresse, Stadt und Land-Werte.

Öffnen Sie zunächst die `BatchUpdate.aspx` Seite, fügen Sie ein DataList-Steuerelement hinzu, und legen dessen `ID` Eigenschaft `Suppliers`. Deaktivieren Sie aus dem Smarttag DataList s, um ein neues ObjectDataSource-Steuerelement, das mit dem Namen hinzuzufügen `SuppliersDataSource`.


[![Erstellen Sie eine neue, mit dem Namen SuppliersDataSource "ObjectDataSource"](performing-batch-updates-cs/_static/image5.png)](performing-batch-updates-cs/_static/image4.png)

**Abbildung 2**: Erstellen einer neuen "ObjectDataSource" mit dem Namen `SuppliersDataSource` ([klicken Sie, um das Bild in voller Größe anzeigen](performing-batch-updates-cs/_static/image6.png))


Konfigurieren Sie zum Abrufen von Daten, die mit dem ObjectDataSource-Steuerelement die `SuppliersBLL` Klasse s `GetSuppliers()` Methode (siehe Abbildung 3). Wie bei dem vorherigen Lernprogramm, anstatt die Lieferanteninformationen über das ObjectDataSource-Steuerelement wird aktualisiert, arbeiten wir direkt mit der Geschäftslogikebene. Legen Sie daher die Dropdown-Liste (keine) auf der Registerkarte "UPDATE" (siehe Abbildung 4).


[![Abrufen von Lieferanteninformationen mithilfe der GetSuppliers()-Methode](performing-batch-updates-cs/_static/image8.png)](performing-batch-updates-cs/_static/image7.png)

**Abbildung 3**: Abrufen von Lieferanten Informationen mithilfe der `GetSuppliers()` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](performing-batch-updates-cs/_static/image9.png))


[![Legen Sie die Dropdown-Liste (keine) in der Registerkarte "Updates"](performing-batch-updates-cs/_static/image11.png)](performing-batch-updates-cs/_static/image10.png)

**Abbildung 4**: Legen Sie die Dropdown-Liste (keine) auf der Registerkarte "UPDATE" ([klicken Sie, um das Bild in voller Größe anzeigen](performing-batch-updates-cs/_static/image12.png))


Nach Abschluss des Assistenten, generiert Visual Studio automatisch die DataList s `ItemTemplate` zum Anzeigen der einzelnen Datenfelder, die von der Datenquelle in ein Label-Steuerelement zurückgegeben. Wir müssen diese Vorlage zu ändern, sodass sie stattdessen die Bearbeitungsschnittstelle bieten. Die `ItemTemplate` kann angepasst werden, mithilfe des Designers mit der Option "Vorlagen bearbeiten" aus dem Smarttag DataList s oder direkt über die deklarative Syntax.

Nehmen Sie einen Moment Zeit, um eine Bearbeiten-Schnittstelle zu erstellen, die wird der Name des Lieferanten s als Text angezeigt, sondern enthält Textfelder für die Lieferanten-e-Adresse, Stadt und Land-Werte. Nach diesen Änderungen sollte Ihre Seite s deklarative Syntax ähnlich der folgenden aussehen:


[!code-aspx[Main](performing-batch-updates-cs/samples/sample1.aspx)]

> [!NOTE]
> Als muss mit dem vorherigen Lernprogramm, DataList-Steuerelement in diesem Tutorial Ansichtszustand aktiviert sein.


In der `ItemTemplate` ich m mithilfe von zwei neuen CSS-Klassen `SupplierPropertyLabel` und `SupplierPropertyValue`, die hinzugefügt wurden die `Styles.css` -Klasse und konfiguriert die gleichen stileinstellungen wie die `ProductPropertyLabel` und `ProductPropertyValue` CSS-Klassen.


[!code-css[Main](performing-batch-updates-cs/samples/sample2.css)]

Finden Sie nach diesen Änderungen auf dieser Seite über einen Browser. Wie in Abbildung 5 gezeigt, wird jedes DataList-Element zeigt den Lieferantennamen als Text und Textfelder zum Anzeigen der Adresse, Stadt und Land verwendet.


[![Jeder Lieferant im DataList-Steuerelement ist bearbeitbar](performing-batch-updates-cs/_static/image14.png)](performing-batch-updates-cs/_static/image13.png)

**Abbildung 5**: jeder Lieferant im DataList-Steuerelement ist bearbeitbar ([klicken Sie, um das Bild in voller Größe anzeigen](performing-batch-updates-cs/_static/image15.png))


## <a name="step-2-adding-an-update-all-button"></a>Schritt 2: Hinzufügen einer Update-Schaltfläche "alle"

Während der einzelnen Lieferanten in Abbildung 5 die Adresse, Stadt und Landesfelder, die in einem Textfeld angezeigt hat, ist gibt es derzeit keine Schaltfläche "Aktualisieren" verfügbar. Anstatt eine Aktualisieren-Schaltfläche pro Element, mit vollständig bearbeitbare DataList-Steuerelementen unterstützt besteht in der Regel eine einzelne alle Aktualisieren-Schaltfläche auf der Seite, die beim Klicken auf updates *alle* der Datensätze im DataList-Steuerelement. In diesem Tutorial können Sie s zwei Update alle Schaltflächen - einen am oberen Rand der Seite und einen am unteren Rand hinzufügen (auch wenn eine der Schaltflächen auf die gleiche Auswirkung haben wird).

Hinzufügen eines Steuerelements Schaltfläche Web über das DataList-Steuerelement und legen Sie zunächst die `ID` Eigenschaft `UpdateAll1`. Als Nächstes fügen Sie die zweite Schaltfläche Websteuerelement unter DataList-Steuerelement, Festlegen der `ID` zu `UpdateAll2`. Legen Sie die `Text` Eigenschaften für die beiden Schaltflächen Alle aktualisieren. Erstellen von Ereignishandlern und schließlich für beide Tasten `Click` Ereignisse. Statt der Aktualisierungslogik in jeder der Ereignishandler, Umgestalten können s Diese Logik an eine dritte Methode `UpdateAllSupplierAddresses`, müssen die Ereignishandler, die diese dritte Methode einfach im aufrufen.


[!code-csharp[Main](performing-batch-updates-cs/samples/sample3.cs)]

Abbildung 6 zeigt die Seite, nachdem die Aktualisierung alle Schaltflächen hinzugefügt wurden.


[![Zwei Update alle Schaltflächen wurden auf der Seite hinzugefügt](performing-batch-updates-cs/_static/image17.png)](performing-batch-updates-cs/_static/image16.png)

**Abbildung 6**: wurden zwei Update alle Schaltflächen auf der Seite hinzugefügt ([klicken Sie, um das Bild in voller Größe anzeigen](performing-batch-updates-cs/_static/image18.png))


## <a name="step-3-updating-all-of-the-suppliers-address-information"></a>Schritt 3: Aktualisieren aller die Lieferanten-Adressinformationen

Mit allen die DataList-s-Elemente die Bearbeitungsschnittstelle anzeigen und durch das Aktualisieren aller Schaltflächen hinzufügen alle diese bleibt ist das Schreiben des Codes zur Ausführung der BatchUpdate. Insbesondere müssen wir so durchlaufen Sie das DataList-s-Elementen, und rufen die `SuppliersBLL` Klasse s `UpdateSupplierAddress` Methode für jede einzelne.

Die Auflistung der `DataListItem` Instanzen dieser Zusammensetzung DataList-Steuerelement kann, über DataList-Steuerelement s zugegriffen werden [ `Items` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.items.aspx). Mit einem Verweis auf eine `DataListItem`, können wir ziehen Sie das entsprechende `SupplierID` aus der `DataKeys` Auflistung und programmgesteuert Verweis im TextBox-Web-innerhalb Steuerelemente der `ItemTemplate` wie der folgende Code veranschaulicht:


[!code-csharp[Main](performing-batch-updates-cs/samples/sample4.cs)]

Eine der Schaltflächen Alle aktualisieren, klickt der Benutzer die `UpdateAllSupplierAddresses` Methode durchläuft die einzelnen `DataListItem` in die `Suppliers` DataList und ruft die `SuppliersBLL` s-Klasse `UpdateSupplierAddress` Methode und die entsprechenden Werte übergeben. Ein nicht eingegebene Wert für die Adresse, Stadt oder Land übergibt ist ein Wert von `Nothing` zu `UpdateSupplierAddress` (anstatt eine leere Zeichenfolge), was dazu führt, in einer Datenbank `NULL` für die zugrunde liegenden Datensatz s-Felder.

> [!NOTE]
> Als eine Erweiterung empfiehlt sich der Status Bezeichnung-Websteuerelement zur Seite hinzufügen, die eine bestätigungsmeldung bereitstellt, nachdem das BatchUpdate ausgeführt wird.


## <a name="updating-only-those-addresses-that-have-been-modified"></a>Aktualisieren nur die Adressen, die geändert wurde.

Der Batch-Update-Algorithmus für dieses Lernprogramm ruft verwendet die `UpdateSupplierAddress` -Methode für *jeder* Lieferanten im DataList-Steuerelement, unabhängig davon, ob ihre Adressinformationen geändert wurde. Während solche Blind sind t in der Regel ein Leistungsproblem aktualisiert, können sie zu überflüssigen Datensätze führen re-Überwachung in die Datenbanktabelle überschreiten. Angenommen, Sie Trigger verwenden, um alle aufzuzeichnen `UPDATE` s, um die `Suppliers` Tabelle in die auf eine Überwachung, jedes Mal, wenn ein Benutzer die Schaltfläche "Alle aktualisieren" klickt, ein neuen Überwachungsdatensatz für jeden Lieferanten werden, im System erstellt wird, unabhängig davon, ob der Benutzer Änderungen vorgenommen, ändert.

Die ADO.NET DataTable und DataAdapter-Klassen dienen zur Unterstützung von Batchaktualisierungen, in denen eine Datenbankkommunikation nur geändert, gelöscht und neue Datensätze zu. Jede Zeile in der Datentabelle verfügt über eine [ `RowState` Eigenschaft](https://msdn.microsoft.com/library/system.data.datarow.rowstate.aspx) , der angibt, ob die Zeile der Datentabelle, die von ihm, die geändert wurde, gelöscht oder unverändert bleibt. Wenn es sich bei eine "DataTable" Anfangs aufgefüllt wird, werden alle Zeilen nicht geändert markiert. Ändern des Werts auf die Zeile s-Spalten wird die Zeile als geändert gekennzeichnet.

In der `SuppliersBLL` Klasse, die wir die Adressinformationen für die angegebene Lieferanten s vom ersten Lesen des Datensatzes der einzelnen Lieferanten in Aktualisieren einer `SuppliersDataTable` und legen Sie dann die `Address`, `City`, und `Country` Spaltenwerte, die mit dem folgenden Code:


[!code-csharp[Main](performing-batch-updates-cs/samples/sample5.cs)]

Dieser Code weist naiv die übergebene Adresse, Stadt und Land-Werte, die `SuppliersRow` in die `SuppliersDataTable` unabhängig davon, ob die Werte geändert haben. Diese Änderungen dazu führen, dass die `SuppliersRow` s `RowState` Eigenschaft als geändert markiert werden. Wenn der Datenzugriffsebene s `Update` Methode aufgerufen wird, wird es angezeigt wird, die die `SupplierRow` geändert wurde, und deshalb sendet eine `UPDATE` Befehl in der Datenbank.

Stellen Sie sich vor, jedoch, dass wir Code, um diese Methode übergebene Adresse, Stadt und Land-Werte nur zuweisen hinzugefügt, wenn Unterschiede zu den `SuppliersRow` s vorhandenen Werte. Im Fall, in dem die Adresse, Stadt und Land identisch mit den vorhandenen Daten sind, werden keine Änderungen vorgenommen und die `SupplierRow` s `RowState` links als unverändert. Das Ergebnis ist, wenn die DAL-s `Update` -Methode aufgerufen wird, wird kein Datenbankaufruf vorgenommen werden, da die `SuppliersRow` wurde nicht geändert.

Um diese Änderung in Kraft gesetzt, ersetzen Sie die Anweisungen, die blind weisen Sie die übergebene Adresse, Stadt und Land-Werte, mit dem folgenden Code:


[!code-csharp[Main](performing-batch-updates-cs/samples/sample6.cs)]

Mit dieser zusätzlichen Code, der DAL s `Update` -Methode sendet ein `UPDATE` Anweisung an die Datenbank für die nur die Datensätze, deren Werte adressbezogenen geändert haben.

Sie können auch wir behalten den Überblick darüber, ob Unterschiede zwischen den Adressfeldern übergebene und die Daten der Datenbank vorhanden sind und, wenn keine vorhanden sind, umgehen Sie einfach den Aufruf der DAL-s `Update` Methode. Dieser Ansatz funktioniert gut, wenn Sie erneut mit dem DB-Methode, leiten, da es sich bei das DB-direktmethode ist t übergeben eine `SuppliersRow` Instanz, deren `RowState` kann überprüft werden, um zu bestimmen, ob ein Datenbankaufruf tatsächlich benötigt wird.

> [!NOTE]
> Jedes Mal die `UpdateSupplierAddress` -Methode wird aufgerufen, um die Datenbank wird ein Aufruf ausgelöst, um Informationen zu den aktualisierten Datensatz abzurufen. Klicken Sie dann, wenn alle Änderungen in den Daten vorhanden sind, einen anderen Aufruf an die Datenbank erfolgt auf die Tabellenzeile zu aktualisieren. Dieser Workflow kann optimiert werden, durch das Erstellen einer `UpdateSupplierAddress` methodenüberladung, die akzeptiert eine `EmployeesDataTable` Instanz mit *alle* der Änderungen aus der `BatchUpdate.aspx` Seite. Es konnte stellen Sie dann einen Aufruf an die Datenbank zum Abrufen aller Datensätze aus der `Suppliers` Tabelle. Die zwei Resultsets kann dann aufgelistet werden, und nur die Datensätze, in dem Änderungen vorgenommen wurden, können aktualisiert werden.


## <a name="summary"></a>Zusammenfassung

In diesem Tutorial wurde erläutert, wie einem vollständig bearbeitbare DataList-Steuerelement, sodass Benutzer schnell geändert werden. die Adressinformationen für mehrere Lieferanten zu erstellen. Wir begonnen, indem Sie definieren die Bearbeitungsschnittstelle ein TextBox-Steuerelement für die Lieferanten-e-Adresse, Stadt und Land-Werte im DataList-Steuerelement s `ItemTemplate`. Als Nächstes haben wir alle aktualisieren Schaltflächen oben und unten DataList-Steuerelement hinzugefügt. Nachdem ein Benutzer über seine Änderungen vorgenommen haben, und klicken Sie auf eine der Schaltflächen Alle aktualisieren, die `DataListItem` s aufgelistet und einem Aufruf an die `SuppliersBLL` s-Klasse `UpdateSupplierAddress` Methode erfolgt.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial wurden Zack Jones und Ken Pespisa. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md)
> [Weiter](handling-bll-and-dal-level-exceptions-cs.md)
