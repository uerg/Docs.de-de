---
uid: web-forms/overview/data-access/working-with-batched-data/batch-inserting-cs
title: "Batch eingefügt (c#) | Microsoft Docs"
author: rick-anderson
description: "Erfahren Sie, wie mehrere Datenbankdatensätze in einem einzigen Vorgang einfügen. In der Benutzeroberflächenebene erweitern wir die GridView, damit der Benutzer zur Eingabe von mehreren n kann..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: cf025e08-48fc-4385-b176-8610aa7b5565
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-inserting-cs
msc.type: authoredcontent
ms.openlocfilehash: 9eb65b99a955770c72b28713d8daa66bcd1d5344
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="batch-inserting-c"></a>Batch eingefügt (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Herunterladen von Code](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_66_CS.zip) oder [PDF herunterladen](batch-inserting-cs/_static/datatutorial66cs1.pdf)

> Erfahren Sie, wie mehrere Datenbankdatensätze in einem einzigen Vorgang einfügen. In der Benutzeroberflächenebene erweitern wir die GridView, um den Benutzer zur Eingabe mehrere neue Datensätze zu ermöglichen. In der Datenzugriffsebene umschließen wir mehrere Insert-Vorgänge innerhalb einer Transaktion, um sicherzustellen, dass alle einfügungen erfolgreich ist oder ein aller einfügungen Rollback.


## <a name="introduction"></a>Einführung

In der [BatchUpdates](batch-updating-cs.md) Lernprogramm erläutert, Anpassen des GridView-Steuerelements, um eine Schnittstelle zu präsentieren, in denen mehrere Datensätze bearbeitet wurden. Der Benutzer Zugriff auf die Seite konnten stellen eine Reihe von Änderungen, und führen Sie dann mit einem einzigen Mausklick einem BatchUpdate. Für Situationen, in denen Benutzer häufig viele Datensätze in einer Aktion aktualisieren, kann eine solche Schnittstelle zahllose Klicks speichern und Tastatur, Maus Kontextwechsel gegenüber der Standardeinstellung pro Zeile Bearbeitungsfeatures, die zuerst in untersucht wurden die [ein Übersicht über das Einfügen, aktualisieren und Löschen von Daten](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) Lernprogramm.

Dieses Konzept kann auch angewendet werden, wenn Sie Datensätze hinzufügen. Stellen Sie sich vor, die hier bei Northwind Traders wir häufig erhalten Lieferungen von Lieferanten, die eine Anzahl von Produkten für eine bestimmte Kategorie enthalten. Beispielsweise können wir eine Lieferung von sechs verschiedenen Tee und Kaffee Produkte aus Tokio Traders erhalten. Wenn der Benutzer die sechs Produkte eine jeweils über ein DetailsView-Steuerelement ein, haben Sie viele der gleichen Werte immer wieder auswählen: müssen die gleiche Kategorie (Getränke), die demselben Lieferanten (Tokyo Traders) auswählen, den gleichen Wert (nicht mehr unterstützt "False"), und den gleichen Einheiten auf Reihenfolgenwert (0). Dieser Eintrag repetitiven Daten ist nicht nur zeitaufwändig jedoch fehleranfällig ist.

Mit ein wenig Aufwand kann erstellt werden, einen Batch, die Schnittstelle, die mit der Benutzer wählen die Supplier "und" Kategorie einmal, geben Sie eine Reihe von Produktnamen und einem Stückpreis, und klicken Sie dann auf eine Schaltfläche zum Hinzufügen neuer Produkte in der Datenbank einfügen (siehe Abbildung 1). Jedes Produkt hinzugefügt wird, dessen `ProductName` und `UnitPrice` Datenfelder werden in die Textfelder ein eingegebenen Werte zugewiesen während seiner `CategoryID` und `SupplierID` Werte werden die Werte aus den DropDownLists auf der obersten fo Form zugewiesen. Die `Discontinued` und `UnitsOnOrder` Werte werden festgelegt, die hartcodierte Werte des `false` und 0 (null) bzw.


[![Die Batch-einfügen-Schnittstelle](batch-inserting-cs/_static/image2.png)](batch-inserting-cs/_static/image1.png)

**Abbildung 1**: die Batch-Schnittstelle zum Einfügen von ([klicken Sie hier, um das Bild in voller Größe angezeigt](batch-inserting-cs/_static/image3.png))


In diesem Lernprogramm erstellen wir eine Seite, die den Batch, die Schnittstelle, die in Abbildung 1 gezeigten einfügen implementiert. Wird mit den vorherigen zwei Lernprogramme, wir die einfügungen innerhalb des Bereichs einer Transaktion, um sicherzustellen, dass Unteilbarkeit umschlossen werden soll. Lassen Sie s beginnen!

## <a name="step-1-creating-the-display-interface"></a>Schritt 1: Erstellen der Anzeigenschnittstelle

