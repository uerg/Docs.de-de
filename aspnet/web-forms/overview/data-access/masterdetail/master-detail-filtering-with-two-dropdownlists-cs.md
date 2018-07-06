---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-with-two-dropdownlists-cs
title: Master/Detail-Filtern mit zwei DropDownList-Steuerelementen (c#) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial wird die Master/Detail-Beziehung zum Hinzufügen einer dritten Ebene, mit zwei DropDownList-Steuerelementen zum Auswählen der gewünschten übergeordneten und der zweiten übergeordneten Ebene s erweitert...
ms.author: aspnetcontent
ms.date: 03/31/2010
ms.assetid: ac4b0d77-4816-4ded-afd0-88dab667aedd
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-with-two-dropdownlists-cs
msc.type: authoredcontent
ms.openlocfilehash: f8f216689494e0f80902c42f425883558c1e21ce
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37842256"
---
<a name="masterdetail-filtering-with-two-dropdownlists-c"></a>Master/Detail-Filtern mit zwei DropDownList-Steuerelementen (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunter](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_8_CS.exe) oder [PDF-Datei herunterladen](master-detail-filtering-with-two-dropdownlists-cs/_static/datatutorial08cs1.pdf)

> In diesem Tutorial wird die Master/Detail-Beziehung zum Hinzufügen einer dritten Ebene, mit zwei DropDownList-Steuerelementen, wählen Sie die gewünschten Datensätze von übergeordneten und der zweiten übergeordneten Ebene erweitert.


## <a name="introduction"></a>Einführung

In der [vorherigen Tutorial](master-detail-filtering-with-a-dropdownlist-cs.md) untersuchten wir die Vorgehensweise beim Anzeigen eines einfachen Master/Detail-Berichts mit einem einzelnen DropDownList-Steuerelement mit den Kategorien und einer GridView-Ansicht mit Produkten, die der ausgewählten Kategorie gehören aufgefüllt. Dieser Bericht-Muster funktioniert gut, wenn Datensätze anzeigen, die eine 1: n Beziehung aufweisen und kann problemlos erweitert werden, um die Szenarien zu arbeiten, die mehrere 1: n Beziehungen enthalten. Beispielsweise müsste ein Auftragserfassungssystem Tabellen, die Kunden, Bestellungen und Bestellpositionen entsprechen. Ein bestimmten Kunden möglicherweise mehrere Aufträge mit jeder Bestellung, die aus mehreren Objekten besteht. Diese Daten können für den Benutzer mit zwei DropDownList-Steuerelementen und einer GridView-Ansicht angezeigt werden. Der ersten Dropdownliste müsste ein Listenelement für jeden Kunden in der Datenbank mit dem zweiten der Inhalt wird vom ausgewählten Kunden aufgegebene Bestellungen. Einer GridView-Ansicht die Positionen von der ausgewählten Reihenfolge aufgelistet.

Während die Northwind-Datenbank enthalten, die kanonische Customer/Order/Order Detailinformationen in der `Customers`, `Orders`, und `Order Details` Tabellen diese Tabellen werden nicht in unserer Architektur erfasst. Dennoch können wir noch veranschaulicht die Verwendung von zwei abhängige DropDownList-Steuerelementen. Der ersten Dropdownliste werden die Kategorien und die zweite die Produkte der ausgewählten Kategorie aufgelistet. Eine DetailsView werden dann die Details des ausgewählten Produkts aufgelistet.

## <a name="step-1-creating-and-populating-the-categories-dropdownlist"></a>Schritt 1: Erstellen und Auffüllen der Dropdownliste für die Kategorien

Unser erstes Ziel ist die DropDownList hinzufügen, die die Kategorien aufgeführt. Diese Schritte wurden im vorherigen Tutorial ausführlich untersucht, aber Sie werden aus Gründen der Vollständigkeit hier zusammengefasst.

Öffnen der `MasterDetailsDetails.aspx` auf der Seite die `Filtering` Ordner hinzufügen, einem DropDownList-Steuerelement auf der Seite legen Sie seine `ID` Eigenschaft `Categories`, und klicken Sie dann auf die Datenquelle konfigurieren-Link in der Smarttag. Wählen Sie aus dem Konfigurations-Assistenten eine neue Datenquelle hinzufügen.


[![Fügen Sie eine neue Datenquelle für das DropDownList hinzu.](master-detail-filtering-with-two-dropdownlists-cs/_static/image2.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image1.png)

**Abbildung 1**: Fügen Sie eine neue Datenquelle für das DropDownList ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-with-two-dropdownlists-cs/_static/image3.png))


