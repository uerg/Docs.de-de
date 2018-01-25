---
uid: web-forms/overview/data-access/working-with-batched-data/batch-updating-vb
title: Batchaktualisierung (VB) | Microsoft Docs
author: rick-anderson
description: "Erfahren Sie, wie mehrere Datenbankdatensätze in einem einzigen Vorgang zu aktualisieren. In der Benutzeroberflächenebene erstellen wir eine GridView, in dem jede Zeile bearbeitbar ist. In den Daten..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: d191a204-d7ea-458d-b81c-0b9049ecb55f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-updating-vb
msc.type: authoredcontent
ms.openlocfilehash: bcfdf734de0b4a4aa0a11f35bd6e40d6b97719cf
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="batch-updating-vb"></a>Batchaktualisierung (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Herunterladen von Code](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_64_VB.zip) oder [PDF herunterladen](batch-updating-vb/_static/datatutorial64vb1.pdf)

> Erfahren Sie, wie mehrere Datenbankdatensätze in einem einzigen Vorgang zu aktualisieren. In der Benutzeroberflächenebene erstellen wir eine GridView, in dem jede Zeile bearbeitbar ist. In der Datenzugriffsebene umschließen wir mehrere Update-Vorgänge innerhalb einer Transaktion, um sicherzustellen, dass alle Updates erfolgreich ausgeführt oder alle Updates ein Rollback aus.


## <a name="introduction"></a>Einführung

In der [vorherigen Lernprogramm](wrapping-database-modifications-within-a-transaction-vb.md) wurde erläutert, wie der Datenzugriffsebene zum Hinzufügen der Unterstützung für Datenbanktransaktionen zu erweitern. Datenbanktransaktionen zu gewährleisten, dass eine Reihe von Anweisungen zur Datenänderung als ein atomarischer Vorgang behandelt werden, der sicherstellt, dass alle Änderungen werden oder alle fehlschlagen wird. Low-Level DAL Funktionalität aus dem Weg bereit wir re um unsere Aufmerksamkeit für das Erstellen von Batches Änderung Schnittstellen zu aktivieren.

In diesem Lernprogramm wird eine GridView erstellen, in dem jede Zeile bearbeitet werden kann (siehe Abbildung 1) ist. Da jede Zeile in die Bearbeitungsoberfläche dort s keine Notwendigkeit für eine Spalte vom Bearbeiten gerendert wird, aktualisieren Sie und brechen Sie ab (Schaltflächen). Stattdessen, es gibt zwei Produkte aktualisieren Schaltflächen auf der Seite, die beim Klicken auf die GridView Zeilen aufgelistet und die Datenbank aktualisieren.


[![Jede Zeile in der GridView ist bearbeitbar](batch-updating-vb/_static/image1.gif)](batch-updating-vb/_static/image1.png)

**Abbildung 1**: jede Zeile in der GridView ist bearbeitbar ([klicken Sie hier, um das Bild in voller Größe angezeigt](batch-updating-vb/_static/image2.png))


Lassen Sie s beginnen!

> [!NOTE]
> In der [BatchUpdates ausführen](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb.md) Lernprogramm erstellten eines Batch Bearbeitungsbereichs Schnittstelle. dabei wird das DataList-Steuerelement. In diesem Lernprogramm eingesetzt wird aus dem vorherigen, die zur verwendet wird, eine GridView, und das BatchUpdate innerhalb des Bereichs einer Transaktion ausgeführt wird. Nach Abschluss dieses Lernprogramms regelmäßigen ich zum früheren Lernprogramms zurückzukehren, und aktualisieren, damit die Datenbank transaktionsbezogenen Funktionalität hinzugefügt, in dem vorherigen Lernprogramm verwenden.


## <a name="examining-the-steps-for-making-all-gridview-rows-editable"></a>Untersuchen die Schritte für alle Zeilen der GridView bearbeitbar zu machen

Entsprechend der Anleitung unter dem [eine Übersicht der einfügen, aktualisieren und Löschen von Daten](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) Lernprogramm GridView bietet integrierte Unterstützung für die zugrunde liegenden Daten regelmäßig pro Zeile zu bearbeiten. Intern die GridView fest, welche Zeile über bearbeitbar ist seine [ `EditIndex` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.editindex(VS.80).aspx). Wie die GridView zu seiner Datenquelle gebunden wird, überprüft er jede Zeile aus, um festzustellen, ob der Index der Zeile der Wert von entspricht `EditIndex`. Wenn dies der Fall ist, Schnittstellen für diese Zeile s werden Felder mit ihren Bearbeiten gerendert. Für BoundFields, bearbeitende diese Schnittstelle ist ein Textfeld, deren `Text` Eigenschaft der Wert des Datenfelds, angegeben durch die BoundField-s. zugewiesen `DataField` Eigenschaft. Für von TemplateFields die `EditItemTemplate` anstelle von dient der `ItemTemplate`.

