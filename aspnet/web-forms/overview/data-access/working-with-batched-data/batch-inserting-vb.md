---
uid: web-forms/overview/data-access/working-with-batched-data/batch-inserting-vb
title: Batch einfügen (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Erfahren Sie, wie Sie mehrere Datenbankdatensätze in einem einzigen Vorgang einfügen. In der Benutzeroberflächenebene erweitern wir die GridView, damit der Benutzer zur Eingabe von mehreren n kann...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: 48e2a4ae-77ca-4208-a204-c38c690ffb59
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-inserting-vb
msc.type: authoredcontent
ms.openlocfilehash: 17a077ed0124a0a9e06c90d0ac137958693fc30e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390087"
---
<a name="batch-inserting-vb"></a>Batch einfügen (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_66_VB.zip) oder [PDF-Datei herunterladen](batch-inserting-vb/_static/datatutorial66vb1.pdf)

> Erfahren Sie, wie Sie mehrere Datenbankdatensätze in einem einzigen Vorgang einfügen. In die UI-Schicht erweitern wir die GridView, um den Benutzer zur Eingabe der mehrere neue Datensätze zu ermöglichen. In der Datenzugriffsebene umschließen wir mehrere Einfügevorgänge innerhalb einer Transaktion, um sicherzustellen, dass alle erfolgreich ausgeführt werden oder ein aller einfügungen Rollback.


## <a name="introduction"></a>Einführung

In der [BatchUpdates](batch-updating-vb.md) Tutorial erläutert, Anpassen der GridView-Steuerelement, um eine Schnittstelle zu präsentieren, in denen mehrere Datensätze bearbeitet wurden. Der Benutzer, die auf der Seite konnte stellen eine Reihe von Änderungen und führen Sie dann mit einem einzigen Mausklick, einem BatchUpdate. In Situationen, in denen Benutzer häufig viele Datensätze in einer Aktion aktualisieren, kann eine solche Schnittstelle unzählige klickt auf Speichern, und Tastatur, Maus Kontextwechsel im Vergleich zu Standard pro Zeile Bearbeitungsfunktionen kennengelernt, die zuerst wieder untersucht wurden die [ein Übersicht über das Einfügen, aktualisieren und Löschen von Daten](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) Tutorial.

Dieses Konzept kann auch angewendet werden, wenn Sie Datensätze hinzufügen. Stellen Sie sich vor, die hier bei Northwind Traders wir häufig erhalten Lieferungen von Lieferanten, die eine Reihe von Produkten für eine bestimmte Kategorie enthalten. Beispielsweise können wir eine Lieferung von sechs verschiedenen Tee und Kaffee Produkte von Tokio Traders erhalten. Wenn ein Benutzer die sechs Produkten, die eine zu einem Zeitpunkt über ein DetailsView-Steuerelement eingibt, werden sie auf den gleichen Werten viele immer wieder haben: sie müssen die gleiche Kategorie (Getränke), die demselben Lieferanten (Tokio Traders) auswählen, die den gleichen Wert (nicht mehr unterstützt "False"), und den gleichen Einheiten auf Order-Wert (0). Diese repetitiven Daten-Eintrag ist nicht nur Zeit in Anspruch nehmen, aber anfällig für Fehler.

Mit ein wenig Aufwand können wir einen Batch, die einfügen-Schnittstelle, den Benutzer den Lieferanten und die Kategorie, geben Sie eine Reihe von Produktnamen und einem Stückpreis ermöglicht, und klicken Sie dann auf eine Schaltfläche zum Hinzufügen von neuen Produkte in der Datenbank, erstellen (siehe Abbildung 1). Jedes Produkt hinzugefügt wird, dessen `ProductName` und `UnitPrice` Datenfelder werden in die Textfelder eingegebenen Werte zugewiesen während seiner `CategoryID` und `SupplierID` Werte werden die Werte aus der DropDownList-Steuerelementen auf der obersten fo Form zugewiesen. Die `Discontinued` und `UnitsOnOrder` Werte werden festgelegt, auf die hartcodierten Werte der `False` und 0 (null) bzw.


[![Die Batch-einfügen-Schnittstelle](batch-inserting-vb/_static/image2.png)](batch-inserting-vb/_static/image1.png)

**Abbildung 1**: die Batch-einfügen-Schnittstelle ([klicken Sie, um das Bild in voller Größe anzeigen](batch-inserting-vb/_static/image3.png))


In diesem Tutorial erstellen wir eine Seite, die den Batch-Benutzeroberfläche in Abbildung 1 einfügen implementiert. Als mit den beiden vorherigen Tutorials werden wir die einfügungen innerhalb des Bereichs einer Transaktion, um die Unteilbarkeit sicherzustellen umschließen. Lassen Sie s beginnen!

## <a name="step-1-creating-the-display-interface"></a>Schritt 1: Erstellen der Anzeigenschnittstelle

In diesem Tutorial besteht aus einer einzelnen Seite, die in zwei Bereiche unterteilt ist: eine Anzeige und eine Region einfügen. Die Anzeigenschnittstelle, die wir in diesem Schritt erstellen, werden die Produkte in einer GridView-Ansicht angezeigt und enthält eine Schaltfläche mit dem Titel mit dem Produktversand Prozess. Wenn diese Schaltfläche geklickt wird, wird die Anzeigenschnittstelle durch die einfügen-Schnittstelle, ersetzt, das in Abbildung 1 dargestellt ist. Die Anzeigenschnittstelle, die nach der hinzufügen-Produkte aus Lieferung zurückgibt oder "Abbrechen"-Schaltflächen geklickt wird. Wir erstellen die einfügende-Schnittstelle in Schritt2.

Beim Erstellen einer Seite mit zwei Schnittstellen, von denen nur eine zu einem Zeitpunkt sichtbar ist, jede Schnittstelle in der Regel befindet sich innerhalb einer [Panel-Websteuerelement](http://www.w3schools.com/aspnet/control_panel.asp), der als Container für andere Steuerelemente dient. Aus diesem Grund müssen die Seite zwei Panel-Steuerelemente eine für jede Schnittstelle.

Öffnen Sie zunächst die `BatchInsert.aspx` auf der Seite die `BatchData` Ordner, und ziehen Sie ein Panel aus der Toolbox in den Designer (siehe Abbildung 2). Legen Sie den Zugriffsbereich s `ID` Eigenschaft `DisplayInterface`. Wenn Sie im Bereich in den Designer, Hinzufügen der `Height` und `Width` Eigenschaften auf 50px 125px, bzw. festgelegt werden. Löschen Sie die Werte dieser Eigenschaften im Eigenschaftenfenster.


[![Ziehen Sie ein Panel aus der Toolbox in den Designer](batch-inserting-vb/_static/image5.png)](batch-inserting-vb/_static/image4.png)

**Abbildung 2**: Ziehen Sie ein Panel aus der Toolbox in den Designer ([klicken Sie, um das Bild in voller Größe anzeigen](batch-inserting-vb/_static/image6.png))


Als Nächstes ziehen Sie eine Schaltfläche und GridView-Steuerelement in den Bereich ein. Legen Sie die s `ID` Eigenschaft `ProcessShipment` und die zugehörige `Text` Eigenschaft, um den Prozess mit dem Produktversand. Legen Sie die GridView s `ID` Eigenschaft `ProductsGrid` und von sein Smarttag, binden Sie es an eine neue, mit dem Namen "ObjectDataSource" `ProductsDataSource`. Konfigurieren Sie zum Abrufen der Daten aus dem ObjectDataSource-Steuerelement die `ProductsBLL` Klasse s `GetProducts` Methode. Da diese GridView nur zum Anzeigen von Daten verwendet wird, legen Sie die Dropdownlisten in der Update-, INSERT-, und Löschen von Registerkarten (keine) aus. Klicken Sie auf "Fertig stellen", um das Konfigurieren von Datenquellen-Assistenten zu beenden.


[![Anzeigen der Daten, die von der ProductsBLL Klasse s GetProducts-Methode zurückgegeben](batch-inserting-vb/_static/image8.png)](batch-inserting-vb/_static/image7.png)

**Abbildung 3**: Anzeigen der Daten, die von zurückgegeben der `ProductsBLL` Klasse s `GetProducts` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](batch-inserting-vb/_static/image9.png))