Dieses Lernprogramm bestehen aus einer einzelnen Seite, die in zwei Bereiche unterteilt ist: einer Region anzeigen und einen Bereich einfügen. Die Anzeigenschnittstelle, die wir in diesem Schritt erstellen müssen, die Produkte zeigt in einem GridView und enthält eine Schaltfläche mit dem Titel mit dem Produktversand Prozess. Wenn diese Schaltfläche geklickt wird, wird die Anzeigenschnittstelle mit der einfügen-Schnittstelle ersetzt, die in Abbildung 1 dargestellt wird. Die Anzeigenschnittstelle, die nach der hinzufügen-Produkte aus Lieferung zurückgibt oder "Abbrechen" Schaltflächen geklickt wird. Wir erstellen in Schritt2 die einfügende-Schnittstelle.

Beim Erstellen einer Seite, die zwei Schnittstellen, von denen nur eine zu einem Zeitpunkt sichtbar ist, jede Schnittstelle in der Regel befindet innerhalb einer [Bereich Websteuerelement](http://www.w3schools.com/aspnet/control_panel.asp), dem dient als Container für andere Steuerelemente. Daher müssen unsere Seite zwei Panel-Steuerelemente in einer für jede Schnittstelle.

Öffnen Sie zunächst die `BatchInsert.aspx` auf der Seite der `BatchData` Ordner, und ziehen Sie ein Panel aus der Toolbox in den Designer (siehe Abbildung 2). Legen Sie im Bereich s `ID` Eigenschaft `DisplayInterface`. Wenn im Bereich dem Designer hinzugefügten seine `Height` und `Width` Eigenschaften auf 50px 125px, bzw. festgelegt werden. Deaktivieren Sie die Werte dieser Eigenschaften im Eigenschaftenfenster.


[![Ziehen Sie ein Panel aus der Toolbox in den Designer](batch-inserting-cs/_static/image5.png)](batch-inserting-cs/_static/image4.png)

**Abbildung 2**: Ziehen Sie ein Panel aus der Toolbox in den Designer ([klicken Sie hier, um das Bild in voller Größe angezeigt](batch-inserting-cs/_static/image6.png))


Ziehen Sie anschließend eine Schaltfläche und GridView-Steuerelement in der Systemsteuerung. Legen Sie die Schaltfläche "s" `ID` Eigenschaft `ProcessShipment` und dessen `Text` Eigenschaft, um den Prozess mit dem Produktversand. Legen Sie die GridView s `ID` Eigenschaft `ProductsGrid` und aus seinem Smarttag binden Sie es an eine neue ObjectDataSource mit dem Namen `ProductsDataSource`. Konfigurieren der ObjectDataSource zum Abrufen der zugehörigen Daten aus der `ProductsBLL` Klasse s `GetProducts` Methode. Da diese GridView nur zum Anzeigen von Daten verwendet wird, legen Sie die Dropdownlisten in der Update-, INSERT-, und Löschen von Registerkarten (keine). Klicken Sie auf "Fertig stellen", um die Datenquelle konfigurieren-Assistenten zu beenden.


[![Anzeigen der Daten aus der ProductsBLL s GetProducts Klassenmethode zurückgegeben](batch-inserting-cs/_static/image8.png)](batch-inserting-cs/_static/image7.png)

**Abbildung 3**: Anzeigen der Daten, die von zurückgegeben der `ProductsBLL` Klasse s `GetProducts` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](batch-inserting-cs/_static/image9.png))


[![Legen Sie die Dropdownlisten in der Update-, INSERT- und Löschen von Registerkarten auf (keine)](batch-inserting-cs/_static/image11.png)](batch-inserting-cs/_static/image10.png)

**Abbildung 4**: Legen Sie das Dropdown-Listen in aktualisieren, einfügen und Löschen von Registerkarten auf (keine) ([klicken Sie hier, um das Bild in voller Größe angezeigt](batch-inserting-cs/_static/image12.png))


Nach Abschluss des Assistenten ObjectDataSource wird Visual Studio BoundFields und eine CheckBoxField für die Product-Datenfelder hinzufügen. Entfernen Sie alle außer den `ProductName`, `CategoryName`, `SupplierName`, `UnitPrice`, und `Discontinued` Felder. Wahlweise können Sie alle Layoutgründen Anpassungen vornehmen. Entschied ich mich, formatieren Sie die `UnitPrice` Felds als Währungswert, neu angeordnet, die Felder und einige Felder umbenannt `HeaderText` Werte. Auch konfigurieren Sie, die GridView Einbeziehung von paging und Sortieren von Unterstützung durch Aktivieren von Paging und Sortieren aktivieren Kontrollkästchen in der GridView-s-Smarttag überprüfen.

Nach dem Hinzufügen der Bereich "," Button "," GridView "und" ObjectDataSource-Steuerelemente und GridView-s-Feldern anpassen, sollte Ihre s deklarativen Seitenmarkup etwa wie folgt aussehen:


[!code-aspx[Main](batch-inserting-cs/samples/sample1.aspx)]

Beachten Sie, die das Markup für die Schaltfläche und GridView angezeigt werden, innerhalb des öffnenden und schließenden `<asp:Panel>` Tags. Da diese Steuerelemente sind, die innerhalb der `DisplayInterface` Bereich können wir dies ausblenden, indem Sie einfach im Bereich s festlegen `Visible` Eigenschaft `false`. Schritt 3 programmgesteuert ändern Bereich s prüft `Visible` Eigenschaft als Antwort auf eine Schaltfläche klicken, um eine Schnittstelle und die andere gleichzeitig anzuzeigen.

