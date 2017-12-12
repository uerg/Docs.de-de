---
uid: web-forms/overview/data-access/enhancing-the-gridview/inserting-a-new-record-from-the-gridview-s-footer-cs
title: "Einfügen eines neuen Datensatzes aus der GridView Fußzeile (c#) | Microsoft Docs"
author: rick-anderson
description: "Während des GridView-Steuerelements keine integrierten Unterstützung für das Einfügen eines neuen Datensatzes Datenmengen bereitstellen, wird in diesem Lernprogramm gezeigt, wie erweitern, die GridView einschließen einer..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/06/2007
ms.topic: article
ms.assetid: 49545652-98af-46ba-9dbc-9ab529805d9b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/inserting-a-new-record-from-the-gridview-s-footer-cs
msc.type: authoredcontent
ms.openlocfilehash: b0208b4df0194abaf37f7f9ac66c9ce24c35d721
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="inserting-a-new-record-from-the-gridviews-footer-c"></a>Einfügen eines neuen Datensatzes aus der GridView Fußzeile (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunterladen](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_53_CS.exe) oder [PDF herunterladen](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/datatutorial53cs1.pdf)

> Während des GridView-Steuerelements keine integrierten Unterstützung für das Einfügen eines neuen Datensatzes Datenmengen bereitstellen, wird in diesem Lernprogramm gezeigt, wie um die GridView Einbeziehung eine Einfügen von Schnittstelle zu erweitern.


## <a name="introduction"></a>Einführung

Entsprechend der Anleitung unter dem [eine Übersicht der einfügen, aktualisieren und Löschen von Daten](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) Lernprogramm mit den Steuerelementen GridView, DetailsView und FormView Web gehören integrierte Möglichkeiten zur Datenänderung. Bei Verwendung mit deklarativer Datenquellensteuerelemente diese drei Websteuerelemente können schnell und einfach konfiguriert werden zum Ändern von Daten per Push – und in Szenarien ohne eine einzige Codezeile schreiben. Leider bieten nur die Steuerelemente, DetailsView und FormView integrierte einfügen, bearbeiten und Löschen von Funktionen. Die GridView bietet nur bearbeiten und Löschen von unterstützt. Jedoch können wir mit ein wenig Gewinkelte fett GridView Einbeziehung eine Schnittstelle einfügen von ergänzen.

In der GridView Einfügefunktionen hinzufügen, sind wir dafür verantwortlich entscheiden, wie neue Datensätze hinzugefügt werden, zum Erstellen der einfügen-Schnittstelle und das Schreiben des Codes zum Einfügen des neuen Datensatzes. Die Zeile in diesem Lernprogramm wird untersucht werden, ob die GridView-s-Fußzeile die einfügende-Schnittstelle hinzugefügt wird (siehe Abbildung 1). Die Zelle für jede Spalte enthält die entsprechenden Daten Auflistung Benutzeroberflächenelement (ein Textfeld für den Produktnamen s, einer DropDownList für den Lieferanten und So weiter). Wir benötigen auch eine Spalte für ein hinzufügen-Schaltfläche geklickt haben, wird dazu führen, dass einen Postback und fügen Sie einen neuen Datensatz in der `Products` Tabelle mit den Werten in der Fußzeile angegeben.


