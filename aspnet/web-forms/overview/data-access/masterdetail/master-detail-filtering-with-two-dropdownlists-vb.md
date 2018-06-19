---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-with-two-dropdownlists-vb
title: Master/Detail-Filtern mit zwei DropDownLists (VB) | Microsoft Docs
author: rick-anderson
description: In diesem Lernprogramm wird erweitert, die Master-/Detail-Beziehung, um eine dritte Ebene, mit zwei DropDownList-Steuerelementen zum Auswählen der gewünschten über- und referenzierende Recor hinzufügen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 11ae4f64-01ba-4823-95f4-a2fe1f84f7d7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-with-two-dropdownlists-vb
msc.type: authoredcontent
ms.openlocfilehash: ee0232cf8f7c0533703a51a4629522fd887f216f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887225"
---
<a name="masterdetail-filtering-with-two-dropdownlists-vb"></a>Master/Detail-Filtern mit zwei DropDownLists (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunterladen](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_8_VB.exe) oder [PDF herunterladen](master-detail-filtering-with-two-dropdownlists-vb/_static/datatutorial08vb1.pdf)

> In diesem Lernprogramm wird erweitert, die Master-/Detail-Beziehung, um eine dritte Ebene, mit zwei DropDownList-Steuerelementen zum Auswählen der gewünschten über- und referenzierende Datensätze hinzufügen.


## <a name="introduction"></a>Einführung

In der [vorherigen Lernprogramm](master-detail-filtering-with-a-dropdownlist-vb.md) untersucht die Vorgehensweise beim Anzeigen eines einfachen Master-/Detail-Berichts mit einer einzelnen DropDownList mit Kategorien und eine GridView, die mit dieser Produkte, die der ausgewählten Kategorie angehören, aufgefüllt. Dieses Muster Bericht funktioniert gut, wenn Datensätze anzeigen, die eine 1: n-Beziehung aufweisen und können problemlos für Szenarien geeignet, die mehrere 1: n-Beziehungen enthalten erweitert werden. Beispielsweise müsste ein bestellungseingabesystem Tabellen, die Kunden, Bestellungen und Einzelposten Reihenfolge entsprechen. Ein bestimmten Kunden möglicherweise mehrere Aufträge mit jede Bestellung, bestehend aus mehreren Elementen. Diese Daten können mit zwei DropDownLists und eine GridView an den Benutzer angezeigt werden. Der erste DropDownList müsste ein Listenelement für jeden Kunden in der Datenbank mit dem zweiten einer Person Inhalt wird vom ausgewählten Kunden aufgegebene Bestellungen. Eine GridView würden die Einzelposten aus der ausgewählten Reihenfolge aufgeführt werden.

Wenn die Northwind-Datenbank enthält die kanonische Customer/Order/Order Detailinformationen in der `Customers`, `Orders`, und `Order Details` Tabellen diese Tabellen werden nicht in unserem Architektur aufgezeichnet. Dennoch eine überaus können wir noch zeigen zwei abhängige DropDownLists verwenden. Der erste DropDownList werden die Produkte, die der ausgewählten Kategorie gehören die Kategorien und die zweite aufgelistet. Eine DetailsView werden dann die Details des ausgewählten Produkts aufgeführt.

## <a name="step-1-creating-and-populating-the-categories-dropdownlist"></a>Schritt 1: Erstellen und Auffüllen der DropDownList Kategorien

Unser erste Ziel besteht darin DropDownList hinzuzufügen, die die Kategorien aufgeführt. Diese Schritte ausführlich untersucht wurden, in dem vorherigen Lernprogramm, jedoch werden hier aus Gründen der Vollständigkeit zusammengefasst.

Öffnen der `MasterDetailsDetails.aspx` auf der Seite der `Filtering` Ordner hinzufügen, eine DropDownList auf der Seite, legen Sie seine `ID` Eigenschaft, um `Categories`, und klicken Sie dann auf den Link "Datenquelle konfigurieren" in das Smarttag. Wählen Sie den Konfigurations-Assistenten eine neue Datenquelle hinzufügen.


