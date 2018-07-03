---
uid: web-forms/overview/data-access/enhancing-the-gridview/inserting-a-new-record-from-the-gridview-s-footer-vb
title: Einfügen eines neuen Datensatzes in den GridView Fuß (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Das GridView-Steuerelement bietet zwar keine integrierten Unterstützung für das Einfügen eines neuen Datensatzes von Daten, handelt es sich bei diesem Lernprogramm wird erläutert, die zum Verbessern der GridView sollen eine...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/06/2007
ms.topic: article
ms.assetid: 528acc48-f20c-4b4e-aa16-4cc02f068ebb
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/inserting-a-new-record-from-the-gridview-s-footer-vb
msc.type: authoredcontent
ms.openlocfilehash: c228128e551f58aa003af10cf787875d26b1fab7
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37375174"
---
<a name="inserting-a-new-record-from-the-gridviews-footer-vb"></a>Einfügen eines neuen Datensatzes in den GridView Fuß (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunter](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_53_VB.exe) oder [PDF-Datei herunterladen](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/datatutorial53vb1.pdf)

> Das GridView-Steuerelement bietet zwar keine integrierten Unterstützung für das Einfügen eines neuen Datensatzes von Daten, veranschaulicht dieses Tutorial verbessern die GridView, um eine Schnittstelle einfügen von aufzunehmen.


## <a name="introduction"></a>Einführung

Siehe die [eine Übersicht der einfügen, aktualisieren und Löschen von Daten](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) Tutorial, mit den Steuerelementen GridView, DetailsView und FormView-Web gehören integrierte Möglichkeiten zur Datenänderung. Bei Verwendung mit der deklarativen Datenquellen-Steuerelemente dieser drei Websteuerelemente können schnell und einfach konfiguriert werden zum Ändern von Daten per Push – sowie in Szenarien ohne eine einzige Zeile Code schreiben. Leider bereit nur die DetailsView und FormView-Steuerelemente, integrierte einfügen, bearbeiten und Löschen von Funktionen. Das GridView bietet nur bearbeiten und Löschen von unterstützt. Allerdings können wir mit etwas Scheitelpunkt fett, GridView um eine Schnittstelle einfügen von aufzunehmen erweitern.

In der GridView Einfügen von Funktionen hinzugefügt haben, können wir dafür entscheiden, wie neue Datensätze hinzugefügt werden, erstellen die einfügende-Schnittstelle und den Code zum Einfügen des neuen Datensatzes zu schreiben. Die Zeile in diesem Lernprogramm aus, die wir uns wird, an die GridView-Fuß s die einfügende-Schnittstelle hinzugefügt (siehe Abbildung 1). Die Zelle für jede Spalte enthält die entsprechenden Daten Auflistung Benutzeroberflächenelement (ein Textfeld für den Produktnamen s, einer DropDownList für Lieferanten und So weiter). Wir benötigen auch eine Spalte ein "Add"-Schaltfläche geklickt haben, wird ein Postback auslösen, und fügen Sie einen neuen Datensatz in die `Products` Tabelle mit den Werten in der Fußzeile angegeben.


[![Die Footerzeile stellt eine Schnittstelle für das Hinzufügen neuer Produkte](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image1.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image1.png)

**Abbildung 1**: die Footerzeile stellt eine Schnittstelle für neue Produkte hinzufügen ([klicken Sie, um das Bild in voller Größe anzeigen](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image2.png))


## <a name="step-1-displaying-product-information-in-a-gridview"></a>Schritt 1: Anzeigen von Produktinformationen in einer GridView-Ansicht

Bevor wir uns darum kümmern, die einfügende-Schnittstelle in der GridView-Fuß s erstellen, können Sie zuerst darauf konzentrieren s zum Hinzufügen einer GridView-Ansicht auf der Seite, die die Produkte in der Datenbank aufgelistet werden. Öffnen Sie zunächst die `InsertThroughFooter.aspx` auf der Seite die `EnhancedGridView` Ordner, und ziehen Sie einer GridView-Ansicht aus der Toolbox auf den Designer, Festlegen der GridView-s `ID` Eigenschaft `Products`. Als Nächstes das GridView-s-Smarttag verwenden, um die Bindung an eine neue, mit dem Namen "ObjectDataSource" `ProductsDataSource`.


[![Erstellen Sie eine neue, mit dem Namen ProductsDataSource "ObjectDataSource"](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image2.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image3.png)

**Abbildung 2**: Erstellen einer neuen "ObjectDataSource" mit dem Namen `ProductsDataSource` ([klicken Sie, um das Bild in voller Größe anzeigen](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image4.png))


Konfigurieren Sie mit dem ObjectDataSource-Steuerelement die `ProductsBLL` Klasse s `GetProducts()` Methode, um Produktinformationen abzurufen. Für dieses Tutorial s Fokus ausschließlich auf das Einfügen von Funktionen hinzufügen können, und sich keine Gedanken über bearbeiten und löschen. Stellen Sie daher sicher, dass die Dropdown-Liste in der Registerkarte "Einfügen", um festgelegt ist `AddProduct()` und die Dropdownlisten auf den Registerkarten Update- und DELETE auf (keine) festgelegt sind.


[![Zuordnung der AddProduct-Methode, mit der "ObjectDataSource"-s-Insert()-Methode](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image3.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image5.png)

**Abbildung 3**: Zuordnung der `AddProduct` Methode, um das "ObjectDataSource"-s `Insert()` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image6.png))


[![Legen Sie das UPDATE und DELETE-Registerkarten in den Dropdownlisten auf (keine)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image4.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image7.png)

**Abbildung 4**: Aktualisieren und Löschen von Registerkarten Dropdownlisten auf (keine) festgelegt ([klicken Sie, um das Bild in voller Größe anzeigen](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image8.png))


Für jede von den entsprechenden Datenfeldern wird Visual Studio nach Abschluss der Konfigurieren von Datenquellen-Assistenten von "ObjectDataSource" s Felder automatisch an die GridView hinzufügen. Lassen Sie alle Felder, die von Visual Studio hinzugefügt. Später in diesem Tutorial werden wir zurückkehren und entfernen, müssen einige der Felder, deren Werte keine Nachteile für Ihr, angegeben werden, wenn Sie einen neuen Datensatz hinzufügen.

Da in der Nähe 80 Produkte in der Datenbank vorhanden sind, muss ein Benutzer bis hin zum unteren Rand der Webseite zu scrollen, um einen neuen Datensatz hinzufügen. Aus diesem Grund können Sie s Aktivieren von Paging, die die einfügende Schnittstelle besser sichtbar und zugreifbar machen. Aktivieren Sie das Kontrollkästchen "Paging aktivieren" aus dem GridView-s-Smarttag einfach, um zum Paging aktivieren.

An diesem Punkt sollte die GridView und "ObjectDataSource" s deklarative Markup etwa wie folgt aussehen:


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample1.aspx)]


[![Alle Felder der Product-Daten werden in einem ausgelagerten GridView angezeigt.](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image5.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image9.png)

**Abbildung 5**: alle Felder der Product-Daten in einem ausgelagerten GridView angezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image10.png))