Beachten Sie, dass die Bearbeitung Workflow startet, klickt ein Benutzer eine Zeile s-Schaltfläche "Bearbeiten". Dies bewirkt, dass einen Postback, setzt die GridView s `EditIndex` -Eigenschaft auf den geklickt wurde Zeilenindex s und bindet die Daten in den Entwurfsbereich. Wenn eine Zeile s-Schaltfläche "Abbrechen" geklickt wird, beim Postback der `EditIndex` festgelegt ist, auf einen Wert von `-1` vor dem erneuten Binden der Daten in den Entwurfsbereich. Seit start des GridView-s-Zeilen Indizierung auf 0 (null) festlegen `EditIndex` auf `-1` wirkt sich die zum Anzeigen von GridView im Nur-Lese-Modus.

Die `EditIndex` Eigenschaft eignet sich gut für pro Zeile zu bearbeiten, kann aber nicht für den Batch zu bearbeiten. Damit die gesamte GridView bearbeitet werden kann, benötigen wir jede Zeile, mit dessen Bearbeitung Oberfläche gerendert werden. Die einfachste Möglichkeit hierfür besteht im erstellen, in dem jedes bearbeitbares Feld implementiert wird, gemäß einer TemplateField mit dessen Bearbeitung Oberfläche der `ItemTemplate`.

In den nächsten Schritten vollständig bearbeitbare GridView erstellen wir. In Schritt 1 wir beginnen, indem die GridView und seine ObjectDataSource erstellen und seine BoundFields und CheckBoxField in von TemplateFields konvertieren. In Schritt 2 und 3 verschieben wir die Bearbeitung Schnittstellen aus der von TemplateFields `EditItemTemplate` s, um ihre `ItemTemplate` s.

## <a name="step-1-displaying-product-information"></a>Schritt 1: Anzeigen von Produktinformationen

Vor Gedanken machen, wir erstellen eine GridView, in denen Zeilen sind, können bearbeitet werden, s starten, indem Sie einfach die Produktinformationen anzeigen lassen. Öffnen der `BatchUpdate.aspx` auf der Seite der `BatchData` Ordner, und ziehen Sie eine GridView aus der Toolbox in den Designer. Legen Sie die GridView s `ID` auf `ProductsGrid` , und wählen Sie das Smarttag, um die Bindung an eine neue, mit dem Namen ObjectDataSource `ProductsDataSource`. Konfigurieren der ObjectDataSource zum Abrufen der zugehörigen Daten aus der `ProductsBLL` Klasse s `GetProducts` Methode.


[![Konfigurieren der ObjectDataSource zur Verwendung der ProductsBLL-Klasse](batch-updating-vb/_static/image2.gif)](batch-updating-vb/_static/image3.png)

**Abbildung 2**: Konfigurieren der ObjectDataSource verwenden die `ProductsBLL` Klasse ([klicken Sie hier, um das Bild in voller Größe angezeigt](batch-updating-vb/_static/image4.png))


[![Abrufen von Produktdaten, die mithilfe der GetProducts-Methode](batch-updating-vb/_static/image3.gif)](batch-updating-vb/_static/image5.png)

**Abbildung 3**: Rufen Sie die Produkt-Daten mithilfe der `GetProducts` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](batch-updating-vb/_static/image6.png))


Wie die GridView dienen die ObjectDataSource s Änderung Funktionen regelmäßig pro Zeile zu arbeiten. Um eine Gruppe von Datensätzen zu aktualisieren, müssen wir viel Code in der ASP.NET Seite "s" Code-Behind-Klasse schreiben, die die Daten batches und übergibt sie an die BLL. Deshalb legen Sie die Dropdownlisten in das ObjectDataSource-s Update-, INSERT- und DELETE Registerkarten (keine). Klicken Sie auf ' Fertig stellen ', um den Assistenten abzuschließen.


[![Legen Sie die Dropdownlisten in der Update-, INSERT- und Löschen von Registerkarten auf (keine)](batch-updating-vb/_static/image4.gif)](batch-updating-vb/_static/image7.png)

**Abbildung 4**: Legen Sie das Dropdown-Listen in aktualisieren, einfügen und Löschen von Registerkarten auf (keine) ([klicken Sie hier, um das Bild in voller Größe angezeigt](batch-updating-vb/_static/image8.png))


Nach Abschluss der Assistent zum Konfigurieren von Datenquellen, sollte das ObjectDataSource s deklarative Markup wie folgt aussehen:


[!code-aspx[Main](batch-updating-vb/samples/sample1.aspx)]