Die neue Datenquelle sollten natürlich einer ObjectDataSource gegeben werden. Nennen Sie diese neue "ObjectDataSource" `CategoriesDataSource` und rufen Sie die `CategoriesBLL` des Objekts `GetCategories()` Methode.


[![Wählen Sie die Klasse CategoriesBLL verwendet](master-detail-filtering-with-two-dropdownlists-cs/_static/image5.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image4.png)

**Abbildung 2**: Wählen Sie zum Verwenden der `CategoriesBLL` Klasse ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-with-two-dropdownlists-cs/_static/image6.png))


[![Konfigurieren von dem ObjectDataSource-Steuerelement zur Verwendung der GetCategories()-Methode](master-detail-filtering-with-two-dropdownlists-cs/_static/image8.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image7.png)

**Abbildung 3**: Konfigurieren von dem ObjectDataSource-Steuerelement verwenden die `GetCategories()` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-with-two-dropdownlists-cs/_static/image9.png))


Nach dem Konfigurieren der "ObjectDataSource" müssen noch angeben, welches Feld der Datenquelle angezeigt werden soll, in der `Categories` DropDownList und welche als Wert für das Listenelement konfiguriert werden sollen. Legen Sie die `CategoryName` -Feld als die Anzeige und `CategoryID` als Wert für jedes Listenelement.


[![Haben Sie die DropDownList-Anzeige "CategoryName" Feld aus, und verwenden CategoryID als Wert](master-detail-filtering-with-two-dropdownlists-cs/_static/image11.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image10.png)

**Abbildung 4**: haben Sie die DropDownList-Anzeige der `CategoryName` Feld und die Verwendung `CategoryID` als Wert ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-with-two-dropdownlists-cs/_static/image12.png))


An diesem Punkt haben wir ein DropDownList-Steuerelement (`Categories`), wird aufgefüllt, die Datensätze aus der `Categories` Tabelle. Wenn der Benutzer eine neue Kategorie aus der Dropdownliste auswählt, sollten wir einen Postback ausgelöst wird, um das Produkt DropDownList zu aktualisieren, die wir jetzt erstellen Sie in Schritt2. Aus diesem Grund überprüfen Sie die Option "AutoPostBack aktivieren" die `categories` DropDownLists-Smarttag.


[![Povolit vlastnost AutoPostBack für Kategorien DropDownList](master-detail-filtering-with-two-dropdownlists-cs/_static/image14.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image13.png)

**Abbildung 5**: AutoPostBack aktivieren, für die `Categories` DropDownList ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-with-two-dropdownlists-cs/_static/image15.png))


## <a name="step-2-displaying-the-selected-categorys-products-in-a-second-dropdownlist"></a>Schritt 2: Anzeigen der ausgewählten Kategorie Produkte in einem zweiten DropDownList-Steuerelement

Mit der `Categories` DropDownList abgeschlossen wird im nächsten Schritt eine DropDownList von Produkten, die in der ausgewählten Kategorie angezeigt. Um dies zu erreichen, fügen Sie einen anderen DropDownList auf der Seite mit dem Namen `ProductsByCategory`. Wie bei der `Categories` DropDownList, erstellen Sie eine neue "ObjectDataSource" für die `ProductsByCategory` DropDownList, die mit dem Namen `ProductsByCategoryDataSource`.