[![Legen Sie die Dropdownlisten in der Update-, INSERT- und DELETE werden Registerkarten (keine)](batch-inserting-vb/_static/image11.png)](batch-inserting-vb/_static/image10.png)

**Abbildung 4**: Legen Sie die Dropdownlisten in der Update-, INSERT- und Löschen von Registerkarten auf (keine) ([klicken Sie, um das Bild in voller Größe anzeigen](batch-inserting-vb/_static/image12.png))


Nach Abschluss des Assistenten "ObjectDataSource" wird in Visual Studio BoundFields und eine CheckBoxField für die Datenfelder Produkt hinzugefügt werden. Entfernen Sie alle außer den `ProductName`, `CategoryName`, `SupplierName`, `UnitPrice`, und `Discontinued` Felder. Alle ästhetischen Anpassungen vornehmen können. Ich habe mich entschieden, zum Formatieren der `UnitPrice` Felds als Währungswert, neu angeordnet, die Felder und einige Felder umbenannt `HeaderText` Werte. Außerdem konfigurieren Sie GridView Einbeziehung Auslagern und Sortieren von Unterstützung durch die Kontrollkästchen aktivieren, Paging und Sortieren aktivieren in das GridView-s-Smarttag überprüfen.

Nach dem Hinzufügen der Steuerelemente Panel "," Schaltfläche "," GridView "und" ObjectDataSource-Steuerelement, und die GridView-s-Felder anpassen, sollte im deklarativen Markup Ihrer Seite s etwa wie folgt aussehen:


[!code-aspx[Main](batch-inserting-vb/samples/sample1.aspx)]

Beachten Sie, die das Markup für die Schaltfläche und ein GridView angezeigt werden, innerhalb des öffnenden und schließenden `<asp:Panel>` Tags. Da diese Steuerelemente in sind die `DisplayInterface` Bereich können wir dies ausblenden, indem Sie einfach im Bereich s festlegen `Visible` Eigenschaft `False`. Schritt 3 untersucht die programmgesteuerte Änderung Bereich s `Visible` Eigenschaft als Reaktion auf eine Schaltfläche klicken, um eine Schnittstelle zu anzuzeigen, während die andere ausblenden.