[![Die Fußzeile stellt eine Schnittstelle für das Hinzufügen neuer Produkte](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image1.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image1.png)

**Abbildung 1**: Fußzeile stellt eine Schnittstelle für neue Produkte hinzufügen ([klicken Sie hier, um das Bild in voller Größe angezeigt](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image2.png))


## <a name="step-1-displaying-product-information-in-a-gridview"></a>Schritt 1: Anzeigen von Produktinformationen in einer GridView

Bevor wir uns mit dem Erstellen der einfügen-Schnittstelle in der Fußzeile des s GridView betreffen, können Sie s zuerst den Fokus auf der Seite, die die Produkte in der Datenbank enthält eine GridView hinzugefügt. Öffnen Sie zunächst die `InsertThroughFooter.aspx` auf der Seite der `EnhancedGridView` Ordner, und ziehen Sie eine GridView aus der Toolbox in den Designer, Festlegen der GridView s `ID` Eigenschaft `Products`. Verwenden Sie anschließend das Smarttag für GridView s, um die Bindung an eine neue ObjectDataSource mit dem Namen `ProductsDataSource`.


[![Erstellen Sie eine neue, mit dem Namen ProductsDataSource ObjectDataSource](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image2.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image3.png)

**Abbildung 2**: Erstellen einer neuen ObjectDataSource namens `ProductsDataSource` ([klicken Sie hier, um das Bild in voller Größe angezeigt](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image4.png))


Konfigurieren der ObjectDataSource verwenden die `ProductsBLL` Klasse s `GetProducts()` Methode, um Produktinformationen abzurufen. Können Sie für dieses Lernprogramm s Fokus streng auf das Einfügen von Funktionen hinzufügen, und keine Gedanken Sie machen bearbeiten und löschen. Stellen Sie daher sicher, dass die Dropdown Liste in der Registerkarte "Einfügen", um festgelegt ist `AddProduct()` und der Dropdownlisten in Update- und DELETE-Registerkarten (None) festgelegt sind.


[![Ordnen Sie die AddProduct-Methode der ObjectDataSource s Insert()-Methode](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image3.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image5.png)

**Abbildung 3**: Zuordnung der `AddProduct` Methode mit dem ObjectDataSource s `Insert()` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image6.png))


[![Legen Sie das UPDATE und DELETE Registerkarten Dropdownlisten auf (keine)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image4.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image7.png)

**Abbildung 4**: Aktualisieren und Löschen von Registerkarten Dropdownlisten auf (None) festgelegt ([klicken Sie hier, um das Bild in voller Größe angezeigt](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image8.png))


Für jede von den entsprechenden Datenfeldern wird Visual Studio nach Abschluss des ObjectDataSource s Konfigurieren von Datenquellen-Assistenten Felder automatisch an die GridView hinzufügen. Lassen Sie alle Felder, die von Visual Studio hinzugefügt. Später in diesem Lernprogramm wir zurückkehren und entfernen Sie möchten einige der Felder, deren Werte t Verschlüsselungskennwort, angegeben werden, wenn Sie einen neuen Datensatz hinzufügen.

Da in der Nähe 80 Produkte in der Datenbank vorhanden sind, müssen ein Benutzer ganz bis zum Ende der Webseite zu scrollen, um einen neuen Datensatz hinzuzufügen. Aus diesem Grund können Sie s aktivieren Sie die Auslagerung auf den die einfügende Schnittstelle mehr sichtbar und zugreifbar machen. Zum Aktivieren von Paging einfach das Kontrollkästchen Sie Paging aktivieren aus dem GridView-s-Smarttag.

An diesem Punkt sollte GridView und ObjectDataSource s deklarative Markup etwa wie folgt aussehen:


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample1.aspx)]