Nehmen Sie einen Moment Zeit, um unseren Fortschritt über einen Browser anzuzeigen. Wie in Abbildung 5 gezeigt, sehen Sie eine Prozess mit dem Produktversand Schaltfläche oben eine GridView, die die zehn Produkte zu einem Zeitpunkt auflistet.


[![GridView Listet die Produkte und bietet sortieren und Paging von Funktionen](batch-inserting-cs/_static/image14.png)](batch-inserting-cs/_static/image13.png)

**Abbildung 5**: GridView Listet die Produkte und bietet sortieren und Paging-Funktionen ([klicken Sie hier, um das Bild in voller Größe angezeigt](batch-inserting-cs/_static/image15.png))


## <a name="step-2-creating-the-inserting-interface"></a>Schritt 2: Erstellen der einfügen-Schnittstelle

Eine Schnittstelle mit der Anzeigenschnittstelle, die abgeschlossen werden es erneut bereit, um das Einfügen von zu erstellen. Können Sie für dieses Lernprogramm s eine einfügende Schnittstelle erstellen, die für einen einzelnen Lieferanten und Kategorie aufgefordert, und klicken Sie dann kann der Benutzer bis zu fünf aus Produktnamen besteht und Unit Price Werte eingeben. Mit dieser Schnittstelle kann die Benutzer einer Minute bis fünf neue Produkte hinzufügen, die alle die gleiche Kategorie und Lieferanten, haben aber eine eindeutige Produkt-Namen und Preise.

Beginnen Sie, indem Sie ziehen ein Panel aus der Toolbox in den Designer, und platzieren es unterhalb der vorhandenen `DisplayInterface` Bereich. Festlegen der `ID` -Eigenschaft dieser neu hinzugefügten Bereich `InsertingInterface` und legen Sie seine `Visible` Eigenschaft, um `false`. Wir fügen Code, der festlegt der `InsertingInterface` Bereich s `Visible` Eigenschaft `true` in Schritt 3. Löschen Sie auch, im Bereich s `Height` und `Width` Eigenschaftswerte.

Als Nächstes müssen wir die einfügende Schnittstelle erstellen, die wieder in Abbildung 1 dargestellt wurde. Diese Schnittstelle kann über eine Reihe von HTML-Techniken erstellt werden, jedoch verwenden wir eine recht einfach: eine vierspaltige, sieben-Zeile-Tabelle.

> [!NOTE]
> Wenn Sie für HTML-Markup eingeben `<table>` Elemente, ich möchte die Quellansicht verwenden. Während der Visual Studio-Tools für das Hinzufügen von verfügt `<table>` Elemente mithilfe des Designers, der Designer scheint alle zu möchte einfügen ungefragt für `style` Einstellungen in das Markup. Nachdem ich erstellt habe haben die `<table>` Markup ich in der Regel zurückgeben in den Designer der Web-Steuerelemente hinzufügen und ihre Eigenschaften festlegen. Beim Erstellen von Tabellen mit vordefinierten Spalten und Zeilen ich möchte mit statischem HTML-Code statt über das [Tabelle Websteuerelement](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.table.aspx) da alle Websteuerelemente in einer Tabelle Websteuerelement platziert nur zugegriffen werden können, mithilfe der `FindControl("controlID")` Muster. Ich, verwenden jedoch Tabelle Websteuerelemente für dynamisch Größe von Tabellen (von denen auf einige Datenbank oder die benutzerspezifische Kriterien, deren Zeilen oder Spalten basieren), seit der Tabelle Web, das Steuerelement programmgesteuert erstellt werden kann.


Geben Sie das folgende Markup innerhalb der `<asp:Panel>` des Tags der `InsertingInterface` Bereich:


[!code-html[Main](batch-inserting-cs/samples/sample2.html)]

Dies `<table>` Markup enthält keine Websteuerelemente noch, wir werden diese vorübergehend hinzufügen. Beachten Sie, dass jedes `<tr>` Element enthält eine bestimmte Einstellung der CSS-Klasse: `BatchInsertHeaderRow` für die Kopfzeile, die an den Lieferanten und Kategorie DropDownLists mich werden; `BatchInsertFooterRow` für die Fußzeile, in denen die Produkte hinzufügen von Schaltflächen Abbrechen und Lieferung geht; und abwechselnde `BatchInsertRow` und `BatchInsertAlternatingRow` Werte für die Zeilen, die das Produkt und die Einheit enthält Preis TextBox-Steuerelemente. Ich Ve erstellt die entsprechenden CSS-Klassen in der `Styles.css` Datei so erteilen Sie der einfügen-Schnittstelle, die die GridView und DetailsView ähnlich steuert wir in diesen Lernprogrammen verwendet haben. Diese CSS-Klassen werden unten gezeigt.


[!code-css[Main](batch-inserting-cs/samples/sample3.css)]

Mit diesem Markup eingegeben wird zurück zur Entwurfsansicht. Dies `<table>` sollte als Tabelle vierspaltige, sieben Zeile im Designer anzeigen, wie die Abbildung 6 veranschaulicht.


[![Die Schnittstelle einfügen besteht aus einer vier-Spalte, Tabelle mit sieben Zeilen](batch-inserting-cs/_static/image17.png)](batch-inserting-cs/_static/image16.png)

