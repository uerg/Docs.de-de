---
uid: web-forms/overview/data-access/masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb
title: "Master/Detail-mit auswählbaren Master GridView mit einer Details-DetailView (VB) | Microsoft Docs"
author: rick-anderson
description: "Dieses Lernprogramm müssen eine GridView, deren Zeilen, den Namen und den Preis jedes Produkts zusammen mit einer Schaltfläche auswählen enthalten. Klicken Sie auf die Schaltfläche für eine Particu auswählen..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 1d1a7c93-971d-4690-9c5e-dac0e5014a09
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb
msc.type: authoredcontent
ms.openlocfilehash: 0db9bf25ce61c31dd8258aaebadf42e7738473ab
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="masterdetail-using-a-selectable-master-gridview-with-a-details-detailview-vb"></a>Master/Detail-mit auswählbaren Master GridView mit einer Details-DetailView (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunterladen](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_10_VB.exe) oder [PDF herunterladen](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/datatutorial10vb1.pdf)

> Dieses Lernprogramm müssen eine GridView, deren Zeilen, den Namen und den Preis jedes Produkts zusammen mit einer Schaltfläche auswählen enthalten. Auswählen der Schaltfläche für ein bestimmtes Produkt führt dazu, dass die vollständigen Details, die in einem DetailsView-Steuerelement auf derselben Seite angezeigt werden.


## <a name="introduction"></a>Einführung

In der [vorherigen Lernprogramm](master-detail-filtering-across-two-pages-vb.md) wurde erläutert, wie zum Erstellen eines Master-/Detail-Berichts mit zwei Webseiten: "master" Webseite, von dem die Liste der Lieferanten; angezeigt und eine Webseite "Details", die bereitgestellt werden, von dem ausgewählten Produkte aufgeführt Lieferanten. Dieses Format zwei Seite kann auf einer einzigen Seite aufeinander. Dieses Lernprogramm müssen eine GridView, deren Zeilen, den Namen und den Preis jedes Produkts zusammen mit einer Schaltfläche auswählen enthalten. Auswählen der Schaltfläche für ein bestimmtes Produkt führt dazu, dass die vollständigen Details, die in einem DetailsView-Steuerelement auf derselben Seite angezeigt werden.


[![Auswählen der Schaltfläche zeigt Details für das Produkt](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image2.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image1.png)

**Abbildung 1**: Auswählen der Schaltfläche zeigt Details für das Produkt ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image3.png))


## <a name="step-1-creating-a-selectable-gridview"></a>Schritt 1: Erstellen einer auswählbaren GridView

Wie bereits erwähnt, die in der zweiseitige Master-/Detail zu melden, dass jeder master Datensatz einen Link enthalten, die beim Klicken auf den Benutzer gesendet, auf der Seite Details zum Übergeben der geklickt wurde Zeile `SupplierID` Wert in der Abfragezeichenfolge. Jede GridView-Zeile, die mit einem HyperLinkField wurde diese ein Link hinzugefügt. Damit der einseitige Master-/Detail-Bericht, benötigen wir eine Schaltfläche für jede GridView Zeile, die beim Klicken auf zeigt die Details. Des GridView-Steuerelements kann so konfiguriert werden, dass eine Schaltfläche auswählen, für jede Zeile enthalten, die einen Postback verursacht, und kennzeichnet diese Zeile als der GridView [SelectedRow](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.selectedrow.aspx).

Durch Hinzufügen eines GridView-Steuerelements zum Starten der `DetailsBySelecting.aspx` auf der Seite der `Filtering` Ordner festlegen seiner `ID` Eigenschaft, um `ProductsGrid`. Als Nächstes fügen Sie eine neue, mit dem Namen ObjectDataSource `AllProductsDataSource` aufruft, die die `ProductsBLL` Klasse `GetProducts()` Methode.