[![Alle Product Datenfelder werden in einem ausgelagerten GridView angezeigt.](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image5.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image9.png)

**Abbildung 5**: alle Produkt Datenfelder werden in einem ausgelagerten GridView angezeigt ([klicken Sie hier, um das Bild in voller Größe angezeigt](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image10.png))


## <a name="step-2-adding-a-footer-row"></a>Schritt 2: Hinzufügen einer Fußzeile

Zusammen mit den Header als auch die Datenzeilen enthält die GridView eine Fußzeile. Kopf- und Fußzeile Zeilen werden angezeigt, abhängig von den Werten der GridView s [ `ShowHeader` ](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.gridview.showheader.aspx) und [ `ShowFooter` ](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.gridview.showfooter.aspx) Eigenschaften. Um die Fußzeile zu anzuzeigen, legen Sie einfach die `ShowFooter` Eigenschaft `true`. Abbildung 6 zeigt, Festlegen der `ShowFooter` Eigenschaft `true` dem Raster eine Fußzeile hinzugefügt.


[![Zum Anzeigen der Fußzeile ShowFooter auf "true" festgelegt](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image6.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image11.png)

**Abbildung 6**: Legen Sie zum Anzeigen der Fußzeile `ShowFooter` auf `True` ([klicken Sie hier, um das Bild in voller Größe angezeigt](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image12.png))


Beachten Sie, dass der Fußzeile eine dunklen rote Hintergrundfarbe verfügt. Dies liegt daran, Design "DataWebControls" wird erstellt und angewendet wird für alle Seiten in der [Anzeigen von Daten mit der ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) Lernprogramm. Insbesondere die `GridView.skin` Datei konfiguriert die `FooterStyle` Eigenschaft solche, die verwendet wird die `FooterStyle` CSS-Klasse. Die `FooterStyle` Klasse definiert ist, `Styles.css` wie folgt:


[!code-css[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample2.css)]

> [!NOTE]
> Wir Ve untersucht die GridView-s-Fußzeile in vorherigen Lernprogrammen verwenden. Bei Bedarf verweisen zurück auf den [Anzeigen von Zusammenfassungsinformationen in die GridView Fußzeile](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs.md) aufzufrischen Tutorial.


Nach dem Festlegen der `ShowFooter` Eigenschaft `true`, nehmen einen Moment Zeit, um die Ausgabe in einem Browser anzuzeigen. Derzeit die Fußzeile Zeile verfügt über t Text oder Web-Steuerelemente enthalten. In Schritt 3 wird nun die Fußzeile für jedes Feld GridView geändert, damit er die entsprechende einfügende Schnittstelle enthält.


[![Die leere Fußzeile wird angezeigt, über das Paging Benutzeroberflächen-Steuerelemente](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image7.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image13.png)

**Abbildung 7**: die leere Fußzeile wird angezeigt, über das Paging Benutzeroberflächen-Steuerelemente ([klicken Sie hier, um das Bild in voller Größe angezeigt](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image14.png))


## <a name="step-3-customizing-the-footer-row"></a>Schritt 3: Anpassen der Fußzeile

In der [mithilfe von TemplateFields, im des GridView-Steuerelements](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) Lernprogramm wurde erläutert, wie stark die Anzeige einer bestimmten GridView-Spalte mithilfe von TemplateFields (im Gegensatz zu BoundFields oder CheckBoxFields) anpassen, in [ Anpassen der Benutzeroberfläche für die Änderung der Daten](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) erläutert, die Bearbeitungsoberfläche in einem GridView anpassen mithilfe von TemplateFields. Beachten Sie, dass ein TemplateField einer Reihe von Vorlagen besteht, die die Mischung von Markup, Websteuerelemente und Databinding-Syntax definiert verwendet für bestimmte Typen von Zeilen. Die `ItemTemplate`, gibt z. B. die Vorlage für nur-Lese Zeilen, während die `EditItemTemplate` definiert die Vorlage für die Zeile bearbeitet werden.

Zusammen mit den `ItemTemplate` und `EditItemTemplate`, die TemplateField enthält auch eine `FooterTemplate` , die den Inhalt für die Fußzeile angibt. Daher können wir die Websteuerelementen benötigt für jedes Feld s einfügen Schnittstelle für Hinzufügen der `FooterTemplate`. Um zu starten, konvertieren Sie alle Felder in der GridView in von TemplateFields aus. Dies kann erfolgen durch Klicken auf die Verknüpfung Spalten bearbeiten im GridView s Smarttag, jedes Feld in der unteren linken Ecke auswählen und auf die Convert-dieses Feld in einen TemplateField-Link.


![Jedes Feld in ein TemplateField konvertieren](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image8.gif)

**Abbildung 8**: jedes Feld in ein TemplateField konvertieren


Durch Klicken auf die Convert wird dieses Feld in ein TemplateField den Typ des aktuellen Felds in eine entsprechende TemplateField. Jede BoundField wird z. B. durch ein TemplateField mit ersetzt eine `ItemTemplate` , enthält eine Bezeichnung, die das entsprechende Datenfeld anzeigt und eine `EditItemTemplate` , die das Feld "Daten" in einem Textfeld anzeigt. Die `ProductName` BoundField wurde in der folgenden TemplateField-Markup konvertiert:


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample3.aspx)]

Entsprechend der `Discontinued` CheckBoxField konvertiert wurde in ein TemplateField, deren `ItemTemplate` und `EditItemTemplate` enthalten ein Kontrollkästchen-Websteuerelement (mit der `ItemTemplate` s Kontrollkästchen deaktiviert). Die schreibgeschützte `ProductID` BoundField wurde in ein TemplateField mit einem Label-Steuerelement in den beiden konvertiert die `ItemTemplate` und `EditItemTemplate`. Kurz gesagt, ist konvertieren eine vorhandene GridView Feld in ein TemplateField eine schnelle und einfache Möglichkeit, die mehr anpassbare TemplateField wechseln, ohne die vorhandene Feld s Funktionalität verloren.

Seit der GridView wir arbeiten verfügt über Bearbeitung t unterstützen wünschen, entfernen Sie die `EditItemTemplate` aus jeder TemplateField verlassen nur die `ItemTemplate`. Nachdem Sie auf diese Weise sollte GridView s deklarative Markup wie folgt aussehen:


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample4.aspx)]

Nun, dass jedes GridView-Feld in ein TemplateField konvertiert wurde, können wir die entsprechende Schnittstelle für die einfügende eingeben, in jedes Feld s `FooterTemplate`. Einige der Felder hat keine Schnittstelle einfügen (`ProductID`, z. B.); andere variieren in den Websteuerelementen verwendet, um die neue Produktinformationen s zu sammeln.

Um die Bearbeitung Schnittstelle zu erstellen, wählen Sie die Vorlagen bearbeiten-Verknüpfung aus dem Smarttag des GridView s ein. Wählen Sie dann aus der Dropdown-Liste das entsprechende Feld s `FooterTemplate` und das entsprechende Steuerelement aus der Toolbox in den Designer ziehen.