Assistenten zum Konfigurieren von Datenquellen wird auch in Visual Studio BoundFields und eine CheckBoxField für das Produkt von Datenfeldern in die GridView zu erstellen. Können Sie für dieses Lernprogramm nur ermöglicht dem Benutzer das Anzeigen und bearbeiten die s-Produktname, Kategorie, Preis und nicht mehr unterstützte Status s ein. Entfernen Sie alle außer den `ProductName`, `CategoryName`, `UnitPrice`, und `Discontinued` Felder und benennen Sie die `HeaderText` Eigenschaften der ersten drei Felder Product Category und Preis,. Schließlich die Kontrollkästchen der Paging aktivieren und die Sortierung in die GridView-s-Smarttag.

An diesem Punkt hat die GridView drei BoundFields (`ProductName`, `CategoryName`, und `UnitPrice`) und eine CheckBoxField (`Discontinued`). Wir benötigen diese vier Felder in von TemplateFields konvertieren, und verschieben Sie die Bearbeitungsoberfläche aus den s TemplateField `EditItemTemplate` auf seine `ItemTemplate`.

> [!NOTE]
> Wir erstellen und Anpassen von TemplateFields in untersucht die [Anpassen der Benutzeroberfläche für die Änderung der Daten](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) Lernprogramm. Wir müssen die Schritte zum Umwandeln der BoundFields und CheckBoxField von TemplateFields durchlaufen und Schnittstellen definieren, deren Bearbeitung ihrer `ItemTemplate` s, aber wenn Sie Festfahren oder muss aufzufrischen, Don ' t in jedem Fall auf dieses Lernprogramm weiter oben Bezug nehmen.


Klicken Sie auf den GridView s smart Tag auf die Spalten bearbeiten-Link, um die Felder (Dialogfeld) zu öffnen. Als nächstes wählen Sie jedes Feld, und klicken Sie auf dieses Feld konvertieren in einen TemplateField-Link.


![Konvertieren Sie die vorhandenen BoundFields und CheckBoxField in von TemplateFields](batch-updating-vb/_static/image5.gif)

**Abbildung 5**: Konvertieren Sie die vorhandenen BoundFields und CheckBoxField in von TemplateFields


Jedes Feld eine TemplateField ist,-Schnittstelle wir re kann jetzt so verschieben Sie die Bearbeitung aus dem `EditItemTemplate` s, um die `ItemTemplate` s.

## <a name="step-2-creating-theproductnameunitprice-anddiscontinuedediting-interfaces"></a>Schritt 2: Erstellen der`ProductName`,`UnitPrice`, und`Discontinued`Schnittstellen bearbeiten

Erstellen der `ProductName`, `UnitPrice`, und `Discontinued` bearbeiten Schnittstellen sind im Thema dieses Schritts und unkompliziert, wie jede Schnittstelle bereits in der TemplateField s definiert ist `EditItemTemplate`. Erstellen der `CategoryName` Schnittstelle bearbeiten ist etwas komplizierter, da wir DropDownList die zutreffenden Kategorien erstellen müssen. Dies `CategoryName` Schnittstelle bearbeiten in Schritt 3 behandelt wird.

S mit beginnen können die `ProductName` TemplateField. Klicken Sie auf den Link Bearbeiten von Vorlagen aus dem GridView-s-Smarttag und einen Drilldown nach unten, um die `ProductName` TemplateField s `EditItemTemplate`. Wählen Sie das Textfeld, in die Zwischenablage kopieren und fügen Sie ihn auf die `ProductName` TemplateField s `ItemTemplate`. Ändern Sie das Textfeld s `ID` Eigenschaft `ProductName`.

Als Nächstes fügen Sie einen RequiredFieldValidator auf die `ItemTemplate` , stellen Sie sicher, dass der Benutzer einen Wert für jeden Produktnamen s zur Verfügung. Legen Sie die `ControlToValidate` Eigenschaft ProductName, die `ErrorMessage` Eigenschaft an Sie müssen den Namen des Produkts enthalten. und die `Text` Eigenschaft \*. Nach dem Herstellen dieser Ergänzungen für das `ItemTemplate`, sollte am Bildschirm wie in Abbildung 6.


[![Die ProductName TemplateField jetzt umfasst ein Textfeld und eine RequiredFieldValidator](batch-updating-vb/_static/image6.gif)](batch-updating-vb/_static/image9.png)

**Abbildung 6**: die `ProductName` TemplateField enthält jetzt ein Textfeld und eine RequiredFieldValidator ([klicken Sie hier, um das Bild in voller Größe angezeigt](batch-updating-vb/_static/image10.png))


Für die `UnitPrice` bearbeiten die Schnittstelle, durch Kopieren das Textfeld aus starten der `EditItemTemplate` auf die `ItemTemplate`. Platzieren Sie als Nächstes ein $ vor die Textfeld, und legen seine `ID` UnitPrice-Eigenschaft und die zugehörige `Columns` Eigenschaft auf 8.

