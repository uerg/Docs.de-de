---
uid: web-forms/overview/data-access/masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs
title: Master-/Detailbericht mit einer auswählbaren GridView-Mastersteuerelements mit einem DetailView-Detailsteuerelement (c#) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial müssen einer GridView-Ansicht, deren Zeilen, die Namen und den Preis jedes Produkts zusammen mit einer Schaltfläche auswählen enthalten. Durch Klicken auf die Schaltfläche auswählen für ein Particu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 0f982827-f8f9-420d-b36b-57b23f5aa519
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs
msc.type: authoredcontent
ms.openlocfilehash: e93461d566f827ff81dedf3651e7bd3d24c3583a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374936"
---
<a name="masterdetail-using-a-selectable-master-gridview-with-a-details-detailview-c"></a>Master-/Detailbericht mit einer auswählbaren GridView-Mastersteuerelements mit einem DetailView-Detailsteuerelement (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunter](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_10_CS.exe) oder [PDF-Datei herunterladen](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/datatutorial10cs1.pdf)

> In diesem Tutorial müssen einer GridView-Ansicht, deren Zeilen, die Namen und den Preis jedes Produkts zusammen mit einer Schaltfläche auswählen enthalten. Klicken Sie auf die Schaltfläche auswählen für ein bestimmtes Produkt führt dazu, dass die vollständigen Details, die in einem DetailsView-Steuerelement auf derselben Seite angezeigt werden.


## <a name="introduction"></a>Einführung

In der [vorherigen Tutorial](master-detail-filtering-across-two-pages-cs.md) erläutert, wie zum Erstellen eines Master-/Detail-Berichts mit zwei Webseiten: eine "master" Webseite, von dem wir die Liste der Lieferanten angezeigt und eine "Details"-Webseite, die diese Produkte von den ausgewählten aufgeführt. Lieferanten. Dieses Format zwei Seite kann auf einer einzigen Seite komprimiert werden. In diesem Tutorial müssen einer GridView-Ansicht, deren Zeilen, die Namen und den Preis jedes Produkts zusammen mit einer Schaltfläche auswählen enthalten. Klicken Sie auf die Schaltfläche auswählen für ein bestimmtes Produkt führt dazu, dass die vollständigen Details, die in einem DetailsView-Steuerelement auf derselben Seite angezeigt werden.


[![Klicken Sie auf die auswählen-Schaltfläche zeigt den Produktanforderungen-Details](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image2.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image1.png)

**Abbildung 1**: Klicken Sie auf die auswählen-Schaltfläche zeigt den Produktanforderungen-Details ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image3.png))


## <a name="step-1-creating-a-selectable-gridview"></a>Schritt 1: Erstellen ein auswählbares GridView

Beachten Sie, von denen in der zwei Seiten von Master-/Detailberichten gemeldet, dass jede Masterdatensatz einen Link enthalten, die beim Klicken auf den Benutzer gesendet, auf der Seite Details zum Übergeben der Zeile, auf die geklickt wurde `SupplierID` Wert in der Abfragezeichenfolge. Jede GridView-Zeile, die mit einem HyperLinkField wurde diese ein Link hinzugefügt. Für den einseitigen Master-/Detail-Bericht, benötigen wir eine Schaltfläche für jeden GridView Zeile, die beim Klicken auf die Details angezeigt. Das GridView-Steuerelement kann konfiguriert werden, um eine auswählen-Schaltfläche für jede Zeile einzuschließen, die bewirkt, dass einen Postback und markiert die Zeile als GridView [SelectedRow](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedrow.aspx).

Durch Hinzufügen einer GridView-Steuerelements zum Starten der `DetailsBySelecting.aspx` auf der Seite die `Filtering` Ordner Festlegen der `ID` Eigenschaft `ProductsGrid`. Fügen Sie eine neue, mit dem Namen "ObjectDataSource" `AllProductsDataSource` aufruft, die die `ProductsBLL` Klasse `GetProducts()` Methode.