## <a name="step-2-adding-a-footer-row"></a>Schritt 2: Hinzufügen einer Fußzeile

Zusammen mit den Header als auch die Datenzeilen enthält die GridView eine Fußzeile. Die Kopf- und Fußzeile Zeilen werden angezeigt, abhängig von den Werten der GridView Zuordnungsvorgänge [ `ShowHeader` ](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.gridview.showheader.aspx) und [ `ShowFooter` ](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.gridview.showfooter.aspx) Eigenschaften. Um die Footerzeile anzuzeigen, legen Sie einfach die `ShowFooter` Eigenschaft `True`. Wie in Abbildung 6 gezeigt, Festlegen der `ShowFooter` Eigenschaft `True` dem Raster eine Fußzeile hinzugefügt.


[![Zum Anzeigen der Fußzeile ShowFooter auf "true" festgelegt](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image6.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image11.png)

**Abbildung 6**: Legen Sie zum Anzeigen der Fußzeile `ShowFooter` zu `True` ([klicken Sie, um das Bild in voller Größe anzeigen](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image12.png))


Beachten Sie, dass die Footerzeile eine dunkelroten Hintergrund-Farbe. Dies liegt an der DataWebControls Design wurde erstellt und die Anwendung auf allen Seiten in der [Anzeigen von Daten mit dem ObjectDataSource-Steuerelement](../basic-reporting/displaying-data-with-the-objectdatasource-vb.md) Tutorial. Insbesondere die `GridView.skin` Datei konfiguriert die `FooterStyle` Eigenschaft solche, die verwendet wird die `FooterStyle` CSS-Klasse. Die `FooterStyle` Klasse ist in definiert `Styles.css` wie folgt:


[!code-css[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample2.css)]

> [!NOTE]
> Wir haben mit der GridView-s-Fußzeile in vorherigen Tutorials untersucht. Bei Bedarf verweisen zurück auf die [Anzeigen von Zusammenfassungsinformationen im GridView Fuß](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb.md) Tutorial zu auffrischen.


Nach dem Festlegen der `ShowFooter` Eigenschaft `True`, können Sie die Ausgabe in einem Browser anzuzeigen. Derzeit die Fußzeile Zeile t einem beliebigen Text- oder Websteuerelemente enthalten. In Schritt 3 ändern wir die Fußzeile für jedes Feld GridView, damit sie die entsprechende Einfügen von Schnittstelle enthält.


[![Der leere Fußzeile wird angezeigt, über das Paging Steuerelemente der Benutzeroberfläche](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image7.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image13.png)

**Abbildung 7**: die leere Fußzeile wird angezeigt, über das Paging Steuerelemente der Benutzeroberfläche ([klicken Sie, um das Bild in voller Größe anzeigen](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image14.png))


## <a name="step-3-customizing-the-footer-row"></a>Schritt 3: Anpassen der Fußzeile

In der [Verwenden von TemplateFields, im GridView-Steuerelement](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) Tutorial wurde erläutert, wie die Anzeige von einer bestimmten GridView-Spalte, verwenden von TemplateFields (im Gegensatz zu BoundFields oder CheckBoxFields) anpassen, in [ Anpassen der Benutzeroberfläche für die Änderung der Daten](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) wir haben uns über das Verwenden von TemplateFields die Bearbeitungsschnittstelle in einer GridView-Ansicht anpassen. Beachten Sie, dass ein TemplateField eine Reihe von Vorlagen besteht, die die Kombination aus Markup, Websteuerelemente und Databinding-Syntax definiert, wird für bestimmte Arten von Zeilen verwendet. Die `ItemTemplate`, gibt beispielsweise die Vorlage für nur-Lese Zeilen, während die `EditItemTemplate` definiert die Vorlage für die Zeile bearbeitet werden.

Zusammen mit den `ItemTemplate` und `EditItemTemplate`, das TemplateField enthält auch eine `FooterTemplate` , die den Inhalt für die Footerzeile angibt. Wir können daher benötigt für jedes Feld s einfügen Schnittstelle für Websteuerelemente hinzufügen der `FooterTemplate`. Um zu starten, konvertieren Sie alle Felder in den GridView-Ansicht in von TemplateFields aus. Dies kann erfolgen durch Klicken auf den Link "Spalten bearbeiten" im GridView s Smarttag, wählen jedes Feld in der unteren linken Ecke, und klicken auf das konvertierte dieses Feld in ein TemplateField-Link.


![Jedes Feld in ein TemplateField konvertieren](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image8.gif)

**Abbildung 8**: jedes Feld in ein TemplateField konvertieren


Klicken Sie auf das konvertierte wird dieses Feld in ein TemplateField den aktuellen Feldtyp in ein TemplateField entspricht. Beispielsweise jede BoundField wird ersetzt durch ein TemplateField mit einer `ItemTemplate` , enthält eine Bezeichnung, die das entsprechenden Datenfeld anzeigt und ein `EditItemTemplate` , die das Datenfeld in einem Textfeld anzeigt. Die `ProductName` BoundField wurde in der folgenden TemplateField-Markup konvertiert:


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample3.aspx)]

