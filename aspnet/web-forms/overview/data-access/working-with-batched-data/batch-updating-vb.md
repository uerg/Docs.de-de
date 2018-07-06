---
uid: web-forms/overview/data-access/working-with-batched-data/batch-updating-vb
title: Batches (VB) aktualisieren | Microsoft-Dokumentation
author: rick-anderson
description: Erfahren Sie, wie Sie mehrere Datenbankdatensätze in einem einzelnen Vorgang aktualisieren. In der UI-Schicht erstellen wir eine GridView, in dem jede Zeile bearbeitet werden kann. In den Daten...
ms.author: aspnetcontent
ms.date: 06/26/2007
ms.assetid: d191a204-d7ea-458d-b81c-0b9049ecb55f
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-updating-vb
msc.type: authoredcontent
ms.openlocfilehash: 7e5062898ca683571df2929eba5d824f9d77accd
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37833400"
---
<a name="batch-updating-vb"></a>Batch-Update (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_64_VB.zip) oder [PDF-Datei herunterladen](batch-updating-vb/_static/datatutorial64vb1.pdf)

> Erfahren Sie, wie Sie mehrere Datenbankdatensätze in einem einzelnen Vorgang aktualisieren. In der UI-Schicht erstellen wir eine GridView, in dem jede Zeile bearbeitet werden kann. In der Datenzugriffsebene umschließen wir die mehrere Update-Vorgänge innerhalb einer Transaktion, um sicherzustellen, dass alle Updates erfolgreich ausgeführt oder alle Updates zurückgesetzt werden.


## <a name="introduction"></a>Einführung

In der [vorherigen Lernprogramm](wrapping-database-modifications-within-a-transaction-vb.md) wurde erläutert, wie der Data Access Layer, zum Hinzufügen der Unterstützung für Datenbanktransaktionen zu erweitern. Datenbanktransaktionen garantieren, dass eine Reihe von Anweisungen zur Datenänderung als einem atomaren Vorgang behandelt wird, wird sichergestellt, dass alle Änderungen werden oder alle fehlschlagen wird. Low-Level DAL Funktionalität aus dem Weg bereit wir erneut unsere Aufmerksamkeit für das Erstellen von Batch-Änderung Schnittstellen zu aktivieren.

In diesem Tutorial Erstellen einer GridView-Ansicht wir, in dem jede Zeile bearbeitet werden (siehe Abbildung 1) ist. Da jede Zeile in der Bearbeitung Oberfläche, dort s keine Notwendigkeit für eine Spalte mit der Bearbeitung gerendert wird, aktualisieren Sie und Abbrechen Sie (Schaltflächen). Stattdessen, es gibt zwei Produkte aktualisieren Schaltflächen auf der Seite, die beim Klicken auf die GridView Zeilen aufgelistet, und Aktualisieren der Datenbank.


[![Jede Zeile in der GridView-Ansicht ist bearbeitbar](batch-updating-vb/_static/image1.gif)](batch-updating-vb/_static/image1.png)

**Abbildung 1**: jede Zeile in der GridView-Ansicht ist bearbeitbar ([klicken Sie, um das Bild in voller Größe anzeigen](batch-updating-vb/_static/image2.png))


Lassen Sie s beginnen!

> [!NOTE]
> In der [Durchführen von BatchUpdates](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb.md) Tutorial, die wir erstellt haben, ein Batch-Bearbeitung Schnittstelle mit dem DataList-Steuerelement. In diesem Tutorial unterscheidet, aus dem vorherigen, verwendet er, einer GridView-Ansicht und des BatchUpdates innerhalb des Bereichs einer Transaktion ausgeführt wird. Nach Abschluss dieses Lernprogramms sollten Sie mit dem früheren Tutorial zurück und aktualisieren, um die Datenbank transaktionsbezogenen Funktionalität hinzugefügt, in dem vorherigen Lernprogramm verwenden.


## <a name="examining-the-steps-for-making-all-gridview-rows-editable"></a>Untersuchen die Schritte zum Erstellen der bearbeitet werden alle Zeilen der GridView

Siehe die [eine Übersicht der einfügen, aktualisieren und Löschen von Daten](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) Tutorial GridView bietet integrierte Unterstützung für die Bearbeitung der zugrunde liegenden Daten auf einer Basis pro Zeile. Intern wird die GridView – Anmerkungen zu dieser, welche Zeile über bearbeitet werden kann die [ `EditIndex` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.editindex(VS.80).aspx). Wie Sie mit der Datenquelle die GridView gebunden wird, überprüft er jede Zeile aus, um festzustellen, ob der Index der Zeile der Wert von entspricht `EditIndex`. Wenn dies der Fall ist, diese Zeile s, die Felder mit gerendert werden, deren Bearbeitung-Schnittstellen. Die Bearbeitungsschnittstelle für BoundFields, befindet sich ein Textfeld, dessen `Text` Eigenschaft erhält den Wert des Felds durch die BoundField-s angegebene `DataField` Eigenschaft. Für von TemplateFields die `EditItemTemplate` wird anstelle von verwendet die `ItemTemplate`.