[![Fügen Sie eine neue Datenquelle für die DropDownList hinzu.](master-detail-filtering-with-two-dropdownlists-vb/_static/image2.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image1.png)

**Abbildung 1**: Hinzufügen einer neuen Datenquelle für DropDownList ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-with-two-dropdownlists-vb/_static/image3.png))


Die neue Datenquelle, natürlich ein ObjectDataSource sollte. Nennen Sie diese neue ObjectDataSource `CategoriesDataSource` und Aufrufen der `CategoriesBLL` des Objekts `GetCategories()` Methode.


[![Wählen Sie die Klasse CategoriesBLL verwendet](master-detail-filtering-with-two-dropdownlists-vb/_static/image5.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image4.png)

**Abbildung 2**: Wählen Sie zum Verwenden der `CategoriesBLL` Klasse ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-with-two-dropdownlists-vb/_static/image6.png))


[![Konfigurieren der ObjectDataSource zur Verwendung der GetCategories()-Methode](master-detail-filtering-with-two-dropdownlists-vb/_static/image8.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image7.png)

**Abbildung 3**: Konfigurieren der ObjectDataSource verwenden die `GetCategories()` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-with-two-dropdownlists-vb/_static/image9.png))


Nach dem Konfigurieren der ObjectDataSource wir benötigen Sie angeben, welches Feld der Datenquelle angezeigt werden soll, in der `Categories` DropDownList und welche als Wert für das Listenelement konfiguriert werden sollen. Legen Sie die `CategoryName` -Felds ist, als die Anzeige und `CategoryID` als Wert für jedes Listenelement.


[![Weisen Sie die CategoryName-Feld und CategoryID verwenden der DropDownList-Anzeige auf, als Wert](master-detail-filtering-with-two-dropdownlists-vb/_static/image11.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image10.png)

**Abbildung 4**: haben Sie die Anzeige der DropDownList der `CategoryName` Feld und Verwendung `CategoryID` als Wert ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-with-two-dropdownlists-vb/_static/image12.png))


An diesem Punkt haben wir ein DropDownList-Steuerelement (`Categories`), wird mit den Datensätzen aufgefüllt, die `Categories` Tabelle. Wenn der Benutzer eine neue Kategorie aus der Dropdownliste auswählt, sollten wir ein Postback ausgeführt wird, um das Produkt DropDownList aktualisieren, die wir die offensichtlichen So erstellen Sie in Schritt2. Aktivieren Sie deshalb die AutoPostBack aktivieren-Option von der `categories` DropDownLists Smarttag.


[![Für die Kategorien DropDownList AutoPostBack aktivieren](master-detail-filtering-with-two-dropdownlists-vb/_static/image14.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image13.png)

**Abbildung 5**: AutoPostBack aktivieren, für die `Categories` DropDownList ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-with-two-dropdownlists-vb/_static/image15.png))


## <a name="step-2-displaying-the-selected-categorys-products-in-a-second-dropdownlist"></a>Schritt 2: Anzeigen der ausgewählten Kategorie Produkte in einer zweiten DropDownList

Mit der `Categories` DropDownList abgeschlossen ist, im nächsten Schritt eine DropDownList von Produkten, die zur ausgewählten Kategorie angezeigt. Um dies zu erreichen, fügen Sie einen anderen DropDownList auf der Seite mit dem Namen `ProductsByCategory`. Wie bei der `Categories` DropDownList, erstellen Sie eine neue ObjectDataSource für die `ProductsByCategory` DropDownList mit dem Namen `ProductsByCategoryDataSource`.