Ebenso die `Discontinued` CheckBoxField konvertiert wurde in ein TemplateField, deren `ItemTemplate` und `EditItemTemplate` ein Kontrollkästchen-Steuerelement enthalten (mit der `ItemTemplate` s Kontrollkästchen deaktiviert). Die schreibgeschützte `ProductID` BoundField in ein TemplateField mit sowohl ein Label-Steuerelement umgewandelt wurde die `ItemTemplate` und `EditItemTemplate`. Kurz gesagt, ist das Konvertieren einer vorhandenen GridView-Feld in ein TemplateField eine schnelle und einfache Möglichkeit, die besser anpassbare TemplateField wechseln, ohne das vorhandene Feld s Funktionalität verloren aus.

Seit der GridView wir erneut die Arbeit mit Unterstützung für t bearbeiten, können Sie entfernen die `EditItemTemplate` aus jeder TemplateField, verlassen Sie einfach die `ItemTemplate`. Nach auf diese Weise sollte die GridView s deklarative Markup wie folgt aussehen:


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample4.aspx)]

Nun, da jedes GridView-Feld in ein TemplateField konvertiert wurde, können wir die entsprechende Einfügen von Schnittstelle in jedes Feld s eingeben `FooterTemplate`. Einige der Felder müssen keiner einfügen-Schnittstelle (`ProductID`, z. B.); andere variieren in den Websteuerelementen verwendet, um die neuen Produkt s Informationen sammeln.

Um die Bearbeitungsschnittstelle zu erstellen, wählen Sie den Vorlagen bearbeiten-Link aus dem GridView-s-Smarttag. Wählen Sie dann aus der Dropdown-Liste das entsprechende Feld s `FooterTemplate` und das entsprechende Steuerelement aus der Toolbox in den Designer ziehen.


