---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/customizing-the-datalist-s-editing-interface-vb
title: Anpassen von DataList-Steuerelement die Schnittstelle (VB) bearbeiten | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial erstellen wir eine umfangreichere Benutzeroberfläche für die Bearbeitung für DataList-Steuerelement, eine, die DropDownList-Steuerelementen und ein Kontrollkästchen enthält.
ms.author: aspnetcontent
ms.date: 10/30/2006
ms.assetid: 718628e2-224c-455f-b33a-a41efd48d5a0
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/customizing-the-datalist-s-editing-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: 78001e977a4696e905317eab35604518d059e66d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828756"
---
<a name="customizing-the-datalists-editing-interface-vb"></a>Anpassen der Bearbeitung von DataList (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunter](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_40_VB.exe) oder [PDF-Datei herunterladen](customizing-the-datalist-s-editing-interface-vb/_static/datatutorial40vb1.pdf)

> In diesem Tutorial erstellen wir eine umfangreichere Benutzeroberfläche für die Bearbeitung für DataList-Steuerelement, eine, die DropDownList-Steuerelementen und ein Kontrollkästchen enthält.


## <a name="introduction"></a>Einführung

Markup und Websteuerelemente im DataList-Steuerelement s `EditItemTemplate` definieren die Schnittstelle bearbeitet werden. In allen Beispielen DataList-Steuerelement bearbeitet werden wir haben überprüft, die bearbeitbare Schnittstelle verfügt über bestehend aus TextBox-Websteuerelemente wurde. In der [vorherigen Lernprogramm](adding-validation-controls-to-the-datalist-s-editing-interface-vb.md) wir verbessert die Bearbeitungszeit Benutzeroberfläche durch Hinzufügen von Validierungssteuerelementen.

Die `EditItemTemplate` Weiter um Websteuerelemente als das Textfeld ein, z. B. DropDownList-Steuerelementen, RadioButtonList, Kalender, erweitert werden kann und so weiter. Wie bei der Textfelder ein, wenn die Bearbeitung-Schnittstelle, um andere Websteuerelemente anpassen, verwenden Sie die folgenden Schritte aus:

1. Fügen Sie das Web-Steuerelement, um die `EditItemTemplate`.
2. Verwenden Sie Databinding-Syntax, um die entsprechende Eigenschaft den Wert im entsprechenden Datenfeld zuweisen.
3. In der `UpdateCommand` programmgesteuerten Zugriff auf die Web-Ereignishandler Steuerelementwert, und übergeben dieses an die entsprechende BLL-Methode.

In diesem Tutorial erstellen wir eine umfangreichere Benutzeroberfläche für die Bearbeitung für DataList-Steuerelement, eine, die DropDownList-Steuerelementen und ein Kontrollkästchen enthält. Wir erstellen vor allem einem DataList-Steuerelement, die Produktinformationen enthält, und ermöglicht die s Produktname, Lieferanten, Kategorie, und nicht mehr unterstützte Status aktualisiert werden (siehe Abbildung 1).


[![Die Bearbeitungsschnittstelle enthält ein Textfeld, zwei DropDownList-Steuerelementen und ein Kontrollkästchen](customizing-the-datalist-s-editing-interface-vb/_static/image2.png)](customizing-the-datalist-s-editing-interface-vb/_static/image1.png)

**Abbildung 1**: die bearbeiten-Schnittstelle enthält ein Textfeld, zwei DropDownList-Steuerelementen und ein Kontrollkästchen ([klicken Sie, um das Bild in voller Größe anzeigen](customizing-the-datalist-s-editing-interface-vb/_static/image3.png))


## <a name="step-1-displaying-product-information"></a>Schritt 1: Anzeigen von Produktinformationen

Bevor wir den bearbeitbaren DataList-s-Schnittstelle erstellen können, müssen wir zuerst die schreibgeschützte Schnittstelle zu erstellen. Öffnen Sie zunächst die `CustomizedUI.aspx` Seite die `EditDeleteDataList` Ordner und Designer fügen Sie einem DataList-Steuerelement auf der Seite festlegen seiner `ID` Eigenschaft `Products`. Erstellen Sie über DataList s Smarttags eine neue "ObjectDataSource". Nennen Sie diese neue "ObjectDataSource" `ProductsDataSource` und konfigurieren Sie ihn zum Abrufen von Daten aus der `ProductsBLL` Klasse s `GetProducts` Methode. Als mit den vorherigen bearbeitbaren DataList-Tutorials aktualisieren die bearbeitete s Produktinformationen wir dazu direkt auf der Geschäftslogikebene. Entsprechend, legen Sie die Dropdownlisten in der Update-, INSERT-, ein, und löschen Sie die Registerkarten (keine).