[![Fügen Sie eine neue Datenquelle für ProductsByCategory DropDownList hinzu.](master-detail-filtering-with-two-dropdownlists-vb/_static/image17.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image16.png)

**Abbildung 6**: Hinzufügen einer neuen Datenquelle für die `ProductsByCategory` DropDownList ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-with-two-dropdownlists-vb/_static/image18.png))


[![Erstellen Sie eine neue, mit dem Namen ProductsByCategoryDataSource ObjectDataSource](master-detail-filtering-with-two-dropdownlists-vb/_static/image20.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image19.png)

**Abbildung 7**: Erstellen einer neuen ObjectDataSource namens `ProductsByCategoryDataSource` ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-with-two-dropdownlists-vb/_static/image21.png))


Da die `ProductsByCategory` DropDownList muss nur die Produkte, die der ausgewählten Kategorie gehören angezeigt haben, das ObjectDataSource Aufrufen der `GetProductsByCategoryID(categoryID)` Methode aus der `ProductsBLL` Objekt.


[![Wählen Sie die Klasse ProductsBLL verwendet](master-detail-filtering-with-two-dropdownlists-vb/_static/image23.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image22.png)

**Abbildung 8**: Wählen Sie zum Verwenden der `ProductsBLL` Klasse ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-with-two-dropdownlists-vb/_static/image24.png))


[![Konfigurieren der ObjectDataSource zur Verwendung der GetProductsByCategoryID(categoryID)-Methode](master-detail-filtering-with-two-dropdownlists-vb/_static/image26.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image25.png)

**Abbildung 9**: Konfigurieren der ObjectDataSource verwenden die `GetProductsByCategoryID(categoryID)` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-with-two-dropdownlists-vb/_static/image27.png))


Im letzten Schritt des Assistenten müssen wir den Wert der angeben der *`categoryID`* Parameter. Weisen Sie diesen Parameter an das ausgewählte Element aus der `Categories` DropDownList.


[![Ziehen Sie die CategoryID-Parameterwert aus der DropDownList Kategorien](master-detail-filtering-with-two-dropdownlists-vb/_static/image29.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image28.png)

**Abbildung 10**: Ziehen Sie die *`categoryID`* Parameterwert aus der `Categories` DropDownList ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-with-two-dropdownlists-vb/_static/image30.png))


Mit der ObjectDataSource konfiguriert übrig bleibt angeben, welche Datenquellenfeldern für die Anzeige und der Wert der DropDownList-Elemente verwendet werden. Anzeigen der `ProductName` Feld, und verwenden Sie die `ProductID` Feld als Wert.


[![Geben Sie die Datenquellenfeldern für Text und Werteigenschaften der DropDownList ListItems verwendet](master-detail-filtering-with-two-dropdownlists-vb/_static/image32.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image31.png)

**Abbildung 11**: Angeben der Datenquelle Felder verwendet, für die DropDownList `ListItem` s " `Text` und `Value` Eigenschaften ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-with-two-dropdownlists-vb/_static/image33.png))


Mit der ObjectDataSource und `ProductsByCategory` DropDownList konfiguriert unserer Seite zeigt zwei DropDownLists: die erste Liste alle Kategorien darüber, während die zweite dieser Produkte, die zur ausgewählten Kategorie aufgelistet. Wenn der Benutzer eine neue Kategorie aus der ersten Dropdownliste auswählt, ein Postback wird Stellen Sie sicher, und der zweite DropDownList werden neu, zeigt dieser Produkte, die neu ausgewählte Kategorie angehören. Abbildung 12 und 13 anzeigen `MasterDetailsDetails.aspx` in Aktion, wenn Sie über einen Browser angezeigt.


[![Bei der ersten Seite besuchen, wird die Kategorie "Getränke" ausgewählt.](master-detail-filtering-with-two-dropdownlists-vb/_static/image35.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image34.png)

**Abbildung 12**: bei der ersten Seite besuchen, die Kategorie "Getränke" ausgewählt ist ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-with-two-dropdownlists-vb/_static/image36.png))