[![Fügen Sie die entsprechende einfügende-Schnittstelle auf jedes Feld s FooterTemplate](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image9.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image15.png)

**Abbildung 9**: Fügen Sie die entsprechende einfügen-Schnittstelle auf jedes Feld s `FooterTemplate` ([klicken Sie, um das Bild in voller Größe anzeigen](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image16.png))


Der folgenden Liste aufgeführt sind die GridView-Felder, die die einfügende Schnittstelle hinzufügen angeben:

- `ProductID` "None".
- `ProductName` Fügen Sie ein Textfeld hinzu, und legen Sie dessen `ID` zu `NewProductName`. Fügen Sie auch ein RequiredFieldValidator-Steuerelement hinzu, um sicherzustellen, dass der Benutzer einen Wert für den neuen Produktnamen s eingibt.
- `SupplierID` "None".
- `CategoryID` "None".
- `QuantityPerUnit` Fügen Sie ein Textfeld, das Festlegen der `ID` zu `NewQuantityPerUnit`.
- `UnitPrice` Fügen Sie ein Textfeld mit dem Namen `NewUnitPrice` und ein CompareValidator-Steuerelement, das den eingegebene Wert wird sichergestellt ist, einen Währungswert größer als oder gleich 0 (null).
- `UnitsInStock` Verwenden Sie ein Textfeld, dessen `ID` nastaven NA hodnotu `NewUnitsInStock`. Enthalten Sie ein CompareValidator-Steuerelement, das sicherstellt, dass der eingegebene Wert ein ganzzahliger Wert größer als oder gleich 0 (null) ist.
- `UnitsOnOrder` Verwenden Sie ein Textfeld, dessen `ID` nastaven NA hodnotu `NewUnitsOnOrder`. Enthalten Sie ein CompareValidator-Steuerelement, das sicherstellt, dass der eingegebene Wert ein ganzzahliger Wert größer als oder gleich 0 (null) ist.
- `ReorderLevel` Verwenden Sie ein Textfeld, dessen `ID` nastaven NA hodnotu `NewReorderLevel`. Enthalten Sie ein CompareValidator-Steuerelement, das sicherstellt, dass der eingegebene Wert ein ganzzahliger Wert größer als oder gleich 0 (null) ist.
- `Discontinued` Fügen Sie ein Kontrollkästchen, das Festlegen der `ID` zu `NewDiscontinued`.
- `CategoryName` Fügen Sie einem DropDownList-Steuerelement hinzu, und legen Sie dessen `ID` zu `NewCategoryID`. Binden Sie es an eine neue, mit dem Namen "ObjectDataSource" `CategoriesDataSource` und konfigurieren Sie ihn zur Verwendung der `CategoriesBLL` Klasse s `GetCategories()` Methode. Die DropDownList-Zuordnungsvorgänge haben `ListItem` s Anzeige der `CategoryName` Daten Feld mit der `CategoryID` Feld "Daten" als Werte.
- `SupplierName` Fügen Sie einem DropDownList-Steuerelement hinzu, und legen Sie dessen `ID` zu `NewSupplierID`. Binden Sie es an eine neue, mit dem Namen "ObjectDataSource" `SuppliersDataSource` und konfigurieren Sie ihn zur Verwendung der `SuppliersBLL` Klasse s `GetSuppliers()` Methode. Die DropDownList-Zuordnungsvorgänge haben `ListItem` s Anzeige der `CompanyName` Daten Feld mit der `SupplierID` Feld "Daten" als Werte.

Für jedes der Steuerelemente zur gültigkeitsprüfung, lösche die `ForeColor` Eigenschaft, damit die `FooterStyle` weißen Vordergrundfarbe für CSS-s-Klasse anstelle des standardmäßigen roten verwendet werden. Auch verwenden, die `ErrorMessage` -Eigenschaft für eine ausführliche Beschreibung, legen Sie jedoch die `Text` Eigenschaft auf ein Sternchen. Um zu verhindern, dass die Überprüfung Steuerelementtext s verursacht die einfügende-Schnittstelle auf zwei Zeilen umbrochen, legen Sie die `FooterStyle` s `Wrap` Eigenschaft auf "false" für jede der `FooterTemplate` s, die einem Validierungssteuerelement verwenden. Fügen Sie abschließend ein ValidationSummary-Steuerelement unterhalb der GridView und legen seine `ShowMessageBox` Eigenschaft `True` und die zugehörige `ShowSummary` Eigenschaft `False`.

Wenn Sie ein neues Produkt hinzufügen möchten, müssen wir geben die `CategoryID` und `SupplierID`. Diese Informationen werden erfasst, über das DropDownList-Steuerelementen in der Fußzeilenzellen für die `CategoryName` und `SupplierName` Felder. Ich habe mich entschieden, diese Felder im Gegensatz zu den `CategoryID` und `SupplierID` von TemplateFields, da im Raster s den Benutzer Datenzeilen wahrscheinlich mehr interessiert die Namen der Kategorie und Lieferanten statt von ihren ID-Werten ist. Da die `CategoryID` und `SupplierID` Werte werden jetzt erfasst wird, der `CategoryName` und `SupplierName` Feld s Einfügen von Schnittstellen, die wir entfernen das `CategoryID` und `SupplierID` von TemplateFields aus der GridView.

Auf ähnliche Weise die `ProductID` wird nicht verwendet werden, wenn Sie ein neues Produkt hinzufügen daher `ProductID` TemplateField ebenfalls entfernt werden kann. Jedoch lassen, s, lassen Sie die `ProductID` Feld im Raster. Neben den Textfeldern, DropDownList-Steuerelementen, Kontrollkästchen und Steuerelementen zur gültigkeitsprüfung, die die einfügende Schnittstelle bilden, benötigen wir auch ein "Add"-Schaltfläche geklickt wird, führt die Logik, um das neue Produkt mit der Datenbank hinzuzufügen. In Schritt 4 nehmen wir eine Schaltfläche "hinzufügen" die einfügen-Schnittstelle in der `ProductID` TemplateField s `FooterTemplate`.

Die Darstellung der verschiedenen Felder GridView verbessern können. Beispielsweise sollten Sie zum Formatieren der `UnitPrice` Werte als Währung, rechtsbündig ausgerichtet der `UnitsInStock`, `UnitsOnOrder`, und `ReorderLevel` Felder, und Aktualisieren der `HeaderText` Werte für die von TemplateFields.

Nach dem Erstellen der Menge nach dem Einfügen von Schnittstellen in den `FooterTemplate` s, entfernen die `SupplierID`, und `CategoryID` von TemplateFields, und verbessern die Ästhetik des Rasters über die Formatierung und die Abstimmung der von TemplateFields, die deklarative GridView-s Markup sollte in etwa wie folgt aussehen:


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample5.aspx)]

