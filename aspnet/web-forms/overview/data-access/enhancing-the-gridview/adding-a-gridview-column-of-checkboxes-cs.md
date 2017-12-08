---
uid: web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs
title: "Hinzufügen einer GridView-Spalte der Kontrollkästchen (c#) | Microsoft Docs"
author: rick-anderson
description: "Dieses Lernprogramm befasst sich mit GridView-Steuerelements für die Benutzer eine intuitive Methode Auswählen mehrerer Zeilen von der G. bereitstellen eine Spalte der Kontrollkästchen hinzugefügt..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/06/2007
ms.topic: article
ms.assetid: f63a9443-2db0-4f80-8246-840d3e86c2a3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs
msc.type: authoredcontent
ms.openlocfilehash: 4796d5d9fcf1f924e9baa9bc56424a9d719425c9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-gridview-column-of-checkboxes-c"></a>Hinzufügen einer GridView-Spalte der Kontrollkästchen (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunterladen](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_52_CS.exe) oder [PDF herunterladen](adding-a-gridview-column-of-checkboxes-cs/_static/datatutorial52cs1.pdf)

> Dieses Lernprogramm befasst sich mit GridView-Steuerelements für die Benutzer eine intuitive Methode Auswählen mehrerer Zeilen der GridView bereitstellen eine Spalte der Kontrollkästchen hinzugefügt.


## <a name="introduction"></a>Einführung

In dem vorherigen Lernprogramm untersucht wir zum Hinzufügen einer Spalteninhalts von Optionsfeldern an die GridView zu einen bestimmten Datensatz auswählen. Eine Spalte von Optionsfeldern ist eine geeignete Benutzeroberfläche auf, wenn der Benutzer beschränkt ist, darf höchstens ein Element aus dem Raster auswählen. In einigen Fällen sollten wir jedoch, damit der Benutzer eine beliebige Anzahl von Elementen aus dem Raster auswählen kann. Webbasiertes e-Mail-Clients, angezeigt in der Regel z. B. die Liste der Nachrichten mit einer Spalte der Kontrollkästchen. Der Benutzer kann eine beliebige Anzahl von Nachrichten auswählen und führen Sie dann eine Aktion aus, z. B. die e-Mail-Nachrichten in einen anderen Ordner verschieben oder löschen.

In diesem Lernprogramm sehen wir, zum Hinzufügen einer Spalteninhalts mit Kontrollkästchen und wie Sie bestimmen, welche Kontrollkästchen auf Postback eingecheckt wurden. Insbesondere müssen wir ein Beispiel für erstellen, die Benutzeroberfläche des webbasierten e-Mail-Clients eng imitiert. Beispiel mit der enthält einer ausgelagerten GridView Auflisten der Produkte in der `Products` Datenbanktabelle mit einem Kontrollkästchen in jeder Zeile (siehe Abbildung 1). Eine Löschen ausgewählter Produkte Schaltfläche geklickt haben, werden die ausgewählten Produkte gelöscht.


[![Jede Zeile des Produkts enthält ein Kontrollkästchen](adding-a-gridview-column-of-checkboxes-cs/_static/image1.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image1.png)

**Abbildung 1**: jede Produktzeile enthält ein Kontrollkästchen ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-a-gridview-column-of-checkboxes-cs/_static/image2.png))


## <a name="step-1-adding-a-paged-gridview-that-lists-product-information"></a>Schritt 1: Hinzufügen von einem ausgelagerten GridView, die Produktinformationen enthält

Bevor wir das Hinzufügen einer Spalte der Kontrollkästchen kümmern, können Sie s zuerst den Fokus auf die Liste der Produkte in eine GridView, die Paging unterstützt. Öffnen Sie zunächst die `CheckBoxField.aspx` auf der Seite der `EnhancedGridView` Ordner, und ziehen Sie eine GridView aus der Toolbox in den Designer, Festlegen seiner `ID` auf `Products`. Wählen Sie als Nächstes die GridView an eine neue, mit dem Namen ObjectDataSource binden `ProductsDataSource`. Konfigurieren der ObjectDataSource verwenden die `ProductsBLL` -Klasse Aufrufen der `GetProducts()` Methode, um die Daten zurückzugeben. Da diese GridView schreibgeschützt sein wird, legen Sie die Dropdownlisten in der Update-, INSERT-, und Löschen von Registerkarten (keine).


[![Erstellen Sie eine neue, mit dem Namen ProductsDataSource ObjectDataSource](adding-a-gridview-column-of-checkboxes-cs/_static/image2.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image3.png)

**Abbildung 2**: Erstellen einer neuen ObjectDataSource namens `ProductsDataSource` ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-a-gridview-column-of-checkboxes-cs/_static/image4.png))