Auch eine CompareValidator zum Hinzufügen der `UnitPrice` s `ItemTemplate` um sicherzustellen, dass der vom Benutzer eingegebene Wert eine gültige Currency-Wert größer als oder gleich 0,00 ist. Legen Sie das Validierungssteuerelement s `ControlToValidate` Eigenschaft UnitPrice, seine `ErrorMessage` Eigenschaft, um Sie müssen einen gültigen Währungswert eingeben. Sie lassen Währung Symbole., seine `Text` Eigenschaft, um \*, dessen `Type` Eigenschaft `Currency`, dessen `Operator` Eigenschaft, um `GreaterThanEqual`, und die zugehörige `ValueToCompare` Eigenschaft auf 0.


[![Das Hinzufügen eines ein CompareValidator, um sicherzustellen, dass der Preis eingegeben ein nicht negativer Currency-Wert ist](batch-updating-vb/_static/image7.gif)](batch-updating-vb/_static/image11.png)

**Abbildung 7**: Hinzufügen einer CompareValidator, um sicherzustellen, dass der Preis eingegeben ist ein nicht negativer Currency-Wert ([klicken Sie hier, um das Bild in voller Größe angezeigt](batch-updating-vb/_static/image12.png))


Für die `Discontinued` TemplateField können Sie das Kontrollkästchen, die bereits definiert, der `ItemTemplate`. Legen Sie einfach seine `ID` zu Discontinued und die zugehörige `Enabled` Eigenschaft, um `True`.

## <a name="step-3-creating-thecategorynameediting-interface"></a>Schritt 3: Erstellen der`CategoryName`Schnittstelle bearbeiten

Die Bearbeitungsoberfläche in der `CategoryName` TemplateField s `EditItemTemplate` enthält ein Textfeld, das den Wert des anzeigt die `CategoryName` Feld "Daten". Wir müssen mit einer DropDownList ersetzen, die die möglichen Kategorien aufgelistet.

> [!NOTE]
> Die [Anpassen der Benutzeroberfläche für die Änderung der Daten](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) Lernprogramm enthält eine Erläuterung mehr umfassende und vollständige zum Anpassen einer Vorlage für das Einschließen einer DropDownList im Gegensatz zu einem Textfeld. Während der folgenden Schritte abgeschlossen sind, werden sie nur sehr knapp dargestellt. Eine eingehendere Betrachtung erstellen und konfigurieren die Kategorien DropDownList finden Sie zurück in die [Anpassen der Benutzeroberfläche für die Änderung der Daten](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) Lernprogramm.


Ziehen eine DropDownList aus der Toolbox auf die `CategoryName` TemplateField s `ItemTemplate`wird durch das Festlegen der `ID` zu `Categories`. An diesem Punkt würde in der Regel die DropDownLists s-Datenquelle über das Smarttag, definieren wir erstellen eine neue ObjectDataSource. Dadurch wird jedoch hinzugefügt, das ObjectDataSource innerhalb der `ItemTemplate`, die führt zu einer ObjectDataSource-Instanz, die für jede Zeile GridView erstellt. Stattdessen können Sie s ObjectDataSource außerhalb der GridView s von TemplateFields zu erstellen. Beenden Sie die vorlagenbearbeitung, und ziehen Sie ein ObjectDataSource aus der Toolbox in den Designer unter dem `ProductsDataSource` ObjectDataSource. Benennen Sie die neue ObjectDataSource `CategoriesDataSource` und konfigurieren es für das Verwenden der `CategoriesBLL` Klasse s `GetCategories` Methode.


[![Konfigurieren der ObjectDataSource zur Verwendung der CategoriesBLL-Klasse](batch-updating-vb/_static/image8.gif)](batch-updating-vb/_static/image13.png)

**Abbildung 8**: Konfigurieren der ObjectDataSource verwenden die `CategoriesBLL` Klasse ([klicken Sie hier, um das Bild in voller Größe angezeigt](batch-updating-vb/_static/image14.png))


[![Abrufen von Kategoriedaten, die mithilfe der GetCategories-Methode](batch-updating-vb/_static/image9.gif)](batch-updating-vb/_static/image15.png)

**Abbildung 9**: Rufen Sie die Kategorie Daten mithilfe der `GetCategories` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](batch-updating-vb/_static/image16.png))


Da diese ObjectDataSource lediglich zum Abrufen von Daten verwendet wird, legen Sie die Dropdownlisten in die Update- und DELETE-Registerkarten, um (keine). Klicken Sie auf ' Fertig stellen ', um den Assistenten abzuschließen.


[![Festlegen der Dropdownlisten in der Update- und DELETE-Registerkarten, um (keine)](batch-updating-vb/_static/image10.gif)](batch-updating-vb/_static/image17.png)

