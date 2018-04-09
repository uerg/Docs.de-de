---
uid: web-forms/overview/data-access/basic-reporting/displaying-data-with-the-objectdatasource-cs
title: Anzeigen von Daten mit der ObjectDataSource (c#) | Microsoft Docs
author: rick-anderson
description: In diesem Lernprogramm prüft das ObjectDataSource-Steuerelement, das mit diesem Steuerelement, das können Sie aus den im vorherigen Lernprogramm ohne Havi erstellt BLL abgerufene Daten binden...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: af882aef-56f5-4e9a-8f95-3977fde20e74
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/displaying-data-with-the-objectdatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: 6fbad218f3e02bde13b8788bf6f1eefac0c4990c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="displaying-data-with-the-objectdatasource-c"></a>Anzeigen von Daten mit der ObjectDataSource (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunterladen](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_4_CS.exe) oder [PDF herunterladen](displaying-data-with-the-objectdatasource-cs/_static/datatutorial04cs1.pdf)

> In diesem Lernprogramm prüft das ObjectDataSource-Steuerelement, das mit diesem Steuerelement, das können Sie aus den im vorherigen Lernprogramm erstellt werden, ohne eine Codezeile schreiben zu müssen BLL abgerufene Daten binden.


## <a name="introduction"></a>Einführung

Mit unseren Anwendung Architektur und Website Seitenlayout abgeschlossen können wir untersuchen, wie Sie eine Vielzahl von häufig und reporting-datenbezogene Aufgaben ausführen. In den vorherigen Lernprogrammen haben wir gesehen, wie Sie Daten aus der DAL und BLL mit einem Daten-Websteuerelement auf einer ASP.NET-Seite programmgesteuert zu binden. Diese Syntax, die der Daten Websteuerelement zuweisen `DataSource` Eigenschaft, um die Daten anzeigen und anschließend durch Aufrufen des Steuerelements `DataBind()` Methode das Muster in 1.x ASP.NET-Anwendungen verwendet wurde, und können weiterhin in der 2.0-Anwendungen verwendet werden. ASP.NET 2.0 Datenquellensteuerelemente bieten jedoch eine deklarative Methode zum Arbeiten mit Daten. Erstellt mit diesen Steuerelementen können Sie aus der BLL abgerufene Daten binden, der [vorherigen Lernprogramm](../introduction/creating-a-business-logic-layer-cs.md) ohne eine Codezeile schreiben zu müssen.

ASP.NET 2.0 enthält fünf integrierte Datenquellensteuerelemente [SqlDataSource](https://msdn.microsoft.com/library/dz12d98w(vs.80).aspx), [AccessDataSource](https://msdn.microsoft.com/library/8e5545e1.aspx), [ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx), [XmlDataSource](https://msdn.microsoft.com/library/e8d8587a(en-US,VS.80).aspx), und [SiteMapDataSource](https://msdn.microsoft.com/library/5ex9t96x(en-US,VS.80).aspx) , obwohl Sie eine eigene erstellen können [benutzerdefinierte Datenquellensteuerelementen](https://msdn.microsoft.com/library/default.asp?url=/library/dnvs05/html/DataSourceCon1.asp), sofern erforderlich. Da wir eine Architektur für unser Tutorial Anwendung entwickelt haben, müssen die ObjectDataSource für unsere BLL-Klassen verwendet werden.


![ASP.NET 2.0 enthält fünf integrierte Datenquellensteuerelemente](displaying-data-with-the-objectdatasource-cs/_static/image1.png)

**Abbildung 1**: ASP.NET 2.0 enthält fünf integrierte Datenquellensteuerelemente


Das ObjectDataSource fungiert als Proxy für die Arbeit mit einem anderen Objekt. So konfigurieren Sie das ObjectDataSource wir geben zugrunde liegende Objekt und seine Methoden wie die ObjectDataSource zugeordnet `Select`, `Insert`, `Update`, und `Delete` Methoden. Sobald diese zugrunde liegenden Objekts angegeben wurde und seine Methoden, die das ObjectDataSource zugeordnet, können wir das ObjectDataSource klicken Sie dann mit einem Daten-Websteuerelement binden. Im Lieferumfang von ASP.NET sind viele Daten Web Controls, einschließlich der GridView, DetailsView RadioButtonList und DropDownList o. ä. Während des Seitenlebenszyklus möglicherweise müssen die Daten Websteuerelement auf die Daten, die es gebunden ist, die ihn durch Aufrufen seiner ObjectDataSource erreichen kann `Select` Methode; Wenn die Daten Websteuerelement einfügen unterstützen, aktualisieren oder löschen, aufgerufen werden kann seine Der ObjectDataSource `Insert`, `Update`, oder `Delete` Methoden. Diese Aufrufe werden dann von der ObjectDataSource an den entsprechenden zugrunde liegende Objekt Methoden weitergeleitet, wie das folgende Diagramm veranschaulicht.


[![Das ObjectDataSource fungiert als Proxy](displaying-data-with-the-objectdatasource-cs/_static/image3.png)](displaying-data-with-the-objectdatasource-cs/_static/image2.png)

**Abbildung 2**: das ObjectDataSource fungiert als Proxy ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-data-with-the-objectdatasource-cs/_static/image4.png))


