---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb
title: Ausführen von BatchUpdates (VB) | Microsoft Docs
author: rick-anderson
description: Informationen zum Erstellen einer vollständig bearbeitbar DataList, in dem sämtliche Elemente sind im, Bearbeitungsmodus, deren Werte gespeichert werden können, indem Sie auf eine Schaltfläche 'Alle aktualisieren' auf der...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: 8dac22a7-91de-4e3b-888f-a4c438b03851
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb
msc.type: authoredcontent
ms.openlocfilehash: 4d28a431c2b09de8c46079e888aa191017de4e30
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="performing-batch-updates-vb"></a>Ausführen von BatchUpdates (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunterladen](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_37_VB.exe) oder [PDF herunterladen](performing-batch-updates-vb/_static/datatutorial37vb1.pdf)

> Informationen zum Erstellen einer vollständig bearbeitbar DataList, in dem sämtliche Elemente sind im, Bearbeitungsmodus, deren Werte gespeichert werden können, indem Sie die Schaltfläche "Alle aktualisieren" auf der Seite.


## <a name="introduction"></a>Einführung

In der [vorherigen Lernprogramm](an-overview-of-editing-and-deleting-data-in-the-datalist-vb.md) untersucht, wie ein DataList auf Elementebene zu erstellen. Wie standard bearbeitbare GridView jedes Element in der DataList enthalten eine Bearbeitung-Schaltfläche geklickt haben, machen würde das Element bearbeitet werden kann. Während dies Elementebene Bearbeitung eignet sich gut für Daten, die nur gelegentlich aktualisiert werden, erfordern bestimmte verwendungsfallbeispielen Benutzer viele Datensätze zu bearbeiten. Wenn ein Benutzer Dutzende von Datensätzen bearbeiten muss und gezwungen, klicken Sie auf Bearbeiten, nehmen ihre Änderungen und Updates für jeweils auf, kann die Menge des Klickens auf ihre Produktivität behindern. In solchen Fällen eine bessere Option darstellt, auf eine vollständig bearbeitbar DataList bieten eine Where *alle* Elemente sind in den Bearbeitungsmodus wechseln und deren Werte bearbeitet werden können, durch ein Update aller Schaltfläche auf der Seite (siehe Abbildung 1).


[![Jedes Element in einer vollständig bearbeitbar DataList kann geändert werden](performing-batch-updates-vb/_static/image2.png)](performing-batch-updates-vb/_static/image1.png)

**Abbildung 1**: jedes Element in einer vollständig bearbeitbar DataList kann geändert werden ([klicken Sie hier, um das Bild in voller Größe angezeigt](performing-batch-updates-vb/_static/image3.png))


In diesem Lernprogramm untersuchen wir zum Aktivieren der Benutzer mit einem vollständig bearbeitbar DataList Lieferanten-Adressinformationen zu aktualisieren.

## <a name="step-1-create-the-editable-user-interface-in-the-datalist-s-itemtemplate"></a>Schritt 1: Erstellen der bearbeitbaren Benutzeroberfläche in DataList s ItemTemplate

In dem vorherigen Lernprogramm, in dem wir erstellen ein standard, Elementebene bearbeitbaren DataList, wir zwei Vorlagen verwendet:

- `ItemTemplate` enthalten, die nur-Lese Benutzeroberfläche (die Bezeichnung-Websteuerelemente zum Anzeigen von jeder s Produktname und Preis).
- `EditItemTemplate` enthalten die Benutzeroberfläche des bearbeiten-Modus (die beiden TextBox Web-Steuerelemente).

DataList-s `EditItemIndex` Eigenschaft bestimmt, welche `DataListItem` (sofern vorhanden) gerendert wird, mithilfe der `EditItemTemplate`. Insbesondere die `DataListItem` , deren `ItemIndex` Wert entspricht die DataList s `EditItemIndex` Eigenschaft wird dargestellt, mit der `EditItemTemplate`. Dieses Modell funktioniert gut, wenn nur ein Element auf eine Zeit, aber auseinander fällt bearbeitet werden kann, für die Erstellung vollständig bearbeitbar DataList.

