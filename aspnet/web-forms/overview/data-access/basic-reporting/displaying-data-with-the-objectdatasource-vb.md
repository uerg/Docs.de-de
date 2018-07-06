---
uid: web-forms/overview/data-access/basic-reporting/displaying-data-with-the-objectdatasource-vb
title: Anzeigen von Daten mit dem ObjectDataSource-Steuerelement (VB) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial überprüft das ObjectDataSource-Steuerelement, das mit diesem Steuerelement, das von der BLL, die im vorherigen Tutorial ohne Havi erstellte abgerufene Daten gebunden werden kann...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: d62c3a63-0940-4019-874e-4a4047df0c1c
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/displaying-data-with-the-objectdatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: a4be0d2096824f95a4e21294c35e36c0badb7cfb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400667"
---
<a name="displaying-data-with-the-objectdatasource-vb"></a>Anzeigen von Daten mit dem ObjectDataSource-Steuerelement (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunter](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_4_VB.exe) oder [PDF-Datei herunterladen](displaying-data-with-the-objectdatasource-vb/_static/datatutorial04vb1.pdf)

> Dieses Lernprogramm wird dargestellt das ObjectDataSource-Steuerelement, das mit diesem Steuerelement, das Sie von der BLL, die im vorherigen Tutorial erstellt haben, ohne eine einzige Zeile Code schreiben zu müssen abgerufene Daten binden können!


## <a name="introduction"></a>Einführung

Mit unserer Anwendung Architektur und die Website Seitenlayout abgeschlossen können wir erkunden, wie Sie eine Vielzahl von Daten - und reporting-bezogenen Aufgaben durchführen können. In den vorherigen Tutorials haben wir gesehen, wie Sie Daten aus der DAL und BLL in einen Daten-Websteuerelement auf einer ASP.NET-Seite programmgesteuert zu binden. Diese Syntax, die der Daten-Websteuerelement zuweisen `DataSource` auf die Daten anzeigen und zum Aufrufen des Steuerelements `DataBind()` Methode wurde das Muster in ASP.NET 1.x-Anwendungen und können weiterhin in Ihrem 2.0-Anwendungen verwendet werden. ASP.NET 2.0 neue Datenquellen-Steuerelemente bieten jedoch eine deklarative Möglichkeit zum Arbeiten mit Daten. Mithilfe dieser Steuerelemente können Sie von der BLL, die im vorherigen Tutorial erstellt haben, ohne eine einzige Zeile Code schreiben zu müssen abgerufene Daten binden!

ASP.NET 2.0 im Lieferumfang von fünf integrierten Datenquellen-Steuerelemente [SqlDataSource](https://msdn.microsoft.com/library/dz12d98w%28vs.80%29.aspx), [AccessDataSource](https://msdn.microsoft.com/library/8e5545e1.aspx), ["ObjectDataSource"](https://msdn.microsoft.com/library/9a4kyhcx.aspx), [XmlDataSource](https://msdn.microsoft.com/library/e8d8587a%28en-US,VS.80%29.aspx), und [SiteMapDataSource](https://msdn.microsoft.com/library/5ex9t96x%28en-US,VS.80%29.aspx) zwar können Sie Ihre eigenen erstellen [benutzerdefinierte Datenquellen-Steuerelemente](https://msdn.microsoft.com/library/default.asp?url=/library/dnvs05/html/DataSourceCon1.asp), falls erforderlich. Da wir eine Architektur für unser Tutorial Anwendung entwickelt haben, wird dem ObjectDataSource-Steuerelement für die BLL-Klassen verwendet werden.


![ASP.NET 2.0 umfasst fünf integrierten Datenquellen-Steuerelemente](displaying-data-with-the-objectdatasource-vb/_static/image1.png)

**Abbildung 1**: ASP.NET 2.0 umfasst fünf integrierten Datenquellen-Steuerelemente


Dem ObjectDataSource-Steuerelement dient als Proxy für die Arbeit mit einem anderen Objekt. So konfigurieren Sie das "ObjectDataSource" geben wir diese zugrunde liegenden Objekt und seine Methoden wie dem ObjectDataSource-Steuerelement zugeordnet `Select`, `Insert`, `Update`, und `Delete` Methoden. Sobald diese zugrunde liegenden Objekts angegeben wurde, und seine Methoden, die dem ObjectDataSource-Steuerelement zugeordnet, können wir dann dem ObjectDataSource-Steuerelement in einen Daten-Websteuerelement binden. Im Lieferumfang von ASP.NET sind viele Daten Websteuerelemente, einschließlich der GridView, DetailsView, RadioButtonList und DropDownList, u. a. Die Daten-Websteuerelement möglicherweise während des Lebenszyklus der Seite, auf die Daten, die es gebunden ist, die sie für die Ausführung wird durch den Aufruf der "ObjectDataSource" `Select` Methode; Wenn die Daten-Websteuerelement einfügen unterstützen, aktualisieren oder löschen, aufgerufen werden können die Der ObjectDataSource `Insert`, `Update`, oder `Delete` Methoden. Diese Aufrufe werden dann von dem ObjectDataSource-Steuerelement an das entsprechende zugrunde liegende Objekt Methoden weitergeleitet, wie das folgende Diagramm veranschaulicht.


[![Dem ObjectDataSource-Steuerelement dient als Proxy](displaying-data-with-the-objectdatasource-vb/_static/image3.png)](displaying-data-with-the-objectdatasource-vb/_static/image2.png)

**Abbildung 2**: das ObjectDataSource-Steuerelement dient als Proxy ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-data-with-the-objectdatasource-vb/_static/image4.png))