[![Legen Sie die Update-, INSERT- und DELETE-Registerkarten Dropdownlisten auf (keine)](customizing-the-datalist-s-editing-interface-vb/_static/image5.png)](customizing-the-datalist-s-editing-interface-vb/_static/image4.png)

**Abbildung 2**: Update-, INSERT- und DELETE Registerkarten Dropdownlisten auf (keine) festgelegt ([klicken Sie, um das Bild in voller Größe anzeigen](customizing-the-datalist-s-editing-interface-vb/_static/image6.png))


Nach der Konfiguration dem ObjectDataSource-Steuerelement, erstellt Visual Studio einen Standard- `ItemTemplate` für DataList-Steuerelement, das den Namen und Wert für die einzelnen Datenfelder aufgeführt zurückgegeben. Ändern der `ItemTemplate` , damit die Vorlage den Produktnamen in führt ein `<h4>` -Elements zusammen mit den Kategorienamen, Lieferantenname, Preis und nicht mehr unterstützte Status. Darüber hinaus fügen eine Bearbeiten-Schaltfläche, die sicherstellen, dass die `CommandName` -Eigenschaftensatz auf Bearbeiten. Deklarativen Markup für meine `ItemTemplate` folgt:


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample1.aspx)]

Das obenstehende Markup zeigt die Produkt-Informationen mithilfe einer &lt;h4&gt; Überschrift für den Namen des Produkts s und eine vierspaltige `<table>` für die verbleibenden Felder. Die `ProductPropertyLabel` und `ProductPropertyValue` CSS-Klassen, die in definierten `Styles.css`, wurde in vorherigen Tutorials erläutert wurde. Abbildung 3 zeigt unseren Fortschritt, wenn Sie über einen Browser angezeigt.


[![Der Name, Lieferanten, Kategorie, Status nicht mehr unterstützt und Preis jedes Produkt wird angezeigt.](customizing-the-datalist-s-editing-interface-vb/_static/image8.png)](customizing-the-datalist-s-editing-interface-vb/_static/image7.png)

**Abbildung 3**: der Name, Lieferanten, Kategorie, Status nicht mehr unterstützt und Preis jedes Produkt angezeigt wird ([klicken Sie, um das Bild in voller Größe anzeigen](customizing-the-datalist-s-editing-interface-vb/_static/image9.png))


## <a name="step-2-adding-the-web-controls-to-the-editing-interface"></a>Schritt 2: Hinzufügen der Websteuerelemente zum die Bearbeitungsschnittstelle

Der erste Schritt bei der Erstellung von benutzerdefinierten DataList-Steuerelement ist, Hinzufügen der erforderlichen Web-Steuerelemente zum Bearbeiten-Schnittstelle der `EditItemTemplate`. Insbesondere benötigen wir eine DropDownList für die Kategorie für den Lieferanten und in der ein Kontrollkästchen für den nicht mehr unterstützte Status. Da Produktpreis s nicht bearbeitet werden, in diesem Beispiel ist, können wir weiterhin, verwenden ein Label-Steuerelement anzuzeigen.

Wenn die Bearbeitungsschnittstelle anpassen möchten, klicken Sie auf den Link "Vorlagen bearbeiten" aus dem DataList-s-Smarttag, und wählen Sie die `EditItemTemplate` Option in der Dropdown-Liste. Fügen Sie einem DropDownList-Steuerelement, das `EditItemTemplate` und legen Sie seine `ID` zu `Categories`.


[![Fügen Sie einer DropDownList für die Kategorien hinzu.](customizing-the-datalist-s-editing-interface-vb/_static/image11.png)](customizing-the-datalist-s-editing-interface-vb/_static/image10.png)

**Abbildung 4**: hinzufügen eine DropDownList für die Kategorien ([klicken Sie, um das Bild in voller Größe anzeigen](customizing-the-datalist-s-editing-interface-vb/_static/image12.png))