[![Durch Auswahl einer anderen Kategorie wird die neue Kategorie Produkte angezeigt.](master-detail-filtering-with-two-dropdownlists-vb/_static/image38.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image37.png)

**Abbildung 13**: Auswählen einer anderen Kategorie zeigt die neue Kategorie Produkte ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-with-two-dropdownlists-vb/_static/image39.png))


Derzeit die `productsByCategory` DropDownList, bei deren Änderung ist *nicht* dazu führen, dass einen Postback. Allerdings sollten wir ein Postback erfolgen, sobald wir eine DetailsView zur Anzeige von Details für das ausgewählte Produkt (Schritt 3) hinzufügen. Daher das Kontrollkästchen AutoPostBack aktivieren aus dem `productsByCategory` DropDownLists Smarttag.


[![Die AutoPostBack-Funktion für die ProductsByCategory DropDownList aktivieren](master-detail-filtering-with-two-dropdownlists-vb/_static/image41.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image40.png)

**Abbildung 14**: Aktivieren Sie die AutoPostBack-Funktion für die `productsByCategory` DropDownList ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-with-two-dropdownlists-vb/_static/image42.png))


## <a name="step-3-using-a-detailsview-to-display-details-for-the-selected-product"></a>Schritt 3: Verwenden einer DetailsView zum Anzeigen von Details für das ausgewählte Produkt

Der letzte Schritt besteht in einem DetailsView Details für das ausgewählte Produkt anzuzeigen. Legen Sie dazu, Hinzufügen einer DetailsView auf der Seite ", seine `ID` Eigenschaft `ProductDetails`, und erstellen Sie eine neue ObjectDataSource für. Konfigurieren Sie diese ObjectDataSource zum Abrufen der zugehörigen Daten aus der `ProductsBLL` -Klasse `GetProductByProductID(productID)` Methode mit den ausgewählten Wert der die `ProductsByCategory` DropDownList für den Wert des der *`productID`* Parameter.


[![Wählen Sie die Klasse ProductsBLL verwendet](master-detail-filtering-with-two-dropdownlists-vb/_static/image44.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image43.png)

**Abbildung 15**: Wählen Sie zum Verwenden der `ProductsBLL` Klasse ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-with-two-dropdownlists-vb/_static/image45.png))


[![Konfigurieren der ObjectDataSource zur Verwendung der GetProductByProductID(productID)-Methode](master-detail-filtering-with-two-dropdownlists-vb/_static/image47.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image46.png)

**Abbildung 16**: Konfigurieren der ObjectDataSource verwenden die `GetProductByProductID(productID)` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-with-two-dropdownlists-vb/_static/image48.png))


[![Der Parameterwert "ProductID" aus der ProductsByCategory DropDownList abrufen](master-detail-filtering-with-two-dropdownlists-vb/_static/image50.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image49.png)

**Abbildung 17**: Ziehen Sie die *`productID`* Parameterwert aus der `ProductsByCategory` DropDownList ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-with-two-dropdownlists-vb/_static/image51.png))


Sie können auswählen, eines der verfügbaren Felder in der `ProductDetails` DetailsView. Ich haben sich entschieden, entfernen Sie die `ProductID`, `SupplierID`, und `CategoryID` Felder neu angeordnet und formatiert die verbleibenden Felder. Darüber hinaus ich deaktiviert, DetailsView des `Height` und `Width` Eigenschaften, sodass DetailsView um an die Breite des erforderlich, um die optimale Anzeige zu erweitern, die Daten, anstatt es zu einer angegebenen Größe beschränkt. Das vollständige Markup wird unten angezeigt:


[!code-aspx[Main](master-detail-filtering-with-two-dropdownlists-vb/samples/sample1.aspx)]