[![Erstellen Sie eine mit dem Namen AllProductsDataSource "ObjectDataSource"](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image5.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image4.png)

**Abbildung 2**: Erstellen einer "ObjectDataSource" mit dem Namen `AllProductsDataSource` ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image6.png))


[![Verwenden Sie die ProductsBLL-Klasse](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image8.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image7.png)

**Abbildung 3**: Verwenden der `ProductsBLL` Klasse ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image9.png))


[![Konfigurieren von dem ObjectDataSource-Steuerelement zum Aufrufen der GetProducts()-Methode](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image11.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image10.png)

**Abbildung 4**: dem ObjectDataSource-Steuerelement Invoke konfigurieren die `GetProducts()` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image12.png))


Bearbeiten Sie den GridView Felder entfernen alle außer den `ProductName` und `UnitPrice` BoundFields. Darüber hinaus können Sie diese BoundFields nach Bedarf anpassen, z. B. beim Formatieren der `UnitPrice` BoundField als Währung und Ändern der `HeaderText` Eigenschaften der BoundFields. Diese Schritte können grafisch dargestellt, auf den Link "Spalten bearbeiten" aus den GridView Smarttag oder durch manuelles Konfigurieren von der deklarativen Syntax erfolgen.


[![Entfernen Sie alle bis auf die ProductName und UnitPrice BoundFields](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image14.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image13.png)

**Abbildung 5**: Entfernen Sie alle außer den `ProductName` und `UnitPrice` BoundFields ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image15.png))


Das endgültige Markup für die GridView ist:


[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/samples/sample1.aspx)]

Als Nächstes müssen wir die GridView als auswählbare, kennzeichnen, die jede Zeile eine auswählen-Schaltfläche hinzugefügt wird. Zu diesem Zweck einfach das Kontrollkästchen Sie Auswahl aktivieren in den GridView Smarttag.


[![Stellen Sie den GridView Zeilen ausgewählt werden](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image17.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image16.png)

**Abbildung 6**: Stellen Sie des GridView Zeilen ausgewählt werden ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image18.png))


Überprüfen die Option zur Mehrfachauswahl aktivieren einer CommandField, fügt die `ProductsGrid` GridView mit seiner `ShowSelectButton` -Eigenschaft auf "true" festgelegt. Dies führt zu einer Schaltfläche auswählen für jede Zeile der GridView, wie in Abbildung 6 gezeigt werden. Standardmäßig werden die Option Schaltflächen als LinkButtons gerendert, aber können Schaltflächen oder ImageButtons stattdessen über die CommandField des `ButtonType` Eigenschaft.


[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/samples/sample2.aspx)]

Wenn eine GridView-Zeile auswählen-Schaltfläche geklickt wird ein Postback Sicherheitsvorschriften und GridView `SelectedRow` Eigenschaft aktualisiert wird. Zusätzlich zu den `SelectedRow` -Eigenschaft, die GridView bietet die [SelectedIndex](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindex%28VS.80%29.aspx), [SelectedValue](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedvalue%28VS.80%29.aspx), und [SelectedDataKey](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selecteddatakey%28VS.80%29.aspx) Eigenschaften. Die `SelectedIndex` Eigenschaft gibt den Index der ausgewählten Zeile zurück, wohingegen die `SelectedValue` und `SelectedDataKey` Eigenschaften zurückgeben, Werte basierend auf des GridView [DataKeyNames-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.datakeynames%28VS.80%29.aspx).

Die `DataKeyNames` Eigenschaft wird verwendet, um eine App zuordnen oder weitere Datenfeld Werte in dem jede Zeile, und wird häufig verwendet, eindeutig identifizierenden Informationen aus den zugrunde liegenden Daten in dem jede Zeile der GridView-Attribut. Die `SelectedValue` -Eigenschaft gibt den Wert des ersten `DataKeyNames` Datenfeld für die ausgewählte Zeile Where als die `SelectedDataKey` Eigenschaft gibt der ausgewählten Zeile `DataKey` Objekt, das alle Werte für die angegebenen Schlüssel Datenfelder für enthält Diese Zeile.