[![Konfigurieren der ObjectDataSource zum Abrufen von Daten mithilfe der GetProducts()-Methode](adding-a-gridview-column-of-checkboxes-cs/_static/image3.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image5.png)

**Abbildung 3**: Konfigurieren der ObjectDataSource zum Abrufen von Daten mithilfe der `GetProducts()` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-a-gridview-column-of-checkboxes-cs/_static/image6.png))


[![Legen Sie die Dropdownlisten in der Update-, INSERT- und Löschen von Registerkarten auf (keine)](adding-a-gridview-column-of-checkboxes-cs/_static/image4.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image7.png)

**Abbildung 4**: Legen Sie das Dropdown-Listen in aktualisieren, einfügen und Löschen von Registerkarten auf (keine) ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-a-gridview-column-of-checkboxes-cs/_static/image8.png))


Visual Studio wird automatisch nach Abschluss des Assistenten für die Datenquelle konfigurieren BoundColumns und eine CheckBoxColumn für die Felder produktbezogene Daten erstellt. Wie wir im vorherigen Lernprogramm haben, entfernen Sie alle außer den `ProductName`, `CategoryName`, und `UnitPrice` BoundFields, und ändern Sie die `HeaderText` Eigenschaften zum Produkt, Kategorie und Preis. Konfigurieren der `UnitPrice` BoundField, damit der Wert als Währung formatiert ist. Konfigurieren Sie auch die GridView zur Unterstützung von Paging durch Aktivieren des Kontrollkästchens Paging aktivieren, aus dem Smarttag.

Können Sie auch die Benutzeroberfläche für das Löschen der ausgewählten Produkte hinzufügen s an. Hinzufügen einer Schaltfläche Websteuerelement unterhalb der GridView Festlegen seiner `ID` auf `DeleteSelectedProducts` und dessen `Text` Eigenschaft zum Löschen ausgewählten Produkten. Anstatt Produkte tatsächlich aus der Datenbank zu löschen, müssen in diesem Beispiel wird nur angezeigt eine Meldung für die Produkte, die gelöscht wurden. Um dies zu berücksichtigen, fügen Sie eine Bezeichnung Websteuerelement unterhalb der Schaltfläche hinzu. Legen Sie seine ID auf `DeleteResults`deaktivieren, dessen `Text` Eigenschaft, und legen seine `Visible` und `EnableViewState` Eigenschaften `false`.

Nachdem diese Änderungen vorgenommen wurden, sollte die GridView, ObjectDataSource Schaltfläche und Bezeichnung s deklarative Markup ähnlich der folgenden:


[!code-aspx[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample1.aspx)]

Nehmen Sie einen Moment Zeit, um die Seite in einem Browser anzeigen (siehe Abbildung 5). An diesem Punkt sollte der Name, Kategorie und Preis für die ersten zehn Produkte angezeigt werden.


[![Der Name, Kategorie und Preis für die ersten zehn Produkte aufgeführt sind](adding-a-gridview-column-of-checkboxes-cs/_static/image5.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image9.png)

**Abbildung 5**: der Name, Kategorie und Preis für die ersten zehn Produkte aufgeführt sind ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-a-gridview-column-of-checkboxes-cs/_static/image10.png))


## <a name="step-2-adding-a-column-of-checkboxes"></a>Schritt 2: Hinzufügen einer Spalte der Kontrollkästchen

Seit ASP.NET 2.0 eine CheckBoxField enthält, kann eine vorstellen, dass es verwendet werden kann, um eine Spalte der Kontrollkästchen an eine GridView hinzuzufügen. Leider ist, die nicht der Fall, wie CheckBoxField für die Verwendung konzipiert ist mit einem booleschen Datenfeld. Um CheckBoxField verwenden müssen wir also die zugrunde liegende Feld "Daten" angeben, dessen Wert überprüft wird, um zu bestimmen, ob das gerenderte Kontrollkästchen aktiviert ist. Wir können keine CheckBoxField verwenden, um nur eine Spalte mit Kontrollkästchen deaktiviert sind.

Stattdessen müssen wir ein TemplateField hinzufügen und fügen Sie ein CheckBox-Websteuerelement, dessen `ItemTemplate`. Fahren Sie fort, und fügen Sie ein TemplateField auf die `Products` GridView und stellen Sie es mit das erste (linken)-Feld. Aus den GridView s smart Tag, klicken Sie auf den Link Vorlagen bearbeiten aus, und ziehen Sie dann ein Kontrollkästchen-Websteuerelement aus der Toolbox in den `ItemTemplate`. Legen Sie dieses Kontrollkästchen s `ID` Eigenschaft `ProductSelector`.