Für eine vollständig bearbeitbar DataList möchten wir *alle* von der `DataListItem` s zum Rendern mithilfe der Benutzeroberfläche bearbeitet werden. Die einfachste Möglichkeit, dies zu erreichen ist so definieren Sie die bearbeitbare Schnittstelle in der `ItemTemplate`. Zum Ändern der Adressinformationen Lieferanten, enthält die bearbeitbare Schnittstelle die Lieferantenname als Text und klicken Sie dann die Textfelder ein, für die Adresse, Stadt und Land Werte.

Öffnen Sie zunächst die `BatchUpdate.aspx` Seite, fügen Sie einem DataList-Steuerelement hinzu, und legen seine `ID` Eigenschaft `Suppliers`. Aus dem Smarttag DataList s, wahlweise ein neues ObjectDataSource-Steuerelement mit dem Namen hinzuzufügen `SuppliersDataSource`.


[![Erstellen Sie eine neue, mit dem Namen SuppliersDataSource ObjectDataSource](performing-batch-updates-vb/_static/image5.png)](performing-batch-updates-vb/_static/image4.png)

**Abbildung 2**: Erstellen einer neuen ObjectDataSource namens `SuppliersDataSource` ([klicken Sie hier, um das Bild in voller Größe angezeigt](performing-batch-updates-vb/_static/image6.png))


Konfigurieren der ObjectDataSource zum Abrufen von Daten mithilfe der `SuppliersBLL` Klasse s `GetSuppliers()` Methode (siehe Abbildung 3). Wie bei dem vorherigen Lernprogramm, anstatt die Lieferanteninformationen über das ObjectDataSource zu aktualisieren, müssen wir direkt mit der Business Logic Layer zusammenarbeiten. Aus diesem Grund die Dropdown-Liste (keine) auf der Registerkarte "UPDATE" festgelegt (siehe Abbildung 4).


[![Abrufen von Lieferanteninformationen mithilfe der GetSuppliers()-Methode](performing-batch-updates-vb/_static/image8.png)](performing-batch-updates-vb/_static/image7.png)

**Abbildung 3**: Abrufen von Lieferanten Informationen mithilfe der `GetSuppliers()` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](performing-batch-updates-vb/_static/image9.png))


[![Legen Sie das Dropdown-Liste (keine) auf der Registerkarte "UPDATE"](performing-batch-updates-vb/_static/image11.png)](performing-batch-updates-vb/_static/image10.png)

**Abbildung 4**: Legen Sie das Dropdown-Liste (keine) auf der Registerkarte "UPDATE" ([klicken Sie hier, um das Bild in voller Größe angezeigt](performing-batch-updates-vb/_static/image12.png))


Nach Abschluss des Assistenten, generiert Visual Studio automatisch die DataList s `ItemTemplate` zum Anzeigen der einzelnen Datenfelder, die von der Datenquelle in ein Label-Websteuerelement zurückgegeben. Wir müssen diese Vorlage zu ändern, sodass sie stattdessen die Bearbeitung Schnittstelle bereitstellt. Die `ItemTemplate` kann angepasst werden, durch den Designer mit der Option "Vorlagen bearbeiten" aus dem Smarttag DataList s oder direkt über die deklarative Syntax.

Nehmen Sie einen Moment Zeit, um eine Bearbeitung Schnittstelle zu erstellen, die werden der Name des Lieferanten s als Text angezeigt, aber enthält Textfelder für die Lieferanten-s-Adresse, Stadt und Land Werte. Nachdem diese Änderungen vorgenommen wurden, sollte Ihre Seite s deklarative Syntax ähnlich der folgenden aussehen:


[!code-aspx[Main](performing-batch-updates-vb/samples/sample1.aspx)]

> [!NOTE]
> Wie muss mit dem vorherigen Lernprogramm DataList in diesem Lernprogramm seinen Ansichtszustand aktiviert haben.