Während der ObjectDataSource kann, zum Aufrufen von Methoden zum Einfügen verwendet werden, aktualisieren oder Löschen von Daten, konzentrieren wir uns nur auf die Rückgabe von Daten; zukünftige Lernprogramme erforschen mithilfe der ObjectDataSource und Daten Websteuerelemente, die Daten ändern.

## <a name="step-1-adding-and-configuring-the-objectdatasource-control"></a>Schritt 1: Hinzufügen und konfigurieren das ObjectDataSource-Steuerelement

Öffnen Sie zunächst die `SimpleDisplay.aspx` auf der Seite der `BasicReporting` Ordner, wechseln Sie zur Entwurfsansicht, und ziehen Sie dann einem ObjectDataSource-Steuerelement aus der Toolbox auf die Entwurfsoberfläche der Seite. Das ObjectDataSource wird als eine "graues Feld" auf der Entwurfsoberfläche angezeigt, da es nicht Markup erstellt; einfach auf Daten zugreift, durch Aufrufen einer Methode aus einem angegebenen Objekt. Die von einem ObjectDataSource zurückgegebenen Daten können durch ein Webserver-Steuerelement, z. B. die GridView, DetailsView FormView und So weiter angezeigt werden.

> [!NOTE]
> Alternativ können Sie Sie ggf. zuerst die Daten Websteuerelement zur Seite und wählen Sie dann aus der Smarttag der &lt;neue Datenquelle&gt; Option aus der Dropdown-Liste.


Zum Angeben der ObjectDataSource zugrunde liegende Objekt und die Methoden dieses Objekts wie das ObjectDataSource zugeordnet, klicken Sie auf die Datenquelle konfigurieren-Verknüpfung zwischen der ObjectDataSource Smarttag.


[![Klicken Sie auf die Datenquellenlink aus dem Smarttag konfigurieren](displaying-data-with-the-objectdatasource-cs/_static/image6.png)](displaying-data-with-the-objectdatasource-cs/_static/image5.png)

**Abbildung 3**: Klicken Sie auf die konfigurieren Datenquellenlink aus dem Smarttag ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-data-with-the-objectdatasource-cs/_static/image7.png))


Daraufhin wird der Assistent zum Konfigurieren von Datenquellen. Zunächst müssen wir das Objekt angeben, die das ObjectDataSource zur Bearbeitung ist. Wenn das Kontrollkästchen "Nur Datenkomponenten anzeigen" aktiviert ist, führt die Dropdown-Liste auf diesem Bildschirm nur die Objekte, die mit ergänzt wurden, haben die `DataObject` Attribut. Derzeit enthält unserer Liste der TableAdapters in das typisierte DataSet und die BLL-Klassen, die wir im vorherigen Lernprogramm erstellt haben. Wenn Sie vergessen haben, fügen die `DataObject` Attribut für die Business Logic Layer-Klassen Sie sehen in dieser Liste. In diesem Fall, deaktivieren Sie das Kontrollkästchen "Nur Datenkomponenten anzeigen", um alle Objekte anzuzeigen, die Klassen BLL (zusammen mit anderen Klassen in das typisierte DataSet die DataTables, DataRows usw.) enthalten soll.