[![Jedes Feld s FooterTemplate Hinzufügen der entsprechenden einfügen-Schnittstelle](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image9.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image15.png)

**Abbildung 9**: Hinzufügen der entsprechenden einfügen-Schnittstelle in jedem Feld s `FooterTemplate` ([klicken Sie hier, um das Bild in voller Größe angezeigt](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image16.png))


Die folgenden Aufzählung Listet die GridView-Felder, die das Einfügen von Schnittstelle hinzuzufügende angeben:

- `ProductID`None.
- `ProductName`Fügen Sie ein Textfeld hinzu, und legen Sie dessen `ID` auf `NewProductName`. Fügen Sie ein RequiredFieldValidator-Steuerelement hinzu, um sicherzustellen, dass der Benutzer einen Wert für den neuen Produktnamen s eingibt.
- `SupplierID`None.
- `CategoryID`None.
- `QuantityPerUnit`Fügen Sie ein Textfeld, das Festlegen seiner `ID` auf `NewQuantityPerUnit`.
- `UnitPrice`Fügen Sie ein Textfeld mit dem Namen `NewUnitPrice` und eine CompareValidator, die wird sichergestellt, den eingegebene Wert dass ist ein Currency-Wert größer als oder gleich 0 (null).
- `UnitsInStock`Verwenden Sie ein Textfeld, deren `ID` festgelegt ist, um `NewUnitsInStock`. Einschließen einer CompareValidator, der sicherstellt, dass der eingegebene Wert einen ganzzahligen Wert größer als oder gleich 0 (null) ist.
- `UnitsOnOrder`Verwenden Sie ein Textfeld, deren `ID` festgelegt ist, um `NewUnitsOnOrder`. Einschließen einer CompareValidator, der sicherstellt, dass der eingegebene Wert einen ganzzahligen Wert größer als oder gleich 0 (null) ist.
- `ReorderLevel`Verwenden Sie ein Textfeld, deren `ID` festgelegt ist, um `NewReorderLevel`. Einschließen einer CompareValidator, der sicherstellt, dass der eingegebene Wert einen ganzzahligen Wert größer als oder gleich 0 (null) ist.
- `Discontinued`Fügen Sie ein Kontrollkästchen, das Festlegen seiner `ID` auf `NewDiscontinued`.
- `CategoryName`Fügen Sie einer DropDownList hinzu, und legen Sie dessen `ID` auf `NewCategoryID`. Binden Sie es an eine neue ObjectDataSource mit dem Namen `CategoriesDataSource` und konfigurieren es für das Verwenden der `CategoriesBLL` Klasse s `GetCategories()` Methode. Der DropDownList s haben `ListItem` s Anzeige der `CategoryName` Daten-Felds mithilfe der `CategoryID` Feld "Daten" als ihre Werte.
- `SupplierName`Fügen Sie einer DropDownList hinzu, und legen Sie dessen `ID` auf `NewSupplierID`. Binden Sie es an eine neue ObjectDataSource mit dem Namen `SuppliersDataSource` und konfigurieren es für das Verwenden der `SuppliersBLL` Klasse s `GetSuppliers()` Methode. Der DropDownList s haben `ListItem` s Anzeige der `CompanyName` Daten-Felds mithilfe der `SupplierID` Feld "Daten" als ihre Werte.

Für die einzelnen Validierungssteuerelemente, löschen, die `ForeColor` Eigenschaft, damit die `FooterStyle` anstelle des standardmäßigen red weißen Vordergrundfarbe für CSS-Klasse verwendet werden. Auch die `ErrorMessage` jedoch eine ausführliche Beschreibung Eigenschaftensatz die `Text` Eigenschaft auf ein Sternchen. Um zu verhindern, dass die Überprüfung Steuerelementtext s verursacht die einfügende-Schnittstelle, um zwei Zeilen zu umschließen, legen Sie die `FooterStyle` s `Wrap` Eigenschaft auf "false" für jede der `FooterTemplate` s, die ein Validierungssteuerelement verwenden. Schließlich fügen ein ValidationSummary Steuerelement unterhalb des GridView und seine `ShowMessageBox` Eigenschaft, um `true` und seine `ShowSummary` Eigenschaft, um `false`.

Wenn Sie ein neues Produkt hinzufügen, müssen wir bieten die `CategoryID` und `SupplierID`. Diese Informationen werden erfasst, durch die DropDownLists in der Fußzeilenzellen für die `CategoryName` und `SupplierName` Felder. Ich ausgewählt haben, verwenden Sie diese Felder im Gegensatz zu den `CategoryID` und `SupplierID` von TemplateFields da im Raster s Daten den Benutzer Zeilen wird wahrscheinlich mehr ihre ID-Werte, anstatt die Namen der Kategorie und Lieferanten anzeigen möchte. Da die `CategoryID` und `SupplierID` Werte werden nun erfasst wird die `CategoryName` und `SupplierName` Feld s Einfügen von Schnittstellen, die wir Entfernen der `CategoryID` und `SupplierID` von TemplateFields aus der GridView.

Auf ähnliche Weise die `ProductID` wird nicht verwendet werden, wenn Sie ein neues Produkt hinzufügen das `ProductID` TemplateField kann auch entfernt werden. Ermöglichen jedoch das s lassen die `ProductID` Feld im Raster. Zusätzlich zu den Textfelder DropDownLists, Kontrollkästchen und Validierungssteuerelemente, die die einfügende Schnittstelle bilden, benötigen wir auch ein hinzufügen-Schaltfläche geklickt wird, führt die Logik, um das neue Produkt zur Datenbank hinzuzufügen. In Schritt 4 schließen wir eine Schaltfläche "hinzufügen" in der einfügen-Schnittstelle in der `ProductID` TemplateField s `FooterTemplate`.

Wahlweise können die Darstellung der verschiedenen Felder GridView zu verbessern. Angenommen, Sie möchten möglicherweise Formatieren der `UnitPrice` Werte als Währung, rechtsbündig der `UnitsInStock`, `UnitsOnOrder`, und `ReorderLevel` Felder und Update der `HeaderText` Werte für die von TemplateFields.

Nach dem Erstellen der Fülle Einfügen von Schnittstellen in den `FooterTemplate` s, entfernt der `SupplierID`, und `CategoryID` von TemplateFields, und verbessern die Ästhetik des Rasters durch formatieren und zum Ausrichten von den von TemplateFields die deklarative GridView-s Markup sollte etwa wie folgt aussehen:


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample5.aspx)]