Nehmen Sie einen Moment Zeit, um unseren Fortschritt über einen Browser anzuzeigen. Wie in Abbildung 5 gezeigt, sehen Sie eine Prozess mit dem Produktversand oberhalb einer GridView-Ansicht auf die Schaltfläche, die die zehn Produkte zu einem Zeitpunkt auflistet.


[![GridView Listet die Produkte und bietet, Sortieren und Paging-Funktionen](batch-inserting-vb/_static/image14.png)](batch-inserting-vb/_static/image13.png)

**Abbildung 5**: GridView Listet die Produkte und bietet sortieren und Paging-Funktionen ([klicken Sie, um das Bild in voller Größe anzeigen](batch-inserting-vb/_static/image15.png))


## <a name="step-2-creating-the-inserting-interface"></a>Schritt 2: Erstellen der einfügen-Schnittstelle

Mit der Anzeige abgeschlossen ist, es erneut bereit, die die einfügende Benutzeroberfläche erstellen. Können Sie für dieses Tutorial eine Einfügen von Schnittstelle zu erstellen, die einen Einzelwert Supplier "und" Kategorie Eingabeaufforderung aus, und klicken Sie dann kann der Benutzer zur Eingabe von bis zu fünf Produktnamen und Werte von planungseinheiten Preis s ein. Mit dieser Schnittstelle kann Benutzer bis zu fünf neue Produkte hinzufügen, die alle Teilen der gleichen Kategorie und Lieferanten, aber einen eindeutigen Product und Preise.

Beginnen Sie mit ziehen ein Panel aus der Toolbox auf den Designer, und platzieren es unterhalb der vorhandenen `DisplayInterface` Bereich. Legen Sie die `ID` Eigenschaften der neu hinzugefügten Bereich `InsertingInterface` und legen Sie seine `Visible` Eigenschaft `False`. Fügen wir Code, der festlegt der `InsertingInterface` Bereich s `Visible` Eigenschaft `True` in Schritt 3. Löschen Sie auch, den Bereich s `Height` und `Width` Eigenschaftswerte.

Als Nächstes müssen wir die einfügende-Schnittstelle zu erstellen, die in Abbildung 1 gezeigt wurde. Diese Schnittstelle kann mithilfe einer Vielzahl von HTML-Verfahren erstellt werden, aber wir verwenden eine ziemlich einfach: eine Tabelle vier Spalten und sieben Zeilen.

> [!NOTE]
> Bei der Eingabe von Markup für HTML `<table>` Elemente, die von mir bevorzugte die Datenquellensicht an. Während Visual Studio-Tools für das Hinzufügen von verfügt `<table>` Elemente über den Designer, der Designer scheint alles zu möchte einfügen ungefragt für `style` Einstellungen in das Markup. Nachdem ich haben die `<table>` Markup ich normalerweise zurück zum Designer, um die Web-Steuerelemente hinzufügen und ihre Eigenschaften festlegen. Beim Erstellen von Tabellen mit vordefinierten Spalten und Zeilen ich verwende lieber statischen HTML-Code anstelle der [Tabelle Websteuerelement](https://msdn.microsoft.com/library/system.web.ui.webcontrols.table.aspx) da Websteuerelementen zu wechseln, in ein Table-Steuerelement nur zugegriffen werden können, mit der `FindControl("controlID")` Muster. Ich, verwenden jedoch Tabelle Websteuerelemente für dynamisch große Tabellen (diejenigen, deren Zeilen oder Spalten für eine Datenbank oder benutzerspezifische Kriterien basieren), da die Web-Tabelle, die Steuerelement programmgesteuert erstellt werden kann.


Geben Sie das folgende Markup innerhalb der `<asp:Panel>` Tags der `InsertingInterface` Bereich:


[!code-html[Main](batch-inserting-vb/samples/sample2.html)]

Dies `<table>` Markup umfasst keine Steuerelemente des Web- noch, diese werden vorübergehend hinzugefügt. Beachten Sie, dass jedes `<tr>` Element enthält eine bestimmte CSS-Klasse-Einstellung: `BatchInsertHeaderRow` für die Headerzeile, in dem der Lieferant und DropDownList-Steuerelementen geht; `BatchInsertFooterRow` für die Fußzeile, in denen die Produkte hinzufügen von Lieferung und die Schaltflächen Abbrechen geht; und die abwechselnden `BatchInsertRow` und `BatchInsertAlternatingRow` Werte für die Zeilen, die das Produkt und Komponententests enthält Preis TextBox-Steuerelemente. Ich Ve erstellt die entsprechende CSS-Klassen in der `Styles.css` Datei, die der einfügen-Schnittstelle eine ähnlich wie GridView und DetailsView aussehen zu verleihen steuert wir in diesen Tutorials verwendet haben. Diese CSS-Klassen werden unten angezeigt.


[!code-css[Main](batch-inserting-vb/samples/sample3.css)]

Mit diesem Markup eingegeben haben zurück zur Entwurfsansicht. Dies `<table>` sollte als Tabelle vier Spalten, die sieben Zeilen im Designer angezeigt, wie in Abbildung 6 gezeigt.


[![Die einfügen-Schnittstelle besteht aus einer vier-Spalte, Tabelle mit sieben Zeilen](batch-inserting-vb/_static/image17.png)](batch-inserting-vb/_static/image16.png)

**Abbildung 6**: die einfügen-Schnittstelle besteht aus einer vier-Spalte, Tabelle mit sieben Zeilen ([klicken Sie, um das Bild in voller Größe anzeigen](batch-inserting-vb/_static/image18.png))


Wir erneut jetzt bereit, das Einfügen von Schnittstelle Websteuerelemente hinzu. Ziehen Sie zwei DropDownList-Steuerelementen aus der Toolbox in die entsprechenden Zellen in der Tabelle für den Lieferanten und eine für die Kategorie ein.

Legen Sie den Lieferanten DropDownList s `ID` Eigenschaft `Suppliers` und binden sie an eine neue, mit dem Namen "ObjectDataSource" `SuppliersDataSource`. Konfigurieren Sie die neue "ObjectDataSource" zum Abrufen der Daten aus der `SuppliersBLL` Klasse s `GetSuppliers` Methode, und legen Sie das UPDATE Registerkarte s Dropdown-Liste (keine). Klicken Sie auf "Fertig stellen", um den Assistenten abzuschließen.


[![Konfigurieren von dem ObjectDataSource-Steuerelement zur Verwendung der SuppliersBLL Klasse s GetSuppliers-Methode](batch-inserting-vb/_static/image20.png)](batch-inserting-vb/_static/image19.png)

**Abbildung 7**: Konfigurieren von dem ObjectDataSource-Steuerelement verwenden die `SuppliersBLL` s-Klasse `GetSuppliers` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](batch-inserting-vb/_static/image21.png))