Nehmen Sie einen Moment Zeit, probieren Sie die `MasterDetailsDetails.aspx` Seite in einem Browser. Auf den ersten Blick scheint es, dass alles wie gewünscht funktioniert, aber es ein Problem feine liegt. Wenn Sie eine neue Kategorie auswählen der `ProductsByCategory` DropDownList so aktualisiert wird, die Produkte für die ausgewählte Kategorie, aber die `ProductDetails` DetailsView fortgesetzt wird, um die vorherigen Produktinformationen anzuzeigen. DetailsView wird aktualisiert, wenn Sie ein anderes Produkt für die ausgewählte Kategorie auswählen. Darüber hinaus, wenn Sie gründlich genug getestet haben, finden Sie, die auf Wunsch ständig neue Kategorien (z. B. Auswahl Getränke aus der `Categories` DropDownList, und klicken Sie dann auf "Gewürze", klicken Sie dann Süßwaren) jeder anderen benutzerkategorieauswahl bewirkt, dass die `ProductDetails`DetailsView aktualisiert werden.

Um dieses Problem concretize zu unterstützen, sehen wir uns ein Beispiel hierfür. Beim ersten die Seite Besuch die Kategorie "Getränke" ausgewählt ist und die zugehörigen Produkte werden geladen, der `ProductsByCategory` DropDownList. Chai ist das ausgewählte Produkt und die Details werden angezeigt, der `ProductDetails` DetailsView, wie in Abbildung 18 dargestellt.


[![Das ausgewählte Produkt Details werden in einem DetailsView angezeigt.](master-detail-filtering-with-two-dropdownlists-vb/_static/image53.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image52.png)

**Abbildung 18**: Details des ausgewählten Produkts in einer DetailsView angezeigt werden ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-with-two-dropdownlists-vb/_static/image54.png))


Wenn Sie die Kategorieauswahl Getränke auf "Gewürze" ändern, was zu ein Postback tritt auf, und die `ProductsByCategory` DropDownList wird entsprechend aktualisiert, DetailsView zeigt Details weiterhin für Chai jedoch.


[![Details des zuvor ausgewählten Produkts werden weiterhin angezeigt.](master-detail-filtering-with-two-dropdownlists-vb/_static/image56.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image55.png)

**Abbildung 19**: der zuvor ausgewählten des Produktdetails werden weiterhin angezeigt ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-with-two-dropdownlists-vb/_static/image57.png))


DetailsView Entnahme aus der Liste ein neues Produkt aktualisiert werden, wie erwartet. Wenn Sie eine neue Kategorie auswählen, nach dem Ändern des Produkts, das Update wird nicht DetailsView erneut aus. Allerdings würde anstatt auf ein neues Produkt eine neue Kategorie ausgewählt haben, DetailsView aktualisieren. Was in der ganzen Welt wird hier passiert?

Das Problem ist ein Problem der zeitlichen Steuerung im Lebenszyklus der Seite. Bei jedem wird eine Seite angefordert, dass er eine Reihe von Schritten als ihr Rendering durchführt. In einen der folgenden Schritte steuert das ObjectDataSource überprüfen, ob keines ihrer `SelectParameters` Werte geändert haben. Wenn also das Websteuerelement Daten gebunden weiß das ObjectDataSource, es muss sich um die Anzeige zu aktualisieren. Wenn z. B. eine neue Kategorie aktiviert ist und die `ProductsByCategoryDataSource` ObjectDataSource erkennt, dass die Parameterwerte geändert wurden und die `ProductsByCategory` DropDownList bindet sich selbst, die Produkte für die ausgewählte Kategorie abrufen.