Wenn Sie über einen Browser angezeigt wird, die Fußzeile des GridView-s enthält jetzt das abgeschlossene einfügen Schnittstelle (siehe Abbildung 10). An diesem Punkt enthalten Einfügen von t Schnittstelle verfügt über eine Möglichkeit für den Benutzer aus, um anzugeben, She s die Daten für das neue Produkt eingegeben und möchte einen neuen Datensatz in die Datenbank einfügen. Darüber hinaus wir Ve noch zu behandeln, wie die Daten eingegeben werden, in der Fußzeile übersetzt werden, in einen neuen Datensatz in die `Products` Datenbank. In Schritt 4 Wir betrachten eine Schaltfläche "hinzufügen" auf die einfügende Schnittstelle eingeschlossen und zum Ausführen von Code auf postback, wenn es s geklickt haben. Schritt 5 wird gezeigt, wie einen neuen Datensatz mit den Daten aus der Fußzeile eingefügt wird.


[![Die Fußzeile GridView stellt eine Schnittstelle für einen neuen Datensatz hinzufügen](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image10.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image17.png)

**Abbildung 10**: GridView Fußzeile stellt eine Schnittstelle für einen neuen Datensatz hinzufügen ([klicken Sie hier, um das Bild in voller Größe angezeigt](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image18.png))


## <a name="step-4-including-an-add-button-in-the-inserting-interface"></a>Schritt 4: Z. B. eine Schaltfläche "hinzufügen" in der einfügen-Schnittstelle

Wir müssen eine Schaltfläche "hinzufügen" an einer beliebigen Stelle in der einfügen-Schnittstelle, da die Fußzeile Zeile s-Schnittstelle gegenwärtig einfügen verfügt nicht über die bedeutet für den Benutzer aus, um anzugeben, dass er die neue Produktinformationen s eingeben abgeschlossen hat. Dies könnte platziert werden, in der bestehenden `FooterTemplate` s oder es kann eine neue Spalte hinzufügen zum Raster für diesen Zweck. Für dieses Lernprogramm können s platzieren Sie die Schaltfläche "hinzufügen" in der `ProductID` TemplateField s `FooterTemplate`.

Klicken Sie im Designer klicken auf den Link Vorlagen bearbeiten, in das Smarttag für GridView s, und wählen Sie dann die `ProductID` Feld s `FooterTemplate` aus der Dropdown-Liste. Die Vorlage an, wenn seine ID auf eine Schaltfläche Websteuerelement (oder eine LinkButton oder ImageButton, falls gewünscht) hinzufügen `AddProduct`, dessen `CommandName` einfügen, und die zugehörige `Text` Eigenschaft hinzufügen, wie in Abbildung 11 dargestellt.


[![Platzieren Sie die Schaltfläche "hinzufügen" in "ProductID" TemplateField s FooterTemplate](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image11.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image19.png)

**Abbildung 11**: Setzen Sie die Schaltfläche "hinzufügen" in der `ProductID` TemplateField s `FooterTemplate` ([klicken Sie hier, um das Bild in voller Größe angezeigt](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image20.png))


Sobald Sie Ve auf die Schaltfläche "hinzufügen" enthalten, testen Sie die Seite in einem Browser. Beachten Sie, dass beim Klicken auf die Schaltfläche "hinzufügen" mit ungültigen Daten in der einfügen-Schnittstelle, die das Postback kurz circuited ist und das Steuerelement ValidationSummary gibt an, die ungültigen Daten (siehe Abbildung 12). Beim Klicken auf die Schaltfläche "hinzufügen" mit entsprechenden Daten eingegeben haben bewirkt ein Postback. Kein Datensatz ist jedoch in der Datenbank hinzugefügt. Wir benötigen viel Code tatsächlich einfügungs-zu schreiben.


[![Die Schaltfläche "hinzufügen"-s ist Postback kurze Circuited wenn ungültige Daten in der einfügen-Schnittstelle](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image12.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image21.png)

**Abbildung 12**: die Schaltfläche "hinzufügen" s Postback ist kurze Circuited, wenn ungültige Daten in der einfügen-Schnittstelle ([klicken Sie hier, um das Bild in voller Größe angezeigt](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image22.png))


> [!NOTE]
> Die Validierungssteuerelemente in der einfügen-Schnittstelle wurden Validierungsgruppe nicht zugewiesen. Diese Vorgehensweise ist, solange die einfügende Schnittstelle nur Satz von Validierungssteuerelemente auf der Seite ist. Wenn es gibt jedoch auch andere Validierungssteuerelemente auf der Seite (z. B. Validierungssteuerelemente in der Bearbeitung Raster s-Schnittstelle), die Validierungssteuerelemente einfügen-Schnittstelle und s Schaltfläche hinzufügen `ValidationGroup` Eigenschaften den gleichen Wert zugewiesen werden dass Ordnen Sie einer bestimmten Validierungsgruppe mit diesen Steuerelementen. Finden Sie unter [unterzogen, wodurch der Validierungssteuerelemente in ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx) für Weitere Informationen zum Partitionieren der Validierungssteuerelemente und die Schaltflächen auf einer Seite Überprüfung gruppiert.


## <a name="step-5-inserting-a-new-record-into-theproductstable"></a>Schritt 5: Einfügen eines neuen Datensatzes in der`Products`Tabelle

Bei Verwendung der integrierten Bearbeitungsfunktionen der GridView verarbeitet die GridView automatisch alle Arbeitsaufgaben, die für die Durchführung der Aktualisierung erforderlich. Insbesondere, wenn auf die Schaltfläche "Aktualisieren" geklickt wird kopiert er die eingegebenen Werte von der Schnittstelle bearbeiten an die Parameter in der ObjectDataSource s `UpdateParameters` Sammlungs- und sicherheitsschwellwerte deaktiviert das Update durch Aufrufen der ObjectDataSource s `Update()` Methode. Da die GridView keine solche integrierte Funktionen zum Einfügen von bereitstellt, implementieren wir muss Code, der das ObjectDataSource-s aufruft `Insert()` -Methode und kopiert die Werte aus den einfügen-mit dem ObjectDataSource s Schnittstelle `InsertParameters` Auflistung .

Diese Logik einfügen sollte ausgeführt werden, nachdem auf die Schaltfläche "hinzufügen" geklickt wurde. Wie in beschrieben die [hinzufügen und reagieren auf Schaltflächen in einem GridView](../custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-cs.md) Lernprogramm jedes Mal, wenn eine Schaltfläche, LinkButton oder ImageButton in einem GridView geklickt wird, die GridView s `RowCommand` beim Postback-Ereignis ausgelöst. Dieses Ereignis wird ausgelöst, ob die Schaltfläche, LinkButton oder ImageButton explizit wie z. B. die Schaltfläche "hinzufügen" in der Fußzeile hinzugefügt wurde oder ob er automatisch durch die GridView hinzugefügt wurde (z. B. die LinkButtons am oberen Rand der einzelnen Spalten beim Sortieren aktivieren aktiviert ist, oder die LinkButtons in der Auslagerungsdatei-Schnittstelle, wenn Paging aktivieren aktiviert ist).

Um dem Benutzer, die Sie auf die Schaltfläche "hinzufügen" zu reagieren, wir müssen daher erstellen Sie einen Ereignishandler für die GridView-s `RowCommand` Ereignis. Da dieses Ereignis wird, wenn ausgelöst *alle* Schaltfläche LinkButton oder ImageButton in die GridView geklickt wird, es s entscheidend, dass wir nur die einfügende Logik Wenn Fortsetzen der `CommandName` Eigenschaft, die in den Ereignis-Handler Code Maps an die übergeben`CommandName` Wert der Schaltfläche "hinzufügen" (Insert). Darüber hinaus sollte es auch nur dann fortgesetzt, wenn der Validierungssteuerelemente gültige Daten melden. Um dies zu berücksichtigen, erstellen Sie einen Ereignishandler für das `RowCommand` Ereignis mit den folgenden Code:


[!code-csharp[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample6.cs)]

> [!NOTE]
> Sie vielleicht warum der Ereignishandler beizubehalten überprüft die `Page.IsValid` Eigenschaft. Nachdem alle erzielter t das Postback unterdrückt werden, wenn ungültige Daten in der einfügen-Schnittstelle bereitgestellt werden? Diese Annahme ist richtig, solange der Benutzer nicht, JavaScript deaktiviert wurde oder Schritte unternommen hat, um die Überprüfungslogik für die clientseitige umgehen. Kurz gesagt, sollten eine nie streng auf die clientseitige Validierung verlassen; eine serverseitige Überprüfung auf Gültigkeit sollte immer ausgeführt werden, bevor Sie mit den Daten arbeiten.


In Schritt 1 erstellten wir die `ProductsDataSource` ObjectDataSource so, dass seine `Insert()` -Methode zugeordnet ist die `ProductsBLL` Klasse s `AddProduct` Methode. Zum Einfügen des neuen Eintrags in der `Products` Tabelle, wir können einfach die ObjectDataSource s Aufrufen `Insert()` Methode:


[!code-csharp[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample7.cs)]

Nachdem die `Insert()` Methode wurde aufgerufen, wird lediglich bleibt so kopieren Sie die Werte von der einfügen-Schnittstelle auf die Parameter übergeben der `ProductsBLL` Klasse s `AddProduct` Methode. Wie wir gesehen, wieder in haben die [untersuchen die Ereignisse zugeordneten einfügen, aktualisieren und Löschen von](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs.md) Tutorial, dies kann erreicht werden, über das ObjectDataSource-s `Inserting` Ereignis. In der `Inserting` Ereignis wir die Steuerelemente aus programmgesteuert verweisen müssen die `Products` GridView s Fußzeile Zeile, und weisen Sie deren Werte in der `e.InputParameters` Auflistung. Wenn der Benutzer einen Wert wie lassen keine der `ReorderLevel` Textfeld leer, die wir benötigen, um anzugeben, dass der Wert in die Datenbank eingefügt werden soll `NULL`. Da die `AddProducts` Methode akzeptiert auf NULL festlegbare Typen für die NULL-Werte zulassen Datenbankfelder, einfach verwenden Sie einen NULL-Werte zulässt und dem Wert `null` im Fall, in denen Benutzereingaben weggelassen wird.


[!code-csharp[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample8.cs)]

Mit der `Inserting` Ereignishandler abgeschlossen, neue Datensätze können hinzugefügt werden die `Products` Datenbanktabelle über die GridView-s-Fußzeile. Fahren Sie fort, und wiederholen Sie den Hinzufügen mehrerer neuer Produkte.

## <a name="enhancing-and-customizing-the-add-operation"></a>Erweitern und Anpassen der Vorgang zum Hinzufügen

Derzeit auf die Schaltfläche "hinzufügen" Fügt einen neuen Datensatz in die Datenbanktabelle bietet jedoch jede Art von visuelles Feedback, dass der Datensatz wurde erfolgreich hinzugefügt. Eine Bezeichnung Web Steuerelement oder die clientseitige Warnfeld würde idealerweise, den Benutzer informieren, den ihre Insert erfolgreich abgeschlossen wurde. Ich lassen Sie dieses als Übung für den Leser.

In diesem Lernprogramm verwendete GridView gilt nicht keine Sortierreihenfolge für die aufgelisteten Produkte noch lässt Endbenutzer zum Sortieren der Daten. Folglich werden die Einträge sortiert, wie in der Datenbank nach deren Primärschlüsselfelds. Da jeder neuer Datensatz hat einen `ProductID` Wert größer als der letzten Zeile jedes Mal, wenn ein neues Produkt hinzugefügt wird, wird es bis zum Ende des Rasters übernommen. Aus diesem Grund empfiehlt es sich den Benutzer automatisch an der letzten Seite des GridView zu senden, nach dem Hinzufügen eines neuen Datensatzes. Dies kann durch Hinzufügen der folgenden Zeile des Codes nach dem Aufruf von `ProductsDataSource.Insert()` in der `RowCommand` -Ereignishandler, um anzugeben, dass der Benutzer zur letzten Seite gesendet werden, nachdem die Daten an die GridView zu binden muss:


[!code-csharp[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample9.cs)]

`SendUserToLastPage`eine boolesche Seitenebene-Variable, die anfänglich ist einen Wert zugewiesen `false`. In der GridView s `DataBound` Ereignishandler, d. h. wenn `SendUserToLastPage` lautet "false", die `PageIndex` Eigenschaft wird aktualisiert, damit um der Benutzer zur letzten Seite zu senden.


[!code-csharp[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample10.cs)]

Der Grund der `PageIndex` -eigenschaftssatzstruktur im die `DataBound` Ereignishandler (im Gegensatz zu der `RowCommand` Ereignishandler) liegt daran, dass beim der `RowCommand` -Ereignishandler wird ausgelöst, wir haben noch auf den neuen Datensatz hinzufügen der `Products` Datenbanktabelle. Aus diesem Grund in die `RowCommand` -Ereignishandler den Index der letzten Seite (`PageCount - 1`) stellt den Index der letzten Seite *vor* das neue Produkt hinzugefügt wurde. Für die meisten der Produkte, die hinzugefügt wird entspricht dem der Index der letzten Seite nach dem das neue Produkt hinzufügen. Wenn führt das hinzugefügte Produkt eine neue letzte Seitenindex, wenn wir nicht ordnungsgemäß aktualisiert, aber die `PageIndex` in die `RowCommand` Ereignishandler wählen Sie dann gelangen auf die Sekunde auf der letzten Seite (der letzten Seitenindex vor dem Hinzufügen des neuen Produkts) im Gegensatz zu der neuen letzten Seite i Ndex. Da die `DataBound` -Ereignishandler ausgelöst wird, nachdem das neue Produkt hinzugefügt wurde und die Daten erneut am Raster gebunden wird durch Festlegen der `PageIndex` Eigenschaft es wir wissen wir re den richtige Seitenindex der letzten abrufen.

Schließlich wird in diesem Lernprogramm verwendete GridView sehr breit aufgrund der Anzahl von Feldern, die für das Hinzufügen eines neuen Produkts gesammelt werden müssen. Aufgrund dieser Breite möglicherweise bevorzugte ein DetailsView s vertikales Layout. Die GridView s Gesamtbreite sammeln weniger Eingaben verringert werden. Vielleicht Verschlüsselungskennwort wir t muss zum Sammeln der `UnitsOnOrder`, `UnitsInStock`, und `ReorderLevel` beim Hinzufügen eines neuen Produkts Felder in diesem Fall diese Felder aus der GridView entfernt werden konnte.

Um die gesammelten Daten anzupassen, können wir zwei Ansätze verwenden:

- Verwenden weiterhin die `AddProduct` übergeben wird, Werte für erwartet, die `UnitsOnOrder`, `UnitsInStock`, und `ReorderLevel` Felder. In der `Inserting` Ereignishandler, d. h. bieten fest programmiert, Standardwerte für diese Eingaben zu verwenden, die von der einfügen-Schnittstelle entfernt wurden.
- Erstellen Sie eine neue Überladung der der `AddProduct` Methode in der `ProductsBLL` -Klasse, die nicht für Eingaben akzeptiert die `UnitsOnOrder`, `UnitsInStock`, und `ReorderLevel` Felder. Konfigurieren Sie Sie dann auf der ASP.NET-Seite die ObjectDataSource, um diese neue Überladung verwenden.

Beide Optionen wird ebenso wie gut funktionieren. In hinter der Lernprogramme wird die letztgenannte Option erstellen mehrere Überladungen für die `ProductsBLL` Klasse s `UpdateProduct` Methode.

## <a name="summary"></a>Zusammenfassung

GridView verfügt nicht über den integrierten Einfügen von Funktionen in der, DetailsView und FormView, jedoch mit ein wenig Aufwand kann eine einfügende Schnittstelle in der Fußzeile hinzugefügt werden. Einfach anzuzeigende Fußzeile in einem GridView Festlegen seiner `ShowFooter` Eigenschaft `true`. Inhalt der Zeile für jedes Feld angepasst werden kann, indem Sie das Feld in ein TemplateField konvertieren und-Schnittstelle hinzufügen, das Einfügen der `FooterTemplate`. Wie wir gesehen, in diesem Lernprogramm haben die `FooterTemplate` enthalten können, Schaltflächen, Textfelder, DropDownLists, Kontrollkästchen, Datenquellen-Steuerelementen für das Auffüllen eines datengesteuerten Web Controls (z. B. DropDownLists) und Validierungssteuerelemente. Zusammen mit den Steuerelementen für das Sammeln von Benutzereingaben s ist eine Schaltfläche "hinzufügen", LinkButton oder ImageButton erforderlich.

Wenn die Schaltfläche "hinzufügen" geklickt wird, wird die s ObjectDataSource `Insert()` Methode wird aufgerufen, um das Einfügen von Workflow zu starten. Das ObjectDataSource ruft dann die konfigurierten Insert-Methode (die `ProductsBLL` Klasse s `AddProduct` Methode, die in diesem Lernprogramm). Wir müssen die Werte von der Schnittstelle mit dem ObjectDataSource s einfügen GridView s kopieren `InsertParameters` Auflistung vor der Insert-Methode aufgerufen wird. Dies geschieht durch Programmgesteuertes Verweisen auf das Einfügen von Websteuerelemente der Benutzeroberfläche in das ObjectDataSource-s `Inserting` -Ereignishandler.

Dieses Lernprogramm ist unsere Betrachtung der Techniken zum Verbessern der Darstellung des GridView s abgeschlossen. Wie Arbeiten mit Binärdaten, z. B. PDF, Word-Dokumente, Bilder und so weiter die Daten und Websteuerelemente untersucht des nächsten Satzes von Lernprogramme.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurde Bernadette Leigh. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Zurück](adding-a-gridview-column-of-checkboxes-cs.md)
[Weiter](adding-a-gridview-column-of-radio-buttons-vb.md)
