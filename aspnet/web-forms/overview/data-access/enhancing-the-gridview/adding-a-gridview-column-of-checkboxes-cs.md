---
uid: web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs
title: Hinzufügen einer GridView-Spalte mit Kontrollkästchen (c#) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial dargestellt, wie ein GridView-Steuerelement, um Benutzern eine intuitive Möglichkeit der Auswählen mehrerer Zeilen mit g eine Spalte mit Kontrollkästchen hinzu...
ms.author: aspnetcontent
ms.date: 03/06/2007
ms.assetid: f63a9443-2db0-4f80-8246-840d3e86c2a3
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs
msc.type: authoredcontent
ms.openlocfilehash: 481ae436ef644bcc4d5a13d060ed87671cfcb4dc
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814780"
---
<a name="adding-a-gridview-column-of-checkboxes-c"></a>Hinzufügen einer GridView-Spalte mit Kontrollkästchen (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunter](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_52_CS.exe) oder [PDF-Datei herunterladen](adding-a-gridview-column-of-checkboxes-cs/_static/datatutorial52cs1.pdf)

> In diesem Tutorial dargestellt, wie eine Spalte mit Kontrollkästchen ein GridView-Steuerelement, um Benutzern eine intuitive Möglichkeit der Auswählen mehrerer Zeilen mit GridView hinzu.


## <a name="introduction"></a>Einführung

Im vorherigen Tutorial untersucht wir, wie Sie eine Spalte mit Optionsfeldern an die GridView für die Auswahl eines bestimmten Datensatzes hinzufügen. Eine Spalte mit Optionsfeldern ist eine geeignete Benutzeroberfläche auf, wenn der Benutzer beschränkt ist, darf höchstens ein Element aus dem Raster auswählen. In einigen Fällen jedoch, sollten wir dem Benutzer ermöglichen, wählen Sie eine beliebige Anzahl von Elementen aus dem Raster. Webbasierte e-Mail-Clients, angezeigt in der Regel z. B. die Liste der Nachrichten mit einer Spalte mit Kontrollkästchen. Der Benutzer eine beliebige Anzahl von Nachrichten aktivieren und dann eine Aktion ausführen, z. B. die e-Mail-Nachrichten in einen anderen Ordner verschieben oder löschen.

In diesem Tutorial sehen wir, wie hinzufügen eine Spalte mit Kontrollkästchen und um zu bestimmen, welche Kontrollkästchen auf Postback eingecheckt wurden. Insbesondere erstellen wir ein Beispiel an, die genau die Benutzeroberfläche des webbasierten e-Mail-Clients imitiert. In unserem Beispiel enthält eine ausgelagerte GridView Auflisten der Produkte in der `Products` Datenbanktabelle mit einem Kontrollkästchen in jeder Zeile (siehe Abbildung 1). Eine ausgewählter Produkte löschen-Schaltfläche, wenn geklickt, werden diese Produkte ausgewählt gelöscht.


[![Jede Zeile des Produkts enthält ein Kontrollkästchen](adding-a-gridview-column-of-checkboxes-cs/_static/image1.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image1.png)

**Abbildung 1**: jede Produktzeile enthält ein Kontrollkästchen ([klicken Sie, um das Bild in voller Größe anzeigen](adding-a-gridview-column-of-checkboxes-cs/_static/image2.png))


## <a name="step-1-adding-a-paged-gridview-that-lists-product-information"></a>Schritt 1: Hinzufügen einer ausgelagerten GridView-Ansicht, die Produktinformationen enthält.

Bevor wir eine Spalte mit Kontrollkästchen hinzufügen fürchten, können Sie zuerst darauf konzentrieren s zum Auflisten der Produkte in einer GridView-Ansicht, die Paging unterstützt. Öffnen Sie zunächst die `CheckBoxField.aspx` auf der Seite die `EnhancedGridView` Ordner, und ziehen Sie einer GridView-Ansicht aus der Toolbox auf den Designer, Festlegen der `ID` zu `Products`. Wählen Sie als Nächstes zum Binden von GridView an eine neue, mit dem Namen "ObjectDataSource" `ProductsDataSource`. Konfigurieren Sie mit dem ObjectDataSource-Steuerelement die `ProductsBLL` Klasse Aufrufen der `GetProducts()` Methode, um die Daten zurückzugeben. Da diese GridView schreibgeschützt ist, legen Sie die Dropdownlisten in der Update-, INSERT-, und Löschen von Registerkarten (keine) aus.


[![Erstellen Sie eine neue, mit dem Namen ProductsDataSource "ObjectDataSource"](adding-a-gridview-column-of-checkboxes-cs/_static/image2.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image3.png)

**Abbildung 2**: Erstellen einer neuen "ObjectDataSource" mit dem Namen `ProductsDataSource` ([klicken Sie, um das Bild in voller Größe anzeigen](adding-a-gridview-column-of-checkboxes-cs/_static/image4.png))


[![Konfigurieren Sie das "ObjectDataSource" zum Abrufen von Daten mithilfe der GetProducts()-Methode](adding-a-gridview-column-of-checkboxes-cs/_static/image3.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image5.png)

**Abbildung 3**: Konfigurieren der "ObjectDataSource" zum Abrufen von Daten mithilfe der `GetProducts()` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](adding-a-gridview-column-of-checkboxes-cs/_static/image6.png))


[![Legen Sie die Dropdownlisten in der Update-, INSERT- und DELETE werden Registerkarten (keine)](adding-a-gridview-column-of-checkboxes-cs/_static/image4.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image7.png)

**Abbildung 4**: Legen Sie die Dropdownlisten in der Update-, INSERT- und Löschen von Registerkarten auf (keine) ([klicken Sie, um das Bild in voller Größe anzeigen](adding-a-gridview-column-of-checkboxes-cs/_static/image8.png))


Visual Studio wird automatisch nach dem Fertigstellen des Assistenten für die Datenquelle konfigurieren BoundColumns und eine CheckBoxColumn für die Felder produktbezogene Daten erstellt. Wie im vorherigen Tutorial weiter, entfernen Sie alle außer den `ProductName`, `CategoryName`, und `UnitPrice` BoundFields, und ändern Sie die `HeaderText` Eigenschaften, die Produkte, Kategorie und Preis. Konfigurieren der `UnitPrice` BoundField, damit der Wert als Währung formatiert ist. Außerdem konfigurieren Sie die GridView zur Unterstützung von Paging durch Aktivieren des Kontrollkästchens Paging aktivieren, aus dem Smarttag.

Lassen Sie s, die auch die Benutzeroberfläche für das Löschen der ausgewählten Produkte hinzufügen. Fügen Sie ein Steuerelement Schaltfläche unterhalb der GridView, Festlegen der `ID` zu `DeleteSelectedProducts` und die zugehörige `Text` Eigenschaft, um die ausgewählten Produkte löschen. Statt tatsächlich Produkte aus der Datenbank zu löschen, in diesem Beispiel zeigen wir einfach eine Meldung, die die Produkte, die gelöscht worden wäre. Um dies zu unterstützen, fügen Sie ein Label-Steuerelement unterhalb der Schaltfläche hinzu. Legen Sie seine ID auf `DeleteResults`, deaktivieren Sie die `Text` -Eigenschaft, und legen dessen `Visible` und `EnableViewState` Eigenschaften `false`.

Nach diesen Änderungen durchführen, sollten die GridView, ObjectDataSource-Steuerelement, Schaltfläche und Bezeichnung s deklarative Markup etwa wie folgt:


[!code-aspx[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample1.aspx)]

Nehmen Sie einen Moment Zeit, um die Seite in einem Browser anzuzeigen (siehe Abbildung 5). An diesem Punkt sollte der Name, Kategorie und Preis der ersten zehn Produkte angezeigt werden.


[![Der Name, Kategorie und der Preis der ersten zehn Produkte werden aufgeführt.](adding-a-gridview-column-of-checkboxes-cs/_static/image5.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image9.png)

**Abbildung 5**: der Name, Kategorie und Preis für die ersten zehn Produkte aufgeführt sind ([klicken Sie, um das Bild in voller Größe anzeigen](adding-a-gridview-column-of-checkboxes-cs/_static/image10.png))


## <a name="step-2-adding-a-column-of-checkboxes"></a>Schritt 2: Hinzufügen einer Spalteninhalts mit Kontrollkästchen

Da ASP.NET 2.0 eine CheckBoxField enthält, eine lässt zunächst vermuten, dass sie zum Hinzufügen einer GridView-Ansicht einer Spalteninhalts mit Kontrollkästchen verwendet werden konnte. Leider ist, die nicht der Fall, wie die CheckBoxField ausgelegt ist ein boolesches Feld. Um die CheckBoxField verwenden müssen wir, also das zugrunde liegende Datenfeld angeben, deren Wert verwendet wird, um festzustellen, ob das gerenderte Kontrollkästchen aktiviert ist. Wir können keine CheckBoxField verwenden, um die nur eine Spalte eines deaktivierten Kontrollkästchen enthalten.

Stattdessen müssen wir ein TemplateField hinzufügen und fügen Sie ein Kontrollkästchen-Steuerelement, dessen `ItemTemplate`. Fahren Sie fort, und fügen Sie ein TemplateField auf die `Products` GridView und legen Sie ihn mit das erste (linken)-Feld. GridView s Smarttags, klicken Sie auf den Link für die Vorlagen bearbeiten und ziehen Sie dann ein Kontrollkästchen-Steuerelement aus der Toolbox in die `ItemTemplate`. Legen Sie dieses Kontrollkästchen s `ID` Eigenschaft `ProductSelector`.


[![Fügen Sie ein Kontrollkästchen-Steuerelement mit dem Namen ProductSelector für ItemTemplate verbleibt TemplateField s hinzu.](adding-a-gridview-column-of-checkboxes-cs/_static/image6.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image11.png)

**Abbildung 6**: Fügen Sie ein Kontrollkästchen Web-Steuerelement mit dem Namen `ProductSelector` der TemplateField s `ItemTemplate` ([klicken Sie, um das Bild in voller Größe anzeigen](adding-a-gridview-column-of-checkboxes-cs/_static/image12.png))


Das TemplateField und Kontrollkästchen-Web-Steuerelement hinzugefügt enthält jede Zeile nun ein Kontrollkästchen. Abbildung 7 zeigt diese Seite, über einen Webbrowser angezeigt werden soll, nachdem das TemplateField und Kontrollkästchen hinzugefügt wurden.


[![Jede Zeile des Produkts enthält nun ein Kontrollkästchen](adding-a-gridview-column-of-checkboxes-cs/_static/image7.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image13.png)

**Abbildung 7**: jede Produktzeile enthält nun ein Kontrollkästchen ([klicken Sie, um das Bild in voller Größe anzeigen](adding-a-gridview-column-of-checkboxes-cs/_static/image14.png))


## <a name="step-3-determining-what-checkboxes-were-checked-on-postback"></a>Schritt 3: Bestimmen welche Kontrollkästchen auf Postback überprüft wurden

An diesem Punkt haben wir eine Spalte mit Kontrollkästchen aber keine Möglichkeit, zu bestimmen, welche Kontrollkästchen auf Postback eingecheckt wurden. Wenn die ausgewählten Produkte löschen-Schaltfläche geklickt wird, müssen wir jedoch wissen, welche Kontrollkästchen aktiviert wurden, um diese Produkte zu löschen.

Das GridView-s [ `Rows` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rows.aspx) ermöglicht den Zugriff auf die Datenzeilen in den GridView-Ansicht. Wir können diese Zeilen durchlaufen programmgesteuerten Zugriff auf das CheckBox-Steuerelement, und klicken Sie dann wenden Sie sich an die `Checked` Eigenschaft, um zu bestimmen, ob das Kontrollkästchen aktiviert ist.

Erstellen Sie einen Ereignishandler für die `DeleteSelectedProducts` Schaltfläche Websteuerelement s `Click` Ereignis und fügen Sie den folgenden Code hinzu:


[!code-csharp[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample2.cs)]

Die `Rows` Eigenschaft gibt eine Auflistung von `GridViewRow` Instanzen dieser Zusammensetzung der GridView-s-Datenzeilen. Die `foreach` hier Schleife zählt dieser Auflistung. Für jede `GridViewRow` Objekt ist, wird die Zeile s Kontrollkästchen erfolgt programmgesteuert mithilfe von `row.FindControl("controlID")`. Wenn das Kontrollkästchen aktiviert ist, die Zeile s entsprechende `ProductID` Wert wird abgerufen, von der `DataKeys` Auflistung. In dieser Übung zeigen wir einfach eine informationsmeldung in die `DeleteResults` bezeichnen, die zwar in einer arbeitsanwendung wir d stattdessen einen Aufruf der `ProductsBLL` s-Klasse `DeleteProduct(productID)` Methode.

Durch das Hinzufügen dieses ereignishandlers, klicken Sie nun auf die ausgewählten Produkte löschen-Schaltfläche zeigt den `ProductID` s der ausgewählten Produkte.


[![Klicken auf die Schaltfläche zum Löschen ausgewählten Produkte werden die ausgewählten Produkte Produkt-IDs aufgeführt.](adding-a-gridview-column-of-checkboxes-cs/_static/image8.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image15.png)

**Abbildung 8**: Wenn die ausgewählten Produkte Schaltfläche zum Löschen von Produkte ausgewählt geklickt wird `ProductID` s werden aufgelistet ([klicken Sie, um das Bild in voller Größe anzeigen](adding-a-gridview-column-of-checkboxes-cs/_static/image16.png))


## <a name="step-4-adding-check-all-and-uncheck-all-buttons"></a>Schritt 4: Alle aktivieren und deaktivieren Sie alle Schaltflächen hinzufügen

Wenn ein Benutzer alle Produkte auf der aktuellen Seite zu löschen möchte, müssen sie jedes der zehn Kontrollkästchen überprüfen. Können wir diesen Prozess zu beschleunigen, durch das Hinzufügen einer alle-Schaltfläche geklickt wird, wählt Sie alle Kontrollkästchen im Raster. Eine Schaltfläche deaktivieren Sie alle wäre gleichermaßen hilfreich.

Fügen Sie zwei Schaltfläche-Web-Steuerelemente auf der Seite, die sie über die GridView zu platzieren. Legen Sie die erste s `ID` zu `CheckAll` und die zugehörige `Text` Eigenschaft, um alle; der Satz das zweite Argument s `ID` zu `UncheckAll` und die zugehörige `Text` Eigenschaft, um alle deaktivieren.


[!code-aspx[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample3.aspx)]

Als Nächstes erstellen Sie eine Methode in der CodeBehind-Klasse, die mit dem Namen `ToggleCheckState(checkState)` , wenn aufgerufen wird, listet die `Products` GridView s `Rows` Auflistung und legt jede Kontrollkästchen s `Checked` -Eigenschaft auf den Wert des übergebenen in *CheckState*  Parameter.


[!code-csharp[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample4.cs)]

Erstellen Sie als Nächstes `Click` -Ereignishandlern für die `CheckAll` und `UncheckAll` Schaltflächen. In `CheckAll` s-Ereignishandler rufen einfach `ToggleCheckState(true)`in `UncheckAll`, rufen Sie `ToggleCheckState(false)`.


[!code-csharp[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample5.cs)]

Mit diesem Code wird ein Postback auslöst und überprüft alle Kontrollkästchen in den GridView-Ansicht auf die Schaltfläche "alle". Ebenso hebt die auf alle deaktivieren Sie alle Kontrollkästchen Auswahl. Abbildung 9 zeigt den Bildschirm, nachdem die Schaltfläche "alle" eingecheckt wurde.


[![Klicken Sie auf die Schaltfläche alle Kontrollkästchen alle Kontrollkästchen ausgewählt](adding-a-gridview-column-of-checkboxes-cs/_static/image9.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image17.png)

**Abbildung 9**: Klicken Sie auf dem Überprüfen aller Schaltfläche wählt alle Kontrollkästchen ([klicken Sie, um das Bild in voller Größe anzeigen](adding-a-gridview-column-of-checkboxes-cs/_static/image18.png))


> [!NOTE]
> Wenn ist die Anzeige einer Spalteninhalts mit Kontrollkästchen, eine Möglichkeit für das Aktivieren oder deaktivieren alle Kontrollkästchen über ein Kontrollkästchen in der Kopfzeile. Darüber hinaus überprüfen Sie alle aktuellen / deaktivieren Sie jegliche Implementierung ein Postback erfordert. Die Kontrollkästchen möglich aktiviert oder deaktiviert, jedoch ausschließlich mit Client-seitige Skript, wodurch einer forschen benutzerfreundlichkeit. Sehen Sie sich, um die Untersuchung mit einem Header-Zeile-Kontrollkästchen für alle aktivieren und deaktivieren Sie alle im Detail, sowie eine Erläuterung zur Verwendung der clientseitigen Techniken, [überprüfen alle Kontrollkästchen in einem GridView mithilfe des clientseitigen Skripts und überprüfen Sie alle Kontrollkästchen](http://aspnet.4guysfromrolla.com/articles/053106-1.aspx).


## <a name="summary"></a>Zusammenfassung

In Fällen, in dem Sie ermöglichen Benutzern das Auswählen von einer beliebigen Anzahl von Zeilen aus einer GridView-Ansicht vor dem fortfahren möchten, ist das Hinzufügen einer Spalteninhalts mit Kontrollkästchen eine Option aus. Wie wir in diesem Tutorial gesehen haben, umfasst das einschließlich eine Spalte mit Kontrollkästchen in den GridView-Ansicht hinzufügen ein TemplateField mit ein Kontrollkästchen-Steuerelement. Mithilfe eines Websteuerelements (im Gegensatz zu injizieren von Markup direkt in die Vorlage wie im vorherigen Tutorial) ASP.NET automatisch speichert, welche Kontrollkästchen wurden und nicht während des Zurücksendens überprüft wurden. Wir können auch programmgesteuert auf die jeweiligen Kontrollkästchen im Code, um festzustellen, ob eine angegebene Kontrollkästchen aktiviert ist, oder So ändern Sie den aktivierten Zustand zugreifen.

In diesem Tutorial und die letzte Lektion erläutert eine Auswahlspalte Zeile an die GridView hinzufügen. In unserem nächsten Tutorial untersuchen wir, mit ein wenig Aufwand, Einfügen von Funktionen der GridView hinzugefügt werden können.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Zurück](adding-a-gridview-column-of-radio-buttons-cs.md)
> [Weiter](inserting-a-new-record-from-the-gridview-s-footer-cs.md)
