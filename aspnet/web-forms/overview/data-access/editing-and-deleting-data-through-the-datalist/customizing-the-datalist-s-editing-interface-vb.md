---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/customizing-the-datalist-s-editing-interface-vb
title: "DataList Anpassen der Benutzeroberfläche (VB) bearbeiten | Microsoft Docs"
author: rick-anderson
description: "In diesem Lernprogramm werden wir eine umfangreichere Bearbeitungsoberfläche für DataList, eine erstellen, die DropDownLists und ein Kontrollkästchen enthält."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: 718628e2-224c-455f-b33a-a41efd48d5a0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/customizing-the-datalist-s-editing-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: bd31c18b9fced12feeda8ca8dea7dace0ef63573
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="customizing-the-datalists-editing-interface-vb"></a>Anpassen der DataList Bearbeitung der Benutzeroberfläche (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunterladen](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_40_VB.exe) oder [PDF herunterladen](customizing-the-datalist-s-editing-interface-vb/_static/datatutorial40vb1.pdf)

> In diesem Lernprogramm werden wir eine umfangreichere Bearbeitungsoberfläche für DataList, eine erstellen, die DropDownLists und ein Kontrollkästchen enthält.


## <a name="introduction"></a>Einführung

Das Markup und Websteuerelementen in DataList s `EditItemTemplate` definieren die bearbeitbare-Schnittstelle. In alle bearbeitbaren DataList Beispiele wir Ve untersucht bisher, die bearbeitbare Schnittstelle verfügt über besteht der TextBox-Websteuerelemente wurde. In der [vorherigen Lernprogramm](adding-validation-controls-to-the-datalist-s-editing-interface-vb.md) wir verbessert die Bearbeitungszeit Benutzeroberfläche durch Hinzufügen von Steuerelementen für die Validierung.

Die `EditItemTemplate` Einbeziehung Websteuerelemente als das Textfeld ein, z. B. DropDownLists RadioButtonList, verwendete Kalender, weiter erweitert werden kann und so weiter. Wie bei der Textfelder, beim Anpassen der Bearbeitung-Schnittstelle, um andere Websteuerelemente enthalten, verwenden Sie die folgenden Schritte aus:

1. Fügen Sie das Web-Steuerelement an die `EditItemTemplate`.
2. Verwenden Sie Databinding-Syntax, um die entsprechende Eigenschaft den Wert des entsprechenden Datenfelds zuweisen.
3. In der `UpdateCommand` Ereignishandler, d. h. programmgesteuerten Zugriff auf das Web steuern Wert, und übergeben Sie ihn an die entsprechende BLL-Methode.

In diesem Lernprogramm werden wir eine umfangreichere Bearbeitungsoberfläche für DataList, eine erstellen, die DropDownLists und ein Kontrollkästchen enthält. Insbesondere erstellen wir eine DataList, die Produktinformationen enthält, und ermöglicht, s, Produktname, Lieferanten, Kategorie und nicht mehr unterstützte Status aktualisiert werden (siehe Abbildung 1).


[![Die Bearbeitungsoberfläche umfasst ein Textfeld, zwei DropDownLists und ein Kontrollkästchen](customizing-the-datalist-s-editing-interface-vb/_static/image2.png)](customizing-the-datalist-s-editing-interface-vb/_static/image1.png)

**Abbildung 1**: die Schnittstelle bearbeiten umfasst ein Textfeld, zwei DropDownLists und ein Kontrollkästchen ([klicken Sie hier, um das Bild in voller Größe angezeigt](customizing-the-datalist-s-editing-interface-vb/_static/image3.png))


## <a name="step-1-displaying-product-information"></a>Schritt 1: Anzeigen von Produktinformationen

Bevor wir die bearbeitbare DataList-s-Schnittstelle erstellen können, müssen wir zunächst die schreibgeschützte Schnittstelle zu erstellen. Öffnen Sie zunächst die `CustomizedUI.aspx` Seite aus der `EditDeleteDataList` Ordner und Designer fügen Sie einem DataList auf der Seite festlegen seiner `ID` Eigenschaft, um `Products`. Erstellen Sie aus den DataList s smart Tag eine neue ObjectDataSource. Nennen Sie diese neue ObjectDataSource `ProductsDataSource` und konfigurieren Sie ihn zum Abrufen von Daten aus der `ProductsBLL` Klasse s `GetProducts` Methode. Wie werden mit den vorherigen bearbeitbaren DataList-Lernprogrammen, wir die bearbeitete s Produktinformationen aktualisieren er direkt der Geschäftslogikebene. Entsprechend, legen Sie die Dropdownlisten in der Update-, INSERT-, ein, und Löschen von Registerkarten (keine).