Während dem ObjectDataSource-Steuerelement kann, zum Aufrufen von Methoden zum Einfügen verwendet werden, aktualisieren oder Löschen von Daten, konzentrieren wir uns nur für die Rückgabe von Daten; zukünftige werden Tutorials werden mit dem ObjectDataSource-Steuerelement und die Daten Websteuerelemente, die Daten ändern.

## <a name="step-1-adding-and-configuring-the-objectdatasource-control"></a>Schritt 1: Hinzufügen und konfigurieren das ObjectDataSource-Steuerelement

Öffnen Sie zunächst die `SimpleDisplay.aspx` auf der Seite die `BasicReporting` Ordner, in die Entwurfsansicht wechseln, und klicken Sie dann ein ObjectDataSource-Steuerelement aus der Toolbox auf der Seite auf die Entwurfsoberfläche ziehen. Dem ObjectDataSource-Steuerelement wird als ein graues Feld auf der Entwurfsoberfläche angezeigt, da er keine Markup erzeugt. Sie greift einfach auf Daten durch Aufrufen einer Methode aus einem angegebenen Objekt. Die von einem ObjectDataSource-Steuerelement zurückgegebenen Daten können von einem Web-Steuerelement, z. B. die GridView, DetailsView, FormView und So weiter angezeigt werden.

> [!NOTE]
> Alternativ Sie eventuell zunächst fügen Sie die Web-Steuerelement auf der Seite und wählen Sie dann aus der Smarttag, das &lt;neue Datenquelle&gt; Option in der Dropdown-Liste.


Um anzugeben, dem ObjectDataSource-Steuerelement zugrunde liegende Objekt und wie die Methoden dieses Objekts dem ObjectDataSource-Steuerelement zugeordnet werden, klicken Sie auf auf den Link "Datenquelle konfigurieren" aus dem ObjectDataSource-Steuerelement Smarttag.


[![Klicken Sie auf den Link "Quelle" Daten aus dem Smarttag konfigurieren](displaying-data-with-the-objectdatasource-vb/_static/image6.png)](displaying-data-with-the-objectdatasource-vb/_static/image5.png)

**Abbildung 3**: Klicken Sie auf den nach unten konfigurieren Datenquellenlink aus dem Smarttag ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-data-with-the-objectdatasource-vb/_static/image7.png))


Daraufhin wird das Konfigurieren von Datenquellen-Assistent aufgerufen. Zuerst müssen wir das Objekt angeben, die Arbeit mit dem ObjectDataSource-Steuerelement ist. Wenn das Kontrollkästchen "Data-Komponenten nur anzeigen" aktiviert ist, listet die Dropdown-Liste auf diesem Bildschirm nur die Objekte, die mit ergänzt wurden, haben die `DataObject` Attribut. Unsere Liste enthält derzeit die TableAdapters, in das typisierte DataSet und den BLL-Klassen, die wir im vorherigen Tutorial erstellt haben. Wenn Sie vergessen haben, fügen die `DataObject` Attribut für die Business Logic Layer-Klassen nicht angezeigt werden in dieser Liste. In diesem Fall deaktivieren Sie das Kontrollkästchen "Data-Komponenten nur anzeigen", um alle Objekte zu anzuzeigen, die die BLL-Klassen (zusammen mit anderen Klassen in das typisierte DataSet vorhandenen DataTables DataRows und So weiter) enthalten soll.