Haben die `Suppliers` DropDownList-Anzeige der `CompanyName` Feld "Daten" und die Verwendung der `SupplierID` Datenfeld als seine `ListItem` s Werte.


[![Das Feld CompanyName-Daten anzeigen und SupplierID als Wert verwenden](batch-inserting-vb/_static/image23.png)](batch-inserting-vb/_static/image22.png)

**Abbildung 8**: Anzeige der `CompanyName` Datenfeld und Verwendung `SupplierID` als Wert ([klicken Sie, um das Bild in voller Größe anzeigen](batch-inserting-vb/_static/image24.png))


Namen der zweiten Dropdownliste `Categories` und binden sie an eine neue, mit dem Namen "ObjectDataSource" `CategoriesDataSource`. Konfigurieren der `CategoriesDataSource` "ObjectDataSource" Verwenden der `CategoriesBLL` Klasse s `GetCategories` Methode, die Gruppe, die die Dropdownlisten in der Update- und DELETE Registerkarten (keine) und klicken Sie auf Fertig stellen, um den Assistenten abzuschließen. Schließlich müssen die DropDownList-Anzeige der `CategoryName` Feld "Daten" und die Verwendung der `CategoryID` als Wert.

Nachdem diese zwei DropDownList-Steuerelementen hinzugefügt und entsprechend konfigurierten ObjectDataSources gebunden wurden, sollte Ihr Bildschirm Abbildung 9 ähneln.


[![Die Kopfzeile enthält jetzt den Lieferanten und Kategorien DropDownList-Steuerelementen](batch-inserting-vb/_static/image26.png)](batch-inserting-vb/_static/image25.png)

**Abbildung 9**: der Header Zeile enthält jetzt die `Suppliers` und `Categories` DropDownList-Steuerelementen ([klicken Sie, um das Bild in voller Größe anzeigen](batch-inserting-vb/_static/image27.png))


Wir müssen jetzt erstellen Sie die Textfelder ein, um die Namen und den Preis für jedes neue Produkt zu sammeln. Ziehen Sie ein TextBox-Steuerelement aus der Toolbox in den Designer für jeden der fünf Product Name und Preis Zeilen. Legen Sie die `ID` Eigenschaften der TextBox-Elemente zu `ProductName1`, `UnitPrice1`, `ProductName2`, `UnitPrice2`, `ProductName3`, `UnitPrice3`und so weiter.

Hinzufügen einer CompareValidator nach jedem des Einzelpreises Textfelder ein, und Festlegen der `ControlToValidate` -Eigenschaft auf die entsprechende `ID`. Auch festlegen, die `Operator` Eigenschaft `GreaterThanEqual`, `ValueToCompare` auf 0 (null) und `Type` zu `Currency`. Diese Einstellungen weisen CompareValidator, stellen Sie sicher, dass der Preis, wenn eingegeben haben, einen gültigen Currency-Wert, der größer als oder gleich 0 (null) ist. Legen Sie die `Text` Eigenschaft \*, und `ErrorMessage` mit dem Preis muss größer als oder gleich 0 (null) sein. Lassen Sie zudem alle Währungssymbole.

> [!NOTE]
> Die einfügende-Schnittstelle umfasst keine RequiredFieldValidator-Steuerelemente, obwohl die `ProductName` -Feld in der `Products` Datenbanktabelle lässt keine `NULL` Werte. Dies ist das da soll den Benutzer bis zu fünf Produkten eingeben können. Beispielsweise würde der Benutzer die ersten drei Zeilen den Produktpreis, Name und Unit bereit, die letzten beiden Zeilen leer lassen, fügen wir hinzu d nur drei neue Produkte an das System. Da `ProductName` ist erforderlich, aber wir müssen programmgesteuert überprüfen, um, dass bei einen Preis je Einheit sicherzustellen, dass eingegeben werden, dass ein entsprechende Product Name-Wert angegeben wird. Wir werden diese Prüfung in Schritt 4 in Angriff nehmen.