**Abbildung 10**: Legen Sie das Dropdown-Listen in aktualisieren und Löschen von Registerkarten auf (keine) ([klicken Sie hier, um das Bild in voller Größe angezeigt](batch-updating-vb/_static/image18.png))


Nach Abschluss des Assistenten die `CategoriesDataSource` s deklarativem Markup sollte wie folgt aussehen:


[!code-aspx[Main](batch-updating-vb/samples/sample2.aspx)]

Mit der `CategoriesDataSource` erstellt und konfiguriert wurde, zurück zu den `CategoryName` TemplateField s `ItemTemplate` und den DropDownList s smart Tag, klicken Sie auf den Link für die Datenquelle auswählen. Wählen Sie im Datenquellen-Assistenten die `CategoriesDataSource` in der ersten Dropdown-Liste aus, und haben die Möglichkeit, `CategoryName` verwendet für die Anzeige und `CategoryID` als Wert.


[![Der DropDownList an die CategoriesDataSource binden](batch-updating-vb/_static/image11.gif)](batch-updating-vb/_static/image19.png)

**Abbildung 11**: der DropDownList zum Binden der `CategoriesDataSource` ([klicken Sie hier, um das Bild in voller Größe angezeigt](batch-updating-vb/_static/image20.png))


An diesem Punkt der `Categories` DropDownList zeigt eine Liste aller Kategorien ausgewählt, jedoch es nicht noch automatisch die entsprechende Kategorie für das Produkt, die auf die Zeile GridView gebunden. Um dies zu erreichen wir festlegen müssen der `Categories` DropDownList s `SelectedValue` für das Produkt s `CategoryID` Wert. Klicken Sie auf die Verknüpfung der DropDownList-s-Smarttag-Datenbindungen bearbeiten, und ordnen Sie die `SelectedValue` Eigenschaft mit dem `CategoryID` Datenfeld der Spalte wie in Abbildung 12 dargestellt.


![Binden Sie den Wert des Produkts s CategoryID zur Eigenschaft "SelectedValue" s DropDownList](batch-updating-vb/_static/image12.gif)

**Abbildung 12**: gebunden Produkt s `CategoryID` Wert mit dem DropDownList s `SelectedValue` Eigenschaft


Eine letzte Problem bleibt: ggf. t Produkt verfügt über eine `CategoryID` Wert angegeben wird, die Databinding-Anweisung auf `SelectedValue` wird eine Ausnahme ausgelöst. Grund hierfür ist der DropDownList enthält nur die Elemente für die Kategorien und bietet keine Option ist für diese Produkte mit einem `NULL` Wert für die Datenbank `CategoryID`. Legen Sie zur Behebung des Problems, der DropDownList s `AppendDataBoundItems` Eigenschaft `True` und Hinzufügen eines neues Elements zu der DropDownList Auslassen der `Value` Eigenschaft vom deklarative Syntax. Stellen Sie also sicher, dass die `Categories` DropDownList s deklarativer Syntax sieht wie folgt:


[!code-aspx[Main](batch-updating-vb/samples/sample3.aspx)]

Hinweis wie die `<asp:ListItem Value="">` – wählen Sie eine--hat seine `Value` Attribut explizit festgelegt werden, um eine leere Zeichenfolge. Verweisen auf die [Anpassen der Benutzeroberfläche für die Änderung der Daten](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) Lernprogramm eine ausführlichere Diskussion zur warum dieses zusätzliche DropDownList-Element erforderlich ist, behandelt der `NULL` Fall und warum Zuweisung von der `Value` die Eigenschaft auf eine leere Zeichenfolge ist wichtig.