[![Fügen Sie eine neue Datenquelle für die ProductsByCategory DropDownList hinzu.](master-detail-filtering-with-two-dropdownlists-cs/_static/image17.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image16.png)

**Abbildung 6**: Hinzufügen einer neuen Datenquelle für die `ProductsByCategory` DropDownList ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-with-two-dropdownlists-cs/_static/image18.png))


[![Erstellen Sie eine neue, mit dem Namen ProductsByCategoryDataSource "ObjectDataSource"](master-detail-filtering-with-two-dropdownlists-cs/_static/image20.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image19.png)

**Abbildung 7**: Erstellen einer neuen "ObjectDataSource" mit dem Namen `ProductsByCategoryDataSource` ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-with-two-dropdownlists-cs/_static/image21.png))


Da die `ProductsByCategory` DropDownList-Anforderungen nur die Produkte der ausgewählten Kategorie angezeigt haben, dem ObjectDataSource-Steuerelement Aufrufen der `GetProductsByCategoryID(categoryID)` Methode aus der `ProductsBLL` Objekt.


[![Wählen Sie die Klasse ProductsBLL verwendet](master-detail-filtering-with-two-dropdownlists-cs/_static/image23.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image22.png)

**Abbildung 8**: Wählen Sie zum Verwenden der `ProductsBLL` Klasse ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-with-two-dropdownlists-cs/_static/image24.png))


[![Konfigurieren von dem ObjectDataSource-Steuerelement zur Verwendung der GetProductsByCategoryID(categoryID)-Methode](master-detail-filtering-with-two-dropdownlists-cs/_static/image26.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image25.png)

**Abbildung 9**: Konfigurieren von dem ObjectDataSource-Steuerelement verwenden die `GetProductsByCategoryID(categoryID)` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-with-two-dropdownlists-cs/_static/image27.png))


Im letzten Schritt des Assistenten geben Sie den Wert der ich die *`categoryID`* Parameter. Weisen Sie diesen Parameter für das ausgewählte Element aus der `Categories` DropDownList.


[![Rufen Sie die CategoryID Parameterwert aus dem Categories DropDownList](master-detail-filtering-with-two-dropdownlists-cs/_static/image29.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image28.png)

**Abbildung 10**: Abrufen der *`categoryID`* Parameterwert aus der `Categories` DropDownList ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-with-two-dropdownlists-cs/_static/image30.png))


Mit dem ObjectDataSource-Steuerelement konfiguriert übrig bleibt angeben, welche Datenfelder für die Quelle für die Anzeige und der Wert für DropDownLists-Elemente verwendet werden. Anzeigen der `ProductName` ein, und verwenden Sie die `ProductID` Feld als Wert.


[![Geben Sie die Datenfelder für Text und Werteigenschaften der DropDownList ListItems verwendet](master-detail-filtering-with-two-dropdownlists-cs/_static/image32.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image31.png)

**Abbildung 11**: Angeben der Data Source Felder verwendet, für die DropDownList `ListItem` s' `Text` und `Value` Eigenschaften ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-with-two-dropdownlists-cs/_static/image33.png))


Mit dem ObjectDataSource-Steuerelement und `ProductsByCategory` DropDownList konfiguriert unserer Seite zeigt zwei DropDownList-Steuerelementen: die erste Listet alle Kategorien, während die zweite dieser Produkte, die der ausgewählten Kategorie gehören aufgelistet werden. Wenn der Benutzer eine neue Kategorie aus der ersten Dropdownliste auswählt, wird ein Postback zur Folge haben, und der zweiten Dropdownliste wird werden erneut gebunden, die Produkte, die die neu ausgewählte Kategorie angehören. Abbildung 12 und 13 zeigt `MasterDetailsDetails.aspx` in Aktion, wenn Sie über einen Browser angezeigt.


[![Wenn die Seite zuerst besuchen zu können, ist die Kategorie "Getränke" ausgewählt.](master-detail-filtering-with-two-dropdownlists-cs/_static/image35.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image34.png)

**Abbildung 12**: Wenn die Seite zuerst besuchen zu können, der die Kategorie "Getränke" ausgewählt ist ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-with-two-dropdownlists-cs/_static/image36.png))