Bei der Validierung der Benutzereingabe s meldet CompareValidator ungültige Daten auf, wenn der Wert ein Währungssymbol enthält. Fügen Sie ein "$" vor jeder des Einzelpreises Textfelder als einen visuellen Hinweis zu dienen, die der Benutzer, um das Währungssymbol zu unterdrücken, bei der Eingabe des Preis angewiesen.

Abschließend fügen Sie ein ValidationSummary-Steuerelement in der `InsertingInterface` Einstellungen im Bereich der `ShowMessageBox` Eigenschaft, um `True` und die zugehörige `ShowSummary` Eigenschaft, um `False`. Mit diesen Einstellungen Wenn der Benutzer eine ungültige Einheit Preis eingibt, ein Sternchen wird neben dem betreffenden TextBox-Steuerelemente angezeigt und die ValidationSummary zeigt eine Client-Side-Messagebox, die die Fehlermeldung anzeigt, die wir zuvor angegeben.

Ihr Bildschirm sollte an diesem Punkt ähnelt Abbildung 10 aussehen.


[![Die Einfügen von-Schnittstelle enthält jetzt die Textfelder für die Produkte Namen und Preise](batch-inserting-vb/_static/image29.png)](batch-inserting-vb/_static/image28.png)

**Abbildung 10**: die einfügen-Schnittstelle nun enthält Textfelder für die Namen der Produkte und Preise ([klicken Sie, um das Bild in voller Größe anzeigen](batch-inserting-vb/_static/image30.png))


Als Nächstes müssen wir den Produkten Hinzufügen von Schaltflächen "Lieferung" und "Abbrechen", das die Footerzeile hinzufügen. Ziehen Sie zwei Schaltflächen-Steuerelemente aus der Toolbox in die Fußzeile der einfügen-Schnittstelle, die Schaltflächen festlegen `ID` Eigenschaften `AddProducts` und `CancelButton` und `Text` Eigenschaften, die Produkte von Lieferungen und "Abbrechen", bzw. hinzugefügt. Legen Sie außerdem die `CancelButton` Steuerelement s `CausesValidation` Eigenschaft `false`.

Abschließend müssen wir ein Label-Steuerelement hinzufügen, die statusmeldungen für die beiden Schnittstellen angezeigt wird. Z. B. wenn ein Benutzer erfolgreich eine neue Lieferung von Produkten hinzufügt, möchten wir auf die Anzeigenschnittstelle zurück und zeigt eine bestätigungsmeldung an. Wenn Sie jedoch der Benutzer einen Preis für ein neues Produkt, aber aufhört der Name des Produkts enthält, müssen wir eine Warnung angezeigt, da die `ProductName` ist ein Pflichtfeld. Da wir diese Meldung, die für beide Schnittstellen angezeigt benötigen, platzieren Sie es am oberen Rand der Seite außerhalb der Bereiche.

Ziehen Sie ein Label-Steuerelement aus der Toolbox auf den oberen Rand der Seite im Designer. Festlegen der `ID` Eigenschaft, um `StatusLabel`, deaktivieren Sie die `Text` -Eigenschaft, und legen die `Visible` und `EnableViewState` Eigenschaften `False`. Wie wir in vorherigen Tutorials gesehen haben, Festlegen der `EnableViewState` Eigenschaft `False` können programmgesteuert die Bezeichnung s Eigenschaftswerte ändern und wieder die Standardwerte für die nachfolgenden Postbacks automatisch zurückgesetzt. Dies vereinfacht den Code für eine Statusmeldung angezeigt, als Reaktion auf eine Benutzeraktion, die die nachfolgenden beim Postback nicht mehr angezeigt. Legen Sie schließlich die `StatusLabel` Steuerelement s `CssClass` Eigenschaft in "Warnung", die den Namen einer CSS-Klasse darstellt, die in definierten `Styles.css` , Text in einer großen, kursiv, fett, Rot Schriftart anzeigt.

Abbildung 11 zeigt Visual Studio-Designer aus, nachdem die Bezeichnung hinzugefügt und konfiguriert wurde.


[![Platzieren Sie das Steuerelement StatusLabel über die zwei Panel-Steuerelemente](batch-inserting-vb/_static/image32.png)](batch-inserting-vb/_static/image31.png)

**Abbildung 11**: Ort der `StatusLabel` Steuerelement über die zwei Panel-Steuerelemente ([klicken Sie, um das Bild in voller Größe anzeigen](batch-inserting-vb/_static/image33.png))


## <a name="step-3-switching-between-the-display-and-inserting-interfaces"></a>Schritt 3: Wechseln zwischen der Anzeige und Einfügen von Schnittstellen

An diesem Punkt haben wir das Markup für unsere Anzeige und das Einfügen von Schnittstellen, sondern wir erneut noch läuft in zwei Aufgaben:

- Wechseln zwischen der Anzeige und das Einfügen von Schnittstellen
- Die Produkte hinzufügen in der Lieferung in der Datenbank