[![Fügen Sie ein Kontrollkästchen-Websteuerelement, der mit dem Namen ProductSelector zu TemplateField s ItemTemplate hinzu](adding-a-gridview-column-of-checkboxes-cs/_static/image6.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image11.png)

**Abbildung 6**: Fügen Sie ein Kontrollkästchen Web-Steuerelement mit dem Namen `ProductSelector` mit dem TemplateField s `ItemTemplate` ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-a-gridview-column-of-checkboxes-cs/_static/image12.png))


Mit dem TemplateField und Kontrollkästchen Web Steuerelement hinzugefügt wird enthält jede Zeile nun ein Kontrollkästchen. Abbildung 7 zeigt diese Seite, wenn über einen Browser angezeigt werden soll, nachdem die TemplateField und Kontrollkästchen hinzugefügt wurden.


[![Jede Zeile des Produkts enthält nun ein Kontrollkästchen](adding-a-gridview-column-of-checkboxes-cs/_static/image7.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image13.png)

**Abbildung 7**: jede Produktzeile enthält nun ein Kontrollkästchen ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-a-gridview-column-of-checkboxes-cs/_static/image14.png))


## <a name="step-3-determining-what-checkboxes-were-checked-on-postback"></a>Schritt 3: Welche Kontrollkästchen bestimmen in Postback überprüft wurden.

Zu diesem Zeitpunkt haben wir eine Spalte mit Kontrollkästchen jedoch keine Möglichkeit zu bestimmen, welche Kontrollkästchen auf Postback eingecheckt wurden. Wenn die ausgewählten Produkte Löschen geklickt wird, müssen wir jedoch wissen, welche Kontrollkästchen aktiviert wurden, um die Produkte zu löschen.

Die GridView s [ `Rows` Eigenschaft](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.rows.aspx) ermöglicht den Zugriff auf die Datenzeilen in der GridView. Wir können diese Zeilen durchlaufen programmgesteuerten Zugriff auf das CheckBox-Steuerelement, und klicken Sie dann wenden Sie sich an ihre `Checked` Eigenschaft, um zu bestimmen, ob das Kontrollkästchen ausgewählt wurde.

Erstellen Sie einen Ereignishandler für das `DeleteSelectedProducts` Schaltfläche Websteuerelement s `Click` Ereignis und fügen Sie den folgenden Code hinzu:


[!code-csharp[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample2.cs)]

Die `Rows` Eigenschaft gibt eine Auflistung von `GridViewRow` , Zusammensetzung die Datenzeilen des GridView-s-Instanzen. Die `foreach` hier-Schleife zählt dieser Auflistung. Für jede `GridViewRow` -Objekt, die Zeile s Kontrollkästchen erfolgt programmgesteuert mithilfe von `row.FindControl("controlID")`. Wenn das Kontrollkästchen aktiviert ist, der Zeile s entspricht `ProductID` Wert wird abgerufen, von der `DataKeys` Auflistung. In dieser Übung zeigen wir einfach eine informationsmeldung in die `DeleteResults` bezeichnen, obwohl in eine funktionierende Anwendung d wir stattdessen Aufrufen der `ProductsBLL` Klasse s `DeleteProduct(productID)` Methode.

Durch das Hinzufügen dieses ereignishandlers ist jetzt klicken auf die Schaltfläche "Löschen ausgewählter Produkte" wird die `ProductID` s ausgewählter Produkte.


[![Klicken auf die Schaltfläche zum Löschen ausgewählten Produkte werden die ausgewählten Produkte Produkt-IDs aufgeführt.](adding-a-gridview-column-of-checkboxes-cs/_static/image8.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image15.png)

**Abbildung 8**: Wenn das Löschen ausgewählt Produkte geklickt wird die ausgewählten Produkte `ProductID` s aufgelistet werden ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-a-gridview-column-of-checkboxes-cs/_static/image16.png))


## <a name="step-4-adding-check-all-and-uncheck-all-buttons"></a>Schritt 4: Alle aktivieren und deaktivieren Sie alle Schaltflächen hinzufügen

Wenn ein Benutzer aller Produkte auf der aktuellen Seite zu löschen möchte, müssen sie jedes der zehn Kontrollkästchen überprüfen. Wir können diesen Prozess zu beschleunigen, durch Hinzufügen einer alle-Schaltfläche geklickt haben, wählt alle Kontrollkästchen im Raster aus. Eine Schaltfläche deaktivieren Sie alle wäre gleichermaßen hilfreich.