[![Legen Sie die Update-, INSERT- und DELETE-Registerkarten Dropdown-Listen zu (keine)](customizing-the-datalist-s-editing-interface-vb/_static/image5.png)](customizing-the-datalist-s-editing-interface-vb/_static/image4.png)

**Abbildung 2**: aktualisieren, einfügen und Löschen von Registerkarten Dropdownlisten auf (None) festgelegt ([klicken Sie hier, um das Bild in voller Größe angezeigt](customizing-the-datalist-s-editing-interface-vb/_static/image6.png))


Nach dem Konfigurieren der ObjectDataSource, erstellt Visual Studio einen Standard- `ItemTemplate` für DataList mit den Namen und Wert für die einzelnen Datenfelder zurückgegeben. Ändern der `ItemTemplate` so, dass die Vorlage, den Produktnamen in auflistet einer `<h4>` -Elements zusammen mit den Kategorienamen, Lieferantenname, Preis und nicht mehr unterstützte Status. Darüber hinaus fügen Sie eine Schaltfläche "Bearbeiten", sicherstellen, dass seine `CommandName` Eigenschaftensatz zu bearbeiten. Das deklarative Markup für meine `ItemTemplate` folgt:


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample1.aspx)]

Angeordnet, dass die oben genannten Markup das Produkt mit einer &lt;h4&gt; Überschrift für den Produktnamen s und eine vierspaltige `<table>` für die verbleibenden Felder. Die `ProductPropertyLabel` und `ProductPropertyValue` in definierten CSS-Klassen `Styles.css`, haben im vorherigen Lernprogrammen erläutert wurde. Abbildung 3 zeigt unseren Fortschritt, wenn Sie über einen Browser angezeigt.


[![Der Name, Lieferanten, Kategorie, Status nicht mehr unterstützt und Preis jedes Produkt wird angezeigt.](customizing-the-datalist-s-editing-interface-vb/_static/image8.png)](customizing-the-datalist-s-editing-interface-vb/_static/image7.png)

**Abbildung 3**: der Name, Lieferanten, Kategorie, Status nicht mehr unterstützt und Preis jedes Produkt wird angezeigt ([klicken Sie hier, um das Bild in voller Größe angezeigt](customizing-the-datalist-s-editing-interface-vb/_static/image9.png))


## <a name="step-2-adding-the-web-controls-to-the-editing-interface"></a>Schritt 2: Hinzufügen der Websteuerelemente, die Bearbeitungsoberfläche

Der erste Schritt beim Erstellen von benutzerdefinierten DataList Bearbeitungsoberfläche der erforderlichen Websteuerelemente hinzugefügt wird die `EditItemTemplate`. Insbesondere benötigen wir eine DropDownList für die Kategorie, eine andere für den Lieferanten und ein Kontrollkästchen für den Status nicht mehr unterstützte. Seit der Produktpreis s nicht bearbeitet werden, in diesem Beispiel ist, können wir weiterhin die Anzeige mit einem Label-Steuerelement.

Um die Bearbeitungsoberfläche anzupassen, klicken Sie auf die Verknüpfung Vorlagen bearbeiten das DataList-s-Smarttag, und wählen Sie die `EditItemTemplate` Option aus der Dropdown-Liste. Hinzufügen eine DropDownList auf die `EditItemTemplate` und legen Sie seine `ID` auf `Categories`.


[![DropDownList für die Kategorien hinzufügen](customizing-the-datalist-s-editing-interface-vb/_static/image11.png)](customizing-the-datalist-s-editing-interface-vb/_static/image10.png)

**Abbildung 4**: hinzufügen eine DropDownList für die Kategorien ([klicken Sie hier, um das Bild in voller Größe angezeigt](customizing-the-datalist-s-editing-interface-vb/_static/image12.png))