Denken Sie daran, dass es sich bei der Bearbeitung Workflow startet, wenn ein Benutzer eine Zeile s Bearbeiten-Schaltfläche klickt. Dies führt dazu, dass einen Postback, legt die GridView-s `EditIndex` -Eigenschaft auf den geklickt wurde Zeilenindex s, und bindet die Daten in das Raster. Wenn eine Zeile s-Abbrechen-Schaltfläche geklickt wird, beim Postback der `EditIndex` festgelegt ist, auf einen Wert von `-1` vor dem erneuten Binden der Daten in das Raster. Da die GridView-s-Zeilen starten die Indizierung auf 0 (null), festlegen `EditIndex` zu `-1` hat den Effekt die GridView in schreibgeschützten Modus anzuzeigen.

Die `EditIndex` Eigenschaft eignet sich gut für jede Zeile zu bearbeiten, aber es dient nicht zum Bearbeiten von Batch. Damit die gesamte GridView bearbeitet werden kann, muss jede Zeile mit der Bearbeitungsoberfläche gerendert werden. Die einfachste Möglichkeit hierzu ist die Erstellung der, in dem jedes bearbeitbares Feld implementiert wird, wie in ein TemplateField mit der Bearbeitung Schnittstelle definiert die `ItemTemplate`.

In den nächsten Schritten vollständig bearbeitbare GridView erstellen wir. In Schritt 1 wir beginnen mit dem Erstellen der GridView und seine "ObjectDataSource" und seine BoundFields und CheckBoxField in von TemplateFields konvertieren. Wir verschieben in den Schritten 2 und 3 die Bearbeitung Schnittstellen aus der von TemplateFields `EditItemTemplate` s, um ihre `ItemTemplate` s.

## <a name="step-1-displaying-product-information"></a>Schritt 1: Anzeigen von Produktinformationen

Bevor wir fürchten, zum Erstellen einer GridView-Ansicht, in denen Zeilen sind, können bearbeitet werden, können zunächst einfach die Produktinformationen s. Öffnen der `BatchUpdate.aspx` auf der Seite die `BatchData` Ordner, und ziehen Sie einer GridView-Ansicht aus der Toolbox in den Designer. Legen Sie die GridView s `ID` zu `ProductsGrid` und sein Smarttag, auswählen, um die Bindung an eine neue, mit dem Namen "ObjectDataSource" `ProductsDataSource`. Konfigurieren Sie das "ObjectDataSource" zum Abrufen der Daten aus der `ProductsBLL` Klasse s `GetProducts` Methode.


[![Konfigurieren von dem ObjectDataSource-Steuerelement zur Verwendung der ProductsBLL-Klasse](batch-updating-vb/_static/image2.gif)](batch-updating-vb/_static/image3.png)

**Abbildung 2**: Konfigurieren von dem ObjectDataSource-Steuerelement verwenden die `ProductsBLL` Klasse ([klicken Sie, um das Bild in voller Größe anzeigen](batch-updating-vb/_static/image4.png))


[![Abrufen der Produktdaten mithilfe der GetProducts-Methode](batch-updating-vb/_static/image3.gif)](batch-updating-vb/_static/image5.png)

**Abbildung 3**: Rufen Sie die Product-Daten mithilfe der `GetProducts` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](batch-updating-vb/_static/image6.png))


Wie GridView dienen die Funktionen zur pfadänderung von "ObjectDataSource" s auf einer Basis pro Zeile. Um eine Gruppe von Datensätzen zu aktualisieren, müssen wir ein paar Codezeilen in der CodeBehind-Klasse in ASP.NET Seite "s" zu schreiben, der batches von Daten und übergibt sie an die BLL. Legen Sie daher die Dropdownlisten in der "ObjectDataSource"-s Update-, INSERT- und DELETE Registerkarten (keine). Klicken Sie auf "Fertig stellen", um den Assistenten abzuschließen.


[![Legen Sie die Dropdownlisten in der Update-, INSERT- und DELETE werden Registerkarten (keine)](batch-updating-vb/_static/image4.gif)](batch-updating-vb/_static/image7.png)

**Abbildung 4**: Legen Sie die Dropdownlisten in der Update-, INSERT- und Löschen von Registerkarten auf (keine) ([klicken Sie, um das Bild in voller Größe anzeigen](batch-updating-vb/_static/image8.png))


Nach Abschluss des Assistenten konfigurieren von Datenquellen sollten "ObjectDataSource" s deklarative Markup wie folgt aussehen:


[!code-aspx[Main](batch-updating-vb/samples/sample1.aspx)]