Wählen Sie aus dem ersten Bildschirm die `ProductsBLL` -Klasse aus der Dropdown Liste, und klicken Sie auf Weiter.


[![Geben Sie das Objekt zu verwenden, mit dem ObjectDataSource-Steuerelement](displaying-data-with-the-objectdatasource-vb/_static/image9.png)](displaying-data-with-the-objectdatasource-vb/_static/image8.png)

**Abbildung 4**: Geben Sie das Objekt zu verwenden, mit dem ObjectDataSource-Steuerelement ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-data-with-the-objectdatasource-vb/_static/image10.png))


Der nächste Bildschirm des Assistenten fordert Sie auf auswählen, welche Methode dem ObjectDataSource-Steuerelement aufgerufen werden soll. Die Dropdownlisten der Methoden, die Daten in das Objekt aus dem vorherigen Bildschirm ausgewählt zurückzugeben. Hier sehen wir `GetProductByProductID`, `GetProducts`, `GetProductsByCategoryID`, und `GetProductsBySupplierID`. Wählen Sie die `GetProducts` Methode aus der Dropdown-Liste und klicken Sie auf Fertig stellen (Wenn Sie hinzugefügt haben die `DataObjectMethodAttribute` zu der `ProductBLL`Methoden wie gezeigt im vorherigen Tutorial, diese Option werden standardmäßig ausgewählt).


[![Wählen Sie die Methode zum Zurückgeben von Daten aus der Registerkarte "SELECT"](displaying-data-with-the-objectdatasource-vb/_static/image12.png)](displaying-data-with-the-objectdatasource-vb/_static/image11.png)

**Abbildung 5**: Wählen Sie die Methode für das Zurückgeben von Daten aus der Registerkarte "auswählen" ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-data-with-the-objectdatasource-vb/_static/image13.png))


## <a name="configure-the-objectdatasource-manually"></a>Konfigurieren Sie manuell dem ObjectDataSource-Steuerelement

Das "ObjectDataSource" Konfigurieren von Datenquellen-Assistent bietet eine schnelle Möglichkeit zum Angeben des Objekts verwendeten und zuzuordnen, welche Methoden des Objekts aufgerufen werden. Sie können jedoch dem ObjectDataSource-Steuerelement über seine Eigenschaften, die entweder über das Eigenschaftenfenster oder direkt im deklarativen Markup konfigurieren. Legen Sie einfach die `TypeName` Eigenschaft in den Typ des zugrunde liegenden Objekts, das verwendet werden, und die `SelectMethod` auf die Methode, die beim Abrufen von Daten aufgerufen werden soll.


[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample1.aspx)]