Als Nächstes aus den DropDownList s smart Tag, die Option Datenquelle auswählen und erstellen Sie eine neue ObjectDataSource mit dem Namen `CategoriesDataSource`. Konfigurieren Sie diese ObjectDataSource verwenden die `CategoriesBLL` Klasse s `GetCategories()` Methode (siehe Abbildung 5). Im nächsten Schritt der DropDownList s Datenquellen Konfigurations-Assistenten für die Datenfelder für jeden fordert `ListItem` s `Text` und `Value` Eigenschaften. Haben Sie die Anzeige der DropDownList der `CategoryName` Feld "Daten" und die Verwendung der `CategoryID` als Wert an, wie in Abbildung 6 veranschaulicht.


[![Erstellen Sie eine neue, mit dem Namen CategoriesDataSource ObjectDataSource](customizing-the-datalist-s-editing-interface-vb/_static/image14.png)](customizing-the-datalist-s-editing-interface-vb/_static/image13.png)

**Abbildung 5**: Erstellen einer neuen ObjectDataSource namens `CategoriesDataSource` ([klicken Sie hier, um das Bild in voller Größe angezeigt](customizing-the-datalist-s-editing-interface-vb/_static/image15.png))


[![Konfigurieren Sie die Anzeige der DropDownList-s und der Wert von Feldern](customizing-the-datalist-s-editing-interface-vb/_static/image17.png)](customizing-the-datalist-s-editing-interface-vb/_static/image16.png)

**Abbildung 6**: Konfigurieren der DropDownList s anzeigen und Feldern ([klicken Sie hier, um das Bild in voller Größe angezeigt](customizing-the-datalist-s-editing-interface-vb/_static/image18.png))


Wiederholen Sie diese Abfolge der Schritte zum Erstellen einer DropDownList für die Lieferanten. Legen Sie die `ID` für diese DropDownList zum `Suppliers` und nennen Sie die ObjectDataSource `SuppliersDataSource`.

Nach dem Hinzufügen der zwei DropDownLists, fügen Sie ein Kontrollkästchen für die nicht mehr unterstützte Status und ein Textfeld für den Namen des Produkts s hinzu. Legen Sie die `ID` s für die Kontrollkästchen und ein Textfeld `Discontinued` und `ProductName`bzw. Fügen Sie einen RequiredFieldValidator, um sicherzustellen, dass der Benutzer einen Wert für den Produktnamen s bereitstellt.

Abschließend fügen Sie die Schaltflächen Aktualisieren und "Abbrechen". Denken Sie daran, dass dieser beiden Schaltflächen unbedingt darauf, ihre `CommandName` Eigenschaften werden festgelegt, zu aktualisieren und bzw. "Abbrechen".

Wahlweise können Sie die Bearbeitungsoberfläche jedoch beliebig anordnen. Ich Ve festgelegt haben, dass der gleiche vierspaltige `<table>` Layout aus der schreibgeschützte Schnittstelle als die folgende deklarative Syntax und Screenshot veranschaulicht:


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample2.aspx)]


[![Die Schnittstelle bearbeiten wird angeordnet, wie die Read-Only-Schnittstelle](customizing-the-datalist-s-editing-interface-vb/_static/image20.png)](customizing-the-datalist-s-editing-interface-vb/_static/image19.png)

**Abbildung 7**: die Bearbeitung-Schnittstelle wird festgelegt, wie die Read-Only-Schnittstelle ([klicken Sie hier, um das Bild in voller Größe angezeigt](customizing-the-datalist-s-editing-interface-vb/_static/image21.png))


## <a name="step-3-creating-the-editcommand-and-cancelcommand-event-handlers"></a>Schritt 3: Erstellen der EditCommand und CancelCommand-Ereignishandler

Derzeit ist keine Databinding-Syntax in der `EditItemTemplate` (mit Ausnahme der `UnitPriceLabel`, die kopiert wurde über wörtlich der `ItemTemplate`). Diese wird vorübergehend die Databinding-Syntax hinzugefügt, aber erste Let s erstellen die Ereignishandler für das DataList s `EditCommand` und `CancelCommand` Ereignisse. Bedenken Sie, dass die Zuständigkeit für die `EditCommand` wird der Ereignishandler für das DataList-Element, dessen Schaltfläche "Bearbeiten" geklickt wurde, die Bearbeitungsoberfläche zu rendern, während die `CancelCommand` s Auftrag zurückzugebenden DataList Datenbankzustands vorab Bearbeitung ist.