Der Assistent zum Konfigurieren von Datenquellen wird auch in Visual Studio um BoundFields und eine CheckBoxField für die Product-Datenfelder in den GridView-Ansicht zu erstellen. In diesem Tutorial können Sie s, die nur dem Benutzer ermöglichen, anzeigen und bearbeiten die Product-s-Name, Kategorie, Preis und nicht mehr unterstützte Status. Entfernen Sie alle außer den `ProductName`, `CategoryName`, `UnitPrice`, und `Discontinued` Felder und benennen Sie die `HeaderText` Eigenschaften der ersten drei Felder Product Category und Preis. Und schließlich überprüfen Sie die Kontrollkästchen Paging aktivieren und die Sortierung zu aktivieren, in das GridView-s-Smarttag.

An diesem Punkt das GridView hat drei BoundFields (`ProductName`, `CategoryName`, und `UnitPrice`) und eine CheckBoxField (`Discontinued`). Wir müssen diese vier Felder in von TemplateFields konvertieren aus, und klicken Sie dann die Bearbeitungsschnittstelle verschieben, aus der TemplateField s `EditItemTemplate` auf seine `ItemTemplate`.

> [!NOTE]
> Erstellen und Anpassen von TemplateFields im behandelt die [Anpassen der Benutzeroberfläche für die Änderung der Daten](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) Tutorial. Begleiten wir durch die Schritte der BoundFields und CheckBoxField in von TemplateFields konvertiert und Schnittstellen definieren, deren Bearbeitung ihrer `ItemTemplate` s, aber wenn Sie Probleme oder eine Auffrischung, Don ' t zögern muss, wieder mit diesen früheren Tutorial zu verweisen.


GridView s Smarttags klicken Sie auf die Spalten bearbeiten-Link, um die Felder (Dialogfeld) zu öffnen. Als nächstes wählen Sie jedes Feld, und klicken Sie auf das konvertierte dieses Feld in ein TemplateField-Link.


![Konvertieren Sie die vorhandenen BoundFields und CheckBoxField in von TemplateFields](batch-updating-vb/_static/image5.gif)

**Abbildung 5**: Konvertieren Sie die vorhandenen BoundFields und CheckBoxField in von TemplateFields


Nun, da jedes Feld ein TemplateField ist, es erneut bereit, um die Bearbeitung zu verschieben-Schnittstelle aus der `EditItemTemplate` s, um die `ItemTemplate` s.

## <a name="step-2-creating-theproductnameunitprice-anddiscontinuedediting-interfaces"></a>Schritt 2: Erstellen der`ProductName`,`UnitPrice`, und`Discontinued`Schnittstellen bearbeiten

Erstellen der `ProductName`, `UnitPrice`, und `Discontinued` bearbeiten die Schnittstellen sind Thema dieses Schritts und recht einfach gehalten wie jede Schnittstelle bereits in das TemplateField s definiert ist `EditItemTemplate`. Erstellen der `CategoryName` bearbeiten-Schnittstelle ist etwas komplizierter, da wir einem DropDownList-Steuerelement die entsprechenden Kategorien zu erstellen müssen. Dies `CategoryName` bearbeiten-Schnittstelle in Schritt 3 angegangen wird.

S Einstieg ermöglichen die `ProductName` TemplateField. Klicken Sie auf den Link "Vorlagen bearbeiten" aus dem GridView-s-Smarttag, und zeigen die `ProductName` TemplateField s `EditItemTemplate`. Wählen Sie das Textfeld, in die Zwischenablage kopieren und fügen Sie ihn in das `ProductName` TemplateField s `ItemTemplate`. Ändern Sie das Textfeld s `ID` Eigenschaft `ProductName`.

Fügen Sie als Nächstes einen RequiredFieldValidator auf die `ItemTemplate` um sicherzustellen, dass der Benutzer einen Wert für jeden Produktnamen s bereitstellt. Legen Sie die `ControlToValidate` Eigenschaft ProductName, die `ErrorMessage` Eigenschaft, um Sie müssen den Namen des Produkts bereitstellen. und die `Text` Eigenschaft \*. Nach dem Herstellen dieser Ergänzungen zu den `ItemTemplate`, Ihr Bildschirm sollte ähnlich wie in Abbildung 6 aussehen.


[![Die ProductName TemplateField jetzt enthält ein Textfeld und einer RequiredFieldValidator](batch-updating-vb/_static/image6.gif)](batch-updating-vb/_static/image9.png)

**Abbildung 6**: die `ProductName` TemplateField enthält nun ein Textfeld und einen RequiredFieldValidator ([klicken Sie, um das Bild in voller Größe anzeigen](batch-updating-vb/_static/image10.png))


Für die `UnitPrice` bearbeiten-Schnittstelle, starten Sie durch Kopieren der im Textfeld die `EditItemTemplate` auf die `ItemTemplate`. Platzieren Sie ein "$" vor das Textfeld und anschließend die `ID` UnitPrice-Eigenschaft und die zugehörige `Columns` Eigenschaft auf 8.