Auch nach Wunsch der Konfigurieren von Datenquellen-Assistent gibt es möglicherweise gelegentlich müssen Sie manuell konfigurieren, dem ObjectDataSource-Steuerelement, wie der Assistent nur Entwickler erstellten Klassen listet. Wenn es sich bei dem ObjectDataSource-Steuerelement zu einer Klasse in .NET Framework, z. B. die Bindung erfolgen soll die [Mitgliedschaftsklasse](https://msdn.microsoft.com/library/system.web.security.membership.aspx), um auf Benutzerkontoinformationen zuzugreifen oder die [Directory-Klasse](https://msdn.microsoft.com/library/system.io.directory.aspx) mit Dateisysteminformationen arbeiten Sie müssen dem ObjectDataSource-Steuerelements Eigenschaften manuell festlegen.

## <a name="step-2-adding-a-data-web-control-and-binding-it-to-the-objectdatasource"></a>Schritt 2: Hinzufügen eines Daten-Websteuerelements und binden es an dem ObjectDataSource-Steuerelement

Nach dem ObjectDataSource-Steuerelement der Seite hinzugefügt und konfiguriert wurde, sind wir bereit, um die Seite zum Anzeigen der Daten, die von dem ObjectDataSource-Steuerelement zurückgegebenen Daten Websteuerelemente hinzuzufügen `Select` Methode. Daten-Websteuerelement können an ein ObjectDataSource-Steuerelement gebunden werden. Sehen wir uns, Anzeigen von Daten mit dem ObjectDataSource-Steuerelement in ein GridView, DetailsView und FormView-Steuerelement.

## <a name="binding-a-gridview-to-the-objectdatasource"></a>Binden einer GridView-Ansicht auf dem ObjectDataSource-Steuerelement

Hinzufügen einer GridView-Steuerelement aus der Toolbox an `SimpleDisplay.aspx`Entwurfsoberfläche. Wählen Sie aus den GridView Smarttag das ObjectDataSource-Steuerelement, die in Schritt 1 hinzugefügt wurde. Dadurch wird eine BoundField automatisch erstellt, in den GridView-Ansicht für jede Eigenschaft, die von den Daten zurückgegeben werden, von dem ObjectDataSource-Steuerelement `Select` Methode (nämlich die Eigenschaften, durch die DataTable Produkte definiert).


[![Die Seite einer GridView-Ansicht hinzugefügt wurde und an dem ObjectDataSource-Steuerelement gebunden](displaying-data-with-the-objectdatasource-vb/_static/image15.png)](displaying-data-with-the-objectdatasource-vb/_static/image14.png)

**Abbildung 6**: ein GridView wurde auf der Seite und dem ObjectDataSource-Steuerelement gebunden ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-data-with-the-objectdatasource-vb/_static/image16.png))


Sie können dann anpassen, neu anordnen oder entfernen den GridView BoundFields, indem Sie auf die Option "Spalten bearbeiten" das Smarttag.


[![Verwalten von GridView BoundFields über das Dialogfeld "Spalten bearbeiten"](displaying-data-with-the-objectdatasource-vb/_static/image18.png)](displaying-data-with-the-objectdatasource-vb/_static/image17.png)

**Abbildung 7**: Verwalten der GridView BoundFields durch das Bearbeiten Spalten (Dialogfeld) ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-data-with-the-objectdatasource-vb/_static/image19.png))


So ändern Sie den GridView BoundFields, entfernen in Ruhe die `ProductID`, `SupplierID`, `CategoryID`, `QuantityPerUnit`, `UnitsInStock`, `UnitsOnOrder`, und `ReorderLevel` BoundFields. Klicken Sie einfach wählen Sie die BoundField aus der Liste in der unteren linken Ecke, und klicken Sie auf die Schaltfläche "löschen" (das rote X), um sie zu entfernen. Als Nächstes ordnen die BoundFields, damit die `CategoryName` und `SupplierName` BoundFields vorausgehen der `UnitPrice` BoundField, indem diese BoundFields und dann auf den Pfeil nach oben. Legen Sie die `HeaderText` Eigenschaften auf den verbleibenden BoundFields `Products`, `Category`, `Supplier`, und `Price`bzw. Als Nächstes muss die `Price` BoundField als Währung formatiert, durch Festlegen der BoundField des `HtmlEncode` Eigenschaft auf "false" und die zugehörige `DataFormatString` Eigenschaft `{0:c}`. Abschließend horizontal ausrichten der `Price` auf der rechten Seite und die `Discontinued` Kontrollkästchen in der Mitte über den `ItemStyle` / `HorizontalAlign` Eigenschaft.


[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample2.aspx)]


[![GridView BoundFields angepasst haben, wurden](displaying-data-with-the-objectdatasource-vb/_static/image21.png)](displaying-data-with-the-objectdatasource-vb/_static/image20.png)

**Abbildung 8**: der GridView BoundFields angepasst worden ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-data-with-the-objectdatasource-vb/_static/image22.png))


## <a name="using-themes-for-a-consistent-look"></a>Verwenden von Designs für ein einheitliches Aussehen