Erstellen Sie diese zwei Ereignishandler, und bitten Sie, den folgenden Code verwenden:


[!code-vb[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample3.vb)]

Mit diesen zwei Ereignishandler eingerichtet ist, die Sie auf die Schaltfläche "Bearbeiten" zeigt die Benutzeroberfläche für die Bearbeitung, und klicken auf die Schaltfläche "Abbrechen", die nur-Lese-Modus gibt das bearbeitete Element zurück. Abbildung 8 zeigt die DataList, nachdem auf die Schaltfläche "Bearbeiten" für Chef Anton s Gumbo Mix geklickt wurde. Seit wir Ve noch hinzuzufügende jede Databinding-Syntax, die Bearbeitungsoberfläche der `ProductName` Textfeld ist leer, die `Discontinued` Kontrollkästchen deaktiviert, und die ersten Elemente ausgewählt werden, aus der `Categories` und `Suppliers` DropDownLists.


[![Durch Klicken auf die Bearbeiten Schaltfläche angezeigt, die Bearbeitungsoberfläche](customizing-the-datalist-s-editing-interface-vb/_static/image23.png)](customizing-the-datalist-s-editing-interface-vb/_static/image22.png)

**Abbildung 8**: auf die Schaltfläche "Bearbeiten" zeigt die Benutzeroberfläche für die Bearbeitung ([klicken Sie hier, um das Bild in voller Größe angezeigt](customizing-the-datalist-s-editing-interface-vb/_static/image24.png))


## <a name="step-4-adding-the-databinding-syntax-to-the-editing-interface"></a>Schritt 4: Hinzufügen der DataBinding-Syntax, die Bearbeitungsoberfläche

Um die Anzeige der aktuellen Werte der Product-s Bearbeitungsoberfläche haben, müssen wir Databinding-Syntax verwenden, um die entsprechenden Web Steuerelementwerte Datenfeldwerte zuweisen. Die Databinding-Syntax kann mithilfe des Designers angewendet werden, gehen Sie auf dem Bildschirm Vorlagen bearbeiten und Smarttags steuert die Link-Datenbindungen bearbeiten aus dem Web auswählen. Alternativ kann die Databinding-Syntax direkt mit deklarativem Markup hinzugefügt werden.

Zuweisen der `ProductName` Datenfeld der Spalte Wert der `ProductName` Textfeld s `Text` -Eigenschaft, die `CategoryID` und `SupplierID` Datenfeld der Spalte Werte die `Categories` und `Suppliers` DropDownLists `SelectedValue` Eigenschaften, und die `Discontinued` Datenfeld der Spalte Wert der `Discontinued` Kontrollkästchen s `Checked` Eigenschaft. Rufen Sie nach diesen Änderungen, durch den Designer oder direkt über die deklaratives Markup erneut auf der Seite über einen Browser, und klicken Sie auf die Schaltfläche "Bearbeiten" für Chef Anton s Gumbo Mix. Wie in Abbildung 9 gezeigt, wurde die Databinding-Syntax die aktuellen Werte in das Textfeld, DropDownLists und Kontrollkästchen hinzugefügt.


[![Durch Klicken auf die Bearbeiten Schaltfläche angezeigt, die Bearbeitungsoberfläche](customizing-the-datalist-s-editing-interface-vb/_static/image26.png)](customizing-the-datalist-s-editing-interface-vb/_static/image25.png)

**Abbildung 9**: auf die Schaltfläche "Bearbeiten" zeigt die Benutzeroberfläche für die Bearbeitung ([klicken Sie hier, um das Bild in voller Größe angezeigt](customizing-the-datalist-s-editing-interface-vb/_static/image27.png))


## <a name="step-5-saving-the-user-s-changes-in-the-updatecommand-event-handler"></a>Schritt 5: Speichern der Benutzer-s-Änderungen in die UpdateCommand-Ereignishandler