Wenn Sie über einen Browser angezeigt wird, der GridView-s-Fußzeile enthält jetzt die abgeschlossene einfügen Schnittstelle (siehe Abbildung 10). An diesem Punkt das Einfügen von Schnittstelle t ein Mittel für den Benutzer an, dass die Daten für das neue Produkt eingegeben und einen neuen Datensatz in die Datenbank einfügen möchte, Julie enthalten. Darüber hinaus wir haben noch zu behandeln, wie in der Fußzeile eingegebenen Daten in einen neuen Eintrag in übersetzt werden die `Products` Datenbank. In Schritt 4, die wir auf eine Schaltfläche "hinzufügen", auf die einfügen-Schnittstelle eingeschlossen und zum Ausführen von Code auf postback, wenn es s geklickt haben. Schritt 5 zeigt, wie Sie einen neuen Datensatz mit den Daten aus der Fußzeile einzufügen.


[![Das GridView-Fuß stellt eine Schnittstelle für das Hinzufügen eines neuen Datensatzes bereit.](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image10.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image17.png)

**Abbildung 10**: der GridView-Fuß stellt eine Schnittstelle für das Hinzufügen eines neuen Datensatzes ([klicken Sie, um das Bild in voller Größe anzeigen](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image18.png))


## <a name="step-4-including-an-add-button-in-the-inserting-interface"></a>Schritt 4: Einschließlich einer Schaltfläche "hinzufügen" in der einfügen-Schnittstelle

Wir müssen eine Schaltfläche "hinzufügen" an einer beliebigen Stelle in der einfügen-Schnittstelle, da die Fußzeile Zeile s-Schnittstelle derzeit einfügen verfügt nicht über die Mittel für den Benutzer aus, um anzugeben, dass sie die neuen Produkt s-Informationen eingegeben haben. Dies könnte in eine der vorhandenen eingefügt `FooterTemplate` s oder wir können eine neue Spalte hinzufügen zum Raster für diesen Zweck. In diesem Tutorial können s platzieren Sie die Schaltfläche "hinzufügen" die `ProductID` TemplateField s `FooterTemplate`.

Aus dem Designer, klicken Sie auf den Link Vorlagen bearbeiten, in das GridView-s-Smarttag, und wählen Sie dann die `ProductID` Feld s `FooterTemplate` aus der Dropdown-Liste. Hinzufügen einer Schaltfläche-Websteuerelement (oder ein LinkButton oder ImageButton, falls gewünscht) für die Vorlage festlegen seiner ID auf `AddProduct`, dessen `CommandName` zum Einfügen, und die zugehörige `Text` Eigenschaft hinzufügen, wie in Abbildung 11 dargestellt.


[![Platzieren Sie die Schaltfläche "hinzufügen" in "ProductID" TemplateField s FooterTemplate](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image11.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image19.png)

**Abbildung 11**: platzieren Sie die Schaltfläche "hinzufügen" in der `ProductID` TemplateField s `FooterTemplate` ([klicken Sie, um das Bild in voller Größe anzeigen](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image20.png))


Nachdem Sie speichern die Schaltfläche "hinzufügen" enthalten, testen Sie Sie auf der Seite in einem Browser. Beachten Sie, dass beim Klicken auf die Schaltfläche "hinzufügen" mit ungültigen Daten, die die einfügende-Benutzeroberfläche, die das Postback kurz circuited ist, und die ValidationSummary-Steuerelement gibt an, die ungültigen Daten (siehe Abbildung 12). Mit der entsprechenden Daten eingegeben haben auslöst ein Postback klicken Sie auf die Schaltfläche "hinzufügen". Jedoch wird kein Datensatz in der Datenbank hinzugefügt. Wir benötigen, schreiben ein paar Codezeilen tatsächlich die Einfügung ausführen.


[![Die Schaltfläche "hinzufügen"-s ist Postback kurze Circuited bei ungültige Daten in der Schnittstelle einfügen](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image12.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image21.png)

**Abbildung 12**: die Schaltfläche "hinzufügen"-s-Postback ist kurz Circuited, wenn ungültige Daten vorhanden, in der einfügen-Schnittstelle sind ([klicken Sie, um das Bild in voller Größe anzeigen](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image22.png))


> [!NOTE]
> Die Validierungssteuerelemente in die einfügende-Schnittstelle wurden Validierungsgruppe nicht zugewiesen werden. Dies funktioniert gut, solange die einfügende-Schnittstelle die einzige Gruppe von Steuerelementen zur gültigkeitsprüfung auf der Seite ist. Wenn es gibt jedoch andere Validierungssteuerelemente auf der Seite (z. B. der Validierungssteuerelemente in der Bearbeitung Raster s-Schnittstelle), die Validierungssteuerelemente in der einfügen-Schnittstelle und fügen Sie die Schaltfläche "-s `ValidationGroup` Eigenschaften den gleichen Wert zugewiesen werden dass Ordnen Sie diese Steuerelemente mit einer bestimmten Validierungsgruppe an. Finden Sie unter [Analyse der Validierungssteuerelemente in ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx) für Weitere Informationen zur Partitionierung der Steuerelemente zur gültigkeitsprüfung sowie -Schaltflächen auf einer Seite in Gruppen von Überprüfung.


## <a name="step-5-inserting-a-new-record-into-theproductstable"></a>Schritt 5: Einfügen eines neuen Datensatzes in die`Products`Tabelle

Bei der Nutzung der integrierten Bearbeitungsfunktionen der GridView, behandelt die GridView automatisch alle notwendigen Aufgaben für die Durchführung der Aktualisierung. Insbesondere wenn auf die Schaltfläche "Aktualisieren" geklickt, wird sie kopiert die eingegebenen Werte von der Bearbeitung-Schnittstelle an die Parameter in der "ObjectDataSource"-s `UpdateParameters` Sammlung und die definierten sicherheitsschwellwerte deaktiviert das Update durch den Aufruf von "ObjectDataSource" s `Update()` Methode. Da diese integrierten Funktionen für das Einfügen von GridView nicht bereitstellt, müssen wir Code, der das "ObjectDataSource"-s aufruft implementieren `Insert()` -Methode und kopiert die Werte aus den Einfügen-der "ObjectDataSource"-s Schnittstelle `InsertParameters` Auflistung .

Diese Logik einfügen sollten ausgeführt werden, nachdem auf die Schaltfläche "Add" geklickt wurde. Siehe die [hinzufügen und reagieren auf Schaltflächen in einer GridView-Ansicht](../custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-vb.md) jedes Mal, wenn eine Schaltfläche, LinkButton oder ImageButton in einer GridView-Ansicht geklickt wird, das GridView-s-Lernprogramm `RowCommand` beim Postback-Ereignis ausgelöst. Dieses Ereignis wird ausgelöst, ob die Schaltfläche, LinkButton oder ImageButton explizit wie z. B. die Schaltfläche "hinzufügen" in der Fußzeile hinzugefügt wurde, oder wenn es automatisch von der GridView hinzugefügt wurde (z. B. die LinkButtons am oberen Rand der einzelnen Spalten beim Sortieren aktivieren ausgewählt ist, oder LinkButtons in den Paging-Schnittstelle, wenn Paging aktivieren ausgewählt wurde).

Aus diesem Grund, um auf der Benutzer auf die Schaltfläche "hinzufügen" reagieren, müssen wir einen Ereignishandler für das GridView-s erstellen `RowCommand` Ereignis. Da jedes Mal, wenn dieses Ereignis wird ausgelöst, *alle* Button, LinkButton oder ImageButton in den GridView-Ansicht geklickt wird, es wichtig, dass wir nur das Einfügen von Logik Wenn fortsetzen s der `CommandName` Eigenschaft, die in den Ereignis-Handler Code Maps an die übergeben`CommandName` Wert der Schaltfläche "hinzufügen" (Einfügen). Darüber hinaus sollten wir auch nur fortgesetzt, wenn die Validierungssteuerelemente gültige Daten melden. Um dies zu unterstützen, erstellen Sie einen Ereignishandler für die `RowCommand` -Ereignis mit den folgenden Code:


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample6.vb)]

> [!NOTE]
> Vielleicht warum der Ereignishandler draußen überprüfen die `Page.IsValid` Eigenschaft. Schließlich gewonnenem t das Postback unterdrückt werden, wenn ungültige Daten in die einfügende-Schnittstelle bereitgestellt werden? Diese Annahme ist richtig, solange der Benutzer nicht, JavaScript deaktiviert wurde oder verfügt über Schritte unternommen, um die clientseitige Validierungslogik zu umgehen. Kurz gesagt, sollte eine niemals ausschließlich auf die clientseitige Validierung verlassen; eine serverseitige Überprüfung auf Gültigkeit sollte immer ausgeführt werden, bevor Sie mit den Daten arbeiten.


In Schritt 1 erstellten wir die `ProductsDataSource` "ObjectDataSource" so, dass die `Insert()` Methode zugeordnet ist die `ProductsBLL` Klasse s `AddProduct` Methode. Zum Einfügen des neuen Datensatzes in die `Products` Tabelle rufen wir einfach das "ObjectDataSource"-s `Insert()` Methode:


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample7.vb)]