Fügen Sie zwei Schaltfläche Web-Steuerelemente, die Seite, die sie über die GridView platziert werden. Legen Sie die erste s `ID` auf `CheckAll` und seine `Text` Eigenschaft, um alle; das zweite s wird festgelegt `ID` auf `UncheckAll` und dessen `Text` Eigenschaft, um alle deaktivieren.


[!code-aspx[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample3.aspx)]

Als Nächstes erstellen Sie eine Methode in der Code-Behind-Klasse, die mit dem Namen `ToggleCheckState(checkState)` , wenn aufgerufen wird, listet die `Products` GridView s `Rows` Auflistung und legt jede Kontrollkästchen s `Checked` -Eigenschaft auf den Wert des übergebenen in *CheckState*  Parameter.


[!code-csharp[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample4.cs)]

Als Nächstes erstellen Sie `Click` -Ereignishandler für das `CheckAll` und `UncheckAll` Schaltflächen. In `CheckAll` s Ereignishandler, d. h. einfach Aufruf `ToggleCheckState(true)`in `UncheckAll`, rufen Sie `ToggleCheckState(false)`.


[!code-csharp[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample5.cs)]

Mit diesem Code bewirkt, dass einen Postback und überprüft alle Kontrollkästchen in der GridView der Schaltfläche alle überprüfen. Klicken Sie auf alle deaktivieren Sie Auswahl ebenso alle Kontrollkästchen. Abbildung 9 zeigt den Bildschirm, nachdem alle Schaltfläche eingecheckt wurde.


[![Klicken auf die Schaltfläche alle Kontrollkästchen wählt alle Kontrollkästchen](adding-a-gridview-column-of-checkboxes-cs/_static/image9.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image17.png)

**Abbildung 9**: Überprüfen Sie alle Schaltfläche wählt alle Kontrollkästchen klicken ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-a-gridview-column-of-checkboxes-cs/_static/image18.png))


> [!NOTE]
> Ist beim Anzeigen einer Spalte der Kontrollkästchen eine Möglichkeit zur Auswahl oder alle Kontrollkästchen durch Aufheben der Auswahl durch ein Kontrollkästchen in der Kopfzeile. Darüber hinaus alle aktuellen aktivieren / deaktivieren Sie alle Implementierung erfordert ein Postback. Die Kontrollkästchen können aktiviert oder deaktiviert, jedoch vollständig über die Skriptdatei auf Clientseite, wodurch eine snappier Benutzeroberfläche sein. Zum Durchsuchen mit einem Header Datenzeilen-Kontrollkästchen für alle aktivieren und deaktivieren Sie alle im Detail, sowie eine Erläuterung zur Verwendung von clientseitigen Techniken, sehen Sie sich [überprüft alle Kontrollkästchen in GridView unter Verwendung des clientseitigen Skripts und überprüfen Sie alle Kontrollkästchen](http://aspnet.4guysfromrolla.com/articles/053106-1.aspx).


## <a name="summary"></a>Zusammenfassung

Hinzufügen einer Spalte der Kontrollkästchen Fälle, in denen Sie können Benutzer wählen eine beliebige Anzahl von Zeilen aus einer GridView, bevor Sie fortfahren, wird eine Option aus. Wie wir in diesem Lernprogramm gesehen haben, umfasst z. B. eine Spalte der Kontrollkästchen in der GridView ein TemplateField mit einem Kontrollkästchen-Steuerelement hinzufügen. Ein Websteuerelement (im Gegensatz zu Räumen Markup direkt in die Vorlage an, wie im vorherigen Lernprogramm) mit ASP.NET automatisch speichert, welche Kontrollkästchen wurden und nicht über Postback überprüft wurden. Wir können auch programmgesteuert auf die entsprechenden Kontrollkästchen im Code, um zu bestimmen, ob eine angegebene Kontrollkästchen aktiviert ist, oder den Aktivierungszustand ändern zugreifen.

Dieses Lernprogramm und das letzte Lesezeichen Hinzufügen einer Zeile-Selektor-Spalte an die GridView betrachtet. Untersuchen wir in unserer nächsten Lernprogramm wie mit einem Bit der Arbeit, Einfügen von Funktionen die GridView hinzugefügt werden können.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

>[!div class="step-by-step"]
[Zurück](adding-a-gridview-column-of-radio-buttons-cs.md)
[Weiter](inserting-a-new-record-from-the-gridview-s-footer-cs.md)