> [!NOTE]
> Es ist ein potenzielle Leistung und Skalierbarkeit Problem hier, das beachtet werden. Da jede Zeile eine DropDownList aufweist, verwendet der `CategoriesDataSource` als Datenquelle, die `CategoriesBLL` Klasse s `GetCategories` -Methode wird aufgerufen,  *n*  Besuchen der Versuche pro Seite, in dem  *n*  ist die Anzahl der Zeilen in der GridView. Diese  *n*  Aufrufe von `GetCategories` ergäben  *n*  Abfragen der Datenbank. Diese Auswirkungen auf die Datenbank konnte durch Zwischenspeichern der zurückgegebenen Kategorien in einem Cache pro Anforderung oder über die Caching-Schicht, die mit einer Abhängigkeit oder eine sehr kurze Zeit-basierte Ablauf zwischengespeichert SQL verkürzt werden. Weitere Informationen zu den pro-Anforderung, die Option zum Zwischenspeichern finden Sie unter [ `HttpContext.Items` einen Cache-Speicher pro Anforderung](http://aspnet.4guysfromrolla.com/articles/060904-1.aspx).


## <a name="step-4-completing-the-editing-interface"></a>Schritt 4: Abschließen der Schnittstelle bearbeiten

Es gibt es eine Reihe von Änderungen an die GridView-s-Vorlagen ohne anzuhalten, wenn Sie unseren Fortschritt anzeigen vorgenommen. Nehmen Sie einen Moment Zeit, um unseren Fortschritt über einen Browser anzuzeigen. Wie in Abbildung 13 gezeigt, jede Zeile wird gerendert, mit dessen `ItemTemplate`, enthält die Zelle s Schnittstelle bearbeiten.


[![Jede Zeile GridView ist bearbeitbar](batch-updating-vb/_static/image13.gif)](batch-updating-vb/_static/image21.png)

**Abbildung 13**: jede Zeile GridView ist bearbeitbar ([klicken Sie hier, um das Bild in voller Größe angezeigt](batch-updating-vb/_static/image22.png))


Es gibt wenige kleinere Probleme bei der Formatierung, denen wir an diesem Punkt erledigen sollte. Beachten Sie, dass zunächst die `UnitPrice` Wert enthält vier Dezimalstellen. Zur Beseitigung dieses Problems zurückgeben, um die `UnitPrice` TemplateField s `ItemTemplate` und dann auf das Textfeld s smart Tag auf den Link-Datenbindungen bearbeiten. Als Nächstes angeben, die die `Text` Eigenschaft als Zahl formatiert werden sollen.


![Die Texteigenschaft als Zahl formatieren](batch-updating-vb/_static/image14.gif)

**Abbildung 14**: Format der `Text` Eigenschaft als Zahl


Zweitens können s zentrieren Sie das Kontrollkästchen in der `Discontinued` Spalte (anstatt es linksbündig ausgerichtet). Klicken Sie auf Spalten bearbeiten, aus dem GridView-s-Smarttag, und wählen Sie die `Discontinued` TemplateField aus der Liste der Felder in der unteren linken Ecke. Drilldown `ItemStyle` und legen Sie die `HorizontalAlign` Eigenschaft Center wie in Abbildung 15 gezeigt.


![Zentrieren Sie das Kontrollkästchen nicht mehr unterstützte](batch-updating-vb/_static/image15.gif)

**Abbildung 15**: Center die `Discontinued` Kontrollkästchen


Als Nächstes die Seite ein ValidationSummary-Steuerelement hinzu, und legen Sie seine `ShowMessageBox` Eigenschaft `True` und seine `ShowSummary` Eigenschaft `False`. Zudem dazu führen, dass die Schaltfläche Web steuert, die beim Klicken auf, wird die s benutzeränderungen aktualisiert. Insbesondere zwei Schaltfläche Web Steuerelemente hinzufügen, einen oben GridView und darunter liegenden, beide Steuerelemente festlegen `Text` Updateprodukten Eigenschaften.

Seit der GridView s Schnittstelle bearbeiten ist definiert in seiner von TemplateFields `ItemTemplate` s, die `EditItemTemplate` s sind überflüssig und können gelöscht werden.

Nach der Formatierung Änderungen vornehmen, die oben erwähnt werden, die Schaltflächen-Steuerelemente hinzufügen und entfernen Sie die unnötige `EditItemTemplate` s, die Seite "s" deklarative Syntax sollte wie folgt aussehen:


[!code-aspx[Main](batch-updating-vb/samples/sample4.aspx)]

Abbildung 16 zeigt diese Seite, wenn Sie über einen Browser angezeigt werden soll, nachdem die Schaltfläche Websteuerelemente hinzugefügt wurden und die Formatierung vorgenommenen Änderungen.


[![Die Seite jetzt umfasst zwei Update-Produkte-Schaltflächen](batch-updating-vb/_static/image16.gif)](batch-updating-vb/_static/image23.png)

**Abbildung 16**: die Seite jetzt umfasst zwei Update Produkte Schaltflächen ([klicken Sie hier, um das Bild in voller Größe angezeigt](batch-updating-vb/_static/image24.png))


## <a name="step-5-updating-the-products"></a>Schritt 5: Aktualisieren die Produkte

Wenn ein Benutzer auf dieser Seite besucht sie ihre Änderungen vornehmen und klicken Sie dann auf eine der beiden Updateprodukten Schaltflächen. An diesem Punkt müssen wir irgendwie speichern, die vom Benutzer eingegebenen Werten für jede Zeile in einer `ProductsDataTable` Instanz, und übergeben Sie, um eine BLL-Methode, die dann besteht, `ProductsDataTable` Instanz mit dem DAL s `UpdateWithTransaction` Methode. Die `UpdateWithTransaction` -Methode, die wir in erstellt die [vorherigen Lernprogramm](wrapping-database-modifications-within-a-transaction-vb.md), wird sichergestellt, dass der Batch von Änderungen als atomarer Vorgang aktualisiert wird.

Erstellen Sie eine Methode mit dem Namen `BatchUpdate` in `BatchUpdate.aspx.vb` und fügen Sie den folgenden Code hinzu:


[!code-vb[Main](batch-updating-vb/samples/sample5.vb)]

Diese Methode startet durch Abrufen aller Produkte zurück in ein `ProductsDataTable` über einen Aufruf an die BLL s `GetProducts` Methode. Er dann listet der `ProductGrid` GridView s [ `Rows` Auflistung](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rows(VS.80).aspx). Die `Rows` Auflistung enthält ein [ `GridViewRow` Instanz](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewrow.aspx) für jede Zeile in der GridView angezeigt. Da wir maximal zehn Zeilen pro Seite die GridView s zeigen `Rows` Auflistung wird nicht mehr als zehn Elemente haben.

Für jede Zeile der `ProductID` ist stammt aus der `DataKeys` Auflistung und die entsprechenden `ProductsRow` ausgewählt ist, aus der `ProductsDataTable`. Die vier TemplateField-Eingabesteuerelemente programmgesteuert verwiesen und ihre Werte zugewiesen werden, um die `ProductsRow` s Instanzeigenschaften. Nach jeder GridView Zeilenwerte s verwendet werden, um Aktualisieren der `ProductsDataTable`, es s übergeben, mit dem BLL s `UpdateWithTransaction` Methode, wie wir gesehen, in dem vorherigen Lernprogramm haben einfach in die DAL s unten `UpdateWithTransaction` Methode.

Der Batch-Update-Algorithmus für dieses Lernprogramm verwendete aktualisiert jede Zeile in der `ProductsDataTable` entspricht, die eine Zeile in der GridView, unabhängig davon, ob die Produktinformationen s geändert wurde. Während solche Blind sind t normalerweise ein Leistungsproblem aktualisiert, können sie zu überflüssig Datensätze führen, wenn Sie die Überwachung der Datenbanktabelle ändert. In der [BatchUpdates ausführen](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb.md) Lernprogramm wir eine Batchaktualisierung-Schnittstelle mit DataList untersucht und Code, der nur die Datensätze aktualisiert, die tatsächlich vom Benutzer geändert wurden hinzugefügt. Wünschen, verwenden Sie die Methoden von [BatchUpdates ausführen](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb.md) so aktualisieren den Code in diesem Lernprogramm, falls gewünscht.

> [!NOTE]
> Beim Binden von der Datenquelle an die GridView über seine Smarttag weist Visual Studio automatisch Werte für die primären Data Source s-Schlüssel mit dem GridView s `DataKeyNames` Eigenschaft. Wenn Sie nicht das ObjectDataSource an die GridView über das Smarttag für GridView s binden, wie in Schritt 1 beschrieben, müssen Sie manuell festlegen, die GridView s `DataKeyNames` -Eigenschaft auf "ProductID", um Zugriff auf die `ProductID` Wert für jede Zeile über die `DataKeys` Auflistung.


Verwendete Code `BatchUpdate` ähnelt, die in der BLL s verwendet `UpdateProduct` Methoden, die der Hauptunterschied liegt, wird der in der `UpdateProduct` Methoden nur einen einzigen `ProductRow` Instanz aus der Architektur abgerufen wird. Der Code, der die Eigenschaften des weist die `ProductRow` ist die gleiche Weise die `UpdateProducts` Methoden und den Code in der `For Each` wurde eine Schleife in `BatchUpdate`, wie das allgemeine Muster ist.

Um dieses Lernprogramm abzuschließen, müssen wir die `BatchUpdate` Methode wird aufgerufen, wenn einer der Updateprodukten Schaltflächen geklickt wird. Erstellen von Ereignishandlern für die `Click` Ereignisse dieser beiden Schaltfläche Steuerelemente, und fügen Sie den folgenden Code in den Ereignishandlern:


[!code-vb[Main](batch-updating-vb/samples/sample6.vb)]

Zunächst erfolgt ein Aufruf zum `BatchUpdate`. Als Nächstes wird die [ `ClientScript` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.page.clientscript(VS.80).aspx) wird verwendet, um JavaScript einfügen, die eine MessageBox-Datenbank, die gelesen angezeigt werden, die Produkte wurden aktualisiert.

Nehmen Sie sich, diesen Code zu testen. Besuchen Sie `BatchUpdate.aspx` über einen Webbrowser, und bearbeiten Sie eine Anzahl von Zeilen, und klicken Sie auf eine der Schaltflächen Updateprodukten. Vorausgesetzt, dass keine Überprüfung der Eingabe-Fehler vorliegen, sollte eine MessageBox-Datenbank angezeigt werden, die gelesen wird, dass die Produkte aktualisiert wurden. Um die Unteilbarkeit des Updates zu überprüfen, sollten Sie einen zufälligen hinzufügen `CHECK` Einschränkung, wie die lässt keine `UnitPrice` 1234.56 Werte. Klicken Sie dann aus `BatchUpdate.aspx`, bearbeiten Sie eine Anzahl von Datensätzen, die sicherstellen festzulegende eines Produkts s `UnitPrice` Wert auf den unzulässigen Wert (1234.56). Dies sollte zu einem Fehler führen, wenn Updateprodukten mit den anderen Änderungen auf, während dieses Batchvorgangs auf ihre ursprünglichen Werte zurückgesetzt.

## <a name="an-alternativebatchupdatemethod"></a>Eine Alternative`BatchUpdate`Methode

Die `BatchUpdate` wir einfach Methode untersucht ruft *alle* der Produkte von den BLL s `GetProducts` Methode und aktualisiert dann nur die Datensätze, die in der GridView angezeigt werden. Dieser Ansatz eignet sich, wenn die GridView Paging nicht verwendet, aber wenn dies der Fall ist, gibt es möglicherweise Hunderte oder Tausende Zehntausenden von Produkten, aber nur zehn Zeilen in die GridView. In einem solchen Fall ist das Abrufen aller Produkte aus der Datenbank nur ändern, davon 10 kleiner als ideal.

Für derartige Situationen sollten Sie die folgenden `BatchUpdateAlternate` Methode stattdessen:


[!code-vb[Main](batch-updating-vb/samples/sample7.vb)]

`BatchMethodAlternate`Zunächst erstellen ein neues leeres `ProductsDataTable` mit dem Namen `products`. Es wird dann die GridView s werden die Schritte `Rows` Auflistung und für jede Zeile, die bestimmten Produktinformationen, die mit der BLL s ruft `GetProductByProductID(productID)` Methode. Der abgerufene `ProductsRow` Instanz verfügt über Eigenschaften, die in der gleichen Weise wie aktualisiert `BatchUpdate`, jedoch erst nach dem Aktualisieren der Zeile, die sie in importieren die `products` `ProductsDataTable` über die DataTable s [ `ImportRow(DataRow)` Methode](https://msdn.microsoft.com/library/system.data.datatable.importrow(VS.80).aspx).

Nach der `For Each` Schleife abgeschlossen wird, `products` enthält mindestens ein `ProductsRow` Instanz für jede Zeile in der GridView. Da jedes von der `ProductsRow` Instanzen hinzugefügt wurden der `products` (anstelle von aktualisiert), wenn wir es Blind übergeben der `UpdateWithTransaction` Methode der `ProductsTableAdatper` wird versucht, jede der Datensätze in die Datenbank eingefügt. Stattdessen müssen wir angeben, dass jede dieser Zeilen (nicht hinzugefügt) geändert wurde.

Dies kann durch Hinzufügen einer neuen Methode, die mit dem Namen BLL `UpdateProductsWithTransaction`. `UpdateProductsWithTransaction`, unten legt der `RowState` aller der `ProductsRow` -Instanzen lautet in der `ProductsDataTable` auf `Modified` und übergibt dann die `ProductsDataTable` der DAL-s `UpdateWithTransaction` Methode.


[!code-vb[Main](batch-updating-vb/samples/sample8.vb)]

## <a name="summary"></a>Zusammenfassung

Die GridView stellt integrierte zeilenspezifische Bearbeitungsfunktionen zur Verfügung, jedoch verfügt nicht über Unterstützung für das Erstellen von vollständig bearbeitbar Schnittstellen. Wie in diesem Lernprogramm wurde gezeigt, solche Schnittstellen sind möglich, aber viel Arbeit erfordert. Um eine GridView zu erstellen, in dem jede Zeile bearbeitet werden kann, muss die GridView-s-Felder in von TemplateFields konvertieren, und definieren Sie die Bearbeitung Schnittstelle innerhalb der `ItemTemplate` s. Aktualisieren Sie darüber hinaus alle - Schaltfläche Websteuerelemente Typ muss die Seite, die getrennt von der GridView hinzugefügt werden. Diese Schaltflächen `Click` Ereignishandler müssen die GridView s auflisten `Rows` -Auflistung, speichern Sie die Änderungen in einem `ProductsDataTable`, und übergeben die aktualisierte Informationen in die entsprechende BLL-Methode.

In den nächsten Lernprogrammen sehen wir, wie eine Schnittstelle für die batchlöschung erstellt werden. Insbesondere jede GridView Zeile wird ein Kontrollkästchen und anstelle von aktualisiert alle-Geben Sie die Schaltflächen, lassen wir Schaltflächen ausgewählten Zeilen löschen.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurden Teresa Murphy und David Suru. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Zurück](wrapping-database-modifications-within-a-transaction-vb.md)
[Weiter](batch-deleting-vb.md)