**Abbildung 6**: die einfügen-Schnittstelle besteht aus einer vier-Spalte, Tabelle mit sieben Zeilen ([klicken Sie hier, um das Bild in voller Größe angezeigt](batch-inserting-cs/_static/image18.png))


Wir re jetzt bereit für die einfügende Schnittstelle Websteuerelementen hinzugefügt. Ziehen Sie zwei DropDownLists aus der Toolbox in die entsprechenden Zellen in der Tabelle für den Lieferanten und eine für die Kategorie.

Legen Sie den Lieferanten DropDownList s `ID` Eigenschaft `Suppliers` und binden es an eine neue ObjectDataSource mit dem Namen `SuppliersDataSource`. Konfigurieren Sie die neue ObjectDataSource zum Abrufen der zugehörigen Daten aus der `SuppliersBLL` Klasse s `GetSuppliers` -Methode und legen Sie das UPDATE Registerkarte s Dropdown-Liste (keine). Klicken Sie auf ' Fertig stellen ', um den Assistenten abzuschließen.


[![Konfigurieren der ObjectDataSource zur Verwendung der SuppliersBLL Klasse s GetSuppliers-Methode](batch-inserting-cs/_static/image20.png)](batch-inserting-cs/_static/image19.png)

**Abbildung 7**: Konfigurieren der ObjectDataSource verwenden die `SuppliersBLL` Klasse s `GetSuppliers` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](batch-inserting-cs/_static/image21.png))


Haben die `Suppliers` DropDownList-Anzeige der `CompanyName` Feld "Daten" und die Verwendung der `SupplierID` Datenfeld der Spalte als seine `ListItem` s-Werte.


[![Zeigen Sie das CompanyName-Datenfeld an und verwenden Sie SupplierID als Wert.](batch-inserting-cs/_static/image23.png)](batch-inserting-cs/_static/image22.png)

**Abbildung 8**: Anzeige der `CompanyName` Feld "Daten" und die Verwendung `SupplierID` als Wert ([klicken Sie hier, um das Bild in voller Größe angezeigt](batch-inserting-cs/_static/image24.png))


Benennen Sie die zweite DropDownList `Categories` und binden es an eine neue ObjectDataSource mit dem Namen `CategoriesDataSource`. Konfigurieren der `CategoriesDataSource` ObjectDataSource verwenden die `CategoriesBLL` Klasse s `GetCategories` Methode, die Menge der Dropdown-Listen, in die Update- und DELETE-Registerkarten (None) und klicken Sie auf Fertig stellen, um den Assistenten abzuschließen. Schließlich haben die DropDownList-Anzeige der `CategoryName` Feld "Daten" und die Verwendung der `CategoryID` als Wert.

Nachdem diese zwei DropDownLists hinzugefügt und entsprechend konfigurierten ObjectDataSources gebunden haben, sollte der Bildschirm in Abbildung 9 ähneln.


[![Die Kopfzeile enthält jetzt die Lieferanten und Kategorien DropDownLists](batch-inserting-cs/_static/image26.png)](batch-inserting-cs/_static/image25.png)

**Abbildung 9**: der Header Zeile enthält jetzt die `Suppliers` und `Categories` DropDownLists ([klicken Sie hier, um das Bild in voller Größe angezeigt](batch-inserting-cs/_static/image27.png))


Jetzt müssen wir die Textfelder ein, um den Namen und den Preis für jede neue Produkt erfassen zu erstellen. Ziehen Sie ein TextBox-Steuerelement aus der Toolbox in den Designer für jeden der fünf Product Name "und" Price Zeilen. Legen Sie die `ID` Eigenschaften der Textfelder, `ProductName1`, `UnitPrice1`, `ProductName2`, `UnitPrice2`, `ProductName3`, `UnitPrice3`und so weiter.

Hinzufügen einer CompareValidator nach jedem Preis pro Einheit Textfelder ein, und Festlegen der `ControlToValidate` -Eigenschaft auf die entsprechende `ID`. Auch festlegen, die `Operator` Eigenschaft `GreaterThanEqual`, `ValueToCompare` auf 0 (null) und `Type` auf `Currency`. Diese Einstellungen weisen CompareValidator, stellen Sie sicher, dass der Kurs, wenn eingegeben haben, eine gültige Currency-Wert, der größer als oder gleich 0 (null) ist. Festlegen der `Text` Eigenschaft \*, und `ErrorMessage` auf den Preis muss größer als oder gleich 0 (null) sein. Darüber hinaus geben lassen Sie aller Währungssymbole Weg.

> [!NOTE]
> Einfügen von Schnittstelle umfasst keine Steuerelemente RequiredFieldValidator, obwohl die `ProductName` -Feld in der `Products` Datenbanktabelle lässt keine `NULL` Werte. Dies ist, da wir, damit den Benutzer bis zu fünf Produkte eingeben können möchten. Beispielsweise sollte der Benutzer für die ersten drei Zeilen den Produktpreis Name und Komponententests bereitstellen, hinzufügen die letzten beiden Zeilen leer lassen, wir d nur drei neue Produkte mit dem System. Da `ProductName` ist erforderlich, allerdings wir müssen programmgesteuert überprüfen, dass bei einem Einzelpreis sicherstellen, dass ein entsprechende Product Name-Wert angegeben wird eingegeben. Wir werden diese Prüfung in Schritt 4 konfigurieren.