[![Eine andere Kategorie auswählen, zeigt die neue Kategorie-Produkte](master-detail-filtering-with-two-dropdownlists-cs/_static/image38.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image37.png)

**Abbildung 13**: Auswählen einer anderen Kategorie zeigt die neue Kategorie Produkte ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-with-two-dropdownlists-cs/_static/image39.png))


Derzeit den `productsByCategory` DropDownList, bei deren Änderung ist *nicht* Postback verursacht. Allerdings möchten wir ein Postback ausgelöst, sobald wir eine DetailsView zum Anzeigen von Details für das ausgewählte Produkt (Schritt 3) hinzufügen. Überprüfen Sie daher das Kontrollkästchen "AutoPostBack aktivieren" aus der `productsByCategory` DropDownLists-Smarttag.


[![Die AutoPostBack-Funktion für die ProductsByCategory DropDownList aktivieren](master-detail-filtering-with-two-dropdownlists-cs/_static/image41.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image40.png)

**Abbildung 14**: Aktivieren Sie das AutoPostBack-Funktion für die `productsByCategory` DropDownList ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-with-two-dropdownlists-cs/_static/image42.png))


## <a name="step-3-using-a-detailsview-to-display-details-for-the-selected-product"></a>Schritt 3: Eine DetailsView verwenden, um Details für das ausgewählte Produkt anzuzeigen.

Der letzte Schritt ist, um die Details für das ausgewählte Produkt in einem DetailsView anzuzeigen. Legen Sie dazu, die Seite ein DetailsView hinzugefügt, die `ID` Eigenschaft `ProductDetails`, und das Erstellen einer neuen "ObjectDataSource" für. Konfigurieren Sie diese "ObjectDataSource", um die Daten aus der pull die `ProductsBLL` -Klasse `GetProductByProductID(productID)` Methode, die mithilfe des ausgewählten Werts für die `ProductsByCategory` DropDownList für den Wert des der *`productID`* Parameter.


[![Wählen Sie die Klasse ProductsBLL verwendet](master-detail-filtering-with-two-dropdownlists-cs/_static/image44.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image43.png)

**Abbildung 15**: Wählen Sie zum Verwenden der `ProductsBLL` Klasse ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-with-two-dropdownlists-cs/_static/image45.png))


[![Konfigurieren von dem ObjectDataSource-Steuerelement zur Verwendung der GetProductByProductID(productID)-Methode](master-detail-filtering-with-two-dropdownlists-cs/_static/image47.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image46.png)

**Abbildung 16**: Konfigurieren von dem ObjectDataSource-Steuerelement verwenden die `GetProductByProductID(productID)` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-with-two-dropdownlists-cs/_static/image48.png))


[![Rufen Sie die ProductID-Parameterwert aus ProductsByCategory DropDownList](master-detail-filtering-with-two-dropdownlists-cs/_static/image50.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image49.png)

**Abbildung 17**: Abrufen der *`productID`* Parameterwert aus der `ProductsByCategory` DropDownList ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-with-two-dropdownlists-cs/_static/image51.png))


Sie können auswählen, um die verfügbaren Felder im DetailsView anzuzeigen. Ich habe mich entschieden habe, zum Entfernen der `ProductID`, `SupplierID`, und `CategoryID` Felder neu angeordnet und formatiert die verbleibenden Felder. Darüber hinaus, dass ich DetailsViews geklärt `Height` und `Width` Eigenschaften, sodass der DetailsView, um auf die Breite erforderlich, um die optimale Anzeige zu erweitern, die Daten, anstatt es auf einer angegebenen Größe beschränkt. Das vollständige Markup wird unten angezeigt:


[!code-aspx[Main](master-detail-filtering-with-two-dropdownlists-cs/samples/sample1.aspx)]