Nachdem die `Insert()` Methode wurde aufgerufen, wird übergeben, bleiben die Werte von der einfügen-Schnittstelle mit den Parametern zu kopieren ist, die `ProductsBLL` s-Klasse `AddProduct` Methode. Wie in beschrieben der [Untersuchen der Ereignisse zugeordnet einfügen, aktualisieren und löschen](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-vb.md) Tutorial, dies kann erreicht werden, über das ObjectDataSource-Steuerelement s `Inserting` Ereignis. In der `Inserting` Ereignis müssen wir verweisen auf die Steuerelemente aus der `Products` s GridView-Fuß-Zeile aus, und weisen Sie deren Werte in der `e.InputParameters` Auflistung. Wenn der Benutzer einen Wert wie eine verlassen lässt die `ReorderLevel` Textfeld leer, die wir benötigen, um anzugeben, dass der Wert in die Datenbank eingefügt werden soll `NULL`. Da die `AddProducts` -Methode akzeptiert die nullable-Typen für die NULL-Werte zulassen Datenbankfelder, einfach einen nullable-Typ verwenden und setzen ihren Wert auf `Nothing` in die Groß-/Kleinschreibung, die eine Benutzereingabe ausgelassen wird.


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample8.vb)]

Mit der `Inserting` Ereignishandler, die abgeschlossen werden, neue Einträge können hinzugefügt werden die `Products` über der Fußzeile der GridView-s-Datenbanktabelle. Fahren Sie fort, und versuchen Sie es Hinzufügen mehrerer neuer Produkte.