In diesen Tutorials bemühen uns, um alle stileinstellungen für die Steuerungsebene zu entfernen, die stattdessen mithilfe von cascading Stylesheets in einer externen Datei nach Möglichkeit definiert. Die `Styles.css` -Datei enthält `DataWebControlStyle`, `HeaderStyle`, `RowStyle`, und `AlternatingRowStyle` Web-Steuerelemente, die in diesen Tutorials verwendeten CSS-Klassen, die bestimmt, die Darstellung der Daten verwendet werden soll. Um dies zu erreichen, legen wir konnten des GridView `CssClass` Eigenschaft `DataWebControlStyle`, und die zugehörige `HeaderStyle`, `RowStyle`, und `AlternatingRowStyle` Eigenschaften `CssClass` Eigenschaften entsprechend.

Wenn wir diese festlegen `CssClass` Eigenschaften in das Websteuerelement, wir denken Sie daran, diese Eigenschaftswerte für jedes einzelnen Daten explizit festlegen müssen, Webserver-Steuerelement hinzugefügt, die Lernprogramme. Ein besser verwaltbare Ansatz besteht darin, die CSS-bezogene Eigenschaften für die GridView, DetailsView definieren und FormView steuert die Verwendung eines Designs. Ein Design ist eine Auflistung von eigenschafteneinstellungen Steuerungsebene, Bilder und CSS-Klassen, die zu Seiten, die auf einem Standort, um eine allgemeine Aussehen und Verhalten erzwingen angewendet werden können.

Unser Design keine Bilder oder CSS-Dateien enthalten (lassen wir das Stylesheet `Styles.css` als-ist, im Stammordner der Webanwendung definiert), aber enthält zwei Designs. Einem Design handelt es sich um eine Datei, die Standardeigenschaften für ein Websteuerelement definiert. Insbesondere haben wir dann eine Skin-Datei für die GridView und DetailsView-Steuerelemente, der angibt, der Standardwert `CssClass`-bezogene Eigenschaften.

Starten Sie durch das Hinzufügen einer neuen Skindatei zu Ihrem Projekt mit dem Namen `GridView.skin` indem mit der rechten Maustaste auf den Projektnamen im Projektmappen-Explorer und neues Element hinzufügen.


[![Fügen Sie die Skindatei eine für GridView.skin](displaying-data-with-the-objectdatasource-vb/_static/image24.png)](displaying-data-with-the-objectdatasource-vb/_static/image23.png)

**Abbildung 9**: Hinzufügen einer Skin-Datei namens `GridView.skin` ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-data-with-the-objectdatasource-vb/_static/image25.png))


Skin-Dateien müssen in ein Design befinden die befinden sich in der `App_Themes` Ordner. Da wir noch nicht über einen solchen Ordner verfügen, bietet Visual Studio Bitte um einen für uns erstellen, wenn Sie eine Skin für unseren ersten hinzufügen. Klicken Sie auf Ja, zum Erstellen der `App_Theme` Ordner, und platzieren Sie die neue `GridView.skin` -Datei.


[![Lassen Sie Visual Studio den Ordner App_Theme erstellen](displaying-data-with-the-objectdatasource-vb/_static/image27.png)](displaying-data-with-the-objectdatasource-vb/_static/image26.png)

**Abbildung 10**: lassen Sie Visual Studio erstellt die `App_Theme` Ordner ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-data-with-the-objectdatasource-vb/_static/image28.png))


Hiermit wird ein neues Design in der `App_Themes` Ordner mit dem Namen "GridView", und die Designdatei `GridView.skin`.


![Das GridView-Design wurde hinzugefügt, zu dem Ordner App_Theme](displaying-data-with-the-objectdatasource-vb/_static/image29.png)

**Abbildung 11**: der GridView-Design wurde wurde hinzugefügt, um die `App_Theme` Ordner


Benennen Sie das GridView-Design in DataWebControls (mit der rechten Maustaste auf den Ordner "GridView" in der `App_Theme` Ordner und wählen Sie umbenennen). Geben Sie anschließend das folgende Markup in der `GridView.skin` Datei:


[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample3.aspx)]

Definiert die Standardeigenschaften für die `CssClass`-bezogenen Eigenschaften für alle GridView auf jeder Seite, die das Design DataWebControls verwendet. Fügen Sie ein anderes Design für DetailsView, eine Web-Steuerelement, das wir in Kürze verwenden von Daten an. Fügen Sie ein neues Design am DataWebControls Design, mit dem Namen `DetailsView.skin` und fügen Sie das folgende Markup hinzu:


[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample4.aspx)]