Wählen Sie diese ersten Bildschirm die `ProductsBLL` Klasse aus der Dropdown-Liste, und klicken Sie auf Weiter.


[![Geben Sie den Objektnamen zu verwenden, mit dem ObjectDataSource-Steuerelement](displaying-data-with-the-objectdatasource-cs/_static/image9.png)](displaying-data-with-the-objectdatasource-cs/_static/image8.png)

**Abbildung 4**: Geben Sie den Objektnamen zu verwenden, mit dem ObjectDataSource-Steuerelement ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-data-with-the-objectdatasource-cs/_static/image10.png))


Der nächste Bildschirm des Assistenten aufgefordert, mit welcher Methode wählen Sie das ObjectDataSource aufgerufen werden sollen. Die Dropdownlisten Methoden, die Daten in der von dem vorherigen Bildschirm ausgewähltes Objekt zurückzugeben. Hier sehen wir `GetProductByProductID`, `GetProducts`, `GetProductsByCategoryID`, und `GetProductsBySupplierID`. Wählen Sie die `GetProducts` Methode aus der Dropdown-Liste und klicken Sie auf Fertig stellen (Wenn Sie hinzugefügt haben die `DataObjectMethodAttribute` auf die `ProductBLL`des Methoden entsprechend in das vorherige Lernprogramm, diese Option werden standardmäßig ausgewählt).


[![Wählen Sie die Methode zum Zurückgeben von Daten aus der Registerkarte "SELECT"](displaying-data-with-the-objectdatasource-cs/_static/image12.png)](displaying-data-with-the-objectdatasource-cs/_static/image11.png)

**Abbildung 5**: Wählen Sie die Methode für die Rückgabe von Daten aus der Registerkarte "auswählen" ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-data-with-the-objectdatasource-cs/_static/image13.png))


## <a name="configure-the-objectdatasource-manually"></a>Das ObjectDataSource manuell konfigurieren

Das ObjectDataSource-Konfigurieren von Datenquellen-Assistent bietet eine schnelle Möglichkeit zum Angeben des Objekts verwendeten und den zuweisen, welche Methoden des Objekts aufgerufen werden. Sie können jedoch das ObjectDataSource über seine Eigenschaften, die über das Fenster "Eigenschaften" oder direkt im deklarativen Markup konfigurieren. Legen Sie einfach die `TypeName` Eigenschaft in den Typ des zugrunde liegenden Objekts verwendet werden, und die `SelectMethod` auf die Methode, die beim Abrufen von Daten aufgerufen werden soll.


[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample1.aspx)]

Auch wenn der Assistent zum Konfigurieren von Datenquellen, die möglicherweise müssen Sie manchmal manuell konfigurieren, das ObjectDataSource gewünscht wie der Assistent nur Klassen Entwickler erstellt führt. Wenn Sie z. B. das ObjectDataSource auf eine Klasse in .NET Framework binden möchten die [Mitgliedschaftsklasse](https://msdn.microsoft.com/library/system.web.security.membership.aspx), um auf Benutzerkontoinformationen zuzugreifen oder die [Directory Klasse](https://msdn.microsoft.com/library/system.io.directory.aspx) zum Arbeiten mit Dateisysteminformationen Sie müssen das ObjectDataSource-Eigenschaften manuell festlegen.

## <a name="step-2-adding-a-data-web-control-and-binding-it-to-the-objectdatasource"></a>Schritt 2: Hinzufügen eines Daten-Web-Steuerelements, und binden es an das ObjectDataSource

Nachdem das ObjectDataSource zur Seite hinzugefügt und konfiguriert wurde, können wir bereit zum Hinzufügen von Daten Websteuerelemente auf der Seite zum Anzeigen der Daten, die das ObjectDataSource zurückgegebenes `Select` Methode. Alle Daten Websteuerelement können an einem ObjectDataSource gebunden werden. Betrachten Sie das ObjectDataSource-Daten in eine GridView, DetailsView und FormView anzeigen.

## <a name="binding-a-gridview-to-the-objectdatasource"></a>Binden eine GridView an das ObjectDataSource

Hinzufügen eines GridView-Steuerelements aus der Toolbox an `SimpleDisplay.aspx`der Entwurfsoberfläche angezeigt. Wählen Sie die GridView Smarttag das ObjectDataSource-Steuerelement, die in Schritt 1 hinzugefügt wurden. Dadurch wird ein BoundField automatisch erstellt, in der GridView für jede Eigenschaft, die die Daten aus der ObjectDataSource zurückgegebenes `Select` Methode (nämlich die Eigenschaften von Produkten DataTable definiert).


[![Eine GridView wurde auf der Seite hinzugefügt und an das ObjectDataSource gebunden](displaying-data-with-the-objectdatasource-cs/_static/image15.png)](displaying-data-with-the-objectdatasource-cs/_static/image14.png)

**Abbildung 6**: A GridView hinzugefügt wurde auf der Seite und an das ObjectDataSource gebunden ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-data-with-the-objectdatasource-cs/_static/image16.png))