Als nächstes wählen Sie aus DropDownList s Smarttags, die Option für die Datenquelle auswählen und erstellen Sie eine neue, mit dem Namen "ObjectDataSource" `CategoriesDataSource`. Konfigurieren Sie diese "ObjectDataSource" Verwenden der `CategoriesBLL` Klasse s `GetCategories()` Methode (siehe Abbildung 5). Im nächsten Schritt die DropDownList-s für die Datenfelder für jeden Assistenten zur Datenquellenkonfiguration fordert `ListItem` s `Text` und `Value` Eigenschaften. Haben Sie die DropDownList-Anzeige der `CategoryName` Feld "Daten" und die Verwendung der `CategoryID` als der Wert, wie in Abbildung 6 dargestellt.


[![Erstellen Sie eine neue, mit dem Namen CategoriesDataSource "ObjectDataSource"](customizing-the-datalist-s-editing-interface-vb/_static/image14.png)](customizing-the-datalist-s-editing-interface-vb/_static/image13.png)

**Abbildung 5**: Erstellen einer neuen "ObjectDataSource" mit dem Namen `CategoriesDataSource` ([klicken Sie, um das Bild in voller Größe anzeigen](customizing-the-datalist-s-editing-interface-vb/_static/image15.png))


[![Konfigurieren der DropDownList-s-Anzeige und der Wert der Felder](customizing-the-datalist-s-editing-interface-vb/_static/image17.png)](customizing-the-datalist-s-editing-interface-vb/_static/image16.png)

**Abbildung 6**: Konfigurieren Sie die DropDownList s anzeigen und den Feldern ([klicken Sie, um das Bild in voller Größe anzeigen](customizing-the-datalist-s-editing-interface-vb/_static/image18.png))


Wiederholen Sie diese Abfolge der Schritte zum Erstellen einer DropDownList für die Lieferanten. Legen Sie die `ID` für diese DropDownList, `Suppliers` und nennen Sie die "ObjectDataSource" `SuppliersDataSource`.

Fügen Sie nach dem Hinzufügen der zwei DropDownList-Steuerelementen, die ein Kontrollkästchen für den Status nicht mehr unterstützte und ein Textfeld für den Namen des Produkts s hinzu. Legen Sie die `ID` s für die Kontrollkästchen und Textfeldern nicht das für `Discontinued` und `ProductName`bzw. Fügen Sie einen RequiredFieldValidator, um sicherzustellen, dass der Benutzer einen Wert für den Namen des Produkts s bereitstellt.

Abschließend fügen Sie die Schaltflächen Update und "Abbrechen". Beachten Sie, dass dieser beiden Schaltflächen es unumgänglich ist, deren `CommandName` Eigenschaften aktualisieren und stornieren, bzw. festgelegt werden.

Können Sie die Bearbeitungsschnittstelle Layout beliebig. Ich haben sich entschieden, den gleichen vier Spalten `<table>` Layout aus der nur-Lese Schnittstelle, wie die folgende deklarative Syntax und den Screenshot veranschaulicht:


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample2.aspx)]


[![Die bearbeiten-Schnittstelle ist, das Layout, wie die Read-Only-Schnittstelle](customizing-the-datalist-s-editing-interface-vb/_static/image20.png)](customizing-the-datalist-s-editing-interface-vb/_static/image19.png)

**Abbildung 7**: die bearbeiten-Schnittstelle ist das Layout wie die Read-Only-Schnittstelle ([klicken Sie, um das Bild in voller Größe anzeigen](customizing-the-datalist-s-editing-interface-vb/_static/image21.png))


## <a name="step-3-creating-the-editcommand-and-cancelcommand-event-handlers"></a>Schritt 3: Erstellen der EditCommand und CancelCommand-Ereignishandler

Derzeit besteht keine Databinding-Syntax in der `EditItemTemplate` (mit Ausnahme der `UnitPriceLabel`, die kopiert wurde über wörtlich der `ItemTemplate`). Wir fügen die Datenbindungssyntax vorübergehend, aber zunächst mitteilen s erstellen die Ereignishandler für das DataList s `EditCommand` und `CancelCommand` Ereignisse. Bedenken Sie, dass die Verantwortung für die `EditCommand` -Ereignishandler ist, um die Bearbeitungsschnittstelle für das DataList-Element, dessen Schaltfläche "Bearbeiten" geklickt wurde, zu rendern, während die `CancelCommand` s Aufgabe besteht darin, DataList-Steuerelement in den Zustand vor der Bearbeitung zurück.