Das Problem, das in diesem Fall tritt auf, ist der Punkt wird im Lebenszyklus Seite, die die ObjectDataSources geänderten Parameter überprüfen auftritt, *vor* die erneute Zuordnung der zugehörigen Daten Websteuerelemente. Daher beim Auswählen einer neuen Kategorie der `ProductsByCategoryDataSource` ObjectDataSource erkennt eine Änderung im Wert des Parameters. Das ObjectDataSource verwendet werden, indem Sie die `ProductDetails` DetailsView, jedoch nicht beachten Sie, solche Änderungen da die `ProductsByCategory` DropDownList wurden noch nicht zu. Weiter unten in den Lebenszyklus der `ProductsByCategory` DropDownList auf seine ObjectDataSource, die Produkte für die neu ausgewählte Kategorie grabbing bindet. Während der `ProductsByCategory` DropDownList Wert geändert wurde, die `ProductDetails` DetailsViews ObjectDataSource hat bereits die Überprüfung der Parameter-Werts durchgeführt; DetailsView zeigt daher die vorherigen Ergebnisse. Diese Aktivität ist in Abbildung 20 dargestellt.


[![Die ProductsByCategory DropDownList-Wert wird nach der ProductDetails DetailsView ObjectDataSource prüft, ob Änderungen geändert](master-detail-filtering-with-two-dropdownlists-vb/_static/image59.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image58.png)

**Abbildung 20**: die `ProductsByCategory` DropDownList Wert ändert sich nach der `ProductDetails` DetailsViews ObjectDataSource überprüft Änderungen ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-with-two-dropdownlists-vb/_static/image60.png))


Um dies zu beheben, wir explizit erneut binden müssen, der `ProductDetails` DetailsView nach der `ProductsByCategory` DropDownList gebunden wurde. Wir erreichen dies durch Aufrufen der `ProductDetails` DetailsViews `DataBind()` Methode bei der `ProductsByCategory` DropDownLists `DataBound` -Ereignis ausgelöst. Fügen Sie den folgenden Ereignishandlercode, auf die `MasterDetailsDetails.aspx` Seite des Code-Behind-Klasse (finden Sie in der "[Programmgesteuertes Festlegen von Parameterwerten für das ObjectDataSource](../basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs.md)" Nähere Informationen zum Hinzufügen eines ereignishandlers):


[!code-vb[Main](master-detail-filtering-with-two-dropdownlists-vb/samples/sample2.vb)]

Nach dieser expliziter Aufruf von der `ProductDetails` DetailsViews `DataBind()` Methode wurde hinzugefügt, das Lernprogramm funktioniert wie erwartet. Abbildung 21 kennzeichnet, wie diese geändert, geschaffen unserer frühere Problem.


[![ProductDetails DetailsView ist explizit aktualisiert bei der ProductsByCategory DropDownLists datengebundenen-Ereignis ausgelöst](master-detail-filtering-with-two-dropdownlists-vb/_static/image62.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image61.png)

**Abbildung 21**: die `ProductDetails` DetailsView wird explizit aktualisiert die `ProductsByCategory` DropDownLists `DataBound` -Ereignis ausgelöst ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-with-two-dropdownlists-vb/_static/image63.png))


## <a name="summary"></a>Zusammenfassung

Die DropDownList dient als ein Element der Benutzeroberfläche ideal für Master-/Detail-Berichte, eine 1: n-Beziehung zwischen der Master- und Detailtabelle Datensätzen besteht. In dem vorherigen Lernprogramm wurde erläutert, wie eine einzelne DropDownList verwenden, um die Produkte angezeigt, die von der ausgewählten Kategorie zu filtern. In diesem Lernprogramm stellen wir GridView von Produkten mit einer DropDownList ersetzt, und eine DetailsView verwendet, um die Details des ausgewählten Produkts anzuzeigen. In diesem Lernprogramm erörterten Konzepte können leicht auf Datenmodellen, die im Zusammenhang mit mehreren 1: n-Beziehungen, z. B. Kunden, Bestellungen und Bestellartikel ausgeweitet werden. Im Allgemeinen können Sie immer eine DropDownList für jede der Entitäten in der 1: n-Beziehungen "one" hinzufügen.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurde Hilton Giesenow. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](master-detail-filtering-with-a-dropdownlist-vb.md)
> [Weiter](master-detail-filtering-across-two-pages-vb.md)