Sie können dann anpassen, neu anordnen oder entfernen die GridView BoundFields, indem Sie auf die Option "Spalten bearbeiten" aus dem Smarttag.


[![Verwalten der GridView BoundFields über das Dialogfeld "Spalten bearbeiten"](displaying-data-with-the-objectdatasource-cs/_static/image18.png)](displaying-data-with-the-objectdatasource-cs/_static/image17.png)

**Abbildung 7**: Verwalten der GridView BoundFields über das Bearbeiten (Dialogfeld) ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-data-with-the-objectdatasource-cs/_static/image19.png))


So ändern Sie die GridView BoundFields, Entfernen von erkundet die `ProductID`, `SupplierID`, `CategoryID`, `QuantityPerUnit`, `UnitsInStock`, `UnitsOnOrder`, und `ReorderLevel` BoundFields. Einfach wählen Sie die BoundField aus der Liste in der unteren linken Ecke aus, und klicken Sie auf die Schaltfläche "löschen" (das rote X), um sie zu entfernen. Als Nächstes ordnen die BoundFields, damit die `CategoryName` und `SupplierName` BoundFields vorausgehen der `UnitPrice` BoundField, indem diese BoundFields und dann auf den Pfeil nach oben. Legen Sie die `HeaderText` Eigenschaften von den verbleibenden BoundFields auf `Products`, `Category`, `Supplier`, und `Price`bzw. Als Nächstes haben die `Price` BoundField als Währung formatiert, durch Festlegen der BoundField `HtmlEncode` Eigenschaft auf "false" und die zugehörige `DataFormatString` Eigenschaft `{0:c}`. Schließlich horizontal ausgerichtet. die `Price` rechts und die `Discontinued` Kontrollkästchen in der Mitte über die `ItemStyle` / `HorizontalAlign` Eigenschaft.


[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample2.aspx)]


[![Die GridView BoundFields angepasst haben, wurden](displaying-data-with-the-objectdatasource-cs/_static/image21.png)](displaying-data-with-the-objectdatasource-cs/_static/image20.png)

**Abbildung 8**: die GridView BoundFields geändert wurden ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-data-with-the-objectdatasource-cs/_static/image22.png))


## <a name="using-themes-for-a-consistent-look"></a>Verwenden von Designs für ein konsistentes Erscheinungsbild

Diese Lernprogramme So entfernen Sie alle Steuerungsebene formateinstellungen bemühen, stattdessen mithilfe von cascading Stylesheets in einer externen Datei nach Möglichkeit definiert. Die `Styles.css` -Datei enthält `DataWebControlStyle`, `HeaderStyle`, `RowStyle`, und `AlternatingRowStyle` CSS-Klassen, die verwendet werden soll, um die Darstellung der Daten zu diktieren Web-Steuerelemente in diesen Lernprogrammen verwendet. Um dies zu erreichen, legen wir konnten der GridView `CssClass` Eigenschaft `DataWebControlStyle`, und die zugehörige `HeaderStyle`, `RowStyle`, und `AlternatingRowStyle` Eigenschaften `CssClass` Eigenschaften entsprechend.