[![Erstellen Sie eine mit dem Namen AllProductsDataSource ObjectDataSource](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image5.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image4.png)

**Abbildung 2**: Erstellen einer ObjectDataSource mit dem Namen `AllProductsDataSource` ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image6.png))


[![Verwenden Sie die ProductsBLL-Klasse](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image8.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image7.png)

**Abbildung 3**: Verwenden der `ProductsBLL` Klasse ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image9.png))


[![Konfigurieren der ObjectDataSource zum Aufrufen der GetProducts()-Methode](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image11.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image10.png)

**Abbildung 4**: Konfigurieren der ObjectDataSource zum Aufrufen der `GetProducts()` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image12.png))


Bearbeiten Sie die GridView Felder entfernen alle außer den `ProductName` und `UnitPrice` BoundFields. Darüber hinaus können Sie diese BoundFields nach Bedarf anpassen, z. B. beim Formatieren der `UnitPrice` BoundField als Währung und Ändern der `HeaderText` Eigenschaften der BoundFields. Diese Schritte können grafisch dar, über den Link "Spalten bearbeiten" aus der GridView smart Tag oder durch manuelles Konfigurieren von deklarativer Syntax erreicht werden.


[![Entfernen Sie alle bis auf die ProductName und UnitPrice BoundFields](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image14.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image13.png)

**Abbildung 5**: Entfernen Sie alle außer den `ProductName` und `UnitPrice` BoundFields ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image15.png))


Das endgültige Markup für die GridView ist:


[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample1.aspx)]

Als Nächstes müssen wir die GridView als auswählbare, kennzeichnen, die jede Zeile eine Schaltfläche auswählen hinzugefügt wird. Zu diesem Zweck einfach das Kontrollkästchen Sie aktivieren Auswahl in die GridView Smarttag.


[![Stellen Sie die GridView Zeilen ausgewählt werden](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image17.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image16.png)

**Abbildung 6**: Stellen Sie der GridView Zeilen auswählbare ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image18.png))


Überprüfen die Option Auswahl Aktivieren einer CommandField, fügt die `ProductsGrid` GridView mit seiner `ShowSelectButton` -Eigenschaft auf "true" festgelegt. Dadurch wird eine Schaltfläche auswählen für jede Zeile der GridView wie Abbildung 6 veranschaulicht wird. Wird standardmäßig die Option Schaltflächen als LinkButtons gerendert werden, jedoch können Sie Schaltflächen oder ImageButtons stattdessen über die CommandField `ButtonType` Eigenschaft.


[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample2.aspx)]

Wenn eine GridView Zeile auswählen geklickt wird ein Postback erfolgt und der GridView `SelectedRow` Eigenschaft aktualisiert wird. Zusätzlich zu den `SelectedRow` -Eigenschaft, um die GridView bietet die [SelectedIndex](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.selectedindex%28VS.80%29.aspx), ["SelectedValue"](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.selectedvalue%28VS.80%29.aspx), und [SelectedDataKey](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.selecteddatakey%28VS.80%29.aspx) Eigenschaften. Die `SelectedIndex` Eigenschaft gibt den Index der ausgewählten Zeile zurück, wohingegen die `SelectedValue` und `SelectedDataKey` Eigenschaften zurückgeben, Werte basierend auf der GridView [DataKeyNames Eigenschaft](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.datakeynames%28VS.80%29.aspx).

Die `DataKeyNames` Eigenschaft wird verwendet, um eine zuzuordnen oder weitere Feld "Daten"-Werte mit jeder Zeile an und wird häufig in "Attribut" eindeutig identifizierende Informationen aus den zugrunde liegenden Daten mit jeder Zeile GridView verwendet. Die `SelectedValue` Eigenschaft gibt den Wert des ersten `DataKeyNames` Feld "Daten" für die ausgewählte Zeile Where als die `SelectedDataKey` Eigenschaft gibt der ausgewählten Zeile `DataKey` Objekt, das alle Werte für die angegebenen Daten Schlüsselfelder für enthält Diese Zeile.