Derzeit die Anzeigenschnittstelle sichtbar ist, aber die einfügende-Schnittstelle ist ausgeblendet. Grund hierfür ist die `DisplayInterface` Bereich s `Visible` -Eigenschaftensatz auf `True` (der Standardwert), während die `InsertingInterface` Bereich s `Visible` -Eigenschaftensatz auf `False`. So wechseln Sie zwischen den beiden Schnittstellen wir lediglich zum Umschalten der einzelnen Steuerelemente s müssen `Visible` -Eigenschaftswert.

Wir möchten, wechseln von der Anzeigenschnittstelle, auf die einfügen-Schnittstelle, wenn der Prozess mit dem Produktversand geklickt wird. Aus diesem Grund erstellen Sie einen Ereignishandler für diese Schaltfläche s `Click` Ereignis, das den folgenden Code enthält:


[!code-vb[Main](batch-inserting-vb/samples/sample4.vb)]

Dieser Code einfach Blendet die `DisplayInterface` Bereich und zeigt die `InsertingInterface` Bereich.

Als Nächstes erstellen Sie Ereignishandler für die Produkte hinzufügen von Steuerelementen von Lieferung und die Schaltfläche "Abbrechen" in der einfügen-Schnittstelle. Wenn eine dieser Schaltflächen geklickt wird, muss die Anzeigenschnittstelle wiederherstellen. Erstellen Sie `Click` -Ereignishandlern für beide Schaltflächen-Steuerelemente, damit sie aufgerufen werden `ReturnToDisplayInterface`, eine Methode, die wir werden vorübergehend hinzufügen. Zusätzlich zum Ausblenden der `InsertingInterface` klicken und mit der `DisplayInterface` Bereich der `ReturnToDisplayInterface` Methode muss die Web-Steuerelemente auf den Zustand vor der Bearbeitung zurückgeben. Dies umfasst das Festlegen der DropDownList-Steuerelementen `SelectedIndex` Eigenschaften auf 0 und Beseitigen der `Text` Eigenschaften der TextBox-Steuerelemente.

> [!NOTE]
> Beachten Sie, was passieren kann Wenn wir nihnen t Zurückgeben der Steuerelemente auf den Zustand vor der Bearbeitung vor der Rückgabe auf die Anzeigenschnittstelle. Ein Benutzer möglicherweise klicken Sie auf die Schaltfläche mit den Prozess mit dem Produktversand, geben Sie die Produkte aus der Lieferung und klicken Sie dann auf Produkte aus Lieferung hinzufügen. Dies würde die Produkte hinzufügen und den Benutzer auf die Anzeigenschnittstelle zurückzugeben. An diesem Punkt kann der Benutzer einer anderen Sendung hinzufügen möchten. Nach dem Klicken auf die Schaltfläche "Prozess mit dem Produktversand", die sie die einfügen-Schnittstelle, aber die DropDownList zurückgeben würde würde Auswahl und TextBox-Werte immer noch mit ihren vorherigen Wert aufgefüllt werden.


[!code-vb[Main](batch-inserting-vb/samples/sample5.vb)]

Beide `Click` Ereignishandler rufen Sie einfach die `ReturnToDisplayInterface` -Methode, obwohl wir zu den Produkten hinzufügen aus Lieferung zurückkehren müssen `Click` -Ereignishandler in Schritt 4, und fügen Sie Code, um die Produkte zu speichern. `ReturnToDisplayInterface` beginnt mit dem Zurückgeben der `Suppliers` und `Categories` DropDownList-Steuerelementen, die ersten Optionen aus. Die zwei Konstanten `firstControlID` und `lastControlID` markieren Sie die Start- und Endindexwerte Steuerelement verwendet, bei der Benennung des Name und Unit Produktpreis Textfelder einfügen, Schnittstelle, und werden verwendet, in die Begrenzungen des der `For` Schleife, die die festlegt`Text`Eigenschaften der TextBox-Steuerelemente auf eine leere Zeichenfolge zurück. Zum Schluss die Panels `Visible` Eigenschaften werden zurückgesetzt, damit die einfügende-Schnittstelle ausgeblendet ist und der Anzeigenschnittstelle, die angezeigt.

Nehmen Sie einen Moment Zeit, um diese Seite in einem Browser zu testen. Wenn die Seite zuerst besuchen sollten Sie die Anzeigenschnittstelle sehen, wie in Abbildung 5 gezeigt wurde. Klicken Sie auf die Schaltfläche "Prozess mit dem Produktversand". Die Seite wird postback, und die einfügende-Schnittstelle sollte jetzt angezeigt werden, wie in Abbildung 12 dargestellt. Entweder die hinzufügen Produkte aus der Lieferung "oder" Abbrechen "klicken, kehren Sie zur Anzeigenschnittstelle zurück.

> [!NOTE]
> Können Sie während der Anzeige der einfügen-Schnittstelle, die CompareValidators auf den Einzelpreis Textfelder zu testen. Daraufhin sollte eine clientseitige Messagebox Warnung, wenn Sie die Produkte hinzufügen, Schaltfläche "Lieferung" mit ungültigen Currency-Werte oder Preise mit dem ein Wert kleiner als 0 (null) klicken.