Wenn wir diese setzen `CssClass` Eigenschaften auf das Websteuerelement wir daran denken, diese Eigenschaftswerte für jede einzelnen Daten explizit festgelegt müssen Webserver-Steuerelement zu unseren Tutorials hinzugefügt. Ein einfacher verwaltbar Ansatz ist die standardmäßige CSS-bezogenen Eigenschaften für die GridView, DetailsView, definieren und FormView steuert die Verwendung eines Designs. Ein Design ist eine Auflistung von Steuerungsebene eigenschafteneinstellungen, Bilder und CSS-Klassen, die Seiten auf einer Website, eine gemeinsame Aussehen und Verhalten zu erzwingen angewendet werden können.

Unser Design wird nicht enthalten, weder Symbole oder CSS-Dateien (müssen wir das Stylesheet lassen `Styles.css` als-im Stammordner der Webanwendung definiert ist,), ist jedoch enthalten zwei Skins. Ein Design handelt es sich um eine Datei, die Standardeigenschaften für ein Websteuerelement definiert. Insbesondere lassen wir eine Skin-Datei für die Steuerelemente GridView und DetailsView, der angibt, des Standardwerts `CssClass`-bezogene Eigenschaften.

Starten Sie durch Hinzufügen einer neuen Skin-Datei dem Projekt mit dem Namen `GridView.skin` indem Sie mit der rechten Maustaste auf den Projektnamen im Projektmappen-Explorer und neues Element hinzufügen.


[![Fügen Sie eine Skindatei mit dem Namen GridView.skin](displaying-data-with-the-objectdatasource-cs/_static/image24.png)](displaying-data-with-the-objectdatasource-cs/_static/image23.png)

**Abbildung 9**: Hinzufügen einer Skin-Datei namens `GridView.skin` ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-data-with-the-objectdatasource-cs/_static/image25.png))


Skin-Dateien müssen in einem Design platziert werden die befinden sich in der `App_Themes` Ordner. Da wir noch solcher Ordner haben, bietet Visual Studio überprüfen eins für uns erstellt, beim Hinzufügen von unseren ersten Design. Klicken Sie auf Ja, um das Erstellen der `App_Theme` Ordner, und platzieren Sie die neue `GridView.skin` Datei vorhanden.


[![Damit Visual Studio den Ordner App_Theme erstellen](displaying-data-with-the-objectdatasource-cs/_static/image27.png)](displaying-data-with-the-objectdatasource-cs/_static/image26.png)

**Abbildung 10**: Erstellen von Visual Studio können die `App_Theme` Ordner ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-data-with-the-objectdatasource-cs/_static/image28.png))


Dies erstellt ein neues Design in der `App_Themes` Ordner mit dem Namen GridView mit der Designdatei `GridView.skin`.


![Das Design "GridView" wurde in den Ordner App_Theme hinzugefügt wurde](displaying-data-with-the-objectdatasource-cs/_static/image29.png)

**Abbildung 11**: die GridView-Design wurde hinzugefügt wurde die `App_Theme` Ordner


Benennen Sie das Design GridView in DataWebControls (mit der rechten Maustaste auf den Ordner "GridView" in der `App_Theme` Ordner, und wählen Sie umbenennen). Geben Sie anschließend das folgende Markup in der `GridView.skin` Datei:


[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample3.aspx)]

Definiert die Standardeigenschaften für die `CssClass`-bezogenen Eigenschaften für alle GridView in eine andere Seite, die das Design DataWebControls verwendet. Fügen Sie für die DetailsView ein Daten-Websteuerelement, die wir in Kürze verwenden wir eine andere Skin hinzu. Hinzufügen ein neues Designs zum DataWebControls Design mit dem Namen `DetailsView.skin` und fügen Sie das folgende Markup hinzu:


[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample4.aspx)]

Im letzte Schritt werden mit unserem definiertes Design das Design für unsere ASP.NET-Seite gelten. Ein Design kann auf Basis von Seite oder für alle Seiten in einer Website angewendet werden. Ermöglicht die Verwendung dieses Designs für alle Seiten der Website. Um dies zu erreichen, fügen Sie das folgende Markup zum `Web.config`des `<system.web>` Abschnitt:


[!code-xml[Main](displaying-data-with-the-objectdatasource-cs/samples/sample5.xml)]

Das ist alles vorhanden ist! Die `styleSheetTheme` Einstellung gibt an, dass die in das Design angegebenen Eigenschaften sollten *nicht* überschreiben die Eigenschaften auf der Steuerelementebene angegeben. Verwenden, um anzugeben, dass die designeinstellungen steuerelementeinstellungen neu erstellt werden soll, die `theme` -Attribut anstelle von `styleSheetTheme`; leider designeinstellungen angegeben, über die `theme` Attribut werden in der Entwurfsansicht von Visual Studio nicht angezeigt. Finden Sie unter [ASP.NET-Designs und Skins Overview](https://msdn.microsoft.com/library/ykzx33wh.aspx) und [serverseitige Stile mithilfe von Designs](https://quickstarts.asp.net/quickstartv20/aspnet/doc/themes/stylesheettheme.aspx) Designs und Skins; Weitere Informationen finden Sie unter [How To: Apply ASP.NET Themes](https://msdn.microsoft.com/library/0yy5hxdk(VS.80).aspx) Weitere Informationen zu Konfigurieren eine Seite, um das Design.


[![Die GridView zeigt an, des Produkts Name, Kategorie, Lieferanten, Preis und nicht mehr unterstützte Informationen](displaying-data-with-the-objectdatasource-cs/_static/image31.png)](displaying-data-with-the-objectdatasource-cs/_static/image30.png)

**Abbildung 12**: GridView zeigt an, des Produkts Name, Kategorie, Lieferanten, Preis und Informationen nicht mehr unterstützt ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-data-with-the-objectdatasource-cs/_static/image32.png))


## <a name="displaying-one-record-at-a-time-in-the-detailsview"></a>Anzeigen von einem Datensatz zu einem Zeitpunkt in der DetailsView

Die GridView zeigt eine Zeile für jeden Datensatz zurückgegeben, die für das Datenquellensteuerelement an dem es gebunden ist. Es gibt jedoch Situationen, wenn es möglicherweise als einzige oder nur ein Datensatz zu einem Zeitpunkt anzeigen möchten. Die [DetailsView-Steuerelement](https://msdn.microsoft.com/library/s3w1w7t4.aspx) bietet diese Funktion, die als HTML zu rendern `<table>` mit zwei Spalten und eine Zeile für jede Spalte oder Eigenschaft, die an das Steuerelement gebunden. Sie können als eine GridView mit einem einzelnen Datensatz um 90 Grad gedreht DetailsView vorstellen.

Starten Sie durch Hinzufügen eines Steuerelements DetailsView *oben* GridView in `SimpleDisplay.aspx`. Als Nächstes an der gleichen ObjectDataSource-Steuerelement als GridView binden. Wie mit GridView, DetailsView für jede Eigenschaft im Objekt zurückgegeben, die für das ObjectDataSource ein BoundField hinzugefügt werden `Select` Methode. Der einzige Unterschied ist, dass der DetailsView BoundFields horizontal statt vertikal angeordnet sind.


[![Die Seite eine DetailsView hinzu, und binden Sie es an das ObjectDataSource](displaying-data-with-the-objectdatasource-cs/_static/image34.png)](displaying-data-with-the-objectdatasource-cs/_static/image33.png)

**Abbildung 13**: die Seite eine DetailsView hinzu, und binden es an das ObjectDataSource ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-data-with-the-objectdatasource-cs/_static/image35.png))


Wie die GridView können die DetailsView BoundFields manipuliert werden, um einer stärker angepassten Anzeige der von der ObjectDataSource zurückgegebenen Daten bereitzustellen. Abbildung 14 zeigt, DetailsView nach seiner BoundFields und `CssClass` Eigenschaften ihrer Darstellung ähnelt der GridView-Beispiel stellen konfiguriert wurden.