## <a name="enhancing-and-customizing-the-add-operation"></a>Erweitern und Anpassen von dem Vorgang hinzufügen

Derzeit auf die Schaltfläche "hinzufügen" Fügt einen neuen Datensatz in die Datenbanktabelle bietet jedoch jede Art von visuellem Feedback, dass der Datensatz wurde erfolgreich hinzugefügt. Im Idealfall würde auf ein Label-Web-Steuerelement oder die clientseitige Warnfeld den Benutzer informieren, den ihre Einfügung erfolgreich abgeschlossen wurde. Ich lassen Sie dieses als Übung für den Leser.

In diesem Tutorial verwendete GridView gilt nicht keine Sortierreihenfolge für die hier aufgeführten Produkte und erlaubt es dem Benutzer die Daten zu sortieren. Daher werden die Datensätze sortiert, wie sie in der Datenbank durch ihre primären Schlüsselfelder sind. Da jeder neuer Datensatz ein `ProductID` Wert größer als der letzten Zeile jedes Mal, wenn ein neues Produkt hinzugefügt wird, ist es am Ende des Rasters übernommen. Aus diesem Grund sollten Sie automatisch den Benutzer an der letzten Seite des GridView senden, nach dem Hinzufügen eines neuen Datensatzes. Dies kann erreicht werden, indem Sie die folgende Codezeile nach dem Aufruf von hinzufügen `ProductsDataSource.Insert()` in die `RowCommand` -Ereignishandler, um anzugeben, dass der Benutzer zur letzten Seite gesendet werden, nachdem die Daten an die GridView zu binden muss:


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample9.vb)]

`SendUserToLastPage` eine auf Seitenebene boolesche Variable, die anfänglich ist ein Wert zugewiesen wird der `False`. In den GridView-s `DataBound` Ereignishandler, wenn `SendUserToLastPage` ist "false", die `PageIndex` -Eigenschaft aktualisiert, um zur letzten Seite des Benutzers zu senden.


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample10.vb)]