Die `DataKeyNames` Eigenschaft wird automatisch beim Binden einer Datenquelle an eine GridView, DetailsView oder FormView-Steuerelement mithilfe des Designers, die eindeutig identifizierenden Felder festgelegt. Während diese Eigenschaft für uns automatisch in den vorherigen Tutorials festgelegt wurde, würde in den Beispielen gearbeitet haben, ohne die `DataKeyNames` angegebene Eigenschaft. Allerdings für die auswählbaren GridView in diesem Tutorial sowie für zukünftige Tutorials, in dem wir untersuchen werden einfügen, aktualisieren und löschen, die `DataKeyNames` muss die Eigenschaft richtig festgelegt werden. Nehmen Sie einen Moment Zeit, um sicherzustellen, dass Ihre GridView `DataKeyNames` -Eigenschaftensatz auf `ProductID`.

Zeigen wir unseren Fortschritt bisher über einen Browser ein. Beachten Sie, dass die GridView, die Namen und den Preis für alle Produkte zusammen mit einem LinkButton auswählen auflistet. Klicken Sie auf die auswählen-Schaltfläche auslöst ein Postback. In Schritt2 sehen wir, wie Sie ein DetailsView-Antworten auf diese Postback verfügen, indem Sie die Details für das ausgewählte Produkt anzeigen.


[![Jede Zeile des Produkts enthält, auf LinkButton](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image20.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image19.png)

**Abbildung 7**: jede Produktzeile enthält ein LinkButton wählen ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image21.png))


### <a name="highlighting-the-selected-row"></a>Die ausgewählte Zeile hervorheben

Die `ProductsGrid` GridView hat eine `SelectedRowStyle` -Eigenschaft, die zum Bestimmen des visuellen Stils für die ausgewählte Zeile verwendet werden kann. Ordnungsgemäß verwendet wird, kann dies die benutzererfahrung verbessern, indem Sie weitere eindeutig mit dem Zeile der GridView derzeit ausgewählt ist. In diesem Tutorial haben Sie lassen Sie uns die ausgewählte Zeile durch einen gelben Hintergrund markiert werden.

Wie bei unseren früheren Tutorials, wir bemühen uns, die Ästhetik-bezogenen Einstellungen, die als CSS-Klassen definiert zu halten. Aus diesem Grund erstellen Sie eine neue CSS-Klasse im `Styles.css` mit dem Namen `SelectedRowStyle`.


[!code-css[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/samples/sample3.css)]

Anzuwendende dieser CSS-Klasse, um die `SelectedRowStyle` Eigenschaft *alle* GridViews in unserer Reihe von Tutorials, bearbeiten die `GridView.skin` des Skinframes die `DataWebControls` Design enthalten die `SelectedRowStyle` Einstellungen wie unten dargestellt:


[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/samples/sample4.aspx)]

Mit folgender Ergänzung wird die ausgewählte GridView-Zeile jetzt durch einen gelben Hintergrundfarbe hervorgehoben.


[![Passen Sie die ausgewählte Zeile Darstellung mit GridView SelectedRowStyle Eigenschaft an](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image23.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image22.png)

**Abbildung 8**: Exemplarische Vorgehensweise: Anpassen von der ausgewählten Zeile Darstellung mithilfe von GridView `SelectedRowStyle` Eigenschaft ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image24.png))


## <a name="step-2-displaying-the-selected-products-details-in-a-detailsview"></a>Schritt 2: Anzeigen von Details für das ausgewählte Produkt in einem DetailsView

Mit der `ProductsGrid` GridView abgeschlossen haben, bleibt eine DetailsView hinzufügen, die Anzeige von Informationen von bestimmten Produkts ausgewählt ist. Fügen Sie einem DetailsView-Steuerelement oben GridView und erstellen Sie eine neue, mit dem Namen "ObjectDataSource" `ProductDetailsDataSource`. Da diese DetailsView bestimmte Informationen über das ausgewählte Produkt angezeigt werden soll, konfigurieren Sie die `ProductDetailsDataSource` verwenden die `ProductsBLL` Klasse `GetProductByProductID(productID)` Methode.