Bei der Validierung von Benutzereingaben s meldet CompareValidator ungültige Daten an, wenn der Wert ein Währungssymbol enthält. Fügen Sie ein $ vor aller Textfelder als einen visuellen Hinweis zu dienen, die den Benutzer, um das Währungssymbol zu unterdrücken, bei der Eingabe des Preis weist Preis pro Einheit.

Schließlich Hinzufügen eines ValidationSummary-Steuerelements innerhalb der `InsertingInterface` Bereich Einstellungen seine `ShowMessageBox` Eigenschaft, um `true` und seine `ShowSummary` Eigenschaft `false`. Mit diesen Einstellungen können Sie, wenn der Benutzer eine ungültige Einheit Preis eingibt, ein Sternchen wird neben dem betreffenden TextBox-Steuerelemente angezeigt und zeigt die ValidationSummary eine clientseitige Messagebox, in der die Fehlermeldung angezeigt, die wir zuvor angegeben.

An diesem Punkt sollte Ihr Bildschirm Abbildung 10 ähneln.


[![Das Einfügen von-Schnittstelle enthält jetzt Textfelder ein, für die Produkte Namen und Preise](batch-inserting-cs/_static/image29.png)](batch-inserting-cs/_static/image28.png)

**Abbildung 10**: der einfügen-Schnittstelle jetzt enthält Textfelder für die Produktnamen und Preise ([klicken Sie hier, um das Bild in voller Größe angezeigt](batch-inserting-cs/_static/image30.png))


Als Nächstes nehmen wir die Produkte hinzufügen von Schaltflächen "Abbrechen" und "Lieferung" Fußzeile hinzugefügt haben. Ziehen Sie zwei Schaltflächen-Steuerelemente aus der Toolbox in die Fußzeile der einfügen-Schnittstelle, die Schaltflächen festlegen `ID` Eigenschaften `AddProducts` und `CancelButton` und `Text` Eigenschaften bzw. Produkte aus der Lieferung und "Abbrechen", hinzugefügt. Darüber hinaus legen Sie die `CancelButton` Steuerelement s `CausesValidation` Eigenschaft `false`.

Schließlich müssen wir ein Bezeichnung-Websteuerelement hinzufügen, die statusmeldungen für die beiden Schnittstellen angezeigt wird. Z. B. wenn ein Benutzer erfolgreich eine neue Lieferung von Produkten hinzufügt, möchten wir auf die Anzeigenschnittstelle zurück und zeigt eine bestätigungsmeldung an. Wenn jedoch der Benutzer einen Preis für ein neues Produkt jedoch bewirkt, dass aus den Produktnamen enthält, müssen wir eine Warnmeldung seit Anzeigen der `ProductName` ist ein Pflichtfeld. Da wir diese Meldung für die anzuzeigenden für beide Schnittstellen benötigen, fügen Sie ihn am oberen Rand der Seite außerhalb der Bereiche.

Ziehen Sie ein Bezeichnung-Websteuerelement aus der Toolbox in den oberen Rand der Seite im Designer. Festlegen der `ID` Eigenschaft `StatusLabel`deaktivieren, die `Text` Eigenschaft, und legen die `Visible` und `EnableViewState` Eigenschaften `false`. Wie wir im vorherigen Lernprogrammen gesehen haben, Festlegen der `EnableViewState` Eigenschaft `false` erlaubt es uns, programmgesteuert die Bezeichnung s Eigenschaftswerte ändern und diese automatisch wieder auf ihre Standardwerte auf den nachfolgenden Postbacks zurückgesetzt. Dies vereinfacht den Code für eine Statusmeldung als Antwort auf eine Benutzeraktion, die auf den nachfolgenden Postbacks wird nicht mehr angezeigt. Legen Sie schließlich die `StatusLabel` Steuerelement s `CssClass` Eigenschaft in "Warnung", die den Namen der CSS-Klasse ist definiert `Styles.css` , die Text in großer, kursiv, fett, Rot Schriftart anzeigt.

Abbildung 11 zeigt die Visual Studio-Designer, nachdem die Bezeichnung hinzugefügt und konfiguriert wurde.


[![Platzieren Sie das Steuerelement StatusLabel über zwei Panel-Steuerelemente](batch-inserting-cs/_static/image32.png)](batch-inserting-cs/_static/image31.png)

**Abbildung 11**: Ort der `StatusLabel` Steuerelement über die zwei Panel-Steuerelemente ([klicken Sie hier, um das Bild in voller Größe angezeigt](batch-inserting-cs/_static/image33.png))


## <a name="step-3-switching-between-the-display-and-inserting-interfaces"></a>Schritt 3: Wechseln zwischen der Anzeige und Einfügen von Schnittstellen

Zu diesem Zeitpunkt haben wir das Markup für unsere Anzeige- und Einfügen von Schnittstellen, sondern wir re bleibt weiterhin mit zwei Aufgaben ausgeführt:

- Umschalten zwischen der Anzeige und das Einfügen von Schnittstellen
- Die Produkte hinzufügen in der Lieferung in der Datenbank