[![Die einfügen-Schnittstelle wird angezeigt, nach dem Klicken auf die Schaltfläche "Prozess Produkt Lieferung"](batch-inserting-vb/_static/image35.png)](batch-inserting-vb/_static/image34.png)

**Abbildung 12**: die einfügen-Schnittstelle wird angezeigt, nach dem Klicken auf die Schaltfläche "Prozess Produkt Lieferung" ([klicken Sie, um das Bild in voller Größe anzeigen](batch-inserting-vb/_static/image36.png))


## <a name="step-4-adding-the-products"></a>Schritt 4: Hinzufügen der Produkte

Alle, die verbleibt, für dieses Tutorial ist auf die Produkte in der Datenbank in den Produkten Hinzufügen von Schaltfläche "Lieferung" s zu speichern, `Click` -Ereignishandler. Dies kann erreicht werden, indem Sie erstellen eine `ProductsDataTable` und das Hinzufügen einer `ProductsRow` Instanz für jedes der angegebenen Produktnamen. Einmal diese `ProductsRow` s wurden hinzugefügt, werden wir einen Aufruf von stellen die `ProductsBLL` s-Klasse `UpdateWithTransaction` -Methode übergeben die `ProductsDataTable`. Bedenken Sie, dass die `UpdateWithTransaction` -Methode, die in erstellt wurde die [Umschließen von Datenbankänderungen innerhalb einer Transaktion](wrapping-database-modifications-within-a-transaction-vb.md) Tutorial, übergibt die `ProductsDataTable` auf die `ProductsTableAdapter` s `UpdateWithTransaction` Methode. Von dort aus eine ADO.NET-Transaktion gestartet wird und die Probleme TableAdatper ein `INSERT` Anweisung an die Datenbank für jede hinzugefügte `ProductsRow` in der Datentabelle. Vorausgesetzt, dass alle Produkte ohne Fehler hinzugefügt werden, die Transaktion ein Commit ausgeführt wird, wird andernfalls ein Rollback.

Der Code für die Produkte hinzufügen, über die Schaltfläche "Lieferung" s `Click` -Ereignishandler auch etwas fehlerprüfung durchführen muss. Da es sich um keine RequiredFieldValidators in der einfügen-Schnittstelle verwendet, kann ein Benutzer einen Preis für ein Produkt beim Auslassen von seinen Namens eingeben. Da der Name des Produkts s erforderlich ist, wenn eine solche Bedingung erweitert müssen wir den Benutzer zu warnen und die einfügungen nicht fortgesetzt. Die vollständige `Click` Ereignishandlercode folgt:


[!code-vb[Main](batch-inserting-vb/samples/sample6.vb)]

Der Ereignishandler startet, indem sichergestellt wird, die die `Page.IsValid` Eigenschaft gibt einen Wert von `True`. Wenn sie zurückgibt `False`, und ist dies eines oder mehrere der CompareValidators werden Berichtsdaten ungültig; in diesem Fall wir sollten nicht versuchen, fügen Sie die eingegebenen Produkte oder wir erhalten eine Ausnahme beim Versuch den Preis der freien Eingabe zuzuweisen Wert der `ProductsRow` s `UnitPrice` Eigenschaft.

Anschließend wird eine neue `ProductsDataTable` Instanz erstellt wird (`products`). Ein `For` Schleife wird verwendet, um das Durchlaufen des Name und Unit Produktpreis Textfelder und die `Text` Eigenschaften werden in der lokalen Variablen gelesen `productName` und `unitPrice`. Wenn sich der Benutzer einen Wert für den Einzelpreis jedoch nicht für den entsprechenden Produktnamen, die `StatusLabel` angezeigt, die die Nachricht, wenn Sie eine Einheit Preis Sie angeben, auch muss den Namen des Produkts enthalten, und der Ereignishandler beendet ist.

Wenn Sie ein Produktnamen bereitgestellt wurde, ein neues `ProductsRow` Instanz wurde mit der `ProductsDataTable` s `NewProductsRow` Methode. Diese neue `ProductsRow` s-Instanz `ProductName` -Eigenschaftensatz auf das aktuelle Produkt Textfeld beim Benennen der `SupplierID` und `CategoryID` Eigenschaften zugewiesen sind die `SelectedValue` Eigenschaften die DropDownList-Steuerelementen im Einfügen von Schnittstelle s-Header. Wenn der Benutzer einen Wert für den Produktpreis s eingegeben, zugewiesen wird die `ProductsRow` s-Instanz `UnitPrice` Eigenschaft; hingegen die Eigenschaft ist links nicht zugewiesen ist, führen eine `NULL` Wert für `UnitPrice` in der Datenbank. Zum Schluss die `Discontinued` und `UnitsOnOrder` Eigenschaften werden die hartcodierten Werten zugewiesen `False` und 0 (null) bzw.

Nachdem Sie die Eigenschaften zugewiesen wurden die `ProductsRow` Instanz, die an die `ProductsDataTable`.