[![Aufrufen der ProductsBLL Klasse GetProductByProductID(productID)-Methode](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image26.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image25.png)

**Abbildung 9**: Rufen Sie die `ProductsBLL` Klasse `GetProductByProductID(productID)` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image27.png))


Haben die *`productID`* Wert des Parameters aus der GridView-Steuerelement `SelectedValue` Eigenschaft. Wie zuvor der GridView erläutert `SelectedValue` Eigenschaft zurückgibt, der erste Datenschlüsselwert für die ausgewählte Zeile. Aus diesem Grund ist es zwingend erforderlich, die des GridView `DataKeyNames` -Eigenschaftensatz auf `ProductID`, sodass der ausgewählten Zeile `ProductID` ist der Rückgabewert von `SelectedValue`.


[![Legen Sie die ProductID-Parameter auf des GridView SelectedValue-Eigenschaft](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image29.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image28.png)

**Abbildung 10**: Legen Sie die *`productID`* Parameter an der GridView `SelectedValue` Eigenschaft ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image30.png))


Sobald die `productDetailsDataSource` "ObjectDataSource" ordnungsgemäß konfiguriert und zur DetailsView gebunden wurde, in diesem Tutorial ist abgeschlossen! Wenn zuerst die Seite besucht wird keine Zeile ausgewählt ist, sodass das GridView `SelectedValue` -Eigenschaft gibt `null`. Da es sich um keine Produkte mit einem `NULL` `ProductID` -Wert, von dem keine Datensätze zurückgegeben werden die `GetProductByProductID(productID)` Methode, dies bedeutet, dass die DetailsView nicht angezeigt wird (siehe Abbildung 11). Klicken Sie nach dem Klicken auf eine GridView-Zeile-Schaltfläche auswählen, erfolgt ein Postback, und DetailsView wird aktualisiert. Dieses Mal des GridView `SelectedValue` -Eigenschaft gibt die `ProductID` der ausgewählten Zeile, die `GetProductByProductID(productID)` Methode gibt eine `ProductsDataTable` mit Informationen über das jeweilige Produkt und DetailsView zeigt diese Informationen (siehe Abbildung 12).


[![Beim ersten besucht, nur die GridView wird](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image32.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image31.png)

**Abbildung 11**: nur die GridView wird angezeigt, bei der ersten Anzeige ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image33.png))


[![Nach dem Auswählen einer Zeile, werden Details zum entsprechenden Produkt angezeigt.](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image35.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image34.png)

**Abbildung 12**: nach dem Auswählen einer Zeile, Details zum entsprechenden Produkt angezeigt werden ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image36.png))


## <a name="summary"></a>Zusammenfassung

In diesem und den drei vorherigen Tutorials haben wir gesehen, dass eine Reihe von Techniken für die Anzeige von Master/Detail-Berichte. Verwenden in diesem Lernprogramm aus, die wir untersucht ein auswählbares GridView, um die Masterdatensätze und einem DetailsView zum Anzeigen von Details zum ausgewählten master Datensatz auf derselben Seite befinden. In den vorherigen Tutorials erläutert, wie Sie zum Anzeigen von Master-/Detail-Berichten mithilfe von DropDownList-Steuerelementen und Anzeigen von Masterdatensätze in eine Webseite und die Detaildatensätze auf einer anderen.

Dieses Lernprogramm schließt unserer Untersuchung von Master/Detail-Berichte. Beginnend mit dem nächsten Tutorialwe beginnen unserem Seminar benutzerdefinierte Formatierung mit der GridView, DetailsView und FormView-Steuerelement. Wir sehen, Anpassen der Darstellung dieser Steuerelemente basierend auf den Daten, die an diese gebunden, wie Sie Daten in den GridView Fuß zusammenfassen und wie Sie Vorlagen verwenden, um ein höheres Maß an Kontrolle über das Layout zu erhalten.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial ist Hilton Giesenow. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](master-detail-filtering-across-two-pages-cs.md)
> [Weiter](master-detail-filtering-with-a-dropdownlist-vb.md)