In der `ItemTemplate` ich m mit zwei neuen CSS-Klassen `SupplierPropertyLabel` und `SupplierPropertyValue`, die hinzugefügt wurden die `Styles.css` Klasse und für die Verwendung der gleichen formateinstellungen als konfiguriert die `ProductPropertyLabel` und `ProductPropertyValue` CSS-Klassen.


[!code-css[Main](performing-batch-updates-vb/samples/sample2.css)]

Finden Sie nachdem diese Änderungen vorgenommen wurden auf dieser Seite über einen Browser. Abbildung 5 jedes DataList-Element zeigt den Lieferantennamen als Text und Textfelder zum Anzeigen von Adresse, Stadt und Land verwendet.


[![Jeder Lieferant in DataList ist bearbeitbar](performing-batch-updates-vb/_static/image14.png)](performing-batch-updates-vb/_static/image13.png)

**Abbildung 5**: jeder Lieferant in DataList ist bearbeitbar ([klicken Sie hier, um das Bild in voller Größe angezeigt](performing-batch-updates-vb/_static/image15.png))


## <a name="step-2-adding-an-update-all-button"></a>Schritt 2: Hinzufügen eines Updates Schaltfläche "alle"

Während der einzelnen Lieferanten in Abbildung 5 seine Adresse Stadt und Landesfelder in einem Textfeld angezeigt hat, ist gibt es derzeit keine Schaltfläche "Aktualisieren" verfügbar. Anstatt eine Schaltfläche "Aktualisieren" pro Artikel mit vollständig bearbeitbar DataList-Steuerelementen unterstützt besteht in der Regel ein einzelnes Optionsfeld an alle aktualisieren auf der Seite, die beim Klicken auf aktualisiert *alle* Datensätze in der DataList. Können Sie für dieses Lernprogramm s Update alle Schaltflächen - am oberen Rand der Seite und im unteren Bereich hinzufügen (obwohl entweder Schaltfläche denselben Effekt hat).

Start durch Hinzufügen einer Schaltfläche Websteuerelement über die DataList, und legen seine `ID` Eigenschaft `UpdateAll1`. Als Nächstes fügen Sie die zweite Schaltfläche Websteuerelement unterhalb der DataList Festlegen seiner `ID` auf `UpdateAll2`. Legen Sie die `Text` Eigenschaften für die beiden Schaltflächen Alle aktualisieren. Erstellen Sie abschließend die Ereignishandler für beide Tasten `Click` Ereignisse. Statt die Update-Logik in jeder der Ereignishandler, Umgestalten Let s Logik auf eine dritte Methode `UpdateAllSupplierAddresses`, müssen die Ereignishandler einfach diese dritte Methode aufrufen.


[!code-vb[Main](performing-batch-updates-vb/samples/sample3.vb)]

Abbildung 6 zeigt die Seite, nachdem die Aktualisierung alle Schaltflächen hinzugefügt wurden.