Erstellen Sie diese zwei Ereignishandler, und Sie den folgenden Code verwenden:


[!code-vb[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample3.vb)]

Mit diesen zwei Ereignishandler vorhanden, die Sie auf die Schaltfläche "Bearbeiten" die Benutzeroberfläche der Bearbeitung angezeigt werden, und klicken auf die Schaltfläche "Abbrechen", die nur-Lese Modus gibt das bearbeitete Element zurück. Abbildung 8 zeigt DataList-Steuerelement aus, nachdem auf die Schaltfläche "Bearbeiten" für Chef Anton s Gumbo Mix geklickt wurde. Seit wir Ve noch hinzuzufügende jede Databinding-Syntax die bearbeiten-Schnittstelle, die `ProductName` Textfeld leer ist, ist der `Discontinued` deaktiviert und die ersten Elemente ausgewählt werden, aus der `Categories` und `Suppliers` DropDownList-Steuerelementen.


[![Klicken Sie auf das Bearbeiten-Schaltfläche zeigt die Bearbeitungsschnittstelle](customizing-the-datalist-s-editing-interface-vb/_static/image23.png)](customizing-the-datalist-s-editing-interface-vb/_static/image22.png)

**Abbildung 8**: Klicken Sie auf die Schaltfläche "Bearbeiten" zeigt die bearbeiten-Schnittstelle ([klicken Sie, um das Bild in voller Größe anzeigen](customizing-the-datalist-s-editing-interface-vb/_static/image24.png))


## <a name="step-4-adding-the-databinding-syntax-to-the-editing-interface"></a>Schritt 4: Die Bearbeitungsschnittstelle die DataBinding-Syntax hinzugefügt

Um die Bearbeitungsschnittstelle Anzeigen der aktuellen Produkt-s-Werte zu erhalten, müssen wir Databinding-Syntax verwenden, um die entsprechenden Werte der Web-Steuerelement die Datenfeldwerte zuweisen. Die Databinding-Syntax kann über den Designer angewendet werden, indem Sie auf dem Bildschirm Vorlagen bearbeiten aus, und wählen Sie den DataBindings bearbeiten-Link aus dem Web Smarttags steuert. Alternativ kann die Datenbindungssyntax direkt mit der deklarativen Markup hinzugefügt werden.

Zuweisen der `ProductName` Datenfeld Wert, der die `ProductName` Textfeld s `Text` -Eigenschaft, die `CategoryID` und `SupplierID` -Datenfeldwerte, um die `Categories` und `Suppliers` DropDownList-Steuerelementen `SelectedValue` Eigenschaften, und die `Discontinued` Datenfeld Wert, der die `Discontinued` Kontrollkästchen s `Checked` Eigenschaft. Nach dem vornehmen dieser Änderungen werden entweder mithilfe des Designers oder direkt über die deklaratives Markup, die Seite über einen Browser, und klicken Sie auf die Schaltfläche "Bearbeiten" für Chef Anton s Gumbo kombinieren. Wie in Abbildung 9 gezeigt, wurde die Datenbindungssyntax die aktuellen Werte in das Textfeld, DropDownList-Steuerelementen, und das Kontrollkästchen hinzugefügt.


[![Klicken Sie auf das Bearbeiten-Schaltfläche zeigt die Bearbeitungsschnittstelle](customizing-the-datalist-s-editing-interface-vb/_static/image26.png)](customizing-the-datalist-s-editing-interface-vb/_static/image25.png)

**Abbildung 9**: Klicken Sie auf die Schaltfläche "Bearbeiten" zeigt die bearbeiten-Schnittstelle ([klicken Sie, um das Bild in voller Größe anzeigen](customizing-the-datalist-s-editing-interface-vb/_static/image27.png))


## <a name="step-5-saving-the-user-s-changes-in-the-updatecommand-event-handler"></a>Schritt 5: Speichern der Benutzer-s-Änderungen in die UpdateCommand-Ereignishandler