[![Zeigt einen einzelnen Datensatz, DetailsView](displaying-data-with-the-objectdatasource-cs/_static/image37.png)](displaying-data-with-the-objectdatasource-cs/_static/image36.png)

**Abbildung 14**: Zeigt einen einzelnen Datensatz, DetailsView ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-data-with-the-objectdatasource-cs/_static/image38.png))


Beachten Sie, DetailsView nur den ersten Datensatz von der Datenquelle zurückgegeben zeigt. Aktivieren wir die Auslagerung für DetailsView, damit der Benutzer alle Datensätze, jeweils einzeln, durchlaufen kann. Zu diesem Zweck kehren Sie zu Visual Studio zurück, und aktivieren Sie das Kontrollkästchen Paging aktivieren, in der DetailsView Smarttag.


[![Aktivieren von Paging im DetailsView-Steuerelement](displaying-data-with-the-objectdatasource-cs/_static/image40.png)](displaying-data-with-the-objectdatasource-cs/_static/image39.png)

**Abbildung 15**: Paging aktivieren, DetailsView--Steuerelement ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-data-with-the-objectdatasource-cs/_static/image41.png))


[![Ermöglicht es mit aktiviertem Paging, DetailsView auf dem Benutzer eines der Produkte anzeigen](displaying-data-with-the-objectdatasource-cs/_static/image43.png)](displaying-data-with-the-objectdatasource-cs/_static/image42.png)

**Abbildung 16**: mit Paging aktiviert, DetailsView ermöglicht es dem Benutzer eines der Produkte anzeigen ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-data-with-the-objectdatasource-cs/_static/image44.png))


Weitere Informationen über das paging in zukünftigen Lernprogrammen erfahren.

## <a name="a-more-flexible-layout-for-showing-one-record-at-a-time"></a>Ein flexibleres Layout für das Anzeigen von einem Datensatz zu einem Zeitpunkt

DetailsView ist in der Anzeige der einzelnen zurückgegebenen Datensatz von der ObjectDataSource ziemlich fest. Sollten wir eine flexiblere Ansicht der Daten ein. Z. B. nicht etwa den Namen des Produkts, Kategorie, Lieferanten, Preis und nicht mehr unterstützte Informationen in einer eigenen Zeile an, wir möglicherweise anzeigen möchten den Produktnamen und Preis eine `<h4>` Überschrift, mit der angezeigten Informationen zur Produktkategorie und Lieferanten unter dem Namen und den Preis in einen kleineren Schriftgrad. Und wir möglicherweise keine Achten Sie darauf, den Eigenschaftennamen (Produkt, Kategorie usw.) neben den Werten anzeigen.

Die [FormView-Steuerelement](https://msdn.microsoft.com/library/fyf1dk77.aspx) bietet dieses Maß an Anpassung. Statt Felder (z. B. die GridView und DetailsView ausgeführt werden), verwendet die FormView-Vorlagen, die eine Mischung aus Web Controls, statische HTML zu ermöglichen und [Databinding-Syntax](http://www.15seconds.com/issue/040630.htm). Wenn Sie mit der Wiederholungsmodul-Steuerelement von ASP.NET vertraut sind 1.x, können Sie der FormView vorstellen, als die Repeater für mit einem einzelnen Datensatz.

Hinzufügen eines FormView-Steuerelements, um die `SimpleDisplay.aspx` Seite der Entwurfsoberfläche angezeigt. Die FormView zeigt Anfangs als einen grauen Block uns darüber informiert werden, dass mindestens des Steuerelements bereitstellen müssen `ItemTemplate`.


[![Die FormView muss enthalten ein ItemTemplate](displaying-data-with-the-objectdatasource-cs/_static/image46.png)](displaying-data-with-the-objectdatasource-cs/_static/image45.png)

**Abbildung 17**: die FormView Include muss ein `ItemTemplate` ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-data-with-the-objectdatasource-cs/_static/image47.png))