Im letzte Schritt werden mit unserer Design definiert das Design auf unserer Seite ASP.NET anwenden. Ein Design kann einmal pro Seite von Seite oder für alle Seiten auf einer Website angewendet werden. Wir verwenden dieses Design für alle Seiten der Website ein. Um dies zu erreichen, fügen Sie das folgende Markup zu `Web.config`des `<system.web>` Abschnitt:


[!code-xml[Main](displaying-data-with-the-objectdatasource-vb/samples/sample5.xml)]

Das ist alles schon! Die `styleSheetTheme` Einstellung gibt an, dass die im Design angegebenen Eigenschaften sollten *nicht* überschreiben die Eigenschaften, die auf der Steuerelementebene angegeben. Verwenden, um anzugeben, dass die designeinstellungen Einstellungen neu erstellt werden soll, die `theme` -Attribut anstelle von `styleSheetTheme`; leider designeinstellungen werden nicht in der Entwurfsansicht für Visual Studio angezeigt. Finden Sie unter [ASP.NET-Designs und Skins Overview](https://msdn.microsoft.com/library/ykzx33wh.aspx) und [serverseitige Stile mithilfe von Designs](https://quickstarts.asp.net/quickstartv20/aspnet/doc/themes/stylesheettheme.aspx) zu Designs und Skins; Weitere Informationen finden Sie unter [so wird's gemacht: Anwenden von ASP.NET-Designs](https://msdn.microsoft.com/library/0yy5hxdk%28VS.80%29.aspx) Weitere Informationen zu Konfigurieren eine Seite, um ein Design zu verwenden.


[![Die GridView zeigt die Name, Kategorie, Lieferanten, Preis und nicht mehr unterstützte Informationen des Produkts](displaying-data-with-the-objectdatasource-vb/_static/image31.png)](displaying-data-with-the-objectdatasource-vb/_static/image30.png)

**Abbildung 12**: die GridView zeigt die Name, Kategorie, Lieferanten, Preis und nicht mehr unterstützte Informationen des Produkts ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-data-with-the-objectdatasource-vb/_static/image32.png))


## <a name="displaying-one-record-at-a-time-in-the-detailsview"></a>Anzeigen von einem Datensatz gleichzeitig in DetailsView

Das GridView zeigt eine Zeile für jeden Datensatz zurückgegeben wird, mit dem Datenquellen-Steuerelement an dem er gebunden ist. Es gibt jedoch Situationen, wenn wir als einzige oder nur ein Datensatz gleichzeitig anzeigen möchten. Die [DetailsView-Steuerelement](https://msdn.microsoft.com/library/s3w1w7t4.aspx) bietet diese Funktion, die als HTML rendern `<table>` mit zwei Spalten und eine Zeile für jede Spalte oder Eigenschaft, die an das Steuerelement gebunden. Sie können als einer GridView-Ansicht mit einem einzelnen Datensatz um 90 Grad gedreht DetailsView vorstellen.

Starten, indem einem DetailsView-Steuerelement hinzufügen *oben* GridView in `SimpleDisplay.aspx`. Als Nächstes binden Sie es an der gleichen ObjectDataSource-Steuerelement wie GridView. Wie Sie mit der GridView, DetailsView für jede Eigenschaft im Objekt zurückgegeben, von dem ObjectDataSource-Steuerelement eine BoundField hinzugefügt werden `Select` Methode. Der einzige besteht Unterschied darin, dass DetailsViews BoundFields horizontal statt vertikal angeordnet werden.


[![Die Seite ein DetailsView hinzu, und ihn mit dem ObjectDataSource-Steuerelement](displaying-data-with-the-objectdatasource-vb/_static/image34.png)](displaying-data-with-the-objectdatasource-vb/_static/image33.png)

**Abbildung 13**: die Seite ein DetailsView hinzu, und ihn mit dem ObjectDataSource-Steuerelement ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-data-with-the-objectdatasource-vb/_static/image35.png))


Wie GridView kann DetailsViews BoundFields weiter optimiert werden, um eine individuellere Anzeige der von dem ObjectDataSource-Steuerelement zurückgegebenen Daten zu ermöglichen. Abbildung 14 zeigt DetailsView nach dessen BoundFields und `CssClass` Eigenschaften so konfiguriert wurden, um seine Darstellung wie im Beispiel GridView zu machen.