Auch eine CompareValidator zum Hinzufügen der `UnitPrice` s `ItemTemplate` um sicherzustellen, dass der vom Benutzer eingegebene Wert eine gültige Currency-Wert größer als oder gleich 0,00 US-Dollar. Legen Sie das Validierungssteuerelement s `ControlToValidate` Eigenschaft UnitPrice, dessen `ErrorMessage` Eigenschaft, um Sie müssen einen gültigen Currency-Wert eingeben. Sie lassen Währung Symbole., seine `Text` Eigenschaft, um \*, dessen `Type` Eigenschaft `Currency`, dessen `Operator` Eigenschaft, um `GreaterThanEqual`, und die zugehörige `ValueToCompare` Eigenschaft auf 0.


[![Hinzufügen einer CompareValidator, um sicherzustellen, dass der Preis eingegeben haben ein nicht negativer Currency-Wert ist](batch-updating-vb/_static/image7.gif)](batch-updating-vb/_static/image11.png)

**Abbildung 7**: Hinzufügen einer CompareValidator, um sicherzustellen, dass der Preis eingegeben ist ein nicht negativer Währungswert ([klicken Sie, um das Bild in voller Größe anzeigen](batch-updating-vb/_static/image12.png))


Für die `Discontinued` TemplateField können Sie das Kontrollkästchen, die bereits in definiert die `ItemTemplate`. Legen Sie einfach die `ID` zu Discontinued und die zugehörige `Enabled` Eigenschaft `True`.

## <a name="step-3-creating-thecategorynameediting-interface"></a>Schritt 3: Erstellen der`CategoryName`bearbeiten-Schnittstelle

Die Bearbeitungsschnittstelle in die `CategoryName` TemplateField s `EditItemTemplate` enthält ein Textfeld, das den Wert der `CategoryName` Feld "Daten". Wir müssen dies mit einem DropDownList-Steuerelement ersetzen, die die möglichen Kategorien auflistet.

> [!NOTE]
> Die [Anpassen der Benutzeroberfläche für die Änderung der Daten](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) Tutorial enthält eine umfassendere und vollständige Erläuterung der Anpassen einer Vorlage aus, um einem DropDownList-Steuerelement im Gegensatz zu einem Textfeld einzuschließen. Während die hier beschriebenen Schritte abgeschlossen sind, wird ihnen nur sehr knapp angezeigt. Eine ausführlichere Betrachtung erstellen und konfigurieren die Kategorien DropDownList verweisen zurück auf die [Anpassen der Benutzeroberfläche für die Änderung der Daten](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) Tutorial.


Ziehen Sie von einem DropDownList-Steuerelement aus der Toolbox auf die `CategoryName` TemplateField s `ItemTemplate`wird durch das Festlegen der `ID` zu `Categories`. An diesem Punkt würde in der Regel die DropDownList-Steuerelementen-s-Datenquelle über sein Smarttag, definieren wir erstellen eine neue "ObjectDataSource". Dadurch wird jedoch hinzugefügt, dem ObjectDataSource-Steuerelement innerhalb der `ItemTemplate`, die führt zu einer "ObjectDataSource"-Instanz, die für jede GridView-Zeile erstellt. Stattdessen können Sie s, die außerhalb der GridView-s von TemplateFields dem ObjectDataSource-Steuerelement zu erstellen. Beendet die vorlagenbearbeitung, und ziehen Sie ein ObjectDataSource-Steuerelement aus der Toolbox in den Designer unter der `ProductsDataSource` "ObjectDataSource". Benennen Sie die neue "ObjectDataSource" `CategoriesDataSource` und konfigurieren Sie ihn zur Verwendung der `CategoriesBLL` Klasse s `GetCategories` Methode.


[![Konfigurieren von dem ObjectDataSource-Steuerelement zur Verwendung der CategoriesBLL-Klasse](batch-updating-vb/_static/image8.gif)](batch-updating-vb/_static/image13.png)

**Abbildung 8**: Konfigurieren von dem ObjectDataSource-Steuerelement verwenden die `CategoriesBLL` Klasse ([klicken Sie, um das Bild in voller Größe anzeigen](batch-updating-vb/_static/image14.png))


[![Rufen Sie die Kategoriedaten mithilfe der Methode GetCategories ab](batch-updating-vb/_static/image9.gif)](batch-updating-vb/_static/image15.png)

**Abbildung 9**: Rufen Sie die Kategorie Daten mithilfe der `GetCategories` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](batch-updating-vb/_static/image16.png))


Da diese "ObjectDataSource" nur zum Abrufen von Daten verwendet wird, legen Sie die Dropdown-Listen in die Update- und DELETE-Registerkarten, um (keine) aus. Klicken Sie auf "Fertig stellen", um den Assistenten abzuschließen.