Testen Sie in Ruhe die `MasterDetailsDetails.aspx` Seite in einem Browser. Auf den ersten Blick scheint es, dass alles wie gewünscht funktioniert, aber es ein kleineres Problem ist. Wenn Sie eine neue Kategorie auswählen der `ProductsByCategory` DropDownList wird aktualisiert, um diese Produkte für die ausgewählte Kategorie, aber die `ProductDetails` DetailsView fortgesetzt wird, um die vorherigen Produktinformationen anzuzeigen. Die DetailsView wird aktualisiert, wenn Sie ein anderes Produkt für die ausgewählte Kategorie auswählen. Darüber hinaus, wenn Sie gründlich genug getestet haben, Sie finden, wenn Sie ständig neue Kategorien auswählen (z. B. Auswahl Getränke aus der `Categories` DropDownList, und klicken Sie dann auf "Gewürze", klicken Sie dann Confections) alle anderen Kategorieauswahl bewirkt, dass die `ProductDetails`DetailsView aktualisiert werden.

Damit dieses Problem nicht konkretisiert werden können, betrachten wir ein konkretes Beispiel. Wenn Sie die Seite zuerst besuchen Sie die Kategorie "Getränke" ausgewählt ist und die zugehörigen Produkte werden geladen, der `ProductsByCategory` DropDownList. Chai entspricht, wird das ausgewählte Produkt und die Details werden angezeigt, der `ProductDetails` DetailsView, wie in Abbildung 18 dargestellt.


[![Das ausgewählte Produkt Details werden in einem DetailsView angezeigt.](master-detail-filtering-with-two-dropdownlists-cs/_static/image53.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image52.png)

**Abbildung 18**: die ausgewählten Produkts Details werden angezeigt, in einem DetailsView ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-with-two-dropdownlists-cs/_static/image54.png))


Wenn Sie die Kategorieauswahl aus Getränke in "Gewürze" ändern, um ein Postback auftritt und die `ProductsByCategory` DropDownList wird entsprechend aktualisiert, aber DetailsView Details weiterhin für Chai angezeigt.


[![Des zuvor ausgewählten Produkts Details werden weiterhin angezeigt.](master-detail-filtering-with-two-dropdownlists-cs/_static/image56.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image55.png)

**Abbildung 19**: die zuvor ausgewählte die Produktdetails werden weiterhin angezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-with-two-dropdownlists-cs/_static/image57.png))


Wählen ein neues Produkt in der Liste aktualisiert DetailsView wie erwartet. Wenn Sie eine neue Kategorie auswählen, nach dem Ändern des Produkts, wird nicht erneut DetailsView aktualisieren. Wenn anstatt auf ein neues Produkt Sie eine neue Kategorie ausgewählt haben, würde jedoch DetailsView aktualisieren. Was in der Welt ist hier passiert?

Das Problem ist ein Problem der zeitlichen Steuerung im Lebenszyklus der Seite. Jedes Mal, wenn eine Seite angefordert, dass es eine Reihe von Schritten als renderingbeschreibung durchläuft. In einem der folgenden Schritte aus dem ObjectDataSource-Steuerelement steuert, überprüfen, wenn ihre `SelectParameters` Werte geändert haben. Wenn also das Web-Steuerelement gebunden weiß dem ObjectDataSource-Steuerelement an, dass die Anzeige aktualisiert werden muss. Wenn z. B. eine neue Kategorie ausgewählt ist, die `ProductsByCategoryDataSource` "ObjectDataSource" erkennt, dass Werte der Parameter geändert haben und die `ProductsByCategory` DropDownList erneuerte Bindungen, die Produkte für die ausgewählte Kategorie abrufen.