Wenn der Benutzer ein Produkt bearbeitet und klickt auf die Schaltfläche "Aktualisieren" ein Postback erfolgt und die DataList s `UpdateCommand` -Ereignis ausgelöst. Im Ereignis-Handler müssen, lesen die Werte aus den Websteuerelementen in die `EditItemTemplate` und Schnittstelle mit der BLL, um das Produkt in der Datenbank zu aktualisieren. Sobald gibt es im vorherigen Lernprogrammen betrachtet die `ProductID` des aktualisierten Produkts Zugriff erfolgt über die `DataKeys` Auflistung. Die Felder eingegebenen erfolgt durch Programmgesteuertes Verweisen auf die Web-Steuerelemente, die unter Verwendung `FindControl("controlID")`, wie der folgende Code zeigt:


[!code-vb[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample4.vb)]

Der Code durch consulting beginnt die `Page.IsValid` Eigenschaft, um sicherzustellen, dass alle Validierungssteuerelemente auf der Seite gültig sind. Wenn `Page.IsValid` ist `True`, das bearbeitete Produkt s `ProductID` Wert aus der gelesen wird die `DataKeys` Auflistung und die Dateneingabe, in websteuerungselemente der `EditItemTemplate` programmgesteuert auf die verwiesen wird. Als Nächstes werden die Werte aus dieser Web-Steuerelemente in Variablen, die dann in die entsprechenden übergeben werden gelesen `UpdateProduct` überladen. Nach dem Aktualisieren der Daten, wird das DataList Datenbankzustands vorab Bearbeitung zurückgegeben.

> [!NOTE]
> Ich Ve weggelassen, die Ausnahmebehandlung Logik hinzugefügt, der [Behandlung von BLL- und DAL-Ebene Ausnahmen](handling-bll-and-dal-level-exceptions-vb.md) Lernprogramm, damit der Code und in diesem Beispiel bleibt mit Fokus. Als Übung fügen Sie nach Abschluss dieses Lernprogramms diese Funktionen hinzu.


## <a name="step-6-handling-null-categoryid-and-supplierid-values"></a>Schritt 6: Behandeln von NULL CategoryID und SupplierID Werte

Die Northwind-Datenbank kann für `NULL` Werte für die `Products` Tabelle s `CategoryID` und `SupplierID` Spalten. Unsere Bearbeitung Schnittstelle verfügt über t jedoch derzeit aufzunehmen `NULL` Werte. Wenn Sie versuchen, ein Produkt zu bearbeiten, verfügt ein `NULL` Wert entweder seine `CategoryID` oder `SupplierID` Spalten, wir kommen ein `ArgumentOutOfRangeException` mit einer Fehlermeldung ähnelt: *"Kategorien" hat eine "SelectedValue" ungültig ist, da dies der Fall ist in der Liste der Elemente nicht vorhanden.* Darüber hinaus dort s derzeit keine Möglichkeit, eine Produktkategorie s oder-Lieferanten in Verbindung ändern Wert aus einem nicht-`NULL` -Wert an ein `NULL` eine.

Zur Unterstützung `NULL` Werte für die Kategorie und Lieferanten DropDownLists, müssen wir eine zusätzliche hinzufügen `ListItem`. Ich Ve (keine) als verwenden möchten die `Text` Wert für diesen `ListItem`, aber Sie können sie etwas anderes (z. B. eine leere Zeichenfolge) ändern, wenn d gewünscht. Schließlich müssen, legen Sie die DropDownLists `AppendDataBoundItems` auf `True`; Wenn Sie vergessen, die Kategorien dazu und Lieferanten, die an der DropDownList gebunden, die statisch hinzugefügt überschrieben werden `ListItem`.

Nach diesen Änderungen, das Markup DropDownLists im DataList s `EditItemTemplate` sollte etwa wie folgt aussehen:


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample5.aspx)]

> [!NOTE]
> Statische `ListItem` s DropDownList mithilfe des Designers oder direkt über die deklarative Syntax hinzugefügt werden kann. Beim Hinzufügen einer DropDownList-Element zur Darstellung einer Datenbank `NULL` Wert, achten Sie darauf, dass Sie zum Hinzufügen der `ListItem` über die deklarative Syntax. Bei Verwendung der `ListItem` Auflistungs-Editor im Designer die generierten deklarative Syntax wird weglassen der `Value` Einstellung ganz, wenn eine leere Zeichenfolge, und Erstellen von deklarativem Markup wie zugewiesen: `<asp:ListItem>(None)</asp:ListItem>`. Während dies ungefährlich, suchen Sie möglicherweise die fehlende `Value` bewirkt, dass der DropDownList zum Verwenden der `Text` Eigenschaftswert an seiner Stelle. Das bedeutet, dass bei dieser `NULL` `ListItem` ist ausgewählt, der Wert (None) versucht, das Daten-Feld "Product" zugewiesen werden soll (`CategoryID` oder `SupplierID`, in diesem Lernprogramm), was zu einer Ausnahme führt. Durch das explizite Festlegen `Value=""`, `NULL` Wert zugewiesen wird, das Produkt Datenfeld der Spalte bei der `NULL` `ListItem` ausgewählt ist.