[![Die DetailsView zeigt einen einzelnen Datensatz](displaying-data-with-the-objectdatasource-vb/_static/image37.png)](displaying-data-with-the-objectdatasource-vb/_static/image36.png)

**Abbildung 14**: DetailsView zeigt einen einzelnen Datensatz ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-data-with-the-objectdatasource-vb/_static/image38.png))


Beachten Sie, dass die DetailsView nur den ersten Datensatz, der von der Datenquelle zurückgegeben zeigt. Wir müssen die Paging für DetailsView aktivieren, damit der Benutzer schrittweise Durchlaufen aller Datensätze, einzeln nacheinander, kann. Zu diesem Zweck zu Visual Studio zurück, und aktivieren Sie das Kontrollkästchen Paging aktivieren, in DetailsViews-Smarttag.


[![Aktivieren von Paging im DetailsView-Steuerelement](displaying-data-with-the-objectdatasource-vb/_static/image40.png)](displaying-data-with-the-objectdatasource-vb/_static/image39.png)

**Abbildung 15**: im DetailsView-Steuerelement Paging aktivieren ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-data-with-the-objectdatasource-vb/_static/image41.png))


[![Mit der Auslagerung ist aktiviert, können DetailsView-den Benutzer eines der Produkte anzeigen](displaying-data-with-the-objectdatasource-vb/_static/image43.png)](displaying-data-with-the-objectdatasource-vb/_static/image42.png)

**Abbildung 16**: mit Paging aktiviert, DetailsView ermöglicht dem Benutzer eines der Produkte anzeigen ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-data-with-the-objectdatasource-vb/_static/image44.png))


Wir sprechen Weitere Informationen zum paging in zukünftigen Lernprogrammen.

## <a name="a-more-flexible-layout-for-showing-one-record-at-a-time"></a>Ein flexibleres Layout für die Anzeige von einem Datensatz zu einem Zeitpunkt

Die DetailsView ist recht in der Anzeige der einzelnen zurückgegebenen Datensatz von dem ObjectDataSource-Steuerelement fest. Wir könnten eine flexiblere Ansicht der Daten. Z. B. nicht etwa den Namen des Produkts, Kategorie, Lieferanten, Preis und nicht mehr unterstützte Informationen in einer separaten Zeile an, wir möglicherweise anzeigen möchten den Produktnamen und Preis einer `<h4>` Überschrift mit der Kategorie und Lieferant angezeigt werden unter dem Namen und den Preis in einen kleineren Schriftgrad. Und wir können es nicht wichtig, um die Eigenschaftennamen (Produkt, Kategorie und So weiter) neben den Werten anzuzeigen.

Die [FormView-Steuerelement](https://msdn.microsoft.com/library/fyf1dk77.aspx) bietet eine solche Anpassung. Statt Felder (z. B. GridView und DetailsView ausgeführt werden), verwendet die FormView-Vorlagen, die eine Mischung aus Websteuerelemente, statisches HTML an, zu ermöglichen und [Databinding-Syntax](http://www.15seconds.com/issue/040630.htm). Wenn Sie mit dem Repeater-Steuerelement von ASP.NET auskennen 1.x, können Sie von FormView vorstellen, als der Repeater für die Anzeige eines einzelnen Datensatzes.

Fügen Sie einem FormView-Steuerelement, um die `SimpleDisplay.aspx` Seite der Entwurfsoberfläche. Das FormView-Steuerelement zeigt zu Beginn als einen grauen Block, in der uns darüber informiert werden, dass ich benötige, zumindest des Steuerelements `ItemTemplate`.


[![Die FormView-Steuerelement muss enthalten eine Elementvorlage](displaying-data-with-the-objectdatasource-vb/_static/image46.png)](displaying-data-with-the-objectdatasource-vb/_static/image45.png)

**Abbildung 17**: das FormView Include muss ein `ItemTemplate` ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-data-with-the-objectdatasource-vb/_static/image47.png))