Das Problem, das in diesem Fall tritt auf, ist der Punkt im Lebenszyklus Seite, mit denen die ObjectDataSources geänderten Parametern überprüft auftritt, *vor* die erneute Bindung des zugeordneten Websteuerelemente. Aus diesem Grund bei der Auswahl einer neuen Kategorie das `ProductsByCategoryDataSource` "ObjectDataSource" erkennt eine Änderung im Wert des Parameters. Dem ObjectDataSource-Steuerelement ein, die die `ProductDetails` DetailsView, jedoch nicht beachten Sie, solche Änderungen da die `ProductsByCategory` DropDownList ist noch nicht erneut gebunden werden. Weiter unten in den Lebenszyklus der `ProductsByCategory` DropDownList die Bindung an die "ObjectDataSource", die Produkte für die neu ausgewählte Kategorie abrufen. Während der `ProductsByCategory` DropDownList Wert geändert wurde, die `ProductDetails` DetailsViews "ObjectDataSource" wurde bereits die Überprüfung der Parameter-Werts vorgenommen; DetailsView zeigt daher die vorherigen Ergebnisse. Diese Interaktion ist in Abbildung 20 dargestellt.


[![Der ProductsByCategory DropDownList-Wert ändert, nachdem die ProductDetails DetailsViews ObjectDataSource-Steuerelement prüft, ob Änderungen](master-detail-filtering-with-two-dropdownlists-cs/_static/image59.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image58.png)

**Abbildung 20**: die `ProductsByCategory` DropDownList-Wert ändert sich nach der `ProductDetails` DetailsViews "ObjectDataSource" überprüft, ob Änderungen ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-with-two-dropdownlists-cs/_static/image60.png))


Zur Umgehung des Problems explizit erneut zu binden muss der `ProductDetails` DetailsView nach der `ProductsByCategory` DropDownList gebunden wurde. Wir erreichen dies durch den Aufruf der `ProductDetails` DetailsView `DataBind()` Methode bei der `ProductsByCategory` DropDownList `DataBound` -Ereignis ausgelöst wird. Fügen Sie den folgenden Ereignishandlercode, der `MasterDetailsDetails.aspx` Seite des Code-Behind-Klasse (finden Sie in der "[Programmgesteuertes Festlegen der Parameterwerte der ObjectDataSource](../basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs.md)" Informationen zum Hinzufügen eines ereignishandlers):


[!code-csharp[Main](master-detail-filtering-with-two-dropdownlists-cs/samples/sample2.cs)]

Nach diesem expliziter Aufruf an die `ProductDetails` DetailsView `DataBind()` Methode wurde hinzugefügt, das Lernprogramm funktioniert wie erwartet. Abbildung 21 hervorgehoben, wie diese geändert, geschaffen, das Problem weiter oben.


[![ProductDetails DetailsView ist explizit aktualisiert bei der ProductsByCategory DropDownList DataBound-Ereignis ausgelöst](master-detail-filtering-with-two-dropdownlists-cs/_static/image62.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image61.png)

**Abbildung 21**: die `ProductDetails` DetailsView ist explizit aktualisiert, wenn die `ProductsByCategory` DropDownList `DataBound` -Ereignis ausgelöst wird ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-with-two-dropdownlists-cs/_static/image63.png))


## <a name="summary"></a>Zusammenfassung

Die DropDownList dient als ideales Benutzeroberflächenelement für Master/Detail-Berichte, die eine 1: n Beziehung zwischen der Master- und Detaildatensätzen. Im vorherigen Tutorial wurde erläutert, wie einem einzelnen DropDownList-Steuerelement verwenden, um die Produkte, die von der ausgewählten Kategorie angezeigt zu filtern. In diesem Tutorial stellen wir GridView-Produkte mit einem DropDownList-Steuerelement ersetzt, und eine DetailsView zum Anzeigen der Details des ausgewählten Produkts verwendet. In diesem Tutorial erörterten Konzepte können mit Datenmodellen, die im Zusammenhang mit mehreren 1: n Beziehungen, wie z. B. Kunden, Bestellungen und Auftragspositionen problemlos erweitert werden. Im Allgemeinen können Sie immer eine DropDownList für jede der Entitäten in der 1: n Beziehungen "one" hinzufügen.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial ist Hilton Giesenow. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](master-detail-filtering-with-a-dropdownlist-cs.md)
> [Weiter](master-detail-filtering-across-two-pages-cs.md)