[![Gruppe der Dropdownlisten in der UPDATE und DELETE-Registerkarten, um (keine)](batch-updating-vb/_static/image10.gif)](batch-updating-vb/_static/image17.png)

**Abbildung 10**: Legen Sie die Dropdownlisten im aktualisieren und Löschen von Registerkarten auf (keine) ([klicken Sie, um das Bild in voller Größe anzeigen](batch-updating-vb/_static/image18.png))


Nach Abschluss des Assistenten, der `CategoriesDataSource` s deklaratives Markup sollte wie folgt aussehen:


[!code-aspx[Main](batch-updating-vb/samples/sample2.aspx)]

Mit der `CategoriesDataSource` erstellt und konfiguriert haben, zurück zu den `CategoryName` TemplateField s `ItemTemplate` und DropDownList s Smarttags, klicken Sie auf den Link für die Datenquelle auswählen. Wählen Sie im Assistenten zum Konfigurieren von Datenquellen die `CategoriesDataSource` option in der ersten Dropdown-Liste, und entscheiden, dass `CategoryName` verwendet für die Anzeige und `CategoryID` als Wert.


[![Die DropDownList an die CategoriesDataSource binden](batch-updating-vb/_static/image11.gif)](batch-updating-vb/_static/image19.png)

**Abbildung 11**: DropDownList, binden die `CategoriesDataSource` ([klicken Sie, um das Bild in voller Größe anzeigen](batch-updating-vb/_static/image20.png))


An diesem Punkt die `Categories` DropDownList Listet alle Kategorien, aber es nicht noch automatisch wählen Sie die entsprechende Kategorie für das Produkt an die GridView-Zeile gebunden. Um dies zu erreichen, wir legen müssen, die `Categories` DropDownList s `SelectedValue` für das Produkt s `CategoryID` Wert. Klicken Sie auf den Link "DataBindings bearbeiten" aus dem DropDownList-s-Smarttag, und ordnen Sie die `SelectedValue` Eigenschaft mit dem `CategoryID` Datenfeld, wie in Abbildung 12 dargestellt.


![Binden von der Product-s-Kategorie-ID-Wert an die SelectedValue-Eigenschaft der DropDownList-s](batch-updating-vb/_static/image12.gif)

**Abbildung 12**: Binden Sie das Produkt s `CategoryID` Wert, der die DropDownList-Zuordnungsvorgänge `SelectedValue` Eigenschaft


Eine letzte Problem bleibt: Wenn das Produkt t haben eine `CategoryID` Wert angegeben wird die Databinding-Anweisung auf `SelectedValue` führt zu einer Ausnahme. Grund hierfür ist die DropDownList enthält nur die Elemente für die Kategorien und bietet keine Option für diese Produkte mit einem `NULL` Wert für die Datenbank `CategoryID`. Legen Sie zur Behebung des Problems, das DropDownList-s `AppendDataBoundItems` Eigenschaft `True` und Hinzufügen eines neuen Elements zu DropDownList, Auslassen der `Value` Eigenschaft aus der deklarativen Syntax. Stellen Sie also sicher, dass die `Categories` DropDownList s deklarativer Syntax sieht wie folgt aus:


[!code-aspx[Main](batch-updating-vb/samples/sample3.aspx)]

Hinweis wie die `<asp:ListItem Value="">` – wählen Sie eine – verfügt über seine `Value` Attribut explizit festgelegt wird, auf eine leere Zeichenfolge. Verweisen zurück auf die [Anpassen der Benutzeroberfläche für die Änderung der Daten](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) Tutorial für eine ausführlichere Erläuterung der warum dieses zusätzliche DropDownList-Element erforderlich ist, behandelt der `NULL` Fall und warum Zuweisung der `Value` die Eigenschaft auf eine leere Zeichenfolge ist unerlässlich.