[![Auf der Seite "wurden zwei Update alle Schaltflächen hinzugefügt.](performing-batch-updates-vb/_static/image17.png)](performing-batch-updates-vb/_static/image16.png)

**Abbildung 6**: wurden zwei Update alle Schaltflächen auf der Seite hinzugefügt ([klicken Sie hier, um das Bild in voller Größe angezeigt](performing-batch-updates-vb/_static/image18.png))


## <a name="step-3-updating-all-of-the-suppliers-address-information"></a>Schritt 3: Aktualisieren aller die Lieferanten-Adressinformationen

Mit allen DataList s-Elemente, die Bearbeitungsoberfläche anzeigen und durch das Hinzufügen der Schaltflächen Alles aktualisieren, bleibt ist das Schreiben des Codes zum Ausführen der Batch-Updates. Insbesondere müssen so durchlaufen Sie das DataList-s-Elemente, und rufen die `SuppliersBLL` Klasse s `UpdateSupplierAddress` für jeden dieser Methode.

Die Auflistung der `DataListItem` Instanzen dieser Zusammensetzung DataList, die über das DataList s möglich [ `Items` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.items.aspx). Mit einem Verweis auf eine `DataListItem`, können wir ziehen Sie das entsprechende `SupplierID` aus der `DataKeys` Auflistung und programmgesteuert Verweis der TextBox-innerhalb Websteuerelemente der `ItemTemplate` wie der folgende Code veranschaulicht:


[!code-vb[Main](performing-batch-updates-vb/samples/sample4.vb)]

Eine der Schaltflächen Alle aktualisieren, klickt der Benutzer die `UpdateAllSupplierAddresses` Methode durchläuft die einzelnen `DataListItem` in der `Suppliers` DataList und ruft die `SuppliersBLL` Klasse s `UpdateSupplierAddress` -Methode auf und übergibt die entsprechenden Werte. Ein nicht eingegeben Wert für Adresse, Ort oder Land übergibt ist ein Wert von `Nothing` auf `UpdateSupplierAddress` (anstatt eine leere Zeichenfolge), was dazu führt, in einer Datenbank `NULL` für die zugrunde liegenden Datensatz s-Felder.

> [!NOTE]
> Als eine Erweiterung dar empfiehlt sich eine Bezeichnung Websteuerelement Status auf der Seite hinzufügen, die einige bestätigungsmeldung bereitstellt, nachdem die Batchaktualisierung durchgeführt wird.


## <a name="updating-only-those-addresses-that-have-been-modified"></a>Aktualisieren nur die Adressen, die geändert wurden.

Der Batch-Update-Algorithmus für dieses Lernprogramm ruft verwendet die `UpdateSupplierAddress` Methode zum *jeder* Lieferanten in DataList, unabhängig davon, ob ihre Adressinformationen geändert wurde. Während solche Blind sind t normalerweise ein Leistungsproblem aktualisiert, können sie zu überflüssig Datensätze führen, wenn Sie die Überwachung der Datenbanktabelle ändert. Angenommen, Sie Trigger verwenden, um alle aufzuzeichnen `UPDATE` s, um die `Suppliers` Tabelle in einer Überwachung, jedes Mal, wenn ein Benutzer die alle Aktualisieren Schaltfläche klickt, ein neuen Überwachungsdatensatz für jeden Lieferanten werden, im System erstellt wird, unabhängig davon, ob der Benutzer eine gemacht, ändert.

Die ADO.NET DataTable und DataAdapter-Klassen dienen zur Unterstützung von BatchUpdates, in denen nur geändert, gelöscht und neue Datensätze führt Kommunikation mit der Datenbank. Jede Zeile in der "DataTable" verfügt über eine [ `RowState` Eigenschaft](https://msdn.microsoft.com/library/system.data.datarow.rowstate.aspx) , der angibt, ob die Zeile wurde hinzugefügt, zu der "DataTable" aus, geändert, gelöscht oder unverändert bleibt. Bei eine "DataTable" anfänglich aufgefüllt wird, werden alle Zeilen nicht geändert markiert. Ändern des Werts auf die Zeile s-Spalten markiert die Zeile geändert wurde.

In der `SuppliersBLL` Klasse, die wir die Adressinformationen angegebenen Lieferanten s, indem zuerst lesen in den einzelnen Lieferanten-Datensatz in aktualisieren einem `SuppliersDataTable` und legen Sie dann die `Address`, `City`, und `Country` Spaltenwerte, die mit dem folgenden Code:


[!code-vb[Main](performing-batch-updates-vb/samples/sample5.vb)]

Dieser Code weist naiv die übergebene Adresse Stadt und Land Werte der `SuppliersRow` in der `SuppliersDataTable` unabhängig davon, ob die Werte geändert haben. Diese Änderungen dazu führen, dass die `SuppliersRow` s `RowState` Eigenschaft als geändert markiert werden. Wenn die Datenzugriffsebene s `Update` -Methode aufgerufen wird, die er erkennt, dass die `SupplierRow` geändert wurde und daher sendet eine `UPDATE` Befehl mit der Datenbank.

Stellen Sie sich vor, allerdings, dass wir Code, damit diese Methode nur durch die übergebene Adresse Stadt und Land Werte zugewiesen wird hinzugefügt, wenn sie die Unterschiede zu den `SuppliersRow` s vorhandene Werte. Im Fall, in dem die Adresse, Stadt und Land identisch mit den vorhandenen Daten sind, keine Änderungen vorgenommen werden und die `SupplierRow` s `RowState` links als unverändert. Das Ergebnis ist, wenn der DAL-s `Update` -Methode aufgerufen wird, wird keine Datenbankaufruf vorgenommen werden, da die `SuppliersRow` wurde nicht geändert.

Um diese Änderung anwenden, ersetzen Sie die Anweisungen, die blind weisen Sie die übergebene Adresse Stadt und Land Werte durch den folgenden Code:


[!code-vb[Main](performing-batch-updates-vb/samples/sample6.vb)]

Mit diesem Code hinzugefügt, die DAL s `Update` -Methode sendet eine `UPDATE` Anweisung an die Datenbank für nur Datensätze, deren Werte adressbezogenen geändert haben.

Alternativ können Sie wir konnten zu verfolgen, ob Unterschiede zwischen den Adressfeldern übergebene und Daten in der Datenbank vorhanden sind und, wenn keine vorhanden sind, umgehen Sie einfach den Aufruf der DAL-s `Update` Methode. Dieser Ansatz funktioniert gut, wenn Sie erneut mit dem DB-Methode, weiterleiten, da DB direkte Methode t übergeben einer `SuppliersRow` Instanz, deren `RowState` überprüft werden können, um zu bestimmen, ob ein Datenbankaufruf tatsächlich benötigt wird.

> [!NOTE]
> Jedes Mal die `UpdateSupplierAddress` Methode aufgerufen wird, wird ein Aufruf zum Abrufen von Informationen zu den aktualisierten Datensatz in der Datenbank ausgelöst. Klicken Sie dann, wenn alle Änderungen in den Daten vorhanden sind, einen weiteren Aufruf an die Datenbank erfolgt auf die Tabellenzeile zu aktualisieren. Dieser Workflow kann optimiert werden, durch das Erstellen einer `UpdateSupplierAddress` methodenüberladung, die akzeptiert ein `EmployeesDataTable` -Zielinstanz mit *alle* der Änderungen aus der `BatchUpdate.aspx` Seite. Es konnte stellen Sie dann die Datenbank zum Abrufen aller Datensätze aus zwei Aufrufen der `Suppliers` Tabelle. Die zwei Resultsets konnte dann aufgelistet werden, und nur die Datensätze, in denen Änderungen aufgetreten sind, aktualisiert werden konnte.


## <a name="summary"></a>Zusammenfassung

In diesem Lernprogramm wurde erläutert, wie zum Erstellen einer vollständig bearbeitbar DataList, dadurch kann einen Benutzer die Adressinformationen für mehrere Lieferanten schnell zu ändern. Wir beginnen, durch die Definition die Bearbeitungsoberfläche ein TextBox-Websteuerelement für die Lieferanten-s-Adresse, Stadt und Land Werte in der DataList s `ItemTemplate`. Als Nächstes haben wir alle aktualisieren Schaltflächen oberhalb und unterhalb der DataList hinzugefügt. Nachdem ein Benutzer seine Änderungen vorgenommen und eine der Update alle Schaltflächen geklickt hat die `DataListItem` s werden aufgezählt und einem Aufruf an die `SuppliersBLL` Klasse s `UpdateSupplierAddress` Methode erfolgt.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurden Zack Jones und Ken Pespisa. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](an-overview-of-editing-and-deleting-data-in-the-datalist-vb.md)
> [Weiter](handling-bll-and-dal-level-exceptions-vb.md)