Die `DataKeyNames` Eigenschaft automatisch auf die Felder eindeutig identifizieren festgelegt, beim Binden einer Datenquelle an eine GridView, DetailsView oder FormView durch den Designer. Während diese Eigenschaft für uns automatisch in den vorherigen Lernprogrammen festgelegt wurde, würde in den Beispielen gearbeitet haben, ohne die `DataKeyNames` -Eigenschaft angegeben. Beachten Sie jedoch bei der auswählbare GridView in diesem Lernprogramm sowie für zukünftigen Lernprogrammen, in dem wir werden untersuchen müssen einfügen, aktualisieren und löschen, die `DataKeyNames` muss die Eigenschaft ordnungsgemäß festgelegt werden. Um sicherzustellen, dass Ihre GridView erkundet `DataKeyNames` -Eigenschaftensatz auf `ProductID`.

Wir unseren Fortschritt bisher über einen Browser anzeigen. Beachten Sie, dass die GridView, den Namen und den Preis aller Produkte zusammen mit einem LinkButton auswählen auflistet. Auswählen der Schaltfläche bewirkt, dass einen Postback. In Schritt2 sehen wir, wie Sie eine DetailsView Antworten auf diese Postback durch Anzeigen der Details für das ausgewählte Produkt.


[![Jede Zeile des Produkts enthält Select LinkButton](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image20.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image19.png)

**Abbildung 7**: jede Produktzeile enthält LinkButton auswählen ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image21.png))


## <a name="highlighting-the-selected-row"></a>Die ausgewählte Zeile hervorheben

Die `ProductsGrid` GridView verfügt über eine `SelectedRowStyle` -Eigenschaft, die den visuellen Stil für die ausgewählte Zeile diktieren verwendet werden kann. Ordnungsgemäß verwendet wird, kann dies die benutzerfreundlichkeit verbessern, indem Sie weitere deutlich mit welcher Zeile die GridView derzeit ausgewählt ist. Haben Sie für dieses Lernprogramm nun die ausgewählte Zeile durch einen gelben Hintergrund markiert werden.

Wie bei unseren früheren Tutorials Wir bemühen, behalten Sie die ästhetischen-bezogenen Einstellungen, die als CSS-Klassen definiert. Aus diesem Grund erstellen Sie eine neue CSS-Klasse in `Styles.css` mit dem Namen `SelectedRowStyle`.


[!code-css[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample3.css)]

Anzuwendende dieser CSS-Klasse, die die `SelectedRowStyle` Eigenschaft *alle* GridViews in unserem Lernprogramm Reihe Bearbeiten der `GridView.skin` Skin in der `DataWebControls` Design einschließen der `SelectedRowStyle` Einstellungen wie unten dargestellt:


[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample4.aspx)]

Durch diese Ergänzung ist die ausgewählte GridView Zeile jetzt mit eine gelbe Hintergrundfarbe markiert.


[![Passen Sie die ausgewählte Zeile Aussehen mithilfe der GridView SelectedRowStyle-Eigenschaft](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image23.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image22.png)

**Abbildung 8**: Anpassen der ausgewählten Zeile Zellendarstellung mithilfe der GridView `SelectedRowStyle` Eigenschaft ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image24.png))


## <a name="step-2-displaying-the-selected-products-details-in-a-detailsview"></a>Schritt 2: Anzeigen von Details des ausgewählten Produkts in einem DetailsView

Mit der `ProductsGrid` GridView abgeschlossen, bleibt eine DetailsView hinzufügen, die Anzeige von Informationen zu dem bestimmten Produkt ausgewählt ist. Hinzufügen eines DetailsView-Steuerelements über die GridView und erstellen Sie eine neue, mit dem Namen ObjectDataSource `ProductDetailsDataSource`. Da diese DetailsView bestimmte Informationen zu dem ausgewählten Produkt angezeigt werden soll, konfigurieren Sie die `ProductDetailsDataSource` verwenden die `ProductsBLL` Klasse `GetProductByProductID(productID)` Methode.