Wenn der Benutzer ein Produkt bearbeitet und klickt auf die Schaltfläche "Aktualisieren", einem Postback und DataList-Steuerelement s `UpdateCommand` -Ereignis ausgelöst wird. In dieser Handler auf, müssen wir die Websteuerelemente im Lesen der Werte der `EditItemTemplate` und Schnittstelle mit der BLL, die das Produkt in der Datenbank zu aktualisieren. Als wir finden Sie in vorherigen Tutorials haben die `ProductID` des aktualisierten Produkts wird über die `DataKeys` Auflistung. Die Felder der freien Eingabe erfolgt durch Programmgesteuertes Verweisen auf die Web-Steuerelemente, die unter Verwendung `FindControl("controlID")`, wie der folgende Code zeigt:


[!code-vb[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample4.vb)]

Der Code von consulting beginnt die `Page.IsValid` Eigenschaft, um sicherzustellen, dass alle Validierungssteuerelemente auf der Seite gültig sind. Wenn `Page.IsValid` ist `True`, das bearbeitete Produkt s `ProductID` aus gelesene Wert der `DataKeys` Sammlung und die Dateneingabe, in websteuerungselemente der `EditItemTemplate` programmgesteuert auf die verwiesen wird. Als Nächstes werden die Werte aus dieser Websteuerelemente gelesen, in der Variablen, die anschließend in die entsprechenden übergeben werden `UpdateProduct` überladen. Nach dem Aktualisieren der Daten, DataList-Steuerelement in den Zustand vor der Bearbeitung zurückgegeben.

> [!NOTE]
> Ich Ve weggelassen, wird der Ausnahmebehandlungslogik in hinzugefügt der [behandeln Geschäftslogikschicht und Ausnahmen auf DAL-Ebene](handling-bll-and-dal-level-exceptions-vb.md) Tutorial damit bleibt der Code und in diesem Beispiel konzentriert sich. Fügen Sie nach Abschluss dieses Lernprogramms diese Funktionalität hinzu, als Übung.


## <a name="step-6-handling-null-categoryid-and-supplierid-values"></a>Schritt 6: Behandeln von NULL CategoryID und SupplierID Werte

Die Northwind-Datenbank ermöglicht `NULL` Werte für die `Products` Tabelle s `CategoryID` und `SupplierID` Spalten. Unsere Bearbeitung Schnittstelle t jedoch derzeit zu berücksichtigen `NULL` Werte. Wenn wir versuchen, ein Produkt zu bearbeiten, verfügt eine `NULL` Wert entweder die `CategoryID` oder `SupplierID` Spalten, wir kommen ein `ArgumentOutOfRangeException` mit einer Fehlermeldung ähnelt: *"Kategorien" wurde ein SelectedValue, das ungültig ist, da dies der Fall ist in der Liste der Elemente nicht vorhanden.* Darüber hinaus gibt es s derzeit keine Möglichkeit, eine Produktkategorie s oder Lieferanten ändern Wert nicht von einem`NULL` -Werts in einen `NULL` eine.

Zur Unterstützung `NULL` Werte für die Kategorie und Lieferanten DropDownList-Steuerelementen, wir müssen einen zusätzlichen `ListItem`. Ich Ve ausgewählt (keine) als der `Text` Wert für diesen `ListItem`, aber Sie können diese auf etwas anderes (z. B. eine leere Zeichenfolge) ändern, wenn d gewünscht. Schließlich müssen die DropDownList-Steuerelementen festgelegt `AppendDataBoundItems` zu `True`; Wenn Sie es vergessen, die Kategorien und Lieferanten zu DropDownList gebunden, die statisch hinzugefügt überschrieben werden `ListItem`.

Nach diesen Änderungen, die DropDownList-Steuerelementen Markup im DataList-Steuerelement s `EditItemTemplate` sollte etwa wie folgt aussehen:


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample5.aspx)]

> [!NOTE]
> Statische `ListItem` s einer DropDownList mithilfe des Designers oder direkt über die deklarative Syntax hinzugefügt werden kann. Beim Hinzufügen einer DropDownList-Elements zur Darstellung einer Datenbank `NULL` -Wert, achten Sie darauf, dass Sie zum Hinzufügen der `ListItem` mithilfe der deklarativen Syntax. Bei Verwendung der `ListItem` Auflistungs-Editor im Designer die deklarative Syntax des generierte lässt die `Value` Einstellung vollständig, wenn eine leere Zeichenfolge, und Erstellen von deklarativen Markup wie zugewiesen: `<asp:ListItem>(None)</asp:ListItem>`. Während dieser harmlos, sehen Sie möglicherweise die fehlende `Value` bewirkt, dass der Dropdownliste mit den `Text` Eigenschaftswert an seiner Stelle. Das bedeutet, dass bei dieser `NULL` `ListItem` wird ausgewählt, der Wert (None) versucht, das Feld der Product-Daten zugewiesen werden soll (`CategoryID` oder `SupplierID`, in diesem Tutorial), die führt zu einer Ausnahme. Durch das explizite Festlegen `Value=""`, `NULL` Wert für das Produkt zugewiesen wird Datenfeld, wenn die `NULL` `ListItem` ausgewählt ist.