Nehmen Sie einen Moment Zeit, um unseren Fortschritt über einen Browser anzuzeigen. Wenn Sie ein Produkt zu bearbeiten, beachten Sie, dass die `Categories` und `Suppliers` DropDownLists beide haben (keine) die Option am Anfang der DropDownList.


[![Die Kategorien und Lieferanten DropDownLists enthalten Option (None)](customizing-the-datalist-s-editing-interface-vb/_static/image29.png)](customizing-the-datalist-s-editing-interface-vb/_static/image28.png)

**Abbildung 10**: die `Categories` und `Suppliers` DropDownLists enthalten, Option (None) ([klicken Sie hier, um das Bild in voller Größe angezeigt](customizing-the-datalist-s-editing-interface-vb/_static/image30.png))


Speichern (keine) als Datenbank option `NULL` -Wert, die wir zur zurückkehren müssen die `UpdateCommand` -Ereignishandler. Ändern der `categoryIDValue` und `supplierIDValue` Variablen, die NULL-Werte zulassen ganze Zahlen sein, und weisen sie einen Wert außer `Nothing` nur, wenn der DropDownList s `SelectedValue` ist eine leere Zeichenfolge:


[!code-vb[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample6.vb)]

Durch diese Änderung Wert `Nothing` übergebenen werden die `UpdateProduct` BLL-Methode, wenn der Benutzer ausgewählt (keine) aus der Dropdown-Listen, die entspricht option eine `NULL` Datenbank-Wert.

## <a name="summary"></a>Zusammenfassung

In diesem Lernprogramm wurde erläutert, wie Sie eine komplexere Bearbeitung DataList-Benutzeroberfläche zu erstellen, die drei verschiedenen Web Eingabesteuerelemente ein Textfeld, zwei DropDownLists und ein Kontrollkästchen zusammen mit Validierungssteuerelemente enthalten. Beim Erstellen der Schnittstelle bearbeiten, die Schritte sind unabhängig von den Websteuerelementen verwendet wird: starten, indem Sie das DataList s Websteuerelementen hinzugefügt `EditItemTemplate`; verwenden Databinding-Syntax, um die entsprechenden Datenwerte in der angegebenen Feld mit dem entsprechenden Web zuweisen Steuerelementeigenschaften; und in der `UpdateCommand` Ereignishandler, d. h. programmgesteuerten Zugriff auf die Web-Steuerelemente und ihre entsprechenden Eigenschaften, deren Werte in der BLL übergeben.

Für die Erstellung einer Bearbeitung Schnittstelle gibt an, ob es s besteht aus nur Textfelder oder eine Auflistung von anderen Websteuerelemente, achten Sie darauf, dass Sie ordnungsgemäß Datenbank behandeln `NULL` Werte. Wenn für die Kontoführung `NULL` s, es ist unbedingt erforderlich, dass Sie nicht nur eine vorhandene richtigerweise `NULL` Wert in die Bearbeitungsoberfläche, sondern auch, die Sie bieten eine Möglichkeit zum Markieren eines Wert als `NULL`. Für DropDownLists im DataList-Steuerelementen unterstützt, das in der Regel bedeutet hinzufügen einen statischen `ListItem` , deren `Value` -Eigenschaft explizit auf eine leere Zeichenfolge festgelegt (`Value=""`), und viel Code zum Hinzufügen der `UpdateCommand` -Ereignishandler, um zu bestimmen, und zwar unabhängig davon, ob die `NULL``ListItem` ausgewählt wurde.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurden Dennis Patterson, David Suru und Randy Schmidt. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Zurück](adding-validation-controls-to-the-datalist-s-editing-interface-vb.md)