Der Grund der `PageIndex` -eigenschaftssatzstruktur im der `DataBound` -Ereignishandler (im Gegensatz zu der `RowCommand` -Ereignishandler) liegt daran, dass bei der `RowCommand` -Ereignishandler wird ausgelöst, wir haben noch zu den neuen Datensatz hinzufügen der `Products` Datenbanktabelle. Aus diesem Grund in die `RowCommand` -Ereignishandler den Index der letzten Seite (`PageCount - 1`) stellt den Index der letzten Seite *vor* das neue Produkt wurde hinzugefügt. Für die meisten Produkte, die hinzugefügt wird entspricht der Index der letzten Seite nach dem Hinzufügen des neuen Produkts. Aber wenn führt hinzugefügte Produkt einen neuen letzte Seitenindex, bei falsch Aktualisierung der `PageIndex` in die `RowCommand` -Ereignishandler dann wir gelangen zum zweiten zur letzten Seite (der letzten Seitenindex vor dem Hinzufügen des neuen Produkts) im Gegensatz zu der neuen letzten Seite i ndextyp. Da die `DataBound` Ereignishandler ausgelöst wird, nachdem das neue Produkt wurde hinzugefügt, und die Daten in das Raster, erneut, durch Festlegen gebunden der `PageIndex` Eigenschaft es wir wissen wir erneut die erste des richtige Indexes der letzten Seite.

Schließlich ist die GridView, die in diesem Tutorial verwendeten sehr breit aufgrund der Anzahl von Feldern, die für das Hinzufügen eines neuen Produkts erfasst werden müssen. Aufgrund dieser Breite möglicherweise ein DetailsView s vertikales Layout bevorzugt. Das GridView-s Gesamtbreite konnte das Sammeln von weniger Eingaben reduziert werden. Vielleicht Wir raten t muss zum Sammeln der `UnitsOnOrder`, `UnitsInStock`, und `ReorderLevel` Felder beim Hinzufügen eines neuen Produkts, in diesem Fall diese Felder aus der GridView entfernt werden konnte.

Um die gesammelten Daten anzupassen, können wir zwei Ansätze verwenden:

- Weiterhin verwenden, die `AddProduct` -Methode, Werte für erwartet, die `UnitsOnOrder`, `UnitsInStock`, und `ReorderLevel` Felder. In der `Inserting` -Ereignishandler geben hartcodiert, Standardwerte für diese Eingaben verwenden, die von der einfügen-Schnittstelle entfernt wurden.
- Erstellen Sie eine neue Überladung der der `AddProduct` -Methode in der die `ProductsBLL` -Klasse, die keine Eingaben für akzeptiert die `UnitsOnOrder`, `UnitsInStock`, und `ReorderLevel` Felder. Konfigurieren Sie Sie dann auf der ASP.NET-Seite die "ObjectDataSource", um diese neue Überladung zu verwenden.

Beide Optionen werden ebenso wie gut funktionieren. In nach Tutorials wird die zweite Option verwenden, erstellen mehrere Überladungen für die `ProductsBLL` Klasse s `UpdateProduct` Methode.

## <a name="summary"></a>Zusammenfassung

GridView verfügt nicht über die integrierten Einfügen von Funktionen, die im DetailsView und FormView-Steuerelement gefunden, aber mit ein wenig Aufwand kann eine Schnittstelle einfügende, das die Footerzeile hinzugefügt werden. Einfach anzeigen, das die Footerzeile in einem GridView fest seine `ShowFooter` Eigenschaft `True`. Inhalt der Zeile für jedes Feld angepasst werden kann, durch das Feld in ein TemplateField konvertieren und-Schnittstelle hinzufügen, das Einfügen der `FooterTemplate`. Wie in diesem Tutorial erläutert die `FooterTemplate` Schaltflächen, Textfelder, DropDownList-Steuerelementen, Kontrollkästchen, Datenquellen-Steuerelemente für das Auffüllen der datengesteuerten von Websteuerelementen (z. B. DropDownList-Steuerelementen) und Steuerelemente zur gültigkeitsprüfung enthalten können. Sowie Steuerelemente zum Erfassen der Benutzereingabe s ist eine Schaltfläche "hinzufügen", LinkButton oder ImageButton erforderlich.

Wenn die Schaltfläche "Add" geklickt wird, das "ObjectDataSource"-s `Insert()` Methode wird aufgerufen, um die einfügen Workflow zu starten. Die ObjectDataSource ruft dann die konfigurierten Insert-Methode (die `ProductsBLL` Klasse s `AddProduct` -Methode, in diesem Tutorial). Wir müssen die Werte aus den einfügen-Schnittstelle, um das "ObjectDataSource"-s GridView-s kopieren `InsertParameters` hinzugefügt werden, bevor der Insert-Methode aufgerufen wird. Dies kann erreicht werden, durch Programmgesteuertes Verweisen auf das Einfügen von Steuerelemente der Benutzeroberfläche Web in der "ObjectDataSource"-s `Inserting` -Ereignishandler.

Dieses Tutorial ist abgeschlossen, unsere Betrachtung Techniken zum Verbessern der GridView-s-Darstellung. Der nächste Satz von Tutorials werden untersucht, wie Sie arbeiten mit Binärdaten wie Bilder, PDFs, Word-Dokumente, und so weiter die Daten und Websteuerelemente.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial wurde Bernadette Leigh. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Vorherige](adding-a-gridview-column-of-checkboxes-vb.md)