Nehmen Sie einen Moment Zeit, um unseren Fortschritt über einen Browser anzuzeigen. Wenn Sie ein Produkt bearbeiten zu können, beachten Sie, dass die `Categories` und `Suppliers` DropDownList-Steuerelementen beide haben einen (keine) Option am Anfang der DropDownList.


[![Die Kategorien und Lieferanten DropDownList-Steuerelementen sind eine (None) Option](customizing-the-datalist-s-editing-interface-vb/_static/image29.png)](customizing-the-datalist-s-editing-interface-vb/_static/image28.png)

**Abbildung 10**: die `Categories` und `Suppliers` DropDownList-Steuerelementen sind eine (None) Option ([klicken Sie, um das Bild in voller Größe anzeigen](customizing-the-datalist-s-editing-interface-vb/_static/image30.png))


Speichern (keine) als Datenbank option `NULL` Wert müssen wir zum Zurückgeben der `UpdateCommand` -Ereignishandler. Ändern der `categoryIDValue` und `supplierIDValue` Variablen auf NULL festlegbare ganze Zahlen sein, und weisen sie einen Wert als `Nothing` nur, wenn die DropDownList-Zuordnungsvorgänge `SelectedValue` handelt es sich nicht um eine leere Zeichenfolge:


[!code-vb[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample6.vb)]

Durch diese Änderung Wert `Nothing` werden an übergeben die `UpdateProduct` BLL-Methode, die bei der Benutzerauswahl (keine) aus der Dropdown-Listen, die entspricht option eine `NULL` Datenbank-Wert.

## <a name="summary"></a>Zusammenfassung

In diesem Tutorial wurde erläutert, wie eine komplexere Bearbeitung von DataList zu erstellen, die drei verschiedene Web Eingabesteuerelemente ein TextBox-Element, zwei DropDownList-Steuerelementen und ein Kontrollkästchen zusammen mit Steuerelementen zur gültigkeitsprüfung enthalten. Wenn Sie die Bearbeitungsschnittstelle zu erstellen, die Schritte sind unabhängig von den Websteuerelementen verwendet werden: durch Hinzufügen der Web-Steuerelemente an die Datenliste s start `EditItemTemplate`; verwenden Databinding-Syntax, um die entsprechenden Datenwerte in der angegebenen Feld mit dem entsprechenden Web zuweisen Eigenschaften des Steuerelements; und dann in der `UpdateCommand` -Ereignishandler programmgesteuerten Zugriff auf die Websteuerelemente und ihre entsprechenden Eigenschaften, übergeben die Werte an die BLL.

Beim Erstellen einer bearbeiten-Schnittstelle, gibt an, ob es s besteht aus nur Textfeldern oder eine Auflistung von anderen Websteuerelemente, achten Sie darauf, um die Datenbank ordnungsgemäß zu behandeln `NULL` Werte. Wenn für die Kontoführung `NULL` s, es ist zwingend erforderlich, dass Sie nicht nur eine vorhandene richtigerweise `NULL` Wert in die Bearbeitungsschnittstelle, sondern auch, die Sie bieten eine Möglichkeit zum Markieren eines Werts als `NULL`. Für DropDownList-Steuerelementen in DataList-Steuerelementen unterstützt, das in der Regel bedeutet hinzufügen einen statischen `ListItem` , deren `Value` -Eigenschaft explizit auf eine leere Zeichenfolge festgelegt (`Value=""`), und Hinzufügen von ein paar Codezeilen, die `UpdateCommand` -Ereignishandler, um zu bestimmen, und zwar unabhängig davon, ob die `NULL``ListItem` ausgewählt wurde.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial wurden Randy Schmidt, Dennis Patterson und David Suru. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Vorherige](adding-validation-controls-to-the-datalist-s-editing-interface-vb.md)