Sie können die FormView binden, direkt an ein Datenquellen-Steuerelement über die FormView Smarttag, die den Standardwert erstellt werden `ItemTemplate` automatisch (zusammen mit einer `EditItemTemplate` und `InsertItemTemplate`, wenn des ObjectDatatSource Steuerelements `InsertMethod` und `UpdateMethod` Eigenschaften festgelegt sind). Allerdings in diesem Beispiel nehmen wir Binden von Daten an die FormView und geben Sie die `ItemTemplate` manuell. Starten, indem Sie die FormView `DataSourceID` Eigenschaft, um die `ID` von das ObjectDataSource-Steuerelement `ObjectDataSource1`. Als Nächstes erstellen Sie die `ItemTemplate` , damit sie Namen und den Preis in des Produkts anzeigt ein `<h4>` Element und die Kategorie und Spediteur Namen unter, die in einen kleineren Schriftgrad.


[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample6.aspx)]


[![Das erste Produkt (Chai) wird in einem benutzerdefinierten Format angezeigt.](displaying-data-with-the-objectdatasource-cs/_static/image49.png)](displaying-data-with-the-objectdatasource-cs/_static/image48.png)

**Abbildung 18**: der erste Produkt (Chai) wird angezeigt, in einem benutzerdefinierten Format ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-data-with-the-objectdatasource-cs/_static/image50.png))


Die `<%# Eval(propertyName) %>` ist die Databinding-Syntax. Die `Eval` -Methode gibt den Wert der angegebenen Eigenschaft für das aktuelle Objekt, das an das Steuerelement FormView gebunden wird. Auschecken des Alex Homer Artikel [vereinfacht und erweitert Data Binding Syntax in ASP.NET 2.0](http://www.15seconds.com/issue/040630.htm) für Weitere Informationen für die Durchsuchung der Datenbindung.

Wie DetailsView, zeigt FormView nur die ersten zurückgegebenen Datensatz von der ObjectDataSource. Sie können Auslagerungsdatei FormView Besucher durchlaufen die Produkte eine zu einem Zeitpunkt zu aktivieren.

## <a name="summary"></a>Zusammenfassung

Zugreifen auf und Anzeigen von Daten aus einer Business Logic Layer können erreicht werden, ohne eine Codezeile Dank ObjectDataSource-Steuerelement von ASP.NET 2.0 Code schreiben zu müssen. Die ObjectDataSource ruft eine angegebene Methode einer Klasse und die Ergebnisse zurückgibt. Diese Ergebnisse können in einer Web-Steuerelement angezeigt werden, die an das ObjectDataSource gebunden ist. In diesem Lernprogramm betrachtet Binden von Steuerelementen GridView, DetailsView und FormView an das ObjectDataSource.

Bisher nur kennen gelernt haben wie der ObjectDataSource zu verwenden, um eine parameterlosen Methode aufrufen, aber was geschieht, wenn wir eine Methode aufrufen, die erwartet möchten Eingabeparameter aufweisen, z. B. die `ProductBLL` Klasse `GetProductsByCategoryID(categoryID)`? Zum Aufrufen einer Methode, die einen oder mehrere Parameter erwartet müssen wir die ObjectDataSource, um die Werte für diese Parameter anzugeben konfigurieren. Wir sehen, wie dies in unserem [nächsten Lernprogramm](declarative-parameters-cs.md).

Viel Spaß beim Programmieren!

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Lernprogramm erläutert finden Sie in den folgenden Ressourcen:

- [Erstellen Sie eigene Datenquellen-Steuerelementen](https://msdn.microsoft.com/library/ms364049.aspx)
- [GridView-Beispiele für ASP.NET 2.0](https://msdn.microsoft.com/library/aa479339.aspx)
- [Vereinfacht und erweitert die Datenbindung Syntax in ASP.NET 2.0](http://www.15seconds.com/issue/040630.htm)
- [Designs in ASP.NET 2.0](http://www.odetocode.com/Articles/423.aspx)
- [Serverseitige-Stilen mithilfe von Designs](https://quickstarts.asp.net/quickstartv20/aspnet/doc/themes/stylesheettheme.aspx)
- [Gewusst wie: Programmgesteuertes Anwenden von ASP.NET-Designs](https://msdn.microsoft.com/library/tx35bd89.aspx)

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurde Hilton Giesenow. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Nächste](declarative-parameters-cs.md)