Momentan die Anzeigenschnittstelle wird angezeigt, aber die einfügende Schnittstelle ausgeblendet wird. Grund hierfür ist, die `DisplayInterface` Bereich s `Visible` -Eigenschaftensatz auf `true` (Standardwert), während die `InsertingInterface` Bereich s `Visible` -Eigenschaftensatz auf `false`. So wechseln Sie zwischen den beiden Schnittstellen wir einfach jedes Steuerelement s umschalten müssen `Visible` Eigenschaftswert.

Wir möchten die von der Anzeigenschnittstelle in der einfügen-Schnittstelle verschoben wird, wenn der Prozess mit dem Produktversand geklickt wird. Aus diesem Grund erstellen Sie einen Ereignishandler für Schaltflächenname s `Click` Ereignis mit dem folgenden Code:


[!code-csharp[Main](batch-inserting-cs/samples/sample4.cs)]

Dieser Code einfach Blendet die `DisplayInterface` Systemsteuerung und zeigt die `InsertingInterface` Bereich.

Als Nächstes erstellen Sie Ereignishandler für die Produkte hinzufügen von Steuerelementen von Lieferung und Schaltfläche "Abbrechen" in der einfügen-Schnittstelle. Wenn eine der folgenden Schaltflächen geklickt wird, müssen es wieder auf die Anzeigenschnittstelle zurückgesetzt. Erstellen Sie `Click` -Ereignishandler für beide Schaltflächen-Steuerelemente, damit sie aufrufen `ReturnToDisplayInterface`, eine Methode, die wir vorübergehend hinzufügen. Zusätzlich zum Ausblenden der `InsertingInterface` Systemsteuerung und mit der `DisplayInterface` Bereich der `ReturnToDisplayInterface` Methode Websteuerelemente zum Zustand vor der Bearbeitung zurückgeben muss. Dies umfasst das Festlegen der DropDownLists `SelectedIndex` Eigenschaften zwischen 0 und gelöscht wird, zu der `Text` Eigenschaften der TextBox-Steuerelemente.

> [!NOTE]
> Beachten Sie, was passieren kann Wenn wir t Zurückgeben der Steuerelemente zum Zustand vor der Bearbeitung vor der Rückgabe auf die Anzeigenschnittstelle. Ein Benutzer möglicherweise klicken Sie auf die Schaltfläche mit den Prozess mit dem Produktversand, geben Sie die Produkte aus der Lieferung und klicken Sie dann auf Produkte aus Lieferung hinzufügen. Dies würde die Produkte hinzufügen und den Benutzer auf die Anzeigenschnittstelle zurück. Zu diesem Zeitpunkt kann der Benutzer einer anderen Sendung hinzufügen möchten. Wenn Sie auf den Prozess mit dem Produktversand-Schaltfläche, die sie der einfügen-Schnittstelle, aber der DropDownList zurückgeben würde würde Auswahl und Textfeldwerte weiterhin mit ihren vorherigen Wert aufgefüllt werden.


[!code-csharp[Main](batch-inserting-cs/samples/sample5.cs)]

Beide `Click` Ereignishandler rufen Sie einfach die `ReturnToDisplayInterface` -Methode, obwohl wir den hinzufügen-Produkten aus Lieferung zurückkehren `Click` -Ereignishandler in Schritt 4 und Code hinzufügen, um die Produkte zu speichern. `ReturnToDisplayInterface`beginnt mit dem Zurückgeben der `Suppliers` und `Categories` DropDownLists ihre erste "Optionen". Die beiden Konstanten `firstControlID` und `lastControlID` markieren Sie die Start- und Endwerten Steuerelement Index benennen den Namen und die Einheit Produktpreis Textfelder einfügen Schnittstelle und werden verwendet, in die Grenzen des verwendet die `for` Schleife, die die festlegt`Text`Eigenschaften der TextBox-Steuerelemente auf eine leere Zeichenfolge zurück. Zum Schluss die Bereiche `Visible` Eigenschaften werden zurückgesetzt, damit die einfügende Schnittstelle ausgeblendet ist und die Anzeigenschnittstelle dargestellt.

Nehmen Sie einen Moment Zeit, um diese Seite in einem Browser zu testen. Beim ersten Seite besuchen sollte die Anzeigenschnittstelle angezeigt werden, wie in Abbildung 5 gezeigt wurde. Klicken Sie auf die Schaltfläche mit den Prozess mit dem Produktversand. Die Seite wird postback und die einfügende-Schnittstelle sollte jetzt angezeigt werden, wie in Abbildung 12 dargestellt. Klicken entweder die Produkte hinzufügen von Schaltflächen Lieferung oder "Abbrechen" zurück auf die Anzeigenschnittstelle.

> [!NOTE]
> Nehmen Sie beim Anzeigen der einfügen-Schnittstelle, einen Moment Zeit zum Testen der CompareValidators auf den Einzelpreis Textfelder ein. Daraufhin sollte eine Warnung, wenn Sie die Produkte hinzufügen, Schaltfläche "Lieferung" mit ungültiger Währungsangaben oder Preise mit einem Wert kleiner als 0 (null) auf die clientseitige-Messagebox.


[![Der einfügen-Schnittstelle wird angezeigt, nach dem Klicken auf die Schaltfläche verarbeiten Produkt Lieferung](batch-inserting-cs/_static/image35.png)](batch-inserting-cs/_static/image34.png)