[![Rufen Sie die ProductsBLL Klasse GetProductByProductID(productID)-Methode](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image26.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image25.png)

**Abbildung 9**: Aufrufen der `ProductsBLL` Klasse `GetProductByProductID(productID)` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image27.png))


Haben die  *`productID`*  Wert des Parameters aus des GridView-Steuerelements `SelectedValue` Eigenschaft. Wie zuvor der GridView erläutert `SelectedValue` Eigenschaft gibt die ersten Daten-Schlüsselwert für die ausgewählte Zeile. Daher ist es zwingend erforderlich, die der GridView `DataKeyNames` -Eigenschaftensatz auf `ProductID`, sodass der ausgewählten Zeile `ProductID` ist der Rückgabewert von `SelectedValue`.


[![Festlegen von "ProductID" und Parameter an der GridView "SelectedValue"-Eigenschaft](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image29.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image28.png)

**Abbildung 10**: Legen Sie die  *`productID`*  Parameter an der GridView `SelectedValue` Eigenschaft ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image30.png))


Sobald die `productDetailsDataSource` ObjectDataSource ordnungsgemäß konfiguriert und, DetailsView gebunden wurde, dieses Lernprogramm ist abgeschlossen! Wenn die Seite zuerst besucht wird keine Zeile ausgewählt ist, sodass der GridView `SelectedValue` -Eigenschaft gibt `Nothing`. Da es sich um keine Produkte mit einer `NULL` `ProductID` Wert keine Einträge werden zurückgegeben, durch die `GetProductByProductID(productID)` -Methode, d. h., DetailsView nicht angezeigt wird (siehe Abbildung 11). Wenn Sie auf eine GridView-Zeile-Schaltfläche auswählen, erfolgt ein Postback und DetailsView wird aktualisiert. Dieses Mal der GridView `SelectedValue` -Eigenschaft gibt die `ProductID` der ausgewählten Zeile der `GetProductByProductID(productID)` Methode gibt ein `ProductsDataTable` mit Informationen über das jeweilige Produkt und DetailsView zeigt diese Details (siehe Abbildung 12).


[![Bei Anzeige erste besucht, nur die GridView](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image32.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image31.png)

**Abbildung 11**: beim ersten Mal besucht hat, wird nur die GridView angezeigt ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image33.png))


[![Bei Auswahl einer Zeile werden das Produkt Details angezeigt.](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image35.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image34.png)

**Abbildung 12**: beim Auswählen einer Zeile, der Product Details angezeigt werden ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image36.png))


## <a name="summary"></a>Zusammenfassung

In diesem Handbuch und die vorherigen drei Lernprogramme haben wir eine Reihe von Techniken für die Anzeige von Master/Detail-Berichten angezeigt. Verwenden in diesem Lernprogramm untersucht eine auswählbare GridView für die master-Datensätze und eine DetailsView Details zu den ausgewählten master Datensatz auf derselben Seite anzuzeigen. In den früheren Lernprogrammen, die wir zur Vorgehensweise beim Anzeigen von Master-/Detail-Berichten mithilfe von DropDownLists und Anzeigen von Masterdatensätze in eine Webseite und Detaildatensätze auf einem anderen gesucht haben.

Dieses Lernprogramm geht unsere Untersuchung von Master/Detail-Berichten verwendet. Beginnend mit den nächsten Lernprogrammen beginnen wir unsere Untersuchung des benutzerdefinierte Formatierung mit GridView, DetailsView und FormView. Wir sehen zum Anpassen der Darstellung dieser Steuerelemente auf Basis der Daten an diese gebunden, wie Daten in die GridView Fußzeile zusammenfassen und wie Sie Vorlagen verwenden, um ein höheres Maß an Kontrolle über das Layout zu erhalten.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurde Hilton Giesenow. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Zurück](master-detail-filtering-across-two-pages-vb.md)