> [!NOTE]
> Es ist ein potenzielles Leistung und Skalierbarkeit Problem hier, das erwähnt werden muss. Da jede Zeile mit einem DropDownList-Steuerelement enthält, die verwendet die `CategoriesDataSource` als Datenquelle, die `CategoriesBLL` s-Klasse `GetCategories` aufgerufene Methode *n* Mal pro Seite besuchen, in denen *n* ist die Anzahl der Zeilen in den GridView-Ansicht. Diese *n* Aufrufe von `GetCategories` führen *n* Abfragen an die Datenbank. Diese Auswirkungen auf die Datenbank kann weniger problematisch werden, durch das Zwischenspeichern der zurückgegebenen Kategorien oder in einem Cache pro Anforderung als auch über die Caching-Schicht, die mittels einer SQL-Abhängigkeit oder eine sehr kurze Zeit-basierte Ablauf zwischengespeichert werden. Weitere Informationen zu den pro Anforderung, die Option zum Zwischenspeichern finden Sie unter [ `HttpContext.Items` ein pro-Request-Cache-Store](http://aspnet.4guysfromrolla.com/articles/060904-1.aspx).


## <a name="step-4-completing-the-editing-interface"></a>Schritt 4: Abschließen der Schnittstelle bearbeiten

Wir haben eine Reihe von Änderungen an den GridView-s-Vorlagen ohne Anhalten und unseren Fortschritt anzeigen hergestellt. Nehmen Sie einen Moment Zeit, um unseren Fortschritt über einen Browser anzuzeigen. Wie in Abbildung 13 gezeigt, jede Zeile mit dargestellt wird seine `ItemTemplate`, die die Zelle s bearbeiten-Schnittstelle enthält.


[![Jede GridView-Zeile ist bearbeitbar](batch-updating-vb/_static/image13.gif)](batch-updating-vb/_static/image21.png)

**Abbildung 13**: jede GridView-Zeile ist bearbeitbar ([klicken Sie, um das Bild in voller Größe anzeigen](batch-updating-vb/_static/image22.png))


Es gibt einige kleinere Probleme bei der Formatierung, denen wir an diesem Punkt kümmern müssen. Beachten Sie, dass zuerst die `UnitPrice` Wert enthält vier Dezimalstellen. Um dieses Problem zu beheben, zurück zu den `UnitPrice` TemplateField s `ItemTemplate` und TextBox s Smarttags, klicken Sie auf den Link DataBindings bearbeiten. Als Nächstes angeben, die die `Text` Eigenschaft als Zahl formatiert werden sollen.


![Formatieren von Text-Eigenschaft als eine Zahl](batch-updating-vb/_static/image14.gif)

**Abbildung 14**: Format der `Text` Eigenschaft als Zahl


Zweitens können s zentrieren Sie das Kontrollkästchen in der `Discontinued` Spalte (anstatt es linksbündig ausgerichtet). Klicken Sie auf den Spalten bearbeiten das GridView-s-Smarttag, und wählen Sie die `Discontinued` TemplateField aus der Liste der Felder in der unteren linken Ecke. Drilldown in `ItemStyle` und legen Sie die `HorizontalAlign` Eigenschaft Center, wie in Abbildung 15 dargestellt.


![Zentrieren Sie das Kontrollkästchen nicht mehr unterstützte](batch-updating-vb/_static/image15.gif)

**Abbildung 15**: Center die `Discontinued` Kontrollkästchen


Als Nächstes fügen Sie ein ValidationSummary-Steuerelement auf der Seite, und legen seine `ShowMessageBox` Eigenschaft `True` und die zugehörige `ShowSummary` Eigenschaft `False`. Fügen Sie auch, dass die Schaltfläche "Web steuert, die beim Klicken auf, die Benutzer s Änderungen aktualisiert. Insbesondere zwei Schaltfläche Web Steuerelemente hinzufügen, einen oberhalb der GridView und darunter liegenden beide Steuerelemente festlegen `Text` Eigenschaften, die Update-Produkte.

Seit der GridView-s bearbeiten-Schnittstelle ist definiert in der von TemplateFields `ItemTemplate` s, die `EditItemTemplate` s überflüssig und können gelöscht werden.

Nachdem Formatänderungen machen oben bereits erwähnt, die Schaltflächen-Steuerelemente hinzufügen und entfernen Sie die unnötige `EditItemTemplate` s, sollte die Seite "s" deklarative Syntax wie folgt aussehen:


[!code-aspx[Main](batch-updating-vb/samples/sample4.aspx)]

Abbildung 16 zeigt diese Seite, wenn Sie über einen Browser angezeigt werden soll, nachdem die Schaltfläche "-Web-Steuerelemente hinzugefügt wurden und die Formatierungen vorgenommenen Änderungen.


[![Die Seite jetzt enthält zwei Schaltflächen der Update-Produkte](batch-updating-vb/_static/image16.gif)](batch-updating-vb/_static/image23.png)

**Abbildung 16**: die Seite jetzt umfasst zwei Update-Produkte Schaltflächen ([klicken Sie, um das Bild in voller Größe anzeigen](batch-updating-vb/_static/image24.png))


## <a name="step-5-updating-the-products"></a>Schritt 5: Aktualisieren der Produkte

Wenn ein Benutzer diese Seite besucht werden sie ihre Änderungen vornehmen, und klicken Sie dann auf eine der zwei Schaltflächen, Update-Produkte. An dieser Stelle müssen wir irgendwie speichern Sie die eingegebenen Werte für jede Zeile in einer `ProductsDataTable` -Instanz, und übergeben, die an eine BLL-Methode, die dann besteht, `ProductsDataTable` Instanz der DAL s `UpdateWithTransaction` Methode. Die `UpdateWithTransaction` -Methode, die wir in erstellt die [vorherigen Lernprogramm](wrapping-database-modifications-within-a-transaction-vb.md), stellt sicher, dass der Batch von Änderungen als atomaren Vorgang aktualisiert werden.

Erstellen Sie eine Methode mit dem Namen `BatchUpdate` in `BatchUpdate.aspx.vb` und fügen Sie den folgenden Code hinzu:


[!code-vb[Main](batch-updating-vb/samples/sample5.vb)]

Diese Methode startet indem Sie alle Produkte abrufen in ein `ProductsDataTable` über einen Aufruf an die BLL s `GetProducts` Methode. Zählt dann die `ProductGrid` GridView s [ `Rows` Auflistung](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rows(VS.80).aspx). Die `Rows` Auflistung enthält ein [ `GridViewRow` Instanz](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewrow.aspx) für jede Zeile in der GridView-Ansicht angezeigt. Da wir höchstens zehn Zeilen pro Seite, die GridView s angezeigt werden `Rows` Auflistung wird nicht mehr als zehn Elemente haben.

Für jede Zeile der `ProductID` ist stammt aus der `DataKeys` Sammlung und die entsprechenden `ProductsRow` ausgewählt ist, aus der `ProductsDataTable`. Die vier TemplateField-input-Steuerelemente programmgesteuert auf die verwiesen wird, und ihre Werte zugewiesen, die `ProductsRow` s Instanzeigenschaften. Nach jeder GridView Zeilenwerte s haben wurde zum Aktualisieren der `ProductsDataTable`, es s, die an die BLL s `UpdateWithTransaction` -Methode, die wie im vorherigen Tutorial beschrieben in der DAL s einfach nach unten ruft `UpdateWithTransaction` Methode.

Der Batch-Update-Algorithmus für dieses Tutorial verwendete aktualisiert jede Zeile in der `ProductsDataTable` , entspricht einer Zeile in der GridView, unabhängig davon, ob die Produktinformationen s geändert wurde. Während solche Blind sind t in der Regel ein Leistungsproblem aktualisiert, können sie zu überflüssigen Datensätze führen re-Überwachung in die Datenbanktabelle überschreiten. In der [Durchführen von BatchUpdates](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb.md) Tutorial wir untersucht einen Batch, die Schnittstelle mit DataList-Steuerelement aktualisiert und Code, der nur die Datensätze aktualisiert wird, die tatsächlich vom Benutzer geändert wurden. Gerne verwenden die Techniken von [Durchführen von BatchUpdates](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb.md) so aktualisieren ggf. den Code in diesem Tutorial.

> [!NOTE]
> Beim Binden von der Datenquelle an die GridView über sein Smarttag, weist Visual Studio automatisch Werte für die primären Datenquelle s-Key der GridView-s `DataKeyNames` Eigenschaft. Wenn Sie nicht dem ObjectDataSource-Steuerelement an die GridView über das GridView-s-Smarttag binden, wie in Schritt 1 beschrieben, müssen Sie manuell festlegen, die GridView s `DataKeyNames` Eigenschaft "ProductID", um Zugriff auf die `ProductID` Wert für jede Zeile über die `DataKeys` Auflistung.


Der Code in verwendet `BatchUpdate` ähnelt, verwendet die BLL s `UpdateProduct` Methoden, die der Hauptunterschied besteht darin, die in der `UpdateProduct` Methoden nur eine einzige `ProductRow` Instanz abgerufen wird, von der Architektur. Den Code, der die Eigenschaften des weist die `ProductRow` ist die gleiche Weise die `UpdateProducts` Methoden und den Code in der `For Each` wurde eine Schleife in `BatchUpdate`, da das gesamte Muster.

Um dieses Tutorial abzuschließen, müssen wir haben die `BatchUpdate` Methode wird aufgerufen, wenn eine der Update-Produkte-Schaltflächen geklickt wird. Erstellen von Ereignishandlern für die `Click` Ereignisse dieser beiden Steuerelemente Schaltfläche, und fügen Sie den folgenden Code in den Ereignishandlern:


[!code-vb[Main](batch-updating-vb/samples/sample6.vb)]

Zunächst erfolgt ein Aufruf zum `BatchUpdate`. Als Nächstes die [ `ClientScript` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.page.clientscript(VS.80).aspx) wird verwendet, um das Einfügen von JavaScript, die eine Messagebox anzeigt, die liest die Produkte wurden aktualisiert.

Kurz, diesen Code zu testen. Besuchen Sie `BatchUpdate.aspx` über einen Browser, bearbeiten Sie eine Anzahl von Zeilen, und klicken Sie auf eine der Schaltflächen Update-Produkte. Vorausgesetzt, dass keine Überprüfung der Eingabe-Fehler vorliegen, sollten Sie eine Messagebox sehen, die gelesen, dass die Produkte aktualisiert wurden. Um die Unteilbarkeit des Updates zu überprüfen, erwägen Sie eine zufällige `CHECK` -Einschränkung, wie eine, die lässt keine `UnitPrice` 1234.56 Werte. Klicken Sie dann aus `BatchUpdate.aspx`, bearbeiten Sie eine Anzahl von Datensätzen, zum Festlegen des Produkts s sicherstellen `UnitPrice` Wert, der den unzulässigen Wert (1234.56). Dies sollte zu einem Fehler führen, wenn Update-Produkte mit den anderen Änderungen auf, während der Batchvorgang wieder auf ihre ursprünglichen Werte zurückgesetzt werden.

## <a name="an-alternativebatchupdatemethod"></a>Eine Alternative`BatchUpdate`Methode

Die `BatchUpdate` Methode, die wir gerade untersucht ruft *alle* der Produkte von der BLL s `GetProducts` Methode und aktualisiert dann nur die Datensätze, die in den GridView-Ansicht angezeigt werden. Dieser Ansatz ist ideal, wenn die GridView Paging nicht verwendet, aber wenn dies der Fall ist, gibt es möglicherweise Hunderte, Tausende oder Zehntausende von Produkten, aber nur zehn Zeilen in den GridView-Ansicht. In diesem Fall ist das Abrufen aller Produkte aus der Datenbank nur 10 Zeilen ändern nicht ideal.

Für solche Situationen sollten Sie mit der folgenden `BatchUpdateAlternate` Methode stattdessen:


[!code-vb[Main](batch-updating-vb/samples/sample7.vb)]

`BatchMethodAlternate` gestartet wird, indem Sie erstellen ein neues leeres `ProductsDataTable` mit dem Namen `products`. Anschließend werden die Schritte der GridView-s `Rows` Auflistung und für jede Zeile der bestimmte Produktinformationen mithilfe der BLL s ruft `GetProductByProductID(productID)` Methode. Der abgerufene `ProductsRow` Instanz verfügt über Eigenschaften, die auf dieselbe Weise wie aktualisiert `BatchUpdate`, jedoch erst nach dem Aktualisieren der Zeile, die dem Import in die `products` `ProductsDataTable` über die DataTable s [ `ImportRow(DataRow)` Methode](https://msdn.microsoft.com/library/system.data.datatable.importrow(VS.80).aspx).

Nach der `For Each` Schleife abgeschlossen ist, `products` enthält mindestens ein `ProductsRow` Instanz für jede Zeile in der GridView-Ansicht. Da jede von der `ProductsRow` Instanzen hinzugefügt wurden die `products` (anstelle von aktualisiert), wenn wir damit Blind übergeben der `UpdateWithTransaction` Methode der `ProductsTableAdatper` versucht, jede der Datensätze in der Datenbank eingefügt. Stattdessen müssen wir angeben, dass jede dieser Zeilen (keine hinzugefügt) geändert wurde.

Dies geschieht durch Hinzufügen einer neuen Methode an die BLL, die mit dem Namen `UpdateProductsWithTransaction`. `UpdateProductsWithTransaction`, unten dargestellt, legt die `RowState` aller der `ProductsRow` -Instanzen in der `ProductsDataTable` zu `Modified` und übergibt dann die `ProductsDataTable` der DAL-s `UpdateWithTransaction` Methode.


[!code-vb[Main](batch-updating-vb/samples/sample8.vb)]

## <a name="summary"></a>Zusammenfassung

GridView pro Zeile integrierten Funktionen enthält, aber Sie verfügt nicht über die Unterstützung für das Erstellen vollständig bearbeitbare Schnittstellen. Wie wir in diesem Tutorial gesehen haben, werden solche Schnittstellen sind möglich, aber ein wenig Aufwand erforderlich. Um einer GridView-Ansicht zu erstellen, in dem jede Zeile bearbeitet werden kann, müssen wir die GridView-s-Felder in von TemplateFields konvertieren und definieren die Bearbeitungsschnittstelle innerhalb der `ItemTemplate` s. Aktualisieren Sie darüber hinaus alle - Schaltfläche websteuerungselemente, hinzugefügt werden auf der Seite getrennt von der GridView. Diese Schaltflächen `Click` Ereignishandler müssen zum Aufzählen der GridView-s `Rows` -Auflistung, speichern Sie die Änderungen in einer `ProductsDataTable`, und übergeben die aktualisierte Informationen in die entsprechende BLL-Methode.

Im nächsten Tutorial sehen wir, wie Sie eine Schnittstelle für das Löschen von Batch zu erstellen. Insbesondere jeder GridView-Zeile wird ein Kontrollkästchen und nicht alle aktualisieren – geben Sie Schaltflächen, haben wir dann die Schaltflächen der ausgewählten Zeilen löschen.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial wurden Teresa Murphy und David Suru. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](wrapping-database-modifications-within-a-transaction-vb.md)
> [Weiter](batch-deleting-vb.md)