**Abbildung 12**: die einfügen-Schnittstelle wird angezeigt, nach dem Klicken auf die Schaltfläche verarbeiten Produkt Lieferung ([klicken Sie hier, um das Bild in voller Größe angezeigt](batch-inserting-cs/_static/image36.png))


## <a name="step-4-adding-the-products"></a>Schritt 4: Hinzufügen, die Produkte

Alle, die für dieses Lernprogramm besteht darin, die Produkte in der Datenbank in den Produkten Hinzufügen von Lieferung Schaltfläche s speichern bleibt `Click` -Ereignishandler. Dies kann durch Erstellen einer `ProductsDataTable` und Hinzufügen von einer `ProductsRow` Instanz für jede der bereitgestellten aus Produktnamen besteht. Sobald diese `ProductsRow` s stellen wir einen Aufruf von hinzugefügt wurden die `ProductsBLL` Klasse s `UpdateWithTransaction` -Methode übergeben der `ProductsDataTable`. Bedenken Sie, dass die `UpdateWithTransaction` -Methode, die in erstellt wurde die [Umbruch Datenbankänderungen innerhalb einer Transaktion](wrapping-database-modifications-within-a-transaction-cs.md) Tutorial, übergibt der `ProductsDataTable` auf die `ProductsTableAdapter` s `UpdateWithTransaction` Methode. Von dort aus eine ADO.NET-Transaktion gestartet wird und die Probleme TableAdatper ein `INSERT` Anweisung an die Datenbank für jede hinzugefügte `ProductsRow` in der "DataTable". Vorausgesetzt, dass alle Produkte ohne Fehler hinzugefügt werden, die Transaktion ein Commit ausgeführt wird, wird ansonsten Rollback.

Der Code für die Produkte hinzufügen, Schaltfläche "Lieferung" s `Click` Ereignishandler auch einem gewissen Grad fehlerüberprüfung durchführen muss. Da es sich um keine RequiredFieldValidators in der einfügen-Schnittstelle verwendet, kann ein Benutzer einen Preis beim Auslassen von seinem Namens für ein Produkt eingeben. Da der Name des Produkts s erforderlich ist, wenn eine solche Bedingung erweitert muss der Benutzer gewarnt werden, und die einfügungen nicht fortsetzen. Die vollständige `Click` Ereignishandlercode folgt:


[!code-csharp[Main](batch-inserting-cs/samples/sample6.cs)]

Der Ereignishandler gestartet wird, indem sichergestellt wird, die die `Page.IsValid` Eigenschaft gibt einen Wert von `true`. Wenn zurückgegeben `false`, klicken Sie dann bedeutet, dass eine oder mehrere der CompareValidators untergeordnet sind ungültige Daten; in diesem Fall möchten wir nicht versucht wird, legen Sie die eingegebenen Produkte den oder wir müssen letztendlich mit einer Ausnahme beim Versuch, den Benutzer eingegeben Einzelpreis zuweisen -Wert an die `ProductsRow` s `UnitPrice` Eigenschaft.

Anschließend wird eine neue `ProductsDataTable` Instanz erstellt wird (`products`). Ein `for` Schleife wird verwendet, um den Namen und die Einheit Produktpreis Textfelder durchlaufen und die `Text` Eigenschaften werden in der lokalen Variablen gelesen `productName` und `unitPrice`. Wenn der Benutzer einen Wert für den Einzelpreis jedoch nicht für den Namen des entsprechenden Produkts eingegeben hat die `StatusLabel` zeigt die Meldung, wenn Sie eine Einheit Preis können Sie angeben, auch muss den Namen des Produkts enthalten, und der Ereignishandler beendet wird.

Wenn ein Produktname, ein neues angegeben wurde `ProductsRow` Instanz wird erstellt, mit der `ProductsDataTable` s `NewProductsRow` Methode. Diese neue `ProductsRow` s-Instanz `ProductName` Eigenschaftensatz für das aktuelle Produkt Textfeld beim Benennen der `SupplierID` und `CategoryID` Eigenschaften zugewiesen sind die `SelectedValue` Eigenschaften der DropDownLists im einfügende Schnittstelle s-Header. Wenn der Benutzer einen Wert für den Produktpreis s eingegeben hat, ihm zugewiesenen der `ProductsRow` s-Instanz `UnitPrice` Eigenschafts-hingegen die Eigenschaft ist nicht zugewiesen, links, der verursacht eine `NULL` Wert für `UnitPrice` in der Datenbank. Schließlich die `Discontinued` und `UnitsOnOrder` zugewiesenen Eigenschaften werden die hartcodierten Werte `false` und 0 (null) bzw.

Nachdem Sie die Eigenschaften zugewiesen wurden die `ProductsRow` Instanz es hinzugefügt wird die `ProductsDataTable`.

Nach dem Abschluss der `for` Schleife, wir überprüfen, ob alle Produkte hinzugefügt wurden. Der Benutzer möglicherweise, nachdem alle Produkte aus Lieferung vor dem Wechsel in eine beliebige aus Produktnamen besteht oder die Preise hinzufügen geklickt haben. Es ist mindestens ein Produkt in der `ProductsDataTable`, die `ProductsBLL` Klasse s `UpdateWithTransaction` -Methode aufgerufen wird. Als Nächstes wird die Daten neu, um die `ProductsGrid` GridView, sodass die neu hinzugefügte Produkte in der Anzeige-Benutzeroberfläche angezeigt werden. Die `StatusLabel` wird aktualisiert, um eine bestätigungsmeldung angezeigt und die `ReturnToDisplayInterface` aufgerufen wird, Ausblenden von der Schnittstelle einfügen und mit der Anzeigenschnittstelle.