Nach Fertigstellung der `For` Schleife überprüft werden, ob alle Produkte hinzugefügt wurden. Der Benutzer kann schließlich aus Lieferung, bevor Sie eingeben, alle Produktnamen oder die Preise der Produkte hinzufügen geklickt haben. Es ist mindestens ein Produkt in der `ProductsDataTable`, `ProductsBLL` s-Klasse `UpdateWithTransaction` Methode wird aufgerufen. Als Nächstes wird die Daten erneut gebunden die `ProductsGrid` GridView, damit die neu hinzugefügte Produkte in der Anzeigenschnittstelle angezeigt werden. Die `StatusLabel` wird aktualisiert, um eine bestätigungsmeldung anzuzeigen und die `ReturnToDisplayInterface` wird aufgerufen, durch das Ausblenden der Schnittstelle einfügen und mit der Anzeigenschnittstelle.

Wenn keine Produkte eingegeben wurden, bleibt die einfügende Schnittstelle angezeigt, aber die Nachricht, die keine Produkte hinzugefügt wurden. Bitte geben Sie die Produktnamen und einem Stückpreis in den Textfeldern angezeigt.

Abbildung s 13, 14 und 15 anzeigen, das Einfügen und Schnittstellen in Aktion anzuzeigen. In Abbildung 13 wird der Benutzer hat eine Einheit Preis ohne einen entsprechenden Produktnamen eingegeben. Abbildung 14 zeigt der Anzeigenschnittstelle nach drei neue, dass Produkte während Abbildung 15 zwei neu hinzugefügten Produkten in den GridView-Ansicht zeigt (der dritte Parameter ist auf der vorherigen Seite) erfolgreich hinzugefügt wurden.


[![Ein Produktname ist erforderlich, bei einem Preis je Einheit eingeben](batch-inserting-vb/_static/image38.png)](batch-inserting-vb/_static/image37.png)

**Abbildung 13**: A Product Name ist erforderlich, bei einem Preis je Einheit eingeben ([klicken Sie, um das Bild in voller Größe anzeigen](batch-inserting-vb/_static/image39.png))


[![Drei neue Teil des Gartens mein wurde für den Lieferanten Mayumi s](batch-inserting-vb/_static/image41.png)](batch-inserting-vb/_static/image40.png)

**Abbildung 14**: drei neue Teil des Gartens mein wurde für den Lieferanten Mayumi s ([klicken Sie, um das Bild in voller Größe anzeigen](batch-inserting-vb/_static/image42.png))


[![Die neuen Produkte finden Sie in der letzten Seite des GridView](batch-inserting-vb/_static/image44.png)](batch-inserting-vb/_static/image43.png)

**Abbildung 15**: die neuen Produkte finden Sie in der letzten Seite des GridView ([klicken Sie, um das Bild in voller Größe anzeigen](batch-inserting-vb/_static/image45.png))


> [!NOTE]
> Der Batch, die in diesem Tutorial verwendete Logik einfügen dient als Wrapper für die einfügungen innerhalb des Bereichs der Transaktion. Um dies zu überprüfen, führen Sie absichtlich einen Fehler auf Datenbankebene. Z. B. statt der Zuweisung von neuen `ProductsRow` s-Instanz `CategoryID` Eigenschaft, um den ausgewählten Wert in der `Categories` DropDownList, weisen Sie es auf einen Wert wie `i * 5`. Hier `i` ist der Indexer Schleife und Werte im Bereich von 1 bis 5. Daher beim Hinzufügen von zwei oder mehr Produkte in Batch fügen Sie das erste Produkt müssen eine gültige `CategoryID` Wert (5), aber nachfolgende Produkte müssen `CategoryID` Werte, die nicht bis zu entsprechen `CategoryID` Werte in der `Categories` Tabelle. Das Endergebnis ist, die während der ersten `INSERT` gelingt, alle weiteren mit einer Verletzung der foreign Key-Einschränkung nicht. Da die batcheinfügung atomarisch, wird die erste `INSERT` wird ein Rollback, zurückgeben, die Datenbank auf seinen Zustand vor der Batch einfügen-Prozess wurde gestartet.


## <a name="summary"></a>Zusammenfassung

Über diese und die beiden vorherigen Tutorials wurde erstellt, Schnittstellen für das Aktualisieren, löschen und Einfügen von Batches von Daten, die die transaktionsunterstützung, die wir, in der Datenzugriffsebene hinzugefügt verwendet die [Umschließen von Datenbankänderungen innerhalb einer Transaktion](wrapping-database-modifications-within-a-transaction-vb.md) Tutorial. Für bestimmte die Effizienz Szenarien diese Benutzeroberflächen der Batch-Verarbeitung stark Endbenutzer Cutting nach unten auf der Anzahl der Klicks, Postbacks und Tastatur, Maus Kontextwechsel, wobei zudem gleichzeitig die Integrität der zugrunde liegenden Daten.

Dieses Tutorial ist unser Blick auf die Arbeiten mit batchdaten abgeschlossen. Der nächste Satz von Tutorials untersucht eine Vielzahl von Szenarien für advanced Data Access Layer, einschließlich der Verwendung von gespeicherter Prozeduren in den TableAdapter s Methoden erstellen, das Konfigurieren der Verbindung und Befehlsebene-Einstellungen in der Datenzugriffsschicht, Verschlüsseln von Verbindungszeichenfolgen und mehr!

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führen Sie Prüfer für dieses Tutorial Hilton Giesenow und S Ren Jacob Lauritsen wurden. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Vorherige](batch-deleting-vb.md)