Sie können das FormView-Steuerelement binden, direkt an ein Datenquellen-Steuerelement über das FormView Smarttags, der Erstellen eines standardmäßigen wird `ItemTemplate` automatisch (zusammen mit einer `EditItemTemplate` und `InsertItemTemplate`, wenn des Steuerelements ObjectDatatSource `InsertMethod` und `UpdateMethod` Eigenschaften festgelegt werden). Allerdings in diesem Beispiel wir die Daten an das FormView-Steuerelement binden und geben Sie die `ItemTemplate` manuell. Starten, indem Sie der FormView `DataSourceID` Eigenschaft, um die `ID` des ObjectDataSource-Steuerelement, `ObjectDataSource1`. Erstellen Sie als Nächstes die `ItemTemplate` , damit die Namen und den Preis in des Produkts angezeigt ein `<h4>` -Element und die Kategorie und Lieferanten, in einen kleineren Schriftgrad.


[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample6.aspx)]


[![Das erste Produkt (Chai) wird in einem benutzerdefinierten Format angezeigt.](displaying-data-with-the-objectdatasource-vb/_static/image49.png)](displaying-data-with-the-objectdatasource-vb/_static/image48.png)

**Abbildung 18**: das erste Produkt (Chai) wird angezeigt, in einem benutzerdefinierten Format ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-data-with-the-objectdatasource-vb/_static/image50.png))


Die `<%# Eval(propertyName) %>` ist die Databinding-Syntax. Die `Eval` -Methode gibt den Wert der angegebenen Eigenschaft für das aktuelle Objekt, das an das FormView-Steuerelement gebunden wird. Lesen Sie Artikel von Alex Homer des [vereinfacht und erweiterte Bindung Datensyntax in ASP.NET 2.0](http://www.15seconds.com/issue/040630.htm) um mehr über die vor-und Nachteile der Datenbindung.

Wie DetailsView zeigt das FormView-Steuerelement nur die ersten zurückgegebenen Datensatz von dem ObjectDataSource-Steuerelement. Paging in das FormView-Steuerelement können Besucher, die Produkte eine seitenweise durchlaufen können.

## <a name="summary"></a>Zusammenfassung

Zugreifen auf und Anzeigen von Daten aus einer Geschäftslogikebene können erreicht werden, ohne eine einzige Zeile Code Dank dem ObjectDataSource-Steuerelement von ASP.NET 2.0 schreiben zu müssen. Die ObjectDataSource ruft eine angegebene Methode einer Klasse und gibt die Ergebnisse zurück. Diese Ergebnisse können in einer Web-Steuerelement angezeigt werden, die auf dem ObjectDataSource-Steuerelement gebunden ist. In diesem Tutorial haben Sie die GridView, DetailsView oder FormView-Steuerelemente auf dem ObjectDataSource-Steuerelement binden.

Bisher haben nur erfahren, wie Sie dem ObjectDataSource-Steuerelement zu verwenden, um eine Methode ohne Parameter aufrufen, aber was geschieht, wenn wir eine Methode aufzurufen, die erwartet möchten Eingabeparameter aufweisen, z. B. die `ProductBLL` Klasse `GetProductsByCategoryID(categoryID)`? Konfigurieren wir dem ObjectDataSource-Steuerelement um die Werte für diese Parameter anzugeben, um eine Methode aufrufen, die einen oder mehrere Parameter erwartet. Erfahren Sie, wie Sie hierzu in unserer [nächsten Tutorial](declarative-parameters-vb.md).

Viel Spaß beim Programmieren!

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Tutorial erläutert finden Sie in den folgenden Ressourcen:

- [Erstellen Sie eigene Datenquellen-Steuerelemente](https://msdn.microsoft.com/library/ms364049.aspx)
- [GridView-Beispiele für ASP.NET 2.0](https://msdn.microsoft.com/library/aa479339.aspx)
- [Vereinfacht und die erweiterte Syntax-Datenbindung in ASP.NET 2.0](http://www.15seconds.com/issue/040630.htm)
- [Designs in ASP.NET 2.0](http://www.odetocode.com/Articles/423.aspx)
- [Serverseitige-Stilen mithilfe von Designs](https://quickstarts.asp.net/quickstartv20/aspnet/doc/themes/stylesheettheme.aspx)
- [Gewusst wie: Programmgesteuertes Anwenden von ASP.NET-Designs](https://msdn.microsoft.com/library/tx35bd89.aspx)

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial ist Hilton Giesenow. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](programmatically-setting-the-objectdatasource-s-parameter-values-cs.md)
> [Weiter](declarative-parameters-vb.md)