Wenn keine Produkte eingegeben wurden, bleibt die einfügende Schnittstelle angezeigt, aber die Nachricht, die keine Produkte hinzugefügt wurden. Bitte geben Sie den Produktnamen und einem Stückpreis in den Textfeldern angezeigt.

Abbildung s 13, 14 und 15 anzeigen, die eingefügt und Schnittstellen in Aktion anzuzeigen. In Abbildung 13 hat der Benutzer eine Einheit Preis ohne entsprechenden Produktname eingegeben. Abbildung 14 zeigt der Anzeigenschnittstelle nach drei neue Produkte während Abbildung 15 zwei der neu hinzugefügten Produkte in der GridView zeigt (das dritte Arbeitsblatt ist auf der vorherigen Seite) erfolgreich hinzugefügt wurden.


[![Ein Product Name ist erforderlich, wenn eine Unit Price eingeben](batch-inserting-cs/_static/image38.png)](batch-inserting-cs/_static/image37.png)

**Abbildung 13**: A Product Name ist erforderlich, wenn eine Unit Price eingeben ([klicken Sie hier, um das Bild in voller Größe angezeigt](batch-inserting-cs/_static/image39.png))


[![Drei neue Teil des Gartens mein wurde für den Lieferanten Mayumi s](batch-inserting-cs/_static/image41.png)](batch-inserting-cs/_static/image40.png)

**Abbildung 14**: drei neue Teil des Gartens mein hinzugefügten für Lieferanten Mayumi s ([klicken Sie hier, um das Bild in voller Größe angezeigt](batch-inserting-cs/_static/image42.png))


[![Die neue Produkte finden Sie in der letzten Seite des GridView](batch-inserting-cs/_static/image44.png)](batch-inserting-cs/_static/image43.png)

**Abbildung 15**: der neue Produkte finden Sie in der letzten Seite des GridView ([klicken Sie hier, um das Bild in voller Größe angezeigt](batch-inserting-cs/_static/image45.png))


> [!NOTE]
> Einfügen von Logik, die in diesem Lernprogramm verwendete Batch dient als Wrapper für die einfügungen innerhalb des Bereichs der Transaktion. Um dies zu überprüfen, führen Sie absichtlich einen Fehler auf Datenbankebene. Z. B. anstelle eines neuen zuweisen `ProductsRow` Instanz s `CategoryID` Eigenschaft, um den ausgewählten Wert in der `Categories` DropDownList, weisen Sie es auf einen Wert wie `i * 5`. Hier `i` ist der Indexer für die Schleife und Werte im Bereich von 1 bis 5. Daher beim Hinzufügen von zwei oder mehr Produkte im Batch legen Sie die erste Produkt müssen eine gültige `CategoryID` Wert (5), aber nachfolgende Produkte haben `CategoryID` Werte, die nicht bis zu entsprechen `CategoryID` Werte in der `Categories` Tabelle. Im Endeffekt ist, die während der ersten `INSERT` funktionieren, alle weiteren mit einer foreign Key-einschränkungsverletzung fehl. Da die batcheinfügung atomar ist, ist der erste `INSERT` wird ein Rollback, zurückgeben, die Datenbank in den Zustand vor dem Einfügen des Batchs von Prozess gestartet wurde.


## <a name="summary"></a>Zusammenfassung

Über diese und die vorherigen zwei Lernprogramme wir Schnittstellen, die zum Aktualisieren, löschen, erstellt haben, und alle die transaktionsunterstützung, die wir, in der Datenzugriffsebene hinzugefügt verwendet Einfügen von Batches von Daten, die [Umbruch Datenbankänderungen innerhalb einer Transaktion](wrapping-database-modifications-within-a-transaction-cs.md) Lernprogramm. Für bestimmte Szenarien, solche Batch-Verarbeitung von Benutzeroberflächen erheblich Endbenutzer Effizienz verbessern durch Ausschneiden nach unten auf der Anzahl der Klicks und Postbacks Tastaturmaus-Kontextwechsel, und gleichzeitig auch die Integrität der zugrunde liegenden Daten.

Dieses Lernprogramm ist unsere Betrachtung der Arbeit mit Daten im Batch abgeschlossen. Der nächste Satz von Lernprogrammen wird erklärt, eine Vielzahl von Szenarien für erweiterte Data Access Layer, einschließlich der Verwendung von gespeicherter Prozeduren in den TableAdapter s Methoden erstellen, das Konfigurieren der Verbindung und Befehlsebene Einstellungen in der DAL, zum Verschlüsseln verwendete Verbindungszeichenfolgen und mehr!

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Führen Sie Prüfer für dieses Lernprogramm Hilton Giesenow und S Ren Jacob Lauritsen wurden. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Zurück](batch-deleting-cs.md)
[Weiter](wrapping-database-modifications-within-a-transaction-vb.md)
